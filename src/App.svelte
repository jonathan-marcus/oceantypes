<script>
  import { onMount } from 'svelte';
  import OceanGame from './lib/OceanGame.svelte';
  
  let isLoading = true;
  
  onMount(() => {
    // Simulate loading resources
    setTimeout(() => {
      isLoading = false;
    }, 1000);
  });
</script>

<main>
  <header>
    <h1>OceanTypes</h1>
    <p>Learn ocean animal spellings!</p>
  </header>
  
  {#if isLoading}
    <div class="loading">
      <p>Loading...</p>
    </div>
  {:else}
    <OceanGame />
  {/if}
  
  <footer>
    <p>Created with ❤️ for ocean-loving kids</p>
  </footer>
</main>

<style>
  main {
    max-width: 960px;
    margin: 0 auto;
    padding: 2rem;
    font-family: 'Comic Sans MS', cursive, sans-serif;
    background-color: #e6f7ff;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
  
  header {
    text-align: center;
    margin-bottom: 2rem;
  }
  
  h1 {
    color: #0077be;
    font-size: 3rem;
    margin-bottom: 0.5rem;
  }
  
  header p {
    color: #0099cc;
    font-size: 1.5rem;
  }
  
  .loading {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  footer {
    margin-top: 2rem;
    text-align: center;
    color: #0099cc;
  }
</style>