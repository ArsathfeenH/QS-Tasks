<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Quantaria - QS Tracker</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f2f2f2;
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 600px;
      margin: 50px auto;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    h2, h3, h4 {
      text-align: center;
    }

    .question {
      margin: 20px 0;
    }

    input[type="text"] {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
    }

    button {
      padding: 10px 20px;
      background-color: #28a745;
      border: none;
      color: white;
      cursor: pointer;
      margin-top: 10px;
    }

    button:hover {
      background-color: #218838;
    }

    .hint, .mcq {
      background: #e9ecef;
      padding: 10px;
      margin-top: 10px;
    }

    .leaderboard, .feedback {
      margin-top: 30px;
      background: #f8f9fa;
      padding: 15px;
      border-radius: 5px;
    }

    .level-buttons {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 20px;
    }

    .hidden {
      display: none;
    }
  </style>
</head>

<body>
  <div class="container">
    <h2>Welcome to Quantaria - QS Tracker</h2>
    <div id="login">
      <label>Enter your name:</label>
      <input type="text" id="playerName" />
      <button onclick="startGame()">Start</button>
    </div>

    <div id="levelSelector" class="hidden">
      <h3>Select a Level</h3>
      <div class="level-buttons" id="levelButtons"></div>
    </div>

    <div id="game" class="hidden">
      <div id="levelTitle"></div>
      <div id="questionSection"></div>
      <input type="text" id="answer" placeholder="Your answer" />
      <button onclick="submitAnswer()">Submit</button>
      <div class="hint" id="hint" style="display:none"></div>
      <button id="dontKnowBtn" onclick="showMCQ()" style="display:none">Don't Know?</button>
      <div class="mcq" id="mcqSection" style="display:none"></div>
      <div id="feedbackSection" class="feedback hidden">
        <h4>Feedback</h4>
        <textarea id="feedback" placeholder="Your feedback (optional)"></textarea><br>
        <button onclick="submitFeedback()">Submit Feedback</button>
      </div>
      <div id="score"></div>
    </div>

    <div class="leaderboard">
      <h3>Leaderboard</h3>
      <ul id="leaderboard"></ul>
    </div>
  </div>

  <script>
    let currentLevel = 0;
    let questionIndex = 0;
    let attempts = 0;
    let score = 0;
    let playerName = '';
    const requiredScore = 70;

    const levels = [
  {
    title: "Level 1: Basic Unit Conversion",
    questions: [
      { q: "1m = __ft", a: "3.28", hint: "Between 3ft and 4ft", options: ["2.4", "3.28", "4.2", "1.5"] },
      { q: "1ft = __inches", a: "12", hint: "Between 10 and 15", options: ["10", "11", "12", "13"] },
      { q: "1yard = __ft", a: "3", hint: "Between 2 and 4", options: ["2", "3", "4", "5"] },
      { q: "1sq.m = __sq.ft", a: "10.764", hint: "Between 10 and 11", options: ["9.9", "10.1", "10.764", "11.3"] },
      { q: "1acre = __cent", a: "100", hint: "Between 90 and 110", options: ["80", "100", "120", "150"] }
    ]
  },
  {
    title: "Level 2: Building Material Identification",
    questions: [
      { q: "Which material is most commonly used for RCC?", a: "Concrete", hint: "It's a mix of cement, sand, and aggregate", options: ["Steel", "Concrete", "Bricks", "Wood"] },
      { q: "Which material is used in waterproofing?", a: "Bitumen", hint: "Sticky black substance", options: ["Sand", "Clay", "Bitumen", "Gravel"] },
      { q: "Which test is used for brick strength?", a: "Compression test", hint: "It's a standard strength test", options: ["Tension test", "Flexure test", "Compression test", "Impact test"] },
      { q: "What is used as binding material in mortar?", a: "Cement", hint: "Comes in bags", options: ["Clay", "Cement", "Lime", "Gypsum"] },
      { q: "Which is a lightweight material?", a: "AAC block", hint: "Used as a replacement for bricks", options: ["Concrete", "Clay brick", "AAC block", "Stone"] }
    ]
  },
  {
    title: "Level 3: Basic BOQ & Measurement Units",
    questions: [
      { q: "Unit of measurement for concrete in BOQ?", a: "cu.m", hint: "It’s a volume unit", options: ["sq.m", "cu.m", "kg", "meter"] },
      { q: "Unit for plastering work?", a: "sq.m", hint: "It’s a surface area unit", options: ["cu.m", "sq.m", "m", "kg"] },
      { q: "What is DSR?", a: "Delhi Schedule of Rates", hint: "Published by CPWD", options: ["Design Schedule Rule", "Delhi Schedule of Rates", "Departmental Standard Rate", "Design Safety Ratio"] },
      { q: "BOQ stands for?", a: "Bill of Quantities", hint: "Used in tendering", options: ["Bill of Quarters", "Board of Quality", "Bill of Quantities", "Bulk of Quantity"] },
      { q: "Which unit is used for steel bars?", a: "kg", hint: "It’s a weight-based item", options: ["cu.m", "sq.m", "kg", "m"] }
    ]
  },
  {
    title: "Level 4: Rate Analysis Basics",
    questions: [
      { q: "Which factor affects labor rate?", a: "Location", hint: "City vs village", options: ["Color", "Location", "Shape", "Size"] },
      { q: "What is included in rate analysis?", a: "Material, labor, overhead", hint: "All direct and indirect costs", options: ["Only material", "Only labor", "Material, labor, overhead", "Only taxes"] },
      { q: "Which document helps in rate analysis?", a: "DSR", hint: "CPWD published", options: ["BOQ", "IS code", "DSR", "Site log"] },
      { q: "Profit margin in rate analysis is usually?", a: "10%", hint: "Common percentage", options: ["5%", "10%", "15%", "20%"] },
      { q: "Rate analysis is mostly done for?", a: "Estimating cost", hint: "For planning budget", options: ["Drawing", "Casting", "Estimating cost", "Inspection"] }
    ]
  },
  {
    title: "Level 5: Quantity Take-Off",
    questions: [
      { q: "Which drawing is most used in QTO?", a: "Plan", hint: "Top view", options: ["Elevation", "Plan", "Section", "Detail"] },
      { q: "Main method used in QTO?", a: "Center line method", hint: "Common for wall calculations", options: ["Center line method", "Plinth method", "Volume method", "Line method"] },
      { q: "Quantity of concrete is taken in?", a: "cu.m", hint: "Always measured in volume", options: ["sq.m", "cu.m", "kg", "m"] },
      { q: "Quantity of plaster is taken in?", a: "sq.m", hint: "Area based measurement", options: ["cu.m", "sq.m", "m", "litre"] },
      { q: "Formula for volume?", a: "L x B x H", hint: "Three dimensions", options: ["L + B + H", "L x B x H", "2 x (L + B)", "L x H"] }
    ]
  },
  {
    title: "Level 6: QS Practical Scenario",
    questions: [
      { q: "A site needs 100 cu.m concrete. 1 truck = 6 cu.m. Trucks needed?", a: "17", hint: "Round up after division", options: ["15", "16", "17", "18"] },
      { q: "A wall is 3m high, 10m long, and 0.2m thick. Volume?", a: "6", hint: "Use L x B x H", options: ["5", "6", "7", "8"] },
      { q: "A room 4x4m is tiled with 2x2ft tiles. How many tiles?", a: "43", hint: "Convert units", options: ["36", "40", "43", "45"] },
      { q: "Rate of painting is ₹25/sq.m. Room has 80sq.m. Total?", a: "2000", hint: "Multiply directly", options: ["1800", "2000", "2200", "2500"] },
      { q: "Steel weight = D²/162. D=12mm. Weight/m?", a: "0.89", hint: "Plug in D", options: ["0.85", "0.89", "0.91", "0.95"] }
    ]
  }
];

    function startGame() {
      playerName = document.getElementById("playerName").value.trim();
      if (!playerName) return alert("Please enter your name.");
      document.getElementById("login").classList.add("hidden");
      document.getElementById("levelSelector").classList.remove("hidden");
      generateLevelButtons();
      loadLeaderboard();
    }

    function generateLevelButtons() {
      const container = document.getElementById("levelButtons");
      container.innerHTML = '';
      for (let i = 0; i < levels.length; i++) {
        const btn = document.createElement("button");
        btn.innerText = levels[i].title;
        btn.disabled = !isLevelUnlocked(i);
        btn.onclick = () => loadLevel(i);
        container.appendChild(btn);
      }
    }

    function isLevelUnlocked(levelIndex) {
      if (levelIndex === 0) return true;
      const previousScore = JSON.parse(localStorage.getItem(playerName + "_score"))?.[levelIndex - 1] || 0;
      return previousScore >= requiredScore;
    }

    function loadLevel(levelNum) {
      currentLevel = levelNum;
      questionIndex = 0;
      attempts = 0;
      score = 0;
      document.getElementById("levelSelector").classList.add("hidden");
      document.getElementById("game").classList.remove("hidden");
      document.getElementById("levelTitle").innerText = levels[currentLevel].title;
      loadQuestion();
    }

    function loadQuestion() {
      const currentQ = levels[currentLevel].questions[questionIndex];
      document.getElementById("questionSection").innerText = currentQ.q;
      document.getElementById("answer").value = "";
      document.getElementById("hint").style.display = "none";
      document.getElementById("dontKnowBtn").style.display = "none";
      document.getElementById("mcqSection").style.display = "none";
    }

    function submitAnswer() {
      const userAnswer = document.getElementById("answer").value.trim();
      const correct = levels[currentLevel].questions[questionIndex].a;
      attempts++;
      if (userAnswer.toLowerCase() === correct.toLowerCase()) {
        let points = attempts <= 2 ? 100 : attempts <= 4 ? 75 : attempts <= 6 ? 50 : 20;
        score += points / levels[currentLevel].questions.length;
        nextOrEnd();
      } else {
        if (attempts === 2) showHint();
        else if (attempts === 4) document.getElementById("dontKnowBtn").style.display = "block";
        else if (attempts > 6) nextOrEnd();
      }
    }

    function showHint() {
      document.getElementById("hint").innerText = levels[currentLevel].questions[questionIndex].hint;
      document.getElementById("hint").style.display = "block";
    }

    function showMCQ() {
      const options = levels[currentLevel].questions[questionIndex].options;
      const mcq = document.getElementById("mcqSection");
      mcq.innerHTML = "";
      options.forEach(opt => {
        const btn = document.createElement("button");
        btn.innerText = opt;
        btn.onclick = () => {
          document.getElementById("answer").value = opt;
          submitAnswer();
        };
        mcq.appendChild(btn);
      });
      mcq.style.display = "block";
    }

    function nextOrEnd() {
      questionIndex++;
      attempts = 0;
      if (questionIndex < levels[currentLevel].questions.length) {
        loadQuestion();
      } else {
        endLevel();
      }
    }

    function endLevel() {
      const finalScore = Math.round(score);
      document.getElementById("score").innerText = `Level Complete! Your Score: ${finalScore}%`;
      let stored = JSON.parse(localStorage.getItem(playerName + "_score")) || [];
      stored[currentLevel] = finalScore;
      localStorage.setItem(playerName + "_score", JSON.stringify(stored));
      loadLeaderboard();
      document.getElementById("feedbackSection").classList.remove("hidden");
    }

    function submitFeedback() {
      const feedback = document.getElementById("feedback").value;
      const msg = `${playerName} - ${Math.round(score)} - ${feedback}`;
      alert("Feedback submitted: " + msg);
    }

    function loadLeaderboard() {
      const board = document.getElementById("leaderboard");
      board.innerHTML = "";
      Object.keys(localStorage).forEach(key => {
        if (key.endsWith("_score")) {
          const name = key.replace("_score", "");
          const scores = JSON.parse(localStorage.getItem(key));
          const total = scores.reduce((a, b) => a + b, 0);
          const li = document.createElement("li");
          li.innerText = `${name}: ${total}%`;
          board.appendChild(li);
        }
      });
    }
  </script>
</body>

</html>
