# SCT_WD_03
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Tic-Tac-Toe Game</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      background: linear-gradient(to right, #ece9e6, #ffffff);
      height: 100vh;
      margin: 0;
      justify-content: center;
    }

    h1 {
      margin-bottom: 10px;
      font-size: 32px;
      color: #333;
    }

    #game-board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      gap: 5px;
    }

    .cell {
      width: 100px;
      height: 100px;
      background-color: #f1f1f1;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2.5em;
      cursor: pointer;
      border-radius: 10px;
      transition: 0.3s;
    }

    .cell:hover {
      background-color: #dcdcdc;
    }

    #status {
      margin-top: 15px;
      font-size: 20px;
    }

    #controls {
      margin-top: 20px;
    }

    button {
      padding: 10px 15px;
      margin: 5px;
      font-size: 16px;
      cursor: pointer;
      border-radius: 5px;
      border: none;
      background-color: #007bff;
      color: white;
    }

    button:hover {
      background-color: #0056b3;
    }

    select {
      padding: 8px;
      font-size: 16px;
    }
  </style>
</head>
<body>

  <h1>Tic-Tac-Toe</h1>

  <div id="controls">
    <select id="mode">
      <option value="player">Player vs Player</option>
      <option value="computer">Player vs Computer</option>
    </select>
    <button onclick="restartGame()">Restart</button>
  </div>

  <div id="game-board"></div>

  <div id="status"></div>

  <script>
    const boardEl = document.getElementById('game-board');
    const statusEl = document.getElementById('status');
    const modeSelect = document.getElementById('mode');

    let board = Array(9).fill("");
    let currentPlayer = "X";
    let gameActive = true;

    function createBoard() {
      boardEl.innerHTML = "";
      board.forEach((_, index) => {
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.dataset.index = index;
        cell.addEventListener('click', handleCellClick);
        boardEl.appendChild(cell);
      });
      updateStatus();
    }

    function handleCellClick(e) {
      const index = e.target.dataset.index;
      if (!gameActive || board[index] !== "") return;

      board[index] = currentPlayer;
      e.target.textContent = currentPlayer;

      if (checkWin(currentPlayer)) {
        statusEl.textContent = `${currentPlayer} wins!`;
        gameActive = false;
        return;
      }

      if (!board.includes("")) {
        statusEl.textContent = "It's a draw!";
        gameActive = false;
        return;
      }

      currentPlayer = currentPlayer === "X" ? "O" : "X";
      updateStatus();

      if (modeSelect.value === "computer" && currentPlayer === "O") {
        setTimeout(computerMove, 500);
      }
    }

    function updateStatus() {
      if (gameActive) {
        statusEl.textContent = `Current Player: ${currentPlayer}`;
      }
    }

    function checkWin(player) {
      const winPatterns = [
        [0,1,2],[3,4,5],[6,7,8], // rows
        [0,3,6],[1,4,7],[2,5,8], // cols
        [0,4,8],[2,4,6]          // diags
      ];

      return winPatterns.some(pattern => 
        pattern.every(index => board[index] === player)
      );
    }

    function computerMove() {
      // Simple AI: pick first empty spot
      const emptyIndices = board.map((val, i) => val === "" ? i : null).filter(i => i !== null);
      if (emptyIndices.length === 0) return;

      const move = emptyIndices[Math.floor(Math.random() * emptyIndices.length)];
      board[move] = "O";
      boardEl.children[move].textContent = "O";

      if (checkWin("O")) {
        statusEl.textContent = `O wins!`;
        gameActive = false;
        return;
      }

      if (!board.includes("")) {
        statusEl.textContent = "It's a draw!";
        gameActive = false;
        return;
      }

      currentPlayer = "X";
      updateStatus();
    }

    function restartGame() {
      board = Array(9).fill("");
      currentPlayer = "X";
      gameActive = true;
      createBoard();
    }

    createBoard();
  </script>

</body>
</html>
