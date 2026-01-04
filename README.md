Google sheet: https://docs.google.com/spreadsheets/d/1Cd2O13E66cZqwp0sTtFdzHk33glmjeboRr13hsL30HA/edit?usp=sharing

App Script:

const SPREADSHEET_ID = "1Cd2O13E66cZqwp0sTtFdzHk33glmjeboRr13hsL30HA";
const SHEET_NAME = "Data";

function doPost(e) {
  const sheet = SpreadsheetApp.openById(SPREADSHEET_ID).getSheetByName(SHEET_NAME);

  const rows = JSON.parse(e.postData.contents);
  rows.forEach(r => {
    sheet.appendRow([
      r.participant_id,
      r.timestamp
    ]);
  });

  return ContentService.createTextOutput("OK");
}
