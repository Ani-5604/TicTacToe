<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background: linear-gradient(90deg, #2CBE20, #4CAE20);
        }

        .container {
            text-align: center;
            margin-top: 50px;
        }

        h1 {
            color: white;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-gap: 5px;
            justify-content: center;
            background-color: #4CAE20;
            margin-bottom: 20px;
            text-align: center;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
        }

        .cell {
            width: 100px;
            height: 100px;
            border: 1px solid blue;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            background-color: #2CBE20;
            cursor: pointer;
            text-align: center;
            border-radius: 10px;
            transition: all 0.3s ease;
        }

        .cell:hover {
            transform: scale(1.1);
        }

        .message {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 10px;
            color: white;
        }

        #replayBtn {
            padding: 10px 20px;
            background-color: #9CBD80;
            color: blue;
            border: none;
            cursor: pointer;
            border-radius: 10px;
            transition: all 0.3s ease;
        }

        #replayBtn:hover {
            background-color: #8BD049;
            transform: scale(1.1);
        }

        .popup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #fff;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            z-index: 9999;
            animation: popupFade 0.5s ease;
        }

        @keyframes popupFade {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .closeBtn {
            margin-top: 10px;
            padding: 5px 10px;
            background-color: #9CBD80;
            color: blue;
            border: none;
            cursor: pointer;
            border-radius: 10px;
            transition: all 0.3s ease;
        }

        .closeBtn:hover {
            background-color: #8BD049;
            transform: scale(1.1);
        }

        .input-field {
            margin-bottom: 10px;
        }

        .options {
            margin-top: 20px;
        }
        
        .marquee {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            background-color: #204dae;
            color: white;
            font-size: 24px;
            padding: 10px;
            animation: marqueeAnimation 10s linear infinite;
        }

        @keyframes marqueeAnimation {
            from {
                transform: translateX(100%);
            }
            to {
                transform: translateX(-100%);
            }
        }

        .options button {
            padding: 10px 20px;
            margin: 0 10px;
            border-radius: 10px;
            border: none;
            cursor: pointer;
            transition: all 0.3s ease;
            background-color: #9CBD80;
            color: blue;
        }

        .options button:hover {
            background-color: #8BD049;
            transform: scale(1.1);
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        ul li {
            margin-bottom: 10px;
        }

        ul li a {
            color: white;
            text-decoration: none;
            transition: all 0.3s ease;
        }

        ul li a:hover {
            color: #8BD049;
        }

        .winner-box {
            width: 200px;
            height: 50px;
            margin: 20px auto;
            border-radius: 5px;
            font-size: 20px;
            font-weight: bold;
            color: white;
            text-align: center;
            line-height: 50px;
        }
    </style>
</head>
<body>
<marquee behavior="scroll" direction="right" class="marquee">
    Tic-Tac-Toe
</marquee>

<div class="container">
    <h1>Tic-Tac-Toe</h1>
    <div class="options">
        <button onclick="startGame('friend')">Play with Friend</button>
        <button onclick="startGame('computer')">Play with Computer</button>
    </div>
    <div id="playerNames" class="input-field" style="display: none;">
        <input type="text" id="player1Name" placeholder="Player 1 Name">
        <input type="text" id="player2Name" placeholder="Player 2 Name">
        <button onclick="startTwoPlayerGame()">Start Game</button>
    </div>
    <div id="board" class="board" style="display: none;">
        <div class="cell" onclick="handleMove(0)"></div>
        <div class="cell" onclick="handleMove(1)"></div>
        <div class="cell" onclick="handleMove(2)"></div>
        <div class="cell" onclick="handleMove(3)"></div>
        <div class="cell" onclick="handleMove(4)"></div>
        <div class="cell" onclick="handleMove(5)"></div>
        <div class="cell" onclick="handleMove(6)"></div>
        <div class="cell" onclick="handleMove(7)"></div>
        <div class="cell" onclick="handleMove(8)"></div>
    </div>
    <div id="message" class="message"></div>
    <button id="replayBtn" onclick="resetGame()">Replay</button>
    <div id="popup" class="popup">
        <span class="popup-content">Game Over!</span>
        <button class="closeBtn" onclick="closePopup()">Close</button>
    </div>
    <div id="winnerBox" class="winner-box"></div> <!-- Winner box -->
</div>

<script>
    let currentPlayer = 'X';
    let player1Name = '';
    let player2Name = '';
    let board = ['', '', '', '', '', '', '', '', ''];
    let gameMode = '';
    let gameOver = false;
    const cellColors = ['#FF5733', '#33FF57', '#FFC300', '#900C3F', '#00CED1', '#FF6347', '#8A2BE2', '#3CB371', '#FF4500'];

    function startGame(mode) {
        gameMode = mode;
        if (mode === 'friend') {
            document.getElementById('playerNames').style.display = 'block';
        } else {
            player1Name = prompt("Enter your name:", "You") || 'You';
            player2Name = 'Computer';
            document.getElementById('playerNames').style.display = 'none';
            document.getElementById('board').style.display = 'grid';
            renderBoard();
            if (currentPlayer === 'O') {
                setTimeout(computerMove, 1000); // Add delay before computer's move
            }
        }
    }

    function startTwoPlayerGame() {
        player1Name = document.getElementById('player1Name').value || 'Player 1';
        player2Name = document.getElementById('player2Name').value || 'Player 2';
        document.getElementById('playerNames').style.display = 'none';
        document.getElementById('board').style.display = 'grid';
        renderBoard();
    }

    function handleMove(index) {
        if (!gameOver && board[index] === '') {
            board[index] = currentPlayer;
            renderBoard();

            if (checkWinner()) {
                showWinner(currentPlayer);
                gameOver = true;
                return;
            } else if (isBoardFull()) {
                document.getElementById('message').innerText = 'It\'s a draw!';
                gameOver = true;
                showWinner('');
                return;
            }

            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            if (gameMode === 'computer' && currentPlayer === 'O' && !gameOver) {
                setTimeout(computerMove, 1000); // Add delay before computer's move
            }
        }
    }

    function computerMove() {
        let emptyCells = [];
        for (let i = 0; i < board.length; i++) {
            if (board[i] === '') {
                emptyCells.push(i);
            }
        }
        if (emptyCells.length > 0) {
            let randomIndex = Math.floor(Math.random() * emptyCells.length);
            let moveIndex = emptyCells[randomIndex];
            board[moveIndex] = 'O';
            renderBoard();

            if (checkWinner()) {
                showWinner('O');
                gameOver = true;
                return;
            } else if (isBoardFull()) {
                document.getElementById('message').innerText = 'It\'s a draw!';
                gameOver = true;
                showWinner('');
                return;
            }

            currentPlayer = 'X';
        }
    }

    function closePopup() {
        document.getElementById('popup').style.display = 'none';
    }

    function renderBoard() {
        const cells = document.getElementsByClassName('cell');
        for (let i = 0; i < cells.length; i++) {
            cells[i].innerText = board[i];
            if (board[i] === 'X') {
                cells[i].style.backgroundColor = 'green';
            } else if (board[i] === 'O') {
                cells[i].style.backgroundColor = 'red';
            } else {
                cells[i].style.backgroundColor = '#2CBE20';
            }
        }
    }

    function checkWinner() {
        const winningConditions = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6]
        ];
        for (let condition of winningConditions) {
            const [a, b, c] = condition;
            if (board[a] !== '' && board[a] === board[b] && board[b] === board[c]) {
                return true;
            }
        }
        return false;
    }

    function isBoardFull() {
        return board.every(cell => cell !== '');
    }

    function resetGame() {
        board = ['', '', '', '', '', '', '', '', ''];
        currentPlayer = 'X';
        gameOver = false;
        document.getElementById('message').innerText = '';
        renderBoard();
        document.getElementById('popup').style.display = 'none';
    }

    function showWinner(winner) {
        const winnerBox = document.getElementById('winnerBox');
        if (winner === 'X') {
            winnerBox.style.backgroundColor = 'green';
            winnerBox.innerText = player1Name + ' wins!';
        } else if (winner === 'O') {
            if (gameMode === 'computer') {
                winnerBox.style.backgroundColor = 'red';
                winnerBox.innerText = 'Computer wins!';
            } else {
                winnerBox.style.backgroundColor = 'red';
                winnerBox.innerText = player2Name + ' wins!';
            }
        } else if (winner === '') {
            winnerBox.style.backgroundColor = 'black';
            winnerBox.style.color = 'white';
            winnerBox.innerText = 'DRAW!';
        }
        winnerBox.style.display = 'block';
    }
</script>
</body>
</html>
