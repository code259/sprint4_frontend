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
        // TODO: here need to call a function that runs a fetch request to analyze-move endpoint
        // on first move "last_evaluation" will be 0, else will be this will be stored in a variable to be sent
    }

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
</script>
