        }

        function drawObstacles() {
            ctx.fillStyle = "red";
            for (let i = 0; i < obstacles.length; i++) {
                ctx.fillRect(obstacles[i].x, canvas.height - obstacleHeight, obstacleWidth, obstacleHeight);
                obstacles[i].x -= gameSpeed;

                // Remove off-screen obstacles
                if (obstacles[i].x + obstacleWidth < 0) {
                    obstacles.splice(i, 1);
                }

                // Collision detection
                if (
                    dino.x < obstacles[i].x + obstacleWidth &&
                    dino.x + dino.width > obstacles[i].x &&
                    dino.y + dino.height > canvas.height - obstacleHeight
                ) {
                    alert("O'yin tugadi! Siz yutqazdingiz!");
                    document.location.reload();
                }
            }
        }

        // Game Loop
        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            dino.draw();
            dino.update();
            drawObstacles();
            requestAnimationFrame(gameLoop);
        }

        // Control player
        window.addEventListener("keydown", (e) => {
            if (e.code === "Space" || e.code === "ArrowUp") {
                dino.jump();
            }
        });

        // Spawn obstacles
        setInterval(createObstacle, 2000);

        // Start game
        gameLoop();
    </script>
</body>
</html>
