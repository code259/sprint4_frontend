---
layout: base
title: Analysis
search_exclude: true
permalink: /analysis/
---

<link rel="stylesheet"
			href="https://unpkg.com/@chrisoakman/chessboardjs@1.0.0/dist/chessboard-1.0.0.min.css"
			integrity="sha384-q94+BZtLrkL1/ohfjR8c6L+A6qzNH9R2hBLwyoAfu3i/WCvQjzL2RQJ3uNHDISdU"
			crossorigin="anonymous">

<h1 style="text-align:center;">Chess Game Analysis</h1>
<div style="text-align:center; margin-bottom: 10px;">
    <input type="file" id="file-input" accept=".pgn" />
</div>
<div class="container">
    <div id="board-container">
        <div id="board" style="width: 400px; margin: auto;"></div>
        <div id="status" style="text-align:center; margin-top: 10px;">Load a PGN file to analyze moves.</div>
    </div>
</div>

<!-- New Section for Moves and Played Moves List -->
<div id="moves-container" style="margin-top: 20px;">
    <h2>Moves</h2>
    <table id="moves-table" border="1" style="width: 100%; text-align: left;">
        <thead>
            <tr>
                <th>Move Number</th>
                <th>Move</th>
                <th>Evaluation</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <!-- Dynamically populated rows -->
        </tbody>
    </table>
</div>

<div id="add-move-container" style="margin-top: 20px;">
    <h2>Add a New Move</h2>
    <form id="add-move-form">
        <label for="move-number">Move Number:</label>
        <input type="number" id="move-number" name="move_number" required />
        <label for="move">Move:</label>
        <input type="text" id="move" name="move" required />
        <label for="evaluation">Evaluation:</label>
        <input type="number" step="0.1" id="evaluation" name="evaluation" required />
        <button type="submit">Add Move</button>
    </form>
</div>

<div id="played-moves-container" style="margin-top: 20px;">
    <h2>Played Moves</h2>
    <ul id="played-moves-list">
        <!-- Dynamically populated list -->
    </ul>
</div>
<!-- End New Section -->

<script src="https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js"></script>
<script src="https://code.jquery.com/jquery-3.5.1.min.js"
			integrity="sha384-ZvpUoO/+PpLXR1lu4jmpXWu80pZlYUAfxl5NsBMWOEPSjUn/6Z/hRTt8+pR6L4N2"
			crossorigin="anonymous"></script>
<script src="https://unpkg.com/@chrisoakman/chessboardjs@1.0.0/dist/chessboard-1.0.0.min.js"
			integrity="sha384-8Vi8VHwn3vjQ9eUHUxex3JSN/NFqUg3QbPyX8kWyb93+8AC/pPWTzj+nHtbC5bxD"
			crossorigin="anonymous"></script>

<script>
    const board = Chessboard('board', {
        position: 'start',
        draggable: false,
        pieceTheme: 'https://chessboardjs.com/img/chesspieces/wikipedia/{piece}.png',
    });

    const game = new Chess();
    let moveIndex = 0; // Tracks the current move in the PGN
    let moveHistory = [];

    const statusEl = document.getElementById('status');
    const fileInput = document.getElementById('file-input');

    // Handle PGN file upload
    fileInput.addEventListener('change', handleFileUpload);

    function handleFileUpload(event) {
        const file = event.target.files[0];
        const reader = new FileReader();

        reader.onload = function (e) {
            const pgn = e.target.result;

            if (game.load_pgn(pgn)) {
                moveHistory = game.history({ verbose: true });
                moveIndex = 0;
                updateBoard();
                statusEl.textContent = "Use Arrow Keys to navigate moves.";
            } else {
                alert("Invalid PGN file. Please upload a valid PGN file.");
            }
        };

        reader.readAsText(file);
    }

    // Function to update board based on moveIndex
    function updateBoard() {
        game.reset();
        for (let i = 0; i < moveIndex; i++) {
            game.move(moveHistory[i]);
        }
        board.position(game.fen());

        // Fetch the evaluation of the current position
        fetchMoveEvaluation(game.fen(), lastEvaluation)
            .then(data => {
                if (data) {
                    // Update the status element with the evaluation and status
                    statusEl.textContent = `Evaluation: ${data.evaluation}, Status: ${data.status}, Best Move: ${data.best_move}`;
                    // Store the current evaluation for the next move
                    lastEvaluation = data.evaluation;
                }
            })
            .catch(error => {
                console.error("Failed to fetch move evaluation:", error);
            });
    }

    const pythonURI = 'http://127.0.0.1:8887/api/evaluation'; // Replace with your backend API URL

    // New Variables and Functions for Moves and Played Moves
    const movesTableBody = document.querySelector('#moves-table tbody');
    const playedMovesList = document.querySelector('#played-moves-list');
    const addMoveForm = document.querySelector('#add-move-form');

    // Fetch all moves from the backend
    async function fetchMoves() {
        try {
            const response = await fetch(pythonURI);
            const moves = await response.json();
            populateMovesTable(moves);
        } catch (error) {
            console.error('Error fetching moves:', error);
        }
    }

    // Populate moves table
    function populateMovesTable(moves) {
        movesTableBody.innerHTML = '';
        moves.forEach(move => {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${move.move_number}</td>
                <td>${move.move}</td>
                <td>${move.evaluation}</td>
                <td>
                    <button onclick="markAsPlayed(${move.id}, '${move.move}')">Played</button>
                    <button onclick="removeMove(${move.id})">Remove</button>
                </td>
            `;
            movesTableBody.appendChild(row);
        });
    }

    // Add new move
    addMoveForm.addEventListener('submit', async (event) => {
        event.preventDefault();
        const moveNumber = document.querySelector('#move-number').value;
        const move = document.querySelector('#move').value;
        const evaluation = document.querySelector('#evaluation').value;

        try {
            const response = await fetch(pythonURI, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    move_number: parseInt(moveNumber, 10),
                    move: move,
                    evaluation: parseFloat(evaluation)
                })
            });

            if (response.ok) {
                fetchMoves(); // Refresh the table
                addMoveForm.reset();
            } else {
                console.error('Failed to add move.');
            }
        } catch (error) {
            console.error('Error adding move:', error);
        }
    });

    // Mark move as played
    async function markAsPlayed(id, move) {
        try {
            const response = await fetch(pythonURI, {
                method: 'PUT',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ id: id, played: true }),
            });

            if (response.ok) {
                const playedMove = document.createElement('li');
                playedMove.textContent = move;
                playedMovesList.appendChild(playedMove);
                fetchMoves(); // Refresh the table
            } else {
                console.error('Failed to mark move as played.');
            }
        } catch (error) {
            console.error('Error marking move as played:', error);
        }
    }

    // Remove move
    async function removeMove(id) {
        try {
            const response = await fetch(pythonURI, {
                method: 'DELETE',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ id: id }),
            });

            if (response.ok) {
                fetchMoves(); // Refresh the table
            } else {
                console.error('Failed to remove move.');
            }
        } catch (error) {
            console.error('Error removing move:', error);
        }
    }

    // Variable to store the last evaluation
    let lastEvaluation = 0.0;

    // Handle Arrow Key Navigation
    document.addEventListener('keydown', function (e) {
        if (!moveHistory.length) return;

        if (e.key === 'ArrowRight') {
            if (moveIndex < moveHistory.length) moveIndex++;
            updateBoard();
        } else if (e.key === 'ArrowLeft') {
            if (moveIndex > 0) moveIndex--;
            updateBoard();
        }
    });

    // Initialize
    fetchMoves();
</script>
