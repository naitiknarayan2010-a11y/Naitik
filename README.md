<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Better World School | Official Portal</title>
    <style>
        :root { 
            --navy: #1a2a6c; --gold: #fdbb2d; --admin-red: #c0392b; --bg: #f8f9fa; 
        }
        body { font-family: 'Segoe UI', sans-serif; margin: 0; background: var(--bg); color: #333; }

        /* BRANDING & HEADER */
        .brand-header {
            background: var(--navy); color: white; text-align: center;
            padding: 25px 20px; border-bottom: 5px solid var(--gold);
        }
        .logo-circle {
            width: 60px; height: 60px; background: white; border-radius: 50%;
            display: inline-flex; align-items: center; justify-content: center;
            font-weight: bold; font-size: 20px; color: var(--navy); margin-bottom: 10px;
        }
        .ticker { background: #fff3cd; color: #856404; padding: 10px; font-size: 13px; text-align: center; border-bottom: 1px solid #ffeeba; }

        /* CONTAINERS */
        .container { max-width: 500px; margin: 0 auto; padding: 15px; }
        .card { background: white; border-radius: 15px; padding: 20px; margin-bottom: 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
        h3 { border-left: 4px solid var(--gold); padding-left: 10px; color: var(--navy); margin: 0 0 15px 0; font-size: 18px; }

        /* FORMS & BUTTONS */
        input, select, textarea { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; }
        .btn { background: var(--navy); color: white; border: none; padding: 15px; border-radius: 10px; width: 100%; font-weight: bold; cursor: pointer; margin-top: 10px; }
        .btn-gold { background: var(--gold); color: var(--navy); }
        .btn-admin { background: var(--admin-red); }

        /* TABLES */
        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th, td { text-align: left; padding: 10px; border-bottom: 1px solid #eee; font-size: 14px; }

        #login-screen, #enquiry-screen, #student-dash, #admin-dash { display: none; }
    </style>
</head>
<body onload="initApp()">

    <div class="brand-header">
        <div class="logo-circle">BWS</div>
        <h1 style="margin:0; font-size: 22px; letter-spacing: 1px;">BETTER WORLD SCHOOL</h1>
    </div>

    <div class="ticker">
        <strong>Notice:</strong> <span id="tickerNotice">Loading latest school updates...</span>
    </div>

    <div id="login-screen" class="container" style="display:block;">
        <div class="card" style="margin-top: 20px;">
            <h3 style="text-align:center; border:none;">Portal Login</h3>
            <input type="text" id="userInput" placeholder="Username (Admin or Student)">
            <button class="btn" onclick="handleLogin()">Sign In</button>
            <hr style="margin: 20px 0; border: 0; border-top: 1px solid #eee;">
            <p style="text-align:center; font-size: 14px; color: #666;">New to BWS?</p>
            <button class="btn btn-gold" onclick="showScreen('enquiry-screen')">Admission Enquiry</button>
        </div>
    </div>

    <div id="enquiry-screen" class="container">
        <div class="card">
            <h3>Admission Enquiry</h3>
            <input type="text" id="enqName" placeholder="Child's Name">
            <select id="enqClass">
                <option>Select Class</option>
                <option>Class 9</option>
                <option>Class 10</option>
            </select>
            <input type="tel" id="enqPhone" placeholder="Parent's Contact">
            <button class="btn btn-gold" onclick="submitEnquiry()">Submit Application</button>
            <button class="btn" style="background:#666;" onclick="showScreen('login-screen')">Back to Login</button>
        </div>
    </div>

    <div id="student-dash" class="container">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
            <h4 id="welcomeUser">Student: Baby</h4>
            <button onclick="location.reload()" style="color:var(--admin-red); background:none; border:none; font-weight:bold;">Logout</button>
        </div>
        
        <div class="card">
            <h3>📢 Latest Notice</h3>
            <p id="viewNotice">Welcome to the new session.</p>
        </div>

        <div class="card">
            <h3>📅 Attendance</h3>
            <p id="viewAtt" style="font-size: 20px; font-weight: bold; margin:0;">Present</p>
        </div>

        <div class="card">
            <h3>📊 Academic Results</h3>
            <table>
                <thead><tr><th>Subject</th><th>Test</th><th>Score</th></tr></thead>
                <tbody id="viewResults"></tbody>
            </table>
        </div>

        <div class="card">
            <h3>📝 Homework</h3>
            <p id="viewHW">No homework assigned.</p>
        </div>
    </div>

    <div id="admin-dash" class="container">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
            <h4 style="color:var(--admin-red)">Management Console</h4>
            <button onclick="location.reload()" style="color:var(--navy); background:none; border:none; font-weight:bold;">Logout</button>
        </div>

        <div class="card">
            <h3>Publish Notice</h3>
            <textarea id="inNotice" rows="2" placeholder="Update school news..."></textarea>
            <button class="btn btn-admin" onclick="saveData('notice')">Update Notice</button>
        </div>

        <div class="card">
            <h3>Daily Attendance</h3>
            <select id="inAtt"><option>Present</option><option>Absent</option></select>
            <button class="btn btn-admin" onclick="saveData('att')">Save Status</button>
        </div>

        <div class="card">
            <h3>Add Result Row</h3>
            <input id="inSub" placeholder="Subject">
            <input id="inTest" placeholder="Test Name">
            <input id="inScore" placeholder="Marks">
            <button class="btn btn-admin" onclick="saveData('result')">Add Result</button>
        </div>

        <div class="card">
            <h3>Update Homework</h3>
            <input id="inHW" placeholder="Today's task...">
            <button class="btn btn-admin" onclick="saveData('hw')">Post HW</button>
        </div>

        <button class="btn" style="background:#666; font-size: 12px;" onclick="clearAllData()">⚠️ Reset System Memory</button>
    </div>

    <script>
        function initApp() {
            const notice = localStorage.getItem('bwsNotice') || "Welcome to BWS!";
            document.getElementById('viewNotice').innerText = notice;
            document.getElementById('tickerNotice').innerText = notice;
            
            const att = localStorage.getItem('bwsAtt') || "Not Taken";
            document.getElementById('viewAtt').innerText = att;
            document.getElementById('viewAtt').style.color = (att === "Absent") ? "#c0392b" : "#27ae60";
            
            document.getElementById('viewHW').innerText = localStorage.getItem('bwsHW') || "None assigned.";
            document.getElementById('viewResults').innerHTML = localStorage.getItem('bwsResults') || "";
        }

        function showScreen(id) {
            const screens = ['login-screen', 'enquiry-screen', 'student-dash', 'admin-dash'];
            screens.forEach(s => document.getElementById(s).style.display = 'none');
            document.getElementById(id).style.display = 'block';
        }

        function handleLogin() {
            let user = document.getElementById('userInput').value.toLowerCase().trim();
            if(user === 'admin') showScreen('admin-dash');
            else if(user === 'student') showScreen('student-dash');
            else alert("Use 'Admin' or 'Student'");
        }

        function submitEnquiry() {
            if(document.getElementById('enqName').value) {
                alert("Enquiry Saved!");
                showScreen('login-screen');
            }
        }

        function saveData(type) {
            if(type === 'notice') localStorage.setItem('bwsNotice', document.getElementById('inNotice').value);
            else if(type === 'att') localStorage.setItem('bwsAtt', document.getElementById('inAtt').value);
            else if(type === 'hw') localStorage.setItem('bwsHW', document.getElementById('inHW').value);
            else if(type === 'result') {
                let row = `<tr><td>${document.getElementById('inSub').value}</td><td>${document.getElementById('inTest').value}</td><td><b>${document.getElementById('inScore').value}</b></td></tr>`;
                localStorage.setItem('bwsResults', (localStorage.getItem('bwsResults') || "") + row);
            }
            alert("Saved!");
            initApp();
        }

        function clearAllData() {
            if(confirm("Erase all marks and notices?")) {
                localStorage.clear();
                location.reload();
            }
        }
    </script>
</body>
</html>
