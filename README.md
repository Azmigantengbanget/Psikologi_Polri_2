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


  { "en": "Anda sedang berpatroli dan melihat seorang pengemudi ojek online dimarahi habis-habisan oleh pelanggan karena kesalahan kecil. Apa tindakan Anda?", "id": "Saya akan mendekat, menengahi dengan tenang, dan memfasilitasi komunikasi yang lebih baik antara keduanya agar masalah tidak berlarut-larut dan mengganggu ketertiban." },
  { "en": "Bagaimana Anda menyikapi rekan kerja yang sangat religius dan sering mengajak Anda beribadah, sementara Anda memiliki keyakinan yang berbeda?", "id": "Saya akan menghormati ajakannya sebagai bentuk niat baik, menolaknya dengan sopan, dan menjelaskan bahwa saya memiliki cara beribadah sendiri. Toleransi adalah kuncinya." },
  { "en": "Anda diminta untuk memberikan data untuk laporan atasan. Anda tahu data tersebut akan digunakan untuk menyalahkan unit lain. Apa yang Anda lakukan?", "id": "Saya akan memberikan data yang objektif dan faktual sesuai permintaan, tanpa menambahkan opini atau interpretasi yang menyudutkan. Tugas saya adalah menyajikan fakta, bukan menghakimi." },
  { "en": "Apa yang Anda lakukan jika Anda merasa atasan memberikan penilaian kinerja yang tidak objektif kepada Anda?", "id": "Saya akan meminta waktu untuk berdiskusi, menanyakan dasar penilaian tersebut dengan data kinerja yang saya miliki, dan menyampaikannya sebagai bahan klarifikasi, bukan sebagai protes." },
  { "en": "Menurut Anda, apa bedanya 'berani' dengan 'nekat'?", "id": "Berani adalah mengambil risiko yang sudah diperhitungkan berdasarkan analisis dan pelatihan. Nekat adalah mengambil risiko tanpa perhitungan, hanya berdasarkan emosi atau dorongan sesaat." },
  { "en": "Anda melihat seorang junior yang sangat cerdas tetapi sulit bekerja dalam tim. Bagaimana Anda membimbingnya?", "id": "Saya akan memberinya tugas yang secara spesifik membutuhkan kolaborasi, memasangkannya dengan rekan yang kooperatif, dan memberikan umpan balik mengenai pentingnya kerja sama tim untuk kesuksesan bersama." },
  { "en": "Bagaimana Anda menjelaskan kepada keluarga jika Anda tidak bisa pulang pada hari raya karena tugas?", "id": "Saya akan menjelaskannya jauh-jauh hari dengan penuh pengertian, menekankan bahwa ini adalah bagian dari tanggung jawab profesi saya untuk memastikan masyarakat lain dapat merayakan dengan aman." },
  { "en": "Apa arti 'presisi' dalam penggunaan senjata api?", "id": "Artinya setiap tembakan yang dilepaskan harus tepat sasaran, beralasan kuat sesuai hukum, dan dilakukan dengan perhitungan matang untuk meminimalisir korban atau kerusakan yang tidak perlu." },
  { "en": "Anda merasa tidak memiliki 'bakat' alami dalam suatu bidang tugas (misal: berbicara di depan umum). Bagaimana Anda mengatasinya?", "id": "Saya percaya 'bakat' bisa diasah. Saya akan proaktif belajar, berlatih secara rutin, dan meminta masukan dari mereka yang lebih ahli untuk mengatasi kekurangan saya." },
  { "en": "Bagaimana Anda memandang seorang tersangka yang jelas-jelas bersalah namun menunjukkan penyesalan yang mendalam?", "id": "Proses hukum harus tetap berjalan secara objektif. Namun, penyesalannya bisa menjadi pertimbangan untuk perlakuan yang lebih humanis selama proses pemeriksaan dan bisa dicatat sebagai bagian dari laporan." },
  { "en": "Lengkapi analogi: JARUM : JAM = JARI : ...", "id": "MANUSIA." },
  { "en": "Lengkapi analogi: BENDUNGAN : AIR = TANGGUL : ...", "id": "LUMPUR (atau BANJIR)." },
  { "en": "Lengkapi analogi: PUJANGGA : KARYA SASTRA = Maestro : ...", "id": "KARYA SENI." },
  { "en": "Lengkapi analogi: MATAHARI : PUSAT = PLANET : ...", "id": "PENGORBIT." },
  { "en": "Lengkapi analogi: KULIT : GATAL = HIDUNG : ...", "id": "BERSIN." },
  { "en": "Lengkapi analogi: TEORI : PRAKTIK = RENCANA : ...", "id": "PELAKSANAAN." },
  { "en": "Lengkapi analogi: FOSIL : PURBAKALA = BINTANG : ...", "id": "ASTRONOMI." },
  { "en": "Lengkapi analogi: KERING : KEMARAU = LEBAT : ...", "id": "HUJAN." },
  { "en": "Lengkapi analogi: SKENARIO : FILM = SILABUS : ...", "id": "MATA PELAJARAN." },
  { "en": "Lengkapi analogi: KERTAS : MENYERAP = CERMIN : ...", "id": "MEMANTULKAN." },
  { "en": "Lanjutkan deret angka: 2, 6, 12, 20, 30, 42, ...", "id": "56 (pola: n(n+1) -> 1x2, 2x3, 3x4, dst)." },
  { "en": "Lanjutkan deret angka: 1, 3, 6, 8, 16, 18, ...", "id": "36 (pola: +2, x2, +2, x2, diulang)." },
  { "en": "Lanjutkan deret huruf: A, C, G, M, U, ...", "id": "E (pola loncat: +2, +4, +6, +8, +10. Setelah U, 10 huruf kemudian adalah E)." },
  { "en": "Lanjutkan deret angka: 2, 10, 4, 12, 6, 14, ...", "id": "8 (pola dua larik: 2,4,6,8,... dan 10,12,14,...)." },
  { "en": "Lanjutkan deret angka: 100, 97, 92, 85, 76, ...", "id": "65 (pola: -3, -5, -7, -9, -11)." },
  { "en": "Lanjutkan deret angka: 4, 2, 1, 1/2, 1/4, ...", "id": "1/8 (pola: dibagi 2)." },
  { "en": "Lanjutkan deret huruf: A, B, F, G, K, L, ...", "id": "P (pola: 2 huruf urut, loncat 3 huruf)." },
  { "en": "Lanjutkan deret angka: 1, 2, 9, 3, 4, 9, 5, ...", "id": "6 (pola: dua angka urut, diselingi angka 9)." },
  { "en": "Lanjutkan deret angka: 1, 1, 4, 2, 9, 3, 16, ...", "id": "4 (pola dua larik: 1,4,9,16 [kuadrat] dan 1,2,3,4)." },
  { "en": "Lanjutkan deret angka: 7, 14, 21, 28, 35, ...", "id": "42 (pola: kelipatan 7)." },
  { "en": "Apa sinonim dari kata 'INTRINSIK'?", "id": "Melekat pada hakikatnya." },
  { "en": "Apa sinonim dari kata 'KOMUTATIF'?", "id": "Bersifat dapat dipertukarkan." },
  { "en": "Apa sinonim dari kata 'PREDILEKSI'?", "id": "Kecenderungan atau pilihan utama." },
  { "en": "Apa sinonim dari kata 'SEREMONIAL'?", "id": "Bersifat upacara." },
  { "en": "Apa sinonim dari kata 'TAKSA'?", "id": "Ambiguitas atau bermakna ganda." },
  { "en": "Apa antonim dari kata 'DEDUKTIF'?", "id": "Induktif." },
  { "en": "Apa antonim dari kata 'FRONTAL'?", "id": "Tidak langsung atau dari belakang." },
  { "en": "Apa antonim dari kata 'MUSYKIL'?", "id": "Mudah atau mungkin." },
  { "en": "Apa antonim dari kata 'PASIF'?", "id": "Aktif." },
  { "en": "Apa antonim dari kata 'SEKUNDER'?", "id": "Primer." },
  { "en": "Jika Tono lebih tinggi dari Budi, dan Tono lebih pendek dari Candra. Siapa yang paling tinggi?", "id": "Candra." },
  { "en": "Semua ikan bertelur. Buaya bertelur. Kesimpulannya adalah...", "id": "Tidak dapat disimpulkan apakah buaya adalah ikan." },
  { "en": "Jika saya lapar, saya makan. Saya tidak makan. Kesimpulannya adalah...", "id": "Saya tidak lapar." },
  { "en": "Sebuah kotak berisi bola merah dan biru. Jumlah bola merah adalah dua kali bola biru. Pernyataan mana yang pasti benar?", "id": "Jumlah total bola adalah kelipatan tiga." },
  { "en": "Jika besok lusa adalah hari Minggu, maka kemarin adalah hari...", "id": "Kamis." },
  { "en": "Berapakah 3/5 dari 50%?", "id": "30% atau 0.3." },
  { "en": "Sebuah mobil melaju dari kota A ke B dengan kecepatan 40 km/jam dan kembali dengan kecepatan 60 km/jam. Berapa kecepatan rata-ratanya?", "id": "48 km/jam." },
  { "en": "Jika 10% dari sebuah angka adalah 25, berapakah 40% dari angka tersebut?", "id": "100." },
  { "en": "Berapakah modus, median, dan mean dari data: 2, 4, 4, 6, 9?", "id": "Modus=4, Median=4, Mean=5." },
  { "en": "Sebuah segitiga siku-siku memiliki sisi miring 13 cm dan salah satu sisi tegaknya 5 cm. Berapa luas segitiga tersebut?", "id": "30 cm²." },
  { "en": "Bagaimana Anda menyikapi kegagalan teknologi (misal: sistem down) saat sedang melayani masyarakat?", "id": "Saya akan segera beralih ke prosedur manual jika memungkinkan, sambil terus berkomunikasi dengan tim teknis dan memberikan penjelasan yang jujur kepada masyarakat." },
  { "en": "Apa yang Anda lakukan jika Anda merasa tidak 'nyambung' dengan generasi yang lebih muda (junior) di unit Anda?", "id": "Saya akan mencoba memahami perspektif dan cara komunikasi mereka, tidak memaksakan cara lama, dan mencari topik atau kegiatan yang bisa menjadi jembatan penghubung." },
  { "en": "Bagaimana Anda membedakan antara 'perintah yang salah' dengan 'perintah yang tidak Anda sukai'?", "id": "Perintah yang salah adalah yang melanggar hukum atau prosedur. Perintah yang tidak saya sukai mungkin hanya tidak sesuai dengan preferensi pribadi saya, namun tetap sah dan harus dilaksanakan." },
  { "en": "Apa arti 'menjadi pembelajar seumur hidup' bagi seorang polisi?", "id": "Artinya tidak pernah berhenti belajar, baik dari pendidikan formal, pelatihan, pengalaman di lapangan, maupun dari interaksi dengan masyarakat, untuk terus beradaptasi dan menjadi lebih baik." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus menegur seorang tokoh masyarakat yang dihormati karena melanggar aturan?", "id": "Saya akan melakukannya dengan cara yang sangat sopan dan penuh hormat, menjelaskan aturannya secara pribadi, dan menekankan bahwa ini demi kebaikan bersama." },
  { "en": "Apa yang Anda lakukan untuk menjaga motivasi pada titik terendah dalam karir Anda?", "id": "Saya akan kembali mengingat alasan utama saya bergabung dengan Polri, melihat dampak positif dari pekerjaan saya, dan berdiskusi dengan rekan atau mentor yang bisa memberikan perspektif baru." },
  { "en": "Bagaimana Anda menanggapi permintaan 'uang rokok' dari oknum petugas lain saat mengurus sesuatu?", "id": "Saya akan menolaknya dengan tegas dan sopan, dan jika memungkinkan, saya akan melaporkan praktik tersebut melalui saluran pengaduan yang ada karena itu merusak citra institusi." },
  { "en": "Menurut Anda, apa kualitas terpenting dari seorang pemimpin dalam situasi krisis?", "id": "Ketenangan. Pemimpin yang tenang mampu berpikir jernih, membuat keputusan yang tepat, dan menularkan rasa percaya diri kepada timnya." },
  { "en": "Apa yang lebih berbahaya: musuh yang terlihat atau musuh dalam selimut?", "id": "Musuh dalam selimut, karena mereka menggerogoti dari dalam, merusak kepercayaan, dan sulit untuk dideteksi." },
  { "en": "Bagaimana Anda memastikan bahwa Anda tidak menjadi 'kebal' atau tidak peduli terhadap penderitaan korban setelah sering menanganinya?", "id": "Dengan secara sadar terus melatih empati, melihat setiap korban sebagai individu dengan cerita unik, dan menyeimbangkannya dengan kehidupan pribadi yang sehat." },
  { "en": "Lengkapi analogi: KULIT : BUKU = SAMPUL : ...", "id": "MAJALAH." },
  { "en": "Lengkapi analogi: TEMBOK : CAT = LANTAI : ...", "id": "KERAMIK (atau KARPET)." },
  { "en": "Lengkapi analogi: MOBIL : GARASI = PESAWAT : ...", "id": "HANGGAR." },
  { "en": "Lengkapi analogi: KELUARGA : RUMAH = LEBAH : ...", "id": "SARANG." },
  { "en": "Lengkapi analogi: KATA : HURUF = MELODI : ...", "id": "NOT." },
  { "en": "Lanjutkan deret angka: 1, 3, 12, 60, 360, ...", "id": "2520 (pola: x3, x4, x5, x6, x7)." },
  { "en": "Lanjutkan deret angka: 2, 3, 5, 8, 13, 21, ...", "id": "34 (pola: deret Fibonacci)." },
  { "en": "Lanjutkan deret huruf: A, G, L, P, S, U, ...", "id": "V (pola loncat: +6, +5, +4, +3, +2, +1)." },
  { "en": "Lanjutkan deret angka: 1, 8, 2, 16, 3, 24, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 8,16,24,...)." },
  { "en": "Lanjutkan deret angka: 99, 96, 91, 84, 75, ...", "id": "64 (pola: -3, -5, -7, -9, -11)." },
  { "en": "Apa sinonim dari kata 'AGREGAT'?", "id": "Keseluruhan atau jumlah." },
  { "en": "Apa sinonim dari kata 'BIAS'?", "id": "Penyimpangan atau keberpihakan." },
  { "en": "Apa sinonim dari kata 'DEDIKASI'?", "id": "Pengabdian." },
  { "en": "Apa antonim dari kata 'MENDUKUNG'?", "id": "Menentang." },
  { "en": "Apa antonim dari kata 'TEORI'?", "id": "Praktik." },
  { "en": "Jika x adalah 15% dari 60, dan y adalah 60% dari 15, maka...", "id": "x = y (keduanya bernilai 9)." },
  { "en": "Sebuah rapat dimulai pukul 10:40 dan selesai pukul 12:15. Berapa menit rapat itu berlangsung?", "id": "95 menit." },
  { "en": "Jika 3 ekor ayam bertelur 3 butir dalam 3 hari, berapa butir telur yang dihasilkan 1 ekor ayam dalam 6 hari?", "id": "2 butir." },
  { "en": "Berapakah 20% dari 1/5?", "id": "0.04 atau 1/25." },
  { "en": "Jika sebuah persegi memiliki luas 64 cm², berapa kelilingnya?", "id": "32 cm." },
  { "en": "Apa yang Anda lakukan jika seorang warga memberikan apresiasi berupa hadiah mahal setelah Anda membantunya?", "id": "Saya akan berterima kasih atas niat baiknya, namun dengan tegas dan sopan menolak hadiah tersebut karena membantu masyarakat adalah kewajiban saya dan menerima hadiah adalah gratifikasi." },
  { "en": "Bagaimana Anda memandang penggunaan media sosial oleh pimpinan Polri?", "id": "Sebagai alat yang efektif untuk berkomunikasi langsung dengan publik, menyampaikan kebijakan, dan membangun citra Polri yang lebih humanis dan transparan." },
  { "en": "Apa yang lebih menantang: menghadapi ancaman fisik atau tekanan psikologis?", "id": "Keduanya menantang, namun tekanan psikologis seringkali lebih sulit karena tidak terlihat, berlangsung lama, dan dapat mempengaruhi pengambilan keputusan secara perlahan." },
  { "en": "Bagaimana Anda memastikan diri Anda tidak terjebak dalam 'zona nyaman'?", "id": "Dengan secara sadar mengambil tantangan baru, terus belajar, dan meminta umpan balik dari rekan atau atasan untuk mengetahui area yang perlu saya tingkatkan." },
  { "en": "Jika Anda harus meringkas tujuan hidup Anda sebagai polisi dalam satu kalimat, apa kalimat itu?", "id": "Menjadi abdi negara yang dapat memberikan rasa aman dan keadilan, sehingga saya bisa tidur nyenyak karena tahu saya telah berbuat baik hari itu." },
  { "en": "Jika setiap hewan berkaki empat adalah mamalia, dan kucing adalah hewan berkaki empat, maka...", "id": "Kucing adalah mamalia." },
  { "en": "Mana yang tidak termasuk dalam kelompok: Jakarta, Bangkok, Manila, Singapura, Kuala Lumpur?", "id": "Singapura (karena merupakan negara-kota, yang lain adalah ibu kota dari negara yang lebih besar)." },
  { "en": "Lengkapi analogi: OBAT : SEMBUH = ILMU : ...", "id": "PANDAI." },
  { "en": "Lanjutkan deret: 1, 4, 2, 8, 3, 12, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 4,8,12,...)." },
  { "en": "Apa sinonim dari 'PARADOKSAL'?", "id": "Bertentangan namun bisa jadi benar." },
  { "en": "Apa antonim dari 'PERCAYA DIRI'?", "id": "Minder atau rendah diri." },
  { "en": "Anda mengetahui bahwa ada kesalahan prosedur dalam penangkapan yang dilakukan oleh tim Anda. Apa yang Anda lakukan sebagai pimpinan tim?", "id": "Saya akan segera menghentikan proses, melakukan koreksi sesuai prosedur yang benar, dan melaporkan kejadian tersebut secara jujur kepada atasan. Keadilan prosedural harus diutamakan." },
  { "en": "Berapakah 1/8 jika diubah ke dalam bentuk desimal dan persen?", "id": "0.125 dan 12.5%." },
  { "en": "Apa arti 'integritas' dalam tindakan sehari-hari di luar kedinasan?", "id": "Artinya tetap menjadi orang yang jujur, menepati janji, dan taat pada norma sosial dan hukum meskipun tidak ada yang mengawasi atau tidak sedang memakai seragam." },
  { "en": "Menurut Anda, apa satu hal yang paling sering disalahpahami oleh masyarakat tentang polisi?", "id": "Bahwa polisi itu kaku dan tidak punya hati. Padahal di balik seragam, kami juga manusia biasa yang memiliki emosi dan berusaha melakukan yang terbaik dalam situasi yang seringkali sangat sulit." },
  { "en": "Seorang kerabat dekat meminta Anda untuk 'mencarikan informasi' mengenai kasus yang ditangani unit lain, dengan alasan untuk 'membantu temannya'. Apa jawaban Anda?", "id": "Saya akan menolak dengan sopan dan menjelaskan bahwa informasi penyidikan bersifat rahasia dan tidak etis bagi saya untuk mencampuri kasus unit lain. Hal tersebut adalah pelanggaran." },
  { "en": "Anda melihat banyak anak muda di lingkungan Anda yang tidak punya kegiatan positif. Apa program sederhana yang bisa Anda inisiasi sebagai Bhabinkamtibmas?", "id": "Saya bisa menginisiasi kegiatan olahraga bersama seperti turnamen futsal atau voli, atau membuat forum diskusi kecil tentang topik yang mereka minati untuk membangun kedekatan dan menyalurkan energi mereka." },
  { "en": "Dua rekan senior Anda saling tidak akur dan membuat suasana kerja tidak nyaman. Bagaimana Anda menempatkan diri?", "id": "Saya akan tetap bersikap netral dan profesional kepada keduanya. Saya tidak akan ikut dalam perselisihan mereka dan fokus menjaga komunikasi yang baik terkait pekerjaan demi soliditas tim." },
  { "en": "Dalam unjuk rasa, seorang demonstran menghina Anda secara pribadi dengan kata-kata yang menyinggung keluarga. Bagaimana Anda menjaga kendali diri?", "id": "Saya akan menarik napas dalam, fokus pada tugas pengamanan, dan mengabaikan hinaan tersebut sebagai provokasi. Menjaga kepala dingin adalah bagian dari profesionalisme saya." },
  { "en": "Apa perbedaan antara 'simulasi' dan 'gladi bersih' dalam konteks pelatihan Polri?", "id": "Simulasi fokus pada latihan aspek-aspek teknis atau taktis tertentu. Gladi bersih adalah latihan terakhir yang meniru kondisi senyata mungkin dari keseluruhan rangkaian kegiatan." },
  { "en": "Anda merasa bahwa atasan Anda membuat keputusan berdasarkan informasi yang tidak lengkap. Apa yang Anda lakukan?", "id": "Saya akan mencari data atau informasi pelengkap yang relevan, lalu meminta izin untuk menyampaikan informasi tambahan tersebut sebagai bahan pertimbangan bagi atasan sebelum keputusan final dibuat." },
  { "en": "Bagaimana Anda menjelaskan kepada warga mengenai pentingnya menjadi saksi dalam sebuah perkara, meskipun mereka takut?", "id": "Saya akan meyakinkan mereka bahwa identitasnya akan dilindungi sesuai hukum, menjelaskan bahwa kesaksian mereka sangat penting untuk keadilan, dan menawarkan pengamanan jika diperlukan." },
  { "en": "Apa arti 'kewaspadaan dini' bagi seorang anggota intelijen?", "id": "Artinya kemampuan untuk mendeteksi potensi ancaman atau gejolak sekecil apapun di masyarakat sebelum hal itu benar-benar terjadi, melalui pengamatan dan analisis yang tajam." },
  { "en": "Anda diminta untuk mengerjakan tugas yang Anda tahu berada di luar yurisdiksi atau kewenangan Anda. Bagaimana sikap Anda?", "id": "Saya akan menolak dengan sopan dan menjelaskan batasan kewenangan saya. Saya bisa membantu mengarahkannya kepada pihak atau unit yang memang berwenang menanganinya." },
  { "en": "Apa warisan non-materi yang paling berharga yang bisa diberikan seorang senior kepada juniornya?", "id": "Teladan integritas, etos kerja yang tinggi, dan kebijaksanaan dalam menghadapi berbagai situasi sulit di lapangan." },
  { "en": "Lengkapi analogi: STIR : MENGARAHKAN = PEDAL GAS : ...", "id": "MEMPERCEPAT." },
  { "en": "Lengkapi analogi: BAJA : JEMBATAN = KAYU : ...", "id": "MEJA (atau KURSI)." },
  { "en": "Lengkapi analogi: HAKIM : VONIS = GURU : ...", "id": "NILAI." },
  { "en": "Lengkapi analogi: PANAS : CAIR = DINGIN : ...", "id": "BEKU." },
  { "en": "Lengkapi analogi: DESAINER : POLA = KOKI : ...", "id": "RESEP." },
  { "en": "Lengkapi analogi: KERINGAT : TUBUH = embun : ...", "id": "DAUN." },
  { "en": "Lengkapi analogi: UNIVERSITAS : FAKULTAS = PERUSAHAAN : ...", "id": "DEPARTEMEN (atau DIVISI)." },
  { "en": "Lengkapi analogi: SASTRAWAN : KATA = PELUKIS : ...", "id": "WARNA." },
  { "en": "Lengkapi analogi: BIBIR : SENYUM = MATA : ...", "id": "KERLINGAN." },
  { "en": "Lengkapi analogi: GUNTING : KAIN = PISAU : ...", "id": "DAGING." },
  { "en": "Lanjutkan deret angka: 3, 5, 9, 17, 33, ...", "id": "65 (pola: x2 - 1)." },
  { "en": "Lanjutkan deret angka: 1, 10, 3, 9, 5, 8, ...", "id": "7 (pola dua larik: 1,3,5,7,... dan 10,9,8,...)." },
  { "en": "Lanjutkan deret huruf: A, B, D, G, K, P, ...", "id": "V (pola loncat: +1, +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 2, 4, 5, 10, 11, 22, ...", "id": "23 (pola: x2, +1, diulang)." },
  { "en": "Lanjutkan deret angka: 3, 9, 13, 19, 23, 29, ...", "id": "33 (pola: +6, +4, +6, +4, +6, +4)." },
  { "en": "Lanjutkan deret angka: 512, 64, 8, 1, ...", "id": "1/8 (Pola: akar pangkat 3 atau dibagi 8. Soal ambigu, jika dibagi 8, jawabannya 1)." },
  { "en": "Lanjutkan deret huruf: A, D, G, J, M, P, ...", "id": "S (pola: loncat 2 huruf)." },
  { "en": "Lanjutkan deret angka: 10, 30, 28, 84, 82, ...", "id": "246 (pola: x3, -2, diulang)." },
  { "en": "Lanjutkan deret angka: 1, 2, 3, 6, 11, 20, ...", "id": "37 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Lanjutkan deret angka: 1, 2, 5, 10, 17, ...", "id": "26 (pola: n² + 1, dimulai dari n=0)." },
  { "en": "Apa sinonim dari kata 'ASUMSI'?", "id": "Anggapan atau dugaan." },
  { "en": "Apa sinonim dari kata 'KOREKTIF'?", "id": "Bersifat memperbaiki." },
  { "en": "Apa sinonim dari kata 'LEGITIM'?", "id": "Sah menurut hukum." },
  { "en": "Apa sinonim dari kata 'NARATIF'?", "id": "Bersifat menguraikan atau bercerita." },
  { "en": "Apa sinonim dari kata 'KUANTITAS'?", "id": "Jumlah atau banyaknya." },
  { "en": "Apa antonim dari kata 'DEFENSIF'?", "id": "Ofensif atau menyerang." },
  { "en": "Apa antonim dari kata 'PROGRESIF'?", "id": "Regresif atau mundur." },
  { "en": "Apa antonim dari kata 'TERATUR'?", "id": "Kacau atau berantakan." },
  { "en": "Apa antonim dari kata 'SINKRON'?", "id": "Asinkron atau tidak selaras." },
  { "en": "Apa antonim dari kata 'ORISINAL'?", "id": "Tiruan atau jiplakan." },
  { "en": "Anda bertemu dua penjaga (A dan B), satu selalu jujur, satu selalu bohong. Pertanyaan apa yang Anda ajukan untuk mengetahui jalan yang benar?", "id": "Saya akan bertanya pada Penjaga A: 'Jalan mana yang akan ditunjuk oleh Penjaga B sebagai jalan yang benar?', lalu saya akan mengambil jalan sebaliknya." },
  { "en": "Semua pemimpin adalah komunikator yang baik. Beberapa orang jujur adalah pemimpin. Kesimpulannya adalah...", "id": "Beberapa orang jujur adalah komunikator yang baik." },
  { "en": "Jika Tika memakai gaun, maka ia pergi ke pesta. Tika tidak pergi ke pesta. Kesimpulannya adalah...", "id": "Tika tidak memakai gaun." },
  { "en": "Lima pelari (A,B,C,D,E) berlomba. A finis sebelum B. C finis setelah E. D finis sebelum E. A finis setelah C. Siapa juaranya?", "id": "D (Urutan: D, E, C, A, B)." },
  { "en": "Jika hari ini hari Sabtu, maka saya tidak bekerja. Saya bekerja hari ini. Kesimpulannya...", "id": "Hari ini bukan hari Sabtu." },
  { "en": "Rata-rata nilai 5 siswa adalah 80. Jika satu siswa dengan nilai 100 keluar dari kelompok, berapa rata-rata nilai 4 siswa yang tersisa?", "id": "75." },
  { "en": "Berapakah nilai dari (2² x 2³) / 2⁴?", "id": "2." },
  { "en": "Sebuah toko memberikan diskon 50% + 20% untuk sebuah produk. Berapa total persentase diskon sebenarnya?", "id": "60%." },
  { "en": "Jika sebuah pekerjaan bisa diselesaikan A dalam 3 jam dan B dalam 6 jam. Jika mereka mulai bersama, jam berapa pekerjaan selesai jika dimulai pukul 08:00?", "id": "Pukul 10:00 (Selesai dalam 2 jam)." },
  { "en": "Berapa banyak bilangan antara 1 dan 100 yang habis dibagi 3 dan 5 sekaligus?", "id": "6 bilangan (15, 30, 45, 60, 75, 90)." },
  { "en": "Bagaimana Anda menyikapi anggota tim yang sering terlambat tetapi hasil kerjanya selalu luar biasa?", "id": "Saya akan berbicara dengannya secara pribadi, mengapresiasi kinerjanya, namun tetap mengingatkan pentingnya disiplin waktu sebagai bagian dari etos kerja dan respek terhadap tim." },
  { "en": "Apa yang Anda lakukan jika Anda merasa ada 'jarak' antara pimpinan dan bawahan di unit Anda?", "id": "Saya bisa mengusulkan kegiatan informal yang santai, seperti olahraga atau makan bersama, untuk mencairkan suasana dan membangun komunikasi yang lebih baik." },
  { "en": "Bagaimana Anda membedakan antara 'ambisius' dan 'haus kekuasaan'?", "id": "Ambisus berarti punya keinginan kuat untuk berprestasi dan maju. Haus kekuasaan berarti punya keinginan untuk mengontrol orang lain dan seringkali mengabaikan cara-cara etis." },
  { "en": "Apa arti 'menjaga kehormatan kesatuan' dalam kehidupan sehari-hari?", "id": "Artinya setiap tindakan, ucapan, dan bahkan unggahan di media sosial harus selalu mempertimbangkan dampaknya terhadap nama baik Polri secara keseluruhan." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus menyampaikan berita yang tidak populer atau mengecewakan kepada tim Anda?", "id": "Saya akan menyampaikannya secara jujur dan transparan, menjelaskan alasannya, memberikan ruang bagi mereka untuk bertanya, dan menunjukkan empati terhadap kekecewaan mereka." },
  { "en": "Apa yang akan Anda lakukan jika melihat ada perlakuan diskriminatif (misal berdasarkan gender atau suku) di lingkungan kerja?", "id": "Saya tidak akan diam saja. Saya akan menentangnya dengan cara yang elegan, menunjukkan melalui tindakan dan perkataan bahwa semua anggota tim harus dihargai berdasarkan kinerjanya, bukan latar belakangnya." },
  { "en": "Menurut Anda, apa tantangan terbesar bagi polisi dalam menjaga netralitas di era media sosial?", "id": "Menahan diri untuk tidak merespons provokasi atau menyukai/membagikan konten politik yang dapat dipersepsikan sebagai bentuk keberpihakan oleh publik." },
  { "en": "Jika Anda menemukan metode kerja yang lebih efektif daripada yang diajarkan saat pendidikan, apa yang Anda lakukan?", "id": "Saya akan tetap menghargai ilmu yang saya dapat, namun saya juga akan menerapkan metode baru tersebut jika terbukti lebih baik, dan mungkin membagikannya sebagai masukan konstruktif." },
  { "en": "Apa yang lebih penting dalam penegakan hukum: kepastian hukum atau kemanfaatan hukum?", "id": "Keduanya sangat penting dan idealnya sejalan. Namun jika terjadi benturan, penegakan hukum yang memberikan kemanfaatan terbesar bagi masyarakat luas seringkali menjadi prioritas, selama tidak menabrak prinsip kepastian hukum secara fundamental." },
  { "en": "Bagaimana Anda melatih mental Anda agar tidak mudah puas dengan pencapaian yang sudah diraih?", "id": "Dengan selalu menetapkan target yang lebih tinggi, melihat pencapaian sebagai batu loncatan (bukan tujuan akhir), dan terus belajar dari orang-orang yang lebih hebat." },
  { "en": "Lengkapi analogi: DETAK : JANTUNG = KEDIP : ...", "id": "MATA." },
  { "en": "Lengkapi analogi: BENDERA : TIANG = LUKISAN : ...", "id": "DINDING (atau BINGKAI)." },
  { "en": "Lengkapi analogi: MIKROFON : PEMBICARA = MEGAFON : ...", "id": "ORATOR." },
  { "en": "Lengkapi analogi: PENYUNTING : NASKAH = KURATOR : ...", "id": "KARYA SENI." },
  { "en": "Lengkapi analogi: PANAS : Mendidih = DINGIN : ...", "id": "Membeku." },
  { "en": "Lanjutkan deret angka: 1, 3, 4, 8, 7, 13, 10, ...", "id": "18 (pola dua larik: 1,4,7,10 [+3] dan 3,8,13,18 [+5])." },
  { "en": "Lanjutkan deret angka: 1, 2, 2, 4, 8, 32, ...", "id": "256 (pola: perkalian dua angka sebelumnya)." },
  { "en": "Lanjutkan deret huruf: A, D, F, I, K, N, ...", "id": "P (pola loncat: +3, +2, +3, +2, +3, +2)." },
  { "en": "Lanjutkan deret angka: 2, 7, 17, 37, 77, ...", "id": "157 (pola: x2 + 3)." },
  { "en": "Lanjutkan deret angka: 81, 27, 9, 3, 1, ...", "id": "1/3 (pola: dibagi 3)." },
  { "en": "Apa sinonim dari kata 'EKSAK'?", "id": "Pasti atau tepat." },
  { "en": "Apa sinonim dari kata 'FRIVOL'?", "id": "Sembrono atau tidak penting." },
  { "en": "Apa sinonim dari kata 'GUGATAN'?", "id": "Tuntutan." },
  { "en": "Apa antonim dari kata 'GEMBIRA'?", "id": "Sedih." },
  { "en": "Apa antonim dari kata 'HETEROGEN'?", "id": "Homogen." },
  { "en": "Berapakah 25% dari (20% dari 400)?", "id": "20." },
  { "en": "Sebuah bak mandi berbentuk kubus dengan sisi 1 meter. Jika diisi air dengan debit 50 liter per menit, berapa lama bak akan penuh?", "id": "20 menit (Volume = 1000 liter)." },
  { "en": "Jika 4x + 2y = 16 dan y = 2, berapa nilai x?", "id": "3." },
  { "en": "Berapa banyak diagonal yang dimiliki oleh sebuah segi delapan (oktagon)?", "id": "20." },
  { "en": "Rata-rata tinggi 10 siswa adalah 165 cm. Jika digabung dengan 5 siswa lain, rata-ratanya menjadi 160 cm. Berapa rata-rata tinggi 5 siswa tersebut?", "id": "150 cm." },
  { "en": "Bagaimana Anda menyikapi kegagalan tim yang disebabkan oleh kesalahan Anda sebagai pemimpin?", "id": "Saya akan secara terbuka mengakui kesalahan saya di hadapan tim dan atasan, mengambil tanggung jawab penuh, dan fokus pada langkah perbaikan untuk memulihkan kepercayaan mereka." },
  { "en": "Apa pentingnya memiliki 'sense of crisis' (kepekaan terhadap krisis)?", "id": "Sangat penting untuk bisa mengantisipasi potensi masalah sebelum membesar dan mempersiapkan langkah-langkah mitigasi secara proaktif." },
  { "en": "Bagaimana Anda mengatasi perbedaan generasi (generation gap) dalam berkomunikasi di tempat kerja?", "id": "Dengan saling menghormati, mencoba memahami gaya komunikasi masing-masing, dan mencari 'bahasa' atau media komunikasi yang bisa diterima oleh semua pihak." },
  { "en": "Apa yang akan Anda lakukan jika Anda merasa tugas yang diberikan melebihi kapasitas Anda?", "id": "Saya akan mencoba mengerjakannya semaksimal mungkin terlebih dahulu, namun jika benar-benar tidak sanggup, saya akan berkomunikasi dengan atasan mengenai kendala yang dihadapi dan meminta arahan atau bantuan." },
  { "en": "Menurut Anda, apakah loyalitas buta itu baik atau buruk? Mengapa?", "id": "Buruk. Loyalitas harus cerdas, artinya setia pada prinsip dan institusi yang benar, bukan pada individu secara membabi buta. Kita harus berani memberi masukan jika ada yang keliru." },
  { "en": "Jika ada 7 orang di sebuah ruangan dan setiap orang bersalaman dengan setiap orang lainnya tepat satu kali, berapa total jabat tangan yang terjadi?", "id": "21 jabat tangan." },
  { "en": "Semua A adalah B, dan beberapa C adalah A. Kesimpulannya?", "id": "Beberapa C adalah B." },
  { "en": "Lengkapi analogi: KAPAS : BENANG = GETAH : ...", "id": "KARET." },
  { "en": "Lanjutkan deret: 1, 4, 2, 5, 3, 6, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 4,5,6,...)." },
  { "en": "Apa sinonim dari 'MANDIRI'?", "id": "Berdiri sendiri atau independen." },
  { "en": "Apa antonim dari kata 'NISBI'?", "id": "Mutlak." },
  { "en": "Anda diberi pilihan: mendapatkan promosi dengan cara yang sedikit 'abu-abu' atau tetap di posisi sekarang dengan cara terhormat. Mana yang Anda pilih?", "id": "Saya akan memilih tetap di posisi sekarang dengan cara terhormat. Kehormatan dan integritas jauh lebih berharga daripada jabatan yang diperoleh dengan cara yang salah." },
  { "en": "Berapa jam yang dibutuhkan untuk menempuh 180 km dengan kecepatan rata-rata 40 km/jam?", "id": "4.5 jam." },
  { "en": "Apa arti 'negara hukum' bagi Anda sebagai penegak hukum?", "id": "Artinya segala tindakan, baik oleh pemerintah maupun warga negara, harus didasarkan pada aturan hukum yang berlaku, dan tidak ada seorang pun yang kebal hukum (equality before the law)." },
  { "en": "Jika Anda bisa mengubah satu hal dari diri Anda, apa itu?", "id": "Jawaban yang baik adalah yang menunjukkan kesadaran diri dan keinginan untuk menjadi lebih baik, misal: 'Saya ingin menjadi pendengar yang lebih sabar', atau 'Saya ingin lebih disiplin dalam mengatur waktu'." },
  { "en": "Anda mengetahui ada jalur 'khusus' atau 'titipan' dalam proses rekrutmen. Seorang teman baik meminta bantuan Anda untuk menggunakan jalur itu. Apa yang Anda katakan?", "id": "Saya akan menasihatinya bahwa cara terbaik untuk lulus adalah dengan persiapan yang matang dan mengikuti tes secara jujur. Menggunakan jalur khusus hanya akan merugikan dirinya sendiri dan institusi di masa depan." },
  { "en": "Bagaimana Anda menyikapi seorang rekan yang sering meminjam uang namun sulit untuk mengembalikannya?", "id": "Saya akan menolaknya dengan sopan untuk peminjaman selanjutnya dan menyarankannya untuk belajar mengelola keuangan dengan lebih baik, tanpa merusak hubungan pertemanan." },
  { "en": "Anda sedang terburu-buru mengejar target, namun ada prosedur administrasi yang panjang dan harus dilalui. Apa yang Anda lakukan?", "id": "Saya akan tetap mengikuti prosedur tersebut dengan sabar. Melewatkan prosedur demi kecepatan hanya akan menimbulkan masalah di kemudian hari." },
  { "en": "Apa yang Anda lakukan jika Anda merasa atasan Anda memberikan pujian yang tidak tulus atau hanya untuk basa-basi?", "id": "Saya akan menerima pujian tersebut dengan biasa saja, dan tetap fokus pada kualitas pekerjaan saya sebagai standar utama, bukan pada pujian." },
  { "en": "Menurut Anda, apa bedanya 'melindungi' masyarakat dengan 'memanjakan' masyarakat?", "id": "Melindungi berarti menegakkan aturan untuk keselamatan bersama. Memanjakan berarti membiarkan pelanggaran kecil demi menyenangkan seseorang, yang justru bisa berbahaya." },
  { "en": "Anda melihat seorang junior memiliki potensi besar tetapi sangat pemalu dan tidak percaya diri. Bagaimana Anda membantunya?", "id": "Saya akan memberinya tanggung jawab kecil secara bertahap, memberikan apresiasi atas setiap keberhasilannya, dan melibatkannya dalam diskusi tim untuk membangun rasa percaya dirinya." },
  { "en": "Bagaimana Anda menjelaskan kepada seorang teman bahwa Anda tidak bisa membantunya 'menyelesaikan' masalah hukum yang sedang dihadapinya?", "id": "Saya akan menjelaskan bahwa sebagai penegak hukum, saya harus netral dan tidak bisa mencampuri proses yang berjalan. Saya hanya bisa menyarankannya untuk mengikuti prosedur hukum yang benar." },
  { "en": "Apa arti 'disiplin' dalam penggunaan waktu pribadi Anda?", "id": "Artinya mampu menyeimbangkan antara waktu untuk istirahat, keluarga, dan pengembangan diri, sehingga saya bisa kembali bertugas dengan kondisi fisik dan mental yang prima." },
  { "en": "Anda merasa ada rekan yang sengaja menyebarkan informasi salah tentang Anda. Bagaimana Anda mengklarifikasinya tanpa menimbulkan konflik?", "id": "Saya tidak akan melabraknya. Saya akan mengklarifikasi informasi yang benar kepada orang-orang yang relevan atau kepada atasan dengan cara yang tenang dan didukung fakta." },
  { "en": "Apa yang lebih penting dalam sebuah tim: semua anggota memiliki keahlian yang sama atau keahlian yang saling melengkapi?", "id": "Keahlian yang saling melengkapi. Itu membuat tim menjadi lebih kuat karena setiap anggota bisa menutupi kekurangan anggota lainnya dan menangani berbagai jenis masalah." },
  { "en": "Lengkapi analogi: SUNGAI : JEMBATAN = JURANG : ...", "id": "TALI (atau JEMBATAN GANTUNG)." },
  { "en": "Lengkapi analogi: KEPOMPONG : KUPU-KUPU = LARVA : ...", "id": "NYAMUK (atau SERANGGA DEWASA)." },
  { "en": "Lengkapi analogi: BINTANG : LANGIT = PULAU : ...", "id": "LAUT." },
  { "en": "Lengkapi analogi: KEBISINGAN : SUARA = KESILAUAN : ...", "id": "CAHAYA." },
  { "en": "Lengkapi analogi: MEDALI : ATLET = IJAZAH : ...", "id": "MAHASISWA (atau PELAJAR)." },
  { "en": "Lengkapi analogi: KAKI : BERLARI = OTAK : ...", "id": "BERPIKIR." },
  { "en": "Lengkapi analogi: KOMPUTER : KEYBOARD = TELEVISI : ...", "id": "REMOTE." },
  { "en": "Lengkapi analogi: PAGI : SARAPAN = MALAM : ...", "id": "MAKAN MALAM." },
  { "en": "Lengkapi analogi: PENYAKIT : DOKTER = KERUSAKAN : ...", "id": "MONTIR." },
  { "en": "Lengkapi analogi: KERING : KERTAS = BASAH : ...", "id": "AIR." },
  { "en": "Lanjutkan deret angka: 2, 4, 16, 256, ...", "id": "65536 (pola: kuadrat dari angka sebelumnya)." },
  { "en": "Lanjutkan deret angka: 1, 5, 4, 8, 7, 11, 10, ...", "id": "14 (pola: +4, -1, +4, -1, diulang)." },
  { "en": "Lanjutkan deret huruf: A, Z, D, Y, G, X, ...", "id": "J (pola dua larik: A,D,G,J [+3] dan Z,Y,X mundur)." },
  { "en": "Lanjutkan deret angka: 1, 3, 11, 43, 171, ...", "id": "683 (pola: x4 - 1)." },
  { "en": "Lanjutkan deret angka: 100, 99, 96, 91, 84, ...", "id": "75 (pola: -1, -3, -5, -7, -9)." },
  { "en": "Lanjutkan deret angka: 3, 3, 6, 9, 21, 39, ...", "id": "69 (pola: jumlah 2 angka sebelumnya + angka sebelumnya)." },
  { "en": "Lanjutkan deret huruf: Z, Y, W, T, P, K, ...", "id": "E (pola mundur: -1, -2, -3, -4, -5, -6)." },
  { "en": "Lanjutkan deret angka: 2, 6, 8, 14, 22, 36, ...", "id": "58 (pola: deret Fibonacci)." },
  { "en": "Lanjutkan deret angka: 1, 10, 100, 1000, ...", "id": "10000 (pola: dikali 10)." },
  { "en": "Lanjutkan deret angka: 4, 6, 10, 16, 26, 42, ...", "id": "68 (pola: deret Fibonacci modifikasi)." },
  { "en": "Apa sinonim dari kata 'KONSISTEN'?", "id": "Taat asas atau ajek." },
  { "en": "Apa sinonim dari kata 'DINAMIS'?", "id": "Bergerak atau berubah-ubah." },
  { "en": "Apa sinonim dari kata 'EFEKTIF'?", "id": "Berhasil guna atau manjur." },
  { "en": "Apa sinonim dari kata 'KOLEKTIF'?", "id": "Bersama-sama." },
  { "en": "Apa sinonim dari kata 'POTENSI'?", "id": "Kemampuan terpendam." },
  { "en": "Apa antonim dari kata 'ABADI'?", "id": "Sementara atau fana." },
  { "en": "Apa antonim dari kata 'CEMERLANG'?", "id": "Buram atau redup." },
  { "en": "Apa antonim dari kata 'ELITIS'?", "id": "Merakyat." },
  { "en": "Apa antonim dari kata 'GUGUP'?", "id": "Tenang." },
  { "en": "Apa antonim dari kata 'HARMONI'?", "id": "Kekacauan atau disonansi." },
  { "en": "Jika Tono lebih gemuk dari Adi, dan Adi lebih kurus dari Budi. Siapa yang pasti tidak paling kurus?", "id": "Tono." },
  { "en": "Semua bunga di taman ini harum. Mawar ini tidak harum. Kesimpulannya adalah...", "id": "Mawar ini bukan dari taman ini." },
  { "en": "Jika saya belajar, maka saya bisa. Jika saya bisa, maka saya lulus. Kesimpulannya...", "id": "Jika saya belajar, maka saya lulus." },
  { "en": "Seorang pembohong berkata: 'Semua yang saya katakan adalah bohong'. Apakah pernyataan itu benar atau salah?", "id": "Paradoks (tidak bisa benar dan tidak bisa salah)." },
  { "en": "Jika 4 kucing makan 4 ikan dalam 4 menit. Berapa waktu yang dibutuhkan 1 kucing untuk makan 1 ikan?", "id": "4 menit." },
  { "en": "Sebuah mobil menempuh 2/5 perjalanan. Jika sisa perjalanannya adalah 60 km, berapa total jarak perjalanan?", "id": "100 km." },
  { "en": "Berapakah 12,5% dari 400?", "id": "50." },
  { "en": "Jika 2x + 3 = 3x - 2, berapakah nilai x?", "id": "5." },
  { "en": "Sebuah persegi panjang memiliki panjang 12 cm dan keliling 40 cm. Berapa luasnya?", "id": "96 cm²." },
  { "en": "Berapa banyak angka 0 pada bilangan satu miliar?", "id": "9." },
  { "en": "Bagaimana Anda menyikapi anggota yang sering memberikan ide-ide hebat tetapi malas dalam eksekusi?", "id": "Saya akan memujinya untuk idenya, lalu memasangkannya dengan anggota lain yang kuat dalam eksekusi, sehingga mereka bisa saling melengkapi sebagai sebuah tim." },
  { "en": "Apa yang Anda lakukan jika Anda merasa beban kerja Anda jauh lebih berat dibandingkan rekan-rekan lain di level yang sama?", "id": "Saya akan fokus menyelesaikan tugas saya terlebih dahulu, kemudian mencari waktu yang baik untuk berdiskusi dengan atasan mengenai distribusi beban kerja secara profesional, bukan dengan mengeluh." },
  { "en": "Bagaimana Anda membedakan antara 'percaya' dengan 'naif'?", "id": "Percaya didasarkan pada rekam jejak dan bukti integritas. Naif adalah percaya begitu saja tanpa melakukan verifikasi atau mempertimbangkan kemungkinan buruk." },
  { "en": "Apa arti 'menjadi panutan' bagi adik-adik atau junior Anda di kepolisian?", "id": "Artinya menunjukkan standar kerja dan perilaku yang tinggi, sehingga mereka memiliki contoh nyata tentang bagaimana menjadi polisi yang profesional dan berintegritas." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus menegakkan aturan yang tidak populer di masyarakat (misal: penertiban PKL)?", "id": "Dengan melakukan sosialisasi terlebih dahulu, menggunakan pendekatan yang humanis, dan menjelaskan bahwa tujuan akhirnya adalah untuk ketertiban dan kebaikan bersama." },
  { "en": "Apa yang akan Anda lakukan jika Anda merasa kehilangan kepercayaan dari atasan Anda?", "id": "Saya akan melakukan introspeksi untuk mencari tahu penyebabnya, kemudian proaktif meminta umpan balik dan menunjukkan melalui kinerja dan sikap bahwa saya layak untuk dipercaya kembali." },
  { "en": "Menurut Anda, apa dampak jangka panjang dari seorang polisi yang sering berbohong, bahkan untuk hal-hal kecil?", "id": "Itu akan mengikis integritasnya secara perlahan, merusak kredibilitasnya di mata rekan dan atasan, dan pada akhirnya bisa membawanya pada pelanggaran yang lebih besar." },
  { "en": "Jika Anda harus memilih, Anda lebih suka bekerja sendiri atau dalam tim? Mengapa?", "id": "Saya siap untuk keduanya. Bekerja sendiri melatih kemandirian, sementara bekerja dalam tim mengasah kemampuan kolaborasi dan menghasilkan solusi yang lebih komprehensif." },
  { "en": "Apa yang lebih sulit: mengakui kesalahan atau memaafkan kesalahan orang lain?", "id": "Keduanya membutuhkan kerendahan hati. Namun, seringkali mengakui kesalahan sendiri lebih sulit karena melibatkan ego pribadi." },
  { "en": "Bagaimana Anda menjaga semangat pengabdian tetap menyala setelah bertahun-tahun bertugas?", "id": "Dengan secara rutin mengingat kembali tujuan awal saya bergabung, melihat dampak positif dari pekerjaan saya pada masyarakat, dan terus mencari cara baru untuk berkontribusi." },
  { "en": "Lengkapi analogi: JARUM : TAJAM = BATU : ...", "id": "KERAS." },
  { "en": "Lengkapi analogi: KENDARAAN : BENSIN = TUBUH : ...", "id": "ENERGI (atau MAKANAN)." },
  { "en": "Lengkapi analogi: PEMIMPIN : ANGGOTA = JENDERAL : ...", "id": "PRAJURIT." },
  { "en": "Lengkapi analogi: CACING : TANAH = IKAN : ...", "id": "AIR." },
  { "en": "Lengkapi analogi: PANAS : MENGUAP = DINGIN : ...", "id": "MEMBEKU." },
  { "en": "Lanjutkan deret angka: 1, 2, 6, 15, 31, 56, ...", "id": "92 (pola: +1, +4, +9, +16, +25, +36 -> bilangan kuadrat)." },
  { "en": "Lanjutkan deret angka: 2, 3, 4, 6, 6, 9, 8, ...", "id": "12 (pola dua larik: 2,4,6,8,... dan 3,6,9,12,...)." },
  { "en": "Lanjutkan deret huruf: A, C, B, D, C, E, D, ...", "id": "F (pola: +2, -1, +2, -1, diulang)." },
  { "en": "Lanjutkan deret angka: 5, 6, 8, 11, 15, ...", "id": "20 (pola: +1, +2, +3, +4, +5)." },
  { "en": "Lanjutkan deret angka: 1, 1, 2, 2, 4, 3, 8, ...", "id": "4 (pola dua larik: 1,2,4,8 [x2] dan 1,2,3,4)." },
  { "en": "Apa sinonim dari kata 'AKSELERASI'?", "id": "Percepatan." },
  { "en": "Apa sinonim dari kata 'BRUTAL'?", "id": "Kejam atau sadis." },
  { "en": "Apa sinonim dari kata 'CITRA'?", "id": "Gambaran atau kesan." },
  { "en": "Apa antonim dari kata 'GAGAL'?", "id": "Berhasil." },
  { "en": "Apa antonim dari kata 'KUAT'?", "id": "Lemah." },
  { "en": "Berapakah 2/5 dari 1 jam?", "id": "24 menit." },
  { "en": "Sebuah segitiga sama sisi memiliki keliling 45 cm. Berapa panjang sisinya?", "id": "15 cm." },
  { "en": "Jika 4 buku harganya Rp 50.000, berapa harga 10 buku?", "id": "Rp 125.000." },
  { "en": "Berapakah 150% dari 80?", "id": "120." },
  { "en": "Jika x = 1/2 dan y = 0.75, mana yang lebih besar?", "id": "y." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda harus bekerja sama dengan mantan 'rival' Anda dalam sebuah tim?", "id": "Saya akan mengesampingkan rivalitas masa lalu, bersikap profesional, dan fokus pada tujuan bersama tim. Keberhasilan tim lebih penting dari ego pribadi." },
  { "en": "Apa arti 'proporsionalitas' dalam memberikan sanksi atau hukuman?", "id": "Artinya berat ringannya hukuman harus seimbang dengan berat ringannya pelanggaran yang dilakukan, tidak kurang dan tidak lebih." },
  { "en": "Bagaimana Anda membangun kepercayaan dengan informan di lapangan?", "id": "Dengan menjaga kerahasiaan identitasnya, memberikan imbalan yang sesuai (jika ada), dan selalu menepati janji yang saya buat kepadanya." },
  { "en": "Apa yang Anda lakukan jika Anda merasa ada kebijakan dari pusat yang tidak cocok untuk diterapkan di daerah Anda?", "id": "Saya akan tetap melaksanakannya sebagai perintah, namun secara bersamaan membuat laporan atau masukan kepada pimpinan mengenai kendala dan saran penyesuaian untuk konteks lokal." },
  { "en": "Menurut Anda, apa satu kebiasaan buruk yang paling harus dihindari oleh seorang anggota Polri?", "id": "Sikap arogan atau merasa paling berkuasa. Sikap ini adalah akar dari banyak pelanggaran dan merusak hubungan dengan masyarakat." },
  { "en": "Jika sebuah pertandingan selalu memiliki...", "id": "Lawan (atau Kompetitor)." },
  { "en": "Semua A adalah B. Sebagian C bukan B. Kesimpulannya?", "id": "Sebagian C bukan A." },
  { "en": "Lengkapi analogi: KAYU : MEJA = KULIT : ...", "id": "SEPATU." },
  { "en": "Lanjutkan deret: 1, 5, 2, 10, 3, 15, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 5,10,15,...)." },
  { "en": "Apa sinonim dari 'DEKADENSI'?", "id": "Kemerosotan moral." },
  { "en": "Apa antonim dari kata 'KRUSIAL'?", "id": "Tidak penting atau sepele." },
  { "en": "Anda melihat seorang rekan memalsukan tanda tangan dalam sebuah laporan administrasi. Apa tindakan Anda?", "id": "Saya akan menegurnya secara langsung dan memintanya untuk tidak mengulanginya karena itu adalah bentuk pemalsuan dan pelanggaran integritas. Jika terulang, saya akan mempertimbangkan untuk melapor." },
  { "en": "Berapa banyak sisi pada sebuah balok?", "id": "6." },
  { "en": "Apa komitmen Anda untuk terus menjaga kebugaran fisik selama berkarir di Kepolisian?", "id": "Saya berkomitmen untuk meluangkan waktu berolahraga secara rutin minimal 3 kali seminggu dan menjaga pola hidup sehat, karena saya sadar fisik yang prima adalah modal utama dalam pelayanan." },
  { "en": "Jika Anda harus menggambarkan sosok polisi ideal dalam tiga kata, apa saja kata-kata itu?", "id": "Tegas, Humanis, dan Terpercaya." },
  { "en": "Anda harus memilih antara menangkap pencuri singkong yang kelaparan (menegakkan hukum) atau melepaskannya karena kemanusiaan. Apa pilihan dan justifikasi moral Anda?", "id": "Saya akan mengamankannya terlebih dahulu untuk mencegah tindakan main hakim sendiri, lalu menerapkan keadilan restoratif dengan memfasilitasi mediasi antara pelaku dan korban, sambil mencari solusi untuk masalah kelaparannya. Hukum ditegakkan, kemanusiaan diutamakan." },
  { "en": "Setelah bertahun-tahun bertugas, apa perubahan paling signifikan pada cara pandang Anda terhadap 'kebaikan' dan 'kejahatan'?", "id": "Saya belajar bahwa batas antara keduanya seringkali abu-abu. Banyak kejahatan lahir dari keputusasaan. Saya tetap tegas pada hukum, tapi lebih berempati pada latar belakang manusia di baliknya dan tidak mudah menghakimi." },
  { "en": "Pimpinan memberikan arahan yang bersifat politis dan dapat mengganggu netralitas. Langkah diplomatis apa yang bisa Anda ambil sebagai perwira menengah?", "id": "Saya tidak akan menentang secara terbuka. Saya akan menyajikan analisis intelijen atau laporan situasi lapangan yang secara objektif menunjukkan potensi risiko keamanan atau penurunan kepercayaan publik jika arahan tersebut dijalankan." },
  { "en": "Anda melihat banyak pelanggaran kecil terjadi berulang kali karena sistem yang rumit. Langkah sistemik apa yang Anda usulkan untuk memperbaiki 'akar masalah'?", "id": "Saya akan mengumpulkan data tentang jenis pelanggaran dan titik kesulitan dalam sistem, lalu membuat usulan penyederhanaan prosedur atau digitalisasi alur kerja, dan menyampaikannya kepada atasan sebagai solusi jangka panjang." },
  { "en": "Apa perbedaan antara 'memiliki wibawa' dan 'menjadi sosok yang ditakuti'?", "id": "Wibawa lahir dari integritas, kompetensi, dan kebijaksanaan, sehingga orang menghormati secara sukarela. Ditakuti lahir dari ancaman dan kekuasaan, di mana orang hanya patuh karena terpaksa." },
  { "en": "Anda melihat seorang rekan menunjukkan gejala 'post-traumatic stress' (PTSD) setelah insiden fatal. Apa tindakan Anda sebagai sesama rekan?", "id": "Saya akan menjadi pendengar yang baik tanpa menghakimi, mengajaknya melakukan kegiatan yang menenangkan, dan dengan sangat hati-hati menyarankannya untuk menemui unit psikologi dinas, karena ini adalah luka yang butuh penanganan profesional." },
  { "en": "Bagaimana Anda menjelaskan kepada anak Anda jika Anda terpaksa berbohong demi sebuah operasi penyamaran?", "id": "Saya akan menjelaskan dengan bahasa sederhana bahwa dalam pekerjaan ayah/ibu, kadang harus berpura-pura menjadi orang lain untuk menangkap penjahat, seperti dalam sebuah film, dan itu adalah bagian dari tugas untuk menjaga keamanan." },
  { "en": "Menurut Anda, apa 'biaya' terbesar dari sebuah kebohongan, sekecil apapun, bagi seorang penegak hukum?", "id": "Biaya terbesarnya adalah erosi kepercayaan. Satu kebohongan bisa merusak kredibilitas yang dibangun bertahun-tahun, baik pada level pribadi maupun institusi." },
  { "en": "Anda dipaksa oleh situasi untuk bekerja sama secara intensif dengan unit lain yang memiliki 'budaya kerja' sangat berbeda dan sering bergesekan. Bagaimana Anda menjadi jembatan?", "id": "Saya akan proaktif mencari kesamaan tujuan, membangun hubungan personal yang baik dengan anggota kunci di unit lain, dan fokus pada komunikasi yang jelas untuk menghindari salah paham." },
  { "en": "Apa yang lebih sulit: memaafkan orang lain yang merugikan Anda, atau memaafkan diri sendiri setelah melakukan kesalahan fatal?", "id": "Seringkali memaafkan diri sendiri jauh lebih sulit. Itu membutuhkan penerimaan yang besar atas ketidaksempurnaan diri dan komitmen kuat untuk belajar dari kesalahan tersebut tanpa terus menerus menyalahkan diri." },
  { "en": "Lengkapi analogi: OKSIGEN : KEHIDUPAN = HUKUM : ...", "id": "KETERATURAN." },
  { "en": "Lengkapi analogi: PUZZLE : POTONGAN = ORKESTRA : ...", "id": "INSTRUMEN." },
  { "en": "Lengkapi analogi: INGATAN : OTAK = DATABASE : ...", "id": "SERVER (atau KOMPUTER)." },
  { "en": "Lengkapi analogi: PENYESALAN : MASA LALU = KEKHAWATIRAN : ...", "id": "MASA DEPAN." },
  { "en": "Lengkapi analogi: SIDANG : PUTUSAN = PERLOMBAAN : ...", "id": "JUARA." },
  { "en": "Lengkapi analogi: KAPAL : PELABUHAN = MOBIL : ...", "id": "TERMINAL (atau GARASI)." },
  { "en": "Lengkapi analogi: PENA : TINTA = LAMPU : ...", "id": "LISTRIK." },
  { "en": "Lengkapi analogi: AKSIOMA : TERBUKTI = HIPOTESIS : ...", "id": "PERLU DIBUKTIKAN." },
  { "en": "Lengkapi analogi: BUTIR : PASIR = TETES : ...", "id": "AIR." },
  { "en": "Lengkapi analogi: AKTOR : SUTRADARA = PRAJURIT : ...", "id": "KOMANDAN." },
  { "en": "Lanjutkan deret angka: 2, 5, 12, 27, 58, ...", "id": "121 (pola: x2 + 1, x2 + 2, x2 + 3, x2 + 4, x2 + 5)." },
  { "en": "Lanjutkan deret angka: 1, 1, 2, 4, 7, 13, 24, ...", "id": "44 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Lanjutkan deret huruf: A, D, G, K, N, R, U, ...", "id": "X (pola loncat: +3, +3, +4, +3, +4, +3, +3... Pola kurang jelas. Alternatif: A,D,G,J,M...)." },
  { "en": "Lanjutkan deret angka: 2, 3, 5, 6, 8, 9, 11, ...", "id": "12 (pola dua larik: 2,5,8,11 [+3] dan 3,6,9,12 [+3])." },
  { "en": "Lanjutkan deret angka: 5, 7, 10, 15, 22, 33, ...", "id": "48 (pola: +2, +3, +5, +7, +11, +15... pola penambahan +1,+2,+2,+4... kurang jelas. Alternatif: +2,+3,+5,+7,+11,+13 [bil prima] -> 33+13=46)." },
  { "en": "Lanjutkan deret angka: 1/16, 1/8, 1/4, 1/2, ...", "id": "1 (pola: dikali 2)." },
  { "en": "Lanjutkan deret huruf: A, B, D, G, L, ...", "id": "S (pola loncat: +1, +2, +3, +5, +7 -> bukan. Coba lagi +1,+2,+3,+4... G+4=K. Jadi A,B,D,G,K,P,V. Pola soal kurang jelas)." },
  { "en": "Lanjutkan deret angka: 1, 4, 3, 8, 5, 12, 7, ...", "id": "16 (pola dua larik: 1,3,5,7,... dan 4,8,12,16,...)." },
  { "en": "Lanjutkan deret angka: 3, 6, 10, 15, 21, 28, ...", "id": "36 (pola: +3, +4, +5, +6, +7, +8)." },
  { "en": "Lanjutkan deret angka: 10, 1, 8, 3, 6, 5, ...", "id": "4 (pola dua larik: 10,8,6,4,... dan 1,3,5,...)." },
  { "en": "Apa sinonim dari kata 'ANULIR'?", "id": "Membatalkan." },
  { "en": "Apa sinonim dari kata 'BABUT'?", "id": "Permadani." },
  { "en": "Apa sinonim dari kata 'CANDA'?", "id": "Kelakar atau gurauan." },
  { "en": "Apa sinonim dari kata 'DAUR'?", "id": "Siklus." },
  { "en": "Apa sinonim dari kata 'EKSODUS'?", "id": "Perpindahan besar-besaran." },
  { "en": "Apa antonim dari kata 'INTELEGENSIA'?", "id": "Kebodohan." },
  { "en": "Apa antonim dari kata 'JANGGAL'?", "id": "Wajar atau lazim." },
  { "en": "Apa antonim dari kata 'KALEIDOSKOP'?", "id": "Seragam." },
  { "en": "Apa antonim dari kata 'LANCUNG'?", "id": "Asli." },
  { "en": "Apa antonim dari kata 'MAESTRO'?", "id": "Amatir." },
  { "en": "Tiga bersaudara (A, B, C) memiliki hobi berbeda (renang, lari, sepeda). A tidak hobi lari. C hobi sepeda. Hobi B adalah...", "id": "Lari." },
  { "en": "Jika pernyataan 'Tidak ada siswa yang malas' adalah salah, maka kesimpulan yang benar adalah...", "id": "Beberapa siswa malas." },
  { "en": "Jika sebuah berita selalu mengandung...", "id": "Informasi." },
  { "en": "Semua planet berbentuk bulat. Bumi adalah planet. Mars berbentuk bulat. Kesimpulan mana yang pasti benar?", "id": "Bumi berbentuk bulat." },
  { "en": "Jika kemarin lusa adalah hari Selasa, maka 2 hari yang akan datang adalah hari...", "id": "Sabtu." },
  { "en": "Berapakah akar pangkat tiga dari 1728?", "id": "12." },
  { "en": "Sebuah mobil dijual dengan harga Rp 120.000.000, penjual mengalami kerugian 20%. Berapa harga beli mobil tersebut?", "id": "Rp 150.000.000." },
  { "en": "Jika a*b = a + b + ab, berapakah nilai dari 3*4?", "id": "19 (3 + 4 + 3x4)." },
  { "en": "Berapa banyak cara menyusun kata 'POLISI'?", "id": "720 (6! = 6x5x4x3x2x1)." },
  { "en": "Sebuah roda berputar 100 kali menempuh jarak 440 meter. Berapa diameter roda tersebut? (π=22/7)", "id": "1.4 meter." },
  { "en": "Bagaimana Anda menyikapi seorang rekan yang sangat kompeten tetapi memiliki masalah 'attitude' (sikap) yang buruk?", "id": "Saya akan tetap menjaga hubungan kerja profesional, namun akan memberi masukan secara pribadi jika sikapnya sudah mengganggu kinerja tim atau melanggar etika." },
  { "en": "Apa yang lebih penting dalam sebuah investigasi: kecepatan mengungkap atau ketepatan bukti?", "id": "Ketepatan bukti. Kecepatan penting, tetapi tanpa bukti yang akurat dan sah secara hukum, pengungkapan kasus menjadi sia-sia dan tidak adil." },
  { "en": "Bagaimana Anda membedakan antara 'instruksi' dan 'delegasi'?", "id": "Instruksi adalah perintah untuk melakukan tugas spesifik dengan cara tertentu. Delegasi adalah memberikan wewenang dan tanggung jawab kepada orang lain untuk menyelesaikan suatu tujuan, dengan cara yang bisa ia tentukan sendiri." },
  { "en": "Apa arti 'sumpah jabatan' bagi Anda, di luar seremoni formalnya?", "id": "Itu adalah kontrak moral pribadi saya dengan Tuhan, Negara, dan masyarakat, yang menjadi pengingat dan benteng integritas saya setiap saat, baik dalam dinas maupun di luar." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus menegakkan hukum pada teman masa kecil Anda?", "id": "Saya akan memisahkan hubungan pribadi dengan tugas profesional. Proses hukum harus berjalan sebagaimana mestinya, dan setelah itu saya bisa memberikan dukungan sebagai seorang teman." },
  { "en": "Apa yang akan Anda lakukan untuk membangun 'resiliensi' atau daya tahan mental?", "id": "Dengan melatih pola pikir yang fleksibel, menerima bahwa kesulitan adalah bagian dari hidup, menjaga hubungan sosial yang sehat, dan memiliki tujuan hidup yang lebih besar dari masalah sehari-hari." },
  { "en": "Menurut Anda, apakah penggunaan kekerasan oleh polisi dalam film memberikan citra yang baik atau buruk?", "id": "Cenderung buruk jika digambarkan secara berlebihan dan tanpa konteks, karena dapat membentuk persepsi publik bahwa kekerasan adalah cara kerja utama polisi, padahal itu adalah alternatif terakhir." },
  { "en": "Jika Anda harus memilih, Anda lebih ingin dikenal sebagai polisi yang 'pintar' atau polisi yang 'bijaksana'?", "id": "Bijaksana. Pintar hanya soal pengetahuan, tetapi bijaksana melibatkan kemampuan menggunakan pengetahuan tersebut dengan empati, keadilan, dan pertimbangan yang matang." },
  { "en": "Apa yang lebih berbahaya bagi institusi: satu pelanggaran besar yang viral atau ribuan pelanggaran kecil yang tersembunyi?", "id": "Ribuan pelanggaran kecil yang tersembunyi. Itu seperti kanker yang menggerogoti integritas dan kepercayaan dari dalam secara sistemik dan perlahan." },
  { "en": "Bagaimana Anda menanggapi keberhasilan sebuah unit lain yang membuat unit Anda terlihat tertinggal?", "id": "Saya akan menjadikannya sebagai motivasi dan bahan studi banding. Saya akan pelajari apa yang membuat mereka berhasil dan mencoba mengadaptasi hal-hal positif tersebut di unit saya." },
  { "en": "Lengkapi analogi: UANG : EKONOMI = CUACA : ...", "id": "METEOROLOGI." },
  { "en": "Lengkapi analogi: DARAT : MOBIL = UDARA : ...", "id": "PESAWAT." },
  { "en": "Lengkapi analogi: PENA : PUJANGGA = PAHAT : ...", "id": "PEMATUNG." },
  { "en": "Lengkapi analogi: KAKU : BESI = LENTUR : ...", "id": "KARET." },
  { "en": "Lengkapi analogi: LAPAR : LEMAS = TIDUR : ...", "id": "SEGAR." },
  { "en": "Lanjutkan deret angka: 2, 4, 3, 6, 5, 10, 9, ...", "id": "18 (pola: x2, -1, diulang)." },
  { "en": "Lanjutkan deret angka: 1, 10, 2, 9, 3, 8, 4, ...", "id": "7 (pola dua larik: 1,2,3,4,... dan 10,9,8,7,...)." },
  { "en": "Lanjutkan deret huruf: A, B, C, F, E, D, G, H, I, ...", "id": "L (pola: 3 maju, 3 mundur, 3 maju, 3 mundur)." },
  { "en": "Lanjutkan deret angka: 3, 4, 8, 17, 33, 58, ...", "id": "94 (pola penambahan bilangan kuadrat: +1,+4,+9,+16,+25,+36)." },
  { "en": "Lanjutkan deret angka: 1, 5, 9, 13, 17, ...", "id": "21 (pola: ditambah 4)." },
  { "en": "Apa sinonim dari kata 'GAMBLANG'?", "id": "Jelas atau terang." },
  { "en": "Apa sinonim dari kata 'HEDONISME'?", "id": "Paham kesenangan." },
  { "en": "Apa sinonim dari kata 'INSINUASI'?", "id": "Sindiran." },
  { "en": "Apa antonim dari kata 'PANDIR'?", "id": "Cerdas atau pandai." },
  { "en": "Apa antonim dari kata 'PEJAL'?", "id": "Rongga atau berongga." },
  { "en": "Berapakah 1/8 + 3/4 ?", "id": "7/8." },
  { "en": "Sebuah segitiga memiliki alas 10 cm dan tinggi 12 cm. Berapa luasnya?", "id": "60 cm²." },
  { "en": "Jika harga 3 kg jeruk adalah Rp 45.000, berapa harga 5 kg jeruk?", "id": "Rp 75.000." },
  { "en": "Jika x adalah 20% dari 50, dan y adalah 50% dari 20, maka...", "id": "x = y." },
  { "en": "Berapa banyak detik dalam 1.5 jam?", "id": "5400 detik." },
  { "en": "Bagaimana Anda memotivasi diri sendiri di hari yang sangat buruk dan penuh kegagalan?", "id": "Saya akan mengingatkan diri sendiri bahwa hari esok adalah kesempatan baru. Saya akan fokus pada satu hal kecil yang bisa saya perbaiki atau selesaikan untuk mengembalikan rasa percaya diri." },
  { "en": "Apa arti 'netralitas' bagi seorang penyidik?", "id": "Artinya tidak memihak kepada pelapor maupun terlapor, dan sepenuhnya mendasarkan proses penyidikan pada alat bukti yang sah tanpa asumsi atau prasangka." },
  { "en": "Bagaimana Anda menjelaskan kepada atasan bahwa sebuah perintah tidak mungkin dilaksanakan karena keterbatasan sumber daya?", "id": "Saya akan menyajikan data mengenai sumber daya yang tersedia dan kebutuhan riil untuk melaksanakan perintah tersebut, lalu menawarkan alternatif atau skala prioritas yang lebih realistis." },
  { "en": "Apa yang akan Anda lakukan jika melihat rekan Anda mengalami kesulitan finansial yang parah dan berpotensi melakukan penyimpangan?", "id": "Saya akan mendekatinya sebagai teman, menawarkan bantuan semampu saya (bukan uang), dan menyarankannya untuk berkonsultasi dengan unit pembinaan atau mencari solusi yang halal." },
  { "en": "Menurut Anda, apa satu keterampilan non-teknis (soft skill) yang paling menentukan keberhasilan seorang polisi?", "id": "Kemampuan komunikasi. Baik dalam menenangkan massa, menginterogasi tersangka, melapor ke atasan, maupun bekerja sama dalam tim, komunikasi adalah kuncinya." },
  { "en": "Jika di sebuah laci ada 10 kaos kaki hitam dan 10 kaos kaki putih, berapa kaos kaki minimal yang harus diambil agar PASTI mendapatkan sepasang kaos kaki dengan warna yang sama?", "id": "3 kaos kaki." },
  { "en": "Semua A yang merupakan B adalah C. X adalah A tetapi bukan C. Kesimpulannya?", "id": "X bukan B." },
  { "en": "Lengkapi analogi: CERITA : ALUR = LAGU : ...", "id": "IRAMA." },
  { "en": "Lanjutkan deret: 1, 1, 2, 3, 5, 8, ...", "id": "13 (pola: deret Fibonacci)." },
  { "en": "Apa sinonim dari kata 'RABAT'?", "id": "Potongan harga atau diskon." },
  { "en": "Apa antonim dari kata 'PREDIKSI'?", "id": "Fakta atau realita." },
  { "en": "Anda diberikan sebuah kasus yang 'sulit' dan tidak ada seorang pun yang mau menanganinya. Apa sikap Anda?", "id": "Saya akan menerimanya sebagai tantangan dan sebuah kehormatan. Ini adalah kesempatan bagi saya untuk belajar banyak dan membuktikan kemampuan saya." },
  { "en": "Berapa 1/5 dari 25%?", "id": "5% atau 0.05." },
  { "en": "Bagaimana Anda mendefinisikan 'karakter'?", "id": "Karakter adalah apa yang Anda lakukan ketika tidak ada seorang pun yang melihat. Itu adalah cerminan sejati dari integritas dan nilai-nilai yang Anda anut." },
  { "en": "Jika Anda harus memilih satu nilai dari 'Tri Brata' yang paling fundamental, mana yang Anda pilih dan mengapa?", "id": "Jawaban bisa bervariasi, tapi yang baik adalah yang disertai justifikasi kuat, misal: 'Berbakti kepada nusa dan bangsa', karena itu adalah landasan dari semua pengabdian lainnya." },
  { "en": "Anda mengetahui ada seorang pimpinan yang sangat dihormati akan pensiun, dan beberapa rekan berinisiatif mengumpulkan 'uang terima kasih' yang jumlahnya besar. Apa sikap Anda?", "id": "Saya akan menghargai niat baik rekan-rekan, namun saya akan menolak untuk berpartisipasi. Memberikan hadiah dalam jumlah besar dapat dikategorikan sebagai gratifikasi, dan lebih baik menunjukkan rasa terima kasih melalui acara perpisahan yang tulus dan tidak mewah." },
  { "en": "Bagaimana Anda menanggapi argumen bahwa beberapa 'aturan tidak tertulis' atau 'tradisi' di kepolisian lebih penting daripada aturan formal?", "id": "Saya berpendapat bahwa aturan formal (hukum dan prosedur) adalah landasan tertinggi. Tradisi yang baik dan mendukung profesionalisme bisa dipertahankan, tetapi jika bertentangan dengan aturan formal, maka aturan formallah yang harus menjadi pedoman." },
  { "en": "Anda dihadapkan pada situasi di mana menegakkan hukum secara kaku akan menyebabkan ketidakadilan yang lebih besar bagi orang miskin. Bagaimana Anda menavigasi dilema ini?", "id": "Saya akan mencari diskresi kepolisian yang memungkinkan. Saya akan menegakkan substansi hukumnya, namun dengan cara yang paling manusiawi dan mempertimbangkan dampak sosialnya, mungkin dengan berkoordinasi dengan dinas sosial atau lembaga terkait." },
  { "en": "Apa perbedaan antara 'menjadi polisi yang baik' dan 'menjadi orang baik yang kebetulan seorang polisi'?", "id": "Menjadi polisi yang baik berarti profesional dalam tugas. Menjadi orang baik yang seorang polisi berarti nilai-nilai kemanusiaan dan integritas pribadinya menjadi fondasi dari profesionalismenya, sehingga tindakannya tidak hanya benar secara hukum, tapi juga baik secara moral." },
  { "en": "Menurut Anda, apa 'musuh' terbesar bagi seorang polisi: penjahat di luar sana, atau godaan dan kelemahan di dalam dirinya sendiri?", "id": "Musuh terbesar adalah godaan dan kelemahan di dalam diri sendiri. Penjahat bisa ditangkap, tetapi kelemahan internal seperti keserakahan, arogansi, atau rasa takut, jika tidak dikendalikan, akan menghancurkan seorang polisi dari dalam." },
  { "en": "Anda melihat seorang rekan yang sangat berprestasi mulai menunjukkan tanda-tanda kelelahan mental (burnout) yang parah. Apa intervensi yang bisa Anda lakukan?", "id": "Saya akan mendekatinya bukan sebagai rekan kerja, tapi sebagai teman. Saya akan mengajaknya melakukan aktivitas di luar pekerjaan untuk me-refresh pikirannya, dan dengan sabar mendorongnya untuk mengambil cuti atau mencari bantuan profesional." },
  { "en": "Bagaimana Anda menjelaskan kepada masyarakat awam tentang konsep 'asas praduga tak bersalah'?", "id": "Saya akan menjelaskannya dengan analogi sederhana: 'Setiap orang harus dianggap tidak bersalah sampai pengadilan membuktikan sebaliknya. Tugas kami adalah mengumpulkan bukti, bukan menghakimi. Ini untuk melindungi agar orang yang tidak bersalah tidak dihukum'." },
  { "en": "Jika Anda harus memilih antara menyelamatkan satu orang yang sangat penting (misal: seorang ilmuwan kunci) atau lima orang biasa dalam sebuah krisis, apa keputusan Anda dan mengapa?", "id": "Sebagai polisi, setiap nyawa berharga sama. Saya akan fokus pada upaya penyelamatan yang paling memungkinkan untuk menyelamatkan jumlah orang terbanyak tanpa memandang status mereka. Prinsip utilitarianisme dalam situasi darurat." },
  { "en": "Apa yang Anda lakukan jika Anda menyadari bahwa Anda tidak lagi merasakan 'semangat' atau 'panggilan jiwa' dalam pekerjaan ini?", "id": "Saya akan melakukan refleksi mendalam untuk mencari tahu akarnya. Saya akan mencoba mencari rotasi tugas atau tantangan baru, dan jika memang 'panggilan' itu hilang, saya akan mempertimbangkan untuk mengabdi dengan cara lain daripada bekerja setengah hati." },
  { "en": "Bagaimana Anda menyeimbangkan antara kebutuhan untuk menjadi 'tegas' dan risiko dicap 'tidak humanis'?", "id": "Kuncinya ada pada komunikasi dan niat. Ketegasan saya harus selalu terlihat bertujuan untuk melindungi atau menegakkan aturan demi kebaikan bersama, bukan untuk menunjukkan kekuasaan. Saya akan tegas pada pelanggaran, tapi tetap humanis pada orangnya." },
  { "en": "Lengkapi analogi: PIKIRAN : IDE = LAHAN : ...", "id": "TANAMAN (atau PADI)." },
  { "en": "Lengkapi analogi: JANJI : UTANG = OPINI : ...", "id": "ARGUMEN." },
  { "en": "Lengkapi analogi: PETA : LOKASI = KAMUS : ...", "id": "DEFINISI." },
  { "en": "Lengkapi analogi: KESALAHAN : BELAJAR = LUKA : ...", "id": "SEMBUH." },
  { "en": "Lengkapi analogi: AIR : MENGHILANGKAN HAUS = API : ...", "id": "MENGHANGATKAN." },
  { "en": "Lengkapi analogi: BUKU : PENGETAHUAN = OLAHRAGA : ...", "id": "KESEHATAN." },
  { "en": "Lengkapi analogi: JANGKRIK : SUARA = KUNANG-KUNANG : ...", "id": "CAHAYA." },
  { "en": "Lengkapi analogi: OTOT : KEKUATAN = OTAK : ...", "id": "KECERDASAN." },
  { "en": "Lengkapi analogi: MATA : FOTOGRAFI = TELINGA : ...", "id": "AUDIO." },
  { "en": "Lengkapi analogi: KOMPAS : PELAUT = PETA : ...", "id": "PENJELAJAH." },
  { "en": "Lanjutkan deret angka: 2, 3, 5, 7, 11, 13, 17, ...", "id": "19 (pola: deret bilangan prima)." },
  { "en": "Lanjutkan deret angka: 1, 4, 13, 40, 121, ...", "id": "364 (pola: x3 + 1)." },
  { "en": "Lanjutkan deret huruf: A, D, H, M, S, ...", "id": "Z (pola loncat: +3, +4, +5, +6, +7)." },
  { "en": "Lanjutkan deret angka: 1, 2, 4, 8, 16, 32, ...", "id": "64 (pola: dikali 2)." },
  { "en": "Lanjutkan deret angka: 2, 6, 7, 21, 22, 66, ...", "id": "67 (pola: x3, +1, diulang)." },
  { "en": "Lanjutkan deret angka: 100, 96, 91, 85, 78, ...", "id": "70 (pola: -4, -5, -6, -7, -8)." },
  { "en": "Lanjutkan deret huruf: A, E, I, O, U, A, ...", "id": "E (pola: perputaran huruf vokal)." },
  { "en": "Lanjutkan deret angka: 1, 3, 2, 6, 3, 9, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 3,6,9,...)." },
  { "en": "Lanjutkan deret angka: 1, 1, 2, 6, 24, 120, ...", "id": "720 (pola: x1, x2, x3, x4, x5, x6)." },
  { "en": "Lanjutkan deret angka: 4, 5, 7, 11, 19, 35, ...", "id": "67 (pola: x2 - 3)." },
  { "en": "Apa sinonim dari kata 'MAKAR'?", "id": "Pemberontakan atau kudeta." },
  { "en": "Apa sinonim dari kata 'NISBI'?", "id": "Relatif." },
  { "en": "Apa sinonim dari kata 'PANDIR'?", "id": "Bodoh." },
  { "en": "Apa sinonim dari kata 'QUORUM'?", "id": "Jumlah minimum anggota yang harus hadir." },
  { "en": "Apa sinonim dari kata 'REMISI'?", "id": "Pengurangan hukuman." },
  { "en": "Apa antonim dari kata 'SABOTASE'?", "id": "Dukungan atau bantuan." },
  { "en": "Apa antonim dari kata 'TAKSA'?", "id": "Jelas atau lugas." },
  { "en": "Apa antonim dari kata 'ULUNG'?", "id": "Pemula atau amatir." },
  { "en": "Apa antonim dari kata 'VIRTUAL'?", "id": "Nyata atau fisik." },
  { "en": "Apa antonim dari kata 'WAHANA'?", "id": "Hambatan." },
  { "en": "Jika pernyataan 'Semua politisi berbohong' adalah benar, dan 'Budi adalah politisi', maka kesimpulannya...", "id": "Budi berbohong." },
  { "en": "Seekor katak jatuh ke dalam sumur 30 meter. Setiap hari ia naik 3 meter dan tergelincir turun 2 meter. Pada hari ke berapa ia keluar dari sumur?", "id": "Hari ke-28 (Pada hari ke-27, ia di posisi 27 meter. Hari ke-28, ia naik 3 meter dan keluar)." },
  { "en": "Jika sebuah virus melipatgandakan jumlahnya setiap jam, dan sebuah wadah penuh oleh virus pada pukul 12:00, pukul berapa wadah tersebut terisi setengah?", "id": "Pukul 11:00." },
  { "en": "Semua B adalah C. Semua A adalah B. Kesimpulan yang pasti benar adalah...", "id": "Semua A adalah C." },
  { "en": "Jika dibutuhkan 10 orang selama 10 hari untuk membangun 10 rumah. Berapa hari yang dibutuhkan 5 orang untuk membangun 5 rumah?", "id": "10 hari (Setiap orang butuh 10 hari untuk membangun 1 rumah)." },
  { "en": "Berapakah 33.33% dari 99?", "id": "33." },
  { "en": "Jika sebuah lingkaran memiliki keliling 88 cm, berapa luasnya? (π=22/7)", "id": "616 cm²." },
  { "en": "Berapakah nilai dari 1 - 1/2 + 1/4 - 1/8 + ... (deret geometri tak hingga)?", "id": "2/3." },
  { "en": "Jika x adalah bilangan prima antara 20 dan 30, dan y adalah bilangan kuadrat antara 10 dan 20. Berapakah x - y?", "id": "7 atau 13 (x bisa 23 atau 29, y = 16)." },
  { "en": "Berapa banyak 0 di akhir dari hasil 100! (100 faktorial)?", "id": "24." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda harus menegakkan hukum yang Anda tahu akan segera direvisi atau diubah?", "id": "Selama hukum tersebut masih berlaku, saya wajib menegakkannya. Tugas saya adalah melaksanakan hukum yang ada saat ini, bukan hukum yang akan ada nanti." },
  { "en": "Apa perbedaan antara 'keadilan hukum' (legal justice) dan 'keadilan sosial' (social justice)?", "id": "Keadilan hukum fokus pada penerapan aturan yang sama bagi semua orang. Keadilan sosial fokus pada pemerataan kesempatan dan hasil, dengan mempertimbangkan ketidaksetaraan struktural yang ada di masyarakat." },
  { "en": "Bagaimana Anda mengelola ekspektasi masyarakat yang seringkali tidak realistis (misal: ingin kasus selesai dalam sehari)?", "id": "Dengan memberikan edukasi dan komunikasi yang transparan mengenai tahapan dan kompleksitas proses penyidikan, tanpa memberikan janji yang tidak bisa ditepati." },
  { "en": "Menurut Anda, apa 'warisan' terburuk yang bisa ditinggalkan seorang polisi?", "id": "Meninggalkan citra bahwa wewenang bisa dibeli dan hukum bisa dinegosiasikan. Itu merusak kepercayaan publik secara fundamental." },
  { "en": "Bagaimana Anda menghadapi dilema antara loyalitas kepada rekan yang melakukan kesalahan dan loyalitas kepada kebenaran?", "id": "Loyalitas tertinggi saya adalah pada kebenaran dan institusi. Membantu rekan menyadari dan bertanggung jawab atas kesalahannya adalah bentuk loyalitas yang sesungguhnya, bukan menutupi kesalahannya." },
  { "en": "Apa yang akan Anda lakukan jika Anda merasa perintah atasan melanggar prinsip kemanusiaan yang paling dasar?", "id": "Saya akan menolak untuk melaksanakannya. Ada batasan di mana perintah harus dihentikan, yaitu ketika ia melanggar hukum universal tentang hak asasi manusia. Saya siap menanggung konsekuensinya." },
  { "en": "Bagaimana Anda menjaga objektivitas saat menangani kasus yang melibatkan korban atau pelaku yang memiliki kesamaan latar belakang dengan Anda?", "id": "Dengan secara sadar memisahkan identitas pribadi saya dari peran profesional saya. Saya akan fokus pada fakta dan bukti, dan jika perlu, meminta rekan lain untuk memberikan pandangan kedua." },
  { "en": "Jika Anda bisa merancang satu program pelatihan untuk semua anggota Polri, program apa itu dan mengapa?", "id": "Pelatihan 'Inteligensi Emosional dan Komunikasi Empatik'. Karena kemampuan mengelola emosi diri dan memahami emosi orang lain adalah kunci untuk mengurangi konflik dan meningkatkan kualitas pelayanan publik." },
  { "en": "Apa yang lebih sulit: menghadapi bahaya fisik atau menanggung beban psikologis dari sebuah kasus?", "id": "Beban psikologis seringkali lebih sulit. Luka fisik bisa sembuh, tetapi trauma psikologis bisa membekas seumur hidup dan mempengaruhi setiap aspek kehidupan jika tidak dikelola dengan baik." },
  { "en": "Bagaimana Anda mendefinisikan 'keberanian sejati' bagi seorang polisi?", "id": "Bukan hanya berani menghadapi penjahat, tetapi juga berani mengakui kesalahan, berani menolak perintah yang salah, dan berani membela kebenaran meskipun sendirian." },
  { "en": "Lengkapi analogi: JARUM : KOMPAS = ANGIN : ...", "id": "ARAH." },
  { "en": "Lengkapi analogi: GURUN : KAKTUS = KUTUB : ...", "id": "PENGUIN (atau BERUANG KUTUB)." },
  { "en": "Lengkapi analogi: OTOBIOGRAFI : DIRI SENDIRI = BIOGRAFI : ...", "id": "ORANG LAIN." },
  { "en": "Lengkapi analogi: KATA : MAKNA = WAJAH : ...", "id": "EKSPRESI." },
  { "en": "Lengkapi analogi: RAGU : YAKIN = GELAP : ...", "id": "TERANG." },
  { "en": "Lanjutkan deret angka: 2, 4, 10, 28, 82, ...", "id": "244 (pola: x3 - 2)." },
  { "en": "Lanjutkan deret angka: 1, 3, 15, 105, 945, ...", "id": "10395 (pola: x3, x5, x7, x9, x11)." },
  { "en": "Lanjutkan deret huruf: A, C, B, D, C, E, D, F, ...", "id": "E (pola: +2, -1, diulang)." },
  { "en": "Lanjutkan deret angka: 1, 2, 5, 12, 27, 58, ...", "id": "121 (pola: x2 + n, dimana n=0,1,2,3...)." },
  { "en": "Lanjutkan deret angka: 1, 1, 2, 4, 7, 11, 16, ...", "id": "22 (pola penambahan: +0,+1,+2,+3,+4,+5,+6)." },
  { "en": "Apa sinonim dari kata 'SINE QUA NON'?", "id": "Syarat mutlak." },
  { "en": "Apa sinonim dari kata 'TANDEM'?", "id": "Berpasangan." },
  { "en": "Apa sinonim dari kata 'USUR'?", "id": "Tua atau kuno." },
  { "en": "Apa antonim dari kata 'VIRTUSO'?", "id": "Amatir." },
  { "en": "Apa antonim dari kata 'ZENIT'?", "id": "Nadir atau titik terendah." },
  { "en": "Berapa jumlah dari 50 bilangan ganjil pertama?", "id": "2500 (n²)." },
  { "en": "Jika 1/x + 1/y = 1/z, maka z = ...", "id": "xy / (x+y)." },
  { "en": "Sebuah jam kehilangan 2 menit setiap jamnya. Jika disetel benar pada pukul 12:00 siang, pukul berapa yang ditunjukkannya saat waktu sebenarnya pukul 18:00?", "id": "Pukul 17:48." },
  { "en": "Berapa peluang mendapatkan jumlah mata dadu 7 jika dua dadu dilempar bersamaan?", "id": "6/36 atau 1/6." },
  { "en": "Jika A adalah 20% lebih besar dari B, maka B adalah berapa persen lebih kecil dari A?", "id": "16.67%." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda harus memilih antara loyalitas kepada teman dan loyalitas kepada prinsip?", "id": "Prinsip harus selalu diutamakan. Loyalitas sejati kepada teman adalah dengan membantunya untuk kembali ke jalan prinsip yang benar, bukan dengan mengikuti kesalahannya." },
  { "en": "Apa arti 'menjadi agen perubahan' di dalam institusi?", "id": "Artinya tidak hanya mengikuti arus, tetapi secara proaktif memberikan contoh, ide, dan tindakan positif untuk mendorong perbaikan dan kemajuan di lingkungan kerja, sekecil apapun itu." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda tahu Anda benar, tetapi seluruh tim menentang Anda?", "id": "Saya akan menyajikan data dan argumen saya sejelas mungkin. Jika tetap ditolak, saya akan menghormati keputusan tim, namun saya akan meminta agar keberatan saya dicatat sebagai bahan evaluasi di kemudian hari." },
  { "en": "Menurut Anda, apa satu hal yang harus selalu diingat oleh seorang polisi setiap kali ia mengenakan seragamnya?", "id": "Bahwa seragam itu bukan miliknya, tetapi milik rakyat. Itu adalah simbol amanah dan tanggung jawab besar untuk melayani dan melindungi, bukan simbol kekuasaan." },
  { "en": "Jika Anda bisa memberikan satu nasihat kepada seluruh calon anggota Polri, apa nasihat itu?", "id": "Masuklah dengan niat yang lurus untuk mengabdi. Karena hanya niat yang lurus yang akan menjadi kompas moral dan sumber kekuatan Anda saat menghadapi tantangan terberat dalam profesi ini." },
  { "en": "Jika A adalah kebalikan dari B, dan B adalah kebalikan dari C, maka hubungan A dan C adalah...", "id": "A sama dengan C." },
  { "en": "Semua X adalah Y. Tidak ada Z yang merupakan Y. Kesimpulannya?", "id": "Tidak ada Z yang merupakan X." },
  { "en": "Lengkapi analogi: BAB : BUKU = ADEGAN : ...", "id": "DRAMA (atau FILM)." },
  { "en": "Lanjutkan deret: 2, 1, 4, 3, 8, 5, 16, ...", "id": "7 (pola dua larik: 2,4,8,16 [x2] dan 1,3,5,7 [+2])." },
  { "en": "Apa sinonim dari kata 'YURISPRUDENSI'?", "id": "Keputusan hakim terdahulu." },
  { "en": "Apa antonim dari kata 'ABSTRAK'?", "id": "Konkret." },
  { "en": "Anda mengetahui ada kesalahan fatal dalam penanganan sebuah kasus oleh senior Anda bertahun-tahun yang lalu. Mengungkapnya akan merusak reputasinya. Apa yang Anda lakukan?", "id": "Saya akan melaporkan temuan tersebut melalui jalur internal yang tepat. Menegakkan kebenaran dan mencegah ketidakadilan di masa depan lebih penting daripada melindungi reputasi seseorang, meskipun itu sulit." },
  { "en": "Berapakah nilai dari 99²?", "id": "9801." },
  { "en": "Bagaimana Anda mendefinisikan 'kehormatan'?", "id": "Kehormatan adalah nilai yang kita berikan pada diri kita sendiri melalui konsistensi antara perkataan, perbuatan, dan prinsip yang kita pegang teguh, terutama saat diuji." },
  { "en": "Apa pertanyaan paling penting yang harus ditanyakan seorang polisi pada dirinya sendiri setiap hari?", "id": "'Apakah tindakan saya hari ini sudah membuat masyarakat merasa lebih aman dan lebih percaya pada kami?'." },
  { "en": "Bagaimana Anda secara sadar mencegah diri Anda agar tidak 'teracuni' oleh wewenang yang Anda miliki, yang dapat mengubah cara Anda memandang orang lain?", "id": "Dengan secara rutin melakukan 'grounding': berinteraksi dengan masyarakat sebagai warga biasa, mendengarkan keluhan mereka, dan selalu mengingatkan diri sendiri bahwa wewenang ini adalah amanah untuk melayani, bukan untuk superioritas." },
  { "en": "Seorang mantan narapidana kembali ke masyarakat, namun ditolak secara ekstrem. Bagaimana Anda menavigasi antara 'hak warga untuk aman' dan 'hak eks-napi untuk berintegrasi'?", "id": "Saya akan memfasilitasi dialog. Kepada warga, saya akan memberikan jaminan pengawasan. Kepada eks-napi, saya akan menekankan kewajiban untuk berkelakuan baik. Tujuannya adalah membangun kembali kepercayaan secara bertahap, bukan pemaksaan." },
  { "en": "Anda ditawari memimpin operasi populer yang akan menaikkan citra Anda, atau tugas penyelidikan senyap yang bisa membongkar akar masalah. Mana yang Anda pilih?", "id": "Saya akan memilih tugas penyelidikan senyap. Dampak jangka panjang dalam memberantas akar masalah jauh lebih berharga bagi masyarakat daripada citra pribadi sesaat." },
  { "en": "Anda menyadari ada 'praktik umum' yang tidak sesuai prosedur di unit Anda dan sudah dianggap wajar. Langkah pertama apa yang paling realistis?", "id": "Langkah pertama adalah saya tidak akan ikut dalam praktik tersebut dan konsisten bekerja sesuai prosedur yang benar. Menjadi contoh adalah cara paling aman untuk memulai perubahan tanpa menimbulkan konflik langsung." },
  { "en": "Apa perbedaan antara 'mematuhi hukum' dan 'percaya pada keadilan'?", "id": "Mematuhi hukum adalah tindakan mengikuti aturan tertulis. Percaya pada keadilan adalah keyakinan pada prinsip moral yang lebih tinggi bahwa setiap tindakan harus membawa kebenaran dan kebaikan, di mana hukum adalah salah satu alatnya." },
  { "en": "Anda harus menginterogasi seorang tersangka yang Anda tahu memiliki informasi krusial, tetapi ia sangat manipulatif dan cerdas secara psikologis. Strategi apa yang Anda gunakan?", "id": "Saya akan menggunakan strategi 'kesabaran'. Saya tidak akan terpancing emosinya, fokus pada inkonsistensi dalam ceritanya, dan menggunakan bukti-bukti objektif untuk mematahkan narasinya secara perlahan, bukan dengan konfrontasi langsung." },
  { "en": "Bagaimana Anda menjelaskan kepada keluarga korban bahwa pelaku kejahatan terhadap mereka mungkin mendapatkan hukuman yang lebih ringan dari tuntutan karena pertimbangan hukum?", "id": "Saya akan menjelaskannya dengan penuh empati, menerangkan bahwa keputusan ada di tangan hakim yang memiliki pertimbangan hukumnya sendiri. Tugas saya sebagai polisi adalah memastikan proses pembuktian sudah maksimal." },
  { "en": "Menurut Anda, apakah 'intuisi' seorang polisi itu nyata? Dan bagaimana cara mengasahnya?", "id": "Nyata, tapi itu bukan hal mistis. Intuisi adalah hasil dari pengenalan pola yang terakumulasi dari ribuan jam pengalaman di lapangan. Cara mengasahnya adalah dengan terus menambah jam terbang dan selalu melakukan refleksi setelah bertugas." },
  { "en": "Anda merasa bahwa sebuah perintah, meskipun sah, akan mengorbankan kesejahteraan anak buah Anda secara tidak perlu. Apa yang Anda lakukan sebagai pemimpin?", "id": "Saya akan menghadap atasan, menjelaskan potensi dampak negatifnya pada anggota, dan mengusulkan cara alternatif untuk mencapai tujuan yang sama dengan risiko yang lebih kecil bagi tim. Ini adalah bagian dari tanggung jawab melindungi anggota." },
  { "en": "Apa satu pelajaran paling berharga yang Anda dapat dari sebuah kegagalan besar?", "id": "Jawaban yang baik adalah yang menunjukkan introspeksi mendalam, misal: 'Kegagalan mengajarkan saya tentang pentingnya persiapan yang matang dan kerendahan hati untuk mendengarkan masukan orang lain, sesuatu yang tidak saya dapatkan dari keberhasilan'." },
  { "en": "Lengkapi analogi: EVOLUSI : PERUBAHAN = REVOLUSI : ...", "id": "PERGANTIAN (atau PEROMBAKAN)." },
  { "en": "Lengkapi analogi: APHORISME : BIJAKSANA = LELUCON : ...", "id": "LUCU." },
  { "en": "Lengkapi analogi: ARGUMEN : DEBAT = BUKTI : ...", "id": "PERSIDANGAN." },
  { "en": "Lengkapi analogi: JALUR : KERETA = ORBIT : ...", "id": "PLANET." },
  { "en": "Lengkapi analogi: BAYI : MERANGKAK = ANAK-ANAK : ...", "id": "BERLARI." },
  { "en": "Lengkapi analogi: MEMORI : AMNESIA = PERASAAN : ...", "id": "APATI." },
  { "en": "Lengkapi analogi: PENULIS : TEMA = SUTRADARA : ...", "id": "GENRE." },
  { "en": "Lengkapi analogi: BUNGA : TAMAN = BUMBU : ...", "id": "DAPUR." },
  { "en": "Lengkapi analogi: GURINDAM : DUA BARIS = SONETA : ...", "id": "EMPAT BELAS BARIS." },
  { "en": "Lengkapi analogi: UDARA : PERNAPASAN = MAKANAN : ...", "id": "PENCERNAAN." },
  { "en": "Lanjutkan deret angka: 1, 2, 4, 5, 10, 11, ...", "id": "22 (pola: +1, x2, diulang)." },
  { "en": "Lanjutkan deret angka: 1, 1, 3, 2, 5, 3, 7, ...", "id": "4 (pola dua larik: 1,3,5,7,... dan 1,2,3,4,...)." },
  { "en": "Lanjutkan deret huruf: A, B, E, F, I, J, ...", "id": "M (pola: 2 huruf urut, loncat 2 huruf, 2 huruf urut, loncat 2, dst)." },
  { "en": "Lanjutkan deret angka: 2, 8, 4, 16, 8, 32, ...", "id": "16 (pola dua larik: 2,4,8,16 [x2] dan 8,16,32 [x2])." },
  { "en": "Lanjutkan deret angka: 3, 8, 15, 24, 35, ...", "id": "48 (pola: n²-1, dimulai dari n=2)." },
  { "en": "Lanjutkan deret angka: 2, 5, 10, 17, 26, ...", "id": "37 (pola: n²+1, dimulai dari n=1)." },
  { "en": "Lanjutkan deret huruf: A, D, I, P, ...", "id": "Y (pola: posisi huruf adalah bilangan kuadrat 1, 4, 9, 16, 25)." },
  { "en": "Lanjutkan deret angka: 10, 2, 12, 4, 14, 6, ...", "id": "16 (pola dua larik: 10,12,14,16,... dan 2,4,6,...)." },
  { "en": "Lanjutkan deret angka: 4, 9, 5, 10, 6, 11, ...", "id": "7 (pola: +5, -4, diulang)." },
  { "en": "Lanjutkan deret angka: 1, 2, 6, 30, 210, ...", "id": "2310 (pola: x2, x3, x5, x7, x11 -> bilangan prima)." },
  { "en": "Apa sinonim dari kata 'ABERASI'?", "id": "Penyimpangan dari normal." },
  { "en": "Apa sinonim dari kata 'BONAFIDE'?", "id": "Dapat dipercaya." },
  { "en": "Apa sinonim dari kata 'CEREBRUM'?", "id": "Otak besar." },
  { "en": "Apa sinonim dari kata 'DEVIASI'?", "id": "Penyimpangan." },
  { "en": "Apa sinonim dari kata 'EKSENTRIK'?", "id": "Aneh atau tidak wajar." },
  { "en": "Apa antonim dari kata 'IZHAR'?", "id": "Menyembunyikan." },
  { "en": "Apa antonim dari kata 'JENERIK'?", "id": "Khusus." },
  { "en": "Apa antonim dari kata 'KAPABEL'?", "id": "Tidak mampu." },
  { "en": "Apa antonim dari kata 'LUMRAH'?", "id": "Aneh atau luar biasa." },
  { "en": "Apa antonim dari kata 'MENDUA'?", "id": "Setia." },
  { "en": "Jika pernyataan 'Semua yang hadir adalah anggota' salah, maka yang pasti benar adalah...", "id": "Beberapa yang hadir bukan anggota." },
  { "en": "Seorang anak memiliki 5 baju dan 3 celana. Berapa banyak kombinasi pakaian berbeda yang bisa ia kenakan?", "id": "15 kombinasi." },
  { "en": "Jika semua X adalah Y, dan tidak ada Z yang merupakan Y. Kesimpulan yang pasti benar adalah...", "id": "Semua X bukan Z." },
  { "en": "Dalam sebuah keluarga, setiap anak perempuan memiliki jumlah saudara laki-laki yang sama dengan jumlah saudara perempuannya. Setiap anak laki-laki memiliki jumlah saudara perempuan dua kali lebih banyak dari jumlah saudara laki-lakinya. Berapa jumlah anak dalam keluarga itu?", "id": "7 anak (4 perempuan, 3 laki-laki)." },
  { "en": "Jika dibutuhkan waktu 60 menit untuk memasak 1 kg daging hingga empuk. Berapa waktu yang dibutuhkan untuk memasak 2 kg daging hingga empuk dalam panci yang lebih besar?", "id": "Tetap 60 menit (atau sedikit lebih lama), karena empuknya daging tergantung suhu dan waktu, bukan jumlah." },
  { "en": "Sebuah mobil bergerak dengan kecepatan 90 km/jam. Berapa jarak yang ditempuh dalam 20 detik?", "id": "500 meter." },
  { "en": "Berapakah nilai dari (1/2)⁻³?", "id": "8." },
  { "en": "Rata-rata dari 7 bilangan adalah 8. Jika salah satu bilangan adalah 14, berapa rata-rata dari 6 bilangan lainnya?", "id": "7." },
  { "en": "Sebuah persegi panjang luasnya 48 cm². Jika panjangnya adalah 2 kali lebarnya, berapa kelilingnya?", "id": "28 cm (panjang 8, lebar 4√3... soal direvisi: jika panjangnya 3 kali lebarnya -> p=12, l=4, keliling=32)." },
  { "en": "Berapakah 2/3 dari 75%?", "id": "50% atau 0.5." },
  { "en": "Bagaimana Anda menyikapi pujian yang ditujukan kepada Anda atas keberhasilan tim?", "id": "Saya akan berterima kasih, lalu segera menyatakan bahwa keberhasilan ini adalah hasil kerja keras seluruh anggota tim, bukan saya seorang." },
  { "en": "Apa perbedaan antara 'wewenang formal' dan 'pengaruh informal' di tempat kerja?", "id": "Wewenang formal didapat dari jabatan atau pangkat. Pengaruh informal didapat dari kompetensi, karakter, dan hubungan baik yang dibangun." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus memberi sanksi kepada senior yang pernah menjadi mentor Anda?", "id": "Dengan berat hati namun tetap profesional. Saya akan menjalankan prosedur yang ada, sambil secara pribadi menyampaikan rasa hormat saya kepadanya di luar konteks penindakan tersebut." },
  { "en": "Menurut Anda, apa 'paradoks' terbesar dalam profesi kepolisian?", "id": "Paradoksnya adalah untuk menciptakan kedamaian, kadang polisi harus menggunakan kekerasan yang terkendali. Dan untuk melindungi kebebasan, kadang polisi harus membatasinya sesuai hukum." },
  { "en": "Bagaimana Anda memulihkan kepercayaan publik setelah sebuah skandal besar menimpa institusi?", "id": "Dengan melakukan tindakan nyata: mengusut tuntas skandal tersebut secara transparan, menghukum yang bersalah, memperbaiki sistem, dan secara konsisten menunjukkan kinerja yang profesional dan melayani setelahnya." },
  { "en": "Apa yang akan Anda lakukan jika Anda merasa target yang dibebankan kepada Anda tidak realistis dan tidak manusiawi?", "id": "Saya akan mencoba menjalankannya terlebih dahulu sambil mengumpulkan data. Kemudian saya akan menghadap atasan dengan data tersebut untuk menunjukkan kendala riil di lapangan dan mengusulkan target yang lebih realistis." },
  { "en": "Bagaimana Anda menjaga api semangat dalam diri saat melihat banyak ketidaksempurnaan dan masalah di dalam institusi?", "id": "Dengan mengubah fokus dari 'masalah yang tidak bisa saya ubah' menjadi 'perbaikan yang bisa saya lakukan'. Saya akan fokus menjadi bagian dari solusi di lingkup terkecil saya." },
  { "en": "Jika Anda bisa menulis satu aturan baru untuk Polri, aturan apa itu?", "id": "Jawaban yang baik adalah yang visioner, misal: 'Setiap anggota wajib meluangkan satu jam per minggu untuk kegiatan sosial tanpa seragam di lingkungannya, untuk membangun hubungan humanis dengan masyarakat'." },
  { "en": "Apa yang lebih penting untuk seorang pemimpin: dicintai atau disegani?", "id": "Disegani. Rasa cinta bisa hilang, tapi rasa segan yang lahir dari kombinasi kompetensi, integritas, dan ketegasan akan bertahan lama dan efektif dalam memimpin." },
  { "en": "Bagaimana Anda mengukur 'keberhasilan' Anda di akhir hari?", "id": "Bukan dari jumlah penangkapan, tetapi dari jumlah masalah yang berhasil saya cegah dan jumlah warga yang merasa terbantu oleh kehadiran saya." },
  { "en": "Lengkapi analogi: BENIH : PETANI = IDE : ...", "id": "PEMIKIR (atau ILMUWAN)." },
  { "en": "Lengkapi analogi: BUKU : HALAMAN = RUMAH : ...", "id": "KAMAR." },
  { "en": "Lengkapi analogi: GURUH : HUJAN = ASAP : ...", "id": "API." },
  { "en": "Lengkapi analogi: WASIT : NETRAL = HAKIM : ...", "id": "ADIL." },
  { "en": "Lengkapi analogi: RASA : LIDAH = BAU : ...", "id": "HIDUNG." },
  { "en": "Lanjutkan deret angka: 2, 4, 12, 14, 42, 44, ...", "id": "132 (pola: x2, +8, x3, +28... pola kurang jelas. Alternatif: 2,4,12,14... x2,+8.. tidak. Pola yang mungkin: x2, +8, x(14/12)... tidak. Coba pola lain: 2,4 (x2), 12,14(+2).. tidak. Pola: x2, +8, .. mari kita coba 2,4,12,48... tidak. Pola yang lebih mungkin: 2,4 (+2), 4,12(x3), 12,14(+2), 14,42(x3), 42,44(+2), 44x3=132)." },
  { "en": "Lanjutkan deret angka: 1, 10, 9, 2, 8, 7, 3, 6, ...", "id": "5 (pola tiga larik: 1,2,3,... 10,9,8,... 9,7,...)." },
  { "en": "Lanjutkan deret huruf: A, B, D, E, H, I, M, N, ...", "id": "S (pola: 2 urut, loncat 2, 2 urut, loncat 3, 2 urut, loncat 4, dst)." },
  { "en": "Lanjutkan deret angka: 1, 3, 6, 11, 18, 29, ...", "id": "42 (pola: deret Fibonacci modifikasi)." },
  { "en": "Lanjutkan deret angka: 5, 15, 12, 36, 33, 99, ...", "id": "96 (pola: x3, -3, diulang)." },
  { "en": "Apa sinonim dari kata 'FORUM'?", "id": "Wadah atau lembaga." },
  { "en": "Apa sinonim dari kata 'GITA'?", "id": "Lagu atau nyanyian." },
  { "en": "Apa sinonim dari kata 'HIBRIDA'?", "id": "Campuran." },
  { "en": "Apa antonim dari kata 'IMPLISIT'?", "id": "Eksplisit atau gamblang." },
  { "en": "Apa antonim dari kata 'JUMBO'?", "id": "Kecil." },
  { "en": "Berapakah 15% dari 250?", "id": "37.5." },
  { "en": "Sebuah balok memiliki ukuran 10x5x4 cm. Berapa luas permukaannya?", "id": "220 cm²." },
  { "en": "Jika 5^x = 125, berapakah nilai x?", "id": "3." },
  { "en": "Berapa banyak bilangan prima antara 30 dan 50?", "id": "5 (31, 37, 41, 43, 47)." },
  { "en": "Jika 3 orang bisa menyelesaikan sebuah lukisan dalam 4 hari, berapa hari yang dibutuhkan jika dikerjakan oleh 2 orang?", "id": "6 hari." },
  { "en": "Bagaimana Anda menghadapi situasi di mana Anda harus menyampaikan kritik kepada atasan Anda?", "id": "Saya akan meminta waktu khusus, menyampaikannya secara empat mata, menggunakan data sebagai dasar, dan membingkainya sebagai 'saran untuk perbaikan' bukan sebagai 'kritik atas kesalahan'." },
  { "en": "Apa perbedaan antara 'memiliki informasi' dan 'memiliki pemahaman'?", "id": "Memiliki informasi berarti sekedar tahu fakta. Memiliki pemahaman berarti mengerti konteks, hubungan sebab-akibat, dan implikasi dari fakta tersebut." },
  { "en": "Bagaimana Anda menjaga semangat Anda tetap tinggi di tengah budaya kerja yang mungkin birokratis dan lambat?", "id": "Dengan fokus pada hal-hal yang bisa saya kendalikan, yaitu kualitas kerja saya sendiri. Saya akan mencari cara untuk menjadi efisien dalam lingkup tugas saya dan merayakan setiap kemajuan kecil." },
  { "en": "Menurut Anda, apa 'pengorbanan' terbesar yang harus dilakukan oleh seorang anggota Polri?", "id": "Pengorbanan waktu dan momen-momen penting bersama keluarga. Tuntutan tugas yang tidak mengenal waktu seringkali mengharuskan hal tersebut." },
  { "en": "Jika Anda bisa memberi nama untuk autobiografi Anda sebagai seorang polisi, apa judulnya?", "id": "Jawaban yang baik adalah yang reflektif dan inspiratif, misal: 'Mengabdi dalam Sunyi', 'Jalan Lurus di Simpang Wewenang', atau 'Catatan Seorang Pelayan Publik'." },
  { "en": "Jika pernyataan 'Beberapa bunga tidak harum' adalah benar, manakah yang pasti salah?", "id": "Pernyataan 'Semua bunga harum'." },
  { "en": "Mana yang tidak termasuk dalam kelompok: Elang, Penguin, Merpati, Gagak?", "id": "Penguin (tidak bisa terbang)." },
  { "en": "Lengkapi analogi: BUMI : GEOLOGI = MASYARAKAT : ...", "id": "SOSIOLOGI." },
  { "en": "Lanjutkan deret: 1, 2, 3, 5, 5, 8, 7, ...", "id": "11 (pola dua larik: 1,3,5,7,... dan 2,5,8,11,...)." },
  { "en": "Apa sinonim dari kata 'KARNIVORA'?", "id": "Pemakan daging." },
  { "en": "Apa antonim dari kata 'PROMINEN'?", "id": "Tersembunyi atau biasa." },
  { "en": "Anda mengetahui seorang junior memanipulasi laporan patroli agar terlihat lebih 'rajin'. Apa tindakan Anda?", "id": "Saya akan memanggilnya, menunjukkan bahwa saya tahu tindakannya, dan menjelaskan bahwa integritas laporan jauh lebih penting daripada kuantitas. Saya akan memberinya kesempatan untuk memperbaiki tanpa harus melapor ke atasan jika ini kesalahan pertamanya." },
  { "en": "Berapa keliling lingkaran dengan jari-jari 21 cm?", "id": "132 cm." },
  { "en": "Bagaimana Anda mendefinisikan 'sukses' dalam sebuah operasi kepolisian?", "id": "Sukses bukan hanya tercapainya target operasi, tetapi juga dilaksanakannya operasi tersebut dengan profesional, sesuai prosedur, dan dengan tingkat risiko yang minimal bagi petugas maupun masyarakat." },
  { "en": "Apa pertanyaan terpenting yang harus Anda jawab sebelum Anda memutuskan untuk menggunakan kekuatan atau senjata?", "id": "'Apakah tindakan ini benar-benar absolut diperlukan, proporsional dengan ancaman, dan apakah semua alternatif lain sudah gagal?'." },
  { "en": "Apa beda fundamental antara 'kepatuhan cerdas' dan 'kepatuhan buta' dalam struktur komando?", "id": "Kepatuhan buta adalah melaksanakan perintah tanpa berpikir. Kepatuhan cerdas adalah memahami tujuan di balik perintah, melaksanakannya dengan cara terbaik, dan berani memberi masukan jika perintah tersebut mengandung risiko atau kekeliruan." },
  { "en": "Anda melihat peraturan sah yang dampaknya secara tidak proporsional merugikan kelompok rentan. Apa peran moral Anda selain 'menegakkan aturan'?", "id": "Peran moral saya adalah membuat laporan atau masukan kepada atasan mengenai dampak sosial dari peraturan tersebut di lapangan, sehingga bisa menjadi bahan pertimbangan untuk evaluasi kebijakan di masa depan." },
  { "en": "Anda menyadari budaya 'menahan perasaan' menyebabkan banyak masalah mental terpendam di unit Anda. Langkah kecil apa untuk membangun budaya lebih terbuka?", "id": "Saya bisa memulainya dengan menormalisasi percakapan tentang hari yang berat. Saat rapat evaluasi, saya tidak hanya bertanya 'apa kesulitan teknis?', tetapi juga 'bagaimana perasaan tim setelah tugas berat kemarin?'." },
  { "en": "Di unit Anda, untuk dapat 'proyek' bagus, sudah umum harus 'menyenangkan' pimpinan. Bagaimana Anda berprestasi tanpa ikut 'permainan' itu?", "id": "Saya akan fokus membangun reputasi melalui kualitas kerja yang luar biasa dan integritas yang tidak tercela. Saya akan membiarkan kinerja saya yang 'berbicara' dan menjadi nilai tawar utama, bukan kedekatan personal." },
  { "en": "Apa perbedaan antara 'mencari kebenaran' dan 'mencari pembenaran' dalam sebuah investigasi?", "id": "Mencari kebenaran berarti mengikuti bukti ke manapun ia mengarah, tanpa prasangka. Mencari pembenaran berarti memulai dengan sebuah kesimpulan, lalu hanya mencari bukti-bukti yang mendukung kesimpulan tersebut." },
  { "en": "Anda harus memilih satu dari dua junior untuk dipromosikan: A sangat cerdas dan kompeten tapi individualis; B tidak secerdas A tapi seorang 'team player' sejati. Siapa yang Anda pilih?", "id": "Saya akan memilih B. Kompetensi bisa diasah, tetapi karakter 'team player' yang mampu mengangkat kinerja seluruh tim jauh lebih berharga untuk kesehatan organisasi jangka panjang." },
  { "en": "Bagaimana Anda menjelaskan konsep 'pelanggaran HAM' kepada anggota yang berargumen bahwa 'tindakan keras diperlukan untuk melawan penjahat keras'?", "id": "Saya akan menjelaskan bahwa kita berbeda dari mereka justru karena kita terikat pada aturan dan penghormatan pada HAM. Menggunakan cara-cara mereka hanya akan mendelegitimasi institusi kita di mata hukum dan masyarakat." },
  { "en": "Menurut Anda, apa yang membedakan seorang 'pemimpin' dari seorang 'manajer'?", "id": "Seorang manajer mengatur sistem dan sumber daya untuk mencapai target. Seorang pemimpin menginspirasi dan memberdayakan manusia untuk mencapai visi. Manajer memastikan kereta berjalan tepat waktu, pemimpin memastikan kereta berjalan ke arah yang benar." },
  { "en": "Anda mengetahui ada kesalahan prosedur dalam sebuah kasus lama yang sudah inkrah (berkekuatan hukum tetap). Apa yang Anda lakukan?", "id": "Ini adalah dilema etis yang berat. Langkah yang paling benar adalah melaporkan temuan tersebut melalui jalur internal resmi untuk ditinjau, karena keadilan harus tetap diupayakan meskipun prosesnya akan sangat sulit." },
  { "en": "Apa satu ketakutan terbesar Anda yang tidak berhubungan dengan bahaya fisik dalam profesi ini?", "id": "Ketakutan terbesar saya adalah kehilangan empati, menjadi sinis, dan lupa pada tujuan awal saya mengabdi, sehingga saya hanya menjadi robot berseragam yang menjalankan tugas tanpa hati." },
  { "en": "Lengkapi analogi: HIPOKRISI : PERKATAAN-PERBUATAN = PARADOKS : ...", "id": "PERNYATAAN-LOGIKA." },
  { "en": "Lengkapi analogi: FONDASI : RUMAH = KONSTITUSI : ...", "id": "NEGARA." },
  { "en": "Lengkapi analogi: KAPAL : SAMUDRA = ROKET : ...", "id": "ANTARIKSA." },
  { "en": "Lengkapi analogi: PERTUMBUHAN : EKONOMI = KESEHATAN : ...", "id": "TUBUH." },
  { "en": "Lengkapi analogi: RELATIVITAS : EINSTEIN = EVOLUSI : ...", "id": "DARWIN." },
  { "en": "Lengkapi analogi: PUISI : IMAJINASI = SEJARAH : ...", "id": "FAKTA." },
  { "en": "Lengkapi analogi: KATA : TULISAN = NADA : ...", "id": "MUSIK." },
  { "en": "Lengkapi analogi: BURAM : JELAS = RAGU : ...", "id": "PASTI." },
  { "en": "Lengkapi analogi: DETEKTIF : PETUNJUK = DOKTER : ...", "id": "GEJALA (SIMPTOM)." },
  { "en": "Lengkapi analogi: PASIR : GURUN = AIR : ...", "id": "LAUTAN." },
  { "en": "Lanjutkan deret angka: 2, 6, 15, 34, 73, ...", "id": "152 (pola: x2 + 2, x2 + 3, x2 + 4, x2 + 5, x2 + 6)." },
  { "en": "Lanjutkan deret angka: 1, 2, 5, 26, 677, ...", "id": "458330 (pola: n²+1 dari angka sebelumnya -> 1²+1=2, 2²+1=5, 5²+1=26, dst)." },
  { "en": "Lanjutkan deret huruf: B, C, E, H, L, Q, ...", "id": "W (pola loncat: +1, +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 1, 2, 3, 2, 4, 6, 3, 6, ...", "id": "9 (pola kelompok: 1,2,3; 2,4,6; 3,6,9 -> n, 2n, 3n)." },
  { "en": "Lanjutkan deret angka: 100, 98, 97, 94, 93, 89, ...", "id": "88 (pola: -2, -1, -3, -1, -4, -1)." },
  { "en": "Lanjutkan deret angka: 1, 4, 27, 256, ...", "id": "3125 (pola: n pangkat n -> 1¹, 2², 3³, 4⁴, 5⁵)." },
  { "en": "Lanjutkan deret huruf: A, K, B, L, C, M, ...", "id": "D (pola dua larik: A,B,C,D,... dan K,L,M,...)." },
  { "en": "Lanjutkan deret angka: 2, 4, 8, 16, 32, ...", "id": "64 (pola: dikali 2)." },
  { "en": "Lanjutkan deret angka: 1, 10, 100, 1000, ...", "id": "10000 (pola: dikali 10)." },
  { "en": "Lanjutkan deret angka: 1, 3, 6, 10, 15, 21, ...", "id": "28 (pola bilangan segitiga: +2, +3, +4, +5, +6, +7)." },
  { "en": "Apa sinonim dari kata 'ADAGIUM'?", "id": "Pepatah atau peribahasa." },
  { "en": "Apa sinonim dari kata 'BAKHTIAR'?", "id": "Bahagia atau beruntung." },
  { "en": "Apa sinonim dari kata 'CIPTA'?", "id": "Kreasi atau karya." },
  { "en": "Apa sinonim dari kata 'DEFENISI'?", "id": "Batasan arti atau makna." },
  { "en": "Apa sinonim dari kata 'FATAMORGANA'?", "id": "Ilusi optik atau khayalan." },
  { "en": "Apa antonim dari kata 'INTERIM'?", "id": "Permanen atau tetap." },
  { "en": "Apa antonim dari kata 'JALANG'?", "id": "Jinak." },
  { "en": "Apa antonim dari kata 'KAPITALISME'?", "id": "Sosialisme." },
  { "en": "Apa antonim dari kata 'LUPUT'?", "id": "Terkena atau didapat." },
  { "en": "Apa antonim dari kata 'MUKADIMAH'?", "id": "Penutup atau epilog." },
  { "en": "Jika pernyataan 'Semua yang manis adalah gula' adalah salah, maka yang pasti benar adalah...", "id": "Ada sesuatu yang manis yang bukan gula." },
  { "en": "Sebuah mobil bergerak ke Timur sejauh 12 km, lalu ke Utara sejauh 5 km. Berapa jarak terpendek dari titik awal?", "id": "13 km (menggunakan teorema Pythagoras)." },
  { "en": "Semua A adalah B. Sebagian C adalah A. Kesimpulan yang pasti benar adalah...", "id": "Sebagian C adalah B." },
  { "en": "Lima orang (A, B, C, D, E) duduk melingkar. A duduk di seberang C. D duduk di antara A dan E. Siapa yang duduk di seberang B?", "id": "E." },
  { "en": "Jika sebuah pernyataan tidak bisa dibuktikan benar dan tidak bisa dibuktikan salah, itu disebut...", "id": "Paradoks." },
  { "en": "Berapa 1/4 dari 2/3 dari 600?", "id": "100." },
  { "en": "Jika 2^x = 64, berapakah nilai 2^(x-1)?", "id": "32 (x=6, jadi 2^5)." },
  { "en": "Sebuah pekerjaan dapat diselesaikan oleh 12 orang dalam 15 hari. Jika setelah 5 hari bekerja, 3 orang sakit. Berapa hari keterlambatan proyek tersebut?", "id": "5 hari keterlambatan." },
  { "en": "Berapakah modus dari data berikut: 1,2,3,4,5,6,7?", "id": "Tidak ada modus." },
  { "en": "Sebuah kubus memiliki volume 125 cm³. Berapa luas permukaannya?", "id": "150 cm² (sisi=5 cm)." },
  { "en": "Bagaimana Anda menyikapi seorang rekan yang selalu mengambil kredit atas ide atau pekerjaan Anda?", "id": "Saya akan mulai mendokumentasikan ide dan pekerjaan saya dengan lebih baik. Dalam rapat atau laporan, saya akan proaktif menyampaikan bagian kontribusi saya secara jelas dan profesional." },
  { "en": "Apa perbedaan antara 'simpati', 'empati', dan 'kasihan'?", "id": "Kasihan adalah perasaan superioritas terhadap penderitaan orang lain. Simpati adalah merasa 'ikut sedih'. Empati adalah mampu memahami dan merasakan penderitaan itu dari sudut pandang mereka, yang mendorong tindakan." },
  { "en": "Bagaimana Anda memulihkan kepercayaan tim setelah Anda sebagai pemimpin membuat keputusan yang terbukti salah?", "id": "Dengan mengakui kesalahan secara terbuka, menjelaskan dasar pemikiran saya saat itu, menunjukkan apa yang saya pelajari, dan melibatkan tim dalam membuat keputusan perbaikan." },
  { "en": "Menurut Anda, apa 'godaan' terbesar bagi seorang polisi: uang, kekuasaan, atau wanita/pria?", "id": "Ketiganya adalah godaan besar, tetapi 'kekuasaan' adalah yang paling berbahaya karena ia menjadi pintu masuk bagi godaan lainnya. Merasa berkuasa membuat seseorang merasa berhak atas uang dan hal lainnya." },
  { "en": "Bagaimana Anda mendefinisikan 'kebijaksanaan' dalam konteks kepolisian?", "id": "Kebijaksanaan adalah kemampuan untuk menerapkan hukum dan wewenang dengan mempertimbangkan konteks, kemanusiaan, dan dampak jangka panjang, bukan hanya berdasarkan teks aturan yang kaku." },
  { "en": "Apa yang akan Anda lakukan jika Anda merasa jenuh dengan sistem dan ingin menyerah?", "id": "Saya akan mencari satu alasan mengapa saya harus bertahan—mungkin untuk keluarga, untuk satu orang yang pernah saya tolong, atau untuk junior yang butuh teladan. Saya akan fokus pada 'satu' alasan itu untuk terus maju." },
  { "en": "Bagaimana Anda menjelaskan kepada publik tentang pentingnya razia atau pemeriksaan rutin yang sering dianggap 'mengganggu'?", "id": "Saya akan menjelaskan bahwa itu adalah tindakan preventif untuk mencegah kejahatan yang lebih besar, seperti terorisme atau peredaran narkoba. Ini seperti 'vaksin' untuk keamanan, mungkin sedikit tidak nyaman tapi penting untuk melindungi semua." },
  { "en": "Jika Anda bisa mengubah satu persepsi negatif masyarakat tentang polisi, apa itu?", "id": "Persepsi bahwa semua polisi sama. Saya ingin masyarakat melihat bahwa di dalam institusi ini ada banyak individu yang bekerja keras dengan tulus dan berintegritas setiap hari." },
  { "en": "Apa yang lebih penting bagi kemajuan institusi: kritik dari dalam atau kritik dari luar?", "id": "Keduanya penting. Kritik dari dalam menunjukkan adanya kesadaran untuk perbaikan. Kritik dari luar berfungsi sebagai cermin dan pengingat agar institusi tidak menjadi arogan dan tetap membumi." },
  { "en": "Bagaimana Anda menjaga 'kemanusiaan' Anda agar tidak hilang ditelan oleh rutinitas dan birokrasi?", "id": "Dengan secara sengaja meluangkan waktu untuk berinteraksi dengan orang-orang di luar 'gelembung' kepolisian, membaca buku fiksi, atau menjadi relawan, untuk terus mengasah perspektif dan empati." },
  { "en": "Lengkapi analogi: PLAGIARISME : TULISAN = PEMALSUAN : ...", "id": "UANG (atau DOKUMEN)." },
  { "en": "Lengkapi analogi: JANGKAR : STABILITAS = BALING-BALING : ...", "id": "GERAKAN." },
  { "en": "Lengkapi analogi: PENAWARAN : PERMINTAAN = SEBAB : ...", "id": "AKIBAT." },
  { "en": "Lengkapi analogi: KODE : RAHASIA = SINYAL : ...", "id": "PESAN." },
  { "en": "Lengkapi analogi: RUMPUT : HIJAU = LANGIT : ...", "id": "BIRU." },
  { "en": "Lanjutkan deret angka: 1, 3, 7, 15, 31, 63, ...", "id": "127 (pola: x2 + 1)." },
  { "en": "Lanjutkan deret angka: 4, 7, 12, 19, 28, 39, ...", "id": "52 (pola: +3, +5, +7, +9, +11, +13)." },
  { "en": "Lanjutkan deret huruf: A, D, E, H, I, L, M, ...", "id": "P (pola: +3,+1,+3,+1,+3,+1,+3)." },
  { "en": "Lanjutkan deret angka: 2, 5, 10, 50, 500, ...", "id": "25000 (pola: perkalian dua angka sebelumnya)." },
  { "en": "Lanjutkan deret angka: 3, 1, 4, 1, 5, 9, ...", "id": "2 (pola: digit dari Pi π=3.141592...)." },
  { "en": "Apa sinonim dari kata 'GENOSIDA'?", "id": "Pemusnahan massal." },
  { "en": "Apa sinonim dari kata 'HAKIKAT'?", "id": "Inti atau esensi." },
  { "en": "Apa sinonim dari kata 'INSINUASI'?", "id": "Sindiran halus." },
  { "en": "Apa antonim dari kata 'KOMPETEN'?", "id": "Tidak mampu." },
  { "en": "Apa antonim dari kata 'KONKLUsi'?", "id": "Premis atau pendahuluan." },
  { "en": "Berapakah akar kuadrat dari 2025?", "id": "45." },
  { "en": "Sebuah mobil menempuh 360 km dalam 5 jam. Berapa km/jam kecepatannya?", "id": "72 km/jam." },
  { "en": "Jika sebuah tas didiskon 20% menjadi Rp 160.000, berapa harga aslinya?", "id": "Rp 200.000." },
  { "en": "Jika 3^x = 81, berapakah x?", "id": "4." },
  { "en": "Berapa banyak rusuk yang dimiliki limas segi empat?", "id": "8." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda tahu seorang informan memberikan info yang benar, tetapi untuk menjatuhkan saingannya?", "id": "Saya akan memverifikasi dan menggunakan informasi yang benar untuk menegakkan hukum, tetapi saya tidak akan membiarkan diri saya menjadi alat dalam persaingan mereka. Saya akan tetap objektif." },
  { "en": "Apa perbedaan antara 'memiliki integritas' dan 'terlihat berintegritas'?", "id": "Memiliki integritas adalah konsistensi karakter yang nyata baik saat diawasi maupun tidak. Terlihat berintegritas bisa jadi hanya pencitraan atau topeng yang dipakai saat ada orang lain." },
  { "en": "Bagaimana Anda mengatasi rasa takut membuat kesalahan dalam sebuah tugas penting?", "id": "Dengan fokus pada persiapan yang sebaik mungkin, berkonsultasi dengan yang lebih berpengalaman, dan meyakini bahwa kesalahan adalah bagian dari proses belajar, selama saya bertanggung jawab atasnya." },
  { "en": "Menurut Anda, apa 'warisan' yang paling ingin Anda tinggalkan untuk anak-anak Anda dari profesi Anda sebagai polisi?", "id": "Warisan nama baik dan pemahaman bahwa hidup adalah tentang pengabdian dan berbuat benar, bukan tentang mengumpulkan materi." },
  { "en": "Jika Anda bisa bertanya satu hal kepada seorang penjahat kambuhan, apa yang akan Anda tanyakan?", "id": "Jawaban yang baik adalah yang mencari pemahaman akar masalah, misal: 'Apa satu hal yang bisa membuatmu berhenti dari jalan ini selamanya?'." },
  { "en": "Mana yang lebih dulu ada, ayam atau telur?", "id": "Telur, yang diletakkan oleh hewan yang bukan ayam tetapi merupakan nenek moyang evolusionernya." },
  { "en": "Semua X adalah Y. Sebagian Y adalah Z. Apakah ada X yang merupakan Z?", "id": "Belum tentu (tidak dapat disimpulkan)." },
  { "en": "Lengkapi analogi: ANGIN : KINCIR = UAP : ...", "id": "TURBIN." },
  { "en": "Lanjutkan deret: 1, 5, 14, 30, 55, ...", "id": "91 (pola: jumlah bilangan kuadrat)." },
  { "en": "Apa sinonim dari kata 'LANUN'?", "id": "Bajak laut." },
  { "en": "Apa antonim dari kata 'NEKAD'?", "id": "Penuh pertimbangan." },
  { "en": "Anda mengetahui bahwa ada bukti yang 'direkayasa' untuk mempercepat penyelesaian sebuah kasus yang menjadi sorotan publik. Apa yang akan Anda lakukan?", "id": "Saya memiliki kewajiban untuk menolak bukti tersebut dan melaporkan tindakan rekayasa itu kepada atasan atau unit Propam. Menegakkan keadilan dengan cara yang tidak adil adalah sebuah kontradiksi." },
  { "en": "Jika sebuah lingkaran memiliki luas 616 cm², berapa kelilingnya?", "id": "88 cm (jari-jari 14 cm)." },
  { "en": "Apa definisi Anda tentang 'kawan sejati' di dalam institusi?", "id": "Kawan sejati adalah yang berani menegurmu saat kamu salah, mendukungmu saat kamu benar, dan selalu mengingatkanmu untuk tetap berada di jalan yang lurus." },
  { "en": "Apa pertanyaan terakhir yang Anda tanyakan pada diri sendiri sebelum tidur setelah hari yang melelahkan?", "id": "Jawaban yang baik adalah yang reflektif, misal: 'Apakah saya sudah menjadi versi terbaik dari diri saya hari ini?', atau 'Adakah kebaikan yang saya tebar hari ini?'." },
  { "en": "Anda mengetahui ada sebuah 'kesalahan sistemik' yang menyebabkan banyak anggota baik terpaksa melakukan pelanggaran administratif kecil. Apa yang lebih prioritas: menindak anggotanya atau memperjuangkan perbaikan sistemnya?", "id": "Memperjuangkan perbaikan sistemnya. Menindak anggota hanya akan memadamkan 'gejala' tanpa menyembuhkan 'penyakitnya'. Memperbaiki akar masalah akan menyelamatkan lebih banyak orang di masa depan." },
  { "en": "Seorang informan memberikan data akurat tentang jaringan teroris, namun Anda tahu ia sendiri adalah pelaku kejahatan lain. Bagaimana Anda mengelola paradoks ini?", "id": "Saya akan memisahkan dua hal. Informasi intelijennya akan saya gunakan untuk membongkar jaringan teroris demi keselamatan publik yang lebih luas. Secara terpisah, proses hukum atas kejahatannya sendiri harus tetap berjalan." },
  { "en": "Bagaimana Anda membedakan antara 'menjadi manusiawi' dengan 'menjadi lemah' dalam konteks penegakan hukum?", "id": "Menjadi manusiawi adalah menggunakan empati dan kebijaksanaan dalam menerapkan hukum. Menjadi lemah adalah membiarkan perasaan atau rasa takut menghalangi penegakan hukum yang seharusnya dilakukan." },
  { "en": "Anda melihat seorang pimpinan yang sangat visioner tetapi cara komunikasinya seringkali menyakitkan dan 'membunuh' motivasi tim. Apa yang bisa Anda lakukan?", "id": "Saya akan mencoba menjadi 'penerjemah' visinya kepada tim dengan bahasa yang lebih positif dan memotivasi. Kepada pimpinan, saya akan mencari cara untuk memberikan masukan tentang dampak komunikasinya secara halus." },
  { "en": "Apa arti 'keadilan' bagi seorang korban yang keluarganya terbunuh, di mana hukuman seberat apapun tidak akan mengembalikan nyawa?", "id": "Keadilan dalam konteks ini bukan hanya tentang hukuman bagi pelaku, tetapi juga tentang pengakuan atas penderitaan korban, pemulihan hak-haknya, dan jaminan dari negara bahwa kejadian serupa tidak akan terulang." },
  { "en": "Anda berada dalam situasi di mana mengikuti prosedur akan membahayakan nyawa tim Anda, sementara melanggarnya berpotensi menyelamatkan mereka. Keputusan apa yang Anda ambil sebagai pemimpin lapangan?", "id": "Keselamatan nyawa tim adalah prioritas tertinggi. Saya akan mengambil keputusan untuk melanggar prosedur dengan risiko yang terukur, dan saya siap mempertanggungjawabkan keputusan tersebut sepenuhnya kepada pimpinan setelahnya." },
  { "en": "Bagaimana Anda menjelaskan kepada diri sendiri dan keluarga tentang 'panggilan jiwa' untuk profesi yang seringkali tidak dihargai dan penuh risiko ini?", "id": "Saya menjelaskannya sebagai sebuah kesempatan langka untuk berada di garis depan dalam menjaga keteraturan dan menolong orang pada saat terlemah mereka. Kepuasan batin dari pengabdian ini tidak bisa dinilai dengan materi atau penghargaan." },
  { "en": "Menurut Anda, apakah mungkin seorang polisi tetap idealis setelah 20 tahun bertugas? Jika ya, bagaimana caranya?", "id": "Mungkin, dengan cara terus menerus 'mengkalibrasi ulang' niat awal, memiliki lingkaran pertemanan yang positif, dan secara sadar mencari serta merayakan setiap kebaikan kecil yang berhasil dilakukan setiap hari." },
  { "en": "Anda harus memilih satu dari dua tugas: A) Mengamankan aset negara bernilai triliunan, atau B) Menyelamatkan satu desa dari konflik komunal. Mana yang Anda pilih?", "id": "Saya memilih menyelamatkan desa dari konflik komunal. Keselamatan nyawa manusia tidak ternilai dan harus selalu menjadi prioritas di atas aset materi, sebesar apapun nilainya." },
  { "en": "Apa satu hal yang Anda harap masyarakat umum tidak akan pernah tahu tentang sisi gelap pekerjaan polisi, dan mengapa?", "id": "Beban psikologis dan trauma yang sering kami bawa pulang. Bukan untuk menutupi, tetapi untuk melindungi keluarga kami dan menjaga persepsi bahwa kami selalu kuat dan siap melindungi mereka kapan saja." },
  { "en": "Lengkapi analogi: KONSTITUSI : PREAMBUL = BUKU : ...", "id": "KATA PENGANTAR." },
  { "en": "Lengkapi analogi: SINAPSIS : NEURON = JEMBATAN : ...", "id": "DARATAN (atau SUNGAI)." },
  { "en": "Lengkapi analogi: EROSI : TANAH = KOROSI : ...", "id": "LOGAM." },
  { "en": "Lengkapi analogi: PENGHAPUS : KESALAHAN TULIS = PERMINTAAN MAAF : ...", "id": "KESALAHAN PERBUATAN." },
  { "en": "Lengkapi analogi: DNA : INFORMASI GENETIK = NOT BALOK : ...", "id": "INFORMASI MUSIK." },
  { "en": "Lengkapi analogi: KEADILAN : HUKUM = KESEHATAN : ...", "id": "KEDOKTERAN." },
  { "en": "Lengkapi analogi: BAYANGAN : CAHAYA = GEMA : ...", "id": "SUARA." },
  { "en": "Lengkapi analogi: KATA : BAHASA = PIKSEL : ...", "id": "GAMBAR." },
  { "en": "Lengkapi analogi: AKAR : POHON = FONDASI : ...", "id": "GEDUNG." },
  { "en": "Lengkapi analogi: KERING : GURUN = LEMBAB : ...", "id": "HUTAN HUJAN." },
  { "en": "Lanjutkan deret angka: 1, 3, 4, 7, 11, 18, 29, ...", "id": "47 (pola: deret Lucas)." },
  { "en": "Lanjutkan deret angka: 2, 6, 10, 30, 34, 102, ...", "id": "106 (pola: x3, +4, diulang)." },
  { "en": "Lanjutkan deret huruf: A, C, F, J, O, ...", "id": "U (pola loncat: +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 1, 4, 10, 22, 46, ...", "id": "94 (pola: x2 + 2)." },
  { "en": "Lanjutkan deret angka: 3, 2, 5, 4, 7, 6, ...", "id": "9 (pola dua larik: 3,5,7,9,... dan 2,4,6,...)." },
  { "en": "Lanjutkan deret angka: 1, 8, 27, 64, 125, ...", "id": "216 (pola: bilangan kubik)." },
  { "en": "Lanjutkan deret huruf: A, E, D, H, G, K, ...", "id": "J (pola dua larik: A,D,G,J,... dan E,H,K,...)." },
  { "en": "Lanjutkan deret angka: 1, 2, 4, 7, 12, 20, ...", "id": "33 (pola penambahan: +1,+2,+3,+5,+8 -> deret Fibonacci)." },
  { "en": "Lanjutkan deret angka: 1, 1, 1, 2, 3, 4, 6, ...", "id": "9 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Lanjutkan deret angka: 100, 1, 99, 2, 98, 3, ...", "id": "97 (pola dua larik: 100,99,98,97,... dan 1,2,3,...)." },
  { "en": "Apa sinonim dari kata 'AFIRMASI'?", "id": "Penegasan atau pengakuan." },
  { "en": "Apa sinonim dari kata 'BIBLIOFILIA'?", "id": "Cinta terhadap buku." },
  { "en": "Apa sinonim dari kata 'DEMAGOG'?", "id": "Penghasut rakyat." },
  { "en": "Apa sinonim dari kata 'EFEMERAL'?", "id": "Sesaat atau singkat." },
  { "en": "Apa sinonim dari kata 'FILANTROP'?", "id": "Dermawan." },
  { "en": "Apa antonim dari kata 'KONKURENSI'?", "id": "Monopoli." },
  { "en": "Apa antonim dari kata 'LIBERAL'?", "id": "Konservatif." },
  { "en": "Apa antonim dari kata 'MAJAL'?", "id": "Tajam." },
  { "en": "Apa antonim dari kata 'NIRWANA'?", "id": "Dunia fana atau samsara." },
  { "en": "Apa antonim dari kata 'OPAS'?", "id": "Majikan." },
  { "en": "Jika pernyataan 'Tidak semua burung bisa terbang' adalah benar, maka yang pasti salah adalah...", "id": "Pernyataan 'Semua burung bisa terbang'." },
  { "en": "Seorang ayah berkata pada anaknya: 'Saat aku seusiamu, usiaku adalah tiga kali usiamu saat itu'. Jika usia ayah sekarang 48 tahun, dan perbandingan usia mereka sekarang 4:1. Berapa usia anak saat ini?", "id": "12 tahun." },
  { "en": "Semua A adalah B. Sebagian C adalah A. Kesimpulan yang pasti benar adalah...", "id": "Sebagian C adalah B." },
  { "en": "Di sebuah pulau, ada ksatria (selalu jujur) dan penjahat (selalu bohong). Anda bertemu 2 orang, A dan B. A berkata 'Setidaknya salah satu dari kami adalah penjahat'. Siapakah A dan B?", "id": "A adalah Ksatria, B adalah Penjahat." },
  { "en": "Jika sebuah buku diletakkan di atas meja, maka...", "id": "Buku memberikan gaya ke bawah, dan meja memberikan gaya ke atas yang sama besar." },
  { "en": "Berapakah 1/6 dari 1 hari dalam jam?", "id": "4 jam." },
  { "en": "Jika 1/x = 3, berapakah nilai dari 1/(x+1)?", "id": "3/4." },
  { "en": "Sebuah bakteri membelah diri menjadi dua setiap 3 menit. Berapa banyak bakteri yang ada setelah setengah jam jika dimulai dari satu bakteri?", "id": "1024 (2 pangkat 10)." },
  { "en": "Berapakah sudut yang dibentuk oleh jarum jam pada pukul 20:00?", "id": "120 derajat." },
  { "en": "Sebuah dadu dan sebuah koin dilempar bersamaan. Berapa peluang munculnya angka genap pada dadu dan gambar pada koin?", "id": "3/12 atau 1/4." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda harus menegakkan aturan yang Anda sendiri anggap 'ketinggalan zaman'?", "id": "Saya akan menegakkannya karena itu masih aturan yang berlaku, namun secara bersamaan saya akan membuat kajian atau usulan kepada pimpinan untuk meninjau kembali relevansi aturan tersebut." },
  { "en": "Apa perbedaan antara 'memiliki kekuasaan' dan 'menjadi berkuasa'?", "id": "Memiliki kekuasaan adalah memegang wewenang formal. Menjadi berkuasa adalah saat wewenang itu meresap ke dalam karakter dan membuat seseorang merasa superior, seringkali menjadi awal dari penyalahgunaan." },
  { "en": "Bagaimana Anda memulihkan semangat tim setelah seorang anggotanya gugur dalam tugas?", "id": "Dengan mengadakan upacara penghormatan yang layak, memastikan keluarga yang ditinggalkan terurus, dan mengingatkan tim bahwa pengorbanan rekan mereka adalah alasan untuk menjadi lebih kuat dan lebih solid, bukan untuk menjadi takut." },
  { "en": "Menurut Anda, apa yang membedakan 'polisi' dengan 'penegak hukum' lainnya (jaksa, hakim)?", "id": "Polisi adalah garda terdepan yang berinteraksi langsung dengan kekacauan sosial di lapangan. Kami adalah 'pintu pertama' dari sistem peradilan pidana, yang menentukan kualitas awal dari sebuah proses hukum." },
  { "en": "Bagaimana Anda mendefinisikan 'keberhasilan' dalam mencegah sebuah kejahatan?", "id": "Keberhasilan sejati adalah saat potensi kejahatan itu tidak pernah terjadi sama sekali berkat tindakan preventif, bukan saat kita berhasil menangkap pelakunya setelah kejadian. Ini adalah keberhasilan yang sunyi dan tak terlihat." },
  { "en": "Apa yang akan Anda lakukan jika Anda merasa bahwa sistem promosi di unit Anda lebih didasarkan pada 'kedekatan' daripada 'prestasi'?", "id": "Saya akan tetap fokus pada peningkatan prestasi dan kompetensi saya secara maksimal. Saya percaya dalam jangka panjang, kualitas sejati akan menemukan jalannya, sambil terus membangun jaringan kerja secara profesional." },
  { "en": "Bagaimana Anda menjelaskan kepada anak muda bahwa menjadi polisi bukan tentang 'gaya' dan 'kuasa', tetapi tentang 'pelayanan'?", "id": "Saya akan menunjukkan contoh nyata, menceritakan kisah-kisah tentang bagaimana polisi membantu korban, menolong orang tersesat, atau bahkan mengatur lalu lintas di tengah hujan. Pelayanan adalah tindakan, bukan penampilan." },
  { "en": "Jika Anda bisa menghapus satu stereotip tentang polisi, apa itu?", "id": "Stereotip bahwa polisi 'mencari-cari kesalahan'. Saya ingin masyarakat paham bahwa tujuan utama kami di lapangan adalah mencegah masalah dan memastikan semua selamat, bukan untuk menghukum." },
  { "en": "Apa yang lebih sulit: menghadapi massa yang marah atau menghadapi satu orang yang putus asa dan ingin bunuh diri?", "id": "Menghadapi satu orang yang putus asa. Itu membutuhkan tingkat empati, kesabaran, dan keterampilan komunikasi psikologis yang jauh lebih dalam, karena satu kata yang salah bisa berakibat fatal." },
  { "en": "Bagaimana Anda mendefinisikan 'jiwa korsa' yang sehat?", "id": "Jiwa korsa yang sehat adalah semangat kebersamaan untuk saling mendukung dalam kebaikan dan profesionalisme, serta saling mengingatkan dan mengoreksi jika ada yang mulai menyimpang dari jalan yang benar." },
  { "en": "Lengkapi analogi: KATA : PUITIS = NADA : ...", "id": "MELODIS." },
  { "en": "Lengkapi analogi: PROTAGONIS : PAHLAWAN = ANTAGONIS : ...", "id": "PENJAHAT." },
  { "en": "Lengkapi analogi: PERTANYAAN : JAWABAN = MASALAH : ...", "id": "SOLUSI." },
  { "en": "Lengkapi analogi: KERING : KERONCONG = BASAH : ...", "id": "LEPEK." },
  { "en": "Lengkapi analogi: KOMPONEN : SISTEM = ORGAN : ...", "id": "ORGANISME." },
  { "en": "Lanjutkan deret angka: 1, 3, 12, 52, 265, ...", "id": "1596 (pola: x1+2, x2+6, x3+10, x4+14...)." },
  { "en": "Lanjutkan deret angka: 2, 3, 6, 11, 18, 27, ...", "id": "38 (pola penambahan: +1,+3,+5,+7,+9,+11)." },
  { "en": "Lanjutkan deret huruf: A, C, G, S, ...", "id": "K (pola: A(1), C(3), G(7), S(19) -> 2n+1... tidak. Pola kurang jelas)." },
  { "en": "Lanjutkan deret angka: 1, 4, 2, 8, 6, 24, 22, ...", "id": "88 (pola: x4, -2, x4, -2, diulang)." },
  { "en": "Lanjutkan deret angka: 1, 10, 100, 1000, ...", "id": "10000 (pola: dikali 10)." },
  { "en": "Apa sinonim dari kata 'GANDUH'?", "id": "Tukar tambah." },
  { "en": "Apa sinonim dari kata 'HAYATI'?", "id": "Berkenaan dengan kehidupan." },
  { "en": "Apa sinonim dari kata 'IMPRESI'?", "id": "Kesan." },
  { "en": "Apa antonim dari kata 'KEBIJAKAN'?", "id": "Kecerobohan." },
  { "en": "Apa antonim dari kata 'LAGAK'?", "id": "Sederhana atau apa adanya." },
  { "en": "Berapakah 2/5 dari 1 menit?", "id": "24 detik." },
  { "en": "Jika 10% dari x adalah 30, berapakah 1/3 dari x?", "id": "100 (x=300)." },
  { "en": "Sebuah persegi memiliki keliling yang sama dengan keliling lingkaran berjari-jari 7 cm. Berapa luas persegi tersebut?", "id": "121 cm² (Keliling lingkaran = 44, sisi persegi = 11)." },
  { "en": "Jika x/y = 3/4 dan y/z = 2/3, maka x/z = ...", "id": "1/2." },
  { "en": "Berapa banyak faktor positif yang dimiliki oleh bilangan 100?", "id": "9 (1, 2, 4, 5, 10, 20, 25, 50, 100)." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda harus menegakkan hukum pada seseorang yang sangat berjasa pada Anda di masa lalu?", "id": "Saya akan memisahkan rasa terima kasih pribadi dengan tugas profesional. Hukum harus ditegakkan. Saya akan melakukannya dengan cara yang paling hormat, dan setelah proses selesai, saya akan menemuinya sebagai pribadi." },
  { "en": "Apa perbedaan antara 'memiliki power' dan 'memiliki authority'?", "id": "Authority adalah hak formal untuk memerintah (wewenang). Power adalah kemampuan riil untuk mempengaruhi, yang bisa datang dari wewenang, pengetahuan, atau karisma. Tidak semua yang punya authority punya power, dan sebaliknya." },
  { "en": "Bagaimana Anda mengatasi 'kejenuhan moral' (moral fatigue) setelah melihat begitu banyak ketidakadilan?", "id": "Dengan secara sadar mencari dan fokus pada kasus-kasus di mana keadilan berhasil ditegakkan, merayakan kemenangan-kemenangan kecil, dan menjaga keyakinan bahwa setiap usaha baik itu berarti." },
  { "en": "Menurut Anda, apa satu hal yang jika hilang dari seorang polisi, maka ia kehilangan segalanya?", "id": "Kepercayaan. Jika masyarakat, rekan, dan atasan tidak lagi percaya padanya, maka wewenang dan seragamnya menjadi tidak berarti." },
  { "en": "Jika Anda bisa memberikan satu nasihat kepada para pembuat kebijakan tentang kepolisian, apa itu?", "id": "Fokuslah pada peningkatan kualitas sumber daya manusia dan kesejahteraannya. Karena secanggih apapun teknologinya, ujung tombak kepolisian adalah manusia yang berintegritas dan profesional." },
  { "en": "Jika Anda memiliki 3 kotak, satu berisi 2 bola emas, satu berisi 2 bola perak, dan satu berisi 1 emas 1 perak. Labelnya semua salah. Anda boleh mengambil satu bola dari satu kotak tanpa melihat. Kotak mana yang Anda pilih untuk memastikan Anda tahu isi semua kotak?", "id": "Kotak berlabel 'Emas & Perak'. Karena labelnya pasti salah, isinya pasti 2 Emas atau 2 Perak. Jika yang keluar Emas, maka kotak itu berisi 2 Emas. Kotak berlabel 'Perak' pasti berisi Emas&Perak, dan kotak 'Emas' berisi 2 Perak." },
  { "en": "Semua X adalah Y. Semua Y adalah Z. Kesimpulannya?", "id": "Semua X adalah Z." },
  { "en": "Lengkapi analogi: PENGETAHUAN : KEBODOHAN = KEBERANIAN : ...", "id": "KETAKUTAN." },
  { "en": "Lanjutkan deret: 1, 2, 3, 6, 11, 20, 37, ...", "id": "68 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Apa sinonim dari kata 'MAJAL'?", "id": "Tumpul." },
  { "en": "Apa antonim dari kata 'ORATOR'?", "id": "Pendengar." },
  { "en": "Anda mengetahui ada dana operasional yang tidak terpakai, dan rekan-rekan sepakat untuk 'membaginya' di akhir tahun. Apa yang Anda lakukan?", "id": "Saya akan menolak dengan tegas dan mengingatkan bahwa itu adalah uang negara yang harus dikembalikan. Mengambilnya adalah bentuk korupsi. Saya akan menyarankan agar dana itu dilaporkan sebagai sisa anggaran." },
  { "en": "Berapa 6! (6 faktorial)?", "id": "720." },
  { "en": "Bagaimana Anda mendefinisikan 'kebahagiaan' sebagai seorang polisi?", "id": "Kebahagiaan adalah perasaan damai di hati saat pulang ke rumah, karena tahu bahwa hari itu saya telah bekerja dengan jujur dan memberikan yang terbaik untuk melindungi masyarakat." },
  { "en": "Apa pertanyaan paling fundamental yang harus dijawab oleh sistem hukum?", "id": "'Bagaimana cara menyeimbangkan antara ketertiban umum dengan kebebasan individu secara adil?'." },
  { "en": "Anda mengetahui sebuah operasi besar akan gagal karena perencanaan yang buruk, namun menyuarakannya akan membuat Anda dicap 'tidak loyal' oleh pimpinan yang merancangnya. Apa yang Anda lakukan?", "id": "Saya akan tetap menyampaikannya secara tertulis dan terukur, dengan data dan analisis risiko yang objektif. Keselamatan anggota dan keberhasilan misi lebih penting daripada citra loyalitas pribadi. Ini adalah bentuk loyalitas yang sesungguhnya kepada institusi." },
  { "en": "Seorang penjahat kelas kakap menawarkan informasi untuk membongkar jaringan yang lebih besar, dengan syarat keluarganya dijamin keamanannya dari ancaman rivalnya. Apakah Anda akan menyetujuinya?", "id": "Ya, dengan persetujuan pimpinan dan koordinasi dengan lembaga perlindungan saksi. Ini adalah langkah taktis yang dibenarkan (prinsip 'the lesser evil') untuk mencapai kebaikan yang lebih besar, yaitu membongkar seluruh jaringan." },
  { "en": "Apa perbedaan antara 'keadilan retributif' (menghukum) dan 'keadilan restoratif' (memulihkan)? Kapan Anda akan cenderung pada yang satu daripada yang lain?", "id": "Retributif fokus pada hukuman setimpal bagi pelaku. Restoratif fokus pada pemulihan korban dan reintegrasi pelaku. Saya akan cenderung restoratif untuk kejahatan ringan dan pelaku pertama kali, dan retributif untuk kejahatan berat dan residivis." },
  { "en": "Anda melihat ada 'pahlawan kesiangan' di unit Anda, yaitu rekan yang baru muncul saat akhir pekerjaan dan mengambil semua pujian. Bagaimana Anda mengelola situasi ini secara bijak?", "id": "Saya akan fokus pada pembagian tugas yang jelas di awal dan sistem pelaporan progres yang transparan. Dengan begitu, kontribusi setiap anggota akan terdokumentasi dengan baik tanpa perlu konfrontasi langsung." },
  { "en": "Menurut Anda, apa 'tirani' terbesar dalam sebuah organisasi: tirani mayoritas atau tirani minoritas yang berkuasa?", "id": "Tirani minoritas yang berkuasa. Karena mereka memiliki wewenang untuk memaksakan kehendak tanpa perlu dukungan logis, sementara tirani mayoritas masih bisa diperdebatkan dan dikoreksi melalui dialog." },
  { "en": "Anda harus memilih antara dua anggota untuk tugas berbahaya: A sangat terampil tapi memiliki anak kecil, B belum berkeluarga tapi keterampilannya sedikit di bawah A. Siapa yang Anda pilih?", "id": "Saya akan memilih berdasarkan kompetensi teknis tertinggi, yaitu A. Namun, saya akan memastikan ada sistem pengamanan dan dukungan ekstra untuk memitigasi risiko, dan berbicara dengannya untuk memastikan kesiapan mentalnya." },
  { "en": "Bagaimana Anda menjelaskan kepada publik bahwa data statistik kejahatan yang 'menurun' tidak selalu berarti situasi 'lebih aman'?", "id": "Saya akan menjelaskan bahwa statistik hanya satu sisi. Rasa aman juga dipengaruhi oleh kejahatan-kejahatan kecil yang tidak terlapor atau ketertiban sosial. Keamanan sejati adalah kombinasi dari data yang baik dan persepsi aman dari masyarakat." },
  { "en": "Apa yang membedakan 'polisi yang humanis' dari 'polisi yang populis'?", "id": "Polisi humanis bertindak berdasarkan empati dan prinsip kemanusiaan dalam kerangka hukum. Polisi populis bertindak berdasarkan apa yang akan menyenangkan mayoritas, bahkan jika itu harus sedikit melanggar aturan." },
  { "en": "Anda mengetahui bahwa sebuah teknologi pengawasan baru yang sangat efektif juga berpotensi besar melanggar privasi warga. Bagaimana sikap Anda terhadap implementasinya?", "id": "Saya akan mendukung implementasinya dengan syarat ada protokol penggunaan yang sangat ketat, pengawasan berlapis, dan batasan yang jelas untuk mencegah penyalahgunaan, sehingga efektivitas dan perlindungan privasi bisa berjalan seimbang." },
  { "en": "Jika Anda bisa kembali ke masa lalu dan mengubah satu keputusan dalam sejarah kepolisian, keputusan apa itu dan mengapa?", "id": "Jawaban yang baik adalah yang menunjukkan pemahaman sejarah dan visi, misal: 'Saya akan mengubah keputusan untuk memisahkan polisi dari militer lebih awal, untuk membangun budaya sipil yang melayani, bukan budaya militeristik, sejak awal kemerdekaan'." },
  { "en": "Lengkapi analogi: ALGORITMA : KOMPUTASI = RESEP : ...", "id": "MASAKAN." },
  { "en": "Lengkapi analogi: GEN : PEWARISAN = MEMORI : ...", "id": "PEMBELAJARAN." },
  { "en": "Lengkapi analogi: KEBENARAN : FILSAFAT = BUKTI : ...", "id": "HUKUM." },
  { "en": "Lengkapi analogi: KEGELAPAN : KETIDAKTAHUAN = CAHAYA : ...", "id": "PENGETAHUAN." },
  { "en": "Lengkapi analogi: SIMPUL : TALI = KESEPAKATAN : ...", "id": "PERJANJIAN." },
  { "en": "Lengkapi analogi: KAPITAL : MODAL = INSAN : ...", "id": "MANUSIA." },
  { "en": "Lengkapi analogi: DOKTRIN : AJARAN = DOGMA : ...", "id": "KEYAKINAN (yang tak terbantahkan)." },
  { "en": "Lengkapi analogi: KACA : RAPUH = BAJA : ...", "id": "KOKOH." },
  { "en": "Lengkapi analogi: PENGALAMAN : GURU = KEGAGALAN : ...", "id": "PELAJARAN." },
  { "en": "Lengkapi analogi: ATOM : FISIKA = SEL : ...", "id": "BIOLOGI." },
  { "en": "Lanjutkan deret angka: 1, 5, 13, 29, 61, 125, ...", "id": "253 (pola: x2 + 3)." },
  { "en": "Lanjutkan deret angka: 2, 4, 5, 10, 12, 24, 27, ...", "id": "54 (pola: x2, +1, x2, +2, x2, +3, x2)." },
  { "en": "Lanjutkan deret huruf: A, C, E, G, I, ...", "id": "K (pola: deret huruf vokal)." },
  { "en": "Lanjutkan deret angka: 1, 2, 6, 21, 88, ...", "id": "445 (pola: x1+1, x2+2, x3+3, x4+4, x5+5)." },
  { "en": "Lanjutkan deret angka: 100, 99, 97, 94, 90, 85, ...", "id": "79 (pola: -1, -2, -3, -4, -5, -6)." },
  { "en": "Lanjutkan deret angka: 3, 4, 6, 9, 13, ...", "id": "18 (pola: +1, +2, +3, +4, +5)." },
  { "en": "Lanjutkan deret huruf: A, F, K, P, U, Z, E, ...", "id": "J (pola: loncat 4 huruf, berputar)." },
  { "en": "Lanjutkan deret angka: 1, 3, 9, 27, 81, ...", "id": "243 (pola: dikali 3)." },
  { "en": "Lanjutkan deret angka: 1, 4, 2, 5, 3, 6, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 4,5,6,...)." },
  { "en": "Lanjutkan deret angka: 1, 2, 4, 7, 13, 24, 44, ...", "id": "81 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Apa sinonim dari kata 'ADITAMA'?", "id": "Terbaik atau utama." },
  { "en": "Apa sinonim dari kata 'BAIDURI'?", "id": "Permata." },
  { "en": "Apa sinonim dari kata 'CENGKAR'?", "id": "Gersang atau tandus." },
  { "en": "Apa sinonim dari kata 'DEDAL'?", "id": "Kotoran atau ampas." },
  { "en": "Apa sinonim dari kata 'FATWA'?", "id": "Nasihat atau keputusan hukum agama." },
  { "en": "Apa antonim dari kata 'IKHTIAR'?", "id": "Pasrah." },
  { "en": "Apa antonim dari kata 'JAHIL'?", "id": "Berilmu." },
  { "en": "Apa antonim dari kata 'KASA'?", "id": "Rapat atau padat." },
  { "en": "Apa antonim dari kata 'LAKONIK'?", "id": "Bertele-tele." },
  { "en": "Apa antonim dari kata 'MUALIM'?", "id": "Murid." },
  { "en": "Jika pernyataan 'Tidak ada politisi yang jujur' adalah salah, maka yang pasti benar adalah...", "id": "Beberapa politisi jujur." },
  { "en": "Sebuah jam dinding maju 5 menit setiap jamnya. Jika disetel benar pada pukul 07:00 pagi, pukul berapa yang ditunjukkannya saat waktu sebenarnya pukul 13:00 siang?", "id": "Pukul 13:30." },
  { "en": "Semua A adalah B. Tidak ada B yang merupakan C. Kesimpulan yang pasti benar adalah...", "id": "Tidak ada A yang merupakan C." },
  { "en": "Seorang pembohong berkata, 'Saya sedang berbohong'. Apakah ia jujur atau bohong?", "id": "Ini adalah paradoks pembohong (liar's paradox)." },
  { "en": "Jika Anda memiliki 2 koin, dan salah satunya pasti memiliki dua sisi gambar. Anda mengambil satu koin secara acak dan sisi yang muncul adalah gambar. Berapa peluang sisi lainnya juga gambar?", "id": "2/3." },
  { "en": "Berapakah 12% dari 120?", "id": "14.4." },
  { "en": "Jika 1/a - 1/b = 1/c, maka c = ...", "id": "ab / (b-a)." },
  { "en": "Sebuah bola dijatuhkan dari ketinggian 10 meter dan memantul kembali 3/4 dari ketinggian sebelumnya. Berapa total jarak yang ditempuh bola sampai berhenti?", "id": "70 meter." },
  { "en": "Berapakah nilai dari 1 + 2 - 3 + 4 + 5 - 6 + ... + 97 + 98 - 99?", "id": "1584." },
  { "en": "Sebuah roda berdiameter 49 cm. Berapa kali ia harus berputar untuk menempuh jarak 154 meter?", "id": "100 kali." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda harus menegakkan hukum pada orang yang jelas-jelas mengalami gangguan jiwa?", "id": "Saya akan mengamankannya dengan cara yang paling humanis untuk mencegah ia membahayakan diri sendiri atau orang lain, lalu segera berkoordinasi dengan unit PPA atau dinas kesehatan jiwa untuk penanganan medis, bukan penegakan hukum pidana." },
  { "en": "Apa perbedaan antara 'keadilan prosedural' dan 'keadilan substantif'?", "id": "Keadilan prosedural berarti proses hukumnya berjalan adil dan sesuai aturan. Keadilan substantif berarti hasil akhir dari proses hukum itu sendiri terasa adil dan sesuai dengan rasa kebenaran." },
  { "en": "Bagaimana Anda memulihkan kepercayaan diri setelah membuat kesalahan yang menyebabkan rekan Anda terluka?", "id": "Dengan bertanggung jawab penuh, meminta maaf secara tulus, memastikan rekan saya mendapat perawatan terbaik, dan menjadikan insiden itu sebagai pelajaran paling keras untuk tidak pernah mengulangi kelalaian yang sama." },
  { "en": "Menurut Anda, apa 'dosa intelektual' terbesar bagi seorang penyidik?", "id": "Confirmation bias, yaitu kecenderungan untuk hanya mencari dan menafsirkan bukti yang mendukung hipotesis awal, dan mengabaikan bukti yang bertentangan." },
  { "en": "Bagaimana Anda mendefinisikan 'kesetiaan' yang benar kepada negara?", "id": "Kesetiaan kepada negara berarti setia pada konstitusi dan nilai-nilai luhur bangsa, bukan pada rezim atau individu yang sedang berkuasa. Termasuk di dalamnya adalah berani mengoreksi jika negara berjalan ke arah yang salah." },
  { "en": "Apa yang akan Anda lakukan jika Anda merasa bahwa sistem hukum itu sendiri yang tidak adil?", "id": "Saya akan tetap bekerja di dalam sistem tersebut untuk menegakkan bagian-bagian yang masih bisa ditegakkan secara adil, sambil menggunakan pengetahuan saya untuk menyuarakan perlunya reformasi hukum melalui jalur-jalur yang konstitusional." },
  { "en": "Bagaimana Anda menjelaskan kepada seorang junior tentang pentingnya 'menjaga kata-kata' di era digital?", "id": "Saya akan jelaskan bahwa 'jejak digital itu abadi'. Satu komentar atau unggahan yang ceroboh bisa menghancurkan karir dan citra institusi yang dibangun bertahun-tahun, jadi berpikirlah seribu kali sebelum mengetik." },
  { "en": "Jika Anda bisa memiliki kemampuan untuk melihat 5 menit ke masa depan, bagaimana Anda akan menggunakannya dalam tugas?", "id": "Saya akan menggunakannya untuk tindakan preventif: mengantisipasi kecelakaan, mencegah serangan mendadak, dan mengevakuasi orang sebelum bahaya terjadi. Bukan untuk menangkap, tapi untuk melindungi." },
  { "en": "Apa yang lebih sulit: membangun sistem yang baik atau mencari orang baik untuk mengisi sistem?", "id": "Membangun sistem yang baik. Karena sistem yang baik dapat memaksa orang yang tidak terlalu baik untuk bertindak benar, sementara sistem yang buruk dapat merusak orang yang paling baik sekalipun." },
  { "en": "Bagaimana Anda mendefinisikan 'keberhasilan' tertinggi dalam hidup Anda, melampaui profesi?", "id": "Keberhasilan tertinggi adalah ketika saya bisa meninggalkan warisan nama baik dan menjadi inspirasi bagi anak-anak saya untuk menjadi manusia yang jujur, adil, dan bermanfaat bagi sesama." },
  { "en": "Lengkapi analogi: KESIMPULAN : PREMIS = ATAP : ...", "id": "FONDASI." },
  { "en": "Lengkapi analogi: METAFORA : KIASAN = HIPERBOLA : ...", "id": "BERLEBIHAN." },
  { "en": "Lengkapi analogi: KEGIGIHAN : TUJUAN = KESETIAAN : ...", "id": "KOMITMEN." },
  { "en": "Lengkapi analogi: ALIRAN : SUNGAI = ARUS : ...", "id": "LISTRIK." },
  { "en": "Lengkapi analogi: POHON : HUTAN = TENTARA : ...", "id": "PASUKAN." },
  { "en": "Lanjutkan deret angka: 1, 2, 5, 14, 41, 122, ...", "id": "365 (pola: x3 - 1)." },
  { "en": "Lanjutkan deret angka: 1, 8, 81, 1024, ...", "id": "15625 (pola: n pangkat (n+1) -> 1², 2³, 3⁴, 4⁵, 5⁶)." },
  { "en": "Lanjutkan deret huruf: A, Z, Y, B, C, X, W, D, E, F, ...", "id": "V (pola: 1 maju, 2 mundur, 2 maju, 3 mundur, 3 maju, 4 mundur...)." },
  { "en": "Lanjutkan deret angka: 2, 3, 6, 11, 20, 37, ...", "id": "68 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Lanjutkan deret angka: 1, 2, 2, 3, 3, 3, 4, 4, 4, ...", "id": "4 (pola: angka n ditulis sebanyak n kali)." },
  { "en": "Apa sinonim dari kata 'INCOGNITO'?", "id": "Menyamar." },
  { "en": "Apa sinonim dari kata 'JENGGALA'?", "id": "Hutan." },
  { "en": "Apa sinonim dari kata 'KILAH'?", "id": "Dalih atau alasan." },
  { "en": "Apa antonim dari kata 'MUKJIZAT'?", "id": "Hal biasa." },
  { "en": "Apa antonim dari kata 'NANAR'?", "id": "Fokus atau sadar." },
  { "en": "Berapakah 125% dari 64?", "id": "80." },
  { "en": "Sebuah kereta melaju dengan kecepatan 120 km/jam. Berapa meter yang ditempuh dalam 1 menit?", "id": "2000 meter." },
  { "en": "Jika 1/x + 1/x + 1/x = 12, berapakah nilai x?", "id": "1/4." },
  { "en": "Berapakah median dari data: 9, 2, 8, 3, 7, 4, 6, 5?", "id": "5.5." },
  { "en": "Sebuah segitiga sama kaki memiliki keliling 36 cm. Jika panjang sisi kakinya 13 cm, berapa luasnya?", "id": "60 cm²." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda harus memilih antara menyelamatkan rekan kerja atau seorang warga sipil dalam situasi berbahaya?", "id": "Ini dilema terberat. Sesuai doktrin, prioritas utama adalah keselamatan masyarakat. Saya akan menyelamatkan warga sipil, sambil berusaha semaksimal mungkin memberikan perlindungan atau bantuan kepada rekan saya." },
  { "en": "Apa perbedaan antara 'menjadi benar' (being right) dan 'melakukan yang benar' (doing the right thing)?", "id": "Menjadi benar seringkali tentang memenangkan argumen atau ego. Melakukan yang benar adalah tentang memilih tindakan yang paling etis dan bermanfaat, bahkan jika itu berarti harus mengalah atau mengakui kesalahan." },
  { "en": "Bagaimana Anda mengatasi 'sinisme' yang mungkin tumbuh setelah melihat sisi terburuk dari kemanusiaan setiap hari?", "id": "Dengan secara sadar mencari dan fokus pada kebaikan yang juga ada: pada rekan yang jujur, pada warga yang membantu, pada korban yang berhasil diselamatkan. Kebaikan harus secara aktif dicari agar tidak tenggelam oleh keburukan." },
  { "en": "Menurut Anda, apa satu hal yang paling sering dilupakan oleh polisi setelah lama bertugas?", "id": "Bagaimana rasanya menjadi warga sipil biasa yang takut dan bingung saat berhadapan dengan aparat. Kehilangan perspektif itu berbahaya." },
  { "en": "Jika Anda bisa meninggalkan satu kalimat di batu nisan Anda yang menggambarkan seluruh pengabdian Anda, kalimat apa itu?", "id": "Jawaban yang baik adalah yang singkat, kuat, dan merangkum filosofi hidup, misal: 'Ia Berusaha Menjaga Garis Antara Keteraturan dan Kekacauan'." },
  { "en": "Jika sebuah kapal tenggelam, mana yang Anda selamatkan lebih dulu: kapten, peta, atau kompas?", "id": "Kapten, karena pengetahuannya (termasuk isi peta dan cara menggunakan kompas) adalah aset paling berharga untuk kelangsungan hidup." },
  { "en": "Semua X adalah Y. Sebagian Y bukan Z. Kesimpulannya?", "id": "Tidak dapat disimpulkan hubungan antara X dan Z." },
  { "en": "Lengkapi analogi: IMAN : KEYAKINAN = ILMU : ...", "id": "PEMAHAMAN." },
  { "en": "Lanjutkan deret: 1, 3, 2, 4, 3, 5, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 3,4,5,...)." },
  { "en": "Apa sinonim dari kata 'OBLIGASI'?", "id": "Surat utang." },
  { "en": "Apa antonim dari kata 'PAKSA'?", "id": "Sukarela." },
  { "en": "Anda mengetahui bahwa atasan Anda akan membuat keputusan strategis yang fatal berdasarkan data yang salah. Anda sudah mencoba memberitahu tapi diabaikan. Apa langkah terakhir Anda?", "id": "Saya akan membuat memo resmi yang mendokumentasikan analisis dan data saya, lalu mengirimkannya melalui jalur hierarki. Ini adalah cara terakhir untuk melepaskan tanggung jawab profesional saya dan memastikan keberatan saya tercatat secara formal." },
  { "en": "Berapa jumlah sudut interior sebuah segi enam (heksagon)?", "id": "720 derajat." },
  { "en": "Bagaimana Anda mendefinisikan 'jiwa' dari institusi Kepolisian?", "id": "Jiwanya adalah 'Pelayanan'. Tanpa jiwa untuk melayani, institusi ini hanya akan menjadi mesin penegak hukum yang dingin dan ditakuti, bukan dicintai dan dipercaya." },
  { "en": "Apa pertanyaan paling penting yang harus dijawab oleh seorang calon polisi sebelum ia bergabung?", "id": "'Apakah saya benar-benar siap untuk menempatkan kepentingan orang lain di atas kepentingan saya sendiri, setiap hari?'." },
  { "en": "Anda mengetahui ada sebuah 'kesalahan sistemik' yang menyebabkan banyak anggota baik terpaksa melakukan pelanggaran administratif kecil. Apa yang lebih prioritas: menindak anggotanya atau memperjuangkan perbaikan sistemnya?", "id": "Memperjuangkan perbaikan sistemnya. Menindak anggota hanya akan memadamkan 'gejala' tanpa menyembuhkan 'penyakitnya'. Memperbaiki akar masalah akan menyelamatkan lebih banyak orang di masa depan." },
  { "en": "Seorang informan memberikan data akurat tentang jaringan teroris, namun Anda tahu ia sendiri adalah pelaku kejahatan lain. Bagaimana Anda mengelola paradoks ini?", "id": "Saya akan memisahkan dua hal. Informasi intelijennya akan saya gunakan untuk membongkar jaringan teroris demi keselamatan publik yang lebih luas. Secara terpisah, proses hukum atas kejahatannya sendiri harus tetap berjalan." },
  { "en": "Bagaimana Anda membedakan antara 'menjadi manusiawi' dengan 'menjadi lemah' dalam konteks penegakan hukum?", "id": "Menjadi manusiawi adalah menggunakan empati dan kebijaksanaan dalam menerapkan hukum. Menjadi lemah adalah membiarkan perasaan atau rasa takut menghalangi penegakan hukum yang seharusnya dilakukan." },
  { "en": "Anda melihat seorang pimpinan yang sangat visioner tetapi cara komunikasinya seringkali menyakitkan dan 'membunuh' motivasi tim. Apa yang bisa Anda lakukan?", "id": "Saya akan mencoba menjadi 'penerjemah' visinya kepada tim dengan bahasa yang lebih positif dan memotivasi. Kepada pimpinan, saya akan mencari cara untuk memberikan masukan tentang dampak komunikasinya secara halus." },
  { "en": "Apa arti 'keadilan' bagi seorang korban yang keluarganya terbunuh, di mana hukuman seberat apapun tidak akan mengembalikan nyawa?", "id": "Keadilan dalam konteks ini bukan hanya tentang hukuman bagi pelaku, tetapi juga tentang pengakuan atas penderitaan korban, pemulihan hak-haknya, dan jaminan dari negara bahwa kejadian serupa tidak akan terulang." },
  { "en": "Anda berada dalam situasi di mana mengikuti prosedur akan membahayakan nyawa tim Anda, sementara melanggarnya berpotensi menyelamatkan mereka. Keputusan apa yang Anda ambil sebagai pemimpin lapangan?", "id": "Keselamatan nyawa tim adalah prioritas tertinggi. Saya akan mengambil keputusan untuk melanggar prosedur dengan risiko yang terukur, dan saya siap mempertanggungjawabkan keputusan tersebut sepenuhnya kepada pimpinan setelahnya." },
  { "en": "Bagaimana Anda menjelaskan kepada diri sendiri dan keluarga tentang 'panggilan jiwa' untuk profesi yang seringkali tidak dihargai dan penuh risiko ini?", "id": "Saya menjelaskannya sebagai sebuah kesempatan langka untuk berada di garis depan dalam menjaga keteraturan dan menolong orang pada saat terlemah mereka. Kepuasan batin dari pengabdian ini tidak bisa dinilai dengan materi atau penghargaan." },
  { "en": "Menurut Anda, apakah mungkin seorang polisi tetap idealis setelah 20 tahun bertugas? Jika ya, bagaimana caranya?", "id": "Mungkin, dengan cara terus menerus 'mengkalibrasi ulang' niat awal, memiliki lingkaran pertemanan yang positif, dan secara sadar mencari serta merayakan setiap kebaikan kecil yang berhasil dilakukan setiap hari." },
  { "en": "Anda harus memilih satu dari dua tugas: A) Mengamankan aset negara bernilai triliunan, atau B) Menyelamatkan satu desa dari konflik komunal. Mana yang Anda pilih?", "id": "Saya memilih menyelamatkan desa dari konflik komunal. Keselamatan nyawa manusia tidak ternilai dan harus selalu menjadi prioritas di atas aset materi, sebesar apapun nilainya." },
  { "en": "Apa satu hal yang Anda harap masyarakat umum tidak akan pernah tahu tentang sisi gelap pekerjaan polisi, dan mengapa?", "id": "Beban psikologis dan trauma yang sering kami bawa pulang. Bukan untuk menutupi, tetapi untuk melindungi keluarga kami dan menjaga persepsi bahwa kami selalu kuat dan siap melindungi mereka kapan saja." },
  { "en": "Lengkapi analogi: KONSTITUSI : PREAMBUL = BUKU : ...", "id": "KATA PENGANTAR." },
  { "en": "Lengkapi analogi: SINAPSIS : NEURON = JEMBATAN : ...", "id": "DARATAN (atau SUNGAI)." },
  { "en": "Lengkapi analogi: EROSI : TANAH = KOROSI : ...", "id": "LOGAM." },
  { "en": "Lengkapi analogi: PENGHAPUS : KESALAHAN TULIS = PERMINTAAN MAAF : ...", "id": "KESALAHAN PERBUATAN." },
  { "en": "Lengkapi analogi: DNA : INFORMASI GENETIK = NOT BALOK : ...", "id": "INFORMASI MUSIK." },
  { "en": "Lengkapi analogi: KEADILAN : HUKUM = KESEHATAN : ...", "id": "KEDOKTERAN." },
  { "en": "Lengkapi analogi: BAYANGAN : CAHAYA = GEMA : ...", "id": "SUARA." },
  { "en": "Lengkapi analogi: KATA : BAHASA = PIKSEL : ...", "id": "GAMBAR." },
  { "en": "Lengkapi analogi: AKAR : POHON = FONDASI : ...", "id": "GEDUNG." },
  { "en": "Lengkapi analogi: KERING : GURUN = LEMBAB : ...", "id": "HUTAN HUJAN." },
  { "en": "Lanjutkan deret angka: 1, 3, 4, 7, 11, 18, 29, ...", "id": "47 (pola: deret Lucas)." },
  { "en": "Lanjutkan deret angka: 2, 6, 10, 30, 34, 102, ...", "id": "106 (pola: x3, +4, diulang)." },
  { "en": "Lanjutkan deret huruf: A, C, F, J, O, ...", "id": "U (pola loncat: +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 1, 4, 10, 22, 46, ...", "id": "94 (pola: x2 + 2)." },
  { "en": "Lanjutkan deret angka: 3, 2, 5, 4, 7, 6, ...", "id": "9 (pola dua larik: 3,5,7,9,... dan 2,4,6,...)." },
  { "en": "Lanjutkan deret angka: 1, 8, 27, 64, 125, ...", "id": "216 (pola: bilangan kubik)." },
  { "en": "Lanjutkan deret huruf: A, K, B, L, C, M, ...", "id": "D (pola dua larik: A,B,C,D,... dan K,L,M,...)." },
  { "en": "Lanjutkan deret angka: 1, 2, 4, 7, 12, 20, ...", "id": "33 (pola penambahan: +1,+2,+3,+5,+8 -> deret Fibonacci)." },
  { "en": "Lanjutkan deret angka: 1, 1, 1, 2, 3, 4, 6, ...", "id": "9 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Lanjutkan deret angka: 100, 1, 99, 2, 98, 3, ...", "id": "97 (pola dua larik: 100,99,98,97,... dan 1,2,3,...)." },
  { "en": "Apa sinonim dari kata 'AFIRMASI'?", "id": "Penegasan atau pengakuan." },
  { "en": "Apa sinonim dari kata 'BIBLIOFILIA'?", "id": "Cinta terhadap buku." },
  { "en": "Apa sinonim dari kata 'DEMAGOG'?", "id": "Penghasut rakyat." },
  { "en": "Apa sinonim dari kata 'EFEMERAL'?", "id": "Sesaat atau singkat." },
  { "en": "Apa sinonim dari kata 'FILANTROP'?", "id": "Dermawan." },
  { "en": "Apa antonim dari kata 'KONKURENSI'?", "id": "Monopoli." },
  { "en": "Apa antonim dari kata 'LIBERAL'?", "id": "Konservatif." },
  { "en": "Apa antonim dari kata 'MAJAL'?", "id": "Tajam." },
  { "en": "Apa antonim dari kata 'NIRWANA'?", "id": "Dunia fana atau samsara." },
  { "en": "Apa antonim dari kata 'OPAS'?", "id": "Majikan." },
  { "en": "Jika pernyataan 'Tidak semua burung bisa terbang' adalah benar, maka yang pasti salah adalah...", "id": "Pernyataan 'Semua burung bisa terbang'." },
  { "en": "Seorang ayah berkata pada anaknya: 'Saat aku seusiamu, usiaku adalah tiga kali usiamu saat itu'. Jika usia ayah sekarang 48 tahun, dan perbandingan usia mereka sekarang 4:1. Berapa usia anak saat ini?", "id": "12 tahun." },
  { "en": "Semua A adalah B. Sebagian C adalah A. Kesimpulan yang pasti benar adalah...", "id": "Sebagian C adalah B." },
  { "en": "Di sebuah pulau, ada ksatria (selalu jujur) dan penjahat (selalu bohong). Anda bertemu 2 orang, A dan B. A berkata 'Setidaknya salah satu dari kami adalah penjahat'. Siapakah A dan B?", "id": "A adalah Ksatria, B adalah Penjahat." },
  { "en": "Jika sebuah buku diletakkan di atas meja, maka...", "id": "Buku memberikan gaya ke bawah, dan meja memberikan gaya ke atas yang sama besar." },
  { "en": "Berapakah 1/6 dari 1 hari dalam jam?", "id": "4 jam." },
  { "en": "Jika 1/x = 3, berapakah nilai dari 1/(x+1)?", "id": "3/4." },
  { "en": "Sebuah bakteri membelah diri menjadi dua setiap 3 menit. Berapa banyak bakteri yang ada setelah setengah jam jika dimulai dari satu bakteri?", "id": "1024 (2 pangkat 10)." },
  { "en": "Berapakah sudut yang dibentuk oleh jarum jam pada pukul 20:00?", "id": "120 derajat." },
  { "en": "Sebuah dadu dan sebuah koin dilempar bersamaan. Berapa peluang munculnya angka genap pada dadu dan gambar pada koin?", "id": "3/12 atau 1/4." },
  { "en": "Bagaimana Anda menyikapi situasi di mana Anda harus menegakkan hukum pada orang yang sangat berjasa pada Anda di masa lalu?", "id": "Saya akan memisahkan rasa terima kasih pribadi dengan tugas profesional. Hukum harus ditegakkan. Saya akan melakukannya dengan cara yang paling hormat, dan setelah proses selesai, saya akan menemuinya sebagai pribadi." },
  { "en": "Apa perbedaan antara 'memiliki power' dan 'memiliki authority'?", "id": "Authority adalah hak formal untuk memerintah (wewenang). Power adalah kemampuan riil untuk mempengaruhi, yang bisa datang dari wewenang, pengetahuan, atau karisma. Tidak semua yang punya authority punya power, dan sebaliknya." },
  { "en": "Bagaimana Anda mengatasi 'sinisme' yang mungkin tumbuh setelah melihat sisi terburuk dari kemanusiaan setiap hari?", "id": "Dengan secara sadar mencari dan fokus pada kebaikan yang juga ada: pada rekan yang jujur, pada warga yang membantu, pada korban yang berhasil diselamatkan. Kebaikan harus secara aktif dicari agar tidak tenggelam oleh keburukan." },
  { "en": "Menurut Anda, apa satu hal yang paling sering dilupakan oleh polisi setelah lama bertugas?", "id": "Bagaimana rasanya menjadi warga sipil biasa yang takut dan bingung saat berhadapan dengan aparat. Kehilangan perspektif itu berbahaya." },
  { "en": "Jika Anda bisa meninggalkan satu kalimat di batu nisan Anda yang menggambarkan seluruh pengabdian Anda, kalimat apa itu?", "id": "Jawaban yang baik adalah yang singkat, kuat, dan merangkum filosofi hidup, misal: 'Ia Berusaha Menjaga Garis Antara Keteraturan dan Kekacauan'." },
  { "en": "Jika sebuah kapal tenggelam, mana yang Anda selamatkan lebih dulu: kapten, peta, atau kompas?", "id": "Kapten, karena pengetahuannya (termasuk isi peta dan cara menggunakan kompas) adalah aset paling berharga untuk kelangsungan hidup." },
  { "en": "Semua X adalah Y. Sebagian Y bukan Z. Kesimpulannya?", "id": "Tidak dapat disimpulkan hubungan antara X dan Z." },
  { "en": "Lengkapi analogi: IMAN : KEYAKINAN = ILMU : ...", "id": "PEMAHAMAN." },
  { "en": "Lanjutkan deret: 1, 3, 2, 4, 3, 5, ...", "id": "4 (pola dua larik: 1,2,3,4,... dan 3,4,5,...)." },
  { "en": "Apa sinonim dari kata 'OBLIGASI'?", "id": "Surat utang." },
  { "en": "Apa antonim dari kata 'PAKSA'?", "id": "Sukarela." },
  { "en": "Anda mengetahui bahwa atasan Anda akan membuat keputusan strategis yang fatal berdasarkan data yang salah. Anda sudah mencoba memberitahu tapi diabaikan. Apa langkah terakhir Anda?", "id": "Saya akan membuat memo resmi yang mendokumentasikan analisis dan data saya, lalu mengirimkannya melalui jalur hierarki. Ini adalah cara terakhir untuk melepaskan tanggung jawab profesional saya dan memastikan keberatan saya tercatat secara formal." },
  { "en": "Berapa jumlah sudut interior sebuah segi enam (heksagon)?", "id": "720 derajat." },
  { "en": "Bagaimana Anda mendefinisikan 'jiwa' dari institusi Kepolisian?", "id": "Jiwanya adalah 'Pelayanan'. Tanpa jiwa untuk melayani, institusi ini hanya akan menjadi mesin penegak hukum yang dingin dan ditakuti, bukan dicintai dan dipercaya." },
  { "en": "Apa pertanyaan paling penting yang harus dijawab oleh seorang calon polisi sebelum ia bergabung?", "id": "'Apakah saya benar-benar siap untuk menempatkan kepentingan orang lain di atas kepentingan saya sendiri, setiap hari?'." },
  { "en": "Anda mengetahui ada sebuah 'kesalahan sistemik' yang menyebabkan banyak anggota baik terpaksa melakukan pelanggaran administratif kecil. Apa yang lebih prioritas: menindak anggotanya atau memperjuangkan perbaikan sistemnya?", "id": "Memperjuangkan perbaikan sistemnya. Menindak anggota hanya akan memadamkan 'gejala' tanpa menyembuhkan 'penyakitnya'. Memperbaiki akar masalah akan menyelamatkan lebih banyak orang di masa depan." },
  { "en": "Seorang informan memberikan data akurat tentang jaringan teroris, namun Anda tahu ia sendiri adalah pelaku kejahatan lain. Bagaimana Anda mengelola paradoks ini?", "id": "Saya akan memisahkan dua hal. Informasi intelijennya akan saya gunakan untuk membongkar jaringan teroris demi keselamatan publik yang lebih luas. Secara terpisah, proses hukum atas kejahatannya sendiri harus tetap berjalan." },
  { "en": "Bagaimana Anda membedakan antara 'menjadi manusiawi' dengan 'menjadi lemah' dalam konteks penegakan hukum?", "id": "Menjadi manusiawi adalah menggunakan empati dan kebijaksanaan dalam menerapkan hukum. Menjadi lemah adalah membiarkan perasaan atau rasa takut menghalangi penegakan hukum yang seharusnya dilakukan." },
  { "en": "Anda melihat seorang pimpinan yang sangat visioner tetapi cara komunikasinya seringkali menyakitkan dan 'membunuh' motivasi tim. Apa yang bisa Anda lakukan?", "id": "Saya akan mencoba menjadi 'penerjemah' visinya kepada tim dengan bahasa yang lebih positif dan memotivasi. Kepada pimpinan, saya akan mencari cara untuk memberikan masukan tentang dampak komunikasinya secara halus." },
  { "en": "Apa arti 'keadilan' bagi seorang korban yang keluarganya terbunuh, di mana hukuman seberat apapun tidak akan mengembalikan nyawa?", "id": "Keadilan dalam konteks ini bukan hanya tentang hukuman bagi pelaku, tetapi juga tentang pengakuan atas penderitaan korban, pemulihan hak-haknya, dan jaminan dari negara bahwa kejadian serupa tidak akan terulang." },
  { "en": "Anda berada dalam situasi di mana mengikuti prosedur akan membahayakan nyawa tim Anda, sementara melanggarnya berpotensi menyelamatkan mereka. Keputusan apa yang Anda ambil sebagai pemimpin lapangan?", "id": "Keselamatan nyawa tim adalah prioritas tertinggi. Saya akan mengambil keputusan untuk melanggar prosedur dengan risiko yang terukur, dan saya siap mempertanggungjawabkan keputusan tersebut sepenuhnya kepada pimpinan setelahnya." },
  { "en": "Bagaimana Anda menjelaskan kepada diri sendiri dan keluarga tentang 'panggilan jiwa' untuk profesi yang seringkali tidak dihargai dan penuh risiko ini?", "id": "Saya menjelaskannya sebagai sebuah kesempatan langka untuk berada di garis depan dalam menjaga keteraturan dan menolong orang pada saat terlemah mereka. Kepuasan batin dari pengabdian ini tidak bisa dinilai dengan materi atau penghargaan." },
  { "en": "Menurut Anda, apakah mungkin seorang polisi tetap idealis setelah 20 tahun bertugas? Jika ya, bagaimana caranya?", "id": "Mungkin, dengan cara terus menerus 'mengkalibrasi ulang' niat awal, memiliki lingkaran pertemanan yang positif, dan secara sadar mencari serta merayakan setiap kebaikan kecil yang berhasil dilakukan setiap hari." },
  { "en": "Anda harus memilih satu dari dua tugas: A) Mengamankan aset negara bernilai triliunan, atau B) Menyelamatkan satu desa dari konflik komunal. Mana yang Anda pilih?", "id": "Saya memilih menyelamatkan desa dari konflik komunal. Keselamatan nyawa manusia tidak ternilai dan harus selalu menjadi prioritas di atas aset materi, sebesar apapun nilainya." },
  { "en": "Apa satu hal yang Anda harap masyarakat umum tidak akan pernah tahu tentang sisi gelap pekerjaan polisi, dan mengapa?", "id": "Beban psikologis dan trauma yang sering kami bawa pulang. Bukan untuk menutupi, tetapi untuk melindungi keluarga kami dan menjaga persepsi bahwa kami selalu kuat dan siap melindungi mereka kapan saja." },
  { "en": "Lengkapi analogi: KONSTITUSI : PREAMBUL = BUKU : ...", "id": "KATA PENGANTAR." },
  { "en": "Lengkapi analogi: SINAPSIS : NEURON = JEMBATAN : ...", "id": "DARATAN (atau SUNGAI)." },
  { "en": "Lengkapi analogi: EROSI : TANAH = KOROSI : ...", "id": "LOGAM." },
  { "en": "Lengkapi analogi: PENGHAPUS : KESALAHAN TULIS = PERMINTAAN MAAF : ...", "id": "KESALAHAN PERBUATAN." },
  { "en": "Lengkapi analogi: DNA : INFORMASI GENETIK = NOT BALOK : ...", "id": "INFORMASI MUSIK." },
  { "en": "Lengkapi analogi: KEADILAN : HUKUM = KESEHATAN : ...", "id": "KEDOKTERAN." },
  { "en": "Lengkapi analogi: BAYANGAN : CAHAYA = GEMA : ...", "id": "SUARA." },
  { "en": "Lengkapi analogi: KATA : BAHASA = PIKSEL : ...", "id": "GAMBAR." },
  { "en": "Lengkapi analogi: AKAR : POHON = FONDASI : ...", "id": "GEDUNG." },
  { "en": "Lengkapi analogi: KERING : GURUN = LEMBAB : ...", "id": "HUTAN HUJAN." },
  { "en": "Lanjutkan deret angka: 1, 3, 4, 7, 11, 18, 29, ...", "id": "47 (pola: deret Lucas)." },
  { "en": "Lanjutkan deret angka: 2, 6, 10, 30, 34, 102, ...", "id": "106 (pola: x3, +4, diulang)." },
  { "en": "Lanjutkan deret huruf: A, C, F, J, O, ...", "id": "U (pola loncat: +2, +3, +4, +5, +6)." },
  { "en": "Lanjutkan deret angka: 1, 4, 10, 22, 46, ...", "id": "94 (pola: x2 + 2)." },
  { "en": "Lanjutkan deret angka: 3, 2, 5, 4, 7, 6, ...", "id": "9 (pola dua larik: 3,5,7,9,... dan 2,4,6,...)." },
  { "en": "Lanjutkan deret angka: 1, 8, 27, 64, 125, ...", "id": "216 (pola: bilangan kubik)." },
  { "en": "Lanjutkan deret huruf: A, K, B, L, C, M, ...", "id": "D (pola dua larik: A,B,C,D,... dan K,L,M,...)." },
  { "en": "Lanjutkan deret angka: 1, 2, 4, 7, 12, 20, ...", "id": "33 (pola penambahan: +1,+2,+3,+5,+8 -> deret Fibonacci)." },
  { "en": "Lanjutkan deret angka: 1, 1, 1, 2, 3, 4, 6, ...", "id": "9 (pola: jumlah 3 angka sebelumnya)." },
  { "en": "Lanjutkan deret angka: 100, 1, 99, 2, 98, 3, ...", "id": "97 (pola dua larik: 100,99,98,97,... dan 1,2,3,...)." }



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
