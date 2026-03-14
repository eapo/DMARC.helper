# DMARC.helper
Load DMARC aggregate XML reports and view their records in a readable table with per‑record details.

## Processing results
Processing the results is easy in Spreadsheets with the following functions after Copy-pasting the list to the Sheet 
* List SPF & DKIM fails on another Sheet: `=FILTER(Sheet1!A:Z, Sheet1!E:E = "fail", Sheet1!F:F = "fail")`
* List unique results filtered by Source IP on another Sheet `=UNIQUE(fails!A:Z)`
