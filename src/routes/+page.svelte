<script>
  export let data;

  let collectedBalls = [];

  // Generate random positions for the dragon balls
  function getRandomPosition() {
    return {
      top: `${Math.random() * 80}vh`, // Adjusted to prevent balls from going off-screen
      left: `${Math.random() * 80}vw`,
    };
  }

  // Generate dragon balls with random positions
  let dragonBalls = Array.from({ length: 7 }, (_, i) => ({
    id: i,
    ...getRandomPosition(),
  }));

  function collectBall(id) {
    console.log(`Collecting ball with id: ${id}`);
    if (!collectedBalls.includes(id)) {
      collectedBalls = [...collectedBalls, id];
    }
  }
</script>

<main>
  <section>
    <h1>{data.person.name}</h1>
    <div class="counter">Collected: {collectedBalls.length}/7</div>
  </section>

  <div class="grid-container">
    {#each dragonBalls as { id, top, left } (id)}
      {#if !collectedBalls.includes(id)}
        <div class="dragon-ball" style="top: {top}; left: {left};" on:click={collectBall(id)}>★</div>
      {/if}
    {/each}
  </div>
</main>

<style>
  main {
    background-image: radial-gradient(#46763c, #38613a);
    height: 100vh;
    overflow: hidden;
    cursor: url('/assets/arrow.png'), auto; /* Ensure you have a valid red cursor image */
  }

  .grid-container {
    background-image: url('/assets/grid-lines.png');
    background-position: center;
    background-repeat: repeat;
    height: 100%;
    position: relative;
  }

  .dragon-ball {
    background-image: radial-gradient(#fea609, #e85e00);
    width: 5rem;
    height: 5rem;
    border-radius: 50%;
    padding: 1rem;
    color: #c50401;
    outline: none;
    border: 0.1rem solid #fff0da;
    text-align: center;
    cursor: pointer;
    position: absolute;
    opacity: 0; /* Initially hidden */
    transition: opacity 0.3s; /* Smooth transition for opacity change */
  }

  .dragon-ball:hover {
    opacity: 1; /* Change opacity on hover */
  }

  .counter {
    font-size: 1.5rem;
    color: white;
    margin-bottom: 1rem;
  }
</style>
