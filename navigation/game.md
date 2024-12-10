---
layout: base
title: Game
search_exclude: true
permalink: /game/
---

<link rel="stylesheet"
      href="https://unpkg.com/@chrisoakman/chessboardjs@1.0.0/dist/chessboard-1.0.0.min.css"
      integrity="sha384-q94+BZtLrkL1/ohfjR8c6L+A6qzNH9R2hBLwyoAfu3i/WCvQjzL2RQJ3uNHDISdU"
      crossorigin="anonymous">

<style>
    .container {
      display: flex;
      justify-content: center;
      align-items: flex-start;
      margin: 20px auto;
      max-width: 1200px;
    }
    #instructions {
      margin-right: 20px;
      max-width: 300px;
      padding: 20px;
      background: #f9f9f9;
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      flex-shrink: 0;
    }
    #instructions h2 {
      text-align: center;
      margin-bottom: 10px;
      color: #333;
    }
    #instructions ul {
      list-style: none;
      padding: 0;
    }
    #instructions ul li {
      margin-bottom: 10px;
      color: #555;
    }
    #instructions ul li b {
      color: #000;
    }
    #board-container {
      text-align: center;
      flex-grow: 1;
    }
    #board {
      margin: 0 auto;
    }
    #status {
      text-align: center;
      margin: 10px 0;
    }
</style>

<h1 style="text-align:center;">Chess Practice</h1>
<div class="container">
  <div id="instructions">
    <h2>Instructions</h2>
    <ul>
      <li><b>Move Pieces:</b> Drag your piece to the desired square.</li>
      <li><b>Follow Chess Rules:</b> Moves must adhere to standard chess rules.</li>
      <li><b>Game Objective:</b> Checkmate the computer’s king to win.</li>
      <li><b>Restart Game:</b> Refresh the page to reset the board.</li>
      <li><b>Tips:</b> Use strategy and think ahead!</li>
    </ul>
  </div>
  <div id="board-container">
    <div id="board" style="width: 400px"></div>
    <div id="status">Your move</div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js"></script>
<script src="https://code.jquery.com/jquery-3.5.1.min.js"
        integrity="sha384-ZvpUoO/+PpLXR1lu4jmpXWu80pZlYUAfxl5NsBMWOEPSjUn/6Z/hRTt8+pR6L4N2"
        crossorigin="anonymous"></script>

<script src="https://unpkg.com/@chrisoakman/chessboardjs@1.0.0/dist/chessboard-1.0.0.min.js"
        integrity="sha384-8Vi8VHwn3vjQ9eUHUxex3JSN/NFqUg3QbPyX8kWyb93+8AC/pPWTzj+nHtbC5bxD"
        crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/chessboard.js/1.0.0/chessboard.min.js"></script>

<script>
    const board = Chessboard('board', {
        draggable: true,
        position: 'start',
        onDragStart: onDragStart,
        pieceTheme: 'https://chessboardjs.com/img/chesspieces/wikipedia/{piece}.png',
        onDrop: handleMove,
        onSnapEnd: onSnapEnd
    });

    const game = new Chess();
    const statusEl = document.getElementById('status');

    function handleMove(source, target, piece, newPos, oldPos, orientation) {
        var move = game.move({
            from: source,
            to: target,
            promotion: 'q' // always promotes to a queen for simplicity
        })

        // illegal move
        if (move === null) return 'snapback'

        window.setTimeout(makeRandomMove, 250)
    }

    function onDragStart (source, piece, position, orientation) {
        if (game.game_over()) return false
        if (piece.search(/^b/) !== -1) return false
    }

    function onSnapEnd () {
        board.position(game.fen())
    }

    function makeRandomMove () {
        var possibleMoves = game.moves()

        // game over
        if (possibleMoves.length === 0) {
            statusEl.textContent = 'Game over!';
            return
        }

        var randomIdx = Math.floor(Math.random() * possibleMoves.length)
        console.log(possibleMoves[randomIdx])
        game.move(possibleMoves[randomIdx])
        board.position(game.fen())
    }
</script>
