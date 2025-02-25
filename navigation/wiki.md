---
layout: post
title: Chess Wiki
search_exclude: true
permalink: /wiki/
---

<style>
  h1 {
    color: #FFFFFF;
    font-family: 'Arial', sans-serif;
    font-size: 3.5em;
  }

  table {
    width: 80%;
    margin: 20px auto;
    border-collapse: collapse;
    background-color: transparent;
    color: #FFFFFF;
    font-family: "Arial", sans-serif;
    font-size: 16px;
    border: 2px solid #000000;
  }

  /* Table header styles */
  th {
    background-color: #000000;
    color: #FFFFFF;
    padding: 10px;
    text-align: center;
    border: 2px solid #444444;
  }

  /* Table body styles */
  td {
    padding: 10px;
    text-align: center;
    border: 1px solid #000000;
  }

  /* Zebra stripes effect */
  tr:nth-child(even) {
    background-color: #444444;
  }

  tr:nth-child(odd) {
    background-color: transparent;
  }

  /* Hover effect */
  tr:hover {
    background-color: #222222;
    color: #FFFFFF;
  }
</style>

<center>
  <h1> Best Chess Tips & Resources </h1>
</center>

<table id="chess-tips" class="table">
  <thead>
    <tr>
      <th>Resource</th>
      <th>Link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10 Chess Tips for Beginners</td>
      <td><a href="https://www.chess.com/blog/OnlineChessTeacher/chess-for-beginners-10-tips-to-start-winning-today" class="chess-link" target="_blank">Read Here</a></td>
    </tr>
    <tr>
      <td>Best Chess Openings for Beginners</td>
      <td><a href="https://www.chess.com/article/view/the-best-chess-openings-for-beginners" class="chess-link" target="_blank">Read Here</a></td>
    </tr>
    <tr>
      <td>How to Improve at Chess</td>
      <td><a href="https://lichess.org/forum/general-chess-discussion/best-way-to-improve-at-chess" class="chess-link" target="_blank">Read Here</a></td>
    </tr>
    <tr>
      <td>Endgame Strategy Guide</td>
      <td><a href="https://www.thechesswebsite.com/chess-end-game-tactics/" class="chess-link" target="_blank">Read Here</a></td>
    </tr>
  </tbody>
</table>

<style>
  .chess-link {
    text-decoration: none;
    color: #FFFFFF;
    font-weight: bold;
  }

  .chess-link:hover {
    text-decoration: underline;
    color: #BBBBBB;
  }
</style>

<style>
    body {
        background-image: url("../../images/background9674.png");
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
        background-attachment: fixed;
    }

    .chat-container {
        width: 50%;
        margin: auto;
        padding: 20px;
        background: rgba(11, 11, 11, 0.8);
        border-radius: 10px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    .chat-box {
        height: 300px;
        overflow-y: auto;
        border: 1px solid #ccc;
        padding: 10px;
        background: white;
    }

    .chat-input {
        margin-top: 10px;
        display: flex;
        flex-direction: column;
    }

    .chat-input input, .chat-input button {
        margin: 5px 0;
        padding: 10px;
        font-size: 16px;
    }

    .message {
        padding: 5px;
        border-bottom: 1px solid #ddd;
        color: black;
    }
</style>

<center>
  <h2 style="color: #FFFFFF; font-family: 'Arial', sans-serif; font-size: 2.5em;">Famous Chess Players</h2>
</center>

<div class="chess-gallery" style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap;">
    <div>
        <h3 style="color: #FFFFFF;">Magnus Carlsen</h3>
        <img src="https://images.chesscomfiles.com/uploads/v1/master_player/3b0ddf4e-bd82-11e8-9421-af517c2ebfed.f2dc9e34.5000x5000o.41b400498a4b.png" width="300" alt="Magnus Carlsen">
    </div>
    <div>
        <h3 style="color: #FFFFFF;">Garry Kasparov</h3>
        <img src="https://www.bigspeak.com/wp-content/uploads/2014/07/1639070742-GettyImages-1157937604.jpg" width="300" alt="Garry Kasparov">
    </div>
    <div>
        <h3 style="color: #FFFFFF;">Bobby Fischer</h3>
        <img src="https://chessacademy.com/cdn/shop/articles/Bobby1_1200x.jpg?v=1651329629" width="300" alt="Bobby Fischer">
    </div>
</div>

<center>
  <h2 style="color: #FFFFFF; font-family: 'Arial', sans-serif; font-size: 2.5em;">Historic Chess Matches</h2>
</center>

<div class="chess-gallery" style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap;">
    <div>
        <h3 style="color: #FFFFFF;">Kasparov vs. Deep Blue</h3>
        <img src="https://eu-images.contentstack.com/v3/assets/blt6b0f74e5591baa03/blt7cc688614b70f9be/65672f49361a98040a9a8940/News_Image_-_2023-11-29T123206.186.jpg" width="300" alt="Kasparov vs Deep Blue">
    </div>
    <div>
        <h3 style="color: #FFFFFF;">Carlsen vs. Anand (2014)</h3>
        <img src="https://images.chesscomfiles.com/uploads/v1/blog/881761.d6c537d3.5000x5000o.c974a4ad60bc.jpg" width="300" alt="Carlsen vs Anand">
    </div>
</div>


<center>
  <h2 style="color: #FFFFFF; font-family: 'Arial', sans-serif; font-size: 2.5em;">Player Recomendations</h2>
</center>

<div class="chat-container">
    <h3>Chat</h3>
    <div class="chat-box" id="chatBox"></div>
    <div class="chat-input">
        <input type="text" id="message" placeholder="Enter your message">
        <button id="sendButton">Send</button>
    </div>
</div>

<script type="module">
    import { pythonURI, fetchOptions } from '/sprint4_frontend/assets/js/api/config.js';
    async function getUser() {
        try {
            const response = await fetch(`${pythonURI}/api/user`, fetchOptions);
            if (!response.ok) {
                window.location.href = `${window.location.origin}/sprint4_frontend/login`;
                throw new Error('Failed to fetch user.');
            }

            const user = await response.json();
            return user['role']
            console.log("Role", user['role'])
        } catch (error) {
            console.error('Error fetching skills:', error);
        }
    }

    async function fetchPosts(){
        try {
            const response = await fetch(`${pythonURI}/api/posts`, fetchOptions);
            if (!response.ok) {
                throw new Error('Failed to fetch posts: ' + response.statusText);
            }

            const posts = await response.json();
            const userRole = await getUser();
            const chatBox = document.getElementById('chatBox');
            chatBox.innerHTML = ``;

            posts.forEach(post => {
                console.log(post);
                const messageElement = document.createElement('div');
                messageElement.classList.add('message');

                const messageContent = document.createElement('span');
                messageContent.innerHTML = `<strong>${post.user_name || "Message"}:</strong> ${post.comment}`;
                messageElement.appendChild(messageContent);

                const updateButton = document.createElement('button');
                updateButton.textContent = 'Update';
                updateButton.style.marginLeft = '10px';
                updateButton.addEventListener('click', () => {
                  updatePost(post.id, post.comment);
                });
                messageElement.appendChild(updateButton);

                if (userRole === 'Admin') {
                  const deleteButton = document.createElement('button');
                  deleteButton.textContent = 'Delete';
                  deleteButton.style.marginLeft = '10px';
                  deleteButton.style.color = 'red';
                  deleteButton.addEventListener('click', () => deletePost(post.id));
                  messageElement.appendChild(deleteButton);
                }

                chatBox.appendChild(messageElement);
            });
        } catch (error) {
            console.error('Error fetching entries:', error);
        }
    }

    async function updatePost(postId, oldComment) {
      // Prompt the user for the new comment
      const newComment = prompt("Enter new content for the message:", oldComment);
      if (newComment === null || newComment.trim() === "") {
        // User cancelled or entered an empty string
        return;
      }
      try {
        const response = await fetch(`${pythonURI}/api/post`, {
          method: 'PUT',
          headers: { 'Accept': 'application/json', 'Content-Type': 'application/json' },
          credentials: 'include',
          body: JSON.stringify({ id: postId, title: "", comment: newComment, channel_id: 1})
        });

        if (!response.ok) {
          throw new Error('Failed to update post.');
        }

        const data = await response.json();
        console.log("Updated post:", data);
        // Refresh posts to show the updated message
        fetchPosts();
      } catch (error) {
        console.error("Error updating post:", error);
      }
    }

    async function deletePost(postId) {
      const userRole = await getUser();
      
      if (userRole !== 'Admin') {
        alert("You do not have permission to delete this message.");
        return;
      }

      if (!confirm("Are you sure you want to delete this message?")) {
        return;
      }

      try {
        const response = await fetch(`${pythonURI}/api/post`, {
          method: 'DELETE',
          headers: { 'Accept': 'application/json', 'Content-Type': 'application/json' },
          credentials: 'include',
          body: JSON.stringify({ id: postId })
        });

        if (!response.ok) throw new Error('Failed to delete post.');

        console.log("Deleted post:", await response.json());
        fetchPosts();
      } catch (error) {
        console.error("Error deleting post:", error);
      }
    }

    async function sendMessage() {
        const message = document.getElementById('message').value.trim();

        try {
            const response = await fetch(`${pythonURI}/api/post`, {
              method: 'POST',
              headers: { 'Accept': 'application/json', 'Content-Type': 'application/json' },
              credentials: 'include',
              body: JSON.stringify({title: "", comment: message, channel_id: 1}),
            });

            if (!response.ok) throw new Error('Failed to add post.');

            const data = await response.json();
            console.log(data);
            fetchPosts();

            } catch (error) {
              console.error('Error adding post:', error);
        }

        document.getElementById('message').value = '';
    }

    document.getElementById('sendButton').addEventListener('click', sendMessage);

    fetchPosts();
</script>

