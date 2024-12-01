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
    #board {
      margin: 20px auto;
    }
    #status {
      text-align: center;
      margin: 10px 0;
    }
</style>

<h1 style="text-align:center;">Chess Practice</h1>
<div id="board" style="width: 400px"></div>
<div id="status">Your move</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js"></script>
<script src="https://code.jquery.com/jquery-3.5.1.min.js"
        integrity="sha384-ZvpUoO/+PpLXR1lu4jmpXWu80pZlYUAfxl5NsBMWOEPSjUn/6Z/hRTt8+pR6L4N2"
        crossorigin="anonymous"></script>

<script src="https://unpkg.com/@chrisoakman/chessboardjs@1.0.0/dist/chessboard-1.0.0.min.js"
        integrity="sha384-8Vi8VHwn3vjQ9eUHUxex3JSN/NFqUg3QbPyX8kWyb93+8AC/pPWTzj+nHtbC5bxD"
        crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/chessboard.js/1.0.0/chessboard.min.js"></script>
<!-- <script src="https://unpkg.com/stockfish/stockfish.js"></script> -->
<script src="https://code259.github.io/sprint4_frontend/assets/js/stockfish-16.1.js"></script>

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
    const stockfish = new Worker('https://code259.github.io/sprint4_frontend/assets/js/stockfish-16.1.js');
    const statusEl = document.getElementById('status');

    function handleMove(source, target, piece, newPos, oldPos, orientation) {
        var move = game.move({
            from: source,
            to: target,
            promotion: 'q' // always promotes to a queen for simplicity
        })

        // illegal move
        if (move === null) return 'snapback'

        window.setTimeout(makeAIMove, 250)
        // window.setTimeout(makeAIMove, 250)
        // updateStatus();
        // setTimeout(makeAIMove, 250);
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
        game.move(possibleMoves[randomIdx])
        board.position(game.fen())
    }

    function makeAIMove() {
        var possibleMoves = game.moves()
        if (possibleMoves.length === 0) {
            statusEl.textContent = 'Game over!';
            return
        }

        stockfish.postMessage(`position fen ${game.fen()}`);
        stockfish.postMessage('go depth 15');

        stockfish.onmessage = function (message) {
            console.log(message)
            if (message.startsWith('bestmove')) {
                const move = message.split(' ')[1];
                game.move({
                    from: move.slice(0, 2),
                    to: move.slice(2, 4),
                    promotion: 'q'
                });
                board.position(game.fen());
                // updateStatus();
            }
        };
    }

    // function updateStatus() {
    //     if (game.game_over()) {
    //         statusEl.textContent = 'Game over!';
    //         return;
    //     }
    //     statusEl.textContent = `Turn: ${game.turn() === 'w' ? 'White' : 'Black'}`;
    // }
</script>