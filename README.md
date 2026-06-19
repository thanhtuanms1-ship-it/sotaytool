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
