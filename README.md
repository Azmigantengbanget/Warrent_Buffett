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



  { "en": "Siapa Warren Buffett?", "id": "Investor legendaris, CEO Berkshire Hathaway." },
  { "en": "Apa julukan Warren Buffett?", "id": "Oracle of Omaha." },
  { "en": "Filosofi investasi utamanya?", "id": "Value investing." },
  { "en": "Siapa guru investasi Buffett?", "id": "Benjamin Graham." },
  { "en": "Buku investasi favoritnya?", "id": "The Intelligent Investor." },
  { "en": "Aturan investasi nomor satu?", "id": "Jangan pernah kehilangan uang." },
  { "en": "Aturan investasi nomor dua?", "id": "Jangan lupakan aturan nomor satu." },
  { "en": "Apa itu 'value investing'?", "id": "Membeli saham di bawah nilai intrinsiknya." },
  { "en": "Apa itu 'nilai intrinsik'?", "id": "Nilai sejati sebuah perusahaan." },
  { "en": "Bagaimana cara menghitungnya?", "id": "Analisis fundamental arus kas masa depan." },
  { "en": "Apa itu 'margin of safety'?", "id": "Beli jauh di bawah nilai intrinsik." },
  { "en": "Tujuannya?", "id": "Melindungi dari kesalahan dan nasib buruk." },
  { "en": "Berapa lama horizon investasinya?", "id": "Selamanya." },
  { "en": "Periode memegang saham favoritnya?", "id": "Selamanya." },
  { "en": "Kapan waktu terbaik menjual?", "id": "Hampir tidak pernah." },
  { "en": "Apa yang harus dibeli?", "id": "Bisnis, bukan saham." },
  { "en": "Fokus pada pergerakan harga?", "id": "Tidak, fokus pada nilai bisnis." },
  { "en": "Apa itu 'Mr. Market'?", "id": "Analogi pasar saham yang emosional." },
  { "en": "Haruskah kita mengikuti Mr. Market?", "id": "Tidak, manfaatkan emosinya." },
  { "en": "Manfaatkan saat Mr. Market pesimis?", "id": "Ya, beli saat dia takut." },
  { "en": "Manfaatkan saat Mr. Market optimis?", "id": "Ya, pertimbangkan untuk menjual." },
  { "en": "Apa kunci utama berinvestasi?", "id": "Kesabaran dan disiplin." },
  { "en": "Apa itu 'circle of competence'?", "id": "Lingkaran kompetensi." },
  { "en": "Artinya?", "id": "Investasi hanya pada yang Anda pahami." },
  { "en": "Seberapa besar lingkarannya?", "id": "Ukuran tidak penting, batasnya yang penting." },
  { "en": "Bagaimana jika tidak paham teknologi?", "id": "Jangan berinvestasi di saham teknologi." },
  { "en": "Apa karakteristik bisnis hebat?", "id": "Keunggulan kompetitif yang tahan lama." },
  { "en": "Apa itu 'economic moat'?", "id": "Parit ekonomi pelindung bisnis." },
  { "en": "Contoh 'economic moat'?", "id": "Merek kuat, biaya beralih tinggi." },
  { "en": "Coca-Cola punya 'moat' apa?", "id": "Merek yang sangat kuat." },
  { "en": "Manajemen seperti apa yang disukai?", "id": "Jujur, kompeten, dan berintegritas." },
  { "en": "Apakah utang perusahaan penting?", "id": "Ya, hindari perusahaan dengan utang besar." },
  { "en": "Mengapa?", "id": "Risiko kebangkrutan lebih tinggi." },
  { "en": "Bagaimana dengan diversifikasi?", "id": "Diversifikasi adalah perlindungan terhadap kebodohan." },
  { "en": "Buffett suka diversifikasi luas?", "id": "Tidak, dia lebih suka konsentrasi." },
  { "en": "Artinya?", "id": "Berinvestasi besar pada beberapa perusahaan hebat." },
  { "en": "Apa kutipannya tentang kesabaran?", "id": "Pasar saham transfer uang ke orang sabar." },
  { "en": "Dari siapa?", "id": "Dari orang yang tidak sabar." },
  { "en": "Kualitas terpenting seorang investor?", "id": "Temperamen, bukan intelek." },
  { "en": "Bagaimana pandangannya tentang pasar?", "id": "Fluktuasi pasar adalah teman Anda." },
  { "en": "Bagaimana pandangannya tentang risiko?", "id": "Risiko datang dari tidak tahu apa." },
  { "en": "Apa yang Anda lakukan?", "id": "Ketahui apa yang Anda lakukan." },
  { "en": "Kapan harus serakah?", "id": "Saat orang lain takut." },
  { "en": "Kapan harus takut?", "id": "Saat orang lain serakah." },
  { "en": "Bagaimana dengan berita pasar?", "id": "Abaikan kebisingan jangka pendek." },
  { "en": "Fokus pada laporan tahunan?", "id": "Ya, baca dan pahami." },
  { "en": "Bagaimana memilih saham?", "id": "Seolah Anda membeli seluruh perusahaan." },
  { "en": "Pentingnya sejarah perusahaan?", "id": "Sangat penting untuk melihat konsistensi." },
  { "en": "Apa itu 'earning power'?", "id": "Kekuatan laba perusahaan." },
  { "en": "Buffett suka bisnis yang membosankan?", "id": "Ya, jika bisnisnya hebat." },
  { "en": "Mengapa?", "id": "Mudah dipahami dan diprediksi." },
  { "en": "Bagaimana dengan saham IPO?", "id": "Umumnya dihindari." },
  { "en": "Apa itu 'return on equity' (ROE)?", "id": "Ukuran profitabilitas perusahaan." },
  { "en": "Buffett suka ROE tinggi?", "id": "Ya, secara konsisten." },
  { "en": "Investasi adalah permainan apa?", "id": "Bukan permainan di mana orang bodoh menang." },
  { "en": "Bagaimana belajar investasi?", "id": "Baca, baca, dan baca." },
  { "en": "Apa itu 'compounding'?", "id": "Efek bola salju bunga berbunga." },
  { "en": "Kunci kekayaan Buffett?", "id": "Kekuatan compounding." },
  { "en": "Membutuhkan apa?", "id": "Waktu dan kesabaran." },
  { "en": "Apa itu 'opportunity cost'?", "id": "Biaya kesempatan." },
  { "en": "Apa investasi terbaik?", "id": "Investasi pada diri sendiri." },
  { "en": "Pentingnya reputasi?", "id": "Butuh 20 tahun membangun, 5 menit hancur." },
  { "en": "Bagaimana dengan prediksi pasar?", "id": "Peramal hanya membuat peramal kaya." },
  { "en": "Jadi, jangan prediksi pasar?", "id": "Benar, tidak ada yang bisa." },
  { "en": "Bagaimana dengan inflasi?", "id": "Bisnis hebat bisa mengatasi inflasi." },
  { "en": "Caranya?", "id": "Dengan 'pricing power'." },
  { "en": "Apa itu 'pricing power'?", "id": "Kemampuan menaikkan harga tanpa kehilangan bisnis." },
  { "en": "Bagaimana dengan emas?", "id": "Bukan investasi yang produktif." },
  { "en": "Mengapa?", "id": "Emas tidak menghasilkan apa-apa." },
  { "en": "Apa itu aset produktif?", "id": "Aset yang menghasilkan arus kas." },
  { "en": "Contohnya?", "id": "Bisnis, pertanian, real estat." },
  { "en": "Apa itu 'owner earnings'?", "id": "Konsep arus kas versi Buffett." },
  { "en": "Lebih baik dari laba bersih?", "id": "Ya, lebih mencerminkan realitas." },
  { "en": "Apa itu 'cigar butt' investing?", "id": "Investasi puntung rokok." },
  { "en": "Gaya siapa itu?", "id": "Gaya awal Benjamin Graham." },
  { "en": "Artinya?", "id": "Membeli perusahaan buruk dengan harga murah." },
  { "en": "Buffett masih menggunakannya?", "id": "Tidak lagi, sudah berevolusi." },
  { "en": "Gaya Buffett sekarang?", "id": "Beli perusahaan hebat harga wajar." },
  { "en": "Siapa yang mempengaruhinya?", "id": "Charlie Munger." },
  { "en": "Siapa Charlie Munger?", "id": "Wakil ketua Berkshire Hathaway." },
  { "en": "Apa itu 'wonderful company'?", "id": "Perusahaan hebat." },
  { "en": "Apa itu 'fair price'?", "id": "Harga yang wajar." },
  { "en": "Lebih baik mana?", "id": "Perusahaan hebat harga wajar." },
  { "en": "Daripada?", "id": "Perusahaan wajar harga hebat." },
  { "en": "Apa itu 'Berkshire Hathaway'?", "id": "Perusahaan holding milik Warren Buffett." },
  { "en": "Bentuknya seperti apa?", "id": "Konglomerat." },
  { "en": "Bagaimana dengan volatilitas?", "id": "Volatilitas bukanlah risiko." },
  { "en": "Risiko sebenarnya adalah?", "id": "Kehilangan modal secara permanen." },
  { "en": "Apa itu 'share buyback'?", "id": "Pembelian kembali saham oleh perusahaan." },
  { "en": "Apakah itu bagus?", "id": "Ya, jika harga saham murah." },
  { "en": "Bagaimana dengan dividen?", "id": "Dividen itu bagus." },
  { "en": "Mana lebih baik, buyback atau dividen?", "id": "Tergantung pada harga saham." },
  { "en": "Berkshire Hathaway membayar dividen?", "id": "Tidak." },
  { "en": "Mengapa?", "id": "Buffett bisa mengalokasikan modal lebih baik." },
  { "en": "Kunci sukses jangka panjang?", "id": "Berpikir seperti pemilik bisnis." },
  { "en": "Bagaimana dengan utang pribadi?", "id": "Hindari utang konsumtif." },
  { "en": "Pentingnya kejujuran?", "id": "Cari orang jujur, cerdas, energik." },
  { "en": "Jika tidak jujur?", "id": "Dua lainnya akan menghancurkanmu." },
  { "en": "Nasihat untuk investor muda?", "id": "Mulai berinvestasi sedini mungkin." },
  { "en": "Investasi di reksa dana indeks?", "id": "Pilihan terbaik untuk kebanyakan orang." },
  { "en": "Reksa dana indeks apa?", "id": "Reksa dana yang mengikuti indeks pasar." },
  { "en": "Contoh indeks?", "id": "S&P 500." },
  { "en": "Mengapa reksa dana indeks bagus?", "id": "Biaya rendah dan terdiversifikasi." },
  { "en": "Apa itu 'costly mistakes'?", "id": "Kesalahan mahal." },
  { "en": "Kesalahan terbesar investor?", "id": "Mencoba aktif saat tidak perlu." },
  { "en": "Bagaimana pandangannya tentang makroekonomi?", "id": "Abaikan prediksi makroekonomi." },
  { "en": "Fokus pada apa?", "id": "Mikroekonomi perusahaan." },
  { "en": "Apa itu 'look-through earnings'?", "id": "Porsi laba perusahaan investasi." },
  { "en": "Pentingnya arus kas bebas?", "id": "Sangat penting untuk nilai perusahaan." },
  { "en": "Apa itu 'free cash flow'?", "id": "Kas tersisa setelah biaya operasional." },
  { "en": "Bisnis ideal menghasilkan?", "id": "Banyak kas." },
  { "en": "Bagaimana dengan bisnis padat modal?", "id": "Umumnya dihindari." },
  { "en": "Apa itu 'capital intensive'?", "id": "Butuh banyak modal untuk beroperasi." },
  { "en": "Bagaimana cara menilai manajemen?", "id": "Baca surat tahunan untuk pemegang saham." },
  { "en": "Apa yang dicari?", "id": "Kejujuran, rasionalitas, dan keterbukaan." },
  { "en": "Manajemen harus berpikir seperti?", "id": "Pemilik bisnis." },
  { "en": "Bagaimana dengan opsi saham manajemen?", "id": "Bisa jadi insentif yang salah." },
  { "en": "Apa itu 'principal-agent problem'?", "id": "Konflik kepentingan antara manajemen dan pemilik." },
  { "en": "Investasi adalah tentang kepastian?", "id": "Ya, cari bisnis yang bisa diprediksi." },
  { "en": "Apa itu 'too hard pile'?", "id": "Tumpukan 'terlalu sulit'." },
  { "en": "Apa isinya?", "id": "Bisnis yang sulit dipahami." },
  { "en": "Tidak apa-apa melewatkan peluang?", "id": "Ya, Anda tidak perlu memukul setiap bola." },
  { "en": "Menunggu bola yang tepat?", "id": "Ya, tunggu di 'sweet spot' Anda." },
  { "en": "Apa itu 'sweet spot'?", "id": "Area di dalam lingkaran kompetensi." },
  { "en": "Pentingnya membaca laporan keuangan?", "id": "Itu adalah bahasa bisnis." },
  { "en": "Bagian mana yang paling penting?", "id": "Catatan kaki (footnotes)." },
  { "en": "Mengapa?", "id": "Seringkali berisi detail penting." },
  { "en": "Apa itu 'goodwill'?", "id": "Aset tak berwujud." },
  { "en": "Bagaimana pandangannya terhadap 'goodwill'?", "id": "Perhatikan dengan skeptis." },
  { "en": "Apa itu 'dollar-cost averaging' (DCA)?", "id": "Investasi rutin tanpa melihat harga." },
  { "en": "Strategi bagus untuk pemula?", "id": "Ya, sangat direkomendasikan." },
  { "en": "Apa itu 'psychology of investing'?", "id": "Psikologi investasi." },
  { "en": "Sangat penting?", "id": "Ya, lebih penting dari teknik." },
  { "en": "Bias perilaku yang umum?", "id": "Herding, overconfidence, loss aversion." },
  { "en": "Apa itu 'herding'?", "id": "Mengikuti keramaian." },
  { "en": "Apa itu 'overconfidence'?", "id": "Terlalu percaya diri." },
  { "en": "Apa itu 'loss aversion'?", "id": "Benci rugi lebih dari suka untung." },
  { "en": "Bagaimana cara mengatasinya?", "id": "Miliki filosofi investasi yang jelas." },
  { "en": "Apakah Buffett menggunakan 'stop-loss'?", "id": "Tidak." },
  { "en": "Mengapa?", "id": "Keputusan jual berdasarkan fundamental bisnis." },
  { "en": "Harga saham turun pertanda apa?", "id": "Kesempatan untuk membeli lebih banyak." },
  { "en": "Jika fundamentalnya masih bagus?", "id": "Ya, jika fundamental tidak berubah." },
  { "en": "Apa itu 'price is what you pay'?", "id": "Harga adalah yang Anda bayar." },
  { "en": "Apa itu 'value is what you get'?", "id": "Nilai adalah yang Anda dapatkan." },
  { "en": "Apa itu 'catalyst' dalam investasi?", "id": "Katalis atau pemicu." },
  { "en": "Pemicu apa?", "id": "Pemicu yang membuat pasar sadar nilai." },
  { "en": "Apakah Buffett menunggu katalis?", "id": "Tidak, dia fokus pada nilai." },
  { "en": "Apa itu 'shareholder yield'?", "id": "Total pengembalian ke pemegang saham." },
  { "en": "Terdiri dari apa?", "id": "Dividen yield dan buyback yield." },
  { "en": "Bagaimana Buffett belajar dari kesalahan?", "id": "Dia mempelajarinya dengan sangat baik." },
  { "en": "Kesalahan terbesar dalam karirnya?", "id": "Membeli Berkshire Hathaway (pabrik tekstil)." },
  { "en": "Pelajaran apa yang didapat?", "id": "Lebih baik beli bisnis hebat." },
  { "en": "Apa itu 'float' asuransi?", "id": "Uang premi yang belum dibayarkan." },
  { "en": "Bagaimana Berkshire menggunakannya?", "id": "Sebagai modal investasi gratis." },
  { "en": "Ini adalah kunci sukses Berkshire?", "id": "Ya, salah satu kunci utamanya." },
  { "en": "Apa itu 'underwriting profit'?", "id": "Laba dari bisnis asuransi." },
  { "en": "Kapan pasar efisien?", "id": "Pasar seringkali efisien." },
  { "en": "Tapi tidak selalu?", "id": "Benar, tidak selalu." },
  { "en": "Tugas investor nilai?", "id": "Menemukan saat pasar tidak efisien." },
  { "en": "Apa itu 'institutional imperative'?", "id": "Kecenderungan meniru institusi lain." },
  { "en": "Apa itu 'due diligence'?", "id": "Uji tuntas." },
  { "en": "Penting sebelum berinvestasi?", "id": "Ya, sangat penting." },
  { "en": "Bagaimana pandangannya terhadap derivatif?", "id": "Senjata pemusnah massal finansial." },
  { "en": "Mengapa?", "id": "Sangat berisiko dan sulit dipahami." },
  { "en": "Apa itu 'intrinsic value compounding'?", "id": "Pertumbuhan nilai intrinsik dari waktu ke waktu." },
  { "en": "Apa itu 'book value'?", "id": "Nilai buku perusahaan." },
  { "en": "Apakah sama dengan nilai intrinsik?", "id": "Tidak, nilai intrinsik biasanya lebih tinggi." },
  { "en": "Bagaimana cara memulai?", "id": "Simpan uang, baca buku, belajar." },
  { "en": "Berapa banyak saham yang perlu diikuti?", "id": "Tidak banyak, hanya beberapa yang hebat." },
  { "en": "Kualitas terbaik dari sebuah bisnis?", "id": "Kemampuan untuk terus menaikkan harga." },
  { "en": "Apa itu 'debt-to-equity ratio'?", "id": "Rasio utang terhadap ekuitas." },
  { "en": "Rasio yang baik?", "id": "Rendah, di bawah 0.5." },
  { "en": "Apa itu 'current ratio'?", "id": "Rasio lancar." },
  { "en": "Mengukur apa?", "id": "Likuiditas jangka pendek perusahaan." },
  { "en": "Apa itu 'profit margin'?", "id": "Margin laba." },
  { "en": "Margin yang baik?", "id": "Tinggi dan stabil." },
  { "en": "Seberapa penting menjadi kontrarian?", "id": "Anda tidak benar karena orang setuju." },
  { "en": "Anda benar karena apa?", "id": "Karena fakta dan alasan Anda benar." },
  { "en": "Bagaimana dengan tips saham?", "id": "Abaikan." },
  { "en": "Bagaimana dengan pakar pasar?", "id": "Abaikan." },
  { "en": "Percaya pada siapa?", "id": "Analisis dan penilaian Anda sendiri." },
  { "en": "Kapan waktu yang buruk untuk berinvestasi?", "id": "Tidak ada, jika Anda berinvestasi jangka panjang." },
  { "en": "Kapan waktu yang baik untuk berinvestasi?", "id": "Saat Anda punya uang dan menemukan bisnis hebat." },
  { "en": "Bagaimana dengan resesi?", "id": "Seringkali waktu terbaik untuk membeli." },
  { "en": "Mengapa?", "id": "Karena harga saham menjadi murah." },
  { "en": "Apa itu 'cash is king'?", "id": "Uang tunai adalah raja." },
  { "en": "Kapan?", "id": "Saat ada peluang besar muncul." },
  { "en": "Buffett suka memegang banyak kas?", "id": "Ya, untuk 'berburu gajah'." },
  { "en": "Apa itu 'berburu gajah'?", "id": "Mencari akuisisi atau investasi besar." },
  { "en": "Pentingnya fokus?", "id": "Sangat penting, jangan terganggu." },
  { "en": "Apa investasi terburuk Buffett?", "id": "Dexter Shoe Company." },
  { "en": "Pelajaran apa yang didapat?", "id": "Moat kompetitif bisa hilang." },
  { "en": "Pentingnya terus belajar?", "id": "Ya, dunia selalu berubah." },
  { "en": "Apakah Buffett menggunakan komputer?", "id": "Sangat jarang." },
  { "en": "Bagaimana dia menganalisis?", "id": "Dengan membaca dan berpikir." },
  { "en": "Kunci untuk hidup yang baik?", "id": "Lakukan apa yang Anda sukai." },
  { "en": "Nasihatnya tentang memilih pasangan?", "id": "Menikahlah dengan orang yang tepat." },
  { "en": "Pentingnya lingkaran pertemanan?", "id": "Berkumpullah dengan orang yang lebih baik." },
  { "en": "Apa itu 'index fund'?", "id": "Sinonim untuk reksa dana indeks." },
  { "en": "Apakah Buffett merekomendasikannya?", "id": "Ya, untuk sebagian besar investor." },
  { "en": "Bagaimana pandangannya tentang biaya investasi?", "id": "Jaga agar biaya tetap rendah." },
  { "en": "Biaya tinggi merusak apa?", "id": "Hasil investasi jangka panjang Anda." },
  { "en": "Apa itu 'management fee'?", "id": "Biaya manajemen reksa dana." },
  { "en": "Reksa dana indeks biayanya?", "id": "Sangat rendah." },
  { "en": "Bagaimana dengan aktivitas trading?", "id": "Trading sering adalah permainan pecundang." },
  { "en": "Apa itu 'turnover' reksa dana?", "id": "Seberapa sering saham diperjualbelikan." },
  { "en": "Turnover tinggi berarti?", "id": "Biaya lebih tinggi, pajak lebih tinggi." },
  { "en": "Apa itu 'capital gains tax'?", "id": "Pajak atas keuntungan modal." },
  { "en": "Menahan saham jangka panjang mengurangi?", "id": "Pajak keuntungan modal." },
  { "en": "Apa itu 'tax efficiency'?", "id": "Efisiensi pajak." },
  { "en": "Bagaimana dengan bisnis komoditas?", "id": "Sulit untuk memiliki 'moat'." },
  { "en": "Mengapa?", "id": "Karena produknya tidak bisa dibedakan." },
  { "en": "Harga ditentukan oleh apa?", "id": "Penawaran dan permintaan." },
  { "en": "Apa itu 'low-cost producer'?", "id": "Produsen dengan biaya terendah." },
  { "en": "Bisa jadi 'moat'?", "id": "Ya, di industri komoditas." },
  { "en": "Bagaimana Buffett memandang inflasi?", "id": "Pajak yang sangat besar." },
  { "en": "Bisnis terbaik saat inflasi?", "id": "Bisnis dengan 'pricing power' tinggi." },
  { "en": "Dan butuh sedikit modal tambahan?", "id": "Ya, betul sekali." },
  { "en": "Apa itu 'franchise value'?", "id": "Nilai waralaba." },
  { "en": "Bisnis yang punya 'franchise value'?", "id": "Punya produk/layanan yang unik." },
  { "en": "Apa itu 'earnings yield'?", "id": "Imbal hasil laba." },
  { "en": "Bagaimana menghitungnya?", "id": "Laba per saham / harga saham." },
  { "en": "Buffett membandingkannya dengan apa?", "id": "Imbal hasil obligasi pemerintah." },
  { "en": "Apa itu 'equity bond'?", "id": "Saham dengan laba yang bisa diprediksi." },
  { "en": "Apa itu 'human capital'?", "id": "Modal manusia (kemampuan diri)." },
  { "en": "Aset terbesar Anda?", "id": "Diri Anda sendiri." },
  { "en": "Pentingnya kesehatan?", "id": "Jaga mesin Anda (tubuh)." },
  { "en": "Bagaimana Buffett menghabiskan waktunya?", "id": "Sebagian besar dengan membaca." },
  { "en": "Seberapa banyak?", "id": "Sekitar 500 halaman per hari." },
  { "en": "Pentingnya berpikir independen?", "id": "Sangat penting, jangan ikuti keramaian." },
  { "en": "Apa itu 'confirmation bias'?", "id": "Mencari info yang mendukung keyakinan." },
  { "en": "Harus dihindari?", "id": "Ya, cari argumen yang berlawanan." },
  { "en": "Bagaimana Buffett mengambil keputusan?", "id": "Sangat rasional dan logis." },
  { "en": "Apa itu 'rationality'?", "id": "Rasionalitas." },
  { "en": "Apa itu 'winner's game'?", "id": "Permainan pemenang." },
  { "en": "Investasi adalah 'winner's game'?", "id": "Bukan, ini permainan 'loser's game'." },
  { "en": "Apa itu 'loser's game'?", "id": "Permainan di mana Anda menang dengan tidak kalah." },
  { "en": "Caranya?", "id": "Dengan menghindari kesalahan besar." },
  { "en": "Apa itu 'unforced error'?", "id": "Kesalahan yang tidak perlu." },
  { "en": "Bagaimana dengan 'leverage' (utang)?", "id": "Jangan pernah berinvestasi dengan utang." },
  { "en": "Mengapa?", "id": "Bisa menghancurkan Anda." },
  { "en": "Tiga kata dari Buffett?", "id": "Margin of safety." },
  { "en": "Pentingnya manajemen waktu?", "id": "Katakan 'tidak' pada hampir semuanya." },
  { "en": "Fokus pada hal penting?", "id": "Ya, pada beberapa prioritas utama." },
  { "en": "Bagaimana dengan 'shorting' saham?", "id": "Jalan menuju rumah miskin." },
  { "en": "Mengapa?", "id": "Potensi kerugian tidak terbatas." },
  { "en": "Apa itu 'selling short'?", "id": "Bertaruh harga saham akan turun." },
  { "en": "Bagaimana dengan 'day trading'?", "id": "Bukan investasi, itu spekulasi." },
  { "en": "Apa itu 'speculation'?", "id": "Spekulasi." },
  { "en": "Perbedaan dengan investasi?", "id": "Fokus pada harga, bukan nilai." },
  { "en": "Bagaimana pandangannya tentang Bitcoin?", "id": "Racun tikus kuadrat." },
  { "en": "Mengapa?", "id": "Aset non-produktif tanpa nilai intrinsik." },
  { "en": "Apa itu 'fear of missing out' (FOMO)?", "id": "Takut ketinggalan." },
  { "en": "Musuh terbesar investor?", "id": "FOMO dan kepanikan." },
  { "en": "Apa itu 'panic selling'?", "id": "Menjual karena panik saat pasar turun." },
  { "en": "Seharusnya apa yang dilakukan?", "id": "Tetap tenang atau beli lagi." },
  { "en": "Apa itu 'stay the course'?", "id": "Tetap pada jalur." },
  { "en": "Nasihatnya untuk resesi?", "id": "Jangan khawatir, teruslah berinvestasi." },
  { "en": "Apa itu 'asset allocation'?", "id": "Alokasi aset." },
  { "en": "Bagaimana alokasi aset Buffett?", "id": "Sebagian besar dalam saham." },
  { "en": "Apa itu 'skin in the game'?", "id": "Manajemen berinvestasi di perusahaannya sendiri." },
  { "en": "Pertanda baik?", "id": "Ya, pertanda sangat baik." },
  { "en": "Pentingnya integritas?", "id": "Cari manajemen dengan integritas tinggi." },
  { "en": "Bagaimana dengan skandal perusahaan?", "id": "Jika ada satu kecoa, biasanya ada banyak." },
  { "en": "Artinya?", "id": "Hindari perusahaan dengan masalah etika." },
  { "en": "Apa itu 'owner-oriented' management?", "id": "Manajemen berorientasi pada pemilik." },
  { "en": "Apa itu 'shareholder letter'?", "id": "Surat tahunan untuk pemegang saham." },
  { "en": "Surat tahunan Berkshire?", "id": "Sangat terkenal dan penuh pelajaran." },
  { "en": "Pentingnya 'cash flow'?", "id": "Kas adalah oksigen bagi bisnis." },
  { "en": "Apa itu 'net income'?", "id": "Laba bersih." },
  { "en": "Mana lebih penting?", "id": "Cash flow lebih penting." },
  { "en": "Mengapa?", "id": "Laba bisa dimanipulasi, kas tidak." },
  { "en": "Apa itu 'creative accounting'?", "id": "Akuntansi kreatif (manipulatif)." },
  { "en": "Apa itu 'balance sheet'?", "id": "Neraca keuangan." },
  { "en": "Apa itu 'income statement'?", "id": "Laporan laba rugi." },
  { "en": "Apa itu 'cash flow statement'?", "id": "Laporan arus kas." },
  { "en": "Ketiganya harus dianalisis?", "id": "Ya, secara bersamaan." },
  { "en": "Apa itu 'compounded annual growth rate' (CAGR)?", "id": "Tingkat pertumbuhan tahunan majemuk." },
  { "en": "Apa itu 'dividend discount model' (DDM)?", "id": "Model valuasi berbasis dividen." },
  { "en": "Apa itu 'discounted cash flow' (DCF)?", "id": "Model valuasi berbasis arus kas." },
  { "en": "Buffett menggunakan DCF?", "id": "Ya, secara mental." },
  { "en": "Kuncinya?", "id": "Memprediksi arus kas masa depan." },
  { "en": "Sulit?", "id": "Ya, karena itulah butuh 'moat'." },
  { "en": "Bagaimana dengan 'growth stocks'?", "id": "Pertumbuhan adalah bagian dari nilai." },
  { "en": "Jadi 'value' vs 'growth'?", "id": "Itu adalah dua sisi koin." },
  { "en": "Apa itu 'growth at a reasonable price' (GARP)?", "id": "Pertumbuhan pada harga yang wajar." },
  { "en": "Gaya Buffett mirip GARP?", "id": "Ya, sangat mirip." },
  { "en": "Pentingnya konsistensi?", "id": "Cari bisnis dengan kinerja konsisten." },
  { "en": "Bagaimana dengan 'turnaround' story?", "id": "Kisah perusahaan bangkit kembali." },
  { "en": "Buffett suka berinvestasi di dalamnya?", "id": "Jarang, karena jarang yang berhasil." },
  { "en": "Apa itu 'dividend aristocrats'?", "id": "Perusahaan yang menaikkan dividen 25+ tahun." },
  { "en": "Tanda bisnis yang sehat?", "id": "Ya, tanda bisnis yang sangat stabil." },
  { "en": "Pentingnya belajar sejarah?", "id": "Sejarah tidak berulang, tapi berirama." },
  { "en": "Mempelajari 'market crash'?", "id": "Ya, untuk belajar dari kepanikan." },
  { "en": "Apa itu 'Black Monday'?", "id": "Crash pasar saham tahun 1987." },
  { "en": "Apa itu 'dot-com bubble'?", "id": "Gelembung saham teknologi tahun 2000." },
  { "en": "Apa itu 'Great Financial Crisis'?", "id": "Krisis keuangan global 2008." },
  { "en": "Apa nasihat terbaiknya?", "id": "Jadilah investor, bukan spekulan." },
  { "en": "Apa itu 'S&P 500'?", "id": "Indeks 500 perusahaan besar AS." },
  { "en": "Investasi favoritnya setelah meninggal?", "id": "90% S&P 500, 10% obligasi." },
  { "en": "Untuk siapa nasihat itu?", "id": "Untuk istrinya." },
  { "en": "Mengapa?", "id": "Strategi sederhana, efektif, dan terdiversifikasi." },
  { "en": "Bagaimana pandangannya terhadap biaya?", "id": "Biaya adalah musuh investor." },
  { "en": "Apa itu 'frictional costs'?", "id": "Biaya transaksi dan pajak." },
  { "en": "Seberapa sering Buffett bertransaksi?", "id": "Sangat jarang." },
  { "en": "Aktivitas adalah musuh dari?", "id": "Hasil investasi yang baik." },
  { "en": "Apa itu 'inactivity'?", "id": "Tidak melakukan apa-apa." },
  { "en": "Terkadang tindakan terbaik adalah?", "id": "Tidak melakukan apa-apa." },
  { "en": "Apa itu 'great business'?", "id": "Bisnis hebat." },
  { "en": "Ciri-cirinya?", "id": "ROE tinggi, utang rendah, 'moat' lebar." },
  { "en": "Bagaimana dengan bisnis yang rumit?", "id": "Hindari, jika Anda tidak memahaminya." },
  { "en": "Contoh industri yang disukai?", "id": "Asuransi, barang konsumsi, perbankan." },
  { "en": "Contoh perusahaan yang dimiliki?", "id": "Coca-Cola, American Express, Apple." },
  { "en": "Mengapa dia membeli Apple?", "id": "Karena ekosistemnya seperti bisnis konsumsi." },
  { "en": "Apa itu 'brand loyalty'?", "id": "Loyalitas merek." },
  { "en": "Penting sebagai 'moat'?", "id": "Ya, salah satu 'moat' terkuat." },
  { "en": "Apa itu 'network effect'?", "id": "Efek jaringan." },
  { "en": "Contohnya?", "id": "Semakin banyak pengguna, semakin berharga." },
  { "en": "American Express punya 'network effect'?", "id": "Ya." },
  { "en": "Apa itu 'switching costs'?", "id": "Biaya beralih ke produk lain." },
  { "en": "Contohnya?", "id": "Pindah dari sistem operasi bank." },
  { "en": "Biaya beralih tinggi itu bagus?", "id": "Ya, itu adalah 'moat' yang kuat." },
  { "en": "Bagaimana dengan paten?", "id": "Bisa jadi 'moat', tapi sementara." },
  { "en": "Apa itu 'intellectual property'?", "id": "Hak kekayaan intelektual." },
  { "en": "Apa pandangannya tentang emas?", "id": "Tidak punya utilitas, hanya rasa takut." },
  { "en": "Bagaimana dengan obligasi?", "id": "Investasi yang sangat buruk saat ini." },
  { "en": "Mengapa?", "id": "Karena imbal hasil riil negatif." },
  { "en": "Apa itu 'real yield'?", "id": "Imbal hasil setelah dikurangi inflasi." },
  { "en": "Apa itu 'inflation'?", "id": "Inflasi." },
  { "en": "Cara terbaik melindungi dari inflasi?", "id": "Investasi pada bisnis hebat." },
  { "en": "Bagaimana dengan properti?", "id": "Bisa jadi investasi yang bagus." },
  { "en": "Jika dibeli dengan harga tepat?", "id": "Ya, dan tidak dengan utang besar." },
  { "en": "Apa itu 'price-to-earnings' (P/E) ratio?", "id": "Rasio harga terhadap laba." },
  { "en": "Apakah Buffett menggunakannya?", "id": "Ya, sebagai panduan awal." },
  { "en": "P/E rendah selalu bagus?", "id": "Tidak selalu, bisa jadi 'value trap'." },
  { "en": "Apa itu 'value trap'?", "id": "Saham murah yang tetap murah." },
  { "en": "Apa itu 'price-to-book' (P/B) ratio?", "id": "Rasio harga terhadap nilai buku." },
  { "en": "Berguna untuk industri apa?", "id": "Perbankan dan asuransi." },
  { "en": "Apa itu 'dividend yield'?", "id": "Imbal hasil dividen." },
  { "en": "Penting?", "id": "Ya, tapi bukan satu-satunya." },
  { "en": "Apa itu 'payout ratio'?", "id": "Persentase laba yang dibayar dividen." },
  { "en": "Payout ratio yang sehat?", "id": "Tidak terlalu tinggi (di bawah 60%)." },
  { "en": "Mengapa?", "id": "Perusahaan butuh laba ditahan." },
  { "en": "Apa itu 'retained earnings'?", "id": "Laba ditahan." },
  { "en": "Untuk apa?", "id": "Untuk bertumbuh dan berinvestasi kembali." },
  { "en": "Satu dolar laba ditahan harus?", "id": "Menghasilkan lebih dari satu dolar nilai." },
  { "en": "Apa itu 'capital allocation'?", "id": "Alokasi modal." },
  { "en": "Tugas utama seorang CEO?", "id": "Alokasi modal yang cerdas." },
  { "en": "Apa itu 'circle of competence' vs 'confidence'?", "id": "Kompetensi vs kepercayaan diri." },
  { "en": "Bahaya 'overconfidence'?", "id": "Membuat kesalahan di luar kompetensi." },
  { "en": "Pentingnya mengakui kesalahan?", "id": "Ya, dan belajar darinya." },
  { "en": "Apa itu 'sunk cost fallacy'?", "id": "Terus berinvestasi karena sudah rugi." },
  { "en": "Harus dihindari?", "id": "Ya, putuskan berdasarkan prospek masa depan." },
  { "en": "Bagaimana dengan 'averaging down'?", "id": "Membeli lagi saat harga turun." },
  { "en": "Bagus atau buruk?", "id": "Bagus jika analisis awal benar." },
  { "en": "Bagaimana jika analisis salah?", "id": "Itu hanya akan memperburuk kerugian." },
  { "en": "Apa itu 'reading financial statements'?", "id": "Membaca laporan keuangan." },
  { "en": "Keterampilan penting?", "id": "Ya, sangat fundamental." },
  { "en": "Apa itu '10-K report'?", "id": "Laporan tahunan perusahaan publik AS." },
  { "en": "Bagaimana dengan 'insider trading'?", "id": "Ilegal dan tidak etis." },
  { "en": "Tapi 'insider buying'?", "id": "Pembelian oleh orang dalam." },
  { "en": "Bisa jadi pertanda baik?", "id": "Ya, jika konsisten dan signifikan." },
  { "en": "Apa itu 'Omaha'?", "id": "Kota tempat tinggal Warren Buffett." },
  { "en": "Apa itu 'Woodstock for Capitalists'?", "id": "Julukan pertemuan tahunan Berkshire." },
  { "en": "Apa itu 'market timing'?", "id": "Mencoba menebak pergerakan pasar." },
  { "en": "Apakah itu berhasil?", "id": "Hampir tidak pernah." },
  { "en": "Daripada 'market timing', lakukan?", "id": "Time in the market." },
  { "en": "Apa itu 'time in the market'?", "id": "Waktu berada di dalam pasar." },
  { "en": "Semakin lama, semakin?", "id": "Baik hasilnya." },
  { "en": "Apa itu 'business cycle'?", "id": "Siklus bisnis (ekspansi, resesi)." },
  { "en": "Apa itu 'economic forecasting'?", "id": "Peramalan ekonomi." },
  { "en": "Buffett melakukannya?", "id": "Tidak, dia tidak peduli." },
  { "en": "Kunci membeli bisnis hebat?", "id": "Bisa membayangkannya 10 tahun lagi." },
  { "en": "Pentingnya kesederhanaan?", "id": "Cari bisnis yang sederhana." },
  { "en": "Apa itu 'boring business'?", "id": "Bisnis yang membosankan." },
  { "en": "Seringkali investasi terbaik?", "id": "Ya." },
  { "en": "Pentingnya menjadi seumur hidup pembelajar?", "id": "Ya, investasi adalah perjalanan belajar." },
  { "en": "Apa itu 'delayed gratification'?", "id": "Menunda kepuasan." },
  { "en": "Kunci untuk menabung dan berinvestasi?", "id": "Ya." },
  { "en": "Bagaimana pandangannya tentang politik?", "id": "Jangan campuradukkan investasi dengan politik." },
  { "en": "Apa itu 'certainty'?", "id": "Kepastian." },
  { "en": "Berinvestasi adalah tentang probabilitas?", "id": "Ya, bukan kepastian mutlak." },
  { "en": "Margin of safety melindungi dari?", "id": "Ketidakpastian dan probabilitas yang salah." },
  { "en": "Apa itu 'optionality'?", "id": "Memiliki pilihan." },
  { "en": "Memegang kas memberikan 'optionality'?", "id": "Ya, untuk membeli saat pasar murah." },
  { "en": "Apa itu 'intellectual honesty'?", "id": "Kejujuran intelektual." },
  { "en": "Penting untuk mengakui apa?", "id": "Apa yang tidak Anda ketahui." },
  { "en": "Bagaimana dengan utang pemerintah?", "id": "Akan dibayar dengan cara inflasi." },
  { "en": "Nasihatnya tentang kartu kredit?", "id": "Hindari seperti wabah." },
  { "en": "Bunga kartu kredit?", "id": "Sangat tinggi dan merusak." },
  { "en": "Bagaimana cara terbaik menjadi kaya?", "id": "Secara perlahan." },
  { "en": "Apa itu 'get-rich-quick scheme'?", "id": "Skema cepat kaya." },
  { "en": "Harus dihindari?", "id": "Ya, selalu." },
  { "en": "Apa itu 'due care'?", "id": "Kewaspadaan." },
  { "en": "Pentingnya independensi berpikir?", "id": "Pikiran Anda aset terbaik Anda." },
  { "en": "Investasi yang sukses itu seperti?", "id": "Menonton cat mengering." },
  { "en": "Artinya?", "id": "Sangat membosankan." },
  { "en": "Apa itu 'hero worship'?", "id": "Pemujaan berlebihan." },
  { "en": "Jangan meniru Buffett secara buta?", "id": "Benar, pahami prinsipnya." },
  { "en": "Apa itu 'punch card' investing?", "id": "Investasi kartu pons." },
  { "en": "Artinya?", "id": "Hanya punya 20 pilihan seumur hidup." },
  { "en": "Membuat Anda lebih selektif?", "id": "Ya, sangat selektif." },
  { "en": "Bagaimana pandangannya tentang teknologi?", "id": "Sulit diprediksi pemenangnya." },
  { "en": "Teknologi bisa menghancurkan 'moat'?", "id": "Ya, sangat cepat." },
  { "en": "Apa itu 'creative destruction'?", "id": "Penghancuran kreatif." },
  { "en": "Bagaimana dengan bank?", "id": "Bisnis yang bagus jika dikelola baik." },
  { "en": "Risiko utama bank?", "id": "Mengambil risiko bodoh dengan utang." },
  { "en": "Apa itu 'leverage'?", "id": "Penggunaan utang untuk investasi." },
  { "en": "Apa itu 'return on tangible assets'?", "id": "Imbal hasil aset berwujud." },
  { "en": "Penting untuk bisnis apa?", "id": "Bisnis yang tidak butuh banyak aset." },
  { "en": "Bagaimana cara membaca surat pemegang saham?", "id": "Cari tanda-tanda kejujuran dan rasionalitas." },
  { "en": "CEO harus mengakui kesalahan?", "id": "Ya, itu pertanda baik." },
  { "en": "CEO harus jelas dalam berkomunikasi?", "id": "Ya, hindari jargon yang rumit." },
  { "en": "Bagaimana dengan akuisisi?", "id": "Sebagian besar akuisisi gagal." },
  { "en": "Mengapa?", "id": "Membayar terlalu mahal." },
  { "en": "Apa itu 'synergy'?", "id": "Sinergi." },
  { "en": "Pandangan Buffett tentang 'synergy'?", "id": "Seringkali hanya ilusi." },
  { "en": "Apa itu 'diworsification'?", "id": "Diversifikasi yang lebih buruk." },
  { "en": "Terjadi kapan?", "id": "Saat perusahaan mengakuisisi bisnis buruk." },
  { "en": "Bagaimana dengan konsultan investasi?", "id": "Tidak diperlukan jika Anda belajar." },
  { "en": "Bagaimana dengan grafik saham?", "id": "Dia tidak pernah menggunakannya." },
  { "en": "Apa itu 'technical analysis'?", "id": "Analisis teknikal." },
  { "en": "Buffett menggunakannya?", "id": "Tidak, dia menggunakan analisis fundamental." },
  { "en": "Apa itu 'fundamental analysis'?", "id": "Analisis fundamental bisnis." },
  { "en": "Apa itu 'beta'?", "id": "Ukuran volatilitas saham." },
  { "en": "Apakah 'beta' sama dengan risiko?", "id": "Menurut Buffett, tidak sama sekali." },
  { "en": "Risiko adalah kemungkinan apa?", "id": "Kehilangan modal secara permanen." },
  { "en": "Bagaimana dengan 'efficient market hypothesis' (EMH)?", "id": "Sebagian besar benar." },
  { "en": "Di mana EMH salah?", "id": "Pasar bisa menjadi sangat irasional." },
  { "en": "Kapan pasar irasional?", "id": "Saat ada ketakutan atau keserakahan ekstrem." },
  { "en": "Apa itu 'animal spirits'?", "id": "Naluri emosional manusia di pasar." },
  { "en": "Siapa yang mencetuskan istilah ini?", "id": "John Maynard Keynes." },
  { "en": "Pentingnya berpikir jangka panjang?", "id": "Itu adalah keunggulan terbesar Anda." },
  { "en": "Investor individu punya keunggulan?", "id": "Ya, tidak ada tekanan jangka pendek." },
  { "en": "Dibandingkan siapa?", "id": "Manajer investasi profesional." },
  { "en": "Mengapa mereka punya tekanan?", "id": "Harus menunjukkan kinerja setiap kuartal." },
  { "en": "Apa itu 'quarterly capitalism'?", "id": "Kapitalisme kuartalan (jangka pendek)." },
  { "en": "Bagaimana dengan valuasi?", "id": "Lebih baik kira-kira benar." },
  { "en": "Daripada?", "id": "Tepat sekali salah." },
  { "en": "Pentingnya kualitas bisnis?", "id": "Waktu adalah teman bisnis hebat." },
  { "en": "Dan musuh dari?", "id": "Bisnis yang biasa-biasa saja." },
  { "en": "Apa itu 'commodity-type business'?", "id": "Bisnis tipe komoditas." },
  { "en": "Sulit menghasilkan keuntungan tinggi?", "id": "Ya, persaingannya ketat." },
  { "en": "Apa itu 'business franchise'?", "id": "Waralaba bisnis (punya 'moat')." },
  { "en": "Lebih disukai dari bisnis komoditas?", "id": "Ya, jauh lebih disukai." },
  { "en": "Bagaimana mengukur 'moat'?", "id": "Lihat profitabilitas historis yang tinggi." },
  { "en": "Dan konsistensi?", "id": "Ya, dan konsistensi." },
  { "en": "Bagaimana dengan inovasi?", "id": "Inovasi bisa menjadi 'moat' yang rapuh." },
  { "en": "Apa itu 'first-mover advantage'?", "id": "Keuntungan menjadi yang pertama." },
  { "en": "Selalu bertahan?", "id": "Tidak, seringkali tidak." },
  { "en": "Apa itu 'brand' vs 'commodity'?", "id": "Merek vs Komoditas." },
  { "en": "Anda membayar lebih untuk merek?", "id": "Ya, karena ada kepercayaan." },
  { "en": "Kunci untuk menjaga 'brand moat'?", "id": "Menjaga kualitas dan citra." },
  { "en": "Bagaimana dengan surat kabar?", "id": "Contoh 'moat' yang hancur." },
  { "en": "Dihancurkan oleh apa?", "id": "Internet." },
  { "en": "Pentingnya beradaptasi?", "id": "Ya, bisnis harus bisa beradaptasi." },
  { "en": "Apa itu 'delegation'?", "id": "Delegasi." },
  { "en": "Gaya manajemen Berkshire?", "id": "Sangat terdesentralisasi." },
  { "en": "Artinya?", "id": "Manajer anak perusahaan punya otonomi." },
  { "en": "Apa itu 'micromanagement'?", "id": "Manajemen mikro (terlalu ikut campur)." },
  { "en": "Buffett melakukannya?", "id": "Tidak, dia sangat menghindarinya." },
  { "en": "Apa yang paling dia hargai?", "id": "Waktu." },
  { "en": "Uang bisa membeli waktu?", "id": "Tidak, uang tidak bisa." },
  { "en": "Bagaimana cara berpikir tentang harga?", "id": "Harga adalah mitra menari Anda." },
  { "en": "Bukan instruktur Anda?", "id": "Benar, jangan biarkan harga mendikte." },
  { "en": "Apa itu 'hindsight bias'?", "id": "Merasa sudah tahu dari awal." },
  { "en": "Apa itu 'anchoring bias'?", "id": "Terpaku pada informasi pertama." },
  { "en": "Contohnya?", "id": "Terpaku pada harga beli saham." },
  { "en": "Keputusan harus berdasarkan apa?", "id": "Nilai intrinsik saat ini." },
  { "en": "Apa itu 'mental models'?", "id": "Model mental." },
  { "en": "Charlie Munger suka menggunakannya?", "id": "Ya, itu adalah kunci kebijaksanaannya." },
  { "en": "Apa itu 'latticework of mental models'?", "id": "Jaringan model mental." },
  { "en": "Membantu apa?", "id": "Membuat keputusan yang lebih baik." },
  { "en": "Bagaimana Buffett menghasilkan ide?", "id": "Membaca, berpikir, dan menyaring." },
  { "en": "Apa itu 'information arbitrage'?", "id": "Keunggulan informasi." },
  { "en": "Apakah Buffett punya itu?", "id": "Tidak, dia menggunakan informasi publik." },
  { "en": "Keunggulannya ada di mana?", "id": "Analisis, temperamen, dan horison waktu." },
  { "en": "Apa itu 'patience premium'?", "id": "Premi kesabaran." },
  { "en": "Apa itu 'look for simplicity'?", "id": "Carilah kesederhanaan." },
  { "en": "Jika rumit, katakan?", "id": "Tidak." },
  { "en": "Apa itu 'business owner' mindset?", "id": "Pola pikir pemilik bisnis." },
  { "en": "Bagaimana cara melatihnya?", "id": "Bayangkan Anda memiliki seluruh perusahaan." },
  { "en": "Apa itu 'scuttlebutt' method?", "id": "Metode investigasi bisnis." },
  { "en": "Siapa yang mempopulerkannya?", "id": "Philip Fisher." },
  { "en": "Caranya?", "id": "Bicara dengan pelanggan, pemasok, pesaing." },
  { "en": "Buffett melakukannya?", "id": "Ya, terutama di awal karirnya." },
  { "en": "Pentingnya etika dalam investasi?", "id": "Berinvestasi pada orang yang Anda percayai." },
  { "en": "Apa itu 'fiduciary duty'?", "id": "Tugas fidusia (kewajiban kepercayaan)." },
  { "en": "Manajer investasi punya tugas ini?", "id": "Ya, kepada klien mereka." },
  { "en": "Pentingnya belajar dari orang lain?", "id": "Belajar dari kesalahan orang lain." },
  { "en": "Mengapa?", "id": "Anda tidak punya waktu membuatnya sendiri." },
  { "en": "Apa itu 'pro-forma earnings'?", "id": "Laba pro-forma (tidak standar)." },
  { "en": "Pandangan Buffett?", "id": "Seringkali menyesatkan." },
  { "en": "Fokus pada laba apa?", "id": "Laba GAAP (standar akuntansi)." },
  { "en": "Apa itu 'EBITDA'?", "id": "Laba sebelum bunga, pajak, depresiasi." },
  { "en": "Pandangan Buffett tentang EBITDA?", "id": "Angka yang tidak masuk akal." },
  { "en": "Mengapa?", "id": "Mengabaikan biaya modal yang nyata." },
  { "en": "Apa itu 'capital expenditures' (CapEx)?", "id": "Belanja modal." },
  { "en": "Sangat penting untuk dipertimbangkan?", "id": "Ya." },
  { "en": "CapEx dibagi menjadi?", "id": "Maintenance CapEx dan Growth CapEx." },
  { "en": "Apa itu 'maintenance CapEx'?", "id": "Belanja modal untuk pemeliharaan." },
  { "en": "Harus dianggap sebagai biaya?", "id": "Ya, biaya nyata." },
  { "en": "Apa itu 'growth CapEx'?", "id": "Belanja modal untuk pertumbuhan." },
  { "en": "Bagaimana dengan bisnis asuransi?", "id": "Model bisnis yang fenomenal." },
  { "en": "Sumber keuntungannya?", "id": "Float dan underwriting profit." },
  { "en": "Apa itu 'loss ratio'?", "id": "Rasio kerugian." },
  { "en": "Apa itu 'expense ratio'?", "id": "Rasio biaya." },
  { "en": "Apa itu 'combined ratio'?", "id": "Gabungan loss dan expense ratio." },
  { "en": "Combined ratio di bawah 100%?", "id": "Berarti underwriting profit." },
  { "en": "Di atas 100%?", "id": "Berarti underwriting loss." },
  { "en": "Berkshire Hathaway sering mencapai apa?", "id": "Underwriting profit." },
  { "en": "Apa itu 'reinsurance'?", "id": "Asuransi untuk perusahaan asuransi." },
  { "en": "Berkshire adalah pemain besar?", "id": "Ya, salah satu yang terbesar." },
  { "en": "Apa itu 'catastrophe bond'?", "id": "Obligasi bencana alam." },
  { "en": "Nasihatnya tentang kesuksesan?", "id": "Ukur kesuksesan dari cinta orang." },
  { "en": "Apa itu 'inner scorecard'?", "id": "Kartu skor internal." },
  { "en": "Apa itu 'outer scorecard'?", "id": "Kartu skor eksternal." },
  { "en": "Mana yang lebih penting?", "id": "Inner scorecard." },
  { "en": "Artinya?", "id": "Hidup sesuai standar Anda sendiri." },
  { "en": "Bukan standar orang lain?", "id": "Benar." },
  { "en": "Pentingnya membaca biografi?", "id": "Belajar dari pengalaman orang sukses." },
  { "en": "Bagaimana dengan pasar obligasi?", "id": "Sangat sulit untuk menghasilkan uang." },
  { "en": "Kecuali jika Anda?", "id": "Memprediksi suku bunga dengan benar." },
  { "en": "Apakah itu mudah?", "id": "Tidak, hampir tidak mungkin." },
  { "en": "Bagaimana Buffett melihat masa depan Amerika?", "id": "Sangat optimis." },
  { "en": "Mengapa?", "id": "Karena potensi manusia dan inovasi." },
  { "en": "Jangan pernah bertaruh melawan siapa?", "id": "Amerika." },
  { "en": "Apa itu 'The American Tailwind'?", "id": "Angin pendorong Amerika." },
  { "en": "Bagaimana pandangannya tentang akademisi keuangan?", "id": "Terlalu fokus pada formula rumit." },
  { "en": "Investasi seharusnya sederhana?", "id": "Ya, tapi tidak mudah." },
  { "en": "Apa itu 'noise'?", "id": "Kebisingan (informasi tidak penting)." },
  { "en": "Investor harus bisa apa?", "id": "Menyaring sinyal dari kebisingan." },
  { "en": "Bagaimana dengan laporan analis?", "id": "Jangan pernah mengandalkannya." },
  { "en": "Lakukan pekerjaan rumah Anda sendiri?", "id": "Ya, selalu." },
  { "en": "Apa itu 'first-level thinking'?", "id": "Pemikiran tingkat pertama (permukaan)." },
  { "en": "Apa itu 'second-level thinking'?", "id": "Pemikiran tingkat kedua (mendalam)." },
  { "en": "Investor hebat harus bisa?", "id": "Melakukan second-level thinking." },
  { "en": "Siapa yang mempopulerkan konsep ini?", "id": "Howard Marks." },
  { "en": "Apa itu 'probabilistic thinking'?", "id": "Berpikir dalam probabilitas." },
  { "en": "Keputusan investasi didasarkan pada?", "id": "Expected value (nilai yang diharapkan)." },
  { "en": "Apa itu 'shareholder-friendly' management?", "id": "Manajemen yang ramah pemegang saham." },
  { "en": "Pentingnya transparansi?", "id": "Sangat penting untuk kepercayaan." },
  { "en": "Bagaimana dengan 'corporate governance'?", "id": "Tata kelola perusahaan yang baik." },
  { "en": "Penting?", "id": "Ya, untuk melindungi investor minoritas." },
  { "en": "Apa itu 'board of directors'?", "id": "Dewan direksi." },
  { "en": "Fungsinya?", "id": "Mengawasi manajemen atas nama pemegang saham." },
  { "en": "Dewan yang baik harus?", "id": "Independen dan kompeten." },
  { "en": "Bagaimana dengan 'stock splits'?", "id": "Tidak menciptakan nilai apa pun." },
  { "en": "Mengapa?", "id": "Hanya memotong kue menjadi lebih banyak." },
  { "en": "Ukuran kuenya tetap sama?", "id": "Ya." },
  { "en": "Saham Berkshire kelas A?", "id": "Sangat mahal, tidak pernah di-split." },
  { "en": "Mengapa?", "id": "Untuk menarik investor jangka panjang." },
  { "en": "Ada saham Berkshire kelas B?", "id": "Ya, harganya lebih terjangkau." },
  { "en": "Apa itu 'inversion'?", "id": "Membalik masalah." },
  { "en": "Teknik berpikir dari siapa?", "id": "Charlie Munger." },
  { "en": "Caranya?", "id": "Pikirkan apa yang tidak boleh dilakukan." },
  { "en": "Untuk menjadi investor hebat, hindari?", "id": "Menjadi investor yang buruk." },
  { "en": "Bagaimana menjadi investor buruk?", "id": "Trading sering, pakai utang, panik." },
  { "en": "Apa itu 'disconfirming evidence'?", "id": "Bukti yang menyangkal keyakinan Anda." },
  { "en": "Harus dicaritahu?", "id": "Ya, secara aktif." },
  { "en": "Apa itu 'cherry-picking' data?", "id": "Memilih data yang mendukung argumen." },
  { "en": "Harus dihindari?", "id": "Ya, itu tidak jujur." },
  { "en": "Apa itu 'narrative fallacy'?", "id": "Kecenderungan menyukai cerita yang bagus." },
  { "en": "Bagaimana dengan 'return to the mean'?", "id": "Kecenderungan kembali ke rata-rata." },
  { "en": "Artinya?", "id": "Kinerja ekstrem jarang bertahan lama." },
  { "en": "Pentingnya kerendahan hati?", "id": "Akui bahwa Anda bisa salah." },
  { "en": "Pasar bisa tetap irasional?", "id": "Lebih lama dari Anda bisa tetap solven." },
  { "en": "Apa itu 'solven'?", "id": "Mampu membayar utang." },
  { "en": "Bagaimana dengan mata uang kripto?", "id": "Aset spekulatif, bukan investasi." },
  { "en": "Apa itu 'blockchain'?", "id": "Teknologi di balik mata uang kripto." },
  { "en": "Teknologinya menarik?", "id": "Ya, tapi bukan berarti asetnya bagus." },
  { "en": "Apa itu 'tulip mania'?", "id": "Gelembung spekulatif tulip di Belanda." },
  { "en": "Pelajaran sejarah?", "id": "Manusia rentan terhadap gelembung spekulatif." },
  { "en": "Bagaimana Buffett mendefinisikan investasi?", "id": "Menunda konsumsi untuk hasil lebih besar." },
  { "en": "Apa itu 'gratification'?", "id": "Kepuasan." },
  { "en": "Investasi membutuhkan 'delayed gratification'?", "id": "Ya." },
  { "en": "Bagaimana dengan kesalahan komisi?", "id": "Kesalahan karena melakukan sesuatu." },
  { "en": "Bagaimana dengan kesalahan omisi?", "id": "Kesalahan karena tidak melakukan sesuatu." },
  { "en": "Kesalahan terbesar Buffett?", "id": "Kesalahan omisi." },
  { "en": "Contohnya?", "id": "Tidak membeli saham Google/Amazon." },
  { "en": "Mengapa dia tidak membeli?", "id": "Di luar lingkaran kompetensinya saat itu." },
  { "en": "Apa itu 'disruptive innovation'?", "id": "Inovasi yang mengganggu pasar." },
  { "en": "Penting untuk diwaspadai?", "id": "Ya, bisa menghancurkan 'moat'." },
  { "en": "Apa itu 'sustainability'?", "id": "Keberlanjutan." },
  { "en": "Penting untuk bisnis?", "id": "Ya, untuk kesuksesan jangka panjang." },
  { "en": "Apa itu 'ESG' investing?", "id": "Investasi berbasis lingkungan, sosial, tata kelola." },
  { "en": "Apakah Buffett investor ESG?", "id": "Tidak secara eksplisit, tapi prinsipnya sejalan." },
  { "en": "Bagaimana?", "id": "Dia berinvestasi pada bisnis berkelanjutan." },
  { "en": "Apa itu 'stakeholder'?", "id": "Pemangku kepentingan." },
  { "en": "Siapa saja?", "id": "Karyawan, pelanggan, komunitas, pemegang saham." },
  { "en": "Bisnis hebat melayani semua stakeholder?", "id": "Ya, dalam jangka panjang." },
  { "en": "Bagaimana dengan filantropi?", "id": "Sangat penting." },
  { "en": "Apa itu 'The Giving Pledge'?", "id": "Janji menyumbangkan sebagian besar kekayaan." },
  { "en": "Buffett ikut serta?", "id": "Ya, salah satu pendirinya." },
  { "en": "Warisan terbesarnya?", "id": "Kebijaksanaan investasi dan filantropi." },
  { "en": "Apa itu 'philanthropy'?", "id": "Filantropi." },
  { "en": "Bagaimana cara menilai CEO?", "id": "Lihat cara mereka mengalokasikan modal." },
  { "en": "Pilihan alokasi modal?", "id": "Investasi, akuisisi, dividen, buyback." },
  { "en": "Apa itu 'compounding machine'?", "id": "Mesin pelipat ganda." },
  { "en": "Bisnis hebat adalah 'compounding machine'?", "id": "Ya, untuk nilai intrinsik." },
  { "en": "Bagaimana dengan makroekonomi global?", "id": "Abaikan, fokus pada bisnisnya." },
  { "en": "Apa itu 'competitive advantage'?", "id": "Keunggulan kompetitif." },
  { "en": "Sama dengan 'economic moat'?", "id": "Ya, istilah yang bisa dipertukarkan." },
  { "en": "Seberapa penting harga beli?", "id": "Sangat penting, itu menentukan imbal hasil." },
  { "en": "Harga bagus bisa mengubah bisnis buruk?", "id": "Tidak, bisnis buruk tetaplah buruk." },
  { "en": "Apa itu 'turnaround'?", "id": "Perusahaan yang mencoba bangkit kembali." },
  { "en": "Buffett berinvestasi di 'turnaround'?", "id": "Jarang, karena jarang yang berhasil." },
  { "en": "Apa kutipannya tentang 'turnaround'?", "id": "Turnaround jarang sekali berbalik." },
  { "en": "Bagaimana dengan pasar saham saat ini?", "id": "Fokus pada nilai, bukan level pasar." },
  { "en": "Apa itu 'stock market crash'?", "id": "Anjloknya pasar saham." },
  { "en": "Kesempatan atau ancaman?", "id": "Kesempatan besar untuk membeli." },
  { "en": "Apa itu 'dollar cost averaging'?", "id": "Menabung rutin di pasar saham." },
  { "en": "Apakah ini cara yang baik?", "id": "Ya, untuk investor non-profesional." },
  { "en": "Apa itu 'lump sum' investing?", "id": "Investasi sekaligus dalam jumlah besar." },
  { "en": "Mana lebih baik, DCA atau lump sum?", "id": "Secara historis, lump sum lebih baik." },
  { "en": "Jika Anda tidak yakin?", "id": "DCA adalah pilihan yang lebih aman." },
  { "en": "Bagaimana dengan utang perusahaan?", "id": "Perhatikan rasio utang terhadap ekuitas." },
  { "en": "Apa itu 'balance sheet strength'?", "id": "Kekuatan neraca keuangan." },
  { "en": "Penting?", "id": "Ya, untuk bertahan saat resesi." },
  { "en": "Apa itu 'working capital'?", "id": "Modal kerja." },
  { "en": "Apa itu 'accounts receivable'?", "id": "Piutang usaha." },
  { "en": "Apa itu 'inventory'?", "id": "Persediaan barang." },
  { "en": "Apa itu 'accounts payable'?", "id": "Utang usaha." },
  { "en": "Apa itu 'cash conversion cycle'?", "id": "Siklus konversi kas." },
  { "en": "Siklus negatif itu bagus?", "id": "Ya, sangat bagus." },
  { "en": "Artinya?", "id": "Pelanggan membayar sebelum perusahaan membayar pemasok." },
  { "en": "Apa itu 'float'?", "id": "Bentuk lain dari siklus kas negatif." },
  { "en": "Bagaimana dengan industri penerbangan?", "id": "Perangkap maut bagi investor." },
  { "en": "Mengapa?", "id": "Padat modal, kompetitif, sensitif harga." },
  { "en": "Apa itu 'barriers to entry'?", "id": "Hambatan masuk bagi pesaing baru." },
  { "en": "Moat yang kuat memiliki?", "id": "Hambatan masuk yang sangat tinggi." },
  { "en": "Bagaimana dengan 'management compensation'?", "id": "Kompensasi manajemen." },
  { "en": "Harus selaras dengan pemegang saham?", "id": "Ya, harus." },
  { "en": "Bagaimana cara terbaiknya?", "id": "Berbasis kinerja jangka panjang." },
  { "en": "Apa itu 'stock options'?", "id": "Opsi saham." },
  { "en": "Bisa merugikan pemegang saham?", "id": "Ya, jika menyebabkan dilusi berlebihan." },
  { "en": "Apa itu 'dilution'?", "id": "Dilusi (penurunan persentase kepemilikan)." },
  { "en": "Apa itu 'book value per share'?", "id": "Nilai buku per saham." },
  { "en": "Pertumbuhannya penting untuk Berkshire?", "id": "Ya, metrik kinerja utamanya." },
  { "en": "Apa itu 'price taker'?", "id": "Perusahaan yang harus menerima harga pasar." },
  { "en": "Apa itu 'price maker'?", "id": "Perusahaan yang bisa menetapkan harga." },
  { "en": "Bisnis dengan 'moat' adalah?", "id": "Price maker." },
  { "en": "Apa itu 'asset-light' business?", "id": "Bisnis yang tidak butuh banyak aset." },
  { "en": "Sangat menguntungkan?", "id": "Ya, seringkali sangat menguntungkan." },
  { "en": "Contohnya?", "id": "Perusahaan perangkat lunak, waralaba." },
  { "en": "Apa itu 'operating leverage'?", "id": "Leverage operasi." },
  { "en": "Biaya tetap tinggi, biaya variabel rendah?", "id": "Ya." },
  { "en": "Apa itu 'scalability'?", "id": "Skalabilitas." },
  { "en": "Artinya?", "id": "Kemampuan tumbuh tanpa biaya sebanding." },
  { "en": "Bisnis perangkat lunak sangat 'scalable'?", "id": "Ya." },
  { "en": "Apa itu 'network effect' vs 'brand'?", "id": "Kekuatan dari pengguna vs persepsi." },
  { "en": "Keduanya bisa menjadi 'moat'?", "id": "Ya, 'moat' yang sangat kuat." },
  { "en": "Apa itu 'deferred tax liability'?", "id": "Kewajiban pajak tangguhan." },
  { "en": "Di neraca Berkshire?", "id": "Sangat besar, sumber pendanaan." },
  { "en": "Apa itu 'contrarian investing'?", "id": "Berinvestasi melawan arus." },
  { "en": "Apakah Buffett seorang 'contrarian'?", "id": "Ya, secara alami." },
  { "en": "Dia membeli saat orang lain?", "id": "Menjual (panik)." },
  { "en": "Dia menjual saat orang lain?", "id": "Membeli (serakah)." },
  { "en": "Pentingnya emosi dalam investasi?", "id": "Kendalikan emosi Anda." },
  { "en": "Investasi butuh IQ tinggi?", "id": "Tidak, butuh temperamen yang stabil." },
  { "en": "Apa itu 'temperament'?", "id": "Temperamen atau watak." },
  { "en": "Apa itu 'fat pitch'?", "id": "Peluang investasi yang sangat bagus." },
  { "en": "Harus menunggu dengan sabar?", "id": "Ya, bertahun-tahun jika perlu." },
  { "en": "Saat datang, ayunkan dengan?", "id": "Kuat (investasi besar)." },
  { "en": "Bagaimana dengan kesalahan?", "id": "Akui dengan cepat dan belajar." },
  { "en": "Pentingnya membaca hal non-investasi?", "id": "Ya, untuk memperluas pemahaman dunia." },
  { "en": "Bagaimana dengan berinvestasi di luar negeri?", "id": "Bisa, jika Anda memahami bisnisnya." },
  { "en": "Berkshire berinvestasi di luar AS?", "id": "Ya, contohnya di Jepang." },
  { "en": "Apa itu 'currency risk'?", "id": "Risiko mata uang." },
  { "en": "Pentingnya hidup hemat?", "id": "Ya, agar punya modal untuk investasi." },
  { "en": "Nasihatnya tentang kemewahan?", "id": "Jangan beli barang mewah sebelum kaya." },
  { "en": "Apa itu 'frugality'?", "id": "Hidup hemat." },
  { "en": "Warren Buffett hidup hemat?", "id": "Ya, terkenal sangat hemat." },
  { "en": "Apa itu 'passion'?", "id": "Gairah." },
  { "en": "Berinvestasi pada apa yang Anda sukai?", "id": "Bukan, pada apa yang Anda pahami." },
  { "en": "Apa itu 'shareholder of one'?", "id": "Perlakukan pemegang saham seolah satu orang." },
  { "en": "Orang itu siapa?", "id": "Anggota keluarga yang percayakan uangnya." },
  { "en": "Bagaimana dengan etika?", "id": "Jangan korbankan reputasi demi uang." },
  { "en": "Reputasi lebih berharga dari uang?", "id": "Ya, jauh lebih berharga." },
  { "en": "Apa itu 'simple' vs 'easy'?", "id": "Sederhana vs mudah." },
  { "en": "Investasi Buffett itu sederhana?", "id": "Ya." },
  { "en": "Tapi apakah itu mudah?", "id": "Tidak, karena butuh disiplin emosional." },
  { "en": "Apa itu 'Mr. Market's mood swings'?", "id": "Perubahan mood Mr. Market." },
  { "en": "Investor rasional harus bagaimana?", "id": "Mengabaikan atau memanfaatkannya." },
  { "en": "Apa itu 'voting machine'?", "id": "Mesin voting." },
  { "en": "Apa itu 'weighing machine'?", "id": "Mesin penimbang." },
  { "en": "Jangka pendek, pasar adalah?", "id": "Mesin voting (popularitas)." },
  { "en": "Jangka panjang, pasar adalah?", "id": "Mesin penimbang (nilai)." },
  { "en": "Kutipan dari siapa?", "id": "Benjamin Graham." },
  { "en": "Pentingnya belajar akuntansi?", "id": "Bahasa bisnis, Anda harus bisa membacanya." },
  { "en": "Apa itu 'generally accepted accounting principles' (GAAP)?", "id": "Prinsip akuntansi yang berlaku umum." },
  { "en": "Apa itu 'non-GAAP earnings'?", "id": "Laba non-GAAP (disesuaikan)." },
  { "en": "Harus waspada?", "id": "Ya, perhatikan dengan skeptis." },
  { "en": "Apa itu 'cash flow from operations'?", "id": "Arus kas dari operasi." },
  { "en": "Metrik arus kas yang penting?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'depreciation'?", "id": "Depresiasi atau penyusutan aset." },
  { "en": "Apakah ini biaya kas?", "id": "Bukan, ini biaya non-kas." },
  { "en": "Tapi apakah ini biaya nyata?", "id": "Ya, karena aset perlu diganti." },
  { "en": "EBITDA mengabaikannya?", "id": "Ya, itulah salah satu kelemahannya." },
  { "en": "Bagaimana dengan 'net profit margin'?", "id": "Margin laba bersih." },
  { "en": "Margin tinggi dan stabil?", "id": "Pertanda 'moat' yang kuat." },
  { "en": "Apa itu 'gross profit margin'?", "id": "Margin laba kotor." },
  { "en": "Mengukur apa?", "id": "Efisiensi produksi dasar." },
  { "en": "Apa itu 'operating profit margin'?", "id": "Margin laba operasi." },
  { "en": "Bagaimana dengan 'barriers to exit'?", "id": "Hambatan untuk keluar dari industri." },
  { "en": "Industri penerbangan punya hambatan ini?", "id": "Ya, sangat tinggi." },
  { "en": "Membuat industri tidak menarik?", "id": "Ya." },
  { "en": "Apa itu 'see's candies'?", "id": "Perusahaan permen milik Berkshire." },
  { "en": "Contoh bisnis ideal?", "id": "Ya, bagi Warren Buffett." },
  { "en": "Mengapa?", "id": "Punya 'pricing power', tidak butuh modal." },
  { "en": "Apa itu 'talent vs temperament'?", "id": "Bakat vs temperamen." },
  { "en": "Mana lebih penting untuk investasi?", "id": "Temperamen." },
  { "en": "Apa itu 'financial literacy'?", "id": "Literasi atau melek finansial." },
  { "en": "Penting untuk semua orang?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'pay yourself first'?", "id": "Bayar dirimu sendiri dulu." },
  { "en": "Artinya?", "id": "Menabung/investasi sebelum belanja." },
  { "en": "Bagaimana Buffett menjadi kaya?", "id": "Mulai muda dan terus compounding." },
  { "en": "Kapan dia membeli saham pertamanya?", "id": "Usia 11 tahun." },
  { "en": "Pentingnya memulai sejak dini?", "id": "Memberi waktu lebih untuk compounding." },
  { "en": "Apa itu 'time horizon'?", "id": "Horizon waktu investasi." },
  { "en": "Investor jangka panjang punya?", "id": "Horizon waktu yang sangat panjang." },
  { "en": "Bagaimana dengan pajak?", "id": "Jadilah mitra diam pemerintah." },
  { "en": "Artinya?", "id": "Minimalkan pajak secara legal." },
  { "en": "Caranya?", "id": "Dengan menahan saham jangka panjang." },
  { "en": "Apa itu 'tax avoidance'?", "id": "Penghindaran pajak (legal)." },
  { "en": "Apa itu 'tax evasion'?", "id": "Penggelapan pajak (ilegal)." },
  { "en": "Bagaimana dengan 'stock market bubble'?", "id": "Gelembung pasar saham." },
  { "en": "Terjadi saat apa?", "id": "Harga aset jauh melampaui nilai." },
  { "en": "Apa yang harus dilakukan?", "id": "Berhati-hati, jangan ikut keserakahan." },
  { "en": "Apa itu 'mean reversion'?", "id": "Kecenderungan kembali ke rata-rata." },
  { "en": "Valuasi tinggi akan kembali?", "id": "Ya, pada akhirnya." },
  { "en": "Apa itu 'margin debt'?", "id": "Utang untuk membeli saham." },
  { "en": "Sangat berbahaya?", "id": "Ya, hindari sama sekali." },
  { "en": "Apa itu 'margin call'?", "id": "Panggilan untuk menambah dana." },
  { "en": "Bisa menyebabkan likuidasi paksa?", "id": "Ya." },
  { "en": "Apa itu 'liquidity'?", "id": "Likuiditas." },
  { "en": "Penting untuk perusahaan?", "id": "Ya, untuk membayar tagihan." },
  { "en": "Penting untuk investor?", "id": "Ya, pegang sedikit kas darurat." },
  { "en": "Apa itu 'emergency fund'?", "id": "Dana darurat." },
  { "en": "Seberapa besar?", "id": "3-6 bulan biaya hidup." },
  { "en": "Apa itu 'asset' vs 'liability'?", "id": "Aset vs liabilitas (kewajiban)." },
  { "en": "Rumah Anda adalah aset?", "id": "Bukan, itu adalah liabilitas." },
  { "en": "Mengapa?", "id": "Karena mengeluarkan uang dari kantong." },
  { "en": "Definisi aset menurut Kiyosaki?", "id": "Sesuatu yang memasukkan uang ke kantong." },
  { "en": "Saham dividen adalah aset?", "id": "Ya." },
  { "en": "Bagaimana dengan inflasi saat ini?", "id": "Sangat buruk bagi pemegang kas." },
  { "en": "Dan juga pemegang obligasi?", "id": "Ya." },
  { "en": "Apa itu 'hyperinflation'?", "id": "Inflasi yang sangat tinggi." },
  { "en": "Apa itu 'deflation'?", "id": "Deflasi (penurunan harga umum)." },
  { "en": "Lebih berbahaya dari inflasi?", "id": "Ya, bisa menyebabkan depresi ekonomi." },
  { "en": "Apa itu 'Great Depression'?", "id": "Depresi Besar tahun 1930-an." },
  { "en": "Pelajaran bagi investor?", "id": "Apapun bisa terjadi di pasar." },
  { "en": "Apa itu 'black swan event'?", "id": "Kejadian langka tak terduga." },
  { "en": "Margin of safety melindungi dari?", "id": "Black swan event." },
  { "en": "Bagaimana dengan kerugian?", "id": "Itu adalah bagian dari permainan." },
  { "en": "Kuncinya?", "id": "Jangan biarkan kerugian menjadi permanen." },
  { "en": "Bagaimana dengan 'luck' (keberuntungan)?", "id": "Keberuntungan berperan dalam hidup." },
  { "en": "Tapi jangka panjang?", "id": "Proses yang baik akan menang." },
  { "en": "Apa itu 'process over outcome'?", "id": "Fokus pada proses, bukan hasil." },
  { "en": "Mengapa?", "id": "Anda tidak bisa mengontrol hasil." },
  { "en": "Anda bisa mengontrol apa?", "id": "Proses pengambilan keputusan Anda." },
  { "en": "Apa itu 'decision journal'?", "id": "Jurnal keputusan." },
  { "en": "Bisa membantu belajar?", "id": "Ya, untuk evaluasi tanpa bias." },
  { "en": "Apa itu 'The Snowball'?", "id": "Judul biografi Warren Buffett." },
  { "en": "Metafora untuk apa?", "id": "Kekuatan compounding." },
  { "en": "Bagaimana Buffett memilih manajer?", "id": "Dia mencari hasrat pada bisnisnya." },
  { "en": "Bukan hanya pada uang?", "id": "Benar." },
  { "en": "Apa itu 'passion'?", "id": "Gairah atau hasrat." },
  { "en": "Buffett masih punya 'passion'?", "id": "Ya, untuk investasi dan Berkshire." },
  { "en": "Apa itu 'tap dancing to work'?", "id": "Menari tap ke tempat kerja." },
  { "en": "Artinya?", "id": "Sangat mencintai pekerjaan Anda." },
  { "en": "Nasihatnya tentang pekerjaan?", "id": "Ambil pekerjaan yang Anda cintai." },
  { "en": "Apa itu 'intrinsic value' vs 'market price'?", "id": "Nilai sejati vs harga pasar." },
  { "en": "Investor nilai mencari apa?", "id": "Saat harga jauh di bawah nilai." },
  { "en": "Apa itu 'reputational risk'?", "id": "Risiko reputasi." },
  { "en": "Berkshire sangat peduli dengan ini?", "id": "Ya, sangat." },
  { "en": "Apa itu 'integrity'?", "id": "Integritas." },
  { "en": "Penting dalam bisnis?", "id": "Sangat penting." },
  { "en": "Bagaimana dengan 'cutting corners'?", "id": "Mengambil jalan pintas." },
  { "en": "Tanda buruk?", "id": "Ya, tanda manajemen yang buruk." },
  { "en": "Apa itu 'simplicity'?", "id": "Kesederhanaan." },
  { "en": "Filosofi Buffett sangat sederhana?", "id": "Ya." },
  { "en": "Apakah mudah untuk diikuti?", "id": "Tidak, butuh disiplin dan kesabaran." },
  { "en": "Apa itu 'delayed gratification'?", "id": "Menunda kepuasan." },
  { "en": "Penting untuk kekayaan?", "id": "Ya, kunci utamanya." },
  { "en": "Apa itu 'frugal'?", "id": "Hemat." },
  { "en": "Bagaimana pandangannya tentang Wall Street?", "id": "Tempat yang menghasilkan biaya besar." },
  { "en": "Nasihatnya?", "id": "Jauhi Wall Street." },
  { "en": "Bagaimana dengan 'hot stocks'?", "id": "Biasanya saham yang harus dihindari." },
  { "en": "Mengapa?", "id": "Karena harganya sudah sangat tinggi." },
  { "en": "Lebih baik mencari saham apa?", "id": "Saham yang diabaikan atau tidak populer." },
  { "en": "Apa itu 'unloved stocks'?", "id": "Saham yang tidak disukai." },
  { "en": "Di mana peluang sering ditemukan?", "id": "Di saham yang tidak disukai." },
  { "en": "Apa itu 'CEO celebrity'?", "id": "CEO yang menjadi selebriti." },
  { "en": "Tanda bahaya?", "id": "Ya, seringkali." },
  { "en": "Mengapa CEO selebriti berbahaya?", "id": "Fokus pada citra, bukan bisnis." },
  { "en": "CEO yang baik fokus pada?", "id": "Operasi bisnis jangka panjang." },
  { "en": "Bagaimana dengan 'gross margin'?", "id": "Margin laba kotor." },
  { "en": "Stabilitasnya menunjukkan apa?", "id": "Adanya keunggulan kompetitif." },
  { "en": "Apa itu 'selling, general, & administrative' (SG&A)?", "id": "Beban penjualan, umum, dan administrasi." },
  { "en": "Persentasenya terhadap laba kotor?", "id": "Rendah dan stabil itu bagus." },
  { "en": "Apa itu 'research & development' (R&D)?", "id": "Biaya riset dan pengembangan." },
  { "en": "Perusahaan R&D tinggi sulit diprediksi?", "id": "Ya, masa depannya tidak pasti." },
  { "en": "Bagaimana dengan 'interest expense'?", "id": "Beban bunga." },
  { "en": "Beban bunga rendah menunjukkan?", "id": "Perusahaan tidak punya banyak utang." },
  { "en": "Apa itu 'tax rate'?", "id": "Tingkat pajak." },
  { "en": "Penting untuk diperiksa?", "id": "Ya, untuk konsistensi." },
  { "en": "Apa itu 'net earnings'?", "id": "Laba bersih." },
  { "en": "Apakah harus konsisten bertumbuh?", "id": "Ya, itu pertanda bisnis hebat." },
  { "en": "Apa itu 'earnings per share' (EPS)?", "id": "Laba per saham." },
  { "en": "Pertumbuhan EPS penting?", "id": "Ya, sangat penting." },
  { "en": "Buyback bisa meningkatkan EPS?", "id": "Ya, karena jumlah saham berkurang." },
  { "en": "Apa itu 'cash equivalents'?", "id": "Setara kas." },
  { "en": "Penting di neraca keuangan?", "id": "Ya, untuk likuiditas." },
  { "en": "Apa itu 'retained earnings' di neraca?", "id": "Akumulasi laba yang tidak dibagikan." },
  { "en": "Mesin compounding Berkshire?", "id": "Pertumbuhan laba ditahan." },
  { "en": "Apa itu 'treasury stock'?", "id": "Saham yang dibeli kembali." },
  { "en": "Apa itu 'intangible assets'?", "id": "Aset tak berwujud." },
  { "en": "Goodwill dan merek termasuk?", "id": "Ya." },
  { "en": "Apa itu 'total liabilities'?", "id": "Total kewajiban atau utang." },
  { "en": "Apa itu 'total equity'?", "id": "Total ekuitas pemegang saham." },
  { "en": "Persamaan akuntansi dasar?", "id": "Aset = Liabilitas + Ekuitas." },
  { "en": "Apa itu 'compounded book value growth'?", "id": "Pertumbuhan nilai buku majemuk." },
  { "en": "Metrik favorit Buffett untuk Berkshire?", "id": "Ya." },
  { "en": "Apa itu 'cash from financing'?", "id": "Arus kas dari pendanaan." },
  { "en": "Apa itu 'cash from investing'?", "id": "Arus kas dari investasi." },
  { "en": "Perusahaan sehat punya CFF negatif?", "id": "Seringkali, karena membayar utang/dividen." },
  { "en": "Perusahaan sehat punya CFI negatif?", "id": "Seringkali, karena investasi aset." },
  { "en": "Perusahaan sehat punya CFO positif?", "id": "Ya, harus positif dan kuat." },
  { "en": "Apa itu 'CFO'?", "id": "Cash Flow from Operations." },
  { "en": "Pelajaran terpenting dari Graham?", "id": "Harga adalah yang utama." },
  { "en": "Pelajaran terpenting dari Munger?", "id": "Kualitas bisnis adalah yang utama." },
  { "en": "Kombinasi keduanya adalah gaya Buffett?", "id": "Ya." },
  { "en": "Bagaimana dia mendefinisikan risiko?", "id": "Tidak tahu apa yang Anda lakukan." },
  { "en": "Mengurangi risiko dengan cara apa?", "id": "Pengetahuan dan lingkaran kompetensi." },
  { "en": "Apa itu 'due diligence'?", "id": "Uji tuntas sebelum berinvestasi." },
  { "en": "Apa itu 'ten-year financials'?", "id": "Data keuangan sepuluh tahun." },
  { "en": "Mengapa 10 tahun?", "id": "Untuk melihat kinerja melewati siklus." },
  { "en": "Bagaimana dengan 'one-time events'?", "id": "Kejadian satu kali." },
  { "en": "Harus diabaikan dari analisis?", "id": "Ya, untuk melihat 'earning power' normal." },
  { "en": "Apa itu 'normalized earnings'?", "id": "Laba yang sudah dinormalisasi." },
  { "en": "Bagaimana Buffett mendapatkan informasi?", "id": "Dari sumber primer (laporan tahunan)." },
  { "en": "Bagaimana dengan rumor?", "id": "Sama sekali tidak penting." },
  { "en": "Apa itu 'buffett indicator'?", "id": "Rasio total kapitalisasi pasar / PDB." },
  { "en": "Mengukur apa?", "id": "Valuasi pasar saham secara keseluruhan." },
  { "en": "Rasio tinggi menunjukkan pasar?", "id": "Mahal atau overvalued." },
  { "en": "Apakah dia menggunakannya untuk 'market timing'?", "id": "Tidak, hanya sebagai pengukur suhu." },
  { "en": "Apa itu 'stewardship'?", "id": "Manajemen yang baik dan bertanggung jawab." },
  { "en": "Bagaimana dengan 'family-owned business'?", "id": "Bisnis milik keluarga." },
  { "en": "Seringkali punya pandangan jangka panjang?", "id": "Ya." },
  { "en": "Apa itu 'founder-led company'?", "id": "Perusahaan yang dipimpin pendiri." },
  { "en": "Seringkali punya visi yang kuat?", "id": "Ya." },
  { "en": "Apa itu 'skin in the game'?", "id": "Manajemen punya kepemilikan saham signifikan." },
  { "en": "Itu pertanda bagus?", "id": "Ya, menyelaraskan kepentingan." },
  { "en": "Apa itu 'stock options backdating'?", "id": "Skandal opsi saham." },
  { "en": "Apa itu 'corporate scandals'?", "id": "Skandal perusahaan." },
  { "en": "Contohnya?", "id": "Enron, WorldCom." },
  { "en": "Pentingnya skeptisisme?", "id": "Jangan percaya begitu saja." },
  { "en": "Apa itu 'trust but verify'?", "id": "Percaya tapi verifikasi." },
  { "en": "Bagaimana Buffett melihat kegagalan?", "id": "Guru terbaik." },
  { "en": "Pentingnya menuliskan ide investasi?", "id": "Ya, untuk menjernihkan pikiran." },
  { "en": "Apa itu 'investment thesis'?", "id": "Alasan mengapa Anda membeli saham." },
  { "en": "Harus ditulis sebelum membeli?", "id": "Ya, sangat disarankan." },
  { "en": "Apa itu 'post-mortem analysis'?", "id": "Analisis setelah investasi selesai." },
  { "en": "Baik untung maupun rugi?", "id": "Ya, untuk belajar." },
  { "en": "Bagaimana dengan 'black box' models?", "id": "Model yang tidak bisa dipahami." },
  { "en": "Buffett menghindarinya?", "id": "Ya, dia harus memahami bisnisnya." },
  { "en": "Bagaimana dengan algoritma trading?", "id": "Dia tidak menggunakannya." },
  { "en": "Apa itu 'high-frequency trading' (HFT)?", "id": "Trading berfrekuensi sangat tinggi." },
  { "en": "Apa itu 'portfolio turnover'?", "id": "Seberapa sering portofolio berubah." },
  { "en": "Portfolio Buffett punya turnover?", "id": "Sangat rendah." },
  { "en": "Filosofinya?", "id": "Kelesuan yang berbatasan dengan kemalasan." },
  { "en": "Apa itu 'activity bias'?", "id": "Bias untuk terus melakukan sesuatu." },
  { "en": "Investor harus melawannya?", "id": "Ya." },
  { "en": "Pentingnya tidur nyenyak?", "id": "Ya, jangan memiliki investasi yang mengganggu tidur." },
  { "en": "Investasi adalah maraton?", "id": "Ya, bukan lari cepat." },
  { "en": "Bagaimana dengan 'hot tips'?", "id": "Tips panas." },
  { "en": "Nasihatnya?", "id": "Tips panas bisa membakar Anda." },
  { "en": "Apa itu 'durable competitive advantage'?", "id": "Keunggulan kompetitif yang tahan lama." },
  { "en": "Kunci dari semua investasi Buffett?", "id": "Ya, ini adalah Holy Grail-nya." },
  { "en": "Bagaimana dengan pajak warisan?", "id": "Dia mendukung pajak warisan." },
  { "en": "Mengapa?", "id": "Untuk mencegah dinasti aristokrasi." },
  { "en": "Apa itu 'ovarian lottery'?", "id": "Lotre ovarium (lahir di keluarga/negara mana)." },
  { "en": "Kunci sukses terbesar?", "id": "Lahir di Amerika." },
  { "en": "Pentingnya bersyukur?", "id": "Ya, dan membalas budi." },
  { "en": "Apa itu 'charity'?", "id": "Amal." },
  { "en": "Bagaimana Buffett menyumbang?", "id": "Melalui yayasan seperti Bill & Melinda Gates." },
  { "en": "Apa itu 'The Giving Pledge'?", "id": "Janji untuk menyumbangkan kekayaan." },
  { "en": "Bagaimana dia melihat uang?", "id": "Uang bisa membeli banyak hal." },
  { "en": "Tapi tidak bisa membeli?", "id": "Waktu dan cinta." },
  { "en": "Nasihat terakhirnya?", "id": "Bergaulah dengan orang yang lebih baik." },
  { "en": "Apa itu ' moat' merek?", "id": "Pelanggan rela bayar lebih." },
  { "en": "Apa itu ' moat' rahasia dagang?", "id": "Formula rahasia seperti Coca-Cola." },
  { "en": "Apa itu ' moat' biaya beralih?", "id": "Sulit bagi pelanggan untuk pindah." },
  { "en": "Apa itu ' moat' biaya rendah?", "id": "Bisa menjual lebih murah dari pesaing." },
  { "en": "Apa itu ' moat' efek jaringan?", "id": "Nilai meningkat seiring jumlah pengguna." },
  { "en": "Pentingnya 'moat' yang melebar?", "id": "Keunggulan kompetitifnya terus bertumbuh." },
  { "en": "Apa itu 'pricing power'?", "id": "Kemampuan menaikkan harga tanpa kehilangan pelanggan." },
  { "en": "Tanda 'moat' terkuat?", "id": "Ya, tanda utama bisnis hebat." },
  { "en": "Apa itu 'toll bridge' business?", "id": "Bisnis jembatan tol." },
  { "en": "Artinya?", "id": "Bisnis yang harus digunakan pelanggan." },
  { "en": "Contohnya?", "id": "American Express." },
  { "en": "Bagaimana dengan 'business model'?", "id": "Model bisnis." },
  { "en": "Harus sederhana dan jelas?", "id": "Ya." },
  { "en": "Jika Anda tidak bisa menjelaskannya?", "id": "Jangan berinvestasi di dalamnya." },
  { "en": "Apa itu 'customer satisfaction'?", "id": "Kepuasan pelanggan." },
  { "en": "Penting untuk 'moat' jangka panjang?", "id": "Ya, sangat penting." },
  { "en": "Bagaimana dengan keluhan pelanggan?", "id": "Perusahaan hebat menanganinya dengan baik." },
  { "en": "Apa itu 'capital-light' business?", "id": "Sinonim untuk 'asset-light' business." },
  { "en": "Apa itu 'return on invested capital' (ROIC)?", "id": "Imbal hasil atas modal yang diinvestasikan." },
  { "en": "Metrik yang disukai Buffett?", "id": "Ya, sangat." },
  { "en": "ROIC tinggi dan konsisten?", "id": "Tanda bisnis berkualitas tinggi." },
  { "en": "Bagaimana dengan 'goodwill' di neraca?", "id": "Hasil dari akuisisi di atas nilai buku." },
  { "en": "Buffett lebih suka 'goodwill' internal?", "id": "Ya, yang dibangun dari merek." },
  { "en": "Apa itu 'organic growth'?", "id": "Pertumbuhan organik (internal)." },
  { "en": "Apa itu 'inorganic growth'?", "id": "Pertumbuhan dari akuisisi." },
  { "en": "Mana yang lebih disukai?", "id": "Pertumbuhan organik." },
  { "en": "Apa itu 'bolt-on acquisition'?", "id": "Akuisisi kecil yang melengkapi bisnis." },
  { "en": "Berkshire sering melakukannya?", "id": "Ya." },
  { "en": "Apa itu 'synergies'?", "id": "Sinergi." },
  { "en": "Pandangan Buffett tentang sinergi?", "id": "Seringkali hanya angan-angan." },
  { "en": "Pentingnya membaca memo Howard Marks?", "id": "Ya, Buffett selalu membacanya." },
  { "en": "Siapa Howard Marks?", "id": "Investor nilai terkenal lainnya." },
  { "en": "Apa itu 'Oaktree Capital'?", "id": "Perusahaan investasi Howard Marks." },
  { "en": "Bagaimana dengan 'market psychology'?", "id": "Psikologi pasar." },
  { "en": "Kunci untuk menjadi kontrarian?", "id": "Pahami dan lawan psikologi pasar." },
  { "en": "Bagaimana dengan 'margin of safety'?", "id": "Tidak ada formula pasti." },
  { "en": "Bergantung pada apa?", "id": "Kepastian dan kualitas bisnis." },
  { "en": "Bisnis lebih pasti, margin?", "id": "Bisa lebih kecil." },
  { "en": "Bisnis kurang pasti, margin?", "id": "Harus jauh lebih besar." },
  { "en": "Apa itu 'discount rate'?", "id": "Tingkat diskonto dalam valuasi DCF." },
  { "en": "Buffett menggunakan tingkat diskonto apa?", "id": "Imbal hasil obligasi jangka panjang." },
  { "en": "Bukan 'WACC'?", "id": "Bukan, itu terlalu akademis." },
  { "en": "Apa itu 'WACC'?", "id": "Weighted Average Cost of Capital." },
  { "en": "Apa itu 'terminal value'?", "id": "Nilai bisnis di akhir periode proyeksi." },
  { "en": "Bagian besar dari nilai DCF?", "id": "Ya, seringkali." },
  { "en": "Bagaimana dengan 'exit multiple'?", "id": "Cara lain menghitung terminal value." },
  { "en": "Valuasi itu seni atau sains?", "id": "Lebih banyak seni daripada sains." },
  { "en": "Lebih baik kira-kira benar?", "id": "Daripada tepat sekali salah." },
  { "en": "Pentingnya manajemen yang rasional?", "id": "Kualitas nomor satu yang dicari." },
  { "en": "Bagaimana mengujinya?", "id": "Lihat keputusan alokasi modal masa lalu." },
  { "en": "Apakah mereka membeli kembali saham?", "id": "Saat harganya murah?" },
  { "en": "Apakah mereka melakukan akuisisi?", "id": "Dengan harga yang masuk akal?" },
  { "en": "Apa itu 'owner's manual'?", "id": "Panduan pemilik yang ditulis Buffett." },
  { "en": "Untuk pemegang saham Berkshire?", "id": "Ya." },
  { "en": "Isinya?", "id": "Prinsip-prinsip dasar bisnis Berkshire." },
  { "en": "Bagaimana Buffett memandang pemegang saham?", "id": "Sebagai mitra bisnis." },
  { "en": "Apa itu 'partnership'?", "id": "Kemitraan." },
  { "en": "Apa itu 'economic reality'?", "id": "Realitas ekonomi." },
  { "en": "Lebih penting dari aturan akuntansi?", "id": "Ya, menurut Buffett." },
  { "en": "Apa itu 'GAAP'?", "id": "Generally Accepted Accounting Principles." },
  { "en": "Apa itu 'intrinsic business value'?", "id": "Nilai bisnis intrinsik." },
  { "en": "Tujuan utama manajemen Berkshire?", "id": "Meningkatkan nilai bisnis intrinsik." },
  { "en": "Apa itu 'decentralized' management?", "id": "Manajemen terdesentralisasi." },
  { "en": "Kantor pusat Berkshire?", "id": "Sangat kecil." },
  { "en": "Berapa banyak karyawan di sana?", "id": "Kurang dari 30 orang." },
  { "en": "Mengelola ratusan ribu karyawan?", "id": "Ya, melalui desentralisasi." },
  { "en": "Apa itu 'trust'?", "id": "Kepercayaan." },
  { "en": "Fondasi dari model Berkshire?", "id": "Ya, kepercayaan pada manajer." },
  { "en": "Pentingnya kesederhanaan dalam komunikasi?", "id": "Ya, tulis agar bisa dimengerti." },
  { "en": "Apa itu 'plain English'?", "id": "Bahasa Inggris yang sederhana." },
  { "en": "Surat tahunan Buffett menggunakan?", "id": "Ya, plain English." },
  { "en": "Apa itu 'frictional costs of activity'?", "id": "Biaya friksional dari aktivitas." },
  { "en": "Contohnya?", "id": "Biaya broker dan pajak." },
  { "en": "Bagaimana dengan 'cash flow statement'?", "id": "Laporan arus kas." },
  { "en": "Bagian terpenting laporan keuangan?", "id": "Bagi banyak investor nilai." },
  { "en": "Mengapa?", "id": "Sulit dimanipulasi." },
  { "en": "Apa itu 'accrual accounting'?", "id": "Akuntansi akrual." },
  { "en": "Bisa menyebabkan perbedaan?", "id": "Ya, antara laba dan kas." },
  { "en": "Laba belum tentu kas?", "id": "Benar." },
  { "en": "Apa itu 'depreciation' dan 'amortization'?", "id": "Depresiasi dan amortisasi." },
  { "en": "Biaya non-kas?", "id": "Ya." },
  { "en": "Ditambahkan kembali di laporan arus kas?", "id": "Ya, di bagian operasi." },
  { "en": "Apa itu 'stock-based compensation'?", "id": "Kompensasi berbasis saham." },
  { "en": "Apakah ini biaya nyata?", "id": "Ya, menurut Buffett." },
  { "en": "Banyak perusahaan teknologi menggunakannya?", "id": "Ya." },
  { "en": "Apa itu 'capex'?", "id": "Capital expenditures (belanja modal)." },
  { "en": "Arus kas bebas dihitung setelah?", "id": "Setelah belanja modal." },
  { "en": "Apa itu 'balance sheet'?", "id": "Neraca." },
  { "en": "Menunjukkan apa?", "id": "Potret finansial pada satu waktu." },
  { "en": "Apa itu 'income statement'?", "id": "Laporan laba rugi." },
  { "en": "Menunjukkan apa?", "id": "Kinerja finansial selama periode." },
  { "en": "Apa itu 'liquidity risk'?", "id": "Risiko likuiditas." },
  { "en": "Apa itu 'solvency risk'?", "id": "Risiko solvabilitas (kebangkrutan)." },
  { "en": "Utang yang banyak meningkatkan risiko?", "id": "Risiko solvabilitas." },
  { "en": "Apa itu 'Graham number'?", "id": "Formula valuasi dari Ben Graham." },
  { "en": "Untuk jenis perusahaan apa?", "id": "Perusahaan 'cigar butt' (biasa)." },
  { "en": "Apa itu 'net-net' investing?", "id": "Membeli saham di bawah modal kerja." },
  { "en": "Gaya Graham?", "id": "Ya, gaya awal Graham." },
  { "en": "Apa itu 'working capital'?", "id": "Modal kerja (aset lancar - utang lancar)." },
  { "en": "Apa itu 'current assets'?", "id": "Aset lancar." },
  { "en": "Apa itu 'current liabilities'?", "id": "Utang lancar." },
  { "en": "Apa itu 'checklist'?", "id": "Daftar periksa." },
  { "en": "Munger menggunakan 'checklist'?", "id": "Ya, untuk menghindari kesalahan bodoh." },
  { "en": "Apa itu 'loss aversion bias'?", "id": "Bias menghindari kerugian." },
  { "en": "Membuat investor menjual terlalu cepat?", "id": "Menjual pemenang, menahan pecundang." },
  { "en": "Apa itu 'disposition effect'?", "id": "Efek disposisi." },
  { "en": "Bagaimana Buffett menghindarinya?", "id": "Fokus pada fundamental bisnis." },
  { "en": "Apa itu 'recency bias'?", "id": "Terlalu percaya pada kejadian terbaru." },
  { "en": "Apa itu 'availability heuristic'?", "id": "Mengandalkan informasi yang mudah diingat." },
  { "en": "Keduanya berbahaya?", "id": "Ya, bisa mengaburkan penilaian." },
  { "en": "Apa itu 'survivorship bias'?", "id": "Hanya melihat yang 'selamat'." },
  { "en": "Contohnya?", "id": "Hanya melihat reksa dana sukses." },
  { "en": "Apa itu 'illusion of control'?", "id": "Ilusi kontrol." },
  { "en": "Merasa bisa mengontrol pasar?", "id": "Ya." },
  { "en": "Apa itu 'mental accounting'?", "id": "Akuntansi mental." },
  { "en": "Memperlakukan uang secara berbeda?", "id": "Ya, berdasarkan asalnya." },
  { "en": "Uang adalah 'fungible'?", "id": "Ya, semua uang sama nilainya." },
  { "en": "Pentingnya berpikir rasional?", "id": "Kunci utama investasi sukses." },
  { "en": "Apa itu 'operating history'?", "id": "Riwayat operasi perusahaan." },
  { "en": "Buffett suka riwayat panjang?", "id": "Ya, untuk melihat konsistensi." },
  { "en": "Bagaimana dengan perusahaan baru?", "id": "Lebih sulit dinilai, risiko lebih tinggi." },
  { "en": "Apa itu 'margin of safety' vs 'diversification'?", "id": "Margin untuk satu saham, diversifikasi untuk portofolio." },
  { "en": "Mana yang lebih penting?", "id": "Bagi Buffett, margin of safety." },
  { "en": "Apa itu 'concentration'?", "id": "Konsentrasi." },
  { "en": "Portofolio Berkshire sangat terkonsentrasi?", "id": "Ya." },
  { "en": "Risikonya?", "id": "Jika salah, kerugian bisa besar." },
  { "en": "Imbal hasilnya?", "id": "Jika benar, keuntungan sangat besar." },
  { "en": "Ini untuk investor ahli?", "id": "Ya, bukan untuk pemula." },
  { "en": "Nasihatnya untuk pemula?", "id": "Beli reksa dana indeks S&P 500." },
  { "en": "Apa itu 'see-through earnings'?", "id": "Sinonim 'look-through earnings'." },
  { "en": "Bagaimana pandangannya tentang emas?", "id": "Aset yang tidak produktif." },
  { "en": "Emas tidak menghasilkan apa-apa?", "id": "Benar, hanya diam di sana." },
  { "en": "Bagaimana dengan pertanian?", "id": "Aset produktif, menghasilkan panen." },
  { "en": "Bagaimana dengan real estat?", "id": "Aset produktif, menghasilkan sewa." },
  { "en": "Apa itu 'productive assets'?", "id": "Aset produktif." },
  { "en": "Fondasi investasi Buffett?", "id": "Membeli aset produktif." },
  { "en": "Apa itu 'equity'?", "id": "Ekuitas atau saham." },
  { "en": "Apa itu 'bond'?", "id": "Obligasi atau surat utang." },
  { "en": "Mana imbal hasil lebih tinggi?", "id": "Saham, dalam jangka panjang." },
  { "en": "Mana risiko lebih tinggi?", "id": "Saham, dalam jangka pendek." },
  { "en": "Apa itu 'equity risk premium'?", "id": "Premi risiko saham." },
  { "en": "Artinya?", "id": "Imbal hasil ekstra dari saham." },
  { "en": "Pentingnya hidup di bawah kemampuan?", "id": "Ya, agar bisa menabung." },
  { "en": "Apa itu 'financial independence'?", "id": "Kemerdekaan finansial." },
  { "en": "Apa itu 'passive income'?", "id": "Pendapatan pasif." },
  { "en": "Dividen adalah pendapatan pasif?", "id": "Ya." },
  { "en": "Sewa properti adalah pendapatan pasif?", "id": "Ya." },
  { "en": "Apa itu 'compound interest'?", "id": "Bunga majemuk atau bunga berbunga." },
  { "en": "Keajaiban dunia ke-8?", "id": "Ya, menurut Albert Einstein." },
  { "en": "Kunci untuk 'compounding'?", "id": "Tingkat imbal hasil dan waktu." },
  { "en": "Bagaimana cara menilai manajemen?", "id": "Tiga kata: integritas, energi, intelijensi." },
  { "en": "Yang terpenting?", "id": "Integritas." },
  { "en": "Tanpa integritas, dua lainnya?", "id": "Akan membunuh Anda." },
  { "en": "Apa itu 'franchise'?", "id": "Waralaba." },
  { "en": "McDonald's adalah contoh?", "id": "Ya, waralaba yang sangat kuat." },
  { "en": "Apa itu 'royalty fee'?", "id": "Biaya royalti." },
  { "en": "Bagaimana dengan 'government regulation'?", "id": "Regulasi pemerintah." },
  { "en": "Bisa menjadi 'moat'?", "id": "Ya, di industri seperti perbankan." },
  { "en": "Bisa juga menghancurkan 'moat'?", "id": "Ya." },
  { "en": "Apa itu 'proprietary technology'?", "id": "Teknologi milik sendiri." },
  { "en": "Bagaimana dengan 'management turnover'?", "id": "Pergantian manajemen." },
  { "en": "Turnover tinggi itu pertanda buruk?", "id": "Ya, seringkali." },
  { "en": "Apa itu 'succession plan'?", "id": "Rencana suksesi." },
  { "en": "Penting untuk perusahaan?", "id": "Ya, terutama dengan CEO tua." },
  { "en": "Siapa penerus Buffett?", "id": "Greg Abel." },
  { "en": "Bagaimana dengan inflasi?", "id": "Pajak tersembunyi bagi investor." },
  { "en": "Cara terbaik melawannya?", "id": "Investasi pada bisnis berkualitas." },
  { "en": "Yang punya 'pricing power'?", "id": "Ya, dan butuh sedikit modal." },
  { "en": "Apa itu 'capital re-investment'?", "id": "Investasi kembali modal." },
  { "en": "Apa itu 'circle of competence'?", "id": "Lingkaran kompetensi." },
  { "en": "Tetap di dalam lingkaran?", "id": "Ya, sangat penting." },
  { "en": "Tidak apa-apa melewatkan Google?", "id": "Ya, jika di luar lingkaran Anda." },
  { "en": "Kuncinya bukan ukuran lingkaran?", "id": "Tapi seberapa baik Anda tahu batasnya." },
  { "en": "Apa itu 'humility'?", "id": "Kerendahan hati." },
  { "en": "Sifat penting bagi investor?", "id": "Ya, sangat." },
  { "en": "Mengakui tidak tahu adalah?", "id": "Awal dari kebijaksanaan." },
  { "en": "Apa itu 'great, good, gruesome' businesses?", "id": "Tiga jenis bisnis menurut Buffett." },
  { "en": "Investasi pada yang mana?", "id": "Hanya pada bisnis yang 'great'." },
  { "en": "Apa itu 'shareholder' vs 'stakeholder'?", "id": "Pemegang saham vs pemangku kepentingan." },
  { "en": "Bagaimana dengan 'socially responsible investing' (SRI)?", "id": "Investasi yang bertanggung jawab sosial." },
  { "en": "Apa itu 'corporate culture'?", "id": "Budaya perusahaan." },
  { "en": "Penting untuk kesuksesan jangka panjang?", "id": "Ya." },
  { "en": "Budaya Berkshire seperti apa?", "id": "Berbasis kepercayaan, otonomi, rasionalitas." },
  { "en": "Bagaimana dengan membeli perusahaan keluarga?", "id": "Berkshire adalah 'pembeli permanen'." },
  { "en": "Artinya?", "id": "Tidak akan menjual kembali bisnisnya." },
  { "en": "Menarik bagi pemilik keluarga?", "id": "Ya, untuk menjaga warisan." },
  { "en": "Apa itu 'permanent holding'?", "id": "Kepemilikan permanen." },
  { "en": "Filosofi 'buy and hold'?", "id": "Beli dan tahan." },
  { "en": "Tapi bukan 'buy and forget'?", "id": "Benar, tetap monitor bisnisnya." },
  { "en": "Apa itu 'monitoring'?", "id": "Pemantauan." },
  { "en": "Seberapa sering?", "id": "Baca laporan kuartalan dan tahunan." },
  { "en": "Bukan harga saham harian?", "id": "Bukan." },
  { "en": "Pentingnya tidur malam yang nyenyak?", "id": "Investasi tidak boleh membuat Anda stres." },
  { "en": "Apa itu 'Kelly Criterion'?", "id": "Formula untuk menentukan ukuran taruhan." },
  { "en": "Munger dan Buffett menggunakannya?", "id": "Ya, secara implisit." },
  { "en": "Artinya?", "id": "Bertaruh besar saat peluang sangat bagus." },
  { "en": "Apa itu 'conviction'?", "id": "Keyakinan." },
  { "en": "Investor hebat punya 'conviction' tinggi?", "id": "Ya, pada ide terbaik mereka." },
  { "en": "Apa itu 'too-hard pile'?", "id": "Tumpukan 'terlalu sulit'." },
  { "en": "Sebagian besar perusahaan masuk ke sana?", "id": "Ya, 99%." },
  { "en": "Kunci terakhir dari Buffett?", "id": "Temukan gairah Anda." }



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
