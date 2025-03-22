<script>
  import { onMount } from 'svelte';
  
  // Game configuration
  let difficultyLevels = [
    { id: 'easy', name: 'Easy', showHints: true },
    { id: 'medium', name: 'Medium', showHints: false },
    { id: 'hard', name: 'Hard', showHints: false }
  ];
  
  // Game state
  let currentLevel = difficultyLevels[0];
  let score = 0;
  let currentAnimal = null;
  let userInput = '';
  let feedback = '';
  let showSuccess = false;
  
  // Animal data
  const animals = [
    { name: 'SHARK', image: 'shark.jpg', hint: 'SH_RK', facts: 'Fast swimmer with sharp teeth!' },
    { name: 'WHALE', image: 'whale.jpg', hint: 'WH_LE', facts: 'The biggest animal in the ocean!' },
    { name: 'OCTOPUS', image: 'octopus.jpg', hint: 'OCT_P_S', facts: 'Has eight arms and is very smart!' },
    { name: 'DOLPHIN', image: 'dolphin.jpg', hint: 'D_LPH_N', facts: 'Playful and very intelligent!' },
    { name: 'CRAB', image: 'crab.jpg', hint: 'CR_B', facts: 'Walks sideways with big claws!' },
    { name: 'TURTLE', image: 'turtle.jpg', hint: 'T_RTL_', facts: 'Has a hard shell and swims slowly!' },
    { name: 'FISH', image: 'fish.jpg', hint: 'F_SH', facts: 'Swims with fins and has scales!' }
  ];
  
  // Start game
  onMount(() => {
    selectRandomAnimal();
  });
  
  function selectRandomAnimal() {
    // Reset state
    userInput = '';
    feedback = '';
    showSuccess = false;
    
    // Select random animal
    const randomIndex = Math.floor(Math.random() * animals.length);
    currentAnimal = animals[randomIndex];
  }
  
  function changeDifficulty(level) {
    currentLevel = level;
  }
  
  function checkAnswer() {
    if (userInput.toUpperCase() === currentAnimal.name) {
      // Correct answer
      score += 1;
      feedback = 'Correct! Great job!';
      showSuccess = true;
      setTimeout(() => {
        selectRandomAnimal();
      }, 2000);
    } else {
      // Incorrect answer
      feedback = 'Try again!';
    }
  }
  
  function getDisplayHint() {
    if (!currentLevel.showHints) return null;
    
    return currentAnimal.hint;
  }
</script>

<div class="game-container">
  <div class="controls">
    <div class="difficulty">
      {#each difficultyLevels as level}
        <button 
          class:active={currentLevel.id === level.id}
          on:click={() => changeDifficulty(level)}
        >
          {level.name}
        </button>
      {/each}
    </div>
    
    <div class="score">
      Score: {score}
    </div>
  </div>
  
  {#if currentAnimal}
    <div class="animal-container">
      <div class="animal-image">
        <!-- Placeholder for actual images -->
        <div class="placeholder-image">{currentAnimal.name}</div>
      </div>
      
      <div class="facts">
        <p>{currentAnimal.facts}</p>
      </div>
      
      <div class="spelling-area">
        {#if getDisplayHint()}
          <div class="hint">
            <span>{getDisplayHint()}</span>
          </div>
        {/if}
        
        <div class="input-area">
          <input 
            type="text" 
            bind:value={userInput} 
            placeholder="Type the animal's name"
            on:keypress={(e) => e.key === 'Enter' && checkAnswer()}
            disabled={showSuccess}
          />
          <button on:click={checkAnswer} disabled={showSuccess}>Check</button>
        </div>
        
        {#if feedback}
          <div class="feedback {showSuccess ? 'success' : ''}">
            {feedback}
          </div>
        {/if}
      </div>
    </div>
  {/if}
  
  <button class="next-button" on:click={selectRandomAnimal}>Next Animal</button>
</div>

<style>
  .game-container {
    background-color: white;
    border-radius: 16px;
    padding: 2rem;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    flex: 1;
  }
  
  .controls {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;
  }
  
  .difficulty button {
    margin-right: 0.5rem;
  }
  
  .difficulty button.active {
    background-color: white;
    color: var(--ocean-blue);
    border-color: var(--ocean-blue);
    font-weight: bold;
  }
  
  .score {
    font-size: 1.5rem;
    font-weight: bold;
    color: var(--ocean-blue);
  }
  
  .animal-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 2rem;
  }
  
  .animal-image {
    width: 300px;
    height: 200px;
    margin-bottom: 1rem;
    border-radius: 8px;
    overflow: hidden;
  }
  
  .placeholder-image {
    width: 100%;
    height: 100%;
    background-color: var(--light-blue);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    color: var(--ocean-blue);
  }
  
  .facts {
    margin-bottom: 1rem;
    text-align: center;
    font-style: italic;
  }
  
  .spelling-area {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 1rem;
  }
  
  .hint {
    font-size: 2rem;
    letter-spacing: 0.5rem;
    margin-bottom: 1rem;
    color: var(--ocean-blue);
    font-weight: bold;
  }
  
  .input-area {
    display: flex;
    margin-bottom: 1rem;
  }
  
  input {
    padding: 0.5rem 1rem;
    font-size: 1.2rem;
    border: 2px solid var(--light-blue);
    border-radius: 8px 0 0 8px;
    font-family: inherit;
  }
  
  input:focus {
    outline: none;
    border-color: var(--ocean-blue);
  }
  
  .input-area button {
    border-radius: 0 8px 8px 0;
  }
  
  .feedback {
    font-size: 1.2rem;
    margin-top: 1rem;
    font-weight: bold;
    color: #ff6b6b;
  }
  
  .feedback.success {
    color: #2ecc71;
  }
  
  .next-button {
    display: block;
    margin: 0 auto;
    padding: 0.75rem 1.5rem;
    font-size: 1.2rem;
  }
</style>