---
layout: base 
title: snake 
search_exclude: false
permalink: /snake/
---


<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<script>
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer();

    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    // Set camera position
    camera.position.set(0, 10, 20);
    camera.lookAt(0, 0, 0);

    // Add a basic light
    const light = new THREE.PointLight(0xffffff, 1, 100);
    light.position.set(10, 20, 10);
    scene.add(light);

    // Create the snake as an array of cubes
    const snake = [];
    const snakeMaterial = new THREE.MeshStandardMaterial({ color: 0x00ff00 });

    for (let i = 0; i < 3; i++) {
        const segment = new THREE.Mesh(new THREE.BoxGeometry(1, 1, 1), snakeMaterial);
        segment.position.set(i, 0, 0);
        scene.add(segment);
        snake.push(segment);
    }

    // Create food
    const foodMaterial = new THREE.MeshStandardMaterial({ color: 0xff0000 });
    const food = new THREE.Mesh(new THREE.SphereGeometry(0.5, 32, 32), foodMaterial);
    food.position.set(Math.random() * 10 - 5, 0, Math.random() * 10 - 5);
    scene.add(food);

    let direction = new THREE.Vector3(0, 0, 0); // Moving along the X-axis
    const speed = 0.1;

    function moveSnake() {
        // Move the head
        const head = snake[0];
        head.position.add(direction);

        // Move the body segments
        for (let i = snake.length - 1; i > 0; i--) {
            snake[i].position.copy(snake[i - 1].position);
        }
    }

    function checkFoodCollision() {
        const head = snake[0];
        if (head.position.distanceTo(food.position) < 1) {
            // Reposition food
            food.position.set(Math.random() * 10 - 5, 0, Math.random() * 10 - 5);

            // Grow the snake
            const tail = snake[snake.length - 1];
            const newSegment = new THREE.Mesh(new THREE.BoxGeometry(1, 1, 1), snakeMaterial);
            newSegment.position.copy(tail.position);
            scene.add(newSegment);
            snake.push(newSegment);
        }
    }

    function checkCollision() {
        const head = snake[0];

        // Check for wall collisions
        if (Math.abs(head.position.x) > 20 || Math.abs(head.position.z) > 20) {
            alert('Game Over');
            window.location.reload();
        }

        // Check for self-collision
        for (let i = 1; i < snake.length; i++) {
            if (head.position.distanceTo(snake[i].position) < 0.5) {
                alert('Game Over');
                window.location.reload();
            }
        }
    }

    document.addEventListener('keydown', (event) => {
        switch (event.key) {
            case 'ArrowUp': direction.set(0, 0, -1); break;  // Move forward
            case 'ArrowDown': direction.set(0, 0, 1); break; // Move backward
            case 'ArrowLeft': direction.set(-1, 0, 0); break; // Move left
            case 'ArrowRight': direction.set(1, 0, 0); break; // Move right
        }
    });

    function animate() {
        requestAnimationFrame(animate);

        moveSnake();
        checkFoodCollision();
        checkCollision();

        renderer.render(scene, camera);
    }

    animate();
</script>
