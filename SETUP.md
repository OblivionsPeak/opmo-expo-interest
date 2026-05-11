# Google Sheets Backend Setup — Expo Interest Form

Every submission from any device flows into one Google Sheet. Data persists permanently.

**Requires:** A Google account (Gmail)

---

## One-time setup (~5 minutes)

### Step 1 — Create the Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com) and create a new blank spreadsheet
2. Name it **OpMo Expo Interest**

### Step 2 — Open Apps Script

1. In the spreadsheet, click **Extensions → Apps Script**
2. Delete any existing code in the editor
3. Paste the following script:

```javascript
function doPost(e) {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getActiveSheet();

  if (sheet.getLastRow() === 0) {
    sheet.appendRow(['Name','Email','Phone','Interests','Veteran Status','Submitted']);
    sheet.getRange(1,1,1,6).setFontWeight('bold').setBackground('#c4a87a').setFontColor('#0d1117');
    sheet.setFrozenRows(1);
  }

  var d = e.parameter;
  sheet.appendRow([
    d.name          || '',
    d.email         || '',
    d.phone         || '',
    d.interests     || '',
    d.veteranStatus || '',
    new Date().toLocaleString('en-US')
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ status: 'ok' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Click **Save** (disk icon) and name the project **OpMo Expo Interest**

### Step 3 — Deploy as a Web App

1. Click **Deploy → New deployment**
2. Click the gear icon next to "Select type" → choose **Web app**
3. Set:
   - **Execute as**: Me
   - **Who has access**: Anyone
4. Click **Deploy**
5. When prompted, click **Authorise access** → sign in → click **Advanced → Go to OpMo Expo Interest (unsafe)** → **Allow**
6. **Copy the Web App URL** — it looks like:
   `https://script.google.com/macros/s/ABC.../exec`

### Step 4 — Add the URL to the form

Open `index.html` and find this line near the top of the `<script>` block:

```javascript
const APPS_SCRIPT_URL = '';
```

Paste your URL inside the quotes:

```javascript
const APPS_SCRIPT_URL = 'https://script.google.com/macros/s/YOUR_ID_HERE/exec';
```

Commit and push — every submission now writes a permanent row to your Google Sheet.

---

## Exporting to Excel

In Google Sheets go to **File → Download → Microsoft Excel (.xlsx)** — done.

Or use the **Export → Excel** button in the Admin Panel to download everything recorded on your device.
