# W1P2020-NC-Groupe-G.R.A.G.G.
Projet Nuit Charrette

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">
    <title>Casse Brique Nuit charrette</title>
    <link rel="stylesheet" href="style.css">

</head>

<body>

    <canvas id="canvas1" width="1080" height="620"></canvas>

    <script>
        /*------VARIABLES------*/

        var canvas = document.getElementById("canvas1");
        var ctx = canvas.getContext("2d");

        /*-Emplacement Balle-*/
        var x = canvas.width / 2;
        var y = canvas.height - 40;

        /*-Vitesse Balle-*/
        var dx = 5;
        var dy = -5;
        var ballRadius = 10;

        /*-Barre de lancement-*/
        var paddleHeight = 10;
        var paddleWidth = 75;
        var paddleX = (canvas.width - paddleWidth) / 2;

        /*-Déplacement de la barre-*/
        var rightPressed = false;
        var leftPressed = false;

        /*-Champ de briques-*/
        var brickRowCount = 5;
        var brickColumnCount = 12;
        var brickWidth = 75;
        var brickHeight = 20;
        var brickPadding = 10;
        var brickOffsetTop = 30;
        var brickOffsetLeft = 30;

        /*-Score-*/
        var score = 0;

        /*-Variable brique + boucle répétition colonne*/
        var bricks = [];
        for (c = 0; c < brickColumnCount; c++) {
            bricks[c] = [];
            for (r = 0; r < brickRowCount; r++) {
                bricks[c][r] = {
                    x: 0,
                    y: 0,
                    status: 1
                };
            }
        }

        /*------EVENEMENT DEPLACEMENTS BARRE------*/

        document.addEventListener("keydown", keyDownHandler, false);
        document.addEventListener("keyup", keyUpHandler, false);

        function keyDownHandler(e) {
            if (e.keyCode == 39) {
                rightPressed = true;
            } else if (e.keyCode == 37) {
                leftPressed = true;
            }
        }

        function keyUpHandler(e) {
            if (e.keyCode == 39) {
                rightPressed = false;
            } else if (e.keyCode == 37) {
                leftPressed = false;
            }
        }

        /*------FONCTION BALLE------*/

        function drawBall() {
            ctx.beginPath();
            ctx.arc(x, y, ballRadius, 0, Math.PI * 2);
            ctx.fillStyle = "#000";
            ctx.fillStroke = "#000";
            ctx.stroke = "10"
            ctx.fill();
            ctx.closePath();

        }

        /*------FONCTION BARRE DE LANCEMENT------*/

        function drawPaddle() {
            ctx.beginPath();
            ctx.rect(paddleX, canvas.height - paddleHeight, paddleWidth, paddleHeight);
            ctx.fillStyle = "#000";
            ctx.fill();
            ctx.closePath();

        }

        /*------FONCTION BRIQUE------*/

        function drawBricks() {
            for (c = 0; c < brickColumnCount; c++) {
                for (r = 0; r < brickRowCount; r++) {
                    if (bricks[c][r].status == 1) {
                        var brickX = (c * (brickWidth + brickPadding)) + brickOffsetLeft;
                        var brickY = (r * (brickHeight + brickPadding)) + brickOffsetTop;
                        bricks[c][r].x = brickX;
                        bricks[c][r].y = brickY;
                        ctx.beginPath();
                        ctx.rect(brickX, brickY, brickWidth, brickHeight);
                        ctx.fillStyle = "#000";
                        ctx.fill();
                        ctx.closePath();
                    }
                }
            }
        }

        /*------FONCTION COLLISION BRIQUE------*/

        function collisionDetection() {
            for (c = 0; c < brickColumnCount; c++) {
                for (r = 0; r < brickRowCount; r++) {
                    var b = bricks[c][r];
                    if (b.status == 1) {
                        if (x > b.x && x < b.x + brickWidth && y > b.y && y < b.y + brickHeight) {
                            dy = -dy;
                            b.status = 0;

                            /*-Message de victoire-*/
                            score++;
                            if (score == brickRowCount * brickColumnCount) {
                                alert("PASSAGE A NIVEAU");
                                document.location.reload();
                            }
                        }
                    }
                }
            }
        }

        /*------FONCTION SCORE------*/

        function drawScore() {
            ctx.font = "16px Arial";
            ctx.fillStyle = "#000";
            ctx.fillText("Score: " + score, 8, 20);
        }


        /*------CLEAR CANVAS & DESSINER ELEMENTS------*/

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBall();
            drawPaddle();
            drawBricks();
            collisionDetection();

            /*------CANVAS REBOND------*/

            if (x + dx > canvas.width - ballRadius || x + dx < ballRadius) {
                dx = -dx;
            }

            /*-Game Over + Détection barre-*/
            if (y + dy < ballRadius) {
                dy = -dy;
            } else if (y + dy > canvas.height - ballRadius) {
                if (x > paddleX && x < paddleX + paddleWidth) {
                    dy = -dy;
                } else {

                    alert("VOUS ÊTES MORT");
                    document.location.reload();
                }
            }

            /*------CONDITIONS DEPLACEMENTS------*/

            if (rightPressed && paddleX < canvas.width - paddleWidth) {
                paddleX += 7;
            } else if (leftPressed && paddleX > 0) {
                paddleX -= 7;
            }

            x += dx;
            y += dy;

        }

        setInterval(draw, 10);

    </script>


</body>

</html>



/*------Le CSS

* {
    padding: 0;
    margin: 0;
}

canvas {
    margin: 0 auto;
    display: block;
    background: #eee;

}

------*/
