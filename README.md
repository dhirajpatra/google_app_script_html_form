# google_app_script_html_form
How to use Google App Script to save the data into google spredsheet  

### Follow the steps
To handle form submission and CSV file appending using only HTML and jQuery, you can use a serverless approach with Google Sheets as a simple backend. The form data will be submitted to a Google Sheets document through Google Apps Script. Here's how you can achieve this:

Create a Google Sheets document to store the form data. In the Google Sheets document, add two headers in the first row: "Name" and "Email."

Create a new Google Apps Script bound to the Google Sheets document:

Open the Google Sheets document.
Click on "Extensions" in the top menu.
Select "Apps Script" to open the Google Apps Script editor.
Check Google Apps Script Permissions: Ensure that the web app is deployed correctly with the correct permissions. It should be set to "Anyone, even anonymous" for simplicity.

Google Apps Script Code: Make sure the Google Apps Script code is correct and does not have any errors. Double-check the doPost function and the Spreadsheet ID (YOUR_SPREADSHEET_ID) to ensure they are correct.
Replace the default code with the following:

`function doGet(e) {
  if (e.parameter && Object.keys(e.parameter).length > 0) {
    var ss = SpreadsheetApp.openById('SPREDSHEET-ID');
    var sheet = ss.getSheetByName('Sheet1'); // Change the sheet name as needed
    var data = [];
    
    // Convert the query parameters into an array of data
    for (var key in e.parameter) {
      data.push([key, e.parameter[key]]);
    }
    
    // Append the data to the Google Sheets
    sheet.getRange(sheet.getLastRow() + 1, 1, data.length, data[0].length).setValues(data);
    
    return ContentService.createTextOutput('Data saved successfully.');
  }
  
  // Return a simple HTML response if no data is provided
  var htmlOutput = '<h1>Form Submission</h1><p>No data submitted.</p>';
  return HtmlService.createHtmlOutput(htmlOutput);
}`

Save the Google Apps Script project and deploy it as a web app:

Click on the floppy disk icon to save the project.
Click on "Deploy" in the top menu.
Select "New Deployment" and configure the settings as follows:
Select "Web app" as the "Type."
Choose "Execute the app as" your Google account.
Set "Who has access" to "Anyone, even anonymous."
Click "Deploy" and authorize the permissions.

Once deployed, you will get a URL for the web app. Copy the URL; it will look something like this:
`https://script.google.com/macros/s/.../exec`

Replace 'YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL' with the URL you copied from the deployed web app.

Now, when you submit the form, the data will be sent to the Google Sheets document, and the Google Apps Script will append it as a new row.

Make sure you have deployed the Google Apps Script web app correctly:

Open your Google Apps Script project.
Click on the "Deploy" menu.
Choose "New deployment."
In the "Deploy as" drop-down, select "Web app."
Set "Project version" to "New."
Under "Execute the app as," choose your Google account.
Under "Who has access to the app," choose "Anyone, even anonymous."
Click "Deploy."
Ensure that the URL of the Google Apps Script web app is correct in your HTML page's JavaScript code:

`$.ajax({
  type: 'POST',
  url: 'https://script.google.com/macros/s/AKfycbyBlhtF1_IZkZVGRytl9b54v91BB07E8Hwj6XbYTxYblwiACJLDi5Ne89fjRSNC12Wa/exec',
  data: JSON.stringify(data),
  dataType: 'text',
  contentType: 'application/json',
  // Rest of the code...
});
`
Replace the URL with the correct URL of your deployed Google Apps Script web app.

Double-check that your Google Apps Script code is correctly handling the form data:

Now you can run your html file anywhere to save the data into that excel sheet. You have to give the permission to google drive. 
