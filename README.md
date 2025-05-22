# mythos-maze
Maze game with quizzes about Ancient Greece
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Maze of the Myths</title>
  <style>
    body {
      font-family: 'Georgia', serif;
      background-color: #fdf6e3;
      text-align: center;
    }

    h1 {
      margin-top: 20px;
    }

    #maze {
      display: grid;
      grid-template-columns: repeat(10, 40px);
      grid-template-rows: repeat(10, 40px);
      gap: 2px;
      margin: 20px auto;
      width: fit-content;
    }

    .cell {
      width: 40px;
      height: 40px;
      border: 1px solid #333;
    }

    .wall {
      background-color: #5a3e36;
    }

    .player {
      background-color: #ffd700;
    }

    .quiz {
      background-color: #9bc1bc;
    }

    .goal {
      background-color: #4caf50;
    }

    #message {
      margin-top: 20px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>Maze of the Myths</h1>
  <p>Use arrow keys to move. Answer quizzes to progress!</p>
  <div id="maze"></div>
  <div id="message"></div>

  <script>
    const width = 10;
    const height = 10;

    const mazeLayout = [
      // W = wall, P = player, Q = quiz, G = goal, _ = path
      'WWWWWWWWWW',
      'WP___W___W',
      'W_W_W_WW_W',
      'W_W_W___QW',
      'W_WWW_WWWW',
      'W___W____W',
      'WWW_W_WW_W',
      'W_Q___W__W',
      'W_WWWWW_GW',
      'WWWWWWWWWW'
    ];

    const maze = document.getElementById('maze');
    const message = document.getElementById('message');

    let playerX = 1, playerY = 1;

    function drawMaze() {
      maze.innerHTML = '';
      for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
          const cell = document.createElement('div');
          cell.classList.add('cell');
          const char = mazeLayout[y][x];

          if (x === playerX && y === playerY) {
            cell.classList.add('player');
          } else if (char === 'W') {
            cell.classList.add('wall');
          } else if (char === 'Q') {
            cell.classList.add('quiz');
          } else if (char === 'G') {
            cell.classList.add('goal');
          }

          maze.appendChild(cell);
        }
      }
    }

    function handleQuiz(x, y) {
      const question = {
        question: "Who is the Greek god of the sea?",
        options: ["Ares", "Poseidon", "Hermes", "Apollo"],
        answer: "Poseidon"
      };

      const userAnswer = prompt(
        `${question.question}\n${question.options.join('\n')}`
      );

      if (userAnswer && userAnswer.toLowerCase() === question.answer.toLowerCase()) {
        alert("Correct! You may proceed.");
        return true;
      } else {
        alert("Incorrect! You're sent back to the start.");
        playerX = 1;
        playerY = 1;
        return false;
      }
    }

    function movePlayer(dx, dy) {
      const newX = playerX + dx;
      const newY = playerY + dy;

      if (newX < 0 || newX >= width || newY < 0 || newY >= height) return;

      const targetChar = mazeLayout[newY][newX];
      if (targetChar === 'W') return;

      if (targetChar === 'Q') {
        const passed = handleQuiz(newX, newY);
        if (!passed) return;
        // Remove the quiz from the layout
        mazeLayout[newY] = mazeLayout[newY].substring(0, newX) + '_' + mazeLayout[newY].substring(newX + 1);
      }

      if (targetChar === 'G') {
        message.innerText = "🎉 You've escaped the Maze of the Myths! Well done!";
      }

      playerX = newX;
      playerY = newY;
      drawMaze();
    }

    document.addEventListener('keydown', (e) => {
      switch (e.key) {
        case 'ArrowUp': movePlayer(0, -1); break;
        case 'ArrowDown': movePlayer(0, 1); break;
        case 'ArrowLeft': movePlayer(-1, 0); break;
        case 'ArrowRight': movePlayer(1, 0); break;
      }
    });

    drawMaze();
  </script>
</body>
</html>
