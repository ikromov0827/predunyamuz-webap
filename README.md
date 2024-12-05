# predunyamuz-webap
http://t.me/PredunyamUz_bot/PredunyamUz_bot
@dp.message_handler(commands=['game'])
async def send_game(message: types.Message):
    await bot.send_game(chat_id=message.chat.id, game_short_name="dino_run")

@dp.callback_query_handler(lambda call: call.game_short_name == "dino_run")
async def game_callback(call: types.CallbackQuery):
    game_url = "https://ikromov0827.github.io/predunyamuz-webap/"
    await call.answer(url=game_url)

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dino O'yini</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            overflow: hidden;
        }
        canvas {
            display: block;
            margin: 0 auto;
            background: #fff;
            border: 2px solid #000;
        }
    </style>
</head>
<body>
    <h1 style="text-align: center;">Google Dino O'yini</h1>
    <canvas id="gameCanvas" width="800" height="300"></canvas>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        // Game settings
        let gameSpeed = 5;
        let gravity = 1.5;

        // Player
        const dino = {
            x: 50,
            y: 200,
            width: 50,
            height: 50,
            dy: 0,
            jumping: false,
            draw() {
                ctx.fillStyle = "black";
                ctx.fillRect(this.x, this.y, this.width, this.height);
            },
            update() {
                if (this.jumping) {
                    this.dy += gravity;
                    this.y += this.dy;

                    if (this.y > 200) {
                        this.y = 200;
                        this.jumping = false;
                        this.dy = 0;
                    }
                }
            },
            jump() {
                if (!this.jumping) {
                    this.jumping = true;
                    this.dy = -20;
                }
            }
        };

        // Obstacles
        const obstacles = [];
        const obstacleWidth = 30;
        const obstacleHeight = 50;

        function createObstacle() {
            const x = canvas.width;
            obstacles.push({ x, width: obstacleWidth, height: obstacleHeight });
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
