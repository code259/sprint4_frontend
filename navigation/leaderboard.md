---
layout: post 
title: Leaderboard 
search_exclude: true
permalink: /leaderboard/
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weekly Chess Leaderboard</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            text-align: center;
            padding: 20px;
        }

        .leaderboard {
            display: flex;
            justify-content: center;
            align-items: flex-end;
            gap: 20px;
            margin-top: 50px;
        }

        .leaderboard .player {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-end;
            text-align: center;
            width: 80px;
            background-color: #222;
            color: white;
            border-radius: 10px;
            padding: 10px;
            position: relative;
        }

        .leaderboard .player .star {
            position: absolute;
            top: -30px;
            font-size: 24px;
            color: gold;
        }

        .leaderboard .player:nth-child(2) .star {
            color: silver;
        }

        .leaderboard .player:nth-child(3) .star {
            color: #cd7f32; /* Bronze */
        }

        .player span {
            font-size: 14px;
        }

        .player .wins {
            font-size: 18px;
            font-weight: bold;
        }

        .player .name {
            margin-top: 10px;
        }

        .leaderboard .player {
            height: 150px; /* Default height for third place */
        }

        .leaderboard .player:nth-child(1) {
            height: 200px; /* Height for first place */
        }

        .leaderboard .player:nth-child(2) {
            height: 170px; /* Height for second place */
        }
    </style>
</head>
<body>
    <h1>Weekly Chess Leaderboard</h1>
    <div class="leaderboard">
        <div class="player">
            <div class="star">★</div>
            <span class="wins">15 Wins</span>
            <div class="name">Player 1</div>
        </div>
        <div class="player">
            <div class="star">★</div>
            <span class="wins">12 Wins</span>
            <div class="name">Player 2</div>
        </div>
        <div class="player">
            <div class="star">★</div>
            <span class="wins">10 Wins</span>
            <div class="name">Player 3</div>
        </div>
    </div>
</body>
</html>
