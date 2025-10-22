<script>
    import { nonpassive } from "svelte/legacy";

</script>
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
                display: flex;
                justify-content: space-between;
                align-items: center;
            }

            .menu-button {
                background: none;
                border: 2px solid var(--border-color);
                color: var(--primary-text);
                font-size: 1.5rem;
                border-radius: 8px;
                cursor: pointer;
                transition: all 0.2s ease;
                padding: 0.25rem 0.75rem;
                line-height: 1;
            }
            .menu-button:hover {
                background-color: var(--border-color);
                color: var(--accent-color);
            }

            .menu-controls {
                display: flex;
                align-items: center;
                gap: 15px;
            }

            .mode-indicator {
                font-size: 1rem;
                font-weight: 500;
                color: var(--secondary-text);
                transition: color 0.2s ease;
            }

            .mode-indicator.instructor-mode {
                color: var(--accent-color);
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
                width: 100%;
            }

            .layer-select:hover {
                border-color: #666;
            }

            .layer-select:focus {
                outline: none;
                border-color: var(--accent-color);
            }
            
            #upload-container {
                display: none
            }
            body.instructor-mode-active #upload-container {
                display: block; /* Show the container */
            }
            .upload-btn {
                display: inline-block;
                background-color: var(--accent-color);
                color: white;
                padding: 14px 15px;
                border-radius: 8px;
                cursor: pointer;
                font-weight: 500;
                text-align: center;
                transition: background-color 0.2s ease;
            }
            .upload-btn:hover {
                background-color: var(--accent-hover);
            }
            #json-upload {
                display: none;
                margin: none;
            }

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

            /* 3D model viewer */
            .model-viewer {
                display: none;
                flex-grow: 1;
                width: 100%;
                margin-top: 5px;
                flex-direction: column;
                min-height: 0;
            }

            .model-viewer.visible {
                display: flex;
            }

            .model-viewer iframe {
                width: 100%;
                flex-grow: 1;
                border: none;
                min-height: 0;
            }

            .model-viewer p {
                color: var(--secondary-text) !important;
                font-size: 12px !important;
                margin: 2px 0 0 0 !important;
                flex-shrink: 0;
            }
            .model-viewer p a {
                color: var(--accent-color) !important;
            }

            .sidebar-overlay {
                display: none;
                position: fixed;
                top: 0;
                left: 0;
                width: 100vw;
                height: 100vh;
                background-color: rgba(0, 0, 0, 0.7);
                justify-content: flex-end;
                z-index: 1000;
                opacity: 0;
                transition: opacity 0.3s ease-in-out;
            }

            .sidebar-overlay.visible {
                display: flex;
                opacity: 1;
            }

            .sidebar-content {
                background-color: var(--surface-dark);
                height: 100%;
                width: 350px;
                max-width: 90%;
                padding: 24px;
                border-left: 1px solid var(--border-color);
                box-shadow: -10px 0 30px rgba(0, 0, 0, 0.5);

                transform: translateX(100%);
                transition: transform 0.3s ease-in-out;
            }

            .sidebar-overlay.visible .sidebar-content {
                transform: translateX(0);
            }

            .sidebar-close {
                position: absolute;
                top: 15px;
                right: 20px;
                font-size: 2.5rem;
                color: var(--secondary-text);
                cursor: pointer;
                line-height: 1;
            }
            .sidebar-close:hover {
                color: var(--primary-text);
            }

            .toggle-switch {
                display: flex;
                align-items: center;
                justify-content: center;
                margin-top: 1rem;
            }

            .toggle-input {
                display: none;
            }

            .toggle-label {
                display: block;
                width: 200px;
                height: 50px;
                background-color: var(--border-color);
                border-radius: 25px;
                position: relative;
                cursor: pointer;
                transition: background-color 0.2s ease;
            }

            .toggle-label::after {
                content: '';
                position: absolute;
                width: 44px;
                height: 44px;
                border-radius: 50%;
                background-color: var(--primary-text);
                top: 3px;
                left: 3px;
                transition: transform 0.2s ease;
            }

            .toggle-label::before {
                content: 'Student';
                position: absolute;
                right: 20px;
                top: 50%;
                transform: translateY(-50%);
                color: var(--primary-text);
                font-weight: bold;
                font-size: 1.1rem;
            }

            .toggle-input:checked + .toggle-label {
                background-color: var(--accent-color);
            }

            .toggle-input:checked + .toggle-label::after {
                transform: translateX(150px);
            }

            .toggle-input:checked + .toggle-label::before {
                content: 'Instructor';
                left: 20px;
                right: auto;
                color: white;
            }
        </style>
    </head>
    <body>
        <div class="app-container">
            <div class="menu-bar">
                <span>Smart Skeleton</span>

                <div class="menu-controls">
                    <span id="mode-indicator" class="mode-indicator">Student</span>

                    <button
                        id="settings-btn"
                        class="menu-button"
                        aria-label="Settings"
                    >
                        ⚙️
                    </button>
                </div>
            </div>
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

                    <div
                        id="skull-model"
                        class="sketchfab-embed-wrapper model-viewer"
                    >
                        <iframe
                            title="Human Skull (Replica)"
                            frameborder="0"
                            allowfullscreen
                            src="https://sketchfab.com/models/7a22ca606b004c12abb377dc511c31f9/embed"
                        >
                        </iframe>
                    </div>

                    <div
                        id="rib-cage-model"
                        class="sketchfab-embed-wrapper model-viewer"
                    >
                        <iframe
                            title="Anatomy human rib cage"
                            frameborder="0"
                            allowfullscreen
                            src="https://sketchfab.com/models/0f1aa77bf02e4d438f8630bd6c53b12e/embed?ui_theme=dark"
                        >
                        </iframe>
                    </div>
                </div>

                <div class="card controls-card">
                    <h2 id="controls-title">Information</h2>
                    <p>Select a system and a body part to learn more.</p>
                    <div class="nav-buttons">
                        <button
                            id="prev-fact-btn"
                            aria-label="Previous fact"
                        >
                            &larr;
                        </button>
                        <button
                            id="next-fact-btn"
                            aria-label="Next fact"
                        >
                            &rarr;
                        </button>
                        <div class="layer-controls">
                            <select
                                id="layer-selector"
                                class="layer-select"
                            ></select>
                        </div>
                        <div id="upload-container">
                          <label for="json-upload" class="upload-btn">Upload JSON File</label>
                          <input type="file" id="json-upload" accept="application/json">
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

        <div id="sidebar-overlay" class="sidebar-overlay">
            <div class="sidebar-content">
                <span
                    class="sidebar-close"
                    id="sidebar-close-btn"
                    >&times;</span
                >
                <h2>Settings</h2>
                <div class="toggle-switch">
                    <input
                        type="checkbox"
                        id="mode-toggle"
                        class="toggle-input"
                    />
                    <label for="mode-toggle" class="toggle-label"></label>
                </div>
            </div>
        </div>
        
        <script>
            document.addEventListener("DOMContentLoaded", () => {
                async function initializeApp() {
                    let anatomyData; 

                    const layerSelector = document.getElementById("layer-selector");
                    const bodyPartSelector = document.getElementById("body-part-selector");
                    const skullModel = document.getElementById("skull-model");
                    const ribCageModel = document.getElementById("rib-cage-model");
                    const controlsTitle = document.getElementById("controls-title");
                    const factText = document.getElementById("fact-text");
                    const prevFactBtn = document.getElementById("prev-fact-btn");
                    const nextFactBtn = document.getElementById("next-fact-btn");
                    const settingsBtn = document.getElementById("settings-btn");
                    const settingsSidebar = document.getElementById("sidebar-overlay");
                    const sidebarCloseBtn = document.getElementById("sidebar-close-btn");
                    const modeToggle = document.getElementById("mode-toggle");
                    const modeIndicator = document.getElementById("mode-indicator");
                    const jsonUpload = document.getElementById('json-upload');
                    
                    let currentLayer, selectedPart, currentFactIndex;

                    function resetAppWithNewData(newData) {
                        anatomyData = newData; 

                        currentLayer = Object.keys(anatomyData)[0] || '';
                        selectedPart = anatomyData[currentLayer] ? Object.keys(anatomyData[currentLayer])[0] : '';
                        currentFactIndex = 0;

                        if (currentLayer && selectedPart) {
                            init();
                        } else {
                            console.error("Invalid or empty data loaded.");
                            factText.textContent = "Error: The loaded data file is invalid or empty.";
                        }
                    }
                    
                    function updateDisplay() {
                        const facts = anatomyData[currentLayer][selectedPart];
                        factText.textContent = facts[currentFactIndex];
                        controlsTitle.textContent = `${selectedPart} (${currentLayer})`;
                        bodyPartSelector.value = selectedPart;
                        skullModel.classList.remove("visible");
                        ribCageModel.classList.remove("visible");
                        const part = selectedPart.toLowerCase();
                        if (part === "head" || part === "skull" || part === "brain") {
                            skullModel.classList.add("visible");
                        } else if (part === "torso" || part === "rib cage" || part === "heart" || part === "lungs" || part === "stomach" || part === "liver") {
                            ribCageModel.classList.add("visible");
                        }
                    }

                    function renderBodyParts() {
                        bodyPartSelector.innerHTML = "";
                        const parts = Object.keys(anatomyData[currentLayer]);
                        parts.forEach((part) => {
                            const option = document.createElement("option");
                            option.value = part;
                            option.textContent = part;
                            bodyPartSelector.appendChild(option);
                        });
                    }

                    function renderLayerOptions() {
                        layerSelector.innerHTML = "";
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

                    bodyPartSelector.addEventListener("change", (e) => {
                        selectedPart = e.target.value;
                        currentFactIndex = 0;
                        updateDisplay();
                    });

                    nextFactBtn.addEventListener("click", () => {
                        const totalFacts = anatomyData[currentLayer][selectedPart].length;
                        currentFactIndex = (currentFactIndex + 1) % totalFacts;
                        updateDisplay();
                    });

                    prevFactBtn.addEventListener("click", () => {
                        const totalFacts = anatomyData[currentLayer][selectedPart].length;
                        currentFactIndex = (currentFactIndex - 1 + totalFacts) % totalFacts;
                        updateDisplay();
                    });

                    settingsBtn.addEventListener("click", () => {
                        settingsSidebar.classList.add("visible");
                    });

                    sidebarCloseBtn.addEventListener("click", () => {
                        settingsSidebar.classList.remove("visible");
                    });

                    settingsSidebar.addEventListener("click", (e) => {
                        if (e.target === settingsSidebar) {
                            settingsSidebar.classList.remove("visible");
                        }
                    });

                    modeToggle.addEventListener("change", (e) => {
                        const isInstructor = e.target.checked;
                        if (isInstructor) {
                            modeIndicator.textContent = "Instructor";
                            modeIndicator.classList.add("instructor-mode");
                            document.body.classList.add("instructor-mode-active");
                        } else {
                            modeIndicator.textContent = "Student";
                            modeIndicator.classList.remove("instructor-mode");
                            document.body.classList.remove("instructor-mode-active");
                        }
                        console.log(`Mode switched to: ${modeIndicator.textContent}`);
                    });
                    
                    jsonUpload.addEventListener('change', (event) => {
                        const file = event.target.files[0];
                        if (!file || file.type !== 'application/json') {
                            alert('Please select a valid JSON file.');
                            return;
                        }

                        const reader = new FileReader();
                        reader.onload = (e) => {
                            try {
                                const newAnatomyData = JSON.parse(e.target.result);
                                console.log('Successfully loaded new data:', newAnatomyData);
                                resetAppWithNewData(newAnatomyData);
                            } catch (error) {
                                alert('Error parsing JSON file. Please check the file format.');
                                console.error('JSON Parse Error:', error);
                            }
                        };
                        reader.readAsText(file);
                    });

                    function init() {
                        renderLayerOptions();
                        renderBodyParts();
                        updateDisplay();
                    }

                    try {
                        const response = await fetch('./src/facts/sample_facts.json');
                        if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);
                        const initialData = await response.json();
                        resetAppWithNewData(initialData);
                    } catch (error) {
                        console.error("Failed to initialize the application:", error);
                        document.body.innerHTML = `<div style="color: red; padding: 20px;"><h2>Error: Could not load initial data.</h2><p>Please make sure the file './facts/sample_facts.json' exists and is accessible.</p></div>`;
                    }
                }

                initializeApp();
            });
        </script>
    </body>
</html>