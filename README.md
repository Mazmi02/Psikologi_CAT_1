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


  { "en": "Apa Kepanjangan SIPSS (Sekolah Inspektur Polisi Sumber Sarjana)?", "id": "Sekolah Inspektur Polisi Sumber Sarjana" },
  { "en": "Apa Itu Sistem CAT (Computer Assisted Test)?", "id": "Tes Berbasis Komputer" },
  { "en": "Tes Kecerdasan Mengukur Kemampuan Apa?", "id": "Kemampuan Kognitif Peserta" },
  { "en": "Apa Tujuan Tes Psikologi (Psychological Test)?", "id": "Menilai Kepribadian Dan Potensi" },
  { "en": "Apa Itu Tes Sikap Kerja (Work Attitude)?", "id": "Penilaian Perilaku Dalam Bekerja" },
  { "en": "Nilai Ambang Batas Disebut Juga Apa?", "id": "Passing Grade" },
  { "en": "Sinonim Kata Adaptasi Adalah?", "id": "Penyesuaian Diri" },
  { "en": "Antonim Kata Progresif Adalah?", "id": "Regresif Atau Mundur" },
  { "en": "Apa Itu Tes Kecermatan?", "id": "Mengukur Ketelitian Dan Kecepatan" },
  { "en": "Logika Aritmatika Berhubungan Dengan Apa?", "id": "Angka Dan Hitungan" },
  { "en": "Logika Verbal Berhubungan Dengan Apa?", "id": "Kata Dan Bahasa" },
  { "en": "Apa Arti Integritas Dalam Sikap Kerja?", "id": "Jujur Dan Konsisten" },
  { "en": "Lanjutan Deret Angka Satu, Tiga, Lima Adalah?", "id": "Tujuh" },
  { "en": "Apa Sinonim Kata Akurat?", "id": "Tepat Dan Teliti" },
  { "en": "Apa Antonim Kata Absurd?", "id": "Masuk Akal" },
  { "en": "Apa Fokus Tes Kepribadian?", "id": "Karakter Dan Emosi" },
  { "en": "Simbol P Pada Tes Kecermatan Biasanya Berupa?", "id": "Huruf Atau Angka Acak" },
  { "en": "Apa Itu Kemampuan Analitis?", "id": "Mengurai Masalah Secara Logis" },
  { "en": "Apa Lawan Kata Tentatif?", "id": "Pasti Atau Tetap" },
  { "en": "Apa Persamaan Kata Komprehensif?", "id": "Menyeluruh Atau Lengkap" },
  { "en": "Deret Dua, Empat, Delapan, Enam Belas, Selanjutnya?", "id": "Tiga Puluh Dua" },
  { "en": "Berapa Waktu Rata-Rata Per Soal Kecerdasan?", "id": "Kurang Dari Satu Menit" },
  { "en": "Apa Itu Daya Tahan Kerja?", "id": "Kemampuan Bekerja Waktu Lama" },
  { "en": "Sikap Asertif Artinya Apa?", "id": "Tegas Namun Sopan" },
  { "en": "Apa Sinonim Kata Kredibel?", "id": "Dapat Dipercaya" },
  { "en": "Apa Antonim Kata Monoton?", "id": "Bervariasi Atau Beragam" },
  { "en": "Tes Pauli Menggunakan Media Apa?", "id": "Kertas Koran Dan Angka" },
  { "en": "Tes Koran Disebut Juga Tes Apa?", "id": "Tes Pauli Atau Kraepelin" },
  { "en": "Apa Yang Dinilai Dari Tes Pauli?", "id": "Kecepatan Dan Stabilitas Kerja" },
  { "en": "Jika Semua A Adalah B, Maka?", "id": "Sebagian B Adalah A" },
  { "en": "Apa Arti Loyalitas Dalam Polri?", "id": "Setia Pada Institusi" },
  { "en": "Apa Sinonim Kata Efisien?", "id": "Tepat Guna" },
  { "en": "Apa Antonim Kata Sporadis?", "id": "Sering Atau Teratur" },
  { "en": "Satu Menit Berapa Detik?", "id": "Enam Puluh Detik" },
  { "en": "Satu Jam Berapa Menit?", "id": "Enam Puluh Menit" },
  { "en": "Apa Itu Tes Spasial?", "id": "Kemampuan Membayangkan Ruang" },
  { "en": "Tes Rotasi Kubus Mengukur Apa?", "id": "Logika Ruang Tiga Dimensi" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Gejolak Atau Naik Turun" },
  { "en": "Apa Antonim Kata Paradoksal?", "id": "Sejalan Atau Konsisten" },
  { "en": "Apa Itu Silogisme?", "id": "Penarikan Kesimpulan Logis" },
  { "en": "Semua Polisi Tegas. Budi Polisi. Kesimpulannya?", "id": "Budi Itu Tegas" },
  { "en": "Apa Arti Transparansi Dalam Seleksi?", "id": "Terbuka Dan Jelas" },
  { "en": "Apa Itu Akuntabel?", "id": "Dapat Dipertanggungjawabkan" },
  { "en": "Prinsip BETAH (Bersih, Transparan, Akuntabel, Humanis) Adalah?", "id": "Prinsip Seleksi Polri" },
  { "en": "Apa Sinonim Kata Signifikan?", "id": "Penting Atau Berarti" },
  { "en": "Apa Antonim Kata Epilog?", "id": "Prolog Atau Pembukaan" },
  { "en": "Deret Lima, Sepuluh, Lima Belas, Selanjutnya?", "id": "Dua Puluh" },
  { "en": "Apa Itu Kemampuan Verbal?", "id": "Penguasaan Kosakata Dan Bahasa" },
  { "en": "Apa Itu Kemampuan Numerik?", "id": "Penguasaan Angka Dan Hitungan" },
  { "en": "Tes Deret Gambar Mengukur Apa?", "id": "Logika Visual" },
  { "en": "Apa Sinonim Kata Implisit?", "id": "Tersirat" },
  { "en": "Apa Antonim Kata Eksplisit?", "id": "Tersirat Atau Implisit" },
  { "en": "Ibukota Indonesia Saat Ini Adalah?", "id": "Jakarta" },
  { "en": "Lambang Negara Indonesia Adalah?", "id": "Garuda Pancasila" },
  { "en": "Apa Tugas Pokok Polri?", "id": "Memelihara Keamanan Dan Ketertiban" },
  { "en": "Apa Semboyan Polri?", "id": "Rastra Sewakottama" },
  { "en": "Apa Arti Rastra Sewakottama?", "id": "Abdi Utama Bagi Nusa Bangsa" },
  { "en": "Tribrata Adalah Apa?", "id": "Pedoman Hidup Polri" },
  { "en": "Catur Prasetya Adalah Apa?", "id": "Pedoman Kerja Polri" },
  { "en": "Siapa Panglima Tertinggi Polri?", "id": "Presiden Republik Indonesia" },
  { "en": "Apa Sinonim Kata Mobilisasi?", "id": "Pengerahan Atau Pergerakan" },
  { "en": "Apa Antonim Kata Statis?", "id": "Dinamis Atau Bergerak" },
  { "en": "Rumus Luas Persegi Adalah?", "id": "Sisi Kali Sisi" },
  { "en": "Rumus Luas Segitiga Adalah?", "id": "Setengah Alas Kali Tinggi" },
  { "en": "Sudut Siku-Siku Besarnya Berapa?", "id": "Sembilan Puluh Derajat" },
  { "en": "Apa Itu Tes Analog Verbal?", "id": "Mencari Hubungan Kata" },
  { "en": "Kertas : Pena = Dinding : ...?", "id": "Kuas Atau Cat" },
  { "en": "Mobil : Roda = Burung : ...?", "id": "Sayap" },
  { "en": "Apa Sinonim Kata Restorasi?", "id": "Pemulihan Atau Perbaikan" },
  { "en": "Apa Antonim Kata Universal?", "id": "Parsial Atau Khusus" },
  { "en": "Apa Itu Ejaan Yang Disempurnakan (EYD)?", "id": "Pedoman Penulisan Bahasa Indonesia" },
  { "en": "Apa Itu Hipotesis?", "id": "Dugaan Sementara" },
  { "en": "Apa Itu Validitas Data?", "id": "Keabsahan Data" },
  { "en": "Tes PAPI (Personality And Preference Inventory) Mengukur?", "id": "Peran Dan Kebutuhan Individu" },
  { "en": "Apa Itu Tes EPPS (Edwards Personal Preference Schedule)?", "id": "Tes Preferensi Kepribadian" },
  { "en": "Dalam Tes Kepribadian, Apakah Ada Benar Salah?", "id": "Tidak Ada Benar Salah" },
  { "en": "Apa Yang Harus Dihindari Saat Tes Kepribadian?", "id": "Berbohong Atau Memanipulasi" },
  { "en": "Apa Itu Skala Likert?", "id": "Skala Pengukuran Sikap" },
  { "en": "Pangkat Terendah Perwira Pertama Adalah?", "id": "Inspektur Polisi Dua" },
  { "en": "Apa Kepanjangan IPDA (Inspektur Polisi Dua)?", "id": "Inspektur Polisi Dua" },
  { "en": "Apa Kepanjangan IPTU (Inspektur Polisi Satu)?", "id": "Inspektur Polisi Satu" },
  { "en": "Apa Kepanjangan AKP (Ajun Komisaris Polisi)?", "id": "Ajun Komisaris Polisi" },
  { "en": "Apa Sinonim Kata Inovasi?", "id": "Pembaruan Atau Terobosan" },
  { "en": "Apa Antonim Kata Fiksi?", "id": "Fakta Atau Nyata" },
  { "en": "Deret Tiga, Enam, Sembilan, Selanjutnya?", "id": "Dua Belas" },
  { "en": "Deret Seratus, Sembilan Puluh, Delapan Puluh, Selanjutnya?", "id": "Tujuh Puluh" },
  { "en": "Apa Itu Minat Dan Bakat?", "id": "Ketertarikan Dan Potensi Bawaan" },
  { "en": "Apa Yang Diukur Tes Minat?", "id": "Preferensi Aktivitas Peserta" },
  { "en": "Apa Itu Stabilitas Emosi?", "id": "Ketenangan Dalam Tekanan" },
  { "en": "Sikap Profesional Artinya?", "id": "Kompeten Dan Bertanggung Jawab" },
  { "en": "Apa Sinonim Kata Elaborasi?", "id": "Penjelasan Terperinci" },
  { "en": "Apa Antonim Kata Kolektif?", "id": "Individual Atau Perorangan" },
  { "en": "Satu Dekade Berapa Tahun?", "id": "Sepuluh Tahun" },
  { "en": "Satu Abad Berapa Tahun?", "id": "Seratus Tahun" },
  { "en": "Apa Itu Soal Cerita Matematika?", "id": "Matematika Dalam Bentuk Kalimat" },
  { "en": "Apa Kunci Mengerjakan Tes Kecermatan?", "id": "Fokus Dan Tahan Lelah" },
  { "en": "Apa Itu Distraktor Dalam Soal?", "id": "Pengecoh Jawaban" },
  { "en": "Apa Kepanjangan NKRI (Negara Kesatuan Republik Indonesia)?", "id": "Negara Kesatuan Republik Indonesia" },
  { "en": "Bhinneka Tunggal Ika Artinya?", "id": "Berbeda Tetap Satu Jua" },
  { "en": "Siapa Bapak Proklamator Indonesia?", "id": "Soekarno Dan Hatta" },
  { "en": "Beras Asal Padi, Maka Sagu Asal?", "id": "Pohon Rumbia" },
  { "en": "Kuda Punya Kaki, Maka Mobil Punya?", "id": "Roda" },
  { "en": "Apa Sinonim Kata Agitasi?", "id": "Hasutan Atau Provokasi" },
  { "en": "Apa Antonim Kata Canggih?", "id": "Kuno Atau Tradisional" },
  { "en": "Jika Hari Ini Senin, Besok Adalah?", "id": "Selasa" },
  { "en": "Apa Itu Tes Wartegg?", "id": "Melengkapi Delapan Kotak Gambar" },
  { "en": "Satu Lusin Berapa Buah?", "id": "Dua Belas Buah" },
  { "en": "Satu Kodi Berapa Buah?", "id": "Dua Puluh Buah" },
  { "en": "Satu Gros Berapa Buah?", "id": "Seratus Empat Puluh Empat" },
  { "en": "Apa Kepanjangan TPA (Tes Potensi Akademik)?", "id": "Tes Potensi Akademik" },
  { "en": "Apa Kepanjangan TKP (Tes Karakteristik Pribadi)?", "id": "Tes Karakteristik Pribadi" },
  { "en": "Apa Kepanjangan TIU (Tes Intelegensia Umum)?", "id": "Tes Intelegensia Umum" },
  { "en": "Apa Kepanjangan TWK (Tes Wawasan Kebangsaan)?", "id": "Tes Wawasan Kebangsaan" },
  { "en": "Deret Satu, Empat, Sembilan, Enam Belas, Selanjutnya?", "id": "Dua Puluh Lima" },
  { "en": "Deret Tiga, Tiga, Enam, Sembilan, Lima Belas, Selanjutnya?", "id": "Dua Puluh Empat" },
  { "en": "Apa Itu Deret Fibonacci?", "id": "Penjumlahan Dua Angka Sebelumnya" },
  { "en": "Apa Sinonim Kata Pailit?", "id": "Bangkrut" },
  { "en": "Apa Antonim Kata Pro?", "id": "Kontra" },
  { "en": "Burung Terbang Dengan Sayap, Ikan Berenang Dengan?", "id": "Sirip" },
  { "en": "Dokter Di Rumah Sakit, Guru Di?", "id": "Sekolah" },
  { "en": "Apa Arti Orientasi Pelayanan?", "id": "Mengutamakan Kepuasan Masyarakat" },
  { "en": "Apa Arti Jejaring Kerja?", "id": "Membangun Hubungan Kerjasama" },
  { "en": "Apa Sinonim Kata Narasi?", "id": "Cerita Atau Deskripsi" },
  { "en": "Apa Antonim Kata Maya?", "id": "Nyata" },
  { "en": "Jika A Lebih Besar B, Dan B Lebih Besar C?", "id": "A Lebih Besar C" },
  { "en": "Apa Itu Premis Mayor?", "id": "Pernyataan Umum Dalam Silogisme" },
  { "en": "Apa Itu Premis Minor?", "id": "Pernyataan Khusus Dalam Silogisme" },
  { "en": "Semua Makhluk Hidup Bernapas. Batu Tidak Bernapas. Kesimpulannya?", "id": "Batu Bukan Makhluk Hidup" },
  { "en": "Lampu : Gelap = Makanan : ...?", "id": "Lapar" },
  { "en": "Haus : Minum = Lelah : ...?", "id": "Istirahat" },
  { "en": "Apa Rumus Kecepatan Rata-Rata?", "id": "Jarak Dibagi Waktu" },
  { "en": "Apa Rumus Mencari Jarak?", "id": "Kecepatan Dikali Waktu" },
  { "en": "Berapa Sudut Lingkaran Penuh?", "id": "Tiga Ratus Enam Puluh Derajat" },
  { "en": "Berapa Sisi Segi Enam?", "id": "Enam Sisi" },
  { "en": "Apa Sinonim Kata Valid?", "id": "Sah Atau Benar" },
  { "en": "Apa Antonim Kata Netral?", "id": "Berpihak" },
  { "en": "Apa Arti Integritas Diri?", "id": "Keselarasan Pikiran Dan Tindakan" },
  { "en": "Apa Arti Semangat Berprestasi?", "id": "Keinginan Unggul Dan Sukses" },
  { "en": "Apa Itu Mengelola Perubahan?", "id": "Beradaptasi Dengan Situasi Baru" },
  { "en": "Apa Arti Pengambilan Keputusan?", "id": "Memilih Solusi Terbaik" },
  { "en": "Pangkat Briptu (Brigadir Polisi Satu) Di Bawah Pangkat?", "id": "Brigadir" },
  { "en": "Pangkat Kompol (Komisaris Polisi) Setara Dengan Pangkat TNI?", "id": "Mayor" },
  { "en": "Apa Kepanjangan AKBP (Ajun Komisaris Besar Polisi)?", "id": "Ajun Komisaris Besar Polisi" },
  { "en": "Apa Kepanjangan KOMBES (Komisaris Besar Polisi)?", "id": "Komisaris Besar Polisi" },
  { "en": "Apa Sinonim Kata Random?", "id": "Acak" },
  { "en": "Apa Antonim Kata Vertikal?", "id": "Horizontal" },
  { "en": "Apa Itu Gambar Pencerminan?", "id": "Bayangan Terbalik Kiri Kanan" },
  { "en": "Apa Itu Rotasi Gambar?", "id": "Perputaran Posisi Objek" },
  { "en": "Apa Itu Kubus Jaring-Jaring?", "id": "Pola Pembentuk Bangun Ruang" },
  { "en": "Kayu : Kertas = Ulat : ...?", "id": "Sutra" },
  { "en": "Apa Sinonim Kata Tendensi?", "id": "Kecenderungan" },
  { "en": "Apa Antonim Kata Sferis?", "id": "Datar" },
  { "en": "Berapa Jumlah Provinsi Di Pulau Jawa?", "id": "Enam Provinsi" },
  { "en": "Siapa Nama Kapolri Saat Ini?", "id": "Jenderal Listyo Sigit Prabowo" },
  { "en": "Apa Warna Seragam PDH (Pakaian Dinas Harian) Polri?", "id": "Cokelat Muda Dan Tua" },
  { "en": "Apa Arti Tribrata Pertama?", "id": "Berbakti Kepada Nusa Bangsa" },
  { "en": "Apa Arti Tribrata Kedua?", "id": "Menegakkan Hukum Dan Kebenaran" },
  { "en": "Apa Arti Tribrata Ketiga?", "id": "Melindungi Dan Mengayomi Masyarakat" },
  { "en": "Jika X Sama Dengan Y, Maka X Kurang Y Adalah?", "id": "Nol" },
  { "en": "Berapa Akar Kuadrat Dari Sembilan?", "id": "Tiga" },
  { "en": "Berapa Akar Kuadrat Dari Enam Belas?", "id": "Empat" },
  { "en": "Berapa Akar Kuadrat Dari Dua Puluh Lima?", "id": "Lima" },
  { "en": "Apa Sinonim Kata Kisi-Kisi?", "id": "Terali Atau Panduan" },
  { "en": "Apa Antonim Kata Pakar?", "id": "Awam" },
  { "en": "Tukang Jahit Butuh Apa?", "id": "Benang Dan Jarum" },
  { "en": "Petani Bekerja Di Mana?", "id": "Sawah Atau Ladang" },
  { "en": "Nelayan Menangkap Apa?", "id": "Ikan" },
  { "en": "Apa Itu Soal Analisis Wacana?", "id": "Memahami Inti Bacaan" },
  { "en": "Apa Itu Ide Pokok Paragraf?", "id": "Gagasan Utama Tulisan" },
  { "en": "Apa Kepanjangan HAM (Hak Asasi Manusia)?", "id": "Hak Asasi Manusia" },
  { "en": "Komnas HAM (Hak Asasi Manusia) Adalah Lembaga?", "id": "Lembaga Independen Negara" },
  { "en": "Apa Sinonim Kata Defisit?", "id": "Kekurangan" },
  { "en": "Apa Antonim Kata Surplus?", "id": "Defisit Atau Kurang" },
  { "en": "Bulan Februari Kabisat Berapa Hari?", "id": "Dua Puluh Sembilan Hari" },
  { "en": "Satu Tahun Berapa Minggu?", "id": "Lima Puluh Dua Minggu" },
  { "en": "Satu Rim Kertas Berapa Lembar?", "id": "Lima Ratus Lembar" },
  { "en": "Apa Itu Tes House Tree Person (HTP)?", "id": "Menggambar Rumah Pohon Orang" },
  { "en": "Apa Itu Tes BAUM?", "id": "Tes Menggambar Pohon" },
  { "en": "Dalam Tes Gambar Pohon, Apa Yang Dilarang?", "id": "Pohon Tumbang Atau Mati" },
  { "en": "Apa Sinonim Kata Tentatif?", "id": "Sementara" },
  { "en": "Apa Antonim Kata Pasti?", "id": "Ragu Atau Tentatif" },
  { "en": "Tiga Lusin Piring Berapa Buah?", "id": "Tiga Puluh Enam" },
  { "en": "Setengah Jam Berapa Menit?", "id": "Tiga Puluh Menit" },
  { "en": "Apa Itu Hirarki Kepangkatan?", "id": "Tingkatan Jabatan Bertingkat" },
  { "en": "Siapa Yang Berhak Melakukan Penyidikan?", "id": "Polisi Atau Penyidik" },
  { "en": "Apa Kepanjangan KUHP (Kitab Undang-Undang Hukum Pidana)?", "id": "Kitab Undang-Undang Hukum Pidana" },
  { "en": "Apa Kepanjangan KUHAP (Kitab Undang-Undang Hukum Acara Pidana)?", "id": "Kitab Undang-Undang Hukum Acara Pidana" },
  { "en": "Apa Sinonim Kata Regulasi?", "id": "Aturan Atau Ketentuan" },
  { "en": "Apa Antonim Kata Anarki?", "id": "Tertib" },
  { "en": "Sepatu : Kaki = Topi : ...?", "id": "Kepala" },
  { "en": "Cincin : Jari = Kalung : ...?", "id": "Leher" },
  { "en": "Apa Kepanjangan PROPAM (Profesi Dan Pengamanan)?", "id": "Profesi Dan Pengamanan" },
  { "en": "Apa Tugas PROPAM (Profesi Dan Pengamanan)?", "id": "Pengawasan Internal Polisi" },
  { "en": "Apa Itu Tes Skala Kematangan?", "id": "Mengukur Kedewasaan Bersikap" },
  { "en": "Apa Itu Kemampuan Interpersonal?", "id": "Kemampuan Bergaul Dengan Orang" },
  { "en": "Apa Sinonim Kata Bonafide?", "id": "Terpercaya Atau Asli" },
  { "en": "Apa Antonim Kata Palsu?", "id": "Asli" },
  { "en": "Air Membeku Pada Suhu Berapa?", "id": "Nol Derajat Celcius" },
  { "en": "Air Mendidih Pada Suhu Berapa?", "id": "Seratus Derajat Celcius" },
  { "en": "Apa Kepanjangan SAR (Search And Rescue)?", "id": "Pencarian Dan Pertolongan" },
  { "en": "Kering Berhubungan Dengan Lembab, Gelap Berhubungan Dengan?", "id": "Remang" },
  { "en": "Matahari Berhubungan Dengan Terang, Api Berhubungan Dengan?", "id": "Panas" },
  { "en": "Ulat Menjadi Kepompong, Maka Bayi Menjadi?", "id": "Remaja Atau Dewasa" },
  { "en": "Apa Sinonim Kata Insinuasi?", "id": "Sindiran" },
  { "en": "Apa Antonim Kata Gasal?", "id": "Genap" },
  { "en": "Deret Dua, Lima, Sepuluh, Tujuh Belas, Selanjutnya?", "id": "Dua Puluh Enam" },
  { "en": "Pola Deret Tadi Adalah Ditambah Ganjil, Benar?", "id": "Benar Tiga Lima Tujuh Sembilan" },
  { "en": "Nol Koma Lima Ditambah Satu Koma Dua Adalah?", "id": "Satu Koma Tujuh" },
  { "en": "Setengah Dikali Seperempat Hasilnya Adalah?", "id": "Seperdelapan" },
  { "en": "Jarak Seratus Kilometer Ditempuh Dua Jam, Kecepatannya?", "id": "Lima Puluh Kilometer Per Jam" },
  { "en": "Semua Polisi Tegap. Sebagian Polisi Berkumis. Kesimpulannya?", "id": "Sebagian Polisi Tegap Dan Berkumis" },
  { "en": "Semua Teknik Jago Hitung. Anda Teknik. Kesimpulannya?", "id": "Anda Jago Hitung" },
  { "en": "Jika Grafik Tes Kecermatan Turun, Artinya Apa?", "id": "Daya Tahan Atau Fokus Menurun" },
  { "en": "Apa Kunci Sukses Tes Kecermatan Simbol?", "id": "Cepat Teliti Dan Konsisten" },
  { "en": "Jika A Kode Satu, B Kode Dua, C Adalah?", "id": "Kode Tiga" },
  { "en": "Saya Sering Mendengar Suara Bisikan Aneh. Jawabannya?", "id": "Tidak" },
  { "en": "Saya Tidak Pernah Marah Seumur Hidup. Jawabannya?", "id": "Tidak Karena Itu Mustahil" },
  { "en": "Saya Lebih Suka Memimpin Daripada Dipimpin. Jawabannya?", "id": "Ya Menunjukkan Jiwa Pemimpin" },
  { "en": "Apakah Anda Pernah Melanggar Lalu Lintas? Jawabannya?", "id": "Pernah Tapi Jarang" },
  { "en": "Dalam Tes Kepribadian, Jawaban Berubah-Ubah Menandakan?", "id": "Kebohongan Atau Tidak Konsisten" },
  { "en": "Apa Itu Tes Pauli Dan Kraepelin?", "id": "Tes Menjumlahkan Angka Beruntun" },
  { "en": "Arah Penjumlahan Tes Pauli Adalah?", "id": "Dari Atas Ke Bawah" },
  { "en": "Arah Penjumlahan Tes Kraepelin Adalah?", "id": "Dari Bawah Ke Atas" },
  { "en": "Apa Sinonim Kata Tendensius?", "id": "Berpihak Atau Melawan" },
  { "en": "Apa Antonim Kata Kulminasi?", "id": "Nadir Atau Titik Terendah" },
  { "en": "Deret Seratus, Lima Puluh, Dua Puluh Lima, Selanjutnya?", "id": "Dua Belas Koma Lima" },
  { "en": "Satu Dekade Ditambah Satu Windu Berapa Tahun?", "id": "Delapan Belas Tahun" },
  { "en": "Harga Sepuluh Ribu Diskon Dua Puluh Persen Jadi?", "id": "Delapan Ribu Rupiah" },
  { "en": "Semua Ikan Bernapas Dengan Insang. Paus Bernapas Paru-Paru.", "id": "Paus Bukan Ikan" },
  { "en": "Logika Rotasi Gambar Sering Disebut Tes Apa?", "id": "Tes Spasial Atau Figural" },
  { "en": "Jika Kubus Diputar Sembilan Puluh Derajat Ke Kanan?", "id": "Sisi Kiri Pindah Ke Atas" },
  { "en": "Tes Kecermatan Hilang Simbol Mengukur Apa?", "id": "Ketelitian Mata Dan Ingatan" },
  { "en": "Saya Merasa Orang Lain Selalu Membicarakan Saya.", "id": "Tidak Itu Tanda Paranoid" },
  { "en": "Saya Suka Mengambil Keputusan Cepat Saat Genting.", "id": "Ya Itu Sikap Polisi" },
  { "en": "Apa Itu Tes Skala Kebohongan (Lie Detector)?", "id": "Mendeteksi Jawaban Tidak Jujur" },
  { "en": "Hewan : Kandang = Manusia : ...?", "id": "Rumah" },
  { "en": "Busur : Panah = Senapan : ...?", "id": "Peluru" },
  { "en": "Apa Kepanjangan TKP (Tempat Kejadian Perkara)?", "id": "Tempat Kejadian Perkara" },
  { "en": "Apa Kepanjangan SPN (Sekolah Polisi Negara)?", "id": "Sekolah Polisi Negara" },
  { "en": "Tiga Perempat Desimalnya Adalah?", "id": "Nol Koma Tujuh Lima" },
  { "en": "Dua Puluh Lima Persen Desimalnya Adalah?", "id": "Nol Koma Dua Lima" },
  { "en": "Jika X Lebih Besar Dari Y, Y Sama Dengan Z?", "id": "X Lebih Besar Dari Z" },
  { "en": "Apa Tujuan Soal Cerita Matematika?", "id": "Penerapan Hitungan Di Kehidupan" },
  { "en": "Saya Mudah Tersinggung Oleh Kritik Orang Lain.", "id": "Tidak Saya Terbuka Kritik" },
  { "en": "Saya Sering Sakit Kepala Tanpa Sebab Jelas.", "id": "Tidak Menunjukkan Fisik Lemah" },
  { "en": "Tes Kecermatan Membutuhkan Daya Tahan Terhadap Apa?", "id": "Rasa Bosan Dan Lelah" },
  { "en": "Apa Sinonim Kata Kognitif?", "id": "Berhubungan Dengan Akal" },
  { "en": "Apa Antonim Kata Objektif?", "id": "Subjektif" },
  { "en": "Prajurit : Senjata = Penulis : ...?", "id": "Pena Atau Komputer" },
  { "en": "Apa Kepanjangan SKEP (Surat Keputusan)?", "id": "Surat Keputusan" },
  { "en": "Apa Kepanjangan TR (Telegram Rahasia)?", "id": "Telegram Rahasia" },
  { "en": "Volume Kubus Dengan Sisi Dua Senti Adalah?", "id": "Delapan Sentimeter Kubik" },
  { "en": "Luas Lingkaran Rumusnya Adalah?", "id": "Pi Kali Jari-Jari Kuadrat" },
  { "en": "Sudut Lurus Besarnya Berapa Derajat?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Sebagian Kertas Putih. Semua Kertas Berguna. Kesimpulannya?", "id": "Sebagian Yang Berguna Itu Putih" },
  { "en": "Apa Itu Kemampuan Figural?", "id": "Logika Gambar Dan Pola" },
  { "en": "Gambar Cermin Dari Huruf B Adalah?", "id": "Huruf B Terbalik Kiri" },
  { "en": "Saya Suka Melanggar Peraturan Jika Terdesak.", "id": "Tidak Harus Taat Aturan" },
  { "en": "Saya Merasa Hidup Saya Tidak Berguna.", "id": "Tidak Tanda Depresi" },
  { "en": "Apa Arti Integritas Dalam Tes Psikologi?", "id": "Keselarasan Hati Ucapan Tindakan" },
  { "en": "Apa Sinonim Kata Residu?", "id": "Sisa Atau Ampas" },
  { "en": "Apa Antonim Kata Skeptis?", "id": "Yakin Atau Optimis" },
  { "en": "Suara : Dengar = Gambar : ...?", "id": "Lihat" },
  { "en": "Apa Kepanjangan MENWA (Resimen Mahasiswa)?", "id": "Resimen Mahasiswa" },
  { "en": "Apa Kepanjangan LANTAS (Lalu Lintas)?", "id": "Lalu Lintas" },
  { "en": "Berapa Hasil Tujuh Kuadrat Dikurang Empat Kuadrat?", "id": "Tiga Puluh Tiga" },
  { "en": "Satu Gros Ditambah Satu Lusin Berapa Buah?", "id": "Seratus Lima Puluh Enam" },
  { "en": "Apa Itu Silogisme Kategorik?", "id": "Logika Semua Dan Sebagian" },
  { "en": "Pola Gambar Segitiga, Kotak, Segi Lima, Selanjutnya?", "id": "Segi Enam" },
  { "en": "Tes Angka Hilang Melatih Kemampuan Apa?", "id": "Fokus Melihat Data Rumpang" },
  { "en": "Saya Sering Lupa Menaruh Barang Pribadi.", "id": "Tidak Saya Orang Teliti" },
  { "en": "Saya Akan Mengembalikan Dompet Yang Saya Temukan.", "id": "Ya Itu Tanda Jujur" },
  { "en": "Dalam Tes Koran, Jika Salah Coret Harus Bagaimana?", "id": "Ganti Coretan Sesuai Instruksi" },
  { "en": "Apa Itu Tes Misi Dan Visi Pribadi?", "id": "Tujuan Hidup Peserta" },
  { "en": "Apa Sinonim Kata Militan?", "id": "Bersemangat Tinggi Atau Keras" },
  { "en": "Apa Antonim Kata Prematur?", "id": "Matang Atau Tepat Waktu" },
  { "en": "Besi : Karat = Roti : ...?", "id": "Jamur" },
  { "en": "Apa Kepanjangan BRIMOB (Brigade Mobil)?", "id": "Brigade Mobil" },
  { "en": "Apa Kepanjangan POLAIRUD (Polisi Air Dan Udara)?", "id": "Polisi Air Dan Udara" },
  { "en": "Berapa Sisi Pada Bangun Limas Segi Empat?", "id": "Lima Sisi" },
  { "en": "Apa Rumus Keliling Persegi Panjang?", "id": "Dua Kali Panjang Tambah Lebar" },
  { "en": "Jika P Adalah Ibu Q, Maka Q Adalah?", "id": "Anak Dari P" },
  { "en": "Analogi Gambar Rotasi Searah Jarum Jam Artinya?", "id": "Berputar Ke Kanan" },
  { "en": "Dalam Tes Kecermatan, Kolom Mana Yang Dikerjakan Dulu?", "id": "Kolom Paling Kiri Biasanya" },
  { "en": "Saya Suka Mencoba Hal Baru Yang Menantang.", "id": "Ya Tanda Adaptif" },
  { "en": "Saya Takut Berada Di Tempat Gelap.", "id": "Tidak Polisi Harus Berani" },
  { "en": "Apa Yang Dinilai Dari Grafik Datar Di Pauli?", "id": "Kestabilan Emosi Dan Kerja" },
  { "en": "Apa Sinonim Kata Evakuasi?", "id": "Pemindahan Ke Tempat Aman" },
  { "en": "Apa Antonim Kata Vokal?", "id": "Diam Atau Pendiam" },
  { "en": "Kulit : Raba = Hidung : ...?", "id": "Cium Atau Bau" },
  { "en": "Apa Kepanjangan SABHARA (Samapta Bhayangkara)?", "id": "Samapta Bhayangkara" },
  { "en": "Apa Kepanjangan BINMAS (Pembinaan Masyarakat)?", "id": "Pembinaan Masyarakat" },
  { "en": "Dua Pangkat Tiga Ditambah Dua Pangkat Dua?", "id": "Dua Belas" },
  { "en": "Berapa Jumlah Titik Sudut Kubus?", "id": "Delapan Titik Sudut" },
  { "en": "Premis Satu Salah, Premis Dua Benar. Kesimpulan?", "id": "Biasanya Tidak Dapat Disimpulkan" },
  { "en": "Tes Figural Pencerminan Air Artinya?", "id": "Bayangan Terbalik Atas Bawah" },
  { "en": "Saya Sering Membatalkan Janji Di Menit Terakhir.", "id": "Tidak Saya Tepat Janji" },
  { "en": "Saya Tetap Tenang Meski Dimarahi Atasan.", "id": "Ya Tanda Emosi Stabil" },
  { "en": "Apa Sinonim Kata Rekonsiliasi?", "id": "Perdamaian Atau Pemulihan" },
  { "en": "Apa Antonim Kata Insomnia?", "id": "Mudah Tidur" },
  { "en": "Kamera : Foto = Mata : ...?", "id": "Pandangan Atau Lihat" },
  { "en": "Gundul : Rambut = Botak : ...?", "id": "Kepala" },
  { "en": "Deret Satu, Tiga, Sembilan, Dua Puluh Tujuh, Selanjutnya?", "id": "Delapan Puluh Satu" },
  { "en": "Pola Deret Tadi Adalah?", "id": "Dikali Tiga" },
  { "en": "Dua Pertiga Sama Dengan Berapa Persen?", "id": "Enam Puluh Enam Koma Sekian" },
  { "en": "Jika P Lebih Besar Q, Q Lebih Besar R?", "id": "P Pasti Lebih Besar R" },
  { "en": "Semua Kucing Mengeong. Harimau Tidak Mengeong.", "id": "Harimau Bukan Kucing" },
  { "en": "Apa Kepanjangan BARESKRIM (Badan Reserse Kriminal)?", "id": "Badan Reserse Kriminal" },
  { "en": "Apa Kepanjangan BAINTELKAM (Badan Intelijen Keamanan)?", "id": "Badan Intelijen Keamanan" },
  { "en": "Saya Sering Merasa Cemas Tanpa Alasan Jelas.", "id": "Tidak Tanda Jiwa Tenang" },
  { "en": "Saya Lebih Suka Bekerja Sendiri Daripada Tim.", "id": "Tidak Polisi Kerja Tim" },
  { "en": "Apa Arti Kompetensi Dalam Sikap Kerja?", "id": "Kemampuan Melakukan Tugas" },
  { "en": "Apa Itu Tes Kecerdasan Emosional?", "id": "Mengelola Emosi Diri Dan Orang" },
  { "en": "Satu Koma Lima Dikali Dua Adalah?", "id": "Tiga" },
  { "en": "Akar Kuadrat Dari Empat Puluh Sembilan Adalah?", "id": "Tujuh" },
  { "en": "Akar Kuadrat Dari Delapan Puluh Satu Adalah?", "id": "Sembilan" },
  { "en": "Apa Sinonim Kata Kuantitas?", "id": "Jumlah" },
  { "en": "Apa Antonim Kata Kualitas?", "id": "Kuantitas (Dalam Konteks Tertentu)" },
  { "en": "Presiden : Negara = Ayah : ...?", "id": "Keluarga" },
  { "en": "Masinis : Kereta = Pilot : ...?", "id": "Pesawat" },
  { "en": "Apa Arti Presisi Dalam Program Kapolri?", "id": "Prediktif Responsibilitas Transparansi Berkeadilan" },
  { "en": "Siapa Pencetus Konsep Presisi?", "id": "Jenderal Listyo Sigit Prabowo" },
  { "en": "Berapa Derajat Sudut Segitiga Sama Sisi?", "id": "Enam Puluh Derajat Tiap Sudut" },
  { "en": "Rumus Volume Balok Adalah?", "id": "Panjang Kali Lebar Kali Tinggi" },
  { "en": "Apa Itu Premis Dalam Logika?", "id": "Pernyataan Dasar Penarikan Kesimpulan" },
  { "en": "Jika Hujan Maka Basah. Tidak Basah Maka?", "id": "Tidak Hujan" },
  { "en": "Saya Pernah Berbohong Untuk Kebaikan Teman.", "id": "Tidak Utamakan Kejujuran Absolut" },
  { "en": "Saya Tidak Suka Diperintah Orang Lain.", "id": "Tidak Polisi Harus Patuh Komando" },
  { "en": "Apa Sinonim Kata Imun?", "id": "Kebal" },
  { "en": "Apa Antonim Kata Labil?", "id": "Stabil" },
  { "en": "Deret Dua, Empat, Delapan, Empat Belas, Selanjutnya?", "id": "Dua Puluh Dua" },
  { "en": "Tiga Lusin Dikurangi Satu Kodi Berapa Buah?", "id": "Enam Belas Buah" },
  { "en": "Apa Itu Tes Kepribadian EPPS?", "id": "Memilih Satu Dari Dua Pernyataan" },
  { "en": "Dalam Tes EPPS, Apakah Boleh Kosong?", "id": "Tidak Boleh Ada Yang Kosong" },
  { "en": "Apa Kepanjangan DIVHUBINTER (Divisi Hubungan Internasional)?", "id": "Divisi Hubungan Internasional" },
  { "en": "Apa Kepanjangan KORLANTAS (Korps Lalu Lintas)?", "id": "Korps Lalu Lintas" },
  { "en": "Siapa Penegak Hukum Di Perairan Indonesia?", "id": "Polairud Dan Bakamla" },
  { "en": "Telinga : Anting = Jari : ...?", "id": "Cincin" },
  { "en": "Kepala : Helm = Kaki : ...?", "id": "Sepatu Atau Kaos Kaki" },
  { "en": "Dua Ratus Dibagi Lima Adalah?", "id": "Empat Puluh" },
  { "en": "Berapa Sisi Segitiga?", "id": "Tiga Sisi" },
  { "en": "Apa Sinonim Kata Konvensional?", "id": "Umum Atau Tradisional" },
  { "en": "Apa Antonim Kata Modern?", "id": "Kuno Atau Tradisional" },
  { "en": "Saya Sering Kehilangan Minat Pada Hobi Saya.", "id": "Tidak Tanda Depresi" },
  { "en": "Saya Suka Menolong Orang Yang Kesusahan.", "id": "Ya Tanda Empati Tinggi" },
  { "en": "Apa Itu Tes Army Alpha?", "id": "Tes Mengikuti Instruksi Pendengaran" },
  { "en": "Kunci Mengerjakan Army Alpha Adalah?", "id": "Fokus Mendengarkan Perintah Narator" },
  { "en": "Berapa Nilai Sudut Satu Putaran Penuh?", "id": "Tiga Ratus Enam Puluh Derajat" },
  { "en": "Pecahan Desimal Seperlima Adalah?", "id": "Nol Koma Dua" },
  { "en": "Apa Kepanjangan KABAG (Kepala Bagian)?", "id": "Kepala Bagian" },
  { "en": "Apa Kepanjangan KASAT (Kepala Satuan)?", "id": "Kepala Satuan" },
  { "en": "Hubungan Logika Kausalitas Artinya?", "id": "Sebab Akibat" },
  { "en": "Api Menyebabkan Asap. Ada Asap Maka?", "id": "Mungkin Ada Api" },
  { "en": "Sinonim Kata Verifikasi Adalah?", "id": "Pemeriksaan Atau Pembuktian" },
  { "en": "Antonim Kata Progres Adalah?", "id": "Kemunduran Atau Stagnan" },
  { "en": "Mobil : Bensin = Manusia : ...?", "id": "Makanan" },
  { "en": "Sapi : Rumput = Harimau : ...?", "id": "Daging" },
  { "en": "Satu Kilogram Berapa Gram?", "id": "Seribu Gram" },
  { "en": "Satu Ton Berapa Kilogram?", "id": "Seribu Kilogram" },
  { "en": "Apa Kepanjangan SDM (Sumber Daya Manusia)?", "id": "Sumber Daya Manusia" },
  { "en": "Asisten SDM Kapolri Bertugas Apa?", "id": "Mengurus Kepegawaian Dan Seleksi" },
  { "en": "Saya Merasa Orang Lain Ingin Menjatuhkan Saya.", "id": "Tidak Itu Perasaan Curiga" },
  { "en": "Saya Siap Ditempatkan Di Seluruh Indonesia.", "id": "Ya Wajib Bagi Anggota Polri" },
  { "en": "Apa Itu Tes Kepribadian Minat Polisi?", "id": "Menilai Kecocokan Dengan Tugas Polisi" },
  { "en": "Apa Sinonim Kata Fleksibel?", "id": "Luwes Atau Mudah Menyesuaikan" },
  { "en": "Apa Antonim Kata Kaku?", "id": "Fleksibel Atau Luwes" },
  { "en": "Deret Lima, Sepuluh, Dua Puluh, Empat Puluh, Selanjutnya?", "id": "Delapan Puluh" },
  { "en": "Tiga Pangkat Tiga Adalah?", "id": "Dua Puluh Tujuh" },
  { "en": "Apa Kepanjangan POLDA (Kepolisian Daerah)?", "id": "Kepolisian Daerah" },
  { "en": "Apa Kepanjangan POLRES (Kepolisian Resor)?", "id": "Kepolisian Resor" },
  { "en": "Apa Kepanjangan POLSEK (Kepolisian Sektor)?", "id": "Kepolisian Sektor" },
  { "en": "Gigi : Kunyah = Jari : ...?", "id": "Pegang" },
  { "en": "Kaki : Jalan = Sayap : ...?", "id": "Terbang" },
  { "en": "Berapa Jumlah Kaki Laba-Laba?", "id": "Delapan Kaki" },
  { "en": "Apa Rumus Luas Trapesium?", "id": "Jumlah Sisi Sejajar Kali Tinggi Bagi Dua" },
  { "en": "Semua Guru Mengajar. Pak Budi Tidak Mengajar.", "id": "Pak Budi Bukan Guru" },
  { "en": "Apa Itu Distraksi Dalam Tes Psikologi?", "id": "Gangguan Konsentrasi" },
  { "en": "Saya Sering Mengalami Mimpi Buruk.", "id": "Tidak Tidur Saya Nyenyak" },
  { "en": "Saya Tidak Pernah Merasa Iri Hati.", "id": "Tidak Itu Manusiawi" },
  { "en": "Apa Arti Hierarki Dalam Kepolisian?", "id": "Tingkatan Pangkat Dan Jabatan" },
  { "en": "Apa Sinonim Kata Dedikasi?", "id": "Pengabdian" },
  { "en": "Apa Antonim Kata Apatis?", "id": "Peduli Atau Antusias" },
  { "en": "Deret Seratus, Sembilan Lima, Sembilan Puluh, Selanjutnya?", "id": "Delapan Puluh Lima" },
  { "en": "Lima Puluh Persen Dari Seratus Adalah?", "id": "Lima Puluh" },
  { "en": "Apa Kepanjangan BNN (Badan Narkotika Nasional)?", "id": "Badan Narkotika Nasional" },
  { "en": "BNN (Badan Narkotika Nasional) Bermitra Dengan?", "id": "Polri Dalam Memberantas Narkoba" },
  { "en": "Apa Itu Tes Wartegg 8 Kotak?", "id": "Melanjutkan Gambar Dari Titik Pola" },
  { "en": "Urutan Menggambar Wartegg Sebaiknya?", "id": "Acak Jangan Berurutan Terlalu Rapi" },
  { "en": "Lilin : Cair = Es : ...?", "id": "Cair" },
  { "en": "Kayu : Arang = Besi : ...?", "id": "Karat Atau Leleh" },
  { "en": "Berapa Sudut Dalam Persegi Panjang?", "id": "Sembilan Puluh Derajat" },
  { "en": "Apa Sinonim Kata Urgent?", "id": "Mendesak" },
  { "en": "Apa Antonim Kata Lambat?", "id": "Cepat" },
  { "en": "Saya Merasa Hidup Ini Membosankan.", "id": "Tidak Hidup Penuh Tantangan" },
  { "en": "Saya Selalu Menjaga Kerapihan Pakaian.", "id": "Ya Cermin Disiplin Diri" },
  { "en": "Apa Arti Korsa Dalam Polri?", "id": "Semangat Persatuan Korps" },
  { "en": "Apa Itu Mutasi Dalam Polri?", "id": "Perpindahan Tugas Atau Jabatan" },
  { "en": "Berapa Akar Kuadrat Dari Seratus Empat Puluh Empat?", "id": "Dua Belas" },
  { "en": "Apa Pola Deret Satu, Empat, Sembilan Tersebut?", "id": "Pola Bilangan Kuadrat" },
  { "en": "Jika Tiga X Sama Dengan Dua Belas, X Adalah?", "id": "Empat" },
  { "en": "Apa Sinonim Kata Intelek?", "id": "Cendekia Atau Pintar" },
  { "en": "Apa Antonim Kata Absolut?", "id": "Relatif Atau Nisbi" },
  { "en": "Haus : Air = Lapar : ...?", "id": "Nasi Atau Makanan" },
  { "en": "Gelap : Lampu = Panas : ...?", "id": "Kipas Atau Ac" },
  { "en": "Apa Kepanjangan YANMA (Pelayanan Markas)?", "id": "Pelayanan Markas" },
  { "en": "Apa Kepanjangan SPRIPIM (Staf Pribadi Pimpinan)?", "id": "Staf Pribadi Pimpinan" },
  { "en": "Saya Sering Melempar Barang Saat Marah. Jawabannya?", "id": "Tidak Tanda Emosi Buruk" },
  { "en": "Saya Merasa Ada Yang Mengikuti Saya Saat Sendirian.", "id": "Tidak Itu Halusinasi" },
  { "en": "Saya Selalu Datang Tepat Waktu Saat Janji.", "id": "Ya Tanda Disiplin" },
  { "en": "Jika Saya Menemukan Uang, Saya Akan Menyimpannya.", "id": "Tidak Harus Dikembalikan" },
  { "en": "Apa Itu Tes Kepribadian MMPI?", "id": "Tes Kesehatan Jiwa Mental" },
  { "en": "Berapa Jumlah Soal Tes MMPI Biasanya?", "id": "Lebih Dari Lima Ratus Soal" },
  { "en": "Satu Perempat Ditambah Satu Perempat Adalah?", "id": "Setengah Atau Nol Koma Lima" },
  { "en": "Nol Koma Tujuh Lima Dikurangi Nol Koma Dua Lima?", "id": "Nol Koma Lima" },
  { "en": "Apa Rumus Luas Lingkaran?", "id": "Pi Kali Jari-Jari Kuadrat" },
  { "en": "Apa Rumus Volume Kubus?", "id": "Sisi Kali Sisi Kali Sisi" },
  { "en": "Semua Burung Bertelur. Elang Adalah Burung. Kesimpulannya?", "id": "Elang Bertelur" },
  { "en": "Sebagian Makanan Manis. Semua Cokelat Manis.", "id": "Cokelat Bagian Makanan Manis" },
  { "en": "Apa Sinonim Kata Kohesi?", "id": "Kepaduan Atau Lekatan" },
  { "en": "Apa Antonim Kata Pasif?", "id": "Aktif" },
  { "en": "Kertas : Tipis = Batu : ...?", "id": "Keras" },
  { "en": "Api : Panas = Es : ...?", "id": "Dingin" },
  { "en": "Apa Kepanjangan DISPSI (Dinas Psikologi)?", "id": "Dinas Psikologi" },
  { "en": "Apa Kepanjangan DOKKES (Kedokteran Dan Kesehatan)?", "id": "Kedokteran Dan Kesehatan" },
  { "en": "Saya Pernah Mengambil Barang Kantor Untuk Pribadi.", "id": "Tidak Itu Korupsi Kecil" },
  { "en": "Saya Suka Bekerja Di Bawah Tekanan Tinggi.", "id": "Ya Tanda Tahan Stress" },
  { "en": "Apa Arti Loyalis Dalam Institusi?", "id": "Setia Pada Pimpinan Institusi" },
  { "en": "Apa Itu Tes Kecermatan Simbol Hilang?", "id": "Mencari Simbol Yang Tidak Ada" },
  { "en": "Berapa Sisi Segi Lima?", "id": "Lima Sisi" },
  { "en": "Deret Tiga, Enam, Dua Belas, Dua Puluh Empat?", "id": "Empat Puluh Delapan" },
  { "en": "Pola Deret Tiga, Enam, Dua Belas Adalah?", "id": "Dikali Dua" },
  { "en": "Apa Sinonim Kata Fluktuatif?", "id": "Naik Turun Atau Tidak Tetap" },
  { "en": "Apa Antonim Kata Mikro?", "id": "Makro Atau Besar" },
  { "en": "Penulis : Buku = Pelukis : ...?", "id": "Lukisan" },
  { "en": "Petani : Cangkul = Dokter : ...?", "id": "Stetoskop" },
  { "en": "Apa Kepanjangan SESPIM (Sekolah Staf Dan Pimpinan)?", "id": "Sekolah Staf Dan Pimpinan" },
  { "en": "Apa Kepanjangan AKPOL (Akademi Kepolisian)?", "id": "Akademi Kepolisian" },
  { "en": "Saya Merasa Orang Tua Tidak Menyayangi Saya.", "id": "Tidak Hubungan Keluarga Baik" },
  { "en": "Saya Sering Berbohong Agar Diterima Teman.", "id": "Tidak Jati Diri Kuat" },
  { "en": "Apa Itu Tes Kemampuan Manajerial?", "id": "Kemampuan Mengelola Organisasi" },
  { "en": "Apa Itu Tes Kemampuan Sosiokultural?", "id": "Kemampuan Berinteraksi Sosial" },
  { "en": "Satu Dekade Kurang Dua Tahun Berapa Tahun?", "id": "Delapan Tahun" },
  { "en": "Dua Lusin Ditambah Tiga Buah Adalah?", "id": "Dua Puluh Tujuh" },
  { "en": "Jika Arah Utara Di Atas, Timur Di Mana?", "id": "Sebelah Kanan" },
  { "en": "Jika Jam Tiga Sore, Jarum Pendek Di Mana?", "id": "Angka Tiga" },
  { "en": "Semua Logam Keras. Emas Adalah Logam. Kesimpulannya?", "id": "Emas Itu Keras" },
  { "en": "Tidak Semua Ular Berbisa. Piton Adalah Ular.", "id": "Piton Belum Tentu Berbisa" },
  { "en": "Apa Sinonim Kata Harmoni?", "id": "Keselarasan" },
  { "en": "Apa Antonim Kata Kacau?", "id": "Teratur Atau Tertib" },
  { "en": "Guru : Kapur = Tentara : ...?", "id": "Senjata Atau Peluru" },
  { "en": "Dompet : Uang = Tas : ...?", "id": "Buku Atau Barang" },
  { "en": "Apa Kepanjangan PAMOBVIT (Pengamanan Objek Vital)?", "id": "Pengamanan Objek Vital" },
  { "en": "Apa Tugas PAMOBVIT (Pengamanan Objek Vital)?", "id": "Mengamankan Lokasi Penting Negara" },
  { "en": "Saya Suka Mengganggu Binatang Saat Kecil.", "id": "Tidak Tanda Psikopat" },
  { "en": "Saya Rela Berkorban Demi Kepentingan Umum.", "id": "Ya Jiwa Pengabdian" },
  { "en": "Apa Itu Indeks Prestasi Kumulatif (IPK)?", "id": "Nilai Rata-Rata Kuliah" },
  { "en": "Berapa Syarat Minimal IPK SIPSS Biasanya?", "id": "Biasanya Dua Koma Tujuh Lima" },
  { "en": "Berapa Sudut Segitiga Siku-Siku?", "id": "Sembilan Puluh Derajat" },
  { "en": "Deret Delapan, Empat, Dua, Satu, Selanjutnya?", "id": "Setengah Atau Nol Koma Lima" },
  { "en": "Pola Deret Delapan, Empat, Dua Adalah?", "id": "Dibagi Dua" },
  { "en": "Apa Sinonim Kata Implementasi?", "id": "Pelaksanaan Atau Penerapan" },
  { "en": "Apa Antonim Kata Teori?", "id": "Praktik" },
  { "en": "Air : Ember = Pakaian : ...?", "id": "Lemari" },
  { "en": "Listrik : Kabel = Air : ...?", "id": "Pipa" },
  { "en": "Apa Kepanjangan STIK (Sekolah Tinggi Ilmu Kepolisian)?", "id": "Sekolah Tinggi Ilmu Kepolisian" },
  { "en": "PTIK Adalah Singkatan Lama Dari Apa?", "id": "Perguruan Tinggi Ilmu Kepolisian" },
  { "en": "Saya Merasa Hidup Tidak Adil Pada Saya.", "id": "Tidak Selalu Bersyukur" },
  { "en": "Saya Suka Mempelajari Budaya Daerah Lain.", "id": "Ya Wawasan Nusantara" },
  { "en": "Apa Itu Tes Wawasan Kebangsaan?", "id": "Menguji Cinta Tanah Air" },
  { "en": "Pancasila Sila Pertama Berbunyi?", "id": "Ketuhanan Yang Maha Esa" },
  { "en": "Sila Kedua Pancasila Berbunyi?", "id": "Kemanusiaan Yang Adil Beradab" },
  { "en": "Sila Ketiga Pancasila Berbunyi?", "id": "Persatuan Indonesia" },
  { "en": "Satu Milenium Berapa Tahun?", "id": "Seribu Tahun" },
  { "en": "Berapa Hari Dalam Tahun Kabisat?", "id": "Tiga Ratus Enam Puluh Enam" },
  { "en": "Semua A Adalah B. C Bukan B. Kesimpulannya?", "id": "C Bukan A" },
  { "en": "Jika Lampu Merah Menyala, Kendaraan Harus?", "id": "Berhenti" },
  { "en": "Apa Sinonim Kata Validasi?", "id": "Pengesahan" },
  { "en": "Apa Antonim Kata Ilegal?", "id": "Legal Atau Sah" },
  { "en": "Baju : Kancing = Pintu : ...?", "id": "Kunci Atau Gagang" },
  { "en": "Sepatu : Tali = Celana : ...?", "id": "Ikat Pinggang" },
  { "en": "Apa Kepanjangan HUMAS (Hubungan Masyarakat)?", "id": "Hubungan Masyarakat" },
  { "en": "Kadiv Humas Polri Berpangkat Apa?", "id": "Inspektur Jenderal Polisi" },
  { "en": "Saya Sering Pura-Pura Sakit Untuk Bolos.", "id": "Tidak Tanda Tidak Jujur" },
  { "en": "Saya Menghormati Pendapat Yang Berbeda.", "id": "Ya Sikap Toleransi" },
  { "en": "Apa Itu Kode Etik Profesi Polri?", "id": "Norma Perilaku Anggota Polri" },
  { "en": "Pelanggaran Kode Etik Disidang Oleh Siapa?", "id": "Komisi Kode Etik Polri" },
  { "en": "Berapa Derajat Sudut Tumpul?", "id": "Lebih Dari Sembilan Puluh" },
  { "en": "Deret Satu, Dua, Empat, Tujuh, Sebelas?", "id": "Enam Belas" },
  { "en": "Pola Deret Satu, Dua, Empat, Tujuh?", "id": "Tambah Satu Tambah Dua Seterusnya" },
  { "en": "Apa Sinonim Kata Kompensasi?", "id": "Ganti Rugi" },
  { "en": "Apa Antonim Kata Rugi?", "id": "Untung Atau Laba" },
  { "en": "Siang : Matahari = Malam : ...?", "id": "Bulan" },
  { "en": "Lapar : Makan = Mengantuk : ...?", "id": "Tidur" },
  { "en": "Apa Kepanjangan KASIUM (Kepala Seksi Umum)?", "id": "Kepala Seksi Umum" },
  { "en": "Apa Kepanjangan KA SPKT (Kepala Sentra Pelayanan)?", "id": "Kepala Sentra Pelayanan Kepolisian Terpadu" },
  { "en": "Berapa Dua Puluh Persen Dari Lima Ratus?", "id": "Seratus" },
  { "en": "Nol Koma Dua Lima Sama Dengan Pecahan Berapa?", "id": "Satu Perempat" },
  { "en": "Apa Sinonim Kata Anulir?", "id": "Batalkan Atau Hapus" },
  { "en": "Apa Antonim Kata Heterogen?", "id": "Homogen Atau Sejenis" },
  { "en": "Deret Dua, Tiga, Lima, Delapan, Tiga Belas?", "id": "Dua Puluh Satu" },
  { "en": "Pola Deret Dua, Tiga, Lima Tersebut Adalah?", "id": "Penjumlahan Dua Angka Sebelumnya" },
  { "en": "Mobil : Garasi = Pesawat : ...?", "id": "Hangar" },
  { "en": "Kapal : Pelabuhan = Kereta : ...?", "id": "Stasiun" },
  { "en": "Apa Kepanjangan POLWAN (Polisi Wanita)?", "id": "Polisi Wanita" },
  { "en": "Apa Kepanjangan SAMSAT (Sistem Administrasi Manunggal Satu Atap)?", "id": "Sistem Administrasi Manunggal Satu Atap" },
  { "en": "Saya Suka Menceritakan Rahasia Teman Kepada Orang Lain.", "id": "Tidak Itu Melanggar Kepercayaan" },
  { "en": "Saya Tetap Fokus Bekerja Meski Sedang Ada Masalah.", "id": "Ya Profesionalisme Kerja" },
  { "en": "Apa Itu Tes Pauli Bagian Parit?", "id": "Garis Bawah Penanda Waktu" },
  { "en": "Apa Tujuan Garis Pada Tes Pauli?", "id": "Melihat Kecepatan Per Interval" },
  { "en": "Satu Windu Berapa Tahun?", "id": "Delapan Tahun" },
  { "en": "Satu Dasawarsa Berapa Tahun?", "id": "Sepuluh Tahun" },
  { "en": "Akar Kuadrat Dari Enam Puluh Empat Adalah?", "id": "Delapan" },
  { "en": "Tiga Belas Kuadrat Adalah?", "id": "Seratus Enam Puluh Sembilan" },
  { "en": "Apa Sinonim Kata Kondusif?", "id": "Mendukung Atau Tenang" },
  { "en": "Apa Antonim Kata Destruktif?", "id": "Konstruktif Atau Membangun" },
  { "en": "Keringat : Olahraga = Asap : ...?", "id": "Api Atau Pembakaran" },
  { "en": "Hujan : Payung = Banjir : ...?", "id": "Tanggul Atau Bendungan" },
  { "en": "Apa Kepanjangan OPS (Operasi)?", "id": "Operasi" },
  { "en": "Apa Kepanjangan GIAT (Kegiatan)?", "id": "Kegiatan" },
  { "en": "Rumus Keliling Lingkaran Adalah?", "id": "Dua Kali Pi Kali Jari-Jari" },
  { "en": "Sudut Putaran Setengah Lingkaran Adalah?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Semua Dokter Ahli Kesehatan. Ayah Bukan Dokter.", "id": "Ayah Belum Tentu Ahli Kesehatan" },
  { "en": "Jika Harga Naik Maka Permintaan Turun. Harga Naik.", "id": "Maka Permintaan Turun" },
  { "en": "Saya Pernah Mengambil Uang Kembalian Yang Berlebih.", "id": "Tidak Itu Bukan Hak Saya" },
  { "en": "Saya Sering Merasa Cemas Menghadapi Masa Depan.", "id": "Tidak Saya Optimis" },
  { "en": "Apa Sinonim Kata Proteksi?", "id": "Perlindungan" },
  { "en": "Apa Antonim Kata Internal?", "id": "Eksternal" },
  { "en": "Deret Sepuluh, Dua Puluh, Empat Puluh, Delapan Puluh?", "id": "Seratus Enam Puluh" },
  { "en": "Tujuh Kali Delapan Adalah?", "id": "Lima Puluh Enam" },
  { "en": "Apa Itu Tes Gambar Orang (Draw A Person)?", "id": "Menggambar Manusia Lengkap" },
  { "en": "Apa Yang Harus Digambar Pada Tes Orang?", "id": "Manusia Utuh Dan Beraktivitas" },
  { "en": "Apa Kepanjangan BIN (Badan Intelijen Negara)?", "id": "Badan Intelijen Negara" },
  { "en": "Apa Kepanjangan KPK (Komisi Pemberantasan Korupsi)?", "id": "Komisi Pemberantasan Korupsi" },
  { "en": "Gurun : Pasir = Hutan : ...?", "id": "Pohon" },
  { "en": "Laut : Asin = Gula : ...?", "id": "Manis" },
  { "en": "Seribu Lima Ratus Bagi Tiga Adalah?", "id": "Lima Ratus" },
  { "en": "Dua Belas Tambah Tiga Belas Kali Dua?", "id": "Tiga Puluh Delapan" },
  { "en": "Apa Sinonim Kata Akseptabel?", "id": "Dapat Diterima" },
  { "en": "Apa Antonim Kata Progresif?", "id": "Konservatif Atau Mundur" },
  { "en": "Saya Sering Membanting Pintu Saat Emosi.", "id": "Tidak Pengendalian Diri Buruk" },
  { "en": "Saya Suka Membantu Orang Menyeberang Jalan.", "id": "Ya Kepedulian Sosial" },
  { "en": "Apa Itu Visi Polri Presisi?", "id": "Transformasi Menuju Polri Yang Tepat" },
  { "en": "Satu Abad Kurang Satu Dekade Berapa Tahun?", "id": "Sembilan Puluh Tahun" },
  { "en": "Satu Lustrum Berapa Tahun?", "id": "Lima Tahun" },
  { "en": "Jika Arah Jam Sembilan, Jarum Menunjuk Angka?", "id": "Sembilan" },
  { "en": "Sudut Siku-Siku Dibagi Dua Adalah?", "id": "Empat Puluh Lima Derajat" },
  { "en": "Semua Murid Belajar. Andi Tidak Belajar.", "id": "Andi Bukan Murid" },
  { "en": "Semua Kendaraan Beroda. Perahu Tidak Beroda.", "id": "Perahu Bukan Kendaraan Darat" },
  { "en": "Apa Sinonim Kata Transisi?", "id": "Peralihan" },
  { "en": "Apa Antonim Kata Feminin?", "id": "Maskulin" },
  { "en": "Sendok : Makan = Pisau : ...?", "id": "Potong" },
  { "en": "Kunci : Gembok = Password : ...?", "id": "Akun Atau Komputer" },
  { "en": "Apa Kepanjangan DENSUS 88 (Detasemen Khusus 88)?", "id": "Detasemen Khusus Delapan Puluh Delapan" },
  { "en": "DENSUS 88 (Detasemen Khusus 88) Menangani Kasus Apa?", "id": "Terorisme" },
  { "en": "Saya Merasa Orang Lain Tidak Menghargai Saya.", "id": "Tidak Itu Pikiran Negatif" },
  { "en": "Saya Selalu Mengerjakan Tugas Sampai Tuntas.", "id": "Ya Tanggung Jawab" },
  { "en": "Apa Itu Tes Papi Kostick?", "id": "Tes Kepribadian Terkait Kerja" },
  { "en": "Dalam Tes Papi Kostick, Apa Arti Huruf G?", "id": "Peran Pekerja Keras" },
  { "en": "Enam Kuadrat Ditambah Delapan Kuadrat Adalah?", "id": "Seratus" },
  { "en": "Akar Seratus Adalah?", "id": "Sepuluh" },
  { "en": "Apa Sinonim Kata Eliminasi?", "id": "Penyingkiran Atau Penghapusan" },
  { "en": "Apa Antonim Kata Mayoritas?", "id": "Minoritas" },
  { "en": "Polisi : Peluit = Wasit : ...?", "id": "Peluit Atau Kartu" },
  { "en": "Dokter : Pasien = Guru : ...?", "id": "Murid" },
  { "en": "Apa Kepanjangan SSDM (Staf Sumber Daya Manusia)?", "id": "Staf Sumber Daya Manusia" },
  { "en": "Apa Kepanjangan SOPS (Staf Operasi)?", "id": "Staf Operasi" },
  { "en": "Saya Sering Lupa Waktu Saat Bermain Game.", "id": "Tidak Saya Bisa Bagi Waktu" },
  { "en": "Saya Tidak Suka Jika Ada Perubahan Mendadak.", "id": "Tidak Saya Harus Adaptif" },
  { "en": "Apa Arti Lambang Rastra Sewakottama?", "id": "Abdi Utama Nusa Bangsa" },
  { "en": "Bunga Bangsa Dalam Polri Biasanya Mengacu Pada?", "id": "Pahlawan Polri Yang Gugur" },
  { "en": "Berapa Sisi Segi Delapan?", "id": "Delapan Sisi" },
  { "en": "Deret Lima Belas, Sepuluh, Lima, Nol?", "id": "Minus Lima" },
  { "en": "Apa Sinonim Kata Intimidasi?", "id": "Ancaman Atau Takut-Takuti" },
  { "en": "Apa Antonim Kata Objektivitas?", "id": "Subjektivitas" },
  { "en": "Emas : Tambang = Mutiara : ...?", "id": "Laut" },
  { "en": "Susu : Sapi = Telur : ...?", "id": "Ayam" },
  { "en": "Apa Kepanjangan KABID (Kepala Bidang)?", "id": "Kepala Bidang" },
  { "en": "Apa Kepanjangan KANIT (Kepala Unit)?", "id": "Kepala Unit" },
  { "en": "Apa Rumus Luas Jajar Genjang?", "id": "Alas Kali Tinggi" },
  { "en": "Berapa Derajat Jumlah Sudut Segitiga?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Sebagian Buah Masam. Jeruk Adalah Buah.", "id": "Mungkin Jeruk Itu Masam" },
  { "en": "Jika Tidak Makan Maka Lapar. Saya Tidak Lapar.", "id": "Maka Saya Sudah Makan" },
  { "en": "Saya Pernah Menyebarkan Berita Bohong (Hoax).", "id": "Tidak Itu Melanggar Hukum" },
  { "en": "Saya Menghormati Orang Yang Lebih Tua.", "id": "Ya Etika Sopan Santun" },
  { "en": "Apa Itu Intelegensi Umum?", "id": "Kemampuan Berpikir Dasar" },
  { "en": "Apa Yang Diukur Dalam Tes Daya Tahan?", "id": "Konsistensi Kinerja Di Bawah Tekanan" },
  { "en": "Berapa Nilai Pi Dalam Matematika?", "id": "Dua Puluh Dua Per Tujuh" },
  { "en": "Tiga Perdelapan Desimalnya Adalah?", "id": "Nol Koma Tiga Tujuh Lima" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Gejolak Atau Ketidaktetapan" },
  { "en": "Apa Antonim Kata Konkrit?", "id": "Abstrak" },
  { "en": "Lilin : Api = Es : ...?", "id": "Panas" },
  { "en": "Kertas : Sobek = Kaca : ...?", "id": "Pecah" },
  { "en": "Berapa Hasil Dari Dua Puluh Lima Kuadrat?", "id": "Enam Ratus Dua Puluh Lima" },
  { "en": "Satu Perdelapan Desimalnya Adalah?", "id": "Nol Koma Satu Dua Lima" },
  { "en": "Apa Sinonim Kata Komprehensif?", "id": "Menyeluruh Atau Lengkap" },
  { "en": "Apa Antonim Kata Parsial?", "id": "Komprehensif Atau Menyeluruh" },
  { "en": "Deret Satu, Tiga, Enam, Sepuluh, Lima Belas?", "id": "Dua Puluh Satu" },
  { "en": "Pola Deret Satu, Tiga, Enam Tersebut Adalah?", "id": "Penambahan Angka Berurutan" },
  { "en": "Bensin : Mobil = Listrik : ...?", "id": "Televisi Atau Elektronik" },
  { "en": "Tinta : Pena = Darah : ...?", "id": "Nadi Atau Tubuh" },
  { "en": "Apa Kepanjangan PUSLABFOR (Pusat Laboratorium Forensik)?", "id": "Pusat Laboratorium Forensik" },
  { "en": "Apa Tugas PUSLABFOR (Pusat Laboratorium Forensik)?", "id": "Pembuktian Ilmiah Tindak Pidana" },
  { "en": "Saya Sering Merasa Gemetar Saat Memegang Benda Kecil.", "id": "Tidak Tanda Saraf Stabil" },
  { "en": "Saya Tidak Pernah Mengeluh Tentang Pekerjaan.", "id": "Tidak Itu Kurang Manusiawi" },
  { "en": "Apa Itu Tes Army Beta?", "id": "Tes Intelegensi Tanpa Bahasa" },
  { "en": "Apa Yang Diuji Dalam Tes Army Beta?", "id": "Kecepatan Persepsi Visual" },
  { "en": "Dua Pertiga Dari Enam Puluh Adalah?", "id": "Empat Puluh" },
  { "en": "Empat Puluh Persen Dari Dua Ratus Adalah?", "id": "Delapan Puluh" },
  { "en": "Apa Rumus Volume Tabung?", "id": "Luas Alas Kali Tinggi" },
  { "en": "Sisi Miring Segitiga Siku-Siku Disebut?", "id": "Hipotenusa" },
  { "en": "Apa Sinonim Kata Elaborasi?", "id": "Pemaparan Secara Rinci" },
  { "en": "Apa Antonim Kata Ringkas?", "id": "Elaborasi Atau Panjang" },
  { "en": "Gunung : Mendaki = Laut : ...?", "id": "Menyelam Atau Berlayar" },
  { "en": "Burung : Udara = Ikan : ...?", "id": "Air" },
  { "en": "Apa Kepanjangan DIVTIK (Divisi Teknologi Informasi Dan Komunikasi)?", "id": "Divisi Teknologi Informasi Komunikasi" },
  { "en": "Apa Kepanjangan DIVKUM (Divisi Hukum)?", "id": "Divisi Hukum" },
  { "en": "Jika P Adalah Kakak Q, Q Adalah?", "id": "Adik P" },
  { "en": "Semua A Adalah B. Sebagian B Adalah C.", "id": "Sebagian A Mungkin C" },
  { "en": "Saya Pernah Mencontek Saat Ujian Sekolah.", "id": "Pernah Tapi Sudah Berubah" },
  { "en": "Saya Merasa Orang Lain Selalu Salah.", "id": "Tidak Itu Sombong" },
  { "en": "Apa Sinonim Kata Stimulus?", "id": "Rangsangan Atau Dorongan" },
  { "en": "Apa Antonim Kata Respon?", "id": "Stimulus" },
  { "en": "Deret Seratus, Delapan Puluh, Enam Puluh, Empat Puluh?", "id": "Dua Puluh" },
  { "en": "Sembilan Kali Sembilan Bagi Sembilan?", "id": "Sembilan" },
  { "en": "Apa Itu Tes Kepribadian 16 PF?", "id": "Enam Belas Faktor Kepribadian" },
  { "en": "Faktor Dominance Dalam Kepribadian Artinya?", "id": "Sifat Dominan Atau Memimpin" },
  { "en": "Apa Kepanjangan KABRESKRIM (Kepala Badan Reserse Kriminal)?", "id": "Kepala Badan Reserse Kriminal" },
  { "en": "KABRESKRIM (Kepala Badan Reserse Kriminal) Berpangkat Apa?", "id": "Komisaris Jenderal Polisi" },
  { "en": "Ulat : Kepompong = Bayi : ...?", "id": "Anak-Anak" },
  { "en": "Malam : Gelap = Siang : ...?", "id": "Terang" },
  { "en": "Seribu Dikurang Dua Ratus Lima Puluh?", "id": "Tujuh Ratus Lima Puluh" },
  { "en": "Tiga Belas Tambah Tujuh Belas Kali Satu?", "id": "Tiga Puluh" },
  { "en": "Apa Sinonim Kata Fundamental?", "id": "Mendasar Atau Pokok" },
  { "en": "Apa Antonim Kata Sekunder?", "id": "Primer Atau Utama" },
  { "en": "Saya Sering Tertidur Saat Bekerja Atau Belajar.", "id": "Tidak Saya Punya Stamina" },
  { "en": "Saya Akan Melaporkan Teman Yang Melanggar Aturan.", "id": "Ya Integritas Penegak Hukum" },
  { "en": "Apa Arti Diskresi Kepolisian?", "id": "Tindakan Atas Penilaian Sendiri" },
  { "en": "Diskresi Dilakukan Dalam Keadaan Apa?", "id": "Keadaan Mendesak Demi Umum" },
  { "en": "Satu Abad Ditambah Satu Windu?", "id": "Seratus Delapan Tahun" },
  { "en": "Berapa Jumlah Hari Bulan Agustus?", "id": "Tiga Puluh Satu Hari" },
  { "en": "Jika Kemarin Jumat, Besok Lusa Adalah?", "id": "Senin" },
  { "en": "Jam Sepuluh Lebih Sepuluh Menit, Jarum Panjang?", "id": "Di Angka Dua" },
  { "en": "Semua Mobil Butuh Bensin. Sepeda Tidak Butuh Bensin.", "id": "Sepeda Bukan Mobil" },
  { "en": "Semua Koruptor Jahat. Sebagian Pejabat Koruptor.", "id": "Sebagian Pejabat Jahat" },
  { "en": "Apa Sinonim Kata Integritas?", "id": "Kejujuran" },
  { "en": "Apa Antonim Kata Munafik?", "id": "Jujur Atau Ikhlas" },
  { "en": "Kayu : Kursi = Tanah Liat : ...?", "id": "Gerabah Atau Bata" },
  { "en": "Kapas : Baju = Kulit : ...?", "id": "Sepatu Atau Tas" },
  { "en": "Apa Kepanjangan DVI (Disaster Victim Identification)?", "id": "Disaster Victim Identification" },
  { "en": "Apa Fungsi DVI (Disaster Victim Identification) Polri?", "id": "Identifikasi Korban Bencana Massal" },
  { "en": "Saya Merasa Gugup Jika Diperhatikan Orang Banyak.", "id": "Tidak Saya Percaya Diri" },
  { "en": "Saya Selalu Menerima Keputusan Rapat Dengan Lapang Dada.", "id": "Ya Kerjasama Tim" },
  { "en": "Apa Itu Tes Minat Holland?", "id": "Tes Tipe Kepribadian Kerja" },
  { "en": "Tipe Realistik Dalam Tes Holland Artinya?", "id": "Suka Bekerja Dengan Alat" },
  { "en": "Tujuh Kuadrat Dikurangi Lima Kuadrat Adalah?", "id": "Dua Puluh Empat" },
  { "en": "Akar Dari Dua Ratus Lima Puluh Enam?", "id": "Enam Belas" },
  { "en": "Apa Sinonim Kata Militansi?", "id": "Ketangguhan Dalam Berjuang" },
  { "en": "Apa Antonim Kata Menyerah?", "id": "Bertahan Atau Melawan" },
  { "en": "Buku : Ilmu = Uang : ...?", "id": "Kekayaan Atau Belanja" },
  { "en": "Kaki : Sepak = Tangan : ...?", "id": "Pukul Atau Tangkap" },
  { "en": "Apa Kepanjangan ASLOG (Asisten Logistik)?", "id": "Asisten Logistik" },
  { "en": "Apa Kepanjangan ASRENA (Asisten Perencanaan)?", "id": "Asisten Perencanaan" },
  { "en": "Saya Suka Membatalkan Janji Jika Malas.", "id": "Tidak Saya Konsisten" },
  { "en": "Saya Tidak Mudah Putus Asa Saat Gagal.", "id": "Ya Mental Baja" },
  { "en": "Apa Itu Bhayangkara Pembina Keamanan Ketertiban Masyarakat?", "id": "Bhabinkamtibmas" },
  { "en": "Apa Kepanjangan BABINSA (Bintara Pembina Desa)?", "id": "Bintara Pembina Desa (TNI)" },
  { "en": "Berapa Sudut Dalam Segi Enam Beraturan?", "id": "Seratus Dua Puluh Derajat" },
  { "en": "Deret Nol, Tiga, Delapan, Lima Belas?", "id": "Dua Puluh Empat" },
  { "en": "Apa Sinonim Kata Krusial?", "id": "Sangat Penting" },
  { "en": "Apa Antonim Kata Trivial?", "id": "Penting Atau Krusial" },
  { "en": "Gelap : Takut = Terang : ...?", "id": "Berani Atau Jelas" },
  { "en": "Sakit : Obat = Lapar : ...?", "id": "Nasi Atau Makan" },
  { "en": "Apa Kepanjangan KABID PROPAM (Kepala Bidang Propam)?", "id": "Kepala Bidang Propam" },
  { "en": "Apa Kepanjangan KABID HUMAS (Kepala Bidang Humas)?", "id": "Kepala Bidang Humas" },
  { "en": "Apa Rumus Luas Belah Ketupat?", "id": "Setengah Kali Diagonal Satu Dua" },
  { "en": "Jumlah Sisi Pada Balok Adalah?", "id": "Enam Sisi" },
  { "en": "Jika Semua A Adalah B, Dan C Adalah A?", "id": "Maka C Adalah B" },
  { "en": "Tidak Ada Manusia Yang Abadi. Saya Manusia.", "id": "Saya Tidak Abadi" },
  { "en": "Saya Pernah Mengambil Barang Teman Tanpa Izin.", "id": "Tidak Itu Mencuri" },
  { "en": "Saya Suka Kerapian Di Meja Kerja.", "id": "Ya Tanda Terorganisir" },
  { "en": "Apa Itu Tes Kepribadian MBTI?", "id": "Tes Tipe Psikologis Myers Briggs" },
  { "en": "Tipe Extrovert Artinya?", "id": "Terbuka Dan Suka Bergaul" },
  { "en": "Tiga Perlima Desimalnya Adalah?", "id": "Nol Koma Enam" },
  { "en": "Tujuh Puluh Lima Persen Pecahannya Adalah?", "id": "Tiga Perempat" },
  { "en": "Apa Sinonim Kata Fluktuatif?", "id": "Tidak Stabil" },
  { "en": "Apa Antonim Kata Permanen?", "id": "Sementara" },
  { "en": "Garam : Asin = Cabai : ...?", "id": "Pedas" },
  { "en": "Es : Dingin = Api : ...?", "id": "Panas" },
  { "en": "Berapa Hasil Setengah Ditambah Sepertiga?", "id": "Lima Per Enam" },
  { "en": "Nol Koma Satu Dikali Nol Koma Satu?", "id": "Nol Koma Nol Satu" },
  { "en": "Apa Sinonim Kata Empiris?", "id": "Berdasarkan Pengalaman Atau Fakta" },
  { "en": "Apa Antonim Kata Teoritis?", "id": "Praktis Atau Empiris" },
  { "en": "Deret Sembilan, Delapan Belas, Tiga Puluh Enam?", "id": "Tujuh Puluh Dua" },
  { "en": "Pola Deret Sembilan, Delapan Belas Tersebut Adalah?", "id": "Dikali Dua" },
  { "en": "Teleskop : Bintang = Mikroskop : ...?", "id": "Bakteri Atau Kuman" },
  { "en": "Pancing : Ikan = Senapan : ...?", "id": "Buruan Atau Hewan" },
  { "en": "Apa Kepanjangan INAFIS (Indonesia Automatic Fingerprint Identification System)?", "id": "Sistem Identifikasi Sidik Jari Otomatis" },
  { "en": "Apa Tugas Utama INAFIS Polri?", "id": "Identifikasi Sidik Jari Di TKP" },
  { "en": "Saya Tidak Pernah Merasa Iri Pada Siapapun.", "id": "Tidak Itu Jawaban Malaikat" },
  { "en": "Saya Suka Bekerja Dengan Target Yang Jelas.", "id": "Ya Berorientasi Hasil" },
  { "en": "Apa Itu Tes Kepribadian EPPS Bagian N-Ach?", "id": "Kebutuhan Akan Prestasi" },
  { "en": "Apa Itu Tes Kepribadian EPPS Bagian N-Aff?", "id": "Kebutuhan Akan Afiliasi Atau Teman" },
  { "en": "Dua Pertiga Dibagi Satu Pertiga Adalah?", "id": "Dua" },
  { "en": "Satu Juta Nolnya Ada Berapa?", "id": "Enam Nol" },
  { "en": "Apa Rumus Luas Permukaan Kubus?", "id": "Enam Kali Sisi Kali Sisi" },
  { "en": "Garis Yang Membelah Lingkaran Jadi Dua Disebut?", "id": "Diameter" },
  { "en": "Apa Sinonim Kata Indikasi?", "id": "Petunjuk Atau Tanda" },
  { "en": "Apa Antonim Kata Kabur?", "id": "Jelas Atau Terang" },
  { "en": "Domba : Wol = Ulat : ...?", "id": "Sutra" },
  { "en": "Lebah : Madu = Sapi : ...?", "id": "Susu" },
  { "en": "Apa Kepanjangan NCB (National Central Bureau) Interpol?", "id": "Biro Pusat Nasional Interpol" },
  { "en": "Markas Besar Interpol Berada Di Negara Mana?", "id": "Perancis Kota Lyon" },
  { "en": "Jika Semua Maling Ditangkap. Budi Tidak Ditangkap.", "id": "Budi Bukan Maling" },
  { "en": "Semua Tentara Berani. Sebagian Orang Berani Mati.", "id": "Sebagian Tentara Berani Mati" },
  { "en": "Saya Sering Merasa Sedih Tanpa Sebab.", "id": "Tidak Tanda Depresi" },
  { "en": "Saya Selalu Memaafkan Orang Yang Menyakiti Saya.", "id": "Ya Tanda Kedewasaan Emosi" },
  { "en": "Apa Sinonim Kata Referensi?", "id": "Rujukan Atau Acuan" },
  { "en": "Apa Antonim Kata Absurd?", "id": "Logis Atau Masuk Akal" },
  { "en": "Deret Satu, Delapan, Dua Puluh Tujuh, Enam Puluh Empat?", "id": "Seratus Dua Puluh Lima" },
  { "en": "Pola Deret Satu, Delapan, Dua Puluh Tujuh?", "id": "Pangkat Tiga Bilangan Asli" },
  { "en": "Apa Itu Tes Pauli Bagian Puncak?", "id": "Prestasi Tertinggi Dalam Kerja" },
  { "en": "Apa Itu Tes Pauli Bagian Jatuh?", "id": "Penurunan Kinerja Atau Kelelahan" },
  { "en": "Apa Kepanjangan FPU (Formed Police Unit)?", "id": "Satuan Tugas Polisi Perdamaian" },
  { "en": "Pasukan Garuda Bhayangkara Dikirim Oleh Siapa?", "id": "Polri Ke Misi PBB" },
  { "en": "Gitar : Petik = Piano : ...?", "id": "Tekan" },
  { "en": "Terompet : Tiup = Drum : ...?", "id": "Pukul" },
  { "en": "Berapa Derajat Sudut Segi Lima Beraturan?", "id": "Seratus Delapan Derajat" },
  { "en": "Satu Koma Dua Dikali Tiga Adalah?", "id": "Tiga Koma Enam" },
  { "en": "Apa Sinonim Kata Kronologi?", "id": "Urutan Waktu Kejadian" },
  { "en": "Apa Antonim Kata Acak?", "id": "Urut Atau Sistematis" },
  { "en": "Saya Suka Mengkritik Teman Di Depan Umum.", "id": "Tidak Itu Tidak Sopan" },
  { "en": "Saya Siap Bekerja Di Hari Libur.", "id": "Ya Loyalitas Tinggi" },
  { "en": "Apa Arti Diskriminasi Dalam Pelayanan?", "id": "Membedakan Perlakuan Kepada Masyarakat" },
  { "en": "Polisi Boleh Melakukan Diskriminasi? Jawabannya?", "id": "Tidak Boleh Sama Sekali" },
  { "en": "Satu Abad Dibagi Dua Dekade?", "id": "Lima" },
  { "en": "Berapa Jumlah Provinsi Di Pulau Sumatera?", "id": "Sepuluh Provinsi" },
  { "en": "Jika Arah Jam Tiga, Jarum Panjang Di Mana?", "id": "Angka Dua Belas" },
  { "en": "Jarum Pendek Di Angka Enam, Jarum Panjang Dua Belas?", "id": "Jam Enam Tepat" },
  { "en": "Semua Mamalia Menyusui. Paus Menyusui.", "id": "Paus Termasuk Mamalia" },
  { "en": "Tidak Ada Api Tanpa Asap. Ada Asap.", "id": "Pasti Ada Api" },
  { "en": "Apa Sinonim Kata Fisiologi?", "id": "Ilmu Fungsi Tubuh" },
  { "en": "Apa Antonim Kata Rohani?", "id": "Jasmani Atau Fisik" },
  { "en": "Dosen : Mahasiswa = Guru : ...?", "id": "Siswa Atau Murid" },
  { "en": "Sopir : Mobil = Kusir : ...?", "id": "Delman Atau Kereta Kuda" },
  { "en": "Apa Kepanjangan BA (Berita Acara)?", "id": "Berita Acara" },
  { "en": "Apa Itu BAP (Berita Acara Pemeriksaan)?", "id": "Catatan Hasil Pemeriksaan Penyidik" },
  { "en": "Saya Merasa Gugup Saat Bertemu Orang Baru.", "id": "Tidak Saya Mudah Bergaul" },
  { "en": "Saya Selalu Memeriksa Pekerjaan Sebelum Diserahkan.", "id": "Ya Teliti Dan Cermat" },
  { "en": "Apa Itu Tes Gambar Pohon Berbuah?", "id": "Menunjukkan Produktivitas Seseorang" },
  { "en": "Apa Makna Akar Dalam Tes Pohon?", "id": "Kestabilan Dan Pegangan Hidup" },
  { "en": "Delapan Kali Delapan Dikurang Empat?", "id": "Enam Puluh" },
  { "en": "Akar Seratus Sembilan Puluh Enam Adalah?", "id": "Empat Belas" },
  { "en": "Apa Sinonim Kata Hierarki?", "id": "Tingkatan Atau Jenjang" },
  { "en": "Apa Antonim Kata Setara?", "id": "Bertingkat Atau Hierarkis" },
  { "en": "Beras : Nasi = Gandum : ...?", "id": "Roti Atau Tepung" },
  { "en": "Air : Uap = Kayu : ...?", "id": "Asap Atau Arang" },
  { "en": "Apa Kepanjangan KAKORLANTAS (Kepala Korps Lalu Lintas)?", "id": "Kepala Korps Lalu Lintas" },
  { "en": "Apa Kepanjangan KAPOLDA (Kepala Kepolisian Daerah)?", "id": "Kepala Kepolisian Daerah" },
  { "en": "Saya Suka Menunda Pekerjaan Sulit.", "id": "Tidak Saya Hadapi Tantangan" },
  { "en": "Saya Tidak Mudah Marah Pada Hal Sepele.", "id": "Ya Kontrol Emosi Baik" },
  { "en": "Apa Itu Tribrata Sebagai Pedoman Hidup?", "id": "Tiga Janji Prajurit Bhayangkara" },
  { "en": "Apa Itu Catur Prasetya Sebagai Pedoman Kerja?", "id": "Empat Janji Kerja Polisi" },
  { "en": "Berapa Sisi Kubus?", "id": "Enam Sisi" },
  { "en": "Deret Tiga Puluh, Dua Puluh Lima, Dua Puluh?", "id": "Lima Belas" },
  { "en": "Apa Sinonim Kata Intervensi?", "id": "Campur Tangan" },
  { "en": "Apa Antonim Kata Membiarkan?", "id": "Mencampuri Atau Intervensi" },
  { "en": "Kertas : Buku = Benang : ...?", "id": "Kain" },
  { "en": "Bata : Dinding = Lidi : ...?", "id": "Sapu" },
  { "en": "Apa Kepanjangan KA SPN (Kepala Sekolah Polisi Negara)?", "id": "Kepala Sekolah Polisi Negara" },
  { "en": "Apa Kepanjangan DANKORBRIMOB (Komandan Korps Brimob)?", "id": "Komandan Korps Brimob" },
  { "en": "Rumus Keliling Persegi Adalah?", "id": "Empat Kali Sisi" },
  { "en": "Satu Liter Sama Dengan Berapa Mililiter?", "id": "Seribu Mililiter" },
  { "en": "Jika P Adalah Q, Dan Q Adalah R?", "id": "P Adalah R" },
  { "en": "Semua Karyawan Dasi. Satpam Karyawan.", "id": "Satpam Pakai Dasi" },
  { "en": "Saya Pernah Mengambil Uang Orang Tua Tanpa Izin.", "id": "Tidak Itu Mencuri" },
  { "en": "Saya Suka Kebersihan Lingkungan.", "id": "Ya Tanda Disiplin" },
  { "en": "Apa Itu Tes Kraepelin Arah Atas?", "id": "Menjumlahkan Angka Ke Atas" },
  { "en": "Apa Yang Dinilai Jika Grafik Kraepelin Menurun?", "id": "Kelelahan Atau Kurang Tahan" },
  { "en": "Tiga Perempat Dikurangi Satu Perempat?", "id": "Setengah Atau Dua Perempat" },
  { "en": "Sembilan Puluh Persen Desimalnya?", "id": "Nol Koma Sembilan" },
  { "en": "Apa Sinonim Kata Klarifikasi?", "id": "Penjelasan Atau Penjernihan" },
  { "en": "Apa Antonim Kata Bingung?", "id": "Jelas Atau Paham" },
  { "en": "Jarum : Jahit = Gunting : ...?", "id": "Potong" },
  { "en": "Cangkul : Tanah = Gergaji : ...?", "id": "Kayu" },
  { "en": "Berapa Hasil Dari Lima Belas Kali Empat?", "id": "Enam Puluh" },
  { "en": "Seperempat Desimalnya Adalah?", "id": "Nol Koma Dua Lima" },
  { "en": "Apa Sinonim Kata Absolut?", "id": "Mutlak" },
  { "en": "Apa Antonim Kata Abstrak?", "id": "Nyata Atau Konkret" },
  { "en": "Deret Dua, Lima, Delapan, Sebelas, Selanjutnya?", "id": "Empat Belas" },
  { "en": "Pola Deret Dua, Lima, Delapan Tersebut Adalah?", "id": "Ditambah Tiga" },
  { "en": "Telinga : Mendengar = Mata : ...?", "id": "Melihat" },
  { "en": "Kaki : Berjalan = Sayap : ...?", "id": "Terbang" },
  { "en": "Apa Kepanjangan DITLANTAS (Direktorat Lalu Lintas)?", "id": "Direktorat Lalu Lintas" },
  { "en": "Apa Kepanjangan DITRESKRIMUM (Direktorat Reserse Kriminal Umum)?", "id": "Direktorat Reserse Kriminal Umum" },
  { "en": "Saya Sering Merasa Orang Lain Membenci Saya.", "id": "Tidak Itu Perasaan Negatif" },
  { "en": "Saya Selalu Menyelesaikan Tugas Tepat Waktu.", "id": "Ya Tanda Disiplin" },
  { "en": "Apa Itu Tes Army Alpha Bagian Instruksi?", "id": "Mengikuti Perintah Lisan Narator" },
  { "en": "Kunci Utama Tes Army Alpha Adalah?", "id": "Konsentrasi Pendengaran" },
  { "en": "Dua Pertiga Dikali Sembilan Adalah?", "id": "Enam" },
  { "en": "Seratus Dibagi Empat Adalah?", "id": "Dua Puluh Lima" },
  { "en": "Apa Rumus Luas Segitiga Siku-Siku?", "id": "Setengah Alas Kali Tinggi" },
  { "en": "Jumlah Sudut Segitiga Adalah?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Apa Sinonim Kata Adaptasi?", "id": "Penyesuaian" },
  { "en": "Apa Antonim Kata Kaku?", "id": "Luwes Atau Fleksibel" },
  { "en": "Gelas : Minum = Piring : ...?", "id": "Makan" },
  { "en": "Pisau : Iris = Palu : ...?", "id": "Pukul" },
  { "en": "Apa Kepanjangan DITRESKRIMSUS (Direktorat Reserse Kriminal Khusus)?", "id": "Direktorat Reserse Kriminal Khusus" },
  { "en": "Apa Kepanjangan DITINTELKAM (Direktorat Intelijen Keamanan)?", "id": "Direktorat Intelijen Keamanan" },
  { "en": "Jika A Lebih Tinggi B, B Lebih Tinggi C?", "id": "A Paling Tinggi" },
  { "en": "Semua Guru Sabar. Pak Ali Guru.", "id": "Pak Ali Sabar" },
  { "en": "Saya Pernah Mengambil Barang Toko Tanpa Bayar.", "id": "Tidak Itu Mencuri" },
  { "en": "Saya Merasa Hidup Ini Penuh Makna.", "id": "Ya Berpikir Positif" },
  { "en": "Apa Sinonim Kata Akurat?", "id": "Tepat Dan Benar" },
  { "en": "Apa Antonim Kata Meleset?", "id": "Tepat Sasaran" },
  { "en": "Deret Seratus, Sembilan Puluh Lima, Sembilan Puluh?", "id": "Delapan Puluh Lima" },
  { "en": "Tiga Pangkat Dua Ditambah Empat Pangkat Dua?", "id": "Dua Puluh Lima" },
  { "en": "Apa Itu Tes Kreapelin Bagian Puncak?", "id": "Kecepatan Tertinggi Peserta" },
  { "en": "Apa Itu Tes Kreapelin Bagian Lembah?", "id": "Kecepatan Terendah Peserta" },
  { "en": "Apa Kepanjangan DITSABHARA (Direktorat Samapta Bhayangkara)?", "id": "Direktorat Samapta Bhayangkara" },
  { "en": "Apa Kepanjangan DITPAMOBVIT (Direktorat Pengamanan Objek Vital)?", "id": "Direktorat Pengamanan Objek Vital" },
  { "en": "Mobil : Roda = Perahu : ...?", "id": "Dayung Atau Layar" },
  { "en": "Kereta : Rel = Bis : ...?", "id": "Jalan Raya" },
  { "en": "Berapa Derajat Sudut Siku-Siku?", "id": "Sembilan Puluh Derajat" },
  { "en": "Satu Setengah Ditambah Setengah Adalah?", "id": "Dua" },
  { "en": "Apa Sinonim Kata Konsisten?", "id": "Tetap Atau Taat Asas" },
  { "en": "Apa Antonim Kata Berubah?", "id": "Tetap Atau Konsisten" },
  { "en": "Saya Suka Memaki Orang Saat Macet.", "id": "Tidak Tanda Emosi Buruk" },
  { "en": "Saya Siap Ditempatkan Di Daerah Terpencil.", "id": "Ya Syarat Mutlak Polri" },
  { "en": "Apa Arti Pelayanan Prima?", "id": "Pelayanan Terbaik Untuk Masyarakat" },
  { "en": "Apa Itu Zona Integritas Polri?", "id": "Wilayah Bebas Korupsi" },
  { "en": "Satu Dasawarsa Ditambah Satu Lustrum?", "id": "Lima Belas Tahun" },
  { "en": "Berapa Jumlah Hari Bulan Februari Kabisat?", "id": "Dua Puluh Sembilan Hari" },
  { "en": "Jika Arah Utara Di Depan, Barat Di Mana?", "id": "Sebelah Kiri" },
  { "en": "Jarum Jam Sembilan Malam Menunjukkan Angka?", "id": "Sembilan Dan Dua Belas" },
  { "en": "Semua Ular Melata. Kobra Adalah Ular.", "id": "Kobra Melata" },
  { "en": "Tidak Ada Gading Yang Tak Retak Artinya?", "id": "Tidak Ada Yang Sempurna" },
  { "en": "Apa Sinonim Kata Objektif?", "id": "Sesuai Fakta" },
  { "en": "Apa Antonim Kata Subjektif?", "id": "Objektif" },
  { "en": "Guru : Sekolah = Dokter : ...?", "id": "Rumah Sakit" },
  { "en": "Petani : Sawah = Nelayan : ...?", "id": "Laut" },
  { "en": "Apa Kepanjangan DITPOLAIRUD (Direktorat Polisi Air Udara)?", "id": "Direktorat Polisi Air Udara" },
  { "en": "Apa Kepanjangan DITBINMAS (Direktorat Pembinaan Masyarakat)?", "id": "Direktorat Pembinaan Masyarakat" },
  { "en": "Saya Merasa Orang Lain Selalu Salah Paham.", "id": "Tidak Introspeski Diri" },
  { "en": "Saya Selalu Menepati Janji Kepada Teman.", "id": "Ya Tanda Integritas" },
  { "en": "Apa Itu Tes Gambar Wartegg Stimulus Melengkung?", "id": "Menggambar Benda Hidup Atau Dinamis" },
  { "en": "Apa Itu Tes Gambar Wartegg Stimulus Kotak?", "id": "Menggambar Benda Mati Atau Teknis" },
  { "en": "Lima Kali Enam Bagi Dua?", "id": "Lima Belas" },
  { "en": "Akar Delapan Puluh Satu Adalah?", "id": "Sembilan" },
  { "en": "Apa Sinonim Kata Efektif?", "id": "Berhasil Guna" },
  { "en": "Apa Antonim Kata Sia-Sia?", "id": "Berguna Atau Efektif" },
  { "en": "Kertas : Pena = Papan Tulis : ...?", "id": "Spidol Atau Kapur" },
  { "en": "Baju : Kain = Meja : ...?", "id": "Kayu" },
  { "en": "Apa Kepanjangan BIDPROPAM (Bidang Profesi Dan Pengamanan)?", "id": "Bidang Profesi Dan Pengamanan" },
  { "en": "Apa Kepanjangan BIDHUMAS (Bidang Hubungan Masyarakat)?", "id": "Bidang Hubungan Masyarakat" },
  { "en": "Saya Suka Berbohong Untuk Menghindari Hukuman.", "id": "Tidak Harus Berani Tanggung Jawab" },
  { "en": "Saya Tidak Mudah Tersinggung Oleh Candaan.", "id": "Ya Mental Kuat" },
  { "en": "Apa Itu Sumpah Pemuda?", "id": "Ikrar Bersatu Tanah Air Bahasa" },
  { "en": "Tanggal Berapa Sumpah Pemuda?", "id": "Dua Puluh Delapan Oktober" },
  { "en": "Berapa Sisi Persegi Panjang?", "id": "Empat Sisi" },
  { "en": "Deret Sepuluh, Dua Puluh, Tiga Puluh, Empat Puluh?", "id": "Lima Puluh" },
  { "en": "Apa Sinonim Kata Relevan?", "id": "Sesuai Atau Berkaitan" },
  { "en": "Apa Antonim Kata Bertentangan?", "id": "Selaras Atau Relevan" },
  { "en": "Sepatu : Kaki = Sarung Tangan : ...?", "id": "Tangan" },
  { "en": "Topi : Kepala = Kacamata : ...?", "id": "Mata" },
  { "en": "Apa Kepanjangan BIDKUM (Bidang Hukum)?", "id": "Bidang Hukum" },
  { "en": "Apa Kepanjangan BIDTIK (Bidang Teknologi Informasi Komunikasi)?", "id": "Bidang Teknologi Informasi Komunikasi" },
  { "en": "Rumus Luas Persegi Panjang Adalah?", "id": "Panjang Kali Lebar" },
  { "en": "Satu Kilogram Sama Dengan Berapa Ons?", "id": "Sepuluh Ons" },
  { "en": "Jika Hari Ini Rabu, Tiga Hari Lagi?", "id": "Sabtu" },
  { "en": "Semua Burung Punya Sayap. Ayam Punya Sayap.", "id": "Ayam Adalah Burung (Kategori)" },
  { "en": "Saya Pernah Mencuri Uang Teman Saat Kecil.", "id": "Jujur Saja Jika Pernah" },
  { "en": "Saya Suka Kerapian Dan Kebersihan.", "id": "Ya Tanda Teratur" },
  { "en": "Apa Itu Tes Pauli Bagian Salah Coret?", "id": "Koreksi Ketelitian Kerja" },
  { "en": "Apa Yang Dinilai Jika Grafik Pauli Naik?", "id": "Semangat Kerja Meningkat" },
  { "en": "Satu Perdua Ditambah Satu Pertiga?", "id": "Lima Per Enam" },
  { "en": "Delapan Puluh Persen Desimalnya?", "id": "Nol Koma Delapan" },
  { "en": "Apa Sinonim Kata Spesifik?", "id": "Khusus" },
  { "en": "Apa Antonim Kata Umum?", "id": "Khusus Atau Spesifik" },
  { "en": "Obeng : Sekrup = Palu : ...?", "id": "Paku" },
  { "en": "Gunting : Kertas = Gergaji : ...?", "id": "Kayu" },
  { "en": "Deret Dua, Tiga, Lima, Tujuh, Sebelas?", "id": "Tiga Belas" },
  { "en": "Apa Pola Deret Dua, Tiga, Lima Tersebut?", "id": "Bilangan Prima" },
  { "en": "Berapa Dua Puluh Lima Persen Dari Satu Juta?", "id": "Dua Ratus Lima Puluh Ribu" },
  { "en": "Satu Setengah Jam Sama Dengan Berapa Menit?", "id": "Sembilan Puluh Menit" },
  { "en": "Apa Sinonim Kata Stagnan?", "id": "Mandeg Atau Tidak Bergerak" },
  { "en": "Apa Antonim Kata Dinamis?", "id": "Statis Atau Diam" },
  { "en": "Termometer : Suhu = Barometer : ...?", "id": "Tekanan Udara" },
  { "en": "Jam : Waktu = Timbangan : ...?", "id": "Berat Atau Massa" },
  { "en": "Apa Kepanjangan TAUD (Tata Urusan Dalam)?", "id": "Tata Urusan Dalam" },
  { "en": "Saya Tidak Pernah Berkata Kasar Seumur Hidup.", "id": "Tidak Itu Mustahil" },
  { "en": "Saya Mampu Mengendalikan Diri Saat Diprovokasi.", "id": "Ya Kematangan Emosi" },
  { "en": "Apa Itu Tes Army Alpha Bagian Angka?", "id": "Menjumlahkan Angka Sesuai Instruksi" },
  { "en": "Apa Kunci Tes Army Alpha?", "id": "Jangan Mendahului Instruksi Narator" },
  { "en": "Dua Pangkat Empat Adalah?", "id": "Enam Belas" },
  { "en": "Akar Seratus Dua Puluh Satu Adalah?", "id": "Sebelas" },
  { "en": "Apa Rumus Volume Limas?", "id": "Sepertiga Luas Alas Kali Tinggi" },
  { "en": "Sudut Putaran Penuh Lingkaran Adalah?", "id": "Tiga Ratus Enam Puluh Derajat" },
  { "en": "Apa Sinonim Kata Kredibilitas?", "id": "Kepercayaan" },
  { "en": "Apa Antonim Kata Meragukan?", "id": "Meyakinkan Atau Kredibel" },
  { "en": "Kamera : Lensa = Manusia : ...?", "id": "Mata" },
  { "en": "Jantung : Pompa = Otak : ...?", "id": "Pikir Atau Kontrol" },
  { "en": "Apa Kepanjangan SETUM (Sekretariat Umum)?", "id": "Sekretariat Umum" },
  { "en": "Apa Kepanjangan SESPIMMEN (Sekolah Staf Pimpinan Menengah)?", "id": "Sekolah Staf Pimpinan Menengah" },
  { "en": "Jika Semua A Adalah B. Sebagian B Adalah C.", "id": "Sebagian A Mungkin C" },
  { "en": "Semua Polisi WNI. Budi Polisi.", "id": "Budi Warga Negara Indonesia" },
  { "en": "Saya Sering Merasa Ingin Mengakhiri Hidup.", "id": "Tidak Itu Tanda Depresi Berat" },
  { "en": "Saya Suka Bekerja Sama Dalam Kelompok.", "id": "Ya Kemampuan Sosial" },
  { "en": "Apa Sinonim Kata Signifikansi?", "id": "Arti Penting" },
  { "en": "Apa Antonim Kata Sepele?", "id": "Penting Atau Signifikan" },
  { "en": "Deret Satu, Satu, Dua, Tiga, Lima, Delapan?", "id": "Tiga Belas" },
  { "en": "Pola Deret Satu, Satu, Dua Disebut Apa?", "id": "Deret Fibonacci" },
  { "en": "Apa Itu Tes Pauli Bagian Garis?", "id": "Tanda Pergantian Waktu Kerja" },
  { "en": "Apa Indikasi Grafik Pauli Menurun Tajam?", "id": "Mudah Menyerah Atau Lelah" },
  { "en": "Apa Kepanjangan LEMDIKLAT (Lembaga Pendidikan Dan Pelatihan)?", "id": "Lembaga Pendidikan Dan Pelatihan" },
  { "en": "Apa Tugas LEMDIKLAT (Lembaga Pendidikan Dan Pelatihan)?", "id": "Menyelenggarakan Pendidikan Polri" },
  { "en": "Kapal : Nahkoda = Kereta : ...?", "id": "Masinis" },
  { "en": "Pesawat : Pilot = Delman : ...?", "id": "Kusir" },
  { "en": "Berapa Derajat Sudut Segitiga Sama Kaki?", "id": "Dua Sudut Sama Besar" },
  { "en": "Satu Koma Lima Ditambah Dua Koma Lima?", "id": "Empat" },
  { "en": "Apa Sinonim Kata Rekayasa?", "id": "Manipulasi Atau Teknik" },
  { "en": "Apa Antonim Kata Alami?", "id": "Buatan Atau Rekayasa" },
  { "en": "Saya Pernah Merasa Sangat Marah Sampai Hilang Kendali.", "id": "Tidak Emosi Harus Stabil" },
  { "en": "Saya Selalu Mengerjakan Tugas Sebaik Mungkin.", "id": "Ya Berorientasi Kualitas" },
  { "en": "Apa Itu Reformasi Birokrasi Polri?", "id": "Perubahan Menuju Polisi Profesional" },
  { "en": "Apa Target Utama Polri Presisi?", "id": "Kepercayaan Publik" },
  { "en": "Satu Lustrum Ditambah Satu Windu?", "id": "Tiga Belas Tahun" },
  { "en": "Berapa Minggu Dalam Satu Bulan?", "id": "Empat Minggu" },
  { "en": "Jika Arah Selatan Di Belakang, Timur Di Mana?", "id": "Sebelah Kiri" },
  { "en": "Jarum Jam Dua Belas Siang Saling Berimpit?", "id": "Ya Berimpit" },
  { "en": "Semua Unggas Bertelur. Ayam Adalah Unggas.", "id": "Ayam Bertelur" },
  { "en": "Logika Deduktif Artinya Apa?", "id": "Umum Ke Khusus" },
  { "en": "Apa Sinonim Kata Supervisi?", "id": "Pengawasan" },
  { "en": "Apa Antonim Kata Pelaksana?", "id": "Supervisor Atau Pengawas" },
  { "en": "Dompet : Kulit = Buku : ...?", "id": "Kertas" },
  { "en": "Ban : Karet = Cincin : ...?", "id": "Emas Atau Logam" },
  { "en": "Apa Kepanjangan KABAG OPS (Kepala Bagian Operasi)?", "id": "Kepala Bagian Operasi" },
  { "en": "Apa Kepanjangan KASAT RESKRIM (Kepala Satuan Reserse Kriminal)?", "id": "Kepala Satuan Reserse Kriminal" },
  { "en": "Saya Sering Merasa Orang Lain Membicarakan Keburukan Saya.", "id": "Tidak Itu Paranoid" },
  { "en": "Saya Mengakui Jika Saya Berbuat Salah.", "id": "Ya Tanda Ksatria" },
  { "en": "Apa Itu Tes Gambar Orang Hujan (DAP Rain)?", "id": "Melihat Respon Terhadap Tekanan" },
  { "en": "Apa Arti Payung Dalam Tes Orang Hujan?", "id": "Perlindungan Diri Dari Masalah" },
  { "en": "Tiga Puluh Bagi Setengah Adalah?", "id": "Enam Puluh" },
  { "en": "Akar Seratus Empat Puluh Empat?", "id": "Dua Belas" },
  { "en": "Apa Sinonim Kata Mobilitas?", "id": "Gerakan Atau Perpindahan" },
  { "en": "Apa Antonim Kata Diam?", "id": "Bergerak Atau Mobilitas" },
  { "en": "Lidah : Rasa = Kulit : ...?", "id": "Raba Atau Sentuh" },
  { "en": "Hidung : Bau = Mata : ...?", "id": "Lihat Atau Cahaya" },
  { "en": "Apa Kepanjangan KASAT LANTAS (Kepala Satuan Lalu Lintas)?", "id": "Kepala Satuan Lalu Lintas" },
  { "en": "Apa Kepanjangan KASAT INTELKAM (Kepala Satuan Intelijen Keamanan)?", "id": "Kepala Satuan Intelijen Keamanan" },
  { "en": "Saya Suka Memecahkan Masalah Yang Rumit.", "id": "Ya Cocok Untuk Reserse" },
  { "en": "Saya Tidak Suka Perubahan Mendadak Dalam Rencana.", "id": "Tidak Harus Fleksibel" },
  { "en": "Siapa Nama Bapak Kapolri Pertama?", "id": "Komisaris Jenderal Polisi Said Sukanto" },
  { "en": "Hari Bhayangkara Diperingati Setiap Tanggal?", "id": "Satu Juli" },
  { "en": "Berapa Sisi Belah Ketupat?", "id": "Empat Sisi" },
  { "en": "Deret Lima Puluh, Empat Puluh Lima, Empat Puluh?", "id": "Tiga Puluh Lima" },
  { "en": "Apa Sinonim Kata Transparansi?", "id": "Keterbukaan" },
  { "en": "Apa Antonim Kata Rahasia?", "id": "Terbuka Atau Transparan" },
  { "en": "Bensin : Motor = Solar : ...?", "id": "Truk Atau Diesel" },
  { "en": "Listrik : Lampu = Baterai : ...?", "id": "Senter Atau Jam" },
  { "en": "Apa Kepanjangan KASAT BINMAS (Kepala Satuan Pembinaan Masyarakat)?", "id": "Kepala Satuan Pembinaan Masyarakat" },
  { "en": "Apa Kepanjangan KASAT SABHARA (Kepala Satuan Samapta)?", "id": "Kepala Satuan Samapta Bhayangkara" },
  { "en": "Rumus Luas Layang-Layang Adalah?", "id": "Setengah Kali Diagonal Satu Dua" },
  { "en": "Satu Meter Kubik Sama Dengan Berapa Liter?", "id": "Seribu Liter" },
  { "en": "Jika Hari Ini Sabtu, Dua Hari Lalu?", "id": "Kamis" },
  { "en": "Semua Dokter Pintar. Sebagian Dokter Sabar.", "id": "Sebagian Orang Pintar Sabar" },
  { "en": "Saya Pernah Berpikir Untuk Melarikan Diri Dari Rumah.", "id": "Tidak Masalah Harus Dihadapi" },
  { "en": "Saya Senang Membantu Teman Yang Kesulitan.", "id": "Ya Solidaritas" },
  { "en": "Apa Itu Tes Koran Bagian Puncak Dan Lembah?", "id": "Fluktuasi Kinerja Kerja" },
  { "en": "Grafik Kerja Yang Baik Bentuknya Bagaimana?", "id": "Naik Atau Stabil Di Atas" },
  { "en": "Satu Perlima Ditambah Dua Perlima?", "id": "Tiga Perlima" },
  { "en": "Dua Puluh Persen Pecahannya Adalah?", "id": "Satu Perlima" },
  { "en": "Apa Sinonim Kata Inisiatif?", "id": "Prakarsa" },
  { "en": "Apa Antonim Kata Pasif?", "id": "Inisiatif Atau Aktif" },
  { "en": "Palu : Paku = Obeng : ...?", "id": "Sekrup" },
  { "en": "Jarum : Benang = Lem : ...?", "id": "Kertas Atau Kayu" },
  { "en": "Berapa Hasil Dari Nol Koma Lima Kuadrat?", "id": "Nol Koma Dua Lima" },
  { "en": "Satu Perempat Jam Sama Dengan Berapa Menit?", "id": "Lima Belas Menit" },
  { "en": "Apa Sinonim Kata Egaliter?", "id": "Sederajat Atau Sama Rata" },
  { "en": "Apa Antonim Kata Otoriter?", "id": "Demokratis Atau Egaliter" },
  { "en": "Deret Tiga, Sembilan, Dua Puluh Tujuh, Delapan Puluh Satu?", "id": "Dua Ratus Empat Puluh Tiga" },
  { "en": "Pola Deret Tiga, Sembilan, Dua Puluh Tujuh Adalah?", "id": "Pangkat Tiga Atau Kali Tiga" },
  { "en": "Mikroskop : Mikroba = Teleskop : ...?", "id": "Bintang" },
  { "en": "Barometer : Tekanan = Termometer : ...?", "id": "Suhu" },
  { "en": "Apa Fungsi Utama YANMA (Pelayanan Markas) Polri?", "id": "Pelayanan Fasilitas Markas Besar" },
  { "en": "Saya Tidak Pernah Merasa Lelah Saat Bekerja.", "id": "Tidak Itu Berlebihan" },
  { "en": "Saya Suka Mengambil Inisiatif Tanpa Menunggu Perintah.", "id": "Ya Sikap Proaktif" },
  { "en": "Apa Itu Tes Army Alpha Bagian Pilihan Ganda?", "id": "Memilih Gambar Sesuai Instruksi" },
  { "en": "Apa Fokus Utama Tes Army Alpha?", "id": "Ketahanan Konsentrasi Pendengaran" },
  { "en": "Dua Koma Lima Dikali Dua Adalah?", "id": "Lima" },
  { "en": "Akar Seratus Enam Puluh Sembilan Adalah?", "id": "Tiga Belas" },
  { "en": "Apa Rumus Luas Permukaan Balok?", "id": "Dua Kali Panjang Lebar Tinggi" },
  { "en": "Sudut Putaran Seperempat Lingkaran Adalah?", "id": "Sembilan Puluh Derajat" },
  { "en": "Apa Sinonim Kata Kapabilitas?", "id": "Kemampuan Atau Kecakapan" },
  { "en": "Apa Antonim Kata Inkompeten?", "id": "Kompeten Atau Mampu" },
  { "en": "Rem : Mobil = Jangkar : ...?", "id": "Kapal" },
  { "en": "Sayap : Burung = Sirip : ...?", "id": "Ikan" },
  { "en": "Apa Kepanjangan PUSINA (Pusat Identifikasi Nasional)?", "id": "Pusat Identifikasi Nasional" },
  { "en": "Jika Semua Bunga Wangi. Melati Adalah Bunga.", "id": "Melati Itu Wangi" },
  { "en": "Sebagian Karyawan Rajin. Semua Karyawan Digaji.", "id": "Sebagian Yang Digaji Rajin" },
  { "en": "Saya Sering Memukul Meja Ketika Marah.", "id": "Tidak Pengendalian Emosi Buruk" },
  { "en": "Saya Menerima Kritik Sebagai Masukan Membangun.", "id": "Ya Sikap Profesional" },
  { "en": "Apa Sinonim Kata Rekonsiliasi?", "id": "Perdamaian Kembali" },
  { "en": "Apa Antonim Kata Konflik?", "id": "Damai Atau Harmoni" },
  { "en": "Deret Empat, Delapan, Enam Belas, Tiga Puluh Dua?", "id": "Enam Puluh Empat" },
  { "en": "Pola Deret Empat, Delapan, Enam Belas Adalah?", "id": "Dikali Dua" },
  { "en": "Apa Itu Tes Pauli Bagian Salah Hitung?", "id": "Indikasi Kurang Teliti" },
  { "en": "Apa Makna Grafik Pauli Yang Stabil?", "id": "Ketahanan Kerja Yang Baik" },
  { "en": "Apa Kepanjangan KORBRIMOB (Korps Brigade Mobil)?", "id": "Korps Brigade Mobil" },
  { "en": "Apa Tugas Utama KORBRIMOB (Korps Brigade Mobil)?", "id": "Menangani Kejahatan Intensitas Tinggi" },
  { "en": "Sepeda : Kayuh = Motor : ...?", "id": "Gas" },
  { "en": "Lilin : Sumbu = Lampu : ...?", "id": "Filamen Atau Kabel" },
  { "en": "Berapa Derajat Sudut Dalam Persegi?", "id": "Sembilan Puluh Derajat" },
  { "en": "Tiga Koma Lima Ditambah Satu Koma Lima?", "id": "Lima" },
  { "en": "Apa Sinonim Kata Elaborasi?", "id": "Penggarapan Secara Tekun" },
  { "en": "Apa Antonim Kata Sederhana?", "id": "Rumit Atau Kompleks" },
  { "en": "Saya Pernah Berkata Bohong Untuk Menghindari Masalah.", "id": "Tidak Jujur Lebih Utama" },
  { "en": "Saya Selalu Menghargai Perbedaan Pendapat.", "id": "Ya Toleransi Tinggi" },
  { "en": "Apa Arti Tri Brata Pedoman Hidup?", "id": "Tiga Nazar Polisi" },
  { "en": "Apa Arti Catur Prasetya Pedoman Kerja?", "id": "Empat Janji Kerja" },
  { "en": "Satu Abad Kurang Satu Windu?", "id": "Sembilan Puluh Dua Tahun" },
  { "en": "Berapa Bulan Dalam Satu Semester?", "id": "Enam Bulan" },
  { "en": "Jika Arah Timur Di Kanan, Utara Di Mana?", "id": "Depan" },
  { "en": "Jarum Jam Enam Sore Membentuk Sudut?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Semua Logam Menghantar Listrik. Tembaga Adalah Logam.", "id": "Tembaga Menghantar Listrik" },
  { "en": "Logika Induktif Artinya Apa?", "id": "Khusus Ke Umum" },
  { "en": "Apa Sinonim Kata Verifikasi?", "id": "Pemeriksaan Kebenaran" },
  { "en": "Apa Antonim Kata Falsifikasi?", "id": "Verifikasi Atau Pembenaran" },
  { "en": "Kuas : Cat = Pena : ...?", "id": "Tinta" },
  { "en": "Jaring : Ikan = Perangkap : ...?", "id": "Tikus Atau Hewan" },
  { "en": "Apa Tugas DIVHUBINTER (Divisi Hubungan Internasional)?", "id": "Kerjasama Kepolisian Antar Negara" },
  { "en": "Saya Sering Merasa Cemas Tanpa Sebab Jelas.", "id": "Tidak Tanda Kecemasan" },
  { "en": "Saya Berani Mengambil Risiko Terukur.", "id": "Ya Jiwa Pemimpin" },
  { "en": "Apa Itu Tes Gambar Pohon Beringin?", "id": "Melambangkan Pengayoman Dan Perlindungan" },
  { "en": "Apa Makna Daun Rimbun Dalam Tes Pohon?", "id": "Keinginan Untuk Belajar" },
  { "en": "Empat Puluh Lima Bagi Sembilan Adalah?", "id": "Lima" },
  { "en": "Akar Dua Ratus Dua Puluh Lima Adalah?", "id": "Lima Belas" },
  { "en": "Apa Sinonim Kata Kulminasi?", "id": "Puncak Tertinggi" },
  { "en": "Apa Antonim Kata Nadir?", "id": "Puncak Atau Kulminasi" },
  { "en": "Kulit : Sentuh = Telinga : ...?", "id": "Dengar" },
  { "en": "Mata : Pandang = Hidung : ...?", "id": "Cium Atau Bau" },
  { "en": "Apa Kepanjangan LEMDIKPOL (Lembaga Pendidikan Polri)?", "id": "Lembaga Pendidikan Polri" },
  { "en": "Apa Kepanjangan SESPIMTI (Sekolah Staf Pimpinan Tinggi)?", "id": "Sekolah Staf Pimpinan Tinggi" },
  { "en": "Saya Suka Menganalisis Data Statistik.", "id": "Ya Kemampuan Analitis" },
  { "en": "Saya Tidak Suka Diatur Oleh Orang Lain.", "id": "Tidak Harus Patuh Pimpinan" },
  { "en": "Siapa Kapolri Saat Tragedi Bom Bali Satu?", "id": "Jenderal Da'i Bachtiar" },
  { "en": "Tanggal Berapa Hari Bhayangkara Diperingati?", "id": "Satu Juli" },
  { "en": "Berapa Sisi Segi Enam Beraturan?", "id": "Enam Sisi" },
  { "en": "Apa Sinonim Kata Akuntabilitas?", "id": "Pertanggungjawaban" },
  { "en": "Apa Antonim Kata Lepas Tangan?", "id": "Bertanggung Jawab" },
  { "en": "Kompor : Api = Kulkas : ...?", "id": "Dingin" },
  { "en": "Televisi : Gambar = Radio : ...?", "id": "Suara" },
  { "en": "Apa Kepanjangan DITPOLSATWA (Direktorat Polisi Satwa)?", "id": "Direktorat Polisi Satwa" },
  { "en": "Apa Tugas DITPOLSATWA (Direktorat Polisi Satwa)?", "id": "Mengelola Anjing Dan Kuda Polisi" },
  { "en": "Rumus Luas Segitiga Sembarang Adalah?", "id": "Setengah Alas Kali Tinggi" },
  { "en": "Satu Desimeter Kubik Sama Dengan?", "id": "Satu Liter" },
  { "en": "Jika Besok Minggu, Kemarin Hari Apa?", "id": "Jumat" },
  { "en": "Semua Ikan Berenang. Lele Adalah Ikan.", "id": "Lele Berenang" },
  { "en": "Saya Pernah Mengambil Milik Orang Lain Tanpa Izin.", "id": "Tidak Itu Perbuatan Kriminal" },
  { "en": "Saya Senang Bekerja Dalam Tim Yang Solid.", "id": "Ya Kerjasama Tim" },
  { "en": "Apa Itu Tes Pauli Bagian Koreksi?", "id": "Memperbaiki Kesalahan Hitung" },
  { "en": "Grafik Kerja Menurun Menandakan Apa?", "id": "Kelelahan Fisik Mental" },
  { "en": "Tiga Perdelapan Ditambah Satu Perdelapan?", "id": "Setengah Atau Empat Perdelapan" },
  { "en": "Tiga Puluh Persen Desimalnya Adalah?", "id": "Nol Koma Tiga" },
  { "en": "Apa Sinonim Kata Inovasi?", "id": "Terobosan Baru" },
  { "en": "Apa Antonim Kata Kuno?", "id": "Modern Atau Inovatif" },
  { "en": "Paku : Palu = Baut : ...?", "id": "Obeng" },
  { "en": "Kunci : Pintu = Kode : ...?", "id": "Brankas Atau Rahasia" },
  { "en": "Berapa Jumlah Rusuk Pada Kubus?", "id": "Dua Belas Rusuk" },
  { "en": "Berapa Jumlah Titik Sudut Balok?", "id": "Delapan Titik Sudut" },
  { "en": "Apa Sinonim Kata Ekskavasi?", "id": "Penggalian" },
  { "en": "Apa Antonim Kata Permanen?", "id": "Sementara Atau Temporer" },
  { "en": "Deret Satu, Empat, Tujuh, Sepuluh, Tiga Belas?", "id": "Enam Belas" },
  { "en": "Pola Deret Satu, Empat, Tujuh Tersebut Adalah?", "id": "Ditambah Tiga" },
  { "en": "Beton : Gedung = Kayu : ...?", "id": "Mebel Atau Rumah" },
  { "en": "Baja : Jembatan = Aspal : ...?", "id": "Jalan Raya" },
  { "en": "Apa Semboyan Korps Brimob Polri?", "id": "Jiwa Ragaku Demi Kemanusiaan" },
  { "en": "Apa Warna Baret Korps Brimob?", "id": "Biru Dongker" },
  { "en": "Saya Tidak Pernah Merasa Bosan Melakukan Rutinitas.", "id": "Tidak Itu Kurang Wajar" },
  { "en": "Saya Suka Tantangan Baru Dalam Pekerjaan.", "id": "Ya Tanda Progresif" },
  { "en": "Apa Itu Tes Kecermatan Kolom Hilang?", "id": "Mencari Simbol Yang Tidak Ada" },
  { "en": "Apa Kunci Tes Kecermatan Kolom Hilang?", "id": "Ingatan Jangka Pendek" },
  { "en": "Dua Pertiga Kali Enam Puluh Adalah?", "id": "Empat Puluh" },
  { "en": "Akar Dua Ratus Lima Puluh Enam?", "id": "Enam Belas" },
  { "en": "Apa Rumus Volume Bola?", "id": "Empat Pertiga Pi Jari-Jari Pangkat Tiga" },
  { "en": "Sudut Segitiga Sama Sisi Besarnya?", "id": "Enam Puluh Derajat" },
  { "en": "Apa Sinonim Kata Probabilitas?", "id": "Kemungkinan" },
  { "en": "Apa Antonim Kata Mustahil?", "id": "Mungkin Atau Probabel" },
  { "en": "Rem : Berhenti = Gas : ...?", "id": "Jalan Atau Cepat" },
  { "en": "Kompas : Arah = Jam : ...?", "id": "Waktu" },
  { "en": "Apa Kepanjangan DENSUS (Detasemen Khusus)?", "id": "Detasemen Khusus" },
  { "en": "Densus 88 Dibentuk Setelah Peristiwa Apa?", "id": "Bom Bali Satu" },
  { "en": "Jika P Lebih Berat Q. Q Lebih Berat R.", "id": "P Paling Berat" },
  { "en": "Semua Tentara Disiplin. Sebagian Polisi Disiplin.", "id": "Disiplin Sifat Tentara Dan Polisi" },
  { "en": "Saya Sering Membanting Pintu Jika Kesal.", "id": "Tidak Tanda Emosi Buruk" },
  { "en": "Saya Selalu Menghormati Atasan Meskipun Berbeda Pendapat.", "id": "Ya Hierarki Kerja" },
  { "en": "Apa Sinonim Kata Rehabilitasi?", "id": "Pemulihan" },
  { "en": "Apa Antonim Kata Perusakan?", "id": "Perbaikan Atau Rehabilitasi" },
  { "en": "Deret Dua, Empat, Delapan, Enam Belas, Tiga Puluh Dua?", "id": "Enam Puluh Empat" },
  { "en": "Pola Deret Dua, Empat, Delapan Adalah?", "id": "Dikali Dua" },
  { "en": "Apa Itu Tes Kraepelin Bagian Ketahanan?", "id": "Kestabilan Jumlah Hitungan Per Kolom" },
  { "en": "Apa Indikasi Grafik Kraepelin Zig-Zag Ekstrem?", "id": "Emosi Tidak Stabil" },
  { "en": "Apa Tugas Utama BARESKRIM (Badan Reserse Kriminal)?", "id": "Penyidikan Tindak Pidana Tingkat Pusat" },
  { "en": "Helm : Kepala = Masker : ...?", "id": "Mulut Dan Hidung" },
  { "en": "Kacamata : Mata = Anting : ...?", "id": "Telinga" },
  { "en": "Berapa Derajat Sudut Putaran Jarum Jam?", "id": "Tiga Ratus Enam Puluh Derajat" },
  { "en": "Nol Koma Lima Ditambah Nol Koma Dua Lima?", "id": "Nol Koma Tujuh Lima" },
  { "en": "Apa Sinonim Kata Identifikasi?", "id": "Pengenalan Atau Penentuan" },
  { "en": "Apa Antonim Kata Kabur?", "id": "Jelas Atau Teridentifikasi" },
  { "en": "Saya Pernah Menggunakan Fasilitas Kantor Untuk Pribadi.", "id": "Tidak Itu Korupsi Waktu" },
  { "en": "Saya Mampu Bekerja Di Bawah Tekanan Tinggi.", "id": "Ya Mental Baja" },
  { "en": "Apa Arti Rastra Sewakottama?", "id": "Abdi Utama Nusa Bangsa" },
  { "en": "Siapa Yang Mengesahkan Pengangkatan Kapolri?", "id": "Presiden Dengan Persetujuan DPR" },
  { "en": "Satu Abad Sama Dengan Berapa Lustrum?", "id": "Dua Puluh Lustrum" },
  { "en": "Berapa Hari Dalam Tahun Biasa?", "id": "Tiga Ratus Enam Puluh Lima" },
  { "en": "Jika Arah Utara Di Kiri, Depan Arah Apa?", "id": "Timur" },
  { "en": "Jarum Jam Sembilan Pagi Membentuk Sudut?", "id": "Sembilan Puluh Derajat" },
  { "en": "Semua Dokter Memakai Jas Putih. Budi Dokter.", "id": "Budi Memakai Jas Putih" },
  { "en": "Logika Silogisme Premis Mayor Adalah?", "id": "Pernyataan Umum" },
  { "en": "Apa Sinonim Kata Konfirmasi?", "id": "Pembenaran Atau Penegasan" },
  { "en": "Apa Antonim Kata Penyangkalan?", "id": "Pengakuan Atau Konfirmasi" },
  { "en": "Laptop : Baterai = Manusia : ...?", "id": "Makanan Atau Energi" },
  { "en": "Mobil : Jalan = Kereta : ...?", "id": "Rel" },
  { "en": "Apa Kepanjangan DIVPROPAM (Divisi Profesi Pengamanan)?", "id": "Divisi Profesi Dan Pengamanan" },
  { "en": "Apa Tugas DIVPROPAM (Divisi Profesi Pengamanan)?", "id": "Menegakkan Disiplin Anggota Polri" },
  { "en": "Saya Sering Merasa Curiga Pada Orang Asing.", "id": "Tidak Berpikir Positif" },
  { "en": "Saya Berani Mengakui Kesalahan Diri Sendiri.", "id": "Ya Sikap Kesatria" },
  { "en": "Apa Itu Tes Gambar Wartegg Stimulus Garis Lurus?", "id": "Menunjukkan Tekad Dan Kekerasan Hati" },
  { "en": "Apa Makna Gambar Matahari Pada Tes Wartegg?", "id": "Optimisme Dan Harapan" },
  { "en": "Delapan Belas Bagi Tiga Kali Dua?", "id": "Dua Belas" },
  { "en": "Akar Seratus Sembilan Puluh Enam?", "id": "Empat Belas" },
  { "en": "Apa Antonim Kata Ditunda?", "id": "Segera Atau Urgent" },
  { "en": "Tangan : Jari = Kaki : ...?", "id": "Jari Kaki" },
  { "en": "Pohon : Daun = Kepala : ...?", "id": "Rambut" },
  { "en": "Apa Kepanjangan PUSDIK (Pusat Pendidikan)?", "id": "Pusat Pendidikan" },
  { "en": "Saya Suka Mempelajari Hal Baru Di Luar Bidang.", "id": "Ya Wawasan Luas" },
  { "en": "Saya Tidak Suka Jika Rencana Saya Berubah.", "id": "Tidak Harus Adaptif" },
  { "en": "Polri Berada Di Bawah Presiden Langsung?", "id": "Ya Di Bawah Presiden" },
  { "en": "Berapa Sisi Layang-Layang?", "id": "Empat Sisi" },
  { "en": "Deret Lima Puluh, Seratus, Dua Ratus, Empat Ratus?", "id": "Delapan Ratus" },
  { "en": "Apa Sinonim Kata Validitas?", "id": "Keabsahan" },
  { "en": "Apa Antonim Kata Batal?", "id": "Sah Atau Valid" },
  { "en": "Kamera : Foto = Printer : ...?", "id": "Cetak Atau Dokumen" },
  { "en": "Scanner : Digital = Printer : ...?", "id": "Fisik Atau Kertas" },
  { "en": "Polisi Satwa Termasuk Dalam Satuan Apa?", "id": "Sabhara" },
  { "en": "Rumus Luas Jajar Genjang Adalah?", "id": "Alas Kali Tinggi" },
  { "en": "Satu Galon Air Berapa Liter (Standar)?", "id": "Sembilan Belas Liter" },
  { "en": "Jika Lusa Selasa, Hari Ini Apa?", "id": "Minggu" },
  { "en": "Semua Kucing Takut Air. Anggora Adalah Kucing.", "id": "Anggora Takut Air" },
  { "en": "Saya Pernah Berbohong Kepada Orang Tua.", "id": "Pernah (Jujur Itu Penting)" },
  { "en": "Saya Senang Jika Teman Saya Sukses.", "id": "Ya Tidak Iri Hati" },
  { "en": "Apa Itu Tes Pauli Bagian Stabilitas?", "id": "Grafik Datar Dan Konsisten" },
  { "en": "Grafik Kerja Naik Turun Drastis Menandakan?", "id": "Emosi Labil" },
  { "en": "Satu Perempat Ditambah Tiga Perempat?", "id": "Satu" },
  { "en": "Nol Koma Sembilan Pecahannya Adalah?", "id": "Sembilan Persepuluh" },
  { "en": "Apa Sinonim Kata Fleksibilitas?", "id": "Keluwesan" },
  { "en": "Apa Antonim Kata Kaku?", "id": "Fleksibel" },
  { "en": "Palu : Besi = Gergaji : ...?", "id": "Kayu" },
  { "en": "Cangkul : Tanah = Jaring : ...?", "id": "Ikan" },
  { "en": "Berapa Hasil Akar Enam Puluh Empat Tambah Enam?", "id": "Empat Belas" },
  { "en": "Nol Koma Dua Kali Nol Koma Lima Adalah?", "id": "Nol Koma Satu" },
  { "en": "Apa Sinonim Kata Presisi?", "id": "Ketepatan Atau Akurasi" },
  { "en": "Apa Antonim Kata Ambigu?", "id": "Jelas Atau Tegas" },
  { "en": "Pola Deret Dua, Empat, Delapan Tersebut Adalah?", "id": "Perkalian Dua Berulang" },
  { "en": "Termometer : Suhu = Higrometer : ...?", "id": "Kelembapan Udara" },
  { "en": "Speedometer : Kecepatan = Odometer : ...?", "id": "Jarak Tempuh" },
  { "en": "Apa Kepanjangan DIVKUM (Divisi Hukum) Polri?", "id": "Divisi Hukum" },
  { "en": "Apa Tugas Utama DIVKUM (Divisi Hukum)?", "id": "Memberikan Bantuan Hukum Internal" },
  { "en": "Saya Merasa Gugup Saat Berbicara Di Depan Umum.", "id": "Tidak Harus Percaya Diri" },
  { "en": "Saya Tidak Pernah Melanggar Lampu Lalu Lintas.", "id": "Tidak Itu Kurang Realistis" },
  { "en": "Apa Itu Tes Army Alpha Bagian Menggambar?", "id": "Membuat Bentuk Sesuai Instruksi" },
  { "en": "Apa Kunci Sukses Tes Army Alpha?", "id": "Fokus Dan Jangan Ragu" },
  { "en": "Tiga Perempat Dikali Empat Adalah?", "id": "Tiga" },
  { "en": "Apa Rumus Luas Trapesium Sama Kaki?", "id": "Jumlah Sisi Sejajar Kali Tinggi Bagi Dua" },
  { "en": "Sudut Dalam Segitiga Sama Sisi Adalah?", "id": "Enam Puluh Derajat" },
  { "en": "Apa Sinonim Kata Signifikan?", "id": "Berarti Atau Penting" },
  { "en": "Apa Antonim Kata Relevan?", "id": "Tidak Berkaitan" },
  { "en": "Kunci : Gembok = Password : ...?", "id": "Akun Atau Akses" },
  { "en": "Rem : Gesekan = Mesin : ...?", "id": "Pembakaran Atau Gerak" },
  { "en": "Apa Kepanjangan PUSINAFI (Pusat Identifikasi Nasional Fisik)?", "id": "Pusat Identifikasi Nasional Fisik" },
  { "en": "Jika Semua A Adalah B. C Bukan B.", "id": "Maka C Bukan A" },
  { "en": "Semua Tentara Bersenjata. Polisi Juga Bersenjata.", "id": "Keduanya Memiliki Senjata" },
  { "en": "Saya Sering Merasa Orang Lain Tidak Adil.", "id": "Tidak Berpikir Positif" },
  { "en": "Saya Selalu Datang Lebih Awal Dari Jadwal.", "id": "Ya Disiplin Waktu" },
  { "en": "Apa Sinonim Kata Konsekuen?", "id": "Konsisten Atau Bertanggung Jawab" },
  { "en": "Apa Antonim Kata Labil?", "id": "Stabil Atau Tetap" },
  { "en": "Deret Lima, Sepuluh, Lima Belas, Dua Puluh?", "id": "Dua Puluh Lima" },
  { "en": "Pola Deret Lima, Sepuluh, Lima Belas Adalah?", "id": "Penambahan Lima" },
  { "en": "Apa Itu Tes Pauli Bagian Puncak Kerja?", "id": "Titik Tertinggi Produktivitas" },
  { "en": "Apa Makna Grafik Pauli Datar Stabil?", "id": "Ketahanan Kerja Baik" },
  { "en": "Apa Kepanjangan KORPOLAIRUD (Korps Kepolisian Air Udara)?", "id": "Korps Kepolisian Air Udara" },
  { "en": "Apa Tugas Utama KORPOLAIRUD (Korps Kepolisian Air Udara)?", "id": "Patroli Perairan Dan Udara" },
  { "en": "Helm : Pelindung = Sepatu : ...?", "id": "Alas Kaki" },
  { "en": "Kacamata : Penglihatan = Alat Bantu Dengar : ...?", "id": "Pendengaran" },
  { "en": "Berapa Derajat Sudut Siku-Siku Dibagi Tiga?", "id": "Tiga Puluh Derajat" },
  { "en": "Satu Setengah Kali Dua Adalah?", "id": "Tiga" },
  { "en": "Apa Sinonim Kata Integritas?", "id": "Kejujuran Mutlak" },
  { "en": "Apa Antonim Kata Korupsi?", "id": "Jujur Atau Integritas" },
  { "en": "Saya Pernah Mengambil Barang Hotel Sebagai Kenang-Kenangan.", "id": "Tidak Itu Pencurian Kecil" },
  { "en": "Saya Suka Membantu Orang Tua Tanpa Diminta.", "id": "Ya Berbakti" },
  { "en": "Apa Arti Semboyan Rastra Sewakottama?", "id": "Abdi Utama Nusa Bangsa" },
  { "en": "Siapa Bapak Kepolisian Negara Republik Indonesia?", "id": "Raden Said Soekanto Tjokrodiatmodjo" },
  { "en": "Satu Abad Dibagi Lima Puluh Tahun?", "id": "Dua Periode" },
  { "en": "Berapa Bulan Dalam Satu Triwulan?", "id": "Tiga Bulan" },
  { "en": "Jika Arah Selatan Di Kanan, Depan Arah Apa?", "id": "Timur" },
  { "en": "Jarum Jam Enam Pagi Membentuk Garis?", "id": "Lurus" },
  { "en": "Semua Mobil Punya Roda. Becak Punya Roda.", "id": "Becak Bukan Mobil" },
  { "en": "Logika Deduktif Dimulai Dari Mana?", "id": "Premis Umum Ke Khusus" },
  { "en": "Apa Sinonim Kata Valid?", "id": "Sah Atau Berlaku" },
  { "en": "Apa Antonim Kata Cacat?", "id": "Sempurna Atau Valid" },
  { "en": "Obeng : Putar = Palu : ...?", "id": "Pukul" },
  { "en": "Gergaji : Potong = Amplas : ...?", "id": "Halus" },
  { "en": "Apa Kepanjangan SETUKPA (Sekolah Pembentukan Perwira)?", "id": "Sekolah Pembentukan Perwira" },
  { "en": "Apa Tugas SETUKPA (Sekolah Pembentukan Perwira)?", "id": "Mendidik Calon Perwira Polisi" },
  { "en": "Saya Sering Merasa Iri Melihat Teman Sukses.", "id": "Tidak Harus Ikut Senang" },
  { "en": "Saya Berani Bertanggung Jawab Atas Kesalahan Tim.", "id": "Ya Jiwa Pemimpin" },
  { "en": "Apa Itu Tes Gambar Pohon Akar Tunggang?", "id": "Fondasi Hidup Yang Kuat" },
  { "en": "Apa Makna Buah Pada Tes Gambar Pohon?", "id": "Hasil Atau Prestasi" },
  { "en": "Dua Puluh Tujuh Bagi Tiga Kali Dua?", "id": "Delapan Belas" },
  { "en": "Apa Sinonim Kata Fluktuatif?", "id": "Berubah-Ubah" },
  { "en": "Apa Antonim Kata Stabil?", "id": "Labil Atau Fluktuatif" },
  { "en": "Telinga : Suara = Kulit : ...?", "id": "Suhu Atau Tekstur" },
  { "en": "Lidah : Rasa = Hidung : ...?", "id": "Aroma" },
  { "en": "Apa Gelar Lulusan STIK (Sekolah Tinggi Ilmu Kepolisian)?", "id": "Sarjana Ilmu Kepolisian" },
  { "en": "Saya Suka Menganalisis Masalah Secara Logis.", "id": "Ya Berpikir Sistematis" },
  { "en": "Saya Mudah Panik Dalam Situasi Darurat.", "id": "Tidak Harus Tenang" },
  { "en": "Kapan Hari Lahir Pancasila?", "id": "Satu Juni" },
  { "en": "Siapa Perumus Pancasila?", "id": "Soekarno, Hatta, Yamin" },
  { "en": "Berapa Sisi Segi Lima Beraturan?", "id": "Lima Sisi" },
  { "en": "Deret Dua Puluh, Empat Puluh, Delapan Puluh?", "id": "Seratus Enam Puluh" },
  { "en": "Apa Antonim Kata Diragukan?", "id": "Terpercaya" },
  { "en": "Bensin : Energi = Makanan : ...?", "id": "Tenaga" },
  { "en": "Listrik : Cahaya = Matahari : ...?", "id": "Sinar" },
  { "en": "Apa Kepanjangan KABID DOKKES (Kepala Bidang Dokkes)?", "id": "Kepala Bidang Kedokteran Kesehatan" },
  { "en": "Apa Kepanjangan KABID KEU (Kepala Bidang Keuangan)?", "id": "Kepala Bidang Keuangan" },
  { "en": "Rumus Luas Lingkaran Adalah?", "id": "Pi Kali Jari-Jari Kuadrat" },
  { "en": "Satu Liter Sama Dengan Berapa Sentimeter Kubik?", "id": "Seribu Sentimeter Kubik" },
  { "en": "Jika Kemarin Lusa Senin, Hari Ini?", "id": "Rabu" },
  { "en": "Semua Burung Bertelur. Pinguin Adalah Burung.", "id": "Pinguin Bertelur" },
  { "en": "Saya Pernah Menyakiti Binatang Saat Kecil.", "id": "Tidak Tanda Kasih Sayang" },
  { "en": "Saya Suka Kebersihan Dan Kerapian.", "id": "Ya Cermin Disiplin" },
  { "en": "Apa Itu Tes Pauli Bagian Lubang?", "id": "Angka Yang Terlewat Dijumlahkan" },
  { "en": "Apa Arti Banyak Lubang Di Tes Pauli?", "id": "Kurang Teliti Atau Terburu-Buru" },
  { "en": "Satu Perdua Kali Sepertiga Adalah?", "id": "Seperenam" },
  { "en": "Lima Puluh Persen Dari Dua Ratus?", "id": "Seratus" },
  { "en": "Apa Sinonim Kata Inisiator?", "id": "Penggagas" },
  { "en": "Apa Antonim Kata Pengikut?", "id": "Pemimpin Atau Inisiator" },
  { "en": "Paku : Tembok = Jarum : ...?", "id": "Kain" },
  { "en": "Kunci : Gembok = Jawaban : ...?", "id": "Masalah" },
  { "en": "Berapa Hasil Dari Nol Koma Satu Kuadrat?", "id": "Nol Koma Nol Satu" },
  { "en": "Satu Perdelapan Ditambah Tiga Perdelapan Adalah?", "id": "Setengah Atau Empat Perdelapan" },
  { "en": "Apa Sinonim Kata Tentatif?", "id": "Belum Pasti Atau Sementara" },
  { "en": "Apa Antonim Kata Sporadis?", "id": "Sering Atau Terus Menerus" },
  { "en": "Deret Dua, Enam, Delapan Belas, Lima Puluh Empat?", "id": "Seratus Enam Puluh Dua" },
  { "en": "Pola Deret Dua, Enam, Delapan Belas Adalah?", "id": "Dikali Tiga" },
  { "en": "Mikroskop : Lensa = Radio : ...?", "id": "Gelombang Atau Suara" },
  { "en": "Kompas : Utara = Magnet : ...?", "id": "Kutub" },
  { "en": "Apa Tugas Utama SSDM (Staf Sumber Daya Manusia)?", "id": "Mengelola Karir Anggota Polri" },
  { "en": "Saya Tidak Pernah Merasa Marah Seumur Hidup.", "id": "Tidak Itu Mustahil" },
  { "en": "Saya Selalu Menjaga Rahasia Yang Dipercayakan.", "id": "Ya Tanda Amanah" },
  { "en": "Apa Itu Tes Army Alpha Bagian Punggung?", "id": "Tidak Ada Bagian Punggung" },
  { "en": "Tes Army Alpha Fokus Pada Indera Apa?", "id": "Pendengaran" },
  { "en": "Dua Pertiga Dibagi Dua Adalah?", "id": "Sepertiga" },
  { "en": "Akar Seratus Enam Puluh Sembilan?", "id": "Tiga Belas" },
  { "en": "Apa Rumus Keliling Lingkaran?", "id": "Dua Kali Pi Kali Jari-Jari" },
  { "en": "Sudut Putaran Setengah Lingkaran?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Apa Sinonim Kata Kognisi?", "id": "Pengenalan Atau Pengetahuan" },
  { "en": "Apa Antonim Kata Emosi?", "id": "Rasio Atau Logika" },
  { "en": "Kertas : Halaman = Rumah : ...?", "id": "Ruangan" },
  { "en": "Buku : Perpustakaan = Obat : ...?", "id": "Apotek" },
  { "en": "Apa Kepanjangan PUSKEU (Pusat Keuangan)?", "id": "Pusat Keuangan" },
  { "en": "Apa Kepanjangan PUSDOKKES (Pusat Kedokteran Kesehatan)?", "id": "Pusat Kedokteran Dan Kesehatan" },
  { "en": "Jika Semua A Adalah B. Sebagian B Bukan C.", "id": "Tidak Dapat Disimpulkan Pasti" },
  { "en": "Semua Tentara Berani. Ayah Seorang Tentara.", "id": "Ayah Pemberani" },
  { "en": "Saya Sering Merasa Orang Lain Menertawakan Saya.", "id": "Tidak Itu Perasaan Minder" },
  { "en": "Saya Suka Keteraturan Dalam Bekerja.", "id": "Ya Tanda Disiplin" },
  { "en": "Apa Sinonim Kata Rekognisi?", "id": "Pengakuan" },
  { "en": "Apa Antonim Kata Pengabaian?", "id": "Perhatian Atau Rekognisi" },
  { "en": "Deret Sepuluh, Dua Belas, Empat Belas, Enam Belas?", "id": "Delapan Belas" },
  { "en": "Pola Deret Sepuluh, Dua Belas, Empat Belas Adalah?", "id": "Ditambah Dua" },
  { "en": "Apa Itu Tes Pauli Bagian Garis Bawah?", "id": "Penanda Waktu Per Menit" },
  { "en": "Apa Makna Grafik Pauli Yang Menanjak?", "id": "Semangat Kerja Meningkat" },
  { "en": "Apa Kepanjangan STIK-PTIK?", "id": "Sekolah Tinggi Ilmu Kepolisian" },
  { "en": "Lulusan STIK Mendapat Gelar Apa?", "id": "Sarjana Ilmu Kepolisian (S.I.K)" },
  { "en": "Telepon : Pulsa = Listrik : ...?", "id": "Token Atau Tagihan" },
  { "en": "Mobil : Supir = Pesawat : ...?", "id": "Pilot" },
  { "en": "Berapa Derajat Sudut Segi Delapan?", "id": "Seratus Tiga Puluh Lima Derajat" },
  { "en": "Satu Koma Dua Bagi Nol Koma Enam?", "id": "Dua" },
  { "en": "Saya Pernah Mengambil Uang Orang Tua Diam-Diam.", "id": "Tidak Itu Mencuri" },
  { "en": "Saya Selalu Mengerjakan Tugas Sampai Selesai.", "id": "Ya Tanggung Jawab" },
  { "en": "Apa Arti Tribrata Ketiga?", "id": "Pelindung Pengayom Pelayan Masyarakat" },
  { "en": "Apa Arti Catur Prasetya Keempat?", "id": "Memelihara Keamanan Ketertiban" },
  { "en": "Satu Windu Ditambah Dua Tahun?", "id": "Sepuluh Tahun" },
  { "en": "Berapa Hari Dalam Bulan April?", "id": "Tiga Puluh Hari" },
  { "en": "Jika Arah Barat Di Kanan, Depan Arah Apa?", "id": "Selatan" },
  { "en": "Jarum Jam Tiga Sore Membentuk Sudut?", "id": "Sembilan Puluh Derajat" },
  { "en": "Semua Kucing Hewan. Semua Hewan Bernapas.", "id": "Kucing Bernapas" },
  { "en": "Logika Induktif Dimulai Dari Mana?", "id": "Premis Khusus Ke Umum" },
  { "en": "Apa Sinonim Kata Verifikasi?", "id": "Pengecekan Ulang" },
  { "en": "Apa Antonim Kata Manipulasi?", "id": "Kejujuran Atau Asli" },
  { "en": "Kuas : Lukis = Pahat : ...?", "id": "Ukir" },
  { "en": "Apa Kepanjangan LEMDIKLAT (Lembaga Pendidikan Pelatihan)?", "id": "Lembaga Pendidikan Dan Pelatihan" },
  { "en": "Apa Tugas LEMDIKLAT (Lembaga Pendidikan Pelatihan)?", "id": "Mendidik Calon Anggota Polri" },
  { "en": "Saya Sering Merasa Sedih Tanpa Alasan.", "id": "Tidak Emosi Stabil" },
  { "en": "Saya Berani Mengambil Keputusan Sulit.", "id": "Ya Jiwa Kepemimpinan" },
  { "en": "Apa Itu Tes Gambar Pohon Berakar?", "id": "Kekuatan Mental Dan Prinsip" },
  { "en": "Apa Makna Dahan Patah Di Tes Pohon?", "id": "Trauma Masa Lalu" },
  { "en": "Empat Puluh Sembilan Bagi Tujuh Kali Dua?", "id": "Empat Belas" },
  { "en": "Apa Sinonim Kata Kulminasi?", "id": "Puncak" },
  { "en": "Apa Antonim Kata Dasar?", "id": "Puncak Atau Atas" },
  { "en": "Kulit : Raba = Mata : ...?", "id": "Lihat" },
  { "en": "Telinga : Dengar = Hidung : ...?", "id": "Cium" },
  { "en": "Saya Suka Menganalisis Data Angka.", "id": "Ya Kemampuan Numerik" },
  { "en": "Saya Tidak Suka Diperintah Tanpa Alasan Jelas.", "id": "Tidak Harus Patuh Perintah" },
  { "en": "Siapa Nama Kapolri Ke-5?", "id": "Jenderal Polisi Hoegeng Imam Santoso" },
  { "en": "Jenderal Hoegeng Dikenal Karena Apa?", "id": "Kejujuran Dan Kesederhanaannya" },
  { "en": "Deret Seratus, Dua Ratus, Empat Ratus?", "id": "Delapan Ratus" },
  { "en": "Apa Sinonim Kata Akuntabel?", "id": "Terukur Dan Jelas" },
  { "en": "Apa Antonim Kata Tertutup?", "id": "Terbuka Atau Transparan" },
  { "en": "Kompor : Panas = Kulkas : ...?", "id": "Dingin" },
  { "en": "Televisi : Visual = Radio : ...?", "id": "Audio" },
  { "en": "Apa Kepanjangan DITPOLAIR (Direktorat Polisi Air)?", "id": "Direktorat Polisi Air" },
  { "en": "Apa Tugas DITPOLAIR (Direktorat Polisi Air)?", "id": "Patroli Perairan Indonesia" },
  { "en": "Rumus Luas Layang-Layang?", "id": "Setengah Kali Diagonal Satu Kali Dua" },
  { "en": "Satu Hektar Sama Dengan Berapa Meter Persegi?", "id": "Sepuluh Ribu Meter Persegi" },
  { "en": "Jika Lusa Kamis, Kemarin Hari Apa?", "id": "Senin" },
  { "en": "Semua Ikan Hidup Di Air. Mujair Ikan.", "id": "Mujair Hidup Di Air" },
  { "en": "Saya Pernah Mengambil Barang Teman.", "id": "Tidak Itu Mencuri" },
  { "en": "Saya Senang Bekerja Sama Dengan Orang Baru.", "id": "Ya Kemampuan Adaptasi" },
  { "en": "Apa Itu Tes Pauli Bagian Jumlah?", "id": "Total Hitungan Yang Diselesaikan" },
  { "en": "Grafik Kerja Stabil Menandakan Apa?", "id": "Emosi Dan Stamina Baik" },
  { "en": "Satu Perlima Kali Lima?", "id": "Satu" },
  { "en": "Dua Puluh Lima Persen Desimalnya?", "id": "Nol Koma Dua Lima" },
  { "en": "Apa Sinonim Kata Inovatif?", "id": "Kreatif Atau Baru" },
  { "en": "Apa Antonim Kata Konservatif?", "id": "Modern Atau Progresif" },
  { "en": "Kunci : Pintu = Sandi : ...?", "id": "Akun" },
  { "en": "Berapa Hasil Tiga Pangkat Tiga Ditambah Tiga?", "id": "Tiga Puluh" },
  { "en": "Nol Koma Lima Dikali Nol Koma Lima?", "id": "Nol Koma Dua Lima" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Ketidaktetapan Atau Gejolak" },
  { "en": "Deret Satu, Tiga, Lima, Tujuh, Sembilan?", "id": "Sebelas" },
  { "en": "Pola Deret Satu, Tiga, Lima Tersebut Adalah?", "id": "Bilangan Ganjil" },
  { "en": "Rem : Gesekan = Lampu : ...?", "id": "Cahaya Atau Energi" },
  { "en": "Dinamo : Listrik = Mesin : ...?", "id": "Gerak Atau Tenaga" },
  { "en": "Apa Kepanjangan BAHARKAM (Badan Pemelihara Keamanan)?", "id": "Badan Pemelihara Keamanan" },
  { "en": "Apa Tugas Utama BAHARKAM (Badan Pemelihara Keamanan)?", "id": "Mewujudkan Keamanan Ketertiban Masyarakat" },
  { "en": "Saya Tidak Pernah Merasa Iri Hati Sedikitpun.", "id": "Tidak Itu Mustahil" },
  { "en": "Saya Selalu Siap Membantu Rekan Kerja.", "id": "Ya Solidaritas Tim" },
  { "en": "Apa Itu Tes Army Alpha Bagian Penjumlahan?", "id": "Menjumlahkan Angka Yang Didengar" },
  { "en": "Apa Kunci Sukses Tes Army Alpha?", "id": "Kecepatan Dan Ketepatan Reaksi" },
  { "en": "Empat Kali Lima Bagi Dua Adalah?", "id": "Sepuluh" },
  { "en": "Apa Rumus Volume Kerucut?", "id": "Sepertiga Pi Jari-Jari Kuadrat Tinggi" },
  { "en": "Sudut Segitiga Siku-Siku Adalah?", "id": "Sembilan Puluh Derajat" },
  { "en": "Apa Antonim Kata Netralitas?", "id": "Keberpihakan Atau Intervensi" },
  { "en": "Mikrofon : Suara = Kamera : ...?", "id": "Gambar Atau Visual" },
  { "en": "Speaker : Audio = Monitor : ...?", "id": "Video Atau Visual" },
  { "en": "Apa Kepanjangan KORPOLAIRUD (Korps Polisi Air Dan Udara)?", "id": "Korps Polisi Air Dan Udara" },
  { "en": "Apa Kepanjangan DITSAMAPTA (Direktorat Samapta)?", "id": "Direktorat Samapta" },
  { "en": "Jika Semua A Adalah B. Sebagian A Adalah C.", "id": "Maka Sebagian B Adalah C" },
  { "en": "Semua Hakim Adil. Pak Budi Hakim.", "id": "Pak Budi Adil" },
  { "en": "Saya Sering Merasa Orang Lain Mengawasi Saya.", "id": "Tidak Itu Paranoid" },
  { "en": "Saya Suka Kerapian Dalam Menyusun Laporan.", "id": "Ya Tanda Profesional" },
  { "en": "Apa Sinonim Kata Validitas?", "id": "Tingkat Kebenaran" },
  { "en": "Deret Dua, Empat, Delapan, Sepuluh, Empat Belas?", "id": "Enam Belas" },
  { "en": "Pola Deret Dua, Empat, Delapan Tersebut Adalah?", "id": "Ditambah Dua" },
  { "en": "Apa Itu Tes Pauli Bagian Garis Tegak?", "id": "Batas Akhir Pengerjaan" },
  { "en": "Apa Makna Grafik Pauli Menurun Di Akhir?", "id": "Kelelahan Fisik Mental" },
  { "en": "Apa Kepanjangan SPKT (Sentra Pelayanan Kepolisian Terpadu)?", "id": "Sentra Pelayanan Kepolisian Terpadu" },
  { "en": "Apa Tugas SPKT (Sentra Pelayanan Kepolisian Terpadu)?", "id": "Melayani Laporan Pengaduan Masyarakat" },
  { "en": "Obeng : Sekrup = Kunci Inggris : ...?", "id": "Baut Atau Mur" },
  { "en": "Gergaji : Potong = Lem : ...?", "id": "Rekat Atau Sambung" },
  { "en": "Berapa Derajat Sudut Segi Lima?", "id": "Seratus Delapan Derajat" },
  { "en": "Satu Koma Lima Kali Dua Adalah?", "id": "Tiga" },
  { "en": "Apa Sinonim Kata Kooperatif?", "id": "Bekerja Sama" },
  { "en": "Apa Antonim Kata Individualis?", "id": "Sosial Atau Kooperatif" },
  { "en": "Saya Pernah Mengambil Makanan Tanpa Membayar.", "id": "Tidak Itu Mencuri" },
  { "en": "Saya Selalu Datang Tepat Waktu Saat Rapat.", "id": "Ya Disiplin Waktu" },
  { "en": "Apa Arti Tribrata Pertama?", "id": "Berbakti Kepada Nusa Dan Bangsa" },
  { "en": "Apa Arti Catur Prasetya Pertama?", "id": "Meniadakan Segala Bentuk Gangguan Keamanan" },
  { "en": "Satu Abad Ditambah Satu Tahun?", "id": "Seratus Satu Tahun" },
  { "en": "Berapa Hari Dalam Bulan Juni?", "id": "Tiga Puluh Hari" },
  { "en": "Jika Arah Timur Di Kiri, Depan Arah Apa?", "id": "Selatan" },
  { "en": "Jarum Jam Sembilan Malam Membentuk Sudut?", "id": "Sembilan Puluh Derajat" },
  { "en": "Semua Ular Reptil. Komodo Reptil.", "id": "Komodo Dan Ular Adalah Reptil" },
  { "en": "Logika Deduktif Dimulai Dari?", "id": "Umum Ke Khusus" },
  { "en": "Apa Sinonim Kata Klarifikasi?", "id": "Penjelasan" },
  { "en": "Apa Antonim Kata Membingungkan?", "id": "Jelas Atau Klarifikasi" },
  { "en": "Tang : Jepit = Palu : ...?", "id": "Pukul" },
  { "en": "Sekop : Gali = Sapu : ...?", "id": "Bersih" },
  { "en": "Apa Kepanjangan PUSDIK LANTAS (Pusat Pendidikan Lalu Lintas)?", "id": "Pusat Pendidikan Lalu Lintas" },
  { "en": "Apa Tugas PUSDIK LANTAS (Pusat Pendidikan Lalu Lintas)?", "id": "Mendidik Spesialisasi Lalu Lintas" },
  { "en": "Saya Sering Merasa Gelisah Tanpa Sebab.", "id": "Tidak Emosi Stabil" },
  { "en": "Saya Berani Mengambil Risiko Demi Kebenaran.", "id": "Ya Integritas Tinggi" },
  { "en": "Apa Itu Tes Gambar Pohon Berbuah Lebat?", "id": "Produktivitas Dan Hasil Kerja" },
  { "en": "Apa Makna Batang Kokoh Di Tes Pohon?", "id": "Kekuatan Ego Dan Prinsip" },
  { "en": "Enam Kali Tujuh Bagi Dua?", "id": "Dua Puluh Satu" },
  { "en": "Akar Delapan Puluh Satu?", "id": "Sembilan" },
  { "en": "Apa Sinonim Kata Estimasi?", "id": "Perkiraan" },
  { "en": "Apa Antonim Kata Pasti?", "id": "Estimasi Atau Kira-Kira" },
  { "en": "Mata : Kacamata = Kaki : ...?", "id": "Tongkat Atau Sepatu" },
  { "en": "Telinga : Alat Bantu = Jantung : ...?", "id": "Pacu Jantung" },
  { "en": "Apa Kepanjangan SESPIM (Sekolah Staf Pimpinan)?", "id": "Sekolah Staf Dan Pimpinan" },
  { "en": "Apa Kepanjangan SESKOAL (Sekolah Staf Komando Angkatan Laut)?", "id": "Sekolah Staf Komando Angkatan Laut" },
  { "en": "Saya Suka Menghitung Anggaran Keuangan.", "id": "Ya Kemampuan Manajerial" },
  { "en": "Saya Tidak Suka Jika Ada Perubahan Rencana.", "id": "Tidak Harus Fleksibel" },
  { "en": "Siapa Kapolri Pada Masa Reformasi?", "id": "Jenderal Rusdihardjo" },
  { "en": "Apa Nama Pasukan Elit Polri?", "id": "Korps Brimob" },
  { "en": "Berapa Sisi Segitiga Sama Sisi?", "id": "Tiga Sisi" },
  { "en": "Apa Sinonim Kata Elaborasi?", "id": "Penjelasan Detail" },
  { "en": "Apa Antonim Kata Singkat?", "id": "Elaborasi Atau Panjang" },
  { "en": "Mobil : Garasi = Kereta : ...?", "id": "Depo Atau Stasiun" },
  { "en": "Pesawat : Hangar = Kapal : ...?", "id": "Dok Atau Pelabuhan" },
  { "en": "Polisi Satwa Menggunakan Hewan Apa Saja?", "id": "Anjing Dan Kuda" },
  { "en": "Rumus Luas Trapesium Adalah?", "id": "Jumlah Sisi Sejajar Kali Tinggi Bagi Dua" },
  { "en": "Satu Mililiter Sama Dengan Berapa CC?", "id": "Satu Sentimeter Kubik" },
  { "en": "Jika Lusa Senin, Hari Ini Apa?", "id": "Sabtu" },
  { "en": "Semua Burung Punya Paruh. Elang Burung.", "id": "Elang Punya Paruh" },
  { "en": "Saya Pernah Berbohong Demi Kebaikan.", "id": "Pernah (Jawaban Manusiawi)" },
  { "en": "Saya Senang Membantu Teman Yang Sedang Sakit.", "id": "Ya Empati Tinggi" },
  { "en": "Apa Itu Tes Pauli Bagian Puncak?", "id": "Prestasi Kerja Tertinggi" },
  { "en": "Grafik Kerja Datar Menandakan Apa?", "id": "Stabilitas Kerja Baik" },
  { "en": "Satu Perempat Kali Empat?", "id": "Satu" },
  { "en": "Tujuh Puluh Lima Persen Desimalnya?", "id": "Nol Koma Tujuh Lima" },
  { "en": "Apa Sinonim Kata Adaptif?", "id": "Mudah Menyesuaikan Diri" },
  { "en": "Apa Antonim Kata Kaku?", "id": "Adaptif Atau Luwes" },
  { "en": "Paku : Palu = Baut : ...?", "id": "Kunci Pas Atau Obeng" },
  { "en": "Kunci : Pintu = Password : ...?", "id": "Akses Atau Akun" },
  { "en": "Berapa Akar Kuadrat Dari Nol Koma Nol Empat?", "id": "Nol Koma Dua" },
  { "en": "Siapa Nama Bapak Reformasi Polri?", "id": "Jenderal Polisi Rusdihardjo" },
  { "en": "Saya Merasa Sangat Bersalah Jika Melanggar Janji Kecil.", "id": "Ya Menunjukkan Integritas Tinggi" },
  { "en": "Apa Sinonim Kata Kuantitas?", "id": "Jumlah Atau Banyaknya" },
  { "en": "Roda A Berputar Kanan, Roda B Bersinggungan Berputar?", "id": "Kiri Atau Berlawanan Arah" },
  { "en": "Apa Kepanjangan PERKAP (Peraturan Kepala Kepolisian)?", "id": "Peraturan Kepala Kepolisian Negara" },
  { "en": "Apa Itu Tes Kecerdasan Spasial Kubus?", "id": "Membayangkan Jaring-Jaring Menjadi Kubus" },
  { "en": "Saya Suka Mengambil Jalan Pintas Meski Melanggar Prosedur.", "id": "Tidak Harus Taat Prosedur" },
  { "en": "Deret Tiga, Enam, Sembilan, Dua Belas, Lima Belas?", "id": "Delapan Belas" },
  { "en": "Mobil : Bensin = Manusia : ...?", "id": "Makanan Atau Kalori" },
  { "en": "Apa Tugas Utama BABINKAMTIBMAS Di Desa?", "id": "Membina Keamanan Ketertiban Desa" },
  { "en": "Dua Puluh Lima Persen Dari Seribu Adalah?", "id": "Dua Ratus Lima Puluh" },
  { "en": "Jika Saya Menemukan Dompet, Saya Akan Melihat Isinya.", "id": "Tidak Langsung Serahkan Petugas" },
  { "en": "Segitiga Siku-Siku, Alas Tiga Tinggi Empat, Sisi Miring?", "id": "Lima (Rumus Pythagoras)" },
  { "en": "Semua Polisi Tegap. Budi Tidak Tegap.", "id": "Budi Bukan Polisi" },
  { "en": "Apa Itu Pasukan Perdamaian Garuda Bhayangkara?", "id": "Polisi Indonesia Di Misi PBB" },
  { "en": "Saya Tidak Suka Jika Orang Lain Lebih Unggul.", "id": "Tidak Itu Sifat Iri" },
  { "en": "Lilin : Cair = Air : ...?", "id": "Uap Atau Gas" },
  { "en": "Berapa Jumlah Bulu Leher Garuda Pancasila?", "id": "Empat Puluh Lima Helai" },
  { "en": "Satu Tiga Perempat Desimalnya Adalah?", "id": "Nol Koma Tujuh Lima" },
  { "en": "Apa Sinonim Kata Inisiasi?", "id": "Permulaan Atau Awal" },
  { "en": "Apa Fungsi Propam Dalam Struktur Polri?", "id": "Polisinya Polisi (Pengawas Internal)" },
  { "en": "Saya Siap Bekerja Di Bawah Tekanan Tinggi.", "id": "Ya Tanda Mental Kuat" },
  { "en": "Deret Satu, Empat, Sembilan, Enam Belas?", "id": "Dua Puluh Lima" },
  { "en": "Apa Itu Prinsip BETAH Dalam Seleksi Polri?", "id": "Bersih Transparan Akuntabel Humanis" },
  { "en": "Kertas : Tipis = Baja : ...?", "id": "Keras Atau Kuat" },
  { "en": "Berapa Derajat Sudut Putaran Setengah Lingkaran?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Apa Antonim Kata Skeptis?", "id": "Yakin Atau Percaya" },
  { "en": "Saya Sering Lupa Waktu Saat Bekerja.", "id": "Tidak Manajemen Waktu Buruk" },
  { "en": "Siapa Panglima Tertinggi Angkatan Bersenjata?", "id": "Presiden Republik Indonesia" },
  { "en": "Nol Koma Satu Ditambah Nol Koma Sembilan?", "id": "Satu" },
  { "en": "Gergaji : Kayu = Gunting : ...?", "id": "Kertas Atau Kain" },
  { "en": "Semua Burung Bersayap. Pinguin Bersayap.", "id": "Pinguin Adalah Burung (Secara Biologi)" },
  { "en": "Saya Tidak Pernah Mengeluh Sakit Seumur Hidup.", "id": "Tidak Itu Mustahil (Lie Detector)" },
  { "en": "Apa Sinonim Kata Kredibilitas?", "id": "Dapat Dipercaya" },
  { "en": "Apa Tugas Utama Densus 88 Antiteror?", "id": "Menangani Tindak Pidana Terorisme" },
  { "en": "Saya Suka Mencoba Hal Baru Yang Menantang.", "id": "Ya Tanda Adaptabilitas" },
  { "en": "Air : Haus = Nasi : ...?", "id": "Lapar" },
  { "en": "Setengah Dikali Setengah Adalah?", "id": "Seperempat" },
  { "en": "Saya Akan Mengembalikan Uang Kembalian Yang Berlebih.", "id": "Ya Tanda Jujur" },
  { "en": "Apa Lambang Bintang Satu Di Pundak Polisi?", "id": "Brigadir Jenderal Polisi" },
  { "en": "Apa Sinonim Kata Implementasi?", "id": "Penerapan Atau Pelaksanaan" },
  { "en": "Saya Merasa Orang Lain Selalu Salah Paham.", "id": "Tidak Introspeksi Diri" },
  { "en": "Dokter : Pasien = Guru : ...?", "id": "Murid Atau Siswa" },
  { "en": "Berapa Jumlah Sila Dalam Pancasila?", "id": "Lima Sila" },
  { "en": "Apa Kepanjangan DOKKES (Dokter Kesehatan)?", "id": "Kedokteran Dan Kesehatan" },
  { "en": "Saya Suka Bekerja Sendiri Daripada Dalam Tim.", "id": "Tidak Polisi Kerja Tim" },
  { "en": "Deret Dua, Lima, Delapan, Sebelas?", "id": "Empat Belas" },
  { "en": "Apa Antonim Kata Virtual?", "id": "Nyata Atau Realitas" },
  { "en": "Kuda : Rumput = Harimau : ...?", "id": "Daging" },
  { "en": "Apa Pangkat Tertinggi Di Golongan Bintara?", "id": "Ajun Inspektur Polisi Satu" },
  { "en": "Satu Jam Berapa Detik?", "id": "Tiga Ribu Enam Ratus Detik" },
  { "en": "Saya Sering Merasa Cemas Tanpa Alasan Jelas.", "id": "Tidak Emosi Stabil" },
  { "en": "Topi : Kepala = Kaus Kaki : ...?", "id": "Kaki" },
  { "en": "Apa Warna Dasar Lambang Polri?", "id": "Hitam Dan Kuning Emas" },
  { "en": "Dua Pertiga Ditambah Sepertiga Adalah?", "id": "Satu" },
  { "en": "Apa Antonim Kata Amatir?", "id": "Profesional Atau Ahli" },
  { "en": "Saya Tidak Pernah Melanggar Peraturan Lalu Lintas.", "id": "Tidak (Jujur Jika Pernah)" },
  { "en": "Apa Kepanjangan DIVTIK (Divisi TIK)?", "id": "Divisi Teknologi Informasi Komunikasi" },
  { "en": "Deret Seratus, Sembilan Puluh, Delapan Puluh?", "id": "Tujuh Puluh" },
  { "en": "Beras : Padi = Bensin : ...?", "id": "Minyak Bumi" },
  { "en": "Berapa Tahun Masa Pendidikan Di Akpol?", "id": "Empat Tahun" },
  { "en": "Akar Sembilan Ditambah Akar Empat?", "id": "Lima" },
  { "en": "Saya Selalu Memaafkan Orang Yang Menyakiti Saya.", "id": "Ya Tanda Dewasa" },
  { "en": "Kubus Memiliki Berapa Sisi?", "id": "Enam Sisi" },
  { "en": "Semua Ikan Berenang. Lele Ikan.", "id": "Lele Berenang" },
  { "en": "Jarum : Menjahit = Pisau : ...?", "id": "Memotong Atau Mengiris" },
  { "en": "Apa Semboyan Bhinneka Tunggal Ika?", "id": "Berbeda-Beda Tetap Satu Jua" },
  { "en": "Satu Lusin Ditambah Setengah Lusin?", "id": "Delapan Belas Buah" },
  { "en": "Saya Suka Menolong Orang Yang Kesulitan.", "id": "Ya Jiwa Pelayanan" },
  { "en": "Apa Kepanjangan POLAIRUD (Polisi Air Udara)?", "id": "Kepolisian Air Dan Udara" },
  { "en": "Deret Satu, Tiga, Sembilan, Dua Puluh Tujuh?", "id": "Delapan Puluh Satu" },
  { "en": "Apa Sinonim Kata Adaptasi?", "id": "Penyesuaian Diri" },
  { "en": "Berapa Jumlah Provinsi Di Indonesia Saat Ini?", "id": "Tiga Puluh Delapan (Update Terakhir)" },
  { "en": "Sepuluh Persen Dari Lima Ratus?", "id": "Lima Puluh" },
  { "en": "Saya Merasa Hidup Saya Penuh Masalah.", "id": "Tidak Selalu Bersyukur" },
  { "en": "Segi Enam Disebut Juga Dengan?", "id": "Heksagon" },
  { "en": "Berapa Sinus Tiga Puluh Derajat?", "id": "Setengah Atau Nol Koma Lima" },
  { "en": "Apa Kepanjangan KOMPOL (Komisaris Polisi)?", "id": "Komisaris Polisi" },
  { "en": "Saya Tidak Pernah Merasa Iri Pada Siapapun.", "id": "Tidak Itu Mustahil" },
  { "en": "Kuas : Lukisan = Pahat : ...?", "id": "Patung" },
  { "en": "Deret Lima, Sepuluh, Delapan, Tiga Belas, Sebelas?", "id": "Enam Belas" },
  { "en": "Apa Tugas Utama DITSIBER (Direktorat Siber)?", "id": "Menangani Kejahatan Dunia Maya" },
  { "en": "Akar Kuadrat Dari Dua Ratus Dua Puluh Lima?", "id": "Lima Belas" },
  { "en": "Saya Suka Bekerja Dengan Deadline Yang Ketat.", "id": "Ya Tanda Profesional" },
  { "en": "Apa Antonim Kata Konsisten?", "id": "Inkonsisten Atau Berubah-Ubah" },
  { "en": "Air : Menguap = Es : ...?", "id": "Mencair" },
  { "en": "Berapa Jumlah Bintang Pangkat Inspektur Jenderal?", "id": "Dua Bintang" },
  { "en": "Tiga Perempat Dikurangi Setengah Adalah?", "id": "Seperempat" },
  { "en": "Saya Sering Menyalahkan Orang Lain Atas Kesalahan Saya.", "id": "Tidak Harus Tanggung Jawab" },
  { "en": "Jika Roda A Berputar Kiri, Roda B Kanan?", "id": "Ya Jika Bersinggungan" },
  { "en": "Semua Guru Mengajar. Pak Budi Tidak Mengajar.", "id": "Pak Budi Bukan Guru (Saat Ini)" },
  { "en": "Saya Merasa Hidup Ini Tidak Adil.", "id": "Tidak Selalu Bersyukur" },
  { "en": "Dompet : Uang = Lemari : ...?", "id": "Pakaian" },
  { "en": "Dua Kali Tiga Pangkat Dua Adalah?", "id": "Delapan Belas" },
  { "en": "Apa Antonim Kata Rasional?", "id": "Irasional Atau Tidak Masuk Akal" },
  { "en": "Saya Selalu Mengembalikan Barang Pinjaman Tepat Waktu.", "id": "Ya Tanda Dapat Dipercaya" },
  { "en": "Apa Itu Tes Wartegg Stimulus Titik Pusat?", "id": "Menggambar Sesuatu Yang Fokus Atau Memusat" },
  { "en": "Deret Satu, Dua, Empat, Delapan, Enam Belas?", "id": "Tiga Puluh Dua" },
  { "en": "Emas : Karat = Berlian : ...?", "id": "Karat (Satuan Berat)" },
  { "en": "Satu Kilogram Sama Dengan Berapa Pon?", "id": "Dua Pon" },
  { "en": "Saya Pernah Berbohong Untuk Menghindari Hukuman.", "id": "Pernah (Jawaban Jujur)" },
  { "en": "Apa Fungsi Utama Sabhara Di Jalanan?", "id": "Patroli Dan Pengaturan" },
  { "en": "Dua Puluh Bagi Nol Koma Lima?", "id": "Empat Puluh" },
  { "en": "Apa Antonim Kata Monoton?", "id": "Variatif" },
  { "en": "Saya Tidak Suka Dikritik Oleh Atasan.", "id": "Tidak Kritik Membangun Diri" },
  { "en": "Bulan : Malam = Matahari : ...?", "id": "Siang" },
  { "en": "Deret Seratus, Lima Puluh, Dua Puluh Lima?", "id": "Dua Belas Koma Lima" },
  { "en": "Apa Itu Konsep Presisi Kapolri?", "id": "Prediktif Responsibilitas Transparansi Berkeadilan" },
  { "en": "Buku : Ilmu = Olahraga : ...?", "id": "Sehat" },
  { "en": "Saya Sering Merasa Gemetar Tanpa Sebab.", "id": "Tidak Syarat Fisik Polisi" },
  { "en": "Jika Besok Senin, Tiga Hari Lalu?", "id": "Kamis" },
  { "en": "Semua Logam Keras. Merkuri Adalah Logam.", "id": "Merkuri Keras (Kecuali Cair)" },
  { "en": "Saya Berani Mengambil Risiko Untuk Kebenaran.", "id": "Ya Integritas" },
  { "en": "Telinga : Dengar = Hidung : ...?", "id": "Bau" },
  { "en": "Siapa Nama Bapak Kepolisian Indonesia?", "id": "Raden Said Soekanto" },
  { "en": "Saya Sering Lupa Menaruh Kunci Kendaraan.", "id": "Tidak Saya Teliti" },
  { "en": "Deret Tiga, Lima, Tujuh, Sembilan?", "id": "Sebelas" },
  { "en": "Garam : Asin = Gula : ...?", "id": "Manis" },
  { "en": "Apa Tugas Utama Densus 88?", "id": "Penanggulangan Terorisme" },
  { "en": "Tiga Kali Empat Bagi Dua?", "id": "Enam" },
  { "en": "Apa Sinonim Kata Komprehensif?", "id": "Menyeluruh" },
  { "en": "Saya Suka Membersihkan Lingkungan Sekitar.", "id": "Ya Kebersihan Sebagian Iman" },
  { "en": "Apa Rumus Keliling Persegi?", "id": "Empat Kali Sisi" },
  { "en": "Saya Tidak Mudah Tersinggung Oleh Candaan.", "id": "Ya Mental Baja" },
  { "en": "Apa Warna Seragam PDL (Pakaian Dinas Lapangan)?", "id": "Cokelat Tua" },
  { "en": "Satu Lusin Berapa Buah?", "id": "Dua Belas" },
  { "en": "Apa Antonim Kata Fiksi?", "id": "Fakta" },
  { "en": "Saya Selalu Menjaga Kerapian Penampilan.", "id": "Ya Cermin Disiplin" },
  { "en": "Deret Delapan, Empat, Dua, Satu?", "id": "Setengah" },
  { "en": "Lilin : Api = Baterai : ...?", "id": "Listrik" },
  { "en": "Berapa Sila Dalam Pancasila?", "id": "Lima Sila" },
  { "en": "Akar Sembilan Tambah Akar Enam Belas?", "id": "Tujuh" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Gejolak" },
  { "en": "Saya Sering Merasa Cemas Berlebihan.", "id": "Tidak Emosi Stabil" },
  { "en": "Saya Suka Bekerja Sama Tim.", "id": "Ya Sinergitas" },
  { "en": "Tanggal Berapa Hari Bhayangkara?", "id": "Satu Juli" },
  { "en": "Sepuluh Persen Dari Seratus?", "id": "Sepuluh" },
  { "en": "Saya Tidak Pernah Berbohong Seumur Hidup.", "id": "Tidak Itu Mustahil" },
  { "en": "Apa Kepanjangan SAMSAT?", "id": "Sistem Administrasi Manunggal Satu Atap" },
  { "en": "Deret Dua, Empat, Delapan, Enam Belas?", "id": "Tiga Puluh Dua" },
  { "en": "Kayu : Kursi = Tanah Liat : ...?", "id": "Gerabah" },
  { "en": "Satu Koma Lima Kali Dua?", "id": "Tiga" },
  { "en": "Apa Sinonim Kata Kredibel?", "id": "Terpercaya" },
  { "en": "Saya Sering Membanting Barang Saat Marah.", "id": "Tidak Kontrol Emosi" },
  { "en": "Apa Kepanjangan BRIMOB?", "id": "Brigade Mobil" },
  { "en": "Jika Arah Utara Di Atas, Timur Di Mana?", "id": "Kanan" },
  { "en": "Saya Suka Tantangan Baru.", "id": "Ya Semangat Belajar" },
  { "en": "Dokter : Rumah Sakit = Guru : ...?", "id": "Sekolah" },
  { "en": "Apa Lambang Negara Indonesia?", "id": "Garuda Pancasila" },
  { "en": "Dua Pangkat Tiga Adalah?", "id": "Delapan" },
  { "en": "Apa Antonim Kata Modern?", "id": "Tradisional" },
  { "en": "Saya Selalu Datang Tepat Waktu.", "id": "Ya Disiplin Waktu" },
  { "en": "Apa Kepanjangan PROPAM?", "id": "Profesi Dan Pengamanan" },
  { "en": "Berapa Akar Kuadrat Dari Tiga Puluh Enam?", "id": "Enam" },
  { "en": "Saya Sering Merasa Orang Lain Ingin Mencelakai Saya.", "id": "Tidak Itu Tanda Paranoid" },
  { "en": "Apa Warna Baret Satuan Brimob?", "id": "Biru Dongker" },
  { "en": "Satu Setengah Dikali Dua Adalah?", "id": "Tiga" },
  { "en": "Saya Suka Mengambil Keputusan Di Saat Genting.", "id": "Ya Tanda Jiwa Pemimpin" },
  { "en": "Apa Antonim Kata Progresif?", "id": "Regresif Atau Mundur" },
  { "en": "Air : Es = Uap : ...?", "id": "Air" },
  { "en": "Dua Puluh Persen Dari Lima Puluh?", "id": "Sepuluh" },
  { "en": "Apa Sinonim Kata Elaborasi?", "id": "Penjelasan Mendalam" },
  { "en": "Saya Tidak Pernah Marah Kepada Siapapun.", "id": "Tidak Itu Mustahil (Lie Detector)" },
  { "en": "Jika Roda A Besar Putar Kanan, Roda B Kecil?", "id": "Putar Kiri Lebih Cepat" },
  { "en": "Semua Tentara Berani. Ayah Tidak Berani.", "id": "Ayah Bukan Tentara" },
  { "en": "Saya Merasa Hidup Saya Sia-Sia.", "id": "Tidak Tanda Depresi" },
  { "en": "Tiga Pangkat Dua Tambah Empat Pangkat Dua?", "id": "Dua Puluh Lima" },
  { "en": "Saya Selalu Mengembalikan Barang Temuan Kepada Pemilik.", "id": "Ya Tanda Integritas" },
  { "en": "Apa Itu Tes Kecermatan Angka Hilang?", "id": "Mencari Angka Yang Tidak Ada" },
  { "en": "Kayu : Lemari = Kapas : ...?", "id": "Baju Atau Kain" },
  { "en": "Apa Kepanjangan DENSUS 88 (Detasemen Khusus)?", "id": "Detasemen Khusus Delapan Puluh Delapan" },
  { "en": "Satu Liter Berapa Sentimeter Kubik?", "id": "Seribu Sentimeter Kubik" },
  { "en": "Saya Pernah Mengambil Uang Orang Tua Tanpa Izin.", "id": "Pernah (Jujur Itu Penting)" },
  { "en": "Apa Sinonim Kata Konvensi?", "id": "Kesepakatan" },
  { "en": "Apa Fungsi Utama Lalu Lintas?", "id": "Kamseltibcarlantas" },
  { "en": "Empat Puluh Bagi Nol Koma Lima?", "id": "Delapan Puluh" },
  { "en": "Apa Antonim Kata Otoriter?", "id": "Demokratis" },
  { "en": "Saya Tidak Suka Jika Rencana Berubah Mendadak.", "id": "Tidak Harus Fleksibel" },
  { "en": "Rumus Volume Kubus Adalah?", "id": "Sisi Pangkat Tiga" },
  { "en": "Saya Suka Membantu Orang Tua Di Rumah.", "id": "Ya Tanda Berbakti" },
  { "en": "Apa Itu Program Presisi Kapolri?", "id": "Prediktif Responsibilitas Transparansi Berkeadilan" },
  { "en": "Gitar : Petik = Drum : ...?", "id": "Pukul" },
  { "en": "Satu Abad Berapa Dekade?", "id": "Sepuluh Dekade" },
  { "en": "Apa Sinonim Kata Regulasi?", "id": "Aturan" },
  { "en": "Saya Sering Merasa Pusing Saat Tertekan.", "id": "Tidak Fisik Harus Prima" },
  { "en": "Jika Hari Ini Rabu, Dua Hari Lagi?", "id": "Jumat" },
  { "en": "Semua Burung Bertelur. Elang Adalah Burung.", "id": "Elang Bertelur" },
  { "en": "Saya Berani Mengakui Kesalahan Di Depan Umum.", "id": "Ya Tanda Ksatria" },
  { "en": "Siapa Bapak Proklamator Indonesia?", "id": "Soekarno Hatta" },
  { "en": "Berapa Derajat Sudut Segitiga Sama Sisi?", "id": "Enam Puluh Derajat" },
  { "en": "Saya Sering Lupa Dimana Menaruh Barang.", "id": "Tidak Saya Teliti" },
  { "en": "Gula : Semut = Bangkai : ...?", "id": "Lalat" },
  { "en": "Apa Tugas Utama Brimob?", "id": "Menangani Kejahatan Intensitas Tinggi" },
  { "en": "Enam Kali Delapan Bagi Dua?", "id": "Dua Puluh Empat" },
  { "en": "Saya Suka Menjaga Kebersihan Lingkungan Kerja.", "id": "Ya Cermin Kedisiplinan" },
  { "en": "Apa Rumus Luas Persegi?", "id": "Sisi Kali Sisi" },
  { "en": "Saya Tidak Mudah Putus Asa Saat Gagal.", "id": "Ya Mental Pejuang" },
  { "en": "Jarum : Benang = Mur : ...?", "id": "Baut" },
  { "en": "Apa Warna Dasar Seragam Polisi?", "id": "Cokelat" },
  { "en": "Apa Antonim Kata Fiksi?", "id": "Non Fiksi Atau Fakta" },
  { "en": "Saya Selalu Berpakaian Rapi Di Mana Saja.", "id": "Ya Menjaga Citra Diri" },
  { "en": "Berapa Sila Pancasila?", "id": "Lima" },
  { "en": "Saya Sering Merasa Curiga Tanpa Alasan.", "id": "Tidak Berpikir Positif" },
  { "en": "Rumus Keliling Lingkaran Adalah?", "id": "Dua Pi Jari-Jari" },
  { "en": "Saya Suka Bekerja Sama Dalam Tim.", "id": "Ya Kerjasama Itu Penting" },
  { "en": "Kaki : Sepatu = Kepala : ...?", "id": "Topi" },
  { "en": "Saya Tidak Pernah Berbohong Demi Kebaikan.", "id": "Pernah (Jawaban Realistis)" },
  { "en": "Buku : Penulis = Lagu : ...?", "id": "Pencipta Atau Komposer" },
  { "en": "Siapa Presiden Pertama Indonesia?", "id": "Insinyur Soekarno" },
  { "en": "Dua Koma Lima Tambah Tiga Koma Lima?", "id": "Enam" },
  { "en": "Saya Sering Membanting Pintu Saat Marah.", "id": "Tidak Kontrol Emosi" },
  { "en": "Jika Matahari Terbit Timur, Terbenam Di?", "id": "Barat" },
  { "en": "Saya Suka Belajar Hal Baru.", "id": "Ya Pengembangan Diri" },
  { "en": "Mobil : Roda = Perahu : ...?", "id": "Layar Atau Dayung" },
  { "en": "Apa Lambang Sila Pertama Pancasila?", "id": "Bintang" },
  { "en": "Saya Selalu Tepat Janji.", "id": "Ya Integritas" },
  { "en": "Berapa Rusuk Pada Limas Segi Empat?", "id": "Delapan Rusuk" },
  { "en": "Saya Tidak Pernah Merasa Lelah Saat Olahraga.", "id": "Tidak Itu Mustahil (Lie Detector)" },
  { "en": "Air : Haus = Obat : ...?", "id": "Sakit" },
  { "en": "Apa Warna Baret Satuan Polisi Perairan?", "id": "Biru Langit" },
  { "en": "Nol Koma Dua Lima Dikali Empat?", "id": "Satu" },
  { "en": "Saya Suka Mengerjakan Tugas Yang Menantang.", "id": "Ya Tanda Berprestasi" },
  { "en": "Apa Antonim Kata Kolektif?", "id": "Individual" },
  { "en": "Siapa Nama Kapolri Ke-3?", "id": "Jenderal Polisi Soetjipto Danoekoesoemo" },
  { "en": "Tiga Puluh Persen Dari Enam Ratus?", "id": "Seratus Delapan Puluh" },
  { "en": "Apa Sinonim Kata Kapabilitas?", "id": "Kemampuan" },
  { "en": "Saya Sering Mengambil Barang Kantor Untuk Di Rumah.", "id": "Tidak Itu Korupsi" },
  { "en": "Jika Roda Depan Berputar Cepat, Roda Belakang?", "id": "Berputar Cepat Juga" },
  { "en": "Semua Hakim Adil. Sebagian Orang Hakim.", "id": "Sebagian Orang Adil" },
  { "en": "Saya Merasa Hidup Saya Penuh Kesialan.", "id": "Tidak Selalu Bersyukur" },
  { "en": "Kapur : Papan Tulis = Spidol : ...?", "id": "Whiteboard" },
  { "en": "Apa Semboyan Satuan Reserse?", "id": "Sidik Sakti Indera Waspada" },
  { "en": "Lima Pangkat Dua Kurang Tiga Pangkat Dua?", "id": "Enam Belas" },
  { "en": "Apa Antonim Kata Fana?", "id": "Abadi" },
  { "en": "Saya Selalu Menepati Janji Kepada Siapapun.", "id": "Ya Tanda Integritas" },
  { "en": "Apa Itu Tes Kecermatan Simbol Hilang?", "id": "Ketelitian Mata Dan Ingatan" },
  { "en": "Deret Dua, Tiga, Lima, Delapan?", "id": "Dua Belas" },
  { "en": "Sapi : Susu = Lebah : ...?", "id": "Madu" },
  { "en": "Satu Ton Berapa Kuintal?", "id": "Sepuluh Kuintal" },
  { "en": "Saya Pernah Melanggar Aturan Sekolah Dulu.", "id": "Pernah (Jujur Itu Baik)" },
  { "en": "Apa Fungsi Utama Intelkam?", "id": "Deteksi Dini Dan Keamanan" },
  { "en": "Enam Puluh Bagi Nol Koma Lima?", "id": "Seratus Dua Puluh" },
  { "en": "Apa Antonim Kata Absolut?", "id": "Relatif" },
  { "en": "Saya Tidak Suka Jika Ada Orang Lebih Pintar.", "id": "Tidak Harus Rendah Hati" },
  { "en": "Saya Suka Menolong Orang Yang Tidak Dikenal.", "id": "Ya Jiwa Sosial" },
  { "en": "Apa Itu Konsep Promoter Kapolri Tito?", "id": "Profesional Modern Terpercaya" },
  { "en": "Buku : Membaca = Pisau : ...?", "id": "Memotong" },
  { "en": "Satu Dekade Berapa Lustrum?", "id": "Dua Lustrum" },
  { "en": "Apa Sinonim Kata Kohesi?", "id": "Kepaduan" },
  { "en": "Saya Sering Merasa Gemetar Saat Ujian.", "id": "Tidak Mental Harus Kuat" },
  { "en": "Jika Hari Ini Jumat, Tiga Hari Lagi?", "id": "Senin" },
  { "en": "Semua Logam Padat. Raksa Adalah Logam.", "id": "Raksa Pengecualian (Cair)" },
  { "en": "Saya Berani Menanggung Risiko Keputusan Saya.", "id": "Ya Tanggung Jawab" },
  { "en": "Mata : Melihat = Lidah : ...?", "id": "Mengecap" },
  { "en": "Siapa Pahlawan Revolusi Dari Polri?", "id": "Karel Satsuit Tubun" },
  { "en": "Berapa Derajat Sudut Lurus?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Apa Antonim Kata Netral?", "id": "Memihak" },
  { "en": "Saya Sering Lupa Nama Orang Yang Baru Dikenal.", "id": "Tidak Saya Perhatian" },
  { "en": "Pasir : Semen = Tepung : ...?", "id": "Telur Atau Gula" },
  { "en": "Apa Tugas Utama Polairud?", "id": "Patroli Perairan Dan Udara" },
  { "en": "Empat Kali Sembilan Bagi Dua?", "id": "Delapan Belas" },
  { "en": "Apa Sinonim Kata Restorasi?", "id": "Pemulihan" },
  { "en": "Saya Suka Kerapian Meja Kerja.", "id": "Ya Cermin Disiplin" },
  { "en": "Jarum : Kain = Pipa : ...?", "id": "Air" },
  { "en": "Apa Warna Seragam PDH (Pakaian Dinas Harian)?", "id": "Cokelat Muda Dan Tua" },
  { "en": "Satu Kodi Berapa Buah?", "id": "Dua Puluh" },
  { "en": "Apa Antonim Kata Nyata?", "id": "Maya Atau Abstrak" },
  { "en": "Saya Selalu Menjaga Etika Sopan Santun.", "id": "Ya Sikap Dasar Polisi" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Ketidaktetapan" },
  { "en": "Saya Sering Merasa Curiga Pada Teman.", "id": "Tidak Berpikir Positif" },
  { "en": "Rumus Keliling Persegi Panjang?", "id": "Dua Kali Panjang Tambah Lebar" },
  { "en": "Saya Suka Bekerja Sama Dalam Kelompok.", "id": "Ya Kerjasama Tim" },
  { "en": "Tanggal Berapa Hari Kemerdekaan RI?", "id": "Tujuh Belas Agustus" },
  { "en": "Lima Puluh Persen Dari Seribu?", "id": "Lima Ratus" },
  { "en": "Saya Tidak Pernah Mengambil Milik Orang Lain.", "id": "Ya (Jujur Dan Tegas)" },
  { "en": "Apa Kepanjangan SIM (Surat Izin Mengemudi)?", "id": "Surat Izin Mengemudi" },
  { "en": "Kayu : Pintu = Kaca : ...?", "id": "Jendela" },
  { "en": "Siapa Presiden Kedua Indonesia?", "id": "Soeharto" },
  { "en": "Tiga Koma Lima Tambah Dua Koma Lima?", "id": "Enam" },
  { "en": "Apa Sinonim Kata Valid?", "id": "Sah" },
  { "en": "Saya Sering Membanting Barang Saat Emosi.", "id": "Tidak Kontrol Diri Buruk" },
  { "en": "Jika Matahari Terbit Timur, Bayangan Di?", "id": "Barat" },
  { "en": "Saya Suka Belajar Hal Baru.", "id": "Ya Inovatif" },
  { "en": "Apa Lambang Sila Ketiga Pancasila?", "id": "Pohon Beringin" },
  { "en": "Dua Pangkat Lima Adalah?", "id": "Tiga Puluh Dua" },
  { "en": "Apa Antonim Kata Modern?", "id": "Kuno" },
  { "en": "Saya Selalu Datang Tepat Waktu.", "id": "Ya Disiplin" }

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
