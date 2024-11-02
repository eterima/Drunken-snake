<script>
	import SnakeGame from "./SnakeGame.svelte";
  </script>
  
  <main>
	<h1>Drunken Snake</h1>
	<SnakeGame />
  </main>
  
  <style>
	main {
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	  font-family: Arial, sans-serif;
	  background: rgb(75,57,255);
	  background: linear-gradient(142deg, rgba(75,57,255,1) 35%, rgba(75,221,249,1) 100%);
	}

	h1 {
		box-shadow: rgb(85, 91, 255) 0px 0px 0px 3px, rgb(31, 193, 27) 0px 0px 0px 6px, rgb(255, 217, 19) 0px 0px 0px 9px, rgb(255, 156, 85) 0px 0px 0px 12px, rgb(255, 85, 85) 0px 0px 0px 15px;
		background-color:white;
	}

	
  </style>
  