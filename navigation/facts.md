---
layout: base
title: Game
search_exclude: true
permalink: /history/
---
<style>
    body {
        font-family: 'Georgia', serif;
        text-align: center;
        background-color: #f4f4f9;
        margin: 0;
        padding: 0;
        color: #333;
    }
    header {
        margin-top: 50px;
    }
    .fact-box {
        background-color: #ffffff;
        margin: 20px auto;
        padding: 40px;
        width: 50%;
        border-radius: 15px;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        font-size: 1.2em;
        line-height: 1.6;
    }
    button {
        color: white;
        border: none;
        padding: 12px 25px;
        margin-top: 20px;
        cursor: pointer;
        border-radius: 30px;
        font-size: 18px;
        transition: background-color 0.3s ease;
    }
    button:hover {
        background-color: #0056b3;
    }
    footer {
        margin-top: 30px;
        color: #555;
        font-size: 0.9em;
    }
    a {
        color: #007BFF;
        text-decoration: none;
    }
    a:hover {
        text-decoration: underline;
    }
</style>

<header>
    <h1>Random Chess Fact</h1>
</header>
<div class="fact-box">
    <p id="chess-fact" style="color: black;">Click the button to get a random chess fact!</p>
</div>
<button onclick="fetchFact()">Get Another Fact</button>
<footer>
    <p><a href="sprint4_frontend/game/">Now that you've learned about it, is it time to play?</a></p>
</footer>

<script>
    function fetchFact() {
    fetch('http://127.0.0.1/api/chess/random_fact') // Replace with your deployed backend URL
        .then(response => response.json())
        .then(data => {
            document.getElementById('chess-fact').textContent = data.fact;
        })
        .catch(error => {
            document.getElementById('chess-fact').innerHTML = 'Could not fetch a fact. Please try again later.';
        });
}

</script>
