<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Anatomy Explorer</title>
        <style>
            :root {
                --background-dark: #121212;
                --surface-dark: #1e1e1e;
                --primary-text: #e0e0e0;
                --secondary-text: #cccccc;
                --border-color: #333333;
                --accent-color: #ed820e;
                --accent-hover: #d3730c;
            }

            /* Basic Reset */
            *,
            *::before,
            *::after {
                box-sizing: border-box;
            }

            body {
                margin: 0;
                font-family:
                    -apple-system,
                    BlinkMacSystemFont,
                    "Segoe UI",
                    Roboto,
                    Oxygen,
                    Ubuntu,
                    Cantarell,
                    "Open Sans",
                    "Helvetica Neue",
                    sans-serif;
                background-color: var(--background-dark);
                color: var(--primary-text);
                overflow: hidden;
            }

            .app-container {
                display: flex;
                flex-direction: column;
                height: 100vh;
                width: 100vw;
            }

            .menu-bar {
                background-color: var(--surface-dark);
                padding: 1rem 2rem;
                font-size: 2rem;
                font-weight: bold;
                color: var(--primary-text);
                flex-shrink: 0;
                border-bottom: 2px solid var(--border-color);
            }

            .grid-container {
                flex-grow: 1;
                display: grid;
                grid-template-columns: 1fr 1fr;
                grid-template-rows: 1fr 1fr;
                gap: 20px;
                padding: 20px;
                overflow: hidden;
            }

            .card {
                background-color: var(--surface-dark);
                border-radius: 12px;
                padding: 24px;
                display: flex;
                flex-direction: column;
                border: 1px solid var(--border-color);
                overflow: hidden;
            }

            .card h2 {
                margin-top: 0;
                color: var(--primary-text);
                border-bottom: 2px solid var(--border-color);
                padding-bottom: 10px;
                text-transform: capitalize;
                flex-shrink: 0;
            }

            /* Layer Controls Card */
            .layer-controls {
                display: flex;
                flex-direction: column;
                gap: 1rem;
            }

            .layer-controls label {
                font-weight: 500;
                color: var(--secondary-text);
            }

            /* This class is now used for BOTH dropdowns */
            .layer-select {
                padding: 12px;
                border-radius: 8px;
                background-color: #333;
                color: var(--primary-text);
                border: 2px solid #444;
                font-size: 1rem;
                cursor: pointer;
                transition:
                    border-color 0.2s ease,
                    background-color 0.2s ease;
                width: 100%; /* Make dropdowns full width */
            }

            .layer-select:hover {
                border-color: #666;
            }

            .layer-select:focus {
                outline: none;
                border-color: var(--accent-color);
            }

            /*
             * * CSS for .body-parts-list, .part-item, and .part-item.selected 
             * has been removed as it's no longer needed.
             * */

            /* Fun Facts Card */
            .fun-facts-card {
                justify-content: space-between;
            }

            .fact-display {
                flex-grow: 1;
                display: flex;
                align-items: center;
                justify-content: center;
                text-align: center;
                font-size: 1.25rem;
                line-height: 1.6;
                color: var(--secondary-text);
                padding: 1rem;
            }

            .nav-buttons {
                display: flex;
                justify-content: left;
                margin-top: 20px;
                flex-shrink: 0;
            }

            .nav-buttons button {
                background-color: #333333;
                border: none;
                width: 50px;
                height: 50px;
                border-radius: 50%;
                font-size: 1.5rem;
                font-weight: bold;
                cursor: pointer;
                transition: background-color 0.2s ease;
                color: var(--primary-text);
                display: flex;
                align-items: center;
                justify-content: center;
            }

            .nav-buttons button:hover {
                background-color: #444444;
            }
        </style>
    </head>
    <body>
        <div class="app-container">
            <div class="menu-bar">Smart Skeleton</div>

            <div class="grid-container">
                <div class="card">
                    <h2>Environment Information</h2>
                </div>

                <div class="card">
                    <h2>Body Parts</h2>
                    <select
                        id="body-part-selector"
                        class="layer-select"
                    ></select>
                    <!-- <div class="sketchfab-embed-wrapper"> <iframe title="Human Skull (Replica)" frameborder="0" allowfullscreen mozallowfullscreen="true" webkitallowfullscreen="true" allow="autoplay; fullscreen; xr-spatial-tracking" xr-spatial-tracking execution-while-out-of-viewport execution-while-not-rendered web-share src="https://sketchfab.com/models/7a22ca606b004c12abb377dc511c31f9/embed"> </iframe> <p style="font-size: 13px; font-weight: normal; margin: 5px; color: #4A4A4A;"> <a href="https://sketchfab.com/3d-models/human-skull-replica-7a22ca606b004c12abb377dc511c31f9?utm_medium=embed&utm_campaign=share-popup&utm_content=7a22ca606b004c12abb377dc511c31f9" target="_blank" rel="nofollow" style="font-weight: bold; color: #1CAAD9;"> Human Skull (Replica) </a> by <a href="https://sketchfab.com/RISDNaturelab?utm_medium=embed&utm_campaign=share-popup&utm_content=7a22ca606b004c12abb377dc511c31f9" target="_blank" rel="nofollow" style="font-weight: bold; color: #1CAAD9;"> RISD Nature Lab </a> on <a href="https://sketchfab.com?utm_medium=embed&utm_campaign=share-popup&utm_content=7a22ca606b004c12abb377dc511c31f9" target="_blank" rel="nofollow" style="font-weight: bold; color: #1CAAD9;">Sketchfab</a></p></div> -->
                    <div class="sketchfab-embed-wrapper"> <iframe title="Anatomy human rib cage" frameborder="0" allowfullscreen mozallowfullscreen="true" webkitallowfullscreen="true" allow="autoplay; fullscreen; xr-spatial-tracking" xr-spatial-tracking execution-while-out-of-viewport execution-while-not-rendered web-share width="640" height="480" src="https://sketchfab.com/models/0f1aa77bf02e4d438f8630bd6c53b12e/embed?ui_theme=dark"> </iframe> <p style="font-size: 13px; font-weight: normal; margin: 5px; color: #4A4A4A;"> <a href="https://sketchfab.com/3d-models/anatomy-human-rib-cage-0f1aa77bf02e4d438f8630bd6c53b12e?utm_medium=embed&utm_campaign=share-popup&utm_content=0f1aa77bf02e4d438f8630bd6c53b12e" target="_blank" rel="nofollow" style="font-weight: bold; color: #1CAAD9;"> Anatomy human rib cage </a> by <a href="https://sketchfab.com/FrancescoMilanese?utm_medium=embed&utm_campaign=share-popup&utm_content=0f1aa77bf02e4d438f8630bd6c53b12e" target="_blank" rel="nofollow" style="font-weight: bold; color: #1CAAD9;"> FrancescoMilanese </a> on <a href="https://sketchfab.com?utm_medium=embed&utm_campaign=share-popup&utm_content=0f1aa77bf02e4d438f8630bd6c53b12e" target="_blank" rel="nofollow" style="font-weight: bold; color: #1CAAD9;">Sketchfab</a></p></div>
                </div>

                <div class="card controls-card">
                    <h2 id="controls-title">Information</h2>
                    <p>Select a system and a body part to learn more.</p>
                    <div class="nav-buttons">
                        <button
                            id="prev-fact-btn"
                            aria-label="Previous fact"
                        >
                            &larr;</button
                        >
                        <button
                            id="next-fact-btn"
                            aria-label="Next fact"
                        >
                            &rarr;</button
                        >
                        <div class="layer-controls">
                            <select
                                id="layer-selector"
                                class="layer-select"
                            ></select>
                        </div>
                    </div>
                </div>

                <div class="card fun-facts-card">
                    <h2>Fun Fact</h2>
                    <div class="fact-display">
                        <p id="fact-text">Welcome to the Anatomy Explorer!</p>
                    </div>
                </div>
            </div>
        </div>

        <script>
            document.addEventListener("DOMContentLoaded", () => {
                const anatomyData = {
                    skin: {
                        Head: [
                            "The skin on your eyelids is the thinnest on your body.",
                            "Your scalp has about 100,000 hair follicles.",
                            "Facial skin has more oil glands than any other part of your body.",
                        ],
                        Arms: [
                            "The skin on your elbows is often thicker and darker due to friction and pressure.",
                            "Goosebumps are caused by tiny muscles flexing in the skin at the base of each hair.",
                            "Your fingerprints are unique patterns of ridges on the skin of your fingertips.",
                        ],
                        Torso: [
                            "The skin on your back is among the thickest on your body.",
                            "Your belly button is home to a whole ecosystem of bacteria.",
                            "Stretch marks can appear on the abdomen when skin grows or shrinks quickly.",
                        ],
                        Legs: [
                            "The skin on the soles of your feet is the thickest on your body.",
                            "Shaving doesn't make hair grow back thicker; it just cuts it at a blunt angle.",
                            "Leg skin has fewer oil glands than facial skin, making it prone to dryness.",
                        ],
                    },
                    muscle: {
                        skull: [
                            "It takes more muscles to frown (43) than it does to smile (17).",
                            "The masseter, or jaw muscle, is the strongest muscle by weight in the human body.",
                            "Your eyes have muscles that are constantly moving, even when you sleep.",
                        ],
                        arm: [
                            "The biceps muscle is responsible for flexing your elbow.",
                            "The triceps, on the back of the arm, is responsible for extending your elbow.",
                            "The muscles that control your fingers are located in your forearm.",
                        ],
                        torso: [
                            "Your diaphragm is a large muscle at the base of the lungs that helps you breathe.",
                            "The abdominal muscles, or 'abs', help protect your internal organs.",
                            "The latissimus dorsi is one of the widest muscles in the human body, covering much of your back.",
                        ],
                        leg: [
                            "The quadriceps group, on the front of the thigh, is made up of four muscles.",
                            "The gluteus maximus, in your buttocks, is the largest muscle in your body.",
                            "The calf muscles are essential for walking, running, and jumping.",
                        ],
                    },
                    bone: {
                        skull: [
                            "The human skull is made up of 22 different bones, 8 in the cranium and 14 in the face.",
                            "Of the 22 bones in the skull, only the mandible (jaw bone) is movable.",
                            "Babies are born with soft spots (fontanelles) in their skulls that fuse together over time.",
                        ],
                        "rib cage": [
                            "The rib cage protects your heart and lungs.",
                            "Most people have 12 pairs of ribs, making 24 ribs in total.",
                            "The bottom two pairs of ribs are called 'floating ribs' because they don't connect to the sternum.",
                        ],
                        arm: [
                            "The humerus is the single bone in your upper arm.",
                            "Your forearm is made of two bones: the radius and the ulna.",
                            "The funny bone isn't a bone at all; it's the ulnar nerve passing over the humerus.",
                        ],
                        leg: [
                            "The femur, or thigh bone, is the longest, heaviest, and strongest bone in the human body.",
                            "The kneecap is technically called the patella.",
                            "The two bones in your lower leg are the tibia (shin bone) and the fibula.",
                        ],
                    },
                    organ: {
                        brain: [
                            "The human brain weighs about 3 pounds but uses 20% of the body's oxygen and calories.",
                            "Information in the brain travels at speeds of up to 268 miles per hour.",
                            "Your brain is more active at night than during the day.",
                        ],
                        heart: [
                            "The average adult heart beats about 100,000 times each day.",
                            "Your heart pumps about 2,000 gallons of blood every day.",
                            "The sound of your heartbeat is caused by the heart valves opening and closing.",
                        ],
                        lungs: [
                            "If you spread them out, the surface area of your lungs would be about the size of a tennis court.",
                            "The right lung is slightly larger than the left lung to make room for the heart.",
                            "Adults breathe about 12 to 16 times per minute when at rest.",
                        ],
                        stomach: [
                            "Your stomach produces a new layer of mucous lining every two weeks to protect itself from its own acid.",
                            "An adult stomach can hold up to 1.5 liters of food and drink.",
                            "Food stays in your stomach for about 2 to 4 hours.",
                        ],
                        liver: [
                            "The liver is the largest internal organ and can regenerate itself from as little as 25% of its tissue.",
                            "Your liver performs over 500 different functions, including filtering toxins from your blood.",
                            "At any given moment, your liver holds about 10% of your body's total blood.",
                        ],
                    },
                };

                let currentLayer = Object.keys(anatomyData)[0]; // Default to first layer ('skin')
                let selectedPart = Object.keys(anatomyData[currentLayer])[0]; // Default to first part of that layer
                let currentFactIndex = 0;

                const layerSelector = document.getElementById("layer-selector");
                
                // --- CHANGED ---
                // Get the new select element
                const bodyPartSelector = document.getElementById("body-part-selector");
                
                const controlsTitle = document.getElementById("controls-title");
                const factText = document.getElementById("fact-text");
                const prevFactBtn = document.getElementById("prev-fact-btn");
                const nextFactBtn = document.getElementById("next-fact-btn");

                function updateDisplay() {
                    const facts = anatomyData[currentLayer][selectedPart];
                    factText.textContent = facts[currentFactIndex];

                    // Update controls title
                    controlsTitle.textContent = `${selectedPart} (${currentLayer})`;

                    // --- CHANGED ---
                    // Set the dropdown's value to match the state
                    bodyPartSelector.value = selectedPart;
                }

                // --- CHANGED ---
                // This function now populates the <select> element with <option>s
                function renderBodyParts() {
                    bodyPartSelector.innerHTML = ""; // Clear existing options
                    const parts = Object.keys(anatomyData[currentLayer]);

                    parts.forEach((part) => {
                        const option = document.createElement("option");
                        option.value = part;
                        option.textContent = part;
                        bodyPartSelector.appendChild(option);
                    });
                }

                function renderLayerOptions() {
                    const layers = Object.keys(anatomyData);
                    layers.forEach((layer) => {
                        const option = document.createElement("option");
                        option.value = layer;
                        option.textContent = layer;
                        layerSelector.appendChild(option);
                    });
                }

                layerSelector.addEventListener("change", (e) => {
                    currentLayer = e.target.value;
                    selectedPart = Object.keys(anatomyData[currentLayer])[0];
                    currentFactIndex = 0;
                    renderBodyParts();
                    updateDisplay();
                });

                // --- ADDED ---
                // Add event listener for the new body part dropdown
                bodyPartSelector.addEventListener("change", (e) => {
                    selectedPart = e.target.value;
                    currentFactIndex = 0;
                    updateDisplay();
                });

                // Handle next fact button
                nextFactBtn.addEventListener("click", () => {
                    const totalFacts =
                        anatomyData[currentLayer][selectedPart].length;
                    currentFactIndex = (currentFactIndex + 1) % totalFacts;
                    updateDisplay();
                });

                // Handle previous fact button
                prevFactBtn.addEventListener("click", () => {
                    const totalFacts =
                        anatomyData[currentLayer][selectedPart].length;
                    currentFactIndex =
                        (currentFactIndex - 1 + totalFacts) % totalFacts;
                    updateDisplay();
                });

                function init() {
                    renderLayerOptions();
                    renderBodyParts();
                    updateDisplay();
                }

                init();
            });
        </script>
    </body>
</html>