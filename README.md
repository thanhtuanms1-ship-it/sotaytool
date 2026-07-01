<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Quy đổi NVL ME</title>
    <style>
        :root {
            --primary-color: #007bff; --bg-color: #f8f9fa; --card-bg: #ffffff;
            --text-color: #333333; --border-color: #cccccc; --danger-color: #dc3545;
            --success-color: #28a745; --warning-color: #ffc107;
        }
        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; user-select: none; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif;
            margin: 0; padding: 0; background-color: var(--bg-color); color: var(--text-color);
            overflow: hidden; height: 100vh; display: flex; flex-direction: column;
        }
        .app-container { display: flex; flex-direction: column; height: 100%; width: 100%; position: relative; }
        
        header {
            background-color: var(--primary-color); color: white; padding: 10px 15px;
            display: flex; flex-direction: column; flex-shrink: 0;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .header-top { 
            display: flex; justify-content: flex-start; align-items: center; width: 100%; height: 26px;
        }
        .back-btn {
            background: none; border: none; color: white; font-size: 0.95rem; display: none; 
            padding: 5px 0; font-weight: bold; cursor: pointer; text-align: left;
        }
        .back-btn:active { opacity: 0.6; }
        .header-title {
            text-align: center; font-weight: bold; font-size: 1.25rem;
            letter-spacing: 0.5px; width: 100%; margin: 4px 0 2px 0;
        }
        
        .content { flex: 1; overflow-y: auto; padding: 15px; -webkit-overflow-scrolling: touch; }
        input, select, button { font-size: 16px !important; padding: 12px; margin-bottom: 12px; border: 1px solid var(--border-color); border-radius: 8px; width: 100%; outline: none; user-select: text; }
        input[type="text"] { text-transform: uppercase; }
        button { background-color: var(--primary-color); color: white; border: none; font-weight: bold; cursor: pointer; }
        button:active { opacity: 0.8; }
        .page { display: none; height: 100%; }
        .page.active { display: block; }
        .menu-grid { display: flex; flex-direction: column; gap: 15px; margin-top: 20px; }
        .menu-btn { padding: 20px; font-size: 1.1rem; background: var(--card-bg); color: var(--primary-color); border: 2px solid var(--primary-color); border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); }
        .tabs { display: flex; border-bottom: 2px solid var(--border-color); margin-bottom: 15px; }
        .tab { flex: 1; text-align: center; padding: 12px; font-weight: bold; color: #666; border-bottom: 3px solid transparent; margin-bottom: -2px; }
        .tab.active { color: var(--primary-color); border-bottom-color: var(--primary-color); }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .form-sticky { position: sticky; top: 0; background: var(--bg-color); z-index: 10; padding-bottom: 5px; }
        .table-container { overflow-x: auto; margin-top: 15px; background: white; border-radius: 8px; border: 1px solid #eee; }
        table { width: 100%; border-collapse: collapse; text-align: left; }
        th, td { padding: 10px; border-bottom: 1px solid #eee; font-size: 13px; }
        th { background-color: #f1f3f5; font-weight: bold; }
        .history-title-box { display: flex; justify-content: space-between; align-items: center; margin-top: 25px; padding: 0 5px; }
        .history-title { font-size: 1rem; font-weight: bold; color: #555; margin: 0; }
        .btn-clear-hist { width: auto; margin: 0; padding: 6px 12px; font-size: 12px !important; background-color: #6c757d; }
        .btn-danger { background-color: var(--danger-color); }
        .btn-warning { background-color: var(--warning-color); color: black; }
        .btn-success { background-color: var(--success-color); }
        .result-box { background: #e7f5ff; border-left: 4px solid var(--primary-color); padding: 15px; border-radius: 4px; margin-top: 15px; font-weight: bold; font-size: 1.1rem; }
        .suggestions { position: absolute; background: white; border: 1px solid var(--border-color); border-radius: 4px; max-height: 150px; overflow-y: auto; width: calc(100% - 30px); z-index: 1000; box-shadow: 0 4px 6px rgba(0,0,0,0.1); display: none; }
        .suggestion-item { padding: 10px; border-bottom: 1px solid #eee; cursor: pointer; }
        .suggestion-item:active { background-color: #f1f3f5; }
        .backup-section { margin-top: 30px; padding: 15px; border-top: 1px dashed var(--border-color); display: flex; gap: 10px; }
    </style>
</head>
<body>
<div class="app-container" onclick="resetDeleteButtons(event)">
    <header>
        <div class="header-top">
            <button class="back-btn" id="backBtn" onclick="goHome()">❮ TRANG CHỦ</button>
        </div>
        <div class="header-title" id="appTitle">QUY ĐỔI NVL ME</div>
    </header>
    <div class="content" id="appContent" onscroll="handleScroll(event)">
        <!-- TRANG CHỦ -->
        <div id="pageHome" class="page active">
            <div class="menu-grid">
                <button class="menu-btn" onclick="navigate('Seal')">QUY ĐỔI SEAL</button>
                <button class="menu-btn" onclick="navigate('TML')">QUY ĐỔI TM'L</button>
            </div>
            <div class="backup-section">
                <button class="btn-success" onclick="exportData()">💾 Sao Lưu</button>
                <button class="btn-warning" onclick="triggerImport()">📂 Phục Hồi</button>
                <input type="file" id="fileInput" accept=".json" style="display:none" onchange="importData(event)">
            </div>
        </div>
        <!-- TRANG QUY ĐỔI SEAL -->
        <div id="pageSeal" class="page">
            <div class="tabs">
                <div class="tab active" onclick="switchTab('Seal', 'calc', event)">Quy Đổi</div>
                <div class="tab" onclick="switchTab('Seal', 'data', event)">Dữ Liệu</div>
            </div>
            <!-- Tab Quy đổi Seal -->
            <div id="tabSealCalc" class="tab-content active">
                <div class="form-sticky">
                    <div style="position:relative;">
                        <input type="text" id="sealCalcName" placeholder="NHẬP TÊN SEAL" oninput="showSuggestions(this, 'seal')" autocomplete="off">
                        <div id="sealSuggestions" class="suggestions"></div>
                    </div>
                    <input type="number" id="sealCalcWeight" placeholder="CÂN NẶNG TOÀN BỘ SEAL" inputmode="decimal">
                    <button onclick="calculateSeal()">BẮT ĐẦU QUY ĐỔI</button>
                    <div id="sealResult" class="result-box" style="display:none;"></div>
                </div>
                <div class="history-title-box">
                    <p class="history-title">Lịch sử quy đổi</p>
                    <button class="btn-clear-hist" onclick="clearHistory('seal', event)">Xóa lịch sử</button>
                </div>
                <div class="table-container">
                    <table>
                        <thead><tr><th>Giờ</th><th>Tên</th><th>Cân nặng</th><th>Số lượng</th></tr></thead>
                        <tbody id="sealHistoryBody"></tbody>
                    </table>
                </div>
            </div>
            <!-- Tab Dữ liệu Seal -->
            <div id="tabSealData" class="tab-content">
                <input type="text" id="sealDataName" placeholder="TÊN SEAL">
                <input type="number" id="sealDataQty" placeholder="SỐ LƯỢNG" inputmode="numeric">
                <input type="number" id="sealDataWeight" placeholder="CÂN NẶNG TỔNG" inputmode="decimal">
                <button class="btn-success" onclick="saveSealData()">LƯU DỮ LIỆU</button>
                <div class="table-container">
                    <table>
                        <thead><tr><th>Tên Seal</th><th>Trọng lượng/Con</th><th style="width: 80px;">Xóa</th></tr></thead>
                        <tbody id="sealTableBody"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- TRANG QUY ĐỔI TM'L -->
        <div id="pageTML" class="page">
            <div class="tabs">
                <div class="tab active" onclick="switchTab('TML', 'calc', event)">Quy Đổi</div>
                <div class="tab" onclick="switchTab('TML', 'data', event)">Dữ Liệu</div>
            </div>
            <!-- Tab Quy đổi TM'L -->
            <div id="tabTMLCalc" class="tab-content active">
                <div class="form-sticky">
                    <div style="position:relative;">
                        <input type="text" id="tmlCalcName" placeholder="NHẬP TÊN TM'L" oninput="showSuggestions(this, 'tml')" autocomplete="off">
                        <div id="tmlSuggestions" class="suggestions"></div>
                    </div>
                    <input type="number" id="tmlCalcWeight" placeholder="TRỌNG LƯỢNG TỔNG CÂN ĐƯỢC" inputmode="decimal">
                    <input type="number" id="tmlCalcRolls" placeholder="SỐ CUỘN HIỆN TẠI" inputmode="numeric">
                    <button onclick="calculateTML()">BẮT ĐẦU QUY ĐỔI</button>
                    <div id="tmlResult" class="result-box" style="display:none;"></div>
                </div>
                <div class="history-title-box">
                    <p class="history-title">Lịch sử quy đổi</p>
                    <button class="btn-clear-hist" onclick="clearHistory('tml', event)">Xóa lịch sử</button>
                </div>
                <div class="table-container">
                    <table>
                        <thead><tr><th>Giờ</th><th>Tên</th><th>Cân nặng</th><th>Số lượng</th></tr></thead>
                        <tbody id="tmlHistoryBody"></tbody>
                    </table>
                </div>
            </div>
            <!-- Tab Dữ liệu TM'L -->
            <div id="tabTMLData" class="tab-content">
                <input type="text" id="tmlDataName" placeholder="TÊN TM'L">
                <input type="number" id="tmlDataShell" placeholder="CÂN NẶNG CỦA VỎ 1 CUỘN" inputmode="decimal">
                <input type="number" id="tmlDataNewWeight" placeholder="CÂN NẶNG CUỘN MỚI NGUYÊN" inputmode="decimal">
                <input type="number" id="tmlDataQty" placeholder="SỐ LƯỢNG TRONG CUỘN MỚI" inputmode="numeric">
                <button class="btn-success" onclick="saveTMLData()">LƯU DỮ LIỆU</button>
                <div class="table-container">
                    <table>
                        <thead><tr><th>Tên TM'L</th><th>Trọng lượng/Con</th><th style="width: 80px;">Xóa</th></tr></thead>
                        <tbody id="tmlTableBody"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</div>
<script>
    let db = { seal: {}, tml: {}, historySeal: [], historyTML: [] };
    if(localStorage.getItem('nvl_me_db_v2')) {
        db = JSON.parse(localStorage.getItem('nvl_me_db_v2'));
    }

    let startY = 0;
    const contentArea = document.getElementById('appContent');
    contentArea.addEventListener('touchstart', function(e) { startY = e.touches.pageY; }, { passive: true });
    contentArea.addEventListener('touchmove', function(e) {
        const moveY = e.touches.pageY;
        if (contentArea.scrollTop <= 0 && moveY > startY) { if(e.cancelable) e.preventDefault(); }
    }, { passive: false });
    function handleScroll(e) { if(contentArea.scrollTop < 0) contentArea.scrollTop = 0; }

    function navigate(pageName) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById('page' + pageName).classList.add('active');
        document.getElementById('backBtn').style.display = 'block';
        document.getElementById('appTitle').innerText = 'QUY ĐỔI ' + pageName.toUpperCase();
        contentArea.scrollTop = 0;
        if(pageName === 'Seal') { renderSealTable(); renderHistory('seal'); }
        if(pageName === 'TML') { renderTMLTable(); renderHistory('tml'); }
    }

    function goHome() {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById('pageHome').classList.add('active');
        document.getElementById('backBtn').style.display = 'none';
        document.getElementById('appTitle').innerText = 'QUY ĐỔI NVL ME';
        contentArea.scrollTop = 0;
    }

    function switchTab(page, tabType, evt) {
        const pageEl = document.getElementById('page' + page);
        pageEl.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        pageEl.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
        evt.target.classList.add('active');
        document.getElementById('tab' + page + (tabType === 'calc' ? 'Calc' : 'Data')).classList.add('active');
        contentArea.scrollTop = 0;
    }

    document.querySelectorAll('input[type="text"]').forEach(input => {
        input.addEventListener('input', function() { this.value = this.value.toUpperCase(); });
    });

    function getShortTime() {
        const now = new Date();
        return String(now.getHours()).padStart(2, '0') + ':' + String(now.getMinutes()).padStart(2, '0');
    }

    // Kiểm tra trùng lặp và lưu Seal
    function saveSealData() {
        const name = document.getElementById('sealDataName').value.trim().toUpperCase();
        const qty = parseFloat(document.getElementById('sealDataQty').value);
        const weight = parseFloat(document.getElementById('sealDataWeight').value);
        if(!name || !qty || !weight) { alert('Vui lòng nhập đủ thông tin!'); return; }
        
        // Chặn trùng lặp tên
        if(db.seal[name]) { alert('LỖI: Tên Seal này đã tồn tại trong danh mục dữ liệu!'); return; }
        
        db.seal[name] = { unitWeight: weight / qty, qty, weight };
        saveDB(); renderSealTable();
        document.getElementById('sealDataName').value = ''; document.getElementById('sealDataQty').value = ''; document.getElementById('sealDataWeight').value = '';
    }

    // Sắp xếp bảng Seal số trước chữ sau
    function renderSealTable() {
        const tbody = document.getElementById('sealTableBody'); tbody.innerHTML = '';
        const sortedKeys = Object.keys(db.seal).sort((a, b) => a.localeCompare(b, undefined, { numeric: true, sensitivity: 'base' }));
        
        sortedKeys.forEach(name => {
            tbody.innerHTML += `<tr><td>${name}</td><td>${db.seal[name].unitWeight.toFixed(4)}</td>
            <td><button class="btn-danger" style="margin:0; padding:6px;" data-confirm="false" onclick="deleteItem(event, 'seal', '${name}')">Xóa</button></td></tr>`;
        });
    }

    function calculateSeal() {
        const name = document.getElementById('sealCalcName').value.trim().toUpperCase();
        const totalWeight = parseFloat(document.getElementById('sealCalcWeight').value);
        const resBox = document.getElementById('sealResult');
        if(!name || !totalWeight) { alert('Vui lòng nhập đủ thông tin!'); return; }
        if(!db.seal[name]) { alert('Không có dữ liệu loại Seal này!'); return; }
        const finalQty = Math.round(totalWeight / db.seal[name].unitWeight);
        resBox.style.display = 'block';
        resBox.innerHTML = `Tổng số Seal hiện có: <span style="color:var(--danger-color); font-size:1.4rem;">${finalQty}</span> con`;
        if(!db.historySeal) db.historySeal = [];
        db.historySeal.unshift({ time: getShortTime(), name, weight: totalWeight, qty: finalQty });
        saveDB(); renderHistory('seal');
    }
    // Kiểm tra trùng lặp và lưu TM'L
    function saveTMLData() {
        const name = document.getElementById('tmlDataName').value.trim().toUpperCase();
        const shell = parseFloat(document.getElementById('tmlDataShell').value);
        const newWeight = parseFloat(document.getElementById('tmlDataNewWeight').value);
        const qty = parseFloat(document.getElementById('tmlDataQty').value);
        if(!name || isNaN(shell) || !newWeight || !qty) { alert('Vui lòng điền đủ thông tin!'); return; }
        
        // Chặn trùng lặp tên
        if(db.tml[name]) { alert('LỖI: Tên TM\'L này đã tồn tại trong danh mục dữ liệu!'); return; }
        
        db.tml[name] = { unitWeight: (newWeight - shell) / qty, shell, newWeight, qty };
        saveDB(); renderTMLTable();
        document.getElementById('tmlDataName').value = ''; document.getElementById('tmlDataShell').value = ''; document.getElementById('tmlDataNewWeight').value = ''; document.getElementById('tmlDataQty').value = '';
    }

    // Sắp xếp bảng TM'L số trước chữ sau
    function renderTMLTable() {
        const tbody = document.getElementById('tmlTableBody'); tbody.innerHTML = '';
        const sortedKeys = Object.keys(db.tml).sort((a, b) => a.localeCompare(b, undefined, { numeric: true, sensitivity: 'base' }));
        
        sortedKeys.forEach(name => {
            tbody.innerHTML += `<tr><td>${name}</td><td>${db.tml[name].unitWeight.toFixed(4)}</td>
            <td><button class="btn-danger" style="margin:0; padding:6px;" data-confirm="false" onclick="deleteItem(event, 'tml', '${name}')">Xóa</button></td></tr>`;
        });
    }

    function calculateTML() {
        const name = document.getElementById('tmlCalcName').value.trim().toUpperCase();
        const totalWeight = parseFloat(document.getElementById('tmlCalcWeight').value);
        const rolls = parseInt(document.getElementById('tmlCalcRolls').value);
        const resBox = document.getElementById('tmlResult');
        if(!name || !totalWeight || !rolls) { alert('Vui lòng điền đủ thông tin!'); return; }
        if(!db.tml[name]) { alert('Dữ liệu loại TM\'L này chưa có!'); return; }
        const insideWeight = totalWeight - (db.tml[name].shell * rolls);
        if(insideWeight <= 0) { alert('Trọng lượng tổng nhỏ hơn trọng lượng vỏ cuộn!'); return; }
        const finalQty = Math.round(insideWeight / db.tml[name].unitWeight);
        resBox.style.display = 'block';
        resBox.innerHTML = `Tổng số TM'L hiện có: <span style="color:var(--danger-color); font-size:1.4rem;">${finalQty}</span> con`;
        if(!db.historyTML) db.historyTML = [];
        db.historyTML.unshift({ time: getShortTime(), name, weight: totalWeight, qty: finalQty });
        saveDB(); renderHistory('tml');
    }

    function renderHistory(type) {
        const tbody = document.getElementById(type === 'seal' ? 'sealHistoryBody' : 'tmlHistoryBody');
        const histData = type === 'seal' ? (db.historySeal || []) : (db.historyTML || []);
        tbody.innerHTML = '';
        histData.forEach(item => {
            tbody.innerHTML += `<tr><td>${item.time}</td><td>${item.name}</td><td>${item.weight}</td><td style="font-weight:bold; color:var(--primary-color)">${item.qty}</td></tr>`;
        });
    }

    function clearHistory(type, e) {
        if (e) e.stopPropagation();
        const btn = document.querySelector(`#page${type === 'seal' ? 'Seal' : 'TML'} .btn-clear-hist`);
        if (btn.getAttribute('data-confirm-hist') !== 'true') {
            resetDeleteButtons();
            btn.setAttribute('data-confirm-hist', 'true');
            btn.innerText = 'XÁC NHẬN XÓA?';
            btn.style.backgroundColor = 'var(--warning-color)';
            btn.style.color = 'black';
            currentActiveDeleteBtn = btn;
        } else {
            if (type === 'seal') db.historySeal = []; else db.historyTML = [];
            saveDB(); renderHistory(type);
            btn.setAttribute('data-confirm-hist', 'false');
            btn.innerText = 'Xóa lịch sử';
            btn.style.backgroundColor = '#6c757d';
            btn.style.color = 'white';
            currentActiveDeleteBtn = null;
        }
    }

    let currentActiveDeleteBtn = null;
    function deleteItem(e, type, name) {
        if (e) e.stopPropagation(); const btn = e.target;
        if (btn.getAttribute('data-confirm') === 'false') {
            resetDeleteButtons(); btn.setAttribute('data-confirm', 'true');
            btn.innerText = 'XÁC?'; btn.style.backgroundColor = 'var(--warning-color)'; btn.style.color = 'black';
            currentActiveDeleteBtn = btn;
        } else {
            delete db[type][name]; saveDB();
            type === 'seal' ? renderSealTable() : renderTMLTable();
            currentActiveDeleteBtn = null;
        }
    }

    function resetDeleteButtons(e) {
        if(currentActiveDeleteBtn) {
            currentActiveDeleteBtn.setAttribute('data-confirm', 'false');
            currentActiveDeleteBtn.setAttribute('data-confirm-hist', 'false');
            if (currentActiveDeleteBtn.classList.contains('btn-clear-hist')) {
                currentActiveDeleteBtn.innerText = 'Xóa lịch sử';
                currentActiveDeleteBtn.style.backgroundColor = '#6c757d';
                currentActiveDeleteBtn.style.color = 'white';
            } else {
                currentActiveDeleteBtn.innerText = 'Xóa';
                currentActiveDeleteBtn.style.backgroundColor = 'var(--danger-color)';
                currentActiveDeleteBtn.style.color = 'white';
            }
            currentActiveDeleteBtn = null;
        }
        document.getElementById('sealSuggestions').style.display = 'none';
        document.getElementById('tmlSuggestions').style.display = 'none';
    }

    function showSuggestions(input, type) {
        const keyword = input.value.trim().toUpperCase();
        const sugBox = document.getElementById(type + 'Suggestions'); sugBox.innerHTML = '';
        if(!keyword) { sugBox.style.display = 'none'; return; }
        const listKeys = Object.keys(db[type]).filter(key => key.includes(keyword));
        if(listKeys.length > 0) {
            sugBox.style.display = 'block';
            listKeys.forEach(key => {
                const div = document.createElement('div'); div.className = 'suggestion-item'; div.innerText = key;
                div.onclick = function(e) { e.stopPropagation(); input.value = key; sugBox.style.display = 'none'; };
                sugBox.appendChild(div);
            });
        } else { sugBox.style.display = 'none'; }
    }

    function saveDB() { localStorage.setItem('nvl_me_db_v2', JSON.stringify(db)); }
    
    // QUAY LẠI CÁCH SAO LƯU KIỂU CŨ THEO YÊU CẦU
    function exportData() {
        const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(db));
        const a = document.createElement('a'); a.setAttribute("href", dataStr);
        a.setAttribute("download", "NVL_ME_Backup_" + new Date().toISOString().slice(0,10) + ".json");
        document.body.appendChild(a); a.click(); a.remove();
    }
    
    function triggerImport() { document.getElementById('fileInput').click(); }
    function importData(e) {
        const file = e.target.files; if (!file) return;
        const reader = new FileReader();
        reader.onload = function(evt) {
            try {
                const importedData = JSON.parse(evt.target.result);
                if(importedData.seal && importedData.tml) { db = importedData; saveDB(); alert('Thành công!'); location.reload(); }
                else { alert('File sai cấu trúc!'); }
            } catch (err) { alert('Lỗi đọc file.'); }
        };
        reader.readAsText(file);
    }
</script>
</body>
</html>
