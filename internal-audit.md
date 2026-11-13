# Internal audit prompts

Copy-ready prompts from *AI for Internal Auditors* by Chandan Gupta, for an AI assistant such as Claude Code working in a folder on your own computer. The file names, columns, thresholds, dates and names come from the book's worked examples, where all companies, people and figures are made up. Replace them with your own. Each prompt is followed by a check to do before you rely on the result.

## Keeping company data out of the chat

### Add this line to any request on company data

**When to use:** Add it to the end of any request that touches company data.

```text
Use a script for this and run it here. Do not open or print the data rows. Show me only counts, totals and the names of any files you save.
```

**Check:** The reply shows only counts, totals and file names. If it shows data rows, stop and tighten the request.

### Build on a fake file, run on the real one

**When to use:** When not even a sample row may leave the file: build and prove the test on made-up data, then run it on the real file yourself.

```text
I will give you only the column headers of a payroll register, not the file: Employee_ID, Name, Pay_Date, Gross_Pay, Net_Pay, Bank_Account, Bank_Code, Termination_Date, Cost_Center.
1. Create Dummy_Payroll.xlsx with 200 made-up rows using exactly these headers, and plant three cases: one employee paid after their termination date, two employees with the same bank account, and one normal leaver paid before leaving.
2. Write payroll_tests.py that takes a file name as an argument and writes Payroll_Exceptions.xlsx with one sheet per test and a Parameters sheet.
3. Run it on the dummy file only and show me that it catches the planted cases and not the normal leaver.
Do not look for or open any other file in this folder.
```

**Check:** Read the script's rules (or ask for them in plain English) before you run it on real data. A test proven on a fake file is only as good as the fake.

## Population completeness and sampling

### Tie an AP extract to the trial balance

**When to use:** Before any full-population test on payables, to prove the extract is complete.

```text
Read only the headers of AP_Invoice_Lines_FY26.csv and GL_TB_AP_FY26.xlsx. Write a local script that totals the invoice lines by GL account and month, compares them to the trial balance movements for the same accounts and months, and writes AP_GL_TieOut.xlsx with a Differences sheet and a Parameters sheet. Print only the overall totals and the number of account-months that differ by more than $1.
```

**Check:** Every account-month difference is explained before you test anything.

### Completeness check and a seeded sample for invoice approval

**When to use:** Selecting a control-testing sample that anyone can re-run, from a register you have tied to the subledger.

```text
These files are company data. Do not open or print their contents. Read only the column headers of AP_Invoice_Register_Q1Q3.csv and AP_Subledger_Summary.xlsx.
Write a Python script sample_p2p07.py that runs on this computer and:
1. Loads the register and prints only: row count, total of Invoice Amount, min and max Invoice Date, number of duplicate Invoice IDs, and the difference to the period total in AP_Subledger_Summary.xlsx.
2. Lists any gaps in the Voucher Number sequence to a sheet, without printing them.
3. Filters to invoices with Invoice Amount over 25000 (the population for the control).
4. Sorts that population by Voucher Number then Line Number, and selects 25 items at random with seed 20261015.
Save P2P07_Sample.xlsx with sheets Population (filtered, sorted, with a row number), Sample, Sequence Gaps and Parameters (file names, file dates, row counts before and after the filter, totals, the difference to the subledger, sort key, seed, size, method, Python and pandas versions, date run).
Keep the threshold, seed and size as settings at the top. Print only the counts and totals.
```

**Check:** Run the script a second time and confirm the same 25 items. Save the population file you sampled from, unchanged, with the working paper.

## Duplicate vendors

### Duplicate vendors without sending a single bank account

**When to use:** Finding possible duplicate vendors in a vendor master without the bank details reaching the chat.

```text
Vendor_Master_US.xlsx is company data. Read only its column headers; do not open the data rows and do not show me any values.
Write a Python script that finds possible duplicate vendors: (a) the same tax ID, (b) the same bank account and routing number, (c) names that match after removing punctuation, case and words like Inc, LLC, Co and Corp.
Run it here. Print only: rows read, the number of groups found by each test, and the number of vendors in more than one group.
Save the groups to Vendor_Duplicates.xlsx, one sheet per test, with a Parameters sheet (file name, row count, rules used, date run). Do not print the groups.
```

**Check:** Open three groups from each sheet and confirm by eye that they really are the same supplier, or explain why not (a parent and a subsidiary can share a tax ID legitimately).

## Vendor master and employee matches

### Vendor master hygiene, including matches to employees

**When to use:** Vendor master hygiene: vendors that match employees, missing tax IDs, PO box addresses, inactive vendors still paid.

```text
These three files contain bank details and personal data. Do not open, print or quote any row. Read only the column headers.
Write a Python script vendor_hygiene.py that runs on this computer and:
1. Normalizes addresses (case, punctuation, "Street"/"St", suite numbers) and bank account plus routing numbers (digits only).
2. Flags vendors whose bank account and routing number match any employee's, and vendors whose normalized address and ZIP match an employee's.
3. Flags vendors with a blank or malformed Tax ID, and vendors whose address is a PO box (PO Box, P.O. Box, POB).
4. Flags payments to vendors on dates after their Status Date where Status is Inactive or Blocked.
Write Vendor_Hygiene.xlsx with a Summary sheet (test, vendors flagged, payment value), one sheet per test showing Vendor ID and Employee ID only, with no bank numbers (show the last 4 digits at most), and a Parameters sheet (files, row counts, normalization rules, date run).
Print only the Summary.
```

**Check:** Expect legitimate matches: employees set up as vendors for relocation or expense refunds. Confirm the reason with the AP Manager.

## Split purchase orders and payment timing

### Split purchase orders and payments outside terms

**When to use:** Purchase orders split to stay under an approval limit, and invoices paid before their date or early against terms.

```text
Read only the headers of PO_Header_FY26.csv and AP_Invoices_Paid_FY26.csv (company data).
Write a local script p2p_timing_split.py with the settings at the top: approval limit 50000, split window 7 days, early-payment margin 10 days.
1. Split POs: group POs by Vendor ID and Requester where each PO is below the limit, the POs fall within the window, and together they reach the limit. Give each group a Group ID.
2. Timing: flag invoices paid before the Invoice Date, and invoices paid more than the margin earlier than Invoice Date plus Payment Terms Days.
Write P2P_Split_Timing.xlsx with sheets Summary, Split Groups (each PO with its Group ID, group total and approver), Paid Before Invoice, Paid Early, and Parameters (files, row counts, totals, settings, date run).
Print only the Summary.
```

**Check:** The approval limit and window in the script match the approval matrix in force for the period.

## Journal entry testing

### Scoring every journal entry against risk criteria

**When to use:** Scoring every manual journal against your own risk criteria and weights, so you choose entries to test from a ranked list.

```text
Read only the headers of JE_Lines_Q4_US.csv. Read JE_Criteria_Q4.xlsx in full (it contains no transaction data).
Write a local script je_scoring.py that aggregates lines to Journal ID and scores each manual journal (Source = Manual) using the weights on the Criteria sheet:
revenue or reserve account (3); Created By equals Approved By (3); creator not on the usual posters list, or on the senior users list (2); total within 5% below an approval threshold (2); Posted Date after the close date for a Q4 period (2); created on a weekend, holiday, or before 07:00 or after 20:00 (1); amount a multiple of 10,000 (1).
Keep all weights, times and percentages as settings read from the Criteria sheet.
Write JE_Scored_Q4.xlsx with: Scored Journals (one row per journal, one Y/N column per criterion, score), Score Distribution, By User, Benford (first-digit counts against the expected proportions, for information), and Parameters.
Print only the score distribution.
```

**Check:** Created timestamps may be in server time, not local time. Confirm before relying on the after-hours flag.

## User access and segregation of duties

### Leavers, dormant accounts and privileged access in SAP

**When to use:** An access review on system user, role and leaver extracts: leavers still active, dormant, generic and privileged users.

```text
These files contain personal data. Read only their headers; do not print any row.
Write a local script access_review_de.py that, as at the extract date 2026-12-31 (a setting):
1. Terminated users still active: mapped to a leaver, not Locked, and Valid To after the Termination Date. Flag any Last Logon after the Termination Date separately.
2. Dormant: active users with no logon for 90 days or more (setting), or never logged on.
3. Generic or shared: dialog users with no Employee ID mapping, or IDs matching patterns such as ADMIN, TEST, TEMP, SHARED or a department name (patterns as a list setting).
4. Privileged: users holding any role in Privileged_Roles.xlsx, with the roles listed.
Write Access_Review_DE.xlsx with one sheet per test, a Summary, and Parameters (files, row counts, extract date, settings, date run). Use User ID and Employee ID only.
Print only the Summary.
```

**Check:** The user count agrees to the system's own user count on the extract date.

### Step 2: SoD conflicts

**When to use:** Segregation-of-duties conflicts from user-role assignments, role permissions, an SoD rule set and a list of mitigating controls.

```text
Create a script access_review.py that writes access_review_output.xlsx. Never print names or data rows; print only the summaries I ask for. Use an as-at date of 2026-09-30, kept as a setting at the top of the script.
Step 1, SoD conflicts. Keep only role assignments valid on the as-at date (treat 9999-12-31 or a blank end date as open-ended). Expand users to roles to activities using role_permissions.xlsx. For each rule in sod_rules.xlsx, list every user who holds both activities. Write a sheet SoD_Conflicts with: User_ID, Department, User_Type, Lock_Status, Rule_ID, Risk_Rating, Roles giving Activity A, Roles giving Activity B, Within_One_Role (Yes if a single role grants both), Mitigation (from mitigating_controls.xlsx: Valid, Expired with the date, or None), and Status (Open conflict, Mitigated, or Locked account). Also write Roles_With_Internal_Conflicts: any role that on its own grants both sides of a rule.
Run it and show me only counts: assignments used and ignored, and conflicts by Status and by Rule_ID.
```

**Check:** Assignments used plus assignments ignored equal the rows in the role extract, and each Mitigated status is backed by the mitigating control's own evidence.

## Travel and expense claims

### T&E: duplicate claims across employees

**When to use:** Duplicate expense claims within and across employees.

```text
Read only the headers of TE_Claims_FY26.csv (Claim ID, Employee ID, Expense Date, Category, Merchant, Amount, Currency, Receipt Attached). Write a local script that flags the same Merchant, Expense Date and Amount claimed by two or more different Employee IDs, and the same employee claiming the same item twice in different claims. Output TE_Duplicates.xlsx with a group number for each set, plus Parameters. Print counts only.
```

**Check:** Trace a few groups to the receipts before you treat any of them as a duplicate.

## Fraud red flags

### First-digit pointer, with caveats

**When to use:** A first-digit (Benford) test inside a defined investigation scope.

```text
Payments_FZE.csv is confidential; read only the headers. Write a local script that runs a first-digit (Benford) test on Amount USD for the in-scope vendors, excluding amounts under $10, and writes Benford_Case031.xlsx with expected and actual percentages per digit, the count per digit, and a chart. Add a note sheet stating that this is a pointer only and listing the population size. Print only the population count.
```

**Check:** Treat the result as a pointer to where to look, not as evidence. Record the population size with it.

## Walkthroughs and risk control matrices

### Fourteen pages of notes into a structured narrative

**When to use:** Turning sanitized walkthrough notes into a structured process narrative, with every step traced to a page of your notes.

```text
Read O2C_UK_Notes_sanitized.docx and O2C_UK_Narrative_FY26.docx.
Draft O2C_UK_Narrative_FY27.docx in this structure for each sub-process (customer master, credit, order entry, dispatch, billing, cash application, credit notes):
1. Numbered steps: Role, what they do, system or report used, and [Notes p.X] for the page of my notes it comes from.
2. Controls mentioned: what the interviewee said, quoted, with the page.
3. Changes from last year: anything that differs from the FY26 narrative.
4. Open points: anything unclear, contradictory between interviewees, or not covered in my notes.
Rules: use only my notes and last year's narrative. Do not fill a step the notes don't describe; write [NOT COVERED] instead. Where two interviewees disagree, give both versions. Keep the roles exactly as written in my notes.
```

**Check:** Pick five steps at random and find them on the cited page of the notes.

### An RCM draft with sources, gaps and the reports controls rely on

**When to use:** Drafting an RCM from an agreed narrative, with gaps, controls not mentioned this year, and the reports controls rely on (IPE).

```text
Read O2C_UK_Narrative_FY27.docx and RCM_O2C_UK_FY26.xlsx.
Draft RCM_O2C_UK_FY27_Draft.xlsx with columns: Sub-process, Risk ID, Risk, Control ID, Control (as described in the narrative), Source Quote, Narrative Step, Owner Role, Frequency, Preventive/Detective, Manual/Automated/IT-dependent manual, Report or Spreadsheet Used, Design Questions, Status.
Rules:
1. Every control must have a Source Quote from the narrative. If you can't quote it, it is not a control in this draft.
2. Where last year's RCM has a control the narrative doesn't mention, add the row with Status = Not Mentioned This Year and leave Control blank.
3. Where a risk has no control in the narrative, add the row with Status = Gap and leave Control blank.
4. In Design Questions, list what I still need to confirm to judge the design: who performs it, what exactly is checked, the threshold, what happens to exceptions, and what evidence is kept.
5. On a second sheet, IPE, list every report, query or spreadsheet a control relies on: Name as written, Control IDs, Source System, How Produced (as described), and What I Need To Test.
Leave Key Control and any design conclusion blank.
```

**Check:** Every Source Quote is in the narrative, word for word. Search for three of them.

## Audit observations

### Five draft observations from field notes

**When to use:** Turning field notes into draft observations in a standard format, with every fact traced to a note line.

```text
Read UK_Warehouse_FieldNotes.docx and Rating_Criteria.docx.
Group the notes into separate issues and draft one observation per issue in Observations_Draft.docx, with these headings:
Title (states the condition, not "Weakness in...")
Criteria (the policy, procedure or standard the notes name; if none, write [CRITERIA TO CONFIRM])
Condition (population, sample or full population, exceptions, period, and the WP reference)
Cause (only if the notes support it; otherwise [CAUSE TO CONFIRM] and three "why" questions to ask)
Effect (what could happen, at the scale the facts support; no speculation)
Recommendation (must address the cause, not restate the condition)
Proposed Rating (with two lines of reasoning against Rating_Criteria.docx; marked PROPOSED)
Management Action Plan, Owner, Target Date: write [TO BE PROVIDED BY MANAGEMENT]
After each observation, add a Sources box quoting the note lines each fact came from.
Do not invent numbers, dates, names or policy references. Refer to people by role only.
```

**Check:** Every number traces to a quoted note line and to the working paper. Remove anything that doesn't.

### Do the recommendations fix the cause?

**When to use:** Reviewing draft recommendations before they go to management.

```text
For each observation in Observations_Draft.docx, state in one line which cause each recommendation addresses. Flag recommendations that only restate the condition ("ensure receipts are posted on time"), that address a person rather than a process, or that management could not show as done. Suggest a sharper version of each flagged one.
```

**Check:** Read each sharper version against the facts. It must still be something management can do and show as done.

## Issue follow-up

### Merge four trackers into one, with aging as at a set date

**When to use:** Merging several issue trackers with different layouts into one master tracker, aged as at a set date.

```text
The four Tracker_*.xlsx files are confidential and contain personal data. Read only their column headers.
1. Propose a mapping from each file's columns to this standard layout: Issue ID, Report ID, Entity, Title, Rating, Owner ID, Original Due Date, Current Due Date, Revisions, Revision Approval Ref, Status, Evidence Ref, Validated By, Validated Date. Show me the mapping as a table and wait for my approval.
2. Then write a local Python script merge_trackers.py that applies the mapping and writes Issues_Tracker_Master.xlsx with sheets: Tracker (no names or emails, Owner ID only), Aging (open issues by rating and bucket: not due, 1-30, 31-60, 61-90, over 90 days overdue), Escalate (High issues more than 30 days overdue, and any issue on its third or later revision), Exceptions (closed with no Evidence Ref or no Validated By, revised date with no approval ref, missing owner or due date), Mapping, Parameters (files, row counts per file, as-at date, date run).
Keep the as-at date and the escalation rules as settings at the top. Print only row counts per sheet.
```

**Check:** Rows in from the four files equal rows in the Tracker sheet. Any difference is explained (duplicates, blank rows) on the Parameters sheet.

## Checking AI output

### Checking a draft committee summary claim by claim

**When to use:** Before a draft report or committee paper goes anywhere: match every claim to a source.

```text
Read AC_SOX_Summary_Q3_Draft.docx, the files in WP_Summaries\ and the PDFs in Policies\. Do not use your own knowledge of any policy or standard.
For every factual claim in the draft, write a row in Claims_Check.xlsx with: Claim (quoted), Source cited in the draft, Source found (file name, section, and the quoted text), Draft wording, Source wording, and Comparison (Same, Stronger than source, Weaker than source, or NOT FOUND).
Treat every policy section, clause, number, rating and conclusion as a claim.
Do not edit the draft. At the end, list the claims marked Stronger, Weaker or NOT FOUND.
```

**Check:** Open the policy yourself and confirm the NOT FOUND. A scanned PDF can be misread; the clause may exist under another heading.

### Separate what it read from what it recalled

**When to use:** After any draft that mentions standards, frameworks, regulations, policy clauses or figures.

```text
In the draft you just wrote, list every standard, framework, regulation, policy clause or figure you mentioned. For each, say whether it came from a file in this folder (give file and page) or from memory. Don't change the draft.
```

**Check:** Look up every item marked as from memory in the source document before it stays in the draft.

### Catch arithmetic done in its head

**When to use:** Any time the assistant gives you a total or other figure.

```text
Before you give me any total, tell me how you calculated it: a script, an Excel formula, or in your head. If it was in your head, recalculate it with a script and show both figures.
```

**Check:** Both figures agree. If they don't, use the script's figure and find out why.

---

More prompts, worked walkthroughs and answer keys: *AI for Internal Auditors* (https://aiforseries.github.io/ai-for-internal-auditors/)
