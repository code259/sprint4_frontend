---
layout: base
title: About
search_exclude: true
permalink: /about/
---

<h3 id="bioList"></h3>

<style>
    #bioList {
        /*styling for the list container */
        padding: 20px;
        margin: 0;
    }

    #bioList li {
   
        padding: 10px;
        margin: 5px 0;
        background-color: #f0f0f0;
        border-radius: 5px;
        list-style: none;
    }
</style>


<script type="module">
    import { pythonURI, fetchOptions } from '/sprint4_frontend/assets/js/api/config.js';

    async function fetchBio() {
        try {
            const response = await fetch(`${pythonURI}/api/student/bulk_dynamic`, fetchOptions);
            if (!response.ok) {
                throw new Error('Failed to fetch groups: ' + response.statusText);
            }

            const bio = await response.json();
            const bioList = document.getElementById('bioList');

            for (const person of bio) {
                const { dob, name, color } = person;
                const newListItem = document.createElement('li');
                newListItem.textContent = `My name is ${name}, and I was born on ${dob}, and my favorite color is ${color}.`;
                bioList.appendChild(newListItem);
            }
        } catch (error) {
            console.error('Error fetching groups:', error);
        }
    }

    fetchBio();
</script>