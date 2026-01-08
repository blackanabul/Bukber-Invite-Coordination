# Bukber-Invite-Coordination
Bukber Invite &amp; Coordination Assistant adalah generator untuk menghasilkan pesan WhatsApp.  Fungsinya untuk menghasilkan paket pesan koordinasi bukber yang siap copas.
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bukber Invite & Coordination Assistant</title>
    <!-- Import Google Font for Handwritten look -->
    <link href="https://fonts.googleapis.com/css2?family=Patrick+Hand&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Sketchy Palette */
            --paper-bg: #fdfcf0; /* Creamy paper */
            --paper-line: #e6e6e6; /* Blueish notebook lines */
            --ink-color: #2c3e50; /* Dark blue ink */
            --pencil-gray: #7f8c8d;
            --marker-green: #6ab04c;
            --marker-green-hover: #badc58;
            --marker-pink: #ff7979;
            --highlight: rgba(246, 229, 141, 0.5); /* Yellow highlighter */
            --border-width: 2px;
            --shadow-offset: 3px;
            --radius-sketch: 2px 255px 3px 25px / 255px 5px 225px 3px; /* Generic messy radius */
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Patrick Hand', 'Comic Sans MS', 'Chalkboard SE', cursive, sans-serif;
        }

        body {
            background-color: var(--paper-bg);
            color: var(--ink-color);
            padding: 20px;
            line-height: 1.6;
            /* Notebook paper pattern */
            background-image: 
                linear-gradient(#e1e8ed 1px, transparent 1px);
            background-size: 100% 24px;
        }

        /* Layout */
        .container {
            max-width: 1100px;
            margin: 0 auto;
            position: relative;
        }

        .header {
            text-align: center;
            margin-bottom: 40px;
            padding: 20px;
            position: relative;
        }

        .header h1 {
            color: var(--ink-color);
            font-size: 2.5rem;
            font-weight: 700;
            letter-spacing: 1px;
            text-shadow: 2px 2px 0px rgba(0,0,0,0.1);
            transform: rotate(-1deg);
        }

        .header p {
            color: var(--pencil-gray);
            font-size: 1.2rem;
            margin-top: 5px;
            transform: rotate(1deg);
        }

        .badge {
            background: var(--marker-green);
            color: #fff;
            padding: 6px 12px;
            /* Messy clip path to look like tape */
            clip-path: polygon(0% 0%, 100% 2%, 98% 100%, 2% 96%);
            font-size: 0.9rem;
            display: inline-block;
            margin-top: 15px;
            transform: rotate(-2deg);
            box-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .main-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 32px;
        }

        @media (min-width: 768px) {
            .main-grid {
                grid-template-columns: 1fr 1fr;
                align-items: start;
            }
        }

        /* Sketchy Cards */
        .card {
            background: #fff;
            border: 2px solid var(--ink-color);
            /* Organic border radius trick */
            border-radius: 255px 15px 225px 15px / 15px 225px 15px 255px;
            padding: 24px;
            box-shadow: var(--shadow-offset) var(--shadow-offset) 0px rgba(0,0,0,0.2);
            position: relative;
            margin-bottom: 20px;
        }

        /* Paper texture for card */
        .card::before {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)' opacity='0.05'/%3E%3C/svg%3E");
            pointer-events: none;
            border-radius: inherit;
            z-index: 0;
        }

        .card > * {
            position: relative; /* Ensure content is above texture */
            z-index: 1;
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
            padding-bottom: 12px;
            border-bottom: 2px dashed var(--pencil-gray);
        }

        .card-title {
            color: var(--ink-color);
            font-size: 1.5rem;
            font-weight: 700;
        }

        /* Form Elements */
        .form-group {
            margin-bottom: 24px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-weight: 700;
            font-size: 1.1rem;
            color: var(--ink-color);
        }

        .form-sublabel {
            font-size: 0.9rem;
            color: var(--pencil-gray);
            font-style: italic;
            margin-bottom: 6px;
            display: block;
        }

        input[type="text"],
        input[type="number"],
        textarea {
            width: 100%;
            padding: 12px;
            background: transparent;
            border: 2px solid var(--ink-color);
            /* Slightly different wobbly border for inputs */
            border-radius: 5px 255px 3px 25px / 255px 5px 225px 3px;
            font-size: 1.1rem;
            color: var(--ink-color);
            transition: all 0.2s;
        }

        input:focus, textarea:focus {
            outline: none;
            border-color: var(--marker-green);
            box-shadow: 2px 4px 8px rgba(106, 176, 76, 0.2);
            transform: scale(1.01);
            background: #fff;
        }

        textarea {
            resize: vertical;
            min-height: 100px;
        }

        /* Toggle as a Checkbox Scribble */
        .toggle-container {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 20px;
            background: var(--highlight);
            padding: 10px 15px;
            border-radius: 20px 5px 20px 5px;
            transform: rotate(-1deg);
            width: fit-content;
        }
        
        input[type="checkbox"] {
            accent-color: var(--marker-green);
            width: 20px;
            height: 20px;
            cursor: pointer;
        }

        /* Buttons */
        .btn {
            background-color: var(--marker-green);
            color: #fff;
            border: 2px solid var(--ink-color);
            padding: 12px 20px;
            /* Messy border radius */
            border-radius: 255px 15px 225px 15px / 15px 225px 15px 255px;
            font-weight: 700;
            cursor: pointer;
            width: 100%;
            font-size: 1.2rem;
            transition: all 0.2s;
            box-shadow: 2px 3px 0px rgba(0,0,0,0.2);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .btn:hover {
            background-color: var(--marker-green-hover);
            transform: translateY(-2px) rotate(1deg);
            box-shadow: 3px 5px 0px rgba(0,0,0,0.2);
        }

        .btn:active {
            transform: translateY(1px);
        }

        .btn:disabled {
            background-color: var(--pencil-gray);
            cursor: not-allowed;
            transform: none;
        }

        .btn-outline {
            background: transparent;
            border: 2px solid var(--ink-color);
            color: var(--ink-color);
            margin-top: 8px;
            padding: 6px 16px;
            border-radius: 20px 5px 25px 5px; /* oblong sketch */
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 2px 2px 0px rgba(0,0,0,0.1);
        }

        .btn-outline:hover {
            background: #fff;
            transform: rotate(-1deg);
        }

        .btn-reset {
            background: var(--marker-pink);
            color: white;
            border: 2px solid var(--ink-color);
            margin-top: 0;
            transform: rotate(2deg);
        }
        
        .btn-reset:hover {
             background: #ff5252;
             transform: rotate(1deg) scale(1.02);
        }

        .btn-group {
            display: grid;
            grid-template-columns: 3fr 1fr;
            gap: 16px;
            margin-top: 30px;
        }

        /* Output Area */
        .output-block {
            background: #fff;
            border: 2px solid var(--ink-color);
            /* Just a box */
            border-radius: 2px; 
            margin-bottom: 24px;
            position: relative;
            box-shadow: 3px 3px 0px rgba(0,0,0,0.1);
            transform: rotate(0.5deg);
        }

        .output-block:nth-child(even) {
            transform: rotate(-0.5deg);
        }

        .output-header-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 16px;
            background: var(--paper-line);
            border-bottom: 2px solid var(--ink-color);
        }

        .output-label {
            font-size: 1rem;
            font-weight: 700;
            color: var(--ink-color);
            text-transform: uppercase;
        }

        .output-content {
            font-family: 'Patrick Hand', cursive; /* Ensure handwritten feel */
            white-space: pre-wrap;
            font-size: 1.1rem;
            color: var(--ink-color);
            padding: 16px;
            max-height: 300px;
            overflow-y: auto;
            line-height: 1.4;
            background: 
                linear-gradient(90deg, transparent 95%, #eee 95%),
                linear-gradient(transparent 95%, #eee 95%);
            background-size: 20px 20px;
        }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 12px;
        }
        ::-webkit-scrollbar-track {
            background: transparent; 
        }
        ::-webkit-scrollbar-thumb {
            background: var(--pencil-gray);
            border: 3px solid transparent;
            background-clip: content-box;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background-color: var(--ink-color); 
        }

        .copy-btn {
            background: #fff;
            border: 1px solid var(--ink-color);
            padding: 4px 10px;
            border-radius: 10px;
            font-size: 0.9rem;
            cursor: pointer;
            color: var(--ink-color);
            box-shadow: 1px 1px 0px rgba(0,0,0,0.1);
        }

        .copy-btn:hover {
            background: var(--highlight);
            transform: scale(1.05);
        }

        .error-msg {
            color: #d63031;
            font-size: 1rem;
            margin-top: 8px;
            display: none;
            align-items: center;
            gap: 6px;
            font-weight: bold;
            transform: rotate(-1deg);
        }
        
        .error-msg::before {
            content: "🖍️";
        }

        .error-msg.visible {
            display: flex;
        }
        
        /* HR as a scribble */
        hr {
            border: 0;
            height: 4px;
            background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 10' preserveAspectRatio='none'%3E%3Cpath d='M0 5 Q 5 0, 10 5 T 20 5 T 30 5 T 40 5 T 50 5 T 60 5 T 70 5 T 80 5 T 90 5 T 100 5' stroke='%237f8c8d' fill='transparent' stroke-width='2'/%3E%3C/svg%3E");
            margin: 30px 0;
            opacity: 0.5;
        }

        /* Toast */
        #toast {
            visibility: hidden;
            min-width: 250px;
            background-color: var(--ink-color);
            color: #fff;
            text-align: center;
            padding: 12px 24px;
            /* Messy bubble shape */
            border-radius: 255px 15px 225px 15px / 15px 225px 15px 255px;
            position: fixed;
            z-index: 100;
            left: 50%;
            bottom: 30px;
            transform: translateX(-50%);
            font-size: 1.1rem;
            font-weight: 700;
            box-shadow: 4px 4px 0px rgba(0,0,0,0.2);
            border: 2px solid #fff;
        }

        #toast.show {
            visibility: visible;
            animation: bounceIn 0.5s;
        }

        @keyframes bounceIn {
            0% { transform: translateX(-50%) scale(0.1); opacity: 0;}
            60% { transform: translateX(-50%) scale(1.2); opacity: 1;}
            100% { transform: translateX(-50%) scale(1); }
        }

        /* Footer / Watermark */
        .footer {
            text-align: center;
            margin-top: 50px;
            margin-bottom: 20px;
            font-size: 1rem;
            color: var(--pencil-gray);
            transform: rotate(-1deg);
        }

        .footer p {
            margin: 5px 0;
        }

        .footer-link {
            color: var(--marker-green);
            text-decoration: none;
            font-weight: bold;
            border-bottom: 2px dashed var(--marker-green);
            transition: all 0.2s;
        }

        .footer-link:hover {
            color: var(--marker-green-hover);
            border-bottom-style: solid;
            background: var(--highlight);
        }

        /* Responsive Fixes */
        @media (max-width: 600px) {
            .header h1 { font-size: 2rem; }
            .btn-group { grid-template-columns: 1fr; }
        }

        /* Hand drawn arrow helper */
        .arrow-helper {
            font-size: 2rem;
            display: block;
            margin-bottom: 10px;
            transform: rotate(90deg);
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>🌙 Bukber Invite & Coordination</h1>
        <p>Coreta-coretan buat bikin undangan bukber yang asik.</p>
        <span class="badge">Sketchy Edition • Copas Ready</span>
    </div>

    <div class="main-grid">
        <!-- LEFT PANEL: INPUT -->
        <div class="input-panel">
            <div class="card">
                <div class="card-header">
                    <span class="card-title">📝 Detail Acara</span>
                </div>
                
                <div class="toggle-container">
                    <input type="checkbox" id="useEmoji" checked>
                    <label for="useEmoji" style="cursor: pointer; font-weight: bold;">Pakai Emoji (biar lebih asik)</label>
                </div>

                <div class="form-group">
                    <label class="form-label">Nama Event</label>
                    <input type="text" id="eventName" value="Bukber Ceria 2024" placeholder="Contoh: Bukber Alumni SMA 1">
                </div>

                <div class="form-group">
                    <label class="form-label">PIC (Koordinator)</label>
                    <input type="text" id="picName" placeholder="[NAMA_PIC]">
                    <span class="form-sublabel">Biarkan kosong untuk placeholder [NAMA_PIC]</span>
                </div>

                <div class="form-group">
                    <label class="form-label">Opsi Tanggal & Jam</label>
                    <span class="form-sublabel">Tulis 1 baris per opsi. Jika > 1 baris, otomatis jadi Polling.</span>
                    <textarea id="dateOptions" placeholder="Contoh:
Sabtu, 16 Mar 18.00
Minggu, 17 Mar 17.30"></textarea>
                    <div id="dateError" class="error-msg">Tanggal wajib diisi minimal 1 opsi.</div>
                </div>

                <div class="form-group">
                    <label class="form-label">Opsi Tempat (Resto)</label>
                    <span class="form-sublabel">Kosongkan jika belum ada ide (akan jadi polling).</span>
                    <textarea id="placeOptions" placeholder="Contoh:
Sate Khas Senayan
Bandar Djakarta"></textarea>
                </div>

                <div class="form-group">
                    <label class="form-label">Link Gmaps (Opsional)</label>
                    <input type="text" id="gmapsLink" placeholder="[LINK_GMAPS]">
                </div>

                <hr>

                <div class="form-group">
                    <label class="form-label">Patungan / DP (Per Orang)</label>
                    <div style="display: flex; align-items: center; gap: 12px;">
                        <span style="font-weight: 700; font-size: 1.2rem; transform:rotate(-5deg);">Rp</span>
                        <input type="number" id="dpAmount" value="0" min="0">
                    </div>
                </div>

                <div class="form-group" id="deadlineGroup" style="display: none;">
                    <label class="form-label">Deadline Bayar <span style="color:var(--marker-pink)">*</span></label>
                    <input type="text" id="dpDeadline" placeholder="Contoh: H-3 (Rabu, 13 Mar 21.00)">
                    <div id="deadlineError" class="error-msg">Deadline wajib diisi jika ada DP.</div>
                </div>

                <div class="form-group" id="paymentDetailsGroup" style="display: none;">
                    <label class="form-label">Info Rekening / E-Wallet</label>
                    <textarea id="bankDetails" placeholder="BCA 1234567890 a.n Budi
Gopay 08123456789"></textarea>
                </div>

                <hr>

                <div class="form-group">
                    <label class="form-label">Daftar Tugas (Task List)</label>
                    <textarea id="taskList" placeholder="Task - PIC"></textarea>
                    <button class="btn btn-outline" id="fillTaskBtn">Isi Template Task</button>
                </div>

                <div class="form-group">
                    <label class="form-label">Catatan Tambahan</label>
                    <textarea id="extraNotes" placeholder="Contoh: Parkir terbatas, bawa mukena sendiri, dresscode putih."></textarea>
                </div>

                <div class="btn-group">
                    <button class="btn" id="generateBtn">Generate Pesan</button>
                    <button class="btn btn-reset" id="resetBtn">Reset</button>
                </div>
            </div>
        </div>

        <!-- RIGHT PANEL: OUTPUT -->
        <div class="output-panel">
            <div class="card" style="background: #fff; transform: rotate(1deg);"> 
                <div class="card-header">
                    <span class="card-title">📨 Hasil (Siap Copas)</span>
                    <button class="btn-outline" id="copyAllBtn" style="margin:0; font-size: 0.9rem;">Copy Semua</button>
                </div>

                <div id="outputContainer">
                    <div style="text-align: center; color: var(--pencil-gray); padding: 60px 20px; border: 2px dashed var(--pencil-gray); border-radius: 10px;">
                        <span class="arrow-helper">👈</span>
                        <p style="font-size: 1.2rem;">Klik tombol <strong>Generate Pesan</strong> di panel kiri buat lihat hasilnya!</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Footer Watermark -->
    <div class="footer">
        <p>&copy; Abdul Muiz Wahyu 2026</p>
        <p>
            <a href="https://www.threads.net/@muizwhy" target="_blank" class="footer-link">Follow me on Threads @muizwhy</a>
        </p>
    </div>
</div>

<div id="toast">Copied to clipboard!</div>

<script>
    // --- DOM ELEMENTS ---
    const els = {
        useEmoji: document.getElementById('useEmoji'),
        eventName: document.getElementById('eventName'),
        picName: document.getElementById('picName'),
        dateOptions: document.getElementById('dateOptions'),
        placeOptions: document.getElementById('placeOptions'),
        gmapsLink: document.getElementById('gmapsLink'),
        dpAmount: document.getElementById('dpAmount'),
        dpDeadline: document.getElementById('dpDeadline'),
        bankDetails: document.getElementById('bankDetails'),
        taskList: document.getElementById('taskList'),
        extraNotes: document.getElementById('extraNotes'),
        
        // Groups
        deadlineGroup: document.getElementById('deadlineGroup'),
        paymentDetailsGroup: document.getElementById('paymentDetailsGroup'),
        
        // Errors
        dateError: document.getElementById('dateError'),
        deadlineError: document.getElementById('deadlineError'),
        
        // Buttons
        generateBtn: document.getElementById('generateBtn'),
        resetBtn: document.getElementById('resetBtn'),
        fillTaskBtn: document.getElementById('fillTaskBtn'),
        copyAllBtn: document.getElementById('copyAllBtn'),
        
        // Output
        outputContainer: document.getElementById('outputContainer'),
        toast: document.getElementById('toast')
    };

    // --- STATE & UTILS ---
    
    // Toggle DP Input visibility
    els.dpAmount.addEventListener('input', () => {
        const val = parseInt(els.dpAmount.value) || 0;
        if (val > 0) {
            els.deadlineGroup.style.display = 'block';
            els.paymentDetailsGroup.style.display = 'block';
        } else {
            els.deadlineGroup.style.display = 'none';
            els.paymentDetailsGroup.style.display = 'none';
        }
    });

    // Fill Task Template
    els.fillTaskBtn.addEventListener('click', () => {
        els.taskList.value = `Booking Tempat - [PIC]
Collect DP - [PIC]
Dokumentasi - [PIC]
Games (Opsional) - [PIC]`;
    });

    // Format Currency
    const formatRupiah = (num) => {
        return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(num);
    };

    // Parse Input Helper
    const parseLines = (text) => text.split('\n').map(l => l.trim()).filter(l => l.length > 0);

    // --- GENERATOR LOGIC ---

    els.generateBtn.addEventListener('click', () => {
        // 1. Validation
        let isValid = true;
        
        // Date Validation
        const dates = parseLines(els.dateOptions.value);
        if (dates.length === 0) {
            els.dateError.classList.add('visible');
            isValid = false;
        } else {
            els.dateError.classList.remove('visible');
        }

        // DP Validation
        const dpVal = parseInt(els.dpAmount.value) || 0;
        if (dpVal > 0 && !els.dpDeadline.value.trim()) {
            els.deadlineError.classList.add('visible');
            isValid = false;
        } else {
            els.deadlineError.classList.remove('visible');
        }

        if (!isValid) return;

        // 2. Data Preparation
        const data = {
            emoji: els.useEmoji.checked,
            event: els.eventName.value || 'Bukber',
            pic: els.picName.value.trim() || '[NAMA_PIC]',
            dates: dates,
            places: parseLines(els.placeOptions.value),
            gmaps: els.gmapsLink.value.trim() || '[LINK_GMAPS]',
            dp: dpVal,
            deadline: els.dpDeadline.value.trim(),
            bank: els.bankDetails.value.trim() || '[NO_REK/QRIS]',
            tasks: parseLines(els.taskList.value),
            notes: els.extraNotes.value.trim()
        };

        // Determine State
        const isDateFix = data.dates.length === 1;
        const isPlaceFix = data.places.length === 1;
        
        // Emojis (Contextual)
        const E = data.emoji ? {
            invite: '🍲', wave: '👋', fire: '😤', ghost: '👻',
            cal: '🗓', loc: '📍', money: '💰', warn: '⚠️',
            check: '✅', bell: '🔔', car: '🚗', info: 'ℹ️'
        } : {
            invite: '', wave: '', fire: '', ghost: '',
            cal: '', loc: '', money: '', warn: '',
            check: '', bell: '', car: '', info: ''
        };

        // --- BLOCK GENERATION ---

        let blocks = {};

        // A. UNDANGAN
        const dateText = isDateFix ? data.dates[0] : 'Cek Voting (Belum fix)';
        const placeText = isPlaceFix ? data.places[0] : (data.places.length === 0 ? '[NAMA_RESTO]' : 'Cek Voting (Belum fix)');
        
        const inviteShort = `${E.invite} *${data.event}* yuk!
Kapan: ${dateText}
Dimana: ${placeText}
List nama yg ikut 👇
1. ${data.pic}
2. ...`;

        const inviteNormal = `Halo semua! ${E.wave}
Silaturahmi yuk di *${data.event}*.

${E.loc} Tempat: ${placeText} ${isPlaceFix ? `(${data.gmaps})` : ''}
${E.cal} Waktu: ${dateText}
${E.money} Budget: ${data.dp > 0 ? formatRupiah(data.dp) : 'Bayar sendiri/patungan'}

Yg bisa join list nama ya biar di-booking! 🙏`;

        const inviteRame = `Woy gaes! ${E.fire}
Jangan cuma wacana, gas *${data.event}*!

Kapan: ${dateText}
Dimana: ${placeText}

Mumpung ${data.pic} mau urusin, buruan list! Jangan ghosting ${E.ghost}
List kehadiran:
1. ...`;

        blocks.invitation = `--- OPSI UNDANGAN (Pilih Satu) ---

1. Versi Singkat:
${inviteShort}

2. Versi Normal:
${inviteNormal}

3. Versi Rame:
${inviteRame}`;

        // B. POLLING
        let pollText = '';
        
        // Time Poll
        if (!isDateFix) {
            pollText += `*POLLING WAKTU* ${E.cal}\nPilih tanggal yg aman:\n`;
            data.dates.forEach((d, i) => pollText += `${i+1}. ${d}\n`);
            pollText += `Reply angka ya (1/${data.dates.length}).\n\n`;
        } else {
            pollText += `Polling Waktu: Tidak perlu (Fix: ${data.dates[0]})\n\n`;
        }

        // Place Poll
        if (!isPlaceFix) {
            pollText += `*POLLING TEMPAT* ${E.loc}\nBingung mau dimana? Vote yuk:\n`;
            if (data.places.length === 0) {
                pollText += `1. [NAMA_RESTO A]\n2. [NAMA_RESTO B]\n3. Usulan lain?\n`;
            } else {
                data.places.forEach((p, i) => pollText += `${i+1}. ${p}\n`);
            }
            pollText += `Pilih nomor berapa?`;
        } else {
            pollText += `Polling Tempat: Tidak perlu (Fix: ${data.places[0]})`;
        }
        
        blocks.polling = pollText.trim();

        // C. PATUNGAN / DP
        if (data.dp > 0) {
            blocks.payment = `*INFO PATUNGAN / DP* ${E.money}
Karena harus booking, tolong transfer DP ya.

Nominal: ${formatRupiah(data.dp)} /orang
Transfer ke:
${data.bank}

${E.warn} *Deadline:* ${data.deadline}
Konfirmasi: Kirim bukti trf ke ${data.pic}.
Format: Nama - Jumlah - Bukti`;
        } else {
            blocks.payment = `*INFO PEMBAYARAN* ${E.money}
Bayar masing-masing di tempat (open bill) atau patungan setelah makan ya.
Siapin cash/QRIS masing-masing.`;
        }

        // D. TASK / PIC
        let taskContent = '';
        if (data.tasks.length > 0) {
            data.tasks.forEach(t => taskContent += `${E.check} ${t}\n`);
        } else {
            taskContent = `${E.check} Booking & Menu: ${data.pic}\n${E.check} Dokumentasi: ...`;
        }
        
        blocks.tasks = `*PEMBAGIAN TUGAS*
Koordinator: ${data.pic}

Biar lancar, minta tolong handle ini:
${taskContent}
Reply "Bantu + task" kalau mau volunteer!`;

        // E. REMINDER
        const placeFinal = isPlaceFix ? data.places[0] : '[NAMA_RESTO]';
        const dateFinal = isDateFix ? data.dates[0] : '[TANGGAL]';
        
        blocks.reminder = `*REMINDER BUKBER* ${E.bell}
Besok/Nanti kita jadi bukber ya!

${E.loc} Lokasi: ${placeFinal} (${data.gmaps})
${E.cal} Kumpul: ${dateFinal}
${E.car} Info: Usahakan datang sebelum Maghrib.

Sampai ketemu guys!`;

        // F. NOTES
        const defaultNotes = `Parkir tersedia.
Toleransi keterlambatan 15 menit.
Info mushola ada di lokasi.`;
        
        blocks.notes = `*NOTES PENTING* ${E.info}
${data.notes ? data.notes : defaultNotes}

Update terakhir oleh: ${data.pic}`;

        // 3. Render Output
        renderOutput(blocks);
    });

    // Helper to Render
    function renderOutput(blocks) {
        els.outputContainer.innerHTML = '';
        
        const labels = {
            invitation: '1. Undangan',
            polling: '2. Polling',
            payment: '3. Patungan / DP',
            tasks: '4. Task & PIC',
            reminder: '5. Reminder',
            notes: '6. Notes Tambahan'
        };

        for (const [key, content] of Object.entries(blocks)) {
            const wrapper = document.createElement('div');
            wrapper.className = 'output-block';
            
            // Header for output block
            const header = document.createElement('div');
            header.className = 'output-header-row';

            const label = document.createElement('div');
            label.className = 'output-label';
            label.innerText = labels[key];
            
            const btn = document.createElement('button');
            btn.className = 'copy-btn';
            btn.innerText = 'Copy';
            btn.onclick = () => copyText(content);
            
            header.appendChild(label);
            header.appendChild(btn);

            const text = document.createElement('div');
            text.className = 'output-content';
            text.innerText = content;
            
            wrapper.appendChild(header);
            wrapper.appendChild(text);
            els.outputContainer.appendChild(wrapper);
        }
    }

    // --- UTILITIES ---

    // Copy Function
    function copyText(text) {
        navigator.clipboard.writeText(text).then(() => {
            showToast();
        }).catch(err => {
            console.error('Failed to copy: ', err);
            // Fallback
            const textarea = document.createElement('textarea');
            textarea.value = text;
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand('copy');
            document.body.removeChild(textarea);
            showToast();
        });
    }

    // Copy All Function
    els.copyAllBtn.addEventListener('click', () => {
        const blocks = document.querySelectorAll('.output-content');
        if (blocks.length === 0) return;
        
        let allText = '';
        blocks.forEach((block, index) => {
            const titles = [
                "--- UNDANGAN ---", "--- POLLING ---", "--- DP ---",
                "--- TASK ---", "--- REMINDER ---", "--- NOTES ---"
            ];
            allText += `${titles[index] || ''}\n${block.innerText}\n\n`;
        });
        
        copyText(allText);
    });

    // Toast
    function showToast() {
        els.toast.className = "show";
        setTimeout(() => { els.toast.className = els.toast.className.replace("show", ""); }, 3000);
    }

    // Reset
    els.resetBtn.addEventListener('click', () => {
        if(confirm('RESET SEMUA INPUT?')) {
            window.location.reload();
        }
    });

</script>

</body>
</html>
