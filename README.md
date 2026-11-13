# AI prompts for audit and finance

Copy-ready prompts for auditors, accountants and finance teams who use AI tools such as Claude Code on real files: ledgers, trial balances, registers, bank statements, GST and tax downloads, and Excel models. Each prompt comes with a line on when to use it and a check to do before you rely on the result.

The prompts are taken from The AI For Series books by Chandan Gupta, CA, CIA. There are 100 here, 20 per book. The books have the rest, with worked walkthroughs on practice files and answer keys.

## The method behind the prompts

1. Don't paste data into a chat. Ask the AI to read only the column headers, then write a script that runs on your own computer over every row.
2. Have the script print only counts, totals and the names of the files it saves. The detail goes into an Excel workbook you open yourself.
3. Check every output against control totals: rows in equal rows out, and totals agree to the ledger, the trial balance or the source report.
4. Keep a clean working folder per job, holding copies of only the files that job needs. Originals stay untouched.
5. Keep judgment with you. The script finds exceptions; you decide what they mean.

## Files

| File | For | Prompts |
|---|---|---|
| [internal-audit.md](internal-audit.md) | Internal auditors: completeness, sampling, P2P, journals, access, observations, follow-up | 20 |
| [excel-finance.md](excel-finance.md) | Finance teams: cleaning exports, reconciliations, close, cash flow, model review | 20 |
| [ca-practice.md](ca-practice.md) | Chartered accountants in India: AIS and TDS, GST, statutory audit, Schedule III, forensic work | 20 |
| [tax-practice.md](tax-practice.md) | Tax practitioners in India: GSTR-1, 2B and 3B, ITR season, TDS, bookkeeping, notices | 20 |
| [ca-students.md](ca-students.md) | CA students in India: tutor-style study prompts, mocks, articleship vouching and reconciliations | 20 |
| [LICENSE](LICENSE) | CC BY 4.0 | |

File names, columns, thresholds and names in the prompts come from the books' worked examples, where all companies, people and figures are made up. Replace them with your own. Where a prompt depends on law, rates or thresholds, it asks the AI to use your own master file or the official text, not its memory.

## From the books

- *AI for Internal Auditors*: https://aiforseries.github.io/ai-for-internal-auditors/
- *Excel + AI for Finance Teams*: https://aiforseries.github.io/excel-ai-for-finance/
- *AI for CAs*: https://aiforseries.github.io/ai-for-cas/
- *AI for Tax Practitioners*: https://aiforseries.github.io/ai-for-tax-practitioners/
- *AI for CA Students*: https://aiforseries.github.io/ai-for-ca-students/

All books in the series: https://aiforseries.github.io/

## Notes

- This is an independent project. It is not affiliated with, sponsored or endorsed by Anthropic, the Institute of Chartered Accountants of India (ICAI) or The Institute of Internal Auditors (The IIA). Claude and Claude Code are products and trademarks of Anthropic, PBC.
- The prompts are for education and are not professional advice. Follow your firm's or employer's policy on client and company data before using any AI tool on it.
- License: [CC BY 4.0](LICENSE). You may copy and adapt the prompts, including at work. Please credit "The AI For Series, Chandan Gupta" with a link to this repository or to https://aiforseries.github.io/.
