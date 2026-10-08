# IBKR Flex Backfill Guide

> **Purpose:** Import Interactive Brokers trades from earlier days into Zorivest. TraderWorkstation (TWS) only reports executions since midnight, so older fills come from a Flex Query through IBKR's Flex Web Service. You set this up once; after that each backfill is two dates and a button.

> **Last Updated:** 2026-09-14

---

## What You Need

| Item | Where it comes from | Where it goes in Zorivest |
|------|---------------------|---------------------------|
| **Flex Web Service token** | IBKR Client Portal → Flex Queries → gear on the **Flex Web Service Configuration** panel | Settings → Broker Integrations → Interactive Brokers → Flex Query backfill |
| **Flex Query ID** | The **Query ID** row in your Activity Flex Query's details (the blue **(i)** icon) | Settings → Broker Integrations → Interactive Brokers → Connection Settings → Flex Query ID |
| **A linked IBKR account** | Your IBKR account number (for example `U1234567`, or `DU…` for paper) | Settings → Broker Integrations → Interactive Brokers → Account Linking |

The token is stored encrypted and is never shown again after you save it. The Query ID is not secret.

---

## Step 1: Turn On the Flex Web Service and Generate a Token

1. Log in to **IBKR Client Portal**.
2. Open **Performance & Reports → Flex Queries**. (On some layouts: **Menu** at the top left **→ Reporting → Flex Queries**.) The page has tabs along the top: Statements, **Flex Queries**, Tax Documents, Third-Party Reports, Transaction History.
3. In the **right-hand column** of the Flex Queries tab, find the **Flex Web Service Configuration** panel and click the **gear** icon at its **top right**. The **Configure Flex Web Service** page opens.
4. At the **top left**, tick **Flex Web Service Status**.
5. Click **Generate New Token** (centre of the page). The **Current Token** section now shows the token, its **Activation Period** (when it expires) and **Valid for IP Address**.
6. Copy the token. Keep it private: anyone with the token and your Query ID can download that report.
7. Click **Save** at the **bottom right**. A green banner confirms: *You have successfully updated your token for Flex Web Service.*

> **IP restriction:** If **Valid for IP Address** shows an address, IBKR only accepts the token from that address. Backfills from any other network (a different Wi-Fi, a VPN, a changed home IP) fail with ErrorCode 1013. Leave it blank unless you need the restriction.

> **Tip:** Generating a new token replaces the old one. After generating, save the new token in Zorivest (Step 5) or backfills will fail.

---

## Step 2: Create an Activity Flex Query

1. Go back to the **Flex Queries** tab. In the **Activity Flex Query** panel (left column, below **Configure with AI**), click **+** at the panel's **top right**, next to **?**. Its tooltip reads **Create**.
2. Enter a **Query Name**, for example `Zorivest Backfill`.
3. Under **Sections**, the sections are a two-column grid of buttons. Click **Trades** in the **right column**, between **Statement of Funds** and **Transaction Fees**.
4. The Trades dialog opens. In the **Options** grid at the top, select **Execution** (right column, top row). A ✓ appears on it. Leave **Symbol Summary**, **Asset Class**, **Order**, **Closed Lots** and **Wash Sales** unselected.
5. Below the Options grid, tick the fields in the next table. You can use **Select All** and then untick what you don't want, but the required fields must stay ticked.
6. Scroll to the bottom of the dialog and click **Save**, then continue down the query form. (The **X** at the dialog's top right closes it without saving.) Ticks in the Trades dialog are kept only once both the dialog and the query are saved.

### Required Fields

Zorivest refuses a statement that is missing any of these, and the error names the field to add. The table lists the ten fields of a working query, labeled and ordered as the Trades dialog shows them.

| Field (as labeled in Client Portal) | Why Zorivest needs it |
|-------------------------------------|-----------------------|
| **Account ID** | Matches the trade to your linked account |
| **Symbol** | The instrument |
| **Quantity** | Shares or contracts |
| **IB Commission** | Fees charged on the fill |
| **Buy/Sell** | Trade direction |
| **IB Execution ID** | Unique fill identity, used to skip duplicates |
| **Asset Class** | Stock, option, future, etc. |
| **Date/Time** | The date and time of the fill |
| **TradePrice** | Fill price (Client Portal writes it without a space) |
| **Trade Date** | The day of the fill |

> **Date/Time or Trade Time:** Zorivest reads the fill time from **Date/Time**. If you leave **Date/Time** unticked, tick both **Trade Date** and **Trade Time** instead.

> **Not substitutes:** **Orig Trade Price**, **Orig Trade Date** and **Orig Trade ID** describe the original trade behind a correction, not the fill. Ticking them does not replace **TradePrice** or **Trade Date**.

### Recommended Fields

| Field | Why |
|-------|-----|
| **Currency** | Correct currency for non-USD trades |
| **FX Rate To Base** | Converts non-USD trades to your base currency |
| **Multiplier** | Correct values for options and futures |
| **Order Reference** (`ibOrderRef`) | Links partial fills of the same order. IBKR only fills this field for single-day queries, so a multi-day backfill may leave it blank; that is harmless |

---

## Step 3: Delivery and General Configuration

Still on the query form, scroll down past **Sections**.

### Delivery Configuration

This block is marked *Applicable for Email, FTP and Flex Web Service*.

| Setting | Choose |
|---------|--------|
| **Accounts** | The account(s) you will backfill |
| **Models** | Optional |
| **Format** | **XML** |
| **Period** | **Any preset.** Zorivest sends its own From/To dates with every request, and IBKR uses those dates instead of the Period you pick here. Choose whatever the list offers, for example Last Business Day, Last 30 Calendar Days or Year to Date. |

### General Configuration

| Setting | Choose | Why |
|---------|--------|-----|
| **Profit and Loss** | Default | Does not affect the import |
| **Include Offsetting Trade/Cancel Pairs?** | **No** | With Yes, IBKR lists both a canceled trade and the row that cancels it. Zorivest imports each row as a fill, so canceled trades would appear as real ones. |
| **Include Currency Rates?** | Either | Does not affect the import |
| **Include Audit Trail Fields?** | Either | Does not affect the import |
| **Display Account Alias in Place of Account ID?** | **No** | With Yes, rows carry your alias instead of the account number, no row matches the linked account, and nothing is imported. |
| **Breakout by Day?** | **No** | Zorivest reads one statement for the whole window |
| **Date Format** | `yyyyMMdd` (keep the default) | The format Zorivest reads |
| **Time Format** | `HHmmss` (keep the default) | The format Zorivest reads. `HH:mm:ss` also works |
| **Date/Time Separator** | `; (semi-colon)` (keep the default) | The format Zorivest reads |

Continue to **Review Your Activity Flex Query**, check the summary, then click **Create** at the **bottom right**. (**Back** at the bottom left returns to the form.)

### Fixing an Existing Query

If your query is missing a required field, you don't need a new query or a new Query ID. On the **Flex Queries** tab, click the **pencil** icon on the right of the query's row (or open the **(i)** details and click **Edit** at the bottom right). Click **Trades**, tick the missing fields, and click **Save** at the bottom of the Trades dialog. Then continue through the form and save the query. The Query ID stays the same.

Reopen the query and click **Trades** to check the ticks stayed. If you close the dialog with **X** or leave the form before saving the query, the changes are lost and the next backfill reports the same missing fields.

---

## Step 4: Copy the Query ID

1. On the **Flex Queries** tab, find your query in the **Activity Flex Query** panel.
2. Click the blue **(i)** icon on the **left** of the query's row.
3. The details dialog opens. The **first row** is **Query ID**, digits only, for example `1234567`. Copy it.

(The icons on the right of the row are **pencil** = edit, **X** = delete and **→** = run the query now. Zorivest doesn't need you to run it.)

---

## Step 5: Save Both Values in Zorivest

1. Open **Settings → Broker Integrations** and select **Interactive Brokers**.
2. Under **Connection Settings**, enter the number from Step 4 in **Flex Query ID**.
3. Under **Flex Query backfill 🔒**, paste the token from Step 1 into **Flex Web Service token**.
4. Click **Save Settings** at the bottom of the page.
5. Check that the token status next to the token field reads **✅ Token stored**. If it reads **⚠️ Token not set**, the token was not saved: paste it again and click **Save Settings**.

> **Note:** The token field is cleared after a successful save. That is expected; the status label is the confirmation.

---

## Step 6: Link the Account

Zorivest only imports trades for linked accounts, so a statement is never attributed to a guessed account.

1. On the same Interactive Brokers settings page, open **Account Linking**.
2. Link your IBKR account number to the Zorivest account the trades belong to.

---

## Step 7: Run the Backfill

1. Open **Import** and find the **Interactive Brokers** card.
2. If the card shows a **⚠️** setup warning, it names what is missing. Click **Open settings →** and finish that step.
3. Pick **From** and **To** dates. The window can be up to **365 days**. IBKR builds each day's Activity statement after the market closes, and answers a window that starts or ends on a weekend, or ends today, with ErrorCode 1003. Zorivest adjusts the window's ends for you: a **From** on a weekend is moved forward to Monday, and a **To** of today, a weekend or a US market holiday (NYSE calendar) is moved back to the previous business day. For example, a **To** of today on a Monday usually becomes the Friday before, or the Thursday when that Friday was a holiday. A **From** on a holiday is sent as picked. The card reports the dates actually imported. A window with no business day before today (for example only today, only a weekend, or only a holiday) cannot be run. Today's trades come in through the TWS connection instead.
4. Click **📥 Backfill from Flex**.

IBKR can take from a few seconds to a few minutes to generate a statement, especially for long windows. When it finishes, the card shows how many trades were imported. Trades that are already in Zorivest are skipped, so running the same window twice is safe.

For more than 365 days of history, run one backfill per year.

---

## Troubleshooting

| Message on the card | Cause | Fix |
|---------------------|-------|-----|
| **No Flex token configured for ibkr** | The token was never saved | Step 5. Confirm the status reads **✅ Token stored** |
| **No Flex Query ID configured for ibkr** | The Query ID field is empty | Step 5 |
| **No linked IBKR accounts** | No account linked | Step 6 |
| **ErrorCode 1003: Statement is not available** | IBKR has no statement for the window yet. Zorivest already moves a **To** of today, a weekend or a US market holiday back to the previous business day, so usually the window ends on the most recent business day and IBKR has not finished that day's statement; how long that takes after the close varies. IBKR may also have no statement for a day closed on a non-US exchange | Import recent trades through the TWS connection, or pick an earlier business day as **To** (Step 7). Moving **To** from a Monday to the Sunday before still sends the same Friday. If the window is already in the past, check that the query's **Accounts** include the linked account (Step 3) |
| **ErrorCode 1012** | Token expired (past its **Activation Period**) | Step 1 (**Generate New Token**), then Step 5 |
| **ErrorCode 1015** | Token is invalid (mistyped, or replaced by a newer one) | Copy the current token again, then Step 5 |
| **ErrorCode 1013** | Token is restricted to a different IP address | Run the backfill from the address in **Valid for IP Address**, or clear that restriction (Step 1) |
| **ErrorCode 1014** | IBKR does not recognize the query: the Query ID is wrong, or the query was deleted | Recheck the Query ID from Step 4, then Step 5 |
| **ErrorCode 1020** | IBKR could not validate the request. The message includes IBKR's own detail | Recheck both the token and the Query ID saved in Step 5, and read IBKR's detail in the message |
| **…missing *N* required fields: *Label* (*field*), …** | The listed fields are not in the statement IBKR sent. Every missing field is named at once | Tick each listed field in the Trades section, click **Save** at the bottom of the Trades dialog, then save the query (see **Fixing an Existing Query**). If they already look ticked, the edit may not have been saved: open the query again and check |
| **…With no dateTime value, Trade Date and Trade Time are both required** | The statement has no **Date/Time**, and **Trade Date**, **Trade Time** (or both) are missing too | Tick **Date/Time** (or both **Trade Date** and **Trade Time**) in the Trades section and save as above (see **Fixing an Existing Query**) |
| **…statement row N … could not be read** | A value in the statement is in an unexpected format | Check **Date Format**, **Time Format** and **Date/Time Separator** are at their defaults (Step 3) |
| **Still generating the Flex statement after …s** | IBKR is slow for this window | Wait a minute and try again, or use a shorter window |
| **ErrorCode 1018** | Too many requests to IBKR | Wait about a minute and try again |
| **📥 Backfill from Flex** stays greyed out | The dates are missing, From is after To, To is after today, the window is longer than 365 days, or it holds no business day before today (only today, a weekend or a US market holiday) | Fix the dates; split long ranges into yearly windows |
| **Backfilled 0 trades** | Either every fill in the window is already in Zorivest (for example, you ran this window before), or the statement had no executions for the linked accounts | If you have not imported this window before: confirm the dates, that the query's **Accounts** include the linked account, and that **Display Account Alias in Place of Account ID?** is **No** (Step 3) |

---

## IBKR Reference

These IBKR pages describe the same screens in more detail. IBKR may move or rename them; the steps above are the ones Zorivest supports.

- [Flex Web Service](https://www.ibkrguides.com/brokerportal/performanceandstatements/flex3.htm): enabling the service and generating a token
- [Activity Flex Query](https://www.ibkrguides.com/orgportal/performanceandstatements/activityflex.htm): creating a query, sections, delivery and general configuration
- [Flex Queries](https://www.ibkrguides.com/orgportal/performanceandstatements/flex.htm): the Flex Queries page overview
- [Trades section fields](https://www.ibkrguides.com/reportingreference/reportguide/tradesfq.htm): definitions of every Trades field
- [Flex Web Service API](https://www.interactivebrokers.com/campus/ibkr-api-page/flex-web-service/): the request format and error codes
