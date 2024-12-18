---
layout: search 
title: Leaderboard 
search_exclude: true
permalink: /leaderboard/
---
{% include nav/home.html %}

const players = [
    { name: 'User 1', wins: 15 },
    { name: 'User 2', wins: 10 },
    { name: 'User 3', wins: 8 },
    { name: 'User 4', wins: 5 },
    { name: 'User 5', wins: 3 }
];

// Function to sort players by wins (descending order)
function sortPlayersByWins(players) {
    return players.sort((a, b) => b.wins - a.wins);
}

// Function to generate the podium leaderboard
function generateLeaderboard(players) {
    const sortedPlayers = sortPlayersByWins(players);
    const leaderboard = document.getElementById('leaderboard');

    leaderboard.innerHTML = ''; // Clear existing leaderboard

    // Podium for the top 3 players
    const podiumDiv = document.createElement('div');
    podiumDiv.className = 'podium';

    const podiumPlaces = ['gold', 'silver', 'bronze'];
    for (let i = 0; i < 3; i++) {
        const player = sortedPlayers[i];
        if (player) {
            const podiumStep = document.createElement('div');
            podiumStep.className = `podium-step ${podiumPlaces[i]}`;
            podiumStep.innerHTML = `
                <div class="podium-rank">${i + 1}</div>
                <div class="podium-name">${player.name}</div>
                <div class="podium-wins">${player.wins} Wins</div>
            `;
            podiumDiv.appendChild(podiumStep);
        }
    }

    leaderboard.appendChild(podiumDiv);

    // Remaining players
    sortedPlayers.slice(3).forEach((player, index) => {
        const playerDiv = document.createElement('div');
        playerDiv.className = 'player';
        playerDiv.innerHTML = `
            <span class="rank">${index + 4}.</span>
            <span class="name">${player.name}</span>
            <span class="wins">Wins: ${player.wins}</span>
        `;
        leaderboard.appendChild(playerDiv);
    });
}

// Function to fetch group data and populate a dropdown menu
async function fetchGroups(pythonURI) {
    try {
        const response = await fetch(`${pythonURI}/api/post`, fetchOptions);
        if (!response.ok) {
            throw new Error('Failed to fetch groups: ' + response.statusText);
        }
        const groups = await response.json();
        const groupSelect = document.getElementById('group_id');
        groups.forEach(group => {
            const option = document.createElement('option');
            option.value = group.id;
            option.textContent = group.name;
            groupSelect.appendChild(option);
        });
    } catch (error) {
        console.error('Error fetching groups:', error);
    }
}

// Fetch options for API calls
export const fetchOptions = {
    method: 'GET',
    mode: 'cors',
    cache: 'default',
    credentials: 'include',
    headers: {
        'Content-Type': 'application/json',
        'X-Origin': 'client'
    },
};

// Initial setup
window.onload = function () {
    generateLeaderboard(players);
    const pythonURI = `${pythonURI}/api/post`; // Replace with actual URI
    fetchGroups(pythonURI);
};

// HTML and CSS
document.body.innerHTML = `
    <div id="leaderboard-container">
        <h1>Weekly Chess Leaderboard</h1>
        <div id="leaderboard"></div>
        <select id="group_id">
            <option value="">Select a group</option>
        </select>
    </div>
`;

document.head.innerHTML += `
    <style>
        /* General Container Styling */
        #leaderboard-container {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 50px;
        }

        #leaderboard {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 20px;
        }

        /* Podium Styling */
        .podium {
            display: flex;
            justify-content: center;
            align-items: flex-end;
            gap: 10px;
            margin-bottom: 20px;
            height: 200px;
        }

        .podium-step {
            width: 100px;
            text-align: center;
            color: white;
            font-weight: bold;
            padding: 10px;
            box-sizing: border-box;
        }

        .podium-step.gold {
            background-color: gold;
            height: 120px;
        }

        .podium-step.silver {
            background-color: silver;
            height: 100px;
        }

        .podium-step.bronze {
            background-color: #cd7f32;
            height: 80px;
        }

        .podium-rank {
            font-size: 24px;
        }

        .podium-name {
            margin: 10px 0;
        }

        .podium-wins {
            font-size: 14px;
        }

        /* Player List Styling */
        .player {
            display: flex;
            align-items: center;
            padding: 5px;
            border-bottom: 1px solid #ccc;
            width: 300px;
        }

        .rank {
            font-weight: bold;
            margin-right: 10px;
        }

        .name {
            flex: 1;
        }

        .wins {
            font-weight: bold;
        }
    </style>
`;
