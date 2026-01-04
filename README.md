Google sheet: https://docs.google.com/spreadsheets/d/1Cd2O13E66cZqwp0sTtFdzHk33glmjeboRr13hsL30HA/edit?usp=sharing

App Script:

const SPREADSHEET_ID = "1Cd2O13E66cZqwp0sTtFdzHk33glmjeboRr13hsL30HA";
const SHEET_NAME = "Data";

function doGet() {
  return ContentService
    .createTextOutput("ready")
    .setMimeType(ContentService.MimeType.TEXT);
}

function doPost(e) {
  const sheet = SpreadsheetApp
    .openById(SPREADSHEET_ID)
    .getSheetByName(SHEET_NAME);

  if (!sheet) {
    return ContentService.createTextOutput("Sheet not found");
  }

  try {
    if (!e || !e.postData || !e.postData.contents) {
      sheet.appendRow([
        new Date().toISOString(),
        "NO POST DATA"
      ]);
      return ContentService.createTextOutput("No post data");
    }

    const rows = JSON.parse(e.postData.contents);

    if (!Array.isArray(rows)) {
      sheet.appendRow([
        new Date().toISOString(),
        "INVALID PAYLOAD",
        e.postData.contents
      ]);
      return ContentService.createTextOutput("Invalid payload");
    }

    rows.forEach(r => {
      sheet.appendRow([
        r.participant_id || "",
        r.timestamp || "",
        r.block || "",
        r.trial || "",
        r.condition || "",
        r.task || "",
        r.global_letter || "",
        r.local_letter || "",
        r.response || "",
        r.correct || "",
        r.rt || ""
      ]);
    });

    return ContentService
      .createTextOutput("OK")
      .setMimeType(ContentService.MimeType.TEXT);

  } catch (err) {
    sheet.appendRow([
      new Date().toISOString(),
      "ERROR",
      err.message
    ]);

    return ContentService
      .createTextOutput("ERROR: " + err.message)
      .setMimeType(ContentService.MimeType.TEXT);
  }
}
