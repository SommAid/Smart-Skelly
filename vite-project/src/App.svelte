<script>
  const bodyParts = ['skull', 'rib cage', 'arm', 'hand', 'leg', 'foot'];
  
  let selectedPart = bodyParts[0];

  let currentFactIndex = 0;

  const funFacts = {
    skull: [
      "The human skull is made up of 22 different bones.",
      "Your skull protects the most important organ in your body: the brain.",
      "Of the 22 bones in the skull, only the mandible (jaw bone) is movable."
    ],
    'rib cage': [
      "The rib cage protects your heart and lungs.",
      "Most people have 12 pairs of ribs, making 24 ribs in total.",
      "The top seven pairs of ribs are called 'true ribs' because they connect directly to the sternum."
    ],
    arm: [
      "The bone in your upper arm (the humerus) is the longest bone in the arm.",
      "Your forearm is made of two bones: the radius and the ulna.",
      "The muscles that move your fingers are located in your forearm, not your hand."
    ],
    hand: [
      "The human hand has 27 bones.",
      "Nearly a quarter of all the bones in your body are located in your hands.",
      "Fingernails are made of a protein called keratin, the same substance as hair."
    ],
    leg: [
      "The femur, or thigh bone, is the longest, heaviest, and strongest bone in the human body.",
      "The fibula, the smaller bone in your lower leg, mainly serves as an attachment point for muscles.",
      "The 'kneecap' is technically called the patella."
    ],
    foot: [
      "The human foot contains 26 bones, 33 joints, and more than 100 muscles, ligaments, and tendons.",
      "About 25% of all the bones in your body are in your feet.",
      "The Achilles tendon is the largest and strongest tendon in the body, connecting the calf muscles to the heel bone."
    ]
  };

  function selectPart(part) {
    selectedPart = part;
    currentFactIndex = 0; 
  }

  function nextFact() {
    const totalFacts = funFacts[selectedPart].length;
    currentFactIndex = (currentFactIndex + 1) % totalFacts;
  }

  function prevFact() {
    const totalFacts = funFacts[selectedPart].length;
    currentFactIndex = (currentFactIndex - 1 + totalFacts) % totalFacts;
  }
  
  $: currentFact = funFacts[selectedPart][currentFactIndex];

</script>

<style>
  :global(body) {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    background-color: #121212;
    color: #e0e0e0;
  }

  .app-container {
    display: flex;
    flex-direction: column;
    height: 100vh;
    width: 100vw;
  }

  .menu-bar {
    background-color: #1e1e1e;
    padding: 1rem 2rem;
    font-size: 1.5rem;
    font-weight: bold;
    color: #ED820E;
    flex-shrink: 0; 
    font-size: 36px;
  }

  .grid-container {
    flex-grow: 1;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    gap: 20px;
    padding: 20px;
  }

  .card {
    background-color: #1e1e1e;
    border-radius: 12px;
    padding: 24px;
    display: flex;
    flex-direction: column;
  }
  
  .card h2 {
    margin-top: 0;
    color: #f0f0f0;
    border-bottom: 2px solid #333333;
    padding-bottom: 10px;
  }

  .body-parts-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
    overflow-y: auto;
  }

  .part-item {
    padding: 15px;
    border: 2px solid #444444;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    text-transform: capitalize;
    transition: background-color 0.2s ease, border-color 0.2s ease;
  }
  
  .part-item:hover {
    background-color: #2a2a2a;
    border-color: #666666;
  }

  .part-item.selected {
    background-color: #ED820E;
    color: white;
    border-color: #d3730c;
  }
  
  .controls-card h2 {
    text-transform: capitalize;
  }

  .fun-facts-card {
    justify-content: space-between; 
  }
  
  .fact-display {
    flex-grow: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    font-size: 1.2rem;
    line-height: 1.6;
    color: #cccccc;
  }
  
  .nav-buttons {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
  }
  
  .nav-buttons button {
    background-color: #333333;
    border: none;
    padding: 10px 20px;
    border-radius: 8px;
    font-size: 1.5rem;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.2s ease;
    color: #e0e0e0;
  }

  .nav-buttons button:hover {
    background-color: #444444;
  }
</style>

<div class="app-container">
  <div class="menu-bar">
    Smart Skeleton
  </div>

  <div class="grid-container">
    <div class="card">
      <h2>Environment Details</h2>
    </div>

    <div class="card">
      <h2>Body Parts</h2>
      <div class="body-parts-list">
        {#each bodyParts as part}
          <div 
            class="part-item" 
            class:selected={part === selectedPart}
            on:click={() => selectPart(part)}
          >
            {part}
          </div>
        {/each}
      </div>
    </div>

    <div class="card controls-card">
      <h2>Controls for: {selectedPart}</h2>
      <p>Control options for the selected part will appear here.</p>
    </div>

    <div class="card fun-facts-card">
      <h2>Fun Fact</h2>
      <div class="fact-display">
        <p>{currentFact}</p>
      </div>
      <div class="nav-buttons">
        <button on:click={prevFact} aria-label="Previous fact">&larr;</button>
        <button on:click={nextFact} aria-label="Next fact">&rarr;</button>
      </div>
    </div>
  </div>
</div>