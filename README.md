<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Sistem Barang Masuk | Inventory + Total Barang Page</title>
    <!-- SheetJS library untuk export Excel -->
    <script src="https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
            background: #eef2f7;
            padding: 20px;
            min-height: 100vh;
        }

        /* Container Utama */
        .app-container {
            max-width: 1400px;
            margin: 0 auto;
        }

        /* Header */
        .main-header {
            background: linear-gradient(135deg, #0f2b3d 0%, #1a3a4f 100%);
            color: white;
            padding: 20px 28px;
            border-radius: 24px 24px 0 0;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .main-header h1 {
            font-size: 1.7rem;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .main-header h1::before {
            content: "📦";
            font-size: 1.8rem;
        }

        .main-header p {
            font-size: 0.85rem;
            opacity: 0.85;
            margin-top: 6px;
        }

        /* Tab Navigation */
        .tab-nav {
            display: flex;
            gap: 8px;
            margin: 20px 0 0 0;
            background: white;
            border-radius: 40px;
            padding: 6px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.05);
        }
        .tab-btn {
            flex: 1;
            background: transparent;
            border: none;
            padding: 12px 20px;
            border-radius: 40px;
            font-weight: 700;
            font-size: 0.9rem;
            cursor: pointer;
            transition: 0.2s;
            color: #4b5563;
        }
        .tab-btn.active {
            background: #0f2b3d;
            color: white;
            box-shadow: 0 2px 8px rgba(15,43,61,0.2);
        }
        .tab-btn:not(.active):hover {
            background: #e2e8f0;
        }

        /* Kartu Form */
        .form-card {
            background: white;
            padding: 24px 28px;
            border-radius: 0 0 20px 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.08);
            margin-bottom: 28px;
        }

        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
            gap: 18px;
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .input-group label {
            font-weight: 600;
            font-size: 0.8rem;
            color: #1e293b;
            letter-spacing: 0.3px;
        }

        .input-group input,
        .input-group select {
            padding: 10px 14px;
            border: 1.5px solid #e2e8f0;
            border-radius: 14px;
            font-size: 0.9rem;
            transition: all 0.2s;
            background: #fafcff;
        }

        .input-group input:focus,
        .input-group select:focus {
            outline: none;
            border-color: #2c6e9e;
            box-shadow: 0 0 0 3px rgba(44,110,158,0.15);
        }

        .btn-group {
            display: flex;
            gap: 12px;
            margin-top: 22px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 10px 22px;
            font-weight: 600;
            border: none;
            border-radius: 40px;
            cursor: pointer;
            font-size: 0.85rem;
            transition: all 0.2s;
        }

        .btn-primary {
            background: #0f2b3d;
            color: white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .btn-primary:hover {
            background: #1a4a65;
            transform: translateY(-1px);
        }

        .btn-secondary {
            background: #f1f5f9;
            color: #334155;
            border: 1px solid #cbd5e1;
        }

        .btn-secondary:hover {
            background: #e2e8f0;
        }

        .btn-edit-mode {
            background: #eab308;
            color: #1e293b;
            border: none;
        }
        .btn-edit-mode:hover {
            background: #ca8a04;
            color: white;
        }

        /* Section Data */
        .data-section {
            background: white;
            border-radius: 24px;
            padding: 20px 24px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.08);
            margin-bottom: 20px;
        }

        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 16px;
            margin-bottom: 20px;
            padding-bottom: 12px;
            border-bottom: 2px solid #eef2f9;
        }

        .title-badge {
            display: flex;
            align-items: baseline;
            gap: 12px;
        }

        .title-badge h2 {
            font-size: 1.3rem;
            color: #0f2b3d;
        }

        .badge {
            background: #e2e8f0;
            padding: 4px 12px;
            border-radius: 30px;
            font-size: 0.7rem;
            font-weight: 700;
            color: #1e293b;
        }

        .aksi-buttons {
            display: flex;
            gap: 10px;
        }

        .small-icon-btn {
            background: #f8fafc;
            border: 1px solid #cbd5e1;
            padding: 6px 14px;
            border-radius: 30px;
            font-size: 0.75rem;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
        }

        .small-icon-btn:hover {
            background: #eef2ff;
        }

        /* Filter Panel (hanya untuk tab riwayat) */
        .filter-panel {
            background: #f9fbfd;
            padding: 16px 20px;
            border-radius: 20px;
            margin-bottom: 24px;
            display: flex;
            flex-wrap: wrap;
            gap: 14px;
            align-items: flex-end;
            border: 1px solid #eef2f8;
        }

        .filter-item {
            flex: 1;
            min-width: 160px;
        }

        .filter-item label {
            font-size: 0.7rem;
            font-weight: 600;
            color: #4b5563;
            display: block;
            margin-bottom: 4px;
        }

        .filter-item input,
        .filter-item select {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #d1d9e8;
            border-radius: 30px;
            font-size: 0.8rem;
        }

        .btn-reset-filter {
            background: white;
            border: 1px solid #cbd5e1;
            padding: 8px 20px;
            border-radius: 30px;
            font-weight: 500;
            cursor: pointer;
            font-size: 0.75rem;
        }

        /* Tabel */
        .table-wrapper {
            overflow-x: auto;
            border-radius: 18px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.8rem;
        }

        th {
            text-align: left;
            padding: 12px 10px;
            background: #f1f5f9;
            color: #1e293b;
            font-weight: 700;
            border-bottom: 2px solid #e2e8f0;
        }

        td {
            padding: 11px 10px;
            border-bottom: 1px solid #edf2f7;
            vertical-align: middle;
            color: #1f2a44;
        }

        .action-buttons {
            display: flex;
            gap: 6px;
            align-items: center;
        }
        .edit-btn {
            background: none;
            border: none;
            font-size: 1.1rem;
            cursor: pointer;
            color: #0f3b5c;
            padding: 4px 8px;
            border-radius: 30px;
            transition: all 0.1s;
        }
        .edit-btn:hover {
            background: #e6f0fa;
            transform: scale(1.02);
        }
        .delete-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: #b91c1c;
            padding: 4px 8px;
            border-radius: 30px;
            transition: all 0.1s;
        }
        .delete-btn:hover {
            background: #fee2e2;
            transform: scale(1.03);
        }

        .empty-row td {
            text-align: center;
            padding: 40px;
            color: #94a3b8;
            font-style: italic;
        }

        .info-search {
            font-size: 0.7rem;
            background: #eef2ff;
            padding: 5px 12px;
            border-radius: 30px;
            display: inline-block;
            margin-bottom: 12px;
        }

        /* Kartu Total Summary */
        .total-summary-card {
            background: #f0f9ff;
            border-radius: 20px;
            padding: 16px 20px;
            margin-bottom: 24px;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            gap: 12px;
            border: 1px solid #cbd5e1;
        }
        .summary-item {
            background: white;
            border-radius: 20px;
            padding: 8px 20px;
            min-width: 150px;
            text-align: center;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }
        .summary-item span:first-child {
            font-size: 0.7rem;
            color: #4b5563;
            display: block;
        }
        .summary-item span:last-child {
            font-size: 1.6rem;
            font-weight: 800;
            color: #0f2b3d;
        }

        /* Notifikasi */
        .toast-msg {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #0f2b3d;
            color: white;
            padding: 10px 18px;
            border-radius: 40px;
            font-size: 0.8rem;
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.3s;
            pointer-events: none;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
        }
        footer {
            margin-top: 20px;
            text-align: center;
            font-size: 0.7rem;
            color: #5b6e8c;
        }
        .hidden-tab {
            display: none;
        }
        @media (max-width: 640px) {
            body { padding: 12px; }
            .form-card { padding: 18px; }
            .btn-group .btn { flex: 1; text-align: center; }
            .filter-panel { flex-direction: column; align-items: stretch; }
            .section-header { flex-direction: column; align-items: flex-start; }
            .tab-btn { font-size: 0.7rem; padding: 8px 12px; }
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="main-header">
        <h1>Sistem Pencatatan Barang Masuk</h1>
        <p>Kelola penerimaan barang (Agregasi)</p>
    </div>

    <!-- TAB NAVIGATION -->
    <div class="tab-nav">
        <button class="tab-btn active" id="tabRiwayatBtn">📋 Riwayat Barang Masuk</button>
        <button class="tab-btn" id="tabTotalBarangBtn">📊 Total Semua Barang</button>
    </div>

    <!-- TAB 1 : RIWAYAT BARANG MASUK (dengan form input) -->
    <div id="tabRiwayat" class="tab-content">
        <div class="form-card">
            <form id="barangForm">
                <div class="form-grid">
                    <div class="input-group"><label>🏭 Nama Supplier *</label><input type="text" id="supplier" placeholder="Contoh: PT, CV" required></div>
                    <div class="input-group"><label>📦 Nama Barang *</label><input type="text" id="namaBarang" placeholder="Nama produk / material" required></div>
                    <div class="input-group"><label>🏷️ Kategori *</label>
                        <select id="kategori" required>
                            <option value="" disabled selected>-- Pilih --</option>
                            <option>Booklet 1 1/4</option><option>Booklet 1 1/4 tips</option><option>Booklet Kss</option>
                            <option>Booklet Kss tips</option><option>Display box</option><option>Filter tips 21</option>
                            <option>Filter tips 26</option><option>Filter tips 30</option><option>Trapezoid</option>
                            <option>Sticker</option><option>Lainnya</option>
                        </select>
                    </div>
                    <div class="input-group"><label>🔢 Jumlah *</label><input type="number" id="jumlah" placeholder="0" min="1" required></div>
                    <div class="input-group"><label>📏 Satuan *</label><input type="text" id="unit" placeholder="pcs, box, kg, roll" required></div>
                    <div class="input-group"><label>📅 Tanggal Masuk *</label><input type="date" id="tanggalMasuk" required></div>
                    <div class="input-group"><label>📝 Catatan / No. PO</label><input type="text" id="catatan" placeholder="Opsional: PO-123 / INV-xxx"></div>
                </div>
                <div class="btn-group">
                    <button type="submit" class="btn btn-primary" id="submitBtn">Tambah Barang</button>
                    <button type="button" id="resetFormBtn" class="btn btn-secondary">🗑️ Reset Form</button>
                    <button type="button" id="cancelEditBtn" class="btn btn-secondary" style="display:none;">✖️ Batalkan Edit</button>
                </div>
            </form>
        </div>

        <div class="data-section">
            <div class="section-header">
                <div class="title-badge"><h2>📋 Riwayat Barang Masuk</h2><span class="badge" id="totalDataCount">0 item</span></div>
                <div class="aksi-buttons"><button id="printBtn" class="small-icon-btn">🖨️ Cetak</button><button id="exportExcelBtn" class="small-icon-btn">📎 Export Excel</button></div>
            </div>
            <div class="filter-panel">
                <div class="filter-item"><label>🔍 Cari Nama Barang</label><input type="text" id="filterNama" placeholder="Ketik nama barang..."></div>
                <div class="filter-item"><label>🏭 Cari Supplier</label><input type="text" id="filterSupplier" placeholder="Nama supplier..."></div>
                <div class="filter-item"><label>📂 Filter Kategori</label><select id="filterKategori"><option value="">Semua Kategori</option><option>Booklet 1 1/4</option><option>Booklet 1 1/4 tips</option><option>Booklet Kss</option><option>Booklet Kss tips</option><option>Display box</option><option>Filter tips 21</option><option>Filter tips 26</option><option>Filter tips 30</option><option>Trapezoid</option><option>Sticker</option><option>Lainnya</option></select></div>
                <button id="clearFilterBtn" class="btn-reset-filter">Reset Filter</button>
            </div>
            <div id="infoFilter" class="info-search"></div>
            <div class="table-wrapper"><table id="mainTable"><thead><tr><th>Supplier</th><th>Nama Barang</th><th>Kategori</th><th>Jumlah</th><th>Unit</th><th>Tanggal Masuk</th><th>Catatan</th><th>Aksi</th></tr></thead><tbody id="tableBody"><tr class="empty-row"><td colspan="8">⚡ Belum ada data. Silakan tambah barang masuk.</td></tr></tbody></table></div>
        </div>
    </div>

    <!-- TAB 2 : TOTAL SEMUA BARANG (HALAMAN RINGKASAN) -->
    <div id="tabTotalBarang" class="tab-content hidden-tab">
        <div class="data-section">
            <div class="section-header">
                <div class="title-badge"><h2>📊 Total Semua Barang (Agregasi)</h2><span class="badge" id="totalAggregateCount">0 item unik</span></div>
                <div class="aksi-buttons"><button id="exportTotalExcelBtn" class="small-icon-btn">📎 Export Total ke Excel</button></div>
            </div>
            <div class="total-summary-card" id="summaryStats">
                <div class="summary-item"><span>Total Transaksi</span><span id="totalTransaksi">0</span></div>
                <div class="summary-item"><span>Total Jumlah Barang</span><span id="grandTotalJumlah">0</span></div>
                <div class="summary-item"><span>Unik Nama Barang</span><span id="uniqueItemsCount">0</span></div>
            </div>
            <div class="filter-panel" style="margin-top:0;">
                <div class="filter-item"><label>🔍 Filter Nama Barang</label><input type="text" id="filterTotalNama" placeholder="Cari nama barang..."></div>
                <div class="filter-item"><label>🏷️ Filter Kategori</label><select id="filterTotalKategori"><option value="">Semua Kategori</option><option>Booklet 1 1/4</option><option>Booklet 1 1/4 tips</option><option>Booklet Kss</option><option>Booklet Kss tips</option><option>Display box</option><option>Filter tips 21</option><option>Filter tips 26</option><option>Filter tips 30</option><option>Trapezoid</option><option>Sticker</option><option>Lainnya</option></select></div>
                <button id="resetTotalFilterBtn" class="btn-reset-filter">Reset Filter</button>
            </div>
            <div class="table-wrapper">
                <table id="totalTable">
                    <thead><tr><th>Nama Barang</th><th>Kategori</th><th>Total Jumlah</th><th>Satuan (umum)</th><th>Jumlah Supplier Terlibat</th><th>Terakhir Masuk</th></tr></thead>
                    <tbody id="totalTableBody"><tr class="empty-row"><td colspan="6">Belum ada data barang.</td></tr></tbody>
                </table>
            </div>
        </div>
    </div>
    <footer>📌 Data tersimpan lokal. Tab Total Barang mengakumulasi jumlah per nama barang + kategori.</footer>
    <div id="toastMessage" class="toast-msg"></div>
</div>

<script>
    // ======================= DATA GLOBAL ========================
    let inventory = [];           
    let filteredInventory = [];   
    let editMode = false;
    let editingId = null;

    // DOM Elemen
    const form = document.getElementById('barangForm');
    const supplierInput = document.getElementById('supplier');
    const namaBarangInput = document.getElementById('namaBarang');
    const kategoriSelect = document.getElementById('kategori');
    const jumlahInput = document.getElementById('jumlah');
    const unitInput = document.getElementById('unit');
    const tglMasukInput = document.getElementById('tanggalMasuk');
    const catatanInput = document.getElementById('catatan');
    const resetFormBtn = document.getElementById('resetFormBtn');
    const cancelEditBtn = document.getElementById('cancelEditBtn');
    const submitBtn = document.getElementById('submitBtn');
    const tableBody = document.getElementById('tableBody');
    const totalDataCount = document.getElementById('totalDataCount');
    const printBtn = document.getElementById('printBtn');
    const exportExcelBtn = document.getElementById('exportExcelBtn');
    const filterNama = document.getElementById('filterNama');
    const filterSupplier = document.getElementById('filterSupplier');
    const filterKategori = document.getElementById('filterKategori');
    const clearFilterBtn = document.getElementById('clearFilterBtn');
    const infoFilter = document.getElementById('infoFilter');
    const toastMsgDiv = document.getElementById('toastMessage');

    // Tab elements
    const tabRiwayatBtn = document.getElementById('tabRiwayatBtn');
    const tabTotalBarangBtn = document.getElementById('tabTotalBarangBtn');
    const tabRiwayatDiv = document.getElementById('tabRiwayat');
    const tabTotalDiv = document.getElementById('tabTotalBarang');
    
    // Total page elements
    const totalTableBody = document.getElementById('totalTableBody');
    const filterTotalNama = document.getElementById('filterTotalNama');
    const filterTotalKategori = document.getElementById('filterTotalKategori');
    const resetTotalFilterBtn = document.getElementById('resetTotalFilterBtn');
    const exportTotalExcelBtn = document.getElementById('exportTotalExcelBtn');
    const totalAggregateCount = document.getElementById('totalAggregateCount');
    const totalTransaksiSpan = document.getElementById('totalTransaksi');
    const grandTotalJumlahSpan = document.getElementById('grandTotalJumlah');
    const uniqueItemsCountSpan = document.getElementById('uniqueItemsCount');

    // Helper
    function showToast(message, isError = false) {
        toastMsgDiv.textContent = message;
        toastMsgDiv.style.backgroundColor = isError ? '#b91c1c' : '#0f2b3d';
        toastMsgDiv.style.opacity = '1';
        setTimeout(() => { toastMsgDiv.style.opacity = '0'; }, 2000);
    }

    function setDefaultDate() {
        if (!tglMasukInput.value) {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            tglMasukInput.value = `${yyyy}-${mm}-${dd}`;
        }
    }

    function loadData() {
        const stored = localStorage.getItem('inventory_barang_masuk');
        if (stored) {
            try {
                inventory = JSON.parse(stored);
                if (!Array.isArray(inventory)) inventory = [];
            } catch(e) { inventory = []; }
        } else { inventory = []; }
        applyFilters();
        renderTotalPage(); // refresh total page
    }

    function saveData() {
        localStorage.setItem('inventory_barang_masuk', JSON.stringify(inventory));
        renderTotalPage();
    }

    function formatDate(dateStr) {
        if (!dateStr) return '-';
        const parts = dateStr.split('-');
        if (parts.length !== 3) return dateStr;
        return `${parts[2]}/${parts[1]}/${parts[0]}`;
    }
    function formatDateForExcel(dateStr) {
        if (!dateStr) return '';
        const parts = dateStr.split('-');
        return parts.length === 3 ? `${parts[2]}/${parts[1]}/${parts[0]}` : dateStr;
    }
    function escapeHtml(str) { if(!str) return ''; return str.replace(/[&<>]/g, function(m) { if(m==='&') return '&amp;'; if(m==='<') return '&lt;'; if(m==='>') return '&gt;'; return m;}); }

    // Filter & Render untuk tab Riwayat
    function applyFilters() {
        const keywordNama = filterNama.value.trim().toLowerCase();
        const keywordSupplier = filterSupplier.value.trim().toLowerCase();
        const kategoriValue = filterKategori.value;
        filteredInventory = inventory.filter(item => {
            let match = true;
            if (keywordNama && !item.namaBarang.toLowerCase().includes(keywordNama)) match = false;
            if (keywordSupplier && !item.supplier.toLowerCase().includes(keywordSupplier)) match = false;
            if (kategoriValue && item.kategori !== kategoriValue) match = false;
            return match;
        });
        renderTable();
        updateInfoFilter();
    }
    function updateInfoFilter() {
        const totalAll = inventory.length;
        const totalFiltered = filteredInventory.length;
        if (filterNama.value || filterSupplier.value || filterKategori.value) infoFilter.textContent = `🔎 Menampilkan ${totalFiltered} dari ${totalAll} data (filter aktif)`;
        else infoFilter.textContent = `📋 Total semua data: ${totalAll}`;
        totalDataCount.textContent = `${totalAll} item`;
    }
    function renderTable() {
        if (!tableBody) return;
        if (filteredInventory.length === 0) { tableBody.innerHTML = `<tr class="empty-row"><td colspan="8">${inventory.length === 0 ? "Belum ada data. Silakan tambah barang masuk." : "Tidak ada data yang sesuai."}</td></tr>`; return; }
        tableBody.innerHTML = filteredInventory.map(item => `<tr data-id="${item.id}"><td>${escapeHtml(item.supplier)}</td><td>${escapeHtml(item.namaBarang)}</td><td>${escapeHtml(item.kategori)}</td><td style="text-align:right">${Number(item.jumlah).toLocaleString()}</td><td>${escapeHtml(item.unit)}</td><td>${formatDate(item.tanggalMasuk)}</td><td>${escapeHtml(item.catatan) || '-'}</td><td class="action-buttons"><button class="edit-btn" data-id="${item.id}" title="Edit Data">✏️</button><button class="delete-btn" data-id="${item.id}" title="Hapus">🗑️</button></td></tr>`).join('');
        document.querySelectorAll('.edit-btn').forEach(btn => btn.addEventListener('click', (e) => { const id = btn.getAttribute('data-id'); loadItemToForm(id); }));
        document.querySelectorAll('.delete-btn').forEach(btn => btn.addEventListener('click', (e) => { const id = btn.getAttribute('data-id'); if(confirm('Yakin hapus?')) deleteItemById(id); }));
    }
    function deleteItemById(id) { inventory = inventory.filter(item => item.id !== id); saveData(); if(editMode && editingId === id) cancelEdit(); applyFilters(); showToast('✅ Data dihapus'); }
    function loadItemToForm(id) {
        const item = inventory.find(i => i.id === id);
        if(!item) return;
        editMode = true; editingId = id;
        supplierInput.value = item.supplier; namaBarangInput.value = item.namaBarang; kategoriSelect.value = item.kategori; jumlahInput.value = item.jumlah; unitInput.value = item.unit; tglMasukInput.value = item.tanggalMasuk; catatanInput.value = item.catatan || '';
        submitBtn.textContent = '✏️ Simpan Perubahan'; submitBtn.classList.add('btn-edit-mode'); cancelEditBtn.style.display = 'inline-block'; showToast('✏️ Mode Edit aktif');
        document.querySelector('.form-card').scrollIntoView({ behavior: 'smooth' });
    }
    function cancelEdit() { editMode = false; editingId = null; resetFormFields(); submitBtn.textContent = 'Tambah Barang'; submitBtn.classList.remove('btn-edit-mode'); cancelEditBtn.style.display = 'none'; showToast('Mode Edit dibatalkan'); }
    function addOrUpdateItem(event) {
        event.preventDefault();
        const supplier = supplierInput.value.trim(), namaBarang = namaBarangInput.value.trim(), kategori = kategoriSelect.value, jumlah = parseInt(jumlahInput.value), unit = unitInput.value.trim(), tanggalMasuk = tglMasukInput.value, catatan = catatanInput.value.trim();
        if (!supplier || !namaBarang || !kategori || !jumlah || !unit || !tanggalMasuk) { alert('Lengkapi semua field wajib!'); return; }
        if (isNaN(jumlah) || jumlah < 1) { alert('Jumlah minimal 1'); return; }
        if (editMode && editingId) {
            const index = inventory.findIndex(i => i.id === editingId);
            if(index !== -1) { inventory[index] = {...inventory[index], supplier, namaBarang, kategori, jumlah, unit, tanggalMasuk, catatan: catatan || '', updatedAt: new Date().toISOString()}; saveData(); applyFilters(); showToast('✅ Data diperbarui'); cancelEdit(); }
            else { alert('Data tidak ditemukan'); cancelEdit(); }
        } else {
            const newItem = { id: Date.now() + '-' + Math.random().toString(36).substring(2,8), supplier, namaBarang, kategori, jumlah, unit, tanggalMasuk, catatan: catatan || '', createdAt: new Date().toISOString() };
            inventory.unshift(newItem); saveData(); resetFormFields(); applyFilters(); showToast('📦 Barang ditambahkan');
        }
    }
    function resetFormFields() { supplierInput.value = ''; namaBarangInput.value = ''; kategoriSelect.value = ''; jumlahInput.value = ''; unitInput.value = ''; catatanInput.value = ''; setDefaultDate(); supplierInput.focus(); }
    function resetAllFilters() { filterNama.value = ''; filterSupplier.value = ''; filterKategori.value = ''; applyFilters(); }
    function printTableData() { if(inventory.length===0){ alert('Tidak ada data'); return; } const dataToPrint = filteredInventory.length>0 ? filteredInventory : inventory; let rowsHtml=''; dataToPrint.forEach(item=>{ rowsHtml+=`<tr><td>${escapeHtml(item.supplier)}</td><td>${escapeHtml(item.namaBarang)}</td><td>${escapeHtml(item.kategori)}</td><td style="text-align:right">${item.jumlah}</td><td>${escapeHtml(item.unit)}</td><td>${formatDate(item.tanggalMasuk)}</td><td>${escapeHtml(item.catatan)||'-'}</td></tr>`;}); const w=window.open('','_blank'); w.document.write(`<html><head><title>Laporan Barang Masuk</title><style>body{font-family:Arial;margin:20px;}table{border-collapse:collapse;width:100%}th,td{border:1px solid #aaa;padding:8px}</style></head><body><h2>Laporan Barang Masuk</h2><p>Total:${dataToPrint.length}</p><table><thead><tr><th>Supplier</th><th>Nama Barang</th><th>Kategori</th><th>Jumlah</th><th>Unit</th><th>Tgl Masuk</th><th>Catatan</th></tr></thead><tbody>${rowsHtml}</tbody></table></body></html>`); w.document.close(); w.print(); }
    function exportToExcel() { if(inventory.length===0){ alert('Tidak ada data'); return; } const data= filteredInventory.length>0 ? filteredInventory : inventory; const sheetData=[['Supplier','Nama Barang','Kategori','Jumlah','Satuan','Tanggal Masuk','Catatan']]; data.forEach(i=>{ sheetData.push([i.supplier, i.namaBarang, i.kategori, i.jumlah, i.unit, formatDateForExcel(i.tanggalMasuk), i.catatan||'']); }); const ws=XLSX.utils.aoa_to_sheet(sheetData); ws['!cols']=[{wch:25},{wch:30},{wch:20},{wch:12},{wch:10},{wch:15},{wch:25}]; const wb=XLSX.utils.book_new(); XLSX.utils.book_append_sheet(wb,ws,'Barang Masuk'); XLSX.writeFile(wb,`Laporan_Barang_Masuk_${new Date().toISOString().slice(0,19).replace(/:/g,'-')}.xlsx`); showToast('Export Excel berhasil'); }

    // ==================== TOTAL PAGE (AGREGASI) ====================
    function buildAggregation() {
        const aggMap = new Map(); // key: namaBarang|kategori
        for(let item of inventory) {
            const key = `${item.namaBarang}|${item.kategori}`;
            if(!aggMap.has(key)) {
                aggMap.set(key, {
                    namaBarang: item.namaBarang,
                    kategori: item.kategori,
                    totalJumlah: 0,
                    suppliersSet: new Set(),
                    lastDate: item.tanggalMasuk,
                    unit: item.unit
                });
            }
            const entry = aggMap.get(key);
            entry.totalJumlah += item.jumlah;
            entry.suppliersSet.add(item.supplier);
            if(item.tanggalMasuk > entry.lastDate) entry.lastDate = item.tanggalMasuk;
        }
        const aggArray = Array.from(aggMap.values()).map(entry => ({
            namaBarang: entry.namaBarang,
            kategori: entry.kategori,
            totalJumlah: entry.totalJumlah,
            unit: entry.unit,
            supplierCount: entry.suppliersSet.size,
            lastDate: entry.lastDate
        }));
        return aggArray;
    }

    function renderTotalPage() {
        let aggData = buildAggregation();
        const keyword = filterTotalNama.value.trim().toLowerCase();
        const kategoriFilter = filterTotalKategori.value;
        let filteredAgg = aggData.filter(item => {
            if(keyword && !item.namaBarang.toLowerCase().includes(keyword)) return false;
            if(kategoriFilter && item.kategori !== kategoriFilter) return false;
            return true;
        });
        // update summary stats
        totalTransaksiSpan.innerText = inventory.length;
        const grandTotal = inventory.reduce((sum,item)=> sum + item.jumlah,0);
        grandTotalJumlahSpan.innerText = grandTotal.toLocaleString();
        const uniqueCount = aggData.length;
        uniqueItemsCountSpan.innerText = uniqueCount;
        totalAggregateCount.innerText = `${filteredAgg.length} item unik`;
        if(filteredAgg.length === 0) { totalTableBody.innerHTML = '<tr class="empty-row"><td colspan="6">Tidak ada data agregasi.</td></tr>'; return; }
        totalTableBody.innerHTML = filteredAgg.map(item => `
            <tr>
                <td>${escapeHtml(item.namaBarang)}</td>
                <td>${escapeHtml(item.kategori)}</td>
                <td style="text-align:right">${item.totalJumlah.toLocaleString()}</td>
                <td>${escapeHtml(item.unit)}</td>
                <td style="text-align:center">${item.supplierCount}</td>
                <td>${formatDate(item.lastDate)}</td>
            </tr>
        `).join('');
    }

    function exportTotalToExcel() {
        let aggData = buildAggregation();
        const keyword = filterTotalNama.value.trim().toLowerCase();
        const kategoriFilter = filterTotalKategori.value;
        let filteredAgg = aggData.filter(item => {
            if(keyword && !item.namaBarang.toLowerCase().includes(keyword)) return false;
            if(kategoriFilter && item.kategori !== kategoriFilter) return false;
            return true;
        });
        if(filteredAgg.length === 0 && aggData.length>0) { alert('Tidak ada data sesuai filter untuk diekspor'); return; }
        if(inventory.length===0){ alert('Belum ada data transaksi'); return; }
        const sheetData = [['Nama Barang', 'Kategori', 'Total Jumlah', 'Satuan', 'Jumlah Supplier Terlibat', 'Terakhir Masuk']];
        filteredAgg.forEach(item => {
            sheetData.push([item.namaBarang, item.kategori, item.totalJumlah, item.unit, item.supplierCount, formatDateForExcel(item.lastDate)]);
        });
        const ws = XLSX.utils.aoa_to_sheet(sheetData);
        ws['!cols'] = [{wch:30},{wch:25},{wch:15},{wch:12},{wch:22},{wch:18}];
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, 'Total Semua Barang');
        XLSX.writeFile(wb, `Total_Barang_${new Date().toISOString().slice(0,19).replace(/:/g,'-')}.xlsx`);
        showToast('Export Total Barang ke Excel berhasil');
    }

    // Event untuk tab total
    filterTotalNama.addEventListener('input', renderTotalPage);
    filterTotalKategori.addEventListener('change', renderTotalPage);
    resetTotalFilterBtn.addEventListener('click', () => { filterTotalNama.value = ''; filterTotalKategori.value = ''; renderTotalPage(); });
    exportTotalExcelBtn.addEventListener('click', exportTotalToExcel);

    // TAB SWITCHING
    function switchTab(tabId) {
        if(tabId === 'riwayat') {
            tabRiwayatDiv.classList.remove('hidden-tab');
            tabTotalDiv.classList.add('hidden-tab');
            tabRiwayatBtn.classList.add('active');
            tabTotalBarangBtn.classList.remove('active');
        } else {
            tabRiwayatDiv.classList.add('hidden-tab');
            tabTotalDiv.classList.remove('hidden-tab');
            tabRiwayatBtn.classList.remove('active');
            tabTotalBarangBtn.classList.add('active');
            renderTotalPage(); // refresh saat tampil
        }
    }
    tabRiwayatBtn.addEventListener('click', () => switchTab('riwayat'));
    tabTotalBarangBtn.addEventListener('click', () => switchTab('total'));

    // Event listeners utama
    form.addEventListener('submit', addOrUpdateItem);
    resetFormBtn.addEventListener('click', () => { if(editMode) cancelEdit(); else resetFormFields(); });
    cancelEditBtn.addEventListener('click', cancelEdit);
    printBtn.addEventListener('click', printTableData);
    exportExcelBtn.addEventListener('click', exportToExcel);
    clearFilterBtn.addEventListener('click', resetAllFilters);
    filterNama.addEventListener('input', applyFilters);
    filterSupplier.addEventListener('input', applyFilters);
    filterKategori.addEventListener('change', applyFilters);
    
    setDefaultDate();
    loadData();
</script>
</body>
</html>
