<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart Attendance QR System</title>

<script defer src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
<script defer src="https://unpkg.com/html5-qrcode"></script>
<script defer src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif;
}

body {
    background: #f1f5f9;
    color: #1e293b;
}

button {
    cursor: pointer;
    border: none;
    border-radius: 7px;
    padding: 10px 15px;
    background: #2563eb;
    color: white;
}

button:hover {
    opacity: .85;
}

input, select {
    width: 100%;
    padding: 11px;
    margin: 7px 0 14px;
    border: 1px solid #cbd5e1;
    border-radius: 6px;
}

h2 {
    margin-bottom: 18px;
}

.login-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    padding: 20px;
}

.login-box {
    background: white;
    width: 400px;
    padding: 30px;
    border-radius: 15px;
    box-shadow: 0 5px 25px #0001;
}

.login-box h1 {
    text-align: center;
    margin-bottom: 10px;
    color: #1d4ed8;
}

.login-box p {
    text-align: center;
    margin-bottom: 20px;
    color: #64748b;
}

.hidden {
    display: none !important;
}

.layout {
    display: flex;
    min-height: 100vh;
}

.sidebar {
    width: 250px;
    background: #172554;
    color: white;
    padding: 20px;
    flex-shrink: 0;
}

.sidebar h2 {
    font-size: 21px;
    margin-bottom: 25px;
}

.sidebar button {
    width: 100%;
    display: block;
    text-align: left;
    margin: 5px 0;
    background: transparent;
    color: #dbeafe;
}

.sidebar button:hover,
.sidebar button.active {
    background: #2563eb;
}

.main {
    flex: 1;
    min-width: 0;
}

.topbar {
    background: white;
    padding: 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 2px 8px #0001;
    gap: 10px;
}

.content {
    padding: 25px;
}

.cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));
    gap: 15px;
    margin-bottom: 25px;
}

.card {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 8px #0001;
}

.card h3 {
    font-size: 14px;
    color: #64748b;
    margin-bottom: 12px;
}

.card h1 {
    color: #1d4ed8;
}

.panel {
    background: white;
    padding: 20px;
    border-radius: 10px;
    margin-bottom: 20px;
    box-shadow: 0 2px 8px #0001;
    overflow-x: auto;
}

table {
    width: 100%;
    border-collapse: collapse;
    min-width: 650px;
}

th, td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #e2e8f0;
}

th {
    background: #eff6ff;
}

.badge {
    padding: 5px 9px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: bold;
}

.Present {
    background: #dcfce7;
    color: #15803d;
}

.Late {
    background: #fef3c7;
    color: #a16207;
}

.Absent {
    background: #fee2e2;
    color: #b91c1c;
}

.Excused {
    background: #dbeafe;
    color: #1d4ed8;
}

.form-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 10px;
}

.actions {
    display: flex;
    gap: 5px;
    flex-wrap: wrap;
}

.danger {
    background: #dc2626;
}

.success {
    background: #16a34a;
}

.warning {
    background: #d97706;
}

.gray {
    background: #64748b;
}

#qrBox {
    display: flex;
    justify-content: center;
    padding: 20px;
}

.notification {
    padding: 12px;
    background: #eff6ff;
    margin: 8px 0;
    border-radius: 6px;
}

.student-profile {
    text-align: center;
    padding: 25px;
}

.student-profile #studentQR {
    display: flex;
    justify-content: center;
    margin: 20px;
}

@media(max-width:800px) {
    .layout {
        flex-direction: column;
    }

    .sidebar {
        width: 100%;
    }

    .sidebar button {
        display: inline-block;
        width: auto;
    }

    .content {
        padding: 15px;
    }

    .topbar {
        flex-wrap: wrap;
    }
}
</style>
</head>

<body>

<!-- LOGIN SCREEN -->

<div id="loginScreen" class="login-container">

<div class="login-box">

<h1>SMART ATTENDANCE</h1>
<p>QR Code Student Attendance System</p>

<label>Login As</label>

<select id="loginRole">
<option value="student">Student</option>
<option value="admin">Administrator</option>
</select>

<input id="loginID" placeholder="Student ID / Username">

<input id="loginPassword" type="password" placeholder="Password">

<button style="width:100%" onclick="login()">LOGIN</button>

<br><br>

<button class="success" style="width:100%"
onclick="showRegistration()">REGISTER AS STUDENT</button>

<br><br>

<small>Administrator demo: admin / admin123</small>

</div>
</div>


<!-- STUDENT REGISTRATION -->

<div id="registrationScreen" class="login-container hidden">

<div class="login-box">

<h1>Student Registration</h1>

<input id="regName" placeholder="Full Name">

<input id="regID" placeholder="Student ID">

<select id="regGrade">
<option value="">Select Grade Level</option>
<option>Grade 7</option>
<option>Grade 8</option>
<option>Grade 9</option>
<option>Grade 10</option>
<option>Grade 11</option>
<option>Grade 12</option>
</select>

<input id="regSection" placeholder="Section">

<input id="regPassword" type="password" placeholder="Create Password">

<button style="width:100%" onclick="registerStudent()">CREATE ACCOUNT</button>

<br><br>

<button class="gray" onclick="backLogin()">BACK TO LOGIN</button>

</div>
</div>


<!-- MAIN APPLICATION -->

<div id="application" class="layout hidden">

<div class="sidebar">

<h2>📚 SMART ATTENDANCE</h2>

<div id="adminMenu">

<button onclick="showPage('dashboard',this)">📊 Dashboard</button>
<button onclick="showPage('qrAttendance',this)">📷 QR Attendance</button>
<button onclick="showPage('liveAttendance',this)">🔴 Live Attendance</button>
<button onclick="showPage('students',this)">👨‍🎓 Students</button>
<button onclick="showPage('classes',this)">🏫 Classes & Sections</button>
<button onclick="showPage('records',this)">📋 Daily Records</button>
<button onclick="showPage('monthly',this)">📅 Monthly Records</button>
<button onclick="showPage('reports',this)">📈 Attendance Reports</button>
<button onclick="showPage('lateStudents',this)">⏰ Late Students</button>
<button onclick="showPage('absentStudents',this)">❌ Absent Students</button>
<button onclick="showPage('notifications',this)">🔔 Notifications</button>
<button onclick="showPage('logs',this)">📝 Activity Logs</button>
<button onclick="showPage('settings',this)">⚙️ Settings</button>
<button onclick="showPage('backup',this)">💾 Backup & Restore</button>

</div>

<div id="studentMenu" class="hidden">

<button onclick="showPage('studentHome',this)">🏠 My Dashboard</button>
<button onclick="showPage('myQR',this)">📱 My QR Code</button>
<button onclick="showPage('scanQR',this)">📷 Scan Attendance</button>
<button onclick="showPage('myRecords',this)">📋 My Attendance</button>
<button onclick="showPage('myNotifications',this)">🔔 Notifications</button>
<button onclick="showPage('myProfile',this)">👤 My Profile</button>

</div>

<br>

<button class="danger" onclick="logout()">LOGOUT</button>

</div>


<div class="main">

<div class="topbar">

<h3 id="pageTitle">Dashboard</h3>

<div>
<span id="currentUser"></span>
<span id="clock"></span>
</div>

</div>

<div class="content">


<!-- ADMIN DASHBOARD -->

<section id="dashboard" class="page">

<h2>Administrator Dashboard</h2>

<div class="cards">

<div class="card">
<h3>Total Registered Students</h3>
<h1 id="totalStudents">0</h1>
</div>

<div class="card">
<h3>Total Present Today</h3>
<h1 id="totalPresent">0</h1>
</div>

<div class="card">
<h3>Total Late Today</h3>
<h1 id="totalLate">0</h1>
</div>

<div class="card">
<h3>Total Absent Today</h3>
<h1 id="totalAbsent">0</h1>
</div>

<div class="card">
<h3>Excused Absences</h3>
<h1 id="totalExcused">0</h1>
</div>

<div class="card">
<h3>Attendance Percentage</h3>
<h1 id="attendancePercent">0%</h1>
</div>

<div class="card">
<h3>Active Students</h3>
<h1 id="activeStudents">0</h1>
</div>

</div>

<div class="panel">

<h3>Attendance Statistics</h3>

<canvas id="attendanceChart"></canvas>

</div>

<div class="panel">

<h3>Recent Attendance Activities</h3>

<table>
<thead>
<tr>
<th>Student</th>
<th>ID</th>
<th>Date</th>
<th>Time</th>
<th>Status</th>
</tr>
</thead>

<tbody id="recentActivities"></tbody>

</table>

</div>

</section>


<!-- QR ATTENDANCE -->

<section id="qrAttendance" class="page hidden">

<h2>QR Attendance Management</h2>

<div class="panel">

<h3>Generate Daily Attendance QR</h3>

<p>Display this QR code so students can scan it to submit their attendance.</p>

<br>

<button onclick="generateAttendanceQR()">GENERATE ATTENDANCE QR</button>

<div id="qrBox"></div>

<p id="qrExpiry"></p>

</div>

</section>


<!-- LIVE ATTENDANCE -->

<section id="liveAttendance" class="page hidden">

<h2>Live Attendance</h2>

<div class="panel">

<table>

<thead>
<tr>
<th>Name</th>
<th>Student ID</th>
<th>Grade</th>
<th>Section</th>
<th>Time</th>
<th>Status</th>
</tr>
</thead>

<tbody id="liveTable"></tbody>

</table>

</div>

</section>


<!-- STUDENT MANAGEMENT -->

<section id="students" class="page hidden">

<h2>Student Management</h2>

<div class="panel">

<h3>Add New Student</h3>

<div class="form-grid">

<input id="newName" placeholder="Student Name">
<input id="newID" placeholder="Student ID">
<input id="newGrade" placeholder="Grade Level">
<input id="newSection" placeholder="Section">
<input id="newPassword" placeholder="Password">

</div>

<button onclick="addStudent()">ADD STUDENT</button>

<button class="success" onclick="exportStudents()">EXPORT STUDENTS CSV</button>

</div>

<div class="panel">

<input id="studentSearch" placeholder="Search student by name or ID..."
oninput="renderStudents()">

<button onclick="document.getElementById('excelImport').click()">IMPORT CSV</button>

<input type="file" id="excelImport" accept=".csv" class="hidden" onchange="importStudents(event)">

<table>

<thead>
<tr>
<th>Name</th>
<th>ID</th>
<th>Grade</th>
<th>Section</th>
<th>Account</th>
<th>Actions</th>
</tr>
</thead>

<tbody id="studentTable"></tbody>

</table>

</div>

</section>


<!-- CLASSES -->

<section id="classes" class="page hidden">

<h2>Class and Section Management</h2>

<div class="panel">

<h3>Create Class / Section</h3>

<input id="classGrade" placeholder="Grade Level">

<input id="classSection" placeholder="Section Name">

<input id="classAdviser" placeholder="Class Adviser">

<input id="classSchedule" placeholder="Schedule">

<button onclick="addClass()">CREATE SECTION</button>

</div>

<div class="panel">

<table>

<thead>
<tr>
<th>Grade Level</th>
<th>Section</th>
<th>Students</th>
<th>Adviser</th>
<th>Schedule</th>
<th>Action</th>
</tr>
</thead>

<tbody id="classTable"></tbody>

</table>

</div>

</section>


<!-- DAILY RECORDS -->

<section id="records" class="page hidden">

<h2>Daily Attendance Records</h2>

<div class="panel">

<input type="date" id="recordDate" onchange="renderRecords()">

<input id="recordSearch" placeholder="Search student..." oninput="renderRecords()">

<button onclick="exportAttendance()">EXPORT ATTENDANCE</button>

<table>

<thead>
<tr>
<th>Student</th>
<th>ID</th>
<th>Date</th>
<th>Time</th>
<th>Status</th>
<th>Remarks</th>
<th>Actions</th>
</tr>
</thead>

<tbody id="recordTable"></tbody>

</table>

</div>

</section>


<!-- MONTHLY RECORDS -->

<section id="monthly" class="page hidden">

<h2>Monthly Attendance Records</h2>

<div class="panel">

<input type="month" id="monthFilter" onchange="renderMonthly()">

<table>

<thead>
<tr>
<th>Student</th>
<th>ID</th>
<th>Present</th>
<th>Late</th>
<th>Absent</th>
<th>Excused</th>
</tr>
</thead>

<tbody id="monthlyTable"></tbody>

</table>

</div>

</section>


<!-- REPORTS -->

<section id="reports" class="page hidden">

<h2>Attendance Reports</h2>

<div class="panel">

<select id="reportType" onchange="renderReports()">

<option value="daily">Daily Report</option>
<option value="weekly">Weekly Report</option>
<option value="monthly">Monthly Report</option>
<option value="yearly">Yearly Report</option>
<option value="individual">Individual Report</option>
<option value="section">Section Report</option>
<option value="grade">Grade Report</option>
<option value="late">Late Report</option>
<option value="absent">Absence Report</option>

</select>

<input type="date" id="reportDate" onchange="renderReports()">

<input id="reportSearch" placeholder="Student ID for individual report">

<button onclick="renderReports()">GENERATE REPORT</button>

<button class="success" onclick="exportAttendance()">DOWNLOAD CSV</button>

<div id="reportResults"></div>

</div>

</section>


<!-- LATE STUDENTS -->

<section id="lateStudents" class="page hidden">

<h2>Late Students</h2>

<div class="panel">

<table>
<thead>
<tr>
<th>Name</th>
<th>ID</th>
<th>Date</th>
<th>Time</th>
</tr>
</thead>

<tbody id="lateTable"></tbody>

</table>

</div>

</section>


<!-- ABSENT STUDENTS -->

<section id="absentStudents" class="page hidden">

<h2>Absent Students</h2>

<div class="panel">

<table>
<thead>
<tr>
<th>Name</th>
<th>ID</th>
<th>Date</th>
<th>Status</th>
</tr>
</thead>

<tbody id="absentTable"></tbody>

</table>

</div>

</section>


<!-- NOTIFICATIONS -->

<section id="notifications" class="page hidden">

<h2>Administrator Notifications</h2>

<div class="panel" id="adminNotifications"></div>

</section>


<!-- LOGS -->

<section id="logs" class="page hidden">

<h2>Activity Logs</h2>

<div class="panel">

<table>
<thead>
<tr>
<th>Date</th>
<th>Time</th>
<th>Activity</th>
<th>User</th>
</tr>
</thead>

<tbody id="logsTable"></tbody>

</table>

</div>

</section>


<!-- SCHOOL YEAR / PROMOTION -->
<section id="schoolYear" class="page hidden">
  <h2>School Year and Student Promotion</h2>
  <div class="panel">
    <h3>Current School Year</h3>
    <p>Current active school year: <strong id="activeSchoolYearLabel"></strong></p>
    <label>New School Year (example: 2027-2028)</label>
    <input id="newSchoolYear" placeholder="2027-2028">
    <p>Starting a new school year preserves previous attendance records, promotes eligible students, and starts a fresh attendance period.</p>
    <button class="success" onclick="startNewSchoolYear()">START NEW SCHOOL YEAR</button>
  </div>
  <div class="panel">
    <h3>New Student Enrollment</h3>
    <p>After starting the new school year, add incoming students in Student Management and assign their new grade and section.</p>
    <button onclick="showPage('students')">GO TO STUDENT MANAGEMENT</button>
  </div>
  <div class="panel">
    <h3>School-Year Attendance History</h3>
    <select id="schoolYearHistoryFilter" onchange="renderSchoolYearHistory()"></select>
    <div id="schoolYearHistory"></div>
  </div>
</section>

<!-- SETTINGS -->

<section id="settings" class="page hidden">

<h2>Attendance Settings</h2>

<div class="panel">

<label>Present Cutoff Time</label>
<input type="time" id="presentTime" value="07:30">

<label>Late Cutoff Time</label>
<input type="time" id="lateTime" value="07:45">

<label>School Name</label>
<input id="schoolName" placeholder="School Name">

<button onclick="saveSettings()">SAVE SETTINGS</button>

</div>

</section>


<!-- BACKUP -->

<section id="backup" class="page hidden">

<h2>Backup and Restore</h2>

<div class="panel">

<p>Download your locally stored demonstration data.</p>

<br>

<button onclick="backupData()">DOWNLOAD BACKUP</button>

<br><br>

<input type="file" id="restoreFile" accept=".json">

<button onclick="restoreData()">RESTORE BACKUP</button>

</div>

</section>


<!-- STUDENT HOME -->

<section id="studentHome" class="page hidden">

<h2>Welcome, <span id="studentWelcome"></span></h2>

<div class="cards">

<div class="card">
<h3>Attendance Percentage</h3>
<h1 id="myPercentage">0%</h1>
</div>

<div class="card">
<h3>Present</h3>
<h1 id="myPresent">0</h1>
</div>

<div class="card">
<h3>Late</h3>
<h1 id="myLate">0</h1>
</div>

<div class="card">
<h3>Absent</h3>
<h1 id="myAbsent">0</h1>
</div>

</div>

<div class="panel">

<h3>Today's Attendance</h3>

<div id="myToday"></div>

</div>

</section>


<!-- STUDENT QR -->

<section id="myQR" class="page hidden">

<h2>My Personal QR Code</h2>

<div class="panel student-profile">

<h3 id="myQRName"></h3>

<p id="myQRID"></p>

<div id="studentQR"></div>

<p>Show this QR code to authorized school personnel for identification.</p>

</div>

</section>


<!-- STUDENT SCANNER -->

<section id="scanQR" class="page hidden">

<h2>Scan Attendance QR Code</h2>

<div class="panel">

<p>Allow camera access and scan the QR code displayed by your school administrator.</p>

<br>

<button onclick="startScanner()">START CAMERA SCANNER</button>

<button class="danger" onclick="stopScanner()">STOP SCANNER</button>

<div id="reader" style="width:100%;max-width:400px;margin:20px auto"></div>

<div id="scanMessage"></div>

</div>

</section>


<!-- STUDENT RECORDS -->

<section id="myRecords" class="page hidden">

<h2>My Attendance History</h2>

<div class="panel">

<table>
<thead>
<tr>
<th>Date</th>
<th>Time</th>
<th>Status</th>
<th>Remarks</th>
</tr>
</thead>

<tbody id="myRecordsTable"></tbody>

</table>

</div>

</section>


<!-- STUDENT NOTIFICATIONS -->

<section id="myNotifications" class="page hidden">

<h2>My Notifications</h2>

<div class="panel" id="studentNotifications"></div>

</section>


<!-- PROFILE -->

<section id="myProfile" class="page hidden">

<h2>My Profile</h2>

<div class="panel">

<p id="profileInfo"></p>

<input id="profileName" placeholder="Update Name">

<button onclick="updateProfile()">UPDATE PROFILE</button>

</div>

</section>


</div>
</div>
</div>



<script>
// SMART ATTENDANCE - repaired demo implementation
"use strict";

const ADMIN_USERNAME = "admin";
const ADMIN_PASSWORD = "admin123";

let students = readStore("students", []);
let attendance = readStore("attendance", []);
let classes = readStore("classes", []);
let logs = readStore("logs", []);
let notifications = readStore("notifications", []);
let settings = readStore("settings", { presentTime: "07:30", lateTime: "07:45", schoolName: "" });
let activeSchoolYear = readStore("activeSchoolYear", `${new Date().getFullYear()}-${new Date().getFullYear()+1}`);
let currentUser = null;
let scanner = null;
let activeToken = "";
let pendingAttendanceToken = "";
let chart = null;

function readStore(key, fallback) {
  try {
    const value = JSON.parse(localStorage.getItem(key));
    return value == null ? fallback : value;
  } catch (e) {
    console.warn("Resetting invalid local data:", key, e);
    return fallback;
  }
}
function saveData() {
  try {
    localStorage.setItem("students", JSON.stringify(students));
    localStorage.setItem("attendance", JSON.stringify(attendance));
    localStorage.setItem("classes", JSON.stringify(classes));
    localStorage.setItem("logs", JSON.stringify(logs));
    localStorage.setItem("notifications", JSON.stringify(notifications));
    localStorage.setItem("settings", JSON.stringify(settings));
    localStorage.setItem("activeSchoolYear", JSON.stringify(activeSchoolYear));
  } catch (e) {
    alert("Unable to save data in this browser. Check storage space or browser settings.");
    console.error(e);
  }
}
function today() {
  const d = new Date();
  return [d.getFullYear(), String(d.getMonth()+1).padStart(2,"0"), String(d.getDate()).padStart(2,"0")].join("-");
}
function timeNow() {
  return new Date().toLocaleTimeString([], {hour:"2-digit", minute:"2-digit", second:"2-digit"});
}
function escapeHTML(value) {
  return String(value ?? "").replace(/[&<>"']/g, c => ({
    "&":"&amp;", "<":"&lt;", ">":"&gt;", '"':"&quot;", "'":"&#39;"
  }[c]));
}
function getStudent(id) { return students.find(s => String(s.id) === String(id)); }
function logActivity(activity) {
  logs.unshift({date:today(), time:timeNow(), activity, user:currentUser?.name || "System"});
  logs = logs.slice(0, 500);
  saveData();
}
function notify(message, target="admin", studentID="") {
  notifications.unshift({message, target, studentID, date:today(), time:timeNow()});
  notifications = notifications.slice(0, 300);
  saveData();
}
function byId(id) { return document.getElementById(id); }
function val(id) { return byId(id)?.value?.trim() || ""; }

function showRegistration() {
  byId("loginScreen").classList.add("hidden");
  byId("registrationScreen").classList.remove("hidden");
}
function backLogin() {
  byId("registrationScreen").classList.add("hidden");
  byId("loginScreen").classList.remove("hidden");
}
function registerStudent() {
  const name=val("regName"), id=val("regID"), grade=val("regGrade"),
        section=val("regSection"), password=byId("regPassword").value;
  if (!name || !id || !grade || !section || !password) return alert("Please complete all fields.");
  if (getStudent(id)) return alert("Student ID already exists.");
  students.push({name,id,grade,section,password,active:true,created:today(),schoolYear:activeSchoolYear});
  saveData();
  notify("New student registered: "+name);
  logActivity("New student registered: "+name);
  ["regName","regID","regGrade","regSection","regPassword"].forEach(id=>{ if(byId(id)) byId(id).value=""; });
  alert("Registration successful! You can now log in.");
  backLogin();
}

function login() {
  const role=byId("loginRole").value;
  const id=val("loginID");
  const password=byId("loginPassword").value;
  if (!id || !password) return alert("Enter your username/Student ID and password.");
  if (role === "admin") {
    if (id !== ADMIN_USERNAME || password !== ADMIN_PASSWORD) {
      return alert("Invalid administrator username or password. Demo login: admin / admin123");
    }
    currentUser={role:"admin",id:ADMIN_USERNAME,name:"Administrator"};
  } else {
    const student=getStudent(id);
    if (!student || student.password !== password) return alert("Invalid Student ID or password.");
    if (student.active === false) return alert("Your account is inactive. Please contact the administrator.");
    currentUser={role:"student",id:student.id,name:student.name};
  }
  byId("loginScreen").classList.add("hidden");
  byId("registrationScreen").classList.add("hidden");
  byId("application").classList.remove("hidden");
  byId("currentUser").textContent=currentUser.name;
  byId("adminMenu").classList.toggle("hidden", currentUser.role !== "admin");
  byId("studentMenu").classList.toggle("hidden", currentUser.role !== "student");
  logActivity("Logged in");
  showPage(currentUser.role === "admin" ? "dashboard" : "studentHome");
  byId("loginPassword").value="";
  if (currentUser.role === "student" && pendingAttendanceToken) {
    const tokenToUse=pendingAttendanceToken;
    pendingAttendanceToken="";
    try {
      const result=markAttendance(currentUser.id,tokenToUse);
      const cleanURL=new URL(window.location.href);
      cleanURL.searchParams.delete("attendanceToken");
      window.history.replaceState({},document.title,cleanURL.toString());
      showPage("studentHome");
      alert("Welcome, "+currentUser.name+"! Your attendance was recorded automatically: "+result+".");
    } catch(err) {
      alert("You are logged in, but attendance was not recorded: "+err.message);
    }
  } else {
    refreshAll();
  }
}
function logout() {
  stopScanner();
  if (chart) { chart.destroy(); chart=null; }
  currentUser=null;
  pendingAttendanceToken="";
  byId("loginRole").disabled=false;
  byId("qrLoginNotice")?.classList.add("hidden");
  byId("application").classList.add("hidden");
  byId("loginScreen").classList.remove("hidden");
  byId("loginID").value="";
  byId("loginPassword").value="";
}
function showPage(pageId, button) {
  if (!currentUser) return;
  const adminPages=["dashboard","qrAttendance","liveAttendance","students","classes","records","monthly","reports","lateStudents","absentStudents","notifications","logs","schoolYear","settings","backup"];
  const studentPages=["studentHome","myQR","scanQR","myRecords","myNotifications","myProfile"];
  if (currentUser.role==="admin" && !adminPages.includes(pageId)) return;
  if (currentUser.role==="student" && !studentPages.includes(pageId)) return;
  document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
  const page=byId(pageId);
  if (page) page.classList.remove("hidden");
  document.querySelectorAll(".sidebar button").forEach(b=>b.classList.remove("active"));
  if (button) button.classList.add("active");
  const heading=page?.querySelector("h2")?.textContent || "Dashboard";
  byId("pageTitle").textContent=heading;
  if (pageId==="dashboard") renderDashboard();
  if (pageId==="students") renderStudents();
  if (pageId==="classes") renderClasses();
  if (pageId==="records") renderRecords();
  if (pageId==="monthly") renderMonthly();
  if (pageId==="reports") renderReports();
  if (pageId==="liveAttendance") renderLive();
  if (pageId==="lateStudents") renderLate();
  if (pageId==="absentStudents") renderAbsent();
  if (pageId==="notifications") renderNotifications();
  if (pageId==="logs") renderLogs();
  if (pageId==="schoolYear") renderSchoolYear();
  if (pageId==="studentHome") renderStudentHome();
  if (pageId==="myQR") renderMyQR();
  if (pageId==="myRecords") renderMyRecords();
  if (pageId==="myNotifications") renderStudentNotifications();
  if (pageId==="myProfile") renderProfile();
  if (pageId!=="scanQR") stopScanner();
}

function statusForNow() {
  const now=new Date();
  const hm=String(now.getHours()).padStart(2,"0")+":"+String(now.getMinutes()).padStart(2,"0");
  const present=settings.presentTime || "07:30";
  const late=settings.lateTime || "07:45";
  return hm <= present ? "Present" : (hm <= late ? "Late" : "Late");
}
function isAttendanceTokenValid(token) {
  if (!token || !String(token).startsWith("ATTENDANCE:")) return false;
  const parts=String(token).split(":");
  const createdAt=Number(parts[3]);
  return parts.length===4 && Number.isFinite(createdAt) && Date.now()-createdAt <= 12*60*60*1000 && Date.now()>=createdAt;
}
function markAttendance(studentID, token) {
  const student=getStudent(studentID);
  if (!student) throw new Error("Student account was not found.");
  if (!token || token !== activeToken || !isAttendanceTokenValid(token)) throw new Error("This attendance QR is invalid or expired. Ask your teacher to generate a new one.");
  if (attendance.some(a=>a.studentID===studentID && a.date===today() && (a.schoolYear||activeSchoolYear)===activeSchoolYear)) throw new Error("Your attendance has already been recorded today.");
  const status=statusForNow();
  attendance.unshift({student:student.name,studentID:student.id,grade:student.grade,section:student.section,date:today(),time:timeNow(),status,remarks:"Attendance QR login",schoolYear:activeSchoolYear});
  saveData();
  notify("Attendance recorded for "+student.name, "student", student.id);
  logActivity("Attendance recorded: "+student.name+" ("+status+")");
  refreshAll();
  return status;
}
function generateAttendanceQR() {
  if (currentUser?.role!=="admin") return;
  const pageURL=new URL(window.location.href);
  if (pageURL.protocol!=="https:" && pageURL.protocol!=="http:") {
    byId("qrBox").innerHTML="";
    byId("qrExpiry").textContent="This app is opened as a local file. Upload it to an HTTPS website first (for example, GitHub Pages or Netlify), then generate the QR code again.";
    return alert("QR cannot open on student phones while this app is running as a local file. Host the website online using HTTPS first.");
  }
  if (["localhost","127.0.0.1","0.0.0.0"].includes(pageURL.hostname)) {
    byId("qrBox").innerHTML="";
    byId("qrExpiry").textContent="This address is only available on this computer. Open the hosted HTTPS website to generate a QR code for students.";
    return alert("You are using a localhost address. Open your public HTTPS website first, then generate the QR code.");
  }
  if (typeof QRCode==="undefined") return alert("QR library is still loading. Please wait a moment and try again.");
  activeToken="ATTENDANCE:"+today()+":"+Math.random().toString(36).slice(2,12)+":"+Date.now();
  const portalURL=new URL(pageURL.origin+pageURL.pathname);
  portalURL.searchParams.set("attendanceToken",activeToken);
  byId("qrBox").innerHTML="";
  new QRCode(byId("qrBox"), {text:portalURL.toString(),width:220,height:220,correctLevel:QRCode.CorrectLevel.M});
  byId("qrExpiry").textContent="Scan with the phone camera. The link opens the Student Login screen automatically. After login, attendance is submitted. Valid for 12 hours. Use the same hosted website on every device.";
  logActivity("Generated daily attendance QR");
}
function renderMyQR() {
  const s=getStudent(currentUser.id);
  if (!s) return;
  byId("myQRName").textContent=s.name;
  byId("myQRID").textContent="Student ID: "+s.id;
  byId("studentQR").innerHTML="";
  if (typeof QRCode!=="undefined") new QRCode(byId("studentQR"), {text:"STUDENT:"+s.id,width:200,height:200,correctLevel:QRCode.CorrectLevel.H});
  else byId("studentQR").textContent="QR library unavailable. Check internet connection.";
}
async function startScanner() {
  if (currentUser?.role!=="student") return alert("Only student accounts can scan attendance.");
  if (!activeToken) return alert("Ask the administrator to generate the attendance QR first.");
  if (typeof Html5Qrcode==="undefined") return alert("QR scanner library did not load. Check your internet connection.");
  if (scanner) { try { await scanner.stop(); } catch(e) {} scanner=null; }
  byId("scanMessage").textContent="Starting camera…";
  scanner=new Html5Qrcode("reader");
  try {
    const cameras=await Html5Qrcode.getCameras();
    if (!cameras || !cameras.length) throw new Error("No camera was found on this device.");
    const camera=cameras.find(c=>/back|environment|rear/i.test(c.label)) || cameras[0];
    await scanner.start(camera.id,{fps:10,qrbox:{width:230,height:230}}, decodedText=>{
      try {
        const status=markAttendance(currentUser.id,decodedText);
        byId("scanMessage").textContent="Attendance recorded successfully. Status: "+status;
        stopScanner();
      } catch(err) {
        byId("scanMessage").textContent=err.message;
      }
    }, ()=>{});
    byId("scanMessage").textContent="Camera is active. Point it at the attendance QR code.";
  } catch(err) {
    byId("scanMessage").textContent="Scanner error: "+(err.message || err);
    scanner=null;
  }
}
async function stopScanner() {
  if (scanner) {
    try { if (scanner.isScanning) await scanner.stop(); } catch(e) { console.warn(e); }
    try { await scanner.clear(); } catch(e) {}
    scanner=null;
  }
}
function addStudent() {
  const name=val("newName"), id=val("newID"), grade=val("newGrade"), section=val("newSection"), password=val("newPassword");
  if (!name||!id||!grade||!section||!password) return alert("Complete all student fields.");
  if (getStudent(id)) return alert("Student ID already exists.");
  students.push({name,id,grade,section,password,active:true,created:today(),schoolYear:activeSchoolYear});
  saveData(); logActivity("Administrator added student: "+name);
  ["newName","newID","newGrade","newSection","newPassword"].forEach(x=>byId(x).value="");
  renderStudents(); renderDashboard();
}
function renderStudents() {
  const q=val("studentSearch").toLowerCase();
  byId("studentTable").innerHTML=students.filter(s=>(s.name+" "+s.id).toLowerCase().includes(q)).map(s=>`
    <tr><td>${escapeHTML(s.name)}</td><td>${escapeHTML(s.id)}</td><td>${escapeHTML(s.grade)}</td><td>${escapeHTML(s.section)}</td>
    <td>${s.active===false?"Inactive":"Active"}</td><td class="actions">
    <button class="warning" onclick="toggleStudent('${encodeURIComponent(s.id)}')">${s.active===false?"Activate":"Deactivate"}</button>
    <button class="danger" onclick="deleteStudent('${encodeURIComponent(s.id)}')">Delete</button></td></tr>`).join("") || `<tr><td colspan="6">No students found.</td></tr>`;
}
function toggleStudent(encoded) {
  const s=getStudent(decodeURIComponent(encoded)); if(!s)return;
  s.active=s.active===false; saveData(); renderStudents();
}
function deleteStudent(encoded) {
  const id=decodeURIComponent(encoded);
  if(!confirm("Delete this student account? Existing attendance records will remain."))return;
  students=students.filter(s=>s.id!==id); saveData(); renderStudents(); renderDashboard();
}
function addClass() {
  const grade=val("classGrade"), section=val("classSection"), adviser=val("classAdviser"), schedule=val("classSchedule");
  if(!grade||!section)return alert("Enter grade level and section.");
  if(classes.some(c=>c.grade===grade&&c.section===section))return alert("This class already exists.");
  classes.push({grade,section,adviser,schedule}); saveData(); logActivity("Created class "+grade+" "+section);
  ["classGrade","classSection","classAdviser","classSchedule"].forEach(x=>byId(x).value=""); renderClasses();
}
function renderClasses() {
  byId("classTable").innerHTML=classes.map((c,i)=>`<tr><td>${escapeHTML(c.grade)}</td><td>${escapeHTML(c.section)}</td><td>${students.filter(s=>s.grade===c.grade&&s.section===c.section).length}</td><td>${escapeHTML(c.adviser)}</td><td>${escapeHTML(c.schedule)}</td><td><button class="danger" onclick="deleteClass(${i})">Delete</button></td></tr>`).join("") || `<tr><td colspan="6">No classes created.</td></tr>`;
}
function deleteClass(i){if(confirm("Delete this class?")){classes.splice(i,1);saveData();renderClasses();}}
function renderDashboard() {
  const todays=attendance.filter(a=>a.date===today());
  const count=s=>todays.filter(a=>a.status===s).length;
  byId("totalStudents").textContent=students.length;
  byId("totalPresent").textContent=count("Present");
  byId("totalLate").textContent=count("Late");
  byId("totalAbsent").textContent=Math.max(0,students.filter(s=>s.active!==false).length-todays.length);
  byId("totalExcused").textContent=count("Excused");
  byId("activeStudents").textContent=students.filter(s=>s.active!==false).length;
  byId("attendancePercent").textContent=(students.length?Math.round(todays.length/students.length*100):0)+"%";
  byId("recentActivities").innerHTML=attendance.slice(0,8).map(a=>`<tr><td>${escapeHTML(a.student)}</td><td>${escapeHTML(a.studentID)}</td><td>${escapeHTML(a.date)}</td><td>${escapeHTML(a.time)}</td><td><span class="badge ${escapeHTML(a.status)}">${escapeHTML(a.status)}</span></td></tr>`).join("") || `<tr><td colspan="5">No attendance yet.</td></tr>`;
  if(typeof Chart!=="undefined" && byId("attendanceChart")) {
    if(chart)chart.destroy();
    chart=new Chart(byId("attendanceChart"),{type:"bar",data:{labels:["Present","Late","Absent","Excused"],datasets:[{label:"Today",data:[count("Present"),count("Late"),Math.max(0,students.length-todays.length),count("Excused")]}]},options:{responsive:true,scales:{y:{beginAtZero:true,precision:0}}}});
  }
}
function renderLive() {
  byId("liveTable").innerHTML=attendance.filter(a=>a.date===today()).map(a=>`<tr><td>${escapeHTML(a.student)}</td><td>${escapeHTML(a.studentID)}</td><td>${escapeHTML(a.grade)}</td><td>${escapeHTML(a.section)}</td><td>${escapeHTML(a.time)}</td><td>${escapeHTML(a.status)}</td></tr>`).join("") || `<tr><td colspan="6">No attendance scanned today.</td></tr>`;
}
function renderRecords() {
  const date=byId("recordDate").value||today(), q=val("recordSearch").toLowerCase();
  byId("recordTable").innerHTML=attendance.filter(a=>a.date===date&&(a.student+" "+a.studentID).toLowerCase().includes(q)).map((a,i)=>`<tr><td>${escapeHTML(a.student)}</td><td>${escapeHTML(a.studentID)}</td><td>${escapeHTML(a.date)}</td><td>${escapeHTML(a.time)}</td><td>${escapeHTML(a.status)}</td><td>${escapeHTML(a.remarks)}</td><td><button class="warning" onclick="editAttendance('${encodeURIComponent(a.studentID)}','${a.date}')">Change status</button></td></tr>`).join("") || `<tr><td colspan="7">No records found.</td></tr>`;
}
function editAttendance(encoded,date) {
  const id=decodeURIComponent(encoded), a=attendance.find(x=>x.studentID===id&&x.date===date); if(!a)return;
  const status=prompt("Enter status: Present, Late, Absent, or Excused",a.status);
  if(!status)return;
  const allowed=["Present","Late","Absent","Excused"];
  if(!allowed.includes(status))return alert("Invalid status.");
  a.status=status;saveData();renderRecords();refreshAll();
}
function renderMonthly() {
  const month=byId("monthFilter").value||today().slice(0,7);
  const gradeQ=val("monthlyGradeFilter").toLowerCase(), sectionQ=val("monthlySectionFilter").toLowerCase();
  const list=students.filter(s=>(!gradeQ||String(s.grade).toLowerCase().includes(gradeQ))&&(!sectionQ||String(s.section).toLowerCase().includes(sectionQ)));
  byId("monthlyTable").innerHTML=list.map(s=>{
    const rows=attendance.filter(a=>a.studentID===s.id&&a.date.startsWith(month)&&(a.schoolYear||activeSchoolYear)===activeSchoolYear);
    return `<tr><td>${escapeHTML(s.name)}</td><td>${escapeHTML(s.id)}</td>${["Present","Late","Absent","Excused"].map(st=>`<td>${rows.filter(a=>a.status===st).length}</td>`).join("")}</tr>`;
  }).join("") || `<tr><td colspan="6">No students match this filter.</td></tr>`;
}
function renderReports() {
  const type=byId("reportType").value, date=byId("reportDate").value||today(), q=val("reportSearch");
  let rows=attendance.slice();
  if(type==="daily")rows=rows.filter(a=>a.date===date);
  if(type==="monthly")rows=rows.filter(a=>a.date.startsWith(date.slice(0,7)));
  if(type==="yearly")rows=rows.filter(a=>a.date.startsWith(date.slice(0,4)));
  if(type==="weekly"){const d=new Date(date+"T00:00:00");const start=new Date(d);start.setDate(d.getDate()-d.getDay());const end=new Date(start);end.setDate(start.getDate()+7);rows=rows.filter(a=>{const x=new Date(a.date+"T00:00:00");return x>=start&&x<end;});}
  if(type==="individual")rows=rows.filter(a=>a.studentID===q);
  if(type==="late")rows=rows.filter(a=>a.status==="Late");
  if(type==="absent")rows=rows.filter(a=>a.status==="Absent");
  if(type==="section"||type==="grade"){const key=prompt(type==="section"?"Enter section name:":"Enter grade level:");if(key)rows=rows.filter(a=>(type==="section"?a.section:a.grade)===key);}
  byId("reportResults").innerHTML=`<h3>${rows.length} attendance record(s)</h3>`+tableHTML(rows);
}
function tableHTML(rows) {
  if(!rows.length)return "<p>No matching records.</p>";
  return `<div style="overflow-x:auto"><table><thead><tr><th>Name</th><th>ID</th><th>Grade</th><th>Section</th><th>Date</th><th>Time</th><th>Status</th></tr></thead><tbody>${rows.map(a=>`<tr><td>${escapeHTML(a.student)}</td><td>${escapeHTML(a.studentID)}</td><td>${escapeHTML(a.grade)}</td><td>${escapeHTML(a.section)}</td><td>${escapeHTML(a.date)}</td><td>${escapeHTML(a.time)}</td><td>${escapeHTML(a.status)}</td></tr>`).join("")}</tbody></table></div>`;
}
function renderLate(){const rows=attendance.filter(a=>a.status==="Late");byId("lateTable").innerHTML=rows.map(a=>`<tr><td>${escapeHTML(a.student)}</td><td>${escapeHTML(a.studentID)}</td><td>${escapeHTML(a.date)}</td><td>${escapeHTML(a.time)}</td></tr>`).join("")||`<tr><td colspan="4">No late records.</td></tr>`;}
function renderAbsent(){const rows=attendance.filter(a=>a.status==="Absent");byId("absentTable").innerHTML=rows.map(a=>`<tr><td>${escapeHTML(a.student)}</td><td>${escapeHTML(a.studentID)}</td><td>${escapeHTML(a.date)}</td><td>Absent</td></tr>`).join("")||`<tr><td colspan="4">No absent records. Students not scanned are counted as absent on the dashboard only.</td></tr>`;}
function renderNotifications(){byId("adminNotifications").innerHTML=notifications.filter(n=>n.target==="admin").map(n=>`<div class="notification">${escapeHTML(n.message)}<br><small>${escapeHTML(n.date)} ${escapeHTML(n.time)}</small></div>`).join("")||"No notifications.";}
function renderStudentNotifications(){byId("studentNotifications").innerHTML=notifications.filter(n=>n.target==="student"&&n.studentID===currentUser.id).map(n=>`<div class="notification">${escapeHTML(n.message)}<br><small>${escapeHTML(n.date)} ${escapeHTML(n.time)}</small></div>`).join("")||"No notifications.";}
function renderLogs(){byId("logsTable").innerHTML=logs.map(l=>`<tr><td>${escapeHTML(l.date)}</td><td>${escapeHTML(l.time)}</td><td>${escapeHTML(l.activity)}</td><td>${escapeHTML(l.user)}</td></tr>`).join("")||`<tr><td colspan="4">No activity logs.</td></tr>`;}
function renderStudentHome(){
  const rows=attendance.filter(a=>a.studentID===currentUser.id), present=rows.filter(a=>a.status==="Present").length, late=rows.filter(a=>a.status==="Late").length, absent=rows.filter(a=>a.status==="Absent").length;
  byId("studentWelcome").textContent=currentUser.name;byId("myPresent").textContent=present;byId("myLate").textContent=late;byId("myAbsent").textContent=absent;
  byId("myPercentage").textContent=(rows.length?Math.round((present+late)/rows.length*100):0)+"%";
  const todayRow=rows.find(a=>a.date===today());byId("myToday").textContent=todayRow?`Recorded at ${todayRow.time} — ${todayRow.status}`:"No attendance recorded today.";
}
function renderMyRecords(){const rows=attendance.filter(a=>a.studentID===currentUser.id);byId("myRecordsTable").innerHTML=rows.map(a=>`<tr><td>${escapeHTML(a.date)}</td><td>${escapeHTML(a.time)}</td><td>${escapeHTML(a.status)}</td><td>${escapeHTML(a.remarks)}</td></tr>`).join("")||`<tr><td colspan="4">No attendance history.</td></tr>`;}
function renderProfile(){const s=getStudent(currentUser.id);if(!s)return;byId("profileInfo").textContent=`Student ID: ${s.id} | Grade: ${s.grade} | Section: ${s.section}`;byId("profileName").value=s.name;}
function updateProfile(){const s=getStudent(currentUser.id),name=val("profileName");if(!s||!name)return alert("Enter your name.");s.name=name;currentUser.name=name;byId("currentUser").textContent=name;saveData();logActivity("Updated student profile");renderStudentHome();alert("Profile updated.");}
function exportCSV(filename, rows) {
  if(!rows.length)return alert("There is no data to export.");
  const cols=Object.keys(rows[0]);const csv=[cols.join(","),...rows.map(r=>cols.map(k=>'"'+String(r[k]??"").replace(/"/g,'""')+'"').join(","))].join("\r\n");
  const blob=new Blob(["\uFEFF"+csv],{type:"text/csv;charset=utf-8;"}),url=URL.createObjectURL(blob),a=document.createElement("a");
  a.href=url;a.download=filename;a.click();URL.revokeObjectURL(url);
}
function exportStudents(){exportCSV("students.csv",students.map(({password,...s})=>s));}
function exportAttendance(){exportCSV("attendance.csv",attendance);}
function importStudents(event){
  const file=event.target.files?.[0];if(!file)return;
  const reader=new FileReader();reader.onload=()=>{
    try{const lines=String(reader.result).trim().split(/\r?\n/);const headers=lines.shift().split(",").map(x=>x.trim().toLowerCase());let added=0;
      for(const line of lines){const cells=line.match(/("([^"]|"")*"|[^,]*)(,|$)/g)||[];const values=cells.map(x=>x.replace(/,$/,"").replace(/^"|"$/g,"").replace(/""/g,'"'));const obj={};headers.forEach((h,i)=>obj[h]=values[i]||"");
        const id=obj.id||obj.studentid;if(id&&!getStudent(id)){students.push({name:obj.name||"",id,grade:obj.grade||"",section:obj.section||"",password:obj.password||"changeme",active:true,created:today(),schoolYear:activeSchoolYear});added++;}}
      saveData();renderStudents();alert(added+" student(s) imported.");
    }catch(e){alert("Could not read CSV file: "+e.message);}
    event.target.value="";
  };reader.readAsText(file);
}
function backupData(){
  const data={students,attendance,classes,logs,notifications,settings,activeToken,activeSchoolYear};
  const blob=new Blob([JSON.stringify(data,null,2)],{type:"application/json"}),url=URL.createObjectURL(blob),a=document.createElement("a");
  a.href=url;a.download="attendance-backup-"+today()+".json";a.click();URL.revokeObjectURL(url);
}
function restoreData(){
  const file=byId("restoreFile").files?.[0];if(!file)return alert("Choose a backup JSON file first.");
  const reader=new FileReader();reader.onload=()=>{
    try{const d=JSON.parse(reader.result);if(!Array.isArray(d.students)||!Array.isArray(d.attendance))throw new Error("Invalid backup file.");
      students=d.students;attendance=d.attendance;classes=Array.isArray(d.classes)?d.classes:[];logs=Array.isArray(d.logs)?d.logs:[];notifications=Array.isArray(d.notifications)?d.notifications:[];settings=d.settings||settings;activeToken=d.activeToken||"";activeSchoolYear=d.activeSchoolYear||activeSchoolYear;
      saveData();refreshAll();alert("Backup restored successfully.");
    }catch(e){alert("Restore failed: "+e.message);}
  };reader.readAsText(file);
}
function renderSchoolYear() {
  byId("activeSchoolYearLabel").textContent=activeSchoolYear;
  if (!val("newSchoolYear")) {
    const parts=String(activeSchoolYear).split("-");
    const start=Number(parts[0]), end=Number(parts[1]);
    byId("newSchoolYear").value=Number.isFinite(start)&&Number.isFinite(end)?`${start+1}-${end+1}`:"";
  }
  const years=[...new Set([activeSchoolYear,...attendance.map(a=>a.schoolYear).filter(Boolean)])].sort();
  byId("schoolYearHistoryFilter").innerHTML=years.map(y=>`<option value="${escapeHTML(y)}">${escapeHTML(y)}</option>`).join("");
  renderSchoolYearHistory();
}
function renderSchoolYearHistory() {
  const year=val("schoolYearHistoryFilter")||activeSchoolYear;
  const rows=attendance.filter(a=>(a.schoolYear||activeSchoolYear)===year);
  byId("schoolYearHistory").innerHTML=`<p><strong>${rows.length}</strong> attendance record(s) for ${escapeHTML(year)}.</p>`+tableHTML(rows);
}
function startNewSchoolYear() {
  if (currentUser?.role!=="admin") return;
  const next=val("newSchoolYear");
  if (!/^\\d{4}-\\d{4}$/.test(next)) return alert("Enter the school year in this format: 2027-2028.");
  if (next===activeSchoolYear) return alert("That is already the active school year.");
  if (!confirm(`Start ${next}? Previous attendance records will be preserved. Students in Grades 7–11 will be promoted one grade level. Grade 12 students will be marked as graduated/inactive. You can add new students afterward.`)) return;
  students.forEach(s=>{
    const match=String(s.grade).match(/(Grade\\s*)?(\\d+)/i);
    if (!match) return;
    const level=Number(match[2]);
    if (level>=7 && level<12) {
      const prefix=match[1]||"Grade ";
      s.grade=prefix+(level+1);
      s.schoolYear=next;
    } else if (level===12) {
      s.active=false;
      s.graduated=true;
      s.schoolYear=next;
    }
  });
  activeSchoolYear=next;
  activeToken="";
  saveData();
  logActivity("Started new school year: "+next);
  notify("New school year "+next+" is now active.");
  renderSchoolYear();
  refreshAll();
  alert("New school year started. Previous attendance records remain saved. Review promoted students and update their sections in Student Management.");
}

function saveSettings(){
  const p=val("presentTime"),l=val("lateTime");
  if(!p||!l||l<p)return alert("Enter valid times. Late cutoff must be the same as or later than present cutoff.");
  settings={presentTime:p,lateTime:l,schoolName:val("schoolName")};saveData();logActivity("Updated attendance settings");alert("Settings saved.");
}
function refreshAll(){
  if(currentUser?.role==="admin"){renderDashboard();renderStudents();renderClasses();renderLive();renderRecords();renderMonthly();renderLate();renderAbsent();renderNotifications();renderLogs();}
  else if(currentUser?.role==="student"){renderStudentHome();renderMyRecords();}
}
function updateClock(){if(byId("clock"))byId("clock").textContent=new Date().toLocaleString();}
document.addEventListener("DOMContentLoaded",()=>{
  if(byId("recordDate"))byId("recordDate").value=today();
  if(byId("reportDate"))byId("reportDate").value=today();
  if(byId("monthFilter"))byId("monthFilter").value=today().slice(0,7);
  if(byId("schoolName"))byId("schoolName").value=settings.schoolName||"";
  if(byId("presentTime"))byId("presentTime").value=settings.presentTime||"07:30";
  if(byId("lateTime"))byId("lateTime").value=settings.lateTime||"07:45";
  const params=new URLSearchParams(window.location.search);
  const incomingToken=params.get("attendanceToken")||"";
  if (incomingToken && isAttendanceTokenValid(incomingToken)) {
    activeToken=incomingToken;
    pendingAttendanceToken=incomingToken;
    byId("loginScreen").classList.remove("hidden");
    byId("registrationScreen").classList.add("hidden");
    byId("application").classList.add("hidden");
    byId("qrLoginNotice")?.classList.remove("hidden");
    byId("loginRole").value="student";
    byId("loginRole").disabled=true;
    byId("loginID")?.focus();
  } else if (incomingToken) {
    alert("This attendance QR has expired. Please ask your administrator for a new QR code.");
    params.delete("attendanceToken");
    window.history.replaceState({},document.title,window.location.pathname);
  }
  updateClock();setInterval(updateClock,1000);
});
</script>
</body>
</html>
