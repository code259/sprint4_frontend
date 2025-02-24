---
layout: base
title: Post
search_exclude: true
permalink: /post
author: Yash, Nikhil, Rohan, Neil
---

<div class="main">
    <div class="content">
        <h2 id="header-title">Chess Talk</h2>
        <div id="friends-container" class="friends-container">
            <div class="friend">
            </div>
        </div>
        <div id="form-container" class="form-container">
            <form id="channelForm">
                <div class="form-inputs">
                    <input type="text" id="title" name="title" placeholder="Enter Title Here" required>
                    <input type="file" id="fileInput" name="fileInput" style="display: none;">
                    <button type="button" onclick="document.getElementById('fileInput').click()" class="file-button">➕</button>
                </div>
                <textarea id="textArea" name="textArea" placeholder="Post Here" required></textarea>
                <button type="submit">Post</button>
            </form>
        </div>
        <div id="channels"></div>
    </div>
</div>

<div id="loginArea"></div>

<script type="module">
    import { pythonURI, fetchOptions } from '../assets/js/api/config.js';

    async function fetchUser() {
        try {
            const response = await fetch(`${pythonURI}/api/user`, fetchOptions);
            if (!response.ok) {
                throw new Error('User not logged in');
            }
            const user = await response.json();
            return user;
        } catch (error) {
            console.warn('User is not logged in:', error);
            return null;
        }
    }

    async function initializePage() {
        const user = await fetchUser();
        const sidebar = document.getElementById('sidebar');
        const loginArea = document.getElementById('loginArea');

        if (user) {
            sidebar.style.display = "flex";
            document.getElementById("header-title").style.display = "block";
            document.getElementById("friends-container").style.display = "flex";
            document.getElementById("form-container").style.display = "block";

            loginArea.innerHTML = `
                <div class="dropdown">
                    <button class="dropbtn">${user.name}</button>
                    <div class="dropdown-content">
                        <a href="{{site.baseurl}}/logout">Logout</a>
                        <a href="{{site.baseurl}}/profile">Profile</a>
                        <a href="{{site.baseurl}}/settings">Settings</a>
                    </div>
                </div>
            `;
        } else {
            sidebar.style.display = "none";
            document.getElementById("header-title").style.display = "none";
            document.getElementById("friends-container").style.display = "none";
            document.getElementById("form-container").style.display = "none";

            loginArea.innerHTML = `
                <a href="/flocker_frontend/login" class="login-btn">Login</a>
            `;
        }
    }

    window.onload = initializePage;
</script>

<div class="main">
    <div class="content">
        <h2 id="header-title" style="display: none;">Channels</h2>
        <div id="friends-container" class="friends-container" style="display: none;">
            <div class="friend">
            </div>
        </div>
        <div id="form-container" class="form-container" style="display: none;">
            <form id="channelForm">
                <div class="form-inputs">
                    <input type="text" id="title" name="title" placeholder="Enter Title Here" required>
                    <input type="file" id="fileInput" name="fileInput" style="display: none;">
                    <button type="button" onclick="document.getElementById('fileInput').click()" class="file-button">➕</button>
                </div>
                <textarea id="textArea" name="textArea" placeholder="Post Here" required></textarea>
                <button type="submit">Post</button>
            </form>
        </div>
        <div id="channels"></div>
    </div>
</div>

<script type="module">
    import { pythonURI, fetchOptions } from '../assets/js/api/config.js';

    async function fetchUser () {
        try {
            const response = await fetch(`${pythonURI}/api/user`, fetchOptions);
            if (!response.ok) {
                throw new Error('User  not logged in');
            }
            const user = await response.json();
            return user;
        } catch (error) {
            console.warn('User  is not logged in:', error);
            return null;
        }
    }

    async function initializePage() {
        const user = await fetchUser ();
        const sidebar = document.getElementById('sidebar');
        const loginArea = document.getElementById('loginArea');

        if (user) {
            sidebar.innerHTML = `
                <a href="/flocker_frontend/create_and_compete/realityroom-home" class="sidebar-btn">🏠 Home</a>
                <a href="/flocker_frontend/create_and_compete/reality_game" class="sidebar-btn">🎮 Game</a>
                <a href="/flocker_frontend/create_and_compete/reality-room-about" class="sidebar-btn">❓ About</a>
                <a href="/flocker_frontend/create_and_com
                
                
                pete/reality-room-terms" class="sidebar-btn">📄 Terms</a>
            `;
            sidebar.style.display = "flex";
            document.getElementById("header-title").style.display = "block";
            document.getElementById("friends-container").style.display = "flex";
            document.getElementById("form-container").style.display = "block";

            loginArea.innerHTML = `
                <div class="dropdown">
                    <button class="dropbtn">${user.name}</button>
                    <div class="dropdown-content">
                        <a href="{{site.baseurl}}/logout">Logout</a>
                        <a href="{{site.baseurl}}/profile">Profile</a>
                        <a href="{{site.baseurl}}/ settings">Settings</a>
                    </div>
                </div>
            `;
        } else {
            sidebar.innerHTML = '';
            sidebar.style.display = "none";
            document.getElementById("header-title").style.display = "none";
            document.getElementById("friends-container").style.display = "none";
            document.getElementById("form-container").style.display = "none";

            loginArea.innerHTML = `
                <a href="/sprint4_frontend/login" class="login-btn">Login</a>
            `;
        }
    }

    window.onload = initializePage;
</script>

<style>
    /* Sidebar */
    .sidebar {
        position: fixed;
        top: 0;
        left: 0;
        width: 150px;
        height: 100%;
        background-color: #121212 !important;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding-top: 20px;
        color: white;
        border-right: 1px solid gray;
    }
    .sidebar-btn {
        background-color: #121212;
        color: white !important;
        border: 2px solid gray;
        margin: 10px 0;
        padding: 10px;
        border-radius: 8px;
        font-size: 16px;
        width: 120px;
        text-align: center;
        cursor: pointer;
        text-decoration: none;
    }
    .main {
        display: flex;
    }
    .content {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100%;
        /* padding-left: 180px; */
    }

    .friends-container {
        display: flex;
        overflow-x: auto; /* horizontal scrolling */
        padding: 10px;
        margin-bottom: 10px;
        margin-top: 20px;
        gap: 10px; 
        scrollbar-width: thin;
        scrollbar-color: #ccc transparent; /* Color for scrollbar */
        width: 750px;
    }

    .friend {
        display: flex;
        flex-direction: column;
        align-items: center;
        position: relative;
        width: 80px; /* Set a width for each profile card */
        text-align: center;
        font-family: Arial, sans-serif;
    }

    .profile-pic {
        width: 60px;
        height: 60px;
        border-radius: 50%; /* Makes the image round */
        overflow: hidden;
        border: 2px solid #ddd;
    }

    .profile-pic img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .friend p {
        margin: 5px 0 0;
        font-size: 14px;
        color: #333;
    }

    .live-badge {
        position: absolute;
        top: -5px;
        left: 15px;
        background-color: #000;
        color: #fff;
        font-size: 12px;
        padding: 2px 6px;
        border-radius: 12px;
        font-weight: bold;
    }

    /* Form Styling */
    .form-container {
        padding: 20px;
        background-color: #f4f4f4;
        border-radius: 12px;
        width: calc(100% - 400px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        font-family: Arial, sans-serif;
    }

    .form-inputs {
        display: flex;
        gap: 10px;
        align-items: center;
    }

    #title {
        flex: 1;
        padding: 12px;
        border-radius: 8px;
        border: 1px solid #ddd;
        font-size: 16px;
    }

    .file-button {
        padding: 10px;
        background-color: #333;
        color: white;
        border: none;
        border-radius: 50%;
        font-size: 18px;
        cursor: pointer;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    #textArea {
        width: 100%;
        padding: 12px;
        border-radius: 8px;
        border: 1px solid #ddd;
        font-size: 16px;
        margin-top: 10px;
        resize: none;
        height: 100px;
    }

    button[type="submit"] {
        align-self: flex-start;
        padding: 10px 20px;
        background-color: #1da1f2;
        color: white;
        border: none;
        border-radius: 8px;
        font-size: 16px;
        font-weight: bold;
        cursor: pointer;
        margin-top: 10px;
        transition: background-color 0.2s ease;
    }

    button[type="submit"]:hover {
        background-color: #1a91da;
    }

    /* Channels Container */
    #channels {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 20px;
        padding-top: 20px;
    }

    /* Post Cards Styling */
    .card {
        width: calc(50% - 20px);
        min-width: 300px;
        padding: 20px;
        background-color: #ffffff;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        border-radius: 8px;
        text-align: left;
    }

    .card-title {
        font-size: 1.2em;
        font-weight: bold;
        color: #333;
    }

    .card-description {
        color: #555;
        font-size: 1em;
        margin-top: 10px;
    }

    .delete-button, .comment-button {
        background-color: #ff4d4d;
        color: white;
        border: none;
        padding: 8px 12px;
        border-radius: 4px;
        cursor: pointer;
        font-size: 0.9em;
        margin-top: 15px;
        transition: background-color 0.3s ease;
        margin-right: 5px;
    }

    .delete-button:hover, .comment-button:hover {
        background-color: #ff1a1a;
    }
</style>

<script type="module">
    import { pythonURI, fetchOptions } from '../assets/js/api/config.js';
    const container = document.getElementById("channels");

    function openChatRoom(button) {
        const channelId = button.getAttribute("id");
        window.location.href = `{{site.baseurl}}/create_and_compete/realityroom?channelId=${channelId}`;
    }

    async function fetchUser() {
        const response = await fetch(`${pythonURI}/api/user`, fetchOptions);
        const user = await response.json();
        console.log(user);
        return user;
    }

    const user = fetchUser();

    async function fetchChannels() {
        try {
            const groupName = 'Reality Room';
            const responseData = {
                group_name: groupName,
            };
            // add filter to get only messages from this channel
            const response = await fetch(`${pythonURI}/api/channels/filter`, {
                ...fetchOptions,
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(responseData)
            });

            if (!response.ok) {
                throw new Error('Failed to fetch channels: ' + response.statusText);
            }
            const channels = await response.json();
            container.innerHTML = "";

            channels.forEach(channel => {
                const card = document.createElement("div");
                card.classList.add("card");

                const title = document.createElement("h3");
                title.classList.add("card-title");
                title.textContent = channel.name;

                // const imageBox = document.createElement("div");
                // title.classList.add("image-box");

                const description = document.createElement("p");
                description.classList.add("card-description");
                description.textContent = channel.attributes["content"];

                const deleteButton = document.createElement("button");
                deleteButton.classList.add("delete-button");
                deleteButton.textContent = "Delete";

                const commentButton = document.createElement("button");
                commentButton.classList.add("comment-button");
                commentButton.textContent = "Comment";
                commentButton.setAttribute("id", channel.id);

                commentButton.onclick = function () {
                    openChatRoom(commentButton);
                };

                card.appendChild(title);
                card.appendChild(description);
                card.appendChild(deleteButton);
                card.appendChild(commentButton);

                container.appendChild(card);
            });
        } catch (error) {
            console.error('Error fetching channels:', error);
        }
    }

    document.getElementById('channelForm').addEventListener('submit', async function(event) {
        event.preventDefault();

        const title = document.getElementById('title').value;
        const content = document.getElementById('textArea').value;
        const group_id = 9;

        const channelData = {
            name: title,
            group_id: group_id,
            attributes: {"content": content}
        };

        try {
            const response = await fetch(`${pythonURI}/api/channel`, {
                ...fetchOptions,
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(channelData)
            });



            if (!response.ok) {
                throw new Error('Failed to add channel: ' + response.statusText);
            }

            fetchChannels();
            document.getElementById('channelForm').reset();
        } catch (error) {
            console.error('Error adding channel:', error);
            alert('Error adding channel: ' + error.message);
        }
    });

    fetchChannels();
</script>

<script type="module">
    import { pythonURI, fetchOptions } from '/sprint4_frontend/assets/js/api/config.js';
    async function fetchUserData() {
        try {
            // create the GET request to fetch user data
            const response = await fetch(`${pythonURI}/api/users`, fetchOptions);
            // checking for a successful response
            if (!response.ok) {
                throw new Error('Failed to fetch user data: ' + response.statusText);
            }
                                                                          
            // parse up response JSON
            const userData = await response.json();
            console.log(userData);
            // get the container where the user data will be displayed
            const friendsContainer = document.querySelector('.friends-container');
            for (const user of userData) {
                const { name, role, pfp } = user;
                const friendDiv = document.createElement('div');
                friendDiv.className = 'friend';
                const profilePicDiv = document.createElement('div');
                profilePicDiv.className = 'profile-pic';
                
                const img = document.createElement('img');
                img.src = "https://plus.unsplash.com/premium_photo-1671656349322-41de944d259b?q=80&w=2787&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D";
                profilePicDiv.appendChild(img);
                const nameTag = document.createElement('p');
                nameTag.textContent = name;
                friendDiv.appendChild(profilePicDiv);
                friendDiv.appendChild(nameTag);
                console.log(name);
                console.log(role);
                console.log(pfp);
                friendsContainer.appendChild(friendDiv);
            }
        } catch (error) {
            console.error('Error fetching user data:', error);
        }
    }
    // call the function to fetch and display user data
    fetchUserData();
</script>