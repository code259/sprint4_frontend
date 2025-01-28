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
      <!-- Data will be dynamically inserted here -->
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


<script>
// URL of the backend API
const API_URL = "http://127.0.0.1:8887/api/pastgame";

// Fetch and display all past games
async function fetchPastGames() {
  try {
    const response = await fetch(API_URL);
    const data = await response.json();
    console.log(data);

    // Populate the table
    const tableBody = document.querySelector("#pastGamesTable tbody");
    tableBody.innerHTML = ""; // Clear existing rows
    data.forEach((game) => {
      const row = document.createElement("tr");
      row.innerHTML = `
        <td>${game.id}</td>
        <td>${game.uid}</td>
        <td>${game.winner}</td>
        <td>${game.elo}</td>
        <td>
          <button onclick="editRow(${game.id}, '${game.uid}', '${game.winner}', '${game.elo}')">Edit</button>
          <button onclick="deletePastGame(${game.id})">Delete</button>
        </td>
      `;
      tableBody.appendChild(row);
    });
  } catch (error) {
    console.error("Error fetching past games:", error);
  }
}


function editRow(id, uid, winner, elo) {
  // Populate the edit form with the current row's data
  document.getElementById("editId").value = id;
  document.getElementById("editUid").value = uid;
  document.getElementById("editWinner").value = winner;
  document.getElementById("editElo").value = elo;

  // Show the edit form
  document.getElementById("editGameForm").style.display = "block";
}

// Add a new game
async function addPastGame(event) {
  event.preventDefault(); // Prevent form submission

  // Get form values
  const uid = document.getElementById("uid").value;
  const winner = document.getElementById("winner").value;
  const elo = document.getElementById("elo").value;

  // Prepare the data
  const newGame = { uid, winner, elo };

  try {
    // Send POST request to the backend
    const response = await fetch(API_URL, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(newGame),
    });

    if (response.ok) {
      // Clear the form
      document.getElementById("addGameForm").reset();

      // Refresh the table
      fetchPastGames();
    } else {
      const errorData = await response.json();
      console.error("Error adding game:", errorData.message);
    }
  } catch (error) {
    console.error("Error adding game:", error);
  }
}

// Attach event listeners
document.getElementById("addGameForm").addEventListener("submit", addPastGame);
document.getElementById("editGameForm").addEventListener("submit", updatePastGame);

// Initial fetch
fetchPastGames();


async function updatePastGame(event) {
  event.preventDefault(); // Prevent the form from submitting normally

  // Get form values
  const id = document.getElementById("editId").value;
  const uid = document.getElementById("editUid").value;
  const winner = document.getElementById("editWinner").value;
  const elo = document.getElementById("editElo").value;

  // Prepare the data
  const updatedGame = { id, uid, winner, elo };

  try {
    // Send PUT request to the backend
    const response = await fetch(`${API_URL}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(updatedGame),
    });

    if (response.ok) {
      // Hide the edit form
      document.getElementById("editGameForm").style.display = "none";

      // Refresh the table to show the updated data
      fetchPastGames();
    } else {
      const errorData = await response.json();
      console.error("Error updating game:", errorData.message);
    }
  } catch (error) {
    console.error("Error updating game:", error);
  }
}



async function deletePastGame(uid) {
  try {
    // Send a POST request with "_method": "DELETE" in the body
    const response = await fetch(API_URL, {
      method: "POST", // Use POST instead of DELETE
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        uid: uid,
        _method: "DELETE", // Indicate the intended method
      }),
    });

    if (response.ok) {
      alert("Game Log deleted successfully!");
      // Refresh the table or UI after deletion
      fetchPastGames();
    } else {
      const errorData = await response.json();
      console.error("Error deleting game log:", errorData.message);
      alert(`Error: ${errorData.message}`);
    }
  } catch (error) {
    console.error("An error occurred while deleting the game log:", error);
    alert("An unexpected error occurred. Please try again.");
  }
}




</script>
