---
layout: base
title: Admin Panel
search_exclude: true
permalink: /admin/
---

<h1>Admin Page: Update User Wins/Losses</h1>

<!-- Container for the typeahead search -->
<div>
  <label for="userSearch">Search User:</label>
  <input type="text" id="userSearch" placeholder="Type to search user..." autocomplete="off">
  <div id="suggestions" style="border: 1px solid #ccc; display: none; position: absolute; background: #fff; z-index: 999;">
    <!-- Suggestions will appear here -->
  </div>
</div>

<!-- Form to display and update user data -->
<div id="editForm" style="margin-top:20px; display:none;">
  <h3 id="selectedUser"></h3>
  <label for="wins">Wins:</label>
  <input type="number" id="wins" min="0" value="0">
  <br><br>
  <label for="losses">Losses:</label>
  <input type="number" id="losses" min="0" value="0">
  <br><br>
  <button id="updateButton">Update</button>
</div>

<script>
  // -------------------------------
  // 1. Configuration & Data
  // -------------------------------
  // Adjust these URLs to match your setup.
  // Endpoint to get all users (should return a JSON array of users).
  const FETCH_USERS_URL = 'http://127.0.0.1:8401/api/user/all'; 
  // Endpoint to update a user's wins/losses in the pastGame table.
  const UPDATE_USER_URL = 'http://127.0.0.1:8401/api/admin/update_stats'; 

  let allUsers = [];        // Will store the entire list of users
  let selectedUser = null;  // The user object currently selected

  // -------------------------------
  // 2. Fetch All Users for the Dropdown
  // -------------------------------
  fetch(FETCH_USERS_URL, {
      method: 'GET',
      credentials: 'include'  // Send cookies with the request
  })
    .then(response => {
      if (!response.ok) {
        throw new Error(`Failed to fetch users: ${response.status}`);
      }
      return response.json();
    })
    .then(data => {
      allUsers = data; // Store the list of users in memory
    })
    .catch(err => console.error('Error fetching users:', err));

  // -------------------------------
  // 3. Typeahead Logic
  // -------------------------------
  const userSearchInput = document.getElementById('userSearch');
  const suggestionsDiv = document.getElementById('suggestions');
  const editFormDiv = document.getElementById('editForm');
  const selectedUserHeader = document.getElementById('selectedUser');
  const winsInput = document.getElementById('wins');
  const lossesInput = document.getElementById('losses');
  const updateButton = document.getElementById('updateButton');

  // Show filtered suggestions as user types
  userSearchInput.addEventListener('input', () => {
    const query = userSearchInput.value.toLowerCase();
    if (!query) {
      suggestionsDiv.style.display = 'none';
      return;
    }
    
    // Filter allUsers by name or uid
    const filtered = allUsers.filter(user => {
      const userName = user.name ? user.name.toLowerCase() : '';
      const userUid  = user.uid  ? user.uid.toLowerCase()  : '';
      return userName.includes(query) || userUid.includes(query);
    });

    if (filtered.length === 0) {
      suggestionsDiv.style.display = 'none';
      return;
    }

    // Build suggestions list
    let html = '';
    filtered.forEach(u => {
      html += `<div class="suggestion-item" style="padding: 5px; cursor: pointer;" data-uid="${u.uid}">
                 ${u.name} (${u.uid})
               </div>`;
    });
    suggestionsDiv.innerHTML = html;
    suggestionsDiv.style.display = 'block';
  });

  // When a suggestion is clicked, fill the form
  suggestionsDiv.addEventListener('click', (e) => {
    const item = e.target.closest('.suggestion-item');
    if (!item) return;
    
    const uid = item.getAttribute('data-uid');
    const userObj = allUsers.find(u => u.uid === uid);
    if (!userObj) return;

    selectedUser = userObj;
    userSearchInput.value = `${userObj.name} (${userObj.uid})`;
    suggestionsDiv.style.display = 'none';

    // If wins/losses are part of the user data, fill them in (adjust as needed)
    const existingWins = userObj.wins || 0;
    const existingLosses = userObj.losses || 0;
    winsInput.value = existingWins;
    lossesInput.value = existingLosses;

    selectedUserHeader.textContent = `Editing: ${userObj.name} (${userObj.uid})`;
    editFormDiv.style.display = 'block';
  });

  // -------------------------------
  // 4. Submit Button Logic to Update Stats
  // -------------------------------
  updateButton.addEventListener('click', () => {
    if (!selectedUser) {
      alert('No user selected!');
      return;
    }
    const newWins   = parseInt(winsInput.value) || 0;
    const newLosses = parseInt(lossesInput.value) || 0;

    const payload = {
      user_uid: selectedUser.uid,
      number_of_wins: newWins,
      number_of_losses: newLosses
    };

    fetch(UPDATE_USER_URL, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json'
      },
      credentials: 'include',  // Send cookies with the request
      body: JSON.stringify(payload)
    })
      .then(res => {
        if (!res.ok) throw new Error(`Status: ${res.status}`);
        return res.json();
      })
      .then(data => {
        alert('User stats updated successfully!');
        console.log('Update response:', data);
      })
      .catch(err => {
        console.error('Error updating user:', err);
        alert('Error updating user stats!');
      });
  });
</script>