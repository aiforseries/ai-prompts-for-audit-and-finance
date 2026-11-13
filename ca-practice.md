# CA practice prompts (India)

Copy-ready prompts from *AI for CAs* by Chandan Gupta, for an AI assistant such as Claude Code working in a folder on your own computer. The file names, columns, thresholds, dates and names come from the book's worked examples, where all companies, people and figures are made up. Replace them with your own. Indian law, rates, thresholds and due dates change: use your own masters and the current official text, as the prompts ask. Each prompt is followed by a check to do before you rely on the result.

## Keeping client data out of the chat

### Add this line to any request on client data

**When to use:** Add it to the end of any request that touches client data.

```text
Use a script for this and run it here. Do not open or print the data rows. Show me only counts, totals and the names of any files you save.
```

**Check:** The reply shows only counts, totals and file names. If it shows data rows, stop and tighten the request.

### Make a coded copy before analysis

**When to use:** Making a coded copy of a client file before any analysis, with names, PANs and bank details replaced or removed.

```text
Look only at the headers of Vendor_Payments_FY26.xlsx. Write a script that makes a copy called Vendor_Payments_coded.xlsx in which Vendor Name becomes V0001, V0002 and so on, PAN becomes P0001 and so on, and bank account numbers are removed.
Save the mapping to Key_DO_NOT_OPEN.xlsx. Run the script, print only the row count, and do not open either output file.
```

**Check:** The coded copy has the same row count as the original. Keep the key file somewhere the assistant is not working.

## Income tax: AIS, 26AS and TDS

### AIS against the books

**When to use:** Matching the AIS with the client's books before filing.

```text
In this folder I have AIS_FY2025-26.json and Tally_Ledgers_FY2025-26.xlsx for a client.
Do not open either file in full. Read only the structure of the JSON and the headers and first 5 rows of each Excel sheet.
Then write a Python script ais_recon.py that runs on this computer and:
1. Flattens the AIS into one table: information category, source name, TAN or PAN of source, date, amount, TDS.
2. Matches each AIS row to the ledger by TAN/PAN where available, then by party name, amount (within ₹10) and date (within 7 days).
3. Writes AIS_Recon.xlsx with sheets: Matched, In AIS not in books, In books not in AIS, Summary by category.
4. In the Summary, show for each category: AIS total, books total, difference.
Run the script and show me only the Summary sheet and the count of rows on each sheet.
```

**Check:** Confirm TDS credit in the computation against 26AS, not only against AIS.

### 26AS TDS credit to TDS receivable

**When to use:** Agreeing TDS credit in 26AS with the TDS receivable ledger.

```text
Compare 26AS_FY2025-26.txt (the text download) with TDS_Receivable_Ledger.xlsx. Write a local script that totals TDS by TAN in each, and lists TANs where the difference is more than ₹100, with the deductor name and both amounts.
```

**Check:** Explain each TAN difference before you rely on the TDS credit in the computation.

### TDS applicability scan across the ledger

**When to use:** Finding expense payments where TDS may have been missed, using your own TDS master for thresholds and rates.

```text
Files in this folder: Kaveri_Ledger_FY2025-26.xlsx (columns: Date, Voucher No, Ledger, Party, PAN, Debit, Credit, Narration) and TDS_Master_FY2025-26.xlsx (columns: Nature, Keywords, Single Threshold, Annual Threshold, Rate). Use only my TDS master for thresholds and rates. Do not use your own figures.
Read only the headers and 5 sample rows of each file. Then write a script tds_scan.py that runs locally and:
1. Tags each expense entry to a Nature using the ledger name and narration keywords. Leave it "Unclassified" if unsure.
2. Totals payments per party per Nature for the year.
3. Flags parties above either threshold where no TDS credit entry exists against the same voucher or party.
4. Flags entries where TDS was booked at a rate different from my master.
5. Flags parties with no PAN.
Write TDS_Exceptions.xlsx with columns: Party, PAN, Nature, Annual Total, TDS Booked, Expected TDS, Issue, Vouchers. Run it and show me the count by Issue.
```

**Check:** Check whether a lower deduction certificate, an exemption or a declaration applies before calling it a default.

## Tax audit

### Cash payments for the tax audit

**When to use:** Listing cash payments for the tax audit report.

```text
From Ledger_FY2025-26.xlsx, write a local script that lists every payment made in cash (Ledger = Cash, or narration contains 'cash'), grouped by party and date, and flags any party with more than one cash payment on the same day. Use the cash limits in my Limits.xlsx, not your own. Output Cash_Payments.xlsx with columns: Date, Party, Amount, Voucher, Same-day count, Above limit (Y/N).
```

**Check:** Confirm the limits in your limits file against the law for the year before you rely on the Above limit column.

### MSME payables split

**When to use:** Splitting payables into MSME and others, with aged MSME bills.

```text
Read Vendor_Master.xlsx (Vendor, PAN, MSME Reg No, MSME Category) and Creditors_Outstanding_31Mar26.xlsx (bill-wise). Write a script that splits outstanding payables into MSME and others, ages each bill from its bill date, and lists MSME bills older than the number of days I'll put in cell B1 of a Settings sheet. Save MSME_Payables.xlsx.
```

**Check:** Confirm MSME status from the registration details, not only from the vendor master.

## GST

### GSTR-2B against the purchase register

**When to use:** GSTR-2B against the purchase register before GSTR-3B is filed.

```text
In this folder: GSTR2B_Jul.xlsx, GSTR2B_Aug.xlsx, GSTR2B_Sep.xlsx (portal downloads) and Purchase_Register_Q2.xlsx. Do not read the files in full. Look at the sheet names, headers and first 5 rows of the B2B sheet in one 2B file and of the purchase register.
Then write a Python script recon_2b.py that runs on this computer and:
1. Combines the B2B and B2B-CDNR sheets of the three 2B files into one table.
2. Builds a match key: GSTIN + invoice number cleaned (remove the financial year in any format, remove letters and symbols, remove leading zeros) + taxable value rounded to the rupee.
3. Matches in rounds: exact invoice number, then match key, then GSTIN + taxable value within 30 days. Record the round in a column called Match Basis.
4. Treats a tax difference of up to ₹5 per invoice as matched, and flags anything larger.
Write ITC_Recon_Q2.xlsx with sheets: Matched, In 2B not in books, In books not in 2B, Amount differences, Summary by month and tax head. Run it and show me only the Summary and the row count of each sheet.
```

**Check:** Tie the 2B total in the script output to the ITC total shown on the portal summary for each month. If they differ, the script missed a sheet or a section.

### Annual tie-out: books, GSTR-1 and GSTR-3B

**When to use:** Annual tie-out of books, GSTR-1, GSTR-3B and e-invoice data before the annual return.

```text
Files: Sales_Register_FY2025-26.xlsx, GSTR1_Summary_FY2025-26.xlsx (one row per month and table), GSTR3B_FY2025-26.xlsx (one sheet per month, as downloaded) and EInvoice_IRN_List.xlsx. Read only headers and 5 rows of each.
Write a local script annual_tieout.py that builds Annual_Tieout.xlsx with:
1. Sheet Monthly: for each month, taxable value and tax (IGST, CGST, SGST, cess) as per books, as per GSTR-1 net of amendments and credit notes, as per GSTR-3B, and the differences.
2. Separate columns for exports (with and without payment of tax), domestic B2B, B2C and nil-rated or exempt supplies, as far as the source files allow.
3. Sheet IRN check: B2B invoices in the sales register with no IRN in the IRN list, and IRNs with no matching invoice in the books.
4. Sheet Notes: a list of every assumption the script made about the files.
Run it and show me the Monthly sheet totals and the count of rows in IRN check.
```

**Check:** Read the Notes sheet first. A wrong assumption about amendments or credit notes moves differences between months.

### Screening an expense ledger for blocked ITC

**When to use:** Screening an expense ledger for ITC that may be blocked, using your own master of categories and keywords.

```text
In this folder: Expense_Ledger_FY2025-26.xlsx (columns: Date, Voucher, Ledger, Party, GSTIN, Narration, Taxable, IGST, CGST, SGST) and Blocked_ITC_Master.xlsx that I maintain (columns: Category, Keywords, Exceptions to check, Notes). Use only my master for categories and keywords.
Read only the headers and 5 rows of each. Write a local script blocked_itc.py that:
1. Tags each entry with ITC to a blocked-credit category if the ledger name or narration matches its keywords.
2. Lists entries that match more than one category, and entries over ₹50,000 with ITC that match none, as "Review".
3. Writes Blocked_ITC_Review.xlsx with columns: Date, Voucher, Ledger, Party, Narration, Taxable, Total ITC, Category, Exceptions to check, Keyword matched.
Run it and show me the total ITC by category. Do not tell me whether ITC is allowed; I will decide.
```

**Check:** For each category, read the 'Exceptions to check' column against the entries. Many blocked categories have exceptions that depend on facts, such as the nature of the business or a legal obligation to provide the item.

## Statutory audit

### General ledger scrutiny that produces an exception workbook

**When to use:** General ledger scrutiny of every row against fixed rules, with an exception workbook for the file.

```text
I am auditing the general ledger in GL_FY2025-26.xlsx. It is client data.
Do not open or print the data rows. Read only the column headers, then write a Python script gl_scrutiny.py that runs on this computer and does the following on every row:
1. Round amounts: debit or credit of ₹1,00,000 or more that is an exact multiple of ₹10,000.
2. Postings dated on a Sunday, or on any date in Holidays_FY26.xlsx.
3. Entries dated 25 March 2026 to 31 March 2026, and 1 April 2026 to 10 April 2026 if present.
4. Narration blank, or containing: adjustment, reversal, suspense, as per MD, cash, misc.
5. Ledgers with fewer than 5 entries in the whole year.
Write GL_Exceptions.xlsx with: a Summary sheet (test, rule used, count, total value);
one sheet per test with Date, Voucher Type, Voucher No, Ledger, Debit, Credit, Narration;
a Combined sheet listing each voucher once with the number of tests it hit, highest first;
and a Parameters sheet recording the file name, row count, debit and credit totals, and the rules above. Show me only the Summary counts on screen. Ask before running anything.
```

**Check:** The debit and credit totals on the Parameters sheet agree with the trial balance. If they don't, the export is incomplete or the script misread a column.

### A documented monetary-unit sample for vouching sales

**When to use:** A documented monetary-unit sample and a vouching tracker that anyone can re-run.

```text
Sales_Register_FY26.xlsx is client data. Read only the headers, then write a local Python script mus_sample.py that:
1. Keeps invoices with a positive amount; puts credit notes and negative lines on a separate sheet called Excluded, with their count and total.
2. Selects a monetary-unit sample of 60 using a fixed interval (population total / 60) and a random start drawn with seed 2026. If an invoice is hit more than once, list it once and show the number of hits.
3. Writes MUS_Sample_Sales.xlsx with these sheets:
Parameters: file name, date run, population count and value, excluded count and value, sample size, interval, seed, random start, number of distinct invoices selected.
Sample: Invoice No, Date, Customer, Amount, Cumulative Amount, Hits.
Vouching Tracker: the sample plus empty columns Invoice Seen, Delivery Challan or e-way bill, Customer Order, Receipt in Bank, Exception (Y/N), Remarks, WP Ref, Done By, Date.
4. Copies the script into the same folder so it can go into the working papers.
Show me only the Parameters sheet on screen.
```

**Check:** Population value on the Parameters sheet plus the Excluded total equals the sales ledger total.

### Confirmation control and reconciliation of replies

**When to use:** Selecting parties for balance confirmations and reconciling the replies.

```text
These files are client data. Read only their headers. Write a local Python script confirmations.py with two modes.
Mode "select": from Debtors_31Mar26.xlsx and Creditors_31Mar26.xlsx, select every party with a balance of ₹10 lakh or more, plus a random 15 of the rest (seed 31), and write Confirmation_Control.xlsx with columns: Ref No, Type (Debtor/Creditor), Party, Balance per Books, Email (from Party_Contacts.xlsx), Date Sent, Sent By, Reminder Date, Reply Date, Balance per Party, Difference, Status, Alternative Procedure, WP Ref.
Flag parties with no email or with an email domain matching the client's own domain.
Mode "reconcile": read the filled-in control sheet, compute Difference, set Status to Agreed, Differs, or No Reply, and for parties in Party_Statements.xlsx match their open items to the ledger by amount and date (within 7 days), listing unmatched items on a Reconciling Items sheet. Show me only counts by Status.
```

**Check:** Your team, not the client, sends the requests and receives the replies. SA 505, External Confirmations, expects the auditor to keep control of the process.

## Financial statements

### Trial balance to Schedule III, with a tie-out

**When to use:** Mapping a trial balance to Schedule III with a tie-out, starting from last year's mapping.

```text
I'm in the client folder. Files: TB_FY2025-26.xlsx (Tally trial balance export) and Mapping_FY2024-25.xlsx (columns: Ledger Name, Schedule III Line). First show me the headers and first 5 rows of the TB, and tell me how debit and credit are shown. Then write a Python script that runs here and does this. Don't read the full files into our chat.
1. Map each ledger using last year's mapping. Match on exact ledger name after trimming spaces.
2. For ledgers with no match, suggest a line based on the ledger name and its parent group, and mark them "SUGGESTED - REVIEW". Don't guess silently.
3. Build Sch3_Draft.xlsx with sheets: Mapping (Ledger, Group, Closing Dr, Closing Cr, Line, Status), Balance Sheet, Profit and Loss, Unmapped, Tie-out.
4. Balance Sheet and P&L must use SUMIF formulas on the Mapping sheet, not pasted values.
5. Tie-out sheet: total debits = total credits in the TB; profit from the P&L flows into reserves; Balance Sheet balances; every ledger mapped exactly once.
Use Indian number format (1,23,456.00). Don't change the source files.
```

**Check:** Every "SUGGESTED - REVIEW" ledger, one by one. Ledger names mislead: "Advance from Gupta" could be a customer advance or a loan.

### Receivable ageing and an expected credit loss matrix

**When to use:** Receivable ageing and an expected credit loss provision matrix from loss rates your team has agreed.

```text
Files: Debtors_Outstanding_31Mar26.xlsx (Tally bill-wise outstanding) and Loss_Rates.xlsx (columns: Bucket, Loss Rate). Reporting date is 31/03/2026. Dates are DD/MM/YYYY. Write a Python script that runs here. Show me only headers, row counts and results in the chat.
1. Age each bill from its due date to 31/03/2026 into the buckets listed in Loss_Rates.xlsx. If a bill has no due date, use the bill date and list those bills separately.
2. Make Ageing.xlsx with sheets: Bill-wise (all bills with days and bucket), Party Summary (party by bucket), Provision Matrix (bucket, gross amount, loss rate, provision as a formula), Exceptions (credit balances, missing dates, future-dated bills), Tie-out.
3. Take the loss rates only from Loss_Rates.xlsx. Do not suggest or change any rate.
4. Tie-out: total of all bills = Sundry Debtors balance of the balance in the TB, and the party summary total = bill-wise total.
```

**Check:** The loss rates in the matrix are the ones your team agreed, unchanged.

### Depreciation under the Companies Act and the Income-tax Act

**When to use:** Depreciation under the Companies Act and the Income-tax Act from a fixed asset register, with the WDV difference.

```text
Files: FAR_FY2025-26.xlsx, Tax_Rates.xlsx (Tax Block, Rate) and Dep_FY2024-25.xlsx (last year's closing schedules). FY is 01/04/2025 to 31/03/2026. Write a Python script that runs here and builds Depreciation_FY2025-26.xlsx with:
1. Books sheet: asset-wise straight-line depreciation using each asset's Useful Life and Residual % from the register, pro rata by days from Date Put to Use for additions. Show opening, additions, deletions, depreciation and closing for each asset, then totals by Asset Class.
2. Tax sheet: block-wise written down value. Opening WDV from last year's file, plus additions, less sale proceeds of deletions, depreciation at the rate in Tax_Rates.xlsx. For additions put to use for less than 180 days in the year, apply half the rate on those additions.
3. Flag any block where deletions exceed opening WDV plus additions, or a block becomes empty. Don't compute those cases; list them for me.
4. Comparison sheet: books WDV and tax WDV per class, and the difference.
5. Tie-out: opening figures agree with last year's closing; additions agree with the register.
Use only the rates and lives in my files. Don't supply any from memory.
```

**Check:** The tax rates in Tax_Rates.xlsx against the current rules for the relevant year. Note that the Income-tax Act, 2025 has replaced the 1961 Act from 1 April 2026, so section references in your templates may need updating.

## Forensic work

### A duplicate payment sweep

**When to use:** A sweep for duplicate payments across all vendors.

```text
Both files are client data. Do not open or print any rows; read only the headers.
Write a Python script dup_payments.py that runs on this computer and finds:
A. Exact duplicates: same vendor code, same invoice number, same amount.
B. Same vendor and same amount, paid within 30 days of each other, different invoice numbers.
C. Near-duplicate invoice numbers for the same vendor: compare after removing spaces, hyphens, slashes, leading zeros and prefixes like INV; also flag numbers that differ by one character (for example 0 and O, 1 and I) where the amounts match.
D. The same invoice number and amount paid to two different vendor codes.
E. Different vendor codes in Vendor_Master.xlsx sharing a bank account number.
Write Duplicate_Sweep.xlsx with a Summary sheet (test, groups, rows, value at risk), one sheet per test where related payments share a Group No, and a Parameters sheet (files, row counts, total paid, the rules and windows used). Keep the 30-day window as a setting. Show me only the Summary on screen.
```

**Check:** Test B will catch legitimate repeats (monthly rent, the laundry contract's fixed monthly charge, retainer fees). Mark those as explained, with the reason, before counting value at risk.

### Matching the vendor master against the employee master

**When to use:** Matching the vendor master against the employee master without personal data reaching the chat.

```text
Employee_Master.xlsx and Vendor_Master.xlsx contain personal data. Read only the headers.
Never print names, PANs, bank account numbers, phone numbers or addresses to the screen.
Write a local script vendor_employee_match.py that cleans and compares:
1. Bank account number plus IFSC (digits only, leading zeros removed).
2. PAN (upper case, spaces removed).
3. Mobile number (last 10 digits).
4. Address: PIN code plus the first house or flat number, as a weaker match.
5. Name: similarity of 90% or more after removing M/s, Shri, Smt and punctuation, as a weak match.
For each match write a row in Vendor_Employee_Match.xlsx with Employee Code, Vendor Code, Match Type, Strength (Strong for 1-3, Weak for 4-5), Employee Status (active or left, with leaving date), and total paid to the vendor from Payments_Jan24_Jun26.xlsx. Show me only counts by match type.
```

**Check:** A match is not misconduct. Employees can be legitimate vendors (with approval), or share a family address. Look for the approval, then decide.

## Checking AI output

### Separate sources from memory

**When to use:** After any answer that mentions sections, rules, forms, rates or due dates.

```text
List every section, rule, form, rate and due date in your answer. For each, tell me whether it came from a file in this folder or from your memory. Mark the memory ones UNVERIFIED.
```

**Check:** Check every item marked UNVERIFIED against the current official text before you use it.

### Make it use the current text

**When to use:** When the answer depends on the law: make it work from the documents you give it.

```text
For the law, use only the PDF documents in this folder. Quote the paragraph and page for every rule you rely on.
If anything you would normally say from memory differs from these documents, tell me, and follow the documents.
```

**Check:** Each quoted paragraph and page is really in the PDF you gave it.

---

More prompts, worked walkthroughs and answer keys: *AI for CAs* (https://aiforseries.github.io/ai-for-cas/)
