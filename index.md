---
layout: base
title: Pawnsy
search_exclude: true
hide: true
---

<style>
        header h1 {
            margin: 0;
            font-size: 24px;
        }

        .hero {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 50px;
        }

        .hero-text {
            max-width: 50%;
        }

        .hero-text h1 {
            font-size: 48px;
            margin-bottom: 20px;
        }

        .hero-text p {
            margin-bottom: 30px;
        }

        .hero-text button {
            padding: 10px 20px;
            margin-right: 10px;
            font-size: 16px;
            cursor: pointer;
        }

        .hero-image img {
            width: 300px;
            border-radius: 10px;
        }

        .stats {
            display: flex;
            justify-content: center;
            margin: 50px 0;
        }

        .stats div {
            margin: 0 20px;
            text-align: center;
        }

        .stats div h2 {
            font-size: 36px;
            margin: 0;
        }

        .stats div p {
            margin: 5px 0 0;
            font-size: 16px;
            color: gray;
        }
        button {
            background-color: transparent;
            color: white;
            border: 2px solid white;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            border-radius: 5px;
            transition: background-color 0.3s ease, color 0.3s ease;
        }

        button:hover {
            background-color: white;
            color: black;
        }
</style>


<section class="hero">
        <div class="hero-text">
            <h1 class="title"></h1>
            <p>Pawnsly is a website with a focus in the game of Chess. Play against three different ai difficulty levels or login to play against friends online. By using Gemini Integration, Pawnsly can analyze your chess boards and turn any losing situation upside down! </p>
            <button id="login-btn">Sign Up!</button>
            <button id="signup-btn">Login!</button>
        </div>
        <div class="hero-image">
            <img src="https://images.unsplash.com/photo-1560174038-da43ac74f01b?q=80&w=2914&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D" alt="Chess Cat">
        </div>
</section>

<section class="stats">
        <div>
            <h2>200</h2>
            <p>Users on the website</p>
        </div>
        <div>
            <h2>1000+</h2>
            <p>Users on the website</p>
        </div>
</section>

<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/TextPlugin.min.js"></script>

<script>
    signup_btn = document.getElementById('signup-btn').onclick = function () {
        location.href = "{{site.baseurl}}/login"
    }
    login_btn = document.getElementById('login-btn').onclick = function () {
        location.href = "{{site.baseurl}}/login"
    }

    gsap.registerPlugin(TextPlugin)
    var timeline = gsap.timeline({delay: 0.2});
    timeline.to(".hero-image", {rotation: 360, x: 5, duration: 1})
    .to(".title", {duration: 1, text: "Pawnsy", ease: "none"})


    window.onload = async function() {
        try {
            const response = await fetch('/check-auth', { credentials: 'include' });
            const data = await response.json();
            console.log(data)
            if (data.isAuthenticated) {
                console.log('logged');
            }
        } catch (error) {
            console.error('Error checking authentication', error);
        }
    };
</script>

