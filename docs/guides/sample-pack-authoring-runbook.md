# Sample-Pack Authoring Runbook

> **Purpose:** Enter a sample dataset into a dedicated authoring database through MCP, in small checkpointed units that can be resumed after any interruption, and then export it back into a sample-pack manifest. This is the D7 authoring track: the pack is built from data a person or agent entered through the same tools a user has, not from hand-written JSON.

> **Last Updated:** 2026-09-21

---

## Scope

- **What this covers:** provisioning an empty `kind: sample` authoring database, binding a backend and MCP to it, entering the dataset one work unit at a time, resuming an interrupted run, and exporting the result.
- **Where the data goes:** only the authoring database. It is registered like any managed sample database, under `<config root>/data/databases/sample/<authoring id>.db`, with the identity `bootstrap_id = sample-authoring` and a namespaced manifest hash (`sha256("zorivest-sample-authoring.v1:" + authoring id)`). That hash is never a real pack hash, so a sample pack can never be applied into an authoring database by mistake.
- **Why an authoring database and not your own (D10):** a sample database refuses every personal import (D10a), and an authoring database is a sample database. Personal records cannot leak into the pack, and pack data cannot land in a personal database.
- **Which entities are entered through MCP:** the nine classes in the coverage matrix: account, trade, plan, report, watchlist, analytics, tax, template and policy. News evidence, AI-review persistence, stored reports and broker records are out of MCP scope by design (D8a, D8c, D8d, D8e) and come from the compiler.
- **Correcting an entry:** there is no trade `update` action. The only way to correct an execution is delete-and-recreate (D8g); see [Correcting an execution (D8g)](#correcting-an-execution-d8g).
- **What this does not cover:** shipping the pack (template build and packaging) or resetting a sample database.

---

## Prerequisites

1. **Provision and activate the authoring database** with the authoring tool. Use a dedicated config root; never a production one.

   ```bash
   uv run python tools/sample_authoring.py provision --config-root <config root> --authoring-id authoring-representative
   ```

   The tool prints one JSON line: `{"activated_now": true, "active": true, "canonical_path": "...", "database_id": "authoring-representative", "registry_revision": 2}`. Authoring ids must match `^authoring-[a-z0-9][a-z0-9-]{2,54}$`; any other id (a pack id, a `sample-` id, `personal-empty-v1`) is refused with exit 1, and no file or registry entry is created. Running `provision` again while the authoring database is active changes nothing and reports the same revision. Add `--no-activate` to register it without switching the active database.

2. **Start a backend bound to that config root.** Set `ZORIVEST_CONFIG_DIR=<config root>` and remove `ZORIVEST_DB_URL`, so the backend resolves the active registry profile. Check `GET /api/v1/health`: `runtime.database_id` must be `authoring-representative`, and `runtime.database_file_identity` must equal the profile's `file_identity_sha256` in `database-registry.json`. **Do not enter anything until both match**: they prove the backend is writing into the authoring file and not into another database.

3. **Point MCP at that backend.** Set `ZORIVEST_API_URL` to the backend's API base (for example `http://127.0.0.1:<port>/api/v1`) in the MCP server's environment. Every write below goes through MCP tools, so a wrong `ZORIVEST_API_URL` would write to a different database. Re-check the health identity whenever the backend restarts.

4. **Have the entry plan ready.** The dataset comes as `entry-units.json` (for the representative fixture: `tests/fixtures/sample_pack/authoring_representative/entry-units.json`) together with its `authoring-source.json`. Pick a checkpoint directory outside the config root, for example `<work dir>/checkpoints`.

---

## Work units

A work unit is a small, ordered group of MCP writes that is read back and checkpointed as a whole. No unit makes more than **10** MCP write calls, so a failure never leaves more than one unit's worth of state to reconcile. Argument placeholders `{"$account": "<account_ref>"}` and `{"$plan": "<plan_ref>"}` are resolved from the ids recorded by earlier units' read-backs.

| Unit | MCP calls | Writes | Identity key | Read back | Pass condition | Checkpoint |
|---|---|---|---|---|---|---|
| `U1-accounts` | `zorivest_account create` ×2 | 2 | account `name` | `zorivest_account list` | both accounts present once, with the planned institution, type and currency | `<checkpoint_dir>/unit-U1-accounts.json` |
| `U2-plan` | `zorivest_plan create`, then `zorivest_plan update` to `status: executed` | 2 | (`ticker`, `account_id`, `entry_price`) | `zorivest_plan list` | one plan with the planned fields and `status` `executed` | `<checkpoint_dir>/unit-U2-plan.json` |
| `U3-executions` | `zorivest_trade create` ×4 | 4 | trade `exec_id` | `zorivest_trade list` for the account | all four executions present once, with the planned price, quantity, fee and time | `<checkpoint_dir>/unit-U3-executions.json` |
| `U4-balances-<n>` | `zorivest_account balance` (`account_id`, `balance`, `snapshot_datetime`), one call per snapshot | ≤ 10 per unit | (`account_id`, `snapshot_datetime`) | `GET /api/v1/accounts/{account_id}/balances` (REST read; MCP `zorivest_account get` shows only the latest snapshot) | every planned snapshot present once with the planned balance | `<checkpoint_dir>/unit-U4-balances-<n>.json` |
| `U5-reviews-<n>` | `zorivest_report create` (`trade_id` = the execution's `exec_id`), one per reviewed trade | ≤ 10 per unit | `trade_id` (one review per trade) | `zorivest_report get` per `trade_id` | each planned review present with the planned grades, `followed_plan`, emotional state, lessons and tags | `<checkpoint_dir>/unit-U5-reviews-<n>.json` |
| `U6-mistakes-<n>` | `zorivest_report track_mistake` (`trade_exec_id`, `category`, `estimated_cost`, `notes`) | ≤ 10 per unit | (`trade_exec_id`, `category`) | `GET /api/v1/mistakes?trade_exec_id=<exec_id>` (REST read; MCP `zorivest_analytics mistake_summary` gives only per-category counts, used as a cross-check) | each planned mistake present once; record every returned `id` in `created_ids` | `<checkpoint_dir>/unit-U6-mistakes-<n>.json` |
| `U7-watchlist-<n>` | `zorivest_watchlist create` ×1, then `add_ticker` per item | ≤ 10 per unit (1 list + up to 9 items) | watchlist `name`; item (`watchlist_id`, `ticker`) | `zorivest_watchlist get` | the list present once with exactly the planned tickers and notes | `<checkpoint_dir>/unit-U7-watchlist-<n>.json` |
| `U8-trials` | `zorivest_analytics record_trial`, one per trial | ≤ 10 per unit | (`strategy_key`, `metric_name`, `params_json`) | `zorivest_analytics list_trials` per `strategy_key` | each planned trial present once with the planned metric value, sample size and outcome | `<checkpoint_dir>/unit-U8-trials.json` |
| `U9-tax-profile` | `zorivest_tax create_profile` ×1 | 1 | `tax_year` | `zorivest_tax get_profile` | the profile present with the planned filing status, rates, state and AGI | `<checkpoint_dir>/unit-U9-tax-profile.json` |
| `U10-templates` | `zorivest_template create`, one per template; then `zorivest_template preview` per template (a render check, not a write) | ≤ 10 per unit | template `name` | `zorivest_template get` | each template present with the planned body, then each preview renders with its sample data | `<checkpoint_dir>/unit-U10-templates.json` |
| `U11-policies` | `zorivest_policy create`, one per policy, with `trigger.enabled: false` | ≤ 10 per unit | policy `name` | `zorivest_policy list` | each policy present once, `enabled` false and `approved` false | `<checkpoint_dir>/unit-U11-policies.json` |

`U1`–`U3` are the units the representative fixture's `entry-units.json` carries. `U4`–`U11` extend the same shape to every MCP-exercised entity class. A class with more than 10 rows (32 balance snapshots, 24 reviews, 15 mistakes) is split into numbered units (`-1`, `-2`, …) of at most 10 writes each. Units depend on earlier ones: `U4` needs accounts, `U5` and `U6` need executions, and `U2` plans need accounts. Everything else is independent.

Unit-specific rules:

- **Balances (`U4`):** MCP cannot set a snapshot's `label` or `currency`; they default to `cash` and `USD`. A snapshot the pack needs with another label or currency is compiler-authored, not entered.
- **Mistakes (`U6`) and excursions:** a trade delete removes the trade's mistakes, excursion metrics and ledger rows (milestone-tracking), so mistakes may be entered before an execution is corrected; the D8g cycle restores the mistakes, but never excursion or ledger rows.
- **Trials (`U8`):** the trial ledger is append-only, and a repeated `record_trial` adds a second row. Reconcile before every write; never replay one blind.
- **Policies (`U11`):** a created policy is never approved (`approved` is always false on create), and there is no MCP approve action. Setting `trigger.enabled: false` keeps it inert. Activating it is a user step in the guide (D8d).

Run the units in table order. For each unit:

1. **Reconcile** (see [Checkpoints and resume](#checkpoints-and-resume)) before issuing any write.
2. **Write** only the calls reconciliation says are missing.
3. **Read back** with the unit's `list` call and compare every planned field.
4. **Checkpoint** only after the read-back passes.

---

## Checkpoints and resume

**Checkpoint file.** After a unit's read-back passes, write `<checkpoint_dir>/unit-<unit_id>.json`:

```json
{"unit_id": "U1-accounts", "completed_at": "2026-09-16T10:00:00Z", "readback_sha256": "<sha256 of the canonical read-back>", "created_ids": {"schwab-primary-taxable": "<account_id>", "vanguard-roth-ira": "<account_id>"}}
```

`readback_sha256` is the SHA-256 digest of the unit's read-back rows, restricted to the unit's own rows and planned fields and serialized as canonical JSON (sorted keys). `created_ids` maps each planned ref to the id the backend assigned; later units resolve `$account` / `$plan` placeholders from it.

**Resume rule.** When a run starts or restarts, take the units in order:

- If a unit's checkpoint exists **and** a fresh read-back produces the same `readback_sha256` digest, skip the unit. It is complete.
- Otherwise run the unit under **reconcile** (`UNIT_RECONCILE`). This covers a missing checkpoint, a digest mismatch, a crash between a committed write and its checkpoint, and a crash between the server's commit and the recorded response.

**Reconcile (`UNIT_RECONCILE`).** Before each planned write, read the unit's rows back through MCP and match the planned write by its identity key: `U1-accounts` by account `name`; `U2-plan` by (`ticker`, `account_id`, `entry_price`), with the status update issued only while `status` is not `executed`; `U3-executions` by `exec_id` within the account; every later unit by the identity key in its row of the work-unit table. Then take exactly one of these outcomes:

| Matches found | Outcome |
|---|---|
| none | **write**: issue the planned call |
| exactly one, and every entry-unit field equals the plan | **adopt**: record its id and issue no write |
| exactly one, and any field differs | stop the unit as **`divergent_state`** |
| more than one | stop the unit as **`ambiguous_state`** |

A stopped unit writes no row and no checkpoint. Reconciliation only ever adopts rows that already landed and writes the missing ones. **It never deletes or overwrites anything**, so replaying a unit any number of times cannot create duplicates or destroy authored data.

### Replay identity

Replaying a completed unit must be a no-op, and the check for that is the **write count**. The end state alone can't show it, because a replay that rewrites identical rows ends in the same state and is still wrong.

- **Rule:** re-running a unit that already has a checkpoint issues **zero** MCP write calls and reproduces the checkpoint's `readback_sha256` exactly.
- **Why it holds:** either the resume rule skips the unit (checkpoint present, digest equal), or reconcile finds every planned row by its identity key and adopts it. Neither path writes.
- **If it breaks:** a replay that issues even one write, or whose fresh read-back digest differs from the checkpoint, is a defect. Stop, and log it under the [Known-issues protocol](#known-issues-protocol) with the replay's receipt. Do not re-checkpoint over it.

The live conformance test `mcp-server/tests/integration/sample-mcp-replay-identity.test.ts` checks this rule by counting the writes a replay issues.

---

## Correcting an execution (D8g)

There is no trade `update` action (D8g). To correct an execution you delete it and recreate it **under the same `exec_id`**, and that stable id is what keeps everything attached to it. The pack's plan link is the execution's `plan_ref` in `authoring-source.json`, keyed by `exec_id`. Authored plans carry no `linked_trade_id`, because no MCP action sets one. So a recreate under the same `exec_id` keeps the plan link without any write.

What a delete does to the execution's dependents, and so what the cycle must restore:

| Dependent | Effect of `zorivest_trade delete` | Restored by |
|---|---|---|
| Human review (`trade_reports`) and its images | deleted with the trade (cascade) | step 4, `zorivest_report create` from the captured review |
| Plans with `linked_trade_id` = this execution | link cleared; the plan, its status and `executed_at` are kept | nothing to restore for authored data (no MCP-set links); a non-null captured link stops the cycle as `divergent_state` |
| Mistake entries | deleted with the trade (milestone-tracking) | removed explicitly in step 2 so resume stays id-exact; re-tracked in step 4 |
| Excursion metrics | deleted with the trade (milestone-tracking) | none: the cycle cannot restore them, so it stops before any write (step 1) |
| Transaction ledger rows | deleted with the trade (milestone-tracking) | none: the cycle cannot restore them, so it stops before any write (step 1) |

### Steps

Every step writes its result into the correction checkpoint `<checkpoint_dir>/correction-<exec_id>.json`, as `phase` plus the data the later steps need. A step records its phase only after its read-back passes.

1. **Capture** (`phase: captured`). Before any delete, read and store:
   - the execution's current row (`zorivest_trade list` for the account) and the corrected fields;
   - its review (`zorivest_report get`, or `null` if there is none);
   - its mistakes (`GET /api/v1/mistakes?trade_exec_id=<exec_id>`), with their `id`s, in id order;
   - every plan (`zorivest_plan list`) with its status and `executed_at`;
   - the rows of `excursion_metrics` and `transaction_ledger` (`zorivest_db row_samples`, `limit: 20`). No other read surface exposes them.

   Stop without writing anything if any plan's `linked_trade_id` names the execution, or if an `excursion_metrics` or `transaction_ledger` row names it in `trade_exec_id`. Also stop if either `row_samples` call returns 20 rows. A full page means the read may be incomplete, so it cannot rule those rows out. The cycle stops on excursion and ledger rows because the delete would remove them and nothing in the cycle can restore them.
2. **Remove captured mistakes** (`phase: blockers_removed`). For each captured mistake: `zorivest_system confirm_token` with `target: "report.delete_mistake"`, then `zorivest_report delete_mistake` with that token. The delete in step 3 would remove them anyway; removing them explicitly here keeps a resume id-exact.
3. **Delete** (`phase: deleted`). `zorivest_system confirm_token` with `target: "delete_trade"`, then `zorivest_trade delete` with the `exec_id` and the token. Read back to confirm the execution is gone.
4. **Recreate, in dependency order** (`phase: recreated`):
   1. `zorivest_trade create` with the **same `exec_id`** and the corrected fields.
   2. `zorivest_report create` from the captured review.
   3. `zorivest_report track_mistake` for each captured mistake, in the captured order.

   Record the new mistake ids in the checkpoint.
5. **Revalidate dependents** (`phase: complete`). Read everything back and require:
   - exactly one execution with that `exec_id`, carrying the corrected fields;
   - the review equal to the captured one;
   - the mistakes equal to the captured ones (ids aside);
   - every plan's status and `executed_at` unchanged.

   Then refresh the checkpoints of every unit the correction touched (`U3`, `U5`, `U6`): their `readback_sha256` changes, because the corrected fields are now the planned ones. If the correction changed the account or ticker, update the execution's `plan_ref` annotation in `authoring-source.json` as well, or `export` refuses the pack with `plan_link_mismatch`.

### Interruption recovery

To resume, re-read the correction checkpoint and run the steps again from the first one. Each step reconciles before it writes:

- **Step 2** deletes only the captured mistake ids that still exist.
- **Step 3** deletes only while `phase` is before `deleted` **and** the execution exists. Once `deleted` is recorded, the execution is never deleted again.
- **Step 4** creates the execution only if it is absent, and adopts it if present with the corrected fields. Any other row under that `exec_id` is `divergent_state`. It creates the review only if `zorivest_report get` finds none, and tracks each mistake only if no row with that (`trade_exec_id`, `category`) exists yet.

The captured data lives in the checkpoint and not in the database. So an interruption **after the delete and before the recreate** is repaired from the checkpoint: the resume recreates the execution, its review and its mistakes. The live test `mcp-server/tests/integration/sample-mcp-d8g-delete-recreate.test.ts` interrupts the cycle at exactly that point. It checks that the resume restores the review, and that the `exec_id` survives the cycle.

---

## Failure handling

- **`divergent_state`:** a row with the unit's identity key exists but disagrees with the plan (for example an account with the planned name but a different institution). Stop. Compare the row with the entry plan and decide by hand whether the plan or the row is wrong. Correcting it is a human decision; this runbook never edits or deletes the row for you.
- **`ambiguous_state`:** more than one row matches one identity key. Stop and report every matching id. The dataset is no longer one-to-one with its plan.
- **Write call fails (validation 422, 404, 409):** the unit stops at that call. Fix the entry plan or the placeholder resolution, then re-run. Reconcile adopts every row that already landed.
- **Crash, timeout or lost response:** re-run. The resume rule and reconcile make the replay converge on exactly the planned rows.
- **Health identity changed** (backend restarted, `ZORIVEST_API_URL` changed): stop all writes and repeat Prerequisites 2–3 before resuming.
- **A write is refused with `sample_database_import_refused`:** you called an import action. Imports are never part of authoring; see [Never](#never).

---

## Export

When every unit is checkpointed, export the authoring database back into a manifest with the authoring tool's `export` subcommand, then prove equivalence with `roundtrip`:

```bash
uv run python tools/sample_authoring.py export --database <canonical_path> --source <authoring-source.json> --out <manifest.json> [--checkpoint-dir <dir>]
uv run python tools/sample_authoring.py roundtrip --manifest <manifest.json> --source <authoring-source.json> --work-dir <dir>
```

`export` opens the database read-only and never edits it. It reads the whole database as one snapshot, so a backend still bound to the file cannot mix two states into one manifest; stopping the backend first is still the simplest way to export the state you last saw. A resumed export reuses a checkpoint only when its records still match the database and source, so an edited or damaged checkpoint is discarded and re-read. Fields that are not database columns (header metadata, plan and report creation dates, execution annotations, overlays) come from `authoring-source.json`. `roundtrip` compiles the manifest, applies it into a fresh sample database, exports that database again, and requires the result to match the input exactly.

---

## Known-issues protocol

Every defect found while authoring the sample dataset is logged in one place, where a later remediation session can find it with a single query. The protocol adds no tooling; it uses the known-issues SSOT (gap analysis §4).

**Tagging.** Create the entry with `tools/issue_triage.py add` (id, title, severity, component, discovered date, root cause, blast radius). Then add the field `discovered_during: sample-seed-dataset` to that entry in `project configuration/context/known-issues.yaml`; the `add` command has no flag for it, and the schema keeps extra fields. Finish with `uv run python tools/issue_triage.py verify` and `uv run python tools/issue_triage.py render`, and never edit `known-issues.md` by hand. To list the whole set: `rg -n "discovered_during: sample-seed-dataset" project configuration/context/known-issues.yaml`.

**Triage rule.** Decide one of two outcomes for each issue as soon as it is found:

- **Vital to continuation:** the issue blocks the current milestone's acceptance. Fix it in the same session. Bug-fix TDD applies: first a failing test that reproduces it, then the fix.
- **Not vital:** log it with a reproduction receipt (the `$env:TEMP/zorivest/` redirect file of the command that shows it) and keep entering. Mark the affected task row `[B]` only when that row is genuinely blocked under the evidence-gated `[B]` rule. A logged issue that does not block the row does not make it `[B]`.

**Session-spawn threshold.** When ≥5 logged non-vital issues in the `sample-seed-dataset` set share a component, or any critical issue is logged, the closeout handoff proposes a dedicated remediation session for that cluster. It names the issue ids and the shared component. It does not fix them inside the authoring session.

---

## Never

- **Never write through REST.** Every write goes through the MCP tools of the nine MCP-exercised entity classes (see the work-unit table), so the dataset exercises the same validation a user's entry does. REST is used only for `GET` health and read checks.
- **Never write to SQLite directly.** Do not open the authoring database for writing with `sqlite3`, SQLAlchemy or any other client. Read-only (`mode=ro`) reads for verification are fine.
- **Never use import actions.** No `zorivest_import` actions (JSON validate/stage/commit, broker sync, Flex backfill) and no TWS connection. A sample database refuses them (D10a), and imported rows would not be authored data.
- **Never delete or overwrite authored rows** as part of a resume. Reconcile adopts or stops; it does not repair. The only sanctioned delete is the captured, checkpointed [D8g correction cycle](#correcting-an-execution-d8g).
- **Never author into a personal or production database.** Always verify the health identity first.
