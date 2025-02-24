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
        <th>User Name</th>
        <th>Number of Wins</th>
      </tr>
    </thead>
    <tbody>
      <!-- data inserted here -->
    </tbody>
  </table>

  <h2>Add New Game</h2>
  <form id="addGameForm">
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

    // Function to get the currently logged-in user
    async function getUser() {
        try {
            const response = await fetch(`${pythonURI}/api/user`, fetchOptions);
            if (!response.ok) throw new Error('Failed to fetch user.');

            const user = await response.json();
            console.log("Logged-in user data:", user);
            return user['id']; // Return the user ID
        } catch (error) {
            console.error('Error fetching user:', error);
        }
    }

    // Fetch and display past games
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
                    <td>${game.user_id}</td>
                    <td>${game.user_name}</td>
                    <td>${game.number_of_wins}</td>
                    <td>${game.number_of_losses}</td>
                    <td>
                        <button onclick="editRow(${game.id}, '${game.user_id}', '${game.user_name}', '${game.number_of_wins}', '${game.number_of_losses}')">Edit</button>
                        <button onclick="deletePastGame(${game.id}, '${game.user_id}')">Delete</button>
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
        event.preventDefault();
        const user_id = await getUser();
        const user_name = document.getElementById("winner").value; // Assuming "winner" field is used for user_name
        const number_of_wins = document.getElementById("elo").value; // Assuming "elo" is number_of_wins
        const number_of_losses = 0; // Default value for new entries

        const newGame = { user_id, user_name, number_of_wins, number_of_losses };

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
        const user_id = document.getElementById("editUid").value;
        const user_name = document.getElementById("editWinner").value;
        const number_of_wins = document.getElementById("editElo").value;
        const number_of_losses = 0; // Assuming edit doesn't include losses

        const updatedGame = { id, user_id, user_name, number_of_wins, number_of_losses };

        try {
            const loggedInUserId = await getUser();
            if (loggedInUserId !== parseInt(user_id)) {
                alert("You can only edit your own games.");
                return;
            }

            const options = { ...fetchOptions };
            options.method = "PUT";
            options.body = JSON.stringify(updatedGame);

            const response = await fetch(`${pythonURI}/api/pastgame`, options);

            if (response.ok) {
                document.getElementById("editGameForm").style.display = "none";
                fetchPastGames();
            } else {
                const errorData = await response.json();
                console.error("Error updating game:", errorData.message);
            }
        } catch (error) {
            console.error("Error updating game:", error);
        }
    }

    // Delete a past game
    async function deletePastGame(id, user_id) {
        try {
            const loggedInUserId = await getUser();
            if (loggedInUserId !== parseInt(user_id)) {
                alert("You can only delete your own games.");
                return;
            }

            const options = { ...fetchOptions };
            options.method = "DELETE";
            options.body = JSON.stringify({ id }); // Pass the game ID to delete

            const response = await fetch(`${pythonURI}/api/pastgame`, options);

            if (response.ok) {
                alert("Game deleted successfully!");
                fetchPastGames();
            } else {
                const errorData = await response.json();
                alert(`Error: ${errorData.message}`);
            }
        } catch (error) {
            alert("An unexpected error occurred. Please try again.");
        }
    }

    // Function to automatically update a game
    async function automaticUpdate() {
        try {
            const options = { ...fetchOptions };
            options.method = "GET";
            // Fetch all past games
            const response = await fetch(`${pythonURI}/api/pastgame`, options);
            if (!response.ok) throw new Error('Failed to fetch past games.');
            const data = await response.json();
            console.log(data);
            // Select the first game (or modify logic as needed)
            if (data.length > 0) {
                const userId = await getUser();
                const userGame = data.find(game => game.user_id === userId); // Filter games by user ID

                const updatedWins = userGame.number_of_wins + 1;                // Prepare the updated game data
                const updatedGame = {
                    id: userGame.id,
                    user_id: userGame.user_id,
                    number_of_wins: updatedWins,
                    number_of_losses: userGame.number_of_losses,
                };
                // Send the PUT request to update the game
                const putOptions = { ...fetchOptions };
                putOptions.method = "PUT";
                putOptions.body = JSON.stringify(updatedGame);
                const putResponse = await fetch(`${pythonURI}/api/pastgame`, putOptions);
                if (putResponse.ok) {
                    console.log(`Game ID ${userGame.id} updated: Wins incremented to ${updatedWins}`);
                    fetchPastGames(); // Refresh the table to reflect the update
                } else {
                    const errorData = await putResponse.json();
                    console.error("Error updating game:", errorData.message);
                }
            } else {
                console.log("No games found to update.");
            }
        } catch (error) {
            console.error("Error in automaticUpdate:", error);
        }
    }

    // Call the automaticUpdate function on page load
         

    // Fetch and display the games on page load
    fetchPastGames();

    // Function to edit a row
    function editRow(id, user_id, user_name, number_of_wins, number_of_losses) {
        document.getElementById("editId").value = id;
        document.getElementById("editUid").value = user_id;
        document.getElementById("editWinner").value = user_name;
        document.getElementById("editElo").value = number_of_wins;

        document.getElementById("editGameForm").style.display = "block";
    }

    // Attach global functions to the window object for onclick handlers
    window.editRow = editRow;
    window.deletePastGame = deletePastGame;

    // Attach event listeners
    document.getElementById("addGameForm").addEventListener("submit", addPastGame);
    document.getElementById("editGameForm").addEventListener("submit", updatePastGame);

    // Initial fetch
    fetchPastGames();

</script>
