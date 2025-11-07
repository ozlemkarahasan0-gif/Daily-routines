<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Daily Routine Word Guess Game</title>
  <style>
    body {
      font-family: "Poppins", sans-serif;
      background: linear-gradient(135deg, #a8edea, #fed6e3);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      text-align: center;
      color: #222;
    }
    h1 {
      color: #2b2b2b;
      text-shadow: 1px 1px #fff;
      font-size: 1.8em;
      margin-bottom: 10px;
    }
    #settings {
      margin: 10px;
    }
    select {
      padding: 8px;
      border-radius: 8px;
      border: 2px solid #4CAF50;
      font-size: 1em;
    }
    #wordDisplay {
      font-size: 2em;
      letter-spacing: 10px;
      margin: 20px;
      word-wrap: break-word;
    }
    input {
      padding: 10px;
      font-size: 1.1em;
      width: 60px;
      text-align: center;
      border: 2px solid #4CAF50;
      border-radius: 8px;
    }
    button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 1em;
      border: none;
      border-radius: 10px;
      background-color: #4CAF50;
      color: white;
      cursor: pointer;
      transition: 0.3s;
    }
    button:hover {
      background-color: #45a049;
    }
    #message {
      font-size: 1.2em;
      margin-top: 10px;
    }
    #score {
      margin-top: 10px;
      font-weight: bold;
    }
    #translation {
      margin-top: 10px;
      font-style: italic;
      color: #333;
    }
    #timer {
      margin-top: 5px;
      font-weight: bold;
      color: #b22222;
    }
    @media (max-width: 600px) {
      #wordDisplay {
        font-size: 1.5em;
        letter-spacing: 6px;
      }
      input {
        width: 40px;
      }
    }
  </style>
</head>
<body>

  <h1>🕒 Daily Routine Word Guess Game</h1>

  <div id="settings">
    <label for="difficulty">Difficulty:</label>
    <select id="difficulty" onchange="setDifficulty()">
      <option value="easy">Easy</option>
      <option value="medium" selected>Medium</option>
      <option value="hard">Hard</option>
    </select>
  </div>

  <div id="wordDisplay"></div>
  <input type="text" id="guessInput" maxlength="1" placeholder="?" autofocus>
  <button onclick="checkGuess()">Guess</button>
  <button onclick="newWord()">New Word</button>

  <div id="message"></div>
  <div id="translation"></div>
  <div id="timer">⏳ Time: 30</div>
  <div id="score">Score: 0 | Lives: 6</div>

  <!-- Ses efektleri -->
  <audio id="correctSound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_25f7f80b14.mp3?filename=correct-2-46134.mp3"></audio>
  <audio id="wrongSound" src="https://cdn.pixabay.com/download/audio/2021/09/06/audio_64d3324cb7.mp3?filename=error-126627.mp3"></audio>
  <audio id="winSound" src="https://cdn.pixabay.com/download/audio/2021/08/04/audio_aa1b5cce26.mp3?filename=success-fanfare-trumpets-6185.mp3"></audio>

  <script>
    const wordList = [
      { word: "wake", tr: "uyanmak" },
      { word: "brush", tr: "fırçalamak" },
      { word: "shower", tr: "duş almak" },
      { word: "breakfast", tr: "kahvaltı yapmak" },
      { word: "work", tr: "çalışmak" },
      { word: "commute", tr: "işe gidip gelmek" },
      { word: "exercise", tr: "egzersiz yapmak" },
      { word: "shave", tr: "tıraş olmak" },
      { word: "sleep", tr: "uyumak" },
      { word: "relax", tr: "dinlenmek" }
    ];

    let selectedWord = "";
    let translation = "";
    let displayWord = [];
    let lives = 6;
    let score = 0;
    let timer = 30;
    let countdown;
    let difficulty = "medium";

    const correctSound = document.getElementById("correctSound");
    const wrongSound = document.getElementById("wrongSound");
    const winSound = document.getElementById("winSound");

    function setDifficulty() {
      difficulty = document.getElementById("difficulty").value;
      if (difficulty === "easy") timer = 45;
      if (difficulty === "medium") timer = 30;
      if (difficulty === "hard") timer = 20;
      newWord();
    }

    function startTimer() {
      clearInterval(countdown);
      countdown = setInterval(() => {
        timer--;
        document.getElementById("timer").textContent = `⏳ Time: ${timer}`;
        if (timer <= 0) {
          clearInterval(countdown);
          document.getElementById("message").textContent = `⏰ Time's up! The word was: ${selectedWord}`;
          lives = 0;
          updateScore();
        }
      }, 1000);
    }

    function newWord() {
      const random = wordList[Math.floor(Math.random() * wordList.length)];
      selectedWord = random.word;
      translation = random.tr;
      displayWord = Array(selectedWord.length).fill("_");
      lives = 6;
      document.getElementById("wordDisplay").textContent = displayWord.join(" ");
      document.getElementById("translation").textContent = `💬 Hint (Türkçe): ${translation}`;
      document.getElementById("message").textContent = "";
      if (difficulty === "easy") timer = 45;
      if (difficulty === "medium") timer = 30;
      if (difficulty === "hard") timer = 20;
      updateScore();
      startTimer();
    }

    function checkGuess() {
      const guess = document.getElementById("guessInput").value.toLowerCase();
      document.getElementById("guessInput").value = "";

      if (!guess.match(/[a-z]/) || guess.length !== 1) {
        document.getElementById("message").textContent = "Please enter a valid letter!";
        return;
      }

      if (selectedWord.includes(guess)) {
        let correct = false;
        for (let i = 0; i < selectedWord.length; i++) {
          if (selectedWord[i] === guess) {
            displayWord[i] = guess;
            correct = true;
          }
        }
        document.getElementById("wordDisplay").textContent = displayWord.join(" ");
        document.getElementById("message").textContent = "✅ Good guess!";
        correctSound.play();
      } else {
        lives--;
        document.getElementById("message").textContent = "❌ Wrong guess!";
        wrongSound.play();
      }

      updateScore();

      if (!displayWord.includes("_")) {
        score += 10;
        winSound.play();
        document.getElementById("message").textContent = `🎉 You guessed it! The word was: ${selectedWord}`;
        clearInterval(countdown);
      }

      if (lives <= 0) {
        document.getElementById("message").textContent = `💀 Game over! The word was: ${selectedWord}`;
        clearInterval(countdown);
      }
    }

    function updateScore() {
      document.getElementById("score").textContent = `Score: ${score} | Lives: ${lives}`;
    }

    newWord();
  </script>

</body>
</html># Daily-routines
&lt;!DOCTYPE html> &lt;html lang="en"> &lt;head>   &lt;meta charset="UTF-8">   &lt;meta name="viewport" content="width=device-width, initial-scale=1.0">   &lt;title>Daily Routine Word Guess Game&lt;/title>   &lt;style>     body {       font-family: "Poppins", sans-serif;       background: linear-gradient(135deg, 
