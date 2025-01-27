---
layout: base
title: Admin Panle
search_exclude: true
permalink: /admin/
---

<div style="max-width: 600px; margin: 50px auto; background-color: #f8f9fa; padding: 20px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">
  <h1 style="text-align: center; color: #333;">Admin Panel</h1>

  <!-- Section for creating a user -->
  <h3 style="color: #333;">Create User</h3>
  <input type="number" id="createUserId" placeholder="Enter User ID" style="width: 100%; margin: 10px 0; padding: 10px; font-size: 16px;">
  <input type="number" id="createWins" placeholder="Enter Wins (optional)" style="width: 100%; margin: 10px 0; padding: 10px; font-size: 16px;">
  <input type="number" id="createLosses" placeholder="Enter Losses (optional)" style="width: 100%; margin: 10px 0; padding: 10px; font-size: 16px;">
  <button id="createUserBtn" style="width: 100%; margin: 10px 0; padding: 10px; font-size: 16px; background-color: #007bff; color: white; border: none; border-radius: 5px; cursor: pointer;">Create User</button>

  <!-- Section for deleting a user -->
  <h3 style="color: #333;">Delete User</h3>
  <input type="number" id="deleteUserId" placeholder="Enter User ID" style="width: 100%; margin: 10px 0; padding: 10px; font-size: 16px;">
  <button id="deleteUserBtn" style="width: 100%; margin: 10px 0; padding: 10px; font-size: 16px; background-color: #f44336; color: white; border: none; border-radius: 5px; cursor: pointer;">Delete User</button>

  <!-- Message display -->
  <div id="message" style="display: none; margin-top: 20px; padding: 10px; border-radius: 5px;"></div>
</div>

<script>
  // Backend URI
  const backendURI = "http://127.0.0.1:8887"; // Replace with your backend URL

  // Function to display messages
  function showMessage(type, text) {
    const messageDiv = document.getElementById('message');
    messageDiv.style.display = "block";
    messageDiv.style.backgroundColor = type === "success" ? "#d4edda" : "#f8d7da";
    messageDiv.style.color = type === "success" ? "#155724" : "#721c24";
    messageDiv.textContent = text;

    setTimeout(() => {
      messageDiv.style.display = "none";
    }, 5000);
  }

  // Event listener for creating a user
  document.getElementById("createUserBtn").addEventListener("click", async () => {
    const userId = document.getElementById("createUserId").value;
    const wins = document.getElementById("createWins").value || 0;
    const losses = document.getElementById("createLosses").value || 0;

    if (!userId) {
      showMessage("error", "User ID is required.");
      return;
    }

    try {
      const response = await fetch(`${backendURI}/api/admin/create_user`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ user_id: parseInt(userId), wins: parseInt(wins), losses: parseInt(losses) }),
      });

      const data = await response.json();
      if (response.ok) {
        showMessage("success", "User created successfully.");
      } else {
        showMessage("error", data.error || "Failed to create user.");
      }
    } catch (error) {
    console.error("Error details:", error);
    showMessage("error", `An error occurred: ${error.message}`);
    }
  });

  // Event listener for deleting a user
  document.getElementById("deleteUserBtn").addEventListener("click", async () => {
    const userId = document.getElementById("deleteUserId").value;

    if (!userId) {
      showMessage("error", "User ID is required.");
      return;
    }

    try {
      const response = await fetch(`${backendURI}/api/admin/delete_user`, {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ user_id: parseInt(userId) }),
      });

      const data = await response.json();
      if (response.ok) {
        showMessage("success", `User with ID ${userId} deleted successfully.`);
      } else {
        showMessage("error", data.error || "Failed to delete user.");
      }
    } catch (error) {
      showMessage("error", "An error occurred. Please try again.");
    }
  });
</script>
