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
      padding: 8px 12px;
      background-color: #0066cc;
      color: #fff;
      border: none;
      border-radius: 6px;
      cursor: pointer;
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
      padding: 6px;
      width: 60%;
      margin-right: 10px;
    }
    .question {
      margin-bottom: 10px;
    }
  </style>
</head>
<body>
  <h1>🏰 Realm of Quantaria: QS Level-Up Tracker</h1>

  <div class="intro">
    <h3>🎮 Welcome, Apprentice QS!</h3>
    <p>This is your personal quest to master Estimation, Costing & Quantity Surveying.</p>
    <p>✅ Complete each level's questions to earn XP and level up!</p>
  </div>

  <div class="welcome">
    <img src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png" alt="Avatar">
    <p style="color:#555;">Start your QS journey below 👇</p>
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
    <div class="task-box" id="tasks1"></div>
    <p id="status1"></p>
  </div>

  <script>
    const levels = {
      1: {
        xp: 100,
        questions: [
          { q: "Convert 1 meter to feet", a: "3.28" },
          { q: "1 foot equals how many inches?", a: "12" },
          { q: "1 yard is how many feet?", a: "3" },
          { q: "How many sq.ft in 1 sq.m?", a: "10.76" },
          { q: "How many sq.m in 1 cent?", a: "40.47" },
          { q: "How many sq.ft in 1 ground?", a: "2400" },
          { q: "1 acre is how many cents?", a: "100" },
          { q: "1 cu.m equals how many liters?", a: "1000" },
          { q: "1 cu.m equals how many cu.ft?", a: "35.3" },
          { q: "1 sq.yard equals how many sq.ft?", a: "9" },
        ]
      }
    };

    let xp = 0;
    let totalXP = 100;

    function renderQuestions(levelId) {
      const container = document.getElementById(`tasks${levelId}`);
      const questions = levels[levelId].questions;
      questions.forEach((item, index) => {
        const div = document.createElement('div');
        div.className = 'question';
        div.innerHTML = `
          <p>${index + 1}. ${item.q}</p>
          <input type="text" id="ans${levelId}_${index}" placeholder="Answer">
          <button class="btn" onclick="checkAnswer(${levelId}, ${index})">Submit</button>
        `;
        container.appendChild(div);
      });
    }

    function checkAnswer(levelId, index) {
      const input = document.getElementById(`ans${levelId}_${index}`);
      const userAnswer = input.value.trim().toLowerCase();
      const correct = levels[levelId].questions[index].a.toLowerCase();
      const status = document.getElementById(`status${levelId}`);
      const button = input.nextElementSibling;

      if (userAnswer === correct) {
        xp += levels[levelId].xp / levels[levelId].questions.length;
        status.innerHTML = `<span class='complete'>✅ Question ${index + 1} Correct!</span>`;
        input.disabled = true;
        button.disabled = true;
        updateProgressBar();
      } else {
        status.innerHTML = `<span style='color:red;'>❌ Question ${index + 1} Incorrect. Try again!</span>`;
      }
    }

    function updateProgressBar() {
      const bar = document.getElementById("progressBar");
      const percent = Math.min((xp / totalXP) * 100, 100).toFixed(0);
      bar.style.width = percent + "%";
      bar.textContent = percent + "%";
    }

    // Render questions for level 1 on page load
    renderQuestions(1);
  </script>
</body>
</html>
