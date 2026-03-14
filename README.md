# DMARC.helper
Load DMARC aggregate XML reports and view their records in a readable table with per‑record details.

## Processing results
Processing the results is easy in Spreadsheets with the following functions after Copy-pasting the list to the Sheet 
* List SPF & DKIM fails on another Sheet: `=FILTER(Sheet1!A:F, Sheet1!E:E = "fail", Sheet1!F:F = "fail")`
* List unique results filtered by Source IP on another Sheet `=UNIQUE(fails!A:F)`

### IP Whoois
Apps Script Extenssion for Google spreadsheet to return real-time geolocation over raw registry data for Source IPs

```
function IPINFO_SINGLE(ip) {
  if (!ip) return ["", "", "", ""];

  const cleanIP = String(ip).trim();
  const url = "https://ipwho.is/" + encodeURIComponent(cleanIP);

  const response = UrlFetchApp.fetch(url, { muteHttpExceptions: true });
  const data = JSON.parse(response.getContentText());

  if (!data.success) {
    return ["Not found", "Not found", "Not found", "Not found"];
  }

  return [
    data.hostname || "",
    data.connection?.isp || "",
    data.country || "",
    data.city || ""
  ];
}

function IPINFO(range) {
  // ARRAYFORMULA passes a 2D array → process row by row
  if (!Array.isArray(range)) {
    return IPINFO_SINGLE(range);
  }

  return range.map(row => {
    const ip = row[0];
    if (!ip) return ["", "", "", ""];
    return IPINFO_SINGLE(ip);
  });
}

```

#### Usage
`=ARRAYFORMULA(IF(B2:B="",,IPINFO(B2:B)))` in the firs cell of a column after the DMARC results, where Column B contains the Source IPs.
