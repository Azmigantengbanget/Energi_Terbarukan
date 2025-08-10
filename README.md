<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [



  { "en": "Apa itu energi terbarukan?", "id": "Energi dari sumber daya alam." },
  { "en": "Apa nama lainnya?", "id": "Energi bersih atau energi hijau." },
  { "en": "Mengapa disebut terbarukan?", "id": "Karena sumbernya tidak akan habis." },
  { "en": "Lawan dari energi terbarukan?", "id": "Energi fosil (tak terbarukan)." },
  { "en": "Contoh energi fosil?", "id": "Batu bara, minyak bumi, gas alam." },
  { "en": "Sumber utama energi terbarukan?", "id": "Matahari, angin, air, panas bumi." },
  { "en": "Energi dari matahari disebut?", "id": "Energi surya atau tenaga surya." },
  { "en": "Energi dari angin disebut?", "id": "Energi angin atau tenaga bayu." },
  { "en": "Energi dari air disebut?", "id": "Energi air atau hidroelektrik." },
  { "en": "Energi dari panas bumi?", "id": "Energi panas bumi atau geotermal." },
  { "en": "Energi dari tumbuhan disebut?", "id": "Energi biomassa." },
  { "en": "Keuntungan utama energi terbarukan?", "id": "Ramah lingkungan dan berkelanjutan." },
  { "en": "Apa itu 'jejak karbon'?", "id": "Total emisi gas rumah kaca." },
  { "en": "Energi terbarukan mengurangi apa?", "id": "Jejak karbon dan polusi udara." },
  { "en": "Alat untuk menangkap energi surya?", "id": "Panel surya atau panel fotovoltaik." },
  { "en": "Apa itu 'fotovoltaik' (PV)?", "id": "Mengubah cahaya matahari menjadi listrik." },
  { "en": "Sel surya terbuat dari?", "id": "Bahan semikonduktor seperti silikon." },
  { "en": "Apa itu 'solar farm'?", "id": "Ladang panel surya skala besar." },
  { "en": "Apa itu 'PLTS'?", "id": "Pembangkit Listrik Tenaga Surya." },
  { "en": "Panel surya di atap?", "id": "PLTS Atap." },
  { "en": "Alat untuk menangkap energi angin?", "id": "Turbin angin atau kincir angin." },
  { "en": "Apa itu 'wind farm'?", "id": "Ladang turbin angin skala besar." },
  { "en": "Apa itu 'PLTB'?", "id": "Pembangkit Listrik Tenaga Bayu." },
  { "en": "Energi angin lebih kuat di?", "id": "Lepas pantai (offshore)." },
  { "en": "Apa itu 'PLTA'?", "id": "Pembangkit Listrik Tenaga Air." },
  { "en": "Bagaimana PLTA bekerja?", "id": "Menggunakan aliran air memutar turbin." },
  { "en": "PLTA membutuhkan apa?", "id": "Bendungan dan waduk." },
  { "en": "Apa itu 'mikrohidro'?", "id": "PLTA skala kecil di sungai." },
  { "en": "Apa itu 'PLTP'?", "id": "Pembangkit Listrik Tenaga Panas Bumi." },
  { "en": "Bagaimana PLTP bekerja?", "id": "Menggunakan uap panas dari bumi." },
  { "en": "Indonesia punya potensi PLTP?", "id": "Ya, sangat besar." },
  { "en": "Mengapa?", "id": "Karena terletak di Cincin Api." },
  { "en": "Apa itu biomassa?", "id": "Bahan organik dari tumbuhan/hewan." },
  { "en": "Contoh biomassa?", "id": "Kayu, limbah pertanian, kotoran ternak." },
  { "en": "Apa itu 'biofuel'?", "id": "Bahan bakar dari sumber biomassa." },
  { "en": "Contoh biofuel?", "id": "Bioetanol dan biodiesel." },
  { "en": "Bioetanol terbuat dari?", "id": "Tebu, jagung, singkong." },
  { "en": "Biodiesel terbuat dari?", "id": "Minyak kelapa sawit, jarak." },
  { "en": "Apa itu 'biogas'?", "id": "Gas metana dari limbah organik." },
  { "en": "Apa itu 'pembangkit listrik biomassa'?", "id": "Membakar biomassa untuk menghasilkan listrik." },
  { "en": "Apa itu energi samudra?", "id": "Energi dari laut." },
  { "en": "Contoh energi samudra?", "id": "Gelombang, pasang surut, arus laut." },
  { "en": "Energi dari pasang surut?", "id": "Energi tidal." },
  { "en": "Energi dari gelombang laut?", "id": "Energi ombak (wave energy)." },
  { "en": "Tantangan utama energi terbarukan?", "id": "Sifatnya intermiten atau tidak stabil." },
  { "en": "Apa itu 'intermiten'?", "id": "Tidak tersedia setiap saat." },
  { "en": "Contohnya?", "id": "Matahari tidak bersinar di malam hari." },
  { "en": "Solusi untuk intermitensi?", "id": "Sistem penyimpanan energi." },
  { "en": "Contoh sistem penyimpanan?", "id": "Baterai, pumped-storage hydropower." },
  { "en": "Apa itu 'baterai skala besar'?", "id": "Menyimpan energi untuk jaringan listrik." },
  { "en": "Apa itu 'transisi energi'?", "id": "Pergeseran dari energi fosil ke terbarukan." },
  { "en": "Apa itu 'net zero emission'?", "id": "Keseimbangan emisi yang dihasilkan dan diserap." },
  { "en": "Apa itu 'smart grid'?", "id": "Jaringan listrik pintar dan modern." },
  { "en": "Fungsi smart grid?", "id": "Mengelola energi terbarukan secara efisien." },
  { "en": "Apa itu 'inverter'?", "id": "Mengubah arus DC menjadi AC." },
  { "en": "Panel surya menghasilkan arus?", "id": "Arus searah (DC)." },
  { "en": "Listrik di rumah menggunakan?", "id": "Arus bolak-balik (AC)." },
  { "en": "Apa itu 'elektronika daya'?", "id": "Bidang rekayasa konversi energi listrik." },
  { "en": "Apa itu 'BMS'?", "id": "Battery Management System." },
  { "en": "Fungsi BMS?", "id": "Melindungi dan mengoptimalkan kinerja baterai." },
  { "en": "Apa itu 'efisiensi energi'?", "id": "Menggunakan lebih sedikit energi untuk hasil sama." },
  { "en": "Apa itu 'konservasi energi'?", "id": "Mengurangi penggunaan energi." },
  { "en": "Keduanya penting untuk transisi?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'kendaraan listrik' (EV)?", "id": "Mobil yang menggunakan motor listrik." },
  { "en": "EV bagian dari transisi energi?", "id": "Ya, di sektor transportasi." },
  { "en": "Apa itu 'hidrogen hijau'?", "id": "Hidrogen dari elektrolisis air." },
  { "en": "Energi elektrolisis dari mana?", "id": "Dari sumber energi terbarukan." },
  { "en": "Hidrogen bisa jadi bahan bakar?", "id": "Ya, untuk sel bahan bakar." },
  { "en": "Apa itu 'fuel cell'?", "id": "Mengubah hidrogen menjadi listrik." },
  { "en": "Emisinya?", "id": "Hanya uap air." },
  { "en": "Apa itu 'carbon capture'?", "id": "Teknologi menangkap emisi karbon." },
  { "en": "Apa itu 'perubahan iklim'?", "id": "Perubahan pola cuaca jangka panjang." },
  { "en": "Penyebab utamanya?", "id": "Emisi gas rumah kaca." },
  { "en": "Gas rumah kaca utama?", "id": "Karbon dioksida (CO2)." },
  { "en": "Energi terbarukan membantu mengatasi?", "id": "Perubahan iklim." },
  { "en": "Apa itu 'Pemanasan Global'?", "id": "Peningkatan suhu rata-rata bumi." },
  { "en": "Apa itu 'Perjanjian Paris'?", "id": "Kesepakatan global mengatasi perubahan iklim." },
  { "en": "Tujuannya?", "id": "Membatasi pemanasan global." },
  { "en": "Potensi energi surya di Indonesia?", "id": "Sangat besar karena di khatulistiwa." },
  { "en": "Potensi energi angin di Indonesia?", "id": "Baik di beberapa wilayah pesisir." },
  { "en": "Apa itu 'feed-in tariff'?", "id": "Insentif harga untuk energi terbarukan." },
  { "en": "Apa itu 'net metering'?", "id": "Menjual kelebihan listrik ke PLN." },
  { "en": "Berlaku untuk PLTS Atap?", "id": "Ya." },
  { "en": "Apa itu 'energi nuklir'?", "id": "Energi dari reaksi nuklir." },
  { "en": "Nuklir termasuk energi bersih?", "id": "Ya, karena tidak menghasilkan CO2." },
  { "en": "Nuklir termasuk energi terbarukan?", "id": "Bukan, karena uranium terbatas." },
  { "en": "Masalah utama energi nuklir?", "id": "Limbah radioaktif dan keselamatan." },
  { "en": "Kebijakan energi nasional disebut?", "id": "KEN (Kebijakan Energi Nasional)." },
  { "en": "Bauran energi terbarukan Indonesia?", "id": "Ditargetkan 23% pada 2025." },
  { "en": "Apa itu 'geopolitik energi'?", "id": "Hubungan politik terkait sumber daya energi." },
  { "en": "Energi terbarukan mengubah geopolitik?", "id": "Ya, mengurangi ketergantungan pada fosil." },
  { "en": "Apa itu 'kincir air'?", "id": "Bentuk awal dari turbin air." },
  { "en": "Tantangan PLTS di malam hari?", "id": "Tidak menghasilkan listrik." },
  { "en": "Dampak bendungan PLTA?", "id": "Perubahan ekosistem, relokasi penduduk." },
  { "en": "Apa itu 'CSP'?", "id": "Concentrated Solar Power." },
  { "en": "Bagaimana CSP bekerja?", "id": "Memfokuskan cahaya matahari dengan cermin." },
  { "en": "Panasnya digunakan untuk apa?", "id": "Mendidihkan air, memutar turbin uap." },
  { "en": "Berbeda dengan panel PV?", "id": "Ya, CSP termal, PV elektrik." },
  { "en": "Keuntungan CSP?", "id": "Bisa menyimpan energi panas." },
  { "en": "Komponen utama turbin angin?", "id": "Bilah (blade), nacelle, menara." },
  { "en": "Apa isi 'nacelle'?", "id": "Gearbox, generator, dan sistem kontrol." },
  { "en": "Fungsi bilah turbin?", "id": "Menangkap energi kinetik angin." },
  { "en": "Berapa jumlah bilah umumnya?", "id": "Tiga bilah." },
  { "en": "Apa itu 'yaw control'?", "id": "Mengatur arah turbin menghadap angin." },
  { "en": "Apa itu 'pitch control'?", "id": "Mengatur sudut kemiringan bilah." },
  { "en": "Tujuannya?", "id": "Mengontrol kecepatan putaran dan daya." },
  { "en": "Apa itu 'cut-in speed'?", "id": "Kecepatan angin minimum untuk beroperasi." },
  { "en": "Apa itu 'cut-out speed'?", "id": "Kecepatan angin maksimum sebelum berhenti." },
  { "en": "Mengapa harus berhenti?", "id": "Untuk melindungi turbin dari kerusakan." },
  { "en": "Apa itu 'kapasitas terpasang'?", "id": "Daya maksimum yang bisa dihasilkan." },
  { "en": "Satuannya?", "id": "Watt (kW, MW, GW)." },
  { "en": "Apa itu 'faktor kapasitas'?", "id": "Rasio output aktual vs maksimum." },
  { "en": "Faktor kapasitas PLTS dan PLTB?", "id": "Lebih rendah dari PLTA atau PLTP." },
  { "en": "Mengapa?", "id": "Karena sifatnya yang intermiten." },
  { "en": "Apa itu 'sel surya film tipis'?", "id": "Sel surya yang fleksibel dan ringan." },
  { "en": "Apa itu 'sel surya perovskite'?", "id": "Teknologi sel surya baru." },
  { "en": "Potensinya?", "id": "Sangat efisien dan murah." },
  { "en": "Apa itu 'agrivoltaics'?", "id": "Pertanian dan PLTS di lahan sama." },
  { "en": "Keuntungannya?", "id": "Optimalisasi penggunaan lahan." },
  { "en": "Apa itu 'floating solar'?", "id": "PLTS yang mengapung di air." },
  { "en": "Contoh lokasi?", "id": "Waduk, danau." },
  { "en": "Keuntungannya?", "id": "Mengurangi penguapan, mendinginkan panel." },
  { "en": "Apa itu 'vertical axis wind turbine' (VAWT)?", "id": "Turbin angin dengan sumbu vertikal." },
  { "en": "Apa itu 'horizontal axis wind turbine' (HAWT)?", "id": "Turbin angin dengan sumbu horizontal." },
  { "en": "Mana yang paling umum?", "id": "HAWT (yang seperti kincir besar)." },
  { "en": "Dampak negatif turbin angin?", "id": "Kebisingan, dampak visual, bahaya burung." },
  { "en": "Apa itu 'geothermal heat pump'?", "id": "Menggunakan suhu tanah untuk pemanasan/pendinginan." },
  { "en": "Berbeda dengan PLTP?", "id": "Ya, skala lebih kecil." },
  { "en": "Apa itu 'enhanced geothermal system' (EGS)?", "id": "Teknologi geotermal di batuan kering." },
  { "en": "Apa itu 'biodegradable'?", "id": "Dapat terurai secara alami." },
  { "en": "Limbah organik bersifat biodegradable?", "id": "Ya." },
  { "en": "Apa itu 'landfill gas'?", "id": "Biogas dari tempat pembuangan sampah." },
  { "en": "Apa itu 'waste-to-energy'?", "id": "Mengubah sampah menjadi energi." },
  { "en": "Apa itu 'insinerasi'?", "id": "Proses pembakaran sampah." },
  { "en": "Kelemahannya?", "id": "Menghasilkan polusi udara." },
  { "en": "Apa itu 'gasifikasi'?", "id": "Mengubah biomassa menjadi gas sintetis." },
  { "en": "Apa itu 'pirolisis'?", "id": "Dekomposisi termal tanpa oksigen." },
  { "en": "Generasi biofuel?", "id": "Ada generasi pertama hingga keempat." },
  { "en": "Generasi pertama dari?", "id": "Tanaman pangan (jagung, tebu)." },
  { "en": "Kontroversinya?", "id": "Bersaing dengan pasokan pangan." },
  { "en": "Generasi kedua dari?", "id": "Limbah pertanian, tanaman non-pangan." },
  { "en": "Generasi ketiga dari?", "id": "Alga atau ganggang." },
  { "en": "Keuntungan alga?", "id": "Tumbuh cepat, tidak butuh lahan." },
  { "en": "Apa itu 'run-of-the-river' hydropower?", "id": "PLTA tanpa bendungan besar." },
  { "en": "Dampak lingkungan lebih rendah?", "id": "Ya, umumnya lebih rendah." },
  { "en": "Apa itu 'pumped-storage hydropower'?", "id": "Sistem penyimpanan energi." },
  { "en": "Bagaimana cara kerjanya?", "id": "Memompa air ke atas saat surplus." },
  { "en": "Lalu?", "id": "Melepas air ke bawah saat butuh." },
  { "en": "Apa itu 'ocean thermal energy conversion' (OTEC)?", "id": "Energi dari perbedaan suhu laut." },
  { "en": "Di mana potensinya?", "id": "Di perairan tropis." },
  { "en": "Apa itu 'salinity gradient power'?", "id": "Energi dari pertemuan air tawar dan asin." },
  { "en": "Apa itu 'distributed generation'?", "id": "Pembangkitan listrik skala kecil terdistribusi." },
  { "en": "Contohnya?", "id": "PLTS Atap." },
  { "en": "Lawan dari 'distributed generation'?", "id": "Pembangkitan terpusat (centralized)." },
  { "en": "Apa itu 'microgrid'?", "id": "Jaringan listrik lokal mandiri." },
  { "en": "Bisa terhubung ke jaringan utama?", "id": "Ya, bisa terhubung atau terpisah." },
  { "en": "Apa itu 'Levelized Cost of Energy' (LCOE)?", "id": "Biaya energi seumur hidup pembangkit." },
  { "en": "Digunakan untuk apa?", "id": "Membandingkan biaya berbagai teknologi." },
  { "en": "LCOE energi terbarukan saat ini?", "id": "Sangat kompetitif, bahkan lebih murah." },
  { "en": "Apa itu 'Sertifikat Energi Terbarukan'?", "id": "Renewable Energy Certificate (REC)." },
  { "en": "Apa itu 'hidrogen biru'?", "id": "Hidrogen dari gas alam dengan CCS." },
  { "en": "Apa itu 'CCS'?", "id": "Carbon Capture and Storage." },
  { "en": "Apa itu 'hidrogen abu-abu'?", "id": "Hidrogen dari gas alam tanpa CCS." },
  { "en": "Mana yang paling bersih?", "id": "Hidrogen hijau." },
  { "en": "Apa itu 'elektroliser'?", "id": "Alat untuk memisahkan air (elektrolisis)." },
  { "en": "Apa itu 'ekonomi hidrogen'?", "id": "Ekonomi berbasis hidrogen sebagai pembawa energi." },
  { "en": "Apa itu 'energi pasif'?", "id": "Desain bangunan yang memanfaatkan alam." },
  { "en": "Contohnya?", "id": "Ventilasi alami, pencahayaan alami." },
  { "en": "Apa itu 'daylighting'?", "id": "Penggunaan cahaya alami secara maksimal." },
  { "en": "Apa itu 'bangunan hijau'?", "id": "Bangunan yang ramah lingkungan." },
  { "en": "Apa itu 'Sistem Manajemen Energi'?", "id": "Energy Management System (EMS)." },
  { "en": "Apa itu 'demand response'?", "id": "Konsumen mengurangi pemakaian saat puncak." },
  { "en": "Insentif untuk konsumen?", "id": "Ya, biasanya ada insentif." },
  { "en": "Apa itu 'energi kinetik'?", "id": "Energi gerak." },
  { "en": "Turbin angin mengubah energi apa?", "id": "Kinetik menjadi mekanik lalu listrik." },
  { "en": "Apa itu 'energi potensial'?", "id": "Energi karena posisi atau ketinggian." },
  { "en": "PLTA mengubah energi apa?", "id": "Potensial menjadi kinetik lalu listrik." },
  { "en": "Apa itu 'Hukum Kekekalan Energi'?", "id": "Energi tidak bisa diciptakan/dimusnahkan." },
  { "en": "Energi hanya bisa apa?", "id": "Berubah dari satu bentuk ke bentuk lain." },
  { "en": "Bahan bakar fosil adalah biomassa?", "id": "Ya, biomassa purba." },
  { "en": "Prosesnya butuh berapa lama?", "id": "Jutaan tahun." },
  { "en": "Apa itu 'daur ulang energi'?", "id": "Memanfaatkan kembali panas buangan." },
  { "en": "Disebut juga?", "id": "Kogenerasi atau CHP." },
  { "en": "Apa itu 'CHP'?", "id": "Combined Heat and Power." },
  { "en": "Apa itu 'circular economy'?", "id": "Ekonomi dengan prinsip mengurangi limbah." },
  { "en": "Energi terbarukan mendukung ini?", "id": "Ya, sangat mendukung." },
  { "en": "Tantangan daur ulang panel surya?", "id": "Memisahkan material berharga." },
  { "en": "Tantangan daur ulang bilah turbin?", "id": "Material kompositnya sulit didaur ulang." },
  { "en": "Apa itu 'krisis energi'?", "id": "Kekurangan pasokan energi." },
  { "en": "Energi terbarukan meningkatkan apa?", "id": "Kemandirian dan ketahanan energi." },
  { "en": "Apa itu 'energi tidal stream'?", "id": "Energi dari arus pasang surut." },
  { "en": "Menggunakan apa?", "id": "Turbin bawah air." },
  { "en": "Apa itu 'tidal barrage'?", "id": "Bendungan di muara sungai." },
  { "en": "Kelemahannya?", "id": "Dampak lingkungan yang besar." },
  { "en": "Apa itu 'diversifikasi energi'?", "id": "Menggunakan berbagai macam sumber energi." },
  { "en": "Tujuannya?", "id": "Meningkatkan keamanan pasokan energi." },
  { "en": "Apa itu 'sel fotokimia'?", "id": "Meniru fotosintesis untuk menghasilkan bahan bakar." },
  { "en": "Disebut juga?", "id": "Daun artifisial." },
  { "en": "Apa itu 'kincir angin apung'?", "id": "Floating offshore wind turbine." },
  { "en": "Keuntungannya?", "id": "Bisa dipasang di perairan dalam." },
  { "en": "Apa itu 'material termoelektrik'?", "id": "Mengubah panas menjadi listrik langsung." },
  { "en": "Digunakan untuk apa?", "id": "Memanfaatkan panas buangan." },
  { "en": "Apa itu 'solar thermal'?", "id": "Menggunakan panas matahari." },
  { "en": "Contoh penggunaan?", "id": "Pemanas air tenaga surya." },
  { "en": "Apa itu 'solar chimney'?", "id": "Pembangkit listrik menggunakan menara." },
  { "en": "Bagaimana cara kerjanya?", "id": "Udara panas naik, memutar turbin." },
  { "en": "Apa itu 'kredit karbon'?", "id": "Izin untuk mengeluarkan sejumlah karbon." },
  { "en": "Bisa diperdagangkan?", "id": "Ya, dalam pasar karbon." },
  { "en": "Apa itu 'pajak karbon'?", "id": "Pajak atas emisi karbon." },
  { "en": "Tujuannya?", "id": "Mengurangi emisi dengan insentif ekonomi." },
  { "en": "Bahan bakar fosil adalah hidrokarbon?", "id": "Ya, sebagian besar." },
  { "en": "Apa itu 'hidrokarbon'?", "id": "Senyawa hidrogen dan karbon." },
  { "en": "Pembakaran hidrokarbon menghasilkan?", "id": "Karbon dioksida (CO2)." },
  { "en": "Apa itu 'desentralisasi energi'?", "id": "Menjauh dari pembangkit terpusat." },
  { "en": "Apa itu 'demokratisasi energi'?", "id": "Masyarakat bisa memproduksi energi sendiri." },
  { "en": "PLTS atap mendukung ini?", "id": "Ya, sangat mendukung." },
  { "en": "Apa itu 'prosumer'?", "id": "Produsen sekaligus konsumen energi." },
  { "en": "Apa itu 'virtual power plant' (VPP)?", "id": "Agregasi sumber energi terdistribusi." },
  { "en": "Apa itu 'Internet of Things' (IoT)?", "id": "Jaringan perangkat yang terhubung." },
  { "en": "Peran IoT dalam energi?", "id": "Monitoring dan kontrol cerdas." },
  { "en": "AI dalam energi terbarukan?", "id": "Prediksi cuaca, optimasi jaringan." },
  { "en": "Contohnya?", "id": "Memprediksi output panel surya." },
  { "en": "Apa itu 'blockchain'?", "id": "Buku besar digital terdesentralisasi." },
  { "en": "Peran blockchain dalam energi?", "id": "Perdagangan energi peer-to-peer." },
  { "en": "Apa itu 'peer-to-peer' (P2P) trading?", "id": "Jual beli langsung antar prosumer." },
  { "en": "Apa itu 'transmisi listrik'?", "id": "Penyaluran listrik jarak jauh." },
  { "en": "Menggunakan apa?", "id": "Saluran udara tegangan tinggi." },
  { "en": "Apa itu 'distribusi listrik'?", "id": "Penyaluran listrik ke konsumen akhir." },
  { "en": "Apa itu 'gardu induk'?", "id": "Substation untuk menaikkan/menurunkan tegangan." },
  { "en": "Apa itu 'transformator'?", "id": "Alat untuk mengubah tegangan listrik." },
  { "en": "Apa itu 'arus searah tegangan tinggi' (HVDC)?", "id": "Transmisi DC untuk jarak jauh." },
  { "en": "Keuntungannya?", "id": "Kerugian daya lebih kecil." },
  { "en": "Apa itu 'energi terbuang'?", "id": "Energi yang hilang dalam sistem." },
  { "en": "Contohnya?", "id": "Panas dari mesin, kerugian transmisi." },
  { "en": "Dampak visual 'wind farm'?", "id": "Dianggap merusak pemandangan." },
  { "en": "Apa itu 'shadow flicker'?", "id": "Bayangan berkedip dari bilah turbin." },
  { "en": "Apa itu 'kebijakan energi'?", "id": "Aturan pemerintah tentang energi." },
  { "en": "Apa itu 'subsidi energi'?", "id": "Bantuan pemerintah untuk menurunkan harga." },
  { "en": "Subsidi fosil menghambat transisi?", "id": "Ya, membuatnya lebih sulit." },
  { "en": "Apa itu 'ekonomi sirkular'?", "id": "Model ekonomi mengurangi limbah." },
  { "en": "Apa itu 'life cycle assessment' (LCA)?", "id": "Analisis dampak lingkungan produk." },
  { "en": "Dari mana sampai mana?", "id": "Dari pembuatan hingga pembuangan." },
  { "en": "Energi terbarukan punya dampak lingkungan?", "id": "Ya, pada tahap manufaktur." },
  { "en": "Contohnya?", "id": "Penambangan silikon, lithium, kobalt." },
  { "en": "Apa itu 'critical minerals'?", "id": "Mineral penting untuk teknologi bersih." },
  { "en": "Apa itu 'urban wind'?", "id": "Pemanfaatan angin di perkotaan." },
  { "en": "Tantangannya?", "id": "Aliran angin tidak teratur." },
  { "en": "Apa itu 'piezoelektrik'?", "id": "Material yang menghasilkan listrik dari tekanan." },
  { "en": "Contoh aplikasi?", "id": "Memanen energi dari langkah kaki." },
  { "en": "Apa itu 'energy harvesting'?", "id": "Memanen energi dari lingkungan sekitar." },
  { "en": "Sumbernya?", "id": "Getaran, panas, cahaya." },
  { "en": "Apa itu 'solar paint'?", "id": "Cat yang bisa menghasilkan listrik." },
  { "en": "Masih dalam pengembangan?", "id": "Ya, masih tahap riset." },
  { "en": "Apa itu 'solar fabric'?", "id": "Kain yang terintegrasi sel surya." },
  { "en": "Apa itu 'biodesain'?", "id": "Desain yang terinspirasi oleh biologi." },
  { "en": "Apa itu 'biomimikri'?", "id": "Meniru strategi alam untuk solusi." },
  { "en": "Contoh di energi?", "id": "Bilah turbin meniru sirip paus." },
  { "en": "Apa itu 'fotosintesis'?", "id": "Proses tumbuhan mengubah cahaya menjadi energi." },
  { "en": "Apa itu 'klorofil'?", "id": "Pigmen hijau untuk fotosintesis." },
  { "en": "Apa itu 'tenaga nuklir fusi'?", "id": "Energi dari penggabungan atom." },
  { "en": "Meniru apa?", "id": "Proses yang terjadi di matahari." },
  { "en": "Potensinya?", "id": "Energi bersih yang hampir tak terbatas." },
  { "en": "Masih dalam pengembangan?", "id": "Ya, tantangannya sangat besar." },
  { "en": "Apa itu 'tenaga nuklir fisi'?", "id": "Energi dari pemecahan atom." },
  { "en": "Digunakan di PLTN saat ini?", "id": "Ya." },
  { "en": "Apa itu 'weather forecasting'?", "id": "Prakiraan cuaca." },
  { "en": "Penting untuk energi terbarukan?", "id": "Ya, untuk memprediksi pasokan." },
  { "en": "Apa itu 'community solar'?", "id": "Proyek PLTS yang dimiliki komunitas." },
  { "en": "Tujuannya?", "id": "Memberi akses energi surya bagi penyewa." },
  { "en": "Apa itu 'energi untuk semua'?", "id": "Tujuan pembangunan berkelanjutan (SDG 7)." },
  { "en": "Apa itu 'keadilan energi'?", "id": "Distribusi manfaat dan beban energi." },
  { "en": "Energi terbarukan bisa menciptakan lapangan kerja?", "id": "Ya, di sektor pekerjaan hijau." },
  { "en": "Apa itu 'pekerjaan hijau'?", "id": "Pekerjaan di sektor lingkungan." },
  { "en": "Apa itu 'osilasi'?", "id": "Gerakan bolak-balik." },
  { "en": "Energi gelombang memanfaatkan osilasi?", "id": "Ya, osilasi permukaan air." },
  { "en": "Apa itu 'material fotokatalitik'?", "id": "Material yang mempercepat reaksi kimia dengan cahaya." },
  { "en": "Bisa untuk produksi hidrogen?", "id": "Ya, area riset aktif." },
  { "en": "Apa itu 'solar updraft tower'?", "id": "Sinonim untuk solar chimney." },
  { "en": "Apa itu 'kemandirian energi'?", "id": "Tidak bergantung pada impor energi." },
  { "en": "Apa itu 'ketahanan energi'?", "id": "Pasokan energi yang andal dan terjangkau." },
  { "en": "Apa itu 'roadmap' energi terbarukan?", "id": "Peta jalan atau rencana pengembangan." },
  { "en": "Apa itu 'investasi hijau'?", "id": "Investasi pada proyek ramah lingkungan." },
  { "en": "Apa itu 'obligasi hijau'?", "id": "Green bonds untuk mendanai proyek hijau." },
  { "en": "Biaya baterai lithium-ion?", "id": "Telah menurun drastis." },
  { "en": "Apa itu 'second-life battery'?", "id": "Baterai EV bekas untuk penyimpanan." },
  { "en": "Apa itu 'solid-state battery'?", "id": "Baterai dengan elektrolit padat." },
  { "en": "Keunggulannya?", "id": "Lebih aman, kepadatan energi tinggi." },
  { "en": "Masih dalam pengembangan?", "id": "Ya, teknologi masa depan." },
  { "en": "Apa itu 'solar car'?", "id": "Mobil yang ditenagai panel surya." },
  { "en": "Apa itu 'solar plane'?", "id": "Pesawat yang ditenagai panel surya." },
  { "en": "Tantangannya?", "id": "Area panel terbatas, butuh cuaca cerah." },
  { "en": "Apa itu 'perangkat off-grid'?", "id": "Perangkat yang tidak terhubung ke jaringan." },
  { "en": "Contohnya?", "id": "Lampu taman tenaga surya." },
  { "en": "Apa itu 'sistem on-grid'?", "id": "Sistem yang terhubung ke jaringan listrik." },
  { "en": "Apa itu 'sistem hybrid'?", "id": "Gabungan on-grid dengan penyimpanan baterai." },
  { "en": "Apa itu 'efek fotovoltaik'?", "id": "Fenomena fisika di balik sel surya." },
  { "en": "Siapa yang menemukannya?", "id": "Alexandre-Edmond Becquerel." },
  { "en": "Apa itu 'semikonduktor'?", "id": "Material antara konduktor dan isolator." },
  { "en": "Contohnya?", "id": "Silikon." },
  { "en": "Apa itu 'doping'?", "id": "Menambahkan pengotor ke semikonduktor." },
  { "en": "Tujuannya?", "id": "Mengubah sifat kelistrikannya." },
  { "en": "Apa itu 'n-type' dan 'p-type'?", "id": "Dua jenis semikonduktor hasil doping." },
  { "en": "Apa itu 'p-n junction'?", "id": "Pertemuan semikonduktor p-type dan n-type." },
  { "en": "Di mana letak 'p-n junction'?", "id": "Di jantung sel surya." },
  { "en": "Apa itu 'foton'?", "id": "Partikel cahaya." },
  { "en": "Foton menabrak elektron menyebabkan?", "id": "Aliran listrik (arus)." },
  { "en": "Apa itu 'band gap'?", "id": "Energi minimum untuk melepaskan elektron." },
  { "en": "Apa itu 'efisiensi sel surya'?", "id": "Persentase cahaya yang diubah jadi listrik." },
  { "en": "Berapa efisiensi sel surya komersial?", "id": "Sekitar 18-23%." },
  { "en": "Apa itu 'maximum power point tracking' (MPPT)?", "id": "Teknik untuk memaksimalkan output daya." },
  { "en": "Dilakukan oleh apa?", "id": "Inverter atau charge controller." },
  { "en": "Apa itu 'charge controller'?", "id": "Mengatur pengisian daya ke baterai." },
  { "en": "Dibutuhkan dalam sistem apa?", "id": "Sistem off-grid dengan baterai." },
  { "en": "Apa itu 'electrification rate'?", "id": "Rasio elektrifikasi atau persentase akses listrik." },
  { "en": "Energi terbarukan bisa meningkatkannya?", "id": "Ya, terutama di daerah terpencil." },
  { "en": "Apa itu 'solar home system' (SHS)?", "id": "PLTS skala kecil untuk rumah." },
  { "en": "Disebut juga?", "id": "Lampu Tenaga Surya Hemat Energi (LTSHE)." },
  { "en": "Apa itu 'electrolysis'?", "id": "Proses memisahkan air menjadi hidrogen/oksigen." },
  { "en": "Membutuhkan apa?", "id": "Listrik." },
  { "en": "Apa itu 'dekarbonisasi'?", "id": "Proses mengurangi emisi karbon." },
  { "en": "Sektor yang perlu dekarbonisasi?", "id": "Listrik, transportasi, industri." },
  { "en": "Apa itu 'Betz's Law'?", "id": "Batas efisiensi teoretis turbin angin." },
  { "en": "Berapa batasnya?", "id": "Sekitar 59.3%." },
  { "en": "Artinya?", "id": "Turbin tidak bisa menangkap 100% energi." },
  { "en": "Apa itu 'aeroelastisitas'?", "id": "Studi interaksi gaya aerodinamis dan elastis." },
  { "en": "Penting untuk desain apa?", "id": "Desain bilah turbin angin." },
  { "en": "Apa itu 'power purchase agreement' (PPA)?", "id": "Perjanjian jual beli listrik." },
  { "en": "Antara siapa?", "id": "Produsen dan pembeli listrik." },
  { "en": "Apa itu 'independent power producer' (IPP)?", "id": "Produsen listrik swasta." },
  { "en": "Apa itu 'utilitas'?", "id": "Perusahaan penyedia layanan publik (PLN)." },
  { "en": "Apa itu 'energi tidal range'?", "id": "Energi dari perbedaan tinggi pasang surut." },
  { "en": "Menggunakan apa?", "id": "Bendungan atau 'barrage'." },
  { "en": "Apa itu 'Hukum Termodinamika Pertama'?", "id": "Hukum kekekalan energi." },
  { "en": "Apa itu 'Hukum Termodinamika Kedua'?", "id": "Entropi (ketidakteraturan) selalu meningkat." },
  { "en": "Implikasinya?", "id": "Tidak ada konversi energi 100% efisien." },
  { "en": "Pasti ada energi yang terbuang?", "id": "Ya, biasanya dalam bentuk panas." },
  { "en": "Apa itu 'entropi'?", "id": "Ukuran ketidakteraturan atau keacakan." },
  { "en": "Apa itu 'efisiensi Carnot'?", "id": "Batas efisiensi teoretis mesin panas." },
  { "en": "Apa itu 'mesin panas'?", "id": "Mesin yang mengubah panas menjadi kerja." },
  { "en": "PLTP adalah mesin panas?", "id": "Ya, menggunakan panas bumi." },
  { "en": "Apa itu 'radiasi matahari'?", "id": "Energi yang dipancarkan oleh matahari." },
  { "en": "Apa itu 'insolation'?", "id": "Ukuran radiasi matahari di suatu lokasi." },
  { "en": "Diukur dalam apa?", "id": "kWh/m²/hari." },
  { "en": "Lokasi dengan insolasi tinggi?", "id": "Gurun, daerah khatulistiwa." },
  { "en": "Apa itu 'albedo'?", "id": "Kemampuan permukaan memantulkan cahaya." },
  { "en": "Permukaan putih punya albedo?", "id": "Tinggi." },
  { "en": "Permukaan gelap punya albedo?", "id": "Rendah." },
  { "en": "Apa itu 'efek rumah kaca'?", "id": "Pemanasan atmosfer oleh gas tertentu." },
  { "en": "Apakah efek ini alami?", "id": "Ya, tapi diperkuat oleh manusia." },
  { "en": "Apa itu 'gas rumah kaca' (GRK)?", "id": "Gas yang memerangkap panas." },
  { "en": "Contoh GRK?", "id": "CO2, metana (CH4), dinitrogen oksida (N2O)." },
  { "en": "Sumber metana?", "id": "Peternakan, tempat pembuangan sampah." },
  { "en": "Biogas mengandung metana?", "id": "Ya, sebagian besar." },
  { "en": "Apa itu 'jejak air'?", "id": "Total volume air tawar yang digunakan." },
  { "en": "Energi terbarukan punya jejak air?", "id": "Ya, terutama PLTA dan CSP." },
  { "en": "Apa itu 'geothermal reservoir'?", "id": "Cadangan air panas di bawah tanah." },
  { "en": "Apa itu 'wellhead'?", "id": "Peralatan di permukaan sumur geotermal." },
  { "en": "Apa itu 'turbin uap'?", "id": "Turbin yang digerakkan oleh uap." },
  { "en": "Digunakan di pembangkit apa?", "id": "PLTP, PLTN, PLTU, CSP." },
  { "en": "Apa itu 'generator'?", "id": "Mengubah energi mekanik menjadi listrik." },
  { "en": "Prinsip kerjanya?", "id": "Induksi elektromagnetik." },
  { "en": "Apa itu 'induksi elektromagnetik'?", "id": "Arus listrik dihasilkan oleh medan magnet." },
  { "en": "Siapa yang menemukannya?", "id": "Michael Faraday." },
  { "en": "Apa itu 'turbin gas'?", "id": "Turbin yang digerakkan oleh gas panas." },
  { "en": "Digunakan di pembangkit apa?", "id": "PLTG, PLTGU." },
  { "en": "Apa itu 'PLTGU'?", "id": "Pembangkit Listrik Tenaga Gas dan Uap." },
  { "en": "Disebut juga?", "id": "Combined cycle power plant." },
  { "en": "Lebih efisien dari PLTG?", "id": "Ya, karena memanfaatkan panas buangan." },
  { "en": "Apa itu 'pemadaman bergilir'?", "id": "Pemadaman listrik terencana." },
  { "en": "Penyebabnya?", "id": "Kekurangan pasokan daya." },
  { "en": "Apa itu 'beban puncak'?", "id": "Waktu penggunaan listrik tertinggi." },
  { "en": "Kapan biasanya terjadi?", "id": "Siang hari atau sore/malam hari." },
  { "en": "Apa itu 'pembangkit peaker'?", "id": "Pembangkit yang hanya beroperasi saat beban puncak." },
  { "en": "Biasanya menggunakan apa?", "id": "Gas alam." },
  { "en": "Baterai bisa jadi 'peaker'?", "id": "Ya, bisa menggantikan pembangkit peaker." },
  { "en": "Apa itu 'keandalan jaringan'?", "id": "Kemampuan jaringan menyediakan listrik terus-menerus." },
  { "en": "Apa itu 'stabilitas jaringan'?", "id": "Kemampuan menjaga frekuensi dan tegangan." },
  { "en": "Frekuensi listrik di Indonesia?", "id": "50 Hertz." },
  { "en": "Tegangan listrik rumah tangga?", "id": "220-230 Volt." },
  { "en": "Apa itu 'kualitas daya'?", "id": "Ukuran seberapa baik daya listrik." },
  { "en": "Apa itu 'harmonisa'?", "id": "Gangguan pada kualitas daya." },
  { "en": "Inverter bisa menyebabkan harmonisa?", "id": "Ya, jika kualitasnya kurang baik." },
  { "en": "Apa itu 'IEA'?", "id": "International Energy Agency." },
  { "en": "Apa itu 'IRENA'?", "id": "International Renewable Energy Agency." },
  { "en": "Apa itu 'UNFCCC'?", "id": "Konvensi PBB tentang Perubahan Iklim." },
  { "en": "Apa itu 'COP'?", "id": "Conference of the Parties." },
  { "en": "Pertemuan apa itu?", "id": "Pertemuan tahunan tentang iklim." },
  { "en": "Apa itu 'IPCC'?", "id": "Intergovernmental Panel on Climate Change." },
  { "en": "Tugas IPCC?", "id": "Menyediakan laporan ilmiah tentang iklim." },
  { "en": "Apa itu 'Sustainable Development Goals' (SDGs)?", "id": "Tujuan Pembangunan Berkelanjutan PBB." },
  { "en": "Energi bersih ada di SDG?", "id": "Ya, SDG nomor 7." },
  { "en": "Apa itu 'electrify everything'?", "id": "Mengganti teknologi fosil dengan listrik." },
  { "en": "Contohnya?", "id": "Kompor induksi, mobil listrik." },
  { "en": "Listriknya harus dari mana?", "id": "Sumber energi terbarukan." },
  { "en": "Apa itu 'grid inertia'?", "id": "Kemampuan jaringan menahan perubahan frekuensi." },
  { "en": "Pembangkit konvensional menyediakan inersia?", "id": "Ya, dari massa generator berputar." },
  { "en": "Inverter menyediakan inersia?", "id": "Tidak secara alami, butuh 'virtual inertia'." },
  { "en": "Apa itu 'virtual inertia'?", "id": "Inersia yang disimulasikan oleh elektronika daya." },
  { "en": "Apa itu 'energy efficiency rating'?", "id": "Peringkat efisiensi energi produk." },
  { "en": "Contohnya?", "id": "Label bintang pada AC." },
  { "en": "Apa itu 'vampire power'?", "id": "Energi yang dikonsumsi saat siaga." },
  { "en": "Disebut juga?", "id": "Phantom load atau standby power." },
  { "en": "Apa itu 'negawatt'?", "id": "Energi yang dihemat." },
  { "en": "Apa itu 'solar constant'?", "id": "Jumlah radiasi matahari di atmosfer atas." },
  { "en": "Apa itu 'air mass' (AM)?", "id": "Ukuran lintasan cahaya matahari." },
  { "en": "AM1.5 adalah standar untuk?", "id": "Pengujian sel surya." },
  { "en": "Apa itu 'soiling'?", "id": "Debu atau kotoran di panel surya." },
  { "en": "Dampaknya?", "id": "Mengurangi output daya." },
  { "en": "Solusinya?", "id": "Pembersihan rutin." },
  { "en": "Apa itu 'degradation' panel surya?", "id": "Penurunan performa seiring waktu." },
  { "en": "Berapa umur panel surya?", "id": "Sekitar 25-30 tahun." },
  { "en": "Apa itu 'inverter string'?", "id": "Satu inverter untuk serangkaian panel." },
  { "en": "Apa itu 'microinverter'?", "id": "Satu inverter untuk satu panel." },
  { "en": "Keuntungannya?", "id": "Optimalisasi per panel, lebih andal." },
  { "en": "Apa itu 'power optimizer'?", "id": "Perangkat DC-DC untuk optimalisasi panel." },
  { "en": "Apa itu 'peak sun hours'?", "id": "Jumlah jam dengan insolasi puncak." },
  { "en": "Digunakan untuk apa?", "id": "Menghitung produksi energi PLTS." },
  { "en": "Apa itu 'Kaplan turbine'?", "id": "Jenis turbin air untuk PLTA." },
  { "en": "Apa itu 'Francis turbine'?", "id": "Jenis turbin air yang umum." },
  { "en": "Apa itu 'Pelton turbine'?", "id": "Jenis turbin air untuk 'head' tinggi." },
  { "en": "Apa itu 'head' dalam PLTA?", "id": "Perbedaan ketinggian air." },
  { "en": "Apa itu 'penstock'?", "id": "Pipa besar yang mengalirkan air." },
  { "en": "Apa itu 'spillway'?", "id": "Saluran pelimpah pada bendungan." },
  { "en": "Dampak turbin angin pada radar?", "id": "Bisa menyebabkan interferensi." },
  { "en": "Apa itu 'blade tip speed'?", "id": "Kecepatan ujung bilah turbin." },
  { "en": "Bisa lebih cepat dari suara?", "id": "Tidak, akan menyebabkan masalah aerodinamis." },
  { "en": "Apa itu 'direct drive' wind turbine?", "id": "Turbin tanpa gearbox." },
  { "en": "Keuntungannya?", "id": "Lebih andal, perawatan lebih sedikit." },
  { "en": "Berapa biaya energi terbarukan?", "id": "Semakin murah dari waktu ke waktu." },
  { "en": "Biaya PLTS telah turun?", "id": "Ya, turun sangat drastis." },
  { "en": "Apa itu 'Swanson's Law'?", "id": "Harga sel surya turun 20% setiap penggandaan volume." },
  { "en": "Mirip dengan 'Moore's Law'?", "id": "Ya, tapi untuk fotovoltaik." },
  { "en": "Apa itu 'land use intensity'?", "id": "Kebutuhan lahan per unit energi." },
  { "en": "Mana yang butuh lahan luas?", "id": "PLTS dan PLTB." },
  { "en": "Mana yang butuh lahan kecil?", "id": "PLTP dan PLTN." },
  { "en": "Apa itu 'capacity credit'?", "id": "Kontribusi pembangkit terhadap keandalan sistem." },
  { "en": "Apa itu 'curtailment'?", "id": "Pembatasan output energi terbarukan." },
  { "en": "Mengapa terjadi?", "id": "Kelebihan pasokan atau keterbatasan jaringan." },
  { "en": "Apa itu 'grid congestion'?", "id": "Kemacetan pada jaringan transmisi." },
  { "en": "Apa itu 'balancing area'?", "id": "Wilayah geografis untuk menyeimbangkan jaringan." },
  { "en": "Apa itu 'ancillary services'?", "id": "Layanan pendukung untuk keandalan jaringan." },
  { "en": "Contohnya?", "id": "Regulasi frekuensi, cadangan daya." },
  { "en": "Baterai bisa menyediakan layanan ini?", "id": "Ya, dengan respons sangat cepat." },
  { "en": "Apa itu 'flywheel energy storage'?", "id": "Menyimpan energi dalam roda berputar." },
  { "en": "Apa itu 'compressed air energy storage' (CAES)?", "id": "Menyimpan energi sebagai udara terkompresi." },
  { "en": "Di mana disimpannya?", "id": "Di gua bawah tanah." },
  { "en": "Apa itu 'vehicle-to-grid' (V2G)?", "id": "Mobil listrik menyuplai daya ke jaringan." },
  { "en": "Potensinya?", "id": "Menjadi sistem penyimpanan energi masif." },
  { "en": "Apa itu 'state of charge' (SoC)?", "id": "Tingkat keterisian baterai." },
  { "en": "Apa itu 'depth of discharge' (DoD)?", "id": "Seberapa dalam baterai dikosongkan." },
  { "en": "Apa itu 'cycle life'?", "id": "Jumlah siklus isi-ulang baterai." },
  { "en": "Apa itu 'thermal runaway'?", "id": "Kegagalan baterai yang menyebabkan panas." },
  { "en": "Baterai lithium-ion bisa mengalaminya?", "id": "Ya, jika tidak dikelola baik." },
  { "en": "Apa itu 'energy density'?", "id": "Jumlah energi per unit volume/berat." },
  { "en": "Apa itu 'power density'?", "id": "Jumlah daya per unit volume/berat." },
  { "en": "Baterai EV butuh apa?", "id": "Energy density tinggi." },
  { "en": "Apa itu 'supercapacitor'?", "id": "Penyimpan energi dengan power density tinggi." },
  { "en": "Apa itu 'bio-architecture'?", "id": "Arsitektur yang terintegrasi dengan alam." },
  { "en": "Apa itu 'passive house'?", "id": "Standar bangunan sangat efisien energi." },
  { "en": "Asalnya dari mana?", "id": "Jerman (Passivhaus)." },
  { "en": "Apa itu 'net-zero energy building'?", "id": "Bangunan yang menghasilkan energi sendiri." },
  { "en": "Apa itu 'geothermal gradient'?", "id": "Peningkatan suhu seiring kedalaman bumi." },
  { "en": "Apa itu 'binary cycle' geothermal plant?", "id": "PLTP untuk suhu air lebih rendah." },
  { "en": "Bagaimana cara kerjanya?", "id": "Menggunakan fluida kerja sekunder." },
  { "en": "Apa itu 'anaerobic digestion'?", "id": "Proses pembuatan biogas." },
  { "en": "Berlangsung tanpa apa?", "id": "Tanpa oksigen." },
  { "en": "Apa itu 'digester'?", "id": "Tangki untuk proses anaerobic digestion." },
  { "en": "Apa itu 'feedstock'?", "id": "Bahan baku untuk proses energi." },
  { "en": "Contoh 'feedstock' biofuel?", "id": "Jagung, tebu, jelantah." },
  { "en": "Apa itu 'albedo effect'?", "id": "Efek pendinginan dari permukaan reflektif." },
  { "en": "Mencairnya es di kutub mengurangi?", "id": "Albedo bumi, mempercepat pemanasan." },
  { "en": "Apa itu 'biochar'?", "id": "Arang dari pirolisis biomassa." },
  { "en": "Bisa untuk apa?", "id": "Memperbaiki tanah dan menyimpan karbon." },
  { "en": "Apa itu 'carbon sequestration'?", "id": "Proses menangkap dan menyimpan karbon." },
  { "en": "Hutan melakukan ini?", "id": "Ya, secara alami." },
  { "en": "Apa itu 'direct air capture' (DAC)?", "id": "Menyerap CO2 langsung dari udara." },
  { "en": "Energi terbarukan bisa didaur ulang?", "id": "Beberapa komponennya bisa." },
  { "en": "Apa itu 'urban heat island'?", "id": "Suhu perkotaan lebih panas dari sekitarnya." },
  { "en": "Atap hijau bisa membantu?", "id": "Ya, bisa mengurangi efek ini." },
  { "en": "Apa itu 'green roof'?", "id": "Atap yang ditanami vegetasi." },
  { "en": "Apa itu 'ocean current energy'?", "id": "Energi dari arus laut." },
  { "en": "Potensinya?", "id": "Besar dan lebih dapat diprediksi." },
  { "en": "Tantangannya?", "id": "Lingkungan laut yang keras." },
  { "en": "Apa itu 'bio-concrete'?", "id": "Beton yang bisa memperbaiki diri." },
  { "en": "Menggunakan apa?", "id": "Bakteri." },
  { "en": "Apa itu 'solar desalination'?", "id": "Membuat air tawar dengan tenaga surya." },
  { "en": "Apa itu 'smart window'?", "id": "Jendela yang bisa mengatur cahaya/panas." },
  { "en": "Apa itu 'electrochromic glass'?", "id": "Kaca yang berubah warna dengan listrik." },
  { "en": "Apa itu 'building-integrated photovoltaics' (BIPV)?", "id": "Panel surya yang menjadi bagian bangunan." },
  { "en": "Contoh BIPV?", "id": "Atap genteng surya, jendela surya." },
  { "en": "Apa itu 'geopolitik mineral'?", "id": "Politik terkait mineral transisi energi." },
  { "en": "Contoh mineralnya?", "id": "Lithium, kobalt, nikel." },
  { "en": "Untuk apa mineral tersebut?", "id": "Baterai dan teknologi bersih lainnya." },
  { "en": "Apa itu 'urban mining'?", "id": "Mendaur ulang mineral dari sampah elektronik." },
  { "en": "Apa itu 'e-waste'?", "id": "Sampah elektronik." },
  { "en": "Apa itu 'kincir angin tanpa bilah'?", "id": "Turbin yang berosilasi." },
  { "en": "Apa itu 'airborne wind energy'?", "id": "Energi angin dari ketinggian." },
  { "en": "Menggunakan apa?", "id": "Layang-layang (kite) atau drone." },
  { "en": "Keuntungannya?", "id": "Angin lebih kuat dan stabil." },
  { "en": "Apa itu 'solar tracker'?", "id": "Sistem yang membuat panel mengikuti matahari." },
  { "en": "Jenisnya?", "id": "Sumbu tunggal dan sumbu ganda." },
  { "en": "Meningkatkan produksi energi?", "id": "Ya, secara signifikan." },
  { "en": "Apa itu 'bifacial solar panel'?", "id": "Panel yang menyerap cahaya dari dua sisi." },
  { "en": "Sisi belakang menyerap apa?", "id": "Cahaya yang dipantulkan dari permukaan." },
  { "en": "Cocok dipasang di atas apa?", "id": "Permukaan reflektif (pasir, air)." },
  { "en": "Apa itu 'solar thermal fuel'?", "id": "Cairan yang menyimpan energi surya." },
  { "en": "Bagaimana cara kerjanya?", "id": "Molekul berubah bentuk saat kena cahaya." },
  { "en": "Energi dilepaskan sebagai apa?", "id": "Sebagai panas saat dibutuhkan." },
  { "en": "Apa itu 'photosynthesis' buatan?", "id": "Meniru fotosintesis untuk membuat bahan bakar." },
  { "en": "Menggunakan apa?", "id": "Cahaya matahari, air, dan CO2." },
  { "en": "Apa itu 'concentrator photovoltaics' (CPV)?", "id": "PV yang menggunakan lensa atau cermin." },
  { "en": "Tujuannya?", "id": "Memfokuskan cahaya ke sel surya kecil." },
  { "en": "Membutuhkan sel surya jenis apa?", "id": "Sel surya multi-junction yang efisien." },
  { "en": "Apa itu 'multi-junction solar cell'?", "id": "Sel surya dengan banyak lapisan." },
  { "en": "Setiap lapisan menangkap apa?", "id": "Panjang gelombang cahaya yang berbeda." },
  { "en": "Apa itu 'quantum dot solar cell'?", "id": "Sel surya menggunakan titik kuantum." },
  { "en": "Apa itu 'space-based solar power'?", "id": "Panel surya di orbit luar angkasa." },
  { "en": "Energi dikirim ke bumi via?", "id": "Gelombang mikro atau laser." },
  { "en": "Masih dalam tahap konsep?", "id": "Ya, masih sangat futuristik." },
  { "en": "Apa itu 'auditor energi'?", "id": "Profesional yang menganalisis penggunaan energi." },
  { "en": "Apa itu 'audit energi'?", "id": "Proses evaluasi penggunaan energi." },
  { "en": "Tujuannya?", "id": "Mengidentifikasi peluang penghematan energi." },
  { "en": "Apa itu 'renewable portfolio standard' (RPS)?", "id": "Kewajiban utilitas menggunakan energi terbarukan." },
  { "en": "Apa itu 'climate tech'?", "id": "Teknologi untuk mengatasi perubahan iklim." },
  { "en": "Energi terbarukan bagian darinya?", "id": "Ya, bagian yang sangat besar." },
  { "en": "Apa itu 'greenwashing'?", "id": "Klaim ramah lingkungan yang menyesatkan." },
  { "en": "Apa itu 'Paris Agreement'?", "id": "Perjanjian iklim global tahun 2015." },
  { "en": "Tujuannya?", "id": "Menjaga kenaikan suhu di bawah 2°C." },
  { "en": "Apa itu 'NDC'?", "id": "Nationally Determined Contribution." },
  { "en": "Apa isinya?", "id": "Komitmen iklim dari setiap negara." },
  { "en": "Apa itu 'carbon offsetting'?", "id": "Menyeimbangkan emisi dengan mendanai proyek hijau." },
  { "en": "Apa itu 'carbon footprint'?", "id": "Jejak karbon." },
  { "en": "Bisa dihitung untuk individu?", "id": "Ya." },
  { "en": "Apa itu 'gasohol'?", "id": "Campuran bensin dan etanol." },
  { "en": "E10 berarti?", "id": "10% etanol, 90% bensin." },
  { "en": "Apa itu 'flex-fuel vehicle'?", "id": "Kendaraan yang bisa pakai berbagai campuran etanol." },
  { "en": "Apa itu 'catalytic converter'?", "id": "Mengurangi emisi gas buang mobil." },
  { "en": "Apa itu 'polusi cahaya'?", "id": "Cahaya buatan berlebih di malam hari." },
  { "en": "Apa itu 'polusi suara'?", "id": "Suara bising yang mengganggu." },
  { "en": "Turbin angin menyebabkan polusi suara?", "id": "Ya, pada jarak dekat." },
  { "en": "Apa itu 'PLTMH'?", "id": "Pembangkit Listrik Tenaga Mikrohidro." },
  { "en": "Cocok untuk daerah mana?", "id": "Daerah pedesaan dengan sungai." },
  { "en": "Apa itu 'solar cooker'?", "id": "Kompor yang memasak dengan panas matahari." },
  { "en": "Apa itu 'hydropower'?", "id": "Tenaga air." },
  { "en": "Apa itu 'solar power'?", "id": "Tenaga surya." },
  { "en": "Apa itu 'wind power'?", "id": "Tenaga angin atau bayu." },
  { "en": "Apa itu 'geothermal power'?", "id": "Tenaga panas bumi." },
  { "en": "Apa itu 'biomass power'?", "id": "Tenaga biomassa." },
  { "en": "Apa itu 'tidal power'?", "id": "Tenaga pasang surut." },
  { "en": "Apa itu 'wave power'?", "id": "Tenaga ombak." },
  { "en": "Apa itu 'watt'?", "id": "Satuan daya." },
  { "en": "Apa itu 'joule'?", "id": "Satuan energi." },
  { "en": "Apa itu 'kilowatt-hour' (kWh)?", "id": "Satuan konsumsi energi listrik." },
  { "en": "Tagihan listrik dalam satuan apa?", "id": "kWh." },
  { "en": "Apa itu 'renewable energy'?", "id": "Energi terbarukan." },
  { "en": "Apa itu 'fossil fuel'?", "id": "Bahan bakar fosil." },
  { "en": "Apa itu 'sustainability'?", "id": "Keberlanjutan." },
  { "en": "Apa itu 'climate change'?", "id": "Perubahan iklim." },
  { "en": "Apa itu 'global warming'?", "id": "Pemanasan global." },
  { "en": "Apa itu 'greenhouse gas'?", "id": "Gas rumah kaca." },
  { "en": "Apa itu 'decarbonization'?", "id": "Dekarbonisasi." },
  { "en": "Apa itu 'energy transition'?", "id": "Transisi energi." },
  { "en": "Apa itu 'energy security'?", "id": "Ketahanan energi." },
  { "en": "Apa itu 'energy independence'?", "id": "Kemandirian energi." },
  { "en": "Apa itu 'energy efficiency'?", "id": "Efisiensi energi." },
  { "en": "Apa itu 'energy conservation'?", "id": "Konservasi energi." },
  { "en": "Apa itu 'smart city'?", "id": "Kota pintar." },
  { "en": "Apa itu 'green building'?", "id": "Bangunan hijau." },
  { "en": "Apa itu 'circular economy'?", "id": "Ekonomi sirkular." },
  { "en": "Apa itu 'zero waste'?", "id": "Nol sampah." },
  { "en": "Energi terbarukan mendukung zero waste?", "id": "Ya, dengan biogas dari limbah." },
  { "en": "Apa itu 'pyranometer'?", "id": "Alat untuk mengukur radiasi surya." },
  { "en": "Apa itu 'anemometer'?", "id": "Alat untuk mengukur kecepatan angin." },
  { "en": "Apa itu 'energy audit'?", "id": "Audit energi." },
  { "en": "Apa itu 'carbon neutral'?", "id": "Karbon netral." },
  { "en": "Sama dengan 'net-zero'?", "id": "Mirip, tapi ada perbedaan teknis." },
  { "en": "Apa itu 'power grid'?", "id": "Jaringan listrik." },
  { "en": "Apa itu 'base load'?", "id": "Permintaan listrik minimum konstan." },
  { "en": "Pembangkit 'base load'?", "id": "PLTN, PLTU, PLTA, PLTP." },
  { "en": "Energi terbarukan yang jadi 'base load'?", "id": "PLTA dan PLTP." },
  { "en": "Apa itu 'peak load'?", "id": "Beban puncak." },
  { "en": "Apa itu 'off-peak'?", "id": "Waktu di luar beban puncak." },
  { "en": "Apa itu 'energy arbitrage'?", "id": "Membeli listrik saat murah, menjual saat mahal." },
  { "en": "Baterai bisa melakukan ini?", "id": "Ya, itu salah satu fungsinya." },
  { "en": "Apa itu 'grid-scale storage'?", "id": "Penyimpanan energi skala jaringan." },
  { "en": "Apa itu 'behind-the-meter'?", "id": "Sistem energi di sisi pelanggan." },
  { "en": "Contohnya?", "id": "PLTS Atap dan baterai rumah." },
  { "en": "Apa itu 'utility-scale solar'?", "id": "PLTS skala besar milik utilitas." },
  { "en": "Apa itu 'monocrystalline silicon'?", "id": "Jenis silikon sel surya." },
  { "en": "Warnanya?", "id": "Hitam pekat." },
  { "en": "Apa itu 'polycrystalline silicon'?", "id": "Jenis silikon sel surya lain." },
  { "en": "Warnanya?", "id": "Biru dengan corak kristal." },
  { "en": "Mana yang lebih efisien?", "id": "Monocrystalline, tapi lebih mahal." },
  { "en": "Apa itu 'amorphous silicon'?", "id": "Silikon tanpa struktur kristal." },
  { "en": "Digunakan di mana?", "id": "Sel surya film tipis." },
  { "en": "Apa itu 'inverter efisiensi'?", "id": "Seberapa baik inverter mengubah DC ke AC." },
  { "en": "Apa itu 'balance of system' (BOS)?", "id": "Semua komponen PLTS selain panel." },
  { "en": "Contoh BOS?", "id": "Inverter, kabel, mounting, baterai." },
  { "en": "Apa itu 'mounting system'?", "id": "Struktur penyangga panel surya." },
  { "en": "Apa itu 'ground-mounted solar'?", "id": "PLTS yang dipasang di tanah." },
  { "en": "Apa itu 'roof-mounted solar'?", "id": "PLTS yang dipasang di atap." },
  { "en": "Apa itu 'solar canopy'?", "id": "Struktur peneduh dengan atap panel surya." },
  { "en": "Contoh lokasi?", "id": "Tempat parkir mobil." },
  { "en": "Apa itu 'silikon'?", "id": "Unsur kimia, semikonduktor utama." },
  { "en": "Bahan bakunya?", "id": "Pasir kuarsa." },
  { "en": "Apa itu 'feed-in tariff' vs 'net metering'?", "id": "Tarif tetap vs selisih ekspor-impor." },
  { "en": "Apa itu 'renewable energy credits' (RECs)?", "id": "Sama dengan sertifikat energi terbarukan." },
  { "en": "Satu REC mewakili apa?", "id": "Satu megawatt-hour (MWh) energi bersih." },
  { "en": "Apa itu 'power-to-x' (P2X)?", "id": "Mengubah listrik terbarukan menjadi produk lain." },
  { "en": "Contoh X?", "id": "Gas (hidrogen), liquid (amonya)." },
  { "en": "Apa itu 'power-to-gas'?", "id": "Mengubah listrik menjadi gas hidrogen." },
  { "en": "Apa itu 'sektor coupling'?", "id": "Menghubungkan sektor listrik dengan sektor lain." },
  { "en": "Contohnya?", "id": "Listrik untuk transportasi (EV)." },
  { "en": "Apa itu 'energi panas surya'?", "id": "Solar thermal energy." },
  { "en": "Apa itu 'parabolic trough'?", "id": "Jenis kolektor surya untuk CSP." },
  { "en": "Bentuknya?", "id": "Seperti palung parabola." },
  { "en": "Apa itu 'power tower'?", "id": "Jenis CSP dengan menara pusat." },
  { "en": "Apa itu 'heliostat'?", "id": "Cermin yang mengikuti matahari." },
  { "en": "Digunakan di mana?", "id": "Sistem power tower." },
  { "en": "Apa itu 'molten salt'?", "id": "Garam cair untuk menyimpan panas." },
  { "en": "Digunakan di CSP?", "id": "Ya, sebagai media penyimpan panas." },
  { "en": "Apa itu 'waste heat'?", "id": "Panas buangan." },
  { "en": "Apa itu 'waste heat recovery'?", "id": "Memanfaatkan kembali panas buangan." },
  { "en": "Apa itu 'Organic Rankine Cycle' (ORC)?", "id": "Siklus daya untuk panas suhu rendah." },
  { "en": "Digunakan di mana?", "id": "PLTP binary cycle, pemulihan panas." },
  { "en": "Apa itu 'carbon intensity'?", "id": "Jumlah emisi karbon per unit energi." },
  { "en": "Energi terbarukan punya 'carbon intensity'?", "id": "Sangat rendah (mendekati nol)." },
  { "en": "Energi fosil punya 'carbon intensity'?", "id": "Sangat tinggi." },
  { "en": "Apa itu 'life-cycle carbon footprint'?", "id": "Jejak karbon seumur hidup." },
  { "en": "Termasuk apa saja?", "id": "Manufaktur, operasi, dan pembuangan." },
  { "en": "Apa itu 'derating'?", "id": "Pengurangan output daya karena kondisi." },
  { "en": "Contohnya?", "id": "Suhu panas pada panel surya." },
  { "en": "Panel surya lebih efisien saat?", "id": "Suhu dingin dan cuaca cerah." },
  { "en": "Apa itu 'DC coupling'?", "id": "Baterai terhubung di sisi DC." },
  { "en": "Apa itu 'AC coupling'?", "id": "Baterai terhubung di sisi AC." },
  { "en": "Apa itu 'grid-forming inverter'?", "id": "Inverter yang bisa menciptakan jaringan sendiri." },
  { "en": "Penting untuk apa?", "id": "Microgrid dan backup daya." },
  { "en": "Apa itu 'grid-following inverter'?", "id": "Inverter yang mengikuti jaringan yang ada." },
  { "en": "Apa itu 'black start capability'?", "id": "Kemampuan memulai pembangkit tanpa daya eksternal." },
  { "en": "PLTA punya kemampuan ini?", "id": "Ya, umumnya punya." },
  { "en": "Apa itu 'energy poverty'?", "id": "Kekurangan akses ke energi modern." },
  { "en": "Apa itu 'just transition'?", "id": "Transisi energi yang adil bagi pekerja." },
  { "en": "Untuk pekerja di sektor mana?", "id": "Sektor bahan bakar fosil." },
  { "en": "Apa itu 'stranded assets'?", "id": "Aset yang kehilangan nilai sebelum waktunya." },
  { "en": "Contohnya?", "id": "Pembangkit listrik batu bara." },
  { "en": "Apa itu 'divestment'?", "id": "Menjual aset karena alasan etis." },
  { "en": "Contohnya?", "id": "Divestasi dari perusahaan bahan bakar fosil." },
  { "en": "Apa itu 'ESG'?", "id": "Environmental, Social, and Governance." },
  { "en": "Kriteria untuk apa?", "id": "Investasi yang bertanggung jawab." },
  { "en": "Apa itu 'sustainable finance'?", "id": "Keuangan yang mempertimbangkan faktor ESG." },
  { "en": "Apa itu 'green hydrogen'?", "id": "Hidrogen hijau." },
  { "en": "Apa itu 'blue hydrogen'?", "id": "Hidrogen biru." },
  { "en": "Apa itu 'grey hydrogen'?", "id": "Hidrogen abu-abu." },
  { "en": "Apa itu 'electrolyzer'?", "id": "Elektroliser." },
  { "en": "Apa itu 'fuel cell vehicle' (FCV)?", "id": "Kendaraan sel bahan bakar." },
  { "en": "Menggunakan bahan bakar apa?", "id": "Hidrogen." },
  { "en": "Apa itu 'battery electric vehicle' (BEV)?", "id": "Kendaraan listrik baterai." },
  { "en": "Apa itu 'plug-in hybrid' (PHEV)?", "id": "Hybrid yang bisa diisi daya." },
  { "en": "Apa itu 'regenerative braking'?", "id": "Mengubah energi pengereman menjadi listrik." },
  { "en": "EV menggunakan ini?", "id": "Ya, untuk meningkatkan jangkauan." },
  { "en": "Apa itu 'range anxiety'?", "id": "Kekhawatiran kehabisan baterai EV." },
  { "en": "Apa itu 'smart charging'?", "id": "Mengisi daya EV saat listrik murah/bersih." },
  { "en": "Apa itu 'wind rose'?", "id": "Diagram yang menunjukkan distribusi arah angin." },
  { "en": "Digunakan untuk apa?", "id": "Penempatan turbin angin." },
  { "en": "Apa itu 'topography'?", "id": "Bentuk permukaan daratan." },
  { "en": "Mempengaruhi kecepatan angin?", "id": "Ya, sangat mempengaruhi." },
  { "en": "Apa itu 'wind shear'?", "id": "Perubahan kecepatan angin dengan ketinggian." },
  { "en": "Semakin tinggi, angin semakin?", "id": "Kuat dan stabil." },
  { "en": "Mengapa menara turbin tinggi?", "id": "Untuk menjangkau angin yang lebih baik." },
  { "en": "Apa itu 'wake effect'?", "id": "Turbulensi di belakang turbin angin." },
  { "en": "Dampaknya?", "id": "Mengurangi efisiensi turbin di belakangnya." },
  { "en": "Bagaimana mengatasinya?", "id": "Dengan penempatan turbin yang cermat." },
  { "en": "Apa itu 'pitch' vs 'yaw'?", "id": "Sudut bilah vs arah turbin." },
  { "en": "Apa itu 'SCADA'?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Digunakan di 'wind farm'?", "id": "Ya, untuk monitoring dan kontrol." },
  { "en": "Apa itu 'derated' power?", "id": "Daya yang dikurangi secara sengaja." },
  { "en": "Apa itu 'insulator'?", "id": "Bahan yang menghambat aliran listrik." },
  { "en": "Apa itu 'conductor'?", "id": "Bahan yang menghantarkan aliran listrik." },
  { "en": "Kabel terbuat dari apa?", "id": "Konduktor (tembaga, aluminium)." },
  { "en": "Apa itu 'grid parity'?", "id": "Biaya energi terbarukan sama dengan fosil." },
  { "en": "Sudah tercapai di banyak tempat?", "id": "Ya, untuk PLTS dan PLTB." },
  { "en": "Apa itu 'energy return on investment' (EROI)?", "id": "Energi didapat dibagi energi diinvestasikan." },
  { "en": "EROI tinggi berarti?", "id": "Sumber energi yang sangat efisien." },
  { "en": "Energi fosil punya EROI?", "id": "Secara historis tinggi, tapi menurun." },
  { "en": "Energi terbarukan punya EROI?", "id": "Terus meningkat seiring teknologi." },
  { "en": "Apa itu 'feed-in premium'?", "id": "Bonus di atas harga pasar." },
  { "en": "Apa itu 'tender' atau 'lelang' energi?", "id": "Mekanisme kompetitif untuk proyek energi." },
  { "en": "Apa itu 'environmental impact assessment' (EIA)?", "id": "Analisis Mengenai Dampak Lingkungan (AMDAL)." },
  { "en": "Diperlukan untuk proyek energi?", "id": "Ya, untuk proyek skala besar." },
  { "en": "Apa itu 'social cost of carbon'?", "id": "Estimasi kerugian ekonomi dari emisi." },
  { "en": "Apa itu 'eksternalitas'?", "id": "Biaya atau manfaat bagi pihak ketiga." },
  { "en": "Polusi adalah eksternalitas?", "id": "Ya, eksternalitas negatif." },
  { "en": "Apa itu 'carbon tax'?", "id": "Pajak karbon." },
  { "en": "Apa itu 'cap and trade'?", "id": "Sistem perdagangan emisi." },
  { "en": "Apa itu 'renewable heat'?", "id": "Panas dari sumber terbarukan." },
  { "en": "Contohnya?", "id": "Geotermal, panas surya, pompa panas." },
  { "en": "Apa itu 'district heating'?", "id": "Sistem pemanas terpusat untuk suatu area." },
  { "en": "Bisa menggunakan energi terbarukan?", "id": "Ya, sangat ideal." },
  { "en": "Apa itu 'pellet kayu'?", "id": "Bahan bakar biomassa padat." },
  { "en": "Apa itu 'torrefaction'?", "id": "Proses memanaskan biomassa tanpa oksigen." },
  { "en": "Tujuannya?", "id": "Meningkatkan kualitasnya sebagai bahan bakar." },
  { "en": "Apa itu 'co-firing'?", "id": "Mencampur biomassa dengan batu bara." },
  { "en": "Di mana dilakukannya?", "id": "Di Pembangkit Listrik Tenaga Uap (PLTU)." },
  { "en": "Tujuannya?", "id": "Mengurangi emisi dari PLTU." },
  { "en": "Apa itu 'bio-energy with carbon capture' (BECCS)?", "id": "Energi bio dengan penangkapan karbon." },
  { "en": "Bisa menghasilkan 'emisi negatif'?", "id": "Ya, secara teoretis bisa." },
  { "en": "Apa itu 'emisi negatif'?", "id": "Menyerap lebih banyak CO2 dari atmosfer." },
  { "en": "Apa itu 'silicon wafer'?", "id": "Kepingan silikon dasar untuk sel surya." },
  { "en": "Apa itu 'ingot'?", "id": "Batangan silikon kristal." },
  { "en": "Apa itu 'ribbon' silicon?", "id": "Teknik membuat wafer tanpa memotong." },
  { "en": "Apa itu 'passivated emitter and rear cell' (PERC)?", "id": "Teknologi untuk meningkatkan efisiensi sel." },
  { "en": "Apa itu 'heterojunction technology' (HJT)?", "id": "Teknologi sel surya efisiensi tinggi." },
  { "en": "Apa itu 'top-contact'?", "id": "Kontak listrik di atas sel." },
  { "en": "Apa itu 'back-contact'?", "id": "Kontak listrik di belakang sel." },
  { "en": "Keuntungannya?", "id": "Tidak ada bayangan, efisiensi lebih tinggi." },
  { "en": "Apa itu 'anti-reflective coating'?", "id": "Lapisan untuk mengurangi pantulan cahaya." },
  { "en": "Apa itu 'bypass diode'?", "id": "Dioda untuk melindungi sel dari bayangan." },
  { "en": "Apa itu 'junction box'?", "id": "Kotak koneksi di belakang panel." },
  { "en": "Apa itu 'PV module'?", "id": "Satu unit panel surya." },
  { "en": "Apa itu 'PV array'?", "id": "Gabungan beberapa panel surya." },
  { "en": "Apa itu 'inverter clipping'?", "id": "Saat daya DC melebihi kapasitas inverter." },
  { "en": "Apa itu 'DC-to-AC ratio'?", "id": "Rasio daya panel DC ke daya inverter AC." },
  { "en": "Disebut juga?", "id": "Inverter loading ratio." },
  { "en": "Biasanya lebih dari 1?", "id": "Ya, untuk memaksimalkan produksi." },
  { "en": "Apa itu 'wind turbine syndrome'?", "id": "Kumpulan gejala kesehatan yang dikeluhkan." },
  { "en": "Terbukti secara ilmiah?", "id": "Tidak, tidak ada bukti ilmiah kuat." },
  { "en": "Apa itu 'NIMBY'?", "id": "Not In My Backyard." },
  { "en": "Artinya?", "id": "Penolakan proyek di lingkungan sendiri." },
  { "en": "Apa itu 'social license to operate'?", "id": "Penerimaan masyarakat terhadap suatu proyek." },
  { "en": "Penting untuk proyek energi?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'environmental justice'?", "id": "Keadilan lingkungan." },
  { "en": "Terkait apa?", "id": "Distribusi dampak lingkungan yang adil." },
  { "en": "Apa itu 'green gentrification'?", "id": "Peningkatan fasilitas hijau menaikkan harga properti." },
  { "en": "Apa itu 'energy mix'?", "id": "Bauran energi." },
  { "en": "Apa itu 'energy security' vs 'energy sovereignty'?", "id": "Ketersediaan vs kontrol atas energi." },
  { "en": "Apa itu 'geothermal fluid'?", "id": "Campuran air panas dan uap." },
  { "en": "Bisa mengandung mineral?", "id": "Ya, bisa sangat korosif." },
  { "en": "Apa itu 'scaling' di geotermal?", "id": "Penumpukan mineral di dalam pipa." },
  { "en": "Apa itu 'reinjection'?", "id": "Memasukkan kembali fluida ke reservoir." },
  { "en": "Tujuannya?", "id": "Menjaga tekanan reservoir, ramah lingkungan." },
  { "en": "Apa itu 'dry steam' plant?", "id": "PLTP yang menggunakan uap langsung." },
  { "en": "Apa itu 'flash steam' plant?", "id": "PLTP yang mengubah air panas menjadi uap." },
  { "en": "Mana yang paling umum?", "id": "Flash steam." },
  { "en": "Apa itu 'turbine blade erosion'?", "id": "Erosi pada bilah turbin." },
  { "en": "Disebabkan oleh apa?", "id": "Partikel kecil atau tetesan air." },
  { "en": "Apa itu 'O&M'?", "id": "Operation and Maintenance (Operasi dan Pemeliharaan)." },
  { "en": "Penting untuk pembangkit listrik?", "id": "Ya, untuk menjaga performa." },
  { "en": "Apa itu 'remote monitoring'?", "id": "Pemantauan jarak jauh." },
  { "en": "Apa itu 'predictive maintenance'?", "id": "Memprediksi kapan perawatan dibutuhkan." },
  { "en": "Menggunakan apa?", "id": "AI dan machine learning." },
  { "en": "Apa itu 'digital twin'?", "id": "Model digital dari aset fisik." },
  { "en": "Digunakan untuk apa?", "id": "Simulasi, monitoring, dan optimasi." },
  { "en": "Apa itu 'offshore wind'?", "id": "PLTB lepas pantai." },
  { "en": "Fondasinya?", "id": "Tetap (fixed-bottom) atau mengapung (floating)." },
  { "en": "Apa itu 'monopile foundation'?", "id": "Jenis fondasi tetap yang umum." },
  { "en": "Apa itu 'subsea cable'?", "id": "Kabel bawah laut." },
  { "en": "Untuk apa?", "id": "Transmisi listrik dari PLTB lepas pantai." },
  { "en": "Apa itu 'HVAC' vs 'HVDC'?", "id": "Transmisi AC vs DC tegangan tinggi." },
  { "en": "Untuk jarak jauh lebih baik?", "id": "HVDC." },
  { "en": "Apa itu 'voltage'?", "id": "Tegangan." },
  { "en": "Apa itu 'current'?", "id": "Arus." },
  { "en": "Apa itu 'resistance'?", "id": "Hambatan." },
  { "en": "Apa itu 'Ohm's Law'?", "id": "Hubungan antara tegangan, arus, hambatan." },
  { "en": "Formulanya?", "id": "V = I * R." },
  { "en": "Apa itu 'power'?", "id": "Daya." },
  { "en": "Formulanya?", "id": "P = V * I." },
  { "en": "Apa itu 'energy'?", "id": "Energi." },
  { "en": "Formulanya?", "id": "E = P * t (Daya x Waktu)." },
  { "en": "Apa itu 'grid stability'?", "id": "Stabilitas jaringan." },
  { "en": "Penting untuk integrasi EBT?", "id": "Ya, sangat krusial." },
  { "en": "Apa itu 'frequency regulation'?", "id": "Menjaga frekuensi jaringan tetap stabil." },
  { "en": "Baterai bisa membantu?", "id": "Ya, dengan respons yang sangat cepat." },
  { "en": "Apa itu 'voltage support'?", "id": "Menjaga tegangan jaringan tetap stabil." },
  { "en": "Apa itu 'ancillary market'?", "id": "Pasar untuk layanan pendukung jaringan." },
  { "en": "Apa itu 'carbon credit' vs 'carbon tax'?", "id": "Insentif berbasis pasar vs pajak." },
  { "en": "Apa itu 'electrochemical storage'?", "id": "Penyimpanan energi kimia (baterai)." },
  { "en": "Apa itu 'mechanical storage'?", "id": "Penyimpanan energi mekanik." },
  { "en": "Contohnya?", "id": "Pumped hydro, flywheel, CAES." },
  { "en": "Apa itu 'thermal storage'?", "id": "Penyimpanan energi panas." },
  { "en": "Contohnya?", "id": "Garam cair (molten salt)." },
  { "en": "Apa itu 'chemical storage'?", "id": "Penyimpanan dalam bentuk ikatan kimia." },
  { "en": "Contohnya?", "id": "Hidrogen." },
  { "en": "Apa itu 'short-duration storage'?", "id": "Penyimpanan untuk beberapa jam." },
  { "en": "Contohnya?", "id": "Baterai lithium-ion." },
  { "en": "Apa itu 'long-duration storage'?", "id": "Penyimpanan untuk berhari-hari atau musim." },
  { "en": "Contohnya?", "id": "Pumped hydro, hidrogen." },
  { "en": "Apa itu 'flow battery'?", "id": "Baterai dengan elektrolit cair eksternal." },
  { "en": "Keuntungannya?", "id": "Mudah diskalakan untuk durasi panjang." },
  { "en": "Apa itu 'lithium-ion battery'?", "id": "Jenis baterai isi ulang populer." },
  { "en": "Digunakan di mana?", "id": "EV, ponsel, penyimpanan jaringan." },
  { "en": "Apa itu 'anoda'?", "id": "Elektroda negatif baterai." },
  { "en": "Apa itu 'katoda'?", "id": "Elektroda positif baterai." },
  { "en": "Apa itu 'elektrolit'?", "id": "Media penghantar ion antar elektroda." },
  { "en": "Apa itu 'separator'?", "id": "Pemisah antara anoda dan katoda." },
  { "en": "Apa itu 'lithium iron phosphate' (LFP)?", "id": "Jenis katoda baterai lithium-ion." },
  { "en": "Keunggulannya?", "id": "Lebih aman, lebih murah, umur panjang." },
  { "en": "Apa itu 'nickel manganese cobalt' (NMC)?", "id": "Jenis katoda baterai lain." },
  { "en": "Keunggulannya?", "id": "Kepadatan energi lebih tinggi." },
  { "en": "Apa itu 'anoda grafit'?", "id": "Anoda yang umum digunakan saat ini." },
  { "en": "Apa itu 'anoda silikon'?", "id": "Anoda masa depan potensial." },
  { "en": "Keunggulannya?", "id": "Kapasitas penyimpanan jauh lebih tinggi." },
  { "en": "Tantangannya?", "id": "Mengembang dan menyusut saat diisi." },
  { "en": "Daur ulang baterai penting?", "id": "Ya, untuk memulihkan mineral berharga." },
  { "en": "Apa itu 'urban planning'?", "id": "Perencanaan kota." },
  { "en": "Energi terbarukan mempengaruhi perencanaan kota?", "id": "Ya, untuk integrasi PLTS dan EV." },
  { "en": "Apa itu 'transit-oriented development' (TOD)?", "id": "Pembangunan berorientasi pada transportasi publik." },
  { "en": "Mengurangi penggunaan energi?", "id": "Ya, di sektor transportasi." },
  { "en": "Apa itu 'walkability'?", "id": "Tingkat kemudahan berjalan kaki." },
  { "en": "Apa itu 'micro-mobility'?", "id": "Kendaraan kecil untuk perjalanan singkat." },
  { "en": "Contohnya?", "id": "Sepeda listrik, skuter listrik." },
  { "en": "Apa itu 'desalinasi'?", "id": "Proses menghilangkan garam dari air laut." },
  { "en": "Membutuhkan banyak energi?", "id": "Ya, sangat intensif energi." },
  { "en": "Bisa ditenagai energi terbarukan?", "id": "Ya, ini adalah solusi ideal." },
  { "en": "Apa itu 'reverse osmosis'?", "id": "Teknologi desalinasi yang umum." },
  { "en": "Apa itu 'brine'?", "id": "Limbah air dengan konsentrasi garam tinggi." },
  { "en": "Dampak lingkungan 'brine'?", "id": "Bisa merusak ekosistem laut." },
  { "en": "Apa itu 'water-energy nexus'?", "id": "Hubungan saling terkait antara air dan energi." },
  { "en": "Artinya?", "id": "Butuh air untuk energi, butuh energi untuk air." },
  { "en": "Apa itu 'food-energy-water nexus'?", "id": "Hubungan antara pangan, energi, dan air." },
  { "en": "Apa itu 'life cycle'?", "id": "Siklus hidup." },
  { "en": "Apa itu 'cradle to grave'?", "id": "Dari pembuatan hingga pembuangan." },
  { "en": "Apa itu 'cradle to cradle'?", "id": "Dari pembuatan hingga didaur ulang." },
  { "en": "Ekonomi sirkular menggunakan pendekatan?", "id": "Cradle to cradle." },
  { "en": "Apa itu 'Paris Climate Accord'?", "id": "Sinonim untuk Perjanjian Paris." },
  { "en": "Apa itu 'mitigasi'?", "id": "Upaya untuk mengurangi emisi." },
  { "en": "Apa itu 'adaptasi'?", "id": "Menyesuaikan diri dengan dampak perubahan iklim." },
  { "en": "Mana yang lebih penting?", "id": "Keduanya sama-sama penting." },
  { "en": "Energi terbarukan adalah strategi?", "id": "Mitigasi." },
  { "en": "Membangun tanggul laut adalah?", "id": "Adaptasi." },
  { "en": "Apa itu 'carbon sink'?", "id": "Penyerap karbon alami." },
  { "en": "Contohnya?", "id": "Hutan, lautan." },
  { "en": "Apa itu 'deforestasi'?", "id": "Penggundulan hutan." },
  { "en": "Dampaknya pada iklim?", "id": "Mengurangi penyerapan karbon, melepaskan karbon." },
  { "en": "Apa itu 'reforestasi'?", "id": "Penanaman kembali hutan." },
  { "en": "Apa itu 'agroforestri'?", "id": "Menggabungkan pertanian dengan pohon." },
  { "en": "Apa itu 'biofuel' vs 'fosil fuel'?", "id": "Dari biomassa vs dari fosil." },
  { "en": "Keduanya melepaskan CO2 saat dibakar?", "id": "Ya." },
  { "en": "Mengapa biofuel lebih baik?", "id": "Karbonnya bagian dari siklus pendek." },
  { "en": "Apa itu 'carbon cycle'?", "id": "Siklus karbon." },
  { "en": "Apa itu 'fast carbon cycle'?", "id": "Siklus karbon antara atmosfer, lautan, biosfer." },
  { "en": "Apa itu 'slow carbon cycle'?", "id": "Siklus karbon melalui batuan dan sedimen." },
  { "en": "Bahan bakar fosil bagian dari?", "id": "Slow carbon cycle." },
  { "en": "Membakarnya memindahkan karbon ke?", "id": "Fast carbon cycle." },
  { "en": "Apa itu 'acid rain'?", "id": "Hujan asam." },
  { "en": "Disebabkan oleh apa?", "id": "Sulfur dioksida dan nitrogen oksida." },
  { "en": "Sumbernya?", "id": "Pembakaran batu bara dan minyak." },
  { "en": "Energi terbarukan mengurangi hujan asam?", "id": "Ya, secara signifikan." },
  { "en": "Apa itu 'smog'?", "id": "Kabut asap." },
  { "en": "Apa itu 'particulate matter' (PM2.5)?", "id": "Partikel polusi udara sangat kecil." },
  { "en": "Berbahaya bagi kesehatan?", "id": "Ya, sangat berbahaya." },
  { "en": "Energi terbarukan mengurangi PM2.5?", "id": "Ya." },
  { "en": "Apa itu 'public health'?", "id": "Kesehatan masyarakat." },
  { "en": "Transisi energi meningkatkan kesehatan publik?", "id": "Ya, melalui udara yang lebih bersih." },
  { "en": "Apa itu 'energy access'?", "id": "Akses terhadap energi." },
  { "en": "Apa itu 'energy equity'?", "id": "Keadilan dalam akses dan biaya energi." },
  { "en": "Apa itu 'low-income household'?", "id": "Rumah tangga berpenghasilan rendah." },
  { "en": "Beban biaya energi bagi mereka?", "id": "Seringkali lebih tinggi secara proporsional." },
  { "en": "Program efisiensi energi bisa membantu?", "id": "Ya, dengan mengurangi tagihan." },
  { "en": "Apa itu 'weatherization'?", "id": "Membuat bangunan lebih tahan cuaca." },
  { "en": "Tujuannya?", "id": "Mengurangi kebutuhan pemanasan/pendinginan." },
  { "en": "Apa itu 'insulation'?", "id": "Isolasi termal." },
  { "en": "Apa itu 'double-glazed window'?", "id": "Jendela dengan dua lapis kaca." },
  { "en": "Lebih efisien?", "id": "Ya, isolasi lebih baik." },
  { "en": "Apa itu 'cool roof'?", "id": "Atap yang memantulkan lebih banyak cahaya." },
  { "en": "Warnanya?", "id": "Cerah atau putih." },
  { "en": "Mengurangi apa?", "id": "Kebutuhan pendinginan (AC)." },
  { "en": "Apa itu 'geothermal energy'?", "id": "Energi panas bumi." },
  { "en": "Apa itu 'hydropower energy'?", "id": "Energi air." },
  { "en": "Apa itu 'solar energy'?", "id": "Energi surya." },
  { "en": "Apa itu 'wind energy'?", "id": "Energi angin." },
  { "en": "Apa itu 'biomass energy'?", "id": "Energi biomassa." },
  { "en": "Apa itu 'ocean energy'?", "id": "Energi samudra." },
  { "en": "Energi terbarukan yang paling banyak digunakan?", "id": "Tenaga air (hydropower)." },
  { "en": "Energi terbarukan yang paling cepat tumbuh?", "id": "Tenaga surya dan angin." },
  { "en": "Negara mana penghasil surya terbesar?", "id": "Tiongkok." },
  { "en": "Negara mana penghasil angin terbesar?", "id": "Tiongkok." },
  { "en": "Negara mana penghasil hidro terbesar?", "id": "Tiongkok." },
  { "en": "Negara mana penghasil geotermal terbesar?", "id": "Amerika Serikat." },
  { "en": "Indonesia di peringkat berapa geotermal?", "id": "Peringkat kedua di dunia." },
  { "en": "Apa itu 'solar irradiance'?", "id": "Daya radiasi matahari per unit area." },
  { "en": "Apa itu 'peak power' (Wp)?", "id": "Daya puncak panel surya." },
  { "en": "Diukur dalam kondisi apa?", "id": "Kondisi tes standar (STC)." },
  { "en": "Apa itu 'Standard Test Conditions' (STC)?", "id": "Kondisi lab untuk menguji panel." },
  { "en": "Suhunya?", "id": "25 derajat Celsius." },
  { "en": "Iradiasinya?", "id": "1000 Watt per meter persegi." },
  { "en": "Apa itu 'in-situ'?", "id": "Di lokasi atau di tempat." },
  { "en": "Apa itu 'ex-situ'?", "id": "Di luar lokasi atau di tempat lain." },
  { "en": "Apa itu 'fly ash'?", "id": "Abu terbang dari pembakaran batu bara." },
  { "en": "Bisa dimanfaatkan?", "id": "Ya, untuk bahan bangunan." },
  { "en": "Apa itu 'bottom ash'?", "id": "Abu yang tersisa di dasar boiler." },
  { "en": "Apa itu 'flue gas desulfurization' (FGD)?", "id": "Teknologi mengurangi emisi sulfur." },
  { "en": "Digunakan di PLTU?", "id": "Ya, di PLTU modern." },
  { "en": "Apa itu 'elektroplating'?", "id": "Proses pelapisan logam dengan listrik." },
  { "en": "Apa itu 'solar-powered water pump'?", "id": "Pompa air tenaga surya." },
  { "en": "Berguna untuk apa?", "id": "Irigasi di daerah terpencil." },
  { "en": "Apa itu 'desentralisasi' vs 'sentralisasi'?", "id": "Tersebar vs terpusat." },
  { "en": "Jaringan listrik masa depan?", "id": "Cenderung lebih terdesentralisasi." },
  { "en": "Apa itu 'resilience'?", "id": "Ketahanan atau kemampuan untuk pulih." },
  { "en": "Microgrid meningkatkan resiliensi jaringan?", "id": "Ya, saat terjadi pemadaman." },
  { "en": "Apa itu 'blackout'?", "id": "Pemadaman listrik skala besar." },
  { "en": "Apa itu 'brownout'?", "id": "Penurunan tegangan listrik." },
  { "en": "Apa itu 'voltage sag'?", "id": "Penurunan tegangan sesaat." },
  { "en": "Apa itu 'voltage swell'?", "id": "Kenaikan tegangan sesaat." },
  { "en": "Apa itu 'power quality'?", "id": "Kualitas daya." },
  { "en": "Penting untuk industri sensitif?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'uninterruptible power supply' (UPS)?", "id": "Penyedia daya darurat sesaat." },
  { "en": "Menggunakan apa?", "id": "Baterai." },
  { "en": "Apa itu 'genset'?", "id": "Generator set (diesel)." },
  { "en": "Apa itu 'cogeneration'?", "id": "Menghasilkan listrik dan panas bersamaan." },
  { "en": "Disebut juga?", "id": "Combined Heat and Power (CHP)." },
  { "en": "Apa itu 'trigeneration'?", "id": "Menghasilkan listrik, panas, dan pendinginan." },
  { "en": "Disebut juga?", "id": "Combined Cooling, Heat, and Power (CCHP)." },
  { "en": "Apa itu 'energy audit' vs 'energy assessment'?", "id": "Seringkali digunakan secara bergantian." },
  { "en": "Apa itu 'ISO 50001'?", "id": "Standar internasional untuk manajemen energi." },
  { "en": "Apa itu 'LEED'?", "id": "Sertifikasi untuk bangunan hijau." },
  { "en": "LEED singkatan dari?", "id": "Leadership in Energy and Environmental Design." },
  { "en": "Apa itu 'BREEAM'?", "id": "Sistem sertifikasi bangunan hijau lain." },
  { "en": "Apa itu 'passive solar design'?", "id": "Desain bangunan yang memanfaatkan matahari pasif." },
  { "en": "Contohnya?", "id": "Jendela besar menghadap ke selatan." },
  { "en": "Apa itu 'thermal mass'?", "id": "Kemampuan material menyimpan panas." },
  { "en": "Contoh material 'thermal mass'?", "id": "Beton, batu, air." },
  { "en": "Apa itu 'daya reaktif'?", "id": "Daya yang dibutuhkan untuk medan magnet." },
  { "en": "Apa itu 'daya aktif'?", "id": "Daya nyata yang melakukan kerja." },
  { "en": "Apa itu 'faktor daya'?", "id": "Rasio daya aktif terhadap daya semu." },
  { "en": "Nilai idealnya?", "id": "Mendekati 1." },
  { "en": "Inverter modern bisa mengatur ini?", "id": "Ya, disebut 'smart inverter'." },
  { "en": "Apa itu 'smart inverter'?", "id": "Inverter dengan fungsi pendukung jaringan." },
  { "en": "Apa itu 'grid code'?", "id": "Aturan teknis untuk terhubung ke jaringan." },
  { "en": "Pembangkit EBT harus mematuhinya?", "id": "Ya." },
  { "en": "Apa itu 'interconnection'?", "id": "Proses menghubungkan pembangkit ke jaringan." },
  { "en": "Apa itu 'wheeling'?", "id": "Menyalurkan listrik melalui jaringan pihak ketiga." },
  { "en": "Apa itu 'deregulasi' pasar listrik?", "id": "Membuka pasar listrik untuk kompetisi." },
  { "en": "Apa itu 'wholesale electricity market'?", "id": "Pasar listrik grosir." },
  { "en": "Apa itu 'retail electricity market'?", "id": "Pasar listrik eceran." },
  { "en": "Apa itu 'stranded cost'?", "id": "Biaya investasi yang tidak bisa dipulihkan." },
  { "en": "Apa itu 'carbon bubble'?", "id": "Potensi bubble ekonomi aset fosil." },
  { "en": "Apa itu 'divestasi'?", "id": "Lawan dari investasi." },
  { "en": "Gerakan divestasi fosil?", "id": "Gerakan untuk menarik investasi dari fosil." },
  { "en": "Apa itu 'bio-mimicry'?", "id": "Biomimikri." },
  { "en": "Apa itu 'bio-inspired'?", "id": "Terinspirasi oleh biologi." },
  { "en": "Bilah turbin meniru sirip paus?", "id": "Ya, untuk meningkatkan aerodinamika." },
  { "en": "Apa itu 'kelp forest'?", "id": "Hutan kelp (rumput laut raksasa)." },
  { "en": "Bisa jadi sumber biomassa?", "id": "Ya, potensial untuk biofuel." },
  { "en": "Apa itu 'bio-rock'?", "id": "Struktur buatan untuk menumbuhkan terumbu karang." },
  { "en": "Menggunakan apa?", "id": "Arus listrik lemah." },
  { "en": "Apa itu 'ocean fertilization'?", "id": "Menambahkan nutrisi ke laut." },
  { "en": "Tujuannya?", "id": "Meningkatkan penyerapan CO2 oleh fitoplankton." },
  { "en": "Kontroversial?", "id": "Ya, dampaknya belum dipahami sepenuhnya." },
  { "en": "Apa itu 'bio-refinery'?", "id": "Kilang yang mengolah biomassa." },
  { "en": "Menghasilkan apa?", "id": "Biofuel, biokimia, dan bioproduk." },
  { "en": "Apa itu 'green chemistry'?", "id": "Kimia yang ramah lingkungan." },
  { "en": "Prinsipnya?", "id": "Mengurangi limbah dan penggunaan bahan berbahaya." },
  { "en": "Apa itu 'atom economy'?", "id": "Ukuran efisiensi reaksi kimia." },
  { "en": "Apa itu 'katalis'?", "id": "Zat yang mempercepat reaksi kimia." },
  { "en": "Apa itu 'enzim'?", "id": "Katalis biologis." },
  { "en": "Digunakan dalam produksi biofuel?", "id": "Ya, untuk memecah selulosa." },
  { "en": "Apa itu 'selulosa'?", "id": "Komponen utama dinding sel tumbuhan." },
  { "en": "Apa itu 'lignin'?", "id": "Polimer kompleks di kayu." },
  { "en": "Tantangan biofuel generasi kedua?", "id": "Memecah lignoselulosa secara efisien." },
  { "en": "Apa itu 'lignoselulosa'?", "id": "Gabungan lignin, selulosa, hemiselulosa." },
  { "en": "Apa itu 'fermentasi'?", "id": "Proses biokimia untuk menghasilkan etanol." },
  { "en": "Menggunakan apa?", "id": "Ragi atau mikroorganisme lain." },
  { "en": "Apa itu 'distilasi'?", "id": "Proses pemurnian cairan dengan pemanasan." },
  { "en": "Digunakan untuk memurnikan etanol?", "id": "Ya." },
  { "en": "Apa itu 'transesterifikasi'?", "id": "Proses kimia untuk membuat biodiesel." },
  { "en": "Bahan bakunya?", "id": "Minyak nabati atau lemak hewani." },
  { "en": "Produk sampingannya?", "id": "Gliserin." },
  { "en": "Gliserin bisa dimanfaatkan?", "id": "Ya, untuk sabun dan kosmetik." },
  { "en": "Apa itu 'jet fuel'?", "id": "Bahan bakar pesawat." },
  { "en": "Apa itu 'sustainable aviation fuel' (SAF)?", "id": "Bahan bakar pesawat berkelanjutan." },
  { "en": "Bisa dibuat dari biofuel?", "id": "Ya, salah satu sumber utamanya." },
  { "en": "Apa itu 'elektro-fuel' (e-fuel)?", "id": "Bahan bakar sintetis dari hidrogen hijau." },
  { "en": "Disebut juga?", "id": "Power-to-liquid." },
  { "en": "Apa itu 'energy carrier'?", "id": "Pembawa energi." },
  { "en": "Contohnya?", "id": "Listrik, hidrogen, bensin." },
  { "en": "Matahari adalah 'primary source'?", "id": "Ya, sumber energi primer." },
  { "en": "Apa itu 'primary energy'?", "id": "Energi dalam bentuk alaminya." },
  { "en": "Contohnya?", "id": "Batu bara mentah, sinar matahari." },
  { "en": "Apa itu 'secondary energy'?", "id": "Energi yang sudah diolah." },
  { "en": "Contohnya?", "id": "Listrik, bensin." },
  { "en": "Apa itu 'final energy'?", "id": "Energi yang sampai ke konsumen." },
  { "en": "Apa itu 'useful energy'?", "id": "Energi yang benar-benar dimanfaatkan." },
  { "en": "Contohnya?", "id": "Cahaya dari lampu, gerak dari mobil." },
  { "en": "Apa itu 'Sankey diagram'?", "id": "Diagram yang memvisualisasikan aliran energi." },
  { "en": "Menunjukkan apa?", "id": "Energi masuk, terpakai, dan terbuang." },
  { "en": "Apa itu 'curtailment' vs 'outage'?", "id": "Sengaja dimatikan vs tidak sengaja." },
  { "en": "Apa itu 'force majeure'?", "id": "Keadaan kahar atau kejadian luar biasa." },
  { "en": "Bisa mempengaruhi pasokan energi?", "id": "Ya, contohnya badai besar." },
  { "en": "Apa itu 'hardening the grid'?", "id": "Membuat jaringan listrik lebih tangguh." },
  { "en": "Terhadap apa?", "id": "Cuaca ekstrem dan serangan." },
  { "en": "Apa itu 'undergrounding'?", "id": "Memasang kabel listrik di bawah tanah." },
  { "en": "Keuntungannya?", "id": "Melindungi dari badai dan angin." },
  { "en": "Kerugiannya?", "id": "Sangat mahal dan perbaikan sulit." },
  { "en": "Apa itu 'utility pole'?", "id": "Tiang listrik." },
  { "en": "Apa itu 'right-of-way'?", "id": "Jalur tanah untuk infrastruktur utilitas." },
  { "en": "Apa itu 'ornithology'?", "id": "Studi tentang burung." },
  { "en": "Relevan untuk turbin angin?", "id": "Ya, untuk studi dampak pada burung." },
  { "en": "Apa itu 'bat mortality'?", "id": "Kematian kelelawar akibat turbin." },
  { "en": "Solusinya?", "id": "Menghentikan turbin saat aktivitas kelelawar tinggi." },
  { "en": "Apa itu 'ultrasonic acoustic deterrent'?", "id": "Alat pengusir kelelawar dengan suara." },
  { "en": "Apa itu 'fish ladder'?", "id": "Struktur untuk membantu ikan melewati bendungan." },
  { "en": "Apa itu 'sedimentation'?", "id": "Penumpukan sedimen di waduk." },
  { "en": "Dampaknya?", "id": "Mengurangi kapasitas dan umur waduk." },
  { "en": "Apa itu 'decommissioning'?", "id": "Proses membongkar pembangkit listrik." },
  { "en": "Apa itu 'repowering'?", "id": "Mengganti turbin lama dengan yang baru." },
  { "en": "Di 'wind farm' yang sama?", "id": "Ya, di lokasi yang sama." },
  { "en": "Apa itu 'wind atlas'?", "id": "Peta yang menunjukkan sumber daya angin." },
  { "en": "Apa itu 'solar atlas'?", "id": "Peta yang menunjukkan sumber daya surya." },
  { "en": "Apa itu 'GIS'?", "id": "Geographic Information System." },
  { "en": "Digunakan untuk pemetaan energi?", "id": "Ya, sangat luas digunakan." },
  { "en": "Apa itu 'remote sensing'?", "id": "Penginderaan jauh." },
  { "en": "Contohnya?", "id": "Citra satelit." },
  { "en": "Apa itu 'LiDAR'?", "id": "Light Detection and Ranging." },
  { "en": "Digunakan untuk apa?", "id": "Pemetaan 3D, analisis sumber daya angin." },
  { "en": "Apa itu 'NIMBY' vs 'YIMBY'?", "id": "Not In My Back Yard vs Yes In My Back Yard." },
  { "en": "YIMBY mendukung proyek EBT?", "id": "Ya, mendukung pembangunan." },
  { "en": "Apa itu 'citizen science'?", "id": "Sains yang melibatkan partisipasi publik." },
  { "en": "Contoh di energi?", "id": "Memantau kualitas udara lokal." },
  { "en": "Apa itu 'energy literacy'?", "id": "Pemahaman tentang energi." },
  { "en": "Penting untuk transisi energi?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'energy democracy'?", "id": "Demokrasi energi." },
  { "en": "Apa itu 'community choice aggregation' (CCA)?", "id": "Komunitas memilih sumber listrik mereka." },
  { "en": "Apa itu 'cooperative' (koperasi)?", "id": "Organisasi yang dimiliki dan dikelola anggotanya." },
  { "en": "Ada koperasi energi?", "id": "Ya, terutama di Eropa dan AS." },
  { "en": "Apa itu 'pump'?", "id": "Pompa." },
  { "en": "Apa itu 'turbine'?", "id": "Turbin." },
  { "en": "Perbedaannya?", "id": "Pompa menambah energi, turbin mengambil." },
  { "en": "Keduanya adalah 'turbomachinery'?", "id": "Ya." },
  { "en": "Apa itu 'fluid dynamics'?", "id": "Studi tentang fluida yang bergerak." },
  { "en": "Penting untuk desain turbin?", "id": "Ya, sangat fundamental." },
  { "en": "Apa itu 'aerodynamics'?", "id": "Studi tentang udara yang bergerak." },
  { "en": "Penting untuk bilah turbin angin?", "id": "Ya." },
  { "en": "Apa itu 'hydrodynamics'?", "id": "Studi tentang air yang bergerak." },
  { "en": "Penting untuk turbin air/tidal?", "id": "Ya." },
  { "en": "Apa itu 'lift'?", "id": "Gaya angkat." },
  { "en": "Apa itu 'drag'?", "id": "Gaya hambat." },
  { "en": "Bilah turbin angin bekerja seperti?", "id": "Sayap pesawat (airfoil)." },
  { "en": "Menghasilkan gaya apa?", "id": "Gaya angkat (lift) untuk memutar." },
  { "en": "Apa itu 'gearbox'?", "id": "Girboks atau kotak roda gigi." },
  { "en": "Fungsinya di turbin angin?", "id": "Meningkatkan kecepatan putaran ke generator." },
  { "en": "Apa itu 'direct-drive'?", "id": "Tanpa girboks." },
  { "en": "Generatornya harus bagaimana?", "id": "Lebih besar dan berputar lambat." },
  { "en": "Apa itu 'permanent magnet generator'?", "id": "Generator dengan magnet permanen." },
  { "en": "Sering digunakan di turbin 'direct-drive'?", "id": "Ya." },
  { "en": "Apa itu 'induction generator'?", "id": "Jenis generator umum lainnya." },
  { "en": "Apa itu 'synchronous' vs 'asynchronous' generator?", "id": "Sinkron vs asinkron." },
  { "en": "Apa itu 'grid frequency'?", "id": "Frekuensi jaringan." },
  { "en": "Generator harus sinkron dengan?", "id": "Frekuensi jaringan." },
  { "en": "Apa itu 'phase'?", "id": "Fasa." },
  { "en": "Listrik di rumah satu fasa?", "id": "Ya, umumnya." },
  { "en": "Transmisi listrik tiga fasa?", "id": "Ya, untuk efisiensi." },
  { "en": "Apa itu 'rectifier'?", "id": "Mengubah AC menjadi DC." },
  { "en": "Apa itu 'inverter'?", "id": "Mengubah DC menjadi AC." },
  { "en": "Apa itu 'converter'?", "id": "Mengubah DC ke DC atau AC ke AC." },
  { "en": "Apa itu 'power electronics'?", "id": "Elektronika daya." },
  { "en": "Kunci untuk energi terbarukan?", "id": "Ya, untuk konversi dan kontrol." },
  { "en": "Bahan semikonduktor untuk 'power electronics'?", "id": "Silikon (Si), Silikon Karbida (SiC)." },
  { "en": "Apa itu 'silicon carbide' (SiC)?", "id": "Semikonduktor 'wide-bandgap'." },
  { "en": "Keunggulannya?", "id": "Lebih efisien pada tegangan tinggi." },
  { "en": "Apa itu 'gallium nitride' (GaN)?", "id": "Semikonduktor 'wide-bandgap' lain." },
  { "en": "Digunakan di mana?", "id": "Charger cepat, inverter efisien." },
  { "en": "Apa itu 'heat sink'?", "id": "Penyerap panas." },
  { "en": "Komponen elektronika daya butuh?", "id": "Ya, untuk membuang panas." },
  { "en": "Apa itu 'thermal management'?", "id": "Manajemen termal atau panas." },
  { "en": "Apa itu 'load'?", "id": "Beban (konsumsi listrik)." },
  { "en": "Apa itu 'load forecasting'?", "id": "Prediksi beban listrik." },
  { "en": "Apa itu 'supply'?", "id": "Pasokan (pembangkitan listrik)." },
  { "en": "Jaringan harus selalu seimbang?", "id": "Ya, pasokan harus sama dengan beban." },
  { "en": "Apa itu 'balancing'?", "id": "Proses menyeimbangkan jaringan." },
  { "en": "Siapa yang melakukannya?", "id": "Operator sistem (system operator)." },
  { "en": "Apa itu 'dispatch'?", "id": "Perintah untuk pembangkit beroperasi." },
  { "en": "Apa itu 'merit order'?", "id": "Urutan dispatch berdasarkan biaya." },
  { "en": "Pembangkit mana yang didahulukan?", "id": "Yang biaya marginalnya paling rendah." },
  { "en": "Energi terbarukan punya biaya marginal?", "id": "Sangat rendah (mendekati nol)." },
  { "en": "Artinya?", "id": "Selalu didahulukan dalam merit order." },
  { "en": "Apa itu 'electricity market'?", "id": "Pasar listrik." },
  { "en": "Apa itu 'spot price'?", "id": "Harga listrik saat ini." },
  { "en": "Bisa negatif?", "id": "Ya, saat pasokan EBT berlebih." },
  { "en": "Artinya?", "id": "Produsen membayar agar listriknya dipakai." },
  { "en": "Apa itu 'curtailment risk'?", "id": "Risiko output energi harus dipotong." },
  { "en": "Apa itu 'merchant risk'?", "id": "Risiko dari fluktuasi harga pasar." },
  { "en": "Apa itu 'hedging'?", "id": "Strategi untuk mengurangi risiko harga." },
  { "en": "Apa itu 'financial derivative'?", "id": "Kontrak keuangan turunan." },
  { "en": "Apa itu 'futures contract'?", "id": "Kontrak untuk membeli/menjual di masa depan." },
  { "en": "Apa itu 'option'?", "id": "Hak (bukan kewajiban) membeli/menjual." },
  { "en": "Digunakan di pasar energi?", "id": "Ya, untuk manajemen risiko." },
  { "en": "Apa itu 'project finance'?", "id": "Pendanaan proyek berdasarkan arus kas." },
  { "en": "Apa itu 'non-recourse debt'?", "id": "Utang yang dijamin oleh proyek itu sendiri." },
  { "en": "Apa itu 'special purpose vehicle' (SPV)?", "id": "Perusahaan yang dibentuk untuk satu proyek." },
  { "en": "Apa itu 'due diligence'?", "id": "Uji tuntas sebelum investasi." },
  { "en": "Apa itu 'bankability'?", "id": "Kelayakan proyek untuk didanai bank." },
  { "en": "Apa itu 'off-taker'?", "id": "Pihak yang membeli output proyek." },
  { "en": "Dalam PPA, siapa 'off-taker'?", "id": "PLN atau perusahaan besar." },
  { "en": "Apa itu 'grid connection study'?", "id": "Studi untuk menghubungkan ke jaringan." },
  { "en": "Apa itu 'environmental permit'?", "id": "Izin lingkungan." },
  { "en": "Apa itu 'social permit'?", "id": "Persetujuan dari masyarakat lokal." },
  { "en": "Apa itu 'permitting risk'?", "id": "Risiko gagal mendapatkan izin." },
  { "en": "Apa itu 'construction risk'?", "id": "Risiko selama fase konstruksi." },
  { "en": "Apa itu 'operational risk'?", "id": "Risiko selama fase operasi." },
  { "en": "Apa itu 'political risk'?", "id": "Risiko dari perubahan kebijakan politik." },
  { "en": "Apa itu 'currency risk'?", "id": "Risiko dari fluktuasi nilai tukar." },
  { "en": "Apa itu 'force majeure risk'?", "id": "Risiko kejadian luar biasa." },
  { "en": "Apa itu 'risk allocation'?", "id": "Alokasi risiko antar pihak." },
  { "en": "Apa itu 'mitigasi risiko'?", "id": "Upaya untuk mengurangi risiko." },
  { "en": "Asuransi adalah bentuk mitigasi?", "id": "Ya, mitigasi risiko finansial." },
  { "en": "Apa itu 'Public-Private Partnership' (PPP)?", "id": "Kerja Sama Pemerintah dengan Badan Usaha." },
  { "en": "Apa itu 'Build-Own-Operate-Transfer' (BOOT)?", "id": "Skema proyek infrastruktur." },
  { "en": "Apa itu 'sovereign wealth fund'?", "id": "Dana investasi milik negara." },
  { "en": "Apa itu 'pension fund'?", "id": "Dana pensiun." },
  { "en": "Bisa berinvestasi di EBT?", "id": "Ya, semakin banyak yang melakukannya." },
  { "en": "Apa itu 'impact investing'?", "id": "Investasi untuk dampak sosial/lingkungan." },
  { "en": "Apa itu 'angel investor'?", "id": "Investor individu pada tahap awal." },
  { "en": "Apa itu 'venture capital'?", "id": "Modal ventura untuk startup." },
  { "en": "Apa itu 'private equity'?", "id": "Investasi pada perusahaan swasta." },
  { "en": "Apa itu 'initial public offering' (IPO)?", "id": "Penawaran saham perdana ke publik." },
  { "en": "Apa itu 'stock market'?", "id": "Pasar saham." },
  { "en": "Apa itu 'bond market'?", "id": "Pasar obligasi." },
  { "en": "Green bond bagian dari ini?", "id": "Ya." },
  { "en": "Apa itu 'credit rating'?", "id": "Peringkat kredit." },
  { "en": "Mempengaruhi biaya pinjaman?", "id": "Ya, sangat mempengaruhi." },
  { "en": "Apa itu 'interest rate'?", "id": "Suku bunga." },
  { "en": "Apa itu 'inflation'?", "id": "Inflasi." },
  { "en": "Apa itu 'GDP'?", "id": "Gross Domestic Product (Produk Domestik Bruto)." },
  { "en": "Apa itu 'energy intensity'?", "id": "Energi yang dibutuhkan per unit PDB." },
  { "en": "Negara efisien punya 'energy intensity'?", "id": "Rendah." },
  { "en": "Apa itu 'decoupling'?", "id": "Memisahkan pertumbuhan ekonomi dari emisi." },
  { "en": "Tujuan utamanya?", "id": "Ekonomi tumbuh, emisi turun." },
  { "en": "Energi terbarukan kunci untuk ini?", "id": "Ya, kunci utama." },
  { "en": "Apa itu 'Jevons paradox'?", "id": "Efisiensi lebih tinggi bisa meningkatkan konsumsi." },
  { "en": "Apa itu 'rebound effect'?", "id": "Penghematan energi berkurang karena perilaku." },
  { "en": "Apa itu 'demand side management' (DSM)?", "id": "Manajemen dari sisi permintaan." },
  { "en": "Contohnya?", "id": "Efisiensi energi, demand response." },
  { "en": "Apa itu 'supply side management'?", "id": "Manajemen dari sisi pasokan." },
  { "en": "Contohnya?", "id": "Membangun pembangkit baru." },
  { "en": "Fokus masa depan?", "id": "Lebih ke manajemen sisi permintaan." },
  { "en": "Apa itu 'energy system modeling'?", "id": "Pemodelan sistem energi." },
  { "en": "Digunakan untuk apa?", "id": "Perencanaan energi jangka panjang." },
  { "en": "Apa itu 'optimization'?", "id": "Menemukan solusi terbaik." },
  { "en": "Apa itu 'simulation'?", "id": "Meniru perilaku sistem." },
  { "en": "Apa itu 'stochastic modeling'?", "id": "Pemodelan yang melibatkan ketidakpastian." },
  { "en": "Penting untuk EBT?", "id": "Ya, karena sifatnya yang intermiten." },
  { "en": "Apa itu 'tipping point'?", "id": "Titik kritis perubahan tak bisa balik." },
  { "en": "Perubahan iklim punya 'tipping points'?", "id": "Ya, dikhawatirkan punya." },
  { "en": "Contohnya?", "id": "Mencairnya lapisan es permanen." },
  { "en": "Apa itu 'permafrost'?", "id": "Tanah beku permanen." },
  { "en": "Jika mencair melepaskan apa?", "id": "Metana dan CO2." },
  { "en": "Apa itu 'feedback loop'?", "id": "Lingkaran umpan balik." },
  { "en": "Mencairnya permafrost adalah contoh?", "id": "Positive feedback loop (memperkuat pemanasan)." },
  { "en": "Apa itu 'global dimming'?", "id": "Pengurangan radiasi matahari oleh polusi." },
  { "en": "Apa itu 'aerosol'?", "id": "Partikel kecil di atmosfer." },
  { "en": "Bisa memantulkan sinar matahari?", "id": "Ya." },
  { "en": "Apa itu 'geoengineering'?", "id": "Rekayasa iklim skala besar." },
  { "en": "Contohnya?", "id": "Menyebar aerosol di stratosfer." },
  { "en": "Sangat kontroversial?", "id": "Ya, risikonya tidak diketahui." },
  { "en": "Apa itu 'solar radiation management' (SRM)?", "id": "Upaya mengurangi radiasi matahari." },
  { "en": "Apa itu 'carbon dioxide removal' (CDR)?", "id": "Upaya menghilangkan CO2 dari atmosfer." },
  { "en": "BECCS adalah contoh?", "id": "CDR." },
  { "en": "Direct Air Capture adalah contoh?", "id": "CDR." },
  { "en": "Apa itu 'bio-oil'?", "id": "Minyak dari pirolisis biomassa." },
  { "en": "Apa itu 'syngas'?", "id": "Gas sintetis dari gasifikasi." },
  { "en": "Komponen utamanya?", "id": "Hidrogen dan karbon monoksida." },
  { "en": "Bisa diubah menjadi bahan bakar cair?", "id": "Ya, melalui proses Fischer-Tropsch." },
  { "en": "Apa itu 'Fischer-Tropsch process'?", "id": "Proses kimia mengubah syngas." },
  { "en": "Apa itu 'amonia hijau'?", "id": "Amonia dari hidrogen hijau." },
  { "en": "Bisa jadi bahan bakar kapal?", "id": "Ya, kandidat utama." },
  { "en": "Apa itu 'bunkering'?", "id": "Proses pengisian bahan bakar kapal." },
  { "en": "Apa itu 'IMO'?", "id": "International Maritime Organization." },
  { "en": "Mengatur emisi perkapalan?", "id": "Ya." },
  { "en": "Apa itu 'ICAO'?", "id": "International Civil Aviation Organization." },
  { "en": "Mengatur emisi penerbangan?", "id": "Ya." },
  { "en": "Apa itu 'CORSIA'?", "id": "Skema offsetting karbon untuk penerbangan." },
  { "en": "Apa itu 'embodied carbon'?", "id": "Emisi karbon dari pembuatan material." },
  { "en": "Bangunan punya 'embodied carbon'?", "id": "Ya, dari semen, baja, dll." },
  { "en": "Apa itu 'operational carbon'?", "id": "Emisi karbon dari operasional bangunan." },
  { "en": "Contohnya?", "id": "Listrik untuk lampu dan AC." },
  { "en": "Apa itu 'whole-life carbon'?", "id": "Gabungan embodied dan operational carbon." },
  { "en": "Apa itu 'urban metabolism'?", "id": "Analogi kota sebagai organisme hidup." },
  { "en": "Mempelajari apa?", "id": "Aliran energi dan material kota." },
  { "en": "Apa itu 'industrial symbiosis'?", "id": "Simbiosis industri." },
  { "en": "Artinya?", "id": "Limbah satu industri jadi bahan baku industri lain." },
  { "en": "Contoh di energi?", "id": "Panas buangan pabrik untuk district heating." },
  { "en": "Apa itu 'Kalundborg Eco-industrial Park'?", "id": "Contoh simbiosis industri terkenal." },
  { "en": "Lokasinya di mana?", "id": "Denmark." },
  { "en": "Apa itu 'demand shifting'?", "id": "Menggeser penggunaan listrik ke waktu lain." },
  { "en": "Contohnya?", "id": "Mengisi daya mobil listrik di malam hari." },
  { "en": "Apa itu 'electrification of heat'?", "id": "Menggunakan listrik untuk pemanasan." },
  { "en": "Contoh teknologinya?", "id": "Pompa panas (heat pump)." },
  { "en": "Apa itu 'heat pump'?", "id": "Alat yang memindahkan panas." },
  { "en": "Bekerja seperti apa?", "id": "Seperti kulkas, tapi bisa dibalik." },
  { "en": "Sangat efisien?", "id": "Ya, bisa lebih dari 100% efisien." },
  { "en": "Bagaimana bisa?", "id": "Karena memindahkan, bukan menciptakan panas." },
  { "en": "Jenis 'heat pump'?", "id": "Air-source, ground-source, water-source." },
  { "en": "Apa itu 'air-source heat pump'?", "id": "Mengambil panas dari udara luar." },
  { "en": "Apa itu 'ground-source heat pump'?", "id": "Mengambil panas dari dalam tanah." },
  { "en": "Disebut juga?", "id": "Geothermal heat pump." },
  { "en": "Apa itu 'coefficient of performance' (COP)?", "id": "Ukuran efisiensi pompa panas." },
  { "en": "COP tinggi berarti?", "id": "Sangat efisien." },
  { "en": "Apa itu 'seasonal performance factor' (SPF)?", "id": "COP rata-rata selama satu musim." },
  { "en": "Apa itu 'thermal comfort'?", "id": "Kenyamanan termal." },
  { "en": "Apa itu 'building envelope'?", "id": "Selubung bangunan (dinding, atap, jendela)." },
  { "en": "Penting untuk efisiensi energi?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'air tightness'?", "id": "Kekedapan udara sebuah bangunan." },
  { "en": "Apa itu 'blower door test'?", "id": "Tes untuk mengukur kekedapan udara." },
  { "en": "Apa itu 'thermal bridge'?", "id": "Jembatan termal." },
  { "en": "Artinya?", "id": "Area di mana panas mudah lolos." },
  { "en": "Apa itu 'infrared thermography'?", "id": "Teknik melihat panas dengan kamera." },
  { "en": "Bisa mendeteksi 'thermal bridge'?", "id": "Ya, sangat efektif." },
  { "en": "Apa itu 'fenestration'?", "id": "Susunan jendela dan pintu." },
  { "en": "Apa itu 'U-value'?", "id": "Ukuran seberapa cepat panas lolos." },
  { "en": "U-value rendah berarti?", "id": "Isolasi yang baik." },
  { "en": "Apa itu 'R-value'?", "id": "Ukuran ketahanan terhadap aliran panas." },
  { "en": "R-value tinggi berarti?", "id": "Isolasi yang baik." },
  { "en": "Apa itu 'low-e coating'?", "id": "Lapisan emisivitas rendah pada kaca." },
  { "en": "Fungsinya?", "id": "Memantulkan panas, menjaga suhu dalam." },
  { "en": "Apa itu 'shading device'?", "id": "Alat peneduh." },
  { "en": "Contohnya?", "id": "Overhang, kisi-kisi (louvers)." },
  { "en": "Apa itu 'natural ventilation'?", "id": "Ventilasi alami." },
  { "en": "Apa itu 'stack effect'?", "id": "Udara panas naik, udara dingin turun." },
  { "en": "Digunakan untuk ventilasi alami?", "id": "Ya." },
  { "en": "Apa itu 'cross ventilation'?", "id": "Ventilasi silang." },
  { "en": "Apa itu 'heat recovery ventilation' (HRV)?", "id": "Ventilasi dengan pemulihan panas." },
  { "en": "Apa itu 'energy recovery ventilation' (ERV)?", "id": "Ventilasi dengan pemulihan energi (panas & kelembaban)." },
  { "en": "Apa itu 'sick building syndrome'?", "id": "Gejala kesehatan terkait bangunan." },
  { "en": "Disebabkan oleh?", "id": "Kualitas udara dalam ruangan buruk." },
  { "en": "Apa itu 'indoor air quality' (IAQ)?", "id": "Kualitas udara dalam ruangan." },
  { "en": "Apa itu 'volatile organic compounds' (VOCs)?", "id": "Senyawa organik mudah menguap." },
  { "en": "Sumber VOC?", "id": "Cat, furnitur baru, pembersih." },
  { "en": "Berbahaya?", "id": "Ya, bisa berbahaya bagi kesehatan." },
  { "en": "Apa itu 'HVAC'?", "id": "Heating, Ventilation, and Air Conditioning." },
  { "en": "Sistem tata udara bangunan?", "id": "Ya." },
  { "en": "Apa itu 'chiller'?", "id": "Mesin pendingin air." },
  { "en": "Apa itu 'boiler'?", "id": "Mesin pemanas air." },
  { "en": "Apa itu 'cooling tower'?", "id": "Menara pendingin." },
  { "en": "Apa itu 'air handling unit' (AHU)?", "id": "Unit penanganan udara." },
  { "en": "Apa itu 'ductwork'?", "id": "Jaringan saluran udara." },
  { "en": "Apa itu 'variable frequency drive' (VFD)?", "id": "Mengatur kecepatan motor secara efisien." },
  { "en": "Apa itu 'building automation system' (BAS)?", "id": "Sistem otomatisasi bangunan." },
  { "en": "Tujuannya?", "id": "Mengontrol HVAC, pencahayaan, keamanan." },
  { "en": "Apa itu 'lighting control'?", "id": "Sistem kontrol pencahayaan." },
  { "en": "Contohnya?", "id": "Sensor gerak, peredup (dimmer)." },
  { "en": "Apa itu 'LED'?", "id": "Light Emitting Diode." },
  { "en": "Sangat efisien?", "id": "Ya, jauh lebih efisien." },
  { "en": "Apa itu 'lumen'?", "id": "Satuan output cahaya." },
  { "en": "Apa itu 'lux'?", "id": "Satuan iluminasi (cahaya per area)." },
  { "en": "Apa itu 'efficacy' (pencahayaan)?", "id": "Lumen per Watt." },
  { "en": "Efficacy tinggi berarti?", "id": "Lampu sangat efisien." },
  { "en": "Apa itu 'color rendering index' (CRI)?", "id": "Kemampuan lampu menunjukkan warna asli." },
  { "en": "CRI tinggi berarti?", "id": "Warna terlihat alami." },
  { "en": "Apa itu 'color temperature'?", "id": "Warna cahaya (hangat atau dingin)." },
  { "en": "Diukur dalam apa?", "id": "Kelvin (K)." },
  { "en": "Cahaya hangat punya Kelvin?", "id": "Rendah (sekitar 2700K)." },
  { "en": "Cahaya dingin punya Kelvin?", "id": "Tinggi (sekitar 5000K)." },
  { "en": "Apa itu 'energy modeling' (bangunan)?", "id": "Simulasi penggunaan energi bangunan." },
  { "en": "Digunakan saat apa?", "id": "Tahap desain." },
  { "en": "Apa itu 'commissioning'?", "id": "Proses memastikan sistem bekerja sesuai desain." },
  { "en": "Apa itu 'retro-commissioning'?", "id": "Commissioning untuk bangunan yang sudah ada." },
  { "en": "Apa itu 'energy performance contract' (EPC)?", "id": "Kontrak di mana penghematan energi dijamin." },
  { "en": "Apa itu 'ESCO'?", "id": "Energy Service Company." },
  { "en": "Perusahaan yang menawarkan EPC?", "id": "Ya." },
  { "en": "Apa itu 'submetering'?", "id": "Memasang meteran tambahan untuk memantau." },
  { "en": "Apa itu 'measurement and verification' (M&V)?", "id": "Mengukur dan memverifikasi penghematan energi." },
  { "en": "Apa itu 'ISO 14001'?", "id": "Standar internasional untuk manajemen lingkungan." },
  { "en": "Apa itu 'environmental management system' (EMS)?", "id": "Sistem manajemen lingkungan." },
  { "en": "Apa itu 'carbon accounting'?", "id": "Akuntansi karbon." },
  { "en": "Apa itu 'scope 1 emissions'?", "id": "Emisi langsung dari sumber milik sendiri." },
  { "en": "Apa itu 'scope 2 emissions'?", "id": "Emisi tidak langsung dari listrik dibeli." },
  { "en": "Apa itu 'scope 3 emissions'?", "id": "Semua emisi tidak langsung lainnya." },
  { "en": "Contoh 'scope 1'?", "id": "Emisi dari boiler milik perusahaan." },
  { "en": "Contoh 'scope 3'?", "id": "Emisi dari perjalanan bisnis karyawan." },
  { "en": "REC bisa mengurangi emisi scope 2?", "id": "Ya, itu salah satu tujuannya." },
  { "en": "Apa itu 'carbon disclosure project' (CDP)?", "id": "Organisasi yang mendorong pelaporan lingkungan." },
  { "en": "Apa itu 'corporate social responsibility' (CSR)?", "id": "Tanggung jawab sosial perusahaan." },
  { "en": "Apa itu 'green marketing'?", "id": "Pemasaran produk berbasis klaim lingkungan." },
  { "en": "Apa itu 'eco-label'?", "id": "Label lingkungan pada produk." },
  { "en": "Apa itu 'fair trade'?", "id": "Perdagangan yang adil." },
  { "en": "Ada sertifikasi fair trade?", "id": "Ya." },
  { "en": "Apa itu 'bio-architecture' vs 'green architecture'?", "id": "Sering digunakan secara bergantian." },
  { "en": "Apa itu 'living building challenge'?", "id": "Standar bangunan hijau yang sangat ketat." },
  { "en": "Apa itu 'biophilia'?", "id": "Kecenderungan manusia terhubung dengan alam." },
  { "en": "Apa itu 'biophilic design'?", "id": "Desain yang mengintegrasikan elemen alam." },
  { "en": "Tujuannya?", "id": "Meningkatkan kesejahteraan penghuni." },
  { "en": "Apa itu 'hydrology'?", "id": "Ilmu tentang air." },
  { "en": "Penting untuk PLTA?", "id": "Ya, untuk menghitung debit air." },
  { "en": "Apa itu 'debit'?", "id": "Volume air yang mengalir per waktu." },
  { "en": "Apa itu 'watershed'?", "id": "Daerah Aliran Sungai (DAS)." },
  { "en": "Apa itu 'meteorology'?", "id": "Ilmu tentang atmosfer dan cuaca." },
  { "en": "Penting untuk PLTS dan PLTB?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'climatology'?", "id": "Ilmu tentang iklim." },
  { "en": "Apa itu 'oceanography'?", "id": "Ilmu tentang lautan." },
  { "en": "Penting untuk energi samudra?", "id": "Ya." },
  { "en": "Apa itu 'geology'?", "id": "Ilmu tentang bumi dan batuan." },
  { "en": "Penting untuk PLTP?", "id": "Ya, untuk menemukan reservoir panas bumi." },
  { "en": "Apa itu 'seismic survey'?", "id": "Survei seismik." },
  { "en": "Digunakan untuk eksplorasi geotermal?", "id": "Ya." },
  { "en": "Apa itu 'drilling'?", "id": "Pengeboran." },
  { "en": "Apa itu 'oil rig'?", "id": "Anjungan minyak." },
  { "en": "Bisa dimanfaatkan untuk PLTB lepas pantai?", "id": "Ya, ada konsep untuk itu." },
  { "en": "Apa itu 'decommissioned oil rig'?", "id": "Anjungan minyak yang sudah tidak dipakai." },
  { "en": "Apa itu 'rigs-to-reefs'?", "id": "Mengubah anjungan menjadi terumbu karang buatan." },
  { "en": "Apa itu 'artificial reef'?", "id": "Terumbu karang buatan." },
  { "en": "Apa itu 'marine protected area' (MPA)?", "id": "Kawasan konservasi perairan." },
  { "en": "PLTB lepas pantai bisa jadi MPA?", "id": "Ya, area di sekitarnya bisa berfungsi sebagai MPA." },
  { "en": "Apa itu 'co-location'?", "id": "Menempatkan beberapa jenis EBT di lokasi sama." },
  { "en": "Contohnya?", "id": "PLTS dan PLTB di satu lahan." },
  { "en": "Keuntungannya?", "id": "Berbagi infrastruktur, output lebih stabil." },
  { "en": "Apa itu 'hybrid power plant'?", "id": "Pembangkit listrik hibrida." },
  { "en": "Contohnya?", "id": "PLTS dengan penyimpanan baterai." },
  { "en": "Apa itu 'coltan'?", "id": "Mineral untuk kapasitor elektronik." },
  { "en": "Apa itu 'rare earth elements'?", "id": "Logam tanah jarang." },
  { "en": "Digunakan di mana?", "id": "Magnet permanen untuk turbin angin." },
  { "en": "Apa itu 'conflict minerals'?", "id": "Mineral yang ditambang di zona konflik." },
  { "en": "Apa itu 'ethical sourcing'?", "id": "Pengadaan bahan baku yang etis." },
  { "en": "Apa itu 'supply chain transparency'?", "id": "Transparansi rantai pasok." },
  { "en": "Apa itu 'gig economy'?", "id": "Ekonomi berbasis pekerjaan sementara/lepas." },
  { "en": "Apa itu 'green jobs'?", "id": "Pekerjaan hijau." },
  { "en": "Instalasi panel surya adalah 'green job'?", "id": "Ya." },
  { "en": "Apa itu 'O&M technician'?", "id": "Teknisi Operasi dan Pemeliharaan." },
  { "en": "Apa itu 'windsmith'?", "id": "Teknisi turbin angin." },
  { "en": "Apa itu 'materials science'?", "id": "Ilmu material." },
  { "en": "Penting untuk EBT?", "id": "Ya, untuk mengembangkan material baru." },
  { "en": "Contohnya?", "id": "Sel surya lebih efisien, bilah lebih ringan." },
  { "en": "Apa itu 'composite material'?", "id": "Material komposit." },
  { "en": "Bilah turbin angin terbuat dari?", "id": "Komposit (fiberglass, carbon fiber)." },
  { "en": "Apa itu 'carbon fiber'?", "id": "Serat karbon." },
  { "en": "Kuat dan ringan?", "id": "Ya." },
  { "en": "Apa itu 'graphene'?", "id": "Lapisan tunggal atom karbon." },
  { "en": "Potensinya untuk energi?", "id": "Baterai, superkapasitor, sel surya." },
  { "en": "Apa itu 'nanotechnology'?", "id": "Teknologi skala nano." },
  { "en": "Bisa meningkatkan efisiensi sel surya?", "id": "Ya, area riset yang menjanjikan." },
  { "en": "Apa itu 'quantum mechanics'?", "id": "Mekanika kuantum." },
  { "en": "Efek fotovoltaik dijelaskan oleh?", "id": "Mekanika kuantum." },
  { "en": "Apa itu 'photonics'?", "id": "Ilmu tentang cahaya." },
  { "en": "Apa itu 'optics'?", "id": "Optik." },
  { "en": "Penting untuk CSP?", "id": "Ya, untuk desain cermin dan lensa." },
  { "en": "Apa itu 'concentrating solar power'?", "id": "CSP." },
  { "en": "Apa itu 'concentrator'?", "id": "Alat untuk memfokuskan cahaya." },
  { "en": "Apa itu 'receiver'?", "id": "Penerima panas yang difokuskan." },
  { "en": "Apa itu 'heat transfer fluid' (HTF)?", "id": "Fluida transfer panas." },
  { "en": "Contohnya?", "id": "Minyak sintetis, garam cair." },
  { "en": "Apa itu 'steam generator'?", "id": "Generator uap." },
  { "en": "Apa itu 'condenser'?", "id": "Kondensor." },
  { "en": "Fungsinya?", "id": "Mengubah uap kembali menjadi air." },
  { "en": "Membutuhkan pendinginan?", "id": "Ya, dengan air atau udara." },
  { "en": "Apa itu 'dry cooling'?", "id": "Pendinginan dengan udara." },
  { "en": "Keuntungannya?", "id": "Menghemat air." },
  { "en": "Kerugiannya?", "id": "Kurang efisien, lebih mahal." },
  { "en": "Penting di daerah kering?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'hydropower' vs 'hydrokinetic'?", "id": "Berbasis 'head' vs berbasis aliran." },
  { "en": "Turbin tidal adalah hidrokinetik?", "id": "Ya." },
  { "en": "Apa itu 'biomimicry' vs 'bio-inspired'?", "id": "Meniru alam vs terinspirasi alam." },
  { "en": "Apa itu 'positive feedback'?", "id": "Umpan balik yang memperkuat." },
  { "en": "Apa itu 'negative feedback'?", "id": "Umpan balik yang menstabilkan." },
  { "en": "Sistem iklim punya keduanya?", "id": "Ya." },
  { "en": "Apa itu 'electrocatalysis'?", "id": "Katalisis reaksi elektrokimia." },
  { "en": "Penting untuk fuel cell/elektroliser?", "id": "Ya, sangat penting." },
  { "en": "Katalis yang umum digunakan?", "id": "Platinum." },
  { "en": "Mahal dan langka?", "id": "Ya." },
  { "en": "Tujuan riset?", "id": "Mencari katalis alternatif yang murah." },
  { "en": "Apa itu 'circularity'?", "id": "Sirkularitas." },
  { "en": "Apa itu 'upcycling'?", "id": "Daur ulang menjadi produk bernilai lebih tinggi." },
  { "en": "Apa itu 'downcycling'?", "id": "Daur ulang menjadi produk bernilai lebih rendah." },
  { "en": "Apa itu 'waste hierarchy'?", "id": "Hierarki pengelolaan sampah." },
  { "en": "Urutannya?", "id": "Reduce, Reuse, Recycle." },
  { "en": "Apa itu 'Reduce'?", "id": "Mengurangi." },
  { "en": "Apa itu 'Reuse'?", "id": "Menggunakan kembali." },
  { "en": "Apa itu 'Recycle'?", "id": "Mendaur ulang." },
  { "en": "Apa itu 'Energy Star'?", "id": "Label efisiensi energi untuk produk." },
  { "en": "Apa itu 'vampire load'?", "id": "Beban vampir atau daya siaga." },
  { "en": "Solusinya?", "id": "Mencabut steker, menggunakan stop kontak pintar." },
  { "en": "Apa itu 'smart plug'?", "id": "Stop kontak pintar." },
  { "en": "Bisa dikontrol dari jarak jauh?", "id": "Ya, melalui aplikasi ponsel." },
  { "en": "Apa itu 'home energy audit'?", "id": "Audit energi untuk rumah." },
  { "en": "Bisa dilakukan sendiri?", "id": "Ya, ada versi sederhananya." },
  { "en": "Apa itu 'thermal imaging camera'?", "id": "Kamera pencitraan termal." },
  { "en": "Bisa mendeteksi kebocoran panas?", "id": "Ya, sangat efektif." },
  { "en": "Apa itu 'carbon tax' vs 'carbon trading'?", "id": "Pajak vs sistem pasar." },
  { "en": "Keduanya adalah 'carbon pricing'?", "id": "Ya, mekanisme penetapan harga karbon." },
  { "en": "Apa itu 'tax credit'?", "id": "Kredit pajak." },
  { "en": "Insentif untuk EBT sering berupa?", "id": "Kredit pajak atau subsidi." },
  { "en": "Apa itu 'geothermal tourism'?", "id": "Pariwisata panas bumi." },
  { "en": "Contohnya?", "id": "Pemandian air panas alami." },
  { "en": "Islandia menggunakan banyak geotermal?", "id": "Ya, untuk listrik dan pemanasan." },
  { "en": "Apa itu 'Blue Lagoon' di Islandia?", "id": "Spa geotermal terkenal." },
  { "en": "Airnya berasal dari mana?", "id": "Air buangan dari pembangkit geotermal." },
  { "en": "Apa itu 'Ring of Fire'?", "id": "Cincin Api Pasifik." },
  { "en": "Indonesia bagian dari 'Ring of Fire'?", "id": "Ya." },
  { "en": "Itulah sebabnya kaya akan?", "id": "Potensi panas bumi." },
  { "en": "Apa itu 'fumarole'?", "id": "Celah yang mengeluarkan uap panas." },
  { "en": "Apa itu 'geyser'?", "id": "Semburan air panas periodik." },
  { "en": "Apa itu 'hot spring'?", "id": "Mata air panas." },
  { "en": "Indikator adanya geotermal?", "id": "Ya, ketiganya adalah indikator permukaan." },
  { "en": "Apa itu 'exploration drilling'?", "id": "Pengeboran eksplorasi." },
  { "en": "Tujuannya?", "id": "Memastikan adanya reservoir panas bumi." },
  { "en": "Risikonya?", "id": "Biaya tinggi dan bisa gagal." },
  { "en": "Apa itu 'production well'?", "id": "Sumur produksi." },
  { "en": "Apa itu 'injection well'?", "id": "Sumur injeksi." },
  { "en": "Apa itu 'steam turbine'?", "id": "Turbin uap." },
  { "en": "Apa itu 'hydropower turbine'?", "id": "Turbin air." },
  { "en": "Apa itu 'wind turbine'?", "id": "Turbin angin." },
  { "en": "Apa itu 'gas turbine'?", "id": "Turbin gas." },
  { "en": "Apa itu 'turbine' vs 'engine'?", "id": "Menggunakan aliran fluida vs pembakaran internal." },
  { "en": "Motor listrik adalah engine?", "id": "Bukan, motor adalah konverter energi." },
  { "en": "Apa itu 'internal combustion engine' (ICE)?", "id": "Mesin pembakaran dalam." },
  { "en": "Kendaraan bensin menggunakan ICE?", "id": "Ya." },
  { "en": "Apa itu 'ICE-ing'?", "id": "Mobil bensin parkir di tempat cas EV." },
  { "en": "Apa itu 'charger etiquette'?", "id": "Etiket menggunakan stasiun pengisian daya." },
  { "en": "Apa itu 'idle fee'?", "id": "Biaya jika mobil selesai cas tapi tidak dipindah." },
  { "en": "Apa itu 'Combined Charging System' (CCS)?", "id": "Standar konektor cas EV." },
  { "en": "Apa itu 'CHAdeMO'?", "id": "Standar konektor cas DC lain." },
  { "en": "Tesla menggunakan konektor sendiri?", "id": "Ya, disebut NACS." },
  { "en": "Apa itu 'Level 1' charging?", "id": "Pengisian daya lambat di stopkontak biasa." },
  { "en": "Apa itu 'Level 2' charging?", "id": "Pengisian daya sedang (di rumah/publik)." },
  { "en": "Apa itu 'Level 3' charging?", "id": "Pengisian daya cepat (DC fast charging)." },
  { "en": "Apa itu 'battery swapping'?", "id": "Menukar baterai kosong dengan yang penuh." },
  { "en": "Lebih cepat dari charging?", "id": "Ya, hanya beberapa menit." },
  { "en": "Apa itu 'range'?", "id": "Jarak tempuh EV dalam satu kali cas." },
  { "en": "Apa itu 'MPGe'?", "id": "Miles per gallon equivalent." },
  { "en": "Ukuran efisiensi untuk apa?", "id": "Kendaraan listrik dan hybrid." },
  { "en": "Apa itu 'one-pedal driving'?", "id": "Mengemudi hanya dengan pedal akselerator." },
  { "en": "Bagaimana pengeremannya?", "id": "Dengan regenerative braking yang kuat." },
  { "en": "Apa itu 'frunk'?", "id": "Bagasi depan (front trunk)." },
  { "en": "Mengapa EV punya 'frunk'?", "id": "Karena tidak ada mesin di depan." },
  { "en": "Apa itu 'skateboard platform'?", "id": "Desain sasis datar untuk EV." },
  { "en": "Isinya?", "id": "Baterai, motor, dan komponen lain." },
  { "en": "Apa itu 'thermal management system' (EV)?", "id": "Sistem untuk mendinginkan/memanaskan baterai." },
  { "en": "Penting untuk performa baterai?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'preconditioning'?", "id": "Mempersiapkan suhu baterai sebelum berkendara/cas." },
  { "en": "Tujuannya?", "id": "Meningkatkan efisiensi dan kecepatan cas." },
  { "en": "Apa itu 'hydrogen embrittlement'?", "id": "Material menjadi rapuh karena hidrogen." },
  { "en": "Tantangan dalam penyimpanan hidrogen?", "id": "Ya, salah satu tantangannya." },
  { "en": "Cara menyimpan hidrogen?", "id": "Gas terkompresi, cair, material padat." },
  { "en": "Penyimpanan sebagai gas butuh tekanan?", "id": "Sangat tinggi." },
  { "en": "Penyimpanan sebagai cairan butuh suhu?", "id": "Sangat rendah (kriogenik)." },
  { "en": "Apa itu 'metal hydride'?", "id": "Material untuk menyimpan hidrogen padat." },
  { "en": "Apa itu 'biogas upgrading'?", "id": "Memurnikan biogas menjadi biometana." },
  { "en": "Biometana sama dengan gas alam?", "id": "Ya, komposisi kimianya hampir sama." },
  { "en": "Bisa disuntikkan ke jaringan gas?", "id": "Ya." },
  { "en": "Apa itu 'renewable natural gas' (RNG)?", "id": "Sinonim untuk biometana." },
  { "en": "Apa itu 'anaerobic' vs 'aerobic'?", "id": "Tanpa oksigen vs dengan oksigen." },
  { "en": "Dekomposisi di TPA?", "id": "Anaerobik, menghasilkan metana." },
  { "en": "Pengomposan adalah proses?", "id": "Aerobik." },
  { "en": "Apa itu 'windbreak'?", "id": "Penghalang angin (pohon, pagar)." },
  { "en": "Apa itu 'shelterbelt'?", "id": "Jalur pohon untuk melindungi dari angin." },
  { "en": "Bisa meningkatkan hasil panen?", "id": "Ya, dengan melindungi tanaman." },
  { "en": "Apa itu 'soil erosion'?", "id": "Erosi tanah." },
  { "en": "Angin bisa menyebabkan erosi?", "id": "Ya." },
  { "en": "Apa itu 'desertification'?", "id": "Proses lahan subur menjadi gurun." },
  { "en": "Perubahan iklim memperburuknya?", "id": "Ya." },
  { "en": "PLTS skala besar di gurun?", "id": "Ya, potensi sangat besar." },
  { "en": "Tantangannya?", "id": "Suhu ekstrem, badai pasir, transmisi jauh." },
  { "en": "Apa itu 'CSP' vs 'PV'?", "id": "Termal vs fotovoltaik." },
  { "en": "Apa itu 'dam'?", "id": "Bendungan." },
  { "en": "Apa itu 'reservoir'?", "id": "Waduk." },
  { "en": "Apa itu 'powerhouse'?", "id": "Gedung pembangkit di PLTA." },
  { "en": "Apa itu 'draft tube'?", "id": "Pipa di bawah turbin air." },
  { "en": "Fungsinya?", "id": "Meningkatkan efisiensi turbin." },
  { "en": "Apa itu 'cavitation'?", "id": "Pembentukan gelembung uap di cairan." },
  { "en": "Bisa merusak turbin?", "id": "Ya, bisa sangat merusak." },
  { "en": "Apa itu 'fish mortality'?", "id": "Kematian ikan akibat turbin." },
  { "en": "Apa itu 'water quality'?", "id": "Kualitas air." },
  { "en": "Bendungan bisa mengubah kualitas air?", "id": "Ya, suhu dan oksigen terlarut." },
  { "en": "Apa itu 'methane emissions from reservoirs'?", "id": "Emisi metana dari dekomposisi di waduk." },
  { "en": "Apa itu 'run-of-river' vs 'storage' hydropower?", "id": "Tanpa waduk vs dengan waduk." },
  { "en": "Mana yang lebih 'dispatchable'?", "id": "Storage hydropower (dengan waduk)." },
  { "en": "Apa itu 'dispatchable'?", "id": "Bisa diatur output dayanya." },
  { "en": "PLTS dan PLTB 'non-dispatchable'?", "id": "Ya, karena bergantung pada cuaca." },
  { "en": "PLTP dan PLTA 'dispatchable'?", "_comment": "PLTP dan PLTA dispatchable?", "id": "Ya, outputnya bisa dikontrol." },
  { "en": "Apa itu 'firm capacity'?", "id": "Kapasitas yang bisa diandalkan." },
  { "en": "Apa itu 'variable renewable energy' (VRE)?", "id": "Energi terbarukan yang bervariasi." },
  { "en": "Contoh VRE?", "id": "Tenaga surya dan angin." },
  { "en": "Apa itu 'ancillary services market'?", "id": "Pasar untuk layanan pendukung jaringan." },
  { "en": "Apa itu 'frequency response'?", "id": "Respons terhadap perubahan frekuensi." },
  { "en": "Baterai punya respons frekuensi cepat?", "id": "Ya, sangat cepat." },
  { "en": "Apa itu 'spinning reserve'?", "id": "Cadangan daya dari generator berputar." },
  { "en": "Apa itu 'non-spinning reserve'?", "id": "Cadangan daya yang bisa cepat online." },
  { "en": "Apa itu 'black start'?", "id": "Memulai kembali jaringan setelah blackout." },
  { "en": "Pembangkit mana yang butuh 'black start'?", "id": "PLTA, beberapa PLTG." },
  { "en": "Apa itu 'island mode'?", "id": "Saat microgrid beroperasi terpisah." },
  { "en": "Apa itu 'interconnector'?", "id": "Jalur transmisi antar jaringan/negara." },
  { "en": "Tujuannya?", "id": "Berbagi sumber daya, meningkatkan keandalan." },
  { "en": "Contohnya?", "id": "Jaringan listrik terintegrasi ASEAN." },
  { "en": "Apa itu 'wheeling charge'?", "id": "Biaya menggunakan jaringan transmisi." },
  { "en": "Apa itu 'load shedding'?", "id": "Pemadaman terencana untuk menyeimbangkan jaringan." },
  { "en": "Apa itu 'rolling blackout'?", "id": "Sinonim untuk pemadaman bergilir." },
  { "en": "Apa itu 'demand forecasting'?", "id": "Prediksi permintaan." },
  { "en": "Apa itu 'weather forecasting'?", "id": "Prakiraan cuaca." },
  { "en": "Penting untuk VRE?", "id": "Ya, sangat krusial." },
  { "en": "Apa itu 'numerical weather prediction'?", "id": "Model cuaca berbasis komputer." },
  { "en": "AI bisa meningkatkan akurasinya?", "id": "Ya, area riset yang aktif." },
  { "en": "Apa itu 'solar panel'?", "id": "Panel surya." },
  { "en": "Apa itu 'wind turbine'?", "id": "Turbin angin." },
  { "en": "Apa itu 'hydropower dam'?", "id": "Bendungan PLTA." },
  { "en": "Apa itu 'geothermal plant'?", "id": "Pembangkit panas bumi." },
  { "en": "Apa itu 'biomass plant'?", "id": "Pembangkit biomassa." },
  { "en": "Apa itu 'nuclear power plant'?", "id": "Pembangkit listrik tenaga nuklir." },
  { "en": "Apa itu 'coal power plant'?", "id": "Pembangkit listrik tenaga uap (batu bara)." },
  { "en": "Apa itu 'gas power plant'?", "id": "Pembangkit listrik tenaga gas." },
  { "en": "Apa itu 'solar cell'?", "id": "Sel surya." },
  { "en": "Apa itu 'electric vehicle'?", "id": "Kendaraan listrik." },
  { "en": "Apa itu 'green hydrogen'?", "id": "Hidrogen hijau." },
  { "en": "Apa itu 'smart grid'?", "id": "Jaringan listrik pintar." },
  { "en": "Apa itu 'microgrid'?", "id": "Jaringan mikro." },
  { "en": "Apa itu 'energy storage'?", "id": "Penyimpanan energi." },
  { "en": "Apa itu 'battery'?", "id": "Baterai." },
  { "en": "Apa itu 'inverter'?", "id": "Inverter." },
  { "en": "Apa itu 'converter'?", "id": "Konverter." },
  { "en": "Apa itu 'transformer'?", "id": "Transformator." },
  { "en": "Apa itu 'generator'?", "id": "Generator." },
  { "en": "Apa itu 'motor'?", "id": "Motor." },
  { "en": "Apa itu 'efficiency'?", "id": "Efisiensi." },
  { "en": "Apa itu 'sustainability'?", "id": "Keberlanjutan." },
  { "en": "Apa itu 'decarbonization'?", "id": "Dekarbonisasi." },
  { "en": "Apa itu 'electrification'?", "id": "Elektrifikasi." },
  { "en": "Apa itu 'policy'?", "id": "Kebijakan." },
  { "en": "Apa itu 'regulation'?", "id": "Regulasi." },
  { "en": "Apa itu 'subsidy'?", "id": "Subsidi." },
  { "en": "Apa itu 'tariff'?", "id": "Tarif." },
  { "en": "Apa itu 'investment'?", "id": "Investasi." },
  { "en": "Apa itu 'finance'?", "id": "Keuangan." },
  { "en": "Apa itu 'carbon market'?", "id": "Pasar karbon." },
  { "en": "Apa itu 'emission trading'?", "id": "Perdagangan emisi." },
  { "en": "Apa itu 'carbon price'?", "id": "Harga karbon." },
  { "en": "Apa itu 'climate justice'?", "id": "Keadilan iklim." },
  { "en": "Apa itu 'energy justice'?", "id": "Keadilan energi." },
  { "en": "Apa itu 'environmental justice'?", "id": "Keadilan lingkungan." },
  { "en": "Apa itu 'social impact'?", "id": "Dampak sosial." },
  { "en": "Apa itu 'environmental impact'?", "id": "Dampak lingkungan." },
  { "en": "Apa itu 'economic impact'?", "id": "Dampak ekonomi." },
  { "en": "Apa itu 'circularity'?", "id": "Sirkularitas." },
  { "en": "Apa itu 'recycling'?", "id": "Daur ulang." },
  { "en": "Apa itu 'reusing'?", "id": "Menggunakan kembali." },
  { "en": "Apa itu 'reducing'?", "id": "Mengurangi." },
  { "en": "Apa itu 'life cycle'?", "id": "Siklus hidup." },
  { "en": "Apa itu 'assessment'?", "id": "Penilaian atau asesmen." },
  { "en": "Apa itu 'mitigation'?", "id": "Mitigasi." },
  { "en": "Apa itu 'adaptation'?", "id": "Adaptasi." },
  { "en": "Apa itu 'resilience'?", "id": "Ketahanan atau resiliensi." },
  { "en": "Apa itu 'technology'?", "id": "Teknologi." },
  { "en": "Apa itu 'innovation'?", "id": "Inovasi." },
  { "en": "Apa itu 'research'?", "id": "Riset atau penelitian." },
  { "en": "Apa itu 'development'?", "id": "Pengembangan." },
  { "en": "Apa itu 'deployment'?", "id": "Penerapan atau deployment." },
  { "en": "Apa itu 'scaling up'?", "id": "Peningkatan skala." },
  { "en": "Apa itu 'grid integration'?", "id": "Integrasi jaringan." },
  { "en": "Apa itu 'intermittency'?", "id": "Intermitensi." },
  { "en": "Apa itu 'variability'?", "id": "Variabilitas." },
  { "en": "Apa itu 'forecasting'?", "id": "Prakiraan atau peramalan." },
  { "en": "Apa itu 'optimization'?", "id": "Optimisasi." },
  { "en": "Apa itu 'simulation'?", "id": "Simulasi." },
  { "en": "Apa itu 'modeling'?", "id": "Pemodelan." },
  { "en": "Apa itu 'data analysis'?", "id": "Analisis data." },
  { "en": "Apa itu 'machine learning'?", "id": "Pembelajaran mesin." },
  { "en": "Apa itu 'artificial intelligence'?", "id": "Kecerdasan buatan." },
  { "en": "Apa itu 'automation'?", "id": "Otomatisasi." },
  { "en": "Apa itu 'digitalization'?", "id": "Digitalisasi." },
  { "en": "Apa itu 'infrastructure'?", "id": "Infrastruktur." },
  { "en": "Apa itu 'transmission line'?", "id": "Jalur transmisi." },
  { "en": "Apa itu 'substation'?", "id": "Gardu induk." },
  { "en": "Apa itu 'off-grid'?", "id": "Luar jaringan (off-grid)." },
  { "en": "Apa itu 'on-grid'?", "id": "Terhubung ke jaringan (on-grid)." },
  { "en": "Energi terbarukan adalah masa depan?", "id": "Ya, kunci untuk masa depan berkelanjutan." }




        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
