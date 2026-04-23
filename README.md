<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Input Barang Masuk + Supplier | Pencarian & Ekspor</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, 'Segoe UI', 'Inter', 'Poppins', sans-serif;
        }

        body {
            background: linear-gradient(145deg, #e0eafc 0%, #cfdef3 100%);
            min-height: 100vh;
            padding: 1rem;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .card {
            max-width: 1400px;
            width: 100%;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.96);
            border-radius: 28px;
            box-shadow: 0 25px 45px -12px rgba(0, 0, 0, 0.35), 0 4px 12px rgba(0, 0, 0, 0.05);
            overflow: hidden;
            transition: transform 0.2s ease;
        }

        .card:hover {
            transform: translateY(-3px);
        }

        .header {
            background: #1e2f5e;
            padding: 1.2rem 1.5rem;
            color: white;
        }

        .header h1 {
            font-size: 1.5rem;
            font-weight: 600;
            letter-spacing: -0.3px;
            display: flex;
            align-items: center;
            gap: 10px;
            flex-wrap: wrap;
        }

        .header h1::before {
            content: "🏭";
            font-size: 1.6rem;
        }

        .header p {
            font-size: 0.8rem;
            opacity: 0.85;
            margin-top: 4px;
        }

        .form-container {
            padding: 1.5rem 1.2rem 1rem;
        }

        .form-group {
            margin-bottom: 1rem;
            display: flex;
            flex-direction: column;
        }

        .form-row {
            display: flex;
            flex-wrap: wrap;
            gap: 0.8rem;
            margin-bottom: 1rem;
        }

        .form-row .form-group {
            flex: 1;
            margin-bottom: 0;
            min-width: 120px;
        }

        label {
            font-weight: 600;
            color: #1f2b48;
            margin-bottom: 5px;
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        input, select, textarea {
            width: 100%;
            padding: 10px 12px;
            font-size: 0.9rem;
            border: 1.5px solid #e2e8f0;
            border-radius: 18px;
            background-color: #ffffff;
            transition: all 0.2s;
            outline: none;
            font-weight: 500;
            color: #0a1c2f;
        }

        input:focus, select:focus, textarea:focus {
            border-color: #1e2f5e;
            box-shadow: 0 0 0 3px rgba(30, 47, 94, 0.2);
        }

        textarea {
            resize: vertical;
            min-height: 70px;
        }

        .button-group {
            display: flex;
            flex-wrap: wrap;
            gap: 0.8rem;
            margin-top: 0.8rem;
            margin-bottom: 0.3rem;
        }

        .btn {
            flex: 1;
            padding: 10px 14px;
            font-weight: 600;
            font-size: 0.9rem;
            border: none;
            border-radius: 40px;
            cursor: pointer;
            transition: all 0.2s;
            background: #f1f5f9;
            color: #1e2f5e;
            border: 1px solid #cbd5e1;
        }

        .btn-primary {
            background: #1e2f5e;
            color: white;
            border: none;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
        }

        .btn-primary:hover {
            background: #0f2147;
            transform: translateY(-2px);
            box-shadow: 0 8px 18px rgba(30, 47, 94, 0.3);
        }

        .btn-secondary {
            background: white;
            border: 1px solid #cbd5e1;
        }

        .btn-secondary:hover {
            background: #f8fafc;
            border-color: #94a3b8;
        }

        .record-section {
            background: #f8fafd;
            border-top: 2px dashed #cbd5e6;
            padding: 1.2rem 1.2rem 1.5rem;
        }

        .toolbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 1.2rem;
        }

        .record-header {
            display: flex;
            align-items: baseline;
            flex-wrap: wrap;
            gap: 8px;
        }

        .record-header h3 {
            font-weight: 700;
            font-size: 1.2rem;
            color: #0f2b3d;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .badge-count {
            background: #e9eef3;
            padding: 3px 10px;
            border-radius: 60px;
            font-size: 0.7rem;
            font-weight: 600;
            color: #1e2f5e;
        }

        .search-panel {
            background: white;
            padding: 0.9rem 1rem;
            border-radius: 28px;
            margin-bottom: 1.2rem;
            display: flex;
            flex-wrap: wrap;
            align-items: flex-end;
            gap: 10px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.05);
            border: 1px solid #e2edf7;
        }
        .search-group {
            flex: 1;
            min-width: 140px;
        }
        .search-group label {
            font-size: 0.65rem;
            margin-bottom: 3px;
            color: #4a5b7a;
        }
        .search-group input, .search-group select {
            padding: 8px 10px;
            font-size: 0.8rem;
            border-radius: 30px;
            background: #f9fcff;
        }
        .clear-search {
            background: none;
            border: 1px solid #cbd5e1;
            padding: 7px 16px;
            border-radius: 40px;
            cursor: pointer;
            font-weight: 500;
            font-size: 0.75rem;
            transition: 0.2s;
            margin-bottom: 0;
            white-space: nowrap;
        }
        .clear-search:hover {
            background: #eef2ff;
        }
        .result-info {
            font-size: 0.7rem;
            background: #eef2ff;
            display: inline-block;
            padding: 3px 10px;
            border-radius: 30px;
            margin-bottom: 10px;
        }

        .action-buttons {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .small-btn {
            padding: 6px 16px;
            font-size: 0.75rem;
            background: white;
            border: 1px solid #cbd5e1;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 5px;
        }

        .small-btn:hover {
            background: #f1f5f9;
            transform: translateY(-1px);
        }

        .table-wrapper {
            overflow-x: auto;
            border-radius: 20px;
            -webkit-overflow-scrolling: touch;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            min-width: 700px;
        }

        th {
            background-color: #eef2f9;
            padding: 10px 8px;
            text-align: left;
            font-size: 0.7rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.4px;
            color: #2c3e66;
        }

        td {
            padding: 10px 8px;
            border-bottom: 1px solid #eef2f9;
            font-size: 0.8rem;
            color: #1f2a40;
            vertical-align: top;
        }

        .delete-btn {
            background: none;
            border: none;
            font-size: 1.1rem;
            cursor: pointer;
            color: #b91c1c;
            transition: 0.1s;
            padding: 4px 6px;
            border-radius: 30px;
        }

        .delete-btn:hover {
            background: #fee2e2;
            transform: scale(1.05);
        }

        .empty-row td {
            text-align: center;
            padding: 1.8rem;
            color: #6c757d;
            font-style: italic;
        }

        footer {
            font-size: 0.65rem;
            text-align: center;
            padding: 0.8rem 1.2rem 1.2rem;
            color: #6c7a91;
            background: white;
            border-top: 1px solid #edf2f7;
        }

        @media (max-width: 600px) {
            body { padding: 0.7rem; }
            .header h1 { font-size: 1.2rem; }
            .header p { font-size: 0.7rem; }
            .form-container { padding: 1rem 0.9rem 0.8rem; }
            .form-row { flex-direction: column; gap: 0.7rem; }
            .form-row .form-group { min-width: 100%; }
            .button-group { flex-direction: column; }
            .btn { width: 100%; }
            .toolbar { flex-direction: column; align-items: stretch; }
            .record-header { justify-content: space-between; }
            .action-buttons { justify-content: flex-end; }
            .search-panel { flex-direction: column; align-items: stretch; border-radius: 20px; padding: 0.8rem; }
            .clear-search { align-self: flex-start; margin-top: 0; }
            th, td { padding: 8px 6px; font-size: 0.7rem; }
        }
        
        @media (max-height: 500px) and (orientation: landscape) {
            body { padding: 0.5rem; }
            .header { padding: 0.5rem 1rem; }
            .header h1 { font-size: 1rem; }
            .header p { display: none; }
            .form-container { padding: 0.6rem 1rem 0.4rem; }
            .form-row { gap: 0.5rem; margin-bottom: 0.5rem; }
            input, select { padding: 6px 8px; font-size: 0.75rem; }
            .btn { padding: 5px 10px; font-size: 0.7rem; }
            .table-wrapper { max-height: 220px; overflow-y: auto; }
            th, td { padding: 5px 6px; font-size: 0.65rem; }
            .record-section { padding: 0.6rem 1rem 0.8rem; }
        }

        @media print {
            body { background: white; padding: 0; margin: 0; }
            .card { box-shadow: none; border-radius: 0; max-width: 100%; }
            .form-container, .button-group, .toolbar .action-buttons, .search-panel, .delete-btn, footer, .btn, #resetFormBtn, .header p {
                display: none !important;
            }
            .record-section { border-top: none; padding: 0; }
            table { box-shadow: none; width: 100%; }
            th, td { border: 1px solid #ccc; }
        }
    </style>
</head>
<body>

<div class="card">
    <div class="header">
        <h1>Input Barang Masuk</h1>
        <p>Data Supplier · Pencarian Nama, Supplier & Kategori · Cetak & Ekspor</p>
    </div>

    <div class="form-container">
        <form id="barangForm">
            <div class="form-row">
                <div class="form-group">
                    <label>🏭 Nama Supplier</label>
                    <input type="text" id="supplier" placeholder="Contoh: PT Sumber Makmur, CV Global Utama" required>
                </div>
                <div class="form-group">
                    <label>📝 Nama Barang</label>
                    <input type="text" id="namaBarang" placeholder="Contoh: Kertas A4, Box Display" required>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>🏷️ Kategori</label>
                    <select id="kategori" required>
                        <option value="" disabled selected>-- Pilih kategori --</option>
                        <option value="Booklet 1 1/4">Booklet 1 1/4</option>
                        <option value="Booklet 1 1/4 tips">Booklet 1 1/4 tips</option>
                        <option value="Booklet Kss">Booklet Kss</option>
                        <option value="Booklet Kss tips">Booklet Kss tips</option>
                        <option value="Display box">Display box</option>
                        <option value="Filter tips 21">Filter tips 21</option>
                        <option value="Filter tips 26">Filter tips 26</option>
                        <option value="Filter tips 30">Filter tips 30</option>
                        <option value="Trapezoid">Trapezoid</option>
                        <option value="Sticker">Sticker</option>
                        <option value="Lainnya">Lainnya</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>🔢 Jumlah</label>
                    <input type="number" id="jumlah" placeholder="0" min="1" required>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>🔖 Satuan / Unit</label>
                    <input type="text" id="unit" placeholder="pcs, box, kg, sheet, roll" required>
                </div>
                <div class="form-group">
                    <label>🗓️ Tanggal Masuk</label>
                    <input type="date" id="tanggalMasuk" required>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>✏️ Catatan / PO</label>
                    <input type="text" id="catatanSupplier" placeholder="Contoh: PO-123 / invoice #INV-001">
                </div>
            </div>

            <div class="button-group">
                <button type="submit" class="btn btn-primary">Tambah Barang</button>
                <button type="button" id="resetFormBtn" class="btn btn-secondary">Reset Form</button>
            </div>
        </form>
    </div>

    <div class="record-section">
        <div class="toolbar">
            <div class="record-header">
                <h3>📋 Log Penerimaan Barang</h3>
                <span class="badge-count" id="totalItemCount">0 item</span>
            </div>
            <div class="action-buttons">
                <button id="printTableBtn" class="small-btn">🖨️ Print</button>
                <button id="downloadCsvBtn" class="small-btn">📎 Download CSV</button>
            </div>
        </div>

        <!-- PANEL PENCARIAN: Nama Barang, Supplier & Kategori -->
        <div class="search-panel">
            <div class="search-group" style="flex:2;">
                <label>🔍 Cari Nama Barang</label>
                <input type="text" id="searchNama" placeholder="Ketik nama barang..." autocomplete="off">
            </div>
            <div class="search-group">
                <label>🏭 Cari Supplier</label>
                <input type="text" id="searchSupplier" placeholder="Nama supplier..." autocomplete="off">
            </div>
            <div class="search-group">
                <label>📂 Filter Kategori</label>
                <select id="searchKategori">
                    <option value="">Semua Kategori</option>
                    <option value="Booklet 1 1/4">Booklet 1 1/4</option>
                    <option value="Booklet 1 1/4 tips">Booklet 1 1/4 tips</option>
                    <option value="Booklet Kss">Booklet Kss</option>
                    <option value="Booklet Kss tips">Booklet Kss tips</option>
                    <option value="Display box">Display box</option>
                    <option value="Filter tips 21">Filter tips 21</option>
                    <option value="Filter tips 26">Filter tips 26</option>
                    <option value="Filter tips 30">Filter tips 30</option>
                    <option value="Trapezoid">Trapezoid</option>
                    <option value="Sticker">Sticker</option>
                    <option value="Lainnya">Lainnya</option>
                </select>
            </div>
            <button id="clearSearchBtn" class="clear-search">✖️ Reset Filter</button>
        </div>
        <div id="searchResultInfo" class="result-info"></div>

        <div class="table-wrapper">
            <table id="dataTable">
                <thead>
                    <tr>
                        <th>Supplier</th>
                        <th>Nama Barang</th>
                        <th>Kategori</th>
                        <th>Jumlah</th>
                        <th>Unit</th>
                        <th>Tgl Masuk</th>
                        <th>Catatan</th>
                        <th>Aksi</th>
                    </tr>
                </thead>
                <tbody id="tableBody">
                    <tr class="empty-row">
                        <td colspan="8">Belum ada data. Silakan tambah barang masuk dari supplier.</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
    <footer>
        ⚡ Data tersimpan di LocalStorage · Pencarian Nama Barang, Supplier & Kategori · Responsif Android
    </footer>
</div>

<script>
    let incomingItems = [];
    let filteredItems = [];

    const form = document.getElementById('barangForm');
    const supplierInput = document.getElementById('supplier');
    const namaBarangInput = document.getElementById('namaBarang');
    const kategoriSelect = document.getElementById('kategori');
    const jumlahInput = document.getElementById('jumlah');
    const unitInput = document.getElementById('unit');
    const tanggalMasukInput = document.getElementById('tanggalMasuk');
    const catatanSupplierInput = document.getElementById('catatanSupplier');
    const resetFormBtn = document.getElementById('resetFormBtn');
    const tableBody = document.getElementById('tableBody');
    const totalItemCountSpan = document.getElementById('totalItemCount');
    const printBtn = document.getElementById('printTableBtn');
    const downloadCsvBtn = document.getElementById('downloadCsvBtn');
    const searchNamaInput = document.getElementById('searchNama');
    const searchSupplierInput = document.getElementById('searchSupplier');
    const searchKategoriSelect = document.getElementById('searchKategori');
    const clearSearchBtn = document.getElementById('clearSearchBtn');
    const searchResultInfo = document.getElementById('searchResultInfo');

    function setDefaultDate() {
        if (!tanggalMasukInput.value) {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            tanggalMasukInput.value = `${yyyy}-${mm}-${dd}`;
        }
    }

    function loadFromStorage() {
        const stored = localStorage.getItem('incoming_supplier_items_v2');
        if (stored) {
            try {
                incomingItems = JSON.parse(stored);
                if (!Array.isArray(incomingItems)) incomingItems = [];
            } catch(e) { incomingItems = []; }
        } else {
            incomingItems = [];
        }
        applyFilters();
    }

    function saveToStorage() {
        localStorage.setItem('incoming_supplier_items_v2', JSON.stringify(incomingItems));
    }

    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        });
    }

    function formatTanggal(dateString) {
        if (!dateString) return '-';
        const parts = dateString.split('-');
        if (parts.length !== 3) return dateString;
        return `${parts[2]}/${parts[1]}/${parts[0]}`;
    }

    function applyFilters() {
        const searchNama = searchNamaInput.value.trim().toLowerCase();
        const searchSupplier = searchSupplierInput.value.trim().toLowerCase();
        const searchKategori = searchKategoriSelect.value;

        filteredItems = incomingItems.filter(item => {
            let matchNama = true;
            let matchSupplier = true;
            let matchKategori = true;
            
            if (searchNama !== "") {
                matchNama = (item.namaBarang || "").toLowerCase().includes(searchNama);
            }
            if (searchSupplier !== "") {
                matchSupplier = (item.supplier || "").toLowerCase().includes(searchSupplier);
            }
            if (searchKategori && searchKategori !== "") {
                matchKategori = (item.kategori || "") === searchKategori;
            }
            return matchNama && matchSupplier && matchKategori;
        });

        const totalFiltered = filteredItems.length;
        const totalAll = incomingItems.length;
        if (searchNama !== "" || searchSupplier !== "" || (searchKategoriSelect.value && searchKategoriSelect.value !== "")) {
            searchResultInfo.textContent = `🔎 Menampilkan ${totalFiltered} dari ${totalAll} item`;
        } else {
            searchResultInfo.textContent = `📋 Total semua: ${totalAll} item`;
        }

        renderTable(filteredItems);
        updateTotalCountDisplay();
    }

    function renderTable(itemsToRender) {
        if (!tableBody) return;
        if (!itemsToRender || itemsToRender.length === 0) {
            tableBody.innerHTML = `<tr class="empty-row"><td colspan="8">📭 Tidak ada data sesuai pencarian atau belum ada barang masuk.</td></tr>`;
            return;
        }

        let htmlRows = '';
        itemsToRender.forEach((item) => {
            const itemId = item.id;
            htmlRows += `
                <tr data-id="${itemId}">
                    <td>${escapeHtml(item.supplier || '-')}</td>
                    <td>${escapeHtml(item.namaBarang)}</td>
                    <td>${escapeHtml(item.kategori)}</td>
                    <td>${escapeHtml(String(item.jumlah))}</td>
                    <td>${escapeHtml(item.unit)}</td>
                    <td>${escapeHtml(formatTanggal(item.tanggalMasuk))}</td>
                    <td>${escapeHtml(item.catatanSupplier || '-')}</td>
                    <td><button class="delete-btn" data-id="${itemId}" title="Hapus">🗑️</button></td>
                </tr>
            `;
        });
        tableBody.innerHTML = htmlRows;

        document.querySelectorAll('.delete-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                e.stopPropagation();
                const id = btn.getAttribute('data-id');
                if (id) deleteItemById(id);
            });
        });
    }

    function deleteItemById(id) {
        const newItems = incomingItems.filter(item => String(item.id) !== String(id));
        if (newItems.length !== incomingItems.length) {
            incomingItems = newItems;
            saveToStorage();
            applyFilters();
        }
    }

    function addItem(itemData) {
        const newId = Date.now() + '-' + Math.random().toString(36).substr(2, 8);
        const newItem = {
            id: newId,
            supplier: itemData.supplier,
            namaBarang: itemData.namaBarang,
            kategori: itemData.kategori,
            jumlah: itemData.jumlah,
            unit: itemData.unit,
            tanggalMasuk: itemData.tanggalMasuk,
            catatanSupplier: itemData.catatanSupplier || ''
        };
        incomingItems.unshift(newItem);
        saveToStorage();
        applyFilters();
    }

    function updateTotalCountDisplay() {
        const showing = filteredItems.length;
        const total = incomingItems.length;
        if (showing === total) {
            totalItemCountSpan.textContent = `${total} item`;
        } else {
            totalItemCountSpan.textContent = `${showing} dari ${total} item`;
        }
    }

    function resetSearchFilters() {
        searchNamaInput.value = '';
        searchSupplierInput.value = '';
        searchKategoriSelect.value = '';
        applyFilters();
    }

    function resetForm() {
        form.reset();
        setDefaultDate();
        kategoriSelect.value = "";
        supplierInput.focus();
    }

    form.addEventListener('submit', (e) => {
        e.preventDefault();
        const supplier = supplierInput.value.trim();
        const namaBarang = namaBarangInput.value.trim();
        const kategori = kategoriSelect.value;
        const jumlah = jumlahInput.value.trim();
        const unit = unitInput.value.trim();
        const tanggalMasuk = tanggalMasukInput.value;
        const catatanSupplier = catatanSupplierInput.value.trim();

        if (!supplier) { alert("Nama Supplier harus diisi!"); supplierInput.focus(); return; }
        if (!namaBarang) { alert("Nama barang harus diisi!"); namaBarangInput.focus(); return; }
        if (!kategori) { alert("Pilih kategori terlebih dahulu!"); return; }
        if (!jumlah || parseInt(jumlah) <= 0) { alert("Jumlah harus lebih dari 0!"); jumlahInput.focus(); return; }
        if (!unit) { alert("Satuan/unit harus diisi!"); unitInput.focus(); return; }
        if (!tanggalMasuk) { alert("Tanggal masuk harus diisi!"); return; }

        addItem({
            supplier: supplier,
            namaBarang: namaBarang,
            kategori: kategori,
            jumlah: parseInt(jumlah),
            unit: unit,
            tanggalMasuk: tanggalMasuk,
            catatanSupplier: catatanSupplier
        });
        resetForm();
    });

    resetFormBtn.addEventListener('click', () => resetForm());
    searchNamaInput.addEventListener('input', () => applyFilters());
    searchSupplierInput.addEventListener('input', () => applyFilters());
    searchKategoriSelect.addEventListener('change', () => applyFilters());
    clearSearchBtn.addEventListener('click', () => resetSearchFilters());

    printBtn.addEventListener('click', () => {
        const printContents = document.getElementById('dataTable').cloneNode(true);
        const headerRow = printContents.querySelector('thead tr');
        if (headerRow && headerRow.children.length >= 8) {
            headerRow.children[7].textContent = '';
        }
        printContents.querySelectorAll('tbody tr').forEach(row => {
            const lastCell = row.querySelector('td:last-child');
            if (lastCell) lastCell.textContent = '';
        });
        const win = window.open();
        win.document.write('<html><head><title>Laporan Barang Masuk dari Supplier</title><style>body{font-family:sans-serif;margin:20px;}table{width:100%;border-collapse:collapse;}th,td{border:1px solid #ccc;padding:8px;text-align:left;}th{background:#f2f2f2;}</style></head><body>');
        win.document.write('<h2>Laporan Penerimaan Barang dari Supplier</h2>');
        win.document.write(printContents.outerHTML);
        win.document.write('</body></html>');
        win.document.close();
        win.print();
    });

    downloadCsvBtn.addEventListener('click', () => {
        const dataToExport = filteredItems.length ? filteredItems : incomingItems;
        if (dataToExport.length === 0) { alert("Tidak ada data untuk diekspor."); return; }
        const headers = ['Supplier', 'Nama Barang', 'Kategori', 'Jumlah', 'Unit', 'Tanggal Masuk', 'Catatan Supplier'];
        const rows = dataToExport.map(item => [
            `"${(item.supplier || '').replace(/"/g, '""')}"`,
            `"${(item.namaBarang || '').replace(/"/g, '""')}"`,
            `"${(item.kategori || '').replace(/"/g, '""')}"`,
            item.jumlah,
            `"${(item.unit || '').replace(/"/g, '""')}"`,
            `"${item.tanggalMasuk || ''}"`,
            `"${(item.catatanSupplier || '').replace(/"/g, '""')}"`
        ]);
        const csvContent = [headers.join(','), ...rows.map(r => r.join(','))].join('\n');
        const blob = new Blob(["\uFEFF" + csvContent], { type: 'text/csv;charset=utf-8;' });
        const link = document.createElement('a');
        const url = URL.createObjectURL(blob);
        link.href = url;
        link.setAttribute('download', 'barang_masuk_supplier.csv');
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        URL.revokeObjectURL(url);
    });

    setDefaultDate();
    loadFromStorage();
</script>
</body>
</html>
