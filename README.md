<!DOCTYPE html>
<html lang="id" class="scroll-smooth">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Tony Store - Jual Akun Game Terpercaya</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.6.0/css/all.min.css">
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght=400;600;700;800&display=swap');
    body { font-family: 'Plus Jakarta Sans', sans-serif; }
    .gold-glow { text-shadow: 0 0 20px rgba(251, 191, 36, 0.6); }
    .stark-grid { background-color: #030308; background-image: radial-gradient(circle at 50% 50%, rgba(10, 10, 35, 0.95), #010103), linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px), linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px); background-size: 100% 100%, 45px 45px, 45px 45px; background-position: center; }
    .stark-gradient { background: linear-gradient(135deg, rgba(153, 27, 27, 0.1) 0%, rgba(5, 5, 12, 0.95) 100%); }
    
    .feature-container { display: flex; flex-direction: column; gap: 1rem; }
    .feature-card { background: #05050c; padding: 1.5rem; border-radius: 1rem; border: 2px solid #0e0e22; color: #9ca3af; transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1); display: flex; gap: 1rem; align-items: start; }
    .feature-card .icon-box { color: #ffffff; transition: color 0.4s ease; }
    
    .feature-card.scanning { transform: scale(0.97); border-color: #fbbf24; background: #08081a; box-shadow: 0 0 25px rgba(251, 191, 36, 0.15); color: #ffffff; }
    .feature-card.scanning .icon-box { color: #fbbf24 !important; }

    .catalog-card-container { cursor: pointer; border-radius: 1.5rem; border: 2px solid #0e0e22; background: #05050c; transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); }
    .catalog-card-container.focus-active { border-color: #fbbf24 !important; box-shadow: 0 0 25px rgba(251, 191, 36, 0.2); }
    
    #tentang-modal, #donasi-modal, #qris-modal { position: fixed; inset: 0; z-index: 200; display: none; align-items: center; justify-content: center; opacity: 0; transition: opacity 0.3s ease-in-out; pointer-events: none; }
    #tentang-modal.show, #donasi-modal.show, #qris-modal.show { display: flex; opacity: 1; pointer-events: auto; }
    .modal-overlay { position: absolute; inset: 0; background-color: rgba(1, 1, 3, 0.7); backdrop-filter: blur(20px); transition: opacity 0.3s ease-in-out; }
    .modal-content { position: relative; background-color: #05050c; border: 2px solid #14142a; width: 90%; max-width: 480px; padding: 2rem; border-radius: 1.5rem; transform: scale(0.95); transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1); z-index: 210; }
    #tentang-modal.show .modal-content, #donasi-modal.show .modal-content, #qris-modal.show .modal-content { transform: scale(1); }
    #scrollable-tos { max-height: 200px; overflow-y: auto; padding-right: 8px; scrollbar-width: thin; scrollbar-color: #fbbf24 #05050c; }

    #loading-screen { position: fixed; inset: 0; z-index: 9999; background-color: #010103; display: flex; flex-direction: column; align-items: center; justify-content: center; transition: transform 0.8s cubic-bezier(0.25, 1, 0.5, 1), visibility 0.8s; }
    #main-content { opacity: 0; visibility: hidden; filter: blur(8px); transition: opacity 0.8s ease, filter 0.8s ease; }
    .loaded #loading-screen { transform: translateY(-100%); pointer-events: none !important; visibility: hidden !important; }
    .loaded #main-content { opacity: 1 !important; visibility: visible !important; filter: blur(0) !important; animation: fadeInUpSmooth 1.2s cubic-bezier(0.25, 1, 0.5, 1) forwards; }
    @keyframes fadeInUpSmooth { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    @keyframes loadingBar { 0% { left: -50%; width: 30%; } 50% { width: 60%; } 100% { left: 100%; width: 30%; } }
  </style>
  <script>
    // ============================================================
    //  MAPPING QR PER NOMINAL
    //  Rename file QR lu sesuai nama di bawah ini bre!
    // ============================================================
    const QR_MAP = {
      1000:   'qris-1000.jpg',
      2000:   'qris-2000.jpg',
      10000:  'qris-10000.jpg',
      20000:  'qris-20000.jpg',
      50000:  'qris-50000.jpg',
      100000: 'qris-100000.jpg'
    };

    function toggleDrawer() {
      const d = document.getElementById('mobile-drawer'), o = document.getElementById('drawer-overlay'), b = document.body;
      if (!d || !o) return;
      if (d.classList.contains('-translate-x-full')) {
        d.classList.remove('-translate-x-full'); o.classList.remove('opacity-0', 'pointer-events-none'); b.style.overflow = 'hidden';
      } else {
        d.classList.add('-translate-x-full'); o.classList.add('opacity-0', 'pointer-events-none'); b.style.overflow = 'auto'; b.style.overflowX = 'hidden';
      }
    }
    function toggleGamesMenu() {
      const s = document.getElementById('games-submenu'), a = document.getElementById('games-arrow');
      if (s && a) {
        if (s.style.display === 'none' || s.style.display === '') { s.style.display = 'flex'; a.style.transform = 'rotate(180deg)'; }
        else { s.style.display = 'none'; a.style.transform = 'rotate(0deg)'; }
      }
    }
    function switchPage(pageId) {
      ['page-utama', 'page-ff', 'page-roblox', 'page-tentang-toko', 'page-ham', 'page-all-links', 'page-donasi'].forEach(p => {
        const el = document.getElementById(p);
        if (el) { if (p === pageId) el.classList.remove('hidden'); else el.classList.add('hidden'); }
      });
      window.scrollTo(0, 0);
      const d = document.getElementById('mobile-drawer');
      if (d && !d.classList.contains('-translate-x-full')) toggleDrawer();
    }
    function handleCardClick(event, element) {
      document.querySelectorAll('.catalog-card-container').forEach(c => c.classList.remove('focus-active'));
      element.classList.add('focus-active');
      event.stopPropagation();
    }
    function openTentangModal() { const m = document.getElementById('tentang-modal'); m.classList.add('show'); document.body.style.overflow = 'hidden'; }
    function closeTentangModal() { const m = document.getElementById('tentang-modal'); m.classList.remove('show'); document.body.style.overflow = 'auto'; document.body.style.overflowX = 'hidden'; }

    let selectedMethod = '';
    let selectedAmount = 0;

    function openDonasiModal(method) {
      selectedMethod = method;
      selectedAmount = 0;
      document.getElementById('donasi-title').innerText = 'Donasi via ' + method.toUpperCase();
      document.querySelectorAll('.btn-nominal-choice').forEach(btn => {
        btn.classList.remove('border-yellow-500', 'bg-yellow-500/10', 'text-yellow-400');
        btn.classList.add('border-zinc-800', 'bg-zinc-950', 'text-zinc-400');
      });
      const m = document.getElementById('donasi-modal');
      m.classList.add('show');
      document.body.style.overflow = 'hidden';
    }
    function closeDonasiModal() {
      document.getElementById('donasi-modal').classList.remove('show');
      document.body.style.overflow = 'auto';
      document.body.style.overflowX = 'hidden';
    }
    function selectNominal(element, amount) {
      selectedAmount = amount;
      document.querySelectorAll('.btn-nominal-choice').forEach(btn => {
        btn.classList.remove('border-yellow-500', 'bg-yellow-500/10', 'text-yellow-400');
        btn.classList.add('border-zinc-800', 'bg-zinc-950', 'text-zinc-400');
      });
      element.classList.remove('border-zinc-800', 'bg-zinc-950', 'text-zinc-400');
      element.classList.add('border-yellow-500', 'bg-yellow-500/10', 'text-yellow-400');
    }
    function prosesDonasi() {
      const nama = document.getElementById('donatur-name').value.trim() || 'Hamba Allah';
      if (selectedAmount === 0) { alert('Pilih salah satu nominal donasi dulu dong bre!'); return; }

      const nominalFormatted = selectedAmount.toLocaleString('id-ID');

      // ── Ambil gambar QR sesuai nominal yang dipilih ──
      const qrSrc = QR_MAP[selectedAmount] || 'qris-1000.jpg';
      document.getElementById('qris-img').src = qrSrc;
      document.getElementById('qris-img').alt = `QR ${selectedMethod.toUpperCase()} Rp ${nominalFormatted}`;

      closeDonasiModal();

      // Tampilkan label nominal & metode di modal QRIS
      document.getElementById('qris-text-nominal').innerText = 'Rp ' + nominalFormatted;
      document.getElementById('qris-text-method').innerText = selectedMethod.toUpperCase();

      document.getElementById('qris-modal').classList.add('show');
      document.body.style.overflow = 'hidden';

      document.getElementById('btn-wa-qris').onclick = function () {
        const text = encodeURIComponent(
          `Halo Admin, gua udah kirim donasi via ${selectedMethod.toUpperCase()} sebesar Rp ${nominalFormatted} atas nama ${nama}. Tolong dicek bre!`
        );
        window.open(`https://wa.me/62895402912875?text=${text}`, '_blank');
      };
    }
    function closeQrisModal() {
      document.getElementById('qris-modal').classList.remove('show');
      document.body.style.overflow = 'auto';
      document.body.style.overflowX = 'hidden';
    }
    function copyNomor() {
      navigator.clipboard.writeText('62895402912875');
      const btn = document.getElementById('btn-copy-num');
      btn.innerHTML = '<i class="fas fa-check mr-2 text-green-400"></i> Berhasil Disalin!';
      setTimeout(() => { btn.innerHTML = '<i class="fas fa-copy mr-2"></i> Salin Nomor HP Admin'; }, 2000);
    }
  </script>
</head>
<body class="stark-grid text-gray-100 selection:bg-yellow-500 selection:text-black antialiased overflow-hidden">

  <!-- ===== MODAL TENTANG / TOS ===== -->
  <div id="tentang-modal">
    <div class="modal-overlay" onclick="closeTentangModal()"></div>
    <div class="modal-content">
      <button onclick="closeTentangModal()" class="absolute -top-3 -right-3 w-8 h-8 rounded-full bg-zinc-900 border border-zinc-700 text-white flex items-center justify-center"><i class="fas fa-times text-xs"></i></button>
      <div class="text-center mb-4"><h3 class="text-2xl font-black text-white">TONY<span class="text-yellow-500">STORE</span></h3></div>
      <div class="mb-4"><p class="text-sm font-semibold text-zinc-300">Website Ini Dibuat Oleh: Tony Stark 🗿</p></div>
      <div id="scrollable-tos" class="space-y-3 text-xs text-zinc-400">
        <p class="font-bold text-yellow-500">Ketentuan Layanan:</p>
        <p>Dengan melakukan transaksi di toko ini, Anda menyetujui seluruh mekanisme serah terima data aman kami. Seluruh proses bersifat transparan.</p>
        <p class="font-bold text-yellow-500">Term of Service Game:</p>
        <p>Pengguna diwajibkan mengamankan kredensial akun secara mandiri setelah data divalidasi dan diserahkan penuh oleh pihak admin kami.</p>
      </div>
    </div>
  </div>

  <!-- ===== MODAL DONASI ===== -->
  <div id="donasi-modal">
    <div class="modal-overlay" onclick="closeDonasiModal()"></div>
    <div class="modal-content border-yellow-500/30">
      <button onclick="closeDonasiModal()" class="absolute -top-3 -right-3 w-8 h-8 rounded-full bg-zinc-900 border border-zinc-700 text-white flex items-center justify-center"><i class="fas fa-times text-xs"></i></button>
      <h3 id="donasi-title" class="text-xl font-black text-white mb-4 text-center">Donasi</h3>
      <div class="space-y-4 text-sm">
        <div>
          <label class="block text-zinc-400 mb-2 font-semibold">Nama Pengirim (Opsional)</label>
          <input type="text" id="donatur-name" placeholder="Contoh: Tony Stark" class="w-full bg-zinc-950 border border-zinc-800 rounded-xl px-4 py-2.5 text-white focus:outline-none focus:border-yellow-500">
        </div>
        <div>
          <label class="block text-zinc-400 mb-2 font-semibold">Pilih Nominal Donasi</label>
          <div class="grid grid-cols-2 gap-2">
            <button onclick="selectNominal(this, 1000)"   class="btn-nominal-choice py-2.5 border border-zinc-800 bg-zinc-950 text-zinc-400 font-bold rounded-xl transition-all focus:outline-none">Rp 1.000</button>
            <button onclick="selectNominal(this, 2000)"   class="btn-nominal-choice py-2.5 border border-zinc-800 bg-zinc-950 text-zinc-400 font-bold rounded-xl transition-all focus:outline-none">Rp 2.000</button>
            <button onclick="selectNominal(this, 10000)"  class="btn-nominal-choice py-2.5 border border-zinc-800 bg-zinc-950 text-zinc-400 font-bold rounded-xl transition-all focus:outline-none">Rp 10.000</button>
            <button onclick="selectNominal(this, 20000)"  class="btn-nominal-choice py-2.5 border border-zinc-800 bg-zinc-950 text-zinc-400 font-bold rounded-xl transition-all focus:outline-none">Rp 20.000</button>
            <button onclick="selectNominal(this, 50000)"  class="btn-nominal-choice py-2.5 border border-zinc-800 bg-zinc-950 text-zinc-400 font-bold rounded-xl transition-all focus:outline-none">Rp 50.000</button>
            <button onclick="selectNominal(this, 100000)" class="btn-nominal-choice py-2.5 border border-zinc-800 bg-zinc-950 text-zinc-400 font-bold rounded-xl transition-all focus:outline-none">Rp 100.000</button>
          </div>
        </div>
        <button onclick="prosesDonasi()" class="w-full py-3 bg-yellow-500 hover:bg-yellow-400 text-black font-extrabold rounded-xl transition-all shadow-lg shadow-yellow-500/10 mt-2">LAKUKAN DONASI <i class="fas fa-arrow-right ml-1"></i></button>
      </div>
    </div>
  </div>

  <!-- ===== MODAL QRIS (gambar QR berubah sesuai nominal) ===== -->
  <div id="qris-modal">
    <div class="modal-overlay" onclick="closeQrisModal()"></div>
    <div class="modal-content border-zinc-800 max-w-[400px] p-5">
      <button onclick="closeQrisModal()" class="absolute -top-3 -right-3 w-8 h-8 rounded-full bg-zinc-900 border border-zinc-700 text-white flex items-center justify-center"><i class="fas fa-times text-xs"></i></button>
      <div class="text-center">
        <span class="text-[10px] bg-yellow-500/10 border border-yellow-500/20 text-yellow-500 px-3 py-1 rounded-full font-black uppercase tracking-wider">
          Metode: <span id="qris-text-method">QRIS</span>
        </span>
        <h4 class="text-2xl font-black text-white mt-2 mb-1" id="qris-text-nominal">Rp 0</h4>
        <p class="text-[11px] text-zinc-400 mb-4 leading-snug">
          Silakan screenshot QRIS di bawah ini, lalu bayar lewat aplikasi
          <span class="text-yellow-400 font-bold">DANA / GOPAY</span> lu bre!
        </p>

        <!-- ── Gambar QR — src diganti otomatis via JS sesuai nominal ── -->
        <div class="bg-white p-3 rounded-2xl mx-auto w-60 h-auto shadow-xl border border-zinc-200 overflow-hidden mb-4">
          <img id="qris-img" src="" alt="QRIS Tony Store" class="w-full h-auto object-contain">
        </div>

        <div class="space-y-2">
          <button id="btn-copy-num" onclick="copyNomor()" class="w-full py-2 bg-zinc-900 border border-zinc-800 text-zinc-300 font-bold text-xs rounded-lg hover:text-white transition-all">
            <i class="fas fa-copy mr-2"></i> Salin Nomor HP Admin
          </button>
          <button id="btn-wa-qris" class="w-full py-2.5 bg-emerald-600 hover:bg-emerald-500 text-white font-black text-xs rounded-lg transition-all shadow-md">
            <i class="fab fa-whatsapp mr-1.5 text-sm"></i> KONFIRMASI PEMBAYARAN VIA WA
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- ===== LOADING SCREEN ===== -->
  <div id="loading-screen">
    <div class="absolute inset-0 bg-[linear-gradient(rgba(255,255,255,0.02)_1px,transparent_1px),linear-gradient(90deg,rgba(255,255,255,0.02)_1px,transparent_1px)] bg-[size:30px_30px] opacity-40 pointer-events-none"></div>
    <div class="relative flex flex-col items-center z-10">
      <div class="absolute w-32 h-32 bg-yellow-500/20 rounded-full blur-2xl animate-pulse"></div>
      <h2 class="text-3xl md:text-4xl font-black tracking-widest text-white animate-bounce">TONY<span class="text-yellow-500 gold-glow">STORE</span></h2>
      <div class="w-40 h-[3px] bg-zinc-800 rounded-full mt-6 overflow-hidden relative">
        <div class="absolute top-0 left-0 h-full w-1/2 bg-gradient-to-r from-yellow-500 to-amber-400 rounded-full animate-[loadingBar_1.2s_infinite_ease-in-out]"></div>
      </div>
    </div>
  </div>
  <script>
    setTimeout(() => {
      document.body.classList.add('loaded');
      document.body.classList.remove('overflow-hidden');
      document.body.classList.add('overflow-x-hidden');
    }, 1500);
  </script>

  <!-- ===== MOBILE DRAWER (di luar main-content biar fixed tidak kena offset) ===== -->
  <div id="mobile-drawer" class="fixed inset-y-0 left-0 w-64 bg-[#030308] border-r border-zinc-900 z-[150] flex flex-col p-6 md:hidden transform -translate-x-full transition-transform duration-300 ease-in-out overflow-y-auto">
    <div class="flex items-center justify-between mb-8">
      <span class="text-xs font-bold text-gray-400 tracking-wider">NAVIGASI</span>
      <button onclick="toggleDrawer()" class="text-gray-400 text-xl hover:text-red-400 transition-colors"><i class="fas fa-times"></i></button>
    </div>
    <div class="flex flex-col gap-3 text-sm font-bold text-gray-300">
      <button onclick="switchPage('page-utama')" class="flex items-center gap-3 w-full py-2 text-white bg-zinc-900/50 px-3 rounded-xl text-left"><i class="fas fa-home text-yellow-500 w-5"></i> Utama</button>
      <button onclick="toggleGamesMenu()" class="flex items-center justify-between w-full py-2 border-b border-zinc-900/50 px-3 hover:text-yellow-400 text-left"><span class="flex items-center gap-3"><i class="fas fa-gamepad text-yellow-500 w-5"></i> Games</span><i id="games-arrow" class="fas fa-chevron-down text-xs transition-transform duration-200"></i></button>
      <div id="games-submenu" style="display: none;" class="flex-col gap-2 pl-8 pb-1">
        <button onclick="switchPage('page-ff')" class="flex items-center gap-3 w-full py-1.5 text-xs text-gray-400 hover:text-red-500 text-left"><i class="fas fa-fire-alt text-red-500 text-[10px]"></i> Free Fire</button>
        <button onclick="switchPage('page-roblox')" class="flex items-center gap-3 w-full py-1.5 text-xs text-gray-400 hover:text-blue-400 text-left"><i class="fas fa-cube text-blue-500 text-[10px]"></i> Roblox</button>
      </div>
      <button onclick="switchPage('page-tentang-toko')" class="flex items-center gap-3 py-2 border-b border-zinc-900/50 px-3 hover:text-yellow-400 text-left w-full"><i class="fas fa-store text-yellow-500 w-5"></i> Tentang Toko</button>
      <button onclick="switchPage('page-ham')" class="flex items-center gap-3 py-2 border-b border-zinc-900/50 px-3 hover:text-emerald-400 text-left w-full"><i class="fas fa-shield-alt text-emerald-500 w-5"></i> Perlindungan HAM & Hukum</button>
      <button onclick="switchPage('page-all-links')" class="flex items-center gap-3 py-2 border-b border-zinc-900/50 px-3 hover:text-purple-400 text-left w-full"><i class="fas fa-link text-purple-500 w-5"></i> All Links Hub</button>
      <button onclick="switchPage('page-donasi')" class="flex items-center gap-3 py-2 border-b border-zinc-900/50 px-3 hover:text-rose-400 text-left w-full"><i class="fas fa-heart text-rose-500 w-5"></i> Donasi Dukungan</button>
      <button onclick="openTentangModal(); toggleDrawer();" class="flex items-center gap-3 py-2 text-zinc-500 text-xs text-left w-full"><i class="fas fa-file-contract w-5"></i> Legalitas (ToS)</button>
    </div>
  </div>
  <div id="drawer-overlay" onclick="toggleDrawer()" class="fixed inset-0 bg-black/60 z-[140] md:hidden opacity-0 pointer-events-none transition-opacity duration-300 ease-in-out"></div>

  <!-- ===== MAIN CONTENT ===== -->
  <div id="main-content" class="w-full max-w-full overflow-x-hidden relative z-10">

    <!-- NAV -->
    <nav class="fixed top-0 left-0 right-0 z-[100] bg-[#04040a]/90 backdrop-blur-md border-b border-zinc-900 h-16">
      <div class="max-w-6xl mx-auto h-full px-6 flex items-center justify-between relative">
        <button onclick="toggleDrawer()" class="text-white text-xl p-2 md:hidden hover:text-yellow-400 transition-colors"><i class="fas fa-bars"></i></button>
        <button onclick="switchPage('page-utama')" class="text-xl font-extrabold text-white tracking-wider">TONY<span class="text-yellow-500">STORE</span></button>
        <div class="hidden md:flex items-center gap-5 text-xs font-semibold text-gray-400">
          <button onclick="switchPage('page-utama')" class="hover:text-yellow-400 transition-colors">Utama</button>
          <button onclick="switchPage('page-ff')" class="hover:text-red-500 transition-colors">Free Fire</button>
          <button onclick="switchPage('page-roblox')" class="hover:text-blue-400 transition-colors">Roblox</button>
          <button onclick="switchPage('page-tentang-toko')" class="hover:text-yellow-500 transition-colors">Tentang Toko</button>
          <button onclick="switchPage('page-ham')" class="hover:text-emerald-400 transition-colors">Perlindungan HAM & Hukum</button>
          <button onclick="switchPage('page-all-links')" class="hover:text-purple-400 transition-colors">All Links</button>
          <button onclick="switchPage('page-donasi')" class="hover:text-rose-400 transition-colors"><i class="fas fa-heart text-xs mr-1"></i> Donasi</button>
          <button onclick="openTentangModal()" class="text-zinc-500 hover:text-white transition-colors text-[11px] border border-zinc-800 px-2 py-0.5 rounded">ToS</button>
        </div>
      </div>
    </nav>


    <!-- ===== PAGE UTAMA ===== -->
    <div id="page-utama" class="w-full">
      <section class="relative min-h-screen flex items-center justify-center pt-24 pb-12">
        <div class="absolute top-1/4 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[300px] md:w-[600px] h-[300px] md:h-[600px] bg-red-700/10 rounded-full blur-[130px] pointer-events-none"></div>
        <div class="max-w-4xl mx-auto px-6 text-center z-10">
          <span class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-zinc-900/80 border border-zinc-800 text-xs font-semibold text-yellow-500 mb-6 uppercase"><span class="w-2 h-2 rounded-full bg-green-500 animate-pulse"></span> 100% Amanah & Bergaransi</span>
          <h1 class="text-4xl sm:text-6xl md:text-8xl font-extrabold tracking-tight mb-4 text-white leading-tight">TONY <span class="text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 to-amber-500 gold-glow">STORE</span></h1>
          <p class="text-base sm:text-xl text-gray-400 max-w-2xl mx-auto mb-10">Tempat nyari akun game murah meriah. <span class="text-yellow-400 font-semibold">Tersedia Spek Polosan maupun Spek Lumayan Sesuai Budgetmu!</span></p>
          <div class="flex flex-col sm:flex-row items-center justify-center gap-4">
            <a href="#produk" class="w-full sm:w-auto bg-gradient-to-r from-yellow-500 to-amber-500 hover:from-yellow-400 hover:to-amber-400 text-black font-bold text-lg px-8 py-4 rounded-xl shadow-lg shadow-yellow-500/20 text-center transition-all">Lihat Katalog <i class="fas fa-arrow-right ml-2 text-sm"></i></a>
            <button onclick="toggleDrawer()" class="w-full sm:w-auto bg-zinc-900/60 hover:bg-zinc-800 border border-zinc-800 text-white font-semibold text-lg px-8 py-4 rounded-xl text-center transition-all">Buka Menu Toko <i class="fas fa-gamepad ml-2 text-sm"></i></button>
          </div>
        </div>
      </section>

      <section id="fitur" class="py-20 border-t border-zinc-900/60">
        <div class="max-w-4xl mx-auto px-6">
          <div class="text-center mb-12"><h2 class="text-3xl md:text-4xl font-extrabold text-white mb-2">Kenapa Harus Belanja Di Sini?</h2><p class="text-gray-400 text-sm">Proses Cepat – Data akun segera dikirim secepat mungkin setelah pembayaran</p></div>
          <div id="wave-area" class="feature-container">
            <div id="feature-1" class="feature-card"><div class="w-10 h-10 rounded-xl bg-zinc-900 flex items-center justify-center icon-box shrink-0"><i class="fas fa-shield-alt"></i></div><div><h3 class="font-bold text-white text-lg">Jaminan Amanah</h3><p class="text-gray-400 text-sm mt-1">Transparan, no rip-off, dan semua akun dipastikan aman datanya sebelum dijual.</p></div></div>
            <div id="feature-2" class="feature-card"><div class="w-10 h-10 rounded-xl bg-zinc-900 flex items-center justify-center icon-box shrink-0"><i class="fas fa-bolt"></i></div><div><h3 class="font-bold text-white text-lg">Proses Cepat</h3><p class="text-gray-400 text-sm mt-1">Gak pakai drama nunggu lama. Begitu deal, data langsung dikirim malam itu juga.</p></div></div>
            <div id="feature-3" class="feature-card"><div class="w-10 h-10 rounded-xl bg-zinc-900 flex items-center justify-center icon-box shrink-0"><i class="fas fa-headset"></i></div><div><h3 class="font-bold text-white text-lg">Bisa Nanya Dulu</h3><p class="text-gray-400 text-sm mt-1">Admin ramah and santai. Mau tanya spek detail atau minta dipandu ganti data juga boleh.</p></div></div>
            <div id="feature-4" class="feature-card"><div class="w-10 h-10 rounded-xl bg-zinc-900 flex items-center justify-center icon-box shrink-0"><i class="fas fa-tags"></i></div><div><h3 class="font-bold text-white text-lg">Harga Bersahabat</h3><p class="text-gray-400 text-sm mt-1">Harga recehan pas banget buat kantong pelajar yang pengen punya akun siap main.</p></div></div>
          </div>
        </div>
      </section>

      <section id="produk" class="py-20 relative">
        <div class="max-w-6xl mx-auto px-6 relative z-10">
          <div class="text-center max-w-2xl mx-auto mb-12"><h2 class="text-3xl md:text-5xl font-extrabold text-white mb-2">Katalog Game</h2><p class="text-gray-400 text-sm">Stok diupdate berkala via Google Sheets otomatis.</p></div>
          <div class="grid md:grid-cols-2 gap-8">
            <div onclick="handleCardClick(event, this)" class="catalog-card-container stark-gradient p-8 flex flex-col justify-between">
              <div>
                <div class="flex items-center justify-between mb-6">
                  <h3 class="text-2xl font-extrabold text-white tracking-wide flex items-center gap-3 whitespace-nowrap"><i class="fas fa-fire-alt text-xl text-red-500"></i> FREE FIRE</h3>
                  <span id="badge-ff" class="text-xs bg-zinc-800 text-zinc-400 px-3 py-1 rounded-full font-bold uppercase">LOADING...</span>
                </div>
                <ul id="list-ff" class="space-y-3 text-gray-300 text-sm mb-8">
                  <li class="flex items-center gap-3"><i id="icon-ff-1" class="fas fa-check-circle text-zinc-600"></i> Tersedia varian akun polosan harga murah meriah</li>
                  <li class="flex items-center gap-3"><i id="icon-ff-2" class="fas fa-check-circle text-zinc-600"></i> Ada juga stok spek lumayan (level mapan & bundle event)</li>
                </ul>
              </div>
              <div id="container-btn-ff">
                <a href="https://wa.me/62895402912875?text=Halo%20Admin" target="_blank" onclick="event.stopPropagation();" class="block w-full text-center font-extrabold py-4 rounded-xl bg-zinc-800 text-zinc-500 text-base transition-colors">Tanya Stok Free Fire</a>
              </div>
            </div>
            <div onclick="handleCardClick(event, this)" class="catalog-card-container p-8 flex flex-col justify-between">
              <div>
                <div class="flex items-center justify-between mb-6">
                  <h3 class="text-2xl font-extrabold text-white tracking-wide flex items-center gap-3 whitespace-nowrap"><i class="fas fa-cube text-xl text-blue-500"></i> ROBLOX</h3>
                  <span id="badge-roblox" class="text-xs bg-zinc-800 text-zinc-400 px-3 py-1 rounded-full font-bold uppercase">LOADING...</span>
                </div>
                <ul id="list-roblox" class="space-y-3 text-gray-300 text-sm mb-8">
                  <li class="flex items-center gap-3"><i id="icon-roblox-1" class="fas fa-check-circle text-zinc-600"></i> Menyediakan akun fresh / polosan siap pakai</li>
                  <li class="flex items-center gap-3"><i id="icon-roblox-2" class="fas fa-check-circle text-zinc-600"></i> Tersedia akun berumur (Old Account) pas di dompet</li>
                </ul>
              </div>
              <div id="container-btn-roblox">
                <a href="https://wa.me/62895402912875?text=Halo%20Admin" target="_blank" onclick="event.stopPropagation();" class="block w-full text-center font-extrabold py-4 rounded-xl bg-zinc-800 text-zinc-500 text-base transition-colors">Tanya Stok Roblox</a>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>

    <!-- ===== PAGE FREE FIRE ===== -->
    <div id="page-ff" class="w-full hidden px-6 max-w-4xl mx-auto pt-28 pb-12">
      <div class="text-center mb-12"><span class="px-4 py-1.5 rounded-full bg-red-500/10 border border-red-500/20 text-xs font-bold text-red-400 uppercase tracking-widest">Medan Pertempuran Booyah</span><h2 class="text-4xl font-black text-white mt-3">FREE FIRE ZONE</h2></div>
      <div class="grid gap-6 space-y-4">
        <div class="bg-zinc-950/90 border border-zinc-800 p-6 rounded-2xl"><h3 class="text-lg font-bold text-red-400 mb-2"><i class="fas fa-quote-left mr-2"></i> Panggung Kehormatan Petarung</h3><p class="text-sm text-zinc-300 leading-relaxed italic">"Di sini bukan cuma soal siapa yang punya skin senjata paling mahal atau bundle paling langka, tapi tentang siapa yang bertahan paling akhir saat zona semakin menjepit. Akun tangguh kami siap jadi senjata rahasiamu menguasai papan peringkat."</p></div>
        <div class="bg-gradient-to-br from-amber-950/40 to-black border border-amber-500/30 p-6 rounded-2xl">
          <h3 class="text-lg font-bold text-yellow-500 mb-3"><i class="fas fa-crosshairs mr-2"></i> Rekomendasi Sensitivitas Auto Headshot (Update Sensi 200 Max)</h3>
          <div class="grid grid-cols-2 sm:grid-cols-3 gap-4 text-sm bg-zinc-900/60 p-4 rounded-xl">
            <div><p class="text-gray-400">Lihat Sekeliling</p><p class="text-xl font-bold text-white">185 - 200</p></div>
            <div><p class="text-gray-400">Red Dot Sight</p><p class="text-xl font-bold text-white">170 - 190</p></div>
            <div><p class="text-gray-400">2x Scope</p><p class="text-xl font-bold text-white">165 - 185</p></div>
            <div><p class="text-gray-400">4x Scope</p><p class="text-xl font-bold text-white">175 - 188</p></div>
            <div><p class="text-gray-400">Sniper Scope</p><p class="text-xl font-bold text-white">110 - 130</p></div>
            <div><p class="text-gray-400">Tombol Tembak</p><p class="text-xl font-bold text-white">40% - 50%</p></div>
          </div>
        </div>
        <div class="bg-zinc-950/80 border border-zinc-800 p-6 rounded-2xl"><h3 class="text-lg font-bold text-red-500 mb-2"><i class="fas fa-gamepad mr-2"></i> Pengenalan Free Fire</h3><p class="text-gray-400 text-sm leading-relaxed">Free Fire adalah game survival battle royale intensitas tinggi berdurasi 10 menit yang menuntut kecepatan taktik, kelincahan menembak, dan ketepatan reflexes penempatan objek pelindung Gloo Wall untuk memenangkan Booyah.</p></div>
        <div class="bg-zinc-950/80 border border-zinc-800 p-6 rounded-2xl"><h3 class="text-lg font-bold text-red-500 mb-2"><i class="fas fa-bolt mr-2"></i> Tips & Trik Kuasai Medan Pertempuran</h3><p class="text-gray-400 text-sm leading-relaxed">Gunakan teknik drag shot dengan menghentak tombol tembak kanan ke arah atas secara mendadak saat membidik musuh. Selalu utamakan rotasi cepat mencari posisi high-ground untuk mempermudah dapet sudut bidik kepala musuh.</p></div>
      </div>
    </div>

    <!-- ===== PAGE ROBLOX ===== -->
    <div id="page-roblox" class="w-full hidden px-6 max-w-4xl mx-auto pt-28 pb-12">
      <div class="text-center mb-12"><span class="px-4 py-1.5 rounded-full bg-blue-500/10 border border-blue-500/20 text-xs font-bold text-blue-400 uppercase tracking-widest">Imajinasi Tanpa Batas</span><h2 class="text-4xl font-black text-white mt-3">ROBLOX STATION</h2></div>
      <div class="grid gap-6 space-y-4">
        <div class="bg-zinc-950/90 border border-zinc-800 p-6 rounded-2xl"><h3 class="text-lg font-bold text-blue-400 mb-2"><i class="fas fa-quote-left mr-2"></i> Menembus Batas Kreativitas</h3><p class="text-sm text-zinc-300 leading-relaxed italic">"Roblox bukan sekadar tumpukan balok digital biasa, ini adalah gerbang di mana identitas barumu dibangun secara bebas bersama jutaan penjelajah di seluruh belahan dunia. Amankan akun terbaikmu dan mulailah mengukir sejarah barumu hari ini."</p></div>
        <div class="bg-zinc-950/80 border border-zinc-800 p-6 rounded-2xl"><h3 class="text-lg font-bold text-blue-400 mb-2"><i class="fas fa-cubes mr-2"></i> Pengenalan Dunia Roblox</h3><p class="text-gray-400 text-sm leading-relaxed">Roblox adalah sebuah semesta virtual masif tempat jutaan orang berkumpul secara global untuk bermain, berkreasi secara bebas, dan menjadi apa saja dalam ruang imajinasi buatan komunitas developer platform.</p></div>
        <div class="bg-zinc-950/80 border border-zinc-800 p-6 rounded-2xl"><h3 class="text-lg font-bold text-blue-400 mb-2"><i class="fas fa-star mr-2"></i> Tips & Trik Pro Platform Gaming</h3><p class="text-gray-400 text-sm leading-relaxed">Manfaatkan fitur Shift Lock di pengaturan untuk menjaga stabilitas arah gerak kamera, trik ini sangat krusial saat melewati rintangan sempit di mode game Obby ekstrem. Selalu amankan data akun dengan mengaktifkan verifikasi 2 langkah (2FA).</p></div>
      </div>
    </div>

    <!-- ===== PAGE TENTANG TOKO ===== -->
    <div id="page-tentang-toko" class="w-full hidden px-6 max-w-xl mx-auto pt-28 pb-12">
      <div class="bg-[#05050c] border border-zinc-900 p-8 rounded-3xl text-center shadow-xl">
        <div class="w-24 h-24 rounded-full overflow-hidden mx-auto mb-4 border-2 border-yellow-500">
          <img src="logotoko.png" alt="Tony Logo" class="w-full h-full object-cover">
        </div>
        <h2 class="text-2xl font-black text-white mb-1">Tentang Tony Store</h2>
        <p class="text-xs text-yellow-500 font-bold tracking-widest uppercase mb-4">Official Owner Identity</p>
        <div class="text-left text-xs sm:text-sm text-zinc-400 space-y-4 border-t border-zinc-900 pt-5">
          <p class="font-semibold text-zinc-200"><i class="fas fa-user text-yellow-500 mr-2"></i> Didirikan Oleh: <span class="text-white">Tony Stark</span> </p>
          <p class="font-semibold text-zinc-200"><i class="fas fa-calendar-alt text-yellow-500 mr-2"></i> Tanggal Berdiri Website: <span class="text-yellow-400">17 Mei 2026</span></p>
          <p class="italic text-zinc-300 leading-relaxed border-l-2 border-yellow-500 pl-3 bg-zinc-950/40 py-2 rounded-r-lg">"Kata-Kata Manis: Kepercayaan kalian adalah detak jantung dari berdirinya store ini. Kami hadir bukan cuma buat cari untung semata, tapi sebagai jembatan paling aman dan terpercaya buat nemenin langkah perjalanan gaming seru kalian."</p>
        </div>
      </div>
    </div>

    <!-- ===== PAGE HAM ===== -->
    <div id="page-ham" class="w-full hidden px-6 max-w-2xl mx-auto pt-28 pb-12">
      <div class="bg-[#05050c] border border-zinc-900 p-6 sm:p-8 rounded-3xl space-y-6">
        <div class="text-center border-b border-zinc-900 pb-4">
          <h2 class="text-2xl font-black text-white"><i class="fas fa-balance-scale text-emerald-500 mr-2"></i> Perlindungan HAM & Hukum Digital</h2>
          <p class="text-xs text-zinc-500 mt-1">Deklarasi Kepatuhan Hukum & Nilai Toleransi Universal</p>
        </div>
        <div class="space-y-3">
          <h3 class="text-sm font-bold text-emerald-400 uppercase tracking-wider"><i class="fas fa-gavel mr-1.5"></i> Pasal & Ayat Hukum Republik Indonesia</h3>
          <div class="p-4 bg-zinc-950 rounded-xl border border-zinc-900 space-y-2.5 text-xs text-zinc-400">
            <p><span class="font-semibold text-white block mb-0.5">Pasal 26 ayat (1) UU ITE:</span> "Kecuali ditentukan lain oleh Peraturan Perundang-undangan, penggunaan setiap informasi melalui media elektronik yang menyangkut data pribadi seseorang harus dilakukan atas persetujuan Orang yang bersangkutan."</p>
            <p><span class="font-semibold text-white block mb-0.5">Pasal 4 UU No. 8 Tahun 1999 (Perlindungan Konsumen):</span> "Konsumen berhak atas kenyamanan, keamanan, dan keselamatan dalam mengonsumsi barang dan/atau jasa yang ditawarkan."</p>
          </div>
        </div>
        <div class="space-y-3">
          <h3 class="text-sm font-bold text-yellow-500 uppercase tracking-wider"><i class="fas fa-book-heart mr-1.5"></i> Kutipan Nilai Luhur Kitab Suci</h3>
          <div class="p-4 bg-zinc-950 rounded-xl border border-zinc-900 space-y-3 text-xs text-zinc-400 italic">
            <div><p class="text-zinc-300">"Dan penuhilah janji; sesungguhnya janji itu pasti diminta pertanggungjawabannya."</p><span class="text-[10px] text-yellow-500/80 font-bold block mt-0.5 text-right">— Q.S. Al-Isra' : 34</span></div>
            <div class="border-t border-zinc-900 pt-2.5"><p class="text-zinc-300">"Janganlah kamu memeras sesamamu manusia dan janganlah kamu merampas haknya; janganlah kau tahan upah seorang upahan sampai pagi."</p><span class="text-[10px] text-yellow-500/80 font-bold block mt-0.5 text-right">— Imamat 19 : 13</span></div>
            <div class="border-t border-zinc-900 pt-2.5"><p class="text-zinc-300">"Kewajibanmu adalah bertindak demi kesejahteraan dunia, dengan demikian engkau tidak akan jatuh ke dalam perilaku tidak jujur."</p><span class="text-[10px] text-yellow-500/80 font-bold block mt-0.5 text-right">— Bhagavad Gita 3 : 20</span></div>
          </div>
        </div>
        <a href="https://aduanberkonten.id/" target="_blank" class="flex items-center justify-between p-4 bg-red-950/20 border border-red-900/40 hover:border-red-500 rounded-xl transition-all">
          <span class="text-xs text-red-400 font-bold"><i class="fas fa-bullhorn mr-2"></i> Pintu Layanan Pengaduan Konten Negatif / Web Resmi KOMDIGI</span>
          <i class="fas fa-external-link-alt text-red-400 text-xs"></i>
        </a>
      </div>
    </div>

    <!-- ===== PAGE ALL LINKS ===== -->
    <div id="page-all-links" class="w-full hidden px-6 max-w-xl mx-auto pt-28 pb-12">
      <div class="bg-[#05050c] border border-zinc-900 p-6 sm:p-8 rounded-3xl text-center">
        <h2 class="text-2xl font-black text-white mb-1"><i class="fas fa-link text-purple-400 mr-1.5"></i> All Links Center</h2>
        <p class="text-xs text-zinc-500 mb-6">Pusat Navigasi Link Resmi Terverifikasi Admin</p>
        <div class="flex flex-col gap-3">
          <a href="https://wa.me/62895402912875" target="_blank" class="w-full p-4 bg-zinc-950 border border-zinc-900 rounded-2xl flex items-center justify-between hover:border-emerald-500 transition-all group"><span class="font-bold text-sm text-gray-200 group-hover:text-emerald-400 transition-colors"><i class="fab fa-whatsapp mr-3 text-emerald-500 text-lg"></i> Link WhatsApp Admin</span><i class="fas fa-chevron-right text-xs text-zinc-600 group-hover:text-emerald-400"></i></a>
          <a href="https://whatsapp.com/channel/0029VbCjWPq9xVJn4Fu9bN0n" target="_blank" class="w-full p-4 bg-zinc-950 border border-zinc-900 rounded-2xl flex items-center justify-between hover:border-yellow-500 transition-all group"><span class="font-bold text-sm text-gray-200 group-hover:text-yellow-400 transition-colors"><i class="fas fa-bullhorn mr-3 text-yellow-500 text-base"></i> Link Saluran Update Stok</span><i class="fas fa-chevron-right text-xs text-zinc-600 group-hover:text-yellow-400"></i></a>
          <a href="https://whatsapp.com/channel/0029Vb8fpve5Ejy68FiI1h0n" target="_blank" class="w-full p-4 bg-zinc-950 border border-zinc-900 rounded-2xl flex items-center justify-between hover:border-amber-500 transition-all group"><span class="font-bold text-sm text-gray-200 group-hover:text-amber-400 transition-colors"><i class="fas fa-star mr-3 text-amber-500 text-base"></i> Link Saluran Testimoni Resmi</span><i class="fas fa-chevron-right text-xs text-zinc-600 group-hover:text-amber-400"></i></a>
          <a href="https://www.instagram.com/cecilionmaghrinb?igsh=MWh6c3E0aXhvczZ5bg==" target="_blank" class="w-full p-4 bg-zinc-950 border border-zinc-900 rounded-2xl flex items-center justify-between hover:border-pink-500 transition-all group"><span class="font-bold text-sm text-gray-200 group-hover:text-pink-400 transition-colors"><i class="fab fa-instagram mr-3 text-pink-500 text-lg"></i> Link Instagram Toko</span><i class="fas fa-chevron-right text-xs text-zinc-600 group-hover:text-pink-400"></i></a>
          <a href="https://www.tiktok.com/@miahedi8" target="_blank" class="w-full p-4 bg-zinc-950 border border-zinc-900 rounded-2xl flex items-center justify-between hover:border-white transition-all group"><span class="font-bold text-sm text-gray-200 group-hover:text-white transition-colors"><i class="fab fa-tiktok mr-3 text-zinc-400 text-lg"></i> Link TikTok Konten</span><i class="fas fa-chevron-right text-xs text-zinc-600 group-hover:text-white"></i></a>
        </div>
      </div>
    </div>

    <!-- ===== PAGE DONASI ===== -->
    <div id="page-donasi" class="w-full hidden px-6 max-w-xl mx-auto pt-28 pb-12">
      <div class="bg-[#05050c] border border-zinc-900 p-6 sm:p-8 rounded-3xl text-center shadow-xl relative overflow-hidden">
        <div class="absolute top-0 left-0 right-0 h-[2px] bg-gradient-to-r from-transparent via-amber-500 to-transparent"></div>
        <div class="w-16 h-16 rounded-full bg-yellow-500/10 border border-yellow-500/20 flex items-center justify-center mx-auto mb-4 text-xl text-yellow-500">
          <i class="fas fa-heart text-red-500 animate-pulse"></i>
        </div>
        <h2 class="text-2xl font-black text-white mb-2">Dukungan & Donasi</h2>
        <p class="text-xs text-zinc-400 leading-relaxed max-w-sm mx-auto mb-6">Website ini dikembangkan mandiri oleh admin. Kalau lu ngerasa kebantu, yuk traktir kopi biar admin makin semangat update stok! 🗿</p>
        <div class="flex flex-col gap-3">
          <button onclick="openDonasiModal('dana')" class="w-full p-4 bg-zinc-950 border border-zinc-900 rounded-2xl flex items-center justify-between font-bold text-sm text-gray-200 hover:border-blue-500 transition-all group">
            <span><i class="fas fa-wallet mr-2 text-blue-400"></i> Donasi via DANA (QRIS Otomatis)</span>
            <i class="fas fa-chevron-right text-xs text-zinc-600 group-hover:text-blue-400"></i>
          </button>
          <button onclick="openDonasiModal('gopay')" class="w-full p-4 bg-zinc-950 border border-zinc-900 rounded-2xl flex items-center justify-between font-bold text-sm text-gray-200 hover:border-emerald-500 transition-all group">
            <span><i class="fas fa-wallet mr-2 text-emerald-400"></i> Donasi via GOPAY (QRIS Otomatis)</span>
            <i class="fas fa-chevron-right text-xs text-zinc-600 group-hover:text-emerald-400"></i>
          </button>
        </div>
        <p class="text-[10px] text-zinc-600 mt-6 italic">*Berapapun donasi lu asli berharga banget. Makasih banyak bre! 🗿</p>
      </div>
    </div>

    <!-- ===== FOOTER ===== -->
    <footer class="mt-20 border-t border-zinc-900/60 bg-[#020205] py-12 relative overflow-hidden">
      <div class="absolute bottom-0 left-1/2 -translate-x-1/2 w-[500px] h-[150px] bg-yellow-500/5 rounded-full blur-[80px] pointer-events-none"></div>
      <div class="max-w-4xl mx-auto px-6 relative z-10">
        <div class="grid sm:grid-cols-2 gap-8 mb-10 items-start text-center sm:text-left">
          <div>
            <h3 class="text-xl font-extrabold text-white mb-3">TONY<span class="text-yellow-500">STORE</span></h3>
            <p class="text-xs text-gray-500 leading-relaxed max-w-xs mx-auto sm:mx-0">Penyedia layanan akun game terpercaya, amanah, cepat, and bersahabat. Kenyamanan transaksi lu adalah prioritas utama kami.</p>
          </div>
          <div class="flex flex-col sm:items-end gap-2 text-xs text-gray-400">
            <span class="font-bold text-white mb-1">Metode Pembayaran Aman :</span>
            <p><i class="fas fa-qrcode text-yellow-500 mr-1"></i> QRIS Otomatis (DANA / GOPAY)</p>
            <p class="text-[10px] text-zinc-600 mt-1">QR per nominal aktif — 6 pilihan tersedia</p>
          </div>
        </div>
        <div class="mb-10 space-y-3">
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
            <a href="https://whatsapp.com/channel/0029VbCjWPq9xVJn4Fu9bN0n" target="_blank" class="w-full py-4 px-6 bg-gradient-to-r from-amber-600 to-yellow-500 hover:from-amber-500 hover:to-yellow-400 text-black font-extrabold text-sm rounded-xl transition-all shadow-lg shadow-yellow-500/10 text-center flex items-center justify-center gap-2"><i class="fas fa-bullhorn text-base"></i> SALURAN STOK ADMIN</a>
            <a href="https://whatsapp.com/channel/0029Vb8fpve5Ejy68FiI1h0n" target="_blank" class="w-full py-4 px-6 bg-purple-800 hover:bg-purple-700 text-white font-extrabold text-sm rounded-xl transition-all shadow-lg shadow-purple-900/20 text-center flex items-center justify-center gap-2"><i class="fas fa-star text-base"></i> SALURAN TESTIMONI</a>
          </div>
          <a href="https://wa.me/62895402912875?text=Halo%20Admin,%20gua%20mau%20tanya%20stok%20akun%20game%20yang%20ready%20dong%20bre!" target="_blank" class="block w-full text-center font-black py-4 rounded-xl bg-emerald-600 text-white shadow-lg shadow-emerald-600/20 hover:bg-emerald-500 transition-colors text-sm sm:text-base uppercase tracking-wider">
            <i class="fab fa-whatsapp mr-1.5 text-lg align-middle"></i> Hubungi Admin Sekarang (Respon Cepat)
          </a>
        </div>
        <div class="border-t border-zinc-900/80 pt-6 flex flex-col sm:flex-row items-center justify-between gap-4 text-[11px] text-zinc-600 font-medium">
          <p>© 2026 <span class="text-zinc-500 font-bold">Tony Store</span>. Seluruh Hak Cipta Dilindungi.</p>
          <div class="flex gap-4">
            <button onclick="openTentangModal()" class="hover:text-zinc-400">Kebijakan Privasi</button>
            <button onclick="openTentangModal()" class="hover:text-zinc-400">Syarat & Ketentuan</button>
          </div>
        </div>
      </div>
    </footer>
  </div><!-- end main-content -->

  <script>
    // ============================================================
    //  GOOGLE SHEETS — pakai CSV export, jauh lebih reliable bre
    //  Pastikan sheet lu sudah di-publish:
    //  File → Share → Publish to web → pilih Sheet1 → CSV → Publish
    // ============================================================
    async function fetchExcelStatus() {
      const sheetId = "1TxnHwG6BXG299hcoIQqaL3LQ2uUNL5F9k9r8FXehq5g";
      // gid=0 = sheet pertama. Kalau sheet lu bukan sheet 1, ganti gid-nya.
      const url = `https://docs.google.com/spreadsheets/d/${sheetId}/export?format=csv&gid=0`;

      try {
        const response = await fetch(url);
        if (!response.ok) throw new Error('Fetch gagal: ' + response.status);

        const csvText = await response.text();

        // ── Parse CSV sederhana (split baris & koma) ──
        const rows = csvText.trim().split('\n').map(row =>
          row.split(',').map(cell => cell.replace(/^"|"$/g, '').trim())
        );

        let statusFF      = 'READY';
        let statusRoblox  = 'READY';
        // Kolom D (index 3) = item atas, Kolom E (index 4) = item bawah
        // Isi YES = centang biru, NO = silang merah
        let item1FF       = 'NO';
        let item2FF       = 'NO';
        let item1Roblox   = 'NO';
        let item2Roblox   = 'NO';

        // Mulai dari baris ke-2 (index 1) biar skip header
        for (let i = 1; i < rows.length; i++) {
          const row = rows[i];
          if (!row || row.length < 2) continue;

          const game   = (row[0] || '').toUpperCase();
          const status = (row[1] || '').toUpperCase().trim();
          // Kolom D & E (index 3 & 4), default NO kalau kosong
          const d      = (row[3] || 'NO').toUpperCase().trim();
          const e      = (row[4] || 'NO').toUpperCase().trim();

          if (game.includes('FREE FIRE') || game.includes('FF')) {
            statusFF    = status;
            item1FF     = d;
            item2FF     = e;
          }
          if (game.includes('ROBLOX')) {
            statusRoblox  = status;
            item1Roblox   = d;
            item2Roblox   = e;
          }
        }

        updateButton('ff',     statusFF,     item1FF,     item2FF);
        updateButton('roblox', statusRoblox, item1Roblox, item2Roblox);

      } catch (error) {
        console.error('Gagal ambil data Google Sheets:', error);
        // Fallback: tampilkan READY, icon default abu-abu
        updateButton('ff',     'READY', 'NO', 'NO');
        updateButton('roblox', 'READY', 'NO', 'NO');
      }
    }

    function updateButton(gameType, status, item1Status, item2Status) {
      const badge     = document.getElementById(`badge-${gameType}`);
      const container = document.getElementById(`container-btn-${gameType}`);
      if (!badge || !container) return;

      // ── Update badge ──
      if (status === 'READY') {
        badge.className = "text-[10px] bg-green-500/10 border border-green-500/20 text-green-400 px-3 py-0.5 rounded-full font-black uppercase tracking-wider animate-pulse";
        badge.innerText = "STOK READY";
      } else if (status === 'EMPTY') {
        badge.className = "text-[10px] bg-orange-500/10 border border-orange-500/20 text-orange-400 px-3 py-0.5 rounded-full font-black uppercase tracking-wider";
        badge.innerText = "STOK HABIS (REQ)";
      } else {
        badge.className = "text-[10px] bg-red-500/10 border border-red-500/20 text-red-400 px-3 py-0.5 rounded-full font-black uppercase tracking-wider";
        badge.innerText = "SOLD OUT";
      }

      // ── Update icon centang/silang — tiap item independen dari kolom D & E ──
      const prefix = gameType === 'ff' ? 'ff' : 'roblox';
      const icon1  = document.getElementById(`icon-${prefix}-1`);
      const icon2  = document.getElementById(`icon-${prefix}-2`);
      // YES = centang biru, selain YES = silang merah
      if (icon1) icon1.className = (item1Status === 'YES')
        ? 'fas fa-check-circle text-blue-400'
        : 'fas fa-times-circle text-red-500';
      if (icon2) icon2.className = (item2Status === 'YES')
        ? 'fas fa-check-circle text-blue-400'
        : 'fas fa-times-circle text-red-500';

      // ── Update tombol ──
      const waTextFF     = encodeURIComponent("Halo Admin, gua mau tanya stok akun Free Fire yang ready dong bre!");
      const waTextFFReq  = encodeURIComponent("Halo Admin, gua mau request stok atau titip cariin akun Free Fire dong bre!");
      const waTextRB     = encodeURIComponent("Halo Admin, gua mau tanya stok akun Roblox yang ready dong bre!");
      const waTextRBReq  = encodeURIComponent("Halo Admin, gua mau request stok atau titip cariin akun Roblox dong bre!");

      if (gameType === 'ff') {
        if (status === 'READY') {
          container.innerHTML = `<a href="https://wa.me/62895402912875?text=${waTextFF}" target="_blank" onclick="event.stopPropagation();" class="block w-full text-center font-extrabold py-4 rounded-xl bg-gradient-to-r from-red-600 to-amber-600 text-white shadow-lg shadow-red-600/20 hover:from-red-500 hover:to-amber-500 transition-colors"><span class="block text-base leading-tight">Tanya Stok Free Fire</span></a>`;
        } else if (status === 'EMPTY') {
          container.innerHTML = `<a href="https://wa.me/62895402912875?text=${waTextFFReq}" target="_blank" onclick="event.stopPropagation();" class="block w-full text-center font-extrabold py-4 rounded-xl bg-orange-600 text-white shadow-lg shadow-orange-600/20 hover:bg-orange-500 transition-colors"><span class="block text-base leading-tight">Request Stok Akun</span></a>`;
        } else {
          container.innerHTML = `<button disabled onclick="event.stopPropagation();" class="block w-full text-center font-extrabold py-4 rounded-xl bg-red-950 text-red-500 border border-red-900/50 cursor-not-allowed opacity-60"><span class="block text-base leading-tight">Stok Habis</span></button>`;
        }
      } else if (gameType === 'roblox') {
        if (status === 'READY') {
          container.innerHTML = `<a href="https://wa.me/62895402912875?text=${waTextRB}" target="_blank" onclick="event.stopPropagation();" class="block w-full text-center font-extrabold py-4 rounded-xl bg-gradient-to-r from-blue-600 to-indigo-600 text-white shadow-lg shadow-blue-600/20 hover:from-blue-500 hover:to-indigo-500 transition-colors"><span class="block text-base leading-tight">Tanya Stok Roblox</span></a>`;
        } else if (status === 'EMPTY') {
          container.innerHTML = `<a href="https://wa.me/62895402912875?text=${waTextRBReq}" target="_blank" onclick="event.stopPropagation();" class="block w-full text-center font-extrabold py-4 rounded-xl bg-orange-600 text-white shadow-lg shadow-orange-600/20 hover:bg-orange-500 transition-colors"><span class="block text-base leading-tight">Request Stok Akun</span></a>`;
        } else {
          container.innerHTML = `<button disabled onclick="event.stopPropagation();" class="block w-full text-center font-extrabold py-4 rounded-xl bg-red-950 text-red-500 border border-red-900/50 cursor-not-allowed opacity-60"><span class="block text-base leading-tight">Stok Habis</span></button>`;
        }
      }
    }

    // ── Scanning wave animasi fitur ──
    const ids = ['feature-1', 'feature-2', 'feature-3', 'feature-4'];
    let index = 0, direction = 1;
    function scanWave() {
      ids.forEach(id => { const el = document.getElementById(id); if (el) el.classList.remove('scanning'); });
      const currentEl = document.getElementById(ids[index]);
      if (currentEl) currentEl.classList.add('scanning');
      index += direction;
      if (index === ids.length - 1 || index === 0) direction *= -1;
    }

    window.addEventListener('DOMContentLoaded', () => {
      fetchExcelStatus();
      setInterval(scanWave, 2000);
    });
  </script>
</body>
</html>


