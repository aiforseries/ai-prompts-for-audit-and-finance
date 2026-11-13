# Excel and finance team prompts

Copy-ready prompts from *Excel + AI for Finance Teams* by Chandan Gupta, for an AI assistant such as Claude Code working in a folder on your own computer. The file names, columns, thresholds, dates and names come from the book's worked examples, where all companies, people and figures are made up. Replace them with your own. Each prompt is followed by a check to do before you rely on the result.

## Keeping data out of the chat

### The pattern in one prompt

**When to use:** The basic pattern for any workbook: let a script look at the layout, then do the work locally and report only totals.

```text
AR_Aging_Jun26.xlsx is our June AR aging. Don't read the file into our chat. First, write and run a short script that prints only the sheet names, the header row and the first 5 rows of the main sheet. Then write a Python script that runs here and saves Past_Due_Jun26.xlsx with every customer whose balance over 90 days is above $5,000: customer, total balance, over 90, last payment date. Show me only the count of customers, the total open balance and the total over 90 days, so I can tie them to the aging's grand total.
```

**Check:** Tie the count and totals it reports to the aging's own grand total before you use the output.

### Check figures without reading back

**When to use:** Whenever the assistant writes Excel formulas into an output file.

```text
When you write formulas into the output workbook, also compute the same totals in Python and print them, so I can compare them with what Excel shows when I open the file. Don't read the output back to check your own formulas: openpyxl won't have calculated them.
```

**Check:** Open the file in Excel and compare its totals with the totals Python printed.

## Profiling and cleaning exports

### Profile three exports before cleaning anything

**When to use:** Before cleaning exports from different systems: get a factual profile of each file first.

```text
In 01_Input there are three sales exports for March 2026: NS_Sales_Mar26.csv, QBO_Sales_Mar26.xlsx and Xero_Sales_Mar26.csv. Don't read them into our chat. Write a Python script that runs here and saves Profile_Mar26.xlsx with one sheet per file showing:
1. Row count, the row where the headers start, and any title or blank rows above it.
2. For each column: header text, how many values look like dates, numbers, text or blanks, and 3 example values. Mask anything in a customer or name column as XXXX.
3. For date columns, whether the values could be month-first or day-first, and how many are ambiguous (day 12 or less).
4. Any rows whose text contains Total, Subtotal or Grand, with their row numbers.
5. The sum of every numeric-looking column, read as numbers where possible.
Print only the row counts and a one-line summary per file.
```

**Check:** Open each file in a text editor or Excel and confirm the header row and row count it reported.

### Clean and consolidate into one table, with a control sheet

**When to use:** Cleaning and stacking several exports into one table, with proof that every input row is kept or deliberately removed.

```text
Using what we learned in Profile_Mar26.xlsx, write clean_sales.py that runs here and builds Sales_Consolidated_Mar26.xlsx. Rules:
1. NetSuite dates are month-first. Xero dates are day-first. QuickBooks dates may be Excel serial numbers; convert them. Fail loudly if any date falls outside March 2026.
2. Amounts: strip currency symbols and commas, turn a trailing minus or parentheses into a negative, and convert to numbers. List any value you could not convert instead of guessing.
3. QuickBooks: combine the two header rows into one, and drop rows containing Total or Subtotal, but copy them to a Removed Rows sheet with their source row number.
4. Stack all three with the same columns: Source, Entity, Invoice No, Invoice Date, Customer, Currency, Amount (local), FX Rate, Amount (USD). FX Rate comes from FX_Rates_Mar26.xlsx by currency, and Amount (USD) must be an Excel formula.
5. Control sheet: for each source, input rows, rows kept, rows removed, and the amount total of the input, the kept rows and the removed rows, with a check that input = kept + removed.
Don't change anything in 01_Input. Show me only the Control sheet.
```

**Check:** Compare each source's input amount total with the export's own total or the ERP report.

### Find near-duplicate customers

**When to use:** Finding likely duplicate customers whose names differ slightly.

```text
Customer_List.xlsx has duplicate customers under slightly different names (Inc vs Inc., extra spaces, & vs and). Without changing the file, write a script that groups likely duplicates, shows a similarity score and the fields that match (address, tax ID, email domain), and saves Customer_Duplicates.xlsx for me to review. Don't merge anything.
```

**Check:** Review each group yourself. Similar names can be different legal entities. Nothing should be merged by the script.

## Bank reconciliation

### Monthly bank rec, standard layout

**When to use:** A monthly bank reconciliation in a standard layout, matched in rounds.

```text
Bank_Jun26_4471.csv is the operating account statement and NS_Cash_Jun26.xlsx is GL account 1010 for June. Write a script that matches them in rounds (exact amount and date; amount with dates within 3 days; one bank deposit equal to the sum of several GL receipts on the same or previous day) and builds Bank_Rec_Jun26.xlsx with the standard layout: balance per bank, deposits in transit, outstanding checks, other reconciling items, balance per books, and a difference cell that must be zero. Show me only the rec page.
```

**Check:** The difference cell is zero, and the balance per bank and balance per books agree to the statement and the GL.

### Find stale reconciling items

**When to use:** Finding reconciling items that carry over from month to month.

```text
Here is last month's Bank_Rec_May26.xlsx and this month's Bank_Rec_Jun26.xlsx. List every reconciling item that appears in both months, with its amount and age, and flag outstanding checks older than 90 days for follow-up.
```

**Check:** Every stale item has an owner and a next step before the reconciliation is signed off.

## Subledger and vendor reconciliations

### AR aging against the GL control account

**When to use:** Proving the AR subledger agrees with the GL control account, and bridging any gap.

```text
Compare NS_AR_Aging_Jun26.xlsx with account 1200 in NS_GL_Detail_Jun26.csv. Don't read customer names into our chat. Write a script that runs here and builds AR_to_GL_Jun26.xlsx:
1. Aging total by bucket and in total; GL balance for 1200 at June 30.
2. From the GL detail, list every June posting to 1200 that did not come from an invoice, a payment or a credit memo (use the transaction type column), with the journal number, preparer if available, and memo.
3. List any customer with a credit balance in the aging, and any unapplied cash.
4. A Bridge sheet: aging total, less or plus each listed item, equals the GL balance, using formulas, with the unexplained remainder shown separately even if it is zero.
```

**Check:** The aging date: an aging run on July 2 isn't the June 30 balance.

### A vendor statement against the AP ledger

**When to use:** Reconciling a vendor statement with your AP ledger when the two sides write invoice numbers differently.

```text
Reconcile Company_Statement_Jun26.xlsx (the vendor's statement) with NS_AP_Company_Jun26.xlsx (our AP ledger) with a script that runs here.
1. Normalize invoice numbers on both sides: upper case, strip the BSS- prefix and leading zeros. Keep the original numbers in their own columns.
2. Match invoices on normalized number and amount; payments on amount and dates within 7 days.
3. Sheets: Matched, On Statement Only, In Our Ledger Only, Amount Differences, and a Summary that starts from their balance and walks to ours, item by item, with formulas.
4. For On Statement Only, add an Age (days) column from the statement date to June 30.
```

**Check:** Send the vendor the list of differences, not your ledger.

## Month-end close

### An accrual list from open POs and receipts

**When to use:** Building an accrual review list from open POs and early-next-month bills.

```text
Files: NS_Open_PO_Jun26.csv (open PO lines at June 30) and NS_AP_Bills_Jul1-5.csv (bills entered July 1 to 5). Show me the headers and the row count of each first. Then write a script that runs here and builds Accruals_Jun26.xlsx:
1. Received Not Billed: lines where quantity received is more than quantity billed. Amount = (received - billed) x unit price, as an Excel formula.
2. For each of those, look for a July bill for the same PO and amount within 2%. If found, show the bill number and date in a Billed In July column.
3. Open Past Due: lines not received whose expected date is before June 30, for me to ask about.
4. Mark lines at or above $2,500 as In Scope. Keep the rest on the sheet, not deleted.
5. A Summary by GL account and by vendor for the In Scope lines, using SUMIFS.
6. A Proposed Journal sheet: debit by GL account, credit to Accrued Liabilities 2150, with a Reviewer and Approved column left blank. Don't post or create anything in NetSuite.
```

**Check:** Total the Proposed Journal and compare it with the In Scope total.

### Flux analysis with draft comments

**When to use:** Flux analysis against prior month and budget, with the driving transactions and factual draft comments.

```text
Files: NS_TB_May26.xlsx, NS_TB_Jun26.xlsx, Budget_FY26.xlsx (Account, then one column per month) and NS_GL_Detail_Jun26.csv. Use a script that runs here; don't read the GL detail into our chat. Build Flux_Jun26.xlsx:
1. Flux sheet: every P&L account with June, May, Budget June, change vs May and vs budget in $ and %, as formulas. Flag In Scope where the change is over $25,000 AND over 10%, for either comparison.
2. Drivers sheet: for each In Scope account, the 5 largest June GL lines by amount (date, vendor or customer, memo, amount) and the same month's largest lines in May for comparison.
3. A Draft Comment column: one factual sentence describing what the drivers show, such as which vendor or which entries account for most of the change. Don't state a business reason unless a memo says it in words. Start each draft with "DRAFT:".
4. A Final Comment column left empty for me, and a Reviewed By column.
```

**Check:** The thresholds in the In Scope formula are the ones in your policy, and use AND, not OR.

### Balance sheet scan

**When to use:** A quick balance sheet scan before the close is signed off.

```text
Compare NS_TB_Jun26.xlsx with NS_TB_May26.xlsx and list balance sheet accounts that changed sign, accounts with a balance this month and none last month (or the reverse), and accounts with no reconciliation file in Recs_Jun26\ whose balance is over $10,000.
```

**Check:** Follow up every account listed. A sign change often points to a misposting or a missing entry.

## Cash flow and budgets

### A 13-week cash flow tied to the revolver

**When to use:** A 13-week cash flow with revolver draws and headroom, where every number is a formula.

```text
Build CashFlow_13wk_Jul26.xlsx with weeks starting Monday July 6, 2026. Use a script that runs here; show me only headers and totals of the aging files, not customer or vendor names.
1. An Assumptions sheet I will fill in: collection % by AR aging bucket and week, payment timing by AP bucket, other receipts and payments by week, minimum cash $1.5 million, revolver limit $25 million. Put my starting values in as placeholders and color all input cells blue.
2. A Cash Flow sheet: opening cash, receipts (AR collections, other), payments (AP, payroll from the calendar, interest and loan payments from the debt schedule, capex, taxes), net flow, closing cash. Every number must be a formula pointing at Assumptions or a source sheet.
3. Revolver rows: draws needed to keep closing cash at the minimum, cumulative balance, and headroom against the limit.
4. A Checks sheet: AR collected in the 13 weeks does not exceed AR at June 30 plus forecast billings; payroll weeks match the calendar; opening cash equals the bank rec balances. Highlight any week where closing cash before draws is below the minimum.
```

**Check:** Click through five cells in the Cash Flow sheet: each should point at an assumption or a source, never at a typed number.

### Budget vs actual by department

**When to use:** Budget vs actual by department that survives new accounts.

```text
Build a BvA for June and year to date from NS_TB_Jun26.xlsx and Budget_FY26.xlsx by department: actual, budget, variance in $ and %, with favorable and unfavorable shown correctly for revenue and expense lines. Use SUMIFS on a mapping sheet so the layout survives new accounts, and list any TB account not in the mapping.
```

**Check:** The list of unmapped TB accounts is empty, and total actuals agree to the trial balance.

## Reviewing spreadsheet models

### A full mechanical review of the inherited model

**When to use:** A mechanical review of an inherited model, without changing the original file.

```text
01_Input/Pricing_Forecast_v19_FINAL2.xlsx is a forecast model I inherited. Do not modify it, and do not read its contents into our chat. Write a Python script (openpyxl, formulas not values) that runs here and saves 02_Work/Model_Review.xlsx with these sheets:
1. Overview: each sheet with its visibility (visible, hidden, very hidden), used range, formula count and typed-number count.
2. Issues: Sheet, Cell, Issue Type, Formula or Value, Neighbor Formula, Severity (High, Medium, Low), Comment.
3. Check for: typed numbers in rows or columns that are otherwise formulas; formulas that differ from their neighbors once cell references are made relative; #REF!, #DIV/0!, #N/A and other errors; external links and the file paths they point to; defined names that are broken or point to other files; SUM and SUMIFS ranges that stop before the last row of their block; hidden rows and columns inside used ranges; and whether iterative calculation is switched on.
4. Circularity: list any chain of cells that refers back to itself, across sheets.
Show me only the Overview counts and the number of issues by type in our chat.
```

**Check:** Open six flagged cells yourself, including two High ones, and confirm the issue is real.

### Find what's hidden

**When to use:** Finding hidden sheets, rows, columns and invisible text in a workbook.

```text
Find every hidden sheet, very hidden sheet, hidden row and hidden column in Capex_Plan.xlsx, and any cell whose font color matches its fill color. For each, show whether anything visible depends on it. Save Hidden_Items.xlsx.
```

**Check:** Unhide and read every item that something visible depends on.

### Diff two versions of a model

**When to use:** Comparing two versions of a model formula by formula.

```text
Compare Pricing_Forecast_v19_FINAL2.xlsx and Pricing_Forecast_v20.xlsx cell by cell, formulas not values, and save Model_Diff.xlsx with Sheet, Cell, v19, v20. Add a count of changes by sheet. Every change should appear in v20's Fix_Log sheet; list any that don't.
```

**Check:** Every change that is not in the fix log gets an explanation before the model goes out.

## Excel formulas

### Write and explain a formula

**When to use:** When you want a formula you can maintain yourself, not a script.

```text
I need an Excel formula, not a script. In AR_Aging.xlsx, column D has the invoice date and column F the amount. Write a formula for column H that puts each row in an aging bucket (Current, 1-30, 31-60, 61-90, Over 90) as of the date in cell B1. Explain each part of the formula and give me two test rows to check it.
```

**Check:** Work the two test rows by hand and confirm the formula returns the same bucket.

## Checking commentary and journals

### Back every sentence with a cell

**When to use:** Before a commentary or report pack is sent: tie every sentence to a cell.

```text
Here is the draft commentary in Flux_Commentary_May26.docx. For each sentence, find the cell in Flux_May26.xlsx that supports it and give the sheet, cell and value. Mark each sentence Supported, Partly supported or Not supported by the workbook. Do not rewrite anything, and don't use any knowledge outside the two files.
```

**Check:** Fix or remove every sentence marked Not supported before the pack goes out.

### Prepare, don't post

**When to use:** Preparing a journal import file from a reviewed schedule, without posting anything.

```text
From Accruals_Jun26.xlsx (reviewed), prepare a journal entry import file in the layout of JE_Import_Template.csv: one line per accrual, debit and credit accounts from the mapping sheet, memo text "June accrual - <vendor>", and a reversing date of 7/1/2026. Add a check that debits equal credits. Save as JE_Accruals_Jun26_for_review.csv. Don't connect to anything.
```

**Check:** Debits equal credits, and a person reviews the file and posts it in the ERP.

---

More prompts, worked walkthroughs and answer keys: *Excel + AI for Finance Teams* (https://aiforseries.github.io/excel-ai-for-finance/)
