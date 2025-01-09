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

        .runners-up {
            margin-top: 50px;
        }

        table {
            width: 80%;
            margin: 0 auto;
            border-collapse: collapse;
            background-color: white;
            text-align: left;
        }

        th, td {
            padding: 10px;
            border: 1px solid #ddd;
        }

        th {
            background-color: #222;
            color: white;
        }
    </style>
</head>
<body>
    <h1>Weekly Chess Leaderboard</h1>
    <div class="leaderboard" id="leaderboard"></div>
    <div class="runners-up">
        <h2>Runners-Up</h2>
        <table>
            <thead>
                <tr>
                    <th>Rank</th>
                    <th>Player</th>
                    <th>Wins</th>
                </tr>
            </thead>
            <tbody id="runners-up-table"></tbody>
        </table>
    </div>

    <script>
        // Static array of player data
        const players = [
            { name: "Player 1", wins: 15 },
            { name: "Player 2", wins: 12 },
            { name: "Player 3", wins: 10 },
            { name: "Player 4", wins: 9 },
            { name: "Player 5", wins: 8 },
            { name: "Player 6", wins: 7 },
            { name: "Player 7", wins: 6 },
            { name: "Player 8", wins: 5 },
            { name: "Player 9", wins: 4 },
            { name: "Player 10", wins: 3 }
        ];

        function updateLeaderboard() {
            const leaderboard = document.getElementById('leaderboard');
            const runnersUpTable = document.getElementById('runners-up-table');
            leaderboard.innerHTML = ''; // Clear existing content
            runnersUpTable.innerHTML = ''; // Clear existing table content

            // Populate the top 3 leaderboard
            players.slice(0, 3).forEach((player, index) => {
                const div = document.createElement('div');
                div.classList.add('player');
                div.innerHTML = `
                    <div class="star">★</div>
                    <span class="wins">${player.wins} Wins</span>
                    <div class="name">${player.name}</div>
                `;
                leaderboard.appendChild(div);
            });

            // Populate the runners-up table (4th to 10th place)
            players.slice(3, 10).forEach((player, index) => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${index + 4}</td>
                    <td>${player.name}</td>
                    <td>${player.wins}</td>
                `;
                runnersUpTable.appendChild(tr);
            });
        }

        // Initial update of the leaderboard and runners-up table
        updateLeaderboard();
    </script>

</body>
</html>
