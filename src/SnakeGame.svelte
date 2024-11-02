<script>
  import { onMount } from "svelte";

  const gridSize = 20; // Number of rows and columns in the grid
  const initialSnake = [{ x: 10, y: 10 }];
  const directions = {
    ArrowUp: { x: 0, y: -1 },
    ArrowDown: { x: 0, y: 1 },
    ArrowLeft: { x: 1, y: 0 },
    ArrowRight: { x: -1, y: 0 },
  };

    // State Variables
  let snake = [...initialSnake];
  let aperol = getRandomPosition();
  let score = 0;
  let direction = directions.ArrowRight;
  let gameInterval;
  let isGameOver = false;

    // Start or restart the game
  onMount(() => {
    startGame();
    window.addEventListener("keydown", changeDirection);
    return () => {
      clearInterval(gameInterval);
      window.removeEventListener("keydown", changeDirection);
    };
  });

  function startGame() {
    clearInterval(gameInterval);
    snake = [...initialSnake];
    aperol = getRandomPosition();
    score = 0;
    direction = directions.ArrowRight;
    isGameOver = false;

    // Set interval for automatic movement
    gameInterval = setInterval(() => {
      moveSnake();
    }, 150);
  }

    // Snake movement

  function moveSnake() {
    if (isGameOver) return;

    const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

    // Check for collision

    if (checkCollision(head)) {
      isGameOver = true;
      clearInterval(gameInterval);
      console.log("Game Over - Collision detected.");
      return;
    }

    // Check if Aperol is consumed

    if (head.x === aperol.x && head.y === aperol.y) {
      score += 1;
      aperol = getRandomPosition();
    } else {
      snake.pop();
    }

    snake.unshift(head); // Add new head
    snake = [...snake]; // Trigger reactivity
  }

  // Change direction
  function changeDirection(event) {
    const newDirection = directions[event.key];
    if (newDirection && (newDirection.x !== -direction.x || newDirection.y !== -direction.y)) {
      direction = newDirection;
    }
  }

   // Check for collisions
  function checkCollision(head) {
    return (
      head.x < 0 ||
      head.x >= gridSize ||
      head.y < 0 ||
      head.y >= gridSize ||
      snake.some(segment => segment.x === head.x && segment.y === head.y)
    );
  }

  // Random position for Aperol
  function getRandomPosition() {
    let position;
    do {
      position = { x: Math.floor(Math.random() * gridSize), y: Math.floor(Math.random() * gridSize) };
    } while (snake.some(segment => segment.x === position.x && segment.y === position.y));
    return position;
  }

   // UI Checks for Snake and Aperol cells
  // Helper functions to check grid positions
  $: isSnakeCell = (x, y) => snake.slice(1).some(segment => segment.x === x && segment.y === y);
  $: isSnakeHead = (x, y) => snake.length && snake[0].x === x && snake[0].y === y;
  $: isAperolCell = (x, y) => aperol.x === x && aperol.y === y;
</script>


<style>
  .grid {
    display: grid;
    grid-template-columns: repeat(20, 60px); /* Increase cell size to 40px */
    grid-template-rows: repeat(20, 60px);
    gap: 2px; /* Adjust the gap between cells for better appearance */
    border: 2px solid #333;
    background-color: #cd72f4;
  }

  .cell {
    width: 60px; /* Match the grid cell size */
    height: 60px;
    background-color: #fefcfe;
  }

  .snake {
    background-color: #4b39ff(130, 36, 212);
  }

  .game-over {
    color: red;
    font-weight: bold;
    margin-top: 10px;
   }

  .image {
    width: 100%; /* Ensure the image fills the entire cell */
    height: 100%;
  }

  .button {
    color: rgb(13, 61, 166);
    box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
 
  }
  .score {
    color:white;
    font-size: larger;
    font-weight: 1000;
      }
</style>

<!-- Render the grid with larger snake and aperol images -->
<div class="grid">
  {#each Array(gridSize) as _, row}
    {#each Array(gridSize) as _, col}
      <div class="cell">
        {#if isSnakeHead(col, row)}
          <img src="/snake-head.png" alt="Snake Head" class="image" />
        {:else if isAperolCell(col, row)}
          <img src="/aperol.png" alt="Aperol" class="image" />
          {:else if isSnakeCell(col, row)}
          <img src="/aperol.png" alt="Snake Head" class="image" />
        {/if}
      </div>
    {/each}
  {/each}
</div>

<p class="score">Aperol drank: {score} </p>
{#if isGameOver}
  <p class="game-over">Drank too much? <button class="button" on:click={startGame}>Play Again</button></p>
{/if}
