<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title data-key="app_title">Veo 3 Prompt Generator</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Mengatur font Inter */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f2f5; /* Warna latar belakang abu-abu muda */
            color: #333;
        }
        body.dark {
            background-color: #1a202c; /* Latar belakang gelap */
            color: #e2e8f0; /* Teks terang */
        }
        .bg-white {
            background-color: #fff;
        }
        .dark .bg-white {
            background-color: #2d3748; /* Latar belakang panel gelap */
        }
        .border-gray-200 {
            border-color: #edf2f7;
        }
        .dark .border-gray-200 {
            border-color: #4a5568;
        }
        .text-gray-800 {
            color: #2d3748;
        }
        .dark .text-gray-800 {
            color: #f7fafc;
        }
        .text-gray-600 {
            color: #718096;
        }
        .dark .text-gray-600 {
            color: #a0aec0;
        }
        .text-gray-700 {
            color: #4a5568;
        }
        .dark .text-gray-700 {
            color: #cbd5e0;
        }
        .border-gray-300 {
            border-color: #cbd5e0;
        }
        .dark .border-gray-300 {
            border-color: #4a5568;
        }
        .placeholder-gray-400::placeholder {
            color: #a0aec0;
        }
        .dark .placeholder-gray-400::placeholder {
            color: #718096;
        }
        .bg-gray-50 {
            background-color: #f9fafb;
        }
        .dark .bg-gray-50 {
            background-color: #1a202c; /* Latar belakang gelap untuk textarea output */
        }
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            width: 24px;
            height: 24px;
            border-radius: 50%;
            border-left-color: #3b82f6; /* Warna biru */
            animation: spin 1s ease infinite;
        }
        .dark .spinner {
            border-left-color: #63b3ed; /* Warna biru lebih terang untuk dark mode */
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        /* Style untuk emoji moon/sun */
        .theme-icon {
            font-size: 1.25rem; /* Ukuran yang sesuai untuk emoji */
            line-height: 1; /* Pastikan tidak ada spasi ekstra */
        }
    </style>
</head>
<body class="p-4 sm:p-8 flex items-center justify-center min-h-screen transition-colors duration-300 ease-in-out">
    <div class="bg-white p-6 sm:p-8 rounded-xl shadow-lg w-full max-w-2xl border border-gray-200 transition-colors duration-300 ease-in-out">
        <h1 class="text-3xl font-bold text-center mb-2 text-gray-800" data-key="main_title">Veo 3 Prompt Generator</h1>
        <p class="text-center text-gray-500 text-sm mb-6">by.RIMURUXRAM</p>
        <p class="text-center text-gray-600 mb-8" data-key="app_description">Buat prompt yang terstruktur untuk Veo 3 dengan mudah!</p>

        <!-- UI Language and Dark Mode Toggles -->
        <div class="flex flex-col sm:flex-row justify-between items-center mb-6 space-y-4 sm:space-y-0 sm:space-x-4">
            <!-- UI Language Selector -->
            <div class="w-full sm:w-1/2">
                <label for="uiLanguageSelect" class="block text-gray-700 text-sm font-semibold mb-2" data-key="select_ui_language_label">Pilih Bahasa UI:</label>
                <select id="uiLanguageSelect" class="w-full p-2 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="id">Bahasa Indonesia</option>
                    <option value="en">English</option>
                </select>
            </div>

            <!-- Dark Mode Toggle with Emojis -->
            <div class="w-full sm:w-1/2 flex items-center justify-end space-x-2">
                <span id="themeIcon" class="theme-icon text-gray-700 dark:text-gray-400">☀️</span>
                <label for="darkModeToggle" class="relative inline-flex items-center cursor-pointer">
                    <input type="checkbox" id="darkModeToggle" class="sr-only peer">
                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border after:border-gray-300 after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
                </label>
            </div>
        </div>

        <!-- Form untuk input prompt -->
        <div class="space-y-6">
            <!-- Deskripsi Adegan -->
            <div>
                <label for="sceneDescription" class="block text-gray-700 text-sm font-semibold mb-2" data-key="scene_description_label">
                    1. Deskripsi Adegan <span class="text-red-500">*</span>: Apa yang terjadi dalam video?
                </label>
                <textarea id="sceneDescription" rows="4" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600" placeholder="Contoh: Seorang astronot melayang di ruang angkasa di samping stasiun luar angkasa, menatap bumi yang biru." data-key="scene_description_placeholder"></textarea>
                <p id="sceneDescriptionError" class="text-red-500 dark:text-red-300 text-xs mt-1 hidden" data-key="scene_description_error">Deskripsi Adegan tidak boleh kosong.</p>
            </div>

            <!-- Target Video Language -->
            <div>
                <label for="targetVideoLanguage" class="block text-gray-700 text-sm font-semibold mb-2" data-key="target_video_language_label">
                    2. Bahasa Video Target (untuk konten video):
                </label>
                <select id="targetVideoLanguage" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_video_language">Pilih Bahasa Video</option>
                    <option value="Indonesian" data-key="opt_lang_indonesian">Bahasa Indonesia</option>
                    <option value="Sunda" data-key="opt_lang_sunda">Bahasa Sunda</option>
                    <option value="Javanese" data-key="opt_lang_javanese">Bahasa Jawa</option>
                    <option value="Malay" data-key="opt_lang_malay">Bahasa Melayu</option>
                    <option value="Kalimantan (Banjar)" data-key="opt_lang_kalimantan">Bahasa Kalimantan (Banjar)</option>
                    <option value="English" data-key="opt_lang_english">English</option>
                    <option value="Japanese" data-key="opt_lang_japanese">日本語 (Japanese)</option>
                    <option value="Arabic" data-key="opt_lang_arabic">العربية (Arabic)</option>
                </select>
            </div>

            <!-- Resolution -->
            <div>
                <label for="resolutionSelect" class="block text-gray-700 text-sm font-semibold mb-2" data-key="resolution_label">
                    3. Resolusi Video:
                </label>
                <select id="resolutionSelect" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_resolution">Pilih Resolusi</option>
                    <option value="720p">720p (HD)</option>
                    <option value="1080p">1080p (Full HD)</option>
                    <option value="1440p">1440p (2K)</option>
                    <option value="2160p (4K)">2160p (4K)</option>
                    <option value="4320p (8K)">4320p (8K)</option>
                </select>
            </div>

            <!-- Frame Rate -->
            <div>
                <label for="frameRateSelect" class="block text-gray-700 text-sm font-semibold mb-2" data-key="frame_rate_label">
                    4. Frame Rate:
                </label>
                <select id="frameRateSelect" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_frame_rate">Pilih Frame Rate</option>
                    <option value="24fps">24fps (Cinematic)</option>
                    <option value="30fps">30fps (Standard)</option>
                    <option value="60fps">60fps (Smooth)</option>
                    <option value="120fps">120fps (Slow-mo)</option>
                </select>
            </div>

            <!-- Gaya Visual -->
            <div>
                <label for="visualStyle" class="block text-gray-700 text-sm font-semibold mb-2" data-key="visual_style_label">
                    5. Gaya Visual: Estetika video.
                </label>
                <select id="visualStyle" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_visual_style">Pilih Gaya Visual</option>
                    <option value="cinematic" data-key="opt_cinematic">Cinematic</option>
                    <option value="documentary" data-key="opt_documentary">Documentary</option>
                    <option value="anime" data-key="opt_anime">Anime</option>
                    <option value="hyperrealistic" data-key="opt_hyperrealistic">Hyperrealistic</option>
                    <option value="surreal" data-key="opt_surreal">Surreal</option>
                    <option value="cartoon" data-key="opt_cartoon">Cartoon</option>
                    <option value="abstract" data-key="opt_abstract">Abstract</option>
                    <option value="photorealistic" data-key="opt_photorealistic">Photorealistic</option>
                    <option value="stylized" data-key="opt_stylized">Stylized</option>
                </select>
            </div>

            <!-- Pencahayaan -->
            <div>
                <label for="lighting" class="block text-gray-700 text-sm font-semibold mb-2" data-key="lighting_label">
                    6. Pencahayaan: Bagaimana pencahayaan diatur.
                </label>
                <select id="lighting" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_lighting">Pilih Pencahayaan</option>
                    <option value="soft lighting" data-key="opt_soft_lighting">Soft Lighting</option>
                    <option value="harsh shadows" data-key="opt_harsh_shadows">Harsh Shadows</option>
                    <option value="golden hour" data-key="opt_golden_hour">Golden Hour</option>
                    <option value="blue hour" data-key="opt_blue_hour">Blue Hour</option>
                    <option value="neon lights" data-key="opt_neon_lights">Neon Lights</option>
                    <option value="studio lighting" data-key="opt_studio_lighting">Studio Lighting</option>
                    <option value="natural light" data-key="opt_natural_light">Natural Light</option>
                    <option value="moody lighting" data-key="opt_moody_lighting">Moody Lighting</option>
                    <option value="backlight" data-key="opt_backlight">Backlight</option>
                </select>
            </div>

            <!-- Sudut/Gerakan Kamera -->
            <div>
                <label for="cameraAngle" class="block text-gray-700 text-sm font-semibold mb-2" data-key="camera_angle_label">
                    7. Sudut/Gerakan Kamera: Bagaimana kamera merekam adegan.
                </label>
                <select id="cameraAngle" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_camera_angle">Pilih Sudut/Gerakan Kamera</option>
                    <option value="low angle" data-key="opt_low_angle">Low Angle</option>
                    <option value="high angle" data-key="opt_high_angle">High Angle</option>
                    <option value="dolly shot" data-key="opt_dolly_shot">Dolly Shot</option>
                    <option value="tracking shot" data-key="opt_tracking_shot">Tracking Shot</option>
                    <option value="zoom in" data-key="opt_zoom_in">Zoom In</option>
                    <option value="zoom out" data-key="opt_zoom_out">Zoom Out</option>
                    <option value="pan" data-key="opt_pan">Pan</option>
                    <option value="tilt" data-key="opt_tilt">Tilt</option>
                    <option value="POV (Point of View)" data-key="opt_pov">POV (Point of View)</option>
                    <option value="static shot" data-key="opt_static_shot">Static Shot</option>
                    <option value="crane shot" data-key="opt_crane_shot">Crane Shot</option>
                    <option value="drone shot" data-key="opt_drone_shot">Drone Shot</option>
                </select>
            </div>

            <!-- Komposisi -->
            <div>
                <label for="composition" class="block text-gray-700 text-sm font-semibold mb-2" data-key="composition_label">
                    8. Komposisi: Penempatan elemen dalam frame.
                </label>
                <select id="composition" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_composition">Pilih Komposisi</option>
                    <option value="rule of thirds" data-key="opt_rule_of_thirds">Rule of Thirds</option>
                    <option value="leading lines" data-key="opt_leading_lines">Leading Lines</option>
                    <option value="symmetrical" data-key="opt_symmetrical">Symmetrical</option>
                    <option value="asymmetrical" data-key="opt_asymmetrical">Asymmetrical</option>
                    <option value="close-up" data-key="opt_close_up">Close-up</option>
                    <option value="wide shot" data-key="opt_wide_shot">Wide Shot</option>
                    <option value="medium shot" data-key="opt_medium_shot">Medium Shot</option>
                    <option value="dutch angle" data-key="opt_dutch_angle">Dutch Angle</option>
                    <option value="bokeh" data-key="opt_bokeh">Bokeh</option>
                    <option value="depth of field" data-key="opt_depth_of_field">Depth of Field</option>
                </select>
            </div>

            <!-- Palet Warna -->
            <div>
                <label for="colorPalette" class="block text-gray-700 text-sm font-semibold mb-2" data-key="color_palette_label">
                    9. Palet Warna: Dominasi warna dalam video.
                </label>
                <select id="colorPalette" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_color_palette">Pilih Palet Warna</option>
                    <option value="vibrant colors" data-key="opt_vibrant_colors">Vibrant Colors</option>
                    <option value="monochromatic" data-key="opt_monochromatic">Monochromatic</option>
                    <option value="warm tones" data-key="opt_warm_tones">Warm Tones</option>
                    <option value="cool tones" data-key="opt_cool_tones">Cool Tones</option>
                    <option value="pastel colors" data-key="opt_pastel_colors">Pastel Colors</option>
                    <option value="earthy tones" data-key="opt_earthy_tones">Earthy Tones</option>
                    <option value="muted colors" data-key="opt_muted_colors">Muted Colors</option>
                    <option value="black and white" data-key="opt_black_and_white">Black and White</option>
                    <option value="desaturated" data-key="opt_desaturated">Desaturated</option>
                </select>
            </div>

            <!-- Suasana/Atmosfer -->
            <div>
                <label for="moodAtmosphere" class="block text-gray-700 text-sm font-semibold mb-2" data-key="mood_atmosphere_label">
                    10. Suasana/Atmosfer: Emosi atau perasaan yang ingin disampaikan.
                </label>
                <select id="moodAtmosphere" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600">
                    <option value="" data-key="select_mood_atmosphere">Pilih Suasana/Atmosfer</option>
                    <option value="mysterious" data-key="opt_mysterious">Mysterious</option>
                    <option value="joyful" data-key="opt_joyful">Joyful</option>
                    <option value="tense" data-key="opt_tense">Tense</option>
                    <option value="calm" data-key="opt_calm">Calm</option>
                    <option value="eerie" data-key="opt_eerie">Eerie</option>
                    <option value="grand" data-key="opt_grand">Grand</option>
                    <option value="futuristic" data-key="opt_futuristic">Futuristic</option>
                    <option value="retro" data-key="opt_retro">Retro</option>
                    <option value="dreamlike" data-key="opt_dreamlike">Dreamlike</option>
                    <option value="peaceful" data-key="opt_peaceful">Peaceful</option>
                    <option value="energetic" data-key="opt_energetic">Energetic</option>
                    <option value="melancholic" data-key="opt_melancholic">Melancholic</option>
                </select>
            </div>

            <!-- Detail Tambahan -->
            <div>
                <label for="additionalDetails" class="block text-gray-700 text-sm font-semibold mb-2" data-key="additional_details_label">
                    11. Detail Tambahan (Opsional): Objek spesifik, catatan lain, dll.
                </label>
                <input type="text" id="additionalDetails" class="w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 text-gray-800 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600" placeholder="Contoh: Fokus pada ekspresi wajah karakter utama." data-key="additional_details_placeholder">
            </div>

            <!-- Tombol Generate Prompt -->
            <button id="generateBtn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded-lg transition duration-300 ease-in-out shadow-md" data-key="generate_prompt_button">
                Generate Prompt
            </button>
        </div>

        <!-- Area Output Prompt -->
        <div class="mt-8">
            <h2 class="text-xl font-semibold mb-4 text-gray-800" data-key="your_prompt_title">Prompt Anda:</h2>
            <textarea id="outputPrompt" rows="6" readonly class="w-full p-3 border rounded-lg bg-gray-50 text-gray-800 resize-none dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600"></textarea>
            <button id="copyBtn" class="mt-4 w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-4 rounded-lg transition duration-300 ease-in-out shadow-md" data-key="copy_prompt_button">
                Salin Prompt
            </button>
            <div id="copySuccessMessage" class="hidden mt-2 text-sm text-green-700 dark:text-green-300 text-center" data-key="copy_success_message">
                Prompt berhasil disalin!
            </div>
        </div>

        <!-- Fitur LLM-Powered: Prompt Suggestion -->
        <div class="mt-8">
            <h2 class="text-xl font-semibold mb-4 text-gray-800" data-key="ai_suggestions_title">✨ Saran Prompt dari AI:</h2>
            <button id="getSuggestionBtn" class="w-full bg-purple-600 hover:bg-purple-700 text-white font-bold py-3 px-4 rounded-lg transition duration-300 ease-in-out shadow-md flex items-center justify-center" data-key="get_suggestion_button">
                <span id="getSuggestionBtnText" data-key="get_suggestion_button_text">Dapatkan Saran Prompt</span>
                <div id="suggestionSpinner" class="spinner ml-3 hidden"></div>
            </button>
            <textarea id="aiSuggestionOutput" rows="6" readonly class="w-full p-3 border rounded-lg bg-gray-50 text-gray-800 resize-none mt-4 dark:bg-gray-700 dark:text-gray-200 dark:border-gray-600" data-key="ai_suggestion_placeholder"></textarea>
            <p id="suggestionErrorMessage" class="text-red-500 dark:text-red-300 text-xs mt-1 hidden text-center" data-key="suggestion_error_message">Terjadi kesalahan saat mendapatkan saran. Coba lagi.</p>
            <p id="aiExplanation" class="text-gray-600 dark:text-gray-400 text-sm mt-4" data-key="ai_explanation_note">
                Catatan: Fitur ini menggunakan AI untuk memberikan saran teks tentang gaya pengisi suara dan contoh skrip. Ini tidak menghasilkan audio nyata.
            </p>
        </div>

        <!-- Fitur Donasi -->
        <div class="mt-8 text-center">
            <h2 class="text-xl font-semibold mb-4 text-gray-800" data-key="donate_title">Dukung Kami!</h2>
            <a href="https://saweria.co/RIMURUXRAM" target="_blank" rel="noopener noreferrer" class="inline-block bg-pink-500 hover:bg-pink-600 text-white font-bold py-3 px-6 rounded-lg transition duration-300 ease-in-out shadow-md" data-key="donate_button">
                Donasi via Saweria
            </a>
            <p class="text-gray-600 dark:text-gray-400 text-sm mt-2" data-key="donate_message">Dukungan Anda sangat berarti untuk pengembangan aplikasi ini.</p>
        </div>

        <p class="text-center text-gray-500 dark:text-gray-400 text-sm mt-8">
            ---
            <span data-key="usage_note">Salin prompt yang dihasilkan dan tempelkan ke antarmuka Veo 3 untuk membuat video Anda.</span>
        </p>
        <p class="text-center text-gray-500 dark:text-gray-400 text-xs mt-4" data-key="copyright_info">
            &copy; 2025 RIMURUXRAM. All rights reserved.
        </p>
    </div>

    <script>
        // --- Referensi Elemen DOM ---
        const sceneDescription = document.getElementById('sceneDescription');
        const sceneDescriptionError = document.getElementById('sceneDescriptionError');
        const targetVideoLanguage = document.getElementById('targetVideoLanguage'); // New: Target Video Language
        const resolutionSelect = document.getElementById('resolutionSelect'); // New: Resolution select
        const frameRateSelect = document.getElementById('frameRateSelect'); // New: Frame Rate select
        const visualStyle = document.getElementById('visualStyle');
        const lighting = document.getElementById('lighting');
        const cameraAngle = document.getElementById('cameraAngle');
        const composition = document.getElementById('composition');
        const colorPalette = document.getElementById('colorPalette');
        const moodAtmosphere = document.getElementById('moodAtmosphere');
        const additionalDetails = document.getElementById('additionalDetails'); // Existing, now for other notes
        const generateBtn = document.getElementById('generateBtn');
        const outputPrompt = document.getElementById('outputPrompt');
        const copyBtn = document.getElementById('copyBtn');
        const copySuccessMessage = document.getElementById('copySuccessMessage');

        const getSuggestionBtn = document.getElementById('getSuggestionBtn');
        const getSuggestionBtnText = document.getElementById('getSuggestionBtnText');
        const suggestionSpinner = document.getElementById('suggestionSpinner');
        const aiSuggestionOutput = document.getElementById('aiSuggestionOutput');
        const suggestionErrorMessage = document.getElementById('suggestionErrorMessage');

        const uiLanguageSelect = document.getElementById('uiLanguageSelect');
        const darkModeToggle = document.getElementById('darkModeToggle');
        const body = document.body;
        const themeIcon = document.getElementById('themeIcon'); // Referensi ke elemen ikon tema

        // --- Objek Terjemahan ---
        // UI Language (id, en)
        const translations = {
            id: {
                app_title: "Veo 3 Prompt Generator",
                main_title: "Veo 3 Prompt Generator",
                app_description: "Buat prompt yang terstruktur untuk Veo 3 dengan mudah!",
                select_ui_language_label: "Pilih Bahasa UI:",
                dark_mode_label: "Dark Mode", // This label is now unused in HTML but kept for consistency
                scene_description_label: "1. Deskripsi Adegan *: Apa yang terjadi dalam video?",
                scene_description_placeholder: "Contoh: Seorang astronot melayang di ruang angkasa di samping stasiun luar angkasa, menatap bumi yang biru.",
                scene_description_error: "Deskripsi Adegan tidak boleh kosong.",
                target_video_language_label: "2. Bahasa Video Target (untuk konten video):",
                select_video_language: "Pilih Bahasa Video",
                opt_lang_indonesian: "Bahasa Indonesia",
                opt_lang_sunda: "Bahasa Sunda",
                opt_lang_javanese: "Bahasa Jawa",
                opt_lang_malay: "Bahasa Melayu",
                opt_lang_kalimantan: "Bahasa Kalimantan (Banjar)",
                opt_lang_english: "English",
                opt_lang_japanese: "日本語 (Japanese)",
                opt_lang_arabic: "العربية (Arabic)",
                resolution_label: "3. Resolusi Video:",
                select_resolution: "Pilih Resolusi",
                frame_rate_label: "4. Frame Rate:",
                select_frame_rate: "Pilih Frame Rate",
                visual_style_label: "5. Gaya Visual: Estetika video.",
                select_visual_style: "Pilih Gaya Visual",
                opt_cinematic: "Cinematic",
                opt_documentary: "Documentary",
                opt_anime: "Anime",
                opt_hyperrealistic: "Hyperrealistic",
                opt_surreal: "Surreal",
                opt_cartoon: "Cartoon",
                opt_abstract: "Abstract",
                opt_photorealistic: "Photorealistic",
                opt_stylized: "Stylized",
                lighting_label: "6. Pencahayaan: Bagaimana pencahayaan diatur.",
                select_lighting: "Pilih Pencahayaan",
                opt_soft_lighting: "Soft Lighting",
                opt_harsh_shadows: "Harsh Shadows",
                opt_golden_hour: "Golden Hour",
                opt_blue_hour: "Blue Hour",
                opt_neon_lights: "Neon Lights",
                opt_studio_lighting: "Studio Lighting",
                opt_natural_light: "Natural Light",
                opt_moody_lighting: "Moody Lighting",
                opt_backlight: "Backlight",
                camera_angle_label: "7. Sudut/Gerakan Kamera: Bagaimana kamera merekam adegan.",
                select_camera_angle: "Pilih Sudut/Gerakan Kamera",
                opt_low_angle: "Low Angle",
                opt_high_angle: "High Angle",
                opt_dolly_shot: "Dolly Shot",
                opt_tracking_shot: "Tracking Shot",
                opt_zoom_in: "Zoom In",
                opt_zoom_out: "Zoom Out",
                opt_pan: "Pan",
                opt_tilt: "Tilt",
                opt_pov: "POV (Point of View)",
                opt_static_shot: "Static Shot",
                opt_crane_shot: "Crane Shot",
                opt_drone_shot: "Drone Shot",
                composition_label: "8. Komposisi: Penempatan elemen dalam frame.",
                select_composition: "Pilih Komposisi",
                opt_rule_of_thirds: "Rule of Thirds",
                opt_leading_lines: "Leading Lines",
                opt_symmetrical: "Symmetrical",
                opt_asymmetrical: "Asymmetrical",
                opt_close_up: "Close-up",
                opt_wide_shot: "Wide Shot",
                opt_medium_shot: "Medium Shot",
                opt_dutch_angle: "Dutch Angle",
                opt_bokeh: "Bokeh",
                opt_depth_of_field: "Depth of Field",
                color_palette_label: "9. Palet Warna: Dominasi warna dalam video.",
                select_color_palette: "Pilih Palet Warna",
                opt_vibrant_colors: "Vibrant Colors",
                opt_monochromatic: "Monochromatic",
                opt_warm_tones: "Warm Tones",
                opt_cool_tones: "Cool Tones",
                opt_pastel_colors: "Pastel Colors",
                opt_earthy_tones: "Earthy Tones",
                opt_muted_colors: "Muted Colors",
                opt_black_and_white: "Black and White",
                opt_desaturated: "Desaturated",
                mood_atmosphere_label: "10. Suasana/Atmosfer: Emosi atau perasaan yang ingin disampaikan.",
                select_mood_atmosphere: "Pilih Suasana/Atmosfer",
                opt_mysterious: "Mysterious",
                opt_joyful: "Joyful",
                opt_tense: "Tense",
                opt_calm: "Calm",
                opt_eerie: "Eerie",
                opt_grand: "Grand",
                opt_futuristic: "Futuristic",
                opt_retro: "Retro",
                opt_dreamlike: "Dreamlike",
                opt_peaceful: "Peaceful",
                opt_energetic: "Energetic",
                opt_melancholic: "Melancholic",
                additional_details_label: "11. Detail Tambahan (Opsional): Objek spesifik, catatan lain, dll.",
                additional_details_placeholder: "Contoh: Fokus pada ekspresi wajah karakter utama.",
                generate_prompt_button: "Generate Prompt",
                your_prompt_title: "Prompt Anda:",
                copy_prompt_button: "Salin Prompt",
                copy_success_message: "Prompt berhasil disalin!",
                ai_suggestions_title: "✨ Saran Prompt dari AI:",
                get_suggestion_button: "Dapatkan Saran Prompt",
                get_suggestion_button_text: "Dapatkan Saran Prompt",
                ai_suggestion_placeholder: "Saran akan muncul di sini...",
                ai_suggestion_no_prompt: "Silakan buat prompt utama terlebih dahulu sebelum meminta saran.",
                ai_suggestion_error_response: "Tidak dapat menghasilkan saran. Struktur respons AI tidak terduga.",
                ai_suggestion_error_api: "Terjadi kesalahan saat menghubungkan ke AI. Mohon coba lagi nanti.",
                ai_explanation_note: "Catatan: Fitur ini menggunakan AI untuk memberikan saran teks tentang gaya pengisi suara dan contoh skrip. Ini tidak menghasilkan audio nyata.",
                donate_title: "Dukung Kami!",
                donate_button: "Donasi via Saweria",
                donate_message: "Dukungan Anda sangat berarti untuk pengembangan aplikasi ini.",
                usage_note: "Salin prompt yang dihasilkan dan tempelkan ke antarmuka Veo 3 untuk membuat video Anda.",
                copyright_info: "&copy; 2025 RIMURUXRAM. All rights reserved.",
                ai_loading_text: "Mendapatkan Saran..."
            },
            en: { // English
                app_title: "Veo 3 Prompt Generator",
                main_title: "Veo 3 Prompt Generator",
                app_description: "Easily create structured prompts for Veo 3!",
                select_ui_language_label: "Select UI Language:",
                dark_mode_label: "Dark Mode", // This label is now unused in HTML but kept for consistency
                scene_description_label: "1. Scene Description *: What is happening in the video?",
                scene_description_placeholder: "Example: An astronaut floats in space next to a space station, gazing at the blue earth.",
                scene_description_error: "Scene description cannot be empty.",
                target_video_language_label: "2. Target Video Language (for video content):",
                select_video_language: "Select Video Language",
                opt_lang_indonesian: "Bahasa Indonesia",
                opt_lang_sunda: "Sunda",
                opt_lang_javanese: "Javanese",
                opt_lang_malay: "Malay",
                opt_lang_kalimantan: "Kalimantan (Banjar)",
                opt_lang_english: "English",
                opt_lang_japanese: "日本語 (Japanese)",
                opt_lang_arabic: "العربية (Arabic)",
                resolution_label: "3. Video Resolution:",
                select_resolution: "Select Resolution",
                frame_rate_label: "4. Frame Rate:",
                select_frame_rate: "Select Frame Rate",
                visual_style_label: "5. Visual Style: The aesthetic of the video.",
                select_visual_style: "Select Visual Style",
                opt_cinematic: "Cinematic",
                opt_documentary: "Documentary",
                opt_anime: "Anime",
                opt_hyperrealistic: "Hyperrealistic",
                opt_surreal: "Surreal",
                opt_cartoon: "Cartoon",
                opt_abstract: "Abstract",
                opt_photorealistic: "Photorealistic",
                opt_stylized: "Stylized",
                lighting_label: "6. Lighting: How the lighting is arranged.",
                select_lighting: "Select Lighting",
                opt_soft_lighting: "Soft Lighting",
                opt_harsh_shadows: "Harsh Shadows",
                opt_golden_hour: "Golden Hour",
                opt_blue_hour: "Blue Hour",
                opt_neon_lights: "Neon Lights",
                opt_studio_lighting: "Studio Lighting",
                opt_natural_light: "Natural Light",
                opt_moody_lighting: "Moody Lighting",
                opt_backlight: "Backlight",
                camera_angle_label: "7. Camera Angle/Movement: How the camera records the scene.",
                select_camera_angle: "Select Camera Angle/Movement",
                opt_low_angle: "Low Angle",
                opt_high_angle: "High Angle",
                opt_dolly_shot: "Dolly Shot",
                opt_tracking_shot: "Tracking Shot",
                opt_zoom_in: "Zoom In",
                opt_zoom_out: "Zoom Out",
                opt_pan: "Pan",
                opt_tilt: "Tilt",
                opt_pov: "POV (Point of View)",
                opt_static_shot: "Static Shot",
                opt_crane_shot: "Crane Shot",
                opt_drone_shot: "Drone Shot",
                composition_label: "8. Composition: Placement of elements in the frame.",
                select_composition: "Select Composition",
                opt_rule_of_thirds: "Rule of Thirds",
                opt_leading_lines: "Leading Lines",
                opt_symmetrical: "Symmetrical",
                opt_asymmetrical: "Asymmetrical",
                opt_close_up: "Close-up",
                opt_wide_shot: "Wide Shot",
                opt_medium_shot: "Medium Shot",
                opt_dutch_angle: "Dutch Angle",
                opt_bokeh: "Bokeh",
                opt_depth_of_field: "Depth of Field",
                color_palette_label: "9. Color Palette: Dominance of colors in the video.",
                select_color_palette: "Select Color Palette",
                opt_vibrant_colors: "Vibrant Colors",
                opt_monochromatic: "Monochromatic",
                opt_warm_tones: "Warm Tones",
                opt_cool_tones: "Cool Tones",
                opt_pastel_colors: "Pastel Colors",
                opt_earthy_tones: "Earthy Tones",
                opt_muted_colors: "Muted Colors",
                opt_black_and_white: "Black and White",
                opt_desaturated: "Desaturated",
                mood_atmosphere_label: "10. Mood/Atmosphere: Emotion or feeling to convey.",
                select_mood_atmosphere: "Select Mood/Atmosphere",
                opt_mysterious: "Mysterious",
                opt_joyful: "Joyful",
                opt_tense: "Tense",
                opt_calm: "Calm",
                opt_eerie: "Eerie",
                opt_grand: "Grand",
                opt_futuristic: "Futuristic",
                opt_retro: "Retro",
                opt_dreamlike: "Dreamlike",
                opt_peaceful: "Peaceful",
                opt_energetic: "Energetic",
                opt_melancholic: "Melancholic",
                additional_details_label: "11. Additional Details (Optional): Specific objects, other notes, etc.",
                additional_details_placeholder: "Example: Focus on the main character's facial expressions.",
                generate_prompt_button: "Generate Prompt",
                your_prompt_title: "Your Prompt:",
                copy_prompt_button: "Copy Prompt",
                copy_success_message: "Prompt copied successfully!",
                ai_suggestions_title: "✨ AI Prompt Suggestions:",
                get_suggestion_button: "Get Prompt Suggestion",
                get_suggestion_button_text: "Get Prompt Suggestion",
                ai_suggestion_placeholder: "Suggestions will appear here...",
                ai_suggestion_no_prompt: "Please generate a main prompt first before asking for suggestions.",
                ai_suggestion_error_response: "Could not generate suggestions. Unexpected AI response structure.",
                ai_suggestion_error_api: "An error occurred while connecting to AI. Please try again later.",
                ai_explanation_note: "Note: This feature uses AI to provide text suggestions for voice-over styles and script examples. It does not generate actual audio.",
                donate_title: "Support Us!",
                donate_button: "Donate via Saweria",
                donate_message: "Your support is very meaningful for the development of this application.",
                usage_note: "Copy the generated prompt and paste it into the Veo 3 interface to create your video.",
                copyright_info: "&copy; 2025 RIMURUXRAM. All rights reserved.",
                ai_loading_text: "Getting Suggestion..."
            }
        };

        // --- Fungsi untuk Menerapkan Terjemahan UI ---
        function applyUILocale(lang) {
            document.querySelectorAll('[data-key]').forEach(element => {
                const key = element.getAttribute('data-key');
                if (translations[lang] && translations[lang][key]) {
                    if (element.tagName === 'INPUT' || element.tagName === 'TEXTAREA') {
                        element.placeholder = translations[lang][key];
                    } else {
                        // For static text content, update textContent
                        element.textContent = translations[lang][key];
                    }
                }
            });
            // Update placeholder for AI suggestion output explicitly
            aiSuggestionOutput.placeholder = translations[lang].ai_suggestion_placeholder;

            // Update theme icon based on current dark mode state
            updateThemeIcon();
        }

        // --- Inisialisasi Bahasa UI ---
        const initialUILang = localStorage.getItem('uiLang') || 'id'; // Default to Indonesian
        uiLanguageSelect.value = initialUILang;
        applyUILocale(initialUILang);

        // --- Event Listener untuk Perubahan Bahasa UI ---
        uiLanguageSelect.addEventListener('change', (event) => {
            const newUILang = event.target.value;
            localStorage.setItem('uiLang', newUILang); // Simpan preferensi bahasa UI
            applyUILocale(newUILang);
        });

        // --- Fungsi untuk Mengelola Dark Mode dan Update Icon ---
        function updateThemeIcon() {
            if (body.classList.contains('dark')) {
                themeIcon.textContent = '🌙'; // Moon emoji for dark mode
                themeIcon.classList.add('dark:text-gray-400'); // Ensure visibility in dark mode
                themeIcon.classList.remove('text-gray-700');
            } else {
                themeIcon.textContent = '☀️'; // Sun emoji for light mode
                themeIcon.classList.add('text-gray-700');
                themeIcon.classList.remove('dark:text-gray-400');
            }
        }

        function toggleDarkMode(isDark) {
            if (isDark) {
                body.classList.add('dark');
                localStorage.setItem('darkMode', 'true');
            } else {
                body.classList.remove('dark');
                localStorage.setItem('darkMode', 'false');
            }
            darkModeToggle.checked = isDark; // Sinkronkan status toggle
            updateThemeIcon(); // Update the emoji icon
        }

        // --- Inisialisasi Dark Mode ---
        const savedDarkMode = localStorage.getItem('darkMode');
        if (savedDarkMode === 'true') {
            toggleDarkMode(true);
        } else {
            toggleDarkMode(false);
        }

        // --- Event Listener untuk Toggle Dark Mode ---
        darkModeToggle.addEventListener('change', () => {
            toggleDarkMode(darkModeToggle.checked);
        });

        // --- Fungsi untuk menghasilkan prompt utama ---
        generateBtn.addEventListener('click', () => {
            // Memastikan deskripsi adegan tidak kosong
            if (sceneDescription.value.trim() === '') {
                sceneDescriptionError.classList.remove('hidden'); // Menampilkan pesan error
                outputPrompt.value = ''; // Mengosongkan output jika ada error
                return; // Menghentikan fungsi
            } else {
                sceneDescriptionError.classList.add('hidden'); // Menyembunyikan pesan error jika sudah valid
            }

            // Mengumpulkan semua bagian prompt
            const parts = [];

            // Tambahkan deskripsi adegan (wajib)
            parts.push(sceneDescription.value.trim());

            // Tambahkan bahasa video target jika dipilih
            if (targetVideoLanguage.value) {
                parts.push(`Video in ${targetVideoLanguage.value}`);
            }
            
            // Tambahkan resolusi dan frame rate jika dipilih
            if (resolutionSelect.value) {
                parts.push(`Resolution: ${resolutionSelect.value}`);
            }
            if (frameRateSelect.value) {
                parts.push(`Frame Rate: ${frameRateSelect.value}`);
            }

            // Tambahkan bagian opsional jika dipilih/diisi
            if (visualStyle.value) {
                parts.push(visualStyle.value);
            }
            if (lighting.value) {
                parts.push(lighting.value);
            }
            if (cameraAngle.value) {
                parts.push(cameraAngle.value);
            }
            if (composition.value) {
                parts.push(composition.value);
            }
            if (colorPalette.value) {
                parts.push(colorPalette.value);
            }
            if (moodAtmosphere.value) {
                parts.push(moodAtmosphere.value);
            }
            if (additionalDetails.value.trim()) {
                parts.push(additionalDetails.value.trim());
            }

            // Gabungkan bagian-bagian prompt dengan koma dan spasi
            const generatedText = parts.join(', ');
            outputPrompt.value = generatedText; // Menampilkan prompt yang dihasilkan
        });

        // --- Fungsi untuk menyalin prompt ke clipboard ---
        copyBtn.addEventListener('click', () => {
            outputPrompt.select(); // Memilih teks di area output
            outputPrompt.setSelectionRange(0, 99999); // Untuk perangkat mobile

            // Menyalin teks ke clipboard
            document.execCommand('copy');

            // Menampilkan pesan sukses
            copySuccessMessage.classList.remove('hidden');
            setTimeout(() => {
                copySuccessMessage.classList.add('hidden');
            }, 2000); // Pesan akan hilang setelah 2 detik
        });

        // --- Fungsi untuk mendapatkan saran prompt pengisi suara dari Gemini API ---
        getSuggestionBtn.addEventListener('click', async () => {
            const currentPrompt = outputPrompt.value.trim();
            const currentUILang = uiLanguageSelect.value; // Get current selected UI language
            const selectedVideoLang = targetVideoLanguage.value || 'English'; // Default to English if no video language is selected

            if (!currentPrompt) {
                aiSuggestionOutput.value = translations[currentUILang].ai_suggestion_no_prompt || "Please generate a main prompt first before asking for suggestions.";
                return;
            }

            // Tampilkan loading spinner dan nonaktifkan tombol
            getSuggestionBtnText.textContent = translations[currentUILang].ai_loading_text || 'Getting Suggestion...';
            suggestionSpinner.classList.remove('hidden');
            getSuggestionBtn.disabled = true;
            aiSuggestionOutput.value = ''; // Bersihkan area saran sebelumnya
            suggestionErrorMessage.classList.add('hidden'); // Sembunyikan pesan error

            try {
                let chatHistory = [];
                // Construct the prompt for LLM, asking for suggestions in the selected video language
                // Explicitly ask for a wide variety of voice-over styles
                const promptForLLM = `I have the following video prompt: "${currentPrompt}". Please provide creative suggestions for various voice-over characters (e.g., anime female, cool male, soft male, deep male, excited female, calm narrator, child, elderly, robotic, energetic, smooth, gravelly, mysterious, authoritative, friendly, sarcastic, etc.) and a short example script (max 2 sentences) that fits this prompt. The suggestions and script MUST be in ${selectedVideoLang}. Focus on style, emotion, and how the voice can enhance the video. Provide suggestions in an easy-to-read format, separating character and script.`;
                
                chatHistory.push({ role: "user", parts: [{ text: promptForLLM }] });

                const payload = { contents: chatHistory };
                const apiKey = ""; // API key will be automatically provided by Canvas
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();
                
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    aiSuggestionOutput.value = text;
                } else {
                    aiSuggestionOutput.value = translations[currentUILang].ai_suggestion_error_response || "Could not generate suggestions. Unexpected AI response structure.";
                    suggestionErrorMessage.classList.remove('hidden');
                }
            } catch (error) {
                console.error("Error fetching AI suggestion:", error);
                aiSuggestionOutput.value = translations[currentUILang].ai_suggestion_error_api || "An error occurred while connecting to AI. Please try again later.";
                suggestionErrorMessage.classList.remove('hidden');
            } finally {
                // Sembunyikan loading spinner dan aktifkan kembali tombol
                getSuggestionBtnText.textContent = translations[currentUILang].get_suggestion_button_text || 'Get Prompt Suggestion';
                suggestionSpinner.classList.add('hidden');
                getSuggestionBtn.disabled = false;
            }
        });
    </script>
</body>
</html>
