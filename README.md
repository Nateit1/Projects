🚀 Interactive Web Projects

A collection of simple but impactful projects I built to explore different areas of programming, from APIs and 3D rendering to real-time tracking and game mechanics. Each project showcases key concepts like UI design, interactivity, and basic physics.

🔧 Projects


📊 Automatic Status Monitor

Tech: JavaScript

Built an automated system that monitors device/system statuses and sends real-time simulated alerts to improve operational awareness and reliability.

<img width="563" height="214" alt="Untitled22" src="https://github.com/user-attachments/assets/931faea2-a8d7-4500-a16d-990ef1b038f1" />
[Uploading index-jsfiddle-7dshbn28-10.html…]()<!DOCTYPE html>
<html>

<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>JSFiddle 7dshbn28</title>

  <style>
    body {
  background: #0f0f1a;
  font-family: 'Orbitron', sans-serif;
  color: #eeeeee;
  padding: 20px;
  display: flex;
  justify-content: center;
}

#app {
  background: #121226;
  padding: 20px 30px;
  border-radius: 15px;
  width: 500px;
  box-shadow: 0 0 15px #ff3f3f;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #ff3f3f;
  text-shadow: 0 0 8px #ff3f3f;
}

.input-row {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

input[type="text"] {
  flex-grow: 1;
  padding: 10px;
  border-radius: 6px;
  border: none;
  font-size: 1rem;
  background: #22223b;
  color: #eee;
  box-shadow: inset 0 0 5px rgb(255, 0, 0);
}

button {
  padding: 10px 15px;
  background: #ff3f3f;
  border: none;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
  color: #121226;
  box-shadow: 0 0 12px #3ffcff;
  transition: background 0.3s ease;
}
button:hover {
  background: #1be8ff;
}

label {
  display: block;
  margin-bottom: 20px;
  font-size: 0.9rem;
  color: #7fffd4;
  text-align: center;
}

#progressContainer {
  width: 100%;
  height: 10px;
  background: #22223b;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 20px;
}

#progressBar {
  width: 0%;
  height: 100%;
  background: linear-gradient(90deg, #00fff7, #00b3cc);
  border-radius: 10px;
  transition: width 0.5s linear;
}

ul#siteList {
  list-style: none;
  padding: 0;
}

ul#siteList li {
  background: #22223b;
  margin-bottom: 15px;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 0 10px #00fff7;
  display: flex;
  justify-content: space-between;
  align-items: center;
  animation: fadeIn 0.8s ease forwards;
}

.status-dot {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  margin-right: 10px;
  box-shadow: 0 0 8px;
  animation: pulse 2s infinite;
}

.online-dot {
  background: #00ff99;
  box-shadow: 0 0 12px #00ff99;
}

.offline-dot {
  background: #ff0044;
  box-shadow: 0 0 12px #ff0044;
}

.status-info {
  flex-grow: 1;
}

.remove-btn {
  background: transparent;
  border: none;
  font-size: 22px;
  color: #ff0044;
  cursor: pointer;
  transition: transform 0.2s ease;
}
.remove-btn:hover {
  transform: scale(1.3);
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

@keyframes fadeIn {
body {
  font-family: 'Segoe UI', sans-serif;
  background-color: #1a1a2e;
  color: #e0e0e0;
  text-align: center;
  padding: 2rem;
}




  </style>

  
</head>
<body>
  <div id="app">
  <h2>Zyntara - Automated Status Monitor</h2>

  <div class="input-row">
    <input type="text" id="urlInput" placeholder="Enter a URL (https://...)" />
    <button onclick="addWebsite()">Add</button>
  </div>

  <label>
    Check every 
    <select id="intervalSelect">
      <option value="30">30s</option>
      <option value="60" selected>1m</option>
      <option value="300">5m</option>
    </select>
  </label>

  <div id="progressContainer">
    <div id="progressBar"></div>
  </div>

  <ul id="siteList"></ul>
</div>

 




  <script>
    let websites = JSON.parse(localStorage.getItem('websites')) || [];
let intervalID;
let progress = 0;
let progressInterval;

function addWebsite() {
  const input = document.getElementById("urlInput");
  const url = input.value.trim();
  if (!url || websites.find(w => w.url === url)) {
    alert('Please enter a unique valid URL.');
    return;
  }
  websites.push({ url, status: "unknown", lastChecked: "" });
  input.value = "";
  saveWebsites();
  updateDisplay();
  checkStatus();
  resetProgress();
}

function removeWebsite(index) {
  websites.splice(index, 1);
  saveWebsites();
  updateDisplay();
}

function saveWebsites() {
  localStorage.setItem('websites', JSON.stringify(websites));
}

function updateDisplay() {
  const list = document.getElementById("siteList");
  list.innerHTML = '';
  websites.forEach((site, index) => {
    const li = document.createElement("li");

    const dot = document.createElement("span");
    dot.className = "status-dot " + (site.status === "online" ? "online-dot" : site.status === "offline" ? "offline-dot" : "");
    
    const info = document.createElement("div");
    info.className = "status-info";
    info.innerHTML = `<strong>${site.url}</strong><br>Status: ${site.status.toUpperCase()} | Last Checked: ${site.lastChecked || 'Never'}`;

    const removeBtn = document.createElement("button");
    removeBtn.className = "remove-btn";
    removeBtn.innerHTML = "×";
    removeBtn.onclick = () => removeWebsite(index);

    li.appendChild(dot);
    li.appendChild(info);
    li.appendChild(removeBtn);
    list.appendChild(li);
  });
}

async function checkStatus() {
  for (let i = 0; i < websites.length; i++) {
    const site = websites[i];
    try {
      await fetch(site.url, { method: "HEAD", mode: "no-cors" });
      if (site.status === 'offline') playSound('online');
      websites[i].status = "online";
    } catch {
      if (site.status === 'online') playSound('offline');
      websites[i].status = "offline";
    }
    websites[i].lastChecked = new Date().toLocaleTimeString();
  }
  updateDisplay();
  saveWebsites();
}

function playSound(type) {
  const sound = type === 'online' ? document.getElementById('onlineSound') : document.getElementById('offlineSound');
  sound.currentTime = 0;
  sound.play();
}

function setIntervalCheck() {
  clearInterval(intervalID);
  clearInterval(progressInterval);
  progress = 0;
  const intervalSeconds = parseInt(document.getElementById("intervalSelect").value);
  intervalID = setInterval(() => {
    checkStatus();
    resetProgress();
  }, intervalSeconds * 1000);
  startProgress(intervalSeconds);
  checkStatus();
}

function resetProgress() {
  progress = 0;
  updateProgressBar();
}

function startProgress(seconds) {
  const progressBar = document.getElementById("progressBar");
  clearInterval(progressInterval);
  progressInterval = setInterval(() => {
    progress += 100 / seconds;
    if (progress > 100) progress = 0;
    updateProgressBar();
  }, 1000);
}

function updateProgressBar() {
  const progressBar = document.getElementById("progressBar");
  progressBar.style.width = progress + "%";
}

document.getElementById("intervalSelect").addEventListener("change", setIntervalCheck);

updateDisplay();
setIntervalCheck();
let automationRunning = false;
let intervalId = null;

function log(message) {
  const logList = document.getElementById("logList");
  const item = document.createElement("li");
  item.textContent = `[${new Date().toLocaleTimeString()}] ${message}`;
  logList.prepend(item);
}

function startAutomation() {
  if (automationRunning) return;
  automationRunning = true;
  document.getElementById("status").textContent = "Running...";
  log("Automation started.");

  intervalId = setInterval(() => {
    const tasks = ["Monitoring network", "Checking logs", "Running scans", "Reporting status"];
    const task = tasks[Math.floor(Math.random() * tasks.length)];
    log(`Executing task: ${task}`);
  }, 2000);
}

  </script>
</body>
</html>



📍 GPS Tracker

Tech: JavaScript, HTML/CSS

Designed a location tracking interface with real-time updates and map visuals to simulate object movement across U.S. locations using mock data.

<img width="594" height="350" alt="Untitled 23" src="https://github.com/user-attachments/assets/88c7f94a-4194-4a14-9ec0-bf876b20e0d1" />



🟢 Weather App with 3D Globe

Tech: Three.js, HTML, CSS, JavaScript

Built a weather app featuring a spinning 3D Earth globe. Users can enter a city name and view simulated weather details like temperature, humidity, condition and wind, powered by a custom mock API.

<img width="1384" height="797" alt="Untitled" src="https://github.com/user-attachments/assets/3cb13a7b-a115-4e05-b797-08e13357c0aa" />



🚗 2D Car Model

Tech: HTML Canvas, JavaScript

Created a 2D car in a dark environment that users can control with keyboard keys. Demonstrates basic physics, movement mechanics, and responsive input handling.

<img width="613" height="414" alt="Untitled-4" src="https://github.com/user-attachments/assets/6b69798b-90cf-41b7-ab09-ddb967f64a03" />



💡 Why These Projects?

Each of these projects helped me:

Practice DOM manipulation and API integration
Explore animation, real-time data handling, and interactivity
Strengthen front-end development fundamentals
