---
layout: base
title: Flocker Social Media Site 
search_exclude: true
hide: true
menu: nav/home.html
---

<style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            line-height: 1.6;
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px 40px;
            /* background: #fff; */
        }

        header h1 {
            margin: 0;
            font-size: 24px;
        }

        nav a {
            margin: 0 15px;
            text-decoration: none;
            color: black;
            font-weight: bold;
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

        .hero-text button:first-child {
            background: black;
            color: white;
            border: none;
        }

        .hero-text button:last-child {
            background: white;
            border: 2px solid black;
            color: black;
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
</style>

<section class="hero">
        <div class="hero-text">
            <h1>Pawnsly</h1>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla at pellentesque dui. Etiam fringilla in tellus id feugiat. Duis nec ornare orci.</p>
            <button>Sign Up!</button>
            <button>Learn More!</button>
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