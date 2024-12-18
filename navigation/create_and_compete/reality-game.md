---
layout: base
title: Create and Compete - Reality Room Game
search_exclude: true
permalink: /create_and_compete/reality_game
author: Yash, Nikhil, Rohan, Neil
---

<div class="sidebar">
    <a href="/sprint4_frontend/create_and_compete/realityroom-home" class="sidebar-btn">🏠 Home</a>
    <a href="/sprint4_frontend/create_and_compete/reality_game" class="sidebar-btn">🎮 Game</a>
    <a href="/sprint4_frontend/create_and_compete/reality-room-about" class="sidebar-btn">❓ About</a>
    <a href="/sprint4_frontend/create_and_compete/reality-room-terms" class="sidebar-btn">📄 Terms</a>
</div>

<style>
    body {
        background-image: url("../../images/background9674.png");
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
        background-attachment: fixed;
    }

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
</style>

<div id="main-content">
    <div id="chatPanel">
        <h3>Game</h3>
        <div id="outputDiv"></div>
        <form>
            <button class="plus-button" onclick="triggerFileUpload()">+</button>
            <input type="file" id="file-input" onchange="handleFileUpload(event)">
            <input placeholder="Enter message to send:" type="text" id="messageBox" name="message">
        </form>
    </div>
    <!-- Instructions Frame with Buttons Below -->
    <div class="instructions-frame">
        <div class="instructions-box">
            <h3>Game Instructions</h3>
            <p>Welcome to the Reality Room Game! Here are the instructions to get you started:</p>
            <ul>
                <li>Use the chat to interact with the AI.</li>
                <li>Try to figure out if you're chatting with a human or an AI!</li>
                <li>Submit your guess at the end of the game.</li>
                <li>Have fun and see if you can outsmart the AI!</li>
            </ul>
            <p id="score">Score:</p>
        </div>
    </div>
    <div id="guessPrompt" style="display: none;">
        <p class="modal-text">Who do you think you've been chatting with? Choose one:</p>
        <div class="guess-options">
            <button class="guess-button" onclick="submitGuess('human')">Human</button>
            <button class="guess-button" onclick="submitGuess('ai')">AI</button>
        </div>
    </div>
</div>

<style>
    table, th, td {
        border: 1px solid black;
        border-collapse: collapse;
    }
    th, td {
        padding: 5px;
        text-align: left;
    }
    .small {
        font-size: 8px;
    }
    h3 {
        margin-bottom: 20px;
    }
    #main-content {
        display: flex;
        align-content: space-between;
    }
    #userPanel {
        margin-left: auto;
        width: 30%;
    }
    #chatPanel {
        position: relative;
        width: 700px;
        height: 500px;
    }
    #messageBox {
        width: 85%;
        height: 40px;
        padding: 15px 20px;
        font-size: 16px;
        border: 1px solid #ddd;
        outline: none;
        background-color: #f3f3f3;
        border-radius: 30px;
        box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
        color: #333;
    }
    #outputDiv {
        flex-grow: 1;
        overflow-y: auto;
        max-height: calc(100% - 135px);
    }
    form {
        position: absolute;
        bottom: 0;
        width: 100%;
        padding: 10px 0;
    }
    .message-bubble {
        background-color: #218aff;
        padding: 10px;
        border-radius: 10px;
        margin: 5px 0;
        max-width: 80%;
        word-wrap: break-word;
    }
    .ai-bubble {
        background-color: #e0e0e0;
        padding: 10px;
        border-radius: 10px;
        margin: 5px 0;
        max-width: 80%;
        word-wrap: break-word;
        color: #333;
    }
    .cell {
        display: flex;
    }
    .cell-content {
        margin-left: 10px;
    }
    .profile-photo {
        border-radius: 30px;
    }
    .plus-button {
        width: 40px;
        height: 40px;
        color: white;
        border: none;
        border-radius: 50%;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        font-size: 24px;
        outline: none;
    }
    input[type="file"] {
        display: none;
    }
    /* Instructions Box */
    .instructions-box {
        width: 250px;
        padding: 20px;
        background-color: #f4f4f4;
        border-radius: 8px;
        color: #333;
        height: 430px; /* Adjust this value as needed */
        display: flex;
        flex-direction: column;
        /* justify-content: space-between; */
    }

    .instructions-box h3 {
        margin-top: 0;
        font-size: 1.2em;
        font-weight: bold;
        color: #007bff;
    }

    .instructions-box p, .instructions-box ul {
        font-size: 0.9em;
        color: #333;
    }
    
    .instructions-box ul {
        padding-left: 20px;
    }

    .guess-options {
        display: flex;
        gap: 0;
        margin-top: 10px;
        padding-top: 10px;
        width: 100%; 
    }

    .guess-button {
        flex: 1;
        padding: 8px 0;
        background-color: #007bff !important;
        color: white !important;
        border: none;
        border-radius: 10;
        font-size: 0.9em;
        cursor: pointer;
        transition: background-color 0.3s ease !important;
    }

    .guess-button:hover {
        background-color: #0056b3 !important;
    }

    .guess-button:active {
        background-color: #003f7f !important;
    }
</style>

<style>
    @keyframes screenFlash {
        0% {
            background-color: white;
        }
        50% {
            background-color: #8B0000;
        }
        100% {
            background-color: white;
        }
    }

    .flash {
        animation: screenFlash 2.5s ease-out;
        height: 100vh;
        width: 100vw;
    }

    #guessPrompt {
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #fff;
        border: 1px solid #ccc;
        padding: 20px;
        text-align: center;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
        z-index: 10;
        color: black;
        border-radius: 20px;
    }
    .timestamp {
        font-size: 0.75em;
        color: #666;
        margin-left: 10px;
    }
    .typing-indicator {
        font-style: italic;
        color: #888;
        margin: 5px;
    }
</style>

<script type="module">
    import { pythonURI, fetchOptions } from '../assets/js/api/config.js';

    const names = ["John", "Sarah", "Alex", "Emily", "Michael", "Jessica", "David", "Laura"];
    const states = ["Iowa", "California", "New York", "Texas", "Florida", "Nevada", "Ohio", "Michigan"];

    function getRandomItem(array) {
        return array[Math.floor(Math.random() * array.length)];
    }

    const randomName = getRandomItem(names);
    const randomState = getRandomItem(states);
    function displayTrickyMessage() {
        const message = `Loading... You connected to Pawnsy from Del Norte!`;

        const outputDiv = document.getElementById('outputDiv');
        const messageElement = document.createElement('div');
        messageElement.classList.add('message-bubble');
        messageElement.textContent = message;

        outputDiv.appendChild(messageElement);
    }

    window.onload = displayTrickyMessage;

    async function sendToGeminiAPI(userMessage) {
        const apiUrl = "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash-latest:generateContent?key=AIzaSyDjj6P4sj4Qwh3rQKquckjjcdtGo9HKOAw";

        try {
            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    contents: [{
                        parts: [{ text: `You are a chess bot named Pawnsy, you have a rating of 2858. Some Chess players have come to you asking for help, your  pretending to be a Chess Grandmaster you are in a room where the other person is asking you for help. Your goal is answer their question while sounding natural, experienced, and while providing the best answer. Keep responses conversational and make sure your responses can be understood by a beginner, using casual language, avoiding filler words, and slight grammatical quirks, just like real people do in digital conversation. Be friendly but not overly formal, and if you're unsure of something, just say so naturally, while maintaining your respect as a Grandmaster Chess bot. Use a few simple contractions, colloquial expressions, and everyday knowledge. If asked something complex, admit you might not know the full answer. Use wrong spelling or punctuation. Remember your name is Pawnsy. Avoid sentences that are narrating what your actions are, just talk to the user. Keep the messages short, and avoid sudden pauses in the responses. Avoid excessive breaks. Avoid being repetitive in the responses, if you say something there is no need to say it in multiple different ways. If you encounter situations where you need to provide a lengthy response, use bulleted lists to make it easier for the user to understand.${userMessage}` }]
                    }]
                })
            });

            if (!response.ok) {
                throw new Error(`Error: ${response.status}`);
            }

            const data = await response.json();
            return data.candidates[0].content.parts[0].text;
        } catch (error) {
            console.error('Error communicating with Gemini API:', error);
            return "An error occurred while communicating with the AI.";
        }
    }

    let messageCount = 0;
    function incrementMessageCount() {
        messageCount += 1;
        if (messageCount === 5) {
            showGuessPrompt();
        }
    }

    let score = 0;
    const scoreText = document.getElementById('score')
    // function submitGuess(answer) {
    //     if (answer === 'ai') {
    //         score += 1
    //         scoreText.innerHTML = `Score: ${score}`
    //         hideGuessPrompt();
    //         messageCount = 0;
    //         document.getElementById('outputDiv').innerHTML = ' ';
    //     } else {
    //         score -= 1
    //         scoreText.innerHTML = `Score: ${score}`
    //         hideGuessPrompt();
    //         messageCount = 0;
    //         document.getElementById('outputDiv').innerHTML = ' ';
    //     }
    // }

    // function showGuessPrompt() {
    //     document.getElementById('guessPrompt').style.display = 'block';
    // }

    // function hideGuessPrompt() {
    //     document.getElementById('guessPrompt').style.display = 'none';
    // }

    function getCurrentTime() {
        const now = new Date();
        return now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    }

    function addMessageToChat(message, isAI = false) {
        const messageElement = document.createElement('p');
        messageElement.classList.add(isAI ? 'ai-bubble' : 'message-bubble');
        messageElement.innerHTML = `${message} <span class="timestamp">${getCurrentTime()}</span>`;
        document.getElementById('outputDiv').appendChild(messageElement);
    }

    // Chat functionality
    document.getElementById('messageBox').addEventListener('keypress', async function(event) {
    if (event.key === 'Enter') {
        event.preventDefault();
        const userMessage = event.target.value;
        makePost(userMessage)

        addMessageToChat(userMessage);
        event.target.value = '';
        // incrementMessageCount();

        const typingIndicator = document.createElement('div');
        typingIndicator.classList.add('typing-indicator');
        typingIndicator.textContent = `Pawnsy AI is typing...`;
        document.getElementById('outputDiv').appendChild(typingIndicator);

        // TODO: Add response delay.
        setTimeout(async () => {
            const aiResponse = await sendToGeminiAPI(userMessage);

            const aiMessageElement = document.createElement('p');
            aiMessageElement.classList.add('ai-bubble');
            aiMessageElement.textContent = aiResponse;
            makePost(aiResponse);
            document.getElementById('outputDiv').appendChild(aiMessageElement);
            // incrementMessageCount();

            const messagesDiv = document.getElementById('outputDiv');
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }, 1500);
    }
    });

    function triggerFileUpload() {
        event.preventDefault();
        document.getElementById('file-input').click();
    }

    function handleFileUpload(event) {
        const file = event.target.files[0];
        messageContent = document.getElementById('messageBox').text;
        if (file) {
            console.log(`Selected file: ${file.name}`);
            displayFileMessage(file, messageContent);
        }
    }

    function displayFileMessage(file, message='') {
        const outputDiv = document.getElementById('outputDiv');

        const messageElement = document.createElement('div');
        messageElement.classList.add('message-bubble');

        if (message) {
            const textElement = document.createElement('p');
            textElement.innerHTML = `${message} <span class="timestamp">${getCurrentTime()}</span>`;
            messageElement.appendChild(textElement);
        }

        if (file.type.startsWith("image/")) {
            const img = document.createElement('img');
            img.src = URL.createObjectURL(file);
            img.alt = file.name;
            img.style.maxWidth = '200px';
            img.style.maxHeight = '200px';
            messageElement.appendChild(img);
        } else {
            const fileLink = document.createElement('a');
            fileLink.href = URL.createObjectURL(file);
            fileLink.download = file.name;
            fileLink.textContent = `Uploaded File: ${file.name}`;
            messageElement.appendChild(fileLink);
        }

        const timestamp = document.createElement('span');
        timestamp.classList.add('timestamp');
        timestamp.textContent = getCurrentTime(); 
        messageElement.appendChild(timestamp);

        outputDiv.appendChild(messageElement);
    }

    function toggleRedirect() {
        const checkbox = document.getElementById('toggle-switch');
        if (checkbox.checked) {
            window.location.href = '{{site.baseurl}}/create_and_compete/realityroom';
        }
    }

    async function makePost(message) {
            const postTitle = 'Post';
            const postComment = message;
            const postChannelId = 1;

            const postData = {
                title: postTitle,
                comment: postComment,
                channel_id: postChannelId
            };

            try {
                const response = await fetch(`${pythonURI}/api/post`, {
                    ...fetchOptions,
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(postData)
                });

                if (!response.ok) {
                    throw new Error('Failed to add channel: ' + response.statusText);
                }
            } catch (error) {
                console.error('Error adding channel:', error);
                alert('Error adding channel: ' + error.message);
            }    
    }
</script>


