---
layout: base 
title: Admin Panel: Update User Stats
search_exclude: false
permalink: /admin_page/
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

  <!-- Form to update stats; initially hidden -->
  <div id="userDetails" style="display: none;">
    <h3 id="selectedUserHeader"></h3>
    <label for="wins">Wins:</label>
    <input type="number" id="wins" min="0" value="0">
    <label for="losses">Losses:</label>
    <input type="number" id="losses" min="0" value="0">
    <button id="updateButton">Update Stats</button>
  </div>
</div>

<script type="module">
  import { pythonURI } from '/sprint4_frontend/assets/js/api/config.js';

  // Define endpoint URLs using pythonURI (without credentials)
  const FETCH_USERS_URL = `${pythonURI}/api/user/all`;
  const UPDATE_USER_URL = `${pythonURI}/api/admin/update_user_stats`;

  let allUsers = [];    // Array to hold all user objects
  let selectedUser = null; // Currently selected user

  // Fetch all users and populate the dropdown
  async function fetchUsers() {
    try {
      const response = await fetch(FETCH_USERS_URL, { method: 'GET' });
      if (!response.ok) {
        throw new Error(`Failed to fetch users: ${response.statusText}`);
      }
      const users = await response.json();
      allUsers = users;
      const selectElem = document.getElementById('userSelect');
      // Clear existing options
      selectElem.innerHTML = `<option value="">-- Select a user --</option>`;
      users.forEach(user => {
        // Option value is the user's uid; display text shows name and uid.
        const option = document.createElement('option');
        option.value = user.uid;
        option.textContent = `${user.name} (${user.uid})`;
        selectElem.appendChild(option);
      });
    } catch (error) {
      console.error('Error fetching users:', error);
    }
  }
  fetchUsers();

  // Listen for changes in the dropdown
  const userSelect = document.getElementById('userSelect');
  const userDetails = document.getElementById('userDetails');
  const selectedUserHeader = document.getElementById('selectedUserHeader');
  const winsInput = document.getElementById('wins');
  const lossesInput = document.getElementById('losses');
  const updateButton = document.getElementById('updateButton');

  userSelect.addEventListener('change', () => {
    const selectedUid = userSelect.value;
    if (!selectedUid) {
      selectedUser = null;
      userDetails.style.display = 'none';
      return;
    }
    // Find the selected user by uid
    const user = allUsers.find(u => u.uid === selectedUid);
    if (user) {
      selectedUser = user;
      selectedUserHeader.textContent = `Editing: ${user.name} (${user.uid})`;
      winsInput.value = user.wins || 0;
      lossesInput.value = user.losses || 0;
      userDetails.style.display = 'block';
    } else {
      selectedUser = null;
      userDetails.style.display = 'none';
    }
  });

  // When the admin clicks "Update Stats", send a PUT request
  updateButton.addEventListener('click', async () => {
    if (!selectedUser) {
      alert("Please select a user.");
      return;
    }
    const newWins = parseInt(winsInput.value) || 0;
    const newLosses = parseInt(lossesInput.value) || 0;

    const payload = {
      uid: selectedUser.uid,
      number_of_wins: newWins,
      number_of_losses: newLosses
    };

    try {
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
