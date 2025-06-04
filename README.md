<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Realm of Quantaria: QS Level-Up Tracker</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f5f7fa;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #3a3a3a;
    }
    .intro {
      background: #e3f2fd;
      padding: 15px;
      border-radius: 10px;
      margin-bottom: 20px;
    }
    .intro h3 {
      margin-top: 0;
    }
    .level {
      background-color: #ffffff;
      border: 2px solid #ccc;
      border-radius: 12px;
      padding: 20px;
      margin-bottom: 20px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    .level h2 {
      color: #0066cc;
    }
    .complete {
      color: green;
      font-weight: bold;
    }
    .btn {
      padding: 10px 16px;
      background-color: #0066cc;
      color: #fff;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      margin-top: 10px;
    }
    .btn:disabled {
      background-color: #ccc;
    }
    .progress-container {
      background-color: #ddd;
      border-radius: 25px;
      margin: 20px 0;
      height: 24px;
      width: 100%;
      overflow: hidden;
    }
    .progress-bar {
      height: 100%;
      width: 0%;
      background-color: #28a745;
      text-align: center;
      line-height: 24px;
      color: white;
      font-weight: bold;
    }
    .xp-bar {
      margin-bottom: 10px;
    }
    a.task-link {
      display: inline-block;
      margin-top: 10px;
      color: #0066cc;
      text-decoration: underline;
    }
    .welcome {
      text-align: center;
      margin-top: 20px;
    }
    .welcome img {
      width: 100px;
    }
    .task-box {
      margin-top: 10px;
    }
    .task-box input[type="text"] {
      padding: 8px;
      width: 60%;
      margin-right: 10px;
    }
  </style>
</head>
<body>
  <h1>🏰 Realm of Quantaria: QS Level-Up Tracker</h1>

  <div class="intro">
    <h3>🎮 Welcome, Apprentice QS!</h3>
    <p>This is your personal quest to master Estimation, Costing & Quantity Surveying.</p>
    <p>✅ Complete each task to earn XP and level up!</p>
  </div>

  <div class="welcome">
    <img src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png" alt="Avatar">
    <p style="color:#555;">Welcome to your QS learning journey! Start your quests below 👇</p>
  </div>

  <div class="xp-bar">
    <label>XP Progress:</label>
    <div class="progress-container">
      <div id="progressBar" class="progress-bar">0%</div>
    </div>
  </div>

  <div class="level" id="level1">
    <h2>Level 1: Valley of Measurements</h2>
    <p>🎯 Objective: Master unit conversions</p>
    <div class="task-box">
      <p>Convert 1 meter to feet:</p>
      <input type="text" id="ans1" placeholder="Enter your answer">
      <button class="btn" onclick="checkAnswer(1, '3.28', 50)">Submit</button>
      <p id="status1"></p>
    </div>
  </div>

  <div class="level" id="level2">
    <h2>Level 2: The BOQ Temple</h2>
    <p>🎯 Objective: Prepare a BOQ for a simple project</p>
    <div class="task-box">
      <p>What is the unit of measurement for plastering?</p>
      <input type="text" id="ans2" placeholder="Enter your answer">
      <button class="btn" onclick="checkAnswer(2, 'sqm', 100)">Submit</button>
      <p id="status2"></p>
    </div>
  </div>

  <div class="level" id="level3">
    <h2>Level 3: Rate Analysis Caverns</h2>
    <p>🎯 Objective: Analyze the rate for one construction item</p>
    <div class="task-box">
      <p>What is the main cost component in concrete rate analysis?</p>
      <input type="text" id="ans3" placeholder="Enter your answer">
      <button class="btn" onclick="checkAnswer(3, 'cement', 100)">Submit</button>
      <p id="status3"></p>
    </div>
  </div>

  <div class="level" id="level4">
    <h2>Level 4: Procurement Docks</h2>
    <p>🎯 Objective: Learn tendering and vendor comparison</p>
    <div class="task-box">
      <p>Which document compares vendor prices?</p>
      <input type="text" id="ans4" placeholder="Enter your answer">
      <button class="btn" onclick="checkAnswer(4, 'comparison sheet', 80)">Submit</button>
      <p id="status4"></p>
    </div>
  </div>

  <div class="level" id="level5">
    <h2>Level 5: Estimator’s Arena</h2>
    <p>🎯 Objective: Estimate total cost including contingencies</p>
    <div class="task-box">
      <p>What is the typical percentage of contingency in cost estimation?</p>
      <input type="text" id="ans5" placeholder="Enter your answer">
      <button class="btn" onclick="checkAnswer(5, '5%', 120)">Submit</button>
      <p id="status5"></p>
    </div>
  </div>

  <div class="level" id="level6">
    <h2>Level 6: Dimension of Drawing Take-Off</h2>
    <p>🎯 Objective: Do take-off from basic drawings</p>
    <div class="task-box">
      <p>Name the method used for quantity take-off from drawings:</p>
      <input type="text" id="ans6" placeholder="Enter your answer">
      <button class="btn" onclick="checkAnswer(6, 'centerline method', 120)">Submit</button>
      <p id="status6"></p>
    </div>
  </div>

  <script>
    let xp = 0;
    let totalXP = 570;

    function checkAnswer(level, correctAnswer, points) {
      const userInput = document.getElementById(`ans${level}`).value.trim().toLowerCase();
      const status = document.getElementById(`status${level}`);
      const button = document.querySelector(`#level${level} .btn`);

      if (userInput === correctAnswer.toLowerCase()) {
        xp += points;
        status.innerHTML = `✅ <span class='complete'>Correct! (${points} XP)</span>`;
        button.disabled = true;
        updateProgressBar();
      } else {
        status.innerHTML = `❌ <span style='color:red;'>Incorrect. Try again!</span>`;
      }
    }

    function updateProgressBar() {
      const bar = document.getElementById("progressBar");
      const percent = Math.min((xp / totalXP) * 100, 100).toFixed(0);
      bar.style.width = percent + "%";
      bar.textContent = percent + "%";
    }
  </script>
</body>
</html>
