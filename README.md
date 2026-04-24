<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Sistem Input Barang Masuk | AutoComplete + Per Bulan</title>
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
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 18px;
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .input-group label {
            font-weight: 600;
            font-size: 0.75rem;
            color: #1e293b;
            letter-spacing: 0.3px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .input-group label i {
            font-size: 1rem;
        }

        .input-group input,
        .input-group select {
            padding: 10px 14px;
            border: 1.5px solid #e2e8f0;
            border-radius: 14px;
            font-size: 0.85rem;
            transition: all 0.2s;
            background: #fafcff;
        }

        .input-group input:focus,
        .input-group select:focus {
            outline: none;
            border-color: #2c6e9e;
            box-shadow: 0 0 0 3px rgba(44,110,158,0.15);
        }

        /* Style untuk datalist */
        datalist {
            background: white;
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
            flex-wrap: wrap;
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

        /* Filter Panel */
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
            min-width: 150px;
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

        /* Tabel dengan lebar kolom yang sesuai */
        .table-wrapper {
            overflow-x: auto;
            border-radius: 18px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.75rem;
        }

        th {
            text-align: left;
            padding: 10px 8px;
            background: #f1f5f9;
            color: #1e293b;
            font-weight: 700;
            border-bottom: 2px solid #e2e8f0;
            white-space: nowrap;
        }

        td {
            padding: 9px 8px;
            border-bottom: 1px solid #edf2f7;
            vertical-align: middle;
            color: #1f2a44;
        }

        /* Grup bulan */
        .month-group {
            background: #eef2ff;
            font-weight: 700;
            font-size: 0.8rem;
            border-top: 2px solid #cbd5e1;
            border-bottom: 2px solid #cbd5e1;
        }
        .month-group td {
            background: #eef2ff;
            padding: 8px 12px;
            color: #0f2b3d;
        }

        .action-buttons {
            display: flex;
            gap: 6px;
            align-items: center;
            flex-wrap: nowrap;
        }
        .edit-btn, .delete-btn {
            background: none;
            border: none;
            cursor: pointer;
            padding: 4px 6px;
            border-radius: 20px;
            font-size: 1rem;
            transition: all 0.1s;
        }
        .edit-btn {
            color: #0f3b5c;
        }
        .edit-btn:hover {
            background: #e6f0fa;
        }
        .delete-btn {
            color: #b91c1c;
        }
        .delete-btn:hover {
            background: #fee2e2;
        }

        /* Pagination */
        .pagination {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 8px;
            margin-top: 24px;
            flex-wrap: wrap;
        }
        .page-btn {
            background: white;
            border: 1px solid #cbd5e1;
            padding: 6px 12px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 0.75rem;
            font-weight: 500;
            transition: all 0.2s;
        }
        .page-btn:hover:not(:disabled) {
            background: #e2e8f0;
        }
        .page-btn.active {
            background: #0f2b3d;
            color: white;
            border-color: #0f2b3d;
        }
        .page-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        .page-info {
            font-size: 0.75rem;
            color: #475569;
            margin: 0 8px;
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

        @media (max-width: 640px) {
            body { padding: 12px; }
            .form-card { padding: 18px; }
            .btn-group .btn { flex: 1; text-align: center; }
            .filter-panel { flex-direction: column; align-items: stretch; }
            .section-header { flex-direction: column; align-items: flex-start; }
            th, td { font-size: 0.7rem; padding: 6px 4px; }
            .edit-btn, .delete-btn { padding: 2px 4px; font-size: 0.9rem; }
        }

        /* Saran otomatis */
        .autocomplete-hint {
            font-size: 0.7rem;
            color: #6c757d;
            margin-top: 2px;
            margin-left: 12px;
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="main-header">
        <h1>Sistem Pencatatan Barang Masuk</h1>
        <p>Input jadi lebih mudah dengan saran otomatis (AutoComplete) | Kelola per bulan</p>
    </div>

    <!-- FORM TAMBAH / EDIT -->
    <div class="form-card">
        <form id="barangForm">
            <div class="form-grid">
                <div class="input-group">
                    <label>🏭 Nama Supplier <i>(auto-saran)</i></label>
                    <input type="text" id="supplier" list="supplierList" placeholder="Ketik atau pilih supplier" autocomplete="off" required>
                    <datalist id="supplierList"></datalist>
                    <div class="autocomplete-hint">💡 Akan muncul saran dari data tersimpan</div>
                </div>
                <div class="input-group">
                    <label>📦 Nama Barang <i>(auto-saran)</i></label>
                    <input type="text" id="namaBarang" list="barangList" placeholder="Ketik atau pilih barang" autocomplete="off" required>
                    <datalist id="barangList"></datalist>
                    <div class="autocomplete-hint">💡 Otomatis menyimpan data baru</div>
                </div>
                <div class="input-group">
                    <label>🏷️ Kategori</label>
                    <select id="kategori" required>
                        <option value="" disabled selected>-- Pilih --</option>
                        <option>Booklet 1 1/4</option><option>Booklet 1 1/4 tips</option>
                        <option>Booklet Kss</option><option>Booklet Kss tips</option>
                        <option>Display box</option><option>Filter tips 21</option>
                        <option>Filter tips 26</option><option>Filter tips 30</option>
                        <option>Trapezoid</option><option>Sticker</option><option>Lainnya</option>
                    </select>
                </div>
                <div class="input-group">
                    <label>🔢 Jumlah</label>
                    <input type="number" id="jumlah" placeholder="0" min="1" required>
                </div>
                <div class="input-group">
                    <label>📏 Satuan <i>(auto-saran)</i></label>
                    <input type="text" id="unit" list="unitList" placeholder="pcs, box, kg, roll" autocomplete="off" required>
                    <datalist id="unitList"></datalist>
                    <div class="autocomplete-hint">💡 pcs, box, kg, roll, dll</div>
                </div>
                <div class="input-group">
                    <label>📅 Tanggal Masuk</label>
                    <input type="date" id="tanggalMasuk" required>
                </div>
                <div class="input-group">
                    <label>📝 Catatan / No. PO</label>
                    <input type="text" id="catatan" placeholder="Opsional: PO-123 / INV-xxx">
                </div>
            </div>
            <div class="btn-group">
                <button type="submit" class="btn btn-primary" id="submitBtn">➕ Tambah Barang</button>
                <button type="button" id="resetFormBtn" class="btn btn-secondary">🗑️ Reset Form</button>
                <button type="button" id="cancelEditBtn" class="btn btn-secondary" style="display:none;">✖️ Batalkan Edit</button>
            </div>
        </form>
    </div>

    <!-- SECTION DATA -->
    <div class="data-section">
        <div class="section-header">
            <div class="title-badge">
                <h2>📋 Riwayat Barang Masuk</h2>
                <span class="badge" id="totalDataCount">0 item</span>
            </div>
            <div class="aksi-buttons">
                <button id="printBtn" class="small-icon-btn">🖨️ Cetak</button>
                <button id="exportExcelBtn" class="small-icon-btn">📎 Export Excel</button>
            </div>
        </div>

        <div class="filter-panel">
            <div class="filter-item">
                <label>🔍 Cari Barang / Supplier</label>
                <input type="text" id="filterText" placeholder="Ketik nama barang atau supplier...">
            </div>
            <div class="filter-item">
                <label>📂 Filter Kategori</label>
                <select id="filterKategori">
                    <option value="">Semua Kategori</option>
                    <option>Booklet 1 1/4</option><option>Booklet 1 1/4 tips</option>
                    <option>Booklet Kss</option><option>Booklet Kss tips</option>
                    <option>Display box</option><option>Filter tips 21</option>
                    <option>Filter tips 26</option><option>Filter tips 30</option>
                    <option>Trapezoid</option><option>Sticker</option><option>Lainnya</option>
                </select>
            </div>
            <div class="filter-item">
                <label>🗓️ Filter Bulan</label>
                <input type="month" id="filterBulan" placeholder="Pilih bulan">
            </div>
            <button id="clearFilterBtn" class="btn-reset-filter">✖️ Reset Filter</button>
        </div>

        <div id="infoFilter" class="info-search"></div>

        <div class="table-wrapper">
            <table id="mainTable">
                <thead>
                    <tr><th>Supplier</th><th>Nama Barang</th><th>Kategori</th><th>Jml</th><th>Satuan</th><th>Tgl Masuk</th><th>Catatan</th><th>Aksi</th></tr>
                </thead>
                <tbody id="tableBody">
                    <tr class="empty-row"><td colspan="8">⚡ Belum ada data. Silakan tambah barang masuk.</td></tr>
                </tbody>
             </table>
        </div>

        <!-- PAGINATION -->
        <div class="pagination" id="paginationContainer"></div>
    </div>
    <footer>📌 Fitur AutoComplete pada Supplier, Nama Barang, dan Satuan akan belajar dari input Anda. Data tersimpan otomatis.</footer>
    <div id="toastMessage" class="toast-msg"></div>
</div>

<script>
    // ======================= DATA GLOBAL ========================
    let inventory = [];           
    let filteredInventory = [];   
    let editMode = false;
    let editingId = null;
    
    // Pagination
    let currentPage = 1;
    const rowsPerPage = 10;

    // DOM Elements
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
    const filterText = document.getElementById('filterText');
    const filterKategori = document.getElementById('filterKategori');
    const filterBulan = document.getElementById('filterBulan');
    const clearFilterBtn = document.getElementById('clearFilterBtn');
    const infoFilter = document.getElementById('infoFilter');
    const toastMsgDiv = document.getElementById('toastMessage');
    const paginationContainer = document.getElementById('paginationContainer');

    // Datalist elements
    const supplierDatalist = document.getElementById('supplierList');
    const barangDatalist = document.getElementById('barangList');
    const unitDatalist = document.getElementById('unitList');

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

    // ========== UPDATE DATALIST (AutoComplete) dari data inventory ==========
    function updateDatalists() {
        // Kumpulkan semua supplier unik
        const suppliers = [...new Set(inventory.map(item => item.supplier).filter(s => s && s.trim()))];
        supplierDatalist.innerHTML = suppliers.map(s => `<option value="${escapeHtml(s)}">`).join('');
        
        // Kumpulkan semua nama barang unik
        const barang = [...new Set(inventory.map(item => item.namaBarang).filter(b => b && b.trim()))];
        barangDatalist.innerHTML = barang.map(b => `<option value="${escapeHtml(b)}">`).join('');
        
        // Kumpulkan semua satuan unik + default satuan umum
        const unitsFromData = [...new Set(inventory.map(item => item.unit).filter(u => u && u.trim()))];
        const defaultUnits = ['pcs', 'box', 'kg', 'roll', 'set', 'pack', 'lembar', 'meter', 'liter'];
        const allUnits = [...new Set([...defaultUnits, ...unitsFromData])];
        unitDatalist.innerHTML = allUnits.map(u => `<option value="${escapeHtml(u)}">`).join('');
    }

    function loadData() {
        const stored = localStorage.getItem('inventory_barang_masuk');
        if (stored) {
            try {
                inventory = JSON.parse(stored);
                if (!Array.isArray(inventory)) inventory = [];
            } catch(e) { inventory = []; }
        } else {
            // Data contoh untuk demonstrasi
            inventory = [];
        }
        updateDatalists();
        applyFilters();
    }

    function saveData() {
        localStorage.setItem('inventory_barang_masuk', JSON.stringify(inventory));
        updateDatalists(); // Update datalist setiap kali data berubah
    }

    function formatDate(dateStr) {
        if (!dateStr) return '-';
        const parts = dateStr.split('-');
        if (parts.length !== 3) return dateStr;
        return `${parts[2]}/${parts[1]}/${parts[0]}`;
    }

    function getMonthYear(dateStr) {
        if (!dateStr) return '';
        const parts = dateStr.split('-');
        if (parts.length !== 3) return '';
        const bulan = new Date(dateStr).toLocaleString('id-ID', { month: 'long' });
        return `${bulan} ${parts[0]}`;
    }

    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
            return c;
        });
    }

    // Filter data (teks, kategori, bulan)
    function applyFilters() {
        const searchText = filterText.value.trim().toLowerCase();
        const kategoriValue = filterKategori.value;
        const bulanValue = filterBulan.value;

        filteredInventory = inventory.filter(item => {
            let match = true;
            if (searchText) {
                const inNama = item.namaBarang.toLowerCase().includes(searchText);
                const inSupplier = item.supplier.toLowerCase().includes(searchText);
                if (!inNama && !inSupplier) match = false;
            }
            if (kategoriValue && item.kategori !== kategoriValue) match = false;
            if (bulanValue) {
                const itemBulan = item.tanggalMasuk.substring(0, 7); // YYYY-MM
                if (itemBulan !== bulanValue) match = false;
            }
            return match;
        });
        
        // Urutkan berdasarkan tanggal terbaru (desc)
        filteredInventory.sort((a, b) => new Date(b.tanggalMasuk) - new Date(a.tanggalMasuk));
        
        currentPage = 1;
        renderTableWithPagination();
        updateInfoFilter();
    }

    function updateInfoFilter() {
        const totalAll = inventory.length;
        const totalFiltered = filteredInventory.length;
        if (filterText.value || filterKategori.value || filterBulan.value) {
            infoFilter.textContent = `🔎 Menampilkan ${totalFiltered} dari ${totalAll} data (filter aktif)`;
        } else {
            infoFilter.textContent = `📋 Total semua data: ${totalAll}`;
        }
        totalDataCount.textContent = `${totalAll} item`;
    }

    // Render tabel dengan grouping per bulan + pagination
    function renderTableWithPagination() {
        if (!tableBody) return;
        
        if (filteredInventory.length === 0) {
            tableBody.innerHTML = `<tr class="empty-row"><td colspan="8">${inventory.length === 0 ? 'Belum ada data. Silakan tambah barang masuk.' : 'Tidak ada data yang sesuai dengan pencarian.'}</td></tr>`;
            paginationContainer.innerHTML = '';
            return;
        }

        // Group data berdasarkan bulan-tahun
        const grouped = {};
        filteredInventory.forEach(item => {
            const monthYearKey = item.tanggalMasuk.substring(0, 7);
            const monthYearDisplay = getMonthYear(item.tanggalMasuk);
            if (!grouped[monthYearKey]) {
                grouped[monthYearKey] = { display: monthYearDisplay, items: [] };
            }
            grouped[monthYearKey].items.push(item);
        });

        // Buat array flat dengan separator bulan
        let flatRows = [];
        const groupKeys = Object.keys(grouped).sort().reverse();
        groupKeys.forEach(key => {
            flatRows.push({ type: 'month-header', monthDisplay: grouped[key].display, monthKey: key, count: grouped[key].items.length });
            grouped[key].items.forEach(item => {
                flatRows.push({ type: 'data', item: item });
            });
        });

        // Pagination pada flatRows
        const totalPages = Math.ceil(flatRows.length / rowsPerPage);
        const startIndex = (currentPage - 1) * rowsPerPage;
        const endIndex = startIndex + rowsPerPage;
        const paginatedRows = flatRows.slice(startIndex, endIndex);

        // Render HTML
        let html = '';
        for (const row of paginatedRows) {
            if (row.type === 'month-header') {
                html += `<tr class="month-group"><td colspan="8"><strong>📅 ${escapeHtml(row.monthDisplay)}</strong> (${row.count} item)</td></tr>`;
            } else {
                const item = row.item;
                html += `
                    <tr data-id="${item.id}">
                        <td>${escapeHtml(item.supplier)}</td>
                        <td>${escapeHtml(item.namaBarang)}</td>
                        <td>${escapeHtml(item.kategori)}</td>
                        <td style="text-align:right">${Number(item.jumlah).toLocaleString()}</td>
                        <td>${escapeHtml(item.unit)}</td>
                        <td>${formatDate(item.tanggalMasuk)}</td>
                        <td>${escapeHtml(item.catatan) || '-'}</td>
                        <td class="action-buttons">
                            <button class="edit-btn" data-id="${item.id}" title="Edit">✏️</button>
                            <button class="delete-btn" data-id="${item.id}" title="Hapus">🗑️</button>
                        </td>
                    </tr>
                `;
            }
        }
        tableBody.innerHTML = html;

        // Event listeners untuk tombol edit/hapus
        document.querySelectorAll('.edit-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                e.stopPropagation();
                const id = btn.getAttribute('data-id');
                loadItemToForm(id);
            });
        });
        document.querySelectorAll('.delete-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                e.stopPropagation();
                const id = btn.getAttribute('data-id');
                if (confirm('Yakin ingin menghapus data ini ?')) deleteItemById(id);
            });
        });

        renderPaginationControls(totalPages);
    }

    function renderPaginationControls(totalPages) {
        if (totalPages <= 1) {
            paginationContainer.innerHTML = '';
            return;
        }
        let btnHtml = '';
        btnHtml += `<button class="page-btn" id="firstPage" ${currentPage === 1 ? 'disabled' : ''}>« Pertama</button>`;
        btnHtml += `<button class="page-btn" id="prevPage" ${currentPage === 1 ? 'disabled' : ''}>‹ Sebelum</button>`;
        
        let startPage = Math.max(1, currentPage - 2);
        let endPage = Math.min(totalPages, currentPage + 2);
        for (let i = startPage; i <= endPage; i++) {
            btnHtml += `<button class="page-btn ${i === currentPage ? 'active' : ''}" data-page="${i}">${i}</button>`;
        }
        
        btnHtml += `<button class="page-btn" id="nextPage" ${currentPage === totalPages ? 'disabled' : ''}>Berikut ›</button>`;
        btnHtml += `<button class="page-btn" id="lastPage" ${currentPage === totalPages ? 'disabled' : ''}>Terakhir »</button>`;
        btnHtml += `<span class="page-info">Halaman ${currentPage} dari ${totalPages}</span>`;
        
        paginationContainer.innerHTML = btnHtml;
        
        document.getElementById('firstPage')?.addEventListener('click', () => { currentPage = 1; renderTableWithPagination(); });
        document.getElementById('prevPage')?.addEventListener('click', () => { if (currentPage > 1) { currentPage--; renderTableWithPagination(); } });
        document.getElementById('nextPage')?.addEventListener('click', () => { if (currentPage < totalPages) { currentPage++; renderTableWithPagination(); } });
        document.getElementById('lastPage')?.addEventListener('click', () => { currentPage = totalPages; renderTableWithPagination(); });
        document.querySelectorAll('.page-btn[data-page]').forEach(btn => {
            btn.addEventListener('click', () => { currentPage = parseInt(btn.dataset.page); renderTableWithPagination(); });
        });
    }

    // CRUD Operations
    function deleteItemById(id) {
        inventory = inventory.filter(item => item.id !== id);
        saveData();
        if (editMode && editingId === id) cancelEdit();
        applyFilters();
        showToast('✅ Data berhasil dihapus');
    }

    function loadItemToForm(id) {
        const item = inventory.find(i => i.id === id);
        if (!item) return;
        editMode = true;
        editingId = id;
        supplierInput.value = item.supplier;
        namaBarangInput.value = item.namaBarang;
        kategoriSelect.value = item.kategori;
        jumlahInput.value = item.jumlah;
        unitInput.value = item.unit;
        tglMasukInput.value = item.tanggalMasuk;
        catatanInput.value = item.catatan || '';
        submitBtn.textContent = '✏️ Simpan Perubahan';
        submitBtn.classList.add('btn-edit-mode');
        cancelEditBtn.style.display = 'inline-block';
        showToast('✏️ Mode Edit aktif - ubah data', false);
        document.querySelector('.form-card').scrollIntoView({ behavior: 'smooth' });
    }

    function cancelEdit() {
        editMode = false;
        editingId = null;
        resetFormFields();
        submitBtn.textContent = '➕ Tambah Barang';
        submitBtn.classList.remove('btn-edit-mode');
        cancelEditBtn.style.display = 'none';
        showToast('Mode Edit dibatalkan', false);
    }

    function addOrUpdateItem(event) {
        event.preventDefault();
        const supplier = supplierInput.value.trim();
        const namaBarang = namaBarangInput.value.trim();
        const kategori = kategoriSelect.value;
        const jumlah = parseInt(jumlahInput.value);
        const unit = unitInput.value.trim();
        const tanggalMasuk = tglMasukInput.value;
        const catatan = catatanInput.value.trim();

        if (!supplier || !namaBarang || !kategori || !jumlah || !unit || !tanggalMasuk) {
            alert('⚠️ Harap lengkapi semua field wajib');
            return;
        }
        if (isNaN(jumlah) || jumlah < 1) { alert('Jumlah minimal 1'); return; }

        if (editMode && editingId) {
            const index = inventory.findIndex(i => i.id === editingId);
            if (index !== -1) {
                inventory[index] = { ...inventory[index], supplier, namaBarang, kategori, jumlah, unit, tanggalMasuk, catatan: catatan || '', updatedAt: new Date().toISOString() };
                saveData();
                applyFilters();
                showToast('✅ Data berhasil diperbarui!');
                cancelEdit();
            } else { cancelEdit(); }
        } else {
            const newItem = { id: Date.now() + '-' + Math.random().toString(36).substring(2, 8), supplier, namaBarang, kategori, jumlah, unit, tanggalMasuk, catatan: catatan || '', createdAt: new Date().toISOString() };
            inventory.unshift(newItem);
            saveData();
            resetFormFields();
            applyFilters();
            showToast('📦 Barang berhasil ditambahkan!');
        }
    }

    function resetFormFields() {
        supplierInput.value = '';
        namaBarangInput.value = '';
        kategoriSelect.value = '';
        jumlahInput.value = '';
        unitInput.value = '';
        catatanInput.value = '';
        setDefaultDate();
        supplierInput.focus();
    }

    function resetAllFilters() {
        filterText.value = '';
        filterKategori.value = '';
        filterBulan.value = '';
        applyFilters();
    }

    function printTableData() {
        if (filteredInventory.length === 0) { alert('Tidak ada data untuk dicetak'); return; }
        let rowsHtml = '';
        filteredInventory.forEach(item => {
            rowsHtml += `<tr><td>${escapeHtml(item.supplier)}</td><td>${escapeHtml(item.namaBarang)}</td><td>${escapeHtml(item.kategori)}</td><td style="text-align:right">${item.jumlah}</td><td>${escapeHtml(item.unit)}</td><td>${formatDate(item.tanggalMasuk)}</td><td>${escapeHtml(item.catatan) || '-'}</td></tr>`;
        });
        const printWindow = window.open('', '_blank');
        printWindow.document.write(`<html><head><title>Laporan Barang Masuk</title><style>body{font-family:Arial;margin:20px}table{border-collapse:collapse;width:100%}th,td{border:1px solid #aaa;padding:8px;text-align:left}th{background:#eef2f5}</style></head><body><h2>📋 Laporan Penerimaan Barang</h2><p>Tanggal cetak: ${new Date().toLocaleString('id-ID')} | Total: ${filteredInventory.length}</p> <table><thead><tr><th>Supplier</th><th>Nama Barang</th><th>Kategori</th><th>Jumlah</th><th>Satuan</th><th>Tgl Masuk</th><th>Catatan</th></tr></thead><tbody>${rowsHtml}</tbody></table></body></html>`);
        printWindow.document.close();
        printWindow.print();
    }

    function exportToExcel() {
        if (filteredInventory.length === 0) { alert('Tidak ada data untuk diekspor'); return; }
        const sheetData = [['Supplier', 'Nama Barang', 'Kategori', 'Jumlah', 'Satuan', 'Tanggal Masuk', 'Catatan']];
        filteredInventory.forEach(item => { sheetData.push([item.supplier, item.namaBarang, item.kategori, item.jumlah, item.unit, formatDate(item.tanggalMasuk), item.catatan || '']); });
        const worksheet = XLSX.utils.aoa_to_sheet(sheetData);
        worksheet['!cols'] = [{wch:25},{wch:30},{wch:20},{wch:12},{wch:10},{wch:15},{wch:25}];
        const workbook = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(workbook, worksheet, 'Barang Masuk');
        XLSX.writeFile(workbook, `Laporan_Barang_Masuk_${new Date().toISOString().slice(0,19).replace(/:/g, '-')}.xlsx`);
        showToast(`📎 Ekspor ${filteredInventory.length} data ke Excel`);
    }

    // Event Listeners
    form.addEventListener('submit', addOrUpdateItem);
    resetFormBtn.addEventListener('click', () => { if (editMode) cancelEdit(); else resetFormFields(); });
    cancelEditBtn.addEventListener('click', cancelEdit);
    printBtn.addEventListener('click', printTableData);
    exportExcelBtn.addEventListener('click', exportToExcel);
    clearFilterBtn.addEventListener('click', resetAllFilters);
    filterText.addEventListener('input', applyFilters);
    filterKategori.addEventListener('change', applyFilters);
    filterBulan.addEventListener('change', applyFilters);

    setDefaultDate();
    loadData();
</script>
</body>
</html>
