---
layout: post 
title: Leaderboard 
search_exclude: true
permalink: /leaderboard/
---

<h1>Pawnsy Leaderboard</h1>
<div id="leaderboard-container"></div>

---
layout: page
title: Leaderboard
permalink: /leaderboard
search_exclude: false
---

<div id="leaderboard-container"></div>

<script>
  function renderLeaderboard(data) {
    let tableHTML = `
      <table border="1" cellspacing="0" cellpadding="8" style="width:100%; margin-top:20px;">
        <thead>
          <tr>
            <th>Rank</th>
            <th>User Name</th>
            <th>Wins</th>
            <th>Losses</th>
            <th>Total Games</th>
          </tr>
        </thead>
        <tbody>
    `;
    data.forEach((entry, index) => {
      tableHTML += `
        <tr>
          <td>${index + 1}</td>
          <td>${entry.user_name}</td>
          <td>${entry.number_of_wins}</td>
          <td>${entry.number_of_losses}</td>
          <td>${entry.total_games}</td>
        </tr>
      `;
    });
    tableHTML += `</tbody></table>`;
    document.getElementById('leaderboard-container').innerHTML = tableHTML;
  }

  // IMPORTANT: Use the absolute URL with :8401
  fetch('http://127.0.0.1:8401/api/leaderboard')
    .then(response => response.json())
    .then(data => renderLeaderboard(data))
    .catch(error => console.error("Error fetching leaderboard:", error));
</script>

