# Tax practice prompts (India: GST, ITR, TDS)

Copy-ready prompts from *AI for Tax Practitioners* by Chandan Gupta, for an AI assistant such as Claude Code working in a folder on your own computer. The file names, columns, thresholds, dates and names come from the book's worked examples, where all companies, people and figures are made up. Replace them with your own. Indian law, rates, thresholds and due dates change: use your own masters and the current official text, as the prompts ask. Each prompt is followed by a check to do before you rely on the result.

## Keeping client data out of the chat

### Add this to every request on client data

**When to use:** Add it to any request on client data.

```text
Do this with a Python script that runs on this computer. Look only at the column headers and the first 5 rows before you write it. Do not print data rows on screen. Show me only counts, totals and the names of the files you save.
```

**Check:** The reply shows only counts, totals and file names. If it shows data rows, stop and tighten the request.

### Tagging a bank statement without sending it

**When to use:** Tagging a client's bank statement to ledgers from your own keyword rules, with a running-balance check.

```text
This folder has SBI_CA_Apr-Sep.xlsx (the client's bank statement) and Narration_Rules.xlsx (columns: Keyword, Ledger). Look only at the headers and first 5 rows of each. Do not open the full statement.
Write a Python script tag_bank.py that runs on this computer and:
1. Reads every line of the statement and tags it with a ledger when the narration contains a keyword from my rules sheet. If two keywords match, tag it "Check: two rules".
2. Leaves lines with no match as "Untagged".
3. Checks that opening balance plus credits minus debits equals the closing balance, line by line, and lists any line where the running balance breaks.
4. Saves Bank_Tagged_Apr-Sep.xlsx with the original columns plus Ledger, and a Summary sheet with totals by ledger.
Run it. On screen, show only: lines read, lines tagged, lines untagged, lines with two rules, balance check result, and totals by ledger. Do not print any narration.
```

**Check:** The opening and closing balances in the summary match the first and last page of the statement.

## Client intake

### A WhatsApp dump becomes a document index

**When to use:** Sorting and indexing documents a client sent on WhatsApp, without opening them in the chat.

```text
The folder 01_From_Client has photos and PDFs a client sent on WhatsApp. Do not open or display any of them. Work only from file names, sizes and dates, and from text that a script extracts locally.
Write a Python script index_docs.py that runs on this computer and:
1. Finds exact duplicates by file content (a checksum) and lists them; keeps one copy.
2. For each PDF that has a text layer, searches the first two pages for the keywords in my sheet Doc_Keywords.xlsx (columns: Keyword, Doc type) and records the doc type it finds. Do not print the text.
3. Makes Contact_Sheet.pdf: every photo and every PDF with no text, as numbered thumbnails, four to a row.
4. Writes Doc_Index.xlsx with columns: No, Original name, Size KB, Date received, Duplicate of, Doc type (found or blank), New name, Checked by.
5. Copies (does not move) each file into 02_Work\Sorted as I118_2026-27_<Doc type>_<No> where the doc type is known, and leaves the rest for me.
Run it and show me only counts: files, duplicates, identified, left to label.
```

**Check:** Files in the folder equals identified plus left to label plus duplicates.

### Missing documents for the whole ITR desk

**When to use:** One list of missing documents for every ITR client, built from your checklist master.

```text
Files in this folder: ITR_Client_Master.xlsx (Client code, Name, Mobile, Types, where Types is a comma list such as "Salaried, Capital gains"), Checklist_Master.xlsx (Type, Document, Required or If applicable) and Docs_Received.xlsx (Client code, Document, Date received). Look only at headers and 3 rows of each.
Write a local script missing_docs.py that:
1. Builds each client's checklist from all of their types, without repeating a document.
2. Marks each document Received (with date), Missing, or If applicable.
3. Writes Missing_Docs.xlsx with a sheet per status, and a Summary sheet: client code, name, number required, received, missing, and a Ready to start column (Yes only when nothing Required is missing).
4. Lists document names in Docs_Received.xlsx that do not appear in the checklist master, so I can fix spelling differences.
Run it and show me only the totals and the count of clients ready to start. Do not print names or mobile numbers.
```

**Check:** Fix the unmatched document names first and re-run. A spelling difference shows up as "missing" and annoys a client who already sent it.

## GSTR-1

### Pre-filing checks on one client's register

**When to use:** Pre-filing checks on one client's sales register before you prepare GSTR-1.

```text
Using ClientA/ClientA_standard_2026-10.xlsx and my Buyer_Master.xlsx (Buyer Name, GSTIN, State) in the same folder, write a local script gstr1_checks.py and run it. Do not print data rows on screen. Checks:
1. Rows with no Buyer GSTIN whose buyer name appears in Buyer_Master with a GSTIN: "B2B missing GSTIN".
2. GSTIN structure: 15 characters, first two digits a state code from the list in my State_Codes.xlsx, characters 3 to 12 in PAN form. Do not verify GSTINs online.
3. Tax type against place of supply: our state is Madhya Pradesh. Place of supply in another state with CGST/SGST, or in our state with IGST: "Tax type check". Also flag where the GSTIN state code differs from the place of supply, for my review only.
4. Invoice series: for each prefix, list missing numbers, repeated numbers and dates outside October 2026.
5. Tax arithmetic: taxable value times rate against tax charged, difference above ₹1.
6. HSN summary: totals by HSN and rate, and a count of documents by type.
Write ClientA_GSTR1_Checks_2026-10.xlsx with one sheet per check and a Summary sheet with the count of rows in each. Show me only the Summary.
```

**Check:** Tie the HSN summary total to the register total before you use it in the return.

## ITC and GSTR-3B

### Run the 2B match for every client and get one status table

**When to use:** Running a 2B match script you already trust for every GST client, with one status table.

```text
This folder, GST_Monthly\2026-10, has one sub-folder per client code. Each has 2B\ (portal downloads) and Books\ (purchase register). My saved script is ..\..\Scripts\match_2b.py; read it first and tell me in three lines what it takes as input and what it writes. Do not change it.
Then write batch_2b.py that runs it for every client folder and:
1. Records "Missing 2B" or "Missing books" when a folder lacks either, and moves on.
2. Catches any error per client, records the message, and continues.
3. From each client's output, reads the ITC total in books, in 2B, the difference, and the count of rows in each exception sheet. Write ITC_Status_2026-10.xlsx: one row per client with those figures, plus Status: Clean (difference under ₹100 and no exceptions), Review, Missing data, or Error. Sort Review clients by the size of the difference.
Show me only the count per status and the top 10 Review clients by difference.
```

**Check:** Compare the 2B total for three clients with the ITC in the portal's 2B summary for that month.

### A GSTR-3B working for your review

**When to use:** A GSTR-3B working that applies your own claim decisions and lists anything still undecided.

```text
In the ClientB folder: ClientB_standard_2026-10.xlsx (sales, standard template), ClientB_GSTR1_Prepared_2026-10.xlsx, ITC_Match_ClientB_2026-10.xlsx and ITC_Decisions_ClientB_2026-10.xlsx (my decisions: invoice, claim Y/N, reason). Read headers and 5 rows of each.
Write a local script that builds ClientB_3B_Working_2026-10.xlsx with:
1. Outward tax by tax head as per books and as per the prepared GSTR-1, and the difference.
2. ITC as per 2B by tax head, less invoices I marked N, plus books-only invoices I marked Y, to give "ITC proposed". Every adjustment is one row with its reason from my decisions file.
3. A list of any invoice in the match file that has no decision from me yet, marked PENDING. Do not decide eligibility or fill in any missing decision.
Show me only the summary table and the number of PENDING rows.
```

**Check:** PENDING must be zero. A pending row means an invoice nobody has decided on.

## ITR season

### AIS, 26AS and Form 16 into one client sheet

**When to use:** Putting AIS, 26AS and interest certificates side by side for one client.

```text
This is one client's folder. The downloads are in Downloads\: AIS.json, 26AS.pdf (a text PDF, not a scan) and Interest_Certificates\*.pdf. Do not print the contents of any file. Read only the structure of the JSON (its keys) and the first page layout of the 26AS, and tell me what you find before going further.
Then write a local script client_sheet.py that:
1. Flattens the AIS into rows: information category, source name, source TAN or PAN, amount, TDS.
2. Reads Part I (TDS) of the 26AS into rows: deductor name, TAN, section as printed, amount paid, TDS deposited.
3. Reads the interest certificates into rows: bank, account last 4 digits, interest, TDS.
4. Matches the three by TAN first, then by name and amount within ₹10.
5. Writes Client_Sheet.xlsx with sheets: All Income (one row per item, with columns AIS, 26AS, Certificate and Source used), TDS Credit, Differences, and Notes (every assumption made).
Run it and show me only the row counts and the totals of each sheet.
```

**Check:** Tie the TDS total in the TDS Credit sheet to the total TDS shown in the 26AS. If they differ, the script missed a page or a deductor.

### AIS items to ask the client about

**When to use:** Listing AIS entries to ask the client about.

```text
From Client_Sheet.xlsx in this folder, list AIS entries above ₹10,000 that have no matching entry in the client's bank statements or certificates, with category, source and amount, so I can ask the client about each one. Use a script; save the list, show me the count.
```

**Check:** Ask the client about each item. Do not leave an AIS entry out of the return only because it is not in the bank statement.

## TDS

### Check PANs and lower deduction certificates

**When to use:** Checking PANs and lower deduction certificates before a TDS return.

```text
Using Client_TDS_Q2_Working.xlsx (sheet Deductees), Vendor_Master.xlsx (Vendor, PAN, Type: Individual, Firm, Company, HUF, Trust) and LDC_List.xlsx (Vendor, PAN, Certificate No, Section, Rate, Valid From, Valid To, Amount Limit), write a local script that adds a PAN check sheet:
1. PAN structure: 10 characters, five letters, four digits, one letter. Flag anything else.
2. The fourth character against the vendor Type in the master (P for an individual, C for a company, F for a firm, H for an HUF, T for a trust). Flag mismatches for my review.
3. PAN in the deductee sheet different from the vendor master for the same vendor.
4. Payments to vendors in LDC_List: whether the payment date is within the validity, the rate applied against the certificate rate, and the running total against the amount limit.
Do not check PANs online. Show me only the count of flags in each check.
```

**Check:** A PAN that passes the structure check can still be wrong or inoperative. Verify flagged and new PANs on the portal yourself.

### Payments where TDS may be missing

**When to use:** Finding payments in TDS-applicable ledgers where nothing was deducted.

```text
From Client_Ledger_Q2.xlsx and my TDS_Master.xlsx, list expense entries in ledgers that my master marks as TDS-applicable where no TDS was deducted, with party, amount and ledger. I will decide which were below the limit or exempt.
```

**Check:** Check each party against the threshold, any exemption and any lower deduction certificate before you call it a default.

## Bookkeeping

### Six monthly statement PDFs into one checked table

**When to use:** Turning several months of bank statement PDFs into one checked table.

```text
In 01_Input there are six bank statement PDFs for one client, one per month. Do not print the statements. First write a short script that prints, for the first PDF only: the number of pages, the column headings it finds, and the first 3 table rows with amounts replaced by 999. Stop there and tell me how the table is laid out.
Then write bank_to_table.py, to run here, that:
1. Reads every PDF in 01_Input and extracts the transaction table into one sheet with columns: Date, Value Date, Narration, Ref No, Debit, Credit, Balance, Source File, Page.
2. Joins narrations that wrap onto a second line.
3. Recomputes the running balance from the opening balance in the first PDF and lists every row where the bank's balance differs from the computed one by more than ₹1.
4. Checks that each month's closing balance equals the next month's opening balance.
5. Lists exact duplicate rows (same date, amount, narration and balance).
Write Bank_H1_Clean.xlsx with sheets Transactions, Balance Breaks, Month Links, Duplicates. Run it and show me only row counts per month and the number of rows on each check sheet.
```

**Check:** Match the row count and the total debits and credits for one month against the PDF's own summary block.

### Categorising 1,400 narrations from your rules master

**When to use:** Categorising bank narrations to ledgers from your own rules master.

```text
Use Bank_H1_Clean.xlsx (sheet Transactions) and my rules master Bank_Rules_SAI01.xlsx (columns: Keyword, Match In, Ledger, Voucher Type, Priority, Notes). Use only my rules; do not invent a ledger.
Write categorise.py, to run here, that:
1. Upper-cases narrations and strips the UPI, NEFT, IMPS, ACH and CHQ prefixes and reference numbers into a separate column called Party Text.
2. Applies the rules in Priority order. Record which keyword matched.
3. If two rules of the same priority match, mark the row Conflict, not either ledger.
4. Rows with no match go to sheet Unknown, grouped by Party Text with count and total.
Write Bank_H1_Categorised.xlsx with sheets Tagged, Conflict, Unknown, Summary by Ledger (count, debit, credit). Run it and show me the Summary by Ledger and the row counts.
```

**Check:** Tagged + Conflict + Unknown rows must equal the rows in the Transactions sheet.

### Bank against books, and the suspense ledger explained

**When to use:** Bank against books, and explaining the suspense ledger.

```text
Files: Bank_H1_Categorised.xlsx (sheet Tagged), Tally_Bank_Ledger_H1.xlsx and Tally_Suspense_Ledger.xlsx (both exported from Tally: Date, Particulars, Vch Type, Vch No, Debit, Credit). Look at headers and 5 rows of each only.
Write bank_vs_books.py, to run here, that:
1. Matches bank rows to Tally bank ledger rows on amount (opposite side) and date within 5 days; where several candidates exist, prefer the same cheque or reference number in the narration.
2. Lists bank rows not in Tally, Tally rows not in the bank, and Tally rows matched only on amount.
3. For each suspense entry, looks for a bank row with the same amount within 10 days and shows the bank narration beside it.
4. Ages the suspense entries that find no bank row: 0-30, 31-90, over 90 days.
Write BRS_and_Suspense.xlsx with those sheets and a summary that reconciles the Tally bank balance to the bank balance on the last date in the Tally file. Run it and show me the summary.
```

**Check:** The reconciliation must close to zero. If it does not, the script missed a row type (often opening balances or cancelled vouchers).

## Notices

### Read a GST scrutiny notice into a working sheet

**When to use:** Reading a GST notice into a working sheet and a plain list of what it asks for.

```text
Read Notice_redacted.pdf in this folder. It is a text PDF, not a scan.
1. In a few lines, tell me: who issued it (designation only), the reference number and date as printed, the tax period, what the notice asks the taxpayer to do, and the date or time limit for reply exactly as printed. Quote the notice's own words for the reply date. Do not work it out.
2. List every provision the notice itself cites, quoted as printed. Do not add any provision of your own.
3. Write a local script that extracts the table of differences into Notice_Table.xlsx with the columns as in the notice, and a Total row. Show me the totals and check that they agree with any total printed in the notice.
4. If any part of the notice is unclear or the text did not extract cleanly, say which page and stop.
```

**Check:** Compare the reply date in its summary with the notice, character by character. Enter it in the notice register yourself.

### A draft reply that cites only what you give it

**When to use:** A first draft of a reply that cites only the provisions and figures you give it.

```text
Using only Reply_Points.docx, Provisions_I_Rely_On.txt and the Bridge and Index sheets of Reply_File.xlsx, draft a reply to the notice in Notice_redacted.pdf.
Rules:
1. Use only the provisions in Provisions_I_Rely_On.txt. Do not cite any other section, rule, circular, notification or case, even if you think it helps. If you think something is missing, list it at the end under "Suggestions for the reviewer", clearly marked as not verified.
2. Every figure must come from Reply_File.xlsx, with the annexure named.
3. Structure: reference to the notice, brief facts, point-by-point response in the notice's order, annexures list, closing. Formal, short paragraphs, no adjectives.
4. Save as Draft_Reply_v1.docx. Then list every figure in the draft with its source cell.
```

**Check:** Search the draft for any section, rule, circular or case you did not supply. Delete it, or verify it from the official source before keeping it.

### Citation audit of a draft

**When to use:** Before a reply is filed: list every citation in the draft.

```text
List every section, rule, notification, circular and case mentioned in Draft_Reply_v2.docx, with the sentence it appears in. Mark which ones also appear in Provisions_I_Rely_On.txt. I will verify the rest myself.
```

**Check:** Verify every reference that is not in your own provisions file against the official text, or remove it.

## Fees

### Who owes what, and since when

**When to use:** Outstanding fees by client, aged, with a list of who to remind this week.

```text
Read the headers and five rows of Fee_Ledger.xlsx (Client Code, Date, Type, Period, Amount, Mode, Reference) and Clients_Master.xlsx.
Write a local script fees.py that:
1. Applies receipts to the oldest unpaid bills of the same client first (first in, first out).
2. Builds Fee_Outstanding.xlsx with: Outstanding (client, name, staff, total due, amount by age: 0 to 30, 31 to 90, 91 to 180, over 180 days, last receipt date), Unapplied (receipts larger than all bills, or with no matching client code), Summary (totals by age and by staff).
3. Adds a sheet Remind This Week: clients with anything over 30 days and no receipt in the last 30 days, with mobile and amount.
Run it. Show me only Summary and the count in Remind This Week and Unapplied.
```

**Check:** Total outstanding should equal total bills less total receipts. Check it in Excel with two SUMIFs.

## Client messages

### Explaining a 2B difference in plain words

**When to use:** Explaining a 2B difference to a client in plain words, in Hindi and English.

```text
Here are the facts, with supplier names replaced by codes: our purchases this month show tax of ₹48,200; the government's statement of our purchases shows ₹39,650; the difference of ₹8,550 is three bills from suppliers S1, S2 and S3 who have not yet uploaded them.
Draft a WhatsApp message to the client in simple Hindi written in Roman script, under 90 words: explain that this credit can be claimed only after the suppliers upload the bills, that we have listed the three bills for him, and that he should ask these suppliers to upload. Do not mention any section, rule or time limit. Do not promise when the credit will come. Then give me the same message in plain English.
```

**Check:** Check every figure in the draft against Imran's working, and that the three figures add up.

## Reading photos and scans

### Photos of purchase bills, read with care

**When to use:** Extracting purchase bill details from photos, with every doubt flagged instead of guessed.

```text
In 01_Input\Bills_Photos there are 46 photos of purchase bills. For each photo, extract: supplier name, GSTIN, bill number, bill date, taxable value, CGST, SGST, IGST, total.
Rules:
1. If any digit is unclear, write the value with a ? and fill a Doubt column saying which field.
2. Check that taxable + taxes = total, and that the GSTIN has 15 characters. Flag any failure.
3. Never guess a GSTIN or an amount.
4. Write Bills_Extracted.xlsx with one row per photo, including the photo file name.
Show me only the number of photos read, the number with a Doubt, and the number failing a check.
```

**Check:** Open every photo with a Doubt or a failed check, and type those values yourself.

---

More prompts, worked walkthroughs and answer keys: *AI for Tax Practitioners* (https://aiforseries.github.io/ai-for-tax-practitioners/)
