---
layout: base
title: 
search_exclude: true
permalink: /pastGame/
---


  
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 20px;
    }
    table, th, td {
      border: 1px solid #ccc;
    }
    th, td {
      padding: 10px;
      text-align: left;
    }
    th {
      background-color: #f4f4f4;
    }
    form {
      display: flex;
      flex-direction: column;
      gap: 10px;
      max-width: 300px;
    }
    input, button {
      padding: 10px;
      font-size: 1rem;
    }
  </style>

<body>
  <h1>Chess Past Games</h1>
  <table id="pastGamesTable">
    <thead>
      <tr>
        <th>ID</th>
        <th>User ID</th>
        <th>Winner</th>
        <th>ELO</th>
      </tr>
    </thead>
    <tbody>
      <!-- data inserted here -->
    </tbody>
  </table>

  <h2>Add New Game</h2>
  <form id="addGameForm">
    <input type="text" id="uid" placeholder="User ID" required />
    <input type="text" id="winner" placeholder="Winner" required />
    <input type="text" id="elo" placeholder="ELO" required />
    <button type="submit">Add Game</button>
  </form>
</body>

<h2>Edit Game</h2>
<form id="editGameForm" style="display: none;">
  <input type="hidden" id="editId" />
  <input type="text" id="editUid" placeholder="User ID" required />
  <input type="text" id="editWinner" placeholder="Winner" required />
  <input type="text" id="editElo" placeholder="ELO" required />
  <button type="submit">Update Game</button>
</form>


<script type="module">
    import { pythonURI, fetchOptions } from '/sprint4_frontend/assets/js/api/config.js';

    async function fetchPastGames() {
        try {
            const options = { ...fetchOptions };
            options.method = "GET"; 
            const response = await fetch(`${pythonURI}/api/pastgame`, options);

            if (!response.ok) {
                throw new Error('Failed to fetch past games: ' + response.statusText);
            }

            const data = await response.json();
            console.log(data);

            const tableBody = document.querySelector("#pastGamesTable tbody");
            tableBody.innerHTML = ""; // Clear the existing rows
            data.forEach((game) => {
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td>${game.id}</td>
                    <td>${game.uid}</td>
                    <td>${game.winner}</td>
                    <td>${game.elo}</td>
                    <td>
                        <button onclick="editRow(${game.id}, '${game.uid}', '${game.winner}', '${game.elo}')">Edit</button>
                        <button onclick="deletePastGame('${game.uid}')">Delete</button>
                    </td>
                `;
                tableBody.appendChild(row);
            });
        } catch (error) {
            console.error("Error fetching past games:", error);
        }
    }

    // Add a new game
    async function addPastGame(event) {
        event.preventDefault(); // this prevents form submission

        const uid = document.getElementById("uid").value;
        const winner = document.getElementById("winner").value;
        const elo = document.getElementById("elo").value;

        const newGame = { uid, winner, elo };

        try {
            const options = { ...fetchOptions };
            options.method = "POST"; 
            options.body = JSON.stringify(newGame); 

            const response = await fetch(`${pythonURI}/api/pastgame`, options);

            if (response.ok) {
                
                document.getElementById("addGameForm").reset();

                
                fetchPastGames();
            } else {
                const errorData = await response.json();
                console.error("Error adding game:", errorData.message);
            }
        } catch (error) {
            console.error("Error adding game:", error);
        }
    }

    // Update a game
    async function updatePastGame(event) {
        event.preventDefault(); 
        
        const id = document.getElementById("editId").value;
        const uid = document.getElementById("editUid").value;
        const winner = document.getElementById("editWinner").value;
        const elo = document.getElementById("editElo").value;

        const updatedGame = { id, uid, winner, elo };

        try {
            const options = { ...fetchOptions };
            options.method = "PUT"; 
            options.body = JSON.stringify(updatedGame); 

            const response = await fetch(`${pythonURI}/api/pastgame`, options);

            if (response.ok) {
                
                document.getElementById("editGameForm").style.display = "none";

                // refresh the datatable to show the updated data
                fetchPastGames();
            } else {
                const errorData = await response.json();
                console.error("Error updating game:", errorData.message);
            }
        } catch (error) {
            console.error("Error updating game:", error);
        }
    }

    // Delete a past game by UID
    async function deletePastGame(uid) {
        try {
            const options = { ...fetchOptions };
            options.method = "DELETE"; 
            options.body = JSON.stringify({ uid: uid }); // Add the UID to the request body

            const response = await fetch(`${pythonURI}/api/pastgame`, options);

            if (response.ok) {
                alert("Game Log deleted successfully!");
                fetchPastGames(); // refresh the table after deletion
            } else {
                const errorData = await response.json();
                alert(`Error: ${errorData.message}`);
            }
        } catch (error) {
            alert("An unexpected error occurred. Please try again.");
        }
    }

    // Function to edit a row
    function editRow(id, uid, winner, elo) {
        // Populate the edit form with the current row's data
        document.getElementById("editId").value = id;
        document.getElementById("editUid").value = uid;
        document.getElementById("editWinner").value = winner;
        document.getElementById("editElo").value = elo;

        // Showing the user the edit form
        document.getElementById("editGameForm").style.display = "block";
    }

    // global window object to make the onclick handlers access these functions globally
    window.editRow = editRow;
    window.deletePastGame = deletePastGame;

    // Attaching event listeners
    document.getElementById("addGameForm").addEventListener("submit", addPastGame);
    document.getElementById("editGameForm").addEventListener("submit", updatePastGame);

    // Initial fetch
    fetchPastGames();
</script>
