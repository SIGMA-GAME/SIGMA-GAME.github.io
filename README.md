<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Test Page</title>
</head>
<body>
    <h1>Game Test Page</h1>
    <canvas id="gameCanvas" width="400" height="400" style="background-color: #000;"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        ctx.fillStyle = 'red';
        ctx.fillRect(50, 50, 100, 100);
    </script>
</body>
</html>
