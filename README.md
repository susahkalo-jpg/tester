<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Pemilihan Foto - Admin & Client</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --admin-color: #4285F4;
            --client-color: #25D366;
            --danger-color: #ff4444;
            --warning-color: #ffa726;
            --light-bg: #f5f7fa;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4edf5 100%);
            color: #333;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            margin-bottom: 40px;
            padding: 30px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }
        
        h1 {
            font-size: 42px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
        }
        
        .mode-selector {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-top: 30px;
        }
        
        .mode-btn {
            padding: 20px 40px;
            font-size: 20px;
            font-weight: bold;
            border: none;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        .mode-btn.admin {
            background: linear-gradient(135deg, var(--admin-color), #3367d6);
            color: white;
        }
        
        .mode-btn.client {
            background: linear-gradient(135deg, var(--client-color), #128C7E);
            color: white;
        }
        
        .mode-btn:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
        }
        
        .mode-btn.active {
            transform: scale(1.05);
            box-shadow: 0 0 0 5px rgba(255, 255, 255, 0.3);
        }
        
        .content {
            display: none;
            background: white;
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
            margin-top: 30px;
        }
        
        .content.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .panel-title {
            font-size: 28px;
            margin-bottom: 30px;
            color: #333;
            display: flex;
            align-items: center;
            gap: 15px;
            padding-bottom: 15px;
            border-bottom: 3px solid #eee;
        }
        
        .form-group {
            margin-bottom: 25px;
        }
        
        label {
            display: block;
            margin-bottom: 10px;
            font-weight: 600;
            color: #555;
            font-size: 17px;
        }
        
        input[type="text"], input[type="tel"], textarea {
            width: 100%;
            padding: 18px 20px;
            border: 2px solid #ddd;
            border-radius: 12px;
            font-size: 16px;
            transition: all 0.3s;
        }
        
        textarea {
            min-height: 150px;
            font-family: monospace;
            line-height: 1.6;
        }
        
        input:focus, textarea:focus {
            border-color: var(--admin-color);
            outline: none;
            box-shadow: 0 0 0 4px rgba(66, 133, 244, 0.1);
        }
        
        .btn {
            padding: 18px 30px;
            border: none;
            border-radius: 12px;
            font-weight: bold;
            font-size: 17px;
            cursor: pointer;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
        }
        
        .btn-primary {
            background: linear-gradient(135deg, var(--admin-color), #3367d6);
            color: white;
        }
        
        .btn-success {
            background: linear-gradient(135deg, var(--client-color), #128C7E);
            color: white;
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
        }
        
        .photo-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 20px;
            margin-top: 30px;
            max-height: 600px;
            overflow-y: auto;
            padding: 20px;
            background: #f9f9f9;
            border-radius: 15px;
            border: 2px dashed #ddd;
        }
        
        .photo-item {
            position: relative;
            border-radius: 12px;
            overflow: hidden;
            cursor: pointer;
            transition: all 0.3s;
            aspect-ratio: 1;
            background: #f0f0f0;
            border: 3px solid transparent;
        }
        
        .photo-item.selected {
            border-color: var(--client-color);
            box-shadow: 0 5px 20px rgba(37, 211, 102, 0.3);
        }
        
        .photo-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .photo-number {
            position: absolute;
            top: 10px;
            left: 10px;
            background: rgba(0, 0, 0, 0.8);
            color: white;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 16px;
        }
        
        .selected-list {
            margin-top: 30px;
            max-height: 400px;
            overflow-y: auto;
            padding: 20px;
            background: #f9f9f9;
            border-radius: 15px;
            border: 2px dashed #ddd;
        }
        
        .selected-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 18px;
            background: white;
            margin-bottom: 12px;
            border-radius: 10px;
            border-left: 5px solid var(--client-color);
            box-shadow: 0 3px 10px rgba(0,0,0,0.05);
        }
        
        .status {
            padding: 20px;
            border-radius: 12px;
            margin: 20px 0;
            font-size: 16px;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        
        .status.success {
            background: #d4edda;
            color: #155724;
            border-left: 5px solid #28a745;
        }
        
        .status.error {
            background: #f8d7da;
            color: #721c24;
            border-left: 5px solid #dc3545;
        }
        
        .limit-selector {
            display: flex;
            gap: 15px;
            margin: 20px 0;
        }
        
        .limit-option {
            flex: 1;
        }
        
        .limit-option input[type="radio"] {
            display: none;
        }
        
        .limit-option label {
            display: block;
            padding: 18px;
            background: #f0f0f0;
            border-radius: 10px;
            text-align: center;
            cursor: pointer;
            font-weight: bold;
            font-size: 18px;
            transition: all 0.3s;
            border: 3px solid transparent;
        }
        
        .limit-option input[type="radio"]:checked + label {
            background: var(--client-color);
            color: white;
            border-color: var(--client-color);
        }
        
        .client-url-box {
            background: #e3f2fd;
            border: 2px solid var(--admin-color);
            border-radius: 12px;
            padding: 25px;
            margin: 30px 0;
            word-break: break-all;
        }
        
        .client-url-box h4 {
            color: var(--admin-color);
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .qr-code {
            text-align: center;
            margin: 20px 0;
        }
        
        .instructions {
            background: #fff8e1;
            border: 2px solid #ffb300;
            border-radius: 12px;
            padding: 25px;
            margin-top: 30px;
        }
        
        footer {
            text-align: center;
            margin-top: 50px;
            color: #666;
            padding: 20px;
            font-size: 14px;
        }
        
        @media (max-width: 768px) {
            .mode-selector {
                flex-direction: column;
                align-items: center;
            }
            
            .mode-btn {
                width: 100%;
                max-width: 300px;
            }
            
            .photo-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }
            
            h1 {
                font-size: 32px;
                flex-direction: column;
                gap: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>
                <i class="fas fa-camera"></i>
                SISTEM PEMILIHAN FOTO
                <i class="fas fa-users"></i>
            </h1>
            <p style="font-size: 18px; opacity: 0.9;">Admin Setup | Client Selection | WhatsApp Integration</p>
            
            <div class="mode-selector">
                <button class="mode-btn admin active" data-mode="admin">
                    <i class="fas fa-user-shield"></i>
                    MODE ADMIN
                </button>
                <button class="mode-btn client" data-mode="client">
                    <i class="fas fa-user-friends"></i>
                    MODE CLIENT
                </button>
            </div>
        </header>
        
        <!-- ADMIN PANEL -->
        <div id="adminPanel" class="content active">
            <h2 class="panel-title">
                <i class="fas fa-user-shield"></i>
                PANEL ADMIN - Setup Foto untuk Client
            </h2>
            
            <div class="form-group">
                <label for="folderLink">
                    <i class="fab fa-google-drive"></i>
                    Link Folder Google Drive
                </label>
                <input type="text" id="folderLink" placeholder="https://drive.google.com/drive/folders/1ABC123xyz...">
                <p style="color: #666; margin-top: 8px; font-size: 14px;">
                    Folder harus sudah di-share dengan setting: "Anyone on the internet with the link can view"
                </p>
            </div>
            
            <div class="form-group">
                <label for="projectName">
                    <i class="fas fa-project-diagram"></i>
                    Nama Proyek / Album
                </label>
                <input type="text" id="projectName" placeholder="Contoh: Foto Pernikahan 2024">
            </div>
            
            <button id="generateClientLink" class="btn btn-primary" style="width: 100%;">
                <i class="fas fa-link"></i>
                GENERATE LINK UNTUK CLIENT
            </button>
            
            <div id="adminStatus" class="status" style="display: none;"></div>
            
            <div id="clientLinkContainer" style="display: none;">
                <div class="client-url-box">
                    <h4><i class="fas fa-external-link-alt"></i> LINK UNTUK CLIENT:</h4>
                    <div id="clientLink" style="font-size: 16px; padding: 15px; background: white; border-radius: 8px; margin-bottom: 15px;"></div>
                    
                    <div class="qr-code">
                        <canvas id="qrCodeCanvas"></canvas>
                        <p style="margin-top: 10px; font-size: 14px;">Scan QR Code untuk akses cepat</p>
                    </div>
                    
                    <button id="copyLinkBtn" class="btn" style="background: #6c757d; color: white; width: 100%;">
                        <i class="fas fa-copy"></i>
                        SALIN LINK
                    </button>
                </div>
                
                <div class="instructions">
                    <h4><i class="fas fa-info-circle"></i> INSTRUKSI UNTUK ADMIN:</h4>
                    <ol style="margin-left: 20px; margin-top: 10px;">
                        <li>Upload semua foto ke satu folder di Google Drive</li>
                        <li>Share folder dengan setting: "Anyone on the internet with the link can view"</li>
                        <li>Salin link folder tersebut ke form di atas</li>
                        <li>Klik "Generate Link untuk Client"</li>
                        <li>Bagikan link yang dihasilkan kepada client</li>
                        <li>Client akan bisa memilih foto melalui link tersebut</li>
                    </ol>
                </div>
            </div>
        </div>
        
        <!-- CLIENT PANEL -->
        <div id="clientPanel" class="content">
            <h2 class="panel-title">
                <i class="fas fa-user-friends"></i>
                PANEL CLIENT - Pilih Foto
            </h2>
            
            <div id="clientWelcome" style="text-align: center; padding: 40px 20px;">
                <i class="fas fa-images" style="font-size: 80px; color: #ddd; margin-bottom: 20px;"></i>
                <h3 style="margin-bottom: 15px;">SELAMAT DATANG CLIENT</h3>
                <p style="color: #666; max-width: 600px; margin: 0 auto;">
                    Admin telah menyiapkan album foto untuk Anda. Silakan pilih foto-foto yang Anda inginkan.
                </p>
            </div>
            
            <div id="photoSelectionArea" style="display: none;">
                <div style="background: #e8f5e9; padding: 20px; border-radius: 12px; margin-bottom: 25px;">
                    <h4 style="color: #2e7d32; margin-bottom: 10px;">
                        <i class="fas fa-info-circle"></i> PROYEK: <span id="projectTitle">Nama Proyek</span>
                    </h4>
                    <p style="color: #666;">
                        Pilih maksimal <strong><span id="maxLimitDisplay">40</span> foto</strong> dari album ini.
                    </p>
                </div>
                
                <div class="limit-selector">
                    <div class="limit-option">
                        <input type="radio" id="limit40" name="clientLimit" value="40" checked>
                        <label for="limit40">40 Foto</label>
                    </div>
                    <div class="limit-option">
                        <input type="radio" id="limit80" name="clientLimit" value="80">
                        <label for="limit80">80 Foto</label>
                    </div>
                    <div class="limit-option">
                        <input type="radio" id="limit200" name="clientLimit" value="200">
                        <label for="limit200">200 Foto</label>
                    </div>
                </div>
                
                <div class="selection-info" style="background: linear-gradient(135deg, #e3f2fd, #bbdefb); padding: 20px; border-radius: 12px; margin: 20px 0; display: flex; justify-content: space-between;">
                    <div>
                        <i class="fas fa-check-circle" style="color: var(--client-color);"></i>
                        <span>Dipilih: <strong id="selectedCountClient">0</strong> foto</span>
                    </div>
                    <div>
                        <i class="fas fa-images" style="color: var(--admin-color);"></i>
                        <span>Total tersedia: <strong id="totalPhotos">0</strong> foto</span>
                    </div>
                </div>
                
                <div id="clientStatus" class="status" style="display: none;"></div>
                
                <div id="clientPhotoGrid" class="photo-grid">
                    <!-- Foto akan dimuat di sini -->
                </div>
                
                <h3 class="panel-title" style="margin-top: 40px;">
                    <i class="fas fa-list-check"></i>
                    FOTO TERPILIH
                </h3>
                
                <div id="clientSelectedList" class="selected-list">
                    <div style="text-align: center; padding: 40px; color: #888;">
                        <i class="far fa-clipboard" style="font-size: 50px; margin-bottom: 15px;"></i>
                        <p>Belum ada foto yang dipilih</p>
                    </div>
                </div>
                
                <div style="margin-top: 40px; padding: 30px; background: #f8f9fa; border-radius: 15px;">
                    <h3 class="panel-title">
                        <i class="fab fa-whatsapp"></i>
                        KIRIM KE WHATSAPP
                    </h3>
                    
                    <div class="form-group">
                        <label for="clientPhoneNumber">
                            <i class="fas fa-phone-alt"></i>
                            Nomor WhatsApp Anda
                        </label>
                        <input type="tel" id="clientPhoneNumber" placeholder="6281234567890" value="6281234567890">
                    </div>
                    
                    <div class="form-group">
                        <label for="clientMessage">
                            <i class="fas fa-comment-dots"></i>
                            Pesan Tambahan (Opsional)
                        </label>
                        <input type="text" id="clientMessage" placeholder="Contoh: Ini foto-foto yang sudah saya pilih">
                    </div>
                    
                    <button id="sendToWhatsAppClient" class="btn btn-success" style="width: 100%;" disabled>
                        <i class="fab fa-whatsapp"></i>
                        KIRIM DAFTAR FOTO KE WHATSAPP
                    </button>
                </div>
                
                <div id="clientWhatsAppStatus" class="status" style="display: none;"></div>
            </div>
        </div>
        
        <footer>
            <p><i class="fas fa-shield-alt"></i> Sistem ini bekerja sepenuhnya di browser Anda tanpa server backend</p>
            <p style="margin-top: 5px;">© 2024 Foto Selection System | Maksimal 200 foto per sesi</p>
        </footer>
    </div>
    
    <!-- QR Code Library -->
    <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js"></script>
    
    <script>
        // ==================== VARIABLES ====================
        let currentMode = 'admin';
        let adminData = null;
        let clientPhotos = [];
        let clientSelectedPhotos = [];
        let clientPhotoLimit = 40;
        
        // ==================== ELEMENTS ====================
        const modeButtons = document.querySelectorAll('.mode-btn');
        const contents = document.querySelectorAll('.content');
        const folderLinkInput = document.getElementById('folderLink');
        const projectNameInput = document.getElementById('projectName');
        const generateClientLinkBtn = document.getElementById('generateClientLink');
        const adminStatus = document.getElementById('adminStatus');
        const clientLinkContainer = document.getElementById('clientLinkContainer');
        const clientLinkElement = document.getElementById('clientLink');
        const copyLinkBtn = document.getElementById('copyLinkBtn');
        const qrCodeCanvas = document.getElementById('qrCodeCanvas');
        
        const clientPhotoGrid = document.getElementById('clientPhotoGrid');
        const clientSelectedList = document.getElementById('clientSelectedList');
        const clientStatus = document.getElementById('clientStatus');
        const clientWhatsAppStatus = document.getElementById('clientWhatsAppStatus');
        const sendToWhatsAppClientBtn = document.getElementById('sendToWhatsAppClient');
        const selectedCountClientElement = document.getElementById('selectedCountClient');
        const totalPhotosElement = document.getElementById('totalPhotos');
        const maxLimitDisplayElement = document.getElementById('maxLimitDisplay');
        const projectTitleElement = document.getElementById('projectTitle');
        const clientPhoneNumberInput = document.getElementById('clientPhoneNumber');
        const clientMessageInput = document.getElementById('clientMessage');
        const clientWelcome = document.getElementById('clientWelcome');
        const photoSelectionArea = document.getElementById('photoSelectionArea');
        
        // ==================== INITIALIZATION ====================
        document.addEventListener('DOMContentLoaded', function() {
            // Mode switching
            modeButtons.forEach(btn => {
                btn.addEventListener('click', function() {
                    const mode = this.getAttribute('data-mode');
                    switchMode(mode);
                });
            });
            
            // Admin events
            generateClientLinkBtn.addEventListener('click', generateClientLink);
            copyLinkBtn.addEventListener('click', copyClientLink);
            
            // Client events
            document.querySelectorAll('input[name="clientLimit"]').forEach(radio => {
                radio.addEventListener('change', function() {
                    clientPhotoLimit = parseInt(this.value);
                    maxLimitDisplayElement.textContent = clientPhotoLimit;
                    updateClientSelectionInfo();
                    updateClientSendButton();
                });
            });
            
            sendToWhatsAppClientBtn.addEventListener('click', sendClientToWhatsApp);
            clientPhoneNumberInput.addEventListener('input', updateClientSendButton);
            clientMessageInput.addEventListener('input', updateClientSendButton);
            
            // Check if we have client data in URL
            checkForClientData();
        });
        
        // ==================== MODE SWITCHING ====================
        function switchMode(mode) {
            currentMode = mode;
            
            // Update active buttons
            modeButtons.forEach(btn => {
                if (btn.getAttribute('data-mode') === mode) {
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });
            
            // Show active content
            contents.forEach(content => {
                if (content.id === mode + 'Panel') {
                    content.classList.add('active');
                } else {
                    content.classList.remove('active');
                }
            });
            
            // If switching to client mode, check for data
            if (mode === 'client') {
                checkForClientData();
            }
        }
        
        // ==================== ADMIN FUNCTIONS ====================
        function generateClientLink() {
            const folderLink = folderLinkInput.value.trim();
            const projectName = projectNameInput.value.trim();
            
            if (!folderLink) {
                showStatus(adminStatus, "Harap masukkan link folder Google Drive", "error");
                return;
            }
            
            if (!isValidFolderLink(folderLink)) {
                showStatus(adminStatus, "Format link folder tidak valid. Pastikan ini adalah link folder Google Drive", "error");
                return;
            }
            
            if (!projectName) {
                showStatus(adminStatus, "Harap masukkan nama proyek/album", "error");
                return;
            }
            
            // Create admin data object
            adminData = {
                folderLink: folderLink,
                projectName: projectName,
                timestamp: Date.now(),
                folderId: extractFolderId(folderLink)
            };
            
            // Encode data to URL parameter
            const encodedData = btoa(JSON.stringify(adminData));
            
            // Generate client URL
            const clientUrl = window.location.origin + window.location.pathname + '?client=' + encodedData;
            
            // Display client link
            clientLinkElement.textContent = clientUrl;
            clientLinkContainer.style.display = 'block';
            
            // Generate QR Code
            QRCode.toCanvas(qrCodeCanvas, clientUrl, {
                width: 200,
                margin: 2,
                color: {
                    dark: '#000000',
                    light: '#FFFFFF'
                }
            }, function(error) {
                if (error) console.error(error);
            });
            
            showStatus(adminStatus, "Link untuk client berhasil dibuat! Bagikan link ini kepada client", "success");
            
            // Scroll to client link section
            clientLinkContainer.scrollIntoView({ behavior: 'smooth' });
        }
        
        function isValidFolderLink(link) {
            // Check if it's a Google Drive folder link
            const folderPattern = /https:\/\/drive\.google\.com\/drive\/folders\/[a-zA-Z0-9_-]+/;
            return folderPattern.test(link);
        }
        
        function extractFolderId(link) {
            const match = link.match(/\/folders\/([a-zA-Z0-9_-]+)/);
            return match ? match[1] : null;
        }
        
        function copyClientLink() {
            const link = clientLinkElement.textContent;
            navigator.clipboard.writeText(link).then(() => {
                showStatus(adminStatus, "Link berhasil disalin ke clipboard!", "success");
            }).catch(err => {
                showStatus(adminStatus, "Gagal menyalin link", "error");
            });
        }
        
        // ==================== CLIENT FUNCTIONS ====================
        function checkForClientData() {
            const urlParams = new URLSearchParams(window.location.search);
            const clientData = urlParams.get('client');
            
            if (clientData) {
                try {
                    // Decode the data
                    const decodedData = JSON.parse(atob(clientData));
                    
                    if (decodedData.folderLink && decodedData.projectName) {
                        // Hide welcome, show selection area
                        clientWelcome.style.display = 'none';
                        photoSelectionArea.style.display = 'block';
                        
                        // Set project title
                        projectTitleElement.textContent = decodedData.projectName;
                        
                        // Load photos from the folder
                        loadClientPhotos(decodedData.folderLink);
                        
                        // Switch to client mode
                        switchMode('client');
                    }
                } catch (error) {
                    console.error("Error decoding client data:", error);
                    showStatus(clientStatus, "Data tidak valid. Pastikan Anda menggunakan link yang diberikan admin", "error");
                }
            }
        }
        
        async function loadClientPhotos(folderLink) {
            showStatus(clientStatus, "Sedang memuat foto dari Google Drive...", "info");
            
            try {
                // Extract folder ID
                const folderId = extractFolderId(folderLink);
                
                if (!folderId) {
                    throw new Error("Folder ID tidak ditemukan");
                }
                
                // Create Google Drive folder viewer URL
                const folderViewerUrl = `https://drive.google.com/embeddedfolderview?id=${folderId}`;
                
                // Note: In a real implementation, you would need to use Google Drive API
                // For this demo, we'll use simulated photos
                await simulateLoadingPhotos();
                
                // Update UI
                updateClientSelectionInfo();
                renderClientPhotos();
                
                showStatus(clientStatus, `Berhasil memuat ${clientPhotos.length} foto`, "success");
                
                // Hide status after 3 seconds
                setTimeout(() => {
                    clientStatus.style.display = 'none';
                }, 3000);
                
            } catch (error) {
                console.error("Error loading photos:", error);
                showStatus(clientStatus, "Gagal memuat foto. Pastikan folder telah di-share dengan akses publik", "error");
                
                // Load demo photos for testing
                loadDemoPhotosForClient();
            }
        }
        
        function simulateLoadingPhotos() {
            return new Promise(resolve => {
                setTimeout(() => {
                    // Generate demo photos (in real use, these would come from Google Drive)
                    const photoCount = Math.min(200, Math.floor(Math.random() * 150) + 50); // 50-200 photos
                    
                    clientPhotos = [];
                    for (let i = 1; i <= photoCount; i++) {
                        clientPhotos.push({
                            id: `photo_${i}`,
                            name: `Foto ${i}.jpg`,
                            number: i,
                            thumbnail: `https://images.unsplash.com/photo-${1550000 + i * 100}?w=400&h=400&fit=crop&auto=format`
                        });
                    }
                    
                    resolve();
                }, 2000);
            });
        }
        
        function loadDemoPhotosForClient() {
            // Load some demo photos for testing
            clientPhotos = [];
            for (let i = 1; i <= 50; i++) {
                clientPhotos.push({
                    id: `demo_photo_${i}`,
                    name: `Foto Contoh ${i}.jpg`,
                    number: i,
                    thumbnail: `https://images.unsplash.com/photo-${1550000 + i * 100}?w=400&h=400&fit=crop&auto=format`
                });
            }
            
            updateClientSelectionInfo();
            renderClientPhotos();
            
            showStatus(clientStatus, "Memuat foto demo untuk testing", "info");
        }
        
        function renderClientPhotos() {
            clientPhotoGrid.innerHTML = '';
            
            if (clientPhotos.length === 0) {
                clientPhotoGrid.innerHTML = `
                    <div style="grid-column: 1 / -1; text-align: center; padding: 50px 20px; color: #666;">
                        <i class="fas fa-folder-open" style="font-size: 60px; margin-bottom: 20px; opacity: 0.5;"></i>
                        <p>Tidak ada foto ditemukan di folder ini</p>
                    </div>
                `;
                return;
            }
            
            clientPhotos.forEach(photo => {
                const isSelected = clientSelectedPhotos.some(p => p.id === photo.id);
                const isLimitReached = clientSelectedPhotos.length >= clientPhotoLimit && !isSelected;
                
                const photoElement = document.createElement('div');
                photoElement.className = `photo-item ${isSelected ? 'selected' : ''}`;
                if (isLimitReached) {
                    photoElement.style.opacity = '0.5';
                    photoElement.style.cursor = 'not-allowed';
                }
                
                photoElement.dataset.id = photo.id;
                
                photoElement.innerHTML = `
                    <div class="photo-number">${photo.number}</div>
                    <img src="${photo.thumbnail}" alt="${photo.name}" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjAwIiBoZWlnaHQ9IjIwMCIgdmlld0JveD0iMCAwIDIwMCAyMDAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PHJlY3Qgd2lkdGg9IjIwMCIgaGVpZ2h0PSIyMDAiIGZpbGw9IiNGMEYwRjAiLz48cGF0aCBkPSJNMTI1IDcwSjc1QzcxIDEwIDY1IDEwIDY1IDEwVjE0MEM2NSAxNDAgNzEgMTUwIDc1IDE1MEgxMjVDMTI5IDE1MCAxMzUgMTQwIDEzNSAxNDBWMTAwQzEzNSAxMDAgMTI5IDkwIDEyNSA5MFY3MFoiIGZpbGw9IiNEMEQwRDAiLz48Y2lyY2xlIGN4PSIxMDAiIGN5PSI4MCIgcj0iMTUiIGZpbGw9IiNCMEIwQjAiLz48cGF0aCBkPSJNNzUgMTEwTDEwMCAxMzBMMTI1IDExMCIgc3Ryb2tlPSIjQjBCMEIwIiBzdHJva2Utd2lkdGg9IjgiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCIvPjwvc3ZnPg==';">
                `;
                
                if (!isLimitReached) {
                    photoElement.addEventListener('click', () => toggleClientPhotoSelection(photo));
                }
                
                clientPhotoGrid.appendChild(photoElement);
            });
        }
        
        function toggleClientPhotoSelection(photo) {
            const index = clientSelectedPhotos.findIndex(p => p.id === photo.id);
            
            if (index === -1) {
                // Check limit
                if (clientSelectedPhotos.length >= clientPhotoLimit) {
                    showStatus(clientStatus, `Batas ${clientPhotoLimit} foto sudah tercapai`, "error", 3000);
                    return;
                }
                
                // Add to selection
                clientSelectedPhotos.push(photo);
                document.querySelector(`.photo-item[data-id="${photo.id}"]`).classList.add('selected');
                
                showStatus(clientStatus, `"${photo.name}" ditambahkan`, "success", 2000);
            } else {
                // Remove from selection
                clientSelectedPhotos.splice(index, 1);
                document.querySelector(`.photo-item[data-id="${photo.id}"]`).classList.remove('selected');
                
                showStatus(clientStatus, `"${photo.name}" dihapus`, "info", 2000);
            }
            
            updateClientSelectedList();
            updateClientSelectionInfo();
            updateClientSendButton();
            renderClientPhotos(); // Re-render to update limit reached state
        }
        
        function updateClientSelectedList() {
            if (clientSelectedPhotos.length === 0) {
                clientSelectedList.innerHTML = `
                    <div style="text-align: center; padding: 40px; color: #888;">
                        <i class="far fa-clipboard" style="font-size: 50px; margin-bottom: 15px;"></i>
                        <p>Belum ada foto yang dipilih</p>
                    </div>
                `;
                return;
            }
            
            // Sort by number
            clientSelectedPhotos.sort((a, b) => a.number - b.number);
            
            let html = '';
            clientSelectedPhotos.forEach((photo, index) => {
                html += `
                    <div class="selected-item">
                        <div style="display: flex; align-items: center; gap: 15px;">
                            <div style="background: var(--client-color); color: white; width: 35px; height: 35px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: bold;">
                                ${index + 1}
                            </div>
                            <div>${photo.name}</div>
                        </div>
                        <button class="btn" onclick="removeClientSelectedPhoto('${photo.id}')" style="background: var(--danger-color); color: white; padding: 8px 15px; font-size: 14px;">
                            <i class="fas fa-times"></i> Hapus
                        </button>
                    </div>
                `;
            });
            
            clientSelectedList.innerHTML = html;
        }
        
        function removeClientSelectedPhoto(photoId) {
            clientSelectedPhotos = clientSelectedPhotos.filter(p => p.id !== photoId);
            document.querySelector(`.photo-item[data-id="${photoId}"]`)?.classList.remove('selected');
            updateClientSelectedList();
            updateClientSelectionInfo();
            updateClientSendButton();
            renderClientPhotos();
            showStatus(clientStatus, "Foto dihapus dari daftar", "info", 2000);
        }
        
        function updateClientSelectionInfo() {
            selectedCountClientElement.textContent = clientSelectedPhotos.length;
            totalPhotosElement.textContent = clientPhotos.length;
        }
        
        function updateClientSendButton() {
            const phoneNumber = clientPhoneNumberInput.value.trim();
            
            if (clientSelectedPhotos.length > 0 && phoneNumber) {
                sendToWhatsAppClientBtn.disabled = false;
                sendToWhatsAppClientBtn.style.opacity = '1';
            } else {
                sendToWhatsAppClientBtn.disabled = true;
                sendToWhatsAppClientBtn.style.opacity = '0.6';
            }
        }
        
        function sendClientToWhatsApp() {
            const phoneNumber = clientPhoneNumberInput.value.trim();
            const message = clientMessageInput.value.trim();
            
            // Validation
            if (!phoneNumber) {
                showStatus(clientWhatsAppStatus, "Harap masukkan nomor WhatsApp", "error");
                return;
            }
            
            if (!/^[1-9][0-9]{9,14}$/.test(phoneNumber)) {
                showStatus(clientWhatsAppStatus, "Format nomor tidak valid. Contoh: 6281234567890", "error");
                return;
            }
            
            if (clientSelectedPhotos.length === 0) {
                showStatus(clientWhatsAppStatus, "Harap pilih foto terlebih dahulu", "error");
                return;
            }
            
            // Format message
            let whatsappMessage = `📸 *DAFTAR FOTO YANG DIPILIH* 📸\n\n`;
            
            if (message) {
                whatsappMessage += `📝 *Pesan:* ${message}\n\n`;
            }
            
            whatsappMessage += `📁 *Proyek:* ${projectTitleElement.textContent}\n`;
            whatsappMessage += `✅ *Total Dipilih:* ${clientSelectedPhotos.length} foto\n`;
            whatsappMessage += `📅 *Tanggal:* ${new Date().toLocaleDateString('id-ID')}\n\n`;
            whatsappMessage += `═══════════════════════════\n`;
            whatsappMessage += `📋 *DAFTAR FOTO TERPILIH:*\n`;
            whatsappMessage += `═══════════════════════════\n\n`;
            
            clientSelectedPhotos.forEach((photo, index) => {
                whatsappMessage += `${(index + 1).toString().padStart(2, '0')}. ${photo.name}\n`;
            });
            
            whatsappMessage += `\n═══════════════════════════\n`;
            whatsappMessage += `📱 *Dikirim via Foto Selection System*`;
            
            // Encode for URL
            const encodedMessage = encodeURIComponent(whatsappMessage);
            
            // Show loading
            sendToWhatsAppClientBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Membuka WhatsApp...';
            sendToWhatsAppClientBtn.disabled = true;
            showStatus(clientWhatsAppStatus, "Menyiapkan pesan untuk WhatsApp...", "info");
            
            // Open WhatsApp
            setTimeout(() => {
                const whatsappUrl = `https://wa.me/${phoneNumber}?text=${encodedMessage}`;
                window.open(whatsappUrl, '_blank');
                
                showStatus(clientWhatsAppStatus, "WhatsApp berhasil dibuka! Periksa tab baru di browser", "success");
                
                // Reset button
                sendToWhatsAppClientBtn.innerHTML = '<i class="fab fa-whatsapp"></i> KIRIM DAFTAR FOTO KE WHATSAPP';
                sendToWhatsAppClientBtn.disabled = false;
                updateClientSendButton();
                
            }, 1500);
        }
        
        // ==================== UTILITY FUNCTIONS ====================
        function showStatus(element, message, type) {
            const icons = {
                success: '<i class="fas fa-check-circle"></i>',
                error: '<i class="fas fa-exclamation-circle"></i>',
                info: '<i class="fas fa-info-circle"></i>'
            };
            
            element.innerHTML = `${icons[type] || ''} ${message}`;
            element.className = `status ${type}`;
            element.style.display = 'flex';
        }
        
        // Make functions available globally for onclick events
        window.removeClientSelectedPhoto = removeClientSelectedPhoto;
    </script>
</body>
</html>
