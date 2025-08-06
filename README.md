<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Relatório Técnico</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for better aesthetics and mobile responsiveness */
        html, body {
            margin: 0;
            padding: 0;
            width: 100%; /* Ensure both take full width */
            height: 100%; /* Ensure both take full height */
            overflow-x: hidden; /* Prevent horizontal scroll */
        }

        body {
            font-family: "Inter", sans-serif;
            background-color: #f0f4f8; /* Light blue-gray background */
            /* Removed display: flex, justify-content, align-items from body for block centering */
            min-height: 100vh; /* Ensures body takes full viewport height */
            padding: 20px; /* Padding around the main container */
            box-sizing: border-box;
        }
        .container {
            background-color: #ffffff;
            border-radius: 16px; /* More rounded corners */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); /* Softer shadow */
            padding: 24px;
            width: 100%;
            max-width: 800px; /* Max width for desktop */
            display: flex;
            flex-direction: column;
            gap: 20px;
            margin: 0 auto; /* This is the key for horizontal centering of the block element */
        }
        h1, h2 {
            color: #1a202c; /* Darker text for headings */
        }
        input[type="text"], textarea {
            padding: 12px 16px;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            outline: none;
            transition: border-color 0.3s ease;
            width: 100%;
            box-sizing: border-box; /* Include padding and border in the element's total width and height */
        }
        input[type="text"]:focus, textarea:focus {
            border-color: #3b82f6; /* Blue focus border */
        }
        button {
            background-color: #3b82f6; /* Blue button */
            color: white;
            padding: 12px 20px;
            border-radius: 12px;
            transition: background-color 0.3s ease, transform 0.1s ease;
            cursor: pointer;
            flex-grow: 1; /* Allow buttons to grow */
        }
        button:hover {
            background-color: #2563eb; /* Darker blue on hover */
            transform: translateY(-1px); /* Slight lift effect */
        }
        .section-card {
            background-color: #f8fafc; /* Lighter background for sections */
            padding: 16px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05); /* Subtle shadow for sections */
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        .camera-controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            margin-top: 10px;
        }
        .camera-buttons-row {
            display: flex;
            gap: 10px;
            width: 100%;
            justify-content: space-between;
        }
        .camera-controls video {
            width: 100%;
            max-width: 400px; /* Max width for video stream */
            height: auto;
            border-radius: 8px;
            background-color: #000;
        }
        .camera-controls canvas {
            display: none; /* Canvas is hidden, used for capturing */
        }
        .photo-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            margin-top: 10px;
        }
        .photo-preview {
            width: 100%;
            max-width: 300px; /* Max width for preview image */
            height: auto;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
            object-fit: cover;
        }
        .photo-comment {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #cbd5e0;
            border-radius: 8px;
            font-size: 0.9em;
            resize: vertical; /* Allow vertical resizing */
        }
        .remove-photo-button {
            background-color: #ef4444; /* Red button for removal */
            color: white;
            padding: 8px 16px;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .remove-photo-button:hover {
            background-color: #dc2626; /* Darker red on hover */
        }
        .message-box {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #333;
            color: white;
            padding: 15px 25px;
            border-radius: 8px;
            z-index: 1000;
            display: none;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
            text-align: center;
        }
        .message-box.show {
            display: block;
            opacity: 1;
        }
        .loading-indicator {
            display: none;
            text-align: center;
            margin-top: 10px;
            font-weight: bold;
            color: #3b82f6;
        }

        /* Styles for printing */
        @media print {
            html, body {
                margin: 0;
                padding: 0;
                width: 100%;
                height: auto; /* Auto height for print */
                overflow: visible; /* Allow content to flow */
            }
            body {
                background-color: #fff;
                display: block;
                min-height: auto;
                padding: 0; /* Remove padding for print */
            }
            .container {
                box-shadow: none;
                border-radius: 0;
                padding: 0;
                max-width: none;
                width: auto;
                margin: 0; /* Remove auto margin for print */
            }
            .no-print {
                display: none !important; /* Ensure anything with no-print is hidden */
            }
            .section-card {
                box-shadow: none;
                border: 1px solid #e2e8f0;
                margin-bottom: 15px;
                page-break-inside: avoid; /* Prevent breaking inside a section */
            }
            .photo-preview {
                max-width: 100% !important; /* Ensure images fit within print area */
                display: block !important; /* Force display for print */
            }
            .photo-preview[src*="placehold.co"] { /* Hides placeholder image on print */
                display: none !important;
            }
            .photo-container { /* Force photo container to display on print */
                display: block !important;
            }
            .photo-comment {
                border: none !important; /* No border for comments in print */
                padding: 0 !important;
                margin-top: 2px !important;
                font-size: 0.8em !important;
                display: block !important; /* Force display for print */
                background-color: transparent !important; /* Ensure no background in print */
                color: #000 !important; /* Ensure text is black */
                resize: none !important; /* Disable resizing in print */
                overflow: visible !important; /* Ensure all content is visible */
                height: auto !important; /* Allow height to adjust to content */
            }
            textarea { /* General textarea style for print */
                border: none !important;
                background-color: transparent !important;
                color: #000 !important;
                resize: none !important;
                overflow: visible !important;
                height: auto !important;
                padding: 0 !important;
                margin: 0 !important;
            }
            input[type="text"]::placeholder,
            textarea::placeholder { /* Hides placeholder text on print */
                color: transparent !important;
            }
        }

        /* Responsive adjustments */
        @media (max-width: 640px) {
            body {
                padding: 10px; /* Adjusted padding for smaller screens */
            }
            .container {
                padding: 16px;
                gap: 15px;
            }
            button {
                width: 100%;
            }
            .camera-buttons-row {
                flex-direction: column;
                gap: 5px;
            }
            .camera-controls video {
                max-width: 100%;
            }
            .photo-preview {
                max-width: 100%;
            }
        }
    </style>
</head>
<body class="bg-gray-100">
    <div class="container">
        <h1 class="text-3xl font-bold text-center text-gray-800 mb-4">Relatório Técnico</h1>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700"> Relatório</h2>
            <label for="reportTitle" class="text-gray-600">ID Máquina:</label>
            <input type="text" id="reportTitle" placeholder="Ex: TE 50XX /O.M 2025xxx.../Horimetro" class="text-xl">

            <label for="reportDate" class="text-gray-600">Data:</label>
            <input type="date" id="reportDate" class="text-base">

            <label for="reportAuthor" class="text-gray-600">Autor:</label>
            <input type="text" id="reportAuthor" placeholder="Seu Nome" class="text-base">
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">1. Descrição</h2>
            <textarea id="problemDescription" rows="5" placeholder="Descreva o problema, a situação ou o objetivo do relatório..." class="text-base"></textarea>
            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoProblem" data-target-canvas="canvasProblem" data-target-img="imgProblem" data-target-comment="commentProblem" data-target-container="photoContainerProblem">Tirar Foto</button>
                    <input type="file" id="fileInputProblem" accept="image/*" class="hidden" data-target-img="imgProblem" data-target-comment="commentProblem" data-target-container="photoContainerProblem">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputProblem">Adicionar Arquivo</button>
                </div>
                <video id="videoProblem" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasProblem" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoProblem" data-canvas-id="canvasProblem" data-img-id="imgProblem" data-comment-id="commentProblem" data-target-container="photoContainerProblem">Capturar Foto</button>
            </div>
            <div id="photoContainerProblem" class="photo-container hidden">
                <img id="imgProblem" class="photo-preview" alt="Foto da Descrição" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
                <textarea id="commentProblem" class="photo-comment" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
                <button class="remove-photo-button no-print" data-target-img="imgProblem" data-target-comment="commentProblem" data-target-container="photoContainerProblem">Remover Foto</button>
            </div>
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">2. Ações Realizadas/Solução</h2>
            <textarea id="solutionProposed" rows="5" placeholder="Descreva as ações realizadas, a solução implementada ou os resultados..." class="text-base"></textarea>
            <div id="solutionPhotosContainer">
                <!-- Novos blocos de foto serão inseridos dinamicamente aqui -->
            </div>
            <div class="no-print">
                <button id="addSolutionPhotoButton" class="bg-blue-500 hover:bg-blue-600 w-full">Adicionar Foto</button>
            </div>
        </div>

        <div class="section-card">
            <h2 class="text-2xl font-semibold text-gray-700">3. Observações/Pendências/Executantes</h2>
            <textarea id="additionalObservations" rows="5" placeholder="Adicione quaisquer observações relevantes as Pendências e os Executantes..." class="text-base"></textarea>
            <div class="camera-controls no-print">
                <div class="camera-buttons-row">
                    <button class="take-photo-button" data-target-video="videoObservations" data-target-canvas="canvasObservations" data-target-img="imgObservations" data-target-comment="commentObservations" data-target-container="photoContainerObservations">Tirar Foto</button>
                    <input type="file" id="fileInputObservations" accept="image/*" class="hidden" data-target-img="imgObservations" data-target-comment="commentObservations" data-target-container="photoContainerObservations">
                    <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-target-input="fileInputObservations">Adicionar Arquivo</button>
                </div>
                <video id="videoObservations" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                <canvas id="canvasObservations" class="hidden"></canvas>
                <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-video-id="videoObservations" data-canvas-id="canvasObservations" data-img-id="imgObservations" data-comment-id="commentObservations" data-target-container="photoContainerObservations">Capturar Foto</button>
            </div>
            <div id="photoContainerObservations" class="photo-container hidden">
                <img id="imgObservations" class="photo-preview" alt="Foto de Observações" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
                <textarea id="commentObservations" class="photo-comment" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
                <button class="remove-photo-button no-print" data-target-img="imgObservations" data-target-comment="commentObservations" data-target-container="photoContainerObservations">Remover Foto</button>
            </div>
        </div>

        <button id="generateReportButton" class="bg-blue-500 text-white p-3 rounded-xl hover:bg-blue-600 transition-all duration-300 shadow-md mt-4 no-print">Gerar Relatório (Imprimir/PDF)</button>
    </div>

    <div id="messageBox" class="message-box"></div>

    <script>
        let currentStream = null; // To hold the active camera stream
        let activeVideoElement = null; // To track the currently active video element
        let activeCanvasElement = null; // To track the currently active canvas element
        let activeImageElement = null; // To track the currently active image element
        let activeCaptureButton = null; // To track the currently active capture button
        let activeCommentElement = null; // To track the currently active comment element

        const messageBox = document.getElementById('messageBox');
        const defaultPlaceholder = "https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto";
        
        let photoCount = 0; // Counter for photos in section 2
        const maxPhotos = 20; // Maximum number of photos allowed
        const solutionPhotosContainer = document.getElementById('solutionPhotosContainer');
        const addSolutionPhotoButton = document.getElementById('addSolutionPhotoButton');

        /**
         * Displays a temporary message box with the given text.
         * @param {string} message - The message to display.
         * @param {number} duration - The duration in milliseconds to show the message.
         */
        function showMessageBox(message, duration = 3000) {
            messageBox.textContent = message;
            messageBox.classList.add('show');
            setTimeout(() => {
                messageBox.classList.remove('show');
            }, duration);
        }

        /**
         * Stops the current camera stream if active and hides associated elements.
         */
        function stopCameraStream() {
            if (currentStream) {
                currentStream.getTracks().forEach(track => track.stop());
                currentStream = null;
            }
            if (activeVideoElement) {
                activeVideoElement.style.display = 'none';
                activeVideoElement.srcObject = null;
                activeVideoElement = null;
            }
            if (activeCaptureButton) {
                activeCaptureButton.style.display = 'none';
                activeCaptureButton = null;
            }
            // Reset active elements related to camera/file input
            activeCanvasElement = null;
            activeImageElement = null;
            activeCommentElement = null;
        }

        /**
         * Initializes camera access for a specific section.
         * @param {HTMLVideoElement} videoElement - The video element to display the stream.
         * @param {HTMLCanvasElement} canvasElement - The canvas element for capturing.
         * @param {HTMLImageElement} imageElement - The image element to display the captured photo.
         * @param {HTMLButtonElement} captureButton - The capture button for this section.
         * @param {HTMLTextAreaElement} commentElement - The textarea element for comments.
         * @param {HTMLElement} photoContainer - The container for the photo and comment elements.
         */
        async function startCamera(videoElement, canvasElement, imageElement, captureButton, commentElement, photoContainer) {
            // Stop any previously active camera stream or file input view
            stopCameraStream();

            activeVideoElement = videoElement;
            activeCanvasElement = canvasElement;
            activeImageElement = imageElement;
            activeCaptureButton = captureButton;
            activeCommentElement = commentElement; // Set active comment element

            // Hide the photo container if starting camera
            photoContainer.classList.add('hidden');

            activeVideoElement.classList.remove('hidden'); // Show video element
            activeCaptureButton.classList.remove('hidden'); // Show capture button

            const constraints = {
                video: {
                    // Try to get the environment (rear) camera first
                    facingMode: { exact: "environment" }
                }
            };

            try {
                // Attempt to get the environment camera
                currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                activeVideoElement.srcObject = currentStream;
                activeVideoElement.play();
            } catch (err) {
                console.warn("Failed to get environment camera, trying user camera:", err);
                // If environment camera fails, try the user (front) camera
                constraints.video.facingMode = "user";
                try {
                    currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                    activeVideoElement.srcObject = currentStream;
                    activeVideoElement.play();
                } catch (userErr) {
                    console.error("Error accessing any camera: ", userErr);
                    showMessageBox('Não foi possível acessar a câmera. Verifique as permissões.');
                    activeVideoElement.classList.add('hidden');
                    activeCaptureButton.classList.add('hidden');
                    stopCameraStream(); // Ensure all related elements are hidden
                }
            }
        }

        /**
         * Sets up event listeners for a new photo section.
         * @param {string} uniqueId - The unique ID for the new section.
         */
        function setupPhotoSectionListeners(uniqueId) {
            const takePhotoButton = document.querySelector(`.take-photo-button[data-id="${uniqueId}"]`);
            const addFileButton = document.querySelector(`.add-file-button[data-id="${uniqueId}"]`);
            const captureButton = document.querySelector(`.capture-button[data-id="${uniqueId}"]`);
            const removePhotoButton = document.querySelector(`.remove-photo-button[data-id="${uniqueId}"]`);
            const fileInput = document.getElementById(`fileInputSolution${uniqueId}`);
            
            // "Tirar Foto" button listener
            takePhotoButton.addEventListener('click', () => {
                const video = document.getElementById(`videoSolution${uniqueId}`);
                const canvas = document.getElementById(`canvasSolution${uniqueId}`);
                const img = document.getElementById(`imgSolution${uniqueId}`);
                const comment = document.getElementById(`commentSolution${uniqueId}`);
                const photoContainer = document.getElementById(`photoContainerSolution${uniqueId}`);
                startCamera(video, canvas, img, captureButton, comment, photoContainer);
            });

            // "Adicionar Arquivo" button listener
            addFileButton.addEventListener('click', () => {
                if (fileInput) {
                    stopCameraStream(); // Stop camera if active
                    fileInput.click(); // Programmatically click the hidden file input
                }
            });

            // "Capturar Foto" button listener
            captureButton.addEventListener('click', () => {
                const video = document.getElementById(`videoSolution${uniqueId}`);
                const canvas = document.getElementById(`canvasSolution${uniqueId}`);
                const img = document.getElementById(`imgSolution${uniqueId}`);
                const photoContainer = document.getElementById(`photoContainerSolution${uniqueId}`);
                
                if (video && currentStream) {
                    const context = canvas.getContext('2d');
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;
                    context.drawImage(video, 0, 0, canvas.width, canvas.height);

                    const imageDataURL = canvas.toDataURL('image/png');
                    img.src = imageDataURL;
                    
                    photoContainer.classList.remove('hidden');
                    stopCameraStream();
                    showMessageBox('Foto capturada!');
                } else {
                    showMessageBox('Nenhuma câmera ativa para capturar.');
                }
            });

            // "Remover Foto" button listener
            removePhotoButton.addEventListener('click', () => {
                const photoBlock = document.getElementById(`photoBlock${uniqueId}`);
                if (photoBlock) {
                    photoBlock.remove();
                    photoCount--;
                    // Enable the add button if we are no longer at the max limit
                    addSolutionPhotoButton.disabled = false;
                    addSolutionPhotoButton.textContent = `Adicionar Foto (${photoCount}/${maxPhotos})`;
                    showMessageBox('Foto removida!');
                }
            });

            // Hidden file input listener
            fileInput.addEventListener('change', (event) => {
                const file = event.target.files[0];
                if (file) {
                    const imgElement = document.getElementById(`imgSolution${uniqueId}`);
                    const photoContainer = document.getElementById(`photoContainerSolution${uniqueId}`);
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        if (imgElement) {
                            imgElement.src = e.target.result;
                            photoContainer.classList.remove('hidden');
                        }
                        showMessageBox('Arquivo adicionado!');
                    };
                    reader.readAsDataURL(file);
                } else {
                    showMessageBox('Nenhum arquivo selecionado.');
                }
            });
        }

        // Main event listener for "Adicionar Foto" button in section 2
        addSolutionPhotoButton.addEventListener('click', () => {
            if (photoCount < maxPhotos) {
                photoCount++;
                const uniqueId = `SolutionPhoto${photoCount}`;

                const photoBlockHTML = `
                    <div id="photoBlock${uniqueId}" class="photo-block">
                        <div class="camera-controls no-print">
                            <div class="camera-buttons-row">
                                <button class="take-photo-button" data-id="${uniqueId}">Tirar Foto ${photoCount}</button>
                                <input type="file" id="fileInputSolution${uniqueId}" accept="image/*" class="hidden">
                                <button class="add-file-button bg-gray-500 hover:bg-gray-600" data-id="${uniqueId}">Adicionar Arquivo ${photoCount}</button>
                            </div>
                            <video id="videoSolution${uniqueId}" autoplay playsinline class="rounded-lg shadow-md hidden"></video>
                            <canvas id="canvasSolution${uniqueId}" class="hidden"></canvas>
                            <button class="capture-button bg-green-500 text-white p-2 rounded-lg hover:bg-green-600 transition-all duration-200 hidden" data-id="${uniqueId}">Capturar Foto</button>
                        </div>
                        <div id="photoContainerSolution${uniqueId}" class="photo-container hidden">
                            <img id="imgSolution${uniqueId}" class="photo-preview" alt="Foto da Solução ${photoCount}" src="https://placehold.co/300x200/e2e8f0/64748b?text=Sem+Foto">
                            <textarea id="commentSolution${uniqueId}" class="photo-comment" rows="2" placeholder="Adicionar comentário para esta foto..."></textarea>
                            <button class="remove-photo-button no-print" data-id="${uniqueId}">Remover Foto</button>
                        </div>
                    </div>
                `;
                
                solutionPhotosContainer.insertAdjacentHTML('beforeend', photoBlockHTML);
                setupPhotoSectionListeners(uniqueId);
                
                addSolutionPhotoButton.textContent = `Adicionar Foto (${photoCount}/${maxPhotos})`;
                if (photoCount === maxPhotos) {
                    addSolutionPhotoButton.disabled = true;
                    showMessageBox('Limite de 20 fotos atingido.');
                }
            } else {
                showMessageBox('Limite de fotos atingido.');
            }
        });


        // Event listeners for static photo sections (problem and observations)
        document.querySelectorAll('.take-photo-button').forEach(button => {
            if (!button.dataset.id) { // Only for static buttons
                button.addEventListener('click', (event) => {
                    const targetVideoId = event.target.dataset.targetVideo;
                    const targetCanvasId = event.target.dataset.targetCanvas;
                    const targetImgId = event.target.dataset.targetImg;
                    const targetCommentId = event.target.dataset.targetComment;
                    const targetContainerId = event.target.dataset.targetContainer;

                    const video = document.getElementById(targetVideoId);
                    const canvas = document.getElementById(targetCanvasId);
                    const img = document.getElementById(targetImgId);
                    const captureBtn = document.querySelector(`.capture-button[data-video-id="${targetVideoId}"]`);
                    const comment = document.getElementById(targetCommentId);
                    const photoContainer = document.getElementById(targetContainerId);

                    startCamera(video, canvas, img, captureBtn, comment, photoContainer);
                });
            }
        });

        document.querySelectorAll('.capture-button').forEach(button => {
            if (!button.dataset.id) { // Only for static buttons
                button.addEventListener('click', (event) => {
                    if (activeVideoElement && currentStream) {
                        const targetVideoId = event.target.dataset.videoId;
                        const targetCanvasId = event.target.dataset.canvasId;
                        const targetImgId = event.target.dataset.imgId;
                        const targetContainerId = event.target.dataset.targetContainer;

                        const video = document.getElementById(targetVideoId);
                        const canvas = document.getElementById(targetCanvasId);
                        const img = document.getElementById(targetImgId);
                        const photoContainer = document.getElementById(targetContainerId);

                        const context = canvas.getContext('2d');
                        canvas.width = video.videoWidth;
                        canvas.height = video.videoHeight;
                        context.drawImage(video, 0, 0, canvas.width, canvas.height);

                        const imageDataURL = canvas.toDataURL('image/png');
                        img.src = imageDataURL;
                        
                        photoContainer.classList.remove('hidden');
                        stopCameraStream();
                        showMessageBox('Foto capturada!');
                    } else {
                        showMessageBox('Nenhuma câmera ativa para capturar.');
                    }
                });
            }
        });

        // Event listeners for "Adicionar Arquivo" buttons
        document.querySelectorAll('.add-file-button').forEach(button => {
            if (!button.dataset.id) { // Only for static buttons
                button.addEventListener('click', (event) => {
                    const targetInputId = event.target.dataset.targetInput;
                    const fileInput = document.getElementById(targetInputId);
                    if (fileInput) {
                        stopCameraStream(); // Stop camera if active
                        fileInput.click(); // Programmatically click the hidden file input
                    }
                });
            }
        });

        // Event listeners for hidden file inputs (static)
        document.querySelectorAll('input[type="file"]').forEach(input => {
            if (!input.dataset.id) { // Only for static inputs
                input.addEventListener('change', (event) => {
                    const file = event.target.files[0];
                    if (file) {
                        const targetImgId = event.target.dataset.targetImg;
                        const targetContainerId = event.target.dataset.targetContainer;

                        const imgElement = document.getElementById(targetImgId);
                        const photoContainer = document.getElementById(targetContainerId);

                        const reader = new FileReader();
                        reader.onload = (e) => {
                            if (imgElement) {
                                imgElement.src = e.target.result;
                                photoContainer.classList.remove('hidden');
                            }
                            showMessageBox('Arquivo adicionado!');
                        };
                        reader.readAsDataURL(file);
                    } else {
                        showMessageBox('Nenhum arquivo selecionado.');
                    }
                });
            }
        });

        // Event listeners for "Remover Foto" buttons (static sections)
        document.querySelectorAll('.remove-photo-button').forEach(button => {
            if (!button.dataset.id) { // Only for static buttons
                button.addEventListener('click', (event) => {
                    const targetImgId = event.target.dataset.targetImg;
                    const targetCommentId = event.target.dataset.targetComment;
                    const targetContainerId = event.target.dataset.targetContainer;

                    const imgElement = document.getElementById(targetImgId);
                    const commentElement = document.getElementById(targetCommentId);
                    const photoContainer = document.getElementById(targetContainerId);

                    imgElement.src = defaultPlaceholder;
                    commentElement.value = '';
                    photoContainer.classList.add('hidden');
                    showMessageBox('Foto removida!');
                });
            }
        });

        // Event listener for Generate Report button
        document.getElementById('generateReportButton').addEventListener('click', () => {
            stopCameraStream(); // Ensure camera is off before printing
            window.print(); // Triggers the browser's print dialog
        });

        // Set current date on load
        document.addEventListener('DOMContentLoaded', () => {
            const today = new Date();
            const year = today.getFullYear();
            const month = String(today.getMonth() + 1).padStart(2, '0'); // Months are 0-indexed
            const day = String(today.getDate()).padStart(2, '0');
            document.getElementById('reportDate').value = `${year}-${month}-${day}`;
        });
    </script>
</body>
</html>
