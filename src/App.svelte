<script>
  import { text } from 'svelte/internal';
import Community from './lib/Community.svelte'
  let p=0.3;
  let sd = 2;
  let del = 100;
  let resetfun;
</script>

<main class="main">
  <div class="inputs">
    Probability
    <input bind:value={p}/>
    Sick Duration
    <input bind:value={sd}>
    Delay
    <input bind:value={del}>
    <button on:click={resetfun}> Reset </button>
  </div>
  <Community size=100 spreadProbInp={p} days=300 sickDurationInp={sd} delayInp={del} bind:reset={resetfun}/>

  
</main>

<style>
  .main{
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-content: space-around;
    align-items: center;
    gap: 10em 10em;
  }

  .inputs{
    display: flex;
    flex-direction: column;
  }

  
</style>
