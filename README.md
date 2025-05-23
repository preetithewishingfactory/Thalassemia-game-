<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DNA Detective: The Genetic Mystery</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(to right, #1e1e2f, #29294d);
      color: #fff;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      overflow-y: auto;
    }
    .game-container {
      background-color: rgba(44, 44, 77, 0.9);
      padding: 40px;
      border-radius: 15px;
      max-width: 800px;
      text-align: center;
      box-shadow: 0 0 20px rgba(0, 255, 255, 0.4);
      margin: 50px auto;
    }
    h1 {
      color: #00ffe1;
      margin-bottom: 10px;
    }
    p {
      font-size: 18px;
      line-height: 1.5;
    }
    .clue {
      background: linear-gradient(45deg, #333355, #444466);
      margin: 20px 0;
      padding: 20px;
      border-radius: 10px;
      cursor: pointer;
      transition: background 0.3s, transform 0.2s;
      box-shadow: 0 0 10px rgba(0,0,0,0.3);
    }
    .clue:hover {
      background: linear-gradient(45deg, #555577, #666688);
      transform: scale(1.03);
    }
    .hidden {
      display: none;
    }
    button {
      margin-top: 20px;
      padding: 12px 25px;
      background-color: #00ffe1;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      color: #000;
      font-weight: bold;
      transition: background-color 0.3s, transform 0.2s;
    }
    button:hover {
      background-color: #00ccc1;
      transform: scale(1.05);
    }
    #result {
      margin-top: 15px;
      font-size: 18px;
      font-weight: bold;
    }
    .emoji {
      font-size: 24px;
    }
    .character-select {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-bottom: 20px;
    }
    .character {
      border: 2px solid #00ffe1;
      border-radius: 10px;
      padding: 10px;
      cursor: pointer;
      width: 120px;
      background-color: #1e1e3f;
    }
    .character:hover {
      background-color: #2e2e5f;
    }
    .fun-fact {
      font-style: italic;
      color: #ffcc00;
      margin-top: 10px;
    }
    .question-box {
      margin: 20px 0;
    }
    .explanation {
      font-size: 16px;
      margin-top: 10px;
      color: #ffcc00;
    }
  </style>
</head>
<body>
  <div class="game-container" id="intro">
    <h1 class="emoji">🕵️‍♂️ DNA Detective: Mission Briefing</h1>
    <p>Welcome, Detective! A mysterious blood disorder is spreading. Only YOU can uncover the genetic truth.</p>
    <p>Choose your character:</p>
    <div class="character-select">
      <div class="character" onclick="startGame('Agent Red')">🧑‍🔬 Agent Red</div>
      <div class="character" onclick="startGame('Dr. Riya')">👨‍⚕️ Dr. Riya</div>
    </div>
  </div>

  <div class="game-container hidden" id="game">
    <h1 class="emoji">🧬 DNA Detective</h1>
    <p id="game-intro"></p>

    <div class="question-box" id="question-box"></div>
    <div id="quiz" class="hidden">
      <h2>🧩 Final Puzzle</h2>
      <p>How can Thalassemia be prevented?</p>
      <button onclick="submitFinal(true)">✅ By getting a genetic test before marriage</button><br><br>
      <button onclick="submitFinal(false)">❌ By avoiding fast food</button>
      <p id="result"></p>
      <p id="final-score"></p>
      <button onclick="restartGame()">🔁 Play Again</button>
    </div>
  </div>

  <script>
    let score = 0;
    let currentIndex = 0;

    const questions = [
      {
        question: "What is Thalassemia?",
        correct: "It is a genetic blood disorder affecting hemoglobin production.",
        explanation: "Thalassemia affects your red blood cells and reduces the amount of hemoglobin, causing anemia.",
        options: [
          "A skin infection",
          "It is a genetic blood disorder affecting hemoglobin production.",
          "A type of allergy"
        ]
      },
      {
        question: "Is Thalassemia inherited?",
        correct: "Yes, it is passed down through genes from parents.",
        explanation: "Thalassemia is passed from parents to children through defective hemoglobin genes.",
        options: [
          "No, it's caused by bacteria",
          "Yes, it is passed down through genes from parents.",
          "Only adults can get it"
        ]
      },
      {
        question: "What are the types of Thalassemia?",
        correct: "Alpha and Beta Thalassemia",
        explanation: "Alpha and Beta refer to the specific hemoglobin chains affected by the mutation.",
        options: [
          "Type 1 and Type 2",
          "Alpha and Beta Thalassemia",
          "Mild and Severe"
        ]
      },
      {
        question: "Which is a symptom of Thalassemia?",
        correct: "Fatigue and pale skin",
        explanation: "Fatigue and pale skin are common symptoms due to low red blood cell levels.",
        options: [
          "Rashes",
          "Fatigue and pale skin",
          "Frequent sneezing"
        ]
      },
      {
        question: "How is Thalassemia diagnosed?",
        correct: "Through blood tests and genetic testing",
        explanation: "A complete blood count and hemoglobin electrophoresis help in diagnosing Thalassemia.",
        options: [
          "Through urine samples",
          "Only physical checkups",
          "Through blood tests and genetic testing"
        ]
      },
      {
        question: "What is a common treatment?",
        correct: "Regular blood transfusions",
        explanation: "Severe Thalassemia often requires lifelong regular blood transfusions.",
        options: [
          "Antibiotics",
          "Regular blood transfusions",
          "Vaccines"
        ]
      },
      {
        question: "Is Thalassemia curable?",
        correct: "No, but it can be prevented",
        explanation: "There's no universal cure, but genetic counseling and testing help prevent it.",
        options: [
          "Yes, with diet",
          "No, but it can be prevented",
          "Yes, easily with medicine"
        ]
      }
    ];

    function startGame(name) {
      document.getElementById("intro").classList.add("hidden");
      document.getElementById("game").classList.remove("hidden");
      document.getElementById("game-intro").textContent = `Welcome, ${name}. Your mission: uncover the truth about this genetic disorder.`;
      showQuestion();
    }

    function showQuestion() {
      const box = document.getElementById("question-box");
      box.innerHTML = '';
      if (currentIndex >= questions.length) {
        document.getElementById("quiz").classList.remove("hidden");
        return;
      }
      const q = questions[currentIndex];
      const qElem = document.createElement("div");
      qElem.innerHTML = `<h2>${q.question}</h2>`;
      q.options.forEach(opt => {
        const btn = document.createElement("button");
        btn.textContent = opt;
        btn.onclick = () => selectAnswer(opt, q);
        qElem.appendChild(btn);
        qElem.appendChild(document.createElement("br"));
      });
      box.appendChild(qElem);
    }

    function selectAnswer(selected, question) {
      const box = document.getElementById("question-box");
      const explanation = document.createElement("p");
      explanation.className = 'explanation';

      if (selected === question.correct) {
        score++;
        explanation.textContent = `✅ Correct! ${question.explanation}`;
        explanation.style.color = '#00ff99';
      } else {
        explanation.textContent = `❌ Incorrect. The correct answer is: "${question.correct}". ${question.explanation}`;
        explanation.style.color = '#ffcc00';
      }

      box.appendChild(explanation);
      currentIndex++;

      setTimeout(showQuestion, 2500);
    }

    function submitFinal(correct) {
      const result = document.getElementById("result");
      const final = document.getElementById("final-score");

      if (correct) {
        score++;
        result.textContent = "🎉 Correct! Thalassemia is preventable through genetic awareness and testing.";
        result.style.color = "#00ff99";
      } else {
        result.textContent = "🚫 Not quite! Prevention comes from awareness, not diet.";
        result.style.color = "#ff4444";
      }

      final.textContent = `Your final score: ${score}/8 - ${score >= 6 ? '🟢 Passed' : '🔴 Try Again'}`;
    }

    function restartGame() {
      score = 0;
      currentIndex = 0;
      document.getElementById("quiz").classList.add("hidden");
      document.getElementById("game").classList.add("hidden");
      document.getElementById("intro").classList.remove("hidden");
    }
  </script>
</body>
</html>



