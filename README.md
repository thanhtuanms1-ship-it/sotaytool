<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <!-- Thêm thuộc tính chặn thu phóng tuyệt đối trên mọi dòng điện thoại -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>QUẢN LÝ DAO</title>
    <style>
        :root { --p: #1e3a8a; --s: #0d9488; --d: #ef4444; --a: #f59e0b; --b: #7c3aed; --bg: #f1f5f9; --txt: #0f172a; }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, sans-serif; touch-action: manipulation; }
        body { background: var(--bg); color: var(--txt); padding: 10px; display: flex; flex-direction: column; align-items: center; min-height: 100vh; }
        .container { width: 100%; max-width: 430px; background: #fff; padding: 14px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.08); }
        h1, h2 { text-align: center; margin-bottom: 16px; color: var(--p); font-size: 1.3rem; font-weight: 700; }
        .menu-grid { display: flex; flex-direction: column; gap: 12px; }
        .btn { width: 100%; padding: 14px; font-size: 1rem; font-weight: bold; border: none; border-radius: 8px; cursor: pointer; text-align: center; }
        .btn-p { background: var(--p); color: white; }
        .btn-s { background: var(--s); color: white; }
        .btn-d { background: var(--d); color: white; padding: 6px 10px; font-size: 0.85rem; border-radius: 6px; width: auto; }
        .btn-back { background: #475569; color: white; margin-bottom: 14px; padding: 10px; font-size: 0.9rem; }
        .backup-section { margin-top: 15px; padding: 12px; border: 1px dashed var(--b); background: #f5f3ff; border-radius: 8px; }
        .btn-b { background: var(--b); color: white; margin-bottom: 8px; font-size: 0.9rem; padding: 12px; }
        .btn-r { background: #64748b; color: white; font-size: 0.9rem; padding: 12px; display: block; text-align: center; cursor: pointer; border-radius: 8px; }
        .form-group { margin-bottom: 12px; }
        .form-group label { display: block; margin-bottom: 5px; font-weight: 600; font-size: 0.9rem; }
        .form-control { width: 100%; padding: 11px; border: 1px solid #cbd5e1; border-radius: 6px; font-size: 1rem; }
        .file-box { margin-bottom: 12px; padding: 12px; border: 2px dashed #cbd5e1; border-radius: 6px; text-align: center; background: #f8fafc; }
        .res-box { background: #f0fdf4; border-left: 4px solid var(--s); padding: 12px; margin: 12px 0; border-radius: 4px; }
        .sec-title { margin-top: 12px; font-size: 0.9rem; color: var(--a); font-weight: bold; border-bottom: 1px solid #e2e8f0; padding-bottom: 4px; }
        .tbl-box { margin-top: 12px; overflow-x: auto; width: 100%; border-radius: 6px; border: 1px solid #e2e8f0; }
        table { width: 100%; border-collapse: collapse; font-size: 0.85rem; text-align: left; }
        th, td { padding: 9px 8px; border-bottom: 1px solid #e2e8f0; white-space: nowrap; }
        th { background: #f8fafc; color: var(--p); font-weight: 600; }
        .page { display: none; width: 100%; }
        .page.active { display: block; }
        /* CSS cho thanh tìm kiếm lịch sử */
        .search-hist-box { background: #f8fafc; border: 1px solid #e2e8f0; padding: 10px; border-radius: 8px; margin-top: 15px; margin-bottom: 5px; }
    </style>
</head>
<body>
    <div class="container">
        <!-- TRANG CHỦ -->
        <div id="page-home" class="page active">
            <h1>QUẢN LÝ DAO OFFLINE</h1>
            <div class="menu-grid">
                <button class="btn btn-p" onclick="switchPage('page-param')">TRA THÔNG SỐ DAO</button>
                <button class="btn btn-s" onclick="switchPage('page-history')">LÝ LỊCH CHẾ DAO</button>
                <div class="backup-section">
                    <p style="font-weight:bold;color:var(--b);margin-bottom:8px;text-align:center;font-size:0.85rem;">SAO LƯU NỘI BỘ</p>
                    <button class="btn btn-b" onclick="exportBackup()">📥 XUẤT FILE SAO LƯU (.JSON)</button>
                    <label class="btn-r">📤 KHÔI PHỤC DỮ LIỆU<input type="file" id="restore-file" accept=".json" onchange="importBackup(this)" style="display:none;"></label>
                </div>
            </div>
        </div>

        <!-- TRANG TRA THÔNG SỐ -->
        <div id="page-param" class="page">
            <button class="btn btn-back" onclick="switchPage('page-home')">← Quay lại</button>
            <h2>TRA THÔNG SỐ DAO</h2>
            <div class="file-box">
                <label style="font-weight:bold;display:block;margin-bottom:8px;font-size:0.85rem;color:var(--p);">NẠP BỘ NHỚ TỪ FILE TXT (XÓA & GHI ĐÈ)</label>
                <input type="file" id="txt-file" accept=".txt" onchange="importTxtFile(this)" class="form-control" style="padding:6px;font-size:0.85rem;">
                <div id="excel-status" style="margin-top:6px;font-weight:bold;color:var(--s);font-size:0.85rem;"></div>
            </div>
            <div class="form-group">
                <label>Nhập tên dao cần tra:</label>
                <input type="text" id="search-input" class="form-control" placeholder="Gõ từ khóa tên dao..." oninput="searchKnife()">
            </div>
            <div id="search-result" style="display:none;">
                <div class="res-box">
                    <p><strong>Tên dao:</strong> <span id="res-name"></span></p>
                    <p style="margin-top:4px;"><strong>CCW:</strong> <span id="res-ccw" style="font-size:1.1rem;color:var(--p);font-weight:bold;"></span></p>
                </div>
                <div class="sec-title">DAO TƯƠNG ĐỒNG (CCW LECH ≤ 0.05)</div>
                <div class="tbl-box">
                    <table id="suggest-table">
                        <thead><tr><th>Tên dao</th><th>CCW</th><th>Độ lệch</th></tr></thead>
                        <tbody></tbody>
                    </table>
                </div>
            </div>
            <div id="search-empty" style="text-align:center;color:#64748b;margin-top:12px;font-size:0.9rem;">Chưa có dữ liệu.</div>
        </div>

        <!-- TRANG LÝ LỊCH CHẾ DAO -->
        <div id="page-history" class="page">
            <button class="btn btn-back" onclick="switchPage('page-home')">← Quay lại</button>
            <h2>LÝ LỊCH CHẾ DAO</h2>
            <form id="history-form" onsubmit="saveHistory(event)">
                <div class="form-group"><label>Dao gốc</label><input type="text" id="input-original" class="form-control" required></div>
                <div class="form-group"><label>Dao thay thế</label><input type="text" id="input-replace" class="form-control" required></div>
                <div class="form-group"><label>Ngày duyệt</label><input type="date" id="input-date" class="form-control" required></div>
                <div class="form-group">
                    <label>Loại Tool</label>
                    <select id="input-type" class="form-control" required>
                        <option value="" disabled selected>Chọn loại tool</option>
                        <option value="A">A</option><option value="B">B</option><option value="C">C</option><option value="D">D</option>
                    </select>
                </div>
                <button type="submit" class="btn btn-s">LƯU LÝ LỊCH</button>
            </form>

            <!-- Thanh tìm kiếm lý lịch mới thêm -->
            <div class="search-hist-box">
                <div class="form-group" style="margin-bottom:0;">
                    <label style="color:var(--p);font-size:0.85rem;">🔍 TÌM KIẾM TRONG LÝ LỊCH:</label>
                    <input type="text" id="search-history-input" class="form-control" style="padding:8px;" placeholder="Nhập mã dao gốc hoặc thay thế để lọc..." oninput="renderHistory()">
                </div>
            </div>

            <div class="tbl-box">
                <table id="history-table">
                    <thead><tr><th>Ngày</th><th>Gốc</th><th>Thay</th><th>Loại</th><th>Xóa</th></tr></thead>
                    <tbody></tbody>
                </table>
            </div>
        </div>
    </div>
    <script>
        let dbKnives = JSON.parse(localStorage.getItem('dbKnives')) || [];
        let historyList = JSON.parse(localStorage.getItem('historyList')) || [];

        window.onload = function() { document.getElementById('input-date').valueAsDate = new Date(); updateStatus(); renderHistory(); };
        function updateStatus() { const div = document.getElementById('excel-status'); if (div) div.innerText = dbKnives.length > 0 ? `Đã lưu ${dbKnives.length} dao.` : `Chưa có dữ liệu.`; }
        function switchPage(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); if(id === 'page-home') updateStatus(); }

        function importTxtFile(el) {
            if (!el.files || el.files.length === 0) return;
            const r = new FileReader(); r.onload = function(e) {
                let txt = e.target.result; if (txt.charCodeAt(0) === 0xFEFF) txt = txt.substr(1);
                const lines = txt.split(/\r?\n/); let tmp = [];
                lines.forEach(l => {
                    if (!l.trim()) return; let cols = l.split(/[\t,;]/);
                    if (cols.length >= 2) {
                        let name = cols[0].trim(), ccw = parseFloat(cols[1].trim().replace(',', '.')) || 0;
                        if (name) tmp.push({ name, ccw });
                    }
                });
                if (tmp.length === 0) alert("Cấu trúc file không đúng!");
                else { dbKnives = tmp; localStorage.setItem('dbKnives', JSON.stringify(dbKnives)); updateStatus(); alert(`Đã ghi đè nạp mới thành công ${dbKnives.length} dao!`); searchKnife(); }
                el.value = '';
            }; r.readAsText(el.files, 'UTF-8');
        }

        function searchKnife() {
            const kw = document.getElementById('search-input').value.trim().toLowerCase();
            const res = document.getElementById('search-result'), emp = document.getElementById('search-empty'), tb = document.querySelector('#suggest-table tbody');
            if (!kw) { res.style.display = 'none'; emp.style.display = 'block'; return; }
            const tgt = dbKnives.find(k => k.name.toLowerCase().includes(kw));
            if (!tgt) { res.style.display = 'none'; emp.style.display = 'block'; emp.innerText = 'Không tìm thấy dao.'; return; }
            emp.style.display = 'none'; res.style.display = 'block'; document.getElementById('res-name').innerText = tgt.name; document.getElementById('res-ccw').innerText = tgt.ccw; tb.innerHTML = '';
            dbKnives.forEach(k => {
                const diff = Math.abs(k.ccw - tgt.ccw);
                if (diff <= 0.05 && k.name !== tgt.name) {
                    const sign = k.ccw > tgt.ccw ? `+${diff.toFixed(3)}` : (k.ccw < tgt.ccw ? `-${diff.toFixed(3)}` : 'Bằng');
                    tb.innerHTML += `<tr><td><strong>${k.name}</strong></td><td>${k.ccw}</td><td style="color:${diff===0?'green':'#b45309'}">${sign}</td></tr>`;
                }
            });
            if (!tb.innerHTML) tb.innerHTML = `<tr><td colspan="3" style="text-align:center;color:#64748b;">Không có dao lệch khoảng ±0.05</td></tr>`;
        }

        function fmtDate(s) { if(!s) return ''; const d = new Date(s); return `${String(d.getDate()).padStart(2,'0')}/${String(d.getMonth()+1).padStart(2,'0')}/${d.getFullYear()}`; }
        function saveHistory(e) {
            e.preventDefault();
            historyList.push({ id: Date.now(), original: document.getElementById('input-original').value.trim(), replace: document.getElementById('input-replace').value.trim(), date: document.getElementById('input-date').value, type: document.getElementById('input-type').value });
            localStorage.setItem('historyList', JSON.stringify(historyList)); renderHistory(); document.getElementById('history-form').reset(); document.getElementById('input-date').valueAsDate = new Date();
        }

        // HÀM HIỂN THỊ CÓ TÍCH HỢP BỘ LỌC TÌM KIẾM LÝ LỊCH LẬP TỨC
        function renderHistory() {
            const tb = document.querySelector('#history-table tbody'); if(!tb) return; tb.innerHTML = '';
            // Lấy từ khóa tìm kiếm lịch sử
            const kw = document.getElementById('search-history-input') ? document.getElementById('search-history-input').value.trim().toLowerCase() : '';
            
            historyList.forEach(i => {
                // Kiểm tra điều kiện lọc theo dao gốc hoặc dao thay thế
                if (!kw || i.original.toLowerCase().includes(kw) || i.replace.toLowerCase().includes(kw)) {
                    tb.innerHTML += `<tr><td>${fmtDate(i.date)}</td><td>${i.original}</td><td>${i.replace}</td><td><span style="font-weight:bold;color:var(--p)">${i.type}</span></td><td><button class="btn btn-d" onclick="delItem(${i.id})">Xóa</button></td></tr>`;
                }
            });
        }
        function delItem(id) { if (confirm("Xác nhận xóa dòng lý lịch này?")) { historyList = historyList.filter(i => i.id !== id); localStorage.setItem('historyList', JSON.stringify(historyList)); renderHistory(); } }

        function exportBackup() {
            if (dbKnives.length === 0 && historyList.length === 0) { alert("Trống dữ liệu!"); return; }
            const b = new Blob([JSON.stringify({ dbKnives, historyList })], {type: 'application/json'});
            const a = document.createElement('a'); a.href = URL.createObjectURL(b); a.download = `saoluu_dao.json`; document.body.appendChild(a); a.click(); document.body.removeChild(a);
        }
        function importBackup(el) {
            if (!el.files || el.files.length === 0) return;
            const r = new FileReader(); r.onload = function(e) {
                try {
                    const d = JSON.parse(e.target.result);
                    if (d.dbKnives || d.historyList) { dbKnives = d.dbKnives || []; historyList = d.historyList || []; localStorage.setItem('dbKnives', JSON.stringify(dbKnives)); localStorage.setItem('historyList', JSON.stringify(historyList)); updateStatus(); renderHistory(); alert("Khôi phục thành công!"); }
                } catch (err) { alert("Lỗi đọc file JSON."); }
                el.value = '';
            }; r.readAsText(el.files);
        }
    </script>
</body>
</html>
