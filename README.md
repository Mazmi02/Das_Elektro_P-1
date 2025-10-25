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


{ "en": "Apa Satuan Dasar Untuk Tegangan Listrik?", "id": "Volt (V)." },
{ "en": "Apa Satuan Dasar Untuk Arus Listrik?", "id": "Ampere (A)." },
{ "en": "Apa Satuan Dasar Untuk Hambatan Listrik?", "id": "Ohm (Ω)." },
{ "en": "Apa Satuan Dasar Untuk Daya Listrik?", "id": "Watt (W)." },
{ "en": "Apa Simbol Huruf Untuk Tegangan Listrik?", "id": "V." },
{ "en": "Apa Simbol Huruf Untuk Arus Listrik?", "id": "I." },
{ "en": "Apa Simbol Huruf Untuk Hambatan Listrik?", "id": "R." },
{ "en": "Apa Simbol Huruf Untuk Daya Listrik?", "id": "P." },
{ "en": "Bagaimana Bunyi Dasar Hukum Ohm?", "id": "V = I x R." },
{ "en": "Apa Rumus Dasar Untuk Menghitung Daya Listrik?", "id": "P = V x I." },
{ "en": "Apa Kepanjangan Dari AC (Alternating Current)?", "id": "Arus Bolak-Balik." },
{ "en": "Apa Kepanjangan Dari DC (Direct Current)?", "id": "Arus Searah." },
{ "en": "Bagaimana Bentuk Gelombang Arus DC (Direct Current)?", "id": "Lurus Dan Konstan." },
{ "en": "Bagaimana Bentuk Gelombang Arus AC (Alternating Current)?", "id": "Berbentuk Sinusoidal." },
{ "en": "Apa Sumber Umum Arus DC (Direct Current)?", "id": "Baterai." },
{ "en": "Apa Sumber Umum Arus AC (Alternating Current)?", "id": "Jaringan Listrik PLN." },
{ "en": "Apa Itu Frekuensi Dalam Konteks Listrik AC?", "id": "Jumlah Siklus Gelombang Per Detik." },
{ "en": "Apa Satuan Untuk Frekuensi Listrik?", "id": "Hertz (Hz)." },
{ "en": "Berapa Frekuensi Standar Listrik PLN Di Indonesia?", "id": "50 Hertz (Hz)." },
{ "en": "Apa Fungsi Utama Dari Resistor?", "id": "Menghambat Aliran Arus." },
{ "en": "Apa Fungsi Utama Dari Kapasitor?", "id": "Menyimpan Muatan Listrik Sementara." },
{ "en": "Apa Satuan Untuk Kapasitansi Sebuah Kapasitor?", "id": "Farad (F)." },
{ "en": "Apa Fungsi Utama Dari Induktor?", "id": "Menyimpan Energi Medan Magnet." },
{ "en": "Apa Satuan Untuk Induktansi Sebuah Induktor?", "id": "Henry (H)." },
{ "en": "Komponen Apa Yang Menentang Perubahan Arus Tiba-Tiba?", "id": "Induktor." },
{ "en": "Komponen Apa Yang Menentang Perubahan Tegangan Tiba-Tiba?", "id": "Kapasitor." },
{ "en": "Apa Nama Lain Untuk Kapasitor?", "id": "Kondensator." },
{ "en": "Apa Itu Bahan Konduktor Listrik?", "id": "Bahan Mudah Menghantarkan Listrik." },
{ "en": "Berikan Satu Contoh Bahan Konduktor Yang Baik?", "id": "Tembaga." },
{ "en": "Apa Itu Bahan Isolator Listrik?", "id": "Bahan Sulit Menghantarkan Listrik." },
{ "en": "Berikan Satu Contoh Bahan Isolator Listrik?", "id": "Karet." },
{ "en": "Apa Fungsi Isolator Pada Kabel Listrik?", "id": "Melindungi Dari Sengatan Listrik." },
{ "en": "Apa Itu Bahan Semikonduktor?", "id": "Bahan Antara Konduktor, Isolator." },
{ "en": "Berikan Satu Contoh Bahan Semikonduktor Murni?", "id": "Silikon (Si)." },
{ "en": "Dimana Bahan Semikonduktor Paling Banyak Digunakan?", "id": "Di Komponen Elektronika." },
{ "en": "Apa Kepanjangan Dari KCL (Kirchhoff's Current Law)?", "id": "Hukum Arus Kirchhoff." },
{ "en": "Apa Bunyi Hukum KCL (Kirchhoff's Current Law)?", "id": "Total Arus Masuk Simpul Nol." },
{ "en": "Dimana Hukum KCL (Kirchhoff's Current Law) Diterapkan?", "id": "Pada Titik Percabangan." },
{ "en": "Apa Kepanjangan Dari KVL (Kirchhoff's Voltage Law)?", "id": "Hukum Tegangan Kirchhoff." },
{ "en": "Apa Bunyi Hukum KVL (Kirchhoff's Voltage Law)?", "id": "Total Tegangan Loop Tertutup Nol." },
{ "en": "Dimana Hukum KVL (Kirchhoff's Voltage Law) Diterapkan?", "id": "Pada Rangkaian Tertutup." },
{ "en": "Hukum KCL Adalah Bentuk Kekekalan Apa?", "id": "Kekekalan Muatan Listrik." },
{ "en": "Hukum KVL Adalah Bentuk Kekekalan Apa?", "id": "Kekekalan Energi Listrik." },
{ "en": "Bagaimana Total Arus Pada Rangkaian Seri?", "id": "Arus Di Setiap Titik Sama." },
{ "en": "Bagaimana Total Tegangan Pada Rangkaian Seri?", "id": "Tegangan Terbagi Di Setiap Komponen." },
{ "en": "Bagaimana Rumus Resistor Pengganti Rangkaian Seri?", "id": "Rs = R1 + R2." },
{ "en": "Bagaimana Total Arus Pada Rangkaian Paralel?", "id": "Arus Terbagi Di Setiap Cabang." },
{ "en": "Bagaimana Total Tegangan Pada Rangkaian Paralel?", "id": "Tegangan Di Setiap Cabang Sama." },
{ "en": "Bagaimana Rumus Resistor Pengganti Rangkaian Paralel?", "id": "1/Rp = 1/R1 + 1/R2." },
{ "en": "Jika Satu Lampu Putus Di Rangkaian Seri, Bagaimana Lainnya?", "id": "Semua Lampu Lain Mati." },
{ "en": "Jika Satu Lampu Putus Di Rangkaian Paralel, Bagaimana Lainnya?", "id": "Lampu Lain Tetap Menyala." },
{ "en": "Bagaimana Instalasi Listrik Di Rumah, Seri Atau Paralel?", "id": "Rangkaian Paralel." },
{ "en": "Apa Fungsi Utama Dari Dioda?", "id": "Menyearahkan Arus Listrik." },
{ "en": "Dioda Mengalirkan Arus Hanya Dalam Berapa Arah?", "id": "Satu Arah." },
{ "en": "Apa Nama Dua Kaki Pada Dioda?", "id": "Anoda Dan Katoda." },
{ "en": "Apa Kepanjangan Dari LED (Light Emitting Diode)?", "id": "Dioda Pemancar Cahaya." },
{ "en": "Apa Fungsi Utama Dari Transistor?", "id": "Sebagai Saklar Atau Penguat." },
{ "en": "Apa Tiga Kaki Pada Transistor BJT?", "id": "Basis, Kolektor, Dan Emitor." },
{ "en": "Apa Kepanjangan BJT (Bipolar Junction Transistor)?", "id": "Transistor Sambungan Bipolar." },
{ "en": "Apa Kepanjangan FET (Field Effect Transistor)?", "id": "Transistor Efek Medan." },
{ "en": "Apa Fungsi Dioda Zener?", "id": "Menstabilkan Tegangan." },
{ "en": "Apa Itu Proses Rectification (Penyearahan)?", "id": "Mengubah Arus AC Menjadi Arus DC." },
{ "en": "Apa Nama Alat Untuk Mengukur Tegangan Listrik?", "id": "Voltmeter." },
{ "en": "Apa Nama Alat Untuk Mengukur Arus Listrik?", "id": "Amperemeter." },
{ "en": "Apa Nama Alat Untuk Mengukur Hambatan Listrik?", "id": "Ohmmeter." },
{ "en": "Apa Kepanjangan AVO Meter (Multimeter)?", "id": "Ampere, Volt, Ohm." },
{ "en": "Apa Fungsi Utama Multimeter (AVO Meter)?", "id": "Mengukur Ampere, Volt, Ohm." },
{ "en": "Bagaimana Posisi Pemasangan Voltmeter Dalam Rangkaian?", "id": "Dipasang Secara Paralel." },
{ "en": "Bagaimana Posisi Pemasangan Amperemeter Dalam Rangkaian?", "id": "Dipasang Secara Seri." },
{ "en": "Apa Fungsi Osiloskop Dalam Teknik Elektro?", "id": "Menampilkan Bentuk Sinyal Listrik." },
{ "en": "Apa Sumbu Vertikal Pada Tampilan Osiloskop?", "id": "Amplitudo Tegangan." },
{ "en": "Apa Sumbu Horizontal Pada Tampilan Osiloskop?", "id": "Waktu." },
{ "en": "Apa Yang Dihasilkan Oleh Arus Listrik Mengalir?", "id": "Medan Magnet." },
{ "en": "Apa Itu Induksi Elektromagnetik?", "id": "Perubahan Medan Magnet Hasilkan Listrik." },
{ "en": "Siapa Nama Penemu Hukum Induksi Elektromagnetik?", "id": "Michael Faraday." },
{ "en": "Apa Fungsi Utama Transformator (Trafo)?", "id": "Mengubah Level Tegangan AC." },
{ "en": "Apa Jenis Trafo Untuk Menaikkan Tegangan?", "id": "Transformator Step-Up." },
{ "en": "Apa Jenis Trafo Untuk Menurunkan Tegangan?", "id": "Transformator Step-Down." },
{ "en": "Apa Fungsi Sekring (Fuse) Dalam Rangkaian?", "id": "Melindungi Dari Arus Berlebih." },
{ "en": "Apa Kepanjangan MCB (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit Miniatur." },
{ "en": "Apa Fungsi Grounding (Pembumian) Pada Sistem Listrik?", "id": "Keamanan Dari Kebocoran Arus." },
{ "en": "Apa Itu Energi Listrik?", "id": "Daya Kali Waktu." },
{ "en": "Apa Satuan Energi Listrik Yang Umum Dipakai PLN?", "id": "Kilowatt-hour (kWh)." },
{ "en": "Apa Kepanjangan kWh (Kilowatt-hour)?", "id": "Kilowatt Jam." },
{ "en": "Apa Itu Rangkaian RLC (Resistor, Inductor, Capacitor)?", "id": "Rangkaian Mengandung R, L, C." },
{ "en": "Apa Itu Impedansi Dalam Rangkaian AC?", "id": "Hambatan Total Rangkaian AC." },
{ "en": "Apa Satuan Untuk Impedansi?", "id": "Ohm (Ω)." },
{ "en": "Apa Itu Resonansi Dalam Rangkaian RLC?", "id": "Reaktansi Induktif Sama Kapasitif." },
{ "en": "Apa Itu Sistem Listrik Tiga Fasa?", "id": "Sistem Listrik Tiga Sinyal AC." },
{ "en": "Apa Keuntungan Utama Sistem Tiga Fasa?", "id": "Mengirimkan Daya Lebih Efisien." },
{ "en": "Apa Nama Komponen Untuk Menyambung Memutus Sirkuit?", "id": "Saklar (Switch)." },
{ "en": "Apa Satuan Untuk Muatan Listrik?", "id": "Coulomb (C)." },
{ "en": "Apa Nama Partikel Pembawa Muatan Negatif?", "id": "Elektron." },
{ "en": "Apa Itu Konduktivitas Listrik?", "id": "Kemampuan Bahan Menghantarkan Listrik." },
{ "en": "Apa Itu Resistivitas Listrik?", "id": "Kemampuan Bahan Menghambat Listrik." },
{ "en": "Apa Nama Lain Untuk Tegangan Listrik?", "id": "Beda Potensial." },
{ "en": "Apa Fungsi Dari Generator Listrik?", "id": "Mengubah Energi Mekanik Ke Listrik." },
{ "en": "Apa Fungsi Dari Motor Listrik?", "id": "Mengubah Energi Listrik Ke Mekanik." },
{ "en": "Apa Nama Partikel Pembawa Muatan Positif?", "id": "Proton." },
{ "en": "Apa Nama Partikel Tanpa Muatan Listrik?", "id": "Neutron." },
  { "en": "Apa Kepanjangan Dari RMS (Root Mean Square)?", "id": "Akar Rata-Rata Kuadrat." },
  { "en": "Apa Nilai RMS (Root Mean Square) Tegangan PLN?", "id": "Nilai Efektif Tegangan AC." },
  { "en": "Apa Itu Tegangan Puncak (Peak Voltage)?", "id": "Nilai Maksimum Gelombang AC." },
  { "en": "Apa Itu Tegangan Puncak Ke Puncak (Peak-To-Peak)?", "id": "Nilai Antara Puncak Positif, Negatif." },
  { "en": "Apa Satuan Untuk Daya Reaktif?", "id": "Volt-Ampere Reactive (VAR)." },
  { "en": "Apa Satuan Untuk Daya Nyata (Real Power)?", "id": "Watt (W)." },
  { "en": "Apa Satuan Untuk Daya Semu (Apparent Power)?", "id": "Volt-Ampere (VA)." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Perbandingan Daya Nyata, Daya Semu." },
  { "en": "Berapa Nilai Ideal Faktor Daya (Power Factor)?", "id": "Nilai Ideal Adalah 1." },
  { "en": "Apa Penyebab Faktor Daya (Power Factor) Buruk?", "id": "Beban Induktif Yang Tinggi." },
  { "en": "Bagaimana Cara Memperbaiki Faktor Daya (Power Factor)?", "id": "Memasang Bank Kapasitor." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Yang Berubah." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit (Logika 0, 1)." },
  { "en": "Apa Kepanjangan LDR (Light Dependent Resistor)?", "id": "Resistor Tergantung Cahaya." },
  { "en": "Bagaimana Resistansi LDR (Light Dependent Resistor) Saat Gelap?", "id": "Resistansi Menjadi Sangat Tinggi." },
  { "en": "Bagaimana Resistansi LDR (Light Dependent Resistor) Saat Terang?", "id": "Resistansi Menjadi Sangat Rendah." },
  { "en": "Apa Kepanjangan NTC (Negative Temperature Coefficient)?", "id": "Koefisien Suhu Negatif." },
  { "en": "Apa Itu Termistor NTC (Negative Temperature Coefficient)?", "id": "Resistansi Turun Saat Suhu Naik." },
  { "en": "Apa Kepanjangan PTC (Positive Temperature Coefficient)?", "id": "Koefisien Suhu Positif." },
  { "en": "Apa Itu Termistor PTC (Positive Temperature Coefficient)?", "id": "Resistansi Naik Saat Suhu Naik." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Yang Dapat Diatur." },
  { "en": "Apa Nama Lain Potensiometer?", "id": "Trimpot (Ukuran Kecil)." },
  { "en": "Apa Fungsi Potensiometer?", "id": "Mengatur Tegangan Atau Arus." },
  { "en": "Apa Itu Rheostat?", "id": "Resistor Variabel Pengatur Arus." },
  { "en": "Apa Fungsi Utama Relay?", "id": "Saklar Elektromagnetik." },
  { "en": "Bagian Apa Yang Bergerak Pada Relay?", "id": "Kontak (Contacts)." },
  { "en": "Bagian Apa Yang Diam Pada Relay?", "id": "Koil (Coil)." },
  { "en": "Apa Kepanjangan NO (Normally Open) Pada Relay?", "id": "Normal Terbuka." },
  { "en": "Apa Kepanjangan NC (Normally Closed) Pada Relay?", "id": "Normal Tertutup." },
  { "en": "Apa Kepanjangan COM (Common) Pada Relay?", "id": "Kaki Umum (Common)." },
  { "en": "Apa Fungsi Dioda Freewheeling Pada Koil Relay?", "id": "Melindungi Dari GGL Induksi Balik." },
  { "en": "Apa Fungsi Utama Saklar (Switch)?", "id": "Menyambung, Memutus Aliran Listrik." },
  { "en": "Apa Kepanjangan SPST (Single Pole Single Throw)?", "id": "Saklar Satu Kutub Satu Arah." },
  { "en": "Apa Kepanjangan SPDT (Single Pole Double Throw)?", "id": "Saklar Satu Kutub Dua Arah." },
  { "en": "Apa Kepanjangan DPST (Double Pole Single Throw)?", "id": "Saklar Dua Kutub Satu Arah." },
  { "en": "Apa Kepanjangan DPDT (Double Pole Double Throw)?", "id": "Saklar Dua Kutub Dua Arah." },
  { "en": "Apa Fungsi Rangkaian Filter Lolos Rendah (Low Pass)?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Fungsi Rangkaian Filter Lolos Tinggi (High Pass)?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Fungsi Rangkaian Filter Lolos Pita (Band Pass)?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Fungsi Rangkaian Filter Tolak Pita (Band Stop)?", "id": "Menolak Rentang Frekuensi Tertentu." },
  { "en": "Komponen Apa Pembentuk Filter RC Pasif?", "id": "Resistor (R) Dan Kapasitor (C)." },
  { "en": "Komponen Apa Pembentuk Filter RL Pasif?", "id": "Resistor (R) Dan Induktor (L)." },
  { "en": "Apa Itu Rangkaian Pembagi Tegangan (Voltage Divider)?", "id": "Menghasilkan Tegangan Output Lebih Rendah." },
  { "en": "Komponen Apa Yang Digunakan Pembagi Tegangan Sederhana?", "id": "Dua Resistor Seri." },
  { "en": "Apa Itu Rangkaian Pembagi Arus (Current Divider)?", "id": "Membagi Arus Ke Cabang Paralel." },
  { "en": "Komponen Apa Yang Digunakan Pembagi Arus Sederhana?", "id": "Dua Resistor Paralel." },
  { "en": "Apa Kepanjangan SCR (Silicon Controlled Rectifier)?", "id": "Penyearah Terkendali Silikon." },
  { "en": "Apa Fungsi SCR (Silicon Controlled Rectifier)?", "id": "Sebagai Saklar Daya Arus DC." },
  { "en": "Berapa Kaki Yang Dimiliki SCR (Silicon Controlled Rectifier)?", "id": "Tiga Kaki (Anoda, Katoda, Gate)." },
  { "en": "Apa Kepanjangan TRIAC (Triode for Alternating Current)?", "id": "Trioda Untuk Arus Bolak-Balik." },
  { "en": "Apa Fungsi TRIAC (Triode for Alternating Current)?", "id": "Sebagai Saklar Daya Arus AC." },
  { "en": "Apa Kepanjangan DIAC (Diode for Alternating Current)?", "id": "Dioda Untuk Arus Bolak-Balik." },
  { "en": "Apa Fungsi DIAC (Diode for Alternating Current)?", "id": "Memicu (Trigger) TRIAC." },
  { "en": "Apa Itu Semikonduktor Tipe-P (P-Type)?", "id": "Semikonduktor Kelebihan Lubang (Hole)." },
  { "en": "Apa Itu Semikonduktor Tipe-N (N-Type)?", "id": "Semikonduktor Kelebihan Elektron." },
  { "en": "Apa Proses Penambahan Ketidakmurnian Pada Semikonduktor?", "id": "Proses Doping." },
  { "en": "Apa Pembawa Muatan Mayoritas Pada Tipe-P?", "id": "Lubang (Hole)." },
  { "en": "Apa Pembawa Muatan Mayoritas Pada Tipe-N?", "id": "Elektron." },
  { "en": "Apa Yang Terbentuk Saat Tipe-P Dan Tipe-N Bertemu?", "id": "Sambungan PN (PN Junction)." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Daerah Kosong Muatan Di Sambungan PN." },
  { "en": "Apa Itu Bias Maju (Forward Bias) Pada Dioda?", "id": "Anoda Positif, Katoda Negatif." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias) Pada Dioda?", "id": "Anoda Negatif, Katoda Positif." },
  { "en": "Kapan Dioda Dapat Menghantarkan Arus?", "id": "Saat Diberi Bias Maju." },
  { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage)?", "id": "Tegangan Saat Dioda Rusak (Bias Mundur)." },
  { "en": "Apa Satuan Untuk Fluks Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Satuan Untuk Kerapatan Fluks Magnetik?", "id": "Tesla (T)." },
  { "en": "Apa Simbol Untuk Fluks Magnetik?", "id": "Simbol Phi (Φ)." },
  { "en": "Apa Simbol Untuk Kerapatan Fluks Magnetik?", "id": "Simbol B." },
  { "en": "Apa Itu Permeabilitas Magnetik?", "id": "Kemampuan Bahan Dilewati Medan Magnet." },
  { "en": "Apa Itu Reluktansi Magnetik?", "id": "Hambatan Bahan Terhadap Fluks Magnetik." },
  { "en": "Apa Bahan Feromagnetik?", "id": "Bahan Yang Sangat Mudah Ditarik Magnet." },
  { "en": "Berikan Contoh Bahan Feromagnetik?", "id": "Besi, Baja, Nikel, Kobalt." },
  { "en": "Apa Itu Elektromagnet?", "id": "Magnet Yang Dihasilkan Arus Listrik." },
  { "en": "Bagaimana Cara Memperkuat Elektromagnet?", "id": "Menambah Arus Atau Jumlah Lilitan." },
  { "en": "Apa Itu Solenoida?", "id": "Kawat Lilitan Berbentuk Kumparan Panjang." },
  { "en": "Apa Fungsi Solenoida?", "id": "Menghasilkan Medan Magnet Terkontrol." },
  { "en": "Apa Kepanjangan PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Tercetak." },
  { "en": "Apa Fungsi PCB (Printed Circuit Board)?", "id": "Tempat Merakit Komponen Elektronika." },
  { "en": "Apa Bahan Dasar PCB (Printed Circuit Board)?", "id": "Bahan Fiberglass (FR-4) Berlapis Tembaga." },
  { "en": "Apa Proses Melarutkan Tembaga Di PCB?", "id": "Etsa (Etching)." },
  { "en": "Gimana Cara Menyambung Komponen Ke PCB?", "id": "Dengan Cara Disolder." },
  { "en": "Apa Bahan Utama Solder (Tin)?", "id": "Timah (Sn) Dan Timbal (Pb)." },
  { "en": "Apa Fungsi Pasta Solder (Flux)?", "id": "Membersihkan Oksida Saat Menyolder." },
  { "en": "Apa Itu Rangkaian Jembatan Wheatstone?", "id": "Mengukur Nilai Hambatan Tidak Dikenal." },
  { "en": "Kapan Jembatan Wheatstone Dikatakan Seimbang?", "id": "Saat Voltmeter Menunjukkan Nol." },
  { "en": "Apa Kepanjangan LCR Meter?", "id": "Alat Ukur L (Induktansi), C (Kapasitansi), R (Resistansi)." },
  { "en": "Apa Fungsi Generator Sinyal (Function Generator)?", "id": "Menghasilkan Berbagai Bentuk Sinyal Uji." },
  { "en": "Sebutkan Tiga Bentuk Sinyal Generator Sinyal?", "id": "Sinus, Kotak, Dan Segitiga." },
  { "en": "Apa Fungsi Catu Daya (Power Supply)?", "id": "Menyediakan Sumber Tegangan DC Stabil." },
  { "en": "Apa Itu Konversi Analog Ke Digital (ADC)?", "id": "Mengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Kepanjangan ADC (Analog-to-Digital Converter)?", "id": "Pengubah Analog Ke Digital." },
  { "en": "Apa Itu Konversi Digital Ke Analog (DAC)?", "id": "Mengubah Sinyal Digital Menjadi Analog." },
  { "en": "Apa Kepanjangan DAC (Digital-to-Analog Converter)?", "id": "Pengubah Digital Ke Analog." },
  { "en": "Apa Satuan Untuk Konduktansi?", "id": "Siemens (S)." },
  { "en": "Apa Kebalikan Dari Resistansi?", "id": "Konduktansi." },
  { "en": "Apa Kebalikan Dari Impedansi?", "id": "Admitansi." },
  { "en": "Apa Gerbang Logika Dasar (Basic Logic Gate)?", "id": "AND, OR, Dan NOT." },
  { "en": "Apa Output Gerbang AND (AND Gate) Jika Input 1, 1?", "id": "Output Adalah 1." },
  { "en": "Apa Output Gerbang OR (OR Gate) Jika Input 0, 0?", "id": "Output Adalah 0." },
  { "en": "Apa Output Gerbang NOT (NOT Gate) Jika Input 1?", "id": "Output Adalah 0." },
  { "en": "Apa Nama Lain Gerbang NOT (NOT Gate)?", "id": "Inverter (Pembalik)." },
  { "en": "Apa Kepanjangan Gerbang NAND (Not AND)?", "id": "Gerbang NOT AND." },
  { "en": "Apa Kepanjangan Gerbang NOR (Not OR)?", "id": "Gerbang NOT OR." },
  { "en": "Apa Kepanjangan Gerbang XOR (Exclusive OR)?", "id": "Gerbang Exclusive OR." },
  { "en": "Kapan Output XOR (Exclusive OR) Bernilai 1?", "id": "Saat Kedua Input Berbeda." },
  { "en": "Kapan Output XNOR (Exclusive NOR) Bernilai 1?", "id": "Saat Kedua Input Sama." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Untuk Logika Digital." },
  { "en": "Siapa Penemu Aljabar Boolean (Boolean Algebra)?", "id": "George Boole." },
  { "en": "Apa Itu Tabel Kebenaran (Truth Table)?", "id": "Tabel Output Berdasarkan Input Logika." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit)?", "id": "Sirkuit Terpadu." },
  { "en": "Apa Fungsi IC (Integrated Circuit)?", "id": "Mengintegrasikan Banyak Komponen Elektronik." },
  { "en": "Apa Itu Flip-Flop Dalam Logika Digital?", "id": "Elemen Memori Penyimpan 1 Bit." },
  { "en": "Apa Kepanjangan TTL (Transistor-Transistor Logic)?", "id": "Logika Transistor-Transistor." },
  { "en": "Apa Kepanjangan CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Semikonduktor Oksida Logam Komplementer." },
  { "en": "Apa Keunggulan CMOS (Complementary Metal-Oxide-Semiconductor) Dibanding TTL?", "id": "Konsumsi Daya Lebih Rendah." },
  { "en": "Apa Itu Sistem Bilangan Biner (Binary System)?", "id": "Sistem Bilangan Basis Dua (0, 1)." },
  { "en": "Apa Itu Bit (Binary Digit)?", "id": "Satuan Data Terkecil (0 Atau 1)." },
  { "en": "Berapa Bit Dalam Satu Byte (Byte)?", "id": "Satu Byte Adalah 8 Bit." },
  { "en": "Apa Kepanjangan Op-Amp (Operational Amplifier)?", "id": "Penguat Operasional." },
  { "en": "Apa Fungsi Utama Op-Amp (Operational Amplifier)?", "id": "Menguatkan Sinyal Listrik." },
  { "en": "Berapa Idealnya Impedansi Input Op-Amp?", "id": "Impedansi Input Ideal Tak Terhingga." },
  { "en": "Berapa Idealnya Impedansi Output Op-Amp?", "id": "Impedansi Output Ideal Nol." },
  { "en": "Berapa Idealnya Penguatan (Gain) Op-Amp Loop Terbuka?", "id": "Penguatan Ideal Tak Terhingga." },
  { "en": "Apa Itu Rangkaian Inverting Op-Amp?", "id": "Membalik Fasa Sinyal Output." },
  { "en": "Apa Itu Rangkaian Non-Inverting Op-Amp?", "id": "Fasa Sinyal Output Sama Input." },
  { "en": "Apa Itu Voltage Follower (Pengikut Tegangan)?", "id": "Rangkaian Op-Amp Dengan Penguatan 1." },
  { "en": "Apa Fungsi Voltage Follower (Pengikut Tegangan)?", "id": "Sebagai Penyangga (Buffer) Impedansi." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Mengembalikan Output Ke Input Inverting." },
  { "en": "Apa Manfaat Umpan Balik Negatif (Negative Feedback)?", "id": "Membuat Penguatan Stabil Terkontrol." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Alat Pendeteksi Besaran Fisik." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Pengubah Satu Bentuk Energi Ke Lainnya." },
  { "en": "Apa Fungsi Termokopel (Thermocouple)?", "id": "Sensor Suhu Berbasis Efek Seebeck." },
  { "en": "Apa Fungsi Strain Gauge (Strain Gauge)?", "id": "Sensor Pengukur Regangan Mekanis." },
  { "en": "Apa Fungsi Mikrofon (Microphone)?", "id": "Transduser Suara Menjadi Sinyal Listrik." },
  { "en": "Apa Fungsi Pengeras Suara (Loudspeaker)?", "id": "Transduser Sinyal Listrik Menjadi Suara." },
  { "en": "Apa Fungsi Sensor Ultrasonik (Ultrasonic Sensor)?", "id": "Mengukur Jarak Menggunakan Gelombang Suara." },
  { "en": "Apa Fungsi Sensor PIR (Passive Infrared)?", "id": "Mendeteksi Gerakan Berbasis Panas Tubuh." },
  { "en": "Apa Bagian Berputar Pada Motor Listrik?", "id": "Rotor (Bagian Bergerak)." },
  { "en": "Apa Bagian Diam Pada Motor Listrik?", "id": "Stator (Bagian Diam)." },
  { "en": "Apa Fungsi Sikat (Brush) Pada Motor DC?", "id": "Menghantarkan Arus Ke Komutator." },
  { "en": "Apa Fungsi Komutator (Commutator) Pada Motor DC?", "id": "Membalik Arah Arus Di Rotor." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Yang Bergerak Per Langkah." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Dengan Kontrol Posisi Presisi." },
  { "en": "Apa Itu Motor Induksi (Induction Motor)?", "id": "Motor AC Berbasis Induksi Elektromagnetik." },
  { "en": "Siapa Penemu Motor Induksi (Induction Motor)?", "id": "Nikola Tesla." },
  { "en": "Apa Itu Slip Pada Motor Induksi?", "id": "Beda Kecepatan Rotor, Medan Magnet." },
  { "en": "Apa Itu Penyearah Setengah Gelombang (Half-Wave Rectifier)?", "id": "Menyearahkan Setengah Siklus AC." },
  { "en": "Apa Itu Penyearah Gelombang Penuh (Full-Wave Rectifier)?", "id": "Menyearahkan Seluruh Siklus AC." },
  { "en": "Berapa Dioda Untuk Jembatan Penyearah (Bridge Rectifier)?", "id": "Empat Buah Dioda." },
  { "en": "Apa Fungsi Filter Kapasitor Setelah Penyearah?", "id": "Menghaluskan Atau Meratakan Tegangan DC." },
  { "en": "Apa Itu Riak (Ripple) Pada Catu Daya DC?", "id": "Sisa Variasi Tegangan AC." },
  { "en": "Apa Itu Regulator Tegangan (Voltage Regulator)?", "id": "Komponen Penstabil Tegangan Output DC." },
  { "en": "Sebutkan Contoh IC (Integrated Circuit) Regulator Tegangan Linear?", "id": "IC 7805 (Output 5 Volt)." },
  { "en": "Apa Kepanjangan SMPS (Switch-Mode Power Supply)?", "id": "Catu Daya Mode Saklar." },
  { "en": "Apa Keunggulan SMPS (Switch-Mode Power Supply) Dibanding Linear?", "id": "Efisiensi Jauh Lebih Tinggi." },
  { "en": "Apa Itu Konverter DC-DC (DC-DC Converter)?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Itu Konverter Buck (Buck Converter)?", "id": "Konverter Penurun Tegangan DC." },
  { "en": "Apa Itu Konverter Boost (Boost Converter)?", "id": "Konverter Penaik Tegangan DC." },
  { "en": "Apa Itu Konverter Buck-Boost (Buck-Boost Converter)?", "id": "Konverter Penaik Atau Penurun Tegangan." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Pengubah Tegangan DC Menjadi AC." },
  { "en": "Apa Kepanjangan PPE (Personal Protective Equipment)?", "id": "Alat Pelindung Diri (APD)." },
  { "en": "Sebutkan Contoh PPE (Personal Protective Equipment) Elektro?", "id": "Sarung Tangan Karet, Kacamata Keselamatan." },
  { "en": "Apa Kepanjangan ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Elektrostatik." },
  { "en": "Mengapa ESD (Electrostatic Discharge) Berbahaya?", "id": "Dapat Merusak Komponen Elektronika." },
  { "en": "Bagaimana Cara Mencegah ESD (Electrostatic Discharge)?", "id": "Menggunakan Gelang Anti-Statis." },
  { "en": "Apa Itu Amplitudo (Amplitude) Sinyal?", "id": "Nilai Puncak Maksimum Sinyal." },
  { "en": "Apa Itu Periode (Period) Sinyal?", "id": "Waktu Untuk Satu Siklus Lengkap." },
  { "en": "Apa Hubungan Frekuensi Dan Periode Sinyal?", "id": "Frekuensi = 1 / Periode (f=1/T)." },
  { "en": "Apa Itu Panjang Gelombang (Wavelength)?", "id": "Jarak Spasial Satu Siklus Gelombang." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Persentase Waktu Sinyal 'On' (Tinggi)." },
  { "en": "Berapa Siklus Kerja (Duty Cycle) Gelombang Kotak Simetris?", "id": "50 Persen." },
  { "en": "Apa Kepanjangan VDR (Voltage Dependent Resistor)?", "id": "Resistor Tergantung Tegangan." },
  { "en": "Apa Nama Lain VDR (Voltage Dependent Resistor)?", "id": "Varistor (Atau MOV)." },
  { "en": "Apa Fungsi Varistor (VDR)?", "id": "Melindungi Rangkaian Dari Lonjakan Tegangan." },
  { "en": "Apa Itu Dioda Schottky (Schottky Diode)?", "id": "Dioda Dengan Tegangan Maju Rendah." },
  { "en": "Apa Keunggulan Dioda Schottky (Schottky Diode)?", "id": "Waktu Switching Sangat Cepat." },
  { "en": "Apa Itu Dioda Varactor (Varicap Diode)?", "id": "Dioda Dengan Kapasitansi Terkendali Tegangan." },
  { "en": "Dimana Dioda Varactor (Varicap Diode) Digunakan?", "id": "Pada Rangkaian Penala (Tuner) Radio." },
  { "en": "Apa Itu Kristal Osilator (Crystal Oscillator)?", "id": "Komponen Penghasil Frekuensi Sangat Stabil." },
  { "en": "Bahan Apa Yang Digunakan Kristal Osilator?", "id": "Bahan Piezoelektrik (Contoh: Kuarsa)." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Bahan Hasilkan Listrik Saat Ditekan." },
  { "en": "Apa Itu Teorema Thevenin (Thevenin's Theorem)?", "id": "Menyederhanakan Rangkaian Menjadi Vth, Rth." },
  { "en": "Apa Itu Teorema Norton (Norton's Theorem)?", "id": "Menyederhanakan Rangkaian Menjadi In, Rn." },
  { "en": "Apa Itu Teorema Superposisi (Superposition Theorem)?", "id": "Menganalisis Rangkaian Linear Multi-Sumber." },
  { "en": "Apa Itu Transfer Daya Maksimum (Maximum Power Transfer)?", "id": "Beban Menerima Daya Maksimal." },
  { "en": "Kapan Transfer Daya Maksimum (Maximum Power Transfer) Terjadi?", "id": "Saat Hambatan Beban Sama Sumber." },
  { "en": "Apa Itu Hubungan Delta (Delta Connection) (Segitiga)?", "id": "Koneksi Tiga Fasa Bentuk Segitiga." },
  { "en": "Apa Itu Hubungan Wye (Wye Connection) (Bintang)?", "id": "Koneksi Tiga Fasa Bentuk Bintang." },
  { "en": "Apa Itu Arus Fasa (Phase Current)?", "id": "Arus Yang Mengalir Pada Satu Fasa." },
  { "en": "Apa Itu Arus Saluran (Line Current)?", "id": "Arus Yang Mengalir Pada Saluran Transmisi." },
  { "en": "Apa Itu Tegangan Fasa (Phase Voltage)?", "id": "Tegangan Antara Fasa Dan Netral." },
  { "en": "Apa Itu Tegangan Saluran (Line Voltage)?", "id": "Tegangan Antara Dua Fasa Berbeda." },
  { "en": "Apa Itu Harmonisa (Harmonics) Dalam Sistem Tenaga?", "id": "Frekuensi Kelipatan Dari Frekuensi Fundamental." },
  { "en": "Apa Efek Buruk Harmonisa (Harmonics)?", "id": "Menyebabkan Pemanasan Berlebih Pada Peralatan." },
  { "en": "Apa Itu Bandwidth (Lebar Pita)?", "id": "Rentang Frekuensi Operasi Efektif." },
  { "en": "Apa Itu Decibel (Decibel) (dB)?", "id": "Satuan Logaritmik Pengukur Penguatan." },
  { "en": "Apa Itu Register Dalam Logika Digital?", "id": "Sekumpulan Flip-Flop Penyimpan Data." },
  { "en": "Apa Fungsi Dari Shift Register (Register Geser)?", "id": "Menggeser Data Bit Per Bit." },
  { "en": "Apa Itu Counter (Pencacah) Digital?", "id": "Rangkaian Pencacah Urutan Pulsa." },
  { "en": "Apa Itu Counter Asinkron (Asynchronous Counter)?", "id": "Pencacah Riak (Ripple Counter)." },
  { "en": "Apa Itu Counter Sinkron (Synchronous Counter)?", "id": "Flip-Flop Dipicu Clock Bersamaan." },
  { "en": "Apa Kepanjangan MUX (Multiplexer)?", "id": "Multiplexer." },
  { "en": "Apa Fungsi MUX (Multiplexer)?", "id": "Memilih Satu Input Ke Output." },
  { "en": "Apa Nama Lain MUX (Multiplexer)?", "id": "Selektor Data (Data Selector)." },
  { "en": "Apa Kepanjangan DEMUX (Demultiplexer)?", "id": "Demultiplexer." },
  { "en": "Apa Fungsi DEMUX (Demultiplexer)?", "id": "Mengarahkan Input Ke Banyak Output." },
  { "en": "Apa Itu Encoder (Penyandi)?", "id": "Mengubah Input Aktif Menjadi Kode Biner." },
  { "en": "Apa Itu Decoder (Pengurai Sandi)?", "id": "Mengubah Kode Biner Menjadi Output Aktif." },
  { "en": "Sebutkan Contoh IC (Integrated Circuit) Decoder Populer?", "id": "IC 7447 (BCD Ke 7-Segment)." },
  { "en": "Apa Itu Rangkaian Aritmetika (Arithmetic Circuit)?", "id": "Rangkaian Operasi Matematika Digital." },
  { "en": "Apa Itu Half Adder (Penjumlah Setengah)?", "id": "Menjumlahkan Dua Buah 1-Bit." },
  { "en": "Apa Itu Full Adder (Penjumlah Penuh)?", "id": "Menjumlahkan Tiga Buah 1-Bit." },
  { "en": "Apa Output Dari Half Adder (Penjumlah Setengah)?", "id": "Sum (Jumlah) Dan Carry (Simpanan)." },
  { "en": "Apa Kepanjangan ALU (Arithmetic Logic Unit)?", "id": "Unit Aritmetika Dan Logika." },
  { "en": "Dimana ALU (Arithmetic Logic Unit) Ditemukan?", "id": "Di Dalam CPU." },
  { "en": "Apa Kepanjangan CPU (Central Processing Unit)?", "id": "Unit Pemrosesan Pusat." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Beda Utama Mikrokontroler Dan Mikroprosesor?", "id": "Mikrokontroler Memiliki RAM, ROM Internal." },
  { "en": "Sebutkan Contoh Mikrokontroler Populer?", "id": "Arduino (Atmega) Atau ESP32." },
  { "en": "Apa Kepanjangan RAM (Random Access Memory)?", "id": "Memori Akses Acak." },
  { "en": "Apa Sifat RAM (Random Access Memory)?", "id": "Volatile (Data Hilang Tanpa Listrik)." },
  { "en": "Apa Kepanjangan ROM (Read Only Memory)?", "id": "Memori Hanya Baca." },
  { "en": "Apa Sifat ROM (Read Only Memory)?", "id": "Non-Volatile (Data Tetap Tersimpan)." },
  { "en": "Apa Kepanjangan EEPROM (Electrically Erasable Programmable ROM)?", "id": "ROM Terprogram Dapat Dihapus Listrik." },
  { "en": "Apa Kepanjangan GPIO (General Purpose Input Output)?", "id": "Pin Input Output Serbaguna." },
  { "en": "Apa Fungsi Pin GPIO (General Purpose Input Output)?", "id": "Membaca Sensor Atau Mengontrol Aktuator." },
  { "en": "Apa Itu Bahasa Assembly (Assembly Language)?", "id": "Bahasa Pemrograman Level Rendah." },
  { "en": "Apa Itu Resistor SMD (Surface Mount Device)?", "id": "Resistor Pemasangan Permukaan." },
  { "en": "Apa Itu Komponen Through-Hole (Tembus Lubang)?", "id": "Komponen Dengan Kaki Menembus PCB." },
  { "en": "Apa Keuntungan SMD (Surface Mount Device) Dibanding Through-Hole?", "id": "Ukuran Jauh Lebih Kecil." },
  { "en": "Bagaimana Cara Membaca Nilai Resistor Gelang Warna?", "id": "Menggunakan Kode Gelang Warna." },
  { "en": "Warna Apa Untuk Toleransi 5 Persen Resistor?", "id": "Gelang Warna Emas." },
  { "en": "Warna Apa Untuk Toleransi 10 Persen Resistor?", "id": "Gelang Warna Perak." },
  { "en": "Bagaimana Cara Membaca Kode Resistor SMD (Surface Mount Device)?", "id": "Menggunakan Kode Angka (Contoh: 104)." },
  { "en": "Apa Arti Kode 104 Pada Resistor SMD?", "id": "100.000 Ohm (100 Kilo Ohm)." },
  { "en": "Apa Itu Kapasitor Elektrolit (Elco)?", "id": "Kapasitor Polar (Berpolaritas)." },
  { "en": "Apa Ciri Kapasitor Elektrolit (Elco)?", "id": "Memiliki Polaritas Positif, Negatif." },
  { "en": "Apa Yang Terjadi Jika Elco Terbalik Polaritas?", "id": "Dapat Meledak Atau Rusak." },
  { "en": "Apa Itu Kapasitor Keramik (Ceramic Capacitor)?", "id": "Kapasitor Non-Polar Bernilai Kecil." },
  { "en": "Apa Fungsi Kapasitor Bypass (Bypass Capacitor)?", "id": "Menyaring Noise Frekuensi Tinggi Ke Ground." },
  { "en": "Apa Fungsi Kapasitor Kopling (Coupling Capacitor)?", "id": "Melewatkan Sinyal AC, Memblokir DC." },
  { "en": "Apa Itu Kapasitor Tantalum (Tantalum Capacitor)?", "id": "Kapasitor Polar Kepadatan Tinggi." },
  { "en": "Apa Itu Dioda Bridge (Bridge Diode) IC?", "id": "Empat Dioda Dalam Satu Paket." },
  { "en": "Apa Tanda Kaki Positif (+) Pada Dioda Bridge?", "id": "Kaki Output DC Positif." },
  { "en": "Apa Tanda Kaki (~) Pada Dioda Bridge?", "id": "Kaki Input AC (Bolak-Balik)." },
  { "en": "Apa Kepanjangan JFET (Junction Field Effect Transistor)?", "id": "Transistor Efek Medan Sambungan." },
  { "en": "Apa Kepanjangan MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET Semikonduktor Oksida Logam." },
  { "en": "Apa Tiga Kaki Pada MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Gate, Drain, Dan Source." },
  { "en": "Apa Keunggulan MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Dikontrol Tegangan, Impedansi Input Tinggi." },
  { "en": "Apa Kepanjangan IGBT (Insulated Gate Bipolar Transistor)?", "id": "Transistor Bipolar Gerbang Terisolasi." },
  { "en": "Apa Gabungan Dari IGBT (Insulated Gate Bipolar Transistor)?", "id": "Gabungan Keunggulan MOSFET, BJT." },
  { "en": "Apa Fungsi Gardu Induk (Substation)?", "id": "Transformasi, Distribusi, Proteksi Listrik." },
  { "en": "Apa Kepanjangan SUTET (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "Saluran Udara Tegangan Ekstra Tinggi." },
  { "en": "Mengapa Transmisi Listrik Menggunakan Tegangan Tinggi?", "id": "Mengurangi Rugi-Rugi Daya (I²R)." },
  { "en": "Apa Fungsi Trafo Distribusi (Distribution Transformer)?", "id": "Menurunkan Tegangan Menengah Ke Konsumen." },
  { "en": "Berapa Tegangan Rendah Standar PLN Di Indonesia?", "id": "220 Volt (Fasa Ke Netral)." },
  { "en": "Berapa Tegangan Saluran (Line) Tiga Fasa PLN?", "id": "380 Volt (Antar Fasa)." },
  { "en": "Apa Itu Netral Dalam Sistem Tiga Fasa?", "id": "Titik Tengah Sambungan Bintang (Wye)." },
  { "en": "Apa Itu Load Balancing (Penyeimbangan Beban)?", "id": "Membagi Beban Sama Rata Antar Fasa." },
  { "en": "Apa Itu Kawat Fasa (Phase Wire)?", "id": "Kawat Bertegangan (Live Wire)." },
  { "en": "Apa Itu Kawat Netral (Neutral Wire)?", "id": "Kawat Kembali (Return Wire) Nol Volt." },
  { "en": "Apa Itu Kawat Ground (Ground Wire) (Arde)?", "id": "Kawat Pengaman Menuju Tanah." },
  { "en": "Apa Warna Kabel Fasa Standar PUIL?", "id": "Hitam, Cokelat, Atau Abu-Abu." },
  { "en": "Apa Warna Kabel Netral Standar PUIL?", "id": "Biru." },
  { "en": "Apa Warna Kabel Ground (Arde) Standar PUIL?", "id": "Hijau-Kuning." },
  { "en": "Apa Kepanjangan PUIL (Peraturan Umum Instalasi Listrik)?", "id": "Peraturan Umum Instalasi Listrik." },
  { "en": "Apa Kepanjangan ELCB (Earth Leakage Circuit Breaker)?", "id": "Pemutus Sirkuit Kebocoran Tanah." },
  { "en": "Apa Fungsi ELCB (Earth Leakage Circuit Breaker)?", "id": "Melindungi Manusia Dari Sengatan Listrik." },
  { "en": "Apa Nama Lain ELCB (Earth Leakage Circuit Breaker)?", "id": "RCCB (Residual Current Circuit Breaker)." },
  { "en": "Apa Kepanjangan GFCI (Ground Fault Circuit Interrupter)?", "id": "Interuptor Sirkuit Gangguan Tanah." },
  { "en": "Bagaimana Prinsip Kerja ELCB (Earth Leakage Circuit Breaker)?", "id": "Membandingkan Arus Fasa, Netral." },
  { "en": "Apa Itu Overload (Beban Berlebih)?", "id": "Arus Melebihi Nilai Normal Terlalu Lama." },
  { "en": "Apa Itu Short Circuit (Hubungan Singkat)?", "id": "Kontak Langsung Antara Fasa, Netral." },
  { "en": "Apa Yang Melindungi Rangkaian Dari Overload (Beban Berlebih)?", "id": "MCB (Thermal) Atau Sekring." },
  { "en": "Apa Yang Melindungi Rangkaian Dari Short Circuit (Hubungan Singkat)?", "id": "MCB (Magnetik) Atau Sekring." },
  { "en": "Apa Itu Surge Arrester (Penangkal Petir)?", "id": "Melindungi Jaringan Dari Sambaran Petir." },
  { "en": "Apa Itu Isolasi Kelas I (Class I Insulation)?", "id": "Peralatan Dengan Koneksi Ground (Arde)." },
  { "en": "Apa Itu Isolasi Kelas II (Class II Insulation)?", "id": "Peralatan Dengan Isolasi Ganda." },
  { "en": "Apa Simbol Isolasi Kelas II (Class II Insulation)?", "id": "Kotak Di Dalam Kotak." },
  { "en": "Apa Itu Lock Out Tag Out (LOTO)?", "id": "Prosedur Keselamatan Perbaikan Listrik." },
  { "en": "Apa Kepanjangan RF (Radio Frequency)?", "id": "Frekuensi Radio." },
  { "en": "Apa Fungsi Antena (Antenna)?", "id": "Mengubah Sinyal Listrik Ke Gelombang Elektromagnetik." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Sinyal Informasi Ke Pembawa." },
  { "en": "Apa Kepanjangan AM (Amplitude Modulation)?", "id": "Modulasi Amplitudo." },
  { "en": "Apa Kepanjangan FM (Frequency Modulation)?", "id": "Modulasi Frekuensi." },
  { "en": "Apa Yang Diubah Pada AM (Amplitude Modulation)?", "id": "Amplitudo Gelombang Pembawa (Carrier)." },
  { "en": "Apa Yang Diubah Pada FM (Frequency Modulation)?", "id": "Frekuensi Gelombang Pembawa (Carrier)." },
  { "en": "Apa Keunggulan FM (Frequency Modulation) Dibanding AM?", "id": "Lebih Tahan Terhadap Gangguan (Noise)." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Memisahkan Sinyal Informasi Dari Pembawa." },
  { "en": "Dimana Proses Demodulasi (Demodulation) Terjadi?", "id": "Di Dalam Penerima (Receiver) Radio." },
  { "en": "Apa Itu Spektrum Elektromagnetik (Electromagnetic Spectrum)?", "id": "Rentang Semua Frekuensi Radiasi Elektromagnetik." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Media Penyalur Sinyal RF (Kabel Koaksial)." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance)?", "id": "Impedansi Khas Saluran Transmisi." },
  { "en": "Berapa Impedansi Kabel Koaksial TV (Coaxial Cable)?", "id": "Umumnya 75 Ohm." },
  { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Pelemahan Sinyal Seiring Jarak." },
  { "en": "Apa Kepanjangan SWR (Standing Wave Ratio)?", "id": "Rasio Gelombang Berdiri." },
  { "en": "Apa Nilai SWR (Standing Wave Ratio) Yang Ideal?", "id": "Nilai Ideal Adalah 1:1." },
  { "en": "Apa Penyebab SWR (Standing Wave Ratio) Yang Buruk?", "id": "Ketidakcocokan Impedansi (Mismatch)." },
  { "en": "Apa Fungsi Smith Chart (Bagan Smith)?", "id": "Alat Bantu Perhitungan Saluran Transmisi." },
  { "en": "Apa Itu Penguat RF (RF Amplifier)?", "id": "Rangkaian Penguat Sinyal Frekuensi Radio." },
  { "en": "Apa Itu Mixer (Pencampur) Dalam Radio?", "id": "Mencampur Dua Frekuensi Menghasilkan Frekuensi Baru." },
  { "en": "Apa Itu Osilator Lokal (Local Oscillator)?", "id": "Penghasil Frekuensi Internal Penerima Radio." },
  { "en": "Apa Itu Frekuensi Menengah (Intermediate Frequency)?", "id": "Frekuensi Hasil Pencampuran Di Penerima." },
  { "en": "Apa Kepanjangan IF (Intermediate Frequency)?", "id": "Frekuensi Menengah." },
  { "en": "Apa Nama Penerima Radio Paling Umum?", "id": "Penerima Superheterodyne." },
  { "en": "Apa Kepanjangan DSP (Digital Signal Processing)?", "id": "Pemrosesan Sinyal Digital." },
  { "en": "Apa Fungsi DSP (Digital Signal Processing)?", "id": "Memanipulasi Sinyal Menggunakan Komputer Digital." },
  { "en": "Apa Itu Noise (Derau) Dalam Sinyal?", "id": "Sinyal Acak Yang Tidak Diinginkan." },
  { "en": "Apa Kepanjangan SNR (Signal-to-Noise Ratio)?", "id": "Rasio Sinyal Terhadap Derau." },
  { "en": "Apakah SNR (Signal-to-Noise Ratio) Tinggi Baik?", "id": "Ya, SNR Tinggi Berarti Sinyal Bersih." },
  { "en": "Apa Itu Distorsi (Distortion) Sinyal?", "id": "Perubahan Bentuk Sinyal Asli." },
  { "en": "Apa Itu Distorsi Harmonik (Harmonic Distortion)?", "id": "Munculnya Frekuensi Harmonisa Yang Tidak Diinginkan." },
  { "en": "Apa Itu Intermodulasi (Intermodulation)?", "id": "Pencampuran Sinyal Yang Menghasilkan Frekuensi Baru." },
  { "en": "Apa Kepanjangan THD (Total Harmonic Distortion)?", "id": "Total Distorsi Harmonik." },
  { "en": "Apa Itu Sistem Kontrol Loop Terbuka?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Kontrol Loop Tertutup?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengembalikan Sebagian Output Ke Input." },
  { "en": "Apa Itu Setpoint (Titik Setel) Kontrol?", "id": "Nilai Referensi Yang Diinginkan." },
  { "en": "Apa Itu Error (Kesalahan) Dalam Sistem Kontrol?", "id": "Selisih Antara Setpoint Dan Output." },
  { "en": "Apa Kepanjangan PID (Proportional Integral Derivative)?", "id": "Kontroler Proportional Integral Derivative." },
  { "en": "Apa Fungsi Kontroler PID (Proportional Integral Derivative)?", "id": "Mengontrol Sistem Agar Stabil, Akurat." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Komponen Penggerak (Contoh: Motor)." },
  { "en": "Apa Itu Sistem Waktu Nyata (Real-Time System)?", "id": "Sistem Yang Merespon Dalam Batas Waktu." },
  { "en": "Apa Kepanjangan PLC (Programmable Logic Controller)?", "id": "Kontroler Logika Terprogram." },
  { "en": "Dimana PLC (Programmable Logic Controller) Digunakan?", "id": "Otomatisasi Industri Dan Mesin." },
  { "en": "Apa Bahasa Pemrograman Umum PLC?", "id": "Ladder Logic (Diagram Tangga)." },
  { "en": "Apa Kepanjangan SCADA (Supervisory Control and Data Acquisition)?", "id": "Kontrol Pengawasan Dan Akuisisi Data." },
  { "en": "Apa Fungsi SCADA (Supervisory Control and Data Acquisition)?", "id": "Memantau, Mengendalikan Proses Industri Skala Besar." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka Grafis Antara Manusia, Mesin." },
  { "en": "Apa Itu Bus Komunikasi (Communication Bus)?", "id": "Jalur Komunikasi Data Bersama." },
  { "en": "Sebutkan Contoh Bus Lapangan (Fieldbus)?", "id": "Modbus, Profibus." },
  { "en": "Apa Itu Optocoupler (Optoisolator)?", "id": "Isolator Sirkuit Menggunakan Cahaya." },
  { "en": "Apa Fungsi Optocoupler (Optoisolator)?", "id": "Mengisolasi Dua Bagian Rangkaian Listrik." },
  { "en": "Apa Komponen Internal Optocoupler (Optoisolator)?", "id": "LED Dan Fototransistor." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Yang Peka Terhadap Cahaya." },
  { "en": "Apa Itu Fototransistor (Phototransistor)?", "id": "Transistor Yang Dikontrol Intensitas Cahaya." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Komponen Pengubah Cahaya Matahari Ke Listrik." },
  { "en": "Apa Kepanjangan PV (Photovoltaic)?", "id": "Photovoltaic (Terkait Sel Surya)." },
  { "en": "Apa Efek Hall (Hall Effect)?", "id": "Tegangan Muncul Akibat Medan Magnet." },
  { "en": "Apa Fungsi Sensor Efek Hall (Hall Effect)?", "id": "Mendeteksi Kehadiran Medan Magnet." },
  { "en": "Dimana Sensor Efek Hall (Hall Effect) Digunakan?", "id": "Sensor Kecepatan Roda (ABS), Motor BLDC." },
  { "en": "Apa Kepanjangan BLDC (Brushless DC Motor)?", "id": "Motor DC Tanpa Sikat." },
  { "en": "Apa Keunggulan Motor BLDC (Brushless DC Motor)?", "id": "Lebih Efisien, Awet, Perawatan Minim." },
  { "en": "Apa Fungsi Solid State Relay (SSR)?", "id": "Relay Elektronik Tanpa Bagian Bergerak." },
  { "en": "Apa Kepanjangan SSR (Solid State Relay)?", "id": "Relay Keadaan Padat." },
  { "en": "Apa Keunggulan SSR (Solid State Relay) Dibanding Relay Mekanis?", "id": "Lebih Cepat, Tahan Lama, Tidak Berisik." },
  { "en": "Apa Itu Peta Karnaugh (Karnaugh Map)?", "id": "Metode Grafis Penyederhanaan Logika Boolean." },
  { "en": "Apa Kepanjangan K-Map (Karnaugh Map)?", "id": "Peta Karnaugh." },
  { "en": "Apa Itu Rangkaian Kombinasional (Combinational Circuit)?", "id": "Output Tergantung Input Saat Ini." },
  { "en": "Apa Itu Rangkaian Sekuensial (Sequential Circuit)?", "id": "Output Tergantung Input, Keadaan Sebelumnya." },
  { "en": "Contoh Rangkaian Kombinasional?", "id": "Decoder, Multiplexer, Adder." },
  { "en": "Contoh Rangkaian Sekuensial?", "id": "Flip-Flop, Counter, Register." },
  { "en": "Apa Itu Sinyal Clock (Detak)?", "id": "Sinyal Digital Sinkronisasi Rangkaian." },
  { "en": "Apa Itu Setup Time (Waktu Pengaturan)?", "id": "Waktu Input Stabil Sebelum Clock." },
  { "en": "Apa Itu Hold Time (Waktu Penahanan)?", "id": "Waktu Input Stabil Setelah Clock." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "Array Gerbang Terprogram Lapangan." },
  { "en": "Apa Itu CPLD (Complex Programmable Logic Device)?", "id": "Perangkat Logika Terprogram Kompleks." },
  { "en": "Apa Bahasa Deskripsi Perangkat Keras (Hardware Description Language)?", "id": "VHDL Atau Verilog." },
  { "en": "Apa Kepanjangan VHDL (VHSIC Hardware Description Language)?", "id": "Bahasa Deskripsi Perangkat Keras VHSIC." },
  { "en": "Apa Fungsi Penganalisis Logika (Logic Analyzer)?", "id": "Alat Ukur Sinyal Digital." },
  { "en": "Apa Fungsi Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Mengukur Sinyal Domain Frekuensi." },
  { "en": "Apa Itu Domain Waktu (Time Domain)?", "id": "Representasi Sinyal Terhadap Waktu." },
  { "en": "Apa Itu Domain Frekuensi (Frequency Domain)?", "id": "Representasi Sinyal Terhadap Frekuensi." },
  { "en": "Alat Apa Yang Menampilkan Sinyal Domain Waktu?", "id": "Osiloskop." },
  { "en": "Alat Apa Yang Mengubah Domain Waktu Ke Frekuensi?", "id": "Transformasi Fourier (Contoh: FFT)." },
  { "en": "Apa Kepanjangan FFT (Fast Fourier Transform)?", "id": "Transformasi Fourier Cepat." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Kompatibilitas Elektromagnetik." },
  { "en": "Apa Kepanjangan EMI (Electromagnetic Interference)?", "id": "Interferensi Elektromagnetik." },
  { "en": "Apa Itu Interferensi EMI (Electromagnetic Interference)?", "id": "Gangguan Elektromagnetik Merusak Sinyal." },
  { "en": "Bagaimana Cara Mengurangi EMI (Electromagnetic Interference)?", "id": "Menggunakan Pelindung (Shielding), Filter." },
  { "en": "Apa Itu Kandang Faraday (Faraday Cage)?", "id": "Pelindung Menghalangi Medan Elektromagnetik." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Tegangan Maksimum Isolator Sebelum Tembus." },
  { "en": "Apa Satuan Kekuatan Dielektrik (Dielectric Strength)?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant)?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
  { "en": "Apa Fungsi Tang Ampere (Clamp Meter)?", "id": "Mengukur Arus Listrik Tanpa Memutus Rangkaian." },
  { "en": "Bagaimana Prinsip Kerja Tang Ampere (Clamp Meter)?", "id": "Prinsip Induksi Magnetik (Trafo Arus)." },
  { "en": "Apa Fungsi Wattmeter (Wattmeter)?", "id": "Alat Mengukur Daya Listrik Nyata." },
  { "en": "Apa Fungsi kWh Meter (kWh Meter)?", "id": "Alat Mengukur Konsumsi Energi Listrik." },
  { "en": "Apa Fungsi Megohmmeter (Insulation Tester)?", "id": "Mengukur Resistansi Isolasi Sangat Tinggi." },
  { "en": "Apa Nama Lain Megohmmeter (Insulation Tester)?", "id": "Megger." },
  { "en": "Apa Fungsi Earth Tester (Penguji Bumi)?", "id": "Mengukur Resistansi Sistem Pembumian." },
  { "en": "Berapa Nilai Resistansi Grounding (Pembumian) Yang Baik?", "id": "Idealnya Di Bawah 1 Ohm." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Jaringan Pembangkit, Transmisi, Distribusi." },
  { "en": "Apa Fungsi Pembangkit Listrik (Power Plant)?", "id": "Menghasilkan Energi Listrik." },
  { "en": "Apa Fungsi Transmisi Listrik (Power Transmission)?", "id": "Menyalurkan Listrik Jarak Jauh." },
  { "en": "Apa Fungsi Distribusi Listrik (Power Distribution)?", "id": "Membagikan Listrik Ke Konsumen." },
  { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Dengan Kontrol Cerdas." },
  { "en": "Apa Itu Pelepasan Beban (Load Shedding)?", "id": "Pemadaman Listrik Terencana Mencegah Kolaps." },
  { "en": "Apa Itu Blackout (Padam Total)?", "id": "Hilangnya Daya Skala Besar." },
  { "en": "Apa Fungsi Kapasitor Bank Di Jaringan Listrik?", "id": "Memperbaiki Faktor Daya (Power Factor)." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Alat Pendeteksi Gangguan Jaringan Listrik." },
  { "en": "Apa Fungsi Rele Proteksi (Protection Relay)?", "id": "Memberi Perintah Trip Ke Pemutus Sirkuit." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Pemutus Arus Otomatis." },
  { "en": "Apa Itu Pemisah (Disconnecting Switch)?", "id": "Saklar Pemisah Tanpa Beban." },
  { "en": "Apa Beda Pemutus Sirkuit Dan Pemisah?", "id": "Pemutus Memutus Arus, Pemisah Tidak." },
  { "en": "Apa Itu Titik Simpul (Node) Dalam Rangkaian?", "id": "Titik Pertemuan Antar Komponen." },
  { "en": "Apa Itu Cabang (Branch) Dalam Rangkaian?", "id": "Jalur Antara Dua Titik Simpul." },
  { "en": "Apa Itu Loop (Loop) Dalam Rangkaian?", "id": "Jalur Tertutup Dalam Rangkaian." },
  { "en": "Apa Satuan Untuk Energi?", "id": "Joule (J)." },
  { "en": "Apa Hubungan Antara Joule Dan Watt?", "id": "Satu Joule Adalah Satu Watt Detik." },
  { "en": "Apa Itu Efisiensi (Efficiency) Sistem?", "id": "Rasio Daya Output Terhadap Daya Input." },
  { "en": "Bagaimana Rumus Efisiensi (Efficiency)?", "id": "Efisiensi = (Daya Output / Daya Input)." },
  { "en": "Mengapa Efisiensi (Efficiency) Tidak Pernah 100 Persen?", "id": "Selalu Ada Energi Terbuang (Panas)." },
  { "en": "Apa Itu Rugi-Rugi Tembaga (Copper Losses)?", "id": "Rugi Akibat Hambatan Kawat Tembaga (I²R)." },
  { "en": "Apa Itu Rugi-Rugi Inti (Core Losses)?", "id": "Rugi Histeresis Dan Arus Eddy." },
  { "en": "Apa Itu Arus Eddy (Eddy Current)?", "id": "Arus Liar Berputar Di Inti Besi." },
  { "en": "Bagaimana Mengurangi Arus Eddy (Eddy Current)?", "id": "Menggunakan Inti Besi Berlaminasi (Berlapis)." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Keterlambatan Magnetisasi Terhadap Medan Magnet." },
  { "en": "Apa Itu Kurva B-H (B-H Curve)?", "id": "Kurva Hubungan Fluks Magnet, Gaya Magnet." },
  { "en": "Apa Itu Saturasi (Saturation) Magnetik?", "id": "Kondisi Inti Tidak Bisa Dimagnetisasi Lagi." },
  { "en": "Apa Itu Remanensi (Remanence) Magnetik?", "id": "Sisa Magnetisme Setelah Medan Hilang." },
  { "en": "Apa Itu Koersivitas (Coercivity) Magnetik?", "id": "Kekuatan Medan Balik Hilangkan Remanensi." },
  { "en": "Apa Itu Bahan Magnet Keras (Hard Magnet)?", "id": "Bahan Sulit Dimagnetkan, Sulit Dihilangkan." },
  { "en": "Apa Itu Bahan Magnet Lunak (Soft Magnet)?", "id": "Bahan Mudah Dimagnetkan, Mudah Dihilangkan." },
  { "en": "Dimana Bahan Magnet Lunak (Soft Magnet) Digunakan?", "id": "Inti Transformator Dan Induktor." },
  { "en": "Apa Kepanjangan PLTA (Pembangkit Listrik Tenaga Air)?", "id": "Pembangkit Listrik Tenaga Air." },
  { "en": "Apa Energi Primer PLTA (Pembangkit Listrik Tenaga Air)?", "id": "Energi Potensial (Gerak) Air." },
  { "en": "Apa Kepanjangan PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Pembangkit Listrik Tenaga Uap." },
  { "en": "Apa Energi Primer PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Batu Bara Atau Bahan Bakar Fosil." },
  { "en": "Apa Kepanjangan PLTG (Pembangkit Listrik Tenaga Gas)?", "id": "Pembangkit Listrik Tenaga Gas." },
  { "en": "Apa Kepanjangan PLTP (Pembangkit Listrik Tenaga Panas Bumi)?", "id": "Pembangkit Listrik Tenaga Panas Bumi." },
  { "en": "Apa Kepanjangan PLTS (Pembangkit Listrik Tenaga Surya)?", "id": "Pembangkit Listrik Tenaga Surya." },
  { "en": "Apa Komponen Utama PLTS (Pembangkit Listrik Tenaga Surya)?", "id": "Panel Surya (Sel Fotovoltaik)." },
  { "en": "Apa Kepanjangan PLTB (Pembangkit Listrik Tenaga Bayu)?", "id": "Pembangkit Listrik Tenaga Bayu (Angin)." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Energi Dari Sumber Alam Berkelanjutan." },
  { "en": "Apa Kode Angka Untuk Warna Hitam Resistor?", "id": "Angka 0." },
  { "en": "Apa Kode Angka Untuk Warna Cokelat Resistor?", "id": "Angka 1." },
  { "en": "Apa Kode Angka Untuk Warna Merah Resistor?", "id": "Angka 2." },
  { "en": "Apa Kode Angka Untuk Warna Oranye Resistor?", "id": "Angka 3." },
  { "en": "Apa Kode Angka Untuk Warna Kuning Resistor?", "id": "Angka 4." },
  { "en": "Apa Kode Angka Untuk Warna Hijau Resistor?", "id": "Angka 5." },
  { "en": "Apa Kode Angka Untuk Warna Biru Resistor?", "id": "Angka 6." },
  { "en": "Apa Kode Angka Untuk Warna Ungu Resistor?", "id": "Angka 7." },
  { "en": "Apa Kode Angka Untuk Warna Abu-Abu Resistor?", "id": "Angka 8." },
  { "en": "Apa Kode Angka Untuk Warna Putih Resistor?", "id": "Angka 9." },
  { "en": "Apa Arti Kode 103 Pada Kapasitor Keramik?", "id": "10.000 pF Atau 10 nF." },
  { "en": "Apa Arti Kode 104 Pada Kapasitor Keramik?", "id": "100.000 pF Atau 100 nF." },
  { "en": "Apa Arti Huruf J Pada Kode Kapasitor?", "id": "Toleransi Kapasitansi 5 Persen." },
  { "en": "Apa Arti Huruf K Pada Kode Kapasitor?", "id": "Toleransi Kapasitansi 10 Persen." },
  { "en": "Apa Arti Huruf M Pada Kode Kapasitor?", "id": "Toleransi Kapasitansi 20 Persen." },
  { "en": "Apa Itu Pita Valensi (Valence Band)?", "id": "Pita Energi Tempat Elektron Valensi." },
  { "en": "Apa Itu Pita Konduksi (Conduction Band)?", "id": "Pita Energi Tempat Elektron Bebas." },
  { "en": "Apa Itu Celah Energi (Energy Gap)?", "id": "Jarak Energi Pita Valensi, Konduksi." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Pada Konduktor?", "id": "Pita Valensi, Konduksi Tumpang Tindih." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Pada Isolator?", "id": "Celah Energi Sangat Lebar." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Pada Semikonduktor?", "id": "Celah Energi Cukup Sempit." },
  { "en": "Apa Yang Terjadi Jika Elektron Melompat Ke Pita Konduksi?", "id": "Elektron Menjadi Konduktif (Arus Listrik)." },
  { "en": "Apa Itu Pasangan Elektron-Lubang (Electron-Hole Pair)?", "id": "Elektron Bebas, Lubang Yang Ditinggalkan." },
  { "en": "Apa Itu Rekombinasi (Recombination) Semikonduktor?", "id": "Elektron Kembali Mengisi Lubang (Hole)." },
  { "en": "Apa Kepanjangan BCD (Binary Coded Decimal)?", "id": "Desimal Yang Dikodekan Biner." },
  { "en": "Apa Itu Kode Gray (Gray Code)?", "id": "Kode Biner Urutan Beda 1 Bit." },
  { "en": "Apa Itu Paritas (Parity) Bit?", "id": "Bit Tambahan Untuk Deteksi Kesalahan." },
  { "en": "Apa Itu Paritas Ganjil (Odd Parity)?", "id": "Total Bit 1 Adalah Ganjil." },
  { "en": "Apa Itu Paritas Genap (Even Parity)?", "id": "Total Bit 1 Adalah Genap." },
  { "en": "Apa Itu Rangkaian Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Logika Menggunakan PMOS Dan NMOS." },
  { "en": "Apa Kelemahan Logika TTL (Transistor-Transistor Logic)?", "id": "Konsumsi Daya Relatif Tinggi." },
  { "en": "Apa Itu Fan-Out (Fan-Out) Gerbang Logika?", "id": "Jumlah Maksimal Input Yang Digerakkan." },
  { "en": "Apa Itu Fan-In (Fan-In) Gerbang Logika?", "id": "Jumlah Maksimal Input Gerbang Logika." },
  { "en": "Apa Itu Noise Margin (Batas Derau)?", "id": "Toleransi Tegangan Kebal Terhadap Noise." },
  { "en": "Apa Itu Latch (Selak)?", "id": "Elemen Memori Dasar (Level-Triggered)." },
  { "en": "Apa Beda Latch (Selak) Dan Flip-Flop?", "id": "Latch Level-Triggered, Flip-Flop Edge-Triggered." },
  { "en": "Apa Itu SR Latch (SR Latch)?", "id": "Latch Set-Reset Menggunakan NAND/NOR." },
  { "en": "Apa Itu D Flip-Flop (Data Flip-Flop)?", "id": "Menyimpan Data (D) Saat Clock Aktif." },
  { "en": "Apa Itu T Flip-Flop (Toggle Flip-Flop)?", "id": "Mengubah (Toggle) Output Saat Clock Aktif." },
  { "en": "Apa Itu JK Flip-Flop (JK Flip-Flop)?", "id": "Gabungan Fungsi SR, D, T Flip-Flop." },
  { "en": "Apa Itu Kondisi Pacu (Race Condition)?", "id": "Masalah Waktu (Timing) Rangkaian Sekuensial." },
  { "en": "Apa Itu Metastabilitas (Metastability)?", "id": "Kondisi Tidak Stabil Rangkaian Digital." },
  { "en": "Apa Itu Aktuator Pneumatik (Pneumatic Actuator)?", "id": "Aktuator Digerakkan Udara Bertekanan." },
  { "en": "Apa Itu Aktuator Hidrolik (Hydraulic Actuator)?", "id": "Aktuator Digerakkan Cairan (Oli) Bertekanan." },
  { "en": "Apa Itu Katup Solenoid (Solenoid Valve)?", "id": "Katup Elektromekanis Pengontrol Aliran Fluida." },
  { "en": "Apa Itu Sensor Proximity (Proximity Sensor) Induktif?", "id": "Mendeteksi Objek Logam Tanpa Sentuhan." },
  { "en": "Apa Itu Sensor Proximity (Proximity Sensor) Kapasitif?", "id": "Mendeteksi Objek Logam, Non-Logam." },
  { "en": "Apa Itu Sensor Fotoelektrik (Photoelectric Sensor)?", "id": "Mendeteksi Objek Menggunakan Cahaya." },
  { "en": "Apa Itu Sensor Encoder (Encoder Sensor)?", "id": "Sensor Pengukur Posisi, Kecepatan Putar." },
  { "en": "Apa Itu Resolver (Resolver)?", "id": "Sensor Sudut Putar Analog Elektromekanis." },
  { "en": "Apa Itu Tachometer (Tachometer)?", "id": "Alat Pengukur Kecepatan Putaran (RPM)." },
  { "en": "Apa Kepanjangan RTD (Resistance Temperature Detector)?", "id": "Detektor Suhu Resistansi." },
  { "en": "Apa Bahan Umum RTD (Resistance Temperature Detector)?", "id": "Bahan Platina (Contoh: Pt100)." },
  { "en": "Apa Itu Jembatan Arus Bolak-Balik (AC Bridge)?", "id": "Jembatan Wheatstone Untuk Mengukur L, C." },
  { "en": "Apa Fungsi Thermostat (Thermostat)?", "id": "Saklar Otomatis Berdasarkan Suhu." },
  { "en": "Apa Fungsi Pressure Switch (Saklar Tekanan)?", "id": "Saklar Otomatis Berdasarkan Tekanan." },
  { "en": "Apa Fungsi Flow Meter (Pengukur Aliran)?", "id": "Alat Mengukur Laju Aliran Fluida." },
  { "en": "Apa Fungsi Level Sensor (Sensor Ketinggian)?", "id": "Alat Mengukur Ketinggian Cairan." },
  { "en": "Apa Itu Sistem Kontrol Umpan Maju (Feedforward)?", "id": "Kontrol Antisipasi Gangguan (Tanpa Umpan Balik)." },
  { "en": "Apa Itu Waktu Tunda (Time Delay) Sistem?", "id": "Waktu Respon Sistem Yang Tertunda." },
  { "en": "Apa Itu Respon Transien (Transient Response)?", "id": "Respon Sistem Sebelum Mencapai Kestabilan." },
  { "en": "Apa Itu Respon Keadaan Tunak (Steady State Response)?", "id": "Respon Sistem Setelah Mencapai Kestabilan." },
  { "en": "Apa Itu Overshoot (Limpasan) Kontrol?", "id": "Output Melebihi Setpoint Sementara." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Sinyal Naik (10% Ke 90%)." },
  { "en": "Apa Itu Waktu Turun (Fall Time)?", "id": "Waktu Sinyal Turun (90% Ke 10%)." },
  { "en": "Apa Itu Waktu Penapakan (Settling Time)?", "id": "Waktu Output Masuk Batas Toleransi Setpoint." },
  { "en": "Apa Itu Kontrol On-Off (Bang-Bang Control)?", "id": "Kontroler Hanya Punya Dua Keadaan (On/Off)." },
  { "en": "Apa Itu Resonansi Paralel (Parallel Resonance)?", "id": "Kondisi Impedansi Total Maksimum." },
  { "en": "Apa Itu Resonansi Seri (Series Resonance)?", "id": "Kondisi Impedansi Total Minimum." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor) Rangkaian?", "id": "Ukuran Selektivitas Rangkaian Resonansi." },
  { "en": "Apa Itu Semikonduktor Intrinsik (Intrinsic)?", "id": "Bahan Semikonduktor Murni." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik (Extrinsic)?", "id": "Bahan Semikonduktor Diberi Doping." },
  { "en": "Apa Bahan Doping Untuk Tipe-P (P-Type)?", "id": "Bahan Trivalen (Contoh: Boron)." },
  { "en": "Apa Bahan Doping Untuk Tipe-N (N-Type)?", "id": "Bahan Pentavalen (Contoh: Fosfor)." },
  { "en": "Apa Itu Arus Drift (Drift Current)?", "id": "Aliran Muatan Akibat Medan Listrik." },
  { "en": "Apa Itu Arus Difusi (Diffusion Current)?", "id": "Aliran Muatan Akibat Beda Konsentrasi." },
  { "en": "Apa Itu Dioda Tunnel (Tunnel Diode)?", "id": "Dioda Dengan Efek Terowongan Kuantum." },
  { "en": "Apa Itu Dioda PIN (PIN Diode)?", "id": "Dioda Dengan Lapisan Intrinsik (I)." },
  { "en": "Apa Fungsi Dioda PIN (PIN Diode)?", "id": "Saklar Frekuensi Tinggi (RF)." },
  { "en": "Apa Kepanjangan LASER (Light Amplification by Stimulated Emission)?", "id": "Penguatan Cahaya Stimulasi Emisi Radiasi." },
  { "en": "Apa Itu Dioda Laser (Laser Diode)?", "id": "Dioda Memancarkan Cahaya Laser Koheren." },
  { "en": "Apa Itu Photocoupler (Photocoupler)?", "id": "Nama Lain Optocoupler." },
  { "en": "Apa Itu Transistor Darlington (Darlington Transistor)?", "id": "Dua BJT (Bipolar Junction Transistor) Terhubung." },
  { "en": "Apa Keuntungan Transistor Darlington (Darlington Transistor)?", "id": "Memiliki Penguatan Arus Sangat Tinggi." },
  { "en": "Apa Itu Thyristor (Thyristor)?", "id": "Keluarga Komponen Semikonduktor Saklar." },
  { "en": "Apa Saja Anggota Keluarga Thyristor (Thyristor)?", "id": "SCR, TRIAC, DIAC." },
  { "en": "Bagaimana Cara Mematikan SCR (Silicon Controlled Rectifier)?", "id": "Arus Turun Dibawah Arus Holding." },
  { "en": "Apa Itu Arus Latching (Latching Current) SCR?", "id": "Arus Minimum Untuk Tetap Menyala." },
  { "en": "Apa Itu Arus Holding (Holding Current) SCR?", "id": "Arus Minimum Jaga SCR Tetap On." },
  { "en": "Apa Kepanjangan GTO (Gate Turn-Off Thyristor)?", "id": "Thyristor Gate Turn-Off." },
  { "en": "Apa Keistimewaan GTO (Gate Turn-Off Thyristor)?", "id": "Dapat Dimatikan Melalui Gerbang (Gate)." },
  { "en": "Apa Kepanjangan UJT (Unijunction Transistor)?", "id": "Transistor Uni-Sambungan." },
  { "en": "Apa Fungsi UJT (Unijunction Transistor)?", "id": "Umumnya Untuk Osilator Relaksasi." },
  { "en": "Apa Itu Rangkaian Osilator (Oscillator Circuit)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Fungsi Osilator (Oscillator)?", "id": "Menghasilkan Frekuensi Clock Atau Carrier." },
  { "en": "Apa Itu Osilator Hartley (Hartley Oscillator)?", "id": "Osilator LC (Induktor Kapasitor) Umpan Balik Induktif." },
  { "en": "Apa Itu Osilator Colpitts (Colpitts Oscillator)?", "id": "Osilator LC (Induktor Kapasitor) Umpan Balik Kapasitif." },
  { "en": "Apa Itu Osilator Clapp (Clapp Oscillator)?", "id": "Modifikasi Osilator Colpitts." },
  { "en": "Apa Itu Osilator Geser Fasa (Phase-Shift Oscillator)?", "id": "Osilator RC (Resistor Kapasitor) Menggeser Fasa." },
  { "en": "Apa Itu Osilator Jembatan Wien (Wien Bridge Oscillator)?", "id": "Osilator RC (Resistor Kapasitor) Hasilkan Sinus Murni." },
  { "en": "Apa Itu Multivibrator (Multivibrator)?", "id": "Rangkaian Osilator Penghasil Sinyal Non-Sinusoidal." },
  { "en": "Apa Itu Multivibrator Astabil (Astable Multivibrator)?", "id": "Osilator Tanpa Keadaan Stabil (Clock)." },
  { "en": "Apa Itu Multivibrator Monostabil (Monostable Multivibrator)?", "id": "Memiliki Satu Keadaan Stabil (Timer)." },
  { "en": "Apa Itu Multivibrator Bistabil (Bistable Multivibrator)?", "id": "Memiliki Dua Keadaan Stabil (Flip-Flop)." },
  { "en": "Apa Contoh IC (Integrated Circuit) Timer Populer?", "id": "IC 555." },
  { "en": "Apa Tiga Mode Operasi IC 555?", "id": "Astabil, Monostabil, Dan Bistabil." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Loop Terkunci Fasa." },
  { "en": "Apa Fungsi PLL (Phase-Locked Loop)?", "id": "Menstabilkan, Mensintesis Frekuensi." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator Terkendali Tegangan." },
  { "en": "Apa Fungsi VCO (Voltage-Controlled Oscillator)?", "id": "Frekuensi Output Diatur Tegangan Input." },
  { "en": "Apa Itu CMRR (Common-Mode Rejection Ratio) Op-Amp?", "id": "Rasio Penolakan Mode Bersama." },
  { "en": "Apakah Nilai CMRR (Common-Mode Rejection Ratio) Tinggi Baik?", "id": "Ya, Sangat Baik Menolak Noise." },
  { "en": "Apa Itu Slew Rate (Laju Perubahan) Op-Amp?", "id": "Kecepatan Perubahan Tegangan Output Maksimal." },
  { "en": "Apa Satuan Slew Rate (Laju Perubahan)?", "id": "Volt Per Microsecond (V/µs)." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Itu Rangkaian Integrator (Integrator Circuit)?", "id": "Rangkaian Op-Amp Menghasilkan Integral Input." },
  { "en": "Apa Itu Rangkaian Diferensiator (Differentiator Circuit)?", "id": "Rangkaian Op-Amp Menghasilkan Turunan Input." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Rangkaian Pembanding Dua Tegangan Input." },
  { "en": "Apa Output Komparator (Comparator)?", "id": "Logika Tinggi (High) Atau Rendah (Low)." },
  { "en": "Apa Itu Schmitt Trigger (Schmitt Trigger)?", "id": "Komparator Dengan Histeresis." },
  { "en": "Apa Fungsi Histeresis (Hysteresis) Schmitt Trigger?", "id": "Mencegah Output Goyang (Chattering)." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Keunggulan Filter Aktif (Active Filter)?", "id": "Dapat Memberikan Penguatan (Gain)." },
  { "en": "Apa Itu Filter Pasif (Passive Filter)?", "id": "Filter Hanya Menggunakan R, L, C." },
  { "en": "Apa Itu Transformasi Laplace (Laplace Transform)?", "id": "Metode Matematika Analisis Rangkaian." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function)?", "id": "Rasio Output Terhadap Input (Domain-s)." },
  { "en": "Apa Itu Poles (Kutub) Fungsi Transfer?", "id": "Akar Bagian Penyebut (Denominator)." },
  { "en": "Apa Itu Zeros (Nol) Fungsi Transfer?", "id": "Akar Bagian Pembilang (Numerator)." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Respon Sistem Terhadap Frekuensi Input." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Grafik Respon Frekuensi (Magnitude, Fasa)." },
  { "en": "Apa Itu Cutoff Frequency (Frekuensi Batas)?", "id": "Frekuensi Saat Daya Turun Setengah." },
  { "en": "Berapa Penurunan Decibel Pada Cutoff Frequency?", "id": "Penurunan Sebesar -3 dB." },
  { "en": "Apa Itu Dekade (Decade) Dalam Frekuensi?", "id": "Perubahan Frekuensi Sepuluh Kali Lipat." },
  { "en": "Apa Itu Oktaf (Octave) Dalam Frekuensi?", "id": "Perubahan Frekuensi Dua Kali Lipat." },
  { "en": "Apa Itu Rangkaian Resonator (Resonator Circuit)?", "id": "Rangkaian Penyimpan Energi Frekuensi Tertentu." },
  { "en": "Apa Itu Antena Dipol (Dipole Antenna)?", "id": "Antena Sederhana (Dua Elemen)." },
  { "en": "Apa Itu Antena Monopol (Monopole Antenna)?", "id": "Antena Setengah Dipol (Satu Elemen)." },
  { "en": "Apa Itu Ground Plane (Bidang Tanah)?", "id": "Permukaan Konduktif Dibawah Antena Monopol." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern) Antena?", "id": "Grafik Arah Pancaran Energi Antena." },
  { "en": "Apa Itu Antena Isotropik (Isotropic Antenna)?", "id": "Antena Memancar Sama Segala Arah (Teoretis)." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan (Directivity) Antena." },
  { "en": "Apa Satuan Penguatan (Gain) Antena?", "id": "Decibel-Isotropic (dBi)." },
  { "en": "Apa Itu Antena Yagi-Uda (Yagi-Uda Antenna)?", "id": "Antena Direktif (Pengarah) TV, Radio." },
  { "en": "Apa Itu Elemen Driven (Driven Element) Antena Yagi?", "id": "Elemen Yang Terhubung Kabel." },
  { "en": "Apa Itu Elemen Reflektor (Reflector Element) Antena Yagi?", "id": "Elemen Pemantul (Lebih Panjang)." },
  { "en": "Apa Itu Elemen Direktur (Director Element) Antena Yagi?", "id": "Elemen Pengarah (Lebih Pendek)." },
  { "en": "Apa Itu Polarisasi (Polarization) Gelombang?", "id": "Orientasi Arah Medan Listrik Gelombang." },
  { "en": "Apa Itu Polarisasi Vertikal (Vertical Polarization)?", "id": "Medan Listrik Berorientasi Vertikal." },
  { "en": "Apa Itu Polarisasi Horizontal (Horizontal Polarization)?", "id": "Medan Listrik Berorientasi Horizontal." },
  { "en": "Apa Itu Waveguide (Pemandu Gelombang)?", "id": "Struktur Pemandu Gelombang Frekuensi Tinggi." },
  { "en": "Apa Itu Serat Optik (Fiber Optic)?", "id": "Media Transmisi Data Menggunakan Cahaya." },
  { "en": "Apa Keunggulan Serat Optik (Fiber Optic)?", "id": "Bandwidth Besar, Kebal Interferensi EMI." },
  { "en": "Apa Itu Refleksi Total Internal (Total Internal Reflection)?", "id": "Prinsip Dasar Transmisi Serat Optik." },
  { "en": "Apa Kepanjangan OTDR (Optical Time-Domain Reflectometer)?", "id": "Reflektometer Domain Waktu Optik." },
  { "en": "Apa Fungsi OTDR (Optical Time-Domain Reflectometer)?", "id": "Menganalisis, Mencari Gangguan Kabel Optik." },
  { "en": "Apa Itu Fusi Splicing (Fusion Splicing)?", "id": "Metode Penyambungan Inti Serat Optik." },
  { "en": "Apa Itu Skin Effect (Efek Kulit)?", "id": "Arus AC Cenderung Mengalir Permukaan Konduktor." },
  { "en": "Kapan Skin Effect (Efek Kulit) Menjadi Signifikan?", "id": "Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Kawat Litz (Litz Wire)?", "id": "Kabel Serabut Terisolasi Kurangi Efek Kulit." },
  { "en": "Apa Itu Pasangan Terpilin (Twisted Pair)?", "id": "Dua Kawat Dipilin Kurangi Interferensi." },
  { "en": "Dimana Kabel Pasangan Terpilin (Twisted Pair) Digunakan?", "id": "Kabel Jaringan (Ethernet), Telepon." },
  { "en": "Apa Kepanjangan UTP (Unshielded Twisted Pair)?", "id": "Pasangan Terpilin Tanpa Pelindung." },
  { "en": "Apa Kepanjangan STP (Shielded Twisted Pair)?", "id": "Pasangan Terpilin Dengan Pelindung." },
  { "en": "Apa Itu Crosstalk (Crosstalk)?", "id": "Gangguan Sinyal Antar Kabel Berdekatan." },
  { "en": "Apa Itu Kabel Koaksial (Coaxial Cable)?", "id": "Kabel Dengan Konduktor Pusat, Pelindung." },
  { "en": "Apa Fungsi Pelindung (Shield) Kabel Koaksial?", "id": "Melindungi Sinyal Dari Interferensi (EMI)." },
  { "en": "Apa Itu Sistem Kontrol Terdistribusi (DCS)?", "id": "Distributed Control System (DCS)." },
  { "en": "Apa Kepanjangan DCS (Distributed Control System)?", "id": "Sistem Kontrol Terdistribusi." },
  { "en": "Apa Beda PLC (Programmable Logic Controller) Dan DCS (Distributed Control System)?", "id": "PLC Fokus Kecepatan, DCS Skala Besar." },
  { "en": "Apa Itu Hubungan Singkat Tiga Fasa?", "id": "Arus Hubungan Singkat Antar Tiga Fasa." },
  { "en": "Apa Itu Hubungan Singkat Fasa Ke Tanah?", "id": "Arus Hubungan Singkat Fasa Ke Tanah." },
  { "en": "Apa Itu Gangguan Simetris (Symmetrical Fault)?", "id": "Gangguan Yang Seimbang (Contoh: Tiga Fasa)." },
  { "en": "Apa Itu Gangguan Asimetris (Asymmetrical Fault)?", "id": "Gangguan Tidak Seimbang (Satu Fasa)." },
  { "en": "Apa Itu Rele Arus Lebih (Overcurrent Relay)?", "id": "Rele Bekerja Berdasarkan Arus Berlebih." },
  { "en": "Apa Kepanjangan OCR (Overcurrent Relay)?", "id": "Rele Arus Lebih." },
  { "en": "Apa Itu Rele Diferensial (Differential Relay)?", "id": "Rele Bekerja Berdasarkan Selisih Arus." },
  { "en": "Dimana Rele Diferensial (Differential Relay) Digunakan?", "id": "Proteksi Transformator, Generator, Busbar." },
  { "en": "Apa Itu Rele Jarak (Distance Relay)?", "id": "Rele Mengukur Impedansi (Jarak) Gangguan." },
  { "en": "Apa Nama Lain Rele Jarak (Distance Relay)?", "id": "Rele Impedansi (Impedance Relay)." },
  { "en": "Apa Itu Rele Frekuensi (Frequency Relay)?", "id": "Rele Bekerja Berdasarkan Perubahan Frekuensi." },
  { "en": "Apa Itu Rele Arah (Directional Relay)?", "id": "Rele Bekerja Berdasarkan Arah Aliran Daya." },
  { "en": "Apa Itu Komutasi (Commutation) Thyristor?", "id": "Proses Mematikan Thyristor (SCR)." },
  { "en": "Apa Itu Komutasi Alami (Natural Commutation)?", "id": "Komutasi Akibat Sifat Alami Rangkaian AC." },
  { "en": "Apa Itu Komutasi Paksa (Forced Commutation)?", "id": "Komutasi Menggunakan Rangkaian Eksternal (DC)." },
  { "en": "Apa Itu Cycloconverter (Cycloconverter)?", "id": "Pengubah Frekuensi AC Ke AC Langsung." },
  { "en": "Apa Itu Cycloinverter (Cycloinverter)?", "id": "Istilah Lain Untuk Cycloconverter." },
  { "en": "Apa Itu Konverter Matriks (Matrix Converter)?", "id": "Pengubah AC Ke AC Tanpa Penyimpanan DC." },
  { "en": "Apa Itu Motor Sinkron (Synchronous Motor)?", "id": "Motor Kecepatan Rotor Sama Medan Putar." },
  { "en": "Kapan Motor Sinkron (Synchronous Motor) Digunakan?", "id": "Aplikasi Kecepatan Konstan Presisi." },
  { "en": "Apa Itu Eksitasi (Excitation) Motor Sinkron?", "id": "Pemberian Arus DC Ke Rotor." },
  { "en": "Apa Fungsi Eksitasi (Excitation) Motor Sinkron?", "id": "Menghasilkan Medan Magnet Rotor." },
  { "en": "Apa Itu Generator Sinkron (Synchronous Generator)?", "id": "Pembangkit Listrik Utama (Alternator)." },
  { "en": "Apa Nama Lain Generator Sinkron?", "id": "Alternator (Penghasil Listrik AC)." },
  { "en": "Apa Itu Motor Asinkron (Asynchronous Motor)?", "id": "Motor Kecepatan Rotor Berbeda Medan Putar." },
  { "en": "Apa Nama Lain Motor Asinkron?", "id": "Motor Induksi (Induction Motor)." },
  { "en": "Apa Itu Belitan Sangkar Tupai (Squirrel Cage)?", "id": "Jenis Rotor Motor Induksi Paling Umum." },
  { "en": "Apa Itu Motor Universal (Universal Motor)?", "id": "Motor Dapat Beroperasi Listrik AC/DC." },
  { "en": "Dimana Motor Universal (Universal Motor) Ditemukan?", "id": "Alat Rumah Tangga (Bor, Blender)." },
  { "en": "Apa Itu Motor DC Shunt (Shunt DC Motor)?", "id": "Belitan Medan Paralel Dengan Jangkar." },
  { "en": "Apa Itu Motor DC Seri (Series DC Motor)?", "id": "Belitan Medan Seri Dengan Jangkar." },
  { "en": "Apa Karakteristik Motor DC Seri?", "id": "Torsi Awal Sangat Tinggi." },
  { "en": "Apa Itu Motor DC Kompon (Compound DC Motor)?", "id": "Memiliki Belitan Medan Seri, Shunt." },
  { "en": "Apa Itu Jangkar (Armature)?", "id": "Bagian Kumparan Motor Hasilkan Torsi." },
  { "en": "Apa Itu Belitan Medan (Field Winding)?", "id": "Kumparan Penghasil Medan Magnet Stator." },
  { "en": "Apa Itu Kesalahan Paralaks (Parallax Error)?", "id": "Kesalahan Pembacaan Akibat Sudut Pandang." },
  { "en": "Apa Itu Akurasi (Accuracy) Alat Ukur?", "id": "Kedekatan Hasil Ukur Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision) Alat Ukur?", "id": "Kemampuan Alat Ukur Beri Hasil Sama." },
  { "en": "Apa Itu Resolusi (Resolution) Alat Ukur?", "id": "Perubahan Terkecil Yang Dapat Diukur." },
  { "en": "Apa Itu Sensitivitas (Sensitivity) Alat Ukur?", "id": "Rasio Perubahan Output Terhadap Input." },
  { "en": "Apa Itu Shunt (Shunt Resistor) Amperemeter?", "id": "Resistor Paralel Perluas Jangkauan Ukur Arus." },
  { "en": "Apa Itu Multiplier (Multiplier Resistor) Voltmeter?", "id": "Resistor Seri Perluas Jangkauan Ukur Tegangan." },
  { "en": "Apa Fungsi Termometer Inframerah (Infrared Thermometer)?", "id": "Mengukur Suhu Permukaan Tanpa Sentuhan." },
  { "en": "Apa Fungsi Tachometer Optik (Optical Tachometer)?", "id": "Mengukur RPM Menggunakan Pantulan Cahaya." },
  { "en": "Apa Fungsi Sound Level Meter (Pengukur Suara)?", "id": "Mengukur Intensitas Kebisingan Suara (dB)." },
  { "en": "Apa Fungsi Lux Meter (Pengukur Cahaya)?", "id": "Mengukur Intensitas Pencahayaan (Lux)." },
  { "en": "Apa Itu Sinyal Deterministik (Deterministic Signal)?", "id": "Sinyal Dapat Diprediksi Secara Matematis." },
  { "en": "Apa Itu Sinyal Acak (Random Signal)?", "id": "Sinyal Tidak Dapat Diprediksi (Noise)." },
  { "en": "Apa Itu Sinyal Periodik (Periodic Signal)?", "id": "Sinyal Berulang Dengan Pola Tetap." },
  { "en": "Apa Itu Sinyal Aperiodik (Aperiodic Signal)?", "id": "Sinyal Tidak Memiliki Pola Berulang." },
  { "en": "Apa Itu Sinyal Waktu Kontinu (Continuous-Time Signal)?", "id": "Sinyal Terdefinisi Setiap Saat." },
  { "en": "Apa Itu Sinyal Waktu Diskrit (Discrete-Time Signal)?", "id": "Sinyal Terdefinisi Hanya Waktu Tertentu." },
  { "en": "Apa Itu Proses Pencuplikan (Sampling)?", "id": "Mengambil Sampel Sinyal Kontinu." },
  { "en": "Apa Itu Teorema Sampling Nyquist (Nyquist Theorem)?", "id": "Frekuensi Sampling Minimal Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Akibat Frekuensi Sampling Terlalu Rendah." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Proses Pembulatan Nilai Sampel." },
  { "en": "Apa Itu Kesalahan Kuantisasi (Quantization Error)?", "id": "Selisih Nilai Asli, Kuantisasi." },
  { "en": "Apa Itu Sistem Linear (Linear System)?", "id": "Sistem Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Sistem Invarian Waktu (Time-Invariant System)?", "id": "Respon Sistem Tidak Berubah Waktu." },
  { "en": "Apa Kepanjangan LTI (Linear Time-Invariant) System?", "id": "Sistem Linear Invarian Waktu." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematika Respon Sistem LTI." },
  { "en": "Apa Itu Respon Impuls (Impulse Response)?", "id": "Output Sistem Diberi Input Impuls." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Analisis Sinyal, Sistem Waktu Diskrit." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Kontrol?", "id": "Kemampuan Sistem Kembali Seimbang." },
  { "en": "Apa Itu Sistem Stabil (Stable System)?", "id": "Output Terkendali (Bounded) Untuk Input Terkendali." },
  { "en": "Apa Itu Sistem Tidak Stabil (Unstable System)?", "id": "Output Tumbuh Tanpa Batas." },
  { "en": "Apa Itu Diagram Blok (Block Diagram)?", "id": "Representasi Grafis Sistem Kontrol." },
  { "en": "Apa Itu Diagram Alir Sinyal (Signal Flow Graph)?", "id": "Metode Analisis Sistem Kontrol." },
  { "en": "Apa Itu Root Locus (Tempat Kedudukan Akar)?", "id": "Metode Grafis Analisis Kestabilan Sistem." },
  { "en": "Dimana Sistem Stabil Pada Root Locus?", "id": "Akar (Poles) Di Sisi Kiri Bidang-s." },
  { "en": "Apa Itu Respon Langkah (Step Response)?", "id": "Output Sistem Diberi Input Unit Step." },
  { "en": "Apa Kepanjangan ASK (Amplitude Shift Keying)?", "id": "Modulasi Pergeseran Amplitudo." },
  { "en": "Apa Kepanjangan FSK (Frequency Shift Keying)?", "id": "Modulasi Pergeseran Frekuensi." },
  { "en": "Apa Kepanjangan PSK (Phase Shift Keying)?", "id": "Modulasi Pergeseran Fasa." },
  { "en": "Apa Itu BPSK (Binary Phase Shift Keying)?", "id": "PSK Menggunakan Dua Fasa (0, 180 Derajat)." },
  { "en": "Apa Itu QPSK (Quadrature Phase Shift Keying)?", "id": "PSK Menggunakan Empat Fasa." },
  { "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?", "id": "Modulasi Menggabungkan Amplitudo, Fasa." },
  { "en": "Apa Itu Baud Rate (Laju Baud)?", "id": "Jumlah Perubahan Simbol Per Detik." },
  { "en": "Apa Beda Bit Rate (Laju Bit) Dan Baud Rate (Laju Baud)?", "id": "Bit Rate Bisa Lebih Tinggi Baud Rate." },
  { "en": "Apa Itu Multiplexing (Multipleksasi)?", "id": "Mengirim Banyak Sinyal Satu Saluran." },
  { "en": "Apa Kepanjangan TDM (Time Division Multiplexing)?", "id": "Multipleksasi Pembagian Waktu." },
  { "en": "Apa Kepanjangan FDM (Frequency Division Multiplexing)?", "id": "Multipleksasi Pembagian Frekuensi." },
  { "en": "Apa Kepanjangan WDM (Wavelength Division Multiplexing)?", "id": "Multipleksasi Pembagian Panjang Gelombang (Optik)." },
  { "en": "Apa Itu Kode Saluran (Line Coding)?", "id": "Metode Pengkodean Data Digital Transmisi." },
  { "en": "Apa Itu Kode NRZ (Non-Return-to-Zero)?", "id": "Kode Saluran Sederhana (Level 1, 0)." },
  { "en": "Apa Itu Kode Manchester (Manchester Code)?", "id": "Kode Saluran (Transisi Di Tengah Bit)." },
  { "en": "Apa Itu Magnetostriksi (Magnetostriction)?", "id": "Perubahan Dimensi Bahan Akibat Magnetisasi." },
  { "en": "Apa Itu Efek Barkhausen (Barkhausen Effect)?", "id": "Noise Akibat Pergerakan Domain Magnetik." },
  { "en": "Apa Itu Domain Magnetik (Magnetic Domain)?", "id": "Area Kecil Dengan Arah Magnet Sama." },
  { "en": "Apa Itu Titik Curie (Curie Temperature)?", "id": "Suhu Hilangnya Sifat Feromagnetik." },
  { "en": "Apa Itu Bahan Diamagnetik (Diamagnetic)?", "id": "Bahan Yang Sedikit Menolak Medan Magnet." },
  { "en": "Apa Itu Bahan Paramagnetik (Paramagnetic)?", "id": "Bahan Yang Sedikit Tertarik Medan Magnet." },
  { "en": "Apa Kepanjangan NEMA (National Electrical Manufacturers Association)?", "id": "Asosiasi Produsen Listrik Nasional (AS)." },
  { "en": "Apa Kepanjangan IEC (International Electrotechnical Commission)?", "id": "Komisi Elektroteknik Internasional." },
  { "en": "Apa Kepanjangan IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Institut Insinyur Listrik, Elektronika." },
  { "en": "Apa Itu Standar IEEE 802.3 (IEEE 802.3)?", "id": "Standar Untuk Jaringan Ethernet." },
  { "en": "Apa Itu Standar IEEE 802.11 (IEEE 802.11)?", "id": "Standar Untuk Jaringan Nirkabel (Wi-Fi)." },
  { "en": "Apa Itu Peringkat IP (IP Rating)?", "id": "Peringkat Proteksi Benda Padat, Cair." },
  { "en": "Apa Arti IP65 (IP65)?", "id": "Proteksi Total Debu, Semprotan Air." },
  { "en": "Apa Itu Arc Flash (Kilatan Busur Api)?", "id": "Ledakan Energi Tinggi Akibat Busur Listrik." },
  { "en": "Apa Itu Transformasi Bintang Ke Segitiga (Y-Δ)?", "id": "Mengubah Rangkaian Bintang (Y) Ke Segitiga (Δ)." },
  { "en": "Apa Itu Transformasi Segitiga Ke Bintang (Δ-Y)?", "id": "Mengubah Rangkaian Segitiga (Δ) Ke Bintang (Y)." },
  { "en": "Apa Fungsi Transformasi Y-Δ (Y-Δ Transformation)?", "id": "Menyederhanakan Analisis Rangkaian Kompleks." },
  { "en": "Apa Itu Port (Port) Rangkaian?", "id": "Sepasang Terminal Untuk Input, Output." },
  { "en": "Apa Itu Rangkaian Dua Port (Two-Port Network)?", "id": "Rangkaian Dengan Dua Pasang Terminal." },
  { "en": "Sebutkan Contoh Parameter Rangkaian Dua Port?", "id": "Parameter Z (Impedansi), Y (Admitansi), H (Hibrid)." },
  { "en": "Apa Itu Parameter Z (Z-Parameters)?", "id": "Parameter Impedansi (Tegangan Fungsi Arus)." },
  { "en": "Apa Itu Parameter Y (Y-Parameters)?", "id": "Parameter Admitansi (Arus Fungsi Tegangan)." },
  { "en": "Apa Itu Parameter H (Hybrid Parameters)?", "id": "Parameter Hibrid (Untuk Analisis Transistor)." },
  { "en": "Apa Itu Parameter ABCD (Transmission Parameters)?", "id": "Parameter Transmisi (Saluran Transmisi)." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Ukuran Kualitas Pasokan Listrik." },
  { "en": "Apa Itu Sag (Sag) Tegangan?", "id": "Penurunan Tegangan Jangka Pendek." },
  { "en": "Apa Itu Swell (Swell) Tegangan?", "id": "Kenaikan Tegangan Jangka Pendek." },
  { "en": "Apa Itu Interupsi (Interruption) Tegangan?", "id": "Hilangnya Pasokan Tegangan Total." },
  { "en": "Apa Itu Transien (Transient) Tegangan?", "id": "Lonjakan Tegangan Sangat Cepat (Spike)." },
  { "en": "Apa Itu Notching (Notching) Tegangan?", "id": "Gangguan Periodik Gelombang Tegangan." },
  { "en": "Apa Itu Flicker (Flicker) Tegangan?", "id": "Fluktuasi Tegangan Sebabkan Lampu Berkedip." },
  { "en": "Apa Penyebab Utama Harmonisa (Harmonics)?", "id": "Beban Non-Linear (SMPS, VFD)." },
  { "en": "Apa Kepanjangan VFD (Variable Frequency Drive)?", "id": "Penggerak Frekuensi Variabel." },
  { "en": "Apa Fungsi VFD (Variable Frequency Drive)?", "id": "Mengatur Kecepatan Putaran Motor Induksi." },
  { "en": "Apa Itu Filter Harmonisa (Harmonic Filter) Pasif?", "id": "Filter RLC Redam Harmonisa." },
  { "en": "Apa Itu Filter Harmonisa (Harmonic Filter) Aktif?", "id": "Filter Elektronik Injeksi Arus Kompensasi." },
  { "en": "Apa Kepanjangan UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Tak Terputus." },
  { "en": "Apa Fungsi UPS (Uninterruptible Power Supply)?", "id": "Menyediakan Daya Listrik Cadangan (Baterai)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply) Standby?", "id": "UPS Beralih Ke Baterai Saat Padam." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply) Online?", "id": "UPS Terus Menerus Konversi Ganda." },
  { "en": "Apa Kepanjangan AVR (Automatic Voltage Regulator)?", "id": "Regulator Tegangan Otomatis." },
  { "en": "Apa Fungsi AVR (Automatic Voltage Regulator)?", "id": "Menjaga Tegangan Output Tetap Stabil." },
  { "en": "Apa Bahan Dasar Utama Semikonduktor?", "id": "Silikon (Si) Murni." },
  { "en": "Apa Itu Wafer (Wafer) Silikon?", "id": "Cakram Tipis Silikon Bahan Dasar IC." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Rangkaian Ke Wafer." },
  { "en": "Apa Itu Photoresist (Photoresist)?", "id": "Bahan Peka Cahaya Untuk Fotolitografi." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Proses Penambahan Atom Impuritas." },
  { "en": "Sebutkan Metode Doping (Doping) Semikonduktor?", "id": "Difusi Atau Implantasi Ion." },
  { "en": "Apa Itu Implantasi Ion (Ion Implantation)?", "id": "Menembakkan Ion Impuritas Ke Wafer." },
  { "en": "Apa Itu Deposisi (Deposition)?", "id": "Proses Pelapisan Bahan Tipis Ke Wafer." },
  { "en": "Apa Kepanjangan CVD (Chemical Vapor Deposition)?", "id": "Deposisi Uap Kimia." },
  { "en": "Apa Itu Etsa (Etching)?", "id": "Proses Pengikisan Bahan Tanpa Pelindung." },
  { "en": "Apa Itu Etsa Kering (Dry Etching)?", "id": "Etsa Menggunakan Plasma Gas." },
  { "en": "Apa Itu Etsa Basah (Wet Etching)?", "id": "Etsa Menggunakan Larutan Kimia Cair." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Ruangan Produksi IC Sangat Bersih." },
  { "en": "Mengapa Produksi IC (Integrated Circuit) Perlu Ruang Bersih?", "id": "Partikel Debu Dapat Merusak Sirkuit." },
  { "en": "Apa Itu Kontrol V/f (V/f Control)?", "id": "Metode Kontrol Kecepatan Motor Induksi." },
  { "en": "Apa Prinsip Kontrol V/f (V/f Control)?", "id": "Menjaga Rasio Tegangan Frekuensi Konstan." },
  { "en": "Apa Itu Kontrol Vektor (Vector Control)?", "id": "Kontrol Motor AC Presisi Tinggi." },
  { "en": "Apa Nama Lain Kontrol Vektor (Vector Control)?", "id": "Field Oriented Control (FOC)." },
  { "en": "Apa Kepanjangan FOC (Field Oriented Control)?", "id": "Kontrol Berorientasi Medan." },
  { "en": "Apa Itu Pengereman Regeneratif (Regenerative Braking)?", "id": "Motor Hasilkan Listrik Saat Pengereman." },
  { "en": "Apa Itu Pengereman Dinamis (Dynamic Braking)?", "id": "Energi Pengereman Dibuang Ke Resistor." },
  { "en": "Apa Itu Inverter Tiga Fasa (Three-Phase Inverter)?", "id": "Pengubah Tegangan DC Ke AC Tiga Fasa." },
  { "en": "Apa Itu Jembatan-H (H-Bridge)?", "id": "Rangkaian Kontrol Arah Putaran Motor DC." },
  { "en": "Berapa Saklar (Switch) Pada Jembatan-H (H-Bridge)?", "id": "Empat Buah Saklar (Transistor)." },
  { "en": "Apa Kepanjangan PWM (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa." },
  { "en": "Apa Fungsi PWM (Pulse Width Modulation) Untuk Motor?", "id": "Mengatur Kecepatan (Tegangan Efektif)." },
  { "en": "Bagaimana PWM (Pulse Width Modulation) Mengatur Kecepatan?", "id": "Mengubah Siklus Kerja (Duty Cycle)." },
  { "en": "Apa Itu Soft Starter (Soft Starter)?", "id": "Alat Start Motor Induksi Perlahan." },
  { "en": "Mengapa Perlu Soft Starter (Soft Starter)?", "id": "Mengurangi Arus Lonjakan Awal." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Dengan Hambatan Listrik Nol." },
  { "en": "Kapan Superkonduktivitas (Superconductivity) Terjadi?", "id": "Pada Suhu Kritis Sangat Dingin." },
  { "en": "Apa Itu Bahan Dielektrik (Dielectric Material)?", "id": "Bahan Isolator Antar Pelat Kapasitor." },
  { "en": "Apa Itu Bahan Piezoelektrik (Piezoelectric Material)?", "id": "Hasilkan Listrik Saat Ditekan Mekanis." },
  { "en": "Apa Itu Efek Termoelektrik (Thermoelectric Effect)?", "id": "Konversi Beda Suhu Langsung Ke Listrik." },
  { "en": "Apa Itu Efek Seebeck (Seebeck Effect)?", "id": "Dasar Kerja Termokopel." },
  { "en": "Apa Itu Efek Peltier (Peltier Effect)?", "id": "Pendinginan Atau Pemanasan Akibat Arus." },
  { "en": "Dimana Efek Peltier (Peltier Effect) Digunakan?", "id": "Pendingin Termoelektrik (TEC)." },
  { "en": "Apa Kepanjangan TEC (Thermoelectric Cooler)?", "id": "Pendingin Termoelektrik." },
  { "en": "Apa Kepanjangan NEC (National Electrical Code)?", "id": "Standar Kelistrikan Nasional (AS)." },
  { "en": "Apa Itu Standar NEMA (NEMA Standard)?", "id": "Standar Spesifikasi Peralatan Listrik (AS)." },
  { "en": "Apa Itu Peringkat NEMA (NEMA Rating) Enclosure?", "id": "Standar Proteksi Kotak Panel Listrik." },
  { "en": "Apa Itu Fibrilasi Ventrikel (Ventricular Fibrillation)?", "id": "Ritme Jantung Kacau Akibat Sengatan Listrik." },
  { "en": "Berapa Arus Minimal Sebabkan Fibrilasi?", "id": "Sekitar 100 Miliampere (mA)." },
  { "en": "Apa Itu Ikatan Ekuipotensial (Equipotential Bonding)?", "id": "Menyambung Semua Konduktor Non-Arus." },
  { "en": "Apa Fungsi Ikatan Ekuipotensial (Equipotential Bonding)?", "id": "Mencegah Beda Potensial Berbahaya." },
  { "en": "Apa Itu Analisis Fourier (Fourier Analysis)?", "id": "Memecah Sinyal Menjadi Komponen Frekuensi." },
  { "en": "Apa Itu Deret Fourier (Fourier Series)?", "id": "Representasi Frekuensi Sinyal Periodik." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Representasi Frekuensi Sinyal Aperiodik." },
  { "en": "Apa Itu Bus I2C (I2C Bus)?", "id": "Bus Komunikasi Serial Dua Kawat." },
  { "en": "Apa Kepanjangan I2C (Inter-Integrated Circuit)?", "id": "Sirkuit Antar Terpadu." },
  { "en": "Apa Dua Kawat Bus I2C (I2C Bus)?", "id": "Serial Data (SDA), Serial Clock (SCL)." },
  { "en": "Apa Itu Bus SPI (SPI Bus)?", "id": "Bus Komunikasi Serial Sinkron." },
  { "en": "Apa Kepanjangan SPI (Serial Peripheral Interface)?", "id": "Antarmuka Periferal Serial." },
  { "en": "Apa Kepanjangan UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Penerima-Pengirim Asinkron Universal." },
  { "en": "Apa Fungsi UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial (Contoh: RS-232)." },
  { "en": "Apa Itu RS-232 (RS-232)?", "id": "Standar Komunikasi Serial Titik Ke Titik." },
  { "en": "Apa Itu RS-485 (RS-485)?", "id": "Standar Komunikasi Serial Diferensial." },
  { "en": "Apa Keunggulan RS-485 (RS-485) Dibanding RS-232?", "id": "Jarak Lebih Jauh, Multi-Perangkat." },
  { "en": "Apa Itu CAN Bus (CAN Bus)?", "id": "Bus Komunikasi Untuk Kendaraan (Otomotif)." },
  { "en": "Apa Kepanjangan CAN (Controller Area Network)?", "id": "Jaringan Area Kontroler." },
  { "en": "Apa Itu Dioda Flyback (Flyback Diode)?", "id": "Nama Lain Dioda Freewheeling." },
  { "en": "Apa Fungsi Dioda Flyback (Flyback Diode)?", "id": "Meredam Lonjakan Tegangan Induktif." },
  { "en": "Apa Itu Konverter Flyback (Flyback Converter)?", "id": "Jenis SMPS (Switch-Mode Power Supply) Terisolasi." },
  { "en": "Apa Itu Snubber (Snubber Circuit)?", "id": "Rangkaian Pelindung Saklar (Umumnya R-C)." },
  { "en": "Apa Fungsi Snubber (Snubber Circuit)?", "id": "Meredam Lonjakan Tegangan Saat Switching." },
  { "en": "Apa Itu Zero Crossing Detector (Detektor Persilangan Nol)?", "id": "Rangkaian Pendeteksi Tegangan AC Melintasi Nol." },
  { "en": "Dimana Zero Crossing Detector (Detektor Persilangan Nol) Digunakan?", "id": "Rangkaian Dimmer (Kontrol Fasa)." },
  { "en": "Apa Itu Kontrol Fasa (Phase Control)?", "id": "Metode Kontrol Daya AC (Memotong Gelombang)." },
  { "en": "Apa Kepanjangan FMEA (Failure Modes and Effects Analysis)?", "id": "Analisis Mode Kegagalan, Efeknya." },
  { "en": "Apa Kepanjangan MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-Rata Antar Kegagalan." },
  { "en": "Apa Kepanjangan MTTF (Mean Time To Failure)?", "id": "Waktu Rata-Rata Menuju Kegagalan." },
  { "en": "Apa Itu Sistem Tenaga On-Grid (On-Grid)?", "id": "Sistem Terhubung Ke Jaringan PLN." },
  { "en": "Apa Itu Sistem Tenaga Off-Grid (Off-Grid)?", "id": "Sistem Mandiri (Tidak Terhubung PLN)." },
  { "en": "Apa Itu Sistem Hibrida (Hybrid System) Tenaga?", "id": "Gabungan (Contoh: PLTS, Baterai, Jaringan)." },
  { "en": "Apa Fungsi Inverter On-Grid (Grid-Tie Inverter)?", "id": "Mengubah DC Ke AC, Sinkron Jaringan." },
  { "en": "Apa Itu Net Metering (Net Metering)?", "id": "Sistem Ekspor-Impor Listrik Ke PLN." },
  { "en": "Apa Kepanjangan BESS (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Baterai." },
  { "en": "Apa Fungsi BESS (Battery Energy Storage System)?", "id": "Menyimpan Energi Listrik Untuk Nanti." },
  { "en": "Apa Itu Load Flow (Aliran Beban) Studi?", "id": "Analisis Aliran Daya Jaringan Listrik." },
  { "en": "Apa Itu Kestabilan Transien (Transient Stability)?", "id": "Kemampuan Sistem Stabil Pasca Gangguan." },
  { "en": "Apa Kepanjangan FACTS (Flexible AC Transmission Systems)?", "id": "Sistem Transmisi AC Fleksibel." },
  { "en": "Apa Fungsi FACTS (Flexible AC Transmission Systems)?", "id": "Meningkatkan Kontrol, Kapasitas Transmisi." },
  { "en": "Apa Kepanjangan HVDC (High Voltage Direct Current)?", "id": "Transmisi Arus Searah Tegangan Tinggi." },
  { "en": "Apa Keunggulan Transmisi HVDC (High Voltage Direct Current)?", "id": "Rugi Rendah Untuk Jarak Sangat Jauh." },
  { "en": "Apa Itu Busbar (Busbar) Listrik?", "id": "Konduktor Pusat Penerima, Pembagi Daya." },
  { "en": "Apa Fungsi Panel Hubung Bagi (PHB)?", "id": "Membagi, Mengendalikan Tenaga Listrik." },
  { "en": "Apa Kepanjangan SDP (Sub Distribution Panel)?", "id": "Panel Distribusi Cabang." },
  { "en": "Apa Kepanjangan MDP (Main Distribution Panel)?", "id": "Panel Distribusi Utama." },
  { "en": "Apa Itu Motor DC Tanpa Sikat?", "id": "Brushless DC Motor (BLDC)." },
  { "en": "Bagaimana Komutasi Motor BLDC (Brushless DC Motor)?", "id": "Komutasi Elektronik (Sensor Hall)." },
  { "en": "Apa Itu Motor Reluktansi (Reluctance Motor)?", "id": "Motor Berbasis Prinsip Reluktansi Minimum." },
  { "en": "Apa Itu Motor Histeresis (Hysteresis Motor)?", "id": "Motor Sinkron Berbasis Efek Histeresis." },
  { "en": "Apa Itu Aktor Linier (Linear Actuator)?", "id": "Aktuator Yang Menghasilkan Gerakan Lurus." },
  { "en": "Apa Itu Kontaktor (Contactor)?", "id": "Saklar Elektromagnetik Untuk Daya Besar." },
  { "en": "Apa Beda Kontaktor (Contactor) Dan Relay (Relay)?", "id": "Kontaktor Untuk Arus Jauh Lebih Besar." },
  { "en": "Apa Kepanjangan NO (Normally Open) Pada Kontaktor?", "id": "Kontak Normal Terbuka." },
  { "en": "Apa Kepanjangan NC (Normally Closed) Pada Kontaktor?", "id": "Kontak Normal Tertutup." },
  { "en": "Apa Itu Thermal Overload Relay (TOR)?", "id": "Rele Proteksi Motor Dari Beban Berlebih." },
  { "en": "Bagaimana Prinsip Kerja TOR (Thermal Overload Relay)?", "id": "Menggunakan Prinsip Bimetal Panas." },
  { "en": "Apa Itu Rangkaian DOL (Direct On Line) Starter?", "id": "Starter Motor Langsung Ke Jaringan." },
  { "en": "Apa Itu Rangkaian Star-Delta (Star-Delta) Starter?", "id": "Starter Motor Mengurangi Arus Awal." },
  { "en": "Bagaimana Koneksi Motor Saat Star (Bintang)?", "id": "Tegangan Rendah, Arus Rendah." },
  { "en": "Bagaimana Koneksi Motor Saat Delta (Segitiga)?", "id": "Tegangan Penuh, Arus Penuh." },
  { "en": "Apa Fungsi Rangkaian Interlock (Pengunci)?", "id": "Mencegah Dua Kondisi Berlawanan Aktif." },
  { "en": "Apa Fungsi Tombol Jogging (Jogging Button)?", "id": "Menjalankan Motor Sesaat (Inching)." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit) 7400?", "id": "Gerbang NAND (NAND Gate) Quad 2-Input." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit) 7402?", "id": "Gerbang NOR (NOR Gate) Quad 2-Input." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit) 7404?", "id": "Inverter (NOT Gate) Hex." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit) 7408?", "id": "Gerbang AND (AND Gate) Quad 2-Input." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit) 7432?", "id": "Gerbang OR (OR Gate) Quad 2-Input." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit) 7486?", "id": "Gerbang XOR (XOR Gate) Quad 2-Input." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit) 7805?", "id": "Regulator Tegangan Linear Positif 5V." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit) 7905?", "id": "Regulator Tegangan Linear Negatif 5V." },
  { "en": "Apa Fungsi Kapasitor Input Pada IC (Integrated Circuit) 7805?", "id": "Filter Riak (Ripple) Tegangan Input." },
  { "en": "Apa Fungsi Kapasitor Output Pada IC (Integrated Circuit) 7805?", "id": "Meningkatkan Kestabilan Respon Transien." },
  { "en": "Apa Kepanjangan LM317 (LM317)?", "id": "IC (Integrated Circuit) Regulator Tegangan Dapat Diatur." },
  { "en": "Apa Kepanjangan LM35 (LM35)?", "id": "Sensor Suhu Analog Presisi." },
  { "en": "Apa Kepanjangan LM358 (LM358)?", "id": "IC (Integrated Circuit) Op-Amp Ganda (Dual)." },
  { "en": "Apa Kepanjangan ULN2003 (ULN2003)?", "id": "IC (Integrated Circuit) Driver Array Darlington." },
  { "en": "Apa Fungsi IC (Integrated Circuit) ULN2003 (ULN2003)?", "id": "Menggerakkan Beban Arus Tinggi (Relay)." },
  { "en": "Apa Itu Sinyal Modulasi AM (Amplitude Modulation)?", "id": "Amplitudo Carrier Berubah Sesuai Informasi." },
  { "en": "Apa Itu Sinyal Modulasi FM (Frequency Modulation)?", "id": "Frekuensi Carrier Berubah Sesuai Informasi." },
  { "en": "Apa Itu Sinyal Modulasi PM (Phase Modulation)?", "id": "Fasa Carrier Berubah Sesuai Informasi." },
  { "en": "Apa Itu Sideband (Pita Samping) Modulasi AM?", "id": "Frekuensi Baru Diatas, Dibawah Carrier." },
  { "en": "Apa Kepanjangan DSB-SC (Double Sideband Suppressed Carrier)?", "id": "Modulasi AM (Amplitude Modulation) Tanpa Carrier." },
  { "en": "Apa Kepanjangan SSB-SC (Single Sideband Suppressed Carrier)?", "id": "Modulasi AM (Amplitude Modulation) Satu Sideband." },
  { "en": "Apa Keuntungan SSB-SC (Single Sideband Suppressed Carrier)?", "id": "Menghemat Bandwidth Dan Daya." },
  { "en": "Apa Itu Demodulator (Demodulator) Selubung?", "id": "Detektor Sinyal AM (Amplitude Modulation) Sederhana." },
  { "en": "Apa Itu Penguat Kelas A (Class A Amplifier)?", "id": "Titik Kerja Di Tengah (Efisiensi Rendah)." },
  { "en": "Apa Itu Penguat Kelas B (Class B Amplifier)?", "id": "Titik Kerja Di Cutoff (Efisiensi Sedang)." },
  { "en": "Apa Itu Penguat Kelas AB (Class AB Amplifier)?", "id": "Titik Kerja Sedikit Diatas Cutoff." },
  { "en": "Apa Itu Penguat Kelas C (Class C Amplifier)?", "id": "Titik Kerja Dibawah Cutoff (Efisiensi Tinggi)." },
  { "en": "Apa Itu Penguat Kelas D (Class D Amplifier)?", "id": "Penguat Switching (Efisiensi Sangat Tinggi)." },
  { "en": "Apa Masalah Penguat Kelas B (Class B)?", "id": "Distorsi Crossover (Crossover Distortion)." },
  { "en": "Bagaimana Penguat Kelas AB (Class AB) Mengatasi Crossover?", "id": "Memberi Sedikit Bias Maju Dioda." },
  { "en": "Dimana Penguat Kelas C (Class C) Digunakan?", "id": "Aplikasi Frekuensi Radio (RF)." },
  { "en": "Dimana Penguat Kelas D (Class D) Digunakan?", "id": "Amplifier Audio Daya Tinggi, Efisien." },
  { "en": "Apa Itu Penguat Push-Pull (Push-Pull Amplifier)?", "id": "Konfigurasi Umum Penguat Kelas B, AB." },
  { "en": "Apa Itu Heat Sink (Heat Sink)?", "id": "Bahan Logam Pembuang Panas Komponen." },
  { "en": "Mengapa Transistor Daya Perlu Heat Sink?", "id": "Mencegah Kerusakan Akibat Panas Berlebih." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance)?", "id": "Hambatan Aliran Panas (Junction Ke Udara)." },
  { "en": "Apa Itu Simplex (Simpleks) Komunikasi?", "id": "Komunikasi Satu Arah Saja." },
  { "en": "Apa Itu Half-Duplex (Setengah Dupleks) Komunikasi?", "id": "Komunikasi Dua Arah, Bergantian." },
  { "en": "Apa Itu Full-Duplex (Dupleks Penuh) Komunikasi?", "id": "Komunikasi Dua Arah, Bersamaan." },
  { "en": "Berikan Contoh Komunikasi Simplex (Simpleks)?", "id": "Siaran Radio Atau Televisi." },
  { "en": "Berikan Contoh Komunikasi Half-Duplex (Setengah Dupleks)?", "id": "Walkie-Talkie (Handy Talky)." },
  { "en": "Berikan Contoh Komunikasi Full-Duplex (Dupleks Penuh)?", "id": "Telepon Genggam (Handphone)." },
  { "en": "Apa Itu Error Correction Code (ECC)?", "id": "Kode Deteksi, Koreksi Kesalahan Data." },
  { "en": "Apa Itu Kode Hamming (Hamming Code)?", "id": "Contoh Sederhana Error Correction Code (ECC)." },
  { "en": "Apa Itu Bandwidth (Lebar Pita) Sinyal?", "id": "Rentang Frekuensi Yang Ditempati Sinyal." },
  { "en": "Apa Itu Channel (Saluran) Komunikasi?", "id": "Media Fisik Transmisi Sinyal." },
  { "en": "Apa Itu Kapasitas Saluran (Channel Capacity) Shannon?", "id": "Laju Data Maksimum Teoretis Saluran." },
  { "en": "Apa Itu Sinyal Baseband (Baseband Signal)?", "id": "Sinyal Informasi Asli (Sebelum Modulasi)." },
  { "en": "Apa Itu Sinyal Broadband (Broadband Signal)?", "id": "Transmisi Frekuensi Lebar (Banyak Saluran)." },
  { "en": "Apa Kepanjangan CDMA (Code Division Multiple Access)?", "id": "Akses Ganda Pembagian Kode." },
  { "en": "Apa Kepanjangan TDMA (Time Division Multiple Access)?", "id": "Akses Ganda Pembagian Waktu." },
  { "en": "Apa Kepanjangan FDMA (Frequency Division Multiple Access)?", "id": "Akses Ganda Pembagian Frekuensi." },
  { "en": "Apa Kepanjangan GSM (Global System for Mobile Communications)?", "id": "Sistem Global Komunikasi Bergerak." },
  { "en": "Apa Itu Sel (Cell) Jaringan Seluler?", "id": "Area Geografis Dilayani Satu BTS." },
  { "en": "Apa Kepanjangan BTS (Base Transceiver Station)?", "id": "Stasiun Pemancar Pangkalan." },
  { "en": "Apa Itu Handoff (Handoff) Jaringan Seluler?", "id": "Proses Perpindahan Panggilan Antar Sel." },
  { "en": "Apa Kepanjangan GPS (Global Positioning System)?", "id": "Sistem Pemosisi Global." },
  { "en": "Bagaimana GPS (Global Positioning System) Bekerja?", "id": "Trilaterasi Sinyal Dari Satelit." },
  { "en": "Apa Itu Jembatan Maxwell (Maxwell Bridge)?", "id": "Jembatan AC (Arus Bolak-Balik) Ukur Induktansi." },
  { "en": "Apa Itu Jembatan Hay (Hay Bridge)?", "id": "Jembatan AC (Arus Bolak-Balik) Ukur Induktansi Q Tinggi." },
  { "en": "Apa Itu Jembatan Schering (Schering Bridge)?", "id": "Jembatan AC (Arus Bolak-Balik) Ukur Kapasitansi, Faktor Disipasi." },
  { "en": "Apa Itu Faktor Disipasi (Dissipation Factor) Kapasitor?", "id": "Ukuran Ketidakidealan Kapasitor (Rugi-Rugi)." },
  { "en": "Apa Kepanjangan MCCB (Molded Case Circuit Breaker)?", "id": "Pemutus Sirkuit Kotak Cetak." },
  { "en": "Apa Kepanjangan ACB (Air Circuit Breaker)?", "id": "Pemutus Sirkuit Udara." },
  { "en": "Apa Kepanjangan VCB (Vacuum Circuit Breaker)?", "id": "Pemutus Sirkuit Vakum." },
  { "en": "Apa Kepanjangan SF6 (Sulfur Hexafluoride) Circuit Breaker?", "id": "Pemutus Sirkuit Gas SF6." },
  { "en": "Apa Fungsi Media Pemadam Busur Api?", "id": "Memadamkan Busur Api Saat Pemutusan." },
  { "en": "Apa Kepanjangan RCBO (Residual Current Breaker with Overcurrent)?", "id": "Pemutus Arus Sisa, Proteksi Arus Lebih." },
  { "en": "Apa Kelebihan RCBO (Residual Current Breaker with Overcurrent)?", "id": "Gabungan Fungsi MCB Dan ELCB." },
  { "en": "Apa Itu Penangkal Petir (Lightning Rod)?", "id": "Pelindung Eksternal Dari Sambaran Petir." },
  { "en": "Apa Fungsi Isolator (Isolator) Jaringan Transmisi?", "id": "Mengisolasi Konduktor Dari Tiang." },
  { "en": "Apa Bahan Umum Isolator (Isolator) Jaringan?", "id": "Bahan Keramik Atau Gelas." },
  { "en": "Apa Kepanjangan ACSR (Aluminum Conductor Steel Reinforced)?", "id": "Konduktor Aluminium Diperkuat Baja." },
  { "en": "Mengapa ACSR (Aluminum Conductor Steel Reinforced) Digunakan?", "id": "Ringan, Kuat, Konduktivitas Baik." },
  { "en": "Apa Fungsi Menara Transmisi (Transmission Tower)?", "id": "Menyangga Konduktor Saluran Udara." },
  { "en": "Apa Itu Transformator Instrumen (Instrument Transformer)?", "id": "Trafo Pengukuran (CT Dan PT)." },
  { "en": "Apa Kepanjangan CT (Current Transformer)?", "id": "Transformator Arus." },
  { "en": "Apa Fungsi CT (Current Transformer)?", "id": "Menurunkan Arus Besar Ke Level Ukur." },
  { "en": "Apa Kepanjangan PT (Potential Transformer)?", "id": "Transformator Potensial (Tegangan)." },
  { "en": "Apa Fungsi PT (Potential Transformer)?", "id": "Menurunkan Tegangan Tinggi Ke Level Ukur." },
  { "en": "Apa Kepanjangan CVT (Capacitive Voltage Transformer)?", "id": "Transformator Tegangan Kapasitif." },
  { "en": "Apa Itu Rele Buchholz (Buchholz Relay)?", "id": "Rele Proteksi Internal Transformator Minyak." },
  { "en": "Gangguan Apa Yang Dideteksi Rele Buchholz?", "id": "Gangguan Gas (Gelembung) Di Minyak." },
  { "en": "Apa Fungsi Minyak Trafo (Transformer Oil)?", "id": "Pendingin Dan Isolasi Listrik." },
  { "en": "Apa Itu Tangki Konservator (Conservator Tank) Trafo?", "id": "Tangki Cadangan Pemuaian Minyak Trafo." },
  { "en": "Apa Itu Pernapasan (Breather) Trafo?", "id": "Alat Filter Udara Masuk Konservator." },
  { "en": "Apa Isi Dari Pernapasan (Breather) Trafo?", "id": "Berisi Silika Gel (Silica Gel)." },
  { "en": "Apa Fungsi Silika Gel (Silica Gel) Trafo?", "id": "Menyerap Kelembaban Udara." },
  { "en": "Apa Warna Silika Gel (Silica Gel) Kering?", "id": "Biru (Terkadang Oranye)." },
  { "en": "Apa Warna Silika Gel (Silica Gel) Jenuh?", "id": "Merah Muda (Pink) Atau Hijau." },
  { "en": "Apa Itu Tap Trafo (Transformer Taps)?", "id": "Penyetelan Rasio Belitan Transformator." },
  { "en": "Apa Kepanjangan OLTC (On-Load Tap Changer)?", "id": "Pengubah Tap Berbeban." },
  { "en": "Apa Fungsi OLTC (On-Load Tap Changer)?", "id": "Mengubah Tap Trafo Saat Beroperasi." },
  { "en": "Apa Kepanjangan NLTC (No-Load Tap Changer)?", "id": "Pengubah Tap Tanpa Beban." },
  { "en": "Apa Kepanjangan ONAN (Oil Natural Air Natural)?", "id": "Pendinginan Minyak Alami, Udara Alami." },
  { "en": "Apa Kepanjangan ONAF (Oil Natural Air Forced)?", "id": "Pendinginan Minyak Alami, Udara Paksa." },
  { "en": "Apa Kepanjangan OFAF (Oil Forced Air Forced)?", "id": "Pendinginan Minyak Paksa, Udara Paksa." },
  { "en": "Apa Itu Trafo Tipe Kering (Dry Type)?", "id": "Transformator Tanpa Menggunakan Minyak." },
  { "en": "Apa Itu Uji Rangkaian Terbuka (Open Circuit) Trafo?", "id": "Pengujian Untuk Mengukur Rugi Inti." },
  { "en": "Rugi Apa Yang Diukur Uji Rangkaian Terbuka?", "id": "Rugi Inti (Histeresis, Arus Eddy)." },
  { "en": "Apa Itu Uji Hubung Singkat (Short Circuit) Trafo?", "id": "Pengujian Untuk Mengukur Rugi Tembaga." },
  { "en": "Rugi Apa Yang Diukur Uji Hubung Singkat?", "id": "Rugi Tembaga (I²R)." },
  { "en": "Apa Itu Regulasi Tegangan (Voltage Regulation) Trafo?", "id": "Perbedaan Tegangan Tanpa Beban, Berbeban." },
  { "en": "Berapa Nilai Ideal Regulasi Tegangan (Voltage Regulation)?", "id": "Idealnya Nol Persen." },
  { "en": "Apa Itu Autotransformator (Autotransformer)?", "id": "Transformator Hanya Dengan Satu Belitan." },
  { "en": "Apa Keuntungan Autotransformator (Autotransformer)?", "id": "Ukuran Fisik Lebih Kecil, Efisien." },
  { "en": "Apa Aturan Tangan Kanan Fleming (Fleming's Right Hand)?", "id": "Aturan Untuk Menentukan Arah GGL Induksi." },
  { "en": "Apa Yang Ditentukan Aturan Tangan Kanan Fleming?", "id": "Arah Arus Induksi (Prinsip Generator)." },
  { "en": "Apa Aturan Tangan Kiri Fleming (Fleming's Left Hand)?", "id": "Aturan Menentukan Arah Gaya Magnetik." },
  { "en": "Apa Yang Ditentukan Aturan Tangan Kiri Fleming?", "id": "Arah Gaya Gerak (Prinsip Motor)." },
  { "en": "Apa Itu GGL Balik (Back EMF)?", "id": "Tegangan Induksi Melawan Arus Motor." },
  { "en": "Dimana GGL Balik (Back EMF) Terjadi?", "id": "Terjadi Di Dalam Motor Listrik." },
  { "en": "Apa Bunyi Hukum Lenz (Lenz's Law)?", "id": "Arah Arus Induksi Melawan Penyebabnya." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik) Induksi?", "id": "Tegangan Timbul Akibat Perubahan Fluks." },
  { "en": "Apa Kepanjangan GGM (Gaya Gerak Magnet)?", "id": "Gaya Gerak Magnet (MMF)." },
  { "en": "Apa Satuan GGM (Gaya Gerak Magnet)?", "id": "Ampere-Turns (Lilitan Ampere)." },
  { "en": "Apa Itu Permeansi (Permeance)?", "id": "Kemudahan Fluks Magnet Mengalir." },
  { "en": "Apa Kebalikan Dari Reluktansi (Reluctance)?", "id": "Permeansi (Permeance)." },
  { "en": "Apa Itu Kekuatan Medan Magnet (H)?", "id": "Intensitas Medan Magnetik." },
  { "en": "Apa Satuan Kekuatan Medan Magnet (H)?", "id": "Ampere Per Meter (A/m)." },
  { "en": "Apa Itu Rangkaian Magnetik (Magnetic Circuit)?", "id": "Jalur Tertutup Untuk Fluks Magnetik." },
  { "en": "Apa Itu Lup Histeresis (Hysteresis Loop)?", "id": "Kurva B-H Menunjukkan Sifat Magnetik." },
  { "en": "Apa Itu Kondensor Sinkron (Synchronous Condenser)?", "id": "Motor Sinkron Tanpa Beban Mekanis." },
  { "en": "Apa Fungsi Kondensor Sinkron (Synchronous Condenser)?", "id": "Memperbaiki Faktor Daya (Menyerap/Suplai VAR)." },
  { "en": "Apa Itu Penaik Fasa (Phase Advancer)?", "id": "Alat Perbaikan Faktor Daya Motor Induksi." },
  { "en": "Apa Itu Pelepasan Korona (Corona Discharge)?", "id": "Pelepasan Listrik Di Sekitar Konduktor." },
  { "en": "Apa Efek Pelepasan Korona (Corona Discharge)?", "id": "Rugi Daya, Noise Radio, Bau Ozon." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Distribusi Arus Terganggu Konduktor Dekat." },
  { "en": "Apa Itu Efek Ferranti (Ferranti Effect)?", "id": "Tegangan Ujung Penerima Lebih Tinggi Pengirim." },
  { "en": "Kapan Efek Ferranti (Ferranti Effect) Terjadi?", "id": "Saluran Transmisi Panjang Tanpa Beban." },
  { "en": "Apa Itu Resistor Pembumian (Grounding Resistor)?", "id": "Resistor Membatasi Arus Gangguan Tanah." },
  { "en": "Apa Kepanjangan NGR (Neutral Grounding Resistor)?", "id": "Resistor Pembumian Netral." },
  { "en": "Apa Itu Saklar Reed (Reed Switch)?", "id": "Saklar Kontak Digerakkan Medan Magnet." },
  { "en": "Apa Kepanjangan LVDT (Linear Variable Differential Transformer)?", "id": "Transformator Diferensial Variabel Linear." },
  { "en": "Apa Fungsi LVDT (Linear Variable Differential Transformer)?", "id": "Sensor Posisi Atau Perpindahan Linear." },
  { "en": "Apa Itu Sensor Piezoelektrik (Piezoelectric Sensor)?", "id": "Sensor Mengubah Tekanan Menjadi Listrik." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RC?", "id": "Waktu Pengisian Kapasitor (63.2%)." },
  { "en": "Apa Rumus Konstanta Waktu (Time Constant) RC?", "id": "Tau (τ) = R x C." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RL?", "id": "Waktu Arus Induktor Mencapai (63.2%)." },
  { "en": "Apa Rumus Konstanta Waktu (Time Constant) RL?", "id": "Tau (τ) = L / R." },
  { "en": "Apa Itu Respon Transien (Transient Response) Rangkaian?", "id": "Respon Sementara Saat Terjadi Perubahan." },
  { "en": "Apa Itu Respon Keadaan Tunak (Steady State Response)?", "id": "Respon Rangkaian Setelah Waktu Lama." },
  { "en": "Apa Itu Rangkaian Orde Pertama (First-Order Circuit)?", "id": "Rangkaian RC (Resistor Kapasitor) Atau RL (Resistor Induktor) Saja." },
  { "en": "Apa Itu Rangkaian Orde Kedua (Second-Order Circuit)?", "id": "Rangkaian RLC (Resistor Induktor Kapasitor)." },
  { "en": "Apa Itu Respon Alami (Natural Response)?", "id": "Respon Rangkaian Tanpa Sumber Eksternal." },
  { "en": "Apa Itu Respon Paksa (Forced Response)?", "id": "Respon Rangkaian Akibat Sumber Eksternal." },
  { "en": "Apa Itu Redaman (Damping) Rangkaian RLC?", "id": "Proses Hilangnya Energi (Osilasi Mereda)." },
  { "en": "Apa Itu Overdamped (Redaman Berlebih)?", "id": "Respon Lambat Tanpa Osilasi." },
  { "en": "Apa Itu Critically Damped (Redaman Kritis)?", "id": "Respon Tercepat Tanpa Osilasi." },
  { "en": "Apa Itu Underdamped (Redaman Kurang)?", "id": "Respon Berosilasi Sebelum Stabil." },
  { "en": "Apa Itu Faktor Redaman (Damping Factor) (Zeta)?", "id": "Parameter Penentu Jenis Redaman RLC." },
  { "en": "Apa Itu Frekuensi Resonansi (Resonance Frequency) Alami?", "id": "Frekuensi Osilasi Rangkaian Tanpa Redaman." },
  { "en": "Apa Itu Frekuensi Resonansi (Resonance Frequency) Teredam?", "id": "Frekuensi Osilasi Rangkaian Underdamped." },
  { "en": "Apa Itu Bidang Kompleks (Complex Plane) (s-Plane)?", "id": "Analisis Rangkaian Menggunakan Transformasi Laplace." },
  { "en": "Apa Sumbu Riil (Real Axis) Bidang-s?", "id": "Mewakili Faktor Redaman (Alpha)." },
  { "en": "Apa Sumbu Imajiner (Imaginary Axis) Bidang-s?", "id": "Mewakili Frekuensi (Omega)." },
  { "en": "Dimana Letak Poles (Poles) Sistem Stabil?", "id": "Di Sisi Kiri Bidang-s." },
  { "en": "Apa Itu Resistansi Input (Input Resistance)?", "id": "Hambatan Dilihat Dari Terminal Input." },
  { "en": "Apa Itu Resistansi Output (Output Resistance)?", "id": "Hambatan Dilihat Dari Terminal Output." },
  { "en": "Apa Itu Penguat Common Emitter (Common Emitter)?", "id": "Konfigurasi Transistor Penguatan Tegangan Tinggi." },
  { "en": "Apa Itu Penguat Common Collector (Common Collector)?", "id": "Konfigurasi Transistor Pengikut Emitor." },
  { "en": "Apa Nama Lain Common Collector (Common Collector)?", "id": "Pengikut Emitor (Emitter Follower)." },
  { "en": "Apa Karakteristik Common Collector (Common Collector)?", "id": "Penguatan Arus Tinggi, Impedansi Input Tinggi." },
  { "en": "Apa Itu Penguat Common Base (Common Base)?", "id": "Konfigurasi Transistor Penguatan Arus Rendah." },
  { "en": "Apa Karakteristik Common Base (Common Base)?", "id": "Impedansi Input Rendah, Output Tinggi." },
  { "en": "Apa Itu Garis Beban (Load Line) DC Transistor?", "id": "Grafik Hubungan Arus, Tegangan Kolektor." },
  { "en": "Apa Itu Titik Kerja (Q-Point) Transistor?", "id": "Titik Operasi DC (Arus, Tegangan Diam)." },
  { "en": "Apa Itu Bias (Biasing) Transistor?", "id": "Pemberian Tegangan DC Atur Titik Kerja." },
  { "en": "Apa Jenis Rangkaian Bias (Biasing) Paling Stabil?", "id": "Rangkaian Pembagi Tegangan (Voltage Divider Bias)." },
  { "en": "Apa Itu Kestabilan Termal (Thermal Stability) Transistor?", "id": "Kemampuan Jaga Titik Kerja Terhadap Suhu." },
  { "en": "Apa Itu Thermal Runaway (Lolos Termal)?", "id": "Peningkatan Suhu Sebabkan Kerusakan Transistor." },
  { "en": "Apa Model Hibrida (Hybrid Model) Transistor?", "id": "Model Parameter-h Untuk Analisis Sinyal Kecil." },
  { "en": "Apa Itu Penguat Sinyal Kecil (Small-Signal Amplifier)?", "id": "Penguat Untuk Sinyal Input Amplitudo Kecil." },
  { "en": "Apa Itu Penguat Sinyal Besar (Large-Signal Amplifier)?", "id": "Penguat Daya (Contoh: Kelas A, B)." },
  { "en": "Apa Kepanjangan MOSFET (Metal-Oxide-Semiconductor FET) Tipe Enhancement?", "id": "Tipe Enhancement (Saluran Dibentuk Tegangan)." },
  { "en": "Apa Kepanjangan MOSFET (Metal-Oxide-Semiconductor FET) Tipe Depletion?", "id": "Tipe Depletion (Saluran Ada Tanpa Tegangan)." },
  { "en": "Apa Itu NMOS (N-Channel MOSFET)?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) Kanal-N." },
  { "en": "Apa Itu PMOS (P-Channel MOSFET)?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) Kanal-P." },
  { "en": "Apa Itu Gerbang Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Logika Gabungan PMOS Dan NMOS." },
  { "en": "Apa Keunggulan Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Konsumsi Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Resistansi Drain-Source (RDS-On) MOSFET?", "id": "Resistansi MOSFET Saat Kondisi On." },
  { "en": "Apa Itu Tegangan Threshold (Threshold Voltage) MOSFET?", "id": "Tegangan Gate Minimum Untuk On." },
  { "en": "Apa Kepanjangan VMOS (Vertical MOSFET)?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) Struktur Vertikal." },
  { "en": "Apa Kepanjangan DMOS (Double-Diffused MOSFET)?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) Difusi Ganda." },
  { "en": "Apa Itu Memori Volatil (Volatile Memory)?", "id": "Memori Hilang Data Tanpa Listrik." },
  { "en": "Apa Itu Memori Non-Volatil (Non-Volatile Memory)?", "id": "Memori Tetap Simpan Data Tanpa Listrik." },
  { "en": "Berikan Contoh Memori Volatil (Volatile Memory)?", "id": "RAM (Random Access Memory)." },
  { "en": "Berikan Contoh Memori Non-Volatil (Non-Volatile Memory)?", "id": "ROM (Read Only Memory), Hard Disk." },
  { "en": "Apa Kepanjangan SRAM (Static RAM)?", "id": "RAM (Random Access Memory) Statis." },
  { "en": "Apa Kepanjangan DRAM (Dynamic RAM)?", "id": "RAM (Random Access Memory) Dinamis." },
  { "en": "Apa Komponen Penyimpan Data SRAM (Static RAM)?", "id": "Flip-Flop (Latch)." },
  { "en": "Apa Komponen Penyimpan Data DRAM (Dynamic RAM)?", "id": "Kapasitor Kecil." },
  { "en": "Mengapa DRAM (Dynamic RAM) Perlu Refresh (Disegarkan)?", "id": "Muatan Kapasitor Bocor Seiring Waktu." },
  { "en": "Mana Yang Lebih Cepat, SRAM (Static RAM) Atau DRAM (Dynamic RAM)?", "id": "SRAM (Static RAM) Jauh Lebih Cepat." },
  { "en": "Mana Yang Lebih Padat, SRAM (Static RAM) Atau DRAM (Dynamic RAM)?", "id": "DRAM (Dynamic RAM) Jauh Lebih Padat." },
  { "en": "Dimana SRAM (Static RAM) Biasanya Digunakan?", "id": "Memori Cache (Cache Memory) CPU." },
  { "en": "Dimana DRAM (Dynamic RAM) Biasanya Digunakan?", "id": "Memori Utama (Main Memory) Komputer." },
  { "en": "Apa Kepanjangan PROM (Programmable ROM)?", "id": "ROM (Read Only Memory) Terprogram." },
  { "en": "Apa Kepanjangan EPROM (Erasable Programmable ROM)?", "id": "ROM (Read Only Memory) Terprogram Dapat Dihapus." },
  { "en": "Bagaimana Cara Menghapus Data EPROM (Erasable Programmable ROM)?", "id": "Menggunakan Sinar Ultraviolet (UV)." },
  { "en": "Apa Ciri Fisik Chip EPROM (Erasable Programmable ROM)?", "id": "Memiliki Jendela Kaca Kuarsa." },
  { "en": "Apa Kepanjangan EEPROM (Electrically Erasable PROM)?", "id": "PROM (Programmable ROM) Terhapus Listrik." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Jenis EEPROM (Electrically Erasable PROM) Kecepatan Tinggi." },
  { "en": "Dimana Memori Flash (Flash Memory) Digunakan?", "id": "USB (Universal Serial Bus) Flash Drive, SSD (Solid State Drive)." },
  { "en": "Apa Kepanjangan SSD (Solid State Drive)?", "id": "Penggerak Keadaan Padat." },
  { "en": "Apa Itu Arsitektur Von Neumann (Von Neumann Architecture)?", "id": "Data, Instruksi Disimpan Memori Sama." },
  { "en": "Apa Itu Arsitektur Harvard (Harvard Architecture)?", "id": "Data, Instruksi Disimpan Memori Terpisah." },
  { "en": "Arsitektur Apa Yang Digunakan Mikrokontroler?", "id": "Umumnya Arsitektur Harvard." },
  { "en": "Apa Itu Sistem Bus (System Bus)?", "id": "Jalur Komunikasi CPU, Memori, I/O." },
  { "en": "Apa Itu Bus Alamat (Address Bus)?", "id": "Bus Penentu Lokasi Alamat Memori." },
  { "en": "Apa Itu Bus Data (Data Bus)?", "id": "Bus Transfer Data Antar Komponen." },
  { "en": "Apa Itu Bus Kontrol (Control Bus)?", "id": "Bus Sinyal Kontrol, Sinkronisasi." },
  { "en": "Apa Itu Antarmuka Paralel (Parallel Interface)?", "id": "Transfer Data Beberapa Bit Bersamaan." },
  { "en": "Apa Itu Antarmuka Serial (Serial Interface)?", "id": "Transfer Data Satu Bit Bergantian." },
  { "en": "Apa Itu Baud (Baud)?", "id": "Ukuran Kecepatan Modulasi Simbol." },
  { "en": "Apa Kepanjangan ASCII (American Standard Code for Information Interchange)?", "id": "Kode Standar Amerika Pertukaran Informasi." },
  { "en": "Apa Itu Sistem Bilangan Heksadesimal (Hexadecimal System)?", "id": "Sistem Bilangan Basis Enam Belas." },
  { "en": "Apa Itu Sistem Bilangan Oktal (Octal System)?", "id": "Sistem Bilangan Basis Delapan." },
  { "en": "Apa Itu Komplemen Dua (Two's Complement)?", "id": "Metode Representasi Bilangan Biner Negatif." },
  { "en": "Apa Itu Floating Point (Titik Mengambang)?", "id": "Metode Representasi Bilangan Pecahan Biner." },
  { "en": "Apa Itu Big-Endian (Big-Endian)?", "id": "Urutan Byte (MSB Disimpan Dahulu)." },
  { "en": "Apa Itu Little-Endian (Little-Endian)?", "id": "Urutan Byte (LSB Disimpan Dahulu)." },
  { "en": "Apa Kepanjangan MSB (Most Significant Bit)?", "id": "Bit Paling Berarti (Paling Kiri)." },
  { "en": "Apa Kepanjangan LSB (Least Significant Bit)?", "id": "Bit Kurang Berarti (Paling Kanan)." },
  { "en": "Apa Itu Firmware (Perangkat Tegar)?", "id": "Perangkat Lunak Tertanam Perangkat Keras." },
  { "en": "Apa Itu BIOS (Basic Input Output System)?", "id": "Firmware (Perangkat Tegar) Inisialisasi Hardware Komputer." },
  { "en": "Apa Kepanjangan UEFI (Unified Extensible Firmware Interface)?", "id": "Antarmuka Firmware Ekstensibel Terpadu." },
  { "en": "Apa Itu Bootloader (Pemuat Boot)?", "id": "Program Pemuat Sistem Operasi." },
  { "en": "Apa Itu Interupsi (Interrupt)?", "id": "Sinyal Penjedaan Program Utama CPU." },
  { "en": "Apa Kepanjangan ISR (Interrupt Service Routine)?", "id": "Rutin Layanan Interupsi." },
  { "en": "Apa Kepanjangan DMA (Direct Memory Access)?", "id": "Akses Memori Langsung." },
  { "en": "Apa Fungsi DMA (Direct Memory Access)?", "id": "Transfer Data Memori Tanpa CPU." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Timer Pencegah Sistem Macet (Hang)." },
  { "en": "Apa Itu Logika Fuzzy (Fuzzy Logic)?", "id": "Logika Berbasis Derajat Kebenaran (Bukan 0/1)." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (Artificial Neural Network)?", "id": "Model Komputasi Terinspirasi Otak Biologis." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning)?", "id": "Cabang AI (Artificial Intelligence) Sistem Belajar Data." },
  { "en": "Apa Kepanjangan AI (Artificial Intelligence)?", "id": "Kecerdasan Buatan." },
  { "en": "Apa Itu Sistem Embedded (Embedded System)?", "id": "Sistem Komputer Tertanam Fungsi Spesifik." },
  { "en": "Apa Kepanjangan IoT (Internet of Things)?", "id": "Internet Untuk Segala." },
  { "en": "Apa Itu Bluetooth (Bluetooth)?", "id": "Standar Komunikasi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan Nirkabel Lokal (WLAN)." },
  { "en": "Apa Kepanjangan WLAN (Wireless Local Area Network)?", "id": "Jaringan Area Lokal Nirkabel." },
  { "en": "Apa Kepanjangan LAN (Local Area Network)?", "id": "Jaringan Area Lokal." },
  { "en": "Apa Kepanjangan WAN (Wide Area Network)?", "id": "Jaringan Area Luas." },
  { "en": "Apa Itu Topologi Jaringan (Network Topology)?", "id": "Susunan Fisik Atau Logis Jaringan." },
  { "en": "Sebutkan Contoh Topologi Jaringan (Network Topology)?", "id": "Bus, Bintang (Star), Cincin (Ring)." },
  { "en": "Apa Itu Topologi Bintang (Star Topology)?", "id": "Semua Perangkat Terhubung Ke Hub Pusat." },
  { "en": "Apa Itu Hub (Hub) Jaringan?", "id": "Perangkat Penghubung Jaringan (Lapisan 1)." },
  { "en": "Apa Itu Switch (Switch) Jaringan?", "id": "Perangkat Penghubung Cerdas (Lapisan 2)." },
  { "en": "Apa Itu Router (Router) Jaringan?", "id": "Perangkat Penghubung Jaringan Berbeda (Lapisan 3)." },
  { "en": "Apa Kepanjangan Model OSI (Open Systems Interconnection)?", "id": "Model Referensi Jaringan Tujuh Lapis." },
  { "en": "Apa Lapisan Pertama (Layer 1) Model OSI?", "id": "Lapisan Fisik (Physical Layer)." },
  { "en": "Apa Lapisan Kedua (Layer 2) Model OSI?", "id": "Lapisan Taut Data (Data Link Layer)." },
  { "en": "Apa Lapisan Ketiga (Layer 3) Model OSI?", "id": "Lapisan Jaringan (Network Layer)." },
  { "en": "Apa Lapisan Keempat (Layer 4) Model OSI?", "id": "Lapisan Transport (Transport Layer)." },
  { "en": "Apa Lapisan Ketujuh (Layer 7) Model OSI?", "id": "Lapisan Aplikasi (Application Layer)." },
  { "en": "Apa Kepanjangan TCP/IP (Transmission Control Protocol/Internet Protocol)?", "id": "Protokol Kontrol Transmisi/Protokol Internet." },
  { "en": "Apa Kepanjangan TCP (Transmission Control Protocol)?", "id": "Protokol Kontrol Transmisi (Connection-Oriented)." },
  { "en": "Apa Kepanjangan UDP (User Datagram Protocol)?", "id": "Protokol Datagram Pengguna (Connectionless)." },
  { "en": "Apa Kepanjangan IP (Internet Protocol)?", "id": "Protokol Internet (Pengalamatan Logis)." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Alamat Fisik Perangkat Keras Jaringan." },
  { "en": "Apa Kepanjangan MAC (Media Access Control)?", "id": "Kontrol Akses Media." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Kepanjangan DHCP (Dynamic Host Configuration Protocol)?", "id": "Protokol Konfigurasi Host Dinamis." },
  { "en": "Apa Fungsi DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberikan Alamat IP Secara Otomatis." },
  { "en": "Apa Kepanjangan DNS (Domain Name System)?", "id": "Sistem Penamaan Domain." },
  { "en": "Apa Fungsi DNS (Domain Name System)?", "id": "Menerjemahkan Nama Domain Ke Alamat IP." },
  { "en": "Apa Kepanjangan HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Transfer Hiperteks." },
  { "en": "Apa Kepanjangan HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Protokol Transfer Hiperteks Aman." },
  { "en": "Apa Kepanjangan FTP (File Transfer Protocol)?", "id": "Protokol Transfer Berkas." },
  { "en": "Apa Kepanjangan SMTP (Simple Mail Transfer Protocol)?", "id": "Protokol Transfer Surat Sederhana." },
  { "en": "Apa Kepanjangan POP3 (Post Office Protocol 3)?", "id": "Protokol Kantor Pos Versi 3." },
  { "en": "Apa Kepanjangan IMAP (Internet Message Access Protocol)?", "id": "Protokol Akses Pesan Internet." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Sistem Keamanan Jaringan (Filter Lalu Lintas)." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Jaringan Pribadi Virtual." },
  { "en": "Apa Fungsi VPN (Virtual Private Network)?", "id": "Membuat Koneksi Jaringan Aman, Terenkripsi." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Proses Mengamankan Data (Mengubah Ke Kode)." },
  { "en": "Apa Itu Dekripsi (Decryption)?", "id": "Proses Membuka Data Terenkripsi." },
  { "en": "Apa Itu Kunci Simetris (Symmetric Key) Enkripsi?", "id": "Satu Kunci Sama Untuk Enkripsi, Dekripsi." },
  { "en": "Apa Itu Kunci Asimetris (Asymmetric Key) Enkripsi?", "id": "Dua Kunci Berbeda (Publik, Privat)." },
  { "en": "Apa Kepanjangan SSL (Secure Sockets Layer)?", "id": "Lapisan Soket Aman." },
  { "en": "Apa Kepanjangan TLS (Transport Layer Security)?", "id": "Keamanan Lapisan Transportasi (Pengganti SSL)." },
  { "en": "Apa Itu Permitivitas (Permittivity) (Epsilon)?", "id": "Kemampuan Bahan Bentuk Medan Listrik." },
  { "en": "Apa Itu Permeabilitas (Permeability) (Mu)?", "id": "Kemampuan Bahan Bentuk Medan Magnet." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant)?", "id": "Rasio Permitivitas Bahan Terhadap Vakum." },
  { "en": "Apa Itu Reluktansi (Reluctance)?", "id": "Hambatan Terhadap Fluks Magnetik." },
  { "en": "Apa Itu Segitiga Daya (Power Triangle)?", "id": "Hubungan Grafis Daya Nyata, Reaktif, Semu." },
  { "en": "Apa Sisi Alas Segitiga Daya?", "id": "Daya Nyata (P) Dalam Watt." },
  { "en": "Apa Sisi Tegak Segitiga Daya?", "id": "Daya Reaktif (Q) Dalam VAR." },
  { "en": "Apa Sisi Miring Segitiga Daya?", "id": "Daya Semu (S) Dalam VA." },
  { "en": "Apa Rumus Faktor Daya (Power Factor) Dari Segitiga Daya?", "id": "PF = P Dibagi S (P/S)." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Ukuran Efisiensi Energi Osilator." },
  { "en": "Apa Itu Grounding (Pembumian) Sistem?", "id": "Pembumian Titik Netral (Proteksi Peralatan)." },
  { "en": "Apa Itu Grounding (Pembumian) Pengaman?", "id": "Pembumian Bodi Peralatan (Proteksi Manusia)." },
  { "en": "Apa Itu Ground Loop (Loop Tanah)?", "id": "Dua Titik Ground Beda Potensial." },
  { "en": "Apa Masalah Ground Loop (Loop Tanah)?", "id": "Menyebabkan Gangguan (Noise) Hum." },
  { "en": "Bagaimana Solusi Ground Loop (Loop Tanah)?", "id": "Menggunakan Grounding Bintang (Star Grounding)." },
  { "en": "Apa Itu Pelindung (Shielding) Kabel?", "id": "Lapisan Konduktif Melindungi Sinyal." },
  { "en": "Apa Fungsi Pelindung (Shielding) Kabel?", "id": "Melindungi Dari Interferensi EMI, RFI." },
  { "en": "Apa Kepanjangan RFI (Radio Frequency Interference)?", "id": "Interferensi Frekuensi Radio." },
  { "en": "Apa Itu Derau Termal (Thermal Noise)?", "id": "Noise Akibat Gerakan Acak Elektron." },
  { "en": "Apa Nama Lain Derau Termal (Thermal Noise)?", "id": "Noise Johnson-Nyquist." },
  { "en": "Apa Itu Derau Tembak (Shot Noise)?", "id": "Noise Akibat Sifat Diskrit Muatan." },
  { "en": "Apa Itu Derau Flicker (Flicker Noise) (1/f)?", "id": "Noise Frekuensi Rendah (Pink Noise)." },
  { "en": "Apa Itu Filter Butterworth (Butterworth Filter)?", "id": "Filter Respon Datar (Maximally Flat)." },
  { "en": "Apa Itu Filter Chebyshev (Chebyshev Filter)?", "id": "Filter Transisi Curam (Ripple Passband)." },
  { "en": "Apa Itu Filter Bessel (Bessel Filter)?", "id": "Filter Respon Fasa Linear (Delay Rata)." },
  { "en": "Apa Itu Filter Eliptik (Elliptic Filter)?", "id": "Filter Sangat Curam (Ripple Passband, Stopband)." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman (Roll-Off) Filter." },
  { "en": "Apa Itu Tegangan Offset Input (Input Offset Voltage) Op-Amp?", "id": "Beda Tegangan Input Agar Output Nol." },
  { "en": "Apa Itu Arus Bias Input (Input Bias Current) Op-Amp?", "id": "Arus DC Rata-Rata Masuk Input." },
  { "en": "Apa Itu Arus Offset Input (Input Offset Current) Op-Amp?", "id": "Selisih Arus Bias Kedua Input." },
  { "en": "Apa Itu Gain Bandwidth Product (GBP) Op-Amp?", "id": "Hasil Kali Penguatan (Gain) Dan Bandwidth." },
  { "en": "Apa Kepanjangan GBP (Gain Bandwidth Product)?", "id": "Produk Penguatan Lebar Pita." },
  { "en": "Apa Itu Konverter Cuk (Cuk Converter)?", "id": "Konverter DC-DC (Penaik/Penurun) Output Negatif." },
  { "en": "Apa Itu Konverter SEPIC (SEPIC Converter)?", "id": "Konverter DC-DC (Penaik/Penurun) Output Positif." },
  { "en": "Apa Kepanjangan SEPIC (Single-Ended Primary-Inductor Converter)?", "id": "Konverter Induktor Primer Ujung Tunggal." },
  { "en": "Apa Itu Refleksi (Reflection) Sinyal Saluran Transmisi?", "id": "Sinyal Memantul Akibat Mismatch Impedansi." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyamakan Impedansi Sumber, Saluran, Beban." },
  { "en": "Mengapa Pencocokan Impedansi (Impedance Matching) Penting?", "id": "Maksimalkan Transfer Daya, Hindari Refleksi." },
  { "en": "Apa Itu Stub (Stub) Saluran Transmisi?", "id": "Saluran Transmisi Pendek Untuk Matching." },
  { "en": "Apa Itu Lebar Pita (Beamwidth) Antena?", "id": "Sudut Pancaran Utama Sinyal Antena." },
  { "en": "Apa Itu Direktifitas (Directivity) Antena?", "id": "Kemampuan Antena Arahkan Energi." },
  { "en": "Apa Itu Efisiensi (Efficiency) Antena?", "id": "Rasio Daya Radiasi Terhadap Daya Input." },
  { "en": "Apa Itu Rasio Depan-Belakang (Front-to-Back Ratio) Antena?", "id": "Rasio Penguatan Arah Depan, Belakang." },
  { "en": "Apa Itu Antena Loop (Loop Antenna)?", "id": "Antena Berbentuk Lingkaran Kawat." },
  { "en": "Apa Itu Antena Parabola (Parabolic Antenna)?", "id": "Antena Reflektor Bentuk Parabola (Direktif)." },
  { "en": "Apa Itu Umpan (Feed) Antena Parabola?", "id": "Elemen Penerima/Pemancar Di Titik Fokus." },
  { "en": "Apa Itu Error Sistematik (Systematic Error)?", "id": "Kesalahan Konsisten, Dapat Diprediksi (Kalibrasi)." },
  { "en": "Apa Itu Error Acak (Random Error)?", "id": "Kesalahan Tidak Dapat Diprediksi (Noise)." },
  { "en": "Apa Itu Error Kotor (Gross Error)?", "id": "Kesalahan Akibat Kecerobohan Manusia." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Proses Penyetelan Alat Ukur Sesuai Standar." },
  { "en": "Apa Itu Ketidakpastian (Uncertainty) Pengukuran?", "id": "Rentang Keraguan Nilai Hasil Ukur." },
  { "en": "Apa Itu Standar Primer (Primary Standard)?", "id": "Standar Referensi Tertinggi, Paling Akurat." },
  { "en": "Apa Itu Standar Sekunder (Secondary Standard)?", "id": "Standar Dikalibrasi Terhadap Standar Primer." },
  { "en": "Apa Itu Standar Kerja (Working Standard)?", "id": "Standar Digunakan Laboratorium Harian." },
  { "en": "Apa Itu Loading Effect (Efek Pembebanan) Voltmeter?", "id": "Voltmeter Mengubah Tegangan Yang Diukur." },
  { "en": "Kapan Loading Effect (Efek Pembebanan) Voltmeter Signifikan?", "id": "Saat Impedansi Voltmeter Rendah." },
  { "en": "Apa Itu Decibel-Milliwatt (dBm)?", "id": "Satuan Daya Decibel Relatif 1 Milliwatt." },
  { "en": "Apa Itu Decibel-Watt (dBW)?", "id": "Satuan Daya Decibel Relatif 1 Watt." },
  { "en": "Berapa 0 dBm (Decibel-Milliwatt)?", "id": "Sama Dengan 1 Milliwatt (mW)." },
  { "en": "Apa Itu Solder (Soldering)?", "id": "Proses Penyambungan Logam Menggunakan Timah." },
  { "en": "Apa Itu Desoldering (Desoldering)?", "id": "Proses Pelepasan Komponen Yang Disolder." },
  { "en": "Apa Itu Solder Wick (Solder Wick)?", "id": "Serabut Tembaga Penyerap Timah Sisa." },
  { "en": "Apa Itu Solder Sucker (Penyedot Timah)?", "id": "Alat Pompa Vakum Menyedot Timah Cair." },
  { "en": "Apa Itu Flux (Fluks) Solder?", "id": "Bahan Kimia Pembersih Oksida Logam." },
  { "en": "Apa Itu Cold Solder Joint (Solder Dingin)?", "id": "Hasil Solderan Buruk (Tidak Mengkilap)." },
  { "en": "Apa Penyebab Cold Solder Joint (Solder Dingin)?", "id": "Pemanasan Kurang Atau Komponen Bergerak." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Single-Layer?", "id": "Papan Sirkuit Tercetak Satu Lapis Tembaga." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Double-Layer?", "id": "Papan Sirkuit Tercetak Dua Lapis Tembaga." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Multi-Layer?", "id": "Papan Sirkuit Tercetak Banyak Lapis Tembaga." },
  { "en": "Apa Itu Via (Via) Pada PCB (Printed Circuit Board)?", "id": "Lubang Konduktif Penghubung Antar Lapis." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan Pelindung (Hijau) Di Atas PCB." },
  { "en": "Apa Fungsi Solder Mask (Masker Solder)?", "id": "Mencegah Hubungan Singkat Saat Solder." },
  { "en": "Apa Itu Silkscreen (Sablon Sutra)?", "id": "Lapisan Teks Putih (Label Komponen) PCB." },
  { "en": "Apa Itu Sinyal Jamak (Multiplexed Signal)?", "id": "Banyak Sinyal Dibagi Satu Saluran." },
  { "en": "Apa Itu Sinyal Diferensial (Differential Signaling)?", "id": "Transmisi Sinyal Gunakan Dua Kawat (Positif, Negatif)." },
  { "en": "Apa Keuntungan Sinyal Diferensial (Differential Signaling)?", "id": "Sangat Kebal Terhadap Noise (CMRR)." },
  { "en": "Sebutkan Contoh Sinyal Diferensial (Differential Signaling)?", "id": "USB (Universal Serial Bus), Ethernet, RS-485." },
  { "en": "Apa Itu Sinyal Single-Ended (Single-Ended Signal)?", "id": "Transmisi Sinyal Relatif Terhadap Ground." },
  { "en": "Apa Itu Slew Rate (Laju Perubahan)?", "id": "Laju Perubahan Tegangan Maksimal Output." },
  { "en": "Apa Satuan Slew Rate (Laju Perubahan) Op-Amp?", "id": "Volt Per Mikrodetik (V/µs)." },
  { "en": "Apa Akibat Slew Rate (Laju Perubahan) Terbatas?", "id": "Distorsi Sinyal Output Frekuensi Tinggi." },
  { "en": "Apa Itu Osilator Pierce (Pierce Oscillator)?", "id": "Osilator Kristal Menggunakan Inverter." },
  { "en": "Apa Itu Bus Address Latch Enable (ALE)?", "id": "Sinyal Pemisah Alamat, Data Bus Jamak." },
  { "en": "Dimana Sinyal ALE (Address Latch Enable) Digunakan?", "id": "Mikroprosesor (Contoh: 8085, 8051)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) N-Channel?", "id": "Transistor MOSFET (Metal-Oxide-Semiconductor FET) Kanal N." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) P-Channel?", "id": "Transistor MOSFET (Metal-Oxide-Semiconductor FET) Kanal P." },
  { "en": "Mode Apa Yang Paling Umum Untuk MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Mode Peningkatan (Enhancement Mode)." },
  { "en": "Apa Itu Daerah Cut-Off (Cut-Off Region) MOSFET?", "id": "Transistor Mati (Off), Tidak Ada Arus." },
  { "en": "Apa Itu Daerah Triode (Triode Region) MOSFET?", "id": "Bertindak Seperti Resistor Terkontrol Tegangan." },
  { "en": "Apa Nama Lain Daerah Triode (Triode Region) MOSFET?", "id": "Daerah Linear (Linear Region)." },
  { "en": "Apa Itu Daerah Saturasi (Saturation Region) MOSFET?", "id": "Transistor Aktif, Arus Konstan (Penguat)." },
  { "en": "Apa Fungsi Utama JFET (Junction Field Effect Transistor)?", "id": "Penguat Sinyal, Saklar Impedansi Tinggi." },
  { "en": "Apa Itu Tegangan Pinch-Off (Pinch-Off Voltage) JFET?", "id": "Tegangan Gate Mematikan Arus Drain." },
  { "en": "Apa Itu Garis Beban (Load Line) AC Penguat?", "id": "Garis Kerja Penguat Sinyal AC." },
  { "en": "Apa Itu Respons Frekuensi Rendah (Low-Frequency Response)?", "id": "Pengaruh Kapasitor Kopling, Bypass." },
  { "en": "Apa Itu Respons Frekuensi Tinggi (High-Frequency Response)?", "id": "Pengaruh Kapasitansi Internal Transistor." },
  { "en": "Apa Itu Penguat Umpan Balik (Feedback Amplifier)?", "id": "Penguat Dengan Sebagian Output Kembali Input." },
  { "en": "Apa Itu Umpan Balik Positif (Positive Feedback)?", "id": "Sinyal Umpan Balik Memperkuat Input." },
  { "en": "Dimana Umpan Balik Positif (Positive Feedback) Digunakan?", "id": "Rangkaian Osilator, Schmitt Trigger." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Sinyal Umpan Balik Melawan Input." },
  { "en": "Apa Manfaat Umpan Balik Negatif (Negative Feedback)?", "id": "Stabilitas, Kurangi Distorsi, Atur Gain." },
  { "en": "Sebutkan Empat Topologi Umpan Balik (Feedback Topology)?", "id": "Seri-Shunt, Shunt-Seri, Seri-Seri, Shunt-Shunt." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Antara Dua Input." },
  { "en": "Apa Itu Sinyal Mode Bersama (Common-Mode Signal)?", "id": "Sinyal Yang Sama Di Kedua Input." },
  { "en": "Apa Itu Sinyal Mode Diferensial (Differential-Mode Signal)?", "id": "Sinyal Yang Berbeda Di Kedua Input." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Rangkaian Menyalin Arus Referensi." },
  { "en": "Apa Itu Beban Aktif (Active Load)?", "id": "Mengganti Resistor Beban Dengan Transistor." },
  { "en": "Apa Keuntungan Beban Aktif (Active Load)?", "id": "Mendapatkan Penguatan Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Rangkaian Cascode (Cascode Amplifier)?", "id": "Gabungan Common Emitter, Common Base." },
  { "en": "Apa Keuntungan Rangkaian Cascode (Cascode Amplifier)?", "id": "Bandwidth (Lebar Pita) Sangat Lebar." },
  { "en": "Apa Itu Resistansi Negatif (Negative Resistance)?", "id": "Karakteristik Arus Turun Saat Tegangan Naik." },
  { "en": "Komponen Apa Punya Resistansi Negatif (Negative Resistance)?", "id": "Dioda Tunnel, UJT (Unijunction Transistor)." },
  { "en": "Apa Itu Rangkaian Pelipat Tegangan (Voltage Multiplier)?", "id": "Menghasilkan Tegangan DC Kelipatan AC." },
  { "en": "Apa Itu Rangkaian Pengganda Tegangan (Voltage Doubler)?", "id": "Menghasilkan Tegangan DC Dua Kali Puncak." },
  { "en": "Apa Itu Rangkaian Penjepit (Clamper Circuit)?", "id": "Rangkaian Penggeser Level DC Sinyal." },
  { "en": "Apa Nama Lain Rangkaian Penjepit (Clamper Circuit)?", "id": "Pemulih DC (DC Restorer)." },
  { "en": "Apa Itu Rangkaian Pemotong (Clipper Circuit)?", "id": "Rangkaian Pemotong Sebagian Sinyal." },
  { "en": "Apa Nama Lain Rangkaian Pemotong (Clipper Circuit)?", "id": "Limiter (Pembatas)." },
  { "en": "Apa Itu Rangkaian Saklar Analog (Analog Switch)?", "id": "Saklar Elektronik Melewatkan Sinyal Analog." },
  { "en": "Contoh Komponen Saklar Analog (Analog Switch)?", "id": "JFET (Junction Field Effect Transistor), MOSFET (Metal-Oxide-Semiconductor FET), IC (Integrated Circuit) 4066." },
  { "en": "Apa Itu Rangkaian Sample and Hold (S&H)?", "id": "Mencuplik, Menahan Tegangan Sinyal Analog." },
  { "en": "Dimana Rangkaian Sample and Hold (S&H) Digunakan?", "id": "Pada Masukan ADC (Analog-to-Digital Converter)." },
  { "en": "Apa Itu Resolusi (Resolution) ADC (Analog-to-Digital Converter)?", "id": "Jumlah Bit Output Digital." },
  { "en": "Apa Itu Flash ADC (Flash Analog-to-Digital Converter)?", "id": "Jenis ADC (Analog-to-Digital Converter) Paling Cepat." },
  { "en": "Apa Itu Successive Approximation ADC (SAR ADC)?", "id": "Jenis ADC (Analog-to-Digital Converter) Paling Umum." },
  { "en": "Apa Itu Dual-Slope ADC (Dual-Slope Analog-to-Digital Converter)?", "id": "Jenis ADC (Analog-to-Digital Converter) Lambat, Akurasi Tinggi." },
  { "en": "Apa Itu Sigma-Delta ADC (Sigma-Delta Analog-to-Digital Converter)?", "id": "Jenis ADC (Analog-to-Digital Converter) Resolusi Sangat Tinggi." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) R-2R Ladder?", "id": "DAC (Digital-to-Analog Converter) Menggunakan Jaringan Resistor R-2R." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) Weighted Resistor?", "id": "DAC (Digital-to-Analog Converter) Resistor Terbobot Biner." },
  { "en": "Apa Itu Monotonisitas (Monotonicity) DAC?", "id": "Output Selalu Naik Sesuai Input." },
  { "en": "Apa Itu Linearitas (Linearity) ADC (Analog-to-Digital Converter) / DAC (Digital-to-Analog Converter)?", "id": "Ukuran Kedekatan Respon Terhadap Garis Lurus." },
  { "en": "Apa Itu Sistem Bilangan Heksadesimal (Hexadecimal)?", "id": "Sistem Bilangan Basis 16." },
  { "en": "Simbol Apa Yang Digunakan Heksadesimal (Hexadecimal)?", "id": "Angka 0-9 Dan Huruf A-F." },
  { "en": "Apa Itu Kode ASCII (American Standard Code for Information Interchange)?", "id": "Standar Pengkodean Karakter Teks." },
  { "en": "Apa Itu Gerbang Logika Universal (Universal Logic Gate)?", "id": "Gerbang Pembentuk Semua Gerbang Lain." },
  { "en": "Gerbang Apa Yang Termasuk Gerbang Universal?", "id": "NAND (Not AND) Dan NOR (Not OR)." },
  { "en": "Apa Itu Propagasi Tunda (Propagation Delay) Gerbang Logika?", "id": "Waktu Tunda Input Berubah Ke Output." },
  { "en": "Apa Itu Disipasi Daya (Power Dissipation) Gerbang Logika?", "id": "Konsumsi Daya Rata-Rata Gerbang." },
  { "en": "Apa Itu Logika Positif (Positive Logic)?", "id": "Level Tinggi Adalah 1, Rendah 0." },
  { "en": "Apa Itu Logika Negatif (Negative Logic)?", "id": "Level Tinggi Adalah 0, Rendah 1." },
  { "en": "Apa Itu Buffer (Penyangga) Logika?", "id": "Penguat Arus (Driver), Non-Inverting." },
  { "en": "Apa Itu Buffer Tri-State (Tri-State Buffer)?", "id": "Buffer Dengan Tiga Keadaan Output." },
  { "en": "Apa Tiga Keadaan Output Tri-State?", "id": "Tinggi (High), Rendah (Low), Impedansi Tinggi (Hi-Z)." },
  { "en": "Apa Fungsi Keadaan Impedansi Tinggi (Hi-Z)?", "id": "Melepas Output Dari Bus (Jalur)." },
  { "en": "Apa Itu Open Collector (Kolektor Terbuka) Output?", "id": "Output Transistor Tanpa Resistor Pull-Up." },
  { "en": "Apa Keuntungan Output Open Collector (Kolektor Terbuka)?", "id": "Dapat Dihubungkan Bersama (Wired-AND)." },
  { "en": "Apa Itu Open Drain (Drain Terbuka) Output?", "id": "Sama Seperti Open Collector (Kolektor Terbuka), Tapi MOSFET." },
  { "en": "Apa Itu Resistor Pull-Up (Pull-Up Resistor)?", "id": "Resistor Ke VCC (Tegangan Positif) Jaga Level Tinggi." },
  { "en": "Apa Itu Resistor Pull-Down (Pull-Down Resistor)?", "id": "Resistor Ke Ground Jaga Level Rendah." },
  { "en": "Apa Itu Rangkaian Aritmetika (Arithmetic Circuit)?", "id": "Rangkaian Operasi Matematika." },
  { "en": "Apa Itu Subtractor (Pengurang) Digital?", "id": "Rangkaian Pengurang Bilangan Biner." },
  { "en": "Bagaimana Membuat Subtractor (Pengurang) Menggunakan Adder (Penjumlah)?", "id": "Menggunakan Metode Komplemen Dua." },
  { "en": "Apa Itu Magnitude Comparator (Pembanding Besaran)?", "id": "Rangkaian Pembanding Dua Bilangan Biner." },
  { "en": "Apa Itu Clock Skew (Kemiringan Clock)?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu Clock Jitter (Gemetar Clock)?", "id": "Variasi Periode Sinyal Clock." },
  { "en": "Apa Itu Counter (Pencacah) Modulus?", "id": "Jumlah Keadaan (State) Counter." },
  { "en": "Apa Itu Counter (Pencacah) Mod-10 (MOD-10)?", "id": "Pencacah Dekade (0 Sampai 9)." },
  { "en": "Apa Itu Counter (Pencacah) BCD (Binary Coded Decimal)?", "id": "Nama Lain Counter (Pencacah) Mod-10." },
  { "en": "Apa Itu Ring Counter (Pencacah Cincin)?", "id": "Register Geser Dengan Output Kembali Input." },
  { "en": "Apa Itu Johnson Counter (Pencacah Johnson)?", "id": "Ring Counter (Pencacah Cincin) Dengan Output Terbalik." },
  { "en": "Apa Itu Dekoder (Decoder) 7-Segment?", "id": "Mengubah Kode BCD (Binary Coded Decimal) Ke Tampilan 7-Segment." },
  { "en": "Apa Itu Tampilan 7-Segment (7-Segment Display)?", "id": "Tampilan LED (Light Emitting Diode) Untuk Angka." },
  { "en": "Apa Itu Common Anode (Anoda Bersama) 7-Segment?", "id": "Semua Anoda LED Terhubung Bersama (Ke VCC)." },
  { "en": "Apa Itu Common Cathode (Katoda Bersama) 7-Segment?", "id": "Semua Katoda LED Terhubung Bersama (Ke Ground)." },
  { "en": "Apa Itu Arsitektur Memori (Memory Architecture)?", "id": "Organisasi Sel Memori." },
  { "en": "Apa Itu Sel Memori (Memory Cell)?", "id": "Unit Dasar Penyimpan Satu Bit." },
  { "en": "Apa Itu Waktu Akses (Access Time) Memori?", "id": "Waktu Baca Data Dari Memori." },
  { "en": "Apa Itu Waktu Siklus (Cycle Time) Memori?", "id": "Waktu Total Operasi Memori (Baca/Tulis)." },
  { "en": "Apa Kepanjangan SDRAM (Synchronous DRAM)?", "id": "DRAM (Dynamic RAM) Sinkron (Mengikuti Clock)." },
  { "en": "Apa Kepanjangan DDR (Double Data Rate) SDRAM?", "id": "SDRAM (Synchronous DRAM) Transfer Data Dua Kali." },
  { "en": "Apa Kepanjangan NVRAM (Non-Volatile RAM)?", "id": "RAM (Random Access Memory) Non-Volatil (Baterai Cadangan)." },
  { "en": "Apa Itu Memori FIFO (First-In First-Out)?", "id": "Memori Antrian (Data Masuk Pertama, Keluar Pertama)." },
  { "en": "Apa Itu Memori LIFO (Last-In First-Out)?", "id": "Memori Tumpukan (Stack) (Data Masuk Terakhir, Keluar Pertama)." },
  { "en": "Apa Nama Lain Memori LIFO (Last-In First-Out)?", "id": "Stack (Tumpukan)." },
  { "en": "Apa Itu Bahasa Assembly (Assembly Language)?", "id": "Bahasa Pemrograman Level Rendah (Mnemonik)." },
  { "en": "Apa Itu Kompilator (Compiler)?", "id": "Penerjemah Bahasa Tinggi Ke Mesin." },
  { "en": "Apa Itu Interpreter (Interpreter)?", "id": "Penerjemah Bahasa Baris Per Baris." },
  { "en": "Apa Itu Assembler (Perakit)?", "id": "Penerjemah Bahasa Assembly Ke Mesin." },
  { "en": "Apa Itu Debugger (Debugger)?", "id": "Alat Bantu Mencari Kesalahan (Bug) Program." },
  { "en": "Apa Itu Diagram Waktu (Timing Diagram)?", "id": "Grafik Sinyal Digital Terhadap Waktu." },
  { "en": "Apa Itu Mesin Keadaan (State Machine)?", "id": "Model Komputasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Diagram Keadaan (State Diagram)?", "id": "Representasi Grafis Mesin Keadaan." },
  { "en": "Apa Itu Keadaan (State) Dalam Mesin Keadaan?", "id": "Kondisi Operasi Sistem Saat Itu." },
  { "en": "Apa Itu Transisi (Transition) Mesin Keadaan?", "id": "Perpindahan Antar Keadaan (State)." },
  { "en": "Apa Itu Mesin Moore (Moore Machine)?", "id": "Output Mesin Keadaan Tergantung Keadaan (State) Saja." },
  { "en": "Apa Itu Mesin Mealy (Mealy Machine)?", "id": "Output Mesin Keadaan Tergantung Keadaan, Input." },
  { "en": "Apa Kepanjangan PLTMH (Pembangkit Listrik Tenaga Mikrohidro)?", "id": "Pembangkit Listrik Tenaga Mikrohidro." },
  { "en": "Apa Sumber Energi PLTMH (Pembangkit Listrik Tenaga Mikrohidro)?", "id": "Aliran Air Skala Kecil (Sungai)." },
  { "en": "Apa Kepanjangan VHDL (VHSIC Hardware Description Language)?", "id": "Bahasa Deskripsi Perangkat Keras VHSIC." },
  { "en": "Apa Kepanjangan Verilog (Verilog Hardware Description Language)?", "id": "Bahasa Deskripsi Perangkat Keras Verilog." },
  { "en": "Apa Fungsi VHDL (VHSIC Hardware Description Language) / Verilog?", "id": "Mendesain Sirkuit Logika Digital." },
  { "en": "Apa Kepanjangan FPGA (Field Programmable Gate Array)?", "id": "Array Gerbang Terprogram Lapangan." },
  { "en": "Apa Kepanjangan ASIC (Application-Specific Integrated Circuit)?", "id": "Sirkuit Terpadu Aplikasi Spesifik." },
  { "en": "Apa Beda FPGA (Field Programmable Gate Array) Dan ASIC (Application-Specific Integrated Circuit)?", "id": "FPGA (Field Programmable Gate Array) Dapat Diprogram Ulang." },
  { "en": "Apa Itu Sintesis (Synthesis) Dalam Desain Digital?", "id": "Proses Terjemah Kode HDL Ke Gerbang." },
  { "en": "Apa Itu Simulasi (Simulation) Dalam Desain Digital?", "id": "Verifikasi Fungsionalitas Desain Kode HDL." },
  { "en": "Apa Itu Rangkaian Logika Dapat Diprogram (PLD)?", "id": "Programmable Logic Device (PLD)." },
  { "en": "Apa Kepanjangan CPLD (Complex Programmable Logic Device)?", "id": "Perangkat Logika Terprogram Kompleks." },
  { "en": "Apa Itu Logika Kombinasional (Combinational Logic)?", "id": "Output Tergantung Input Saat Ini." },
  { "en": "Apa Itu Logika Sekuensial (Sequential Logic)?", "id": "Output Tergantung Input, Keadaan Terdahulu." },
  { "en": "Apa Elemen Dasar Logika Sekuensial (Sequential Logic)?", "id": "Elemen Memori (Flip-Flop)." },
  { "en": "Apa Itu Analisis State-Space (Ruang Keadaan)?", "id": "Metode Analisis Sistem Kontrol (Matriks)." },
  { "en": "Apa Itu Vektor Keadaan (State Vector) (x)?", "id": "Kumpulan Variabel Keadaan Sistem." },
  { "en": "Apa Itu Matriks Keadaan (State Matrix) (A)?", "id": "Matriks Dinamika Internal Sistem." },
  { "en": "Apa Itu Matriks Input (Input Matrix) (B)?", "id": "Matriks Hubungan Input Ke Keadaan." },
  { "en": "Apa Itu Matriks Output (Output Matrix) (C)?", "id": "Matriks Hubungan Keadaan Ke Output." },
  { "en": "Apa Itu Keterkendalian (Controllability) Sistem Kontrol?", "id": "Kemampuan Input Mengubah Keadaan Sistem." },
  { "en": "Apa Itu Keteramatan (Observability) Sistem Kontrol?", "id": "Kemampuan Output Tentukan Keadaan Internal." },
  { "en": "Apa Itu Kestabilan BIBO (Bounded-Input Bounded-Output)?", "id": "Input Terbatas Hasilkan Output Terbatas." },
  { "en": "Apa Itu Diagram Nyquist (Nyquist Plot)?", "id": "Plot Respon Frekuensi Domain Polar." },
  { "en": "Apa Itu Kriteria Kestabilan Nyquist (Nyquist Stability Criterion)?", "id": "Menentukan Kestabilan Sistem Loop Tertutup." },
  { "en": "Apa Itu Gain Margin (GM) (Batas Penguatan)?", "id": "Ukuran Kestabilan Relatif (Penguatan)." },
  { "en": "Apa Itu Phase Margin (PM) (Batas Fasa)?", "id": "Ukuran Kestabilan Relatif (Fasa)." },
  { "en": "Apa Itu Kompensator (Compensator) Sistem Kontrol?", "id": "Rangkaian Tambahan Perbaikan Respon Sistem." },
  { "en": "Apa Itu Kompensator Lead (Lead Compensator)?", "id": "Memperbaiki Respon Transien (Kecepatan)." },
  { "en": "Apa Itu Kompensator Lag (Lag Compensator)?", "id": "Memperbaiki Error Keadaan Tunak (Akurasi)." },
  { "en": "Apa Itu Kompensator Lead-Lag (Lead-Lag Compensator)?", "id": "Gabungan Keuntungan Lead Dan Lag." },
  { "en": "Apa Itu Logika Fuzzy (Fuzzy Logic)?", "id": "Logika Berbasis Derajat Kebenaran." },
  { "en": "Apa Itu Fuzzifikasi (Fuzzification)?", "id": "Proses Mengubah Input Crisp Ke Fuzzy." },
  { "en": "Apa Itu Defuzzifikasi (Defuzzification)?", "id": "Proses Mengubah Output Fuzzy Ke Crisp." },
  { "en": "Apa Itu Mesin Inferensi (Inference Engine) Fuzzy?", "id": "Pemroses Aturan Logika Fuzzy (IF-THEN)." },
  { "en": "Apa Kepanjangan ANN (Artificial Neural Network)?", "id": "Jaringan Saraf Tiruan." },
  { "en": "Apa Unit Dasar ANN (Artificial Neural Network)?", "id": "Neuron (Simpul Pemroses)." },
  { "en": "Apa Itu Pembelajaran Terbimbing (Supervised Learning)?", "id": "Belajar Dari Data Berlabel (Input-Output)." },
  { "en": "Apa Itu Pembelajaran Tak Terbimbing (Unsupervised Learning)?", "id": "Belajar Pola Dari Data Tak Berlabel." },
  { "en": "Apa Itu Algoritma Genetika (Genetic Algorithm)?", "id": "Algoritma Optimasi Terinspirasi Evolusi." },
  { "en": "Apa Kepanjangan DAQ (Data Acquisition System)?", "id": "Sistem Akuisisi Data." },
  { "en": "Apa Fungsi DAQ (Data Acquisition System)?", "id": "Mengumpulkan, Mengukur, Merekam Sinyal Fisik." },
  { "en": "Apa Kepanjangan SCADA (Supervisory Control and Data Acquisition)?", "id": "Kontrol Pengawasan Dan Akuisisi Data." },
  { "en": "Apa Fungsi SCADA (Supervisory Control and Data Acquisition)?", "id": "Memonitor, Mengontrol Proses Industri Besar." },
  { "en": "Apa Kepanjangan HMI (Human Machine Interface)?", "id": "Antarmuka Manusia Mesin." },
  { "en": "Apa Fungsi HMI (Human Machine Interface)?", "id": "Visualisasi, Kontrol Proses Oleh Operator." },
  { "en": "Apa Kepanjangan DCS (Distributed Control System)?", "id": "Sistem Kontrol Terdistribusi." },
  { "en": "Apa Beda SCADA (Supervisory Control and Data Acquisition) Dan DCS (Distributed Control System)?", "id": "SCADA (Supervisory Control and Data Acquisition) Berbasis Event, DCS (Distributed Control System) Proses." },
  { "en": "Apa Itu Bus Lapangan (Fieldbus)?", "id": "Jaringan Komunikasi Digital Industri." },
  { "en": "Sebutkan Contoh Protokol Bus Lapangan (Fieldbus)?", "id": "Modbus, Profibus, CANopen." },
  { "en": "Apa Kepanjangan Modbus (Modbus)?", "id": "Protokol Komunikasi Serial (Master-Slave)." },
  { "en": "Apa Kepanjangan Profibus (Process Field Bus)?", "id": "Bus Lapangan Standar Eropa (Siemens)." },
  { "en": "Apa Itu Protokol HART (HART Protocol)?", "id": "Protokol Komunikasi Sinyal Analog, Digital." },
  { "en": "Apa Kepanjangan HART (Highway Addressable Remote Transducer)?", "id": "Transduser Jarak Jauh Dapat Dialamati." },
  { "en": "Apa Keunikan Protokol HART (HART Protocol)?", "id": "Sinyal Digital Menumpang Sinyal 4-20mA." },
  { "en": "Apa Itu Sinyal Analog 4-20mA (4-20mA)?", "id": "Standar Sinyal Arus Transmisi Industri." },
  { "en": "Mengapa Menggunakan Arus (4-20mA) Bukan Tegangan?", "id": "Lebih Kebal Noise, Deteksi Kabel Putus." },
  { "en": "Apa Itu Smart Transmitter (Pemancar Cerdas)?", "id": "Sensor Dengan Komunikasi Digital (HART)." },
  { "en": "Apa Itu Kalibrator Loop (Loop Calibrator)?", "id": "Alat Uji, Kalibrasi Loop Arus 4-20mA." },
  { "en": "Apa Itu Pneumatik (Pneumatic)?", "id": "Sistem Berbasis Udara Bertekanan." },
  { "en": "Apa Itu Hidrolik (Hydraulic)?", "id": "Sistem Berbasis Cairan (Oli) Bertekanan." },
  { "en": "Apa Itu Katup Kontrol (Control Valve)?", "id": "Katup Pengatur Laju Aliran Fluida." },
  { "en": "Apa Itu Positioner (Positioner) Katup?", "id": "Alat Kontrol Posisi Katup Presisi." },
  { "en": "Apa Itu Sel Beban (Load Cell)?", "id": "Sensor Pengukur Berat Atau Gaya." },
  { "en": "Apa Prinsip Kerja Sel Beban (Load Cell)?", "id": "Menggunakan Strain Gauge (Pengukur Regangan)." },
  { "en": "Apa Itu Sensor pH (pH Sensor)?", "id": "Mengukur Tingkat Keasaman, Kebasaan Cairan." },
  { "en": "Apa Itu Sensor Konduktivitas (Conductivity Sensor)?", "id": "Mengukur Kemampuan Cairan Hantarkan Listrik." },
  { "en": "Apa Itu Sensor ORP (Oxidation-Reduction Potential)?", "id": "Mengukur Potensi Oksidasi-Reduksi Larutan." },
  { "en": "Apa Itu Termografi Inframerah (Infrared Thermography)?", "id": "Pengambilan Gambar Berbasis Radiasi Panas." },
  { "en": "Apa Fungsi Termografi Inframerah (Infrared Thermography) Elektro?", "id": "Mendeteksi Koneksi Longgar, Overheating." },
  { "en": "Apa Itu Analisis Getaran (Vibration Analysis)?", "id": "Mendiagnosis Kerusakan Mesin Berputar (Motor)." },
  { "en": "Apa Itu Keselamatan Fungsional (Functional Safety)?", "id": "Aspek Keselamatan Bergantung Sistem Kontrol." },
  { "en": "Apa Kepanjangan SIL (Safety Integrity Level)?", "id": "Tingkat Integritas Keselamatan." },
  { "en": "Apa Itu Peringkat SIL (Safety Integrity Level)?", "id": "Ukuran Keandalan Sistem Pengaman (1-4)." },
  { "en": "Apa Kepanjangan HAZOP (Hazard and Operability Study)?", "id": "Studi Bahaya Dan Operabilitas." },
  { "en": "Apa Kepanjangan LOPA (Layer of Protection Analysis)?", "id": "Analisis Lapisan Proteksi." },
  { "en": "Apa Kepanjangan ESD (Emergency Shutdown System)?", "id": "Sistem Rem Darurat." },
  { "en": "Apa Itu Sekring HRC (High Rupturing Capacity)?", "id": "Sekring Kapasitas Pemutusan Arus Tinggi." },
  { "en": "Apa Itu Kapasitas Pemutusan (Breaking Capacity)?", "id": "Arus Maksimal Dapat Diputus Aman." },
  { "en": "Apa Itu Arus Let-Through (Let-Through Current)?", "id": "Arus Maksimal Dilewatkan Sekring Saat Putus." },
  { "en": "Apa Itu Koordinasi Proteksi (Protection Coordination)?", "id": "Pengaturan Proteksi Selektif (Diskriminasi)." },
  { "en": "Apa Tujuan Koordinasi Proteksi (Protection Coordination)?", "id": "Memadamkan Area Gangguan Terkecil Saja." },
  { "en": "Apa Itu Kurva Waktu-Arus (Time-Current Curve) (TCC)?", "id": "Grafik Karakteristik Waktu Trip Proteksi." },
  { "en": "Apa Itu Kestabilan Sudut Rotor (Rotor Angle Stability)?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Kestabilan Tegangan (Voltage Stability)?", "id": "Kemampuan Sistem Jaga Tegangan Stabil." },
  { "en": "Apa Itu Kolaps Tegangan (Voltage Collapse)?", "id": "Penurunan Tegangan Drastis Tak Terkendali." },
  { "en": "Apa Itu Kompensasi Seri (Series Compensation)?", "id": "Memasang Kapasitor Seri Di Saluran Transmisi." },
  { "en": "Apa Fungsi Kompensasi Seri (Series Compensation)?", "id": "Mengurangi Reaktansi Saluran Transmisi." },
  { "en": "Apa Itu Kompensasi Shunt (Shunt Compensation)?", "id": "Memasang Reaktor Atau Kapasitor Paralel." },
  { "en": "Apa Fungsi Kompensasi Shunt (Shunt Compensation)?", "id": "Mengatur Tegangan, Daya Reaktif." },
  { "en": "Apa Itu Reaktor Shunt (Shunt Reactor)?", "id": "Induktor Paralel Menyerap Daya Reaktif (VAR)." },
  { "en": "Apa Itu Kapasitor Shunt (Shunt Capacitor)?", "id": "Kapasitor Paralel Menyuplai Daya Reaktif (VAR)." },
  { "en": "Apa Kepanjangan SVC (Static VAR Compensator)?", "id": "Kompensator VAR Statis." },
  { "en": "Apa Kepanjangan STATCOM (Static Synchronous Compensator)?", "id": "Kompensator Sinkron Statis." },
  { "en": "Apa Fungsi SVC (Static VAR Compensator) / STATCOM (Static Synchronous Compensator)?", "id": "Kontrol Daya Reaktif, Kestabilan Tegangan." },
  { "en": "Apa Itu Sistem Pembumian Solid (Solid Grounding)?", "id": "Netral Terhubung Langsung Ke Tanah." },
  { "en": "Apa Itu Sistem Pembumian Resistan (Resistance Grounding)?", "id": "Netral Dihubungkan Tanah Melalui Resistor." },
  { "en": "Apa Itu Sistem Pembumian Reaktans (Reactance Grounding)?", "id": "Netral Dihubungkan Tanah Melalui Reaktor." },
  { "en": "Apa Itu Sistem Floating (Floating System) (IT)?", "id": "Netral Tidak Dibumikan Sama Sekali." },
  { "en": "Apa Itu Arus Gangguan Tanah (Ground Fault Current)?", "id": "Arus Mengalir Ke Tanah Saat Gangguan." },
  { "en": "Apa Itu Sistem Tenaga Listrik Tiga Fasa?", "id": "Sistem Tiga Sumber Tegangan AC." },
  { "en": "Berapa Perbedaan Fasa Antar Tegangan Tiga Fasa?", "id": "120 Derajat Listrik." },
  { "en": "Apa Itu Urutan Fasa (Phase Sequence)?", "id": "Urutan Puncak Tegangan Fasa (R-S-T)." },
  { "en": "Apa Kepanjangan RST (RST) Fasa?", "id": "Simbol Fasa R, S, Dan T." },
  { "en": "Apa Itu Beban Seimbang (Balanced Load) Tiga Fasa?", "id": "Impedansi Ketiga Fasa Sama Besar." },
  { "en": "Apa Itu Beban Tidak Seimbang (Unbalanced Load) Tiga Fasa?", "id": "Impedansi Ketiga Fasa Tidak Sama." },
  { "en": "Apa Arus Di Kawat Netral Beban Seimbang?", "id": "Arus Netral Idealnya Nol Ampere." },
  { "en": "Apa Fungsi Kawat Netral Beban Tidak Seimbang?", "id": "Mengalirkan Arus Ketidakseimbangan." },
  { "en": "Bagaimana Hubungan Tegangan Saluran, Fasa (Koneksi Bintang)?", "id": "V Saluran = √3 x V Fasa." },
  { "en": "Bagaimana Hubungan Arus Saluran, Fasa (Koneksi Bintang)?", "id": "I Saluran = I Fasa." },
  { "en": "Bagaimana Hubungan Tegangan Saluran, Fasa (Koneksi Delta)?", "id": "V Saluran = V Fasa." },
  { "en": "Bagaimana Hubungan Arus Saluran, Fasa (Koneksi Delta)?", "id": "I Saluran = √3 x I Fasa." },
  { "en": "Apa Rumus Daya Nyata (P) Tiga Fasa?", "id": "P = √3 x Vl x Il x Cos(θ)." },
  { "en": "Apa Rumus Daya Reaktif (Q) Tiga Fasa?", "id": "Q = √3 x Vl x Il x Sin(θ)." },
  { "en": "Apa Rumus Daya Semu (S) Tiga Fasa?", "id": "S = √3 x Vl x Il." },
  { "en": "Apa Itu Metode Dua Wattmeter (Two-Wattmeter Method)?", "id": "Mengukur Daya Tiga Fasa Dua Wattmeter." },
  { "en": "Kapan Metode Dua Wattmeter (Two-Wattmeter Method) Digunakan?", "id": "Sistem Tiga Fasa Tiga Kawat." },
  { "en": "Apa Itu Komponen Simetris (Symmetrical Components)?", "id": "Metode Analisis Gangguan Asimetris." },
  { "en": "Sebutkan Tiga Komponen Simetris (Symmetrical Components)?", "id": "Urutan Positif, Negatif, Nol." },
  { "en": "Apa Itu Komponen Urutan Positif (Positive Sequence)?", "id": "Urutan Fasa Normal (R-S-T)." },
  { "en": "Apa Itu Komponen Urutan Negatif (Negative Sequence)?", "id": "Urutan Fasa Terbalik (R-T-S)." },
  { "en": "Apa Efek Komponen Urutan Negatif (Negative Sequence)?", "id": "Menyebabkan Pemanasan Berlebih Pada Motor." },
  { "en": "Apa Itu Komponen Urutan Nol (Zero Sequence)?", "id": "Tiga Komponen Sefasa (Tidak Berbeda Fasa)." },
  { "en": "Kapan Komponen Urutan Nol (Zero Sequence) Muncul?", "id": "Hanya Saat Terjadi Gangguan Ke Tanah." },
  { "en": "Apa Itu Per-Unit (Per-Unit) Sistem?", "id": "Normalisasi Nilai Listrik Terhadap Basis." },
  { "en": "Mengapa Menggunakan Sistem Per-Unit (Per-Unit)?", "id": "Menyederhanakan Perhitungan Sistem Tenaga." },
  { "en": "Apa Itu Nilai Basis (Base Value) Per-Unit?", "id": "Nilai Referensi (Contoh: MVA, kV)." },
  { "en": "Apa Itu Diagram Satu Garis (One-Line Diagram)?", "id": "Representasi Sederhana Jaringan Listrik." },
  { "en": "Apa Kepanjangan SLD (Single Line Diagram)?", "id": "Diagram Garis Tunggal." },
  { "en": "Apa Itu Studi Hubung Singkat (Short Circuit Study)?", "id": "Analisis Perhitungan Arus Gangguan Maksimal." },
  { "en": "Apa Itu Studi Stabilitas (Stability Study)?", "id": "Analisis Kemampuan Sistem Pulih Dari Gangguan." },
  { "en": "Apa Itu Inertia (Inersia) Generator?", "id": "Kemampuan Generator Simpan Energi Kinetik." },
  { "en": "Apa Itu Persamaan Ayunan (Swing Equation)?", "id": "Persamaan Dinamika Sudut Rotor Generator." },
  { "en": "Apa Itu Penyesuaian Beban Frekuensi (Load Frequency Control)?", "id": "Kontrol Jaga Frekuensi Sistem Tetap Stabil." },
  { "en": "Apa Kepanjangan LFC (Load Frequency Control)?", "id": "Kontrol Penyesuaian Beban Frekuensi." },
  { "en": "Apa Itu Kontrol Tegangan (Voltage Control)?", "id": "Kontrol Jaga Level Tegangan Sistem." },
  { "en": "Bagaimana Cara Mengatur Tegangan (Voltage Control) Generator?", "id": "Mengatur Arus Eksitasi (Penguatan) Rotor." },
  { "en": "Apa Kepanjangan AVR (Automatic Voltage Regulator) Generator?", "id": "Regulator Tegangan Otomatis Generator." },
  { "en": "Apa Itu Droop (Droop) Kecepatan Governor?", "id": "Pengaturan Penurunan Kecepatan Generator." },
  { "en": "Apa Itu Operasi Isokron (Isochronous Operation) Governor?", "id": "Menjaga Kecepatan (Frekuensi) Tepat Konstan." },
  { "en": "Apa Itu Pembangkitan Terdistribusi (Distributed Generation)?", "id": "Pembangkit Listrik Skala Kecil Tersebar." },
  { "en": "Apa Kepanjangan DG (Distributed Generation)?", "id": "Pembangkitan Terdistribusi." },
  { "en": "Apa Itu Jaringan Mikro (Microgrid)?", "id": "Jaringan Listrik Lokal (Bisa Mandiri)." },
  { "en": "Apa Itu Islanding (Islanding) Jaringan?", "id": "Kondisi Jaringan Mikro Terputus PLN." },
  { "en": "Apa Itu Trafo Zig-Zag (Zig-Zag Transformer)?", "id": "Transformator Khusus Untuk Pembumian." },
  { "en": "Apa Fungsi Trafo Zig-Zag (Zig-Zag Transformer)?", "id": "Menyediakan Jalur Arus Urutan Nol." },
  { "en": "Apa Itu Trafo Scott-T (Scott-T Transformer)?", "id": "Transformator Pengubah Tiga Fasa Ke Dua Fasa." },
  { "en": "Apa Itu Trafo Pembumian (Grounding Transformer)?", "id": "Nama Lain Trafo Zig-Zag." },
  { "en": "Apa Itu Recloser (Penutup Balik Otomatis)?", "id": "Pemutus Sirkuit Menutup Kembali Otomatis." },
  { "en": "Dimana Recloser (Penutup Balik Otomatis) Digunakan?", "id": "Jaringan Distribusi (Melindungi Gangguan Temporer)." },
  { "en": "Apa Itu Sectionalizer (Pemisah Seksi)?", "id": "Saklar Otomatis Isolasi Gangguan Permanen." },
  { "en": "Apa Itu Lighting Arrester (Penangkal Petir) Jaringan?", "id": "Pelindung Jaringan Dari Tegangan Lebih Petir." },
  { "en": "Apa Nama Lain Lighting Arrester (Penangkal Petir)?", "id": "Surge Arrester (Penangkal Surja)." },
  { "en": "Apa Itu GIS (Gas Insulated Switchgear)?", "id": "Switchgear (Peralatan Hubung) Isolasi Gas SF6." },
  { "en": "Apa Keuntungan GIS (Gas Insulated Switchgear)?", "id": "Ukuran Sangat Kompak, Keandalan Tinggi." },
  { "en": "Apa Itu AIS (Air Insulated Switchgear)?", "id": "Switchgear (Peralatan Hubung) Isolasi Udara." },
  { "en": "Apa Itu Ruang Kontrol (Control Room)?", "id": "Ruang Pusat Operasi, Pemantauan Sistem." },
  { "en": "Apa Kepanjangan EMS (Energy Management System)?", "id": "Sistem Manajemen Energi." },
  { "en": "Apa Fungsi EMS (Energy Management System)?", "id": "Mengoptimalkan Operasi Sistem Tenaga Listrik." },
  { "en": "Apa Itu Pengiriman Ekonomis (Economic Dispatch)?", "id": "Pembagian Beban Pembangkit Paling Murah." },
  { "en": "Apa Itu Biaya Bahan Bakar (Fuel Cost) Pembangkit?", "id": "Biaya Operasi Utama Pembangkit Termal." },
  { "en": "Apa Itu Biaya Marginal (Marginal Cost) Pembangkit?", "id": "Biaya Tambahan Hasilkan Satu MWh Lagi." },
  { "en": "Apa Itu Cadangan Berputar (Spinning Reserve)?", "id": "Kapasitas Pembangkit Siap Naik Cepat." },
  { "en": "Mengapa Cadangan Berputar (Spinning Reserve) Penting?", "id": "Menjaga Stabilitas Frekuensi Saat Gangguan." },
  { "en": "Apa Itu Black Start (Black Start) Pembangkit?", "id": "Kemampuan Pembangkit Hidup Tanpa Listrik PLN." },
  { "en": "Pembangkit Apa Yang Mampu Black Start (Black Start)?", "id": "PLTA (Pembangkit Listrik Tenaga Air), PLTD (Pembangkit Listrik Tenaga Diesel)." },
  { "en": "Apa Itu Pasar Listrik (Electricity Market)?", "id": "Mekanisme Jual Beli Tenaga Listrik." },
  { "en": "Apa Itu Deregulasi (Deregulation) Listrik?", "id": "Pemisahan Bisnis Pembangkitan, Transmisi, Penjualan." },
  { "en": "Apa Kepanjangan ISO (Independent System Operator)?", "id": "Operator Sistem Independen (Pengatur Jaringan)." },
  { "en": "Apa Kepanjangan TSO (Transmission System Operator)?", "id": "Operator Sistem Transmisi." },
  { "en": "Apa Kepanjangan DSO (Distribution System Operator)?", "id": "Operator Sistem Distribusi." },
  { "en": "Apa Itu Konsumen Industri (Industrial Consumer)?", "id": "Pengguna Listrik Skala Besar (Pabrik)." },
  { "en": "Apa Itu Konsumen Komersial (Commercial Consumer)?", "id": "Pengguna Listrik Bisnis (Mall, Kantor)." },
  { "en": "Apa Itu Konsumen Residensial (Residential Consumer)?", "id": "Pengguna Listrik Rumah Tangga." },
  { "en": "Apa Itu Tarif Listrik (Electricity Tariff)?", "id": "Harga Jual Tenaga Listrik Per kWh." },
  { "en": "Apa Itu Tarif Blok (Block Tariff)?", "id": "Tarif Berbeda Berdasarkan Jumlah Konsumsi." },
  { "en": "Apa Itu Tarif Waktu Pakai (Time-of-Use Tariff)?", "id": "Tarif Berbeda Berdasarkan Waktu Pemakaian." },
  { "en": "Apa Kepanjangan WBP (Waktu Beban Puncak)?", "id": "Waktu Beban Puncak (Tarif Mahal)." },
  { "en": "Apa Kepanjangan LWBP (Luar Waktu Beban Puncak)?", "id": "Luar Waktu Beban Puncak (Tarif Murah)." },
  { "en": "Apa Itu Faktor Beban (Load Factor)?", "id": "Rasio Beban Rata-Rata Terhadap Puncak." },
  { "en": "Apa Itu Faktor Keberagaman (Diversity Factor)?", "id": "Rasio Jumlah Beban Puncak Individu, Puncak Sistem." },
  { "en": "Apa Itu Faktor Utilisasi (Utilization Factor)?", "id": "Rasio Beban Puncak Terhadap Kapasitas Terpasang." },
  { "en": "Apa Itu Analisis Aliran Daya (Power Flow Analysis)?", "id": "Perhitungan Aliran Daya, Tegangan Jaringan." },
  { "en": "Apa Itu Bus Slack (Slack Bus)?", "id": "Bus Referensi (Tegangan, Sudut Fasa Tetap)." },
  { "en": "Apa Itu Bus Beban (PQ Bus)?", "id": "Bus Beban (Daya P, Q Diketahui)." },
  { "en": "Apa Itu Bus Generator (PV Bus)?", "id": "Bus Generator (Daya P, Tegangan V Diketahui)." },
  { "en": "Metode Apa Untuk Analisis Aliran Daya?", "id": "Metode Gauss-Seidel, Newton-Raphson." },
  { "en": "Metode Mana Yang Lebih Cepat Konvergen?", "id": "Metode Newton-Raphson." },
  { "en": "Apa Itu Matriks Admitansi Bus (Bus Admittance Matrix)?", "id": "Matriks Y-Bus (Admitansi Antar Bus)." },
  { "en": "Apa Itu Matriks Impedansi Bus (Bus Impedance Matrix)?", "id": "Matriks Z-Bus (Impedansi Antar Bus)." },
  { "en": "Apa Itu Saluran Transmisi Rugi (Lossy Line)?", "id": "Saluran Transmisi Memperhitungkan Resistansi (R)." },
  { "en": "Apa Itu Saluran Transmisi Tanpa Rugi (Lossless Line)?", "id": "Saluran Transmisi Mengabaikan Resistansi (R=0)." },
  { "en": "Apa Itu Saluran Transmisi Pendek (Short Line)?", "id": "Saluran Mengabaikan Kapasitansi Shunt." },
  { "en": "Apa Itu Saluran Transmisi Menengah (Medium Line)?", "id": "Saluran Model Pi (π) Atau T." },
  { "en": "Apa Itu Saluran Transmisi Panjang (Long Line)?", "id": "Saluran Model Parameter Terdistribusi." },
  { "en": "Apa Itu Impedansi Gelombang (Surge Impedance Loading)?", "id": "SIL (Surge Impedance Loading)." },
  { "en": "Apa Kepanjangan SIL (Surge Impedance Loading)?", "id": "Beban Impedansi Gelombang." },
  { "en": "Apa Itu Kecepatan Propagasi (Velocity of Propagation)?", "id": "Kecepatan Gelombang Elektromagnetik Di Saluran." },
  { "en": "Apa Itu Fasor (Phasor)?", "id": "Representasi Vektor Bilangan Kompleks Gelombang Sinus." },
  { "en": "Mengapa Menggunakan Fasor (Phasor) Dalam Analisis AC?", "id": "Menyederhanakan Perhitungan Rangkaian AC." },
  { "en": "Apa Itu Domain Fasor (Phasor Domain)?", "id": "Analisis Rangkaian Menggunakan Fasor (Frekuensi Konstan)." },
  { "en": "Bagaimana Representasi Impedansi (Z) Resistor Di Domain Fasor?", "id": "Z = R (Bilangan Riil)." },
  { "en": "Bagaimana Representasi Impedansi (Z) Induktor Di Domain Fasor?", "id": "Z = jωL (Imajiner Positif)." },
  { "en": "Bagaimana Representasi Impedansi (Z) Kapasitor Di Domain Fasor?", "id": "Z = 1 / (jωC) Atau -j / (ωC)." },
  { "en": "Apa Itu Reaktansi (Reactance) (X)?", "id": "Bagian Imajiner Dari Impedansi (X)." },
  { "en": "Apa Itu Reaktansi Induktif (Inductive Reactance) (Xl)?", "id": "Reaktansi Yang Dihasilkan Induktor (ωL)." },
  { "en": "Apa Itu Reaktansi Kapasitif (Capacitive Reactance) (Xc)?", "id": "Reaktansi Dihasilkan Kapasitor (1/ωC)." },
  { "en": "Apa Itu Admitansi (Admittance) (Y)?", "id": "Kebalikan Dari Impedansi (Y = 1/Z)." },
  { "en": "Apa Satuan Admitansi (Admittance) (Y)?", "id": "Siemens (S)." },
  { "en": "Apa Itu Konduktansi (Conductance) (G)?", "id": "Bagian Riil Dari Admitansi (G)." },
  { "en": "Apa Itu Suseptansi (Susceptance) (B)?", "id": "Bagian Imajiner Dari Admitansi (B)." },
  { "en": "Apa Itu Daya Kompleks (Complex Power) (S)?", "id": "Kombinasi Daya Nyata (P), Reaktif (Q)." },
  { "en": "Apa Rumus Daya Kompleks (Complex Power) (S)?", "id": "S = P + jQ." },
  { "en": "Apa Rumus Daya Kompleks (Complex Power) (S) Fasor?", "id": "S = V x I* (Konjugat Arus)." },
  { "en": "Apa Itu Faktor Daya (Power Factor) Terdahulu (Leading)?", "id": "Arus Mendahului Tegangan (Beban Kapasitif)." },
  { "en": "Apa Itu Faktor Daya (Power Factor) Terlambat (Lagging)?", "id": "Arus Tertinggal Tegangan (Beban Induktif)." },
  { "en": "Bagaimana Koreksi Faktor Daya (Power Factor Correction)?", "id": "Menambahkan Kapasitor Paralel Beban Induktif." },
  { "en": "Apa Tujuan Koreksi Faktor Daya (Power Factor Correction)?", "id": "Mengurangi Arus Total, Rugi-Rugi Saluran." },
  { "en": "Apa Itu Resonansi (Resonance) Rangkaian AC?", "id": "Kondisi Reaktansi Induktif Sama Kapasitif (Xl=Xc)." },
  { "en": "Apa Frekuensi Resonansi (Resonance Frequency) (ωo) RLC Seri?", "id": "ωo = 1 / √(LC)." },
  { "en": "Apa Impedansi Total (Z) RLC Seri Saat Resonansi?", "id": "Impedansi Minimum (Z = R)." },
  { "en": "Apa Arus (I) RLC Seri Saat Resonansi?", "id": "Arus Maksimum (I = V/R)." },
  { "en": "Apa Impedansi Total (Z) RLC Paralel Saat Resonansi?", "id": "Impedansi Maksimum (Z = R)." },
  { "en": "Apa Arus (I) RLC Paralel Saat Resonansi?", "id": "Arus Minimum." },
  { "en": "Apa Itu Faktor Kualitas (Q-Factor) Rangkaian Resonansi?", "id": "Ukuran Tajamnya Kurva Resonansi." },
  { "en": "Apa Itu Lebar Pita (Bandwidth) Rangkaian Resonansi?", "id": "Rentang Frekuensi Antara Titik Setengah Daya." },
  { "en": "Apa Hubungan Q-Factor (Faktor Kualitas) Dan Bandwidth (Lebar Pita)?", "id": "Bandwidth = Frekuensi Resonansi / Q-Factor." },
  { "en": "Apa Itu Filter Pasif (Passive Filter) Orde Satu?", "id": "Filter RC (Resistor Kapasitor) Atau RL (Resistor Induktor)." },
  { "en": "Apa Itu Filter Pasif (Passive Filter) Orde Dua?", "id": "Filter RLC (Resistor Induktor Kapasitor)." },
  { "en": "Apa Itu Atenuasi (Attenuation) Filter?", "id": "Pelemahan Sinyal Di Stopband." },
  { "en": "Apa Itu Roll-Off (Roll-Off) Filter?", "id": "Kecuraman Transisi Dari Passband Ke Stopband." },
  { "en": "Berapa Roll-Off (Roll-Off) Filter Orde Satu?", "id": "-20 dB Per Dekade." },
  { "en": "Berapa Roll-Off (Roll-Off) Filter Orde Dua?", "id": "-40 dB Per Dekade." },
  { "en": "Apa Itu Passband (Pita Lolos)?", "id": "Rentang Frekuensi Yang Dilewatkan Filter." },
  { "en": "Apa Itu Stopband (Pita Henti)?", "id": "Rentang Frekuensi Yang Ditolak Filter." },
  { "en": "Apa Itu Riak Passband (Passband Ripple)?", "id": "Variasi Penguatan Di Dalam Passband." },
  { "en": "Filter Apa Yang Punya Riak Passband (Passband Ripple)?", "id": "Filter Chebyshev Dan Eliptik." },
  { "en": "Apa Itu Pola Lissajous (Lissajous Pattern)?", "id": "Pola Osiloskop (Mode XY)." },
  { "en": "Apa Fungsi Pola Lissajous (Lissajous Pattern)?", "id": "Mengukur Perbedaan Fasa, Frekuensi." },
  { "en": "Bentuk Apa Lissajous (Lissajous Pattern) Jika Fasa Sama (0 Derajat)?", "id": "Garis Lurus Diagonal Kanan." },
  { "en": "Bentuk Apa Lissajous (Lissajous Pattern) Jika Fasa 90 Derajat?", "id": "Lingkaran Sempurna (Jika Amplitudo Sama)." },
  { "en": "Bentuk Apa Lissajous (Lissajous Pattern) Jika Fasa 180 Derajat?", "id": "Garis Lurus Diagonal Kiri." },
  { "en": "Apa Fungsi Power Factor Meter (Pengukur Faktor Daya)?", "id": "Mengukur Faktor Daya (Cos θ) Beban." },
  { "en": "Apa Itu Elektrodinamometer (Electrodynamometer)?", "id": "Prinsip Dasar Wattmeter Analog." },
  { "en": "Apa Itu Kumparan Arus (Current Coil) Wattmeter?", "id": "Kumparan Diam (Dihubung Seri)." },
  { "en": "Apa Itu Kumparan Tegangan (Voltage Coil) Wattmeter?", "id": "Kumparan Bergerak (Dihubung Paralel)." },
  { "en": "Apa Itu Jembatan Kelvin (Kelvin Bridge)?", "id": "Modifikasi Jembatan Wheatstone." },
  { "en": "Apa Fungsi Jembatan Kelvin (Kelvin Bridge)?", "id": "Mengukur Resistansi Sangat Kecil." },
  { "en": "Apa Itu Efek Pemanasan (Joule Heating)?", "id": "Energi Listrik Berubah Jadi Panas (I²R)." },
  { "en": "Apa Itu Hukum Joule (Joule's Law)?", "id": "Panas = I² x R x t." },
  { "en": "Apa Itu Sekring (Fuse) Tipe Lambat (Slow-Blow)?", "id": "Sekring Tahan Lonjakan Arus Sesaat." },
  { "en": "Apa Itu Sekring (Fuse) Tipe Cepat (Fast-Blow)?", "id": "Sekring Putus Sangat Cepat Arus Berlebih." },
  { "en": "Dimana Sekring Lambat (Slow-Blow Fuse) Digunakan?", "id": "Rangkaian Motor (Arus Start Tinggi)." },
  { "en": "Dimana Sekring Cepat (Fast-Blow Fuse) Digunakan?", "id": "Rangkaian Elektronika Sensitif." },
  { "en": "Apa Itu Busur Listrik (Electric Arc)?", "id": "Pelepasan Listrik Plasma Antar Konduktor." },
  { "en": "Kapan Busur Listrik (Electric Arc) Terjadi?", "id": "Saat Pemutusan Arus Tinggi (Saklar)." },
  { "en": "Mengapa Busur Listrik (Electric Arc) Berbahaya?", "id": "Suhu Ekstrem, Merusak Peralatan." },
  { "en": "Apa Itu Ruang Pemadam Busur (Arc Chute)?", "id": "Bagian Pemutus Sirkuit Pendingin Busur." },
  { "en": "Apa Itu Ledakan Busur (Arc Blast)?", "id": "Ledakan Energi Tinggi Dari Busur Listrik." },
  { "en": "Apa Itu Resistivitas (Resistivity) (Rho)?", "id": "Hambatan Jenis Intrinsik Bahan." },
  { "en": "Apa Satuan Resistivitas (Resistivity)?", "id": "Ohm-Meter (Ω·m)." },
  { "en": "Apa Rumus Resistansi (R) Berbasis Resistivitas (ρ)?", "id": "R = ρ * (L / A)." },
  { "en": "Apa Itu Koefisien Suhu (Temperature Coefficient) Resistor?", "id": "Perubahan Resistansi Terhadap Perubahan Suhu." },
  { "en": "Apa Itu Resistor Film Karbon (Carbon Film Resistor)?", "id": "Resistor Umum, Murah (Lapisan Karbon)." },
  { "en": "Apa Itu Resistor Film Logam (Metal Film Resistor)?", "id": "Resistor Presisi, Noise Rendah (Lapisan Logam)." },
  { "en": "Apa Itu Resistor Kawat Lilit (Wirewound Resistor)?", "id": "Resistor Daya Tinggi (Lilitan Kawat)." },
  { "en": "Apa Itu Resistor SMD (Surface Mount Device) (Chip Resistor)?", "id": "Resistor Ukuran Sangat Kecil (Pasang Permukaan)." },
  { "en": "Apa Itu Jaringan Resistor (Resistor Network/Array)?", "id": "Beberapa Resistor Dalam Satu Paket." },
  { "en": "Apa Itu Kapasitor Film (Film Capacitor)?", "id": "Kapasitor Non-Polar (Dielektrik Plastik)." },
  { "en": "Apa Keunggulan Kapasitor Film (Film Capacitor)?", "id": "Stabil, Toleransi Ketat (Aplikasi Audio)." },
  { "en": "Apa Itu Kapasitor Mika (Mica Capacitor)?", "id": "Kapasitor Stabil Frekuensi Tinggi (RF)." },
  { "en": "Apa Itu Kapasitor Variabel (Variable Capacitor)?", "id": "Kapasitansi Dapat Diatur Manual." },
  { "en": "Dimana Kapasitor Variabel (Variable Capacitor) Digunakan?", "id": "Rangkaian Penala (Tuner) Radio." },
  { "en": "Apa Itu Kapasitor Trimmer (Trimmer Capacitor)?", "id": "Kapasitor Variabel Kecil (Set Sekali)." },
  { "en": "Apa Itu Induktor Inti Udara (Air Core Inductor)?", "id": "Induktor Tanpa Inti Magnetik." },
  { "en": "Dimana Induktor Inti Udara (Air Core Inductor) Digunakan?", "id": "Aplikasi Frekuensi Sangat Tinggi (RF)." },
  { "en": "Apa Itu Induktor Inti Ferit (Ferrite Core Inductor)?", "id": "Induktor Inti Ferit (Frekuensi Tinggi)." },
  { "en": "Apa Itu Induktor Inti Besi (Iron Core Inductor)?", "id": "Induktor Inti Besi (Frekuensi Rendah)." },
  { "en": "Apa Itu Toroid (Toroidal Inductor)?", "id": "Induktor Berbentuk Cincin Donat." },
  { "en": "Apa Keuntungan Induktor Toroid (Toroidal Inductor)?", "id": "Medan Magnet Terkandung, Efisiensi Tinggi." },
  { "en": "Apa Itu Manik Ferit (Ferrite Bead)?", "id": "Komponen Pasif Penekan Noise EMI/RFI." },
  { "en": "Apa Fungsi Manik Ferit (Ferrite Bead)?", "id": "Bertindak Seperti Induktor Frekuensi Tinggi." },
  { "en": "Apa Itu Mode Bersama (Common Mode) Noise?", "id": "Noise Muncul Sefasa Di Dua Kawat." },
  { "en": "Apa Itu Mode Diferensial (Differential Mode) Noise?", "id": "Noise Muncul Beda Fasa Di Dua Kawat." },
  { "en": "Apa Itu Choke (Choke) Mode Bersama?", "id": "Induktor Khusus Menekan Noise Mode Bersama." },
  { "en": "Apa Itu Saturasi (Saturation) Inti Induktor?", "id": "Kondisi Inti Tidak Bisa Simpan Magnet." },
  { "en": "Apa Akibat Saturasi (Saturation) Inti Induktor?", "id": "Nilai Induktansi Turun Drastis." },
  { "en": "Apa Itu Dioda Pemulihan Cepat (Fast Recovery Diode)?", "id": "Dioda Waktu Pemulihan Mundur Cepat." },
  { "en": "Apa Itu Dioda Pemulihan Lambat (Standard Recovery Diode)?", "id": "Dioda Penyearah Frekuensi Rendah (Jaringan)." },
  { "en": "Apa Itu Arus Bocor Balik (Reverse Leakage Current)?", "id": "Arus Kecil Mengalir Saat Bias Mundur." },
  { "en": "Apa Itu Dioda Avalanche (Avalanche Diode)?", "id": "Dioda Tembus (Breakdown) Mode Avalanche." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Dioda Tembus (Breakdown) Mode Zener." },
  { "en": "Apa Beda Efek Zener Dan Avalanche?", "id": "Zener (Tegangan Rendah), Avalanche (Tegangan Tinggi)." },
  { "en": "Apa Kepanjangan TVS (Transient Voltage Suppressor) Diode?", "id": "Dioda Penekan Tegangan Transien." },
  { "en": "Apa Fungsi Dioda TVS (Transient Voltage Suppressor)?", "id": "Melindungi Rangkaian Dari Lonjakan (Surge)." },
  { "en": "Apa Itu Rangkaian Clamping (Clamping Circuit)?", "id": "Nama Lain Rangkaian Penjepit (Clamper)." },
  { "en": "Apa Itu Rangkaian Crowbar (Crowbar Circuit)?", "id": "Rangkaian Proteksi Tegangan Lebih (Hubung Singkat)." },
  { "en": "Apa Komponen Utama Rangkaian Crowbar (Crowbar Circuit)?", "id": "Thyristor (SCR)." },
  { "en": "Apa Itu Konverter AC Ke AC (AC-to-AC Converter)?", "id": "Mengubah Tegangan, Frekuensi AC." },
  { "en": "Contoh Konverter AC Ke AC (AC-to-AC Converter)?", "id": "Transformator, Cycloconverter, Kontrol Fasa." },
  { "en": "Apa Itu Konverter DC Ke AC (DC-to-AC Converter)?", "id": "Inverter (Pengubah DC Ke AC)." },
  { "en": "Apa Itu Konverter AC Ke DC (AC-to-DC Converter)?", "id": "Penyearah (Rectifier)." },
  { "en": "Apa Itu Konverter DC Ke DC (DC-to-DC Converter)?", "id": "Pengubah Level Tegangan DC (Chopper)." },
  { "en": "Hukum Coulomb (Coulomb's Law) Menjelaskan Gaya Apa?", "id": "Gaya Antar Muatan Listrik." },
  { "en": "Apa Satuan Untuk Kuat Medan Listrik (E)?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Rumus Hukum Coulomb (Coulomb's Law)?", "id": "F = k * (q1*q2) / r²." },
  { "en": "Apa Itu Garis Medan Listrik (Electric Field Lines)?", "id": "Garis Arah Gaya Listrik Positif." },
  { "en": "Apa Itu Potensial Listrik (Electric Potential)?", "id": "Energi Potensial Per Satuan Muatan." },
  { "en": "Apa Satuan Potensial Listrik (Electric Potential)?", "id": "Volt (V)." },
  { "en": "Apa Itu Permukaan Ekuipotensial (Equipotential Surface)?", "id": "Permukaan Dengan Potensial Sama." },
  { "en": "Apa Itu Hukum Gauss (Gauss's Law) Listrik?", "id": "Fluks Listrik Total Terkait Muatan." },
  { "en": "Apa Itu Dielektrik (Dielectric)?", "id": "Bahan Isolator Dalam Medan Listrik." },
  { "en": "Apa Itu Polarisasi (Polarization) Dielektrik?", "id": "Pergeseran Muatan Internal Bahan Dielektrik." },
  { "en": "Apa Itu Kapasitansi (Capacitance) Pelat Sejajar?", "id": "C = ε * (A / d)." },
  { "en": "Apa Itu Energi Tersimpan Di Kapasitor?", "id": "E = 1/2 * C * V²." },
  { "en": "Apa Itu Energi Tersimpan Di Induktor?", "id": "E = 1/2 * L * I²." },
  { "en": "Apa Itu Arus Konvensional (Conventional Current)?", "id": "Aliran Muatan Positif (Teoretis)." },
  { "en": "Apa Arah Aliran Elektron (Electron Flow)?", "id": "Berlawanan Arah Arus Konvensional." },
  { "en": "Apa Itu Rapat Arus (Current Density) (J)?", "id": "Arus Listrik Per Satuan Luas." },
  { "en": "Apa Satuan Rapat Arus (Current Density) (J)?", "id": "Ampere Per Meter Persegi (A/m²)." },
  { "en": "Apa Itu Hukum Ampere (Ampere's Law)?", "id": "Hubungan Arus Listrik, Medan Magnet." },
  { "en": "Apa Itu Hukum Biot-Savart (Biot-Savart Law)?", "id": "Menghitung Medan Magnet Akibat Arus." },
  { "en": "Apa Itu Gaya Lorentz (Lorentz Force)?", "id": "Gaya Pada Muatan Bergerak Medan Magnet." },
  { "en": "Apa Rumus Gaya Lorentz (Lorentz Force)?", "id": "F = q(E + v x B)." },
  { "en": "Apa Itu Kerapatan Energi Magnetik (Magnetic Energy Density)?", "id": "Energi Magnetik Per Satuan Volume." },
  { "en": "Apa Itu Trafo Ideal (Ideal Transformer)?", "id": "Transformator Tanpa Rugi-Rugi Sama Sekali." },
  { "en": "Apa Itu Rasio Belitan (Turns Ratio) Trafo?", "id": "Rasio Jumlah Lilitan Primer, Sekunder." },
  { "en": "Apa Rumus Rasio Belitan (Turns Ratio) (a)?", "id": "a = Np / Ns." },
  { "en": "Bagaimana Hubungan Tegangan Trafo Ideal (Ideal Transformer)?", "id": "Vp / Vs = Np / Ns." },
  { "en": "Bagaimana Hubungan Arus Trafo Ideal (Ideal Transformer)?", "id": "Ip / Is = Ns / Np." },
  { "en": "Apa Itu Trafo Tiga Fasa (Three-Phase Transformer)?", "id": "Transformator Untuk Sistem Tiga Fasa." },
  { "en": "Sebutkan Tipe Koneksi Trafo Tiga Fasa?", "id": "Bintang-Bintang, Bintang-Delta, Delta-Bintang, Delta-Delta." },
  { "en": "Apa Itu Koneksi Y-Y (Y-Y Connection)?", "id": "Koneksi Bintang-Bintang." },
  { "en": "Apa Itu Koneksi Δ-Y (Delta-Wye Connection)?", "id": "Koneksi Delta-Bintang." },
  { "en": "Apa Itu Arus Inrush (Inrush Current) Trafo?", "id": "Arus Lonjakan Tinggi Saat Trafo Dihidupkan." },
  { "en": "Apa Penyebab Arus Inrush (Inrush Current) Trafo?", "id": "Saturasi Inti Magnetik Sesaat." },
  { "en": "Apa Itu Arus Magnetisasi (Magnetizing Current)?", "id": "Arus Pembentuk Fluks Magnetik Inti." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp) Ideal?", "id": "Penguat Sempurna (Gain Tak Terhingga)." },
  { "en": "Apa Itu Virtual Ground (Tanah Virtual) Op-Amp?", "id": "Titik Input Inverting (Tegangan Nol Volt)." },
  { "en": "Kapan Virtual Ground (Tanah Virtual) Terjadi?", "id": "Konfigurasi Inverting (Umpan Balik Negatif)." },
  { "en": "Apa Itu Rangkaian Penjumlah (Summing Amplifier) Op-Amp?", "id": "Menjumlahkan Beberapa Sinyal Input Tegangan." },
  { "en": "Apa Itu Rangkaian Pengurang (Subtractor Amplifier) Op-Amp?", "id": "Mengurangkan Dua Sinyal Input Tegangan." },
  { "en": "Apa Itu Dioda Ideal (Ideal Diode)?", "id": "Saklar Sempurna (On/Off Instan)." },
  { "en": "Apa Tegangan Maju (Forward Voltage) Dioda Silikon?", "id": "Sekitar 0.7 Volt." },
  { "en": "Apa Tegangan Maju (Forward Voltage) Dioda Germanium?", "id": "Sekitar 0.3 Volt." },
  { "en": "Apa Tegangan Maju (Forward Voltage) Dioda Schottky?", "id": "Sekitar 0.2 Hingga 0.4 Volt." },
  { "en": "Apa Itu Waktu Pemulihan Mundur (Reverse Recovery Time)?", "id": "Waktu Dioda Berhenti Konduksi (Mati)." },
  { "en": "Apa Itu Kapasitansi Sambungan (Junction Capacitance) Dioda?", "id": "Kapasitansi Internal Dioda (Daerah Deplesi)." },
  { "en": "Apa Itu Transistor Mode Cut-Off (Cut-Off Mode)?", "id": "Transistor Mati (Saklar Terbuka)." },
  { "en": "Apa Itu Transistor Mode Saturasi (Saturation Mode)?", "id": "Transistor Jenuh (Saklar Tertutup)." },
  { "en": "Apa Itu Transistor Mode Aktif (Active Mode)?", "id": "Transistor Sebagai Penguat Sinyal." },
  { "en": "Apa Itu Penguatan Arus (Beta) (β) BJT?", "id": "Rasio Arus Kolektor Terhadap Arus Basis." },
  { "en": "Apa Nama Lain Beta (β) Transistor?", "id": "hFE (Parameter Hibrid)." },
  { "en": "Apa Itu Penguatan Arus (Alpha) (α) BJT?", "id": "Rasio Arus Kolektor Terhadap Arus Emitor." },
  { "en": "Apa Hubungan Alpha (α) Dan Beta (β)?", "id": "β = α / (1 - α)." },
  { "en": "Apa Itu Daerah Kerja Aman (Safe Operating Area)?", "id": "Batas Operasi Aman Transistor (SOA)." },
  { "en": "Apa Kepanjangan SOA (Safe Operating Area)?", "id": "Daerah Kerja Aman." },
  { "en": "Apa Itu Peringkat Daya (Power Rating) Resistor?", "id": "Daya Maksimum Dapat Dibuang Resistor." },
  { "en": "Apa Yang Terjadi Jika Resistor Kelebihan Daya?", "id": "Resistor Akan Terbakar Atau Rusak." },
  { "en": "Apa Itu Tegangan Kerja (Working Voltage) Kapasitor?", "id": "Tegangan DC Maksimum Kapasitor." },
  { "en": "Apa Yang Terjadi Kapasitor Kelebihan Tegangan?", "id": "Dielektrik Tembus (Rusak, Meledak)." },
  { "en": "Apa Kepanjangan ESR (Equivalent Series Resistance) Kapasitor?", "id": "Resistansi Seri Ekuivalen Internal Kapasitor." },
  { "en": "Apa Kepanjangan ESL (Equivalent Series Inductance) Kapasitor?", "id": "Induktansi Seri Ekuivalen Internal Kapasitor." },
  { "en": "Apa Itu Faktor Disipasi (Dissipation Factor) Kapasitor?", "id": "Ukuran Rugi-Rugi Internal Kapasitor." },
  { "en": "Apa Itu Induktor Mode Diferensial (Differential Mode Inductor)?", "id": "Induktor Penekan Noise Mode Diferensial." },
  { "en": "Apa Itu Inti Ferit (Ferrite Core)?", "id": "Bahan Magnetik Keramik (Frekuensi Tinggi)." },
  { "en": "Apa Itu Inti Serbuk Besi (Iron Powder Core)?", "id": "Inti Magnetik (Penyimpan Energi DC)." },
  { "en": "Apa Itu Pengujian Kontinuitas (Continuity Test)?", "id": "Memeriksa Sambungan Sirkuit (Buzzer Multimeter)." },
  { "en": "Apa Itu Pengujian Dioda (Diode Test) Multimeter?", "id": "Mengukur Tegangan Maju Dioda." },
  { "en": "Bagaimana Tes Dioda Yang Baik (Forward Bias)?", "id": "Menunjukkan Tegangan Maju (Contoh: 0.7V)." },
  { "en": "Bagaimana Tes Dioda Yang Baik (Reverse Bias)?", "id": "Menunjukkan 'OL' (Open Loop)." },
  { "en": "Bagaimana Tes Dioda Yang Rusak (Hubung Singkat)?", "id": "Menunjukkan '0V' Kedua Arah." },
  { "en": "Bagaimana Tes Dioda Yang Rusak (Terbuka)?", "id": "Menunjukkan 'OL' Kedua Arah." },
  { "en": "Apa Itu Timer (Pewaktu) Mikrokontroler?", "id": "Periferal Penghitung Pulsa Clock Internal." },
  { "en": "Apa Fungsi Timer (Pewaktu) Mikrokontroler?", "id": "Menghasilkan Penundaan, Sinyal PWM, Mengukur Frekuensi." },
  { "en": "Apa Itu Counter (Pencacah) Mikrokontroler?", "id": "Periferal Penghitung Pulsa Eksternal." },
  { "en": "Apa Itu Mode PWM (Pulse Width Modulation) Timer?", "id": "Mode Timer Hasilkan Sinyal PWM." },
  { "en": "Apa Itu Mode Input Capture (Input Capture) Timer?", "id": "Merekam Waktu Saat Sinyal Input Berubah." },
  { "en": "Apa Itu Mode Output Compare (Output Compare) Timer?", "id": "Menghasilkan Sinyal Output Saat Nilai Sama." },
  { "en": "Apa Itu Watchdog Timer (WDT) Mikrokontroler?", "id": "Timer Reset Otomatis Saat Program Macet." },
  { "en": "Apa Itu Real-Time Clock (RTC) Mikrokontroler?", "id": "Periferal Penyimpan Waktu, Tanggal Aktual." },
  { "en": "Apa Kepanjangan RTC (Real-Time Clock)?", "id": "Jam Waktu Nyata." },
  { "en": "Apa Itu Brown-Out Detector (BOD) Mikrokontroler?", "id": "Detektor Penurunan Tegangan Catu Daya." },
  { "en": "Apa Fungsi BOD (Brown-Out Detector)?", "id": "Mencegah Operasi Error Saat Tegangan Turun." },
  { "en": "Apa Itu Sleep Mode (Mode Tidur) Mikrokontroler?", "id": "Mode Konsumsi Daya Sangat Rendah." },
  { "en": "Apa Itu Sistem Kontrol Otomatis (Automatic Control)?", "id": "Sistem Mengatur Diri Sendiri (Umpan Balik)." },
  { "en": "Apa Itu Kontroler Proporsional (P Controller)?", "id": "Output Kontrol Proporsional Terhadap Error." },
  { "en": "Apa Itu Kontroler Integral (I Controller)?", "id": "Output Kontrol Proporsional Integral Error." },
  { "en": "Apa Fungsi Aksi Integral (I Action)?", "id": "Menghilangkan Error Keadaan Tunak (Steady-State Error)." },
  { "en": "Apa Itu Kontroler Derivatif (D Controller)?", "id": "Output Kontrol Proporsional Laju Error." },
  { "en": "Apa Fungsi Aksi Derivatif (D Action)?", "id": "Memberi Respon Cepat (Antisipasi Perubahan)." },
  { "en": "Apa Itu Error Keadaan Tunak (Steady-State Error)?", "id": "Selisih Setpoint, Output Saat Stabil." },
  { "en": "Apa Itu Kontroler PI (Proportional-Integral)?", "id": "Gabungan Kontrol Proporsional Dan Integral." },
  { "en": "Apa Itu Kontroler PD (Proportional-Derivative)?", "id": "Gabungan Kontrol Proporsional, Derivatif." },
  { "en": "Apa Itu Penalaan (Tuning) PID (Proportional Integral Derivative)?", "id": "Proses Penentuan Parameter Kp, Ki, Kd." },
  { "en": "Metode Apa Untuk Penalaan (Tuning) PID?", "id": "Metode Ziegler-Nichols." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Komponen Penggerak (Output Fisik Kontrol)." },
  { "en": "Sebutkan Contoh Aktuator (Actuator) Listrik?", "id": "Motor Listrik, Solenoida, Relay." },
  { "en": "Apa Itu Elektrolisis (Electrolysis)?", "id": "Proses Penguraian Senyawa Kimia Listrik." },
  { "en": "Apa Itu Hukum Faraday (Faraday's Law) Elektrolisis?", "id": "Massa Zat Berbanding Lurus Muatan Listrik." },
  { "en": "Apa Itu Baterai (Battery) Primer?", "id": "Baterai Sekali Pakai (Tidak Bisa Isi Ulang)." },
  { "en": "Apa Itu Baterai (Battery) Sekunder?", "id": "Baterai Dapat Diisi Ulang." },
  { "en": "Sebutkan Contoh Baterai (Battery) Sekunder?", "id": "Aki (Lead-Acid), Lithium-Ion (Li-Ion)." }



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
