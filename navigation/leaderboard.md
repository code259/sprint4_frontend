---
layout: base 
title: Leaderboard
search_exclude: true
permalink: /leaderboard/
---


<style>
    .leaderboard {
        max-width: 600px;
        margin: auto;
        text-align: center;
        font-family: Arial, sans-serif;
        background: #1c1c1c;
        color: white;
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
    }

    h2 {
        margin-bottom: 20px;
        color: #4db8ff;
    }

    .podium {
        display: flex;
        justify-content: center;
        gap: 10px;
        margin-bottom: 20px;
    }

    .podium div {
        background: #333;
        padding: 15px;
        border-radius: 10px;
        text-align: center;
    }

    .first { background: gold; color: black; }
    .second { background: silver; }
    .third { background: #cd7f32; }

    table {
        width: 100%;
        border-collapse: collapse;
        background: #252525;
        color: white;
    }

    th, td {
        padding: 10px;
        border-bottom: 1px solid #444;
    }

    th {
        background: #4db8ff;
        color: black;
    }

    tr:hover {
        background: #333;
    }
</style>

<div class="leaderboard">
    <h2>Most Active Users!</h2>
    <div class="podium">
        <div class="second place">
            <p>2</p>
            <div class="avatar">🏆</div>
            <p id="second-name">Player B</p>
            <p id="second-score">1400</p>
        </div>
        <div class="first place">
            <p>1</p>
            <div class="avatar">🥇</div>
            <p id="first-name">Player A</p>
            <p id="first-score">1500</p>
        </div>
        <div class="third place">
            <p>3</p>
            <div class="avatar">🥉</div>
            <p id="third-name">Player C</p>
            <p id="third-score">1300</p>
        </div>
    </div>
    <table>
        <thead>
            <tr>
                <th>Rank</th>
                <th>Name</th>
                <th>Total Games</th>
            </tr>
        </thead>
        <tbody id="leaderboard-body">
        </tbody>
    </table>
</div>

<script type="module">
    import { pythonURI, fetchOptions } from '/sprint4_frontend/assets/js/api/config.js';

    async function fetchGames(){
        try {
            const response = await fetch(`${pythonURI}/api/pastgames`, fetchOptions);
            if (!response.ok) {
                throw new Error('Failed to fetch groups: ' + response.statusText);
            }

            const games = await response.json();
            const leaderboardBody = document.getElementById("leaderboard-body");
            leaderboardBody.innerHTML = "";

            games.sort((a, b) => (b.number_of_wins + b.number_of_losses) - (a.number_of_wins + a.number_of_losses));

            games.forEach((game, index) => {
                const score = game.number_of_wins + game.number_of_losses;
                const row = `<tr>
                    <td>${index + 1}</td>
                    <td>${game.user_name}</td>
                    <td>${score}</td>
                </tr>`;
                leaderboardBody.innerHTML += row;
            });

            if (games.length > 0) {
                document.getElementById("first-name").textContent = games[0].user_name;
                document.getElementById("first-score").textContent = games[0].number_of_wins + games[0].number_of_losses;
            }
            if (games.length > 1) {
                document.getElementById("second-name").textContent = games[1].user_name;
                document.getElementById("second-score").textContent = games[1].number_of_wins + games[1].number_of_losses;
            }
            if (games.length > 2) {
                document.getElementById("third-name").textContent = games[2].user_name;
                document.getElementById("third-score").textContent = games[2].number_of_wins + games[2].number_of_losses;
            }
        } catch (error) {
            console.error('Error fetching entries:', error);
        }
    }

    fetchGames();
</script>
