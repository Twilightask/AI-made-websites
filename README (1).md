# 📘 TestXpress - BBA CET Mock Test Platform

**TestXpress** is a responsive and user-friendly mock test platform built for students preparing for the **BBA CET entrance exam**. It offers section-wise tests, live scoring, time tracking, result history, and user login with Google Sheets as a backend.

---

## 🚀 Features

- 🔐 **Student Login & Registration**
  - New users can register with details (Name, Std, College, Phone)
  - Returning users can log in with phone number and name
  - Data saved via **Google Apps Script** to Google Sheets

- 🧠 **Subject-Wise Tests**
  - English
  - Aptitude
  - General Knowledge
  - Logical Reasoning
  - Computer Basics

- ⏱️ **Timer-Based Tests**
  - Auto-submission when time runs out
  - Timer is shown at the top of each test

- 📊 **Instant Score Display**
  - Shows score and time taken
  - Personalized feedback based on score

- 🗂️ **Score History Page**
  - Tracks all past test attempts in table format
  - Data stored in `localStorage`

- 📩 **Security Features**
  - Tab switching warning and termination
  - Disabled right-click and copy
  - Source-code view prevention

---

## 🧱 Tech Stack

- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Google Apps Script (Web App API)
- **Storage:** Google Sheets + Browser LocalStorage

---

## 📂 Folder Structure

```bash
/ (root)
│
├── index.html                # Registration Page
├── login.html                # Login Page
├── dashboard.html            # Test Dashboard
├── score-history.html        # Score History Page
├── result.html               # Result Display
│
├── english-test.html         # English Test Page
├── aptitude-test.html        # Aptitude Test Page
├── gk-test.html              # General Knowledge Test Page
├── computer-basic-test.html  # Computer Test Page
├── logical-reasoning-test.html  # Logical Reasoning Page
│
├── style.css                 # Global Stylesheet
├── login-style.css           # Login Page Styling
├── index-style.css           # Registration Page Styling
```

---

## 📝 Google Apps Script Setup

1. Open [Google Sheets](https://sheets.google.com)
2. Go to `Extensions > Apps Script`
3. Paste the following code:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Sheet1");
  sheet.appendRow([
    e.parameter.firstName || "",
    e.parameter.lastName || "",
    e.parameter.std || "",
    e.parameter.college || "",
    e.parameter.phone || "",
    new Date()
  ]);
  return ContentService.createTextOutput(JSON.stringify({ result: "success" }))
                       .setMimeType(ContentService.MimeType.JSON);
}

function doGet(e) {
  var name = e.parameter.name;
  var phone = e.parameter.phone;
  if (!name || !phone) return ContentService.createTextOutput("fail")
                      .setMimeType(ContentService.MimeType.TEXT)
                      .setHeader("Access-Control-Allow-Origin", "*");
  var data = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Sheet1").getDataRange().getValues();
  for (var i = 1; i < data.length; i++) {
    let fullName = data[i][0] + " " + data[i][1];
    if (fullName.trim().toLowerCase() === name.trim().toLowerCase() && data[i][4] == phone)
      return ContentService.createTextOutput("success")
                           .setMimeType(ContentService.MimeType.TEXT)
                           .setHeader("Access-Control-Allow-Origin", "*");
  }
  return ContentService.createTextOutput("fail")
                       .setMimeType(ContentService.MimeType.TEXT)
                       .setHeader("Access-Control-Allow-Origin", "*");
}
```

4. Deploy it as a web app:
   - Click `Deploy > Manage Deployments`
   - Set **access** to "Anyone"
   - Copy the **Web App URL** and use it in your frontend code

---

## 📌 To Do (Future Improvements)

- Export test history to PDF
- Admin panel for adding/editing questions
- Timer pause/resume logic
- Leaderboard based on highest scores

---

## 🙌 Made with passion by Aayush Kumbhar

> This project is part of a computer engineering course assignment focused on front-end development, JavaScript logic, and cloud integration with Google Sheets.