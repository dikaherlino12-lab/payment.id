<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Payment Gateway - Dika Herlino</title>
    <!-- FontAwesome Ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }

        body {
            position: relative;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 30px 20px;
            overflow-x: hidden;
            background-color: #f8fafc;
        }

        /* --- Latar Belakang Foto Terang & Tanpa Blur --- */
        .bg-image {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url('https://files.catbox.moe/oetijz.webp');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            filter: brightness(0.95);
            z-index: 0;
        }

        .bg-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.15);
            z-index: 1;
        }

        /* --- Animasi Gelembung --- */
        .bubbles {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 2;
            overflow: hidden;
            pointer-events: none;
        }

        .bubble {
            position: absolute;
            bottom: -100px;
            background: rgba(255, 255, 255, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.6);
            border-radius: 50%;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
            animation: rise 10s infinite ease-in-out;
        }

        .bubble:nth-child(1) { width: 40px; height: 40px; left: 10%; animation-duration: 8s; }
        .bubble:nth-child(2) { width: 20px; height: 20px; left: 20%; animation-duration: 5s; animation-delay: 1s; }
        .bubble:nth-child(3) { width: 50px; height: 50px; left: 35%; animation-duration: 10s; animation-delay: 2s; }
        .bubble:nth-child(4) { width: 80px; height: 80px; left: 50%; animation-duration: 12s; }
        .bubble:nth-child(5) { width: 35px; height: 35px; left: 65%; animation-duration: 7s; animation-delay: 3s; }
        .bubble:nth-child(6) { width: 60px; height: 60px; left: 80%; animation-duration: 9s; animation-delay: 1s; }
        .bubble:nth-child(7) { width: 25px; height: 25px; left: 90%; animation-duration: 6s; animation-delay: 4s; }

        @keyframes rise {
            0% { transform: translateY(0) scale(1); opacity: 0; }
            20% { opacity: 0.8; }
            100% { transform: translateY(-120vh) scale(1.2); opacity: 0; }
        }

        /* --- Kartu Utama --- */
        .card-container {
            position: relative;
            z-index: 3;
            background: rgba(255, 255, 255, 0.55);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.8);
            width: 100%;
            max-width: 440px;
            padding: 30px 24px;
            border-radius: 24px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.25),
                        0 0 15px rgba(255, 255, 255, 0.5);
            text-align: center;
            color: #1e293b;
        }

        /* Video Header */
        .video-wrapper {
            position: relative;
            width: 100%;
            height: 180px;
            border-radius: 16px;
            overflow: hidden;
            margin-bottom: 20px;
            border: 2px solid rgba(255, 255, 255, 0.8);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
            cursor: pointer;
        }

        .video-wrapper video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }

        .sound-btn {
            position: absolute;
            bottom: 12px;
            right: 12px;
            background: rgba(255, 255, 255, 0.85);
            color: #0284c7;
            border: 1px solid rgba(255, 255, 255, 0.9);
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 6px;
            cursor: pointer;
            backdrop-filter: blur(5px);
            z-index: 5;
        }

        .profile-name {
            font-size: 22px;
            font-weight: 700;
            margin-bottom: 4px;
            color: #0f172a;
        }

        .profile-bio {
            font-size: 13px;
            color: #334155;
            margin-bottom: 20px;
            font-weight: 500;
        }

        /* --- Daftar Kotak Metode Pembayaran Langsung --- */
        .payment-list-container {
            display: flex;
            flex-direction: column;
            gap: 14px;
            margin-bottom: 20px;
        }

        .payment-box {
            background: rgba(255, 255, 255, 0.75);
            border: 1px solid rgba(255, 255, 255, 0.9);
            border-radius: 14px;
            padding: 16px;
            text-align: center;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            transition: transform 0.2s ease;
        }

        .payment-box:hover {
            transform: translateY(-2px);
            background: rgba(255, 255, 255, 0.9);
        }

        .payment-box-title {
            font-size: 13px;
            font-weight: 700;
            color: #0f172a;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
        }

        .qris-img {
            width: 100%;
            max-width: 170px;
            height: auto;
            border-radius: 8px;
            border: 2px solid #e2e8f0;
            margin-bottom: 8px;
            background: #fff;
            padding: 4px;
        }

        .payment-detail-text {
            font-size: 12px;
            color: #1e293b;
            font-weight: 600;
            margin-bottom: 4px;
        }

        .payment-detail-sub {
            font-size: 11px;
            color: #475569;
            margin-bottom: 12px;
        }

        .btn-wa-pay {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
            background: #25d366;
            color: white;
            padding: 8px 12px;
            border-radius: 8px;
            font-size: 11px;
            font-weight: 600;
            text-decoration: none;
            transition: background 0.2s;
            width: 100%;
        }

        .btn-wa-pay:hover {
            background: #20ba5a;
        }

        /* Warna Ikon */
        .fa-qrcode { color: #8b5cf6; }
        .fa-wallet-dana { color: #108ee9; }
        .fa-wallet-gopay { color: #00a5ec; }
        .fa-building-columns { color: #f97316; }

        /* --- Langkah-langkah Bersinar (Di Atas) --- */
        .steps-container {
            margin-top: 15px;
            background: rgba(255, 255, 255, 0.6);
            border: 1px solid rgba(2, 132, 199, 0.4);
            border-radius: 14px;
            padding: 14px;
            text-align: left;
        }

        .steps-container h4 {
            font-size: 12px;
            color: #0284c7;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 6px;
            font-weight: 700;
        }

        .step-item {
            font-size: 11px;
            color: #1e293b;
            margin-bottom: 6px;
            padding: 7px 10px;
            background: rgba(255, 255, 255, 0.85);
            border-radius: 8px;
            position: relative;
            overflow: hidden;
            animation: glowingBorder 3s infinite alternate;
        }

        @keyframes glowingBorder {
            0% {
                box-shadow: 0 0 2px rgba(2, 132, 199, 0.2), inset 0 0 2px rgba(2, 132, 199, 0.1);
                border-color: rgba(2, 132, 199, 0.3);
            }
            100% {
                box-shadow: 0 0 10px rgba(56, 189, 248, 0.8), inset 0 0 5px rgba(56, 189, 248, 0.5);
                border-color: #38bdf8;
            }
        }

        .step-item:nth-child(2) { animation-delay: 0.5s; }
        .step-item:nth-child(3) { animation-delay: 1s; }
        .step-item:nth-child(4) { animation-delay: 1.5s; }

        /* --- Riwayat Transaksi & Jumlah Transaksi (Di Paling Bawah) --- */
        .transaction-section {
            margin-top: 15px;
            background: rgba(255, 255, 255, 0.6);
            border: 1px solid rgba(203, 213, 225, 0.8);
            border-radius: 14px;
            padding: 14px;
            text-align: left;
        }

        .transaction-section h4 {
            font-size: 12px;
            color: #0f172a;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 6px;
            font-weight: 700;
        }

        .transaction-list {
            max-height: 130px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 6px;
            margin-bottom: 10px;
        }

        .trx-item {
            background: rgba(255, 255, 255, 0.8);
            padding: 8px 10px;
            border-radius: 8px;
            font-size: 11px;
            border-left: 3px solid #10b981;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .trx-item.pending {
            border-left-color: #f59e0b;
        }

        .trx-info b {
            display: block;
            color: #0f172a;
        }

        .trx-info span {
            color: #64748b;
            font-size: 10px;
        }

        .trx-amount {
            font-weight: 700;
            color: #0f172a;
            text-align: right;
        }

        .trx-amount span {
            display: block;
            font-size: 9px;
            font-weight: 500;
            color: #64748b;
        }

        /* Kotak Total & No Transaksi di Paling Bawah Riwayat */
        .transaction-summary {
            background: rgba(255, 255, 255, 0.9);
            border: 1px dashed #0284c7;
            border-radius: 10px;
            padding: 10px;
            font-size: 11px;
            color: #0f172a;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .transaction-summary div span {
            display: block;
            color: #64748b;
            font-size: 9.5px;
        }

        .transaction-summary div b {
            color: #0284c7;
            font-size: 12px;
        }

        .footer {
            margin-top: 15px;
            font-size: 10px;
            color: #475569;
            font-weight: 500;
        }
    </style>
</head>
<body>

    <!-- Latar Belakang Foto -->
    <div class="bg-image"></div>
    <div class="bg-overlay"></div>

    <!-- Animasi Gelembung -->
    <div class="bubbles">
        <div class="bubble"></div>
        <div class="bubble"></div>
        <div class="bubble"></div>
        <div class="bubble"></div>
        <div class="bubble"></div>
        <div class="bubble"></div>
        <div class="bubble"></div>
    </div>

    <!-- Kartu Utama -->
    <div class="card-container">
        
        <!-- Video Header -->
        <div class="video-wrapper" id="videoContainer">
            <video id="myVideo" autoplay loop muted playsinline>
                <source src="https://files.catbox.moe/ae657y.mp4" type="video/mp4">
                Browser Anda tidak mendukung tag video.
            </video>
            <button class="sound-btn" id="soundBtn">
                <i class="fa-solid fa-volume-xmark" id="soundIcon"></i>
                <span id="soundText"></span>
            </button>
        </div>

        <h1 class="profile-name"></h1>
        <p class="profile-bio">Silakan Pilih Metode Pembayaran</p>

        <!-- Daftar Metode Pembayaran (Semua Tampil Langsung) -->
        <div class="payment-list-container">
            
            <!-- 1. QRIS -->
            <div class="payment-box">
                <div class="payment-box-title"><i class="fa-solid fa-qrcode"></i> QRIS (All Payment)</div>
                <img src="https://files.catbox.moe/637104.jpg" alt="QRIS Code" class="qris-img">
                <p class="payment-detail-sub">scan qris di atas dengan m-banking atau e-wallet.</p>
                <a href="#" target="_blank" class="btn-wa-pay" id="waQrisBtn">
                    <i class="fa-brands fa-whatsapp"></i> konfirmasi qris via whatsapp 
                </a>
            </div>

            <!-- 2. DANA -->
            <div class="payment-box">
                <div class="payment-box-title"><i class="fa-solid fa-wallet fa-wallet-dana"></i> DANA</div>
                <p class="payment-detail-text">083157686023</p>
                <p class="payment-detail-sub">Atas Nama: TIDAK ADA</p>
                <a href="#" target="_blank" class="btn-wa-pay" id="waDanaBtn">
                    <i class="fa-brands fa-whatsapp"></i> konfirmasi dana via whatsapp 
                </a>
            </div>

            <!-- 3. GOPAY -->
            <div class="payment-box">
                <div class="payment-box-title"><i class="fa-solid fa-wallet fa-wallet-gopay"></i> GOPAY</div>
                <p class="payment-detail-text">085655570905</p>
                <p class="payment-detail-sub">Atas Nama: RUMIATIK ATAU DIKA HERLINO</p>
                <a href="#" target="_blank" class="btn-wa-pay" id="waGopayBtn">
                    <i class="fa-brands fa-whatsapp"></i> konfirmasi gopay via whatsapp 
                </a>
            </div>

            <!-- 4. SeaBank -->
            <div class="payment-box">
                <div class="payment-box-title"><i class="fa-solid fa-building-columns"></i> SeaBank</div>
                <p class="payment-detail-text">901417989184</p>
                <p class="payment-detail-sub">Atas Nama: HAMDAN</p>
                <a href="#" target="_blank" class="btn-wa-pay" id="waSeabankBtn">
                    <i class="fa-brands fa-whatsapp"></i> konfirmasi seabank via whatsapp 
                </a>
            </div>

        </div>

        <!-- Langkah-langkah Pembayaran (Bergerak Bersinar) - Di Atas -->
        <div class="steps-container">
            <h4><i class="fa-solid fa-circle-info"></i> Langkah-Langkah Pembayaran:</h4>
            <div class="step-item">1. pilih metode pembayaran di atas.</div>
            <div class="step-item">2. lakukan transfer sesuai nominal tagihan.</div>
            <div class="step-item">3. klik tombol konfirmasi whatsapp untuk kirim bukti.</div>
            <div class="step-item">4. cek status transaksi pada riwayat di bawah.</div>
        </div>

        <!-- Riwayat Transaksi & Jumlah Transaksi - Di Paling Bawah -->
        <div class="transaction-section">
            <h4><i class="fa-solid fa-clock-rotate-left"></i> Riwayat & Jumlah Transaksi:</h4>
            <div class="transaction-list">
                <div class="trx-item">
                    <div class="trx-info">
                        <b>Buy WhatsApp Bot</b>
                        <span>QRIS • Sukses</span>
                    </div>
                    <div class="trx-amount">
                        Rp 15.000
                        <span>#TRX-98241</span>
                    </div>
                </div>
                <div class="trx-item">
                    <div class="trx-info">
                        <b>Panel DigitalOcean</b>
                        <span>DANA • Sukses</span>
                    </div>
                    <div class="trx-amount">
                        Rp 8.500
                        <span>#TRX-98240</span>
                    </div>
                </div>
                <div class="trx-item pending">
                    <div class="trx-info">
                        <b>RDP VPS</b>
                        <span>SeaBank • Pending</span>
                    </div>
                    <div class="trx-amount" style="color: #f59e0b;">
                        Rp 12.000
                        <span style="color: #b45309;">#TRX-98239</span>
                    </div>
                </div>
            </div>

            <!-- Jumlah Transaksi Fake & No Transaksi Terakhir di Paling Bawah -->
            <div class="transaction-summary">
                <div>
                    <span>Total Transaksi Fake:</span>
                    <b>142 Berhasil</b>
                </div>
                <div style="text-align: right;">
                    <span>No. Transaksi Terakhir:</span>
                    <b>#TRX-98241</b>
                </div>
            </div>
        </div>

        <div class="footer">
            © 2026 Dika Herlino. All rights reserved.
        </div>

    </div>

    <!-- Script JavaScript untuk Tombol WhatsApp -->
    <script>
        // Ganti nomor WhatsApp di bawah ini dengan nomor Anda (format internasional tanpa tanda +, contoh: 6281234567890)
        const nomorAdminWhatsApp = "6285753320475";

        // Set otomatis link WhatsApp untuk setiap metode pembayaran
        document.getElementById('waQrisBtn').href = `https://wa.me/${nomorAdminWhatsApp}?text=` + encodeURIComponent("Halo Admin, saya sudah melakukan pembayaran via QRIS (#TRX-98241). Berikut bukti transfernya:");
        document.getElementById('waDanaBtn').href = `https://wa.me/${nomorAdminWhatsApp}?text=` + encodeURIComponent("Halo Admin, saya sudah melakukan pembayaran via DANA. Berikut bukti transfernya:");
        document.getElementById('waGopayBtn').href = `https://wa.me/${nomorAdminWhatsApp}?text=` + encodeURIComponent("Halo Admin, saya sudah melakukan pembayaran via GOPAY. Berikut bukti transfernya:");
        document.getElementById('waSeabankBtn').href = `https://wa.me/${nomorAdminWhatsApp}?text=` + encodeURIComponent("Halo Admin, saya sudah melakukan pembayaran via SeaBank. Berikut bukti transfernya:");

        // Kontrol Video & Suara
        const video = document.getElementById('myVideo');
        const soundBtn = document.getElementById('soundBtn');
        const soundIcon = document.getElementById('soundIcon');
        const soundText = document.getElementById('soundText');
        const videoContainer = document.getElementById('videoContainer');

        function toggleSound(e) {
            if (e) e.stopPropagation();
            if (video.muted) {
                video.muted = false;
                soundIcon.className = 'fa-solid fa-volume-high';
                soundText.innerText = 'Suara On';
            } else {
                video.muted = true;
                soundIcon.className = 'fa-solid fa-volume-xmark';
                soundText.innerText = 'Suara Off';
            }
        }
        soundBtn.addEventListener('click', toggleSound);
        videoContainer.addEventListener('click', toggleSound);
    </script>
</body>
</html>
