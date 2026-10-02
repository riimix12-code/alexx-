
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Puk-Puk & Comfort Station Buat Kia Jamet 🌸</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Fredoka & Quicksand -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@300;400;500;600;700&family=Quicksand:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- QRCode.js CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <!-- Canvas Confetti CDN -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <!-- FontAwesome Icons CDN -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        lavender: { 50: '#F5F0FF', 100: '#EAE0FF', 200: '#E6E6FA', 300: '#CBB8FF', 500: '#9E75FF', 700: '#7545E0' },
                        peach: { 50: '#FFF5F2', 100: '#FFE5DD', 200: '#FFDAB9', 300: '#FFC4B8', 500: '#FF8A75' },
                        rosesoft: { 50: '#FFF0F3', 100: '#FFD1DC', 200: '#FFB6C1', 300: '#FFA0B0', 500: '#FF6B8B' },
                        mintsoft: '#E2ECE9',
                        creamsoft: '#FFF9F2'
                    },
                    fontFamily: {
                        fredoka: ['Fredoka', 'sans-serif'],
                        quicksand: ['Quicksand', 'sans-serif']
                    }
                }
            }
        }
    </script>

    <style>
        body {
            font-family: 'Fredoka', 'Quicksand', sans-serif;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
            background: linear-gradient(135deg, #F5F0FF 0%, #FFDAB9 50%, #FFB6C1 100%);
            background-attachment: fixed;
        }

        /* 3D Flip Card Styles */
        .perspective-1000 {
            perspective: 1000px;
        }
        .transform-style-3d {
            transform-style: preserve-3d;
            transition: transform 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        .backface-hidden {
            backface-visibility: hidden;
            -webkit-backface-visibility: hidden;
        }
        .rotate-y-180 {
            transform: rotateY(180deg);
        }

        /* 3D Foldable Envelope Styles */
        .envelope-container {
            perspective: 1000px;
        }
        .envelope-box {
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.5s ease;
        }
        .envelope-flap {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 50%;
            background: #FFB6C1;
            clip-path: polygon(0 0, 50% 100%, 100% 0);
            transform-origin: top center;
            transition: transform 0.6s ease-in-out, z-index 0.3s;
            z-index: 3;
            border-top: 2px solid #FFA0B0;
        }
        .envelope-box.open .envelope-flap {
            transform: rotateX(180deg);
            z-index: 1;
        }
        .envelope-letter {
            transition: transform 0.6s ease-in-out 0.3s, opacity 0.4s ease;
            opacity: 0.9;
        }
        .envelope-box.open .envelope-letter {
            transform: translateY(-40px) scale(1.02);
            opacity: 1;
            z-index: 4;
        }

        /* Soft Floating & Pulse Animations */
        @keyframes floatSlow {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-8px) rotate(2deg); }
        }
        .animate-float {
            animation: floatSlow 4s ease-in-out infinite;
        }

        @keyframes pulseGlow {
            0%, 100% { box-shadow: 0 0 20px rgba(230, 230, 250, 0.7), 0 0 35px rgba(255, 182, 193, 0.5); }
            50% { box-shadow: 0 0 35px rgba(230, 230, 250, 0.95), 0 0 55px rgba(255, 182, 193, 0.8); }
        }
        .glow-effect {
            animation: pulseGlow 3s infinite;
        }

        /* Glassmorphism background effect */
        .glass-card {
            background: rgba(255, 255, 255, 0.82);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.9);
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #FFF5F5;
        }
        ::-webkit-scrollbar-thumb {
            background: #CBB8FF;
            border-radius: 10px;
        }
    </style>
</head>
<body class="min-h-screen text-slate-700 relative overflow-x-hidden pb-12">

    <!-- Interactive Background Canvas (Sparkles, Clouds, Floating Hearts) -->
    <canvas id="bg-canvas" class="fixed inset-0 pointer-events-none z-0"></canvas>

    <!-- Main Application Container -->
    <div class="max-w-4xl mx-auto px-4 py-6 relative z-10 space-y-8">

        <!-- Top Header & Audio Control Bar -->
        <header class="glass-card rounded-3xl p-4 md:p-6 shadow-xl shadow-pink-100/50 flex flex-col sm:flex-row items-center justify-between gap-4 transition-all">
            <div class="flex items-center space-x-3 text-center sm:text-left">
                <div class="w-12 h-12 rounded-2xl bg-gradient-to-tr from-peach-300 to-lavender-300 flex items-center justify-center text-2xl shadow-inner animate-bounce">
                    🌸
                </div>
                <div>
                    <h1 id="header-title" class="text-xl md:text-2xl font-bold text-slate-800 tracking-wide">
                        Comfort Zone Kia Jamet 💕
                    </h1>
                    <p class="text-xs md:text-sm text-pink-500 font-medium">
                        Sakit perut dapet go away! Mode Puk-Puk Aktif ✨
                    </p>
                </div>
            </div>

            <!-- Top Controls (Ambient Audio Synth & Quick Actions) -->
            <div class="flex items-center space-x-2 bg-lavender-50/90 p-2 rounded-2xl border border-lavender-100 shadow-sm">
                <!-- Audio Ambient Generator Toggle -->
                <button id="btn-ambient-toggle" onclick="toggleAmbientAudio()" class="px-3 py-2 rounded-xl bg-white shadow-sm hover:bg-lavender-100 text-slate-700 text-xs font-semibold flex items-center space-x-1.5 transition">
                    <i id="ambient-icon" class="fa-solid fa-music text-lavender-500"></i>
                    <span id="ambient-label">Ambient Sound</span>
                </button>
                <input type="range" id="ambient-volume" min="0" max="1" step="0.05" value="0.25" oninput="updateVolume(this.value)" class="w-16 accent-lavender-500 cursor-pointer" title="Volume Suasana">

                <button onclick="openSettingsModal()" class="p-2.5 rounded-xl bg-white shadow-sm hover:bg-peach-100 text-slate-600 transition" title="Pengaturan & Personalize">
                    <i class="fa-solid fa-gear text-peach-500"></i>
                </button>

                <button onclick="scrollToQRHub()" class="p-2.5 rounded-xl bg-white shadow-sm hover:bg-rosesoft-100 text-slate-600 transition" title="Bagikan / Buka via QR Code">
                    <i class="fa-solid fa-qrcode text-pink-500"></i>
                </button>
            </div>
        </header>

        <!-- Hero Feature: Virtual "Puk-Puk Perut" Button -->
        <section class="glass-card rounded-3xl p-6 md:p-10 text-center shadow-xl shadow-rose-100/50 relative overflow-hidden">
            <div class="absolute -top-6 -right-6 w-28 h-28 bg-peach-200/50 rounded-full blur-xl"></div>
            <div class="absolute -bottom-6 -left-6 w-28 h-28 bg-lavender-200/50 rounded-full blur-xl"></div>

            <div class="inline-block px-4 py-1.5 bg-peach-100 text-peach-500 rounded-full text-xs font-bold tracking-wider mb-4 border border-peach-200">
                ✨ PENAWAR SAKIT PERUT SPESIAL ✨
            </div>

            <h2 class="text-2xl md:text-3xl font-extrabold text-slate-800 mb-2">
                Puk-Puk Perut <span id="display-nickname-hero" class="text-pink-500">Kia Jamet</span> 💆‍♀️
            </h2>
            <p class="text-xs md:text-sm text-slate-500 max-w-md mx-auto mb-6">
                Tekan tombol di bawah selagi dengerin suara lembutnya. Setiap pencetan kasih rasa hangat & perhatian melimpah!
            </p>

            <!-- Counter Badge -->
            <div class="inline-flex items-center space-x-2 bg-gradient-to-r from-lavender-100 via-peach-100 to-rosesoft-100 px-5 py-2.5 rounded-2xl shadow-inner mb-8 border border-white">
                <span class="text-xs md:text-sm font-semibold text-slate-600">Total Puk-Puk Sayang:</span>
                <span id="puk-counter" class="text-2xl md:text-3xl font-black text-lavender-700 animate-pulse">0</span>
                <span class="text-lg">💖</span>
            </div>

            <!-- Big Puk-Puk Button -->
            <div class="relative inline-block my-2">
                <button id="puk-btn" onclick="triggerPukPuk(event)" class="group relative w-48 h-48 md:w-56 md:h-56 rounded-full bg-gradient-to-tr from-peach-300 via-rosesoft-200 to-lavender-300 p-2 shadow-2xl glow-effect transform active:scale-95 transition-transform duration-150 cursor-pointer focus:outline-none">
                    <div class="w-full h-full rounded-full bg-white/95 backdrop-blur-sm flex flex-col items-center justify-center p-4 border-4 border-white group-hover:bg-white transition">
                        <span class="text-5xl md:text-6xl mb-2 transform group-hover:scale-110 group-active:rotate-12 transition duration-200">
                            🧸
                        </span>
                        <span class="text-base md:text-lg font-bold text-slate-700 group-hover:text-pink-500">
                            Puk-Puk Perut Kia
                        </span>
                        <span class="text-xs text-pink-400 font-medium">
                            (Klik disini manis)
                        </span>
                    </div>
                </button>
            </div>

            <!-- Floating Feedback Message -->
            <p id="puk-quote" class="text-xs md:text-sm font-semibold text-lavender-700 mt-4 h-6 transition-all">
                "Klik tombolnya untuk dapetin usapan hangat virtual..."
            </p>
        </section>

        <!-- Interactive Cramp Check Question (Dodging Button Feature) -->
        <section class="bg-gradient-to-r from-peach-50/90 via-lavender-50/90 to-rosesoft-50/90 rounded-3xl p-6 md:p-8 shadow-lg border border-white relative overflow-hidden">
            <div class="text-center max-w-lg mx-auto space-y-4">
                <div class="text-3xl">🩺</div>
                <h3 class="text-xl md:text-2xl font-bold text-slate-800">
                    Masih Sakit Nggak Perutnya, <span id="display-nickname-check" class="text-pink-500">Kia</span>? 🥺
                </h3>
                <p class="text-xs md:text-sm text-slate-500">
                    Jawab sejujurnya yaa! Mas siap siaga bantu apa aja biarpun jauh.
                </p>

                <!-- Interactive Choice Buttons -->
                <div id="cramp-btn-container" class="relative min-h-[140px] flex flex-col sm:flex-row items-center justify-center gap-4 pt-2">
                    <!-- Dodging Button -->
                    <button id="btn-dodge" onmouseenter="dodgeButton(event)" onclick="caughtDodgeButton()" class="absolute transition-all duration-200 ease-out px-6 py-3 bg-rosesoft-200 text-rose-700 font-bold rounded-2xl shadow-md border border-rosesoft-300 text-xs md:text-sm hover:bg-rosesoft-300 z-20">
                        Masih sakit banget!! 😭
                    </button>

                    <!-- Relief Button -->
                    <button onclick="triggerReliefShower()" class="px-6 py-3 bg-mintsoft text-teal-700 font-bold rounded-2xl shadow-md border border-teal-200 text-xs md:text-sm hover:bg-teal-100 transition transform hover:scale-105 z-10 sm:translate-x-32 mt-16 sm:mt-0">
                        Udah lumayan mendingan 🥰
                    </button>
                </div>
            </div>
        </section>

        <!-- Interactive 3D Foldable Envelope & Comforting Letter -->
        <section class="glass-card rounded-3xl p-6 md:p-8 shadow-xl shadow-purple-100/50">
            <div class="text-center mb-6">
                <span class="text-xs font-bold tracking-widest text-lavender-700 uppercase bg-lavender-100 px-3 py-1 rounded-full">
                    Surat Kasih Sayang 💌
                </span>
                <h3 class="text-2xl font-bold text-slate-800 mt-2">
                    Buka Surat Khusus Untukmu
                </h3>
            </div>

            <div class="envelope-container flex justify-center py-4">
                <div id="interactive-envelope" class="envelope-box w-72 md:w-96 bg-peach-100 rounded-2xl shadow-lg border-2 border-peach-200 cursor-pointer overflow-hidden p-4 relative" onclick="toggleEnvelope()">
                    <!-- Top Flap -->
                    <div class="envelope-flap"></div>

                    <!-- Envelope Front Graphic -->
                    <div class="h-36 flex flex-col items-center justify-center text-center relative z-10 pt-4">
                        <span class="text-3xl mb-1">💌</span>
                        <span class="text-xs font-bold text-slate-600">Klik Untuk Membuka Surat</span>
                        <span id="display-nickname-envelope" class="text-xs font-bold text-pink-500">Untuk Kia Jamet</span>
                    </div>

                    <!-- Letter Content -->
                    <div id="envelope-letter" class="envelope-letter bg-white p-5 rounded-xl shadow-md border border-pink-100 space-y-3 mt-2 text-left">
                        <h4 id="letter-title-display" class="font-bold text-pink-500 text-base border-b border-pink-100 pb-2">
                            Surat Puk-Puk Buat Kia Jamet ✨
                        </h4>
                        <p id="letter-body-display" class="text-xs md:text-sm text-slate-600 leading-relaxed whitespace-pre-line">
                            Halo Kia sayang... Mas tau perutnya lagi sakit banget & gak enak badan ya? 🥺 
                            Jangan cemberut terus ya, dapet itu wajar banget kok! Mas di sini siap puk-puk perut Kia, siap beliin jajan kesukaan, dan siap dengerin kening Kia berkerut. 
                            Rebahan yang nyaman ya manis, minum air hangat, pakai kompresnya. Love you so much! 💕
                        </p>
                        <div class="text-right text-xs font-bold text-slate-400 pt-2">
                            Dari Mas yang Selalu Sayang 🥰
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Period Comfort Toolkit Checklist -->
        <section class="glass-card rounded-3xl p-6 md:p-8 shadow-xl shadow-pink-100/50">
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                <div>
                    <h3 class="text-xl md:text-2xl font-bold text-slate-800 flex items-center gap-2">
                        <span>🍵</span> Checklist Pertolongan Cramp
                    </h3>
                    <p class="text-xs md:text-sm text-slate-500">
                        Ceklis langkah santai ini biar perut makin adem & nyaman ya!
                    </p>
                </div>
                <!-- Progress bar -->
                <div class="w-full md:w-48 bg-slate-100 h-4 rounded-full overflow-hidden p-0.5 border border-slate-200">
                    <div id="toolkit-progress" class="bg-gradient-to-r from-peach-300 to-lavender-300 h-full rounded-full transition-all duration-500 w-0"></div>
                </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4" id="toolkit-list">
                <div onclick="toggleCheckItem(this)" class="toolkit-card cursor-pointer bg-slate-50 hover:bg-peach-50/60 border border-slate-200/80 p-4 rounded-2xl flex items-start space-x-3 transition-all">
                    <div class="check-box w-6 h-6 rounded-lg border-2 border-slate-300 flex items-center justify-center mt-0.5 transition-colors">
                        <i class="fa-solid fa-check text-xs text-white opacity-0 transition-opacity"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-slate-700 text-sm">🍵 Teh Hangat / Chamomile</h4>
                        <p class="text-xs text-slate-500 mt-0.5">Membantu melemaskan otot perut yang tegang.</p>
                    </div>
                </div>

                <div onclick="toggleCheckItem(this)" class="toolkit-card cursor-pointer bg-slate-50 hover:bg-peach-50/60 border border-slate-200/80 p-4 rounded-2xl flex items-start space-x-3 transition-all">
                    <div class="check-box w-6 h-6 rounded-lg border-2 border-slate-300 flex items-center justify-center mt-0.5 transition-colors">
                        <i class="fa-solid fa-check text-xs text-white opacity-0 transition-opacity"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-slate-700 text-sm">🌡️ Warm Compress / Kompres Perut</h4>
                        <p class="text-xs text-slate-500 mt-0.5">Tempelkan kompres hangat di bagian perut bawah.</p>
                    </div>
                </div>

                <div onclick="toggleCheckItem(this)" class="toolkit-card cursor-pointer bg-slate-50 hover:bg-peach-50/60 border border-slate-200/80 p-4 rounded-2xl flex items-start space-x-3 transition-all">
                    <div class="check-box w-6 h-6 rounded-lg border-2 border-slate-300 flex items-center justify-center mt-0.5 transition-colors">
                        <i class="fa-solid fa-check text-xs text-white opacity-0 transition-opacity"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-slate-700 text-sm">🍫 Chocolate Supply / Cokelat Kesukaan</h4>
                        <p class="text-xs text-slate-500 mt-0.5">Menaikkan mood & hormon gembira (endorfin).</p>
                    </div>
                </div>

                <div onclick="toggleCheckItem(this)" class="toolkit-card cursor-pointer bg-slate-50 hover:bg-peach-50/60 border border-slate-200/80 p-4 rounded-2xl flex items-start space-x-3 transition-all">
                    <div class="check-box w-6 h-6 rounded-lg border-2 border-slate-300 flex items-center justify-center mt-0.5 transition-colors">
                        <i class="fa-solid fa-check text-xs text-white opacity-0 transition-opacity"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-slate-700 text-sm">🛌 Rest Mode / Rebahan Maksimal</h4>
                        <p class="text-xs text-slate-500 mt-0.5">Posisi meringkuk melemaskan tekanan perut.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Mood Booster Flip Cards Grid (6 Interactive Voucher Cards) -->
        <section class="glass-card rounded-3xl p-6 md:p-8 shadow-xl shadow-rose-100/50">
            <div class="text-center mb-6">
                <span class="text-xs font-bold tracking-widest text-pink-500 uppercase bg-pink-100 px-3 py-1 rounded-full">
                    Kupon & Janji Spesial 🎁
                </span>
                <h3 class="text-2xl font-bold text-slate-800 mt-2">
                    Mood Booster Flip Cards
                </h3>
                <p class="text-xs text-slate-500 mt-1">Tap kartu di bawah untuk membuka janji manis & hadiah khusus!</p>
            </div>

            <div id="flip-cards-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
                <!-- Dynamically populated via JS -->
            </div>
        </section>

        <!-- Cozy Video / Media Player Section -->
        <section class="glass-card rounded-3xl p-6 md:p-8 shadow-xl shadow-indigo-100/50">
            <div class="text-center mb-6">
                <span class="text-xs font-bold tracking-widest text-lavender-700 uppercase bg-lavender-100 px-3 py-1 rounded-full">
                    Relaxing Atmosphere 🎧
                </span>
                <h3 class="text-2xl font-bold text-slate-800 mt-2">
                    Cozy Lofi & Music Visualizer
                </h3>
                <p class="text-xs text-slate-500 mt-1">Putar video/musik santai sambil rebahan & puk-puk perut</p>
            </div>

            <div class="aspect-video w-full max-w-2xl mx-auto rounded-2xl overflow-hidden shadow-lg border-2 border-white bg-slate-900 relative">
                <iframe id="lofi-video" class="w-full h-full" src="https://www.youtube-nocookie.com/embed/jfKfPfyJRdk?autoplay=0&controls=1&rel=0" title="Cozy Lofi Beats" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
            </div>
        </section>

        <!-- QR Code Share Generator Hub ("Bagikan / Buka via QR Code") -->
        <section id="qr-hub-section" class="glass-card rounded-3xl p-6 md:p-8 shadow-xl shadow-teal-100/50">
            <div class="text-center mb-6">
                <span class="text-xs font-bold tracking-widest text-teal-600 uppercase bg-teal-100 px-3 py-1 rounded-full">
                    Akses Luar AI & Mobile 📱
                </span>
                <h3 class="text-2xl font-bold text-slate-800 mt-2">
                    QR Code Share Hub
                </h3>
                <p class="text-xs text-slate-500 mt-1">
                    Scan QR code di bawah pakai HP atau bagikan link website ini agar bisa dibuka di browser luar kapan saja!
                </p>
            </div>

            <div class="flex flex-col md:flex-row items-center justify-center gap-8 bg-white/70 p-6 rounded-2xl border border-teal-100">
                <!-- QR Canvas Display -->
                <div class="flex flex-col items-center space-y-3">
                    <div id="qr-hub-canvas-container" class="p-4 bg-white rounded-2xl shadow-md border border-lavender-200"></div>
                    <button onclick="downloadQRCode()" class="px-4 py-2 bg-lavender-100 hover:bg-lavender-200 text-lavender-700 text-xs font-bold rounded-xl transition flex items-center gap-1.5">
                        <i class="fa-solid fa-download"></i>
                        <span>Download Gambar QR</span>
                    </button>
                </div>

                <!-- Link Customizer & Copy -->
                <div class="flex-1 w-full max-w-md space-y-4">
                    <div>
                        <label class="block text-xs font-bold text-slate-700 mb-1">Target Website Link / URL:</label>
                        <div class="flex gap-2">
                            <input type="text" id="input-qr-url" class="flex-1 px-3 py-2 text-xs border rounded-xl focus:ring-2 focus:ring-teal-300 outline-none bg-white" placeholder="https://..." readonly>
                            <button onclick="regenerateQRCode()" class="px-3 py-2 bg-teal-500 text-white rounded-xl text-xs font-bold hover:bg-teal-600 transition">
                                Refresh
                            </button>
                        </div>
                    </div>

                    <div class="space-y-2">
                        <button onclick="copyWebsiteLink()" class="w-full py-3 bg-gradient-to-r from-peach-300 via-rosesoft-200 to-lavender-300 text-slate-800 font-bold rounded-xl text-xs shadow-md hover:opacity-90 transition flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-copy text-pink-500"></i>
                            <span>Salin Link Website Sekarang</span>
                        </button>
                    </div>

                    <p id="qr-toast-msg" class="text-xs font-bold text-teal-600 text-center h-4"></p>
                </div>
            </div>
        </section>

        <!-- Footer -->
        <footer class="text-center text-xs text-slate-400 py-4">
            Dibuat dengan segenap rasa sayang khusus buat <span id="display-nickname-footer" class="font-bold text-pink-400">Kia Jamet</span> 🌸 • Semangat dapetnya!
        </footer>
    </div>

    <!-- MODAL 1: Personalization / Settings Modal -->
    <div id="modal-settings" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-3xl p-6 md:p-8 max-w-md w-full shadow-2xl space-y-4 border border-slate-100 relative max-h-[90vh] overflow-y-auto">
            <button onclick="closeSettingsModal()" class="absolute top-4 right-4 text-slate-400 hover:text-slate-600 text-xl font-bold">✕</button>
            
            <h3 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                ⚙️ Personalize Untuk Girlfriend
            </h3>
            <p class="text-xs text-slate-500">Ubah nama panggilan, isi surat, dan janji kupon sesuai keinginanmu.</p>

            <div class="space-y-3 text-xs md:text-sm">
                <div>
                    <label class="block font-bold text-slate-700 mb-1">Nama Panggilan / Nickname:</label>
                    <input type="text" id="input-nickname" class="w-full px-3 py-2 border rounded-xl focus:ring-2 focus:ring-lavender-300 outline-none">
                </div>

                <div>
                    <label class="block font-bold text-slate-700 mb-1">Judul Surat:</label>
                    <input type="text" id="input-letter-title" class="w-full px-3 py-2 border rounded-xl focus:ring-2 focus:ring-lavender-300 outline-none">
                </div>

                <div>
                    <label class="block font-bold text-slate-700 mb-1">Isi Surat Kasih Sayang:</label>
                    <textarea id="input-letter-body" rows="4" class="w-full px-3 py-2 border rounded-xl focus:ring-2 focus:ring-lavender-300 outline-none"></textarea>
                </div>

                <div class="pt-2 border-t">
                    <label class="block font-bold text-slate-700 mb-2">Edit 6 Kupon Mood Booster:</label>
                    <div id="promises-inputs-container" class="space-y-2">
                        <!-- Dynamic inputs for promises -->
                    </div>
                </div>
            </div>

            <div class="flex space-x-2 pt-2">
                <button onclick="saveSettings()" class="flex-1 py-3 bg-gradient-to-r from-peach-300 to-lavender-300 text-slate-800 font-bold rounded-xl shadow-md hover:opacity-90 transition">
                    Simpan Perubahan 💾
                </button>
                <button onclick="resetSettings()" class="px-4 py-3 bg-slate-100 text-slate-600 font-bold rounded-xl hover:bg-slate-200 transition" title="Reset Default">
                    Reset
                </button>
            </div>
        </div>
    </div>

    <!-- MODAL 2: Food Treat Voucher Modal (Triggered by dodging button) -->
    <div id="modal-voucher" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-3xl p-6 md:p-8 max-w-sm w-full text-center shadow-2xl space-y-4 border border-rosesoft-200 relative animate-bounce-short">
            <div class="w-16 h-16 bg-peach-100 rounded-full flex items-center justify-center mx-auto text-3xl">
                🍧
            </div>
            <h3 class="text-xl font-bold text-slate-800">
                Spesial Voucher Jajan Kia! 🛵
            </h3>
            <p class="text-xs text-slate-600">
                Duhh kasihan banget perutnya masih sakit... Bebas pilih makanan / minuman kesukaan Kia sekarang, langsung Mas orderin!
            </p>

            <div class="grid grid-cols-2 gap-2 text-xs font-semibold">
                <button onclick="selectTreat('Boba & Milk Tea 🧋')" class="p-3 bg-peach-50 hover:bg-peach-100 rounded-xl border border-peach-200 text-slate-700">
                    🧋 Boba / Milk Tea
                </button>
                <button onclick="selectTreat('Seblak / Food Pedes Warm 🍜')" class="p-3 bg-rosesoft-100 hover:bg-rosesoft-200 rounded-xl border border-rosesoft-300 text-slate-700">
                    🍜 Seblak Warm
                </button>
                <button onclick="selectTreat('Ice Cream Comfort 🍦')" class="p-3 bg-lavender-50 hover:bg-lavender-100 rounded-xl border border-lavender-200 text-slate-700">
                    🍦 Ice Cream Box
                </button>
                <button onclick="selectTreat('Cokelat Panas & Pastry 🥐')" class="p-3 bg-amber-50 hover:bg-amber-100 rounded-xl border border-amber-200 text-slate-700">
                    🥐 Cokelat Warm
                </button>
            </div>

            <button onclick="closeVoucherModal()" class="w-full py-2.5 bg-slate-100 text-slate-600 font-bold text-xs rounded-xl hover:bg-slate-200 transition">
                Tutup dulu, mau di-puk-puk aja 💆‍♀️
            </button>
        </div>
    </div>

    <script>
        // Default Configuration & LocalStorage Keys
        const STORAGE_KEY = 'kia_comfort_app_v2';

        let defaultState = {
            nickname: "Kia Jamet",
            letterTitle: "Surat Puk-Puk Buat Kia Jamet ✨",
            letterBody: "Halo Kia sayang... Mas tau perutnya lagi sakit banget & gak enak badan ya? 🥺\nJangan cemberut terus ya, dapet itu wajar banget kok!\n\nMas di sini siap puk-puk perut Kia, siap beliin jajan kesukaan, dan siap dengerin kening Kia berkerut. Rebahan yang nyaman ya manis, minum air hangat, pakai kompresnya. Love you so much! 💕",
            pukCount: 0,
            promises: [
                "Puk-puk tanpa batas 🖐️",
                "Free Food Treat / Bebas Pilih Makanan Enak 🍰🍕",
                "Setia Mendengarkan Curhat / No Judge Zone 🎧",
                "Pijit Bahu & Punggung 💆‍♀️",
                "Bebas Marah-Marah Day 👑",
                "Movie Night Sync & Ice Cream 🍦"
            ]
        };

        let appState = { ...defaultState };

        // Audio Context & Synth setup
        let audioCtx = null;
        let ambientOscs = [];
        let ambientGain = null;
        let isAmbientPlaying = false;

        // Puk Sound Quotes
        const pukQuotes = [
            "Usapan hangat dikirim... Perutnya adem yaaa 🌸",
            "Puk-puk sayang! Rasa sakitnya ditransfer ke angin~ 🍃",
            "Cup cup cup... Sayang Kia banyak-banyak! 💖",
            "Satu puk-puk hangat buat si cantik yang lagi cemberut 🧸",
            "Semoga sakitnya berkurang 1000% ya manis! ✨",
            "Mas di sini mendampingi Kia virtual! 🥰"
        ];

        window.addEventListener('DOMContentLoaded', () => {
            loadState();
            initCanvas();
            renderDynamicContent();
            renderFlipCards();
            initQRHub();
        });

        function loadState() {
            const saved = localStorage.getItem(STORAGE_KEY);
            if (saved) {
                try {
                    appState = { ...defaultState, ...JSON.parse(saved) };
                } catch (e) {
                    console.error("Failed to parse storage:", e);
                }
            }
        }

        function saveStateToStorage() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(appState));
        }

        function renderDynamicContent() {
            document.getElementById('display-nickname-hero').innerText = appState.nickname;
            document.getElementById('display-nickname-check').innerText = appState.nickname;
            document.getElementById('display-nickname-envelope').innerText = "Untuk " + appState.nickname;
            document.getElementById('display-nickname-footer').innerText = appState.nickname;
            document.getElementById('puk-counter').innerText = appState.pukCount;

            document.getElementById('letter-title-display').innerText = appState.letterTitle;
            document.getElementById('letter-body-display').innerText = appState.letterBody;

            document.getElementById('header-title').innerText = "Comfort Zone " + appState.nickname + " 💕";
        }

        function initAudio() {
            if (!audioCtx) {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                audioCtx = new AudioContext();
            }
        }

        function playPukSound() {
            initAudio();
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }

            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();

            osc.type = 'sine';
            osc.frequency.setValueAtTime(340, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(160, audioCtx.currentTime + 0.16);

            gain.gain.setValueAtTime(0.5, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.16);

            osc.connect(gain);
            gain.connect(audioCtx.destination);

            osc.start();
            osc.stop(audioCtx.currentTime + 0.16);
        }

        function toggleAmbientAudio() {
            initAudio();
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }

            if (isAmbientPlaying) {
                stopAmbientAudio();
            } else {
                startAmbientAudio();
            }
        }

        function startAmbientAudio() {
            const freqs = [174.61, 220.00, 261.63, 329.63]; // F Major 7 soft chord
            ambientOscs = [];
            
            ambientGain = audioCtx.createGain();
            const vol = parseFloat(document.getElementById('ambient-volume').value) || 0.25;
            ambientGain.gain.setValueAtTime(vol, audioCtx.currentTime);

            freqs.forEach(f => {
                const osc = audioCtx.createOscillator();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(f, audioCtx.currentTime);
                osc.detune.setValueAtTime((Math.random() - 0.5) * 8, audioCtx.currentTime);

                osc.connect(ambientGain);
                osc.start();
                ambientOscs.push(osc);
            });

            ambientGain.connect(audioCtx.destination);
            isAmbientPlaying = true;

            document.getElementById('ambient-label').innerText = "Playing ♪";
            document.getElementById('ambient-icon').className = "fa-solid fa-volume-high text-pink-500 animate-pulse";
        }

        function stopAmbientAudio() {
            if (ambientOscs.length > 0) {
                ambientOscs.forEach(osc => {
                    try { osc.stop(); } catch(e){}
                });
                ambientOscs = [];
            }
            isAmbientPlaying = false;
            document.getElementById('ambient-label').innerText = "Ambient Sound";
            document.getElementById('ambient-icon').className = "fa-solid fa-music text-lavender-500";
        }

        function updateVolume(val) {
            if (ambientGain && audioCtx) {
                ambientGain.gain.setValueAtTime(parseFloat(val), audioCtx.currentTime);
            }
        }

        function triggerPukPuk(event) {
            playPukSound();

            appState.pukCount++;
            document.getElementById('puk-counter').innerText = appState.pukCount;
            saveStateToStorage();

            const randomQuote = pukQuotes[Math.floor(Math.random() * pukQuotes.length)];
            const quoteEl = document.getElementById('puk-quote');
            quoteEl.innerText = `"${randomQuote}"`;
            quoteEl.classList.add('scale-105', 'text-pink-500');
            setTimeout(() => quoteEl.classList.remove('scale-105', 'text-pink-500'), 200);

            spawnFloatingElements(event.clientX || event.touches?.[0]?.clientX, event.clientY || event.touches?.[0]?.clientY);
        }

        function spawnFloatingElements(x, y) {
            if (!x || !y) {
                const rect = document.getElementById('puk-btn').getBoundingClientRect();
                x = rect.left + rect.width / 2;
                y = rect.top + rect.height / 2;
            }

            const icons = ['🖐️', '💖', '🌸', '🧸', '✨', '🍼'];
            for (let i = 0; i < 6; i++) {
                const el = document.createElement('div');
                el.innerText = icons[Math.floor(Math.random() * icons.length)];
                el.className = 'fixed pointer-events-none text-2xl z-50 transition-all duration-1000 ease-out';
                
                const offsetX = (Math.random() - 0.5) * 140;
                const offsetY = -Math.random() * 120 - 40;

                el.style.left = `${x}px`;
                el.style.top = `${y}px`;
                document.body.appendChild(el);

                requestAnimationFrame(() => {
                    el.style.transform = `translate(${offsetX}px, ${offsetY}px) scale(${1 + Math.random() * 0.5})`;
                    el.style.opacity = '0';
                });

                setTimeout(() => el.remove(), 1000);
            }
        }

        function toggleEnvelope() {
            const env = document.getElementById('interactive-envelope');
            env.classList.toggle('open');
        }

        let dodgeCount = 0;
        function dodgeButton(e) {
            dodgeCount++;
            const btn = document.getElementById('btn-dodge');
            const container = document.getElementById('cramp-btn-container');
            const containerRect = container.getBoundingClientRect();

            if (dodgeCount > 5) {
                caughtDodgeButton();
                return;
            }

            const newX = (Math.random() - 0.5) * (containerRect.width - 150);
            const newY = (Math.random() - 0.5) * 80;

            btn.style.transform = `translate(${newX}px, ${newY}px)`;
        }

        function caughtDodgeButton() {
            document.getElementById('modal-voucher').classList.remove('hidden');
            const btn = document.getElementById('btn-dodge');
            btn.style.transform = 'translate(0, 0)';
            dodgeCount = 0;
        }

        function selectTreat(treatName) {
            closeVoucherModal();
            triggerReliefShower();
            setTimeout(() => {
                showCustomToast(`Pesanan ${treatName} dicatat! Siap-siap dapet kiriman yaa 🛵💕`);
            }, 300);
        }

        function closeVoucherModal() {
            document.getElementById('modal-voucher').classList.add('hidden');
        }

        function toggleCheckItem(card) {
            const box = card.querySelector('.check-box');
            const icon = card.querySelector('.fa-check');

            card.classList.toggle('bg-peach-100/70');
            card.classList.toggle('border-peach-300');

            if (box.classList.contains('bg-pink-500')) {
                box.classList.remove('bg-pink-500', 'border-pink-500');
                icon.classList.add('opacity-0');
            } else {
                box.classList.add('bg-pink-500', 'border-pink-500');
                icon.classList.remove('opacity-0');
            }

            updateChecklistProgress();
        }

        function updateChecklistProgress() {
            const cards = document.querySelectorAll('.toolkit-card');
            let checked = 0;
            cards.forEach(c => {
                if (c.querySelector('.check-box').classList.contains('bg-pink-500')) {
                    checked++;
                }
            });

            const percent = (checked / cards.length) * 100;
            document.getElementById('toolkit-progress').style.width = `${percent}%`;

            if (checked === cards.length) {
                triggerReliefShower();
                showCustomToast("Yeay! Semua checklist kenyamanan udah selesai! Kia panutan 🌸");
            }
        }

        function renderFlipCards() {
            const grid = document.getElementById('flip-cards-grid');
            grid.innerHTML = '';

            const icons = ['🖐️', '🍰', '🎧', '💆‍♀️', '👑', '🍦'];

            appState.promises.forEach((promiseText, index) => {
                const cardWrapper = document.createElement('div');
                cardWrapper.className = "perspective-1000 h-36 cursor-pointer";
                
                cardWrapper.innerHTML = `
                    <div class="card-inner w-full h-full transform-style-3d relative rounded-2xl shadow-md border border-white" onclick="flipCard(this)">
                        <div class="absolute inset-0 bg-gradient-to-br from-peach-100 via-rosesoft-50 to-lavender-100 rounded-2xl p-4 flex flex-col items-center justify-center text-center backface-hidden">
                            <span class="text-3xl mb-1">${icons[index] || '🎁'}</span>
                            <span class="text-xs font-bold text-slate-600">Kupon Mood Booster #${index + 1}</span>
                            <span class="text-[10px] text-pink-400 font-medium">Klik untuk buka!</span>
                        </div>
                        <div class="absolute inset-0 bg-gradient-to-br from-lavender-200 to-peach-200 rounded-2xl p-4 flex flex-col items-center justify-center text-center backface-hidden rotate-y-180 border-2 border-white">
                            <span class="text-xs font-bold text-slate-800 leading-snug">${promiseText}</span>
                            <span class="text-[10px] text-teal-600 font-bold mt-2 bg-white/80 px-2 py-0.5 rounded-full">✨ Berlaku Selamanya</span>
                        </div>
                    </div>
                `;
                grid.appendChild(cardWrapper);
            });
        }

        function flipCard(cardInner) {
            playPukSound();
            cardInner.classList.toggle('rotate-y-180');
        }

        function openSettingsModal() {
            document.getElementById('input-nickname').value = appState.nickname;
            document.getElementById('input-letter-title').value = appState.letterTitle;
            document.getElementById('input-letter-body').value = appState.letterBody;

            const container = document.getElementById('promises-inputs-container');
            container.innerHTML = '';
            appState.promises.forEach((p, idx) => {
                const inp = document.createElement('input');
                inp.type = 'text';
                inp.value = p;
                inp.className = 'promise-input w-full px-3 py-1.5 border rounded-lg text-xs outline-none focus:ring-1 focus:ring-lavender-300';
                container.appendChild(inp);
            });

            document.getElementById('modal-settings').classList.remove('hidden');
        }

        function closeSettingsModal() {
            document.getElementById('modal-settings').classList.add('hidden');
        }

        function saveSettings() {
            appState.nickname = document.getElementById('input-nickname').value.trim() || defaultState.nickname;
            appState.letterTitle = document.getElementById('input-letter-title').value.trim() || defaultState.letterTitle;
            appState.letterBody = document.getElementById('input-letter-body').value.trim() || defaultState.letterBody;

            const promiseInputs = document.querySelectorAll('.promise-input');
            let updatedPromises = [];
            promiseInputs.forEach((inp, idx) => {
                updatedPromises.push(inp.value.trim() || defaultState.promises[idx]);
            });
            appState.promises = updatedPromises;

            saveStateToStorage();
            renderDynamicContent();
            renderFlipCards();
            closeSettingsModal();
            showCustomToast("Pengaturan berhasil disimpan! ✨");
        }

        function resetSettings() {
            appState = { ...defaultState, pukCount: appState.pukCount };
            saveStateToStorage();
            renderDynamicContent();
            renderFlipCards();
            closeSettingsModal();
            showCustomToast("Pengaturan telah direset ke awal!");
        }

        let qrcodeObj = null;

        function initQRHub() {
            const currentUrl = window.location.href;
            document.getElementById('input-qr-url').value = currentUrl;
            generateQR(currentUrl);
        }

        function generateQR(url) {
            const container = document.getElementById('qr-hub-canvas-container');
            container.innerHTML = '';
            qrcodeObj = new QRCode(container, {
                text: url,
                width: 150,
                height: 150,
                colorDark: "#9E75FF",
                colorLight: "#FFFFFF",
                correctLevel: QRCode.CorrectLevel.H
            });
        }

        function regenerateQRCode() {
            const url = document.getElementById('input-qr-url').value.trim() || window.location.href;
            generateQR(url);
            showCustomToast("QR Code diperbarui!");
        }

        function downloadQRCode() {
            const container = document.getElementById('qr-hub-canvas-container');
            const img = container.querySelector('img');
            const canvasEl = container.querySelector('canvas');

            let src = '';
            if (img && img.src) {
                src = img.src;
            } else if (canvasEl) {
                src = canvasEl.toDataURL("image/png");
            }

            if (src) {
                const a = document.createElement('a');
                a.href = src;
                a.download = `ComfortZone_KiaJamet_QR.png`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                showCustomToast("Gambar QR Code berhasil di-download! 📥");
            } else {
                showCustomToast("Gagal mengambil gambar QR Code.");
            }
        }

        function copyWebsiteLink() {
            const tempInput = document.createElement('input');
            tempInput.value = window.location.href;
            document.body.appendChild(tempInput);
            tempInput.select();
            document.execCommand('copy');
            document.body.removeChild(tempInput);

            document.getElementById('qr-toast-msg').innerText = "Link berhasil disalin ke clipboard! 📋";
            setTimeout(() => {
                document.getElementById('qr-toast-msg').innerText = "";
            }, 3000);
        }

        function scrollToQRHub() {
            document.getElementById('qr-hub-section').scrollIntoView({ behavior: 'smooth' });
        }

        function showCustomToast(message) {
            const toast = document.createElement('div');
            toast.className = 'fixed bottom-6 left-1/2 -translate-x-1/2 bg-slate-800 text-white text-xs font-semibold px-6 py-3 rounded-2xl shadow-xl z-50 animate-bounce';
            toast.innerText = message;
            document.body.appendChild(toast);
            setTimeout(() => toast.remove(), 2600);
        }

        let canvas, ctx, particles = [];

        function initCanvas() {
            canvas = document.getElementById('bg-canvas');
            ctx = canvas.getContext('2d');
            resizeCanvas();

            window.addEventListener('resize', resizeCanvas);

            particles = [];
            for (let i = 0; i < 28; i++) {
                particles.push(createParticle());
            }

            requestAnimationFrame(animateCanvas);
        }

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }

        function createParticle(isCelebration = false) {
            return {
                x: Math.random() * canvas.width,
                y: isCelebration ? canvas.height + 20 : Math.random() * canvas.height,
                size: Math.random() * 14 + 10,
                speedY: isCelebration ? -(Math.random() * 3 + 2) : -(Math.random() * 0.6 + 0.2),
                speedX: (Math.random() - 0.5) * 0.5,
                opacity: Math.random() * 0.6 + 0.3,
                symbol: isCelebration ? '💖' : (Math.random() > 0.5 ? '🌸' : (Math.random() > 0.5 ? '✨' : '☁️'))
            };
        }

        function animateCanvas() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            particles.forEach((p, idx) => {
                p.y += p.speedY;
                p.x += p.speedX;

                ctx.globalAlpha = p.opacity;
                ctx.font = `${p.size}px sans-serif`;
                ctx.fillText(p.symbol, p.x, p.y);

                if (p.y < -30) {
                    particles[idx] = createParticle();
                    particles[idx].y = canvas.height + 20;
                }
            });

            requestAnimationFrame(animateCanvas);
        }

        function triggerReliefShower() {
            if (typeof confetti === 'function') {
                confetti({
                    particleCount: 80,
                    spread: 70,
                    origin: { y: 0.6 }
                });
            }
            for (let i = 0; i < 25; i++) {
                particles.push(createParticle(true));
            }
            showCustomToast("Heart shower dikirim!! Semoga perut Kia makin adem yaa 💕");
        }
    </script>
</body>
</html>
