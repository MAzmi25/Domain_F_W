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


  { "en": "Apa Itu Domain Waktu?", "id": "Representasi Sinyal Sebagai Fungsi Waktu." },
  { "en": "Sumbu Horizontal Domain Waktu?", "id": "Sumbu Waktu (Detik Menit Jam)." },
  { "en": "Sumbu Vertikal Domain Waktu?", "id": "Amplitudo Atau Kekuatan Sinyal." },
  { "en": "Contoh Sinyal Domain Waktu?", "id": "Grafik Detak Jantung (EKG)." },
  { "en": "Apa Yang Kita Lihat Di Domain Waktu?", "id": "Bagaimana Sinyal Berubah Seiring Waktu." },
  { "en": "Apa Itu Domain Frekuensi?", "id": "Representasi Sinyal Sebagai Komponen Frekuensi." },
  { "en": "Sumbu Horizontal Domain Frekuensi?", "id": "Sumbu Frekuensi (Hertz Radian)." },
  { "en": "Sumbu Vertikal Domain Frekuensi?", "id": "Amplitudo Atau Fasa Sinyal." },
  { "en": "Apa Yang Kita Lihat Di Domain Frekuensi?", "id": "Frekuensi Apa Saja Yang Membangun Sinyal." },
  { "en": "Bagaimana Dua Domain Ini Terhubung?", "id": "Melalui Transformasi Fourier Dan Transformasi Laplace." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Alat Matematis Untuk Pindah Antar Domain." },
  { "en": "Transformasi Dari Waktu Ke Frekuensi?", "id": "Transformasi Fourier Maju (Forward Fourier Transform)." },
  { "en": "Transformasi Dari Frekuensi Ke Waktu?", "id": "Transformasi Fourier Balik (Inverse Fourier Transform)." },
  { "en": "Apa Itu Spektrum?", "id": "Representasi Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu Amplitudo Spektrum?", "id": "Menunjukkan Kekuatan Setiap Komponen Frekuensi." },
  { "en": "Apa Itu Fasa Spektrum?", "id": "Menunjukkan Pergeseran Waktu Setiap Komponen Frekuensi." },
  { "en": "Sinyal Sederhana Di Domain Waktu?", "id": "Gelombang Sinus Murni Pada Satu Frekuensi." },
  { "en": "Bagaimana Tampilan Sinus Di Domain Frekuensi?", "id": "Sebuah Puncak Tunggal Pada Frekuensinya." },
  { "en": "Sinyal Kompleks Di Domain Waktu?", "id": "Gabungan Dari Banyak Gelombang Sinus." },
  { "en": "Bagaimana Tampilan Sinyal Kompleks Di Frekuensi?", "id": "Banyak Puncak Pada Berbagai Frekuensi." },
  { "en": "Apa Itu Sinyal Periodik?", "id": "Sinyal Yang Berulang Dalam Pola Tertentu." },
  { "en": "Spektrum Sinyal Periodik?", "id": "Terdiri Dari Garis-Garis Diskrit (Harmonik)." },
  { "en": "Apa Itu Sinyal Non-Periodik?", "id": "Sinyal Yang Tidak Pernah Berulang Tepat." },
  { "en": "Spektrum Sinyal Non-Periodik?", "id": "Spektrum Kontinu Yang Tidak Terputus." },
  { "en": "Apa Itu Frekuensi Dasar?", "id": "Frekuensi Terendah Dari Sinyal Periodik." },
  { "en": "Apa Itu Harmonik?", "id": "Kelipatan Bilangan Bulat Dari Frekuensi Dasar." },
  { "en": "Instrumen Apa Yang Berguna Di Domain Waktu?", "id": "Osiloskop Untuk Melihat Bentuk Gelombang." },
  { "en": "Instrumen Apa Yang Berguna Di Domain Frekuensi?", "id": "Spectrum Analyzer Untuk Melihat Spektrum." },
  { "en": "Mana Yang Lebih Baik?", "id": "Keduanya Memberi Perspektif Berbeda Yang Berguna." },
  { "en": "Analogi Untuk Dua Domain?", "id": "Bahan Kue Dan Resep Kue." },
  { "en": "Bahan Kue Adalah?", "id": "Domain Frekuensi (Komponen-Komponennya)." },
  { "en": "Kue Jadi Adalah?", "id": "Domain Waktu (Hasil Akhir Gabungan)." },
  { "en": "Apa Itu Sinyal DC?", "id": "Sinyal Dengan Frekuensi Nol (Konstan)." },
  { "en": "Bagaimana Tampilan Sinyal DC Di Frekuensi?", "id": "Satu Puncak Di Frekuensi Nol." },
  { "en": "Apa Itu Sinyal AC?", "id": "Sinyal Yang Bervariasi Seiring Waktu." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Yang Ditempati Sinyal." },
  { "en": "Bandwidth Diukur Di Domain Mana?", "id": "Diukur Dalam Domain Frekuensi." },
  { "en": "Sinyal Berubah Cepat Di Waktu?", "id": "Memiliki Bandwidth Lebar Di Frekuensi." },
  { "en": "Sinyal Berubah Lambat Di Waktu?", "id": "Memiliki Bandwidth Sempit Di Frekuensi." },
  { "en": "Apa Itu Filter?", "id": "Sistem Yang Memodifikasi Spektrum Sinyal." },
  { "en": "Filter Bekerja Di Domain Mana?", "id": "Paling Mudah Dianalisis Di Domain Frekuensi." },
  { "en": "Apa Itu Filter Low-Pass?", "id": "Melewatkan Frekuensi Rendah Menahan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Frekuensi Tinggi Menahan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Pita Frekuensi Tertentu Saja." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Di Domain Waktu." },
  { "en": "Efek Konvolusi Di Domain Frekuensi?", "id": "Menjadi Operasi Perkalian Yang Jauh Lebih Mudah." },
  { "en": "Ini Disebut Apa?", "id": "Teorema Konvolusi Yang Sangat Penting." },
  { "en": "Perkalian Di Domain Waktu?", "id": "Menjadi Konvolusi Di Domain Frekuensi." },
  { "en": "Apa Itu Impuls Dirac?", "id": "Pulsa Sangat Sempit Dengan Luas Satu." },
  { "en": "Spektrum Impuls Dirac?", "id": "Spektrum Datar Mengandung Semua Frekuensi." },
  { "en": "Apa Itu Fungsi Jendela?", "id": "Fungsi Untuk Memilih Sebagian Sinyal." },
  { "en": "Efek Fungsi Jendela?", "id": "Menyebabkan Penyebaran Spektrum Di Domain Frekuensi." },
  { "en": "Apa Itu Kebisingan (Noise)?", "id": "Sinyal Acak Yang Tidak Diinginkan." },
  { "en": "Bagaimana Kebisingan Terlihat Di Frekuensi?", "id": "Sering Tersebar Di Seluruh Spektrum." },
  { "en": "Bagaimana Cara Mengurangi Kebisingan?", "id": "Menggunakan Filter Untuk Menghilangkan Frekuensi Noise." },
  { "en": "Apa Itu Sampling?", "id": "Mengukur Sinyal Kontinu Pada Interval Waktu." },
  { "en": "Efek Sampling Di Domain Frekuensi?", "id": "Menyebabkan Spektrum Asli Berulang Kembali." },
  { "en": "Apa Itu Aliasing?", "id": "Tumpang Tindih Spektrum Akibat Sampling Lambat." },
  { "en": "Bagaimana Mencegah Aliasing?", "id": "Sampling Cukup Cepat (Teorema Nyquist)." },
  { "en": "Domain Waktu Berguna Untuk?", "id": "Melihat Amplitudo Puncak Dan Durasi Sinyal." },
  { "en": "Domain Frekuensi Berguna Untuk?", "id": "Melihat Komposisi Harmonik Dan Bandwidth." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Parameter Yang Diukur Di Domain Waktu." },
  { "en": "Waktu Naik Cepat Berarti?", "id": "Komponen Frekuensi Tinggi Yang Kuat." },
  { "en": "Apa Itu Overshoot?", "id": "Sinyal Melebihi Nilai Targetnya Sesaat." },
  { "en": "Overshoot Diukur Di Domain?", "id": "Domain Waktu Tentunya." },
  { "en": "Apa Itu Respon Transien?", "id": "Perilaku Awal Sistem Saat Diberi Input." },
  { "en": "Apa Itu Respon Tunak (Steady-State)?", "id": "Perilaku Sistem Setelah Waktu Yang Lama." },
  { "en": "Analisis Respon Transien Dilakukan Di?", "id": "Domain Waktu Atau Domain Laplace (s)." },
  { "en": "Analisis Respon Tunak Dilakukan Di?", "id": "Domain Frekuensi (Fourier) Sangat Efektif." },
  { "en": "Apa Itu Fasa?", "id": "Informasi Posisi Relatif Gelombang Dalam Waktu." },
  { "en": "Pentingnya Informasi Fasa?", "id": "Sangat Penting Untuk Merekonstruksi Sinyal Asli." },
  { "en": "Bisakah Kita Mendengar Fasa?", "id": "Telinga Manusia Kurang Sensitif Terhadap Fasa." },
  { "en": "Bisakah Kita Mendengar Amplitudo Spektrum?", "id": "Ya Itu Yang Menentukan Nada Suara." },
  { "en": "Apa Itu Distorsi Harmonik Total (THD)?", "id": "Ukuran Seberapa Banyak Harmonik Yang Tidak Diinginkan." },
  { "en": "THD Diukur Di Domain Mana?", "id": "Domain Frekuensi Dengan Mengukur Kekuatan Harmonik." },
  { "en": "Apa Itu Sinyal Waktu Kontinu?", "id": "Sinyal Didefinisikan Untuk Setiap Titik Waktu." },
  { "en": "Apa Itu Sinyal Waktu Diskrit?", "id": "Sinyal Didefinisikan Hanya Pada Interval Tertentu." },
  { "en": "Transformasi Untuk Sinyal Diskrit?", "id": "Transformasi Fourier Waktu Diskrit (DTFT)." },
  { "en": "Apa Itu Fast Fourier Transform (FFT)?", "id": "Algoritma Cepat Untuk Menghitung Transformasi Fourier." },
  { "en": "FFT Bekerja Pada Sinyal?", "id": "Sinyal Waktu Diskrit Yang Sudah Di-sampling." },
  { "en": "Sumbu Waktu Variabelnya Apa?", "id": "t (time) Dalam Satuan Detik." },
  { "en": "Sumbu Frekuensi Variabelnya Apa?", "id": "f (frekuensi) Atau ω (frekuensi sudut)." },
  { "en": "Hubungan f Dan ω?", "id": "Omega Sama Dengan Dua Pi Kali f." },
  { "en": "Variabel Domain Laplace?", "id": "s (Frekuensi Kompleks σ + jω)." },
  { "en": "Domain Laplace Adalah Perluasan Dari?", "id": "Domain Frekuensi Dengan Memasukkan Faktor Redaman." },
  { "en": "Gelombang Kotak Di Domain Waktu?", "id": "Sinyal Yang Naik Turun Secara Tajam." },
  { "en": "Spektrum Gelombang Kotak?", "id": "Terdiri Dari Harmonik Ganjil Yang Melemah." },
  { "en": "Gelombang Segitiga Di Domain Waktu?", "id": "Sinyal Yang Naik Turun Secara Linier." },
  { "en": "Spektrum Gelombang Segitiga?", "id": "Juga Harmonik Ganjil Tapi Melemah Lebih Cepat." },
  { "en": "Sinyal Lebih Halus Di Waktu?", "id": "Komponen Frekuensi Tingginya Lebih Lemah." },
  { "en": "Sinyal Dengan Sudut Tajam Di Waktu?", "id": "Komponen Frekuensi Tingginya Lebih Kuat." },
  { "en": "Apa Itu Sistem Linier?", "id": "Sistem Yang Memenuhi Prinsip Superposisi." },
  { "en": "Sistem Linier Tidak Menghasilkan?", "id": "Frekuensi Baru Yang Tidak Ada Di Input." },
  { "en": "Apa Itu Sistem Non-Linier?", "id": "Sistem Yang Tidak Memenuhi Prinsip Superposisi." },
  { "en": "Sistem Non-Linier Bisa Menghasilkan?", "id": "Harmonik Baru Dan Distorsi Lainnya." },
  { "en": "Analisis Non-Linier Sulit Di?", "id": "Domain Frekuensi Secara Langsung." },
  { "en": "Prinsip Ketidakpastian Heisenberg?", "id": "Tidak Bisa Tahu Posisi Waktu Frekuensi Tepat." },
  { "en": "Lokalisasi Baik Di Waktu?", "id": "Menyebabkan Lokalisasi Buruk Di Frekuensi." },
  { "en": "Lokalisasi Baik Di Frekuensi?", "id": "Menyebabkan Lokalisasi Buruk Di Waktu." },
  { "en": "Apa Beda Domain Waktu Dan Frekuensi?", "id": "Waktu Melihat 'Kapan' Frekuensi Melihat 'Apa'." },
  { "en": "Sumbu X Di Domain Waktu?", "id": "Waktu (t) Dalam Satuan Detik." },
  { "en": "Sumbu Y Di Domain Waktu?", "id": "Amplitudo Sinyal (Volt Amper)." },
  { "en": "Sumbu X Di Domain Frekuensi?", "id": "Frekuensi (f) Dalam Satuan Hertz." },
  { "en": "Sumbu Y Di Domain Frekuensi?", "id": "Besaran Amplitudo Dan Fasa Sinyal." },
  { "en": "Grafik Domain Waktu Disebut?", "id": "Bentuk Gelombang Atau Waveform." },
  { "en": "Grafik Domain Frekuensi Disebut?", "id": "Spektrum Sinyal (Spectrum)." },
  { "en": "Alat Ukur Domain Waktu?", "id": "Osiloskop (Oscilloscope)." },
  { "en": "Alat Ukur Domain Frekuensi?", "id": "Penganalisis Spektrum (Spectrum Analyzer)." },
  { "en": "Detak Jantung Dianalisis Di?", "id": "Domain Waktu Untuk Melihat Ritme." },
  { "en": "Kualitas Audio Dianalisis Di?", "id": "Domain Frekuensi Untuk Melihat Harmonik." },
  { "en": "Apa Itu Periode (T)?", "id": "Waktu Untuk Menyelesaikan Satu Siklus." },
  { "en": "Periode Diukur Di Domain?", "id": "Domain Waktu." },
  { "en": "Apa Itu Frekuensi (f)?", "id": "Jumlah Siklus Per Detik." },
  { "en": "Frekuensi Diukur Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Hubungan Periode Dan Frekuensi?", "id": "Frekuensi Adalah Satu Dibagi Periode (1/T)." },
  { "en": "Sinyal Tajam Di Waktu?", "id": "Memiliki Banyak Komponen Frekuensi Tinggi." },
  { "en": "Sinyal Halus Di Waktu?", "id": "Memiliki Sedikit Komponen Frekuensi Tinggi." },
  { "en": "Apa Itu Teorema Fourier?", "id": "Setiap Sinyal Adalah Jumlah Gelombang Sinus." },
  { "en": "Teorema Ini Adalah Dasar Dari?", "id": "Analisis Domain Frekuensi Modern." },
  { "en": "Apa Itu Koefisien Fourier?", "id": "Amplitudo Dari Setiap Gelombang Sinus Komponen." },
  { "en": "Koefisien Ini Dipetakan Di?", "id": "Domain Frekuensi." },
  { "en": "Untuk Pindah Ke Domain Frekuensi Kita?", "id": "Melakukan Transformasi Fourier." },
  { "en": "Untuk Kembali Ke Domain Waktu Kita?", "id": "Melakukan Transformasi Fourier Balik." },
  { "en": "Operasi Perkalian Di Waktu?", "id": "Menjadi Konvolusi Di Frekuensi." },
  { "en": "Operasi Konvolusi Di Waktu?", "id": "Menjadi Perkalian Di Frekuensi." },
  { "en": "Mana Yang Lebih Mudah Dihitung?", "id": "Perkalian Di Domain Frekuensi Jauh Lebih Mudah." },
  { "en": "Efek Sistem LTI Di Waktu?", "id": "Konvolusi Antara Input Dan Respon Impuls." },
  { "en": "Efek Sistem LTI Di Frekuensi?", "id": "Perkalian Antara Spektrum Input Dan Respon Frekuensi." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Bagaimana Sistem Merespon Setiap Frekuensi." },
  { "en": "Respon Frekuensi Adalah?", "id": "Transformasi Fourier Dari Respon Impuls." },
  { "en": "Pulsa Sempit Di Waktu?", "id": "Memiliki Spektrum Sangat Lebar Di Frekuensi." },
  { "en": "Pulsa Lebar Di Waktu?", "id": "Memiliki Spektrum Sangat Sempit Di Frekuensi." },
  { "en": "Hubungan Ini Disebut?", "id": "Sifat Penskalaan (Scaling Property)." },
  { "en": "Menunda Sinyal Di Waktu?", "id": "Mengubah Fasa Di Domain Frekuensi." },
  { "en": "Sifat Ini Disebut Apa?", "id": "Sifat Pergeseran Waktu (Time Shift Property)." },
  { "en": "Menggeser Sinyal Di Frekuensi?", "id": "Mengalikan Dengan Sinusoid Kompleks Di Waktu." },
  { "en": "Sifat Ini Disebut Apa?", "id": "Sifat Pergeseran Frekuensi (Modulasi)." },
  { "en": "Apa Itu Durasi Sinyal?", "id": "Panjang Waktu Sinyal Ada." },
  { "en": "Durasi Diukur Di Domain?", "id": "Domain Waktu." },
  { "en": "Bandwidth Dan Durasi Berbanding?", "id": "Berbanding Terbalik Satu Sama Lain." },
  { "en": "Ini Adalah Prinsip Dasar?", "id": "Prinsip Ketidakpastian Dalam Sinyal." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Kedatangan Sinyal." },
  { "en": "Jitter Adalah Fenomena Domain?", "id": "Domain Waktu." },
  { "en": "Efek Jitter Di Frekuensi?", "id": "Menyebabkan Noise Fasa (Phase Noise)." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Jumlah Data Untuk Representasi Sinyal." },
  { "en": "Kompresi MP3 Bekerja Di?", "id": "Domain Frekuensi Dengan Membuang Komponen Lemah." },
  { "en": "Kompresi ZIP Bekerja Di?", "id": "Domain Waktu Dengan Mencari Pola Berulang." },
  { "en": "Apa Itu Sinyal Real?", "id": "Sinyal Yang Nilainya Selalu Bilangan Riil." },
  { "en": "Spektrum Sinyal Real?", "id": "Memiliki Simetri Konjugat Kompleks." },
  { "en": "Artinya Magnitudo Spektrum?", "id": "Simetris Terhadap Frekuensi Nol (Genap)." },
  { "en": "Artinya Fasa Spektrum?", "id": "Anti-Simetris Terhadap Frekuensi Nol (Ganjil)." },
  { "en": "Apa Itu Sinyal Kompleks?", "id": "Sinyal Yang Nilainya Bisa Bilangan Kompleks." },
  { "en": "Spektrum Sinyal Kompleks?", "id": "Tidak Harus Simetris Sama Sekali." },
  { "en": "Sinyal Kompleks Digunakan Di?", "id": "Komunikasi Digital Modern (Modulasi QAM)." },
  { "en": "Apa Itu Energi Sinyal?", "id": "Integral Kuadrat Amplitudo Sinyal." },
  { "en": "Teorema Parseval Menyatakan?", "id": "Energi Di Domain Waktu Frekuensi Sama." },
  { "en": "Apa Itu Kepadatan Spektral Energi?", "id": "Distribusi Energi Sinyal Per Satuan Frekuensi." },
  { "en": "Apa Itu Daya Sinyal?", "id": "Energi Rata-Rata Sinyal Per Satuan Waktu." },
  { "en": "Kapan Kita Menganalisis Daya?", "id": "Untuk Sinyal Periodik Yang Energinya Tak Hingga." },
  { "en": "Apa Itu Kepadatan Spektral Daya?", "id": "Distribusi Daya Sinyal Di Domain Frekuensi." },
  { "en": "Kepadatan Spektral Diukur Dengan?", "id": "Spectrum Analyzer." },
  { "en": "Gelombang Otak (EEG) Dianalisis Di?", "id": "Domain Frekuensi Untuk Melihat Pita Delta Theta." },
  { "en": "Analisis Getaran Mesin Dilakukan Di?", "id": "Domain Frekuensi Untuk Deteksi Kerusakan." },
  { "en": "Apa Itu Fungsi Jendela Persegi?", "id": "Memotong Sinyal Secara Tiba-Tiba." },
  { "en": "Efek Jendela Persegi Di Frekuensi?", "id": "Menimbulkan Bocoran Spektral (Spectral Leakage)." },
  { "en": "Apa Itu Jendela Hamming Atau Hanning?", "id": "Fungsi Jendela Halus Untuk Kurangi Bocoran." },
  { "en": "Apa Itu Resolusi Waktu?", "id": "Kemampuan Membedakan Dua Peristiwa Berdekatan." },
  { "en": "Apa Itu Resolusi Frekuensi?", "id": "Kemampuan Membedakan Dua Frekuensi Berdekatan." },
  { "en": "Resolusi Waktu Baik?", "id": "Berarti Resolusi Frekuensi Buruk." },
  { "en": "Resolusi Frekuensi Baik?", "id": "Berarti Resolusi Waktu Buruk." },
  { "en": "Ini Adalah Trade-off Fundamental?", "id": "Ya Dikenal Sebagai Batas Gabor." },
  { "en": "Apa Itu Plot Air Terjun (Waterfall)?", "id": "Plot Spektrum Yang Berubah Seiring Waktu." },
  { "en": "Plot Ini Menampilkan Tiga Sumbu?", "id": "Waktu Frekuensi Dan Amplitudo Sinyal." },
  { "en": "Apa Itu Spektogram?", "id": "Sama Dengan Plot Air Terjun." },
  { "en": "Spektogram Digunakan Untuk Menganalisis?", "id": "Sinyal Ucapan Musik Dan Sinyal Non-Stasioner." },
  { "en": "Sinyal Non-Stasioner Adalah?", "id": "Sinyal Yang Statistiknya Berubah Seiring Waktu." },
  { "en": "Apa Itu Koherensi?", "id": "Ukuran Hubungan Linier Antara Dua Sinyal." },
  { "en": "Koherensi Dianalisis Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Sumbu Logaritmik?", "id": "Skala Sumbu Yang Berbasis Logaritma." },
  { "en": "Plot Spektrum Sering Menggunakan?", "id": "Sumbu Frekuensi Logaritmik Dan Amplitudo Desibel." },
  { "en": "Kenapa Sumbu Logaritmik Berguna?", "id": "Untuk Melihat Rentang Dinamis Yang Sangat Lebar." },
  { "en": "Desibel (dB) Adalah Satuan?", "id": "Rasio Logaritmik Untuk Mengukur Amplitudo." },
  { "en": "Apa Itu Oktaf?", "id": "Interval Frekuensi Dimana Frekuensi Menjadi Dua Kali." },
  { "en": "Apa Itu Dekade?", "id": "Interval Frekuensi Dimana Frekuensi Menjadi Sepuluh Kali." },
  { "en": "Oktaf Dan Dekade Adalah Skala?", "id": "Skala Logaritmik Untuk Sumbu Frekuensi." },
  { "en": "Representasi Fasor Adalah?", "id": "Representasi Sinyal Sinusoidal Di Domain Frekuensi." },
  { "en": "Fasor Adalah Bilangan?", "id": "Bilangan Kompleks Yang Mewakili Amplitudo Fasa." },
  { "en": "Analisis Rangkaian AC Menggunakan?", "id": "Metode Fasor Di Domain Frekuensi." },
  { "en": "Apa Itu Impedansi?", "id": "Hambatan Kompleks Terhadap Arus Di Domain Frekuensi." },
  { "en": "Apa Itu Reaktansi?", "id": "Bagian Imajiner Dari Impedansi." },
  { "en": "Analisis Rangkaian Di Waktu?", "id": "Menghasilkan Persamaan Diferensial Yang Rumit." },
  { "en": "Analisis Rangkaian Di Frekuensi?", "id": "Menghasilkan Persamaan Aljabar Yang Mudah." },
  { "en": "Apa Itu Domain-z?", "id": "Domain Frekuensi Untuk Sinyal Waktu Diskrit." },
  { "en": "Hubungan Domain-z Dan Domain Frekuensi?", "id": "Lingkaran Satuan Di Domain-z Adalah Domain Frekuensi." },
  { "en": "Sinyal Musik Adalah Sinyal?", "id": "Sinyal Kompleks Di Domain Waktu." },
  { "en": "Not Musik Adalah?", "id": "Frekuensi Dasar Di Domain Frekuensi." },
  { "en": "Timbre Atau Warna Suara?", "id": "Ditentukan Oleh Amplitudo Harmonik Di Spektrum." },
  { "en": "Apa Sifat Ganda Waktu Dan Frekuensi?", "id": "Sempit Di Satu Domain Lebar Di Lainnya." },
  { "en": "Apa Efek Integrator Di Domain Waktu?", "id": "Menghaluskan Sinyal Dan Menghilangkan Detail." },
  { "en": "Apa Efek Integrator Di Domain Frekuensi?", "id": "Meredam Komponen Frekuensi Tinggi (Low-Pass)." },
  { "en": "Apa Efek Diferensiator Di Domain Waktu?", "id": "Menajamkan Tepi Dan Menyoroti Perubahan." },
  { "en": "Apa Efek Diferensiator Di Domain Frekuensi?", "id": "Menguatkan Komponen Frekuensi Tinggi (High-Pass)." },
  { "en": "Kenapa Tepi Tajam Butuh Frekuensi Tinggi?", "id": "Perubahan Cepat Membutuhkan Komponen Frekuensi Cepat." },
  { "en": "Spektrum Datar Di Frekuensi Artinya Apa?", "id": "Sinyal Impuls Sangat Sempit Di Waktu." },
  { "en": "Satu Puncak Di Frekuensi Artinya Apa?", "id": "Sinyal Sinus Murni Di Domain Waktu." },
  { "en": "Apa Blok Pembangun Sinyal Di Waktu?", "id": "Fungsi Impuls Dirac Yang Digeser." },
  { "en": "Apa Blok Pembangun Sinyal Di Frekuensi?", "id": "Fungsi Sinusoid Kompleks (e^jωt)." },
  { "en": "Sinyal Genap Di Waktu Punya Spektrum?", "id": "Spektrum Yang Bernilai Riil Dan Genap." },
  { "en": "Sinyal Ganjil Di Waktu Punya Spektrum?", "id": "Spektrum Yang Bernilai Imajiner Dan Ganjil." },
  { "en": "Apa Itu Sinyal Genap (Even)?", "id": "Simetris Terhadap Sumbu Vertikal (f(t)=f(-t))." },
  { "en": "Apa Itu Sinyal Ganjil (Odd)?", "id": "Anti-Simetris Terhadap Titik Asal (f(t)=-f(-t))." },
  { "en": "Setiap Sinyal Bisa Dipecah?", "id": "Ya Menjadi Penjumlahan Bagian Genap Ganjil." },
  { "en": "Sifat Ini Berguna Untuk?", "id": "Menyederhanakan Perhitungan Transformasi Fourier." },
  { "en": "Apa Itu Delay Waktu?", "id": "Sinyal Bergeser Ke Kanan Di Sumbu Waktu." },
  { "en": "Efek Delay Waktu Di Frekuensi?", "id": "Menyebabkan Pergeseran Fasa Linier Negatif." },
  { "en": "Apa Itu Pergeseran Fasa?", "id": "Semua Komponen Frekuensi Bergeser Waktunya." },
  { "en": "Fasa Nol Berarti Apa?", "id": "Semua Sinusoid Mulai Pada Puncak Maksimumnya." },
  { "en": "Plot Magnitudo Menunjukkan?", "id": "'Seberapa Banyak' Setiap Frekuensi Ada." },
  { "en": "Plot Fasa Menunjukkan?", "id": "'Bagaimana' Frekuensi Itu Tersusun Dalam Waktu." },
  { "en": "Apa Itu Sistem Kausal?", "id": "Output Tidak Mendahului Input Yang Menyebabkannya." },
  { "en": "Sifat Kausalitas Adalah Fenomena?", "id": "Domain Waktu (Sebab Dan Akibat)." },
  { "en": "Apa Itu Respon Impuls Sistem?", "id": "Output Sistem Jika Inputnya Impuls." },
  { "en": "Respon Impuls Adalah Representasi?", "id": "Sistem Dalam Domain Waktu." },
  { "en": "Apa Itu Respon Frekuensi Sistem?", "id": "Transformasi Fourier Dari Respon Impulsnya." },
  { "en": "Respon Frekuensi Adalah Representasi?", "id": "Sistem Dalam Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Sinyal Yang Sifat Statistiknya Konstan." },
  { "en": "Analisis Fourier Cocok Untuk Sinyal?", "id": "Sinyal Stasioner Periodik Atau Non-Periodik." },
  { "en": "Apa Itu Transformasi Wavelet?", "id": "Menganalisis Sinyal Di Waktu Dan Frekuensi Bersamaan." },
  { "en": "Wavelet Baik Untuk Sinyal?", "id": "Sinyal Non-Stasioner Yang Berubah Sifatnya." },
  { "en": "Apa Itu Plot Bode?", "id": "Plot Respon Frekuensi (Magnitudo Dan Fasa)." },
  { "en": "Plot Bode Adalah Tampilan Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Plot Respon Step?", "id": "Plot Output Sistem Terhadap Input Step." },
  { "en": "Plot Respon Step Adalah Tampilan Di?", "id": "Domain Waktu." },
  { "en": "Apa Itu Sinyal Pita Dasar (Baseband)?", "id": "Spektrum Terpusat Di Sekitar Frekuensi Nol." },
  { "en": "Apa Itu Sinyal Lolos Pita (Passband)?", "id": "Spektrum Terpusat Di Sekitar Frekuensi Pembawa." },
  { "en": "Radio AM/FM Adalah Sinyal?", "id": "Sinyal Lolos Pita (Passband)." },
  { "en": "Sinyal Audio Adalah Sinyal?", "id": "Sinyal Pita Dasar (Baseband)." },
  { "en": "Apa Itu Modulasi?", "id": "Menggeser Spektrum Sinyal Ke Frekuensi Lebih Tinggi." },
  { "en": "Modulasi Adalah Operasi?", "id": "Perkalian Di Domain Waktu." },
  { "en": "Apa Itu Demodulasi?", "id": "Menggeser Spektrum Sinyal Kembali Ke Frekuensi Nol." },
  { "en": "Apa Itu Sinyal Pita Sempit (Narrowband)?", "id": "Sinyal Dengan Bandwidth Sangat Sempit." },
  { "en": "Apa Itu Sinyal Pita Lebar (Wideband)?", "id": "Sinyal Dengan Bandwidth Sangat Lebar." },
  { "en": "Wifi Dan 4G Adalah Sinyal?", "id": "Sinyal Pita Lebar." },
  { "en": "Apa Itu Kanal Komunikasi?", "id": "Medium Fisik Tempat Sinyal Dilewatkan." },
  { "en": "Karakteristik Kanal Dideskripsikan Oleh?", "id": "Respon Frekuensi Dari Kanal Tersebut." },
  { "en": "Apa Itu Fading?", "id": "Fluktuasi Amplitudo Sinyal Akibat Kanal." },
  { "en": "Fading Adalah Fenomena Domain?", "id": "Domain Waktu." },
  { "en": "Apa Itu Equalizer?", "id": "Filter Untuk Membatalkan Efek Distorsi Kanal." },
  { "en": "Equalizer Bekerja Di Domain?", "id": "Domain Frekuensi Untuk Mengoreksi Spektrum." },
  { "en": "Apa Itu Desain Filter?", "id": "Menentukan Respon Frekuensi Yang Diinginkan." },
  { "en": "Realisasi Filter Adalah?", "id": "Membangun Rangkaian Fisik Dari Desain." },
  { "en": "Apa Itu Orde Filter?", "id": "Menentukan Ketajaman Transisi Filter." },
  { "en": "Orde Filter Lebih Tinggi?", "id": "Transisi Lebih Tajam Mendekati Ideal." },
  { "en": "Apa Itu Ripple?", "id": "Variasi Gain Di Passband Filter." },
  { "en": "Ripple Terlihat Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Group Delay?", "id": "Ukuran Penundaan Sinyal Melewati Sistem." },
  { "en": "Group Delay Dihitung Di Domain?", "id": "Domain Frekuensi Dari Turunan Fasa." },
  { "en": "Apa Itu Fasa Linier?", "id": "Fasa Berubah Linier Terhadap Frekuensi." },
  { "en": "Sistem Dengan Fasa Linier?", "id": "Tidak Menyebabkan Distorsi Bentuk Gelombang." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit Dalam Waktu Dan Amplitudo." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Dalam Waktu Dan Amplitudo." },
  { "en": "Dunia Nyata Adalah Dunia?", "id": "Analog Dan Kontinu." },
  { "en": "Komputer Bekerja Di Dunia?", "id": "Digital Dan Diskrit." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Menjadi Digital." },
  { "en": "Proses ADC Melibatkan?", "id": "Sampling Dan Kuantisasi Sinyal." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Kembali Ke Analog." },
  { "en": "Apa Itu Interpolasi?", "id": "Menebak Nilai Di Antara Sampel." },
  { "en": "DAC Melakukan Proses?", "id": "Interpolasi Dan Penghalusan Sinyal." },
  { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Derau Dengan Spektrum Amplitudo Datar." },
  { "en": "Artinya Derau Putih Mengandung?", "id": "Semua Frekuensi Dengan Kekuatan Sama." },
  { "en": "Apa Itu Derau Merah Jambu (Pink Noise)?", "id": "Spektrum Menurun Seiring Kenaikan Frekuensi." },
  { "en": "Telinga Manusia Menganggap Pink Noise?", "id": "Terdengar Lebih Seimbang Daripada White Noise." },
  { "en": "Apa Itu Sinyal Chirp?", "id": "Sinyal Dimana Frekuensinya Berubah Seiring Waktu." },
  { "en": "Radar Dan Sonar Menggunakan?", "id": "Sinyal Chirp Untuk Pengukuran Jarak." },
  { "en": "Apa Itu Time-Frequency Analysis?", "id": "Analisis Yang Menampilkan Kedua Domain Sekaligus." },
  { "en": "Spektogram Adalah Contoh?", "id": "Representasi Analisis Waktu-Frekuensi." },
  { "en": "Apa Itu Sinyal Vokal?", "id": "Sinyal Periodik Dihasilkan Pita Suara." },
  { "en": "Apa Itu Sinyal Konsonan?", "id": "Sinyal Non-Periodik Mirip Derau." },
  { "en": "Pengenalan Ucapan Menganalisis?", "id": "Perubahan Spektrum Sinyal Seiring Waktu." },
  { "en": "Apa Itu Amplitudo Modulasi (AM)?", "id": "Amplitudo Pembawa Diubah Sesuai Sinyal Informasi." },
  { "en": "Apa Itu Frekuensi Modulasi (FM)?", "id": "Frekuensi Pembawa Diubah Sesuai Sinyal Informasi." },
  { "en": "AM Dan FM Adalah Proses?", "id": "Domain Waktu." },
  { "en": "Efeknya Paling Jelas Terlihat Di?", "id": "Domain Frekuensi (Sidebands)." },
  { "en": "Apa Itu Multiplexing?", "id": "Mengirim Banyak Sinyal Melalui Satu Kanal." },
  { "en": "Apa Itu FDM (Frequency Division Multiplexing)?", "id": "Membagi Kanal Berdasarkan Pita Frekuensi." },
  { "en": "FDM Adalah Konsep Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu TDM (Time Division Multiplexing)?", "id": "Membagi Kanal Berdasarkan Selot Waktu." },
  { "en": "TDM Adalah Konsep Domain?", "id": "Domain Waktu." },
  { "en": "Ponsel Menggunakan Kombinasi?", "id": "Berbagai Jenis Skema Multiplexing." },
  { "en": "Melihat Sinyal Di Waktu?", "id": "Memberi Informasi Tentang Evolusi Sinyal." },
  { "en": "Melihat Sinyal Di Frekuensi?", "id": "Memberi Informasi Tentang Komposisi Sinyal." },
  { "en": "Kedua Tampilan Saling Melengkapi?", "id": "Ya Memberikan Pemahaman Lengkap Tentang Sinyal." },
  { "en": "Durasi Sinyal Pendek Berarti Spektrumnya?", "id": "Sangat Lebar Dan Tersebar Luas." },
  { "en": "Bandwidth Sempit Berarti Sinyal Di Waktu?", "id": "Berubah Secara Lambat Dan Sangat Halus." },
  { "en": "Kenapa Melihat Getaran Mesin Di Frekuensi?", "id": "Untuk Mengidentifikasi Frekuensi Getaran Abnormal." },
  { "en": "Kenapa Menganalisis Sinyal EKG Di Waktu?", "id": "Untuk Melihat Interval Dan Ritme Jantung." },
  { "en": "Efek Perkalian Sinyal Di Waktu?", "id": "Menghasilkan Konvolusi Di Domain Frekuensi." },
  { "en": "Efek Menjumlahkan Sinyal Di Waktu?", "id": "Spektrumnya Juga Akan Saling Menjumlah." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Sifat Statistiknya Tidak Berubah Seiring Waktu." },
  { "en": "Contoh Sinyal Stasioner?", "id": "Derau Putih Atau Gelombang Sinus Murni." },
  { "en": "Apa Itu Sinyal Non-Stasioner?", "id": "Sifat Statistiknya Berubah Seiring Waktu." },
  { "en": "Contoh Sinyal Non-Stasioner?", "id": "Sinyal Ucapan Musik Atau Sinyal Chirp." },
  { "en": "Dalam Analogi Musik, Ritme Adalah?", "id": "Pola Peristiwa Dalam Domain Waktu." },
  { "en": "Dalam Analogi Musik, Nada Adalah?", "id": "Frekuensi Dasar Dalam Domain Frekuensi." },
  { "en": "Gain Sistem Adalah Parameter Domain?", "id": "Domain Frekuensi Yang Menunjukkan Penguatan." },
  { "en": "Waktu Tunda Sistem Adalah Parameter Domain?", "id": "Domain Waktu Yang Menunjukkan Penundaan." },
  { "en": "Apa Itu Pita Sisi (Sideband)?", "id": "Pita Frekuensi Baru Akibat Proses Modulasi." },
  { "en": "Pita Sisi Terlihat Di Domain Mana?", "id": "Domain Frekuensi Di Sekitar Frekuensi Pembawa." },
  { "en": "Apa Itu Pembawa (Carrier)?", "id": "Gelombang Frekuensi Tinggi Untuk Membawa Informasi." },
  { "en": "Pembawa Adalah Sinyal Sederhana Di?", "id": "Kedua Domain (Sinus Murni)." },
  { "en": "Informasi Adalah Sinyal Kompleks Di?", "id": "Domain Waktu." },
  { "en": "Apa Itu Linearitas Sistem?", "id": "Prinsip Superposisi Berlaku Pada Sistem." },
  { "en": "Linearitas Mudah Dianalisis Di?", "id": "Domain Frekuensi Karena Tidak Ada Frekuensi Baru." },
  { "en": "Apa Itu Non-Linearitas Sistem?", "id": "Menghasilkan Distorsi Dan Komponen Frekuensi Baru." },
  { "en": "Distorsi Dikuantifikasi Di Domain?", "id": "Domain Frekuensi (Misalnya THD)." },
  { "en": "Apa Itu Sinyal Real-Time?", "id": "Sinyal Yang Diproses Saat Itu Juga." },
  { "en": "Pemrosesan Real-Time Adalah Konsep?", "id": "Domain Waktu." },
  { "en": "Apa Itu Pemrosesan Offline?", "id": "Sinyal Direkam Dulu Lalu Diproses." },
  { "en": "FFT Adalah Pemrosesan?", "id": "Offline Pada Blok Data." },
  { "en": "Apa Itu Windowing?", "id": "Mengalikan Sinyal Dengan Fungsi Jendela." },
  { "en": "Windowing Adalah Operasi Domain?", "id": "Domain Waktu." },
  { "en": "Efek Windowing Terlihat Di?", "id": "Domain Frekuensi (Bocoran Spektral)." },
  { "en": "Apa Itu Zero Padding?", "id": "Menambahkan Sampel Nol Pada Sinyal." },
  { "en": "Zero Padding Adalah Operasi Domain?", "id": "Domain Waktu." },
  { "en": "Efek Zero Padding Di Frekuensi?", "id": "Meningkatkan Resolusi Tampilan Spektrum." },
  { "en": "Apakah Zero Padding Menambah Informasi?", "id": "Tidak Hanya Menginterpolasi Tampilan Spektrum." },
  { "en": "Sinyal Simetris Di Waktu?", "id": "Memiliki Fasa Nol Atau 180 Derajat." },
  { "en": "Apa Itu Simetri Genap?", "id": "Simetris Terhadap Sumbu Y." },
  { "en": "Apa Itu Simetri Ganjil?", "id": "Simetris Terhadap Titik Asal." },
  { "en": "Kosinus Adalah Fungsi?", "id": "Fungsi Genap." },
  { "en": "Sinus Adalah Fungsi?", "id": "Fungsi Ganjil." },
  { "en": "Transformasi Fourier Kosinus?", "id": "Bernilai Riil." },
  { "en": "Transformasi Fourier Sinus?", "id": "Bernilai Imajiner." },
  { "en": "Apa Itu Plot Konstelasi?", "id": "Plot Sinyal Komunikasi Di Bidang Kompleks." },
  { "en": "Plot Konstelasi Adalah Tampilan?", "id": "Domain Waktu (Pada Momen Sampling)." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Tampilan Sinyal Digital Untuk Analisis Kualitas." },
  { "en": "Diagram Mata Adalah Tampilan?", "id": "Domain Waktu Yang Ditumpuk." },
  { "en": "Bukaan Mata Yang Lebar Menandakan?", "id": "Sinyal Berkualitas Baik Dengan Noise Rendah." },
  { "en": "Apa Itu Intersymbol Interference (ISI)?", "id": "Satu Simbol Mengganggu Simbol Berikutnya." },
  { "en": "ISI Adalah Fenomena Domain?", "id": "Domain Waktu." },
  { "en": "Penyebab ISI Adalah?", "id": "Distorsi Kanal Di Domain Frekuensi." },
  { "en": "Equalizer Mengurangi ISI Di?", "id": "Domain Waktu Dengan Mengoreksi Domain Frekuensi." },
  { "en": "Apa Itu Respon Impuls Kanal?", "id": "Bagaimana Kanal Merespon Pulsa Sangat Pendek." },
  { "en": "Respon Impuls Kanal Adalah Deskripsi?", "id": "Kanal Dalam Domain Waktu." },
  { "en": "Respon Frekuensi Kanal Adalah Deskripsi?", "id": "Kanal Dalam Domain Frekuensi." },
  { "en": "Jika Kanal Sempurna Respon Impulsnya?", "id": "Sebuah Impuls Sempurna." },
  { "en": "Jika Kanal Sempurna Respon Frekuensinya?", "id": "Garis Datar Sempurna." },
  { "en": "Kanal Nyata Menyebabkan Respon Impuls?", "id": "Menyebar Dan Memiliki Ekor." },
  { "en": "Ekor Inilah Yang Menyebabkan?", "id": "Intersymbol Interference (ISI)." },
  { "en": "Apa Itu Osilasi?", "id": "Gerakan Berulang Di Sekitar Titik Setimbang." },
  { "en": "Osilasi Adalah Perilaku Di?", "id": "Domain Waktu." },
  { "en": "Frekuensi Osilasi Adalah Parameter?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Peluruhan (Decay)?", "id": "Penurunan Amplitudo Sinyal Seiring Waktu." },
  { "en": "Peluruhan Adalah Perilaku Di?", "id": "Domain Waktu." },
  { "en": "Laju Peluruhan Terkait Dengan?", "id": "Komponen Riil Di Domain Laplace." },
  { "en": "Apa Itu Time Constant?", "id": "Waktu Yang Dibutuhkan Sinyal Untuk Meluruh." },
  { "en": "Time Constant Adalah Parameter?", "id": "Domain Waktu." },
  { "en": "Time Constant Dan Bandwidth?", "id": "Berbanding Terbalik Satu Sama Lain." },
  { "en": "Sistem Cepat Memiliki Time Constant?", "id": "Kecil Dan Bandwidth Yang Lebar." },
  { "en": "Sistem Lambat Memiliki Time Constant?", "id": "Besar Dan Bandwidth Yang Sempit." },
  { "en": "Apa Itu Sinyal Deterministik?", "id": "Sinyal Yang Bisa Diprediksi Secara Tepat." },
  { "en": "Apa Itu Sinyal Stokastik (Acak)?", "id": "Sinyal Yang Tidak Bisa Diprediksi Tepat." },
  { "en": "Fourier Digunakan Untuk Sinyal?", "id": "Sinyal Deterministik." },
  { "en": "Analisis Sinyal Acak Menggunakan?", "id": "Kepadatan Spektral Daya (Power Spectral Density)." },
  { "en": "Kepadatan Spektral Adalah Konsep?", "id": "Domain Frekuensi Untuk Sinyal Acak." },
  { "en": "Apa Itu Autokorelasi?", "id": "Mengukur Kemiripan Sinyal Dengan Dirinya Sendiri." },
  { "en": "Autokorelasi Adalah Fungsi Domain?", "id": "Domain Waktu (Lag Waktu)." },
  { "en": "Hubungan Keduanya Diberikan Oleh?", "id": "Teorema Wiener-Khinchin." },
  { "en": "Apa Itu Cross-korelasi?", "id": "Mengukur Kemiripan Antara Dua Sinyal Berbeda." },
  { "en": "GPS Menggunakan Cross-korelasi Di?", "id": "Domain Waktu Untuk Menentukan Penundaan Sinyal." },
  { "en": "Apa Itu Sinyal Chirp?", "id": "Sinyal Sinusoidal Dengan Frekuensi Yang Menyapu." },
  { "en": "Frekuensi Sinyal Chirp Berubah Di?", "id": "Domain Waktu." },
  { "en": "Spektrum Sinyal Chirp?", "id": "Memiliki Bandwidth Yang Lebar Dan Datar." },
  { "en": "Apa Itu Transformasi Hilbert?", "id": "Menggeser Fasa Semua Komponen Frekuensi." },
  { "en": "Seberapa Besar Pergeseran Fasanya?", "id": "Minus 90 Derajat." },
  { "en": "Transformasi Hilbert Adalah Operasi?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Kompleks Dengan Spektrum Satu Sisi." },
  { "en": "Bagaimana Membuat Sinyal Analitik?", "id": "Menggabungkan Sinyal Asli Dengan Transformasi Hilbertnya." },
  { "en": "Apa Itu Amplitudo Sesaat?", "id": "Amplop Dari Sinyal Di Domain Waktu." },
  { "en": "Apa Itu Frekuensi Sesaat?", "id": "Laju Perubahan Fasa Di Domain Waktu." },
  { "en": "Keduanya Dihitung Dari?", "id": "Sinyal Analitik." },
  { "en": "Bisakah Sinyal Memiliki Bandwidth Tak Terbatas?", "id": "Secara Teoritis Ya (Contoh Impuls)." },
  { "en": "Bisakah Sinyal Memiliki Durasi Tak Terbatas?", "id": "Secara Teoritis Ya (Contoh Sinus Murni)." },
  { "en": "Sinyal Dunia Nyata Selalu?", "id": "Terbatas Dalam Waktu Dan Bandwidth." },
  { "en": "Asumsi Tak Terbatas Adalah?", "id": "Penyederhanaan Matematis Yang Berguna." },
  { "en": "Apa Itu 'Waktu'?", "id": "Dimensi Dimana Peristiwa Terjadi Berurutan." },
  { "en": "Apa Itu 'Frekuensi'?", "id": "Ukuran Keterulangan Suatu Peristiwa." },
  { "en": "Keduanya Adalah Cara Pandang?", "id": "Berbeda Untuk Menganalisis Fenomena Yang Sama." },
  { "en": "Memahami Keduanya Adalah Kunci?", "id": "Dalam Bidang Teknik Elektro Dan Fisika." },
  { "en": "Apa Efek Filter Low-Pass Di Domain Waktu?", "id": "Menghaluskan Sinyal Menghilangkan Perubahan Cepat." },
  { "en": "Apa Efek Filter High-Pass Di Domain Waktu?", "id": "Menyoroti Tepi Tajam Menghilangkan Komponen DC." },
  { "en": "Apa Tujuan Utama Proses Modulasi?", "id": "Menggeser Spektrum Sinyal Ke Frekuensi Lain." },
  { "en": "Kenapa Modulasi Penting Untuk Radio?", "id": "Agar Banyak Stasiun Bisa Berbagi Udara." },
  { "en": "Periodisitas Di Domain Waktu Berarti?", "id": "Spektrum Diskrit Di Domain Frekuensi." },
  { "en": "Spektrum Diskrit Berarti Sinyal Di Waktu?", "id": "Sinyal Periodik Yang Berulang Terus." },
  { "en": "Sinyal Kontinu Di Frekuensi Berarti?", "id": "Sinyal Non-Periodik Di Domain Waktu." },
  { "en": "Kenapa Gambar Kabur Bisa Ditajamkan?", "id": "Dengan Menguatkan Kembali Komponen Frekuensi Tinggi." },
  { "en": "Proses Penajaman Gambar Adalah?", "id": "Operasi Filter High-Pass Di Domain Frekuensi." },
  { "en": "Pengaturan 'Time/Div' Ada Di Alat Apa?", "id": "Osiloskop Untuk Mengatur Skala Sumbu Waktu." },
  { "en": "Pengaturan 'Center Frequency' Ada Di Alat Apa?", "id": "Spectrum Analyzer Untuk Frekuensi Tengah." },
  { "en": "Pengaturan 'Span' Ada Di Alat Apa?", "id": "Spectrum Analyzer Untuk Rentang Frekuensi." },
  { "en": "Pengaturan 'Volts/Div' Ada Di Alat Apa?", "id": "Osiloskop Untuk Mengatur Skala Amplitudo." },
  { "en": "Apa Itu Trigger Di Osiloskop?", "id": "Menstabilkan Tampilan Sinyal Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Real?", "id": "Sinyal Yang Amplitudonya Selalu Bilangan Riil." },
  { "en": "Apa Itu Sinyal Kompleks?", "id": "Sinyal Yang Amplitudonya Bisa Bilangan Kompleks." },
  { "en": "Spektrum Sinyal Real Selalu?", "id": "Memiliki Simetri Magnitudo Genap." },
  { "en": "Apa Yang Terjadi Pada Fasa Saat Diferensiasi?", "id": "Fasa Bergeser Sebesar 90 Derajat." },
  { "en": "Apa Yang Terjadi Pada Fasa Saat Integrasi?", "id": "Fasa Bergeser Sebesar -90 Derajat." },
  { "en": "Operasi Ini Adalah Operasi Domain?", "id": "Frekuensi Yang Mempengaruhi Fasa Sinyal." },
  { "en": "Apa Itu Plot Air Terjun?", "id": "Visualisasi Tiga Dimensi Spektrum Seiring Waktu." },
  { "en": "Plot Ini Berguna Untuk Sinyal?", "id": "Sinyal Non-Stasioner Seperti Sinyal Ucapan." },
  { "en": "Bagaimana Sinyal Ucapan Non-Stasioner?", "id": "Spektrumnya Berubah Sangat Cepat." },
  { "en": "Apa Itu Formant?", "id": "Puncak Resonansi Dalam Spektrum Sinyal Vokal." },
  { "en": "Formant Adalah Fitur Domain?", "id": "Domain Frekuensi Yang Mencirikan Huruf Vokal." },
  { "en": "Apa Itu Pitch?", "id": "Persepsi Frekuensi Dasar Suara Manusia." },
  { "en": "Pitch Terkait Dengan Parameter?", "id": "Frekuensi Dasar (F0) Di Domain Frekuensi." },
  { "en": "Apa Itu Jendela Waktu (Time Window)?", "id": "Interval Waktu Sinyal Yang Dianalisis." },
  { "en": "Jendela Waktu Pendek Memberi Resolusi?", "id": "Resolusi Frekuensi Buruk Resolusi Waktu Baik." },
  { "en": "Jendela Waktu Panjang Memberi Resolusi?", "id": "Resolusi Frekuensi Baik Resolusi Waktu Buruk." },
  { "en": "Apa Itu Transformasi Fourier Jangka Pendek (STFT)?", "id": "Dasar Matematis Untuk Membuat Spektogram." },
  { "en": "STFT Menganalisis Sinyal Di?", "id": "Jendela Waktu Kecil Yang Bergeser." },
  { "en": "Apa Itu Sistem Invarian Waktu?", "id": "Sifatnya Tidak Berubah Seiring Waktu." },
  { "en": "Respon Impuls Sistem Invarian Waktu?", "id": "Selalu Sama Kapanpun Diukur." },
  { "en": "Apa Itu Sistem Varian Waktu?", "id": "Sifatnya Berubah Seiring Waktu." },
  { "en": "Contoh Sistem Varian Waktu?", "id": "Roket Yang Kehilangan Massa Saat Terbang." },
  { "en": "Analisis Sistem Varian Waktu?", "id": "Jauh Lebih Rumit Daripada Sistem Invarian." },
  { "en": "Domain Frekuensi Menyederhanakan Analisis?", "id": "Hanya Untuk Sistem Linier Invarian Waktu." },
  { "en": "Apa Itu Aliasing Waktu?", "id": "Efek Dual Dari Aliasing Frekuensi." },
  { "en": "Aliasing Waktu Terjadi Saat?", "id": "Sampling Di Domain Frekuensi Terlalu Jarang." },
  { "en": "Apa Itu Sinyal Deterministik?", "id": "Sinyal Yang Bisa Diprediksi Secara Matematis." },
  { "en": "Apa Itu Sinyal Acak?", "id": "Sinyal Yang Tidak Bisa Diprediksi Tepat." },
  { "en": "Spektrum Sinyal Acak Dideskripsikan Oleh?", "id": "Kepadatan Spektral Daya (Power Spectral Density)." },
  { "en": "Bentuk Gelombang Sinyal Acak?", "id": "Selalu Terlihat Berbeda Setiap Saat." },
  { "en": "Spektrum Sinyal Acak Stasioner?", "id": "Bisa Tetap Sama Rata-Rata." },
  { "en": "Operasi Turunan Di Waktu?", "id": "Menguatkan Frekuensi Tinggi Di Spektrum." },
  { "en": "Operasi Integral Di Waktu?", "id": "Meredam Frekuensi Tinggi Di Spektrum." },
  { "en": "Apa Itu Sinyal Diskrit?", "id": "Sinyal Yang Didefinisikan Hanya Pada Titik Waktu." },
  { "en": "Apa Itu Sinyal Kontinu?", "id": "Sinyal Yang Didefinisikan Pada Semua Waktu." },
  { "en": "Transformasi Fourier Untuk Sinyal Diskrit?", "id": "Discrete-Time Fourier Transform (DTFT)." },
  { "en": "Hasil DTFT Adalah Spektrum?", "id": "Spektrum Kontinu Dan Periodik." },
  { "en": "Apa Itu DFT (Discrete Fourier Transform)?", "id": "Versi Diskrit Dari DTFT." },
  { "en": "DFT Mengubah Sampel Waktu Diskrit Menjadi?", "id": "Sampel Frekuensi Diskrit." },
  { "en": "FFT Adalah Algoritma Untuk?", "id": "Menghitung DFT Dengan Sangat Cepat." },
  { "en": "Apa Itu Teorema Sampling Nyquist-Shannon?", "id": "Frekuensi Sampling Harus Dua Kali Bandwidth." },
  { "en": "Jika Teorema Sampling Dilanggar?", "id": "Terjadi Distorsi Aliasing Yang Tidak Bisa Diperbaiki." },
  { "en": "Teorema Ini Adalah Jembatan Fundamental?", "id": "Antara Dunia Sinyal Kontinu Dan Diskrit." },
  { "en": "Analisis Sinyal Digital Bekerja Di?", "id": "Domain Waktu Dan Frekuensi Diskrit." },
  { "en": "Apa Itu Sinyal Pita Terbatas (Bandlimited)?", "id": "Sinyal Yang Spektrumnya Nol Di Atas Frekuensi." },
  { "en": "Sinyal Dunia Nyata Apakah Bandlimited?", "id": "Tidak Sepenuhnya Tapi Bisa Diaproksimasi." },
  { "en": "Filter Anti-Aliasing Adalah?", "id": "Filter Low-Pass Sebelum Proses Sampling." },
  { "en": "Tujuan Filter Anti-Aliasing?", "id": "Memastikan Sinyal Menjadi Pita Terbatas." },
  { "en": "Rekonstruksi Sinyal Adalah Proses?", "id": "Mengubah Sinyal Diskrit Kembali Ke Kontinu." },
  { "en": "Rekonstruksi Ideal Membutuhkan?", "id": "Filter Low-Pass Sempurna (Sinc Filter)." },
  { "en": "Apakah Filter Sinc Praktis?", "id": "Tidak Karena Respon Impulsnya Tak Terhingga." },
  { "en": "Apa Arti Fisis Dari Fasa?", "id": "Menyelaraskan Semua Komponen Frekuensi." },
  { "en": "Jika Fasa Diacak Apa Yang Terjadi?", "id": "Bentuk Gelombang Di Waktu Akan Hancur." },
  { "en": "Tapi Spektrum Magnitudonya?", "id": "Tetap Sama Tidak Berubah." },
  { "en": "Ini Menunjukkan Pentingnya Informasi?", "id": "Informasi Fasa Untuk Rekonstruksi Bentuk." },
  { "en": "Apa Itu Sistem Causal?", "id": "Sama Dengan Sistem Kausal." },
  { "en": "Apa Itu Sistem Memori?", "id": "Output Bergantung Pada Input Masa Lalu." },
  { "en": "Kapasitor Dan Induktor Adalah Komponen?", "id": "Komponen Dengan Memori." },
  { "en": "Resistor Adalah Komponen?", "id": "Komponen Tanpa Memori (Memoryless)." },
  { "en": "Karakteristik Sistem Dengan Memori?", "id": "Respon Frekuensinya Bervariasi Dengan Frekuensi." },
  { "en": "Karakteristik Sistem Tanpa Memori?", "id": "Respon Frekuensinya Datar (Konstan)." },
  { "en": "Apa Itu Delay Fasa?", "id": "Ukuran Penundaan Waktu Pada Frekuensi Tertentu." },
  { "en": "Apa Itu Delay Grup?", "id": "Ukuran Penundaan Amplop Sinyal." },
  { "en": "Keduanya Adalah Parameter Domain?", "id": "Domain Frekuensi Yang Berdampak Di Waktu." },
  { "en": "Jika Delay Grup Konstan?", "id": "Bentuk Sinyal Tidak Mengalami Distorsi." },
  { "en": "Apa Itu Spektrum Daya?", "id": "Sama Dengan Kepadatan Spektral Daya." },
  { "en": "Apa Itu Spektrum Energi?", "id": "Sama Dengan Kepadatan Spektral Energi." },
  { "en": "Sinyal Periodik Memiliki Spektrum?", "id": "Spektrum Daya." },
  { "en": "Sinyal Non-Periodik Memiliki Spektrum?", "id": "Spektrum Energi." },
  { "en": "Apa Itu Sinyal Kuasa?", "id": "Nama Lain Untuk Sinyal Daya." },
  { "en": "Apa Itu Hubungan Kramers-Kronig?", "id": "Menghubungkan Bagian Riil Imajiner Respon Frekuensi." },
  { "en": "Untuk Sistem Apa Hubungan Ini Berlaku?", "id": "Sistem Kausal Linier Invarian Waktu." },
  { "en": "Artinya Jika Tahu Magnitudo?", "id": "Kita Bisa Menghitung Fasa Minimumnya." },
  { "en": "Apa Itu Sistem Fasa Minimum?", "id": "Sistem Stabil Yang Inversnya Juga Stabil." },
  { "en": "Sistem Fasa Minimum Memiliki?", "id": "Pergeseran Fasa Paling Kecil Yang Mungkin." },
  { "en": "Melihat Denyut Nadi Adalah Pengukuran?", "id": "Domain Waktu (Jumlah Denyut Per Menit)." },
  { "en": "Mendengar Tinggi Rendah Nada Adalah Pengukuran?", "id": "Domain Frekuensi (Pitch Suara)." },
  { "en": "Kamera Mengambil Gambar Di?", "id": "Domain Spasial (Mirip Domain Waktu)." },
  { "en": "Transformasi Fourier 2D Mengubah Gambar Ke?", "id": "Domain Frekuensi Spasial." },
  { "en": "Frekuensi Spasial Rendah Mewakili?", "id": "Area Gambar Yang Warnanya Halus." },
  { "en": "Frekuensi Spasial Tinggi Mewakili?", "id": "Tepi Tajam Dan Detail Gambar." },
  { "en": "Format JPEG Mengompres Gambar Dengan?", "id": "Membuang Komponen Frekuensi Spasial Tinggi." },
  { "en": "Derau Impulsif Di Waktu Terlihat Seperti?", "id": "Derau Merata Di Seluruh Spektrum Frekuensi." },
  { "en": "Nada Murni Di Waktu Terlihat Seperti?", "id": "Satu Garis Tajam Di Domain Frekuensi." },
  { "en": "Bahasa Domain Waktu Mendeskripsikan?", "id": "Evolusi Sinyal Saat Demi Saat." },
  { "en": "Bahasa Domain Frekuensi Mendeskripsikan?", "id": "Resep Atau Komposisi Intrinsik Sinyal." },
  { "en": "Respon Frekuensi Datar Artinya Respon Impulsnya?", "id": "Sebuah Impuls Yang Sangat Tajam Sempit." },
  { "en": "Respon Frekuensi Sempit Artinya Respon Impulsnya?", "id": "Menyebar Luas Dan Berosilasi Lambat." },
  { "en": "Amplitudo Puncak Sinyal Diamati Di?", "id": "Domain Waktu Menggunakan Osiloskop." },
  { "en": "Komposisi Harmonik Sinyal Diamati Di?", "id": "Domain Frekuensi Menggunakan Spectrum Analyzer." },
  { "en": "Domain Frekuensi Memecah Sinyal Menjadi?", "id": "Jumlah Dari Banyak Gelombang Sinus Sederhana." },
  { "en": "Domain Waktu Mensintesis Sinyal Dengan?", "id": "Menjumlahkan Semua Komponen Sinusoidal." },
  { "en": "Jika Ingin Tahu Frekuensi Tepat Sinyal Harus?", "id": "Berlangsung Sangat Lama Atau Tak Terhingga." },
  { "en": "Jika Ingin Tahu Waktu Tepat Sinyal Harus?", "id": "Memiliki Spektrum Sangat Lebar (Bandwidth)." },
  { "en": "Apa Itu Dualitas?", "id": "Sifat Simetris Antara Domain Waktu Frekuensi." },
  { "en": "Bentuk Sinyal Sinc Di Waktu?", "id": "Spektrumnya Berbentuk Kotak Persegi Di Frekuensi." },
  { "en": "Bentuk Sinyal Kotak Di Waktu?", "id": "Spektrumnya Berbentuk Fungsi Sinc Di Frekuensi." },
  { "en": "Bentuk Sinyal Gaussian Di Waktu?", "id": "Spektrumnya Juga Berbentuk Gaussian Di Frekuensi." },
  { "en": "Fungsi Gaussian Adalah?", "id": "Bentuk Optimal Untuk Keseimbangan Waktu-Frekuensi." },
  { "en": "Apa Itu Sumbu Waktu?", "id": "Dimensi Yang Merepresentasikan Progresi Peristiwa." },
  { "en": "Apa Itu Sumbu Frekuensi?", "id": "Dimensi Yang Merepresentasikan Spektrum Getaran." },
  { "en": "Mengubah Skala Waktu (Mempercepat)?", "id": "Melebarkan Dan Melemahkan Spektrum Frekuensi." },
  { "en": "Mengubah Skala Waktu (Memperlambat)?", "id": "Menyempitkan Dan Menguatkan Spektrum Frekuensi." },
  { "en": "Diferensiasi Di Waktu Setara Dengan?", "id": "Perkalian Dengan jω Di Domain Frekuensi." },
  { "en": "Integrasi Di Waktu Setara Dengan?", "id": "Pembagian Dengan jω Di Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sama Dengan Sinyal Pita Dasar." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sama Dengan Sinyal Lolos Pita." },
  { "en": "Operasi Apa Yang Mengubah Baseband Ke Passband?", "id": "Modulasi (Perkalian Dengan Frekuensi Pembawa)." },
  { "en": "Bagaimana Kita Melihat Dunia Sehari-hari?", "id": "Secara Dominan Dalam Domain Waktu." },
  { "en": "Bagaimana Telinga Kita Bekerja?", "id": "Seperti Spectrum Analyzer Biologis." },
  { "en": "Koklea Di Telinga Dalam?", "id": "Memisahkan Suara Menjadi Komponen Frekuensi Berbeda." },
  { "en": "Jadi Persepsi Suara Adalah Proses?", "id": "Domain Frekuensi." },
  { "en": "Mata Kita Bekerja Seperti Apa?", "id": "Sensor Domain Spasial (Mirip Waktu)." },
  { "en": "Apa Itu Warna?", "id": "Persepsi Domain Frekuensi Dari Gelombang Cahaya." },
  { "en": "Cahaya Merah Adalah Frekuensi?", "id": "Frekuensi Rendah Dalam Spektrum Tampak." },
  { "en": "Cahaya Biru Adalah Frekuensi?", "id": "Frekuensi Tinggi Dalam Spektrum Tampak." },
  { "en": "Prisma Memisahkan Cahaya Menjadi?", "id": "Spektrum Warna (Domain Frekuensi)." },
  { "en": "Sebuah Foto Adalah Representasi?", "id": "Domain Spasial." },
  { "en": "Representasi Frekuensi Foto Menunjukkan?", "id": "Tingkat Detail Dan Tekstur Gambar." },
  { "en": "Gambar Halus Punya Spektrum?", "id": "Didominasi Komponen Frekuensi Rendah." },
  { "en": "Gambar Tajam Punya Spektrum?", "id": "Memiliki Banyak Komponen Frekuensi Tinggi." },
  { "en": "Apa Itu Basis Fourier?", "id": "Kumpulan Fungsi Sinus Dan Kosinus Ortogonal." },
  { "en": "Transformasi Fourier Memproyeksikan Sinyal?", "id": "Ke Atas Setiap Anggota Basis Fourier." },
  { "en": "Hasil Proyeksi Adalah?", "id": "Koefisien Fourier (Amplitudo Dan Fasa)." },
  { "en": "Apa Itu Ortogonalitas?", "id": "Fungsi Tidak Tumpang Tindih Satu Sama Lain." },
  { "en": "Ortogonalitas Memungkinkan Kita?", "id": "Memisahkan Dan Menganalisis Setiap Komponen Frekuensi." },
  { "en": "Apa Itu Sinyal Waktu Nyata?", "id": "Sama Dengan Sinyal Real-Time." },
  { "en": "Dapatkah Kita Melihat Spektrum Secara Real-Time?", "id": "Ya Menggunakan Spectrum Analyzer Dengan FFT." },
  { "en": "FFT Menghitung DFT Dari?", "id": "Blok Sinyal Yang Terus Diperbarui." },
  { "en": "Apa Itu Laju Bingkai (Frame Rate)?", "id": "Seberapa Sering Spektrum Diperbarui Per Detik." },
  { "en": "Apa Itu Resolusi Bandwidth (RBW)?", "id": "Setting Spectrum Analyzer Untuk Resolusi Frekuensi." },
  { "en": "RBW Sempit Memberi Resolusi?", "id": "Resolusi Frekuensi Sangat Baik." },
  { "en": "Kekurangan RBW Sempit?", "id": "Membutuhkan Waktu Sapu (Sweep Time) Lebih Lama." },
  { "en": "Apa Itu Video Bandwidth (VBW)?", "id": "Filter Low-Pass Untuk Menghaluskan Tampilan Spektrum." },
  { "en": "Amplitudo Diukur Di Sumbu?", "id": "Sumbu Y Dari Grafik Domain Waktu." },
  { "en": "Fasa Diukur Di Sumbu?", "id": "Sumbu Y Dari Grafik Spektrum Fasa." },
  { "en": "Apa Itu Sinyal Pass-Band?", "id": "Nama Lain Untuk Sinyal Lolos Pita." },
  { "en": "Apa Itu Spektrum Satu Sisi?", "id": "Spektrum Yang Ditampilkan Hanya Untuk Frekuensi Positif." },
  { "en": "Spektrum Satu Sisi Untuk Sinyal?", "id": "Sinyal Real Yang Memiliki Simetri." },
  { "en": "Apa Itu Spektrum Dua Sisi?", "id": "Spektrum Ditampilkan Untuk Frekuensi Positif Negatif." },
  { "en": "Frekuensi Negatif Punya Arti Fisik?", "id": "Tidak Tapi Konstruk Matematis Yang Berguna." },
  { "en": "Frekuensi Negatif Muncul Dari?", "id": "Penggunaan Eksponensial Kompleks e^jωt." },
  { "en": "Sistem LTI Memproses Frekuensi Negatif?", "id": "Secara Simetris Dengan Frekuensi Positif." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Deskripsi Sistem Di Domain Frekuensi." },
  { "en": "Magnitudo Fungsi Transfer?", "id": "Penguatan Sistem Pada Setiap Frekuensi." },
  { "en": "Fasa Fungsi Transfer?", "id": "Pergeseran Fasa Sistem Pada Setiap Frekuensi." },
  { "en": "Apa Itu Respon Impuls?", "id": "Deskripsi Sistem Di Domain Waktu." },
  { "en": "Keduanya Adalah Pasangan Transformasi?", "id": "Ya Pasangan Transformasi Fourier." },
  { "en": "Mengetahui Respon Frekuensi Berarti?", "id": "Kita Bisa Menghitung Respon Impulsnya." },
  { "en": "Mengetahui Respon Impuls Berarti?", "id": "Kita Bisa Menghitung Respon Frekuensinya." },
  { "en": "Untuk Analisis Sistem Mana Yang Lebih Intuitif?", "id": "Respon Frekuensi Sering Lebih Intuitif." },
  { "en": "Kenapa Respon Frekuensi Intuitif?", "id": "Karena Terkait Konsep Seperti Filter Bandwidth." },
  { "en": "Apa Itu Noise Figure?", "id": "Ukuran Degradasi Sinyal Akibat Noise." },
  { "en": "Noise Figure Adalah Parameter Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Rasio Kekuatan Sinyal Terhadap Kekuatan Noise." },
  { "en": "SNR Lebih Mudah Dianalisis Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Jendela Bartlett?", "id": "Fungsi Jendela Berbentuk Segitiga." },
  { "en": "Apa Itu Jendela Blackman-Harris?", "id": "Fungsi Jendela Dengan Kinerja Sangat Baik." },
  { "en": "Pemilihan Jendela Tergantung Pada?", "id": "Aplikasi Dan Trade-off Yang Diinginkan." },
  { "en": "Apa Itu Sinyal Termodulasi?", "id": "Sinyal Yang Telah Melalui Proses Modulasi." },
  { "en": "Bentuk Gelombang Sinyal Termodulasi?", "id": "Terlihat Seperti Sinusoid Frekuensi Tinggi." },
  { "en": "Amplop Sinyal Termodulasi?", "id": "Membawa Informasi Asli Dari Sinyal Baseband." },
  { "en": "Deteksi Amplop Adalah Proses?", "id": "Domain Waktu Untuk Demodulasi Sinyal AM." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Generalisasi Transformasi Fourier Untuk Analisis Transien." },
  { "en": "Sumbu Tambahan Di Domain Laplace?", "id": "Sumbu Riil σ Untuk Faktor Redaman." },
  { "en": "Sumbu Imajiner Di Domain Laplace?", "id": "Sama Dengan Domain Frekuensi Fourier." },
  { "en": "Jadi Fourier Adalah Irisan?", "id": "Irisan Bidang Laplace Sepanjang Sumbu jω." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Di Waktu Dan Amplitudo." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit Di Waktu Dan Amplitudo." },
  { "en": "Peralatan Elektronik Modern Bekerja Dengan?", "id": "Sinyal Digital." },
  { "en": "Dunia Fisik Menghasilkan Sinyal?", "id": "Sinyal Analog." },
  { "en": "Pemrosesan Sinyal Digital (DSP)?", "id": "Memanipulasi Sinyal Di Domain Digital." },
  { "en": "DSP Melakukan Filtering Dan FFT Di?", "id": "Domain Waktu Dan Frekuensi Diskrit." },
  { "en": "Apa Itu Kuantisasi?", "id": "Pembulatan Amplitudo Sinyal Ke Level Terdekat." },
  { "en": "Kuantisasi Menimbulkan Error?", "id": "Ya Disebut Noise Kuantisasi." },
  { "en": "Noise Kuantisasi Terlihat Seperti?", "id": "Derau Putih Di Domain Frekuensi." },
  { "en": "Bit Lebih Banyak Dalam Kuantisasi?", "id": "Mengurangi Noise Kuantisasi Secara Signifikan." },
  { "en": "Kedua Domain Adalah Alat?", "id": "Matematis Untuk Memahami Dunia Fisik." },
  { "en": "Mana Yang 'Lebih Nyata'?", "id": "Domain Waktu Sesuai Dengan Pengalaman Manusia." },
  { "en": "Mana Yang 'Lebih Fundamental'?", "id": "Domain Frekuensi Mendeskripsikan Komposisi Bawaan." },
  { "en": "Hubungan Overshoot Di Waktu Dan Fasa?", "id": "Pergeseran Fasa Non-Linier Sebabkan Overshoot." },
  { "en": "Hubungan Waktu Naik Dan Bandwidth?", "id": "Waktu Naik Cepat Membutuhkan Bandwidth Lebar." },
  { "en": "Filtering Dilakukan Di Domain Mana?", "id": "Bisa Di Waktu (Digital) Frekuensi (Analog)." },
  { "en": "Efek Filtering Diamati Di Domain Mana?", "id": "Paling Mudah Diamati Di Domain Frekuensi." },
  { "en": "Di Domain Waktu, Frekuensi Bersifat?", "id": "Implisit Terkandung Dalam Bentuk Gelombang." },
  { "en": "Di Domain Frekuensi, Durasi Bersifat?", "id": "Implisit Terkait Dengan Lebar Spektrum." },
  { "en": "Kenapa Transformasi Fourier Menggunakan Integral?", "id": "Untuk Menjumlahkan Kontribusi Sinyal Setiap Saat." },
  { "en": "Kenapa Transformasi Fourier Menggunakan Eksponensial Kompleks?", "id": "Cara Ringkas Merepresentasikan Sinus Dan Kosinus." },
  { "en": "Kenapa Suara Bass Terdengar Menembus Dinding?", "id": "Panjang Gelombang Besar Sulit Diblokir." },
  { "en": "Kenapa Siulan Punya Nada Yang Jelas?", "id": "Energi Terpusat Di Satu Puncak Frekuensi." },
  { "en": "Apa Representasi Koefisien Fourier?", "id": "Amplitudo Dan Fasa Setiap Komponen Harmonik." },
  { "en": "Bagaimana Sinyal Waktu Direkonstruksi Dari Spektrum?", "id": "Menjumlahkan Semua Komponen Sinusoidnya Kembali." },
  { "en": "Proses Rekonstruksi Ini Disebut?", "id": "Transformasi Fourier Balik (Inverse Fourier Transform)." },
  { "en": "Apa Itu Basis Sinyal?", "id": "Kumpulan Sinyal Dasar Untuk Representasi." },
  { "en": "Basis Untuk Domain Frekuensi?", "id": "Fungsi Sinus Dan Kosinus (Eksponensial Kompleks)." },
  { "en": "Basis Untuk Domain Waktu?", "id": "Fungsi Impuls Dirac (Delta Functions)." },
  { "en": "Setiap Sampel Di Waktu Adalah Koefisien?", "id": "Koefisien Dari Basis Impuls Dirac." },
  { "en": "Setiap Puncak Di Frekuensi Adalah Koefisien?", "id": "Koefisien Dari Basis Sinus Dan Kosinus." },
  { "en": "Apa Itu Decimation?", "id": "Mengurangi Laju Sampling Sinyal Digital." },
  { "en": "Decimation Adalah Operasi Domain?", "id": "Domain Waktu." },
  { "en": "Efek Decimation Di Frekuensi?", "id": "Melebarkan Spektrum (Berisiko Aliasing)." },
  { "en": "Apa Itu Interpolasi Sinyal?", "id": "Meningkatkan Laju Sampling Sinyal Digital." },
  { "en": "Interpolasi Adalah Operasi Domain?", "id": "Domain Waktu (Menyisipkan Sampel Nol)." },
  { "en": "Efek Interpolasi Di Frekuensi?", "id": "Menyempitkan Spektrum Relatif Terhadap Laju Baru." },
  { "en": "Apa Itu Sinyal Real?", "id": "Sinyal Dengan Nilai Amplitudo Bilangan Riil." },
  { "en": "Spektrum Sinyal Real Selalu?", "id": "Simetris Secara Konjugat Kompleks." },
  { "en": "Artinya Apa Bagi Magnitudo?", "id": "Magnitudo Adalah Fungsi Genap (Simetris)." },
  { "en": "Artinya Apa Bagi Fasa?", "id": "Fasa Adalah Fungsi Ganjil (Anti-Simetris)." },
  { "en": "Jika Sinyal Real Dan Genap Di Waktu?", "id": "Spektrumnya Akan Real Dan Genap." },
  { "en": "Jika Sinyal Real Dan Ganjil Di Waktu?", "id": "Spektrumnya Akan Imajiner Dan Ganjil." },
  { "en": "Apa Itu Transformasi Kosinus Diskrit (DCT)?", "id": "Transformasi Terkait Fourier Menggunakan Kosinus." },
  { "en": "DCT Digunakan Dalam Kompresi?", "id": "Kompresi Gambar Seperti JPEG." },
  { "en": "DCT Bekerja Di Domain?", "id": "Domain Frekuensi Spasial." },
  { "en": "Keuntungan DCT Untuk Gambar?", "id": "Memusatkan Sebagian Besar Energi Gambar." },
  { "en": "Apa Itu Kuantisasi Spektral?", "id": "Membulatkan Nilai Koefisien Di Domain Frekuensi." },
  { "en": "Kompresi Terjadi Saat Koefisien?", "id": "Koefisien Kecil Dibulatkan Menjadi Nol." },
  { "en": "Apa Itu Efek Doppler?", "id": "Perubahan Frekuensi Akibat Gerakan Sumber." },
  { "en": "Efek Doppler Adalah Fenomena?", "id": "Domain Frekuensi Yang Teramati Di Waktu." },
  { "en": "Ambulans Mendekat Frekuensi Terdengar?", "id": "Lebih Tinggi Dari Frekuensi Aslinya." },
  { "en": "Ambulans Menjauh Frekuensi Terdengar?", "id": "Lebih Rendah Dari Frekuensi Aslinya." },
  { "en": "Apa Itu Time Reversal (Pembalikan Waktu)?", "id": "Memainkan Sinyal Secara Terbalik (f(-t))." },
  { "en": "Efek Time Reversal Di Frekuensi?", "id": "Membuat Konjugat Kompleks Dari Spektrum Asli." },
  { "en": "Apa Yang Terjadi Pada Magnitudo?", "id": "Magnitudo Spektrum Tidak Berubah Sama Sekali." },
  { "en": "Apa Yang Terjadi Pada Fasa?", "id": "Tanda Dari Fasa Spektrum Menjadi Terbalik." },
  { "en": "Apa Itu Sinyal Multirate?", "id": "Sistem Yang Menggunakan Beberapa Laju Sampling." },
  { "en": "Decimation Dan Interpolasi Adalah Inti?", "id": "Pemrosesan Sinyal Multirate." },
  { "en": "Apa Itu Bank Filter?", "id": "Kumpulan Filter Band-Pass Untuk Memisahkan Sinyal." },
  { "en": "Bank Filter Bekerja Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Contoh Penggunaan Bank Filter?", "id": "Graphic Equalizer Pada Sistem Audio." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Generalisasi Fourier Untuk Menganalisis Transien." },
  { "en": "Laplace Menambah Sumbu Apa?", "id": "Sumbu Riil (σ) Untuk Faktor Peluruhan." },
  { "en": "Domain Laplace Disebut Juga?", "id": "Bidang-s (s-Plane)." },
  { "en": "Domain Fourier Adalah Irisan?", "id": "Bidang-s Tepat Di Sumbu Imajiner (σ=0)." },
  { "en": "Analisis Stabilitas Sistem Dilakukan Di?", "id": "Domain Laplace (Melihat Posisi Pole)." },
  { "en": "Apa Itu Plot Air Terjun?", "id": "Urutan Spektrum FFT Seiring Waktu." },
  { "en": "Plot Ini Adalah Jembatan Antara?", "id": "Domain Waktu Dan Domain Frekuensi." },
  { "en": "Melihat Frekuensi Berubah Seiring Waktu Adalah?", "id": "Analisis Waktu-Frekuensi." },
  { "en": "Apa Itu Frekuensi Nyquist?", "id": "Setengah Dari Frekuensi Sampling (Fs/2)." },
  { "en": "Frekuensi Tertinggi Yang Bisa Direpresentasikan?", "id": "Adalah Frekuensi Nyquist." },
  { "en": "Frekuensi Di Atas Nyquist Akan?", "id": "Mengalami Aliasing Ke Frekuensi Lebih Rendah." },
  { "en": "Apa Itu Envelope Sinyal?", "id": "Batas Atas Amplitudo Sinyal." },
  { "en": "Envelope Adalah Fitur Domain?", "id": "Domain Waktu." },
  { "en": "Untuk Sinyal AM Envelope Membawa?", "id": "Informasi Sinyal Asli (Baseband)." },
  { "en": "Apa Itu Sinyal Periodik Kuasi?", "id": "Sinyal Yang Hampir Periodik Tapi Tidak Sempurna." },
  { "en": "Contoh Sinyal Periodik Kuasi?", "id": "Getaran Mesin Atau Sinyal Ucapan Vokal." },
  { "en": "Spektrumnya Terlihat Seperti?", "id": "Garis Harmonik Yang Sedikit Melebar." },
  { "en": "Apa Itu Jitter Fase?", "id": "Sama Dengan Fasa Noise (Phase Noise)." },
  { "en": "Osilator Ideal Di Frekuensi?", "id": "Satu Garis Impuls Sempurna." },
  { "en": "Osilator Nyata Di Frekuensi?", "id": "Memiliki 'Rok' Noise Fasa Di Sekitarnya." },
  { "en": "Noise Fasa Adalah Masalah Domain?", "id": "Domain Frekuensi Yang Berasal Dari Ketidakstabilan Waktu." },
  { "en": "Apa Itu Siklus Tugas (Duty Cycle)?", "id": "Rasio Waktu 'On' Terhadap Total Periode." },
  { "en": "Siklus Tugas Adalah Parameter?", "id": "Domain Waktu Untuk Sinyal Pulsa." },
  { "en": "Mengubah Siklus Tugas Mengubah?", "id": "Amplitudo Harmonik Di Domain Frekuensi." },
  { "en": "Gelombang Kotak 50% Punya Harmonik?", "id": "Hanya Harmonik Ganjil." },
  { "en": "Jika Siklus Tugas Bukan 50%?", "id": "Harmonik Genap Akan Mulai Muncul." },
  { "en": "Apa Itu Dispersi?", "id": "Kecepatan Gelombang Bergantung Pada Frekuensi." },
  { "en": "Dispersi Menyebabkan Distorsi Di?", "id": "Domain Waktu (Pulsa Melebar)." },
  { "en": "Contoh Dispersi?", "id": "Pemisahan Warna Oleh Prisma." },
  { "en": "Serat Optik Mengalami Dispersi?", "id": "Ya Membatasi Jarak Dan Kecepatan Data." },
  { "en": "Kompensasi Dispersi Adalah Proses?", "id": "Filter Domain Frekuensi Untuk Memperbaiki Fasa." },
  { "en": "Menganalisis Sinyal Di Waktu Baik Untuk?", "id": "Mendeteksi Anomali Sesaat (Glitches)." },
  { "en": "Menganalisis Sinyal Di Frekuensi Baik Untuk?", "id": "Mendeteksi Interferensi Periodik Tersembunyi." },
  { "en": "Apa Itu Interferensi?", "id": "Sinyal Lain Yang Mengganggu Sinyal Utama." },
  { "en": "Interferensi Mudah Dilihat Di?", "id": "Domain Frekuensi Sebagai Puncak Tak Diinginkan." },
  { "en": "Apa Itu Siklostasioner?", "id": "Sinyal Acak Yang Statistiknya Periodik." },
  { "en": "Sinyal Komunikasi Digital Sering?", "id": "Bersifat Siklostasioner Karena Laju Simbol." },
  { "en": "Apa Itu Cepstrum?", "id": "Transformasi Fourier Dari Logaritma Spektrum." },
  { "en": "Cepstrum Berguna Untuk Mendeteksi?", "id": "Tingkat Pitch Dan Gema Dalam Sinyal." },
  { "en": "Apa Itu 'Quefrency'?", "id": "Sumbu 'X' Dari Cepstrum (Unit Waktu)." },
  { "en": "Melihat Bentuk Gelombang Adalah Pandangan?", "id": "Mikroskopik Sinyal Dari Waktu Ke Waktu." },
  { "en": "Melihat Spektrum Adalah Pandangan?", "id": "Makroskopik Komposisi Rata-Rata Sinyal." },
  { "en": "Sinyal Yang Sama Dua Tampilan?", "id": "Ya Dua Sisi Dari Koin Yang Sama." },
  { "en": "Transformasi Adalah Jembatan?", "id": "Jembatan Matematis Antara Dua Pandangan Ini." },
  { "en": "Untuk Memahami Sinyal Sepenuhnya?", "id": "Kita Perlu Memahami Kedua Domain." },
  { "en": "Jika Fasa Spektrum Nol?", "id": "Sinyal Di Waktu Akan Simetris Genap." },
  { "en": "Jika Magnitudo Spektrum Datar?", "id": "Sinyal Di Waktu Adalah Impuls." },
  { "en": "Jika Magnitudo Spektrum Berbentuk Sinc?", "id": "Sinyal Di Waktu Adalah Pulsa Kotak." },
  { "en": "Hubungan Ini Adalah Inti Dari?", "id": "Sifat Dualitas Transformasi Fourier." },
  { "en": "Spektrum Yang Menyebar Di Frekuensi Artinya?", "id": "Banyak Detail Tajam Di Domain Waktu." },
  { "en": "Sinyal Yang Berosilasi Di Waktu Artinya?", "id": "Ada Puncak Energi Di Domain Frekuensi." },
  { "en": "Apa Ide Dasar Di Balik Filtering?", "id": "Membuang Atau Menyimpan Komponen Frekuensi Tertentu." },
  { "en": "Apa Ide Dasar Di Balik Sampling?", "id": "Merepresentasikan Sinyal Kontinu Dengan Sampel." },
  { "en": "Untuk Melihat Distorsi Harmonik, Domain Apa?", "id": "Domain Frekuensi Untuk Melihat Kekuatan Harmonik." },
  { "en": "Untuk Mengukur Durasi Pulsa, Domain Apa?", "id": "Domain Waktu Menggunakan Sumbu Waktu." },
  { "en": "Apa Sifat Basis Impuls Di Waktu?", "id": "Sangat Terlokalisasi Sempurna Di Satu Titik." },
  { "en": "Apa Sifat Basis Sinusoid Di Waktu?", "id": "Sangat Tersebar Di Seluruh Sumbu Waktu." },
  { "en": "Kapan Domain Frekuensi Sama Dengan Sumbu jω?", "id": "Saat Menganalisis Sinyal Steady-State (Fourier)." },
  { "en": "Domain Laplace Lebih Umum Karena?", "id": "Mencakup Analisis Transien Dengan Sumbu Riil." },
  { "en": "Kenapa Kita Bedakan Biola Dan Piano?", "id": "Karena Spektrum Amplitudo Harmoniknya Berbeda." },
  { "en": "Perbedaan Ini Adalah Fenomena Domain?", "id": "Domain Frekuensi Yang Didengar Telinga." },
  { "en": "Apa Itu 'Warna' Suara (Timbre)?", "id": "Kombinasi Unik Harmonik Di Domain Frekuensi." },
  { "en": "Apa Itu 'Nada' Suara (Pitch)?", "id": "Frekuensi Dasar (F0) Di Domain Frekuensi." },
  { "en": "Apa Itu 'Kenyaringan' Suara (Loudness)?", "id": "Amplitudo Gelombang Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Terkuantisasi?", "id": "Amplitudo Diskrit Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Tersampel?", "id": "Sinyal Diskrit Di Sepanjang Sumbu Waktu." },
  { "en": "Sinyal Digital Adalah Diskrit Di?", "id": "Kedua Domain Waktu Dan Amplitudo." },
  { "en": "Apa Efek Konvolusi Dengan Impuls?", "id": "Tidak Mengubah Sinyal Sama Sekali." },
  { "en": "Apa Efek Perkalian Dengan Konstanta?", "id": "Spektrum Juga Dikalikan Dengan Konstanta Sama." },
  { "en": "Pergeseran Waktu Mempengaruhi Magnitudo Spektrum?", "id": "Tidak Magnitudo Spektrum Tetap Sama." },
  { "en": "Pergeseran Waktu Mempengaruhi Fasa Spektrum?", "id": "Ya Menambah Pergeseran Fasa Linier." },
  { "en": "Apa Itu Respon Frekuensi Nol?", "id": "Sama Dengan Gain DC Sistem." },
  { "en": "Respon Frekuensi Nol Adalah?", "id": "Respon Sistem Terhadap Input DC." },
  { "en": "Apa Itu Average Power (Daya Rata-Rata)?", "id": "Parameter Domain Waktu Untuk Sinyal Periodik." },
  { "en": "Apa Itu Total Energy (Energi Total)?", "id": "Parameter Domain Waktu Untuk Sinyal Non-Periodik." },
  { "en": "Teorema Rayleigh Menghubungkan Energi Di?", "id": "Domain Waktu Dan Domain Frekuensi." },
  { "en": "Apa Itu Simetri Hermite?", "id": "Sama Dengan Simetri Konjugat Kompleks." },
  { "en": "Simetri Hermite Adalah Sifat?", "id": "Spektrum Sinyal Yang Bernilai Riil." },
  { "en": "Jika Sinyal Riil Maka Spektrumnya?", "id": "Pasti Memiliki Simetri Hermite." },
  { "en": "Apa Itu Transformasi Fourier Invers?", "id": "Sama Dengan Transformasi Fourier Balik." },
  { "en": "Apa Itu Sintesis Fourier?", "id": "Membangun Sinyal Waktu Dari Spektrumnya." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Memecah Sinyal Waktu Menjadi Spektrumnya." },
  { "en": "Dunia Waktu Adalah Dunia?", "id": "Sintesis Atau Gabungan." },
  { "en": "Dunia Frekuensi Adalah Dunia?", "id": "Analisis Atau Dekomposisi." },
  { "en": "Apa Itu Sinyal Loloh-Jalur?", "id": "Nama Lain Untuk Sinyal Band-Pass." },
  { "en": "Apa Itu Sinyal Jalur-Dasar?", "id": "Nama Lain Untuk Sinyal Baseband." },
  { "en": "Apa Itu Demodulasi Koheren?", "id": "Demodulasi Yang Membutuhkan Sinkronisasi Fasa." },
  { "en": "Sinkronisasi Fasa Adalah Penyelarasan Di?", "id": "Domain Waktu." },
  { "en": "Apa Itu Demodulasi Non-Koheren?", "id": "Deteksi Amplop Yang Tidak Butuh Fasa." },
  { "en": "Apa Itu Resolusi Spektral?", "id": "Sama Dengan Resolusi Frekuensi." },
  { "en": "Bagaimana Meningkatkan Resolusi Spektral?", "id": "Dengan Mengamati Sinyal Lebih Lama." },
  { "en": "Lebih Lama Di Waktu Berarti?", "id": "Resolusi Lebih Baik Di Frekuensi." },
  { "en": "Apa Itu Sinyal Multikomponen?", "id": "Sinyal Terdiri Dari Beberapa Frekuensi Berbeda." },
  { "en": "Contoh Sinyal Multikomponen?", "id": "Akord Musik (Beberapa Nada Sekaligus)." },
  { "en": "Spektrumnya Akan Menunjukkan?", "id": "Beberapa Puncak Frekuensi Yang Berbeda." },
  { "en": "Apa Itu Sinyal Monokromatik?", "id": "Sinyal Dengan Satu Warna Atau Frekuensi." },
  { "en": "Contoh Sinyal Monokromatik?", "id": "Gelombang Sinus Murni Atau Sinar Laser." },
  { "en": "Spektrumnya Akan Menunjukkan?", "id": "Satu Puncak Frekuensi Yang Sangat Tajam." },
  { "en": "Apa Itu White Light (Cahaya Putih)?", "id": "Gabungan Dari Semua Warna (Frekuensi)." },
  { "en": "Spektrum Cahaya Putih?", "id": "Spektrum Datar Dan Lebar." },
  { "en": "Ini Mirip Dengan Sinyal?", "id": "Derau Putih (White Noise)." },
  { "en": "Apa Itu Gema (Echo)?", "id": "Versi Tertunda Dari Sinyal Asli." },
  { "en": "Gema Adalah Fenomena Domain?", "id": "Domain Waktu." },
  { "en": "Efek Gema Di Frekuensi?", "id": "Menimbulkan Pola Riak (Ripple) Di Spektrum." },
  { "en": "Pola Riak Ini Disebut?", "id": "Efek Comb Filter." },
  { "en": "Apa Itu De-konvolusi?", "id": "Membatalkan Efek Konvolusi." },
  { "en": "De-konvolusi Di Frekuensi Adalah?", "id": "Pembagian Spektral." },
  { "en": "Untuk Menghilangkan Gema Kita Melakukan?", "id": "De-konvolusi Dengan Respon Impuls Gema." },
  { "en": "Apa Itu Transient?", "id": "Peristiwa Singkat Di Domain Waktu." },
  { "en": "Spektrum Sinyal Transient?", "id": "Biasanya Sangat Lebar Dan Kontinu." },
  { "en": "Apa Itu Steady-State?", "id": "Perilaku Sinyal Setelah Semua Transient Lenyap." },
  { "en": "Spektrum Sinyal Steady-State Periodik?", "id": "Terdiri Dari Garis-Garis Harmonik Diskrit." },
  { "en": "Sumbu Frekuensi Bisa Linier Atau?", "id": "Logaritmik." },
  { "en": "Skala Linier Baik Untuk?", "id": "Melihat Detail Pita Frekuensi Sempit." },
  { "en": "Skala Logaritmik Baik Untuk?", "id": "Melihat Rentang Frekuensi Sangat Lebar." },
  { "en": "Sumbu Amplitudo Bisa Linier Atau?", "id": "Logaritmik (Desibel)." },
  { "en": "Skala Desibel Baik Untuk?", "id": "Melihat Sinyal Kuat Dan Lemah Bersamaan." },
  { "en": "Sebuah Sinyal Direpresentasikan Sebagai?", "id": "Vektor Dalam Ruang Berdimensi Tinggi." },
  { "en": "Transformasi Fourier Adalah Perubahan?", "id": "Perubahan Basis Dari Basis Waktu Ke Frekuensi." },
  { "en": "Basis Waktu Adalah?", "id": "Kumpulan Impuls Dirac Yang Bergeser." },
  { "en": "Basis Frekuensi Adalah?", "id": "Kumpulan Sinusoid Dengan Frekuensi Berbeda." },
  { "en": "Kedua Basis Ini Adalah?", "id": "Basis Ortogonal." },
  { "en": "Apa Itu Sinyal Waktu-Kausal?", "id": "Sama Dengan Sinyal Kausal." },
  { "en": "Kapan Sinyal Disebut Kausal?", "id": "Bernilai Nol Untuk Waktu Negatif." },
  { "en": "Sistem Fisik Selalu?", "id": "Kausal Karena Waktu Maju." },
  { "en": "Jika Sistem Tidak Linier?", "id": "Frekuensi Output Bisa Berbeda Dari Input." },
  { "en": "Contoh Sederhana?", "id": "Dioda Penyearah (Rectifier)." },
  { "en": "Input Sinus Ke Penyearah?", "id": "Outputnya Mengandung Banyak Harmonik Genap Ganjil." },
  { "en": "Analisis Non-Linier Menggunakan?", "id": "Deret Volterra Atau Simulasi Waktu." },
  { "en": "Apa Itu Plot Fasa?", "id": "Grafik Fasa Terhadap Frekuensi." },
  { "en": "Fasa Yang 'Dibuka' (Unwrapped)?", "id": "Untuk Menghilangkan Lompatan 2π Yang Tiba-Tiba." },
  { "en": "Apa Itu Phase Delay?", "id": "Sama Dengan Penundaan Fasa." },
  { "en": "Apa Itu Group Delay?", "id": "Sama Dengan Penundaan Grup." },
  { "en": "Mana Yang Lebih Penting Untuk Bentuk Sinyal?", "id": "Group Delay Lebih Penting." },
  { "en": "Apa Itu Dispersi Kromatik?", "id": "Dispersi Akibat Indeks Bias Bergantung Frekuensi." },
  { "en": "Dispersi Ini Adalah Fenomena Domain?", "id": "Domain Frekuensi." },
  { "en": "Efeknya Di Domain Waktu?", "id": "Pulsa Cahaya Melebar Dan Terdistorsi." },
  { "en": "Sistem Komunikasi Optik Harus?", "id": "Mengkompensasi Efek Dispersi Kromatik." },
  { "en": "Apa Itu Transformasi Wavelet Diskrit (DWT)?", "id": "Implementasi Praktis Dari Transformasi Wavelet." },
  { "en": "DWT Digunakan Dalam Kompresi?", "id": "Standar JPEG 2000." },
  { "en": "Apa Itu 'Mother Wavelet'?", "id": "Bentuk Gelombang Prototipe Dalam Analisis Wavelet." },
  { "en": "Mother Wavelet Diregangkan Dan Digeser Untuk?", "id": "Menganalisis Sinyal Di Skala Waktu Berbeda." },
  { "en": "Analisis Wavelet Adalah Jembatan?", "id": "Antara Analisis Waktu Dan Frekuensi." },
  { "en": "Melihat Amplitudo Adalah Pengamatan?", "id": "Domain Waktu." },
  { "en": "Melihat Periode Adalah Pengamatan?", "id": "Domain Waktu." },
  { "en": "Melihat Harmonik Adalah Pengamatan?", "id": "Domain Frekuensi." },
  { "en": "Melihat Bandwidth Adalah Pengamatan?", "id": "Domain Frekuensi." },
  { "en": "Satu Sinyal Dua Dunia?", "id": "Ya Dua Cara Pandang Yang Berbeda." },
  { "en": "Perubahan Cepat Di Waktu Menyiratkan Apa?", "id": "Adanya Komponen Frekuensi Tinggi Yang Signifikan." },
  { "en": "Pola Berulang Di Waktu Menyiratkan Apa?", "id": "Spektrum Diskrit Terdiri Dari Garis Harmonik." },
  { "en": "Apa Peran Fasa Dalam Domain Waktu?", "id": "Menentukan Bentuk Dan Posisi Puncak Gelombang." },
  { "en": "Apa Peran Modulasi Bagi Spektrum?", "id": "Menggeser Seluruh Spektrum Ke Frekuensi Lain." },
  { "en": "Menganalisis Crosstalk Lebih Mudah Di Domain?", "id": "Domain Frekuensi Untuk Melihat Kebocoran Spektral." },
  { "en": "Menganalisis Latensi Lebih Mudah Di Domain?", "id": "Domain Waktu Untuk Mengukur Penundaan." },
  { "en": "Sampling Di Bawah Nyquist Menyebabkan Apa?", "id": "Aliasing Dimana Frekuensi Tinggi Meniru Rendah." },
  { "en": "Aliasing Menyebabkan Apa Dalam Audio?", "id": "Nada Artefak Aneh Yang Tidak Ada Aslinya." },
  { "en": "Bagaimana Mata Melihat Frekuensi Spasial Rendah?", "id": "Sebagai Area Warna Yang Luas Seragam." },
  { "en": "Bagaimana Mata Melihat Frekuensi Spasial Tinggi?", "id": "Sebagai Tepi Obyek Dan Tekstur Detail." },
  { "en": "Apa Itu Sinyal Real?", "id": "Sinyal Yang Amplitudonya Selalu Bilangan Riil." },
  { "en": "Apa Itu Sinyal Kompleks?", "id": "Sinyal Yang Amplitudonya Bisa Bilangan Kompleks." },
  { "en": "Sinyal Dunia Nyata Adalah?", "id": "Sinyal Real." },
  { "en": "Sinyal Kompleks Digunakan Untuk?", "id": "Penyederhanaan Matematis Dalam Analisis." },
  { "en": "Transformasi Fourier Balik Menggunakan?", "id": "Integral Untuk Menjumlahkan Semua Komponen Frekuensi." },
  { "en": "Jika Hanya Ada Satu Frekuensi?", "id": "Sinyal Di Waktu Adalah Sinusoid Murni." },
  { "en": "Jika Ada Banyak Frekuensi?", "id": "Bentuk Gelombang Di Waktu Menjadi Kompleks." },
  { "en": "Jika Spektrum Lebar Dan Datar?", "id": "Sinyal Di Waktu Menyerupai Derau Acak." },
  { "en": "Apa Itu Waktu Koherensi?", "id": "Durasi Waktu Dimana Sinyal Tetap Terkorelasi." },
  { "en": "Waktu Koherensi Adalah Konsep?", "id": "Domain Waktu." },
  { "en": "Apa Itu Bandwidth Koherensi?", "id": "Rentang Frekuensi Dimana Kanal Bersifat Datar." },
  { "en": "Bandwidth Koherensi Adalah Konsep?", "id": "Domain Frekuensi." },
  { "en": "Keduanya Berbanding Terbalik?", "id": "Ya Waktu Koherensi Panjang Bandwidth Sempit." },
  { "en": "Amplitudo Instan Diukur Di?", "id": "Domain Waktu." },
  { "en": "Daya Rata-rata Diukur Di?", "id": "Domain Waktu." },
  { "en": "Bandwidth 3-dB Diukur Di?", "id": "Domain Frekuensi." },
  { "en": "Frekuensi Cutoff Diukur Di?", "id": "Domain Frekuensi." },
  { "en": "Gelombang Elektromagnetik Memiliki Komponen?", "id": "Medan Listrik Dan Medan Magnet." },
  { "en": "Komponen Ini Berosilasi Di?", "id": "Domain Waktu Dan Ruang." },
  { "en": "Spektrumnya Menunjukkan Apa?", "id": "Distribusi Energi Di Berbagai Frekuensi." },
  { "en": "Apa Itu Sinyal Baseband Kompleks?", "id": "Representasi Passband Di Sekitar Frekuensi Nol." },
  { "en": "Representasi Ini Menyederhanakan?", "id": "Analisis Dan Simulasi Sistem Komunikasi." },
  { "en": "Apa Itu Komponen In-phase (I)?", "id": "Bagian Riil Dari Sinyal Baseband Kompleks." },
  { "en": "Apa Itu Komponen Quadrature (Q)?", "id": "Bagian Imajiner Sinyal Baseband Kompleks." },
  { "en": "Data I/Q Adalah Representasi?", "id": "Domain Waktu Dari Sinyal Passband." },
  { "en": "Efek Menghilangkan Fasa Spektrum?", "id": "Bentuk Gelombang Hancur Tapi Terdengar Sama." },
  { "en": "Efek Menghilangkan Magnitudo Spektrum?", "id": "Sinyal Lenyap Sama Sekali." },
  { "en": "Apa Itu Konjugat Kompleks?", "id": "Mengubah Tanda Bagian Imajiner." },
  { "en": "Konjugasi Di Domain Frekuensi Sama Dengan?", "id": "Pembalikan Waktu Dan Konjugasi Di Waktu." },
  { "en": "Sebuah Pulsa Pendek Di Waktu?", "id": "Membutuhkan Bandwidth Lebar Untuk Transmisi." },
  { "en": "Sinyal Telepon Suara Punya Bandwidth?", "id": "Sempit Karena Berubah Relatif Lambat." },
  { "en": "Sinyal Video Punya Bandwidth?", "id": "Lebar Karena Detail Gambar Berubah Cepat." },
  { "en": "Apa Itu Sinyal Orthonormal?", "id": "Kumpulan Sinyal Yang Ortogonal Dan Berenergi Satu." },
  { "en": "Basis Sinus Dan Kosinus?", "id": "Adalah Contoh Basis Sinyal Ortogonal." },
  { "en": "Apa Itu OFDM (Orthogonal FDM)?", "id": "Skema Modulasi Digital Efisiensi Tinggi." },
  { "en": "Ortogonalitas OFDM Ada Di Domain?", "id": "Domain Frekuensi Antar Sub-carrier." },
  { "en": "Efek OFDM Di Domain Waktu?", "id": "Amplitudo Sinyal Bisa Berfluktuasi Sangat Besar." },
  { "en": "Apa Itu Peak-to-Average Power Ratio (PAPR)?", "id": "Rasio Amplitudo Puncak Dan Rata-rata." },
  { "en": "PAPR Adalah Masalah Di Domain?", "id": "Domain Waktu Sinyal OFDM." },
  { "en": "Transformasi Fourier Adalah Operator?", "id": "Operator Linier." },
  { "en": "Jika Sinyal Diskala Di Waktu?", "id": "Spektrumnya Juga Diskala Terbalik Di Frekuensi." },
  { "en": "Apa Itu Spektrum Terlipat (Folded Spectrum)?", "id": "Hasil Dari Proses Aliasing." },
  { "en": "Sistem LTI Tidak Bisa?", "id": "Menghasilkan Frekuensi Yang Tidak Ada Di Input." },
  { "en": "Sistem Non-Linier Bisa?", "id": "Menciptakan Harmonik Dan Intermodulasi." },
  { "en": "Apa Itu Distorsi Intermodulasi?", "id": "Munculnya Frekuensi Penjumlahan Dan Pengurangan." },
  { "en": "Intermodulasi Adalah Masalah Domain?", "id": "Domain Frekuensi Akibat Non-Linearitas." },
  { "en": "Apa Itu Titik Intersep Orde Ketiga (IP3)?", "id": "Ukuran Kinerja Linearitas Sistem." },
  { "en": "IP3 Adalah Parameter Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Memetakan Sinyal Waktu Ke Bidang Kompleks." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Memetakan Sinyal Waktu Ke Domain Frekuensi." },
  { "en": "Kapan Keduanya Hampir Sama?", "id": "Untuk Sinyal Stabil Yang Ada Selamanya." },
  { "en": "Apa Itu Fungsi Transfer Sistem?", "id": "Deskripsi Perilaku Sistem Di Domain Frekuensi." },
  { "en": "Fungsi Transfer Didefinisikan Sebagai?", "id": "Rasio Spektrum Output Terhadap Input." },
  { "en": "Jika Input Impuls Spektrumnya?", "id": "Satu Di Semua Frekuensi." },
  { "en": "Maka Output Sistem Adalah?", "id": "Fungsi Transfer Itu Sendiri." },
  { "en": "Ini Menghubungkan Respon Impuls Dengan?", "id": "Fungsi Transfer Sistem." },
  { "en": "Apa Itu Jeda Waktu (Time Lag)?", "id": "Pergeseran Antara Dua Sinyal Di Waktu." },
  { "en": "Jeda Waktu Diukur Dengan?", "id": "Fungsi Cross-korelasi Di Domain Waktu." },
  { "en": "Apa Itu Pergeseran Fasa?", "id": "Pergeseran Jeda Waktu Di Domain Frekuensi." },
  { "en": "Sistem Fasa Linier Punya?", "id": "Jeda Waktu Konstan Untuk Semua Frekuensi." },
  { "en": "Apa Itu Sinyal Periodik?", "id": "Sinyal Yang Berulang Persis Sama." },
  { "en": "Spektrumnya Berupa?", "id": "Garis-Garis Diskrit Di Frekuensi Harmonik." },
  { "en": "Apa Itu Sinyal Aperiodik?", "id": "Sinyal Yang Tidak Berulang." },
  { "en": "Spektrumnya Berupa?", "id": "Kurva Kontinu Di Domain Frekuensi." },
  { "en": "Keterkaitan Waktu Dan Frekuensi?", "id": "Saling Terikat Melalui Transformasi Fourier." },
  { "en": "Apa Itu Jendela Analisis?", "id": "Durasi Waktu Sinyal Yang Diambil Untuk FFT." },
  { "en": "Jendela Lebih Panjang Memberi Resolusi?", "id": "Resolusi Frekuensi Lebih Baik." },
  { "en": "Jendela Lebih Pendek Memberi Resolusi?", "id": "Resolusi Waktu Lebih Baik." },
  { "en": "Ini Adalah Trade-off Praktis?", "id": "Dalam Semua Alat Analisis Spektrum." },
  { "en": "Apa Itu Desain Pulsa?", "id": "Membentuk Pulsa Di Waktu Untuk Spektrum Tertentu." },
  { "en": "Contoh Desain Pulsa?", "id": "Filter Raised-Cosine Untuk Komunikasi Digital." },
  { "en": "Tujuannya Adalah Menghilangkan?", "id": "Intersymbol Interference (ISI)." },
  { "en": "Kriteria Nyquist Untuk ISI Nol?", "id": "Kriteria Desain Di Domain Frekuensi." },
  { "en": "Apa Itu Doppler Spread?", "id": "Penyebaran Spektrum Akibat Pergerakan Cepat." },
  { "en": "Doppler Spread Adalah Fenomena Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Delay Spread?", "id": "Penyebaran Sinyal Di Waktu Akibat Multipath." },
  { "en": "Delay Spread Adalah Fenomena Domain?", "id": "Domain Waktu." },
  { "en": "Keduanya Adalah Karakteristik Dari?", "id": "Kanal Komunikasi Nirkabel." },
  { "en": "Apa Itu Waktu-Bandwidth Product?", "id": "Ukuran Kompleksitas Sinyal." },
  { "en": "Nilai Minimumnya Adalah Untuk?", "id": "Pulsa Berbentuk Gaussian." },
  { "en": "Apa Itu Sinyal Analytic?", "id": "Sama Dengan Sinyal Analitik." },
  { "en": "Apa Itu Frekuensi Instan?", "id": "Sama Dengan Frekuensi Sesaat." },
  { "en": "Frekuensi Instan Adalah Konsep?", "id": "Domain Waktu." },
  { "en": "Menghitungnya Memerlukan Informasi Dari?", "id": "Domain Frekuensi (Transformasi Hilbert)." },
  { "en": "Osiloskop Menampilkan Sinyal Vs?", "id": "Waktu." },
  { "en": "Spectrum Analyzer Menampilkan Sinyal Vs?", "id": "Frekuensi." },
  { "en": "Keduanya Adalah Alat Fundamental?", "id": "Untuk Insinyur Elektronik." },
  { "en": "Melihat Sinyal Di Kedua Domain?", "id": "Memberikan Pemahaman Yang Sangat Mendalam." },
  { "en": "Apa Beda Linearitas Dan Invariansi Waktu?", "id": "Linearitas Soal Amplitudo Invariansi Soal Waktu." },
  { "en": "Bagaimana Fourier Merepresentasikan Tepi Tajam?", "id": "Dengan Menjumlahkan Banyak Sekali Harmonik Tinggi." },
  { "en": "Apa Itu Efek Gibbs?", "id": "Overshoot Kecil Dekat Tepi Tajam." },
  { "en": "Efek Gibbs Adalah Fenomena Domain?", "id": "Domain Waktu Akibat Keterbatasan Frekuensi." },
  { "en": "Informasi Apa Yang Eksplisit Di Domain Waktu?", "id": "Amplitudo Puncak Durasi Dan Bentuk Sesaat." },
  { "en": "Informasi Apa Yang Implisit Di Domain Waktu?", "id": "Komposisi Frekuensi Dan Bandwidth Sinyal." },
  { "en": "Informasi Apa Yang Eksplisit Di Domain Frekuensi?", "id": "Komposisi Harmonik Bandwidth Dan Daya." },
  { "en": "Informasi Apa Yang Implisit Di Domain Frekuensi?", "id": "Bentuk Gelombang Tepat Dan Durasi." },
  { "en": "Melihat 'Ringing' Di Domain Waktu Menandakan?", "id": "Adanya Resonansi Atau Filter Tajam." },
  { "en": "Melihat Puncak Tajam Di Domain Frekuensi Menandakan?", "id": "Adanya Komponen Periodik Kuat Di Waktu." },
  { "en": "Dunia Peristiwa Adalah Domain?", "id": "Domain Waktu." },
  { "en": "Dunia Komposisi Adalah Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Asumsi Utama Analisis Fourier?", "id": "Sinyal Dapat Direpresentasikan Sebagai Jumlah Sinusoid." },
  { "en": "Apa Batasan Dari Melihat Sinyal Di Waktu?", "id": "Sulit Melihat Komponen Frekuensi Tersembunyi." },
  { "en": "Apa Itu Transformasi Fourier Cepat (FFT)?", "id": "Algoritma Efisien Untuk Menghitung DFT." },
  { "en": "FFT Bekerja Pada Data Domain?", "id": "Data Domain Waktu Yang Telah Disampel." },
  { "en": "Output FFT Adalah Data Domain?", "id": "Data Domain Frekuensi Diskrit." },
  { "en": "Apa Itu Panjang FFT (FFT Length)?", "id": "Jumlah Sampel Waktu Yang Diproses." },
  { "en": "Panjang FFT Mempengaruhi Resolusi?", "id": "Resolusi Frekuensi Dari Hasil Spektrum." },
  { "en": "Panjang FFT Lebih Besar Resolusi?", "id": "Resolusi Frekuensi Menjadi Lebih Baik." },
  { "en": "Apa Itu 'Bin' FFT?", "id": "Satu Titik Data Di Spektrum Frekuensi." },
  { "en": "Setiap Bin Mewakili?", "id": "Kekuatan Sinyal Di Pita Frekuensi Sempit." },
  { "en": "Perkalian Di Waktu Menjadi?", "id": "Konvolusi Di Frekuensi." },
  { "en": "Konvolusi Di Waktu Menjadi?", "id": "Perkalian Di Frekuensi." },
  { "en": "Sifat Mana Yang Lebih Mudah Secara Komputasi?", "id": "Perkalian Di Domain Frekuensi." },
  { "en": "Oleh Karena Itu Filtering Sering Dilakukan Di?", "id": "Domain Frekuensi Menggunakan FFT." },
  { "en": "Langkah Filtering Di Frekuensi?", "id": "FFT Kalikan Dengan Respon Frekuensi IFFT." },
  { "en": "Metode Ini Disebut?", "id": "Metode Overlap-Add Atau Overlap-Save." },
  { "en": "Apa Itu DC Offset?", "id": "Nilai Rata-rata Sinyal Tidak Nol." },
  { "en": "DC Offset Adalah Fitur Domain?", "id": "Domain Waktu." },
  { "en": "Bagaimana DC Offset Terlihat Di Frekuensi?", "id": "Sebagai Puncak Di Frekuensi Nol (DC Bin)." },
  { "en": "Bagaimana Menghilangkan DC Offset?", "id": "Menggunakan Filter High-Pass Atau Mengurangkan Rata-rata." },
  { "en": "Apa Itu Simetri Setengah Gelombang?", "id": "Setengah Siklus Kedua Adalah Negatif Pertama." },
  { "en": "Spektrum Sinyal Ini Hanya Mengandung?", "id": "Harmonik Ganjil Saja." },
  { "en": "Contoh Sinyal Simetri Setengah Gelombang?", "id": "Gelombang Kotak Dan Gelombang Segitiga." },
  { "en": "Apa Itu Transformasi Kosinus?", "id": "Varian Fourier Untuk Sinyal Genap." },
  { "en": "Apa Itu Transformasi Sinus?", "id": "Varian Fourier Untuk Sinyal Ganjil." },
  { "en": "Apa Itu Sistem Fasa Linier?", "id": "Menunda Semua Frekuensi Dengan Waktu Sama." },
  { "en": "Sistem Fasa Linier Tidak Mengubah?", "id": "Bentuk Gelombang Sinyal." },
  { "en": "Respon Impuls Sistem Fasa Linier?", "id": "Selalu Simetris Terhadap Pusatnya." },
  { "en": "Apa Itu Sistem Fasa Minimum?", "id": "Menghasilkan Penundaan Terkecil Yang Mungkin." },
  { "en": "Apa Itu Sistem Fasa Maksimum?", "id": "Menghasilkan Penundaan Terbesar Yang Mungkin." },
  { "en": "Apa Itu Sistem Fasa Campuran?", "id": "Kombinasi Dari Fasa Minimum Dan All-Pass." },
  { "en": "Sistem Nyata Biasanya Adalah?", "id": "Sistem Fasa Campuran." },
  { "en": "Bagaimana Kita Mendengar?", "id": "Telinga Melakukan Analisis Domain Frekuensi." },
  { "en": "Bagaimana Kita Melihat?", "id": "Mata Melakukan Analisis Domain Spasial (Waktu)." },
  { "en": "Apa Itu Audio Digital?", "id": "Representasi Suara Di Domain Waktu Diskrit." },
  { "en": "Apa Itu Laju Sampel (Sample Rate)?", "id": "Seberapa Sering Sinyal Diukur Per Detik." },
  { "en": "Laju Sampel Adalah Parameter?", "id": "Domain Waktu." },
  { "en": "Laju Sampel Menentukan Apa Di Frekuensi?", "id": "Bandwidth Maksimum Yang Bisa Direkam." },
  { "en": "Laju Sampel CD Audio?", "id": "44.1 KiloHertz." },
  { "en": "Bandwidth Terdengar Manusia?", "id": "Sekitar 20 Hertz Hingga 20 KiloHertz." },
  { "en": "Kenapa 44.1 kHz Dipilih?", "id": "Memenuhi Kriteria Nyquist (2 x 20 kHz)." },
  { "en": "Apa Itu Kedalaman Bit (Bit Depth)?", "id": "Jumlah Bit Per Sampel." },
  { "en": "Kedalaman Bit Menentukan?", "id": "Rentang Dinamis Sinyal." },
  { "en": "Rentang Dinamis Adalah Konsep?", "id": "Domain Waktu (Amplitudo)." },
  { "en": "Kebisingan Latar Belakang (Noise Floor) Terlihat Di?", "id": "Domain Frekuensi." },
  { "en": "Kedalaman Bit Lebih Tinggi?", "id": "Menurunkan Tingkat Kebisingan Latar Belakang." },
  { "en": "Apa Itu Sinyal Jam (Clock Signal)?", "id": "Sinyal Periodik Untuk Sinkronisasi." },
  { "en": "Sinyal Jam Ideal Di Waktu?", "id": "Gelombang Kotak Sempurna." },
  { "en": "Sinyal Jam Ideal Di Frekuensi?", "id": "Deretan Harmonik Ganjil." },
  { "en": "Jitter Jam Adalah Deviasi Di?", "id": "Domain Waktu." },
  { "en": "Efek Jitter Jam Di Frekuensi?", "id": "Menyebarkan Energi Harmonik (Noise Fasa)." },
  { "en": "Pergeseran Doppler Terjadi Di?", "id": "Domain Frekuensi." },
  { "en": "Penyebab Pergeseran Doppler?", "id": "Gerakan Relatif Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Termodulasi Fasa (PSK)?", "id": "Informasi Dikodekan Dalam Fasa Sinyal." },
  { "en": "PSK Adalah Modulasi Di Domain?", "id": "Domain Waktu (Mengubah Fasa)." },
  { "en": "Apa Itu Sinyal Termodulasi Amplitudo (ASK)?", "id": "Informasi Dikodekan Dalam Amplitudo Sinyal." },
  { "en": "ASK Adalah Modulasi Di Domain?", "id": "Domain Waktu (Mengubah Amplitudo)." },
  { "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?", "id": "Menggabungkan ASK Dan PSK." },
  { "en": "QAM Mengubah Apa Di Waktu?", "id": "Amplitudo Dan Fasa Sinyal Secara Bersamaan." },
  { "en": "Apa Itu Ruang Vektor?", "id": "Konsep Matematis Untuk Sinyal Dan Sistem." },
  { "en": "Sinyal Adalah Vektor Di?", "id": "Ruang Fungsi." },
  { "en": "Transformasi Fourier Adalah Perubahan?", "id": "Perubahan Basis Dalam Ruang Fungsi." },
  { "en": "Dari Basis Waktu Ke?", "id": "Basis Frekuensi." },
  { "en": "Dua Perspektif Ini Saling?", "id": "Lengkap Dan Memberi Informasi Berbeda." },
  { "en": "Apa Itu Plot Polar?", "id": "Plot Magnitudo Vs Fasa Di Koordinat Polar." },
  { "en": "Plot Polar Adalah Tampilan?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Plot Nyquist?", "id": "Plot Bagian Riil Vs Imajiner Respon Frekuensi." },
  { "en": "Plot Nyquist Adalah Tampilan?", "id": "Domain Frekuensi." },
  { "en": "Plot Nyquist Digunakan Untuk Analisis?", "id": "Stabilitas Sistem Umpan Balik." },
  { "en": "Stabilitas Tergantung Pada Lingkaran?", "id": "Di Sekitar Titik Kritis (-1,0)." },
  { "en": "Titik (-1,0) Adalah Titik?", "id": "Gain Satu Dan Pergeseran Fasa 180 Derajat." },
  { "en": "Kondisi Ini Menyebabkan Osilasi?", "id": "Ya Disebut Kriteria Stabilitas Barkhausen." },
  { "en": "Osilasi Adalah Fenomena Domain?", "id": "Domain Waktu." },
  { "en": "Analisisnya Dilakukan Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Waktu-Singkat?", "id": "Sinyal Dengan Durasi Sangat Pendek." },
  { "en": "Contoh Sinyal Waktu-Singkat?", "id": "Letusan Petir Atau Klik Mouse." },
  { "en": "Spektrum Sinyal Waktu-Singkat?", "id": "Sangat Lebar Mencakup Banyak Frekuensi." },
  { "en": "Apa Itu Sinyal Frekuensi-Singkat?", "id": "Sinyal Sangat Terlokalisasi Di Frekuensi." },
  { "en": "Contoh Sinyal Frekuensi-Singkat?", "id": "Nada Kalibrasi Atau Sinusoid Murni." },
  { "en": "Bentuk Gelombang Sinyal Frekuensi-Singkat?", "id": "Berlangsung Sangat Lama Di Domain Waktu." },
  { "en": "Apa Itu Prinsip Dualitas?", "id": "Bentuk Operasi Di Satu Domain Mirip Lainnya." },
  { "en": "Perkalian Di Waktu Dual Dengan?", "id": "Konvolusi Di Frekuensi." },
  { "en": "Sifat Dualitas Ini Adalah?", "id": "Sifat Elegan Dari Transformasi Fourier." },
  { "en": "Sinyal Halus Di Waktu Menyiratkan Spektrum?", "id": "Spektrum Yang Cepat Meluruh Ke Nol." },
  { "en": "Tepi Tajam Di Waktu Menyiratkan Spektrum?", "id": "Spektrum Yang Lambat Meluruh (Banyak Harmonik)." },
  { "en": "Parameter 'Overshoot' Diukur Di Domain?", "id": "Domain Waktu." },
  { "en": "Parameter 'Bandwidth' Diukur Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Parameter 'Rise Time' Diukur Di Domain?", "id": "Domain Waktu." },
  { "en": "Parameter 'Cutoff Frequency' Diukur Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Parameter 'Periode' Diukur Di Domain?", "id": "Domain Waktu." },
  { "en": "Parameter 'Harmonik' Diamati Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Kenapa Filter Ideal Tidak Bisa Dibuat?", "id": "Karena Memerlukan Respon Impuls Non-Kausal." },
  { "en": "Apa Artinya Respon Impuls Non-Kausal?", "id": "Sistem Harus Merespon Sebelum Diberi Input." },
  { "en": "Kenapa Aliasing Menciptakan Frekuensi 'Hantu'?", "id": "Frekuensi Tinggi Menyamar Sebagai Frekuensi Rendah." },
  { "en": "Operasi Apa Di Waktu Yang Jadi Perkalian?", "id": "Operasi Konvolusi Di Domain Waktu." },
  { "en": "Apa Yang Dilakukan Transformasi Fourier Pada Sinyal?", "id": "Memecah Sinyal Menjadi Komponen Sinusoidnya." },
  { "en": "Apa Yang Dilakukan Transformasi Fourier Balik?", "id": "Menyusun Kembali Sinyal Dari Komponen Sinusoid." },
  { "en": "Warna Pelangi Adalah Contoh Domain?", "id": "Domain Frekuensi Dari Cahaya Matahari." },
  { "en": "Detak Jam Adalah Contoh Domain?", "id": "Domain Waktu (Pulsa Periodik)." },
  { "en": "Apa Sifat Dasar Dari Waktu?", "id": "Progresi Maju Tak Terhindarkan." },
  { "en": "Apa Sifat Dasar Dari Frekuensi?", "id": "Ukuran Pengulangan Atau Siklus." },
  { "en": "Dua Domain Ini Saling?", "id": "Dual Dan Terikat Oleh Ketidakpastian." },
  { "en": "Apa Itu Sinyal Real-Valued?", "id": "Sinyal Yang Amplitudonya Selalu Bilangan Riil." },
  { "en": "Spektrum Sinyal Real-Valued?", "id": "Selalu Simetris Secara Konjugat." },
  { "en": "Apa Itu Sinyal Complex-Valued?", "id": "Sinyal Dengan Amplitudo Berupa Bilangan Kompleks." },
  { "en": "Spektrum Sinyal Complex-Valued?", "id": "Tidak Harus Simetris." },
  { "en": "Apa Itu 'Isi' Sinyal?", "id": "Informasi Yang Dibawa Oleh Sinyal." },
  { "en": "Informasi Dapat Direpresentasikan Di?", "id": "Kedua Domain Waktu Dan Frekuensi." },
  { "en": "Contoh Informasi Di Waktu?", "id": "Durasi Dan Urutan Nada Musik." },
  { "en": "Contoh Informasi Di Frekuensi?", "id": "Nada Dan Timbre Instrumen Musik." },
  { "en": "Apa Itu Windowing Artefact?", "id": "Distorsi Spektrum Akibat Pemotongan Sinyal." },
  { "en": "Artefak Ini Adalah Contoh?", "id": "Efek Domain Waktu Di Domain Frekuensi." },
  { "en": "Apa Itu Interpolasi Linear?", "id": "Menghubungkan Titik Sampel Dengan Garis Lurus." },
  { "en": "Interpolasi Adalah Proses Domain?", "id": "Domain Waktu." },
  { "en": "Efek Interpolasi Linear Di Frekuensi?", "id": "Bekerja Seperti Filter Low-Pass Sederhana." },
  { "en": "Apa Itu Decimating Filter?", "id": "Filter Low-Pass Sebelum Proses Decimation." },
  { "en": "Tujuannya Adalah Mencegah?", "id": "Aliasing Selama Proses Penurunan Sampling." },
  { "en": "Apa Itu Sistem Resonansi?", "id": "Sistem Yang Bergetar Kuat Pada Frekuensi Tertentu." },
  { "en": "Resonansi Terlihat Seperti Puncak Tajam Di?", "id": "Domain Frekuensi." },
  { "en": "Di Domain Waktu Resonansi Terlihat Seperti?", "id": "Osilasi Yang Meredam Sangat Lambat." },
  { "en": "Jembatan Runtuh Akibat Angin Adalah Contoh?", "id": "Resonansi Mekanis Yang Merusak." },
  { "en": "Menyetel Radio Adalah Proses?", "id": "Memilih Puncak Frekuensi Yang Diinginkan." },
  { "en": "Apa Itu Orthogonality?", "id": "Dua Fungsi Tidak Tumpang Tindih." },
  { "en": "Basis Sinusoid Adalah?", "id": "Kumpulan Fungsi Yang Saling Ortogonal." },
  { "en": "Sifat Ini Memungkinkan?", "id": "Pemisahan Sinyal Menjadi Komponen Unik." },
  { "en": "Apa Itu Spektrum Fasa Nol?", "id": "Sinyal Dengan Fasa Nol Di Semua Frekuensi." },
  { "en": "Sinyal Dengan Spektrum Fasa Nol?", "id": "Simetris Sempurna Di Sekitar t=0." },
  { "en": "Apa Itu Spektrum Fasa Linier?", "id": "Fasa Berubah Linier Dengan Frekuensi." },
  { "en": "Sinyal Dengan Spektrum Fasa Linier?", "id": "Bentuknya Sama Tapi Mengalami Penundaan." },
  { "en": "Apa Itu Fasa Non-Linier?", "id": "Menyebabkan Distorsi Bentuk Gelombang Di Waktu." },
  { "en": "Mana Yang Paling Merusak Kualitas Audio?", "id": "Distorsi Amplitudo Non-Linier." },
  { "en": "Mana Yang Paling Merusak Kualitas Data?", "id": "Distorsi Fasa Non-Linier." },
  { "en": "Apa Itu 'Domain Spasial'?", "id": "Analogi Domain Waktu Untuk Gambar." },
  { "en": "Sumbu X Dan Y Mewakili?", "id": "Posisi Piksel Dalam Gambar." },
  { "en": "Apa Itu 'Domain Frekuensi Spasial'?", "id": "Analogi Domain Frekuensi Untuk Gambar." },
  { "en": "Frekuensi Spasial Mewakili?", "id": "Tingkat Perubahan Atau Detail Gambar." },
  { "en": "Operasi 'Blur' Adalah Filter?", "id": "Filter Low-Pass Di Domain Frekuensi Spasial." },
  { "en": "Operasi 'Sharpen' Adalah Filter?", "id": "Filter High-Pass Di Domain Frekuensi Spasial." },
  { "en": "Analisis Getaran Menggunakan Domain?", "id": "Frekuensi Untuk Mencari Tanda Kerusakan." },
  { "en": "Analisis Seismik Menggunakan Domain?", "id": "Waktu Untuk Menentukan Lokasi Gempa." },
  { "en": "Apa Itu Sinyal Chirp?", "id": "Frekuensinya Berubah Seiring Waktu." },
  { "en": "Sinyal Chirp Adalah Non-Stasioner?", "id": "Ya Karena Spektrumnya Berubah." },
  { "en": "Apa Itu Teorema Wiener-Khinchin?", "id": "Menghubungkan Autokorelasi Dengan Spektrum Daya." },
  { "en": "Autokorelasi Adalah Operasi Domain?", "id": "Domain Waktu." },
  { "en": "Spektrum Daya Adalah Properti Domain?", "id": "Domain Frekuensi." },
  { "en": "Teorema Ini Adalah Jembatan Untuk?", "id": "Sinyal Acak Antara Dua Domain." },
  { "en": "Jika Sinyal Bergeser Waktu Autokorelasinya?", "id": "Tidak Berubah (Untuk Sinyal Stasioner)." },
  { "en": "Apa Itu Ergodisitas?", "id": "Rata-rata Waktu Sama Dengan Rata-rata Ensemble." },
  { "en": "Apa Itu Sinyal Deterministik?", "id": "Tidak Memiliki Komponen Acak." },
  { "en": "Apa Itu Sinyal Stokastik?", "id": "Memiliki Komponen Acak (Noise)." },
  { "en": "Fourier Untuk Sinyal?", "id": "Deterministik." },
  { "en": "Spektrum Daya Untuk Sinyal?", "id": "Stokastik." },
  { "en": "Apa Itu Titik Puncak Sinyal?", "id": "Nilai Maksimum Sinyal Di Domain Waktu." },
  { "en": "Apa Itu Frekuensi Puncak Sinyal?", "id": "Frekuensi Dengan Energi Terbesar Di Spektrum." },
  { "en": "Apa Itu 'Momen' Sinyal?", "id": "Parameter Statistik Bentuk Gelombang." },
  { "en": "Momen Pertama Adalah?", "id": "Nilai Rata-rata (Komponen DC)." },
  { "en": "Momen Kedua Adalah?", "id": "Variansi (Terkait Daya AC)." },
  { "en": "Semua Momen Diukur Di?", "id": "Domain Waktu." },
  { "en": "Apa Itu Transformasi Hartley?", "id": "Transformasi Riil Yang Mirip Fourier." },
  { "en": "Apa Itu Transformasi Hadamard?", "id": "Transformasi Berbasis Fungsi Kotak." },
  { "en": "Dasar Dari Semua Transformasi?", "id": "Memproyeksikan Sinyal Ke Fungsi Basis." },
  { "en": "Pemilihan Basis Tergantung Pada?", "id": "Jenis Sinyal Dan Informasi Yang Dicari." },
  { "en": "Basis Sinusoid Baik Untuk?", "id": "Sinyal Stasioner Dan Sistem LTI." },
  { "en": "Basis Wavelet Baik Untuk?", "id": "Sinyal Non-Stasioner Dengan Transien." },
  { "en": "Representasi Waktu-Frekuensi Adalah?", "id": "Kompromi Antara Dua Domain." },
  { "en": "Apa Itu Sel Resolusi?", "id": "Area Minimum Di Plot Waktu-Frekuensi." },
  { "en": "Luas Sel Resolusi?", "id": "Konstan Dan Dibatasi Prinsip Ketidakpastian." },
  { "en": "STFT Memiliki Sel Resolusi?", "id": "Berbentuk Sama Di Seluruh Plot." },
  { "en": "Wavelet Memiliki Sel Resolusi?", "id": "Berubah Bentuk (Adaptif)." },
  { "en": "Resolusi Waktu Baik Di Frekuensi?", "id": "Tinggi (Untuk Analisis Transien)." },
  { "en": "Resolusi Frekuensi Baik Di Frekuensi?", "id": "Rendah (Untuk Analisis Nada Lambat)." },
  { "en": "Apa Itu Sinyal Pita Suara?", "id": "Sinyal Audio Yang Mencakup Frekuensi Ucapan." },
  { "en": "Apa Itu Sinyal Video Komposit?", "id": "Sinyal Analog Yang Menggabungkan Kecerahan Warna." },
  { "en": "Informasi Kecerahan Ada Di Frekuensi?", "id": "Rendah." },
  { "en": "Informasi Warna Ada Di Frekuensi?", "id": "Tinggi (Sub-pembawa Warna)." },
  { "en": "Pemisahan Ini Adalah Contoh?", "id": "Frequency Division Multiplexing (FDM)." },
  { "en": "Melihat Osilogram Adalah Analisis?", "id": "Domain Waktu." },
  { "en": "Melihat Spektra Adalah Analisis?", "id": "Domain Frekuensi." },
  { "en": "Keduanya Adalah Cara Pandang?", "id": "Valid Dan Saling Melengkapi." },
  { "en": "Insinyur Yang Baik Menggunakan?", "id": "Kedua Domain Untuk Memecahkan Masalah." },
  { "en": "Apa Beda Sinyal Periodik Dan Acak?", "id": "Periodik Dapat Diprediksi Acak Tidak." },
  { "en": "Apa Beda Filter Analog Dan Digital?", "id": "Analog Kontinu Digital Bekerja Pada Sampel." },
  { "en": "Apa Arti Fasa Linier Di Domain Waktu?", "id": "Sinyal Hanya Tertunda Tanpa Distorsi Bentuk." },
  { "en": "Apa Arti Spektrum Datar Di Domain Waktu?", "id": "Sinyal Berenergi Besar Dalam Waktu Singkat." },
  { "en": "Bagaimana Menciptakan Sinyal Pita Sempit?", "id": "Dengan Membuat Sinyal Berubah Lambat Di Waktu." },
  { "en": "Bagaimana Menciptakan Sinyal Pita Lebar?", "id": "Dengan Membuat Sinyal Berubah Cepat Di Waktu." },
  { "en": "Desain Equalizer Lebih Mudah Di Domain?", "id": "Domain Frekuensi Untuk Membentuk Ulang Spektrum." },
  { "en": "Melihat Glitch Sinyal Lebih Mudah Di Domain?", "id": "Domain Waktu Untuk Melihat Peristiwa Sesaat." },
  { "en": "Transformasi Fourier Sinyal DC Adalah?", "id": "Impuls Dirac Tepat Di Frekuensi Nol." },
  { "en": "Transformasi Fourier Impuls Waktu Adalah?", "id": "Garis Datar Di Semua Frekuensi." },
  { "en": "Osiloskop Adalah Jendela Ke Dunia Domain?", "id": "Domain Waktu." },
  { "en": "FFT Adalah Jendela Ke Dunia Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu 'Domain'?", "id": "Perspektif Atau Cara Pandang Suatu Sinyal." },
  { "en": "Apa Itu 'Transformasi'?", "id": "Jembatan Matematis Antara Dua Domain." },
  { "en": "Sinyal Yang Sama Dua Representasi?", "id": "Ya Benar Sekali." },
  { "en": "Representasi Waktu Menekankan?", "id": "Urutan Dan Evolusi Peristiwa." },
  { "en": "Representasi Frekuensi Menekankan?", "id": "Komposisi Dan Sifat Getaran." },
  { "en": "Apa Itu Sinyal Real?", "id": "Dapat Diukur Langsung Di Dunia Fisik." },
  { "en": "Apa Itu Sinyal Kompleks?", "id": "Alat Matematis Untuk Menganalisis Sinyal Real." },
  { "en": "Apa Itu Amplitudo?", "id": "Kekuatan Atau Intensitas Sinyal Di Waktu." },
  { "en": "Apa Itu Magnitudo?", "id": "Kekuatan Komponen Frekuensi Di Spektrum." },
  { "en": "Apa Itu Fasa?", "id": "Informasi Penyelarasan Waktu Komponen Frekuensi." },
  { "en": "Menggeser Sinyal Di Waktu?", "id": "Merotasi Fasa Secara Linier Di Frekuensi." },
  { "en": "Mengalikan Di Waktu Dengan Sinusoid?", "id": "Menggeser Spektrum Di Domain Frekuensi." },
  { "en": "Operasi Ini Disebut?", "id": "Modulasi Atau Heterodyning." },
  { "en": "Apa Itu Sinyal Diskrit-Waktu?", "id": "Sama Dengan Sinyal Waktu Diskrit." },
  { "en": "Apa Itu Sinyal Kontinu-Waktu?", "id": "Sama Dengan Sinyal Waktu Kontinu." },
  { "en": "DFT Bekerja Pada Sinyal?", "id": "Diskrit-Waktu Dan Periodik." },
  { "en": "DTFT Bekerja Pada Sinyal?", "id": "Diskrit-Waktu Dan Non-Periodik." },
  { "en": "Transformasi Fourier Bekerja Pada Sinyal?", "id": "Kontinu-Waktu Dan Non-Periodik." },
  { "en": "Deret Fourier Bekerja Pada Sinyal?", "id": "Kontinu-Waktu Dan Periodik." },
  { "en": "Keempat Transformasi Ini Memetakan?", "id": "Dari Domain Waktu Ke Domain Frekuensi." },
  { "en": "Sinyal Dengan Sudut Tajam?", "id": "Memiliki Harmonik Yang Meluruh Perlahan." },
  { "en": "Sinyal Dengan Kurva Halus?", "id": "Memiliki Harmonik Yang Meluruh Cepat." },
  { "en": "Jadi Kehalusan Di Waktu Terkait?", "id": "Kecepatan Peluruhan Spektrum Di Frekuensi." },
  { "en": "Apa Itu Frekuensi Spasial?", "id": "Ukuran Pengulangan Pola Dalam Ruang." },
  { "en": "Domain Spasial Adalah Analogi?", "id": "Domain Waktu Untuk Gambar." },
  { "en": "Domain Frekuensi Spasial Adalah Analogi?", "id": "Domain Frekuensi Untuk Gambar." },
  { "en": "Apa Itu Sinyal Rapat (Dense)?", "id": "Sinyal Dengan Banyak Komponen Frekuensi." },
  { "en": "Apa Itu Sinyal Jarang (Sparse)?", "id": "Sinyal Dengan Sedikit Komponen Frekuensi Aktif." },
  { "en": "Kompresi Sinyal Bekerja Baik Pada?", "id": "Sinyal Yang Jarang Di Domain Transformasi." },
  { "en": "Apa Itu Domain Wavelet?", "id": "Domain Yang Menunjukkan Lokasi Waktu-Frekuensi." },
  { "en": "Wavelet Baik Untuk Sinyal?", "id": "Non-Stasioner Dengan Transien Singkat." },
  { "en": "Apa Itu Sinyal Rapat Waktu?", "id": "Sinyal Yang Aktif Hampir Sepanjang Waktu." },
  { "en": "Apa Itu Sinyal Jarang Waktu?", "id": "Sinyal Yang Aktif Hanya Sesekali." },
  { "en": "Contoh Sinyal Jarang Waktu?", "id": "Sinyal Radar Atau Neuron Yang Menembak." },
  { "en": "Analogi Sederhana?", "id": "Melihat Hutan Dan Melihat Pohonnya." },
  { "en": "Melihat Hutan Adalah Domain?", "id": "Frekuensi (Gambaran Umum Komposisi)." },
  { "en": "Melihat Pohonnya Adalah Domain?", "id": "Waktu (Detail Peristiwa Individual)." },
  { "en": "Apa Itu Respon Impuls Sistem LTI?", "id": "Karakteristik Lengkap Sistem Di Domain Waktu." },
  { "en": "Apa Itu Respon Frekuensi Sistem LTI?", "id": "Karakteristik Lengkap Sistem Di Domain Frekuensi." },
  { "en": "Keduanya Adalah Pasangan Transformasi?", "id": "Ya Keduanya Mengandung Informasi Yang Sama." },
  { "en": "Memilih Domain Adalah Soal?", "id": "Kenyamanan Dan Intuisi Untuk Masalah Tertentu." },
  { "en": "Apa Itu Resolusi?", "id": "Kemampuan Untuk Membedakan Detail." },
  { "en": "Resolusi Waktu Diukur Dalam?", "id": "Detik Atau Milidetik." },
  { "en": "Resolusi Frekuensi Diukur Dalam?", "id": "Hertz Atau KiloHertz." },
  { "en": "Sinyal Terdistorsi Terlihat Jelas Di?", "id": "Domain Waktu (Bentuk Berubah)." },
  { "en": "Penyebab Distorsi Dilihat Di?", "id": "Domain Frekuensi (Spektrum Tidak Datar)." },
  { "en": "Apa Itu Efek Multipath?", "id": "Sinyal Tiba Melalui Beberapa Jalur Berbeda." },
  { "en": "Multipath Adalah Fenomena Domain?", "id": "Domain Waktu (Gema Dan Penundaan)." },
  { "en": "Efek Multipath Di Frekuensi?", "id": "Menyebabkan Pola Comb Filter Di Spektrum." },
  { "en": "Apa Itu Fading Selektif Frekuensi?", "id": "Beberapa Frekuensi Melemah Lebih Dari Lainnya." },
  { "en": "Ini Disebabkan Oleh?", "id": "Efek Multipath." },
  { "en": "Apa Itu Waktu Tunda Propagasi?", "id": "Waktu Sinyal Merambat Dari A Ke B." },
  { "en": "Ini Adalah Parameter Murni Domain?", "id": "Domain Waktu." },
  { "en": "Efeknya Di Frekuensi Adalah?", "id": "Pergeseran Fasa Linier." },
  { "en": "Apa Itu Spektrometer Optik?", "id": "Alat Yang Bekerja Seperti Spectrum Analyzer." },
  { "en": "Tapi Untuk Sinyal Apa?", "id": "Sinyal Cahaya." },
  { "en": "Outputnya Adalah Spektrum?", "id": "Spektrum Cahaya Terhadap Panjang Gelombang." },
  { "en": "Panjang Gelombang Dan Frekuensi?", "id": "Berbanding Terbalik Satu Sama Lain." },
  { "en": "Sistem Stabil Tidak Punya?", "id": "Pole Di Setengah Kanan Bidang Laplace." },
  { "en": "Apa Itu Sistem Pass-All?", "id": "Sistem Yang Melewatkan Semua Frekuensi." },
  { "en": "Magnitudo Respon Frekuensinya?", "id": "Konstan Sama Dengan Satu." },
  { "en": "Sistem Ini Hanya Mempengaruhi?", "id": "Fasa Sinyal." },
  { "en": "Penundaan Murni Adalah Contoh?", "id": "Sistem Pass-All." },
  { "en": "Apa Itu Sinyal Deterministik Periodik?", "id": "Dianalisis Dengan Deret Fourier." },
  { "en": "Apa Itu Sinyal Deterministik Aperiodik?", "id": "Dianalisis Dengan Transformasi Fourier." },
  { "en": "Apa Itu Sinyal Acak Stasioner?", "id": "Dianalisis Dengan Kepadatan Spektral Daya." },
  { "en": "Apa Itu Sinyal Acak Non-Stasioner?", "id": "Dianalisis Dengan Spektogram Atau Wavelet." },
  { "en": "Semua Adalah Alat Untuk?", "id": "Melihat Sisi Frekuensi Dari Sinyal." },
  { "en": "Apa Itu Proses Stokastik?", "id": "Sama Dengan Sinyal Atau Proses Acak." },
  { "en": "Apa Itu Mean (Rata-rata)?", "id": "Nilai DC Sinyal Di Domain Waktu." },
  { "en": "Apa Itu Variansi?", "id": "Daya AC Sinyal Di Domain Waktu." },
  { "en": "Apa Itu Distribusi Probabilitas?", "id": "Deskripsi Amplitudo Sinyal Di Domain Waktu." },
  { "en": "Deskripsi Sinyal Acak Di Frekuensi?", "id": "Adalah Kepadatan Spektral Dayanya." },
  { "en": "Apa Itu Proses Gaussian?", "id": "Sinyal Acak Dengan Distribusi Normal." },
  { "en": "Spektrum Proses Gaussian Putih?", "id": "Spektrum Datar Di Semua Frekuensi." },
  { "en": "Ini Adalah Model Matematis?", "id": "Untuk Derau Termal Di Komponen Elektronik." },
  { "en": "Jika Sinyal Bergeser Di Waktu?", "id": "Energinya Tidak Berubah." },
  { "en": "Ini Disebut Sifat?", "id": "Invariansi Pergeseran Energi." },
  { "en": "Apa Itu Jendela Rectangular?", "id": "Sama Dengan Fungsi Jendela Persegi." },
  { "en": "Bentuk Spektrum Jendela Rectangular?", "id": "Berbentuk Fungsi Sinc." },
  { "en": "Fungsi Sinc Memiliki Lobe?", "id": "Satu Lobe Utama Dan Banyak Sidelobe." },
  { "en": "Sidelobe Menyebabkan Masalah Apa?", "id": "Bocoran Spektral (Spectral Leakage)." },
  { "en": "Jendela Lainnya Didesain Untuk?", "id": "Mengurangi Amplitudo Dari Sidelobe." },
  { "en": "Dengan Konsekuensi Apa?", "id": "Lobe Utama Menjadi Sedikit Lebih Lebar." },
  { "en": "Ini Adalah Trade-off Desain?", "id": "Antara Resolusi Dan Kebocoran." },
  { "en": "Domain Waktu Berguna Untuk Menjawab?", "id": "'Kapan' Dan 'Seberapa Besar'." },
  { "en": "Domain Frekuensi Berguna Untuk Menjawab?", "id": "'Apa Saja' Dan 'Seberapa Banyak'." },
  { "en": "Jika Sinyal Waktu Diskrit, Spektrumnya?", "id": "Selalu Periodik Di Domain Frekuensi." },
  { "en": "Jika Sinyal Waktu Periodik, Spektrumnya?", "id": "Selalu Diskrit Di Domain Frekuensi." },
  { "en": "Apa Esensi Dari Proses Modulasi?", "id": "Menumpangkan Informasi Pada Gelombang Frekuensi Tinggi." },
  { "en": "Apa Esensi Dari Proses Sampling?", "id": "Merepresentasikan Sinyal Kontinu Dengan Sampel Diskrit." },
  { "en": "Properti 'Kausalitas' Didefinisikan Di Domain?", "id": "Domain Waktu (Akibat Tidak Mendahului Sebab)." },
  { "en": "Properti 'Bandwidth' Didefinisikan Di Domain?", "id": "Domain Frekuensi (Rentang Frekuensi Sinyal)." },
  { "en": "Filter FIR Diimplementasikan Di Domain?", "id": "Domain Waktu (Sebagai Operasi Konvolusi)." },
  { "en": "Desain Filter FIR Dilakukan Di?", "id": "Domain Frekuensi (Menentukan Respon Frekuensi)." },
  { "en": "Getaran Jembatan Adalah Fenomena Domain?", "id": "Domain Waktu (Gerakan Fisik Yang Teramati)." },
  { "en": "Frekuensi Resonansi Jembatan Adalah Properti Domain?", "id": "Domain Frekuensi (Sifat Bawaan Struktur)." },
  { "en": "Apa Beda Tujuan Filtering Dan Modulasi?", "id": "Filtering Hapus Modulasi Geser Frekuensi." },
  { "en": "Apa Beda Sampling Dan Kuantisasi?", "id": "Sampling Mendiskritkan Waktu Kuantisasi Amplitudo." },
  { "en": "Sinyal Dunia Nyata Selalu Real?", "id": "Ya Amplitudo Yang Diukur Selalu Riil." },
  { "en": "Fasa Adalah Properti Relatif?", "id": "Ya Antara Komponen Frekuensi Berbeda." },
  { "en": "Apa Itu 'Waktu' Dalam Konteks Sinyal?", "id": "Variabel Independen Yang Terus Berjalan." },
  { "en": "Apa Itu 'Frekuensi' Dalam Konteks Sinyal?", "id": "Ukuran Pengulangan Per Satuan Waktu." },
  { "en": "Sinyal Tak Terhingga Di Waktu?", "id": "Spektrumnya Bisa Sangat Sempit (Impuls)." },
  { "en": "Sinyal Tak Terhingga Di Frekuensi?", "id": "Bentuknya Impuls Di Domain Waktu." },
  { "en": "Apa Itu Kebocoran Spektral (Spectral Leakage)?", "id": "Energi Bocor Ke Bin Frekuensi Tetangga." },
  { "en": "Penyebab Kebocoran Spektral?", "id": "Pemotongan Sinyal Tiba-tiba Di Domain Waktu." },
  { "en": "Fungsi Jendela Mengurangi Kebocoran Dengan?", "id": "Membuat Pemotongan Sinyal Lebih Halus." },
  { "en": "Apa Itu Sinyal Multirate?", "id": "Sistem Yang Menggunakan Lebih Dari Satu Sample Rate." },
  { "en": "Perubahan Sample Rate Terjadi Di?", "id": "Domain Waktu (Decimation Interpolasi)." },
  { "en": "Apa Itu Sinyal Stasioner Orde Lebar?", "id": "Sinyal Acak Dengan Mean Konstan Autokorelasi." },
  { "en": "Sifat Ini Didefinisikan Di?", "id": "Domain Waktu." },
  { "en": "Spektrum Sinyal Seperti Ini?", "id": "Dapat Didefinisikan Dengan Baik." },
  { "en": "Bagaimana Dua Domain Ini Digunakan Bersama?", "id": "Sebagai Alat Analisis Yang Saling Melengkapi." },
  { "en": "Masalah Di Waktu Bisa Jadi?", "id": "Solusi Di Frekuensi." },
  { "en": "Masalah Di Frekuensi Bisa Jadi?", "id": "Solusi Di Waktu." },
  { "en": "Contoh Masalah Waktu Solusi Frekuensi?", "id": "Menghilangkan Noise Menggunakan Filter." },
  { "en": "Contoh Masalah Frekuensi Solusi Waktu?", "id": "Mengurangi PAPR OFDM Dengan Windowing." },
  { "en": "Apa Itu Transformasi Fourier Invers Cepat?", "id": "IFFT (Inverse Fast Fourier Transform)." },
  { "en": "IFFT Mengubah Apa?", "id": "Dari Domain Frekuensi Ke Domain Waktu." },
  { "en": "FFT Dan IFFT Adalah Pasangan?", "id": "Operasi Yang Saling Berkebalikan." },
  { "en": "Apa Itu Sinyal Deterministik?", "id": "Tidak Ada Keacakan Bentuknya Pasti." },
  { "en": "Spektrumnya Juga Deterministik?", "id": "Ya Bisa Dihitung Secara Tepat." },
  { "en": "Apa Itu Sinyal Acak?", "id": "Memiliki Keacakan Tidak Bisa Diprediksi." },
  { "en": "Spektrumnya Dideskripsikan Secara?", "id": "Statistik (Kepadatan Spektral Daya)." },
  { "en": "Apa Itu Resonator?", "id": "Sistem Yang Bergetar Pada Frekuensi Tertentu." },
  { "en": "Resonator Adalah Filter?", "id": "Filter Band-Pass Yang Sangat Sempit." },
  { "en": "Apa Itu Faktor-Q?", "id": "Ukuran Kualitas Sebuah Resonator." },
  { "en": "Faktor-Q Tinggi Berarti Di Frekuensi?", "id": "Puncak Resonansi Sangat Tajam Dan Sempit." },
  { "en": "Faktor-Q Tinggi Berarti Di Waktu?", "id": "Osilasi Meredam Dengan Sangat Lambat." },
  { "en": "Apa Itu Redaman (Damping)?", "id": "Proses Penghilangan Energi Dari Osilasi." },
  { "en": "Redaman Adalah Fenomena Domain?", "id": "Domain Waktu." },
  { "en": "Efek Redaman Di Frekuensi?", "id": "Melebarkan Puncak Resonansi." },
  { "en": "Redaman Lebih Besar Berarti?", "id": "Puncak Resonansi Lebih Lebar Faktor-Q Rendah." },
  { "en": "Apa Itu Teorema Dualitas?", "id": "Bentuk Operasi Di Satu Domain Sama Lainnya." },
  { "en": "Contoh Teorema Dualitas?", "id": "Sinc Di Waktu Jadi Kotak Di Frekuensi." },
  { "en": "Apa Itu Time-Bandwidth Product?", "id": "Ukuran Yang Menghubungkan Durasi Dan Bandwidth." },
  { "en": "Nilainya Selalu Terbatas?", "id": "Ya Tidak Bisa Nol (Prinsip Ketidakpastian)." },
  { "en": "Sinyal Gaussian Meminimalkan?", "id": "Nilai Time-Bandwidth Product." },
  { "en": "Apa Itu Catu Daya Switching?", "id": "Menggunakan Saklar Cepat Untuk Konversi Tegangan." },
  { "en": "Proses Switching Terjadi Di?", "id": "Domain Waktu." },
  { "en": "Proses Ini Menghasilkan Apa Di Frekuensi?", "id": "Harmonik Frekuensi Tinggi Yang Tidak Diinginkan." },
  { "en": "Harmonik Ini Perlu Diapakan?", "id": "Dihilangkan Menggunakan Filter Low-Pass." },
  { "en": "Apa Itu Spektroskopi?", "id": "Ilmu Mempelajari Interaksi Materi Dan Radiasi." },
  { "en": "Spektroskopi Adalah Analisis Di?", "id": "Domain Frekuensi (Spektrum Cahaya)." },
  { "en": "Setiap Atom Punya Spektrum?", "id": "Spektrum Emisi Dan Absorpsi Yang Unik." },
  { "en": "Spektrum Ini Seperti Sidik Jari?", "id": "Ya Untuk Mengidentifikasi Komposisi Bintang." },
  { "en": "Denyut Nadi Diamati Di?", "id": "Domain Waktu." },
  { "en": "Harmonik Denyut Jantung (HRV) Dianalisis Di?", "id": "Domain Frekuensi." },
  { "en": "Analisis HRV Berguna Untuk?", "id": "Menilai Kesehatan Sistem Saraf Otonom." },
  { "en": "Apa Itu Sinyal Pita Lebar Ultra (UWB)?", "id": "Sinyal Dengan Bandwidth Sangat Ekstrem." },
  { "en": "Bentuk Sinyal UWB Di Waktu?", "id": "Pulsa-Pulsa Sangat Pendek Dan Sempit." },
  { "en": "Apa Itu Transformasi Fourier Fraksional?", "id": "Generalisasi Transformasi Fourier." },
  { "en": "Transformasi Ini Memetakan Sinyal Ke?", "id": "Domain Antara Waktu Dan Frekuensi." },
  { "en": "Apa Itu Domain spasial-temporal?", "id": "Domain Dengan Dimensi Ruang Dan Waktu." },
  { "en": "Contoh Sinyal Spasial-Temporal?", "id": "Video (Urutan Gambar Seiring Waktu)." },
  { "en": "Analisanya Menggunakan?", "id": "Transformasi Fourier Tiga Dimensi." },
  { "en": "Apa Itu Phase Vocoder?", "id": "Algoritma Untuk Mengubah Pitch Tempo Audio." },
  { "en": "Phase Vocoder Bekerja Di?", "id": "Domain Frekuensi Menggunakan STFT." },
  { "en": "Mengubah Pitch Berarti?", "id": "Menggeser Spektrum Di Sumbu Frekuensi." },
  { "en": "Mengubah Tempo Berarti?", "id": "Memanipulasi Fasa Di Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Sifatnya Tidak Berubah Seiring Waktu." },
  { "en": "Apa Itu Sinyal Non-Stasioner?", "id": "Sifatnya Berubah Seiring Waktu." },
  { "en": "Musik Adalah Sinyal?", "id": "Non-Stasioner." },
  { "en": "Derau Kipas Angin Adalah Sinyal?", "id": "Stasioner." },
  { "en": "Fourier Klasik Untuk Sinyal?", "id": "Stasioner." },
  { "en": "Analisis Waktu-Frekuensi Untuk Sinyal?", "id": "Non-Stasioner." },
  { "en": "Apa Itu Gelombang Gravitasi?", "id": "Riak Dalam Ruang-Waktu." },
  { "en": "Bentuk Gelombang Terdeteksi Adalah Sinyal?", "id": "Domain Waktu (Sinyal Chirp)." },
  { "en": "Analisisnya Dilakukan Di?", "id": "Kedua Domain Untuk Konfirmasi." },
  { "en": "Pola Di Waktu Mencocokkan?", "id": "Prediksi Teori Relativitas Umum." },
  { "en": "Spektrumnya Menunjukkan Sinyal?", "id": "Chirp Dengan Frekuensi Yang Meningkat." },
  { "en": "Apa Itu Sinyal Elektromiografi (EMG)?", "id": "Sinyal Listrik Dari Aktivitas Otot." },
  { "en": "Analisis EMG Di Frekuensi?", "id": "Bisa Menunjukkan Kelelahan Otot." },
  { "en": "Saat Otot Lelah Spektrumnya?", "id": "Bergeser Ke Arah Frekuensi Rendah." },
  { "en": "Apa Itu Sinyal Seismik?", "id": "Getaran Tanah Akibat Gempa Atau Ledakan." },
  { "en": "Gelombang-P Dan Gelombang-S Tiba Di?", "id": "Waktu Berbeda (Analisis Domain Waktu)." },
  { "en": "Komposisi Tanah Mempengaruhi?", "id": "Spektrum Frekuensi Dari Sinyal Seismik." },
  { "en": "Apa Itu Vektor Sinyal?", "id": "Representasi Sinyal Diskrit Sebagai Vektor Angka." },
  { "en": "DFT Adalah Operasi Matriks?", "id": "Ya Perkalian Matriks Dengan Vektor Sinyal." },
  { "en": "Matriks DFT Adalah?", "id": "Matriks Yang Kolomnya Adalah Basis Sinusoid." },
  { "en": "Operasi Ini Adalah Perubahan?", "id": "Perubahan Basis Dari Basis Waktu Ke Frekuensi." },
  { "en": "Melihat Bentuk Gelombang Adalah?", "id": "Melihat Data Dalam Basis Waktu." },
  { "en": "Melihat Spektrum Adalah?", "id": "Melihat Data Dalam Basis Frekuensi." },
  { "en": "Apa 'Kata Benda' Di Domain Waktu?", "id": "Peristiwa Sesaat Atau Nilai Sampel." },
  { "en": "Apa 'Kata Sifat' Di Domain Frekuensi?", "id": "Komposisi Harmonik Yang Menggambarkan Sinyal." },
  { "en": "Informasi Apa Yang Sama Di Kedua Domain?", "id": "Energi Total Atau Daya Rata-rata Sinyal." },
  { "en": "Mengasumsikan Sinyal Periodik Saat Tidak, Menyebabkan?", "id": "Bocoran Spektral Di Domain Frekuensi." },
  { "en": "FFT Adalah Alat Atau Konsep?", "id": "Alat Komputasi Cepat Untuk Menganalisis Spektrum." },
  { "en": "Spektrum Adalah Alat Atau Konsep?", "id": "Konsep Representasi Sinyal Di Domain Frekuensi." },
  { "en": "Dualitas Dari Sinyal Gaussian Adalah?", "id": "Sinyal Gaussian Lain Di Domain Frekuensi." },
  { "en": "Dualitas Dari Proses Diferensiasi Adalah?", "id": "Perkalian Dengan Ramp Di Domain Frekuensi." },
  { "en": "Apa Beda Jendela Waktu Dan Durasi Sinyal?", "id": "Jendela Adalah Pengamatan Durasi Sifat Sinyal." },
  { "en": "Apa Beda Bandwidth Sinyal Dan Kanal?", "id": "Bandwidth Sinyal Kebutuhan Kanal Adalah Kapasitas." },
  { "en": "Sinyal Digital Di Waktu Adalah?", "id": "Urutan Angka-Angka (Sampel)." },
  { "en": "Sinyal Digital Di Frekuensi Adalah?", "id": "Urutan Bilangan Kompleks (Koefisien DFT)." },
  { "en": "Panjang Urutan Ini Sama?", "id": "Ya Jumlah Sampel Waktu Frekuensi Sama." },
  { "en": "Apa Itu Over-sampling?", "id": "Sampling Jauh Di Atas Laju Nyquist." },
  { "en": "Keuntungan Over-sampling Di Waktu?", "id": "Memberi Lebih Banyak Sampel Untuk Diproses." },
  { "en": "Efek Over-sampling Di Frekuensi?", "id": "Meninggalkan Ruang Kosong Di Antara Spektrum." },
  { "en": "Ruang Kosong Ini Memudahkan?", "id": "Desain Filter Anti-Aliasing Yang Lebih Praktis." },
  { "en": "Apa Itu Sinyal Real-Time?", "id": "Sinyal Yang Harus Diproses Tanpa Penundaan." },
  { "en": "Analisis Real-Time Terbatas Oleh?", "id": "Kecepatan Komputasi (Misalnya FFT)." },
  { "en": "Apa Itu Laten?", "id": "Waktu Tunda Total Dalam Sistem." },
  { "en": "Latensi Adalah Ukuran Domain?", "id": "Domain Waktu." },
  { "en": "Apa Itu Sinyal Pita Tunggal (Single Sideband)?", "id": "Versi Modulasi AM Yang Lebih Efisien." },
  { "en": "Bagaimana Cara Kerjanya?", "id": "Membuang Salah Satu Pita Sisi Redundan." },
  { "en": "Operasi Ini Dilakukan Di Domain?", "id": "Domain Frekuensi Menggunakan Filter Tajam." },
  { "en": "Hasilnya Di Waktu Adalah Sinyal?", "id": "Sinyal Kompleks Yang Sulit Divisualisasikan." },
  { "en": "Apa Itu Sinyal Terkorelasi?", "id": "Dua Sinyal Yang Memiliki Kemiripan Pola." },
  { "en": "Apa Itu Sinyal Tak Terkorelasi?", "id": "Dua Sinyal Yang Tidak Memiliki Hubungan." },
  { "en": "Derau Putih Adalah Tak Terkorelasi?", "id": "Ya Dengan Versi Dirinya Yang Digeser." },
  { "en": "Kecuali Untuk Pergeseran Apa?", "id": "Pergeseran Waktu Nol (Tentu Saja)." },
  { "en": "Ini Membuat Fungsi Autokorelasinya?", "id": "Sebuah Impuls Di Domain Waktu." },
  { "en": "Spektrum Dayanya Sesuai Teorema?", "id": "Spektrum Datar Di Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Pita Terbatas Waktu?", "id": "Sinyal Yang Durasinya Benar-Benar Terbatas." },
  { "en": "Spektrumnya Akan Berlangsung?", "id": "Selamanya (Tidak Pernah Benar-Benar Nol)." },
  { "en": "Apa Itu Sinyal Pita Terbatas Frekuensi?", "id": "Sinyal Yang Spektrumnya Benar-Benar Terbatas." },
  { "en": "Bentuk Gelombangnya Akan Berlangsung?", "id": "Selamanya Di Domain Waktu." },
  { "en": "Sinyal Bisa Terbatas Di Kedua Domain?", "id": "Tidak Ini Adalah Konsekuensi Prinsip Ketidakpastian." },
  { "en": "Apa Itu Resampling?", "id": "Mengubah Laju Sampling Sinyal Digital." },
  { "en": "Resampling Adalah Proses Domain?", "id": "Domain Waktu." },
  { "en": "Ini Melibatkan Kombinasi?", "id": "Interpolasi Dan Decimation." },
  { "en": "Apa Itu Konvolusi Sirkular?", "id": "Konvolusi Yang Terjadi Pada Sinyal Periodik." },
  { "en": "Perkalian DFT Menghasilkan?", "id": "Konvolusi Sirkular Di Domain Waktu." },
  { "en": "Bukan Konvolusi Linier?", "id": "Bukan Untuk Mendapat Linier Perlu Zero Padding." },
  { "en": "Apa Itu Efek Tepi?", "id": "Error Yang Terjadi Di Tepi Blok Data." },
  { "en": "Efek Tepi Adalah Masalah Domain?", "id": "Domain Waktu Dalam Pemrosesan Blok." },
  { "en": "Metode Overlap-Add/Save Mengatasi?", "id": "Masalah Efek Tepi Ini." },
  { "en": "Apa Itu Sistem Orde Pecahan?", "id": "Sistem Dengan Turunan Orde Non-Integer." },
  { "en": "Respon Frekuensinya Bisa?", "id": "Memiliki Kemiringan Non-Integer Di Plot Bode." },
  { "en": "Apa Itu Derau 1/f?", "id": "Sama Dengan Derau Merah Jambu (Pink Noise)." },
  { "en": "Kenapa Disebut Derau 1/f?", "id": "Karena Spektrum Dayanya Sebanding 1/f." },
  { "en": "Derau Ini Terdengar Alami?", "id": "Ya Telinga Manusia Menganggapnya Lebih Alami." },
  { "en": "Apa Itu Derau Coklat (Brownian)?", "id": "Spektrum Daya Sebanding 1/f Kuadrat." },
  { "en": "Terdengar Seperti Apa?", "id": "Gemuruh Rendah Seperti Air Terjun." },
  { "en": "Derau Biru (Blue Noise)?", "id": "Spektrum Daya Sebanding Dengan f." },
  { "en": "Derau Ungu (Violet Noise)?", "id": "Spektrum Daya Sebanding f Kuadrat." },
  { "en": "Semua 'Warna' Derau Ini?", "id": "Didefinisikan Oleh Bentuk Spektrumnya." },
  { "en": "Bentuknya Dilihat Di Domain?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Tegangan?", "id": "Sinyal Dimana Amplitudo Adalah Volt." },
  { "en": "Apa Itu Sinyal Arus?", "id": "Sinyal Dimana Amplitudo Adalah Amper." },
  { "en": "Apa Itu Sinyal Daya?", "id": "Daya Sesaat Di Domain Waktu." },
  { "en": "Apa Itu Sinyal Energi?", "id": "Energi Kumulatif Di Domain Waktu." },
  { "en": "Tampilan Osiloskop Adalah?", "id": "Tegangan Terhadap Waktu." },
  { "en": "Tampilan Spectrum Analyzer Adalah?", "id": "Daya Terhadap Frekuensi." },
  { "en": "Apa Itu Transformasi Laplace Bilateral?", "id": "Versi Laplace Yang Bekerja Untuk Sinyal Non-Kausal." },
  { "en": "Ini Analog Dengan?", "id": "Transformasi Fourier Biasa." },
  { "en": "Apa Itu Transformasi Laplace Unilateral?", "id": "Versi Laplace Untuk Sinyal Kausal." },
  { "en": "Ini Berguna Untuk Menyelesaikan?", "id": "Masalah Nilai Awal (Persamaan Diferensial)." },
  { "en": "Apa Itu Teorema Wiener-Plancherel?", "id": "Nama Lain Untuk Teorema Energi Parseval." },
  { "en": "Apa Itu Proses Linear?", "id": "Proses Yang Bisa Dideskripsikan Oleh Sistem LTI." },
  { "en": "Apa Itu Proses Non-Linear?", "id": "Proses Yang Menghasilkan Distorsi Harmonik." },
  { "en": "Kliping (Clipping) Adalah Proses?", "id": "Non-Linear Ekstrem." },
  { "en": "Kliping Di Waktu Menyebabkan?", "id": "Banyak Sekali Harmonik Di Domain Frekuensi." },
  { "en": "Apa Itu Sinyal pita-terbatas (Band-limited)?", "id": "Sinyal Tanpa Energi Di Atas Frekuensi Tertentu." },
  { "en": "Apa Itu Sinyal waktu-terbatas (Time-limited)?", "id": "Sinyal Dengan Durasi Terbatas." },
  { "en": "Keduanya Tidak Bisa Terjadi Bersamaan?", "id": "Benar Menurut Prinsip Ketidakpastian." },
  { "en": "Sinyal Nyata Selalu Terbatas?", "id": "Terbatas Waktu Tapi Tidak Terbatas Pita." },
  { "en": "Namun Energinya Di Frekuensi Tinggi?", "id": "Bisa Diabaikan Sehingga Dianggap Terbatas." },
  { "en": "Asumsi Ini Adalah Dasar?", "id": "Teorema Sampling Nyquist." },
  { "en": "Dunia Waktu Itu Lokal?", "id": "Ya Setiap Titik Waktu Adalah Peristiwa Lokal." },
  { "en": "Dunia Frekuensi Itu Global?", "id": "Ya Setiap Koefisien Dipengaruhi Seluruh Sinyal." },
  { "en": "Mengubah Satu Sampel Di Waktu?", "id": "Mengubah Semua Koefisien Di Domain Frekuensi." },
  { "en": "Mengubah Satu Koefisien Di Frekuensi?", "id": "Mengubah Semua Sampel Di Domain Waktu." },
  { "en": "Apa Itu Analisis Vektor Sinyal (VSA)?", "id": "Alat Yang Menampilkan Waktu Dan Frekuensi." },
  { "en": "VSA Bisa Menampilkan Apa?", "id": "Spektogram Diagram Konstelasi Dan Diagram Mata." },
  { "en": "Apa Itu Sinyal Seismograf?", "id": "Grafik Getaran Tanah Di Domain Waktu." },
  { "en": "Analisis Frekuensinya Untuk?", "id": "Menentukan Struktur Bumi Dan Besaran Gempa." },
  { "en": "Apa Itu Sinyal Sonar?", "id": "Pulsa Suara Di Domain Waktu." },
  { "en": "Gema Yang Kembali Dianalisis Di?", "id": "Domain Waktu Untuk Menentukan Jarak." },
  { "en": "Pergeseran Doppler Gema Dianalisis Di?", "id": "Domain Frekuensi Untuk Menentukan Kecepatan." },
  { "en": "Apa Itu Sinyal Radar?", "id": "Pulsa Elektromagnetik Di Domain Waktu." },
  { "en": "Analisis Radar Mirip Dengan?", "id": "Analisis Sonar Tapi Menggunakan Gelombang Radio." },
  { "en": "Melihat Riak Di Catu Daya?", "id": "Riak Adalah Osilasi Kecil Di Domain Waktu." },
  { "en": "Frekuensi Riak Dilihat Di?", "id": "Domain Frekuensi." },
  { "en": "Sumber Riak Adalah Proses?", "id": "Penyearahan (Rectification) Yang Tidak Sempurna." },
  { "en": "Analisis Keduanya Penting Untuk?", "id": "Desain Dan Diagnosis Sistem Elektronik." },
  { "en": "Domain Waktu Menjawab 'Kapan'?", "id": "Ya Kapan Peristiwa Terjadi." },
  { "en": "Domain Frekuensi Menjawab 'Dari Apa'?", "id": "Ya Dari Apa Sinyal Itu Terbuat." },
  { "en": "Domain Waktu Peduli Tentang Apa?", "id": "Urutan Kejadian Durasi Dan Amplitudo Sesaat." },
  { "en": "Domain Frekuensi Peduli Tentang Apa?", "id": "Komposisi Periodisitas Dan Energi Harmonik." },
  { "en": "Transformasi Fourier Bertindak Seperti Prisma Untuk?", "id": "Memisahkan Sinyal Menjadi Frekuensi Komponennya." },
  { "en": "Simetri Genap Di Waktu Menyebabkan Spektrum?", "id": "Bernilai Riil Tanpa Komponen Fasa Imajiner." },
  { "en": "Simetri Ganjil Di Waktu Menyebabkan Spektrum?", "id": "Bernilai Imajiner Murni Tanpa Komponen Riil." },
  { "en": "Domain Waktu Seperti Menonton Film?", "id": "Ya Melihat Cerita Berlangsung Adegan Per Adegan." },
  { "en": "Domain Frekuensi Seperti Melihat Daftar Pemeran?", "id": "Ya Melihat Siapa Saja Yang Membangun Cerita." },
  { "en": "Kenapa Menggunakan FFT Bukan DFT Langsung?", "id": "Karena Jauh Lebih Cepat Secara Komputasi." },
  { "en": "Kompleksitas Komputasi DFT?", "id": "Seperempat N Kuadrat (Sangat Lambat)." },
  { "en": "Kompleksitas Komputasi FFT?", "id": "Seperempat N Log N (Sangat Cepat)." },
  { "en": "Apa Beda Spektrum Dan Spektogram?", "id": "Spektrum Statis Spektogram Spektrum Bergerak." },
  { "en": "Apa Beda Konvolusi Dan Korelasi?", "id": "Konvolusi Membalik Satu Sinyal Korelasi Tidak." },
  { "en": "Konvolusi Untuk Analisis?", "id": "Sistem LTI (Filtering)." },
  { "en": "Korelasi Untuk Analisis?", "id": "Kemiripan Antara Dua Sinyal." },
  { "en": "Apa Itu 'Sinyal'?", "id": "Besaran Fisik Yang Berubah Membawa Informasi." },
  { "en": "Apa Itu 'Sistem'?", "id": "Proses Yang Memodifikasi Sinyal Input." },
  { "en": "Keduanya Bisa Dianalisis Di?", "id": "Domain Waktu Dan Domain Frekuensi." },
  { "en": "Deskripsi Sistem Di Waktu?", "id": "Respon Impuls h(t)." },
  { "en": "Deskripsi Sistem Di Frekuensi?", "id": "Respon Frekuensi H(f)." },
  { "en": "Apa Yang Terjadi Pada Sinyal Periodik?", "id": "Bentuknya Berulang Setiap Periode T." },
  { "en": "Energi Sinyal Periodik?", "id": "Tak Terhingga." },
  { "en": "Daya Sinyal Periodik?", "id": "Terbatas Dan Bernilai Positif." },
  { "en": "Apa Itu Sinyal Transien?", "id": "Sinyal Non-Periodik Dengan Durasi Terbatas." },
  { "en": "Energi Sinyal Transien?", "id": "Terbatas Dan Bernilai Positif." },
  { "en": "Daya Sinyal Transien?", "id": "Nol (Karena Durasinya Terbatas)." },
  { "en": "Filter FIR Adalah Konvolusi Di?", "id": "Domain Waktu." },
  { "en": "Filter IIR Dideskripsikan Di?", "id": "Domain Waktu (Persamaan Diferensial)." },
  { "en": "Desain Keduanya Dilakukan Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Delay Konstan?", "id": "Semua Frekuensi Ditunda Dengan Jumlah Sama." },
  { "en": "Delay Konstan Sama Dengan?", "id": "Fasa Linier Di Domain Frekuensi." },
  { "en": "Sinyal Tidak Terdistorsi Jika?", "id": "Gain Datar Dan Fasa Linier." },
  { "en": "Kondisi Ini Disebut Transmisi?", "id": "Transmisi Bebas Distorsi (Distortionless)." },
  { "en": "Apakah Transmisi Ini Mungkin?", "id": "Secara Ideal Ya Secara Praktis Sulit." },
  { "en": "Kanal Nyata Selalu Memiliki?", "id": "Gain Tidak Datar Dan Fasa Non-Linier." },
  { "en": "Tugas Equalizer Adalah?", "id": "Mengkompensasi Ketidakidealan Kanal." },
  { "en": "Jika Sinyal Di Waktu Sangat Kompleks?", "id": "Spektrumnya Bisa Jadi Sangat Sederhana." },
  { "en": "Contohnya Adalah?", "id": "Derau Putih (Sangat Kompleks Di Waktu)." },
  { "en": "Spektrum Derau Putih?", "id": "Sangat Sederhana (Garis Datar)." },
  { "en": "Jika Sinyal Di Waktu Sangat Sederhana?", "id": "Spektrumnya Bisa Jadi Sangat Kompleks." },
  { "en": "Contohnya Adalah?", "id": "Impuls Dirac (Sangat Sederhana Di Waktu)." },
  { "en": "Spektrum Impuls Dirac?", "id": "Kompleks (Garis Datar Mengandung Semua Frekuensi)." },
  { "en": "Apa Arti Fisis Frekuensi Nol?", "id": "Komponen DC Atau Nilai Rata-rata Sinyal." },
  { "en": "Apa Arti Fisis Frekuensi Tak Hingga?", "id": "Perubahan Sesaat Atau Diskontinuitas." },
  { "en": "Sinyal Dunia Nyata Punya Frekuensi Tak Hingga?", "id": "Tidak Karena Inersia Fisik." },
  { "en": "Apa Itu Bandwidth Esensial?", "id": "Pita Frekuensi Dimana Sebagian Besar Energi Ada." },
  { "en": "Contohnya 99% Energi?", "id": "Ya Itu Definisi Yang Umum." },
  { "en": "Parameter Ini Diukur Di?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Titik Setengah Daya?", "id": "Frekuensi Dimana Daya Turun Setengah." },
  { "en": "Ini Sama Dengan Berapa Desibel?", "id": "Turun 3 Desibel (dB)." },
  { "en": "Bandwidth 3-dB Adalah?", "id": "Ukuran Standar Untuk Bandwidth Sistem." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sinyal Dengan Spektrum Di Sekitar DC." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sinyal Dengan Spektrum Di Sekitar Frekuensi Pembawa." },
  { "en": "Proses Downconversion Mengubah?", "id": "Sinyal Passband Kembali Ke Baseband." },
  { "en": "Ini Adalah Inti Dari?", "id": "Penerima Radio Superheterodyne." },
  { "en": "Operasi Ini Melibatkan Perkalian Di?", "id": "Domain Waktu (Dengan Osilator Lokal)." },
  { "en": "Diikuti Oleh Filtering Di?", "id": "Domain Frekuensi (Filter Low-Pass)." },
  { "en": "Apakah Waktu Bisa Negatif?", "id": "Dalam Matematika Ya Dalam Fisika Klasik Tidak." },
  { "en": "Apakah Frekuensi Bisa Negatif?", "id": "Hanya Konstruk Matematis Yang Berguna." },
  { "en": "Frekuensi Negatif Membantu Representasi?", "id": "Fasa Sinyal Secara Ringkas." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Kompleks Tanpa Frekuensi Negatif." },
  { "en": "Sinyal Analitik Berguna Untuk?", "id": "Menghitung Amplop Dan Frekuensi Sesaat." },
  { "en": "Amplop Adalah Properti Domain?", "id": "Waktu." },
  { "en": "Frekuensi Sesaat Adalah Properti Domain?", "id": "Waktu." },
  { "en": "Untuk Mendapatkannya Kita Perlu?", "id": "Informasi Dari Domain Frekuensi." },
  { "en": "Apa Itu Ruang (Space)?", "id": "Dimensi Fisik (Panjang Lebar Tinggi)." },
  { "en": "Domain Spasial Adalah Representasi?", "id": "Sinyal Terhadap Ruang (Contoh Gambar)." },
  { "en": "Domain Frekuensi Spasial Adalah?", "id": "Representasi Komposisi Spasial Sinyal." },
  { "en": "Garis Halus Di Gambar Adalah?", "id": "Frekuensi Spasial Rendah." },
  { "en": "Tekstur Kasar Di Gambar Adalah?", "id": "Frekuensi Spasial Tinggi." },
  { "en": "Apa Itu Sinyal Broadband?", "id": "Sama Dengan Sinyal Pita Lebar." },
  { "en": "Apa Itu Sinyal Narrowband?", "id": "Sama Dengan Sinyal Pita Sempit." },
  { "en": "Apa Itu Spektrum Simetris?", "id": "Bagian Kiri Dan Kanan Adalah Cerminan." },
  { "en": "Apa Itu Spektrum Anti-Simetris?", "id": "Bagian Kiri Adalah Negatif Cerminan Kanan." },
  { "en": "Magnitudo Spektrum Sinyal Real?", "id": "Selalu Simetris." },
  { "en": "Fasa Spektrum Sinyal Real?", "id": "Selalu Anti-Simetris." },
  { "en": "Sinyal Yang Sama Dua Cara Pandang?", "id": "Ya Itulah Inti Dari Analisis Ini." },
  { "en": "Cara Pandang Waktu Adalah?", "id": "Lokal Dan Sesaat." },
  { "en": "Cara Pandang Frekuensi Adalah?", "id": "Global Dan Rata-rata." },
  { "en": "Mengubah Satu Titik Di Waktu?", "id": "Mengubah Seluruh Spektrum Di Frekuensi." },
  { "en": "Mengubah Satu Titik Di Frekuensi?", "id": "Mengubah Seluruh Bentuk Gelombang Di Waktu." },
  { "en": "Transformasi Itu Holistik?", "id": "Ya Mempertimbangkan Seluruh Sinyal Sekaligus." },
  { "en": "Apa Itu Gelombang Berdiri (Standing Wave)?", "id": "Pola Getaran Tetap Di Domain Waktu." },
  { "en": "Penyebab Gelombang Berdiri?", "id": "Interferensi Antara Gelombang Dan Pantulannya." },
  { "en": "Analisisnya Dilakukan Di?", "id": "Kedua Domain." },
  { "en": "Di Waktu Terlihat Titik Diam?", "id": "Ya Disebut Simpul (Node)." },
  { "en": "Di Frekuensi Terlihat Resonansi?", "id": "Ya Pada Frekuensi Harmonik Tertentu." },
  { "en": "Apa Itu Transformasi Fourier Invers?", "id": "Proses Dari Frekuensi Kembali Ke Waktu." },
  { "en": "Inputnya Adalah?", "id": "Spektrum Kompleks (Magnitudo Dan Fasa)." },
  { "en": "Outputnya Adalah?", "id": "Sinyal Bernilai Riil Di Domain Waktu." },
  { "en": "Apa Yang Terjadi Pada Frekuensi Negatif?", "id": "Berkombinasi Dengan Positif Untuk Membatalkan Bagian Imajiner." },
  { "en": "Ini Adalah Konsekuensi Dari?", "id": "Simetri Hermite Dari Spektrum." },
  { "en": "Dua Perspektif Satu Realitas?", "id": "Benar Sekali." },
  { "en": "Realitasnya Adalah Sinyal Di?", "id": "Domain Waktu." },
  { "en": "Frekuensi Adalah Konstruk Matematis?", "id": "Ya Namun Dengan Interpretasi Fisik Kuat." },
  { "en": "Tanpa Frekuensi Analisis Akan?", "id": "Sangat Sulit Dan Kurang Intuitif." },
  { "en": "Kedua Domain Tak Terpisahkan?", "id": "Ya Dalam Teori Sinyal Dan Sistem." },
  { "en": "Apa Ide Utama Di Balik Dua Domain?", "id": "Satu Sinyal Punya Dua Representasi Berbeda." },
  { "en": "Kenapa Transformasi Antar Domain Sangat Berguna?", "id": "Karena Masalah Sulit Bisa Menjadi Mudah." },
  { "en": "Untuk Mengukur Jitter, Domain Apa Terbaik?", "id": "Domain Waktu Untuk Melihat Deviasi Timing." },
  { "en": "Untuk Mendesain Filter Audio, Domain Apa?", "id": "Domain Frekuensi Untuk Membentuk Spektrum Nada." },
  { "en": "Untuk Menganalisis Ritme Musik, Domain Apa?", "id": "Domain Waktu Untuk Melihat Pola Ketukan." },
  { "en": "Untuk Mengidentifikasi Nada Musik, Domain Apa?", "id": "Domain Frekuensi Untuk Melihat Pitch Harmonik." },
  { "en": "Dualitas Dari Pulsa Kotak Adalah Apa?", "id": "Fungsi Sinc Di Domain Lainnya." },
  { "en": "Dualitas Dari Impuls Adalah Apa?", "id": "Garis Datar Di Domain Lainnya." },
  { "en": "Simetri Spektrum Sinyal Real Adalah?", "id": "Simetri Konjugat Kompleks (Hermitian)." },
  { "en": "Apa Itu 'Bentuk Gelombang' (Waveform)?", "id": "Representasi Visual Sinyal Di Domain Waktu." },
  { "en": "Apa Itu 'Spektrum' (Spectrum)?", "id": "Representasi Visual Sinyal Di Domain Frekuensi." },
  { "en": "Suara Guntur Adalah Sinyal Domain?", "id": "Domain Waktu (Transien Berenergi Tinggi)." },
  { "en": "Warna Langit Adalah Fenomena Domain?", "id": "Domain Frekuensi (Hamburan Cahaya Matahari)." },
  { "en": "Langkah Pertama Analisis Sinyal Adalah?", "id": "Akuisisi Atau Perekaman Sinyal Di Waktu." },
  { "en": "Tujuan Akhir Analisis Sinyal Adalah?", "id": "Mengekstrak Informasi Yang Berguna Dari Sinyal." },
  { "en": "Domain Waktu Adalah Dunia 'Menjadi'?", "id": "Ya Melihat Proses Terjadi Seiring Waktu." },
  { "en": "Domain Frekuensi Adalah Dunia 'Adalah'?", "id": "Ya Melihat Komposisi Sinyal Yang Tetap." },
  { "en": "Melihat Riak Air Adalah Pengamatan?", "id": "Domain Waktu Dan Ruang." },
  { "en": "Menganalisis Panjang Gelombang Riak Adalah?", "id": "Analisis Domain Frekuensi Spasial." },
  { "en": "Puncak Gelombang Adalah Fitur?", "id": "Domain Waktu." },
  { "en": "Dasar Gelombang Adalah Fitur?", "id": "Domain Waktu." },
  { "en": "Jarak Antar Puncak Adalah Fitur?", "id": "Domain Waktu (Periode)." },
  { "en": "Komposisi Nada Adalah Fitur?", "id": "Domain Frekuensi." },
  { "en": "Kekuatan Harmonik Adalah Fitur?", "id": "Domain Frekuensi." },
  { "en": "Fasa Awal Adalah Fitur?", "id": "Domain Waktu." },
  { "en": "Pergeseran Fasa Adalah Fitur?", "id": "Domain Frekuensi." },
  { "en": "Apa Itu Sinyal Broadband?", "id": "Sinyal Dengan Bandwidth Sangat Lebar." },
  { "en": "Apa Itu Sinyal Carrierless?", "id": "Sinyal Baseband Tanpa Modulasi Ke Frekuensi Tinggi." },
  { "en": "Contoh Sinyal Carrierless?", "id": "Ethernet Atau Sinyal USB." },
  { "en": "Apa Itu Sinyal Termodulasi?", "id": "Sinyal Passband Yang Menggunakan Carrier." },
  { "en": "Contoh Sinyal Termodulasi?", "id": "Sinyal Radio Wifi Atau Seluler." },
  { "en": "Sinyal Kontinu Punya Spektrum?", "id": "Non-Periodik." },
  { "en": "Sinyal Diskrit Punya Spektrum?", "id": "Periodik." },
  { "en": "Sinyal Periodik Punya Spektrum?", "id": "Diskrit." },
  { "en": "Sinyal Non-Periodik Punya Spektrum?", "id": "Kontinu." },
  { "en": "Sifat Ini Adalah Inti?", "id": "Dualitas Antara Dua Domain." },
  { "en": "Apa Itu Channel Impulse Response (CIR)?", "id": "Deskripsi Kanal Di Domain Waktu." },
  { "en": "Apa Itu Channel Frequency Response (CFR)?", "id": "Deskripsi Kanal Di Domain Frekuensi." },
  { "en": "Keduanya Adalah Pasangan Transformasi?", "id": "Ya Pasangan Transformasi Fourier." },
  { "en": "Jika CIR Pendek Dan Tajam?", "id": "CFR Akan Lebar Dan Datar." },
  { "en": "Jika CIR Menyebar Luas?", "id": "CFR Akan Memiliki Banyak Fading." },
  { "en": "Apa Itu Sinyal Stasioner?", "id": "Statistiknya Tidak Berubah Seiring Waktu." },
  { "en": "Apa Itu Sinyal Ergodik?", "id": "Rata-rata Waktu Sama Dengan Rata-rata Ensemble." },
  { "en": "Kebanyakan Sinyal Stasioner Dianggap?", "id": "Ergodik Untuk Memudahkan Analisis." },
  { "en": "Apa Itu Representasi Waktu?", "id": "Urutan Sampel Amplitudo." },
  { "en": "Apa Itu Representasi Frekuensi?", "id": "Kumpulan Koefisien Kompleks." },
  { "en": "Jumlah Angka Di Keduanya Sama?", "id": "Ya (Untuk DFT)." },
  { "en": "Tidak Ada Informasi Yang Hilang?", "id": "Benar Hanya Perspektif Yang Berubah." },
  { "en": "Apa Itu Efek Pendengaran Manusia?", "id": "Tidak Linier Terhadap Frekuensi Dan Amplitudo." },
  { "en": "Skala Mel Dan Bark Adalah?", "id": "Skala Frekuensi Psikoakustik." },
  { "en": "Skala Ini Meniru Respon?", "id": "Telinga Manusia Terhadap Frekuensi." },
  { "en": "Analisis Audio Sering Menggunakan?", "id": "Skala Frekuensi Logaritmik Atau Psikoakustik." },
  { "en": "Apa Itu Pemrosesan Sinyal 1D?", "id": "Memproses Sinyal Dengan Satu Variabel (Waktu)." },
  { "en": "Apa Itu Pemrosesan Sinyal 2D?", "id": "Memproses Sinyal Dengan Dua Variabel (Gambar)." },
  { "en": "Apa Itu Pemrosesan Sinyal 3D?", "id": "Memproses Sinyal Tiga Variabel (Video Volume)." },
  { "en": "Konsep Domain Frekuensi Berlaku?", "id": "Ya Untuk Semua Dimensi." },
  { "en": "Untuk Melihat Durasi Kita Butuh?", "id": "Domain Waktu." },
  { "en": "Untuk Melihat Komposisi Kita Butuh?", "id": "Domain Frekuensi." },
  { "en": "Untuk Melihat Evolusi Komposisi Kita Butuh?", "id": "Analisis Waktu-Frekuensi (Spektogram)." },
  { "en": "Ketiganya Adalah Alat Analisis?", "id": "Fundamental Dalam Ilmu Sinyal." },
  { "en": "Apa Itu Windowing Dalam Analisis Spektral?", "id": "Mengalikan Blok Sinyal Dengan Fungsi Jendela." },
  { "en": "Ini Dilakukan Di Domain?", "id": "Domain Waktu." },
  { "en": "Tujuannya Di Domain Frekuensi?", "id": "Mengurangi Efek Samping Bocoran Spektral." },
  { "en": "Apa Itu Jitter Deterministik?", "id": "Jitter Dengan Pola Yang Dapat Diprediksi." },
  { "en": "Spektrumnya Menunjukkan?", "id": "Spur Harmonik Diskrit." },
  { "en": "Apa Itu Jitter Acak?", "id": "Jitter Tanpa Pola Yang Bisa Diprediksi." },
  { "en": "Spektrumnya Menunjukkan?", "id": "Peningkatan Noise Floor Yang Melebar." },
  { "en": "Analisis Jitter Dilakukan Di?", "id": "Kedua Domain Waktu Dan Frekuensi." },
  { "en": "Apa Itu Sinyal Siklostasioner?", "id": "Sinyal Acak Yang Statistiknya Berulang." },
  { "en": "Fitur Ini Digunakan Untuk?", "id": "Deteksi Sinyal Di Bawah Noise." },
  { "en": "Korelasi Siklik Adalah Analisis?", "id": "Domain Frekuensi Dua Dimensi." },
  { "en": "Apa Itu Time-Domain Reflectometry (TDR)?", "id": "Mengukur Pantulan Sinyal Di Kabel." },
  { "en": "TDR Bekerja Di Domain?", "id": "Domain Waktu Untuk Mencari Lokasi Kerusakan." },
  { "en": "Apa Itu Vector Network Analyzer (VNA)?", "id": "Mengukur Karakteristik Jaringan Di Frekuensi." },
  { "en": "VNA Bekerja Di Domain?", "id": "Domain Frekuensi (Mengukur Parameter-S)." },
  { "en": "Keduanya Adalah Alat Komplementer?", "id": "Ya Untuk Menganalisis Integritas Sinyal." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Representasi Kontinu Di Waktu Dan Amplitudo." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Representasi Diskrit Di Waktu Dan Amplitudo." },
  { "en": "Transformasi Fourier Berlaku Untuk?", "id": "Kedua Jenis Sinyal (Dengan Varian Berbeda)." },
  { "en": "Apa Itu Teorema Wiener-Khinchin?", "id": "Menghubungkan Autokorelasi Dan Spektrum Daya." },
  { "en": "Autokorelasi Adalah Operasi?", "id": "Domain Waktu." },
  { "en": "Spektrum Daya Adalah Properti?", "id": "Domain Frekuensi." },
  { "en": "Teorema Ini Adalah Jembatan Untuk?", "id": "Sinyal Acak Antara Dua Domain." },
  { "en": "Puncak Tajam Di Autokorelasi?", "id": "Menandakan Adanya Periodisitas Tersembunyi." },
  { "en": "Ini Akan Sesuai Dengan Puncak?", "id": "Di Spektrum Daya Pada Frekuensi Sama." },
  { "en": "Transformasi Fourier Itu Sendiri?", "id": "Sebuah Sinyal Di Domain Frekuensi." },
  { "en": "Bisa Kita Transformasikan Lagi?", "id": "Ya Hasilnya Kembali Ke Domain Waktu Terbalik." },
  { "en": "Ini Adalah Sifat?", "id": "Dualitas Dari Transformasi Fourier." },
  { "en": "Apa Itu Sinyal Chirp?", "id": "Frekuensinya Berubah Linier Seiring Waktu." },
  { "en": "Ini Adalah Sinyal Non-Stasioner?", "id": "Ya Contoh Klasik Sinyal Non-Stasioner." },
  { "en": "Analisisnya Membutuhkan Tampilan?", "id": "Waktu-Frekuensi Seperti Spektogram." },
  { "en": "Spektogramnya Akan Menunjukkan?", "id": "Garis Lurus Diagonal." },
  { "en": "Melihat Bentuk Adalah Analisis?", "id": "Domain Waktu." },
  { "en": "Melihat Komposisi Adalah Analisis?", "id": "Domain Frekuensi." },
  { "en": "Melihat Pola Adalah Analisis?", "id": "Bisa Di Kedua Domain." },
  { "en": "Pola Di Waktu Adalah?", "id": "Ritme Dan Perulangan." },
  { "en": "Pola Di Frekuensi Adalah?", "id": "Struktur Harmonik Dan Tanda Tangan Spektral." },
  { "en": "Pemahaman Lengkap Membutuhkan?", "id": "Kemampuan Berpindah Antara Kedua Perspektif." },
  { "en": "Seri Ini Telah Menjelajahi?", "id": "Fondasi Dasar Dari Dua Domain Ini." },
  { "en": "Semoga Memberi Pemahaman?", "id": "Yang Lebih Baik Dan Intuitif." }



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
