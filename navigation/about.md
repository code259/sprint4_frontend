---
layout: base
title: About
search_exclude: true
permalink: /about/
---

<h3 id="bioList"></h3>

<script type="module">
    import { pythonURI, fetchOptions } from '/sprint4_frontend/assets/js/api/config.js';

    async function fetchBio() {
        try {
            const response = await fetch(`${pythonURI}/api/student/bulk`, fetchOptions);
            if (!response.ok) {
                throw new Error('Failed to fetch groups: ' + response.statusText);
            }

            const bio = await response.json();
            const bioList = document.getElementById('bioList');

            for (const key in bio) {
                if (bio.hasOwnProperty(key)) {
                    const person = bio[key];
                    const { DOB, Name, 'Favorite Color': color } = person;
                    const newListItem = document.createElement('li');
                    newListItem.textContent = `My name is ${Name}, and I was born on ${DOB}, and my favorite color is ${color}.`;
                    bioList.appendChild(newListItem);
                }
            }
        } catch (error) {
            console.error('Error fetching groups:', error);
        }
    }

    fetchBio();
</script>