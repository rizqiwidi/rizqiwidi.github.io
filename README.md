<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WOM Finance - Policy System v6</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Plus Jakarta Sans', sans-serif; }
    .main-bg { background: linear-gradient(135deg, #f1f5f9 0%, #e0e7ff 50%, #ede9fe 100%); min-height: 100vh; }
    .card { background: white; border-radius: 1.25rem; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05); border: 1px solid rgba(255,255,255,0.6); }
    .form-input { width: 100%; padding: 0.75rem 1rem; border: 1px solid #e2e8f0; border-radius: 0.75rem; background: #f8fafc; transition: all 0.2s; font-size: 0.875rem; color: #334155; }
    .form-input:focus { border-color: #6366f1; background: white; box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.15); outline: none; }
    .nav-tab { padding: 0.75rem 1.25rem; font-weight: 600; font-size: 0.875rem; border-radius: 0.75rem; transition: all 0.3s; color: #64748b; border: 1px solid transparent; background: transparent; cursor: pointer; }
    .nav-tab:hover { background: rgba(255,255,255,0.6); color: #4f46e5; }
    .nav-tab.active { background: white; color: #4f46e5; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); border-color: #e0e7ff; }
    .btn-check { background: linear-gradient(135deg, #6366f1 0%, #8b5cf6 100%); color: white; font-weight: 600; padding: 0.75rem 1.5rem; border-radius: 0.75rem; border: none; cursor: pointer; box-shadow: 0 4px 11px rgba(99, 102, 241, 0.3); transition: all 0.2s; }
    .btn-check:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(99, 102, 241, 0.4); }
    .prod-item { padding: 1rem; border: 2px solid #e2e8f0; border-radius: 1rem; cursor: pointer; transition: all 0.2s; background: white; text-align: center; }
    .prod-item:hover { border-color: #a5b4fc; }
    .prod-item.active { border-color: #6366f1; background: #eef2ff; box-shadow: 0 4px 10px rgba(99, 102, 241, 0.1); }
    .page-section { display: none; animation: fadeIn 0.4s ease; }
    .page-section.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    .rp-wrap { position: relative; }
    .rp-pre { position: absolute; left: 1rem; top: 50%; transform: translateY(-50%); color: #64748b; font-weight: 500; font-size: 0.875rem; pointer-events: none; }
    .rp-inp { padding-left: 2.5rem !important; }
    .section-title { font-weight: 700; font-size: 0.8rem; color: #475569; border-left: 4px solid #6366f1; padding-left: 0.75rem; margin-bottom: 0.75rem; text-transform: uppercase; letter-spacing: 0.05em; }
    .section-title.teal { border-color: #14b8a6; }
    .section-title.cyan { border-color: #06b6d4; }
    .section-title.rose { border-color: #f43f5e; }
    .section-title.amber { border-color: #f59e0b; }
    .result-item { display: flex; gap: 0.5rem; align-items: flex-start; padding: 0.5rem 0; border-bottom: 1px solid #f1f5f9; }
    .result-item:last-child { border-bottom: none; }
    .status-badge { display: inline-flex; align-items: center; gap: 0.25rem; padding: 0.25rem 0.75rem; border-radius: 9999px; font-size: 0.75rem; font-weight: 600; }
    .status-pass { background: #dcfce7; color: #166534; border: 1px solid #bbf7d0; }
    .status-fail { background: #fee2e2; color: #991b1b; border: 1px solid #fecaca; }
    .status-warn { background: #fef3c7; color: #92400e; border: 1px solid #fde68a; }
    .hidden { display: none !important; }
  </style>
</head>
<body class="main-bg text-slate-700">

  <!-- Header -->
  <header class="sticky top-0 z-50 backdrop-blur-lg bg-white/40 border-b border-white/50">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 py-4 flex flex-col md:flex-row justify-between items-center gap-4">
      <div class="flex items-center gap-3">
        <div class="w-10 h-10 bg-gradient-to-br from-indigo-500 to-purple-500 rounded-xl flex items-center justify-center shadow-md">
          <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"></path></svg>
        </div>
        <div>
          <h1 class="font-bold text-lg text-slate-800">Dompet Kredit</h1>
          <p class="text-xs text-slate-500">Credit Eligibility Checker</p>
        </div>
      </div>
      <nav class="flex gap-2 p-1 bg-white/50 rounded-xl backdrop-blur-sm border border-white/50">
        <button onclick="navigatePage('norkil')" id="btn-norkil" class="nav-tab active">Normal Kilat</button>
        <button onclick="navigatePage('deviasi')" id="btn-deviasi" class="nav-tab">Deviasi</button>
        <button onclick="navigatePage('super')" id="btn-super" class="nav-tab">MobilKu Super</button>
      </nav>
    </div>
  </header>

  <!-- Main -->
  <main class="max-w-7xl mx-auto px-4 sm:px-6 py-6">

    <!-- ================= NORMAL KILAT PAGE ================= -->
    <section id="page-norkil" class="page-section active">
      <div class="grid grid-cols-12 gap-6">
        <div class="col-span-12 lg:col-span-8 space-y-5">
          
          <!-- Product Selection -->
          <div class="card p-5">
            <h3 class="section-title">Pilih Produk Pembiayaan</h3>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
              <div id="prod-mobilku" onclick="selectProduct('mobilku')" class="prod-item active"><div class="font-bold text-indigo-700">MobilKu</div><div class="text-xs text-slate-500 mt-1">SK 186</div></div>
              <div id="prod-motorku" onclick="selectProduct('motorku')" class="prod-item"><div class="font-bold text-teal-700">MotorKu</div><div class="text-xs text-slate-500 mt-1">SK 167</div></div>
              <div id="prod-newbike_bpjs" onclick="selectProduct('newbike_bpjs')" class="prod-item"><div class="font-bold text-cyan-700">NewBike BPJS</div><div class="text-xs text-slate-500 mt-1">SK 156</div></div>
              <div id="prod-newbike_nonbpjs" onclick="selectProduct('newbike_nonbpjs')" class="prod-item"><div class="font-bold text-rose-700">NewBike Non-BPJS</div><div class="text-xs text-slate-500 mt-1">SK 155</div></div>
            </div>
          </div>

          <!-- Input Form -->
          <div class="card p-5">
            <div class="space-y-5">
              
              <!-- Data Konsumen -->
              <div>
                <h3 class="section-title">Data Konsumen</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4">
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Status Konsumen *</label>
                    <select id="nk_status" class="form-input" onchange="updateNorkilUI()">
                      <option value="baru">Konsumen Baru</option>
                      <option value="eksisting">Eksisting</option>
                    </select>
                  </div>
                  <div id="row_rating" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Rating *</label>
                    <select id="nk_rating" class="form-input" onchange="updateNorkilUI()">
                      <option value="excellent">Excellent</option>
                      <option value="good">Good</option>
                      <option value="normal">Normal</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Lama Kerja *</label>
                    <select id="nk_lama_kerja" class="form-input">
                      <option value="">Pilih</option>
                      <option value="kurang">Kurang dari 1 Tahun</option>
                      <option value="1tahun">1 Tahun atau Lebih</option>
                      <option value="6bulan">6 Bulan (Khusus NB)</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Status Rumah *</label>
                    <select id="nk_rumah" class="form-input" onchange="updateNorkilUI()">
                      <option value="sendiri">Milik Sendiri/Keluarga</option>
                      <option value="sewa">Sewa/Kontrak/Kost</option>
                    </select>
                  </div>
                  <div id="row_lama_tinggal" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Lama Tinggal (Bulan) *</label>
                    <input type="number" id="nk_lama_tinggal" class="form-input" placeholder="Jumlah bulan">
                  </div>
                  <div id="row_region" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Region *</label>
                    <select id="nk_region" class="form-input">
                      <option value="lainnya">Lainnya</option>
                      <option value="jabodebek">Jabodebek</option>
                      <option value="banten">Banten</option>
                      <option value="kalimantan">Kalimantan</option>
                      <option value="sulawesi">Sulawesi</option>
                      <option value="jatim_bnt">Jatim BNT</option>
                      <option value="sumbagut">Sumbagut</option>
                      <option value="sumbagsel">Sumbagsel</option>
                      <option value="jabar">Jabar</option>
                      <option value="jatengut">Jateng Utara</option>
                      <option value="jatengsel">Jateng Selatan</option>
                    </select>
                  </div>
                </div>
              </div>

              <!-- Pengecekan Data -->
              <div>
                <h3 class="section-title teal">Hasil Pengecekan Data</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
                  <div id="row_dukcapil" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Dukcapil *</label>
                    <select id="nk_dukcapil" class="form-input">
                      <option value="match">Match</option>
                      <option value="notmatch">Not Match</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">KBIJ Pemohon *</label>
                    <select id="nk_kbij" class="form-input" onchange="updateNorkilUI()">
                      <option value="yes">Yes</option>
                      <option value="no">No</option>
                      <option value="notfound">Not Found</option>
                    </select>
                  </div>
                  <div id="row_kbij_pas">
                    <label class="block text-xs font-medium text-slate-600 mb-1">KBIJ Pasangan *</label>
                    <select id="nk_kbij_pas" class="form-input">
                      <option value="yes">Yes</option>
                      <option value="no">No</option>
                      <option value="notfound">Not Found</option>
                      <option value="tidakada">Tidak Ada Pasangan</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Internal Neg List *</label>
                    <select id="nk_internal" class="form-input">
                      <option value="tidak">Tidak Termasuk</option>
                      <option value="ya">Termasuk</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Riwayat TB/WO *</label>
                    <select id="nk_tbwo" class="form-input">
                      <option value="tidak">Tidak Pernah</option>
                      <option value="ya">Pernah</option>
                    </select>
                  </div>
                  <div id="row_dpd" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">DPD Terakhir (Hari) *</label>
                    <input type="number" id="nk_dpd" class="form-input" placeholder="0">
                  </div>
                  <div id="row_bd_norkil" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Baki Debet BI *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="nk_bd" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                </div>
              </div>

              <!-- BPJS Check -->
              <div id="row_bpjs" class="hidden">
                <h3 class="section-title cyan">Pengecekan BPJS</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Status BPJS *</label>
                    <select id="nk_bpjs_status" class="form-input">
                      <option value="terdaftar">Terdaftar</option>
                      <option value="tidak">Tidak Terdaftar</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Penghasilan JMO *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="nk_bpjs_income" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="Dari JMO">
                    </div>
                  </div>
                </div>
              </div>

              <!-- Keuangan -->
              <div>
                <h3 class="section-title cyan">Keuangan</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Jenis Penghasilan *</label>
                    <select id="nk_jenis_inc" class="form-input">
                      <option value="fixed">Fixed Income</option>
                      <option value="nonfixed">Non Fixed</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Penghasilan *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="nk_income" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Angsuran Diajukan *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="nk_angs" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Total Exposure *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="nk_exp" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                  <!-- LTV Field -->
                  <div id="row_ltv" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">LTV Diajukan (%) *</label>
                    <input type="number" id="nk_ltv" class="form-input" placeholder="Contoh: 80">
                  </div>
                  <!-- DP Field -->
                  <div id="row_dp_perc" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Down Payment (%) *</label>
                    <input type="number" id="nk_dp_perc" class="form-input" placeholder="Contoh: 20">
                  </div>
                  <div id="row_angs_old" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Angsuran Sebelumnya *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="nk_angs_old" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Tenor *</label>
                    <select id="nk_tenor" class="form-input">
                      <option value="12">12 Bulan</option>
                      <option value="24">24 Bulan</option>
                      <option value="36">36 Bulan</option>
                      <option value="48">48 Bulan</option>
                    </select>
                  </div>
                </div>
              </div>

              <!-- Kendaraan -->
              <div id="row_kendaraan">
                <h3 class="section-title rose">Data Kendaraan</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4">
                  <div id="row_jenis_kend">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Jenis Kendaraan *</label>
                    <select id="nk_jenis_kend" class="form-input" onchange="updateNorkilUI()">
                      <option value="passenger">Passenger Car</option>
                      <option value="commercial">Commercial Car</option>
                    </select>
                  </div>
                  <div id="row_kategori_kend">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Kategori Kendaraan *</label>
                    <select id="nk_kategori" class="form-input">
                      <option value="A">Kategori A</option>
                      <option value="B">Kategori B</option>
                      <option value="C">Kategori C</option>
                    </select>
                  </div>
                  <div id="row_thn_kend">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Tahun Kendaraan *</label>
                    <input type="number" id="nk_thn_kend" class="form-input" placeholder="Contoh: 2018">
                  </div>
                  <div id="row_bpkb">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Kepemilikan BPKB *</label>
                    <select id="nk_bpkb" class="form-input">
                      <option value="sendiri">Sendiri/Pasangan/Ortu</option>
                      <option value="oranglain">Orang Lain</option>
                    </select>
                  </div>
                  <div id="row_tipe_motor" class="hidden">
                    <label class="block text-xs font-medium text-slate-600 mb-1">Tipe Motor *</label>
                    <select id="nk_tipe_motor" class="form-input">
                      <option value="standar">Standar (Beat, Revo, Mio, dll)</option>
                      <option value="premium">Premium (PCX, NMAX, Vario 160, dll)</option>
                    </select>
                  </div>
                </div>
              </div>

              <div class="pt-4 flex justify-end">
                <button onclick="checkNorkil()" class="btn-check">Cek Kelayakan</button>
              </div>
            </div>
          </div>
        </div>

        <!-- Result Column -->
        <div class="col-span-12 lg:col-span-4">
          <div class="sticky top-28">
            <div class="card p-5 bg-white/90 backdrop-blur-sm border-indigo-100">
              <h3 class="section-title">Hasil Analisa</h3>
              <div id="res-norkil" class="text-sm text-slate-500 text-center py-8">Menunggu input...</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ================= DEVIASI PAGE ================= -->
    <section id="page-deviasi" class="page-section">
      <div class="grid grid-cols-12 gap-6">
        <div class="col-span-12 lg:col-span-8 space-y-5">
          <div class="card p-5">
            <div class="mb-5">
              <h2 class="text-lg font-bold text-slate-800">Pendeteksi Deviasi</h2>
              <p class="text-sm text-slate-500">Masukkan kondisi aktual untuk menemukan level approval.</p>
            </div>
            <div class="space-y-5">
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                <div>
                  <label class="block text-xs font-medium text-slate-600 mb-1">Jenis Produk *</label>
                  <select id="dev_prod" class="form-input" onchange="updateDeviasiUI()">
                    <option value="motor">MotorKu / NB</option>
                    <option value="mobil">MobilKu</option>
                    <option value="masku">MasKu</option>
                  </select>
                </div>
                <div>
                  <label class="block text-xs font-medium text-slate-600 mb-1">Status Konsumen *</label>
                  <select id="dev_status" class="form-input" onchange="updateDeviasiUI()">
                    <option value="baru">Baru</option>
                    <option value="eksisting">Eksisting</option>
                  </select>
                </div>
                <div id="dev_row_rating" class="hidden">
                  <label class="block text-xs font-medium text-slate-600 mb-1">Rating Internal *</label>
                  <select id="dev_rating" class="form-input">
                    <option value="none">Normal / Baru</option>
                    <option value="excellent">Excellent</option>
                    <option value="good">Good</option>
                  </select>
                </div>
              </div>

              <div class="bg-indigo-50 p-4 rounded-xl border border-indigo-100">
                <h4 class="font-semibold text-sm text-indigo-800 mb-3">Kondisi Negative List</h4>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                  <div>
                    <label class="block text-xs text-slate-600 mb-1">Kolektibilitas BI *</label>
                    <select id="dev_kol" class="form-input">
                      <option value="1">Kol 1</option>
                      <option value="2">Kol 2</option>
                      <option value="3">Kol 3</option>
                      <option value="4">Kol 4</option>
                      <option value="5">Kol 5</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs text-slate-600 mb-1">Baki Debet (Rp) *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="dev_bd" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                </div>
              </div>

              <div class="bg-teal-50 p-4 rounded-xl border border-teal-100">
                <h4 class="font-semibold text-sm text-teal-800 mb-3">Kondisi Profil & DSR</h4>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                  <div>
                    <label class="block text-xs text-slate-600 mb-1">Penghasilan *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="dev_income" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                  <div>
                    <label class="block text-xs text-slate-600 mb-1">Total Angsuran *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="dev_angs" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                  <div>
                    <label class="block text-xs text-slate-600 mb-1">Usia Pemohon *</label>
                    <input type="number" id="dev_usia" class="form-input" placeholder="Tahun">
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Total Exposure *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="dev_exp" class="form-input rp-inp" oninput="formatRupiah(this)" placeholder="0">
                    </div>
                  </div>
                </div>
              </div>

              <div class="bg-rose-50 p-4 rounded-xl border border-rose-100">
                <h4 class="font-semibold text-sm text-rose-800 mb-3">Kondisi Produk & Kendaraan</h4>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                  <div>
                    <label class="block text-xs text-slate-600 mb-1">Tenor Diajukan *</label>
                    <select id="dev_tenor" class="form-input">
                      <option value="12">12 Bulan</option>
                      <option value="24">24 Bulan</option>
                      <option value="36">36 Bulan</option>
                      <option value="48">48 Bulan</option>
                      <option value="60">60 Bulan</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs text-slate-600 mb-1">LTV Diajukan (%) *</label>
                    <input type="number" id="dev_ltv" class="form-input" placeholder="Contoh: 85">
                  </div>
                  <div>
                    <label class="block text-xs text-slate-600 mb-1">Usia Kendaraan (Tahun) *</label>
                    <input type="number" id="dev_usia_kend" class="form-input" placeholder="Usia kendaraan">
                  </div>
                </div>
              </div>

              <div class="pt-4 flex justify-end">
                <button onclick="checkDeviasi()" class="btn-check">Cek Level Approval</button>
              </div>
            </div>
          </div>
        </div>
        <div class="col-span-12 lg:col-span-4">
          <div class="sticky top-28">
            <div class="card p-5 bg-white/90 backdrop-blur-sm border-teal-100">
              <h3 class="section-title">Hasil Deteksi</h3>
              <div id="res-deviasi" class="text-sm text-slate-500 text-center py-8">Menunggu input...</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ================= SUPER PAGE ================= -->
    <section id="page-super" class="page-section">
      <div class="grid grid-cols-12 gap-6">
        <div class="col-span-12 lg:col-span-8 space-y-5">
          <div class="card p-5">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-5 border-b pb-4 gap-3">
              <div>
                <h2 class="text-lg font-bold text-slate-800">MobilKu Super</h2>
                <p class="text-xs text-slate-500">Program RO & Take Over</p>
              </div>
              <div class="flex bg-slate-100 rounded-lg p-1">
                <button id="super-ro" onclick="setSuperType('ro')" class="px-4 py-1.5 text-xs font-bold rounded bg-white shadow text-indigo-600">RO Super</button>
                <button id="super-to" onclick="setSuperType('to')" class="px-4 py-1.5 text-xs font-bold rounded text-slate-500">Take Over</button>
              </div>
            </div>

            <div class="space-y-5">
              <div>
                <h3 class="section-title">Data Konsumen</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4">
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Rating Konsumen *</label>
                    <select id="sp_rating" class="form-input">
                      <option value="excellent">Excellent</option>
                      <option value="good">Good</option>
                      <option value="normal">Normal</option>
                      <option value="warning">Warning</option>
                      <option value="bad">Bad Customer</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Usia Pemohon *</label>
                    <input type="number" id="sp_usia" class="form-input" placeholder="Tahun">
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Jenis Pekerjaan *</label>
                    <select id="sp_kerja" class="form-input">
                      <option value="karyawan">Karyawan</option>
                      <option value="wiraswasta">Wiraswasta</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Status Rumah *</label>
                    <select id="sp_rumah" class="form-input">
                      <option value="milik">Milik Sendiri/Keluarga</option>
                      <option value="sewa">Sewa/Kontrak/Kost</option>
                    </select>
                  </div>
                </div>
              </div>

              <div>
                <h3 class="section-title teal">Pengecekan Negative List</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">KBIJ Pemohon *</label>
                    <select id="sp_kbij" class="form-input">
                      <option value="yes">Yes</option>
                      <option value="no">No</option>
                      <option value="notfound">Not Found</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Internal Neg List *</label>
                    <select id="sp_internal" class="form-input">
                      <option value="tidak">Tidak Termasuk</option>
                      <option value="ya">Termasuk</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Riwayat TB/WO *</label>
                    <select id="sp_tbwo" class="form-input">
                      <option value="tidak">Tidak Pernah</option>
                      <option value="ya">Pernah</option>
                    </select>
                  </div>
                </div>
              </div>

              <div>
                <h3 class="section-title cyan">Keuangan</h3>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Penghasilan *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="sp_income" class="form-input rp-inp" oninput="formatRupiah(this)">
                    </div>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Angsuran Baru *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="sp_angs" class="form-input rp-inp" oninput="formatRupiah(this)">
                    </div>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Exposure *</label>
                    <div class="rp-wrap">
                      <span class="rp-pre">Rp</span>
                      <input type="text" id="sp_exp" class="form-input rp-inp" oninput="formatRupiah(this)">
                    </div>
                  </div>
                </div>
              </div>

              <div>
                <h3 class="section-title rose">Data Kendaraan</h3>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Jenis Kendaraan *</label>
                    <select id="sp_jenis" class="form-input">
                      <option value="passenger">Passenger</option>
                      <option value="commercial">Commercial</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Tahun Kendaraan *</label>
                    <input type="number" id="sp_thn" class="form-input" placeholder="2018">
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Tenor *</label>
                    <select id="sp_tenor" class="form-input">
                      <option value="12">12 Bulan</option>
                      <option value="24">24 Bulan</option>
                      <option value="36">36 Bulan</option>
                      <option value="48">48 Bulan</option>
                      <option value="60">60 Bulan</option>
                    </select>
                  </div>
                </div>
              </div>

              <div id="row_to_specific" class="hidden">
                <h3 class="section-title amber">Data Take Over</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Angsuran Sudah Dibayar (Bulan) *</label>
                    <input type="number" id="sp_to_bayar" class="form-input" placeholder="Jumlah bulan">
                  </div>
                  <div>
                    <label class="block text-xs font-medium text-slate-600 mb-1">Kondisi Pembayaran *</label>
                    <select id="sp_to_kondisi" class="form-input">
                      <option value="current">Current</option>
                      <option value="od">Ada Overdue</option>
                    </select>
                  </div>
                </div>
              </div>

              <div class="pt-4 flex justify-end">
                <button onclick="checkSuper()" class="btn-check">Cek Kelayakan Super</button>
              </div>
            </div>
          </div>
        </div>
        <div class="col-span-12 lg:col-span-4">
          <div class="sticky top-28">
            <div class="card p-5 bg-white/90 backdrop-blur-sm border-rose-100">
              <h3 class="section-title">Hasil Evaluasi</h3>
              <div id="res-super" class="text-sm text-slate-500 text-center py-8">Menunggu input...</div>
            </div>
          </div>
        </div>
      </div>
    </section>

  </main>

  <script>
    /* ===== UTILITIES ===== */
    const $ = (id) => document.getElementById(id);
    const formatRupiah = (input) => { let value = input.value.replace(/\D/g, ''); input.value = new Intl.NumberFormat('id-ID').format(value); };
    const parseNum = (str) => { if (!str) return 0; return parseInt(str.toString().replace(/\D/g, ''), 10) || 0; };
    const getVal = (id) => $(id) ? $(id).value : '';
    const formatCurr = (num) => 'Rp ' + new Intl.NumberFormat('id-ID').format(num);

    /* ===== NAVIGATION LOGIC ===== */
    function navigatePage(pageName) {
      document.querySelectorAll('.page-section').forEach(el => el.classList.remove('active'));
      document.querySelectorAll('.nav-tab').forEach(el => el.classList.remove('active'));
      $('page-' + pageName).classList.add('active');
      $('btn-' + pageName).classList.add('active');
    }

    /* ===== VALIDATION HELPER ===== */
    function checkEmpty(requiredIds) {
      for (let id of requiredIds) {
        const el = $(id);
        if (el && el.offsetParent !== null) { 
           if (el.tagName === "SELECT" && (el.value === "")) { alert('Data belum lengkap.'); return false; }
           if (el.tagName === "INPUT") {
             if(el.type === "number" && el.value === "") { alert('Data belum lengkap.'); return false; }
             if(el.classList.contains('rp-inp') && parseNum(el.value) === 0) { alert('Data belum lengkap.'); return false; }
           }
        }
      }
      return true;
    }

    /* ===== NORMAL KILAT LOGIC ===== */
    let currentProduct = 'mobilku';
    
    function selectProduct(prod) {
      currentProduct = prod;
      document.querySelectorAll('.prod-item').forEach(c => c.classList.remove('active'));
      $('prod-' + prod).classList.add('active');
      updateNorkilUI();
    }

    function updateNorkilUI() {
      const status = getVal('nk_status');
      const kbij = getVal('nk_kbij');
      const rumah = getVal('nk_rumah');
      
      if (status === 'eksisting') $('row_rating').classList.remove('hidden');
      else $('row_rating').classList.add('hidden');

      if (rumah === 'sewa') $('row_lama_tinggal').classList.remove('hidden');
      else $('row_lama_tinggal').classList.add('hidden');

      // Reset All Special Fields
      $('row_dp_perc').classList.add('hidden');
      $('row_ltv').classList.add('hidden');
      $('row_region').classList.add('hidden');
      $('row_bpjs').classList.add('hidden');
      $('row_dukcapil').classList.add('hidden');
      $('row_kbij_pas').classList.add('hidden');
      $('row_dpd').classList.add('hidden');
      $('row_bd_norkil').classList.add('hidden');
      $('row_angs_old').classList.add('hidden');
      $('row_jenis_kend').classList.add('hidden');
      $('row_kategori_kend').classList.add('hidden');
      $('row_tipe_motor').classList.add('hidden');

      if (currentProduct === 'mobilku') {
        $('row_dukcapil').classList.remove('hidden');
        $('row_kbij_pas').classList.remove('hidden');
        $('row_jenis_kend').classList.remove('hidden');
        $('row_kategori_kend').classList.remove('hidden');
        $('row_thn_kend').classList.remove('hidden');
        $('row_bpkb').classList.remove('hidden');
        $('row_ltv').classList.remove('hidden'); // SK 186 requires LTV
      } 
      else if (currentProduct === 'motorku') {
        $('row_tipe_motor').classList.remove('hidden');
        $('row_thn_kend').classList.remove('hidden');
        $('row_bpkb').classList.remove('hidden');
        $('row_ltv').classList.remove('hidden'); // SK 167 requires LTV
        $('row_region').classList.remove('hidden'); // SK 167 requires Region for Max Angs
        
        if (status === 'eksisting' && kbij === 'no') {
          $('row_dpd').classList.remove('hidden');
          $('row_bd_norkil').classList.remove('hidden');
        }
        if (status === 'eksisting') $('row_angs_old').classList.remove('hidden');
      }
      else if (currentProduct === 'newbike_bpjs') {
        $('row_bpjs').classList.remove('hidden');
        $('row_dp_perc').classList.remove('hidden'); // SK 156 uses DP
        $('row_tipe_motor').classList.remove('hidden');
        $('row_thn_kend').classList.remove('hidden');
        $('row_bpkb').classList.remove('hidden');
      }
      else if (currentProduct === 'newbike_nonbpjs') {
        $('row_dp_perc').classList.remove('hidden'); // SK 155 uses DP
        $('row_region').classList.remove('hidden'); // SK 155 Region specific IIR
        $('row_tipe_motor').classList.remove('hidden');
        $('row_thn_kend').classList.remove('hidden');
        $('row_bpkb').classList.remove('hidden');
      }
    }

    function checkNorkil() {
      let required = ['nk_status', 'nk_lama_kerja', 'nk_income', 'nk_angs', 'nk_exp', 'nk_tenor'];
      
      if (currentProduct === 'mobilku') required.push('nk_thn_kend', 'nk_kategori', 'nk_ltv');
      else if (currentProduct === 'motorku') required.push('nk_tipe_motor', 'nk_ltv');
      else if (currentProduct === 'newbike_bpjs') required.push('nk_bpjs_status', 'nk_bpjs_income', 'nk_dp_perc');
      else if (currentProduct === 'newbike_nonbpjs') required.push('nk_dp_perc');
      
      if (getVal('nk_rumah') === 'sewa') required.push('nk_lama_tinggal');
      if (getVal('nk_status') === 'eksisting' && currentProduct === 'motorku') required.push('nk_angs_old');
      
      if (!checkEmpty(required)) return;

      let res = [];
      let status = 'pass';
      const income = parseNum(getVal('nk_income'));
      const angs = parseNum(getVal('nk_angs'));
      const exp = parseNum(getVal('nk_exp'));
      const ltv = parseNum(getVal('nk_ltv'));
      const thn = parseNum(getVal('nk_thn_kend'));
      const age = new Date().getFullYear() - thn;
      
      // Common Checks
      if (getVal('nk_internal') === 'ya') { res.push({t: 'fail', m: 'SK: Terindikasi Internal Negative List.'}); status='fail'; } else { res.push({t: 'pass', m: 'Bebas Internal Neg List'}); }
      if (getVal('nk_tbwo') === 'ya') { res.push({t: 'fail', m: 'SK: Memiliki riwayat TB/WO.'}); status='fail'; } else { res.push({t: 'pass', m: 'Bebas TB/WO'}); }

      // MobilKu Logic (SK 186)
      if (currentProduct === 'mobilku') {
        if (getVal('nk_dukcapil') !== 'match') { res.push({t: 'fail', m: 'SK 186: Dukcapil Wajib Match.'}); status='fail'; } else { res.push({t: 'pass', m: 'Dukcapil Match'}); }
        if (getVal('nk_lama_kerja') !== '1tahun') { res.push({t: 'fail', m: 'SK 186: Lama Kerja minimal 1 Tahun.'}); status='fail'; } else { res.push({t: 'pass', m: 'Lama Kerja Sesuai'}); }
        if (getVal('nk_rumah') === 'sewa') { res.push({t: 'fail', m: 'SK 186: Status sewa tidak diperbolehkan.'}); status='fail'; } else { res.push({t: 'pass', m: 'Status Rumah Sesuai'}); }
        
        const kbij = getVal('nk_kbij'); const kbijPas = getVal('nk_kbij_pas');
        if (kbij === 'yes') { res.push({t: 'pass', m: 'KBIJ Pemohon Yes'}); }
        else if (kbij === 'notfound') {
           if (kbijPas === 'yes') {
             res.push({t: 'pass', m: 'KBIJ Pemohon NF & Pasangan Yes -> Sesuai'}); 
             res.push({t: 'warn', m: 'SK 186: Wajib validasi ulang oleh CAC.'}); 
             if(status!=='fail') status='warn';
           } else {
             res.push({t: 'fail', m: 'SK 186: KBIJ Pemohon & Pasangan NF -> Tidak Dapat Diproses.'}); status='fail';
           }
        } else { res.push({t: 'fail', m: 'SK 186: KBIJ No tidak diperbolehkan Normal Kilat.'}); status='fail'; }

        if (getVal('nk_kategori') !== 'A') { res.push({t: 'fail', m: 'SK 186: Hanya Kategori A.'}); status='fail'; } else { res.push({t: 'pass', m: 'Kategori Sesuai'}); }
        if (age > 10) { res.push({t: 'fail', m: 'SK 186: Usia kendaraan > 10 tahun.'}); status='fail'; } else { res.push({t: 'pass', m: 'Usia Kendaraan Sesuai'}); }
        if (getVal('nk_bpkb') === 'oranglain') { res.push({t: 'warn', m: 'SK 186: BPKB Orang Lain WAJIB BBN.'}); if(status!=='fail')status='warn'; }

        if (exp > 175000000) { res.push({t: 'fail', m: 'SK 186: Exposure > 175 Juta.'}); status='fail'; } else { res.push({t: 'pass', m: 'Exposure Sesuai'}); }
        
        // LTV Check (SK 186)
        let maxLTV = 0;
        if (getVal('nk_bpkb') === 'sendiri') {
            maxLTV = age <= 5 ? 80 : 70;
        } else {
            maxLTV = age <= 5 ? 70 : 60;
        }
        if (ltv > maxLTV) { res.push({t: 'fail', m: 'SK 186: LTV ' + ltv + '% > Max ' + maxLTV + '%.'}); status='fail'; } else { res.push({t: 'pass', m: 'LTV Sesuai (Max ' + maxLTV + '%)'}); }

        const iir = income > 0 ? (angs / income) * 100 : 0; const maxIIR = getVal('nk_jenis_inc') === 'fixed' ? 40 : 30;
        if (iir > maxIIR) { res.push({t: 'fail', m: 'SK 186: IIR > ' + maxIIR + '%.'}); status='fail'; } else { res.push({t: 'pass', m: 'IIR Sesuai'}); }
      } 
      
      // MotorKu Logic (SK 167)
      else if (currentProduct === 'motorku') {
        const statusKons = getVal('nk_status'); const rating = getVal('nk_rating'); const kbij = getVal('nk_kbij');
        const maxExp = statusKons === 'baru' ? 25000000 : 45000000;
        if (exp > maxExp) { res.push({t: 'fail', m: 'SK 167: Exposure > limit.'}); status='fail'; } else { res.push({t: 'pass', m: 'Exposure Sesuai'}); }
        if (statusKons === 'baru' && income < 2500000) { res.push({t: 'fail', m: 'SK 167: Income min 2.5 Juta.'}); status='fail'; } else { res.push({t: 'pass', m: 'Income Sesuai'}); }
        
        // Max Angsuran Check (SK 167 Table 3) - Only for Baru
        if (statusKons === 'baru') {
            const region = getVal('nk_region');
            const tipe = getVal('nk_tipe_motor');
            
            // Map: { Region: { Standar: x, Premium: y } }
            // Values derived from SK 167 Text
            const maxAngsMap = {
                'jabodebek': { standar: 1500000, premium: 1350000 },
                'banten': { standar: 1500000, premium: 1350000 },
                'kalimantan': { standar: 1500000, premium: 1500000 },
                'sulawesi': { standar: 1500000, premium: 1350000 },
                'jatim_bnt': { standar: 1250000, premium: 1250000 }, // Assumed same as standard if not listed
                'sumbagut': { standar: 1250000, premium: 1350000 },
                'sumbagsel': { standar: 1350000, premium: 1350000 },
                'jabar': { standar: 1200000, premium: 1000000 },
                'jatengut': { standar: 1000000, premium: 1000000 },
                'jatengsel': { standar: 1000000, premium: 1000000 }
            };
            
            const limitObj = maxAngsMap[region] || { standar: 1500000, premium: 1500000 };
            const maxAngs = tipe === 'premium' ? limitObj.premium : limitObj.standar;
            
            if (angs > maxAngs) { res.push({t: 'fail', m: 'SK 167: Angsuran > ' + formatCurr(maxAngs) + ' (' + tipe + ' ' + region + ').'}); status='fail'; } 
            else { res.push({t: 'pass', m: 'Angsuran Sesuai Region & Tipe'}); }
        }

        // LTV Check (SK 167 Table 4b)
        let maxLTV_Motor = 85;
        if (statusKons === 'eksisting' && (rating === 'excellent' || rating === 'good')) {
            maxLTV_Motor = age <= 10 ? 90 : 85;
        } else {
            maxLTV_Motor = age <= 10 ? 85 : 80;
        }
        if (ltv > maxLTV_Motor) { res.push({t: 'fail', m: 'SK 167: LTV ' + ltv + '% > Max ' + maxLTV_Motor + '%.'}); status='fail'; } else { res.push({t: 'pass', m: 'LTV Sesuai (Max ' + maxLTV_Motor + '%)'}); }

        if (kbij === 'no') {
          if (statusKons === 'eksisting' && (rating === 'excellent' || rating === 'good')) {
             const dpd = parseNum(getVal('nk_dpd')); const bd = parseNum(getVal('nk_bd'));
             if (bd > 30000000 && dpd > 60) { res.push({t: 'pass', m: 'SK 167: KBIJ No Exc/Good (BD>30jt, DPD>60) OK'}); }
             else { res.push({t: 'fail', m: 'SK 167: KBIJ No Exc/Good memerlukan BD>30jt & DPD>60'}); status='fail'; }
          } else { res.push({t: 'warn', m: 'SK 167: KBIJ No Normal/Baru -> Perhatikan LTV 70%'}); if(status!=='fail')status='warn'; }
        } else { res.push({t: 'pass', m: 'KBIJ Sesuai'}); }

        if (statusKons === 'eksisting') {
          const oldAngs = parseNum(getVal('nk_angs_old'));
          if (oldAngs > 0 && angs > (oldAngs * 1.5)) { res.push({t: 'fail', m: 'SK 167: Angsuran baru > 1.5x lama.'}); status='fail'; } 
          else if (oldAngs > 0) { res.push({t: 'pass', m: 'Rasio Angsuran Sesuai'}); }
        }
        
        // Rumah Logic
        if (getVal('nk_rumah') === 'sewa') {
          if (statusKons === 'baru' || (statusKons === 'eksisting' && rating === 'normal')) { res.push({t: 'fail', m: 'SK 167: Tidak diperbolehkan sewa.'}); status='fail'; }
          else { res.push({t: 'pass', m: 'Status Rumah Sesuai (Exc/Good)'}); }
        } else { res.push({t: 'pass', m: 'Status Rumah Sesuai'}); }
      } 
      
      // NewBike BPJS (SK 156)
      else if (currentProduct === 'newbike_bpjs') {
        if (getVal('nk_bpjs_status') !== 'terdaftar') { res.push({t: 'fail', m: 'SK 156: Wajib terdaftar BPJS.'}); status='fail'; } else { res.push({t: 'pass', m: 'BPJS Terdaftar'}); }
        if (parseNum(getVal('nk_bpjs_income')) === 0) { res.push({t: 'fail', m: 'SK 156: Penghasilan JMO wajib diisi.'}); status='fail'; }
        if (getVal('nk_lama_kerja') !== '6bulan' && getVal('nk_lama_kerja') !== '1tahun') { res.push({t: 'fail', m: 'SK 156: Lama kerja min 6 bln.'}); status='fail'; } else { res.push({t: 'pass', m: 'Lama Kerja Sesuai'}); }
        if (exp > 45000000) { res.push({t: 'fail', m: 'SK 156: Max Exposure 45 Juta.'}); status='fail'; } else { res.push({t: 'pass', m: 'Exposure Sesuai'}); }
        
        const dpPerc = parseNum(getVal('nk_dp_perc'));
        if (getVal('nk_rumah') === 'sewa') {
           const lamaTinggal = parseNum(getVal('nk_lama_tinggal'));
           if (lamaTinggal < 12) { res.push({t: 'fail', m: 'SK 156: Sewa min 12 bln tinggal.'}); status='fail'; }
           else if (dpPerc < 15) { res.push({t: 'fail', m: 'SK 156: Sewa -> Min DP 15%.'}); status='fail'; }
           else { res.push({t: 'pass', m: 'Status Rumah & DP Sesuai'}); }
        } else {
          if (dpPerc < 5) { res.push({t: 'fail', m: 'SK 156: Min DP 5%.'}); status='fail'; } else { res.push({t: 'pass', m: 'DP Sesuai (Min 5%)'}); }
        }
        const iir = income > 0 ? (angs / income) * 100 : 0;
        if (iir > 45) { res.push({t: 'fail', m: 'SK 156: IIR > 45%.'}); status='fail'; } else { res.push({t: 'pass', m: 'IIR Sesuai'}); }
      } 
      
      // NewBike Non-BPJS (SK 155)
      else if (currentProduct === 'newbike_nonbpjs') {
        const rating = getVal('nk_rating'); const statusKons = getVal('nk_status');
        if (exp > 45000000) { res.push({t: 'fail', m: 'SK 155: Max Exposure 45 Juta.'}); status='fail'; } else { res.push({t: 'pass', m: 'Exposure Sesuai'}); }
        
        const dpPerc = parseNum(getVal('nk_dp_perc'));
        if (getVal('nk_rumah') === 'sewa') {
           const lamaTinggal = parseNum(getVal('nk_lama_tinggal'));
           if (lamaTinggal < 12) { res.push({t: 'fail', m: 'SK 155: Sewa min 12 bln tinggal.'}); status='fail'; }
           else if (dpPerc < 15) { res.push({t: 'fail', m: 'SK 155: Sewa -> Min DP 15%.'}); status='fail'; }
           else { res.push({t: 'pass', m: 'Status Rumah & DP Sesuai'}); }
        } else {
          if (statusKons === 'eksisting' && (rating === 'excellent' || rating === 'good')) {
            if (dpPerc < 8) { res.push({t: 'fail', m: 'SK 155: Exc/Good -> Min DP 8%.'}); status='fail'; } else { res.push({t: 'pass', m: 'DP Sesuai (Min 8%)'}); }
          } else { res.push({t: 'pass', m: 'DP Sesuai (Standar)'}); }
        }
        
        let maxIIR = getVal('nk_jenis_inc') === 'fixed' ? 30 : 25;
        const region = getVal('nk_region');
        if (region === 'jatengsel' || region === 'jatengut' || region === 'jabar') { maxIIR = getVal('nk_jenis_inc') === 'fixed' ? 35 : 30; }
        const iir = income > 0 ? (angs / income) * 100 : 0;
        if (iir > maxIIR) { res.push({t: 'fail', m: 'SK 155: IIR > ' + maxIIR + '%.'}); status='fail'; } else { res.push({t: 'pass', m: 'IIR Sesuai'}); }
      }

      renderResult('res-norkil', res, status);
    }

    /* ===== DEVIASI LOGIC ===== */
    function updateDeviasiUI() {
      const status = getVal('dev_status');
      if (status === 'eksisting') $('dev_row_rating').classList.remove('hidden');
      else $('dev_row_rating').classList.add('hidden');
    }

    function checkDeviasi() {
      if (!checkEmpty(['dev_income', 'dev_angs', 'dev_usia', 'dev_ltv', 'dev_bd', 'dev_exp', 'dev_usia_kend'])) return;
      let res = []; let highestApp = 'CAC'; let hasReject = false;
      const kol = parseInt(getVal('dev_kol')); const bd = parseNum(getVal('dev_bd')); const status = getVal('dev_status'); const rating = getVal('dev_rating'); const prod = getVal('dev_prod');

      if (kol > 1 || bd > 0) {
        let devHandled = false;
        if (status === 'eksisting' && (rating === 'excellent' || rating === 'good')) {
          if (kol >= 2 && kol <= 5) { res.push({t: 'warn', m: 'Matriks: Kol ' + kol + ' Exc/Good -> CAC'}); devHandled = true; }
        }
        if (!devHandled) {
          if (kol === 5) {
            if (bd > 50000000) { res.push({t: 'fail', m: 'Kol 5 BD > 50jt -> CRO'}); highestApp = 'CRO'; }
            else if (bd > 30000000) { res.push({t: 'fail', m: 'Kol 5 BD 30-50jt -> CADH'}); if(highestApp!=='CRO')highestApp='CADH'; }
            else if (bd > 5000000) { res.push({t: 'fail', m: 'Kol 5 BD 5-30jt -> CDDH'}); if(highestApp!=='CRO' && highestApp!=='CADH')highestApp='CDDH'; }
            else if (bd > 1000000) { res.push({t: 'warn', m: 'Kol 5 BD 1-5jt -> CDH'}); if(highestApp==='CAC')highestApp='CDH'; }
            else { res.push({t: 'warn', m: 'Kol 5 BD <= 1jt -> CAM'}); if(highestApp==='CAC')highestApp='CAM'; }
          } else if (kol === 4) {
             if (bd > 50000000) { res.push({t: 'fail', m: 'Kol 4 BD > 50jt -> CADH'}); if(highestApp!=='CRO')highestApp='CADH'; }
             else { res.push({t: 'fail', m: 'Kol 4 BD <= 50jt -> CDDH'}); if(highestApp!=='CRO' && highestApp!=='CADH')highestApp='CDDH'; }
          } else if (kol === 3) {
             if (bd > 50000000) { res.push({t: 'fail', m: 'Kol 3 BD > 50jt -> CDDH'}); if(highestApp!=='CRO' && highestApp!=='CADH')highestApp='CDDH'; }
             else if (bd > 5000000) { res.push({t: 'warn', m: 'Kol 3 BD 5-50jt -> CAM'}); if(highestApp==='CAC')highestApp='CAM'; }
             else { res.push({t: 'warn', m: 'Kol 3 BD <= 5jt -> CAC'}); }
          } else if (kol === 2) {
             if (bd > 50000000) { res.push({t: 'warn', m: 'Kol 2 BD > 50jt -> CDH'}); if(highestApp==='CAC')highestApp='CDH'; }
             else if (bd > 5000000) { res.push({t: 'warn', m: 'Kol 2 BD 5-50jt -> CAM'}); if(highestApp==='CAC')highestApp='CAM'; }
             else { res.push({t: 'warn', m: 'Kol 2 BD <= 5jt -> CAC'}); }
          }
        }
      } else { res.push({t: 'pass', m: 'BI Checking Normal'}); }

      const income = parseNum(getVal('dev_income')); const angs = parseNum(getVal('dev_angs')); const dsr = income > 0 ? (angs / income) * 100 : 0;
      if (dsr > 80) { res.push({t: 'fail', m: 'DSR > 80% -> CDH'}); if(highestApp!=='CRO' && highestApp!=='CADH')highestApp='CDH'; }
      else if (dsr > 65) { res.push({t: 'warn', m: 'DSR > 65% -> CAM'}); if(highestApp==='CAC')highestApp='CAM'; }
      else if (dsr > 55) { 
        if(prod === 'motor') { res.push({t: 'warn', m: 'Motor DSR > 55% -> CAC'}); } 
        else { res.push({t: 'warn', m: 'Mobil/NB DSR > 55% -> CAM'}); if(highestApp==='CAC')highestApp='CAM'; }
      } else { res.push({t: 'pass', m: 'DSR Normal'}); }

      const usia = parseNum(getVal('dev_usia'));
      if (usia > 70) { res.push({t: 'fail', m: 'Usia > 70 Thn -> Reject'}); hasReject = true; }
      else if (usia > 60) { res.push({t: 'warn', m: 'Usia > 60 Thn -> CAC'}); }
      else { res.push({t: 'pass', m: 'Usia Normal'}); }

      const ltv = parseNum(getVal('dev_ltv'));
      if (prod === 'motor') {
        if (ltv > 95) { res.push({t: 'fail', m: 'LTV Motor > 95% -> Reject'}); hasReject = true; }
        else if (ltv > 85) { res.push({t: 'warn', m: 'LTV Motor > 85% -> CAM'}); if(highestApp==='CAC')highestApp='CAM'; }
        else { res.push({t: 'pass', m: 'LTV Normal'}); }
      } else {
        if (ltv > 90) { res.push({t: 'fail', m: 'LTV Mobil > 90% -> Reject'}); hasReject = true; }
        else if (ltv > 85) { res.push({t: 'warn', m: 'LTV Mobil > 85% -> CAM'}); if(highestApp==='CAC')highestApp='CAM'; }
        else { res.push({t: 'pass', m: 'LTV Normal'}); }
      }

      const tenor = parseNum(getVal('dev_tenor'));
      if (tenor > 48) { res.push({t: 'warn', m: 'Tenor > 48 -> CAC'}); }
      else if (prod === 'motor' && tenor > 36) { res.push({t: 'warn', m: 'Tenor Motor > 36 -> CAC'}); }
      else { res.push({t: 'pass', m: 'Tenor Normal'}); }

      const usiaKend = parseNum(getVal('dev_usia_kend'));
      if (usiaKend > 10) { res.push({t: 'warn', m: 'Usia Kendaraan > 10 thn -> CAM'}); if(highestApp==='CAC')highestApp='CAM'; }
      else { res.push({t: 'pass', m: 'Usia Kendaraan Normal'}); }

      if (hasReject) { res.push({t: 'fail', m: '<strong>STATUS: REJECT</strong>'}); renderResult('res-deviasi', res, 'fail'); }
      else { res.push({t: 'warn', m: '<strong>Approval Final: ' + highestApp + '</strong>'}); renderResult('res-deviasi', res, highestApp === 'CAC' ? 'pass' : 'warn'); }
    }

    /* ===== SUPER LOGIC ===== */
    let superType = 'ro';
    function setSuperType(t) {
      superType = t;
      if (t === 'ro') { $('super-ro').className = "px-4 py-1.5 text-xs font-bold rounded bg-white shadow text-indigo-600"; $('super-to').className = "px-4 py-1.5 text-xs font-bold rounded text-slate-500"; $('row_to_specific').classList.add('hidden'); }
      else { $('super-to').className = "px-4 py-1.5 text-xs font-bold rounded bg-white shadow text-indigo-600"; $('super-ro').className = "px-4 py-1.5 text-xs font-bold rounded text-slate-500"; $('row_to_specific').classList.remove('hidden'); }
    }

    function checkSuper() {
      let required = ['sp_usia', 'sp_income', 'sp_angs', 'sp_exp', 'sp_thn'];
      if (superType === 'to') required.push('sp_to_bayar');
      if (!checkEmpty(required)) return;
      let res = []; let status = 'pass'; let hasDeviasi = false;

      const rating = getVal('sp_rating');
      if (rating === 'warning' || rating === 'bad') { res.push({t: 'fail', m: 'SK Super: Rating Warning/Bad tidak dibiayai.'}); status='fail'; } else { res.push({t: 'pass', m: 'Rating Sesuai'}); }
      if (getVal('sp_internal') === 'ya') { res.push({t: 'fail', m: 'SK Super: Internal Negative List.'}); status='fail'; } else { res.push({t: 'pass', m: 'Bebas Internal Neg List'}); }
      if (getVal('sp_tbwo') === 'ya') { res.push({t: 'fail', m: 'SK Super: Riwayat TB/WO.'}); status='fail'; } else { res.push({t: 'pass', m: 'Bebas TB/WO'}); }
      if (getVal('sp_kbij') === 'no') { res.push({t: 'fail', m: 'SK Super: KBIJ No tidak diperbolehkan.'}); status='fail'; } else { res.push({t: 'pass', m: 'KBIJ Sesuai'}); }
      
      const usia = parseNum(getVal('sp_usia'));
      if (usia > 60) { res.push({t: 'fail', m: 'SK Super: Usia max 60 Thn.'}); status='fail'; } else { res.push({t: 'pass', m: 'Usia Sesuai'}); }
      if (getVal('sp_rumah') === 'sewa') { res.push({t: 'fail', m: 'SK Super: Tidak boleh sewa.'}); status='fail'; } else { res.push({t: 'pass', m: 'Status Rumah Sesuai'}); }
      if (parseNum(getVal('sp_exp')) > 250000000) { res.push({t: 'fail', m: 'SK Super: Max Exposure 250 Jt.'}); status='fail'; } else { res.push({t: 'pass', m: 'Exposure Sesuai'}); }
      
      const income = parseNum(getVal('sp_income')); const angs = parseNum(getVal('sp_angs')); const iir = income > 0 ? (angs / income) * 100 : 0; const maxIIR = superType === 'ro' ? 40 : 30;
      if (iir > maxIIR) { res.push({t: 'fail', m: 'SK Super: IIR > ' + maxIIR + '%.'}); status='fail'; } else { res.push({t: 'pass', m: 'IIR Sesuai'}); }

      const thn = parseNum(getVal('sp_thn')); const age = new Date().getFullYear() - thn; const jenis = getVal('sp_jenis');
      let maxAge = (superType === 'ro' && jenis === 'passenger') ? 17 : 10;
      if (age > maxAge) { res.push({t: 'fail', m: 'SK Super: Usia kendaraan > ' + maxAge + ' thn.'}); status='fail'; } else { res.push({t: 'pass', m: 'Usia Kendaraan Sesuai'}); }

      if (parseNum(getVal('sp_tenor')) > 48) { res.push({t: 'warn', m: 'SK Super: Tenor > 48 = Deviasi.'}); if(status!=='fail')status='warn'; hasDeviasi=true; } else { res.push({t: 'pass', m: 'Tenor Sesuai'}); }

      if (superType === 'to') {
        const bayar = parseNum(getVal('sp_to_bayar'));
        if (bayar < 12) { res.push({t: 'fail', m: 'SK Super TO: Min 12 bln pembayaran.'}); status='fail'; } else { res.push({t: 'pass', m: 'Riwayat Pembayaran Sesuai'}); }
        if (getVal('sp_to_kondisi') !== 'current') { res.push({t: 'warn', m: 'SK Super TO: Kondisi tidak current -> Deviasi.'}); if(status!=='fail')status='warn'; hasDeviasi=true; }
        const exp = parseNum(getVal('sp_exp')); let appLevel = exp > 75000000 ? 'CMG' : 'CAC';
        res.push({t: 'warn', m: '<strong>Approval Level: ' + appLevel + '</strong>'});
      }

      renderResult('res-super', res, status);
    }

    /* ===== RENDER HELPER ===== */
    function renderResult(containerId, items, globalStatus) {
      const container = $(containerId); let html = '';
      let statusClass = 'status-warn'; let statusText = 'Layak dengan Catatan'; let icon = '!';
      if (globalStatus === 'pass') { statusClass = 'status-pass'; statusText = 'Layak Kredit'; icon = '✓'; }
      if (globalStatus === 'fail') { statusClass = 'status-fail'; statusText = 'Tidak Layak'; icon = '✗'; }
      html += '<div class="status-badge ' + statusClass + ' mb-4 justify-center text-sm">' + icon + ' ' + statusText + '</div>';
      html += '<div class="space-y-1">';
      items.forEach(item => {
        const i = item.t === 'pass' ? '<span class="text-green-500 font-bold">✓</span>' : (item.t === 'fail' ? '<span class="text-red-500 font-bold">✗</span>' : '<span class="text-amber-500 font-bold">!</span>');
        html += '<div class="result-item"><div class="w-5 flex-shrink-0">' + i + '</div><span class="text-slate-600 text-xs">' + item.m + '</span></div>';
      });
      html += '</div>';
      container.innerHTML = html;
    }

    // Initialize
    updateNorkilUI();
    updateDeviasiUI();
  </script>
</body>
</html>
