<!-- HTML file (public/index.html) -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rock Paper Scissors Online</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Rock Paper Scissors</h1>
        <div class="game-area">
            <!-- Welcome screen -->
            <div id="welcome-screen">
                <h2>Play with friends worldwide!</h2>
                <div class="input-group">
                    <input type="text" id="username-input" placeholder="Your name" maxlength="15">
                </div>
                <div class="button-group">
                    <button id="create-btn" class="btn">Create Game</button>
                    <div class="or-divider">OR</div>
                    <div class="input-group">
                        <input type="text" id="room-input" placeholder="Room code" maxlength="6">
                        <button id="join-btn" class="btn">Join Game</button>
                    </div>
                </div>
            </div>

            <!-- Waiting screen -->
            <div id="waiting-screen" class="hidden">
                <h2>Waiting for opponent...</h2>
                <div class="room-info">
                    <p>Room Code: <span id="room-code" class="highlight"></span></p>
                    <p>Share this code with a friend to play!</p>
                </div>
            </div>

            <!-- Game screen -->
            <div id="game-screen" class="hidden">
                <div class="scoreboard">
                    <div id="player-score">You: <span>0</span></div>
                    <div id="vs">VS</div>
                    <div id="opponent-score">Opponent: <span>0</span></div>
                </div>
                
                <div id="status-message">Choose your move!</div>
                
                <div id="moves-container">
                    <button class="move-btn" data-move="rock">✊</button>
                    <button class="move-btn" data-move="paper">✋</button>
                    <button class="move-btn" data-move="scissors">✌️</button>
                </div>
                
                <div id="result-display" class="hidden">
                    <div class="moves-display">
                        <div class="move-display" id="player-move"></div>
                        <div class="versus">VS</div>
                        <div class="move-display" id="opponent-move"></div>
                    </div>
                    <div id="round-result"></div>
                    <button id="play-again-btn" class="btn">Play Again</button>
                </div>
            </div>
        </div>
        
        <div id="notification" class="hidden"></div>
    </div>

    <script src="/socket.io/socket.io.js"></script>
    <script src="script.js"></script>
</body>
</html>

/* CSS file (public/styles.css) */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Arial', sans-serif;
}

body {
    background-color: #f0f2f5;
    color: #333;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.container {
    width: 90%;
    max-width: 600px;
    padding: 20px;
}

h1 {
    text-align: center;
    margin-bottom: 30px;
    color: #2c3e50;
}

h2 {
    text-align: center;
    margin-bottom: 20px;
    color: #34495e;
}

.game-area {
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.input-group {
    margin-bottom: 15px;
}

input {
    width: 100%;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 5px;
    font-size: 16px;
    margin-bottom: 10px;
}

.btn {
    background-color: #3498db;
    color: white;
    border: none;
    padding: 12px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    width: 100%;
    transition: background-color 0.3s;
}

.btn:hover {
    background-color: #2980b9;
}

.or-divider {
    text-align: center;
    margin: 15px 0;
    color: #7f8c8d;
    position: relative;
}

.or-divider::before,
.or-divider::after {
    content: "";
    position: absolute;
    top: 50%;
    width: 45%;
    height: 1px;
    background-color: #ddd;
}

.or-divider::before {
    left: 0;
}

.or-divider::after {
    right: 0;
}

.highlight {
    font-weight: bold;
    color: #e74c3c;
    font-size: 1.2em;
}

.room-info {
    text-align: center;
    margin-top: 20px;
    padding: 15px;
    background-color: #f8f9fa;
    border-radius: 5px;
}

.scoreboard {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding: 10px;
    background-color: #f8f9fa;
    border-radius: 5px;
}

#vs {
    font-weight: bold;
    color: #7f8c8d;
}

#status-message {
    text-align: center;
    margin-bottom: 20px;
    font-size: 18px;
    font-weight: bold;
    color: #34495e;
}

#moves-container {
    display: flex;
    justify-content: space-around;
    margin-bottom: 30px;
}

.move-btn {
    width: 80px;
    height: 80px;
    font-size: 40px;
    border-radius: 50%;
    border: 1px solid #ddd;
    background-color: white;
    cursor: pointer;
    transition: transform 0.2s, background-color 0.2s;
}

.move-btn:hover {
    transform: scale(1.1);
    background-color: #f8f9fa;
}

.moves-display {
    display: flex;
    justify-content: space-around;
    align-items: center;
    margin-bottom: 20px;
}

.move-display {
    width: 100px;
    height: 100px;
    font-size: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f8f9fa;
    border-radius: 10px;
}

#round-result {
    text-align: center;
    margin-bottom: 20px;
    font-size: 18px;
    font-weight: bold;
}

#notification {
    position: fixed;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    padding: 15px 30px;
    background-color: #e74c3c;
    color: white;
    border-radius: 5px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    z-index: 100;
}

.versus {
    font-weight: bold;
    font-size: 20px;
    color: #7f8c8d;
}

.hidden {
    display: none;
}

@media (max-width: 500px) {
    .container {
        width: 95%;
        padding: 10px;
    }
    
    .game-area {
        padding: 20px;
    }
    
    .move-btn {
        width: 60px;
        height: 60px;
        font-size: 30px;
    }
    
    .move-display {
        width: 80px;
        height: 80px;
        font-size: 40px;
    }
}

/* JavaScript file (public/script.js) */
document.addEventListener('DOMContentLoaded', () => {
    // DOM Elements
    const welcomeScreen = document.getElementById('welcome-screen');
    const waitingScreen = document.getElementById('waiting-screen');
    const gameScreen = document.getElementById('game-screen');
    const usernameInput = document.getElementById('username-input');
    const roomInput = document.getElementById('room-input');
    const createBtn = document.getElementById('create-btn');
    const joinBtn = document.getElementById('join-btn');
    const roomCodeDisplay = document.getElementById('room-code');
    const movesContainer = document.getElementById('moves-container');
    const moveButtons = document.querySelectorAll('.move-btn');
    const statusMessage = document.getElementById('status-message');
    const resultDisplay = document.getElementById('result-display');
    const playerMoveDisplay = document.getElementById('player-move');
    const opponentMoveDisplay = document.getElementById('opponent-move');
    const roundResult = document.getElementById('round-result');
    const playAgainBtn = document.getElementById('play-again-btn');
    const playerScoreDisplay = document.getElementById('player-score').querySelector('span');
    const opponentScoreDisplay = document.getElementById('opponent-score').querySelector('span');
    const notification = document.getElementById('notification');

    // Game state
    let socket = io();
    let username = '';
    let roomId = '';
    let playerRole = null; // 'player1' or 'player2'
    let opponentUsername = '';

    // Move icons
    const moveIcons = {
        rock: '✊',
        paper: '✋',
        scissors: '✌️'
    };

    // Initialize the game
    function init() {
        // Setup event listeners
        createBtn.addEventListener('click', createRoom);
        joinBtn.addEventListener('click', joinRoom);
        moveButtons.forEach(button => {
            button.addEventListener('click', makeMove);
        });
        playAgainBtn.addEventListener('click', playAgain);

        // Socket event listeners
        socket.on('roomCreated', handleRoomCreated);
        socket.on('roomJoined', handleRoomJoined);
        socket.on('gameReady', handleGameReady);
        socket.on('opponentMoved', handleOpponentMoved);
        socket.on('roundResult', handleRoundResult);
        socket.on('gameReset', handleGameReset);
        socket.on('playerLeft', handlePlayerLeft);
        socket.on('error', handleError);
    }

    // Create a new game room
    function createRoom() {
        if (!validateUsername()) return;
        
        username = usernameInput.value.trim();
        socket.emit('createRoom', username);
    }

    // Join an existing game room
    function joinRoom() {
        if (!validateUsername()) return;
        if (!validateRoomCode()) return;
        
        username = usernameInput.value.trim();
        roomId = roomInput.value.trim().toUpperCase();
        socket.emit('joinRoom', roomId, username);
    }

    // Make a move
    function makeMove(e) {
        const move = e.target.getAttribute('data-move');
        socket.emit('makeMove', move);
        
        // Disable move buttons and update UI
        moveButtons.forEach(btn => btn.disabled = true);
        statusMessage.textContent = 'Waiting for opponent...';
    }

    // Play another round
    function playAgain() {
        socket.emit('playAgain');
    }

    // Handle room created event
    function handleRoomCreated(newRoomId, playerName) {
        roomId = newRoomId;
        username = playerName;
        playerRole = 'player1';
        
        roomCodeDisplay.textContent = roomId;
        showScreen(waitingScreen);
    }

    // Handle room joined event
    function handleRoomJoined(joinedRoomId, playerName) {
        roomId = joinedRoomId;
        username = playerName;
        playerRole = 'player2';
        
        roomCodeDisplay.textContent = roomId;
        showScreen(waitingScreen);
    }

    // Handle game ready event (both players joined)
    function handleGameReady(gameData) {
        // Set opponent username based on player role
        if (playerRole === 'player1') {
            opponentUsername = gameData.player2.username;
            document.getElementById('player-score').textContent = `You (${username}): `;
            document.getElementById('opponent-score').textContent = `${opponentUsername}: `;
        } else {
            opponentUsername = gameData.player1.username;
            document.getElementById('player-score').textContent = `You (${username}): `;
            document.getElementById('opponent-score').textContent = `${opponentUsername}: `;
        }
        
        showScreen(gameScreen);
        resetGameUI();
    }

    // Handle opponent moved event
    function handleOpponentMoved() {
        statusMessage.textContent = 'Opponent has made their move. Your turn!';
    }

    // Handle round result event
    function handleRoundResult(data) {
        const playerMove = playerRole === 'player1' 
            ? data.moves[Object.keys(data.moves)[0]] 
            : data.moves[Object.keys(data.moves)[1]];
            
        const opponentMove = playerRole === 'player1' 
            ? data.moves[Object.keys(data.moves)[1]] 
            : data.moves[Object.keys(data.moves)[0]];
        
        // Display the moves
        playerMoveDisplay.textContent = moveIcons[playerMove];
        opponentMoveDisplay.textContent = moveIcons[opponentMove];
        
        // Show the result
        if (data.winner === username) {
            roundResult.textContent = 'You win!';
            roundResult.style.color = '#27ae60';
        } else if (data.winner === null) {
            roundResult.textContent = 'It\'s a tie!';
            roundResult.style.color = '#7f8c8d';
        } else {
            roundResult.textContent = 'You lose!';
            roundResult.style.color = '#e74c3c';
        }
        
        // Update scores
        playerScoreDisplay.textContent = data.scores[username];
        opponentScoreDisplay.textContent = data.scores[opponentUsername];
        
        // Show result display
        movesContainer.style.display = 'none';
        statusMessage.style.display = 'none';
        resultDisplay.classList.remove('hidden');
    }

    // Handle game reset event
    function handleGameReset() {
        resetGameUI();
    }

    // Handle player left event
    function handlePlayerLeft(playerName) {
        showNotification(`${playerName} has left the game`);
        showScreen(welcomeScreen);
    }

    // Handle error event
    function handleError(errorMsg) {
        showNotification(errorMsg);
    }

    // Validate username input
    function validateUsername() {
        const username = usernameInput.value.trim();
        if (!username) {
            showNotification('Please enter your name');
            return false;
        }
        return true;
    }

    // Validate room code input
    function validateRoomCode() {
        const roomCode = roomInput.value.trim();
        if (!roomCode) {
            showNotification('Please enter a room code');
            return false;
        }
        return true;
    }

    // Show a specific screen
    function showScreen(screen) {
        welcomeScreen.classList.add('hidden');
        waitingScreen.classList.add('hidden');
        gameScreen.classList.add('hidden');
        
        screen.classList.remove('hidden');
    }

    // Reset game UI for a new round
    function resetGameUI() {
        // Reset move buttons
        moveButtons.forEach(btn => btn.disabled = false);
        
        // Reset status message
        statusMessage.textContent = 'Choose your move!';
        statusMessage.style.display = 'block';
        
        // Hide result display and show moves container
        resultDisplay.classList.add('hidden');
        movesContainer.style.display = 'flex';
    }

    // Show notification
    function showNotification(message) {
        notification.textContent = message;
        notification.classList.remove('hidden');
        
        setTimeout(() => {
            notification.classList.add('hidden');
        }, 3000);
    }

    // Start the game
    init();
});
