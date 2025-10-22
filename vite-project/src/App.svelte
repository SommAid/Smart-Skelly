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
                --correct-color: #28a745;
                --incorrect-color: #dc3545;
            }

            *,
            *::before,
            *::after {
                box-sizing: border-box;
            }

            body {
                margin: 0;
                font-family:
                    -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
                    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
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

            #quiz-btn {
                font-size: 1rem;
                padding: 0.5rem 1rem;
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
                grid-template-rows: 1fr;
                gap: 20px;
                padding: 20px;
                overflow: hidden;
            }

            .left-column {
                display: flex;
                flex-direction: column;
                gap: 20px;
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
                display: none;
            }
            body.instructor-mode-active #upload-container {
                display: block;
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

            .fun-facts-card {
                flex-grow: 1;
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
                align-items: center;
                gap: 10px;
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
                content: "";
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
                content: "Student";
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
                content: "Instructor";
                left: 20px;
                right: auto;
                color: white;
            }

            /* Quiz Modal Styles */
            .quiz-modal-overlay {
                display: none;
                position: fixed;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                background: rgba(0, 0, 0, 0.8);
                justify-content: center;
                align-items: center;
                z-index: 2000;
                opacity: 0;
                transition: opacity 0.3s ease;
            }

            .quiz-modal-overlay.visible {
                display: flex;
                opacity: 1;
            }

            .quiz-modal-content {
                background: var(--surface-dark);
                padding: 30px;
                border-radius: 12px;
                width: 90%;
                max-width: 600px;
                max-height: 90vh;
                overflow-y: auto;
                border: 1px solid var(--border-color);
                transform: scale(0.9);
                transition: transform 0.3s ease;
            }

            .quiz-modal-overlay.visible .quiz-modal-content {
                transform: scale(1);
            }

            .quiz-modal-content h2 {
                margin-top: 0;
            }

            #quiz-topic-form {
                max-height: 400px;
                overflow-y: auto;
                padding-right: 15px; /* for scrollbar */
            }

            .quiz-layer-group {
                margin-bottom: 1.5rem;
            }
            .quiz-layer-group h3 {
                color: var(--accent-color);
                border-bottom: 1px solid var(--border-color);
                padding-bottom: 8px;
            }

            .quiz-checkbox-container {
                display: block;
                position: relative;
                padding-left: 35px;
                margin-bottom: 12px;
                cursor: pointer;
                font-size: 18px;
                user-select: none;
            }

            .quiz-checkbox-container input {
                position: absolute;
                opacity: 0;
                cursor: pointer;
                height: 0;
                width: 0;
            }

            .checkmark {
                position: absolute;
                top: 0;
                left: 0;
                height: 25px;
                width: 25px;
                background-color: var(--border-color);
                border-radius: 4px;
                transition: background-color 0.2s ease;
            }

            .quiz-checkbox-container:hover input ~ .checkmark {
                background-color: #555;
            }

            .quiz-checkbox-container input:checked ~ .checkmark {
                background-color: var(--accent-color);
            }

            .checkmark:after {
                content: "";
                position: absolute;
                display: none;
            }

            .quiz-checkbox-container input:checked ~ .checkmark:after {
                display: block;
            }

            .quiz-checkbox-container .checkmark:after {
                left: 9px;
                top: 5px;
                width: 5px;
                height: 10px;
                border: solid white;
                border-width: 0 3px 3px 0;
                transform: rotate(45deg);
            }

            .quiz-button {
                display: block;
                width: 100%;
                padding: 15px;
                font-size: 1.2rem;
                font-weight: bold;
                color: white;
                background-color: var(--accent-color);
                border: none;
                border-radius: 8px;
                cursor: pointer;
                transition: background-color 0.2s ease;
                margin-top: 20px;
            }
            .quiz-button:hover {
                background-color: var(--accent-hover);
            }

            #quiz-questions-container .question {
                font-size: 1.3rem;
                margin-bottom: 1.5rem;
            }

            #quiz-questions-container .answers {
                display: flex;
                flex-direction: column;
                gap: 10px;
            }

            #quiz-questions-container .answer-btn {
                text-align: left;
                background: var(--border-color);
                color: var(--primary-text);
                border: 2px solid transparent;
                padding: 15px;
                border-radius: 8px;
                cursor: pointer;
                font-size: 1rem;
                transition: all 0.2s ease;
            }
            #quiz-questions-container .answer-btn:hover:not(:disabled) {
                border-color: var(--accent-color);
            }

            #quiz-questions-container .answer-btn.correct {
                background-color: var(--correct-color);
                border-color: var(--correct-color);
                color: white;
            }

            #quiz-questions-container .answer-btn.incorrect {
                background-color: var(--incorrect-color);
                border-color: var(--incorrect-color);
                color: white;
            }

            #quiz-questions-container .answer-btn:disabled {
                cursor: not-allowed;
                opacity: 0.8;
            }

            #quiz-results {
                text-align: center;
                font-size: 1.5rem;
            }

            #next-question-btn {
                margin-top: 20px;
            }
        </style>
    </head>
    <body>
        <div class="app-container">
            <div class="menu-bar">
                <span>Smart Skeleton</span>

                <div class="menu-controls">
                    <span id="mode-indicator" class="mode-indicator"
                        >Student</span
                    >
                    <button id="quiz-btn" class="menu-button"
                        >Take a Quiz</button
                    >
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
                <div class="left-column">
                    <div class="card controls-card">
                        <h2 id="controls-title">Controls</h2>
                        <div class="nav-buttons">
                            <button id="prev-fact-btn" aria-label="Previous fact">
                                &larr;
                            </button>
                            <button id="next-fact-btn" aria-label="Next fact">
                                &rarr;
                            </button>
                            <div class="layer-controls">
                                <select id="layer-selector" class="layer-select"
                                ></select>
                            </div>
                            <div id="upload-container">
                                <label for="json-upload" class="upload-btn"
                                    >Upload JSON File</label
                                >
                                <input
                                    type="file"
                                    id="json-upload"
                                    accept="application/json"
                                />
                            </div>
                        </div>
                    </div>

                    <div class="card fun-facts-card">
                        <h2 id="fun-fact-title">Skeleton Display</h2>
                        <div class="fact-display">
                            <p id="fact-text">Welcome to the Anatomy Explorer!</p>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <h2>Body Parts</h2>
                    <select id="body-part-selector" class="layer-select"
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
                    <div id="leg-model" class="sketchfab-embed-wrapper model-viewer">
                      <iframe title="Leg anterior muscles" frameborder="0" allowfullscreen src="https://sketchfab.com/models/28f28c361da743139b6eea6b9ba68f59/embed">
                      </iframe>
                    </div>
                    <div id="arm-model" class="sketchfab-embed-wrapper model-viewer">
                        <iframe title="Anatomy - Hand and arm bones" frameborder="0" allowfullscreen src="https://sketchfab.com/models/719f6ae9f03b4246b9105fb3301a6f02/embed">
                        </iframe>
                    </div>
                </div>
            </div>
        </div>

        <div id="sidebar-overlay" class="sidebar-overlay">
            <div class="sidebar-content">
                <span class="sidebar-close" id="sidebar-close-btn">&times;</span
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

        <div id="quiz-modal-overlay" class="quiz-modal-overlay">
            <div class="quiz-modal-content">
                <span class="sidebar-close" id="quiz-close-btn">&times;</span>
                <div id="quiz-view-container">
                    <h2>Select Quiz Topics</h2>
                    <form id="quiz-topic-form"></form>
                    <button id="start-quiz-btn" class="quiz-button"
                        >Start Quiz</button
                    >
                </div>
                <div id="quiz-questions-container" style="display: none;">
                    <div id="quiz-question-content"></div>
                    <button
                        id="next-question-btn"
                        class="quiz-button"
                        style="display:none;">Next Question</button
                    >
                </div>
                <div id="quiz-results" style="display: none;"></div>
            </div>
        </div>

        <script>
            document.addEventListener("DOMContentLoaded", () => {
                async function initializeApp() {
                    let anatomyData;

                    // Get elements with updating UI features
                    const layerSelector = document.getElementById("layer-selector");
                    const bodyPartSelector = document.getElementById("body-part-selector");
                    const skullModel = document.getElementById("skull-model");
                    const ribCageModel = document.getElementById("rib-cage-model");
                    const legModel = document.getElementById("leg-model");
                    const armModel = document.getElementById("arm-model");
                    const funfactTitle = document.getElementById("fun-fact-title"); // Corrected ID
                    const factText = document.getElementById("fact-text");
                    const prevFactBtn = document.getElementById("prev-fact-btn");
                    const nextFactBtn = document.getElementById("next-fact-btn");
                    const settingsBtn = document.getElementById("settings-btn");
                    const settingsSidebar = document.getElementById("sidebar-overlay");
                    const sidebarCloseBtn = document.getElementById("sidebar-close-btn");
                    const modeToggle = document.getElementById("mode-toggle");
                    const modeIndicator = document.getElementById("mode-indicator");
                    const jsonUpload = document.getElementById("json-upload");

                    // Quiz Elements
                    const quizBtn = document.getElementById("quiz-btn");
                    const quizModalOverlay = document.getElementById("quiz-modal-overlay");
                    const quizCloseBtn = document.getElementById("quiz-close-btn");
                    const quizTopicForm = document.getElementById("quiz-topic-form");
                    const startQuizBtn = document.getElementById("start-quiz-btn");
                    const quizViewContainer = document.getElementById("quiz-view-container");
                    const quizQuestionsContainer = document.getElementById("quiz-questions-container");
                    const quizQuestionContent = document.getElementById("quiz-question-content");
                    const nextQuestionBtn = document.getElementById("next-question-btn");
                    const quizResults = document.getElementById("quiz-results");

                    let currentLayer, selectedPart, currentFactIndex;
                    let quizQuestions = [];
                    let currentQuestionIndex = 0;
                    let score = 0;

                    function resetAppWithNewData(newData) {
                        anatomyData = newData;

                        currentLayer = Object.keys(anatomyData)[0] || "";
                        selectedPart = anatomyData[currentLayer]
                            ? Object.keys(anatomyData[currentLayer])[0]
                            : "";
                        currentFactIndex = 0;

                        if (currentLayer && selectedPart) {
                            init();
                        } else {
                            console.error("Invalid or empty data loaded.");
                            factText.textContent =
                                "Error: The loaded data file is invalid or empty.";
                        }
                    }

                    function updateDisplay() {
                        const facts = anatomyData[currentLayer][selectedPart];
                        factText.textContent = facts[currentFactIndex];
                        funfactTitle.textContent = `Skeleton Display : ${selectedPart} (${currentLayer})`;
                        bodyPartSelector.value = selectedPart;

                        // Hide all models
                        skullModel.classList.remove("visible");
                        ribCageModel.classList.remove("visible");
                        legModel.classList.remove("visible");
                        armModel.classList.remove("visible");

                        const part = selectedPart.toLowerCase();

                        // Show the correct model
                        if (part === "head" || part === "skull" || part === "brain") {
                            skullModel.classList.add("visible");
                        } else if (
                            part === "torso" ||
                            part === "rib cage" ||
                            part === "heart" ||
                            part === "lungs" ||
                            part === "stomach" ||
                            part === "liver"
                        ) {
                            ribCageModel.classList.add("visible");
                        } else if (part === "leg" || part === "legs") { // Added condition
                            legModel.classList.add("visible");
                        } else if (part === "arm" || part === "arms") { // Added condition
                            armModel.classList.add("visible");
                        }
                    }
                    // --- END updateDisplay MODIFICATION ---

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
                        selectedPart = Object.keys(
                            anatomyData[currentLayer],
                        )[0];
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
                        const totalFacts =
                            anatomyData[currentLayer][selectedPart].length;
                        currentFactIndex = (currentFactIndex + 1) % totalFacts;
                        updateDisplay();
                    });

                    prevFactBtn.addEventListener("click", () => {
                        const totalFacts =
                            anatomyData[currentLayer][selectedPart].length;
                        currentFactIndex =
                            (currentFactIndex - 1 + totalFacts) % totalFacts;
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
                            document.body.classList.add(
                                "instructor-mode-active",
                            );
                        } else {
                            modeIndicator.textContent = "Student";
                            modeIndicator.classList.remove("instructor-mode");
                            document.body.classList.remove(
                                "instructor-mode-active",
                            );
                        }
                    });

                    jsonUpload.addEventListener("change", (event) => {
                        const file = event.target.files[0];
                        if (!file || file.type !== "application/json") {
                            console.warn("Please select a valid JSON file.");
                            return;
                        }

                        const reader = new FileReader();
                        reader.onload = (e) => {
                            try {
                                const newAnatomyData = JSON.parse(
                                    e.target.result,
                                );
                                resetAppWithNewData(newAnatomyData);
                            } catch (error) {
                                console.error("JSON Parse Error:", error);
                            }
                        };
                        reader.readAsText(file);
                    });

                    function openQuizModal() {
                        populateQuizForm();
                        quizModalOverlay.classList.add("visible");
                    }

                    function closeQuizModal() {
                        quizModalOverlay.classList.remove("visible");
                        setTimeout(() => {
                            quizViewContainer.style.display = "block";
                            quizQuestionsContainer.style.display = "none";
                            quizResults.style.display = "none";
                            quizQuestionContent.innerHTML = "";
                            quizResults.innerHTML = "";
                        }, 300);
                    }

                    function populateQuizForm() {
                        quizTopicForm.innerHTML = "";
                        for (const layer in anatomyData) {
                            const layerGroup = document.createElement("div");
                            layerGroup.className = "quiz-layer-group";
                            layerGroup.innerHTML = `<h3>${layer}</h3>`;

                            for (const part in anatomyData[layer]) {
                                const checkboxId =
                                    `quiz-${layer}-${part}`.replace(
                                        /\s+/g,
                                        "-",
                                    );
                                const checkboxWrapper =
                                    document.createElement("label");
                                checkboxWrapper.className =
                                    "quiz-checkbox-container";
                                checkboxWrapper.setAttribute("for", checkboxId);
                                checkboxWrapper.textContent = part;

                                const checkbox =
                                    document.createElement("input");
                                checkbox.type = "checkbox";
                                checkbox.id = checkboxId;
                                checkbox.value = `${layer}|${part}`;

                                const checkmark =
                                    document.createElement("span");
                                checkmark.className = "checkmark";

                                checkboxWrapper.appendChild(checkbox);
                                checkboxWrapper.appendChild(checkmark);
                                layerGroup.appendChild(checkboxWrapper);
                            }
                            quizTopicForm.appendChild(layerGroup);
                        }
                    }

                    function generateQuiz() {
                        const selectedTopics = [];
                        quizTopicForm
                            .querySelectorAll('input[type="checkbox"]:checked')
                            .forEach((cb) => {
                                selectedTopics.push(cb.value.split("|"));
                            });

                        if (selectedTopics.length === 0) {
                            // A custom modal alert would be better here
                            alert("Please select at least one topic.");
                            return;
                        }

                        let allFacts = [];
                        Object.values(anatomyData).forEach((layer) => {
                            Object.values(layer).forEach((facts) => {
                                allFacts.push(...facts);
                            });
                        });
                        allFacts = [...new Set(allFacts)]; // Remove duplicates

                        quizQuestions = selectedTopics.map(([layer, part]) => {
                            const correctFacts = anatomyData[layer][part];
                            const correctAnswer =
                                correctFacts[
                                    Math.floor(
                                        Math.random() * correctFacts.length,
                                    )
                                ];

                            let incorrectAnswers = [];
                            while (incorrectAnswers.length < 3) {
                                const randomFact =
                                    allFacts[
                                        Math.floor(
                                            Math.random() * allFacts.length,
                                        )
                                    ];
                                if (
                                    randomFact !== correctAnswer &&
                                    !incorrectAnswers.includes(randomFact) &&
                                    !correctFacts.includes(randomFact)
                                ) {
                                    incorrectAnswers.push(randomFact);
                                }
                            }

                            return {
                                question: `Which of the following is true about the ${part}?`,
                                answers: [
                                    ...incorrectAnswers,
                                    correctAnswer,
                                ].sort(() => Math.random() - 0.5),
                                correctAnswer: correctAnswer,
                            };
                        });

                        currentQuestionIndex = 0;
                        score = 0;
                        quizViewContainer.style.display = "none";
                        quizQuestionsContainer.style.display = "block";
                        quizResults.style.display = "none";
                        displayQuestion();
                    }

                    function displayQuestion() {
                        nextQuestionBtn.style.display = "none";

                        if (currentQuestionIndex >= quizQuestions.length) {
                            showResults();
                            return;
                        }

                        const q = quizQuestions[currentQuestionIndex];
                        quizQuestionContent.innerHTML = `
                            <div class="question">Q${currentQuestionIndex + 1}: ${q.question}</div>
                            <div class="answers">
                                ${q.answers.map((answer) => `<button class="answer-btn">${answer}</button>`).join("")}
                            </div>
                        `;

                        document
                            .querySelectorAll(".answer-btn")
                            .forEach((btn) => {
                                btn.addEventListener("click", handleAnswer);
                            });
                    }

                    function handleAnswer(e) {
                        const selectedButton = e.target;
                        const selectedAnswer = selectedButton.textContent;
                        const correctAnswer =
                            quizQuestions[currentQuestionIndex].correctAnswer;

                        const answerButtons =
                            document.querySelectorAll(".answer-btn");

                        answerButtons.forEach((btn) => {
                            btn.disabled = true;
                            if (btn.textContent === correctAnswer) {
                                btn.classList.add("correct");
                            }
                        });

                        if (selectedAnswer === correctAnswer) {
                            score++;
                        } else {
                            selectedButton.classList.add("incorrect");
                        }

                        if (currentQuestionIndex < quizQuestions.length - 1) {
                            nextQuestionBtn.textContent = "Next Question";
                        } else {
                            nextQuestionBtn.textContent = "Show Results";
                        }
                        nextQuestionBtn.style.display = "block";
                    }

                    function showResults() {
                        quizQuestionsContainer.style.display = "none";
                        quizResults.style.display = "block";
                        quizResults.innerHTML = `
                            <h2>Quiz Complete!</h2>
                            <p>You scored ${score} out of ${quizQuestions.length}</p>
                            <button id="retake-quiz-btn" class="quiz-button">Try Again</button>
                        `;

                        document
                            .getElementById("retake-quiz-btn")
                            .addEventListener("click", () => {
                                quizResults.style.display = "none";
                                quizViewContainer.style.display = "block";
                            });
                    }

                    quizBtn.addEventListener("click", openQuizModal);
                    quizCloseBtn.addEventListener("click", closeQuizModal);
                    quizModalOverlay.addEventListener("click", (e) => {
                        if (e.target === quizModalOverlay) closeQuizModal();
                    });
                    startQuizBtn.addEventListener("click", generateQuiz);
                    nextQuestionBtn.addEventListener("click", () => {
                        currentQuestionIndex++;
                        displayQuestion();
                    });

                    function init() {
                        renderLayerOptions();
                        renderBodyParts();
                        updateDisplay();
                    }

                    try {
                        const response = await fetch(
                            "./public/sample_facts.json", // Make sure this path is correct
                        );
                        if (!response.ok)
                            throw new Error(
                                `HTTP error! Status: ${response.status}`,
                            );
                        const initialData = await response.json();
                        resetAppWithNewData(initialData);
                    } catch (error) {
                        console.error(
                            "Failed to initialize the application:",
                            error,
                        );
                        document.body.innerHTML = `<div style="color: red; padding: 20px;"><h2>Error: Could not load initial data.</h2><p>Please make sure the file 'sample_facts.json' exists and is accessible.</p></div>`;
                    }
                }
                initializeApp();
            });
        </script>
    </body>
</html>