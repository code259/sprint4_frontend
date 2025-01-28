---
layout: base 
title: User Lookup 
search_exclude: false
permalink: /lookup/
---

<div class="container mt-5">
  <h1 class="text-center">Chess User Stats Lookup</h1>
  <div class="mt-4">
    <label for="userId" class="form-label">Enter User ID:</label>
    <input type="number" id="userId" class="form-control" placeholder="User ID" required>
    <button id="lookupButton" class="btn btn-primary mt-3">Look Up</button>
  </div>

  <div id="results" class="mt-4 d-none">
    <h3>User Stats</h3>
    <p><strong>ID:</strong> <span id="userIdResult"></span></p>
    <p><strong>Wins:</strong> <span id="winsResult"></span></p>
    <p><strong>Losses:</strong> <span id="lossesResult"></span></p>
  </div>

  <div id="error" class="alert alert-danger mt-4 d-none"></div>
</div>

<script>
  document.getElementById('lookupButton').addEventListener('click', async () => {
    const userId = document.getElementById('userId').value;
    const resultsDiv = document.getElementById('results');
    const errorDiv = document.getElementById('error');
    
    resultsDiv.classList.add('d-none');
    errorDiv.classList.add('d-none');

    if (!userId) {
      errorDiv.textContent = "Please enter a valid User ID.";
      errorDiv.classList.remove('d-none');
      return;
    }

    try {
      const response = await fetch(`http://127.0.0.1:8887/api/user_stats/${userId}`);
      if (!response.ok) {
        throw new Error(await response.text());
      }

      const data = await response.json();
      document.getElementById('userIdResult').textContent = data.user_id;
      document.getElementById('winsResult').textContent = data.wins;
      document.getElementById('lossesResult').textContent = data.losses;

      resultsDiv.classList.remove('d-none');
    } catch (error) {
      errorDiv.textContent = error.message || "Failed to fetch user stats.";
      errorDiv.classList.remove('d-none');
    }
  });
</script>