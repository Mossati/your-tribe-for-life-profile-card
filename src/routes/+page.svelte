<script>
  export let data;

  let count = 0;
  let collected = [];
  const totalDragonBalls = 7;
  let pingActive = false;
  let showMenu = true;
  let gameStarted = false;
  let wishGranted = false;

  // Haal de menu-sectie weg en start het spel
  function removeMenu() {
    showMenu = false;
    gameStarted = true;
    startPingEffect();
  }

  // Functie om random posities te genereren voor de dragon balls
  function randomPosition() {
    return {
      top: `${Math.random() * 80}vh`,
      left: `${Math.random() * 80}vw`
    };
  }

  // Voeg een dragon ball toe aan de array en update de counter
  function addBall(index) {
    if (!collected.includes(index)) {
      collected.push(index); // Voeg bal toe aan verzameling
      count++;
    }
  }

  // Ping effect: zet elke 3 seconden de opacity op 0.5 voor 1 seconde
  function startPingEffect() {
    setInterval(() => {
      pingActive = true;
      setTimeout(() => {
        pingActive = false;
      }, 1000);
    }, 3000);
  }

  function grantWish() {
    wishGranted = true;
  }
</script>

<main>
  {#if showMenu}
    <section class="menu">
      <h1 class="title">Dragon Radar</h1>
      <p class="mission">Collect all Dragon Balls to view my profile card</p>
      <button class="button" on:click={removeMenu}>Play</button>
    </section>
  {/if}

  {#if gameStarted}
    <p class="title counter">Dragon Balls collected: {count}/{totalDragonBalls}</p>

    <div class="grid-container">
      {#each Array(totalDragonBalls) as _, index}
      {#if !collected.includes(index)}
        <button
          aria-label="dragon-ball"
          class="dragon-ball"
          on:click={() => addBall(index)}
          class:ping={pingActive}
          style="top: {randomPosition().top}; left: {randomPosition().left};">
        </button>
      {/if}
    {/each}
    </div>
  {/if}

  {#if count === totalDragonBalls && wishGranted === false}
  <section class="shenron-awakening">
    <h2>Do you wish to see the profile card?</h2>
    <button class="button" on:click={grantWish}>Wish!</button>
  </section>
  {/if}

  {#if wishGranted}
    <section class="profile-card">
      <h2 class="title">Profile Card</h2>
      <img src={data.person.avatar} alt="avatar">
      <h3>{data.person.name + ' ' + data.person.prefix + ' ' + data.person.surname}</h3>
      <p><span>Nickname</span> {data.person.nickname}</p>
      <p><span>Biography</span> {data.person.bio}</p>
      <p><span>Squad</span> 2C</p>
      <a class="button" href="{data.person.website}">{data.person.github_handle}</a>
    </section>
  {/if}
</main>

<style>
  /* =================================================== */
  /* Global */
  /* =================================================== */
  main {
    background-image: url('/assets/grid-lines.png'), linear-gradient(190deg, var(--background-radar-primary), var(--background-radar-secondary), var(--background-radar-tertiary));
    background-position: center;
    background-repeat: repeat;
    height: 100vh;
    overflow: hidden;
    cursor: url('/assets/arrow.png'), auto;
    /* border: 1rem solid transparent; 
    border-image: linear-gradient(to right, var(--background-border-primary), var(--background-border-secondary)); 
    border-image-slice: 1; */
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: relative;
  }

  .grid-container {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .counter {
    margin-top: 1rem;
    display: flex;
    flex-direction: row;
    align-items: center;
  }

  .counter::after {
    content: "";
    width: 1rem;
    height: 1rem;
    display: inline-block;
    background-image: radial-gradient(var(--background-ball-primary), var(--background-ball-secondary));
    border-radius: 50%;
    margin-left: 0.5rem;
  }
  /* =================================================== */
  /* Menu en Profile Card */
  /* =================================================== */
  .menu, .profile-card {
    z-index: 2;
    background-image: radial-gradient(var(--background-main-primary), var(--background-main-secondary));
    padding: 1rem;
    color: var(--text-primary);
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    gap: 1rem;
  }

  .menu {
    background-image: url('/assets/goku.png'), radial-gradient(var(--background-main-primary), var(--background-main-secondary));
    background-repeat: no-repeat;
  }

  .title {
    background-color: var(--background-header-primary);
    color: #fefefe;
    padding: 1rem;
    border: 0.4rem solid var(--text-secondary);
  }

  .mission {
    color: var(--text-secondary);
    background-color: rgba(0, 0, 0, 0.5);
    border-radius: 1rem;
    padding: 0.5rem;
  }

  .profile-card img {
    width: 10rem;
    height: 10rem;
    border: 0.4rem solid var(--text-secondary);
  }

  span {
    color: var(--text-secondary);
    font-weight: 600;
    display: block;
    margin-bottom: 0.5rem;
  }

  .shenron-awakening {
    z-index: 2;
    background-image: url('/assets/shenron.png');
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
    padding: 1rem;
    color: var(--text-secondary);
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    gap: 1rem;
  }
  /* =================================================== */
  /* Buttons */
  /* =================================================== */
  .button {
    background-color: var(--background-button-primary);
    color: var(--text-secondary);
    padding: 1rem 5rem;
    border-radius: 1rem;
    outline: none;
    text-decoration: none;
    border: 0.1rem solid var(--text-secondary);
    font-weight: 600;
    font-size: 1rem;
  }

  .button:hover {
    background: transparent;
  }
  /* =================================================== */
  /* Dragon Balls */
  /* =================================================== */
  .dragon-ball {
    background-image: radial-gradient(var(--background-ball-primary), var(--background-ball-secondary));
    width: 2.5rem;
    height: 2.5rem;
    border-radius: 50%;
    padding: 1rem;
    outline: none;
    border: none;
    box-shadow: 0 0 50px 15px var(--background-ball-primary);
    text-align: center;
    cursor: pointer;
    position: absolute;
    opacity: 0;
    transition: opacity 0.2s;
  }

  .dragon-ball:hover {
    opacity: 1;
  }

  /* De 'ping' class activeert het radar-effect */
  .dragon-ball.ping {
    opacity: 0.5;
    transition: opacity 0.5s;
  }
</style>
