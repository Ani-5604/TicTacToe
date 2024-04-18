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
<marquee behavior="scroll" direction="left">
    Tic-Tac-Toe
</marquee>
<div class="container">
    <h1>Tic-Tac-Toe</h1>
    <div id="board" class="board">
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
    const cellColors = ['#FF5733', '#33FF57', '#FFC300', '#900C3F', '#00CED1', '#FF6347', '#8A2BE2', '#3CB371', '#FF4500', '#FF5733', '#55FF57', '#FFC300', '#985D3F', '#00FFD1', '#AF6347', '#6A2BE2', '#3CB371', '#BF4569'];

    function handleMove(index) {
        let winnerName = currentPlayer === 'X' ? player1Name : player2Name;
        if (board[index] === '' && !checkWinner()) {
            board[index] = currentPlayer;
            renderBoard();
            changeCellColor(index); // Change color of the clicked cell

            if (checkWinner()) {
                showWinner(currentPlayer); // Show winner
                document.getElementById('message').innerText = `${currentPlayer} wins!`;
                document.getElementById('popup').style.display = 'block'; // Show the popup
            } else if (isBoardFull()) {
                document.getElementById('message').innerText = 'It\'s a draw!';
                document.getElementById('popup').style.display = 'block'; // Show the popup
            } else {
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            }
        }
    }

    function closePopup() {
        document.getElementById('popup').style.display = 'none';
    }

    function renderBoard() {
        const cells = document.getElementsByClassName('cell');
        for (let i = 0; i < cells.length; i++) {
            cells[i].innerText = board[i];
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
        document.getElementById('message').innerText = '';
        renderBoard();
        changeBoardColor(); // Change board color
        changeCellColors(); // Change cell colors
        resetWinnerBox(); // Reset winner box
        document.getElementById('popup').style.display = 'none'; // Hide the popup
    }

    function changeBoardColor() {
        const boardElement = document.getElementById('board');
        boardElement.style.backgroundColor = getRandomColor();
    }

    function changeCellColor(index) {
        const cell = document.getElementsByClassName('cell')[index];
        cell.style.backgroundColor = currentPlayer === 'X' ? 'green' : 'red'; // Change color based on currentPlayer
    }

    function changeCellColors() {
        const cells = document.getElementsByClassName('cell');
        for (let i = 0; i < cells.length; i++) {
            cells[i].style.backgroundColor = getRandomColor(); // Change color of each cell
        }
    }

    function showWinner(winner) {
        const winnerBox = document.getElementById('winnerBox');
        winnerBox.innerText = winner === 'X' ? 'X wins!' : 'O wins!';
        winnerBox.style.backgroundColor = winner === 'X' ? 'green' : 'red';
        winnerBox.style.display = 'block';
    }

    function resetWinnerBox() {
        const winnerBox = document.getElementById('winnerBox');
        winnerBox.innerText = '';
        winnerBox.style.backgroundColor = '';
        winnerBox.style.display = 'none';
    }

    function getRandomColor() {
        return cellColors[Math.floor(Math.random() * cellColors.length)];
    }
</script>
</body>
</html>
