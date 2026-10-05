<!DOCTYPE html>
<html lang="id" class="h-full bg-slate-900 text-slate-100">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StoryToon AI - Generator Ilustrasi Kartun Naskah Cerita</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Fredoka:wght@500;600;700&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                        display: ['Fredoka', 'cursive'],
                    },
                    colors: {
                        brand: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                        },
                        cartoon: {
                            purple: '#8b5cf6',
                            pink: '#ec4899',
                            yellow: '#f59e0b',
                            blue: '#3b82f6',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        .glass-panel {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }
        .story-card-shadow {
            box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.5);
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #475569;
        }
    </style>
</head>
<body class="h-full flex flex-col font-sans antialiased selection:bg-purple-500 selection:text-white">

    <header class="border-b border-slate-800 bg-slate-950/80 backdrop-blur sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-purple-600 via-pink-500 to-amber-400 flex items-center justify-center text-white shadow-lg shadow-purple-500/20 font-display text-xl font-bold">
                    🎨
                </div>
                <div>
                    <h1 class="font-display text-xl font-bold bg-gradient-to-r from-purple-400 via-pink-300 to-amber-200 bg-clip-text text-transparent">
                        StoryToon AI
                    </h1>
                    <p class="text-xs text-slate-400 hidden sm:block">Ubah Naskah Cerita Menjadi Ilustrasi Kartun Bergambar</p>
                </div>
            </div>

            <div class="flex items-center gap-3">
                <span id="api-status-badge" class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-medium bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                    <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                    Gemini AI Ready
                </span>
                <button onclick="scrollToSection('story-input-section')" class="text-xs font-medium px-3 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-200 transition">
                    + Buat Baru
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 flex flex-col gap-8">

        <section id="story-input-section" class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- Left Side: Story Input Form -->
            <div class="lg:col-span-7 flex flex-col gap-4 glass-panel p-5 sm:p-6 rounded-2xl">
                <div class="flex items-center justify-between">
                    <label for="story-text" class="font-display text-lg font-semibold text-slate-100 flex items-center gap-2">
                        <i data-lucide="book-open" class="w-5 h-5 text-purple-400"></i>
                        Naskah Cerita
                    </label>
                    <div class="flex items-center gap-2">
                        <span class="text-xs text-slate-400">Contoh Cerita:</span>
                        <select id="preset-select" onchange="loadPresetStory(this.value)" class="text-xs bg-slate-800 border border-slate-700 text-purple-300 rounded-lg px-2.5 py-1 focus:outline-none focus:border-purple-500">
                            <option value="">-- Pilih Contoh --</option>
                            <option value="kancil">Kancil & Buaya Cerdas</option>
                            <option value="space">Astronaut Cilik di Planet Permen</option>
                            <option value="dragon">Naga Kecil Pengawal Hutan</option>
                        </select>
                    </div>
                </div>

                <div class="relative">
                    <textarea id="story-text" rows="7" 
                        placeholder="Tulis atau tempel naskah cerita Anda di sini... (Contoh: Pada suatu hari, Kancil yang cerdik berjalan di tepi sungai. Ia melihat buah-buahan segar di seberang sungai, namun sungai itu dipenuhi oleh buaya-buaya yang lapar...)" 
                        class="w-full bg-slate-900/90 border border-slate-700 rounded-xl p-4 text-sm text-slate-200 placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-purple-500/50 focus:border-purple-500 transition resize-y"></textarea>
                    <div class="absolute bottom-3 right-3 text-xs text-slate-500" id="char-count">0 karakter</div>
                </div>

                <!-- Customization Options -->
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 pt-2">
                    <div>
                        <label class="block text-xs font-semibold text-slate-300 mb-1.5 flex items-center gap-1.5">
                            <i data-lucide="palette" class="w-4 h-4 text-pink-400"></i>
                            Gaya Visual Kartun
                        </label>
                        <select id="art-style" class="w-full bg-slate-900 border border-slate-700 text-slate-200 rounded-xl px-3 py-2 text-sm focus:outline-none focus:border-purple-500">
                            <option value="3d-pixar">3D Film Animasi (Pixar / Disney Style)</option>
                            <option value="2d-storybook" selected>2D Buku Cerita Bergambar Classic</option>
                            <option value="anime-cute">Anime Fantasy / Cute Chibi Style</option>
                            <option value="comic-vibrant">Komik Warna Vibrant & Expressive</option>
                            <option value="watercolor">Cat Air (Soft Watercolor Cartoon)</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold text-slate-300 mb-1.5 flex items-center gap-1.5">
                            <i data-lucide="aspect-ratio" class="w-4 h-4 text-amber-400"></i>
                            Rasio Gambar
                        </label>
                        <select id="aspect-ratio" class="w-full bg-slate-900 border border-slate-700 text-slate-200 rounded-xl px-3 py-2 text-sm focus:outline-none focus:border-purple-500">
                            <option value="16:9" selected>Lansekap (16:9) - Layar & TV</option>
                            <option value="9:16">Potret (9:16) - Story & Mobile</option>
                            <option value="1:1">Persegi (1:1) - Feed Medsos</option>
                            <option value="4:3">Standar Buku (4:3)</option>
                        </select>
                    </div>
                </div>

                <!-- Action Button -->
                <div class="pt-3">
                    <button id="btn-generate-story" onclick="startStoryPipeline()" class="w-full py-3.5 px-6 rounded-xl bg-gradient-to-r from-purple-600 via-pink-600 to-amber-500 hover:from-purple-500 hover:via-pink-500 hover:to-amber-400 text-white font-display font-semibold text-base shadow-lg shadow-purple-600/30 hover:shadow-purple-600/50 transition-all transform active:scale-[0.99] flex items-center justify-center gap-2">
                        <i data-lucide="wand2" class="w-5 h-5"></i>
                        <span>Analisis Naskah & Hasilkan Kartun</span>
                    </button>
                </div>
            </div>

            <!-- Right Side: Info & Guide Card -->
            <div class="lg:col-span-5 flex flex-col justify-between glass-panel p-5 sm:p-6 rounded-2xl bg-slate-900/50">
                <div>
                    <h3 class="font-display text-base font-bold text-slate-100 flex items-center gap-2 mb-3">
                        <i data-lucide="sparkles" class="w-5 h-5 text-amber-400"></i>
                        Bagaimana Cara Kerjanya?
                    </h3>
                    <ul class="space-y-3 text-xs text-slate-300">
                        <li class="flex items-start gap-2.5">
                            <span class="w-5 h-5 rounded-full bg-purple-500/20 text-purple-400 font-bold flex items-center justify-center shrink-0 text-xs">1</span>
                            <span><strong>Pemisahan Adegan Otomatis:</strong> AI membaca naskah Anda dan membaginya secara cerdas menjadi beberapa babak/adegan penting.</span>
                        </li>
                        <li class="flex items-start gap-2.5">
                            <span class="w-5 h-5 rounded-full bg-pink-500/20 text-pink-400 font-bold flex items-center justify-center shrink-0 text-xs">2</span>
                            <span><strong>Ekstraksi Deskripsi Visual:</strong> AI merancang prompt visual khusus untuk menjaga alur cerita dan karakter tetap konsisten.</span>
                        </li>
                        <li class="flex items-start gap-2.5">
                            <span class="w-5 h-5 rounded-full bg-amber-500/20 text-amber-400 font-bold flex items-center justify-center shrink-0 text-xs">3</span>
                            <span><strong>Generasi Gambar AI:</strong> Model Gemini Flash Image membuat ilustrasi kartun berkualitas untuk tiap adegan.</span>
                        </li>
                    </ul>
                </div>

                <div class="mt-6 p-4 rounded-xl bg-purple-950/30 border border-purple-500/20 text-xs text-purple-200/80">
                    <div class="font-semibold text-purple-300 mb-1 flex items-center gap-1.5">
                        <i data-lucide="lightbulb" class="w-4 h-4 text-amber-300"></i> Tip Hasil Terbaik:
                    </div>
                    Tuliskan aksi atau deskripsi situasi yang jelas dalam cerita Anda. Semakin deskriptif naskah, semakin ekspresif adegan kartun yang dihasilkan!
                </div>
            </div>
        </section>

        <section id="progress-section" class="hidden glass-panel p-5 rounded-2xl border border-purple-500/30">
            <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-3">
                <div class="flex items-center gap-3">
                    <div class="p-2 rounded-lg bg-purple-500/20 text-purple-400">
                        <i data-lucide="loader-2" class="w-5 h-5 animate-spin" id="progress-spinner"></i>
                    </div>
                    <div>
                        <h4 class="font-display font-semibold text-sm text-slate-100" id="progress-title">Sedang Memproses Storyboard...</h4>
                        <p class="text-xs text-slate-400" id="progress-subtitle">Menganalisis adegan dari naskah cerita...</p>
                    </div>
                </div>
                <div class="text-xs font-semibold text-purple-300 shrink-0" id="progress-count">
                    0 / 0 Adegan
                </div>
            </div>
            
            <div class="w-full bg-slate-800 rounded-full h-2.5 overflow-hidden">
                <div id="progress-bar" class="bg-gradient-to-r from-purple-500 via-pink-500 to-amber-400 h-2.5 rounded-full transition-all duration-300 w-0"></div>
            </div>
        </section>

        <section id="storyboard-section" class="hidden flex flex-col gap-6">
            
            <!-- Controls and Header Bar -->
            <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 border-b border-slate-800 pb-4">
                <div>
                    <h2 class="font-display text-2xl font-bold text-slate-100 flex items-center gap-2">
                        <i data-lucide="clapperboard" class="w-6 h-6 text-purple-400"></i>
                        Hasil Storyboard Kartun
                    </h2>
                    <p class="text-xs text-slate-400" id="story-stats-text">Naskah berhasil dibagi menjadi beberapa adegan</p>
                </div>

                <div class="flex items-center gap-2 self-start sm:self-auto">
                    <!-- Tab Switcher -->
                    <div class="bg-slate-800 p-1 rounded-xl flex items-center gap-1 text-xs font-medium">
                        <button id="tab-grid-btn" onclick="switchViewMode('grid')" class="px-3 py-1.5 rounded-lg bg-purple-600 text-white shadow flex items-center gap-1.5">
                            <i data-lucide="layout-grid" class="w-4 h-4"></i> Grid Adegan
                        </button>
                        <button id="tab-storybook-btn" onclick="switchViewMode('storybook')" class="px-3 py-1.5 rounded-lg text-slate-400 hover:text-slate-200 flex items-center gap-1.5">
                            <i data-lucide="book" class="w-4 h-4"></i> Mode Buku
                        </button>
                    </div>

                    <button onclick="generateAllPendingImages()" id="btn-retry-all" class="px-3 py-1.5 text-xs font-medium bg-slate-800 hover:bg-slate-700 border border-slate-700 text-slate-200 rounded-xl flex items-center gap-1.5 transition">
                        <i data-lucide="refresh-cw" class="w-3.5 h-3.5"></i> Regenerasi Semua
                    </button>
                </div>
            </div>

            <!-- View 1: Grid Mode Adegan -->
            <div id="view-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Scene Cards Will Be Injected Here Dynamically -->
            </div>

            <!-- View 2: Storybook Presentation Mode -->
            <div id="view-storybook" class="hidden max-w-4xl mx-auto w-full flex flex-col gap-8 py-4">
                <!-- Storybook Cards Will Be Injected Here Dynamically -->
            </div>

        </section>

    </main>

    <!-- Image View Modal -->
    <div id="image-modal" class="fixed inset-0 bg-black/80 backdrop-blur-md z-50 hidden flex items-center justify-center p-4">
        <div class="relative max-w-5xl w-full max-h-[90vh] bg-slate-900 border border-slate-800 rounded-2xl overflow-hidden flex flex-col">
            <div class="flex items-center justify-between px-4 py-3 border-b border-slate-800 bg-slate-950">
                <h3 class="font-display font-semibold text-sm text-slate-200" id="modal-title">Detail Gambar Adegan</h3>
                <button onclick="closeModal()" class="p-1 rounded-lg text-slate-400 hover:text-white hover:bg-slate-800">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <div class="flex-1 overflow-auto p-4 flex items-center justify-center bg-black/40">
                <img id="modal-img" src="" alt="Detail Adegan" class="max-h-[70vh] object-contain rounded-xl shadow-2xl">
            </div>
            <div class="p-4 border-t border-slate-800 bg-slate-950 flex justify-between items-center">
                <p id="modal-snippet" class="text-xs text-slate-300 italic max-w-xl truncate"></p>
                <a id="modal-download-btn" href="#" download="adegan-kartun.png" class="px-4 py-2 bg-purple-600 hover:bg-purple-500 text-white rounded-xl text-xs font-semibold flex items-center gap-1.5 transition">
                    <i data-lucide="download" class="w-4 h-4"></i> Unduh Gambar
                </a>
            </div>
        </div>
    </div>

    <!-- Notification Toast -->
    <div id="toast" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 max-w-md bg-slate-800 border border-purple-500/40 text-slate-100 px-4 py-3 rounded-xl shadow-2xl flex items-center gap-3">
        <div id="toast-icon" class="text-purple-400 shrink-0">
            <i data-lucide="info" class="w-5 h-5"></i>
        </div>
        <div id="toast-message" class="text-xs font-medium">Notifikasi</div>
    </div>

    <script>
        // Preset Stories Sample Data
        const PRESET_STORIES = {
            kancil: `Pada suatu hari yang cerah, Kancil yang cerdik berjalan-jalan di pinggir hutan dekat sungai yang deras. Ia merasa sangat lapar dan melihat kebun buah-buahan yang ranum di seberang sungai.
Namun, sungai itu ditunggui oleh kawanan buaya besar yang siap menerkam siapapun.
Kancil tidak kehilangan akal. Ia berdiri di tepi sungai lalu berteriak memanggil Sang Buaya Pemimpin, "Hai Buaya! Raja Hutan menyuruhku menghitung jumlah kalian untuk diberi hadiah daging lezat!"
Buaya yang tergiur langsung memanggil seluruh kawanannya untuk berbaris rapi dari tepi sungai ke seberang.
Dengan santai dan gembira, Kancil melompati punggung buaya satu per satu sambil berhitung "Satu, dua, tiga!".
Setelah sampai di seberang sungai dengan selamat, Kancil tertawa riang dan memakan buah-buahan yang manis, meninggalkan buaya yang baru sadar telah terpedaya.`,
            
            space: `Dino adalah seorang anak laki-laki berusia 8 tahun yang bercita-cita menjadi astronaut hebat. Suatu malam, roket mainannya tiba-tiba membesar dan menyala di dalam kamarnya!
Dino mengenakan helm astronautnya dan terbang menembus awan malam menuju luar angkasa.
Ia mendarat di sebuah planet ajaib berwarna merah muda yang seluruh daratannya terbuat dari gula-gula dan pohon permen kapas.
Di sana, Dino bertemu dengan makhluk alien kecil yang lucu berbentuk seperti beruang permen gummy berwarna hijau.
Mereka bermain melompat di atas trampoline jeli dan makan es krim raksasa bersama di bawah langit bertabur bintang warna-warni.`,
            
            dragon: `Di dalam Hutan Kristal yang rimbun, hiduplah seekor naga kecil bernama Pip yang sisiknya bersinar keemasan. Tidak seperti naga lain yang menyemburkan api, Pip hanya bisa menyemburkan gelembung sabun berwarna-warni.
Suatu sore, seekor serigala hitam besar mencoba mengganggu kawanan kelinci kecil di tepi danau.
Pip dengan berani terbang menghadang serigala tersebut dan menyemburkan ribuan gelembung sabun raksasa.
Gelembung-gelembung tersebut mengurung sang serigala sampai terangkat melayang-layang di udara dengan lucunya.
Para hewan hutan bersorak gembira dan merayakan keberanian naga kecil Pip yang menjadi pahlawan hutan.`
        };

        // Application State
        let scenesData = [];
        let isProcessing = false;

        // Initialize Lucide Icons
        document.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            
            const storyTextarea = document.getElementById('story-text');
            storyTextarea.addEventListener('input', () => {
                document.getElementById('char-count').textContent = `${storyTextarea.value.length} karakter`;
            });
        });

        // Helper to load sample story
        function loadPresetStory(key) {
            if (key && PRESET_STORIES[key]) {
                const textarea = document.getElementById('story-text');
                textarea.value = PRESET_STORIES[key];
                document.getElementById('char-count').textContent = `${textarea.value.length} karakter`;
                showToast("Contoh naskah berhasil dimuat!", "success");
            }
        }

        function scrollToSection(id) {
            document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
        }

        // Toast notification system
        function showToast(msg, type = "info") {
            const toast = document.getElementById('toast');
            const msgEl = document.getElementById('toast-message');
            const iconEl = document.getElementById('toast-icon');

            msgEl.textContent = msg;
            
            if (type === "error") {
                iconEl.innerHTML = `<i data-lucide="alert-circle" class="w-5 h-5 text-red-400"></i>`;
            } else if (type === "success") {
                iconEl.innerHTML = `<i data-lucide="check-circle-2" class="w-5 h-5 text-emerald-400"></i>`;
            } else {
                iconEl.innerHTML = `<i data-lucide="info" class="w-5 h-5 text-purple-400"></i>`;
            }
            lucide.createIcons();

            toast.classList.remove('translate-y-20', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');

            setTimeout(() => {
                toast.classList.remove('translate-y-0', 'opacity-100');
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3500);
        }

        // Step 1: Analyze Story & Breakdown Scenes via Gemini Flash Text API
        async function startStoryPipeline() {
            const storyText = document.getElementById('story-text').value.trim();
            const artStyle = document.getElementById('art-style').value;
            const aspectRatio = document.getElementById('aspect-ratio').value;

            if (!storyText) {
                showToast("Silakan masukkan naskah cerita terlebih dahulu!", "error");
                return;
            }

            if (isProcessing) return;
            isProcessing = true;

            // UI Progress updates
            document.getElementById('progress-section').classList.remove('hidden');
            document.getElementById('storyboard-section').classList.add('hidden');
            updateProgress(10, "Menganalisis Naskah Cerita...", "Membagi cerita menjadi adegan visual logis...");
            
            const btn = document.getElementById('btn-generate-story');
            btn.disabled = true;
            btn.classList.add('opacity-50', 'cursor-not-allowed');

            try {
                // Call Gemini 3 Flash to split story into structured scenes JSON
                const scenes = await analyzeStoryTextWithGemini(storyText, artStyle);
                
                if (!scenes || scenes.length === 0) {
                    throw new Error("Gagal mengekstrak adegan dari naskah.");
                }

                scenesData = scenes.map((s, idx) => ({
                    id: idx + 1,
                    snippet: s.textSnippet,
                    visualDescription: s.visualDescription,
                    prompt: s.imagePrompt,
                    status: 'pending', // 'pending', 'generating', 'completed', 'error'
                    imageUrl: null,
                    errorMsg: null
                }));

                updateProgress(30, "Adegan Berhasil Diproses", `Ditemukan ${scenesData.length} adegan. Mempersiapkan visual...`);
                
                // Display initial scene skeletons in grid and storybook
                renderScenesUI();
                document.getElementById('storyboard-section').classList.remove('hidden');
                scrollToSection('storyboard-section');

                // Step 2: Generate Image for Each Scene sequentially
                await generateAllSceneImages(artStyle, aspectRatio);

            } catch (err) {
                console.error("Story Pipeline Error:", err);
                showToast("Terjadi kesalahan: " + (err.message || "Gagal memproses cerita"), "error");
                updateProgress(100, "Pemrosesan Terhenti", "Terjadi kesalahan saat memproses.", true);
            } finally {
                isProcessing = false;
                btn.disabled = false;
                btn.classList.remove('opacity-50', 'cursor-not-allowed');
            }
        }

        async function analyzeStoryTextWithGemini(story, artStyle) {
            const apiKey = ""; // Canvas runtime environment automatically replaces empty key
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;

            const systemInstruction = `Kamu adalah seorang sutradara storyboard animasi profesional dan illustrator kartun. 
Tugasmu adalah menganalisis naskah cerita Bahasa Indonesia yang diberikan dan membaginya menjadi beberapa adegan visual utama (jumlah adegan menyesuaikan panjang naskah secara proposional, biasanya antara 3 hingga 7 adegan).

Untuk setiap adegan, kamu harus membuat:
1. textSnippet: Potongan kalimat/paragraf asli dari naskah yang menggambarkan adegan tersebut.
2. visualDescription: Deskripsi visual adegan secara detail dalam bahasa Indonesia (siapa karakternya, ekspresi wajah, lingkungan/latar tempat, aksi yang dilakukan).
3. imagePrompt: Prompt visual bahasa Inggris yang dioptimalkan untuk pembuatan gambar AI kartun, mencakup detail karakter, aksi, latar belakang, serta konsistensi visual.`;

            const prompt = `Analisis naskah cerita berikut dan ekstrak menjadi adegan-adegan kartun bergambar:
            
NASKAH CERITA:
"""
${story}
"""`;

            const payload = {
                contents: [{ parts: [{ text: prompt }] }],
                systemInstruction: { parts: [{ text: systemInstruction }] },
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "ARRAY",
                        items: {
                            type: "OBJECT",
                            properties: {
                                "sceneNumber": { "type": "INTEGER" },
                                "textSnippet": { "type": "STRING" },
                                "visualDescription": { "type": "STRING" },
                                "imagePrompt": { "type": "STRING" }
                            },
                            "propertyOrdering": ["sceneNumber", "textSnippet", "visualDescription", "imagePrompt"]
                        }
                    }
                }
            };

            // Fetch with exponential backoff retry logic
            let response = null;
            let retries = 3;
            let delay = 1000;

            while (retries > 0) {
                try {
                    response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    if (response.ok) break;
                } catch (e) {
                    console.warn(`Retry API text analysis (${retries} left)...`);
                }
                retries--;
                if (retries > 0) {
                    await new Promise(r => setTimeout(r, delay));
                    delay *= 2;
                }
            }

            if (!response || !response.ok) {
                throw new Error("Gagal berkomunikasi dengan Gemini AI Text API.");
            }

            const data = await response.json();
            const textContent = data.candidates?.[0]?.content?.parts?.[0]?.text;
            
            if (!textContent) {
                throw new Error("Format respon AI tidak valid.");
            }

            return JSON.parse(textContent);
        }

        async function generateAllSceneImages(artStyle, aspectRatio) {
            const total = scenesData.length;
            
            for (let i = 0; i < total; i++) {
                const scene = scenesData[i];
                scene.status = 'generating';
                renderSingleSceneUI(scene.id);

                const currentProgress = 30 + Math.round(((i) / total) * 70);
                updateProgress(
                    currentProgress, 
                    `Membentuk Gambar Adegan ${i + 1} dari ${total}...`, 
                    `Gaya: ${getStyleLabel(artStyle)}`
                );

                try {
                    const imgUrl = await fetchCartoonImage(scene.prompt, artStyle, aspectRatio);
                    scene.imageUrl = imgUrl;
                    scene.status = 'completed';
                } catch (err) {
                    console.error(`Error generating image for scene ${scene.id}:`, err);
                    scene.status = 'error';
                    scene.errorMsg = err.message || "Gagal memuat gambar";
                }

                renderSingleSceneUI(scene.id);
            }

            updateProgress(100, "Selesai!", `Semua ${total} ilustrasi kartun berhasil dibuat!`, false);
            setTimeout(() => {
                document.getElementById('progress-section').classList.add('hidden');
            }, 3000);
            showToast("Semua gambar kartun selesai dibuat!", "success");
        }

        // Single Scene Image Generation
        async function fetchCartoonImage(basePrompt, styleKey, aspectRatio) {
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3.1-flash-image:generateContent?key=${apiKey}`;

            // Style prompt modifiers
            const styleModifiers = {
                '3d-pixar': '3D Pixar and Disney animated movie style, vibrant rich lighting, expressive cute character design, highly detailed 3D render, digital artwork',
                '2d-storybook': 'Charming 2D children storybook illustration style, clean line art, warm soft colors, enchanting fairy tale aesthetic',
                'anime-cute': 'Cute anime fantasy art style, vibrant colors, expressive eyes, smooth cell shading, anime storybook art',
                'comic-vibrant': 'Vibrant colorful comic book illustration, dynamic comic panel art, expressive lines, bold color palette',
                'watercolor': 'Soft pastel watercolor storybook cartoon illustration, gentle artistic brush strokes, magical dreamlike aesthetic'
            };

            const selectedStyle = styleModifiers[styleKey] || styleModifiers['2d-storybook'];
            const fullPrompt = `${selectedStyle}. Scene: ${basePrompt}. High quality, vibrant, cute cartoon style, storybook illustration, no ugly faces, clean art.`;

            const payload = {
                contents: [
                    {
                        role: 'user',
                        parts: [{ text: fullPrompt }]
                    }
                ],
                generationConfig: {
                    responseModalities: ['IMAGE'],
                    imageConfig: {
                        aspectRatio: aspectRatio || "16:9"
                    }
                }
            };

            // Fetch with retry logic
            let response = null;
            let retries = 3;
            let delay = 1500;

            while (retries > 0) {
                try {
                    response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    if (response.ok) break;
                } catch (e) {
                    console.warn(`Retry Image Generation (${retries} left)...`);
                }
                retries--;
                if (retries > 0) {
                    await new Promise(r => setTimeout(r, delay));
                    delay *= 2;
                }
            }

            if (!response || !response.ok) {
                throw new Error("Gagal membuat gambar dari Gemini Image Model.");
            }

            const result = await response.json();
            const base64Data = result?.candidates?.[0]?.content?.parts?.find(p => p.inlineData)?.inlineData?.data;

            if (!base64Data) {
                throw new Error("Model tidak mengembalikan data gambar.");
            }

            return `data:image/png;base64,${base64Data}`;
        }

        // Regenerate single image
        async function regenerateSingleScene(sceneId) {
            const scene = scenesData.find(s => s.id === sceneId);
            if (!scene) return;

            const artStyle = document.getElementById('art-style').value;
            const aspectRatio = document.getElementById('aspect-ratio').value;

            scene.status = 'generating';
            renderSingleSceneUI(sceneId);

            showToast(`Membuat ulang gambar Adegan #${sceneId}...`, "info");

            try {
                const imgUrl = await fetchCartoonImage(scene.prompt, artStyle, aspectRatio);
                scene.imageUrl = imgUrl;
                scene.status = 'completed';
                showToast(`Gambar Adegan #${sceneId} selesai dibuat ulang!`, "success");
            } catch (err) {
                scene.status = 'error';
                scene.errorMsg = err.message || "Gagal memuat ulang gambar";
                showToast(`Gagal membuat ulang Adegan #${sceneId}`, "error");
            }

            renderSingleSceneUI(sceneId);
        }

        // Update visual prompt for scene
        function updateScenePrompt(sceneId, newPrompt) {
            const scene = scenesData.find(s => s.id === sceneId);
            if (scene) {
                scene.prompt = newPrompt;
            }
        }

        function renderScenesUI() {
            const gridContainer = document.getElementById('view-grid');
            const storybookContainer = document.getElementById('view-storybook');

            gridContainer.innerHTML = '';
            storybookContainer.innerHTML = '';

            document.getElementById('story-stats-text').textContent = 
                `Naskah berhasil dibagi menjadi ${scenesData.length} adegan visual.`;

            scenesData.forEach((scene) => {
                // Grid element card container
                const gridCard = document.createElement('div');
                gridCard.id = `grid-card-${scene.id}`;
                gridCard.className = "glass-panel rounded-2xl overflow-hidden flex flex-col border border-slate-800 transition hover:border-slate-700";
                gridContainer.appendChild(gridCard);

                // Storybook element card container
                const storyCard = document.createElement('div');
                storyCard.id = `story-card-${scene.id}`;
                storyCard.className = "glass-panel p-6 rounded-2xl flex flex-col md:flex-row gap-6 items-center story-card-shadow";
                storybookContainer.appendChild(storyCard);

                renderSingleSceneUI(scene.id);
            });

            lucide.createIcons();
        }

        function renderSingleSceneUI(sceneId) {
            const scene = scenesData.find(s => s.id === sceneId);
            if (!scene) return;

            const gridCard = document.getElementById(`grid-card-${scene.id}`);
            const storyCard = document.getElementById(`story-card-${scene.id}`);

            if (gridCard) {
                gridCard.innerHTML = `
                    <div class="p-3.5 bg-slate-950/80 border-b border-slate-800 flex items-center justify-between">
                        <span class="font-display font-bold text-xs px-2.5 py-1 rounded-full bg-purple-500/20 text-purple-300 border border-purple-500/30">
                            Adegan #${scene.id}
                        </span>
                        <div class="flex items-center gap-1">
                            ${scene.status === 'completed' ? `
                                <a href="${scene.imageUrl}" download="storytoon-adegan-${scene.id}.png" class="p-1.5 rounded-lg hover:bg-slate-800 text-slate-400 hover:text-emerald-400 transition" title="Unduh Gambar">
                                    <i data-lucide="download" class="w-4 h-4"></i>
                                </a>
                                <button onclick="openModal('${scene.imageUrl}', '${escapeHtml(scene.snippet)}', ${scene.id})" class="p-1.5 rounded-lg hover:bg-slate-800 text-slate-400 hover:text-white transition" title="Perbesar">
                                    <i data-lucide="maximize-2" class="w-4 h-4"></i>
                                </button>
                            ` : ''}
                            <button onclick="regenerateSingleScene(${scene.id})" ${scene.status === 'generating' ? 'disabled' : ''} class="p-1.5 rounded-lg hover:bg-slate-800 text-slate-400 hover:text-purple-300 transition" title="Buat Ulang Gambar">
                                <i data-lucide="refresh-cw" class="w-4 h-4 ${scene.status === 'generating' ? 'animate-spin text-amber-400' : ''}"></i>
                            </button>
                        </div>
                    </div>

                    <!-- Image Display Area -->
                    <div class="relative w-full aspect-video bg-slate-950/90 flex items-center justify-center overflow-hidden border-b border-slate-800/80">
                        ${renderSceneImageContent(scene)}
                    </div>

                    <!-- Text & Prompt Box -->
                    <div class="p-4 flex flex-col gap-3 flex-1 justify-between bg-slate-900/40">
                        <div>
                            <p class="text-xs text-slate-200 font-medium leading-relaxed italic border-l-2 border-purple-500 pl-2.5 py-0.5">
                                "${escapeHtml(scene.snippet)}"
                            </p>
                        </div>

                        <!-- Editable Prompt Accordion -->
                        <details class="text-xs group">
                            <summary class="cursor-pointer text-slate-400 hover:text-purple-300 flex items-center gap-1 font-medium select-none py-1">
                                <i data-lucide="sparkles" class="w-3.5 h-3.5 text-amber-400"></i>
                                <span>Edit Prompt AI Visual</span>
                                <i data-lucide="chevron-down" class="w-3.5 h-3.5 ml-auto transition-transform group-open:rotate-180"></i>
                            </summary>
                            <div class="pt-2">
                                <textarea onchange="updateScenePrompt(${scene.id}, this.value)" rows="2" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-xs text-slate-300 focus:outline-none focus:border-purple-500">${escapeHtml(scene.prompt)}</textarea>
                            </div>
                        </details>
                    </div>
                `;
            }

            if (storyCard) {
                const isEven = scene.id % 2 === 0;
                storyCard.innerHTML = `
                    <div class="w-full md:w-1/2 aspect-video rounded-xl overflow-hidden bg-slate-950 flex items-center justify-center border border-slate-800 shadow-lg ${isEven ? 'md:order-2' : 'md:order-1'} relative group">
                        ${renderSceneImageContent(scene, false)}
                    </div>
                    <div class="w-full md:w-1/2 flex flex-col gap-3 ${isEven ? 'md:order-1' : 'md:order-2'}">
                        <div class="flex items-center justify-between">
                            <span class="font-display text-xs font-bold px-3 py-1 rounded-full bg-pink-500/10 text-pink-400 border border-pink-500/20 w-max">
                                Bagian ${scene.id}
                            </span>
                            ${scene.status === 'completed' ? `
                                <a href="${scene.imageUrl}" download="storytoon-adegan-${scene.id}.png" class="inline-flex items-center gap-1.5 px-3 py-1.5 bg-emerald-500/10 hover:bg-emerald-500/20 text-emerald-400 border border-emerald-500/30 rounded-lg text-xs font-medium transition">
                                    <i data-lucide="download" class="w-3.5 h-3.5"></i> Unduh Gambar
                                </a>
                            ` : ''}
                        </div>
                        <p class="text-sm text-slate-100 leading-relaxed font-sans font-medium">
                            "${escapeHtml(scene.snippet)}"
                        </p>
                        <p class="text-xs text-slate-400 bg-slate-950/50 p-3 rounded-xl border border-slate-800">
                            <strong>Visual:</strong> ${escapeHtml(scene.visualDescription)}
                        </p>
                    </div>
                `;
            }

            lucide.createIcons();
        }

        function renderSceneImageContent(scene, showActions = true) {
            if (scene.status === 'generating') {
                return `
                    <div class="flex flex-col items-center gap-2 p-4 text-center">
                        <div class="w-8 h-8 border-3 border-purple-500 border-t-transparent rounded-full animate-spin"></div>
                        <span class="text-xs font-medium text-purple-300">Membuat Ilustrasi Kartun...</span>
                    </div>
                `;
            }

            if (scene.status === 'error') {
                return `
                    <div class="flex flex-col items-center gap-2 p-4 text-center">
                        <i data-lucide="alert-triangle" class="w-8 h-8 text-amber-400"></i>
                        <span class="text-xs text-slate-300">${escapeHtml(scene.errorMsg || "Gagal memuat")}</span>
                        <button onclick="regenerateSingleScene(${scene.id})" class="mt-1 px-3 py-1 bg-purple-600 hover:bg-purple-500 text-white text-xs rounded-lg font-medium transition">
                            Coba Lagi
                        </button>
                    </div>
                `;
            }

            if (scene.status === 'completed' && scene.imageUrl) {
                return `
                    <img src="${scene.imageUrl}" alt="Adegan ${scene.id}" class="w-full h-full object-cover transition-transform duration-500 hover:scale-105">
                `;
            }

            return `
                <div class="flex flex-col items-center gap-1 text-slate-500">
                    <i data-lucide="image" class="w-8 h-8"></i>
                    <span class="text-xs">Menunggu Giliran...</span>
                </div>
            `;
        }

        // Generate All Pending/Retry Images
        async function generateAllPendingImages() {
            const artStyle = document.getElementById('art-style').value;
            const aspectRatio = document.getElementById('aspect-ratio').value;
            
            document.getElementById('progress-section').classList.remove('hidden');
            await generateAllSceneImages(artStyle, aspectRatio);
        }

        function switchViewMode(mode) {
            const gridBtn = document.getElementById('tab-grid-btn');
            const storybookBtn = document.getElementById('tab-storybook-btn');
            const gridView = document.getElementById('view-grid');
            const storybookView = document.getElementById('view-storybook');

            if (mode === 'grid') {
                gridView.classList.remove('hidden');
                storybookView.classList.add('hidden');
                gridBtn.className = "px-3 py-1.5 rounded-lg bg-purple-600 text-white shadow flex items-center gap-1.5";
                storybookBtn.className = "px-3 py-1.5 rounded-lg text-slate-400 hover:text-slate-200 flex items-center gap-1.5";
            } else {
                gridView.classList.add('hidden');
                storybookView.classList.remove('hidden');
                storybookBtn.className = "px-3 py-1.5 rounded-lg bg-purple-600 text-white shadow flex items-center gap-1.5";
                gridBtn.className = "px-3 py-1.5 rounded-lg text-slate-400 hover:text-slate-200 flex items-center gap-1.5";
            }
        }

        function updateProgress(percent, title, subtitle, isError = false) {
            const bar = document.getElementById('progress-bar');
            const titleEl = document.getElementById('progress-title');
            const subEl = document.getElementById('progress-subtitle');
            const countEl = document.getElementById('progress-count');
            const spinner = document.getElementById('progress-spinner');

            bar.style.width = `${percent}%`;
            if (title) titleEl.textContent = title;
            if (subtitle) subEl.textContent = subtitle;
            
            const completedCount = scenesData.filter(s => s.status === 'completed').length;
            countEl.textContent = `${completedCount} / ${scenesData.length} Adegan`;

            if (isError) {
                spinner.classList.remove('animate-spin');
            }
        }

        function getStyleLabel(key) {
            const labels = {
                '3d-pixar': '3D Pixar/Disney Movie',
                '2d-storybook': '2D Classic Storybook',
                'anime-cute': 'Anime / Chibi Style',
                'comic-vibrant': 'Komik Warna',
                'watercolor': 'Cat Air Halus'
            };
            return labels[key] || 'Kartun';
        }

        // Modal Handlers
        function openModal(imgUrl, snippet, sceneNum) {
            document.getElementById('modal-img').src = imgUrl;
            document.getElementById('modal-title').textContent = `Ilustrasi Adegan #${sceneNum}`;
            document.getElementById('modal-snippet').textContent = `"${snippet}"`;
            document.getElementById('modal-download-btn').href = imgUrl;
            document.getElementById('modal-download-btn').download = `storytoon-adegan-${sceneNum}.png`;
            document.getElementById('image-modal').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('image-modal').classList.add('hidden');
        }

        function escapeHtml(str) {
            if (!str) return '';
            return str
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
