---
layout: base
title: Game Manager
search_exclude: true
permalink: /manager/
---

<style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            padding: 20px;
        }
        .container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-top: 20px;
        }
        .card {
            background-color: #fff;
            color: black;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 20px;
            width: 300px;
            text-align: center;
        }
        .card h3 {
            margin-bottom: 10px;
        }
        .card p {
            margin-bottom: 20px;
            font-size: 14px;
        }
        .button {
            padding: 10px 20px;
            margin: 5px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .delete {
            background-color: red;
            color: white;
        }
        .update {
            background-color: blue;
            color: white;
        }
        .analyze {
            background-color: green;
            color: white;
        }
</style>

<h1>Chess Game Manager</h1>
<div class="container" id="cards-container"></div>

<script type="module">
    import { pythonURI, fetchOptions } from '/sprint4_frontend/assets/js/api/config.js';

    window.onload = function() {
        fetchGames();
    };

    async function fetchGames(){
        try {
            const response = await fetch(`${pythonURI}/api/pgns`, fetchOptions);
            if (!response.ok) {
                throw new Error('Failed to fetch groups: ' + response.statusText);
            }

            const games = await response.json();
            const container = document.getElementById('cards-container');
            container.innerHTML = '';

            games.forEach(game => {
                const card = document.createElement('div');
                card.className = 'card';
                card.innerHTML = `
                    <h2>${game.name}</h2>
                    <h4>Played By: ${game.user_name}</h4>
                    <br>
                    <p>${game.pgn}</p>
                    <button class="button update">Update</button>
                    <button class="button delete">Delete</button>
                    <button class="button analyze">Analyze</button>
                    <button class="button download">Download PGN File</button>
                `;

                card.querySelector('.delete').addEventListener('click', () => deleteGame(game.id, game.user_name));
                card.querySelector('.update').addEventListener('click', () => updateGame(game.id, game.user_name));
                card.querySelector('.analyze').addEventListener('click', () => redirectToAnalyze());
                card.querySelector('.download').addEventListener('click', () => downloadFile(game.pgn));
                container.appendChild(card);
            });
        } catch (error) {
            console.error('Error fetching entries:', error);
        }
    }

    async function downloadFile(moves) {
        console.log("Downloading moves as PGN file!")
        const pgnContent = `[Event "Casual Game"]
[Site "Unknown"]
[Date "2025.02.23"]
[Round "1"]
[White "Player1"]
[Black "Player2"]
[Result "*"]

${moves}`;

        const blob = new Blob([pgnContent], { type: "text/plain" });
        const url = URL.createObjectURL(blob);
        const a = document.createElement("a");
        a.href = url;
        a.download = "game.pgn";
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
        URL.revokeObjectURL(url);
    }

    async function getUser() {
        try {
            const response = await fetch(`${pythonURI}/api/user`, fetchOptions);
            if (!response.ok) throw new Error('Failed to fetch user.');

            const user = await response.json();
            return user['name']
        } catch (error) {
            console.error('Error fetching skills:', error);
        }
    }

    async function deleteGame(id, name) {
        let user_name = await getUser();
        if (user_name.toLowerCase() !== name.toLowerCase()) {
            console.log('Logged in user cannot remove another users card')
            return
        }

        try {
            const response = await fetch(`${pythonURI}/api/pgn`, {
                method: 'DELETE',
                headers: {
                    'Accept': 'application/json',
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ id: id })
            });
            if (!response.ok) {
                throw new Error('Failed to delete: ' + response.statusText);
            }

            fetchGames();
        } catch (error) {
            console.error('Error deleting entry:', error);
        }
    }

    async function updateGame(id, name) {
        let user_name = await getUser();
        if (user_name.toLowerCase() !== name.toLowerCase()) {
            console.log('Logged in user cannot update another users card')
            return
        }

        const newName = prompt("Enter the new name for the game:");
        try {
            const response = await fetch(`${pythonURI}/api/pgn`, {
                method: 'PATCH',
                headers: {
                    'Accept': 'application/json',
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ id: id, name: newName })
            });
            if (!response.ok) {
                throw new Error('Failed to delete: ' + response.statusText);
            }

            fetchGames();
        } catch (error) {
            console.error('Error deleting entry:', error);
        }
    }

    function redirectToAnalyze() {
        window.location.href = `/sprint4_frontend/analysis`;
    }
</script>