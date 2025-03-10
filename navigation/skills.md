---
layout: base
title: Skills Rating
search_exclude: true
permalink: /skills/
---
<style>
    body {
        margin: 0;
        font-family: Arial, sans-serif;
        background-color: #121212;
        color: #E0E0E0;
    }

    header.page {
        background-color: #1A1A1A;
        padding: 20px;
        text-align: center;
        font-size: 24px;
        font-weight: bold;
        border-radius: 15px;
        margin: 20px;
    }

    .subtitle {
        margin-top: -10px;
        font-size: 16px;
        color: #A0A0A0;
    }

    .main {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 30px;
    }

    table {
        width: 80%;
        border-collapse: collapse;
        margin-top: 20px;
    }

    table th,
    table td {
        padding: 10px;
        text-align: center;
        border: 1px #A0A0A0;
    }

    table th {
        background-color: #252525;
        color: #E0E0E0;
    }

    table td {
        background-color: #1E1E1E;
    }

    .input-form {
        display: flex;
        justify-content: center;
        gap: 10px;
        margin-top: 20px;
    }

    .input-form input {
        padding: 10px;
        font-size: 16px;
        border: 1px solid #A0A0A0;
        border-radius: 5px;
        background-color: #222222;
        color: white;
        outline: none;
    }

    .input-form button {
        padding: 10px 20px;
        font-size: 16px;
        border: none;
        background-color: #5DADE2;
        color: white;
        border-radius: 5px;
        cursor: pointer;
    }

    .input-form button:hover {
        background-color: #4A90C2;
    }

    .action-buttons {
        display: flex;
        justify-content: center;
        gap: 5px;
    }

    .action-buttons button {
        padding: 5px 10px;
        font-size: 14px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }

    .update-button {
        background-color: #3A3A3A;
        color: white;
    }

    .update-button:hover {
        background-color: #505050;
    }

    .delete-button {
        background-color: #8B0000;
        color: white;
    }

    .delete-button:hover {
        background-color:  #A00000;
    }
</style>

<header class="page">
    Skill Levels Table
    <div class="subtitle">Track your skills and levels easily</div>
</header>

<div class="main">
    <!-- Input Form for Adding Skills -->
    <form class="input-form" id="addSkillForm">
        <input type="text" id="skillInput" placeholder="Skill Name" required />
        <input type="number" id="rankInput" placeholder="Level (1-10)" min="1" max="10" required />
        <button type="submit" id="addButton">Add Skill</button>
    </form>

    <table>
        <thead>
            <tr>
                <th>ID</th>
                <th>Skill</th>
                <th>Level</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody id="skillsTable">
            <!-- Rows will be added dynamically here -->
        </tbody>
    </table>
</div>


<script type="module">
    import { pythonURI, fetchOptions } from '/sprint4_frontend/assets/js/api/config.js';

    // Fetch skills from the database and display them in the table (GET)
    async function fetchSkills() {
        try {
            const response = await fetch(`${pythonURI}/api/skill`, fetchOptions);
            if (!response.ok) throw new Error('Failed to fetch skills.');

            const skills = await response.json();
            const table = document.getElementById('skillsTable');
            table.innerHTML = ''; // Clear table before populating

            skills.forEach(skill => addRowToTable(skill.id, skill.skill_name, skill.skill_level));
        } catch (error) {
            console.error('Error fetching skills:', error);
        }
    }

    async function getUser() {
        try {
            const response = await fetch(`${pythonURI}/api/user`, fetchOptions);
            if (!response.ok) throw new Error('Failed to fetch user.');

            const user = await response.json();
            console.log("all data", user)
            console.log("id", user['id'])
            return user['id']
        } catch (error) {
            console.error('Error fetching skills:', error);
        }
    }

    // Add a new skill to the database and table (POST)
    async function addSkill(event) {
        event.preventDefault(); // Prevent form submission

        const skillInput = document.getElementById('skillInput').value.trim();
        const rankInput = document.getElementById('rankInput').value.trim();

        const userId = await getUser();

        if (skillInput && rankInput) {
            const newSkill = {
                skill_name: skillInput,
                skill_level: rankInput,
                user_id: userId,
            };

            try {
                const response = await fetch(`${pythonURI}/api/skill`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(newSkill),
                });

                if (!response.ok) throw new Error('Failed to add skill.');

                // Add the new row to the table
                const result = await response.json();
                addRowToTable(result.id, result.skill_name, result.skill_level);

                document.getElementById('skillInput').value = '';
                document.getElementById('rankInput').value = '';
            } catch (error) {
                console.error('Error adding skill:', error);
            }
        }
    }

    // Add a new row to the table (helper function)
    async function addRowToTable(id, skillName, level) {
        const table = document.getElementById('skillsTable');
        const row = table.insertRow();

        row.insertCell(0).textContent = id;
        row.insertCell(1).textContent = skillName;
        row.insertCell(2).textContent = level;

        const actionCell = row.insertCell(3);
        actionCell.classList.add('action-buttons');

        // Update Button
        const updateButton = document.createElement('button');
        updateButton.textContent = 'Update';
        updateButton.classList.add('update-button');
        const userId = await getUser();
        updateButton.onclick = () => updateRow(row, id, userId);

        // Delete Button
        const deleteButton = document.createElement('button');
        deleteButton.textContent = 'Delete';
        deleteButton.classList.add('delete-button');
        deleteButton.onclick = () => deleteRow(row, id);

        actionCell.appendChild(updateButton);
        actionCell.appendChild(deleteButton);
    }

    // Update a skill in the database and table (PUT)
    async function updateRow(row, id, userId) {
        const skillCell = row.cells[1];
        const levelCell = row.cells[2];

        const newSkillName = prompt('Update Skill Name:', skillCell.textContent);
        const newLevel = prompt('Update Level:', levelCell.textContent);

        if (newSkillName !== null && newLevel !== null) {
            const updatedSkill = {
                id: id,
                skill_name: newSkillName.trim(),
                skill_level: newLevel.trim(),
                user_id: userId,
            };

            try {
                const response = await fetch(`${pythonURI}/api/skill`, {
                    method: 'PUT',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(updatedSkill),
                });

                if (!response.ok) throw new Error('Failed to update skill.');

                // Update the table row on success
                skillCell.textContent = updatedSkill.skill_name;
                levelCell.textContent = updatedSkill.skill_level;
            } catch (error) {
                console.error('Error updating skill:', error);
            }
        }
    }

    // Delete a skill from the database and remove the row (DELETE)
    async function deleteRow(row, id) {
        if (confirm('Are you sure you want to delete this skill?')) {
            try {
                const response = await fetch(`${pythonURI}/api/skill`, {
                    method: 'DELETE',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ id }),
                });

                if (!response.ok) throw new Error('Failed to delete skill.');

                // Remove the row from the table on success
                row.remove();
            } catch (error) {
                console.error('Error deleting skill:', error);
            }
        }
    }

    // Attach event listener to the form
    document.getElementById('addSkillForm').addEventListener('submit', addSkill);
    fetchSkills();
    getUser();

</script>
