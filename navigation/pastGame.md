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

<script>
// URL of the backend API
const API_URL = "http://127.0.0.1:8887/api/pastgame";

// Fetch and display all past games
async function fetchPastGames() {
  try {
    const response = await fetch(API_URL);
    const data = await response.json();
    console.log(data)
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
      `;
      tableBody.appendChild(row);
    });
  } catch (error) {
    console.error("Error fetching past games:", error);
  }
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

// Initial fetch
fetchPastGames();

</script>
