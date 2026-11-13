# CA student prompts (India)

Copy-ready prompts from *AI for CA Students* by Chandan Gupta, for an AI assistant such as Claude Code working in a folder on your own computer. The file names, columns, thresholds, dates and names come from the book's worked examples, where all names and figures are made up. Replace them with your own. Each prompt is followed by a check to do before you rely on the result.

## Tutor mode: check, don't solve

### Turning "solve this" into "check this"

**When to use:** You have attempted a question and your answer does not match.

```text
Read recon_q.txt (the question) and my_attempt.jpg (my handwritten attempt).
Do NOT solve the question and do NOT tell me the correct final figure.
1. Go through my attempt line by line and tell me the FIRST line where I went wrong.
2. Say what kind of mistake it is (concept, arithmetic, copying a figure, wrong sign, format).
3. Ask me one question that would help me see it myself.
Stop there. I will redo it and show you again.
```

**Check:** Your redo reaches the teacher's figure by your own working.

### Getting a hint when you are truly stuck

**When to use:** You are stuck on a question and have not started.

```text
I am stuck on the question in dt_question.txt and have not started.
Do not solve it. Give me hints one at a time, smallest first:
Hint 1: which concept or standard this question is really testing (no figures).
Wait for me to say "next" before each further hint.
Hint 2: what I should list or compute first.
Hint 3: the format of the working I should draw up.
If I ask for the full solution, remind me that I asked you not to, and give me Hint 3 instead.
```

**Check:** You wrote a full attempt after the hints, on paper.

### Find out if you really understand

**When to use:** Testing whether you really understand a concept. Change the topic to your own.

```text
I think I understand the difference between a provision and a contingent liability. Ask me five questions, one at a time, from easy to hard, to find out if I really do. Don't tell me the answer to a question until I have replied.
```

**Check:** Answer each question before you read the next. If you could not answer two, go back to your notes.

### Review a theory answer without rewriting it

**When to use:** Getting feedback on a theory answer without having it rewritten.

```text
Here is my answer to a 4-mark theory question, written in 6 minutes: [paste]. Do not rewrite it. Tell me which key point is missing, which sentence wastes words, and how an examiner might mark the structure.
```

**Check:** Check the missing point against your study material or the suggested answer, not the AI alone.

## Study plan

### A plan built from your own chapter list and real week

**When to use:** Building a study plan from your own chapter list and your real week.

```text
Read my_week.txt and chapters.xlsx in this folder. My exam starts on [date]. Today is [date].
1. From my_week.txt, work out how many study hours I realistically have each week. Assume 80% of the free time I listed, not 100%. Show me the calculation.
2. Make study_plan.xlsx with one row per day until the exam: Date, Planned hours, Chapter or task, Type (first reading, practice, revision 1, revision 2, mock), and a blank Done column.
3. Give weaker chapters (1 or 2) more time and schedule them earlier. Every chapter must get at least two revision rounds. Keep the last 3 weeks for revision and mocks only.
4. Keep one day a week with no new chapter: catch-up only.
Before you make the file, ask me up to 5 questions about anything you are unsure of.
```

**Check:** The weekly hours match your real week. If they look generous, they are.

## Revision and self-tests

### A self-test that marks you against your own notes

**When to use:** A self-test from your own revision sheets, marked against your own notes.

```text
Make a self-test from sheets/ch03_sheet.md, ch04_sheet.md and ch07_sheet.md, with the matching notes files as the source.
Write 10 questions in test_01.md: 4 short recall, 3 "explain why", 3 small made-up numerical or situational questions. Mix the chapters. Do not include the answers in this file.
Save the model answers separately in test_01_answers.md, each with the notes file it came from. Don't show them to me.
I will write my answers in my_answers_01.md. When I say "mark it", compare my answers with test_01_answers.md, give each a mark out of 2, say what was missing, and tell me which chapter to revise first.
```

**Check:** Where it marked you wrong, check its model answer against your notes before you accept it.

## Practice questions

### Fresh practice with the numbers changed

**When to use:** Fresh made-up practice on an idea you have just fixed, to see if the fix sticks.

```text
Write 3 new made-up practice questions on process costing with normal and abnormal loss, at CA Inter level, similar in difficulty to practice/pc_q3.txt but with different figures and one small twist each (for example abnormal gain, or opening work in progress).
Do not copy or closely paraphrase any question from ICAI material, past papers or textbooks. Make them up.
Save the questions to practice/pc_variants.txt and the full worked solutions to practice/pc_variants_solutions.txt. Before saving, work each solution twice in different ways (for example units first, then cost) and tell me only whether both ways agree. Do not show me the solutions.
```

**Check:** If it said the two methods disagreed on any question, treat that solution as suspect and check it yourself, or discard the question.

### Check a tax computation against your own figures

**When to use:** Checking a tax computation using only the rates and limits from your own study material.

```text
Check my computation of total income in practice/tax_q5_attempt.xlsx for the made-up question in practice/tax_q5.txt. Use ONLY the rates, limits and deductions listed in my_tax_figures.txt, which I copied from my own study material for my attempt. If you think a figure in that file is wrong or out of date, tell me separately; do not use your own. Find the first wrong cell, the type of mistake, and ask me one question. No corrected computation.
```

**Check:** The figures in your rates file match your current study material for the attempt you are writing.

## Accounting and auditing standards

### Explain, teach back, then quiz: Ind AS 115

**When to use:** Learning a standard: explanation, teach-back, then a quiz. Change the standard and example to your own.

```text
Topic: Ind AS 115, Revenue from Contracts with Customers. My notes are in notes/indas115.docx.
Step 1. Explain the five-step model in plain words in under 200 words, with one small made-up example (a company selling a machine with installation and a year of support). Don't quote paragraph numbers.
Step 2. Then stop and ask me to explain, in my own words, how to decide whether the installation is a separate performance obligation. Wait for my answer.
Step 3. Tell me what my explanation got right, what it missed and anything I got wrong, compared with my notes. If my notes and your understanding differ, say so and tell me exactly what to check in the standard; don't decide it for me.
Step 4. Quiz me with 5 questions, one at a time, harder each time. The last two must be short made-up fact patterns.
At the end, list every rule you relied on so I can check each one in the standard.
```

**Check:** Each rule on its "relied on" list, looked up in the standard or your study material.

### A compare table for the ones you mix up

**When to use:** A comparison table for standards you mix up, built only from your notes.

```text
Read notes/sa500.docx, notes/sa230.docx and notes/sa580.docx (my own notes).
Make compare_sa.md with a table: rows are Purpose, What it requires the auditor to do, Typical examples, Common exam confusion; columns are the three standards.
Use only my notes. Where my notes don't cover a cell, write "not in my notes" rather than filling it in.
Below the table, write 4 short made-up situations, and for each ask me which standard is most relevant and why. Don't answer them; wait for my replies, then mark them.
```

**Check:** Check the table's "requires" row against the standards' requirements, not against memory.

## Mock tests

### Where the marks and the minutes went

**When to use:** Finding where marks and minutes went in a mock you have marked yourself.

```text
Read mock_log.xlsx (sheet Mock1). It is my own record of a mock I wrote and marked myself.
1. Add columns: Minutes over or under plan, and Marks lost.
2. Using my Why column, put each lost mark into one of: knowledge, method, arithmetic, presentation, not attempted. If my note is unclear, put "ask me" and ask me at the end.
3. Add a sheet Analysis with: total marks lost by category, the questions where I lost the most time, and a one-line comment on the pattern.
4. Then give me three specific changes for my next mock, each one I can do in the exam hall without any help. No general advice like "manage time better".
```

**Check:** The marks and minutes totals match your marked paper.

## Client data in articleship

### The script pattern, as a prompt

**When to use:** The basic safe pattern for any task on client files.

```text
This folder has client files. Do not open them. Read only the column headings and the first 3 rows of each, using a short Python script.
Then write a script that does the task, run it here, and print only counts and totals. Save any lists to an Excel file in this folder and do not print them.
```

**Check:** The reply shows only counts and totals. Get your principal's permission before you use AI on any client file.

### Make a masked copy

**When to use:** Making a masked copy of a client file before analysis.

```text
Look only at the headings of [file]. Write a script that makes a copy called [file]_masked.xlsx in which party names become P001, P002 and so on, PAN and account numbers are removed, and narrations are deleted. Save the key to key_DO_NOT_OPEN.xlsx. Run it and print only the row count. Do not open either output.
```

**Check:** The masked copy has the same row count as the original, and you keep the key file closed.

### Make a practice file with the same shape as the real one

**When to use:** Making a practice file with the same shape as a real one, with planted problems.

```text
Make two made-up practice files for a bank reconciliation, for an imaginary steel trader. Use no real names, banks or account numbers.
1. practice_bank_book.xlsx with columns Date, Vch Type, Vch No., Particulars, Chq No., Debit, Credit, for October 2025, about 60 rows, with an opening balance row.
2. practice_statement.xlsx with columns Txn Date, Description, Chq/Ref No, Withdrawal (Dr), Deposit (Cr), Balance.
Plant 5 reconciling items of different kinds, and write what you planted to answers_DO_NOT_OPEN.md.
Do not tell me what you planted.
```

**Check:** Do the reconciliation yourself first, then open the answers file and compare.

## Vouching and ledger scrutiny

### A scrutiny workbook from the expense ledgers

**When to use:** Ledger scrutiny with your principal's permission, without the assistant reading the rows.

```text
This is client data. My principal has allowed me to use Claude Code on it, on the condition that you do not read the data rows.
Read only the column headers of Client_Expenses_FY26.xlsx and tell me which column you think is the date, ledger, party, voucher number, amount and narration. Wait for me to confirm.
Then write a Python script scrutiny.py that runs here, on every row, and flags:
1. Amounts of ₹50,000 or more that are exact multiples of ₹10,000.
2. Entries dated on a Sunday.
3. Entries dated 25 to 31 March 2026.
4. Narration blank, or containing: adjustment, as discussed, as per director, misc, cash.
5. Same party and same amount within 7 days of each other (possible duplicate).
6. In ledgers with "Repair" in the name, narrations containing: new, purchase, installation.
Save Scrutiny_Client_FY26.xlsx with a Summary sheet (test, rule, count, total amount), one sheet per test, and a Parameters sheet (file name, rows read, total amount, the rules above).
On screen, show me only the Summary. Ask before running anything.
```

**Check:** Rows read and total amount on the Parameters sheet match the export when you add it up in Excel.

### A seeded sample and a vouching tracker

**When to use:** A seeded sample and a vouching tracker to fill in at the client's office.

```text
Using Client_Expenses_FY26.xlsx and Scrutiny_Client_FY26.xlsx in this folder, write a script sample.py that runs here and does not print any data rows.
1. Take every entry from the "possible duplicate" and "possible capital" sheets.
2. From all other entries of ₹5,000 or more, pick 25 at random using seed 2026.
3. Save Vouching_Tracker_Client.xlsx with one row per sample item: Date, Voucher No, Ledger, Party, Amount, Narration, Reason selected, and these blank columns for me: Bill seen (Y/N), Bill date and number, Approved by, Payment mode and reference, Agrees to ledger (Y/N), Remarks, W/P ref.
4. Add a Parameters sheet: population count and value, the rule for step 2, the seed, and the date and time of the run.
On screen show only: how many items came from each step, and their total value.
```

**Check:** Run sample.py a second time and confirm you get exactly the same 25 random items.

## Excel and Tally exports

### Clean a Tally ledger export into one table

**When to use:** Cleaning a Tally ledger export (one sheet per ledger) into one table.

```text
This is client data. Don't print data rows. Look at the first 12 rows of the first two sheets of Client2_Expenses_FY26.xlsx only, to learn the layout, and describe the layout back to me in plain words.
Then write clean_export.py that runs here and, for every sheet:
1. Skips title rows, blank rows, and rows whose Particulars is "Opening Balance", "Carried Over", "Brought Forward" or "Total".
2. Moves a narration line (a row with no date and no amount) into a Narration column of the entry above it.
3. Converts text dates like 02-Apr-25 to real dates and amounts like "1,25,000.00 Dr" to numbers, debit positive, credit negative.
4. Adds the sheet name as the Ledger column.
Save Client2_Expenses_clean.xlsx with sheets Clean (all entries) and Check (for each ledger: total per the export's own Total row, total after cleaning, difference).
Print only the Check sheet and the number of rows written.
```

**Check:** Every difference on the Check sheet is zero. If not, find the row before you use the file.

## Reconciliations

### A GSTR-2B match with invoice numbers cleaned first

**When to use:** A GSTR-2B match where invoice numbers are written differently on each side.

```text
This is client data. Do not open or print data rows.
Read only the headers of Purchase_Register_Sep.xlsx and GSTR2B_Sep.xlsx and tell me which columns you will use for supplier GSTIN, invoice number, invoice date, taxable value and tax. Wait for me to confirm.
Then write recon_2b.py that runs here and:
1. Cleans invoice numbers on both sides for matching only: upper case, remove spaces, slashes, hyphens and leading zeros. Keep the original numbers in the output.
2. Matches on GSTIN plus cleaned invoice number.
3. Puts every row in one of: Matched, Amount differs (taxable or tax differs by more than ₹1), Only in books, Only in 2B.
Save Recon_2B_Sep.xlsx with one sheet per bucket and a Summary sheet with counts and totals per bucket and a check that rows in equal rows out on each side.
Print only the Summary.
```

**Check:** Open five "Only in books" rows and search for them in 2B by supplier name, in case the GSTIN is wrong on one side.

## Working papers and review

### From rough notes to a working-paper narrative

**When to use:** Turning your rough notes into a working-paper narrative without anything invented.

```text
Read repairs_notes.txt. These are my own notes on work I have done. Turn them into a working-paper narrative under four headings: Objective, Procedure, Results, Conclusion.
Rules:
- Use only facts in my notes. Do not add any figure, date, document or conclusion that isn't there.
- Where something a reviewer would need is missing (a source, a count, a W/P reference, who approved something), write [MISSING: what] in the text instead of guessing.
- Plain English, short sentences, past tense for work done.
Save it as Repairs_WP_draft.docx and, separately, list all the [MISSING] items for me.
```

**Check:** Every figure in the draft is one you can point to in your files.

### Replying to review points, one by one

**When to use:** Replying to your manager's review points, honestly marking what is still open.

```text
Read review_points.txt. Each point from my manager is followed by my rough answer.
For each point, draft a reply of at most three sentences that says what was done, where the evidence is (W/P reference), and the result. If my rough answer says "not done" or is unclear, write "OPEN:" and what I still have to do. Don't invent work I haven't described.
Save a table to Review_Replies.xlsx with columns: Point no., Point, Reply, Status (Cleared / Open), W/P ref.
Then tell me which of my rough answers are weakest and why, in one line each.
```

**Check:** Each "Cleared" reply points to evidence that is actually in the file.

---

More prompts, worked walkthroughs and answer keys: *AI for CA Students* (https://aiforseries.github.io/ai-for-ca-students/)
