---
layout: base 
title: Admin Panel Update User Stats
search_exclude: false
permalink: /admin/
---


<style>
  .admin-panel {
    max-width: 600px;
    margin: auto;
    padding: 20px;
    font-family: Arial, sans-serif;
    background: #1c1c1c;
    color: white;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.5);
  }
  label, select, input, button {
    display: block;
    width: 100%;
    margin: 10px 0;
    font-size: 16px;
  }
  select, input {
    padding: 8px;
    border-radius: 4px;
    border: 1px solid #ccc;
  }
  button {
    padding: 10px;
    background: #4db8ff;
    border: none;
    border-radius: 5px;
    color: black;
    cursor: pointer;
  }
  button:hover {
    background: #39a6e0;
  }
</style>

<div class="admin-panel">
  <h1>Admin Panel: Update User Stats</h1>

  <!-- Dropdown for user selection -->
  <label for="userSelect">Select User:</label>
  <select id="userSelect">
    <option value="">-- Select a user --</option>
  </select>

  <!-- Form to update stats (always visible) -->
  <div id="userDetails">
    <h3 id="selectedUserHeader">Select a user to edit</h3>
    <label for="wins">New Wins:</label>
    <input type="number" id="wins" min="0" value="0">
    <label for="losses">New Losses:</label>
    <input type="number" id="losses" min="0" value="0">
    <button id="updateButton">Update Stats</button>
  </div>
</div>

<script type="module">
  import { pythonURI } from '/sprint4_frontend/assets/js/api/config.js';

  // Endpoint URLs using pythonURI
  const FETCH_PASTGAMES_URL = `${pythonURI}/api/pastgames`;
  const UPDATE_USER_URL = `${pythonURI}/api/admin/update_user_stats`;

  // Debug: Log the update URL
  console.log("UPDATE_USER_URL:", UPDATE_USER_URL);

  let allRecords = [];    // All records from pastGame (may include duplicates)
  let uniqueRecords = []; // Unique records grouped by user_id
  let selectedRecord = null;

  // Fetch and deduplicate records from the pastGame table
  async function fetchRecords() {
    try {
      const response = await fetch(FETCH_PASTGAMES_URL, { method: 'GET' });
      if (!response.ok) {
        throw new Error(`Failed to fetch records: ${response.statusText}`);
      }
      const records = await response.json();
      console.log("Fetched pastGame records:", records);
      allRecords = records;
      const seen = new Set();
      uniqueRecords = records.filter(record => {
        if (!seen.has(record.user_id)) {
          seen.add(record.user_id);
          return true;
        }
        return false;
      });
      const selectElem = document.getElementById('userSelect');
      selectElem.innerHTML = `<option value="">-- Select a user --</option>`;
      uniqueRecords.forEach(record => {
        const option = document.createElement('option');
        option.value = record.user_id.toString(); // using user_id as string
        option.textContent = `${record.user_name} (ID: ${record.user_id})`;
        selectElem.appendChild(option);
      });
    } catch (error) {
      console.error('Error fetching pastGame records:', error);
    }
  }
  fetchRecords();

  // Handle dropdown selection changes
  const userSelect = document.getElementById('userSelect');
  const selectedUserHeader = document.getElementById('selectedUserHeader');
  const winsInput = document.getElementById('wins');
  const lossesInput = document.getElementById('losses');
  const updateButton = document.getElementById('updateButton');

  userSelect.addEventListener('change', () => {
    const selectedUserIdStr = userSelect.value;
    console.log("Dropdown selected value:", selectedUserIdStr);
    if (!selectedUserIdStr) {
      selectedRecord = null;
      selectedUserHeader.textContent = "Select a user to edit";
      winsInput.value = 0;
      lossesInput.value = 0;
      return;
    }
    const selectedUserId = parseInt(selectedUserIdStr);
    const record = uniqueRecords.find(r => r.user_id === selectedUserId);
    console.log("Matching record:", record);
    if (record) {
      selectedRecord = record;
      selectedUserHeader.textContent = `Editing: ${record.user_name} (ID: ${record.user_id})`;
      winsInput.value = record.number_of_wins || 0;
      lossesInput.value = record.number_of_losses || 0;
    } else {
      selectedRecord = null;
      selectedUserHeader.textContent = "Select a user to edit";
      winsInput.value = 0;
      lossesInput.value = 0;
    }
  });

  // When Update is clicked, send a PUT request
  updateButton.addEventListener('click', async () => {
    if (!selectedRecord) {
      alert("Please select a user.");
      return;
    }
    const newWins = parseInt(winsInput.value) || 0;
    const newLosses = parseInt(lossesInput.value) || 0;

    // Prepare payload using "uid": convert user_id to a string
    const payload = {
      user_id: selectedRecord.user_id,  // Send user_id as integer
      wins: newWins,  // Use "wins" instead of "number_of_wins"
      losses: newLosses  // Use "losses" instead of "number_of_losses"
    };

    try {
        console.log("hi")
        console.log(payload)
      const response = await fetch(UPDATE_USER_URL, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });
      if (!response.ok) {
        throw new Error(`Status: ${response.status}`);
      }
      const result = await response.json();
      alert('User stats updated successfully!');
      console.log("Update response:", result);
    } catch (error) {
      console.error('Error updating user stats:', error);
      alert('Error updating user stats.');
    }
  });
</script>
