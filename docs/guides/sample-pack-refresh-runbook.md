# Sample-Pack Refresh Runbook

> **Purpose:** Keep the built-in Improving Investor sample and its user guide current. This runbook covers the annual freshness review, the version bump rules, re-recording the manifest and its hashes, and regenerating the guide and its channels. A refresh never changes or deletes a sample database that a user already has.

> **Last Updated:** 2026-10-07

---

## Scope

- **What this covers:** the annual review of the frozen sample pack and a new `dataset_version` of the same pack. It also covers the new manifest `sha256`, the acceptance receipt, and the regenerated guide, screenshots and channels.
- **What a refresh reads:** the shipped manifest `tests/fixtures/sample_pack/improving_investor_v1/manifest.json` and its `acceptance-receipt.json`, the user guide in `docs/user-guide/`, and the channel files in `docs/user-guide-channels/`.
- **Where a refresh writes:** a work folder outside the shipped trees, until the new version ships. The shipped trees are `tests/fixtures/sample_pack/` and `ui/resources/sample-pack/`.
- **What this does not cover:** entering new sample data. Use the authoring runbook for that, then continue here with its exported manifest.
- **Placeholders:** `<work>` is your work folder, `<version>` is the new `dataset_version`, and `<date>` is a `YYYY-MM-DD` date.

---

## Cadence

The review is annual. It falls due on the first anniversary of the manifest's `freshness_date`. A `freshness_date` of 29 February falls due on 28 February of the next year. The review stays due until a version with a new `freshness_date` ships.

1. <!-- step: refresh-01 --> Check whether the review is due: `uv run python tools/sample_pack_refresh.py due`. The tool prints `freshness_date=<date> review_due=<date> status=due` or `status=not_due`. Add `--today <date>` to check another day, or `--manifest <path>` to check another manifest.

---

## Freshness review

The review starts from the shipped pack. These steps only read files.

2. <!-- step: refresh-02 --> Check that the shipped manifest still matches its acceptance receipt: `uv run python tools/sample_pack_freeze.py --check`.
3. <!-- step: refresh-03 --> Check that the user guide is still valid and bound to the shipped pack: `uv run python tools/validate_user_guide.py`. The pack line in `docs/user-guide/README.md` must equal the manifest's `dataset_id` and `dataset_version`.
4. <!-- step: refresh-04 --> Check that the channel files still equal a fresh render: `uv run python tools/render_user_guide_channels.py --check`.

Then compare the guide with the current app. Look for screens that changed, labels that changed, and facts in the pack that are now wrong. Each change you find goes into the new version.

---

## Version bump rules

Any change to the canonical manifest bytes changes the manifest `sha256`, so it needs a new `dataset_version`. A version never names two different sets of bytes. Pick the level from the largest change:

- **Patch:** text-only changes to the dataset card or labels. The change adds, deletes or renames no row, and it changes no figure. A change of `freshness_date` alone is also a patch.
- **Minor:** the change adds rows or changes values, and it keeps every existing id.
- **Major:** the change deletes or renames an id, or it changes the manifest `schema_version`.

The `dataset_id` never changes inside a major line.

The first patch candidate is the dataset card's `reset_instructions` text. It reads "Delete the sample database and re-apply the manifest." That is not what a reset does. A reset asks for a confirmation and keeps the previous files until the new copy is in place. The frozen pack can change this text only in a new version.

---

## Re-recording the manifest and hashes

Do these steps in `<work>`. Never edit a file in a shipped tree by hand.

5. <!-- step: refresh-05 --> Put the changed manifest in `<work>/manifest.json`. For a data or card change, use the authoring tool. Run `uv run python tools/sample_authoring.py export --database <path> --source <authoring-source.json> --out <work>/manifest.json`. For a change of `freshness_date` alone, copy the shipped manifest to `<work>/manifest.json`.
6. <!-- step: refresh-06 --> Set the new version and record the new hash: `uv run python tools/sample_pack_refresh.py rehash --manifest <work>/manifest.json --version <version> --out <work>/manifest-<version>.json --freshness-date <date>`. The tool prints `rehash_status=PASS version=<version> sha256=<hash>`. It refuses a version that is not greater than the current one. It also refuses an output file that exists, an output in a shipped tree, and an input with a stale `sha256`.
7. <!-- step: refresh-07 --> Build the template and its descriptor `pack.json` from the new manifest: `uv run python tools/build_sample_pack_template.py --manifest <work>/manifest-<version>.json --out <work>/sample-pack`. Then run the same command with `--check` added. The check reads the template's identity row and compares it with `pack.json` and the manifest.
8. <!-- step: refresh-08 --> Write the acceptance receipt for the new manifest: `uv run python tools/sample_pack_freeze.py --write --accepted <work>/manifest-<version>.json --out <work>/acceptance-receipt.json`. Then run the same command with `--check` in place of `--write`.
9. <!-- step: refresh-09 --> Ship the new version. Replace the shipped `manifest.json` and `acceptance-receipt.json` with the files from `<work>`, in a reviewed change. Do this step only when the conditions in [Existing sandboxes](#existing-sandboxes) allow it. The package build then makes the shipped template in `ui/resources/sample-pack/` from the new manifest.

---

## Regenerating the guide

Regenerate the guide after a new version ships, or after the app's screens change. Steps 10, 11, 13, 14 and 16 write to the repository.

10. <!-- step: refresh-10 --> Set the pack line on the line after the title of `docs/user-guide/README.md` to `<!-- pack: <dataset_id>@<dataset_version> -->`, with the values of the shipped manifest.
11. <!-- step: refresh-11 --> Capture the screenshots again. Delete `docs/user-guide/images/` first, so no old image stays. Run `npm run build` in `ui/`, then run the ten capture specs `ui/tests/e2e/guide-*.spec.ts` with `npx playwright test --reporter=json`. Set `ZORIVEST_GUIDE_CAPTURE=1`, `ZORIVEST_GUIDE_CAPTURE_LOG=<log file>` and `PLAYWRIGHT_JSON_OUTPUT_NAME=<results file>`. Run the capture against a temporary data folder, so it never opens your own data. Then run `uv run python tools/finalize_guide_captures.py --log <log file> --results <results file>`. It deletes the metadata from each image and writes `docs/user-guide/images/capture-receipt.json`.
12. <!-- step: refresh-12 --> Check the guide: `uv run python tools/validate_user_guide.py`. It checks the images, the capture receipt and the pack line against the shipped manifest.
13. <!-- step: refresh-13 --> Render the channels: `uv run python tools/render_user_guide_channels.py --write`. If a chapter changed, update its narration file in `docs/user-guide-channels/storyboard-narration/` first. Each claim in a narration file cites a sentence of its chapter.
14. <!-- step: refresh-14 --> Review the narration claims again for every changed narration file, and record the result in `docs/user-guide-channels/narration-claim-review.md`. A reviewer who did not write the narration judges each claim against the chapter text. Three false claims go into the review without a mark. If the reviewer accepts one of them, the review is void, so do it again with three new false claims. Fix a rejected claim in the narration, never in the chapter.
15. <!-- step: refresh-15 --> Check the channels: `uv run python tools/render_user_guide_channels.py --check`. The check fails when a narration file changed after its review.
16. <!-- step: refresh-16 --> Publish the changes: `uv run python scripts/render_public_docs.py --update-entries <path> …`, with every changed chapter, image and guide. This updates `release/public-docs-source-map.json`, the public copies and the public file list. Then check the map: `uv run python scripts/render_public_docs.py`.

---

## Existing sandboxes

A sample sandbox is the sample database that a user opened in the app. A new `dataset_version` never changes or deletes an existing sandbox.

- Provisioning with the descriptor of a new version refuses when the sandbox already exists.
- The app refuses a reset onto a pack with a different manifest hash, and the sandbox stays as it was.
- A reset asks the user for a confirmation. It keeps the previous files until the new copy is in place.

No new version of an existing sample pack ships until the app's reset accepts the new version. Until then, the steps that make a new version run only on copies in `<work>`.

17. <!-- step: refresh-17 --> Prove that a new version leaves an existing sandbox unchanged: `uv run pytest tests/integration/test_sample_pack_version_bump.py -q`. The test makes a sandbox from the shipped pack and a newer version in a temporary folder. It then requires both the provisioning and the reset with the newer version to refuse, with no change to the sandbox.

---

## Never

- **Never edit a shipped tree by hand.** The shipped trees change only by step 9, in a reviewed change. `rehash` refuses an output in a shipped tree.
- **Never use one `dataset_version` for two different manifests.** Each set of manifest bytes gets its own version.
- **Never change `dataset_id` inside a major line.**
- **Never ship a new version of an existing sample pack before the app's reset accepts it.**
- **Never change or delete a user's sample sandbox as part of a refresh.**
- **Never edit a chapter to make a narration claim pass.** Fix the narration and review it again.
- **Never edit the generated channel files, the public copies or `release/public-docs-source-map.json` by hand.** Regenerate them with their tools.
