<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Math King Game 👑</title>
  <style>
    * {
      box-sizing: border-box;
    }
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      font-family: 'Segoe UI', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      background: linear-gradient(to right, #00c6ff, #0072ff);
      transition: background 0.5s ease;
    }
    .slide {
      display: none;
      width: 100%;
      max-width: 600px;
      text-align: center;
      padding: 30px;
      background: rgba(255, 255, 255, 0.95);
      border-radius: 16px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    }
    .active {
      display: block;
    }
    h1, h2, h3 {
      margin: 10px 0;
    }
    button {
      padding: 12px 24px;
      font-size: 16px;
      border-radius: 12px;
      background: #0077ff;
      color: #fff;
      border: none;
      cursor: pointer;
      margin: 10px;
    }
    button:hover {
      background: #005ccc;
    }
    input[type="number"], select {
      padding: 10px;
      font-size: 16px;
      border-radius: 10px;
      border: 2px solid #0077ff;
      margin: 10px 0;
      width: 80%;
    }
    label {
      display: block;
      margin-top: 15px;
      font-size: 16px;
    }
    #result, #celebration {
      font-size: 20px;
      margin-top: 10px;
      min-height: 30px;
    }
  </style>
</head>
<body>
  <div id="startScreen" class="slide active">
    <h1>👑 Math King</h1>
    <button onclick="startGame()">Start Game</button>
    <button onclick="showSlide('settingsScreen')">Settings ⚙️</button>
  </div>  <div id="settingsScreen" class="slide">
    <h2>⚙️ Settings</h2>
    <label for="themeSelect">Choose Theme:</label>
    <select id="themeSelect" onchange="changeTheme()">
      <option value="linear-gradient(to right, #00c6ff, #0072ff)">Blue Sky</option>
      <option value="linear-gradient(to right, #ff758c, #ff7eb3)">Pink Dream</option>
      <option value="linear-gradient(to right, #43e97b, #38f9d7)">Green Breeze</option>
      <option value="linear-gradient(to right, #e1eec3, #f05053)">Sunset</option>
      <option value="linear-gradient(to right, #ffecd2, #fcb69f)">Peach</option>
      <option value="linear-gradient(to right, #a1c4fd, #c2e9fb)">Ice Blue</option>
      <option value="linear-gradient(to right, #f7971e, #ffd200)">Sunny Day</option>
      <option value="linear-gradient(to right, #fdfbfb, #ebedee)">White Cloud</option>
      <option value="linear-gradient(to right, #c2e59c, #64b3f4)">Mint</option>
      <option value="linear-gradient(to right, #3a1c71, #d76d77, #ffaf7b)">Sunrise Mix</option>
      <!-- Add more gradients as needed -->
    </select><label for="modeSelect">Select Mode:</label>
<select id="modeSelect">
  <option value="add">Addition ➕</option>
  <option value="sub">Subtraction ➖</option>
  <option value="mul">Multiplication ✖️</option>
  <option value="div">Division ➗</option>
  <option value="all" selected>All</option>
</select>

<label><input type="checkbox" id="soundToggle" checked /> Sound ON</label>
<br>
<button onclick="showSlide('startScreen')">⬅ Back</button>

  </div>  <div id="gameScreen" class="slide">
    <h2 id="level">Level: 1 / 50</h2>
    <p id="lives">❤️❤️❤️</p>
    <h3 id="question">Loading...</h3>
    <input type="number" id="answer" placeholder="Enter answer">
    <br>
    <button onclick="checkAnswer()">Submit</button>
    <p id="result"></p>
    <p id="celebration"></p>
    <p id="score">Score: 0</p>
  </div>  <div id="gameOverScreen" class="slide">
    <h2>💔 Game Over</h2>
    <p style="font-size: 40px;">💔💔💔</p>
    <p>Your Score: <span id="finalScore">0</span></p>
    <button onclick="startGame()">Play Again</button>
    <button onclick="showSlide('startScreen')">Home</button>
  </div>  <div id="winScreen" class="slide">
    <h2>🎉 You Are the Math King! 👑</h2>
    <p>Final Score: <span id="winScore">0</span></p>
    <p>🏆 Level 50 Completed!</p>
    <p>🎊🎉💯🔥</p>
    <button onclick="startGame()">Play Again</button>
    <button onclick="showSlide('startScreen')">Home</button>
  </div>  <script>
    let level = 1, score = 0, lives = 3, num1, num2, operator;

    function showSlide(id) {
      document.querySelectorAll('.slide').forEach(s => s.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    function changeTheme() {
      const bg = document.getElementById('themeSelect').value;
      document.body.style.background = bg;
    }

    function playSound(type) {
      if (document.getElementById('soundToggle').checked) {
        let sound = new Audio(type === 'correct' ? 'https://cdn.pixabay.com/audio/2022/03/15/audio_eab6a6fc62.mp3' : 'https://cdn.pixabay.com/audio/2022/03/15/audio_34d2092f79.mp3');
        sound.play();
      }
    }

    function getOperator(mode) {
      const ops = ['+', '-', '*', '/'];
      return mode === 'all' ? ops[Math.floor(Math.random() * ops.length)] : mode === 'add' ? '+' : mode === 'sub' ? '-' : mode === 'mul' ? '*' : '/';
    }

    function generateQuestion() {
      const mode = document.getElementById('modeSelect').value;
      operator = getOperator(mode);
      let max = 10 + level * 2;
      num1 = Math.floor(Math.random() * max) + 1;
      num2 = Math.floor(Math.random() * max) + 1;
      if (operator === '/') {
        num1 = num1 * num2;
      }
      if (operator === '-' && num1 < num2) [num1, num2] = [num2, num1];
      document.getElementById('question').innerText = `What is ${num1} ${operator} ${num2}?`;
    }

    function checkAnswer() {
      const userAnswer = parseInt(document.getElementById('answer').value);
      let correct = operator === '+' ? num1 + num2 : operator === '-' ? num1 - num2 : operator === '*' ? num1 * num2 : num1 / num2;

      if (userAnswer === correct) {
        playSound('correct');
        document.getElementById('result').innerText = '✅ Correct!';
        document.getElementById('celebration').innerText = getCelebration();
        score += 10;
        level++;
        if (level > 50) {
          document.getElementById('winScore').innerText = score;
          showSlide('winScreen');
          return;
        }
      } else {
        playSound('wrong');
        document.getElementById('result').innerText = '❌ Wrong!';
        document.getElementById('celebration').innerText = '';
        lives--;
        if (lives === 0) {
          document.getElementById('finalScore').innerText = score;
          showSlide('gameOverScreen');
          return;
        }
      }
      updateGame();
    }

    function updateGame() {
      document.getElementById('level').innerText = `Level: ${level} / 50`;
      document.getElementById('score').innerText = `Score: ${score}`;
      document.getElementById('lives').innerText = '❤️'.repeat(lives) + '💔'.repeat(3 - lives);
      document.getElementById('answer').value = '';
      setTimeout(() => {
        document.getElementById('result').innerText = '';
        generateQuestion();
      }, 800);
    }

    function getCelebration() {
      const emojis = ['🎉', '🥳', '🤩', '😍', '👏', '🌟'];
      return emojis[Math.floor(Math.random() * emojis.length)];
    }

    function startGame() {
      score = 0; level = 1; lives = 3;
      showSlide('gameScreen');
      updateGame();
    }
  </script></body>
</html>
