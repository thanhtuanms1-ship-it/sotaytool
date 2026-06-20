<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Quản lý ME</title>
    <style>
        :root { --bg: #f4f6f9; --p: #2563eb; --s: #16a34a; --d: #dc2626; --txt: #1f2937; }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: system-ui, sans-serif; font-size: 14px; }
        body { background: var(--bg); color: var(--txt); padding-bottom: 20px; width: 100vw; overflow-x: hidden; }
        .h { background: var(--p); color: #fff; padding: 12px; text-align: center; font-size: 18px; font-weight: bold; position: sticky; top: 0; z-index: 99; }
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; padding: 10px; }
        .m-btn { background: #fff; border: 1px solid #e5e7eb; padding: 15px 5px; border-radius: 10px; font-weight: bold; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 5px; }
        .m-btn svg { width: 24px; height: 24px; fill: var(--p); }
        .page { display: none; padding: 10px; }
        .page.active { display: block; }
        .b-btn { background: #4b5563; color: #fff; border: none; padding: 6px 12px; border-radius: 6px; margin-bottom: 10px; font-weight: bold; }
        .card { background: #fff; padding: 12px; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); margin-bottom: 10px; border: 1px solid #e5e7eb; }
        .f-g { margin-bottom: 8px; }
        .f-g label { display: block; font-size: 12px; font-weight: 600; margin-bottom: 4px; color: #4b5563; }
        .inp { width: 100%; padding: 8px; border: 1px solid #e5e7eb; border-radius: 6px; outline: none; }
        .btn { width: 100%; padding: 10px; border: none; border-radius: 6px; color: #fff; font-weight: bold; margin-top: 5px; }
        .btn-p { background: var(--p); } .btn-s { background: var(--s); } .btn-d { background: var(--d); }
        .box { background: #e0e7ff; border: 1px solid #bfdbfe; }
        .item { background: #f9fafb; border: 1px solid #e5e7eb; padding: 8px; border-radius: 6px; margin-top: 6px; word-break: break-all; }
        .tit { font-weight: bold; color: var(--p); border-bottom: 1px dashed #e5e7eb; padding-bottom: 2px; margin-bottom: 4px; }
        .err { color: var(--d); font-size: 12px; font-weight: bold; display: none; background: #fee2e2; padding: 4px; border-radius: 4px; margin: 4px 0; }
        .list { background: #fff; border: 1px solid #e5e7eb; max-height: 120px; overflow-y: auto; border-radius: 6px; display: none; }
        .tbl { width: 100%; border-collapse: collapse; background: #fff; table-layout: fixed; margin-top: 5px; }
        .tbl th { background: #e0e7ff; color: var(--p); padding: 6px; text-align: left; }
        .tbl td { padding: 6px; border-bottom: 1px solid #f3f4f6; word-break: break-all; }
    </style>
</head>
<body>
    <div class="h">Quản lý ME</div>

    <!-- TRANG CHỦ -->
    <div id="mainMenu" class="page active">
        <div class="grid">
            <button class="m-btn" onclick="go('pK')"><svg viewBox="0 0 24 24"><path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm-2 10h-4v4h-2v-4H7v-2h4V7h2v4h4v2z"/></svg>Khuôn Dập</button>
            <button class="m-btn" onclick="go('pD')"><svg viewBox="0 0 24 24"><path d="M19.14 12.94c.04-.3.06-.61.06-.94s-.02-.64-.07-.94l2.03-1.58c.18-.14.23-.41.12-.61l-1.92-3.32c-.12-.22-.37-.29-.59-.22l-2.39.96c-.5-.38-1.03-.7-1.62-.94l-.36-2.54c-.04-.24-.24-.41-.48-.41h-3.84c-.24 0-.43.17-.47.41l-.36 2.54c-.59.24-1.13.57-1.62.94l-2.39-.96c-.22-.08-.47 0-.59.22L2.74 8.87c-.12.21-.08.47.12.61l2.03 1.58c-.05.3-.09.63-.09.94s.02.64.07.94l-2.03 1.58c-.18.14-.23.41-.12.61l1.92 3.32c.12.22.37.29.59.22l2.39-.96c.5.38 1.03.7 1.62.94l.36 2.54c.05.24.24.41.48.41h3.84c.24 0 .44-.17.47-.41l.36-2.54c.59-.24 1.13-.56 1.62-.94l2.39.96c.22.08.47 0 .59-.22l1.92-3.32c.12-.22.07-.47-.12-.61l-2.01-1.58zM12 15.6c-1.98 0-3.6-1.62-3.6-3.6s1.62-3.6 3.6-3.6 3.6 1.62 3.6 3.6-1.62 3.6-3.6 3.6z"/></svg>Thông Số Dao</button>
            <button class="m-btn" onclick="go('pR')"><svg viewBox="0 0 24 24"><path d="M16 17.01V10h-2v7.01h-3L15 21l4-3.99h-3zM9 3L5 6.99h3V14h2V6.99h3L9 3z"/></svg>Dao Thay Thế</button>
            <button class="m-btn" onclick="go('pM')"><svg viewBox="0 0 24 24"><path d="M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm-5 14H4v-4h11v4zm0-5H4V9h11v4zm5 5h-4V9h4v9z"/></svg>Mã NVL</button>
        </div>
        <div class="card">
            <h2 style="color:#4b5563;margin-bottom:8px;">Hệ thống</h2>
            <button class="btn btn-s" onclick="bak()">📥 Sao lưu bộ nhớ (JSON)</button>
            <button class="btn btn-p" style="background:#8b5cf6;" onclick="document.getElementById('fRe').click()">📤 Khôi phục dữ liệu</button>
            <input type="file" id="fRe" accept=".json" style="display:none" onchange="res(this)">
        </div>
    </div>

    <!-- KHUÔN DẬP -->
    <div id="pK" class="page">
        <button class="b-btn" onclick="go('mainMenu')">⬅ Trở về</button>
        <div class="card">
            <h2>📦 Nhập Khuôn</h2>
            <div class="f-g"><label>Tên Khuôn *</label><input type="text" id="kTe" class="inp"></div>
            <div class="grid" style="padding:0;margin-bottom:8px;">
                <div class="f-g"><label>SL Thực Tế</label><input type="number" id="kTh" class="inp" value="0" oninput="g()"></div>
                <div class="f-g"><label>SL Nhập</label><input type="number" id="kNh" class="inp" value="0" oninput="g()"></div>
            </div>
            <div class="grid" style="padding:0;margin-bottom:8px;">
                <div class="f-g"><label>Chế Từ 1</label><input type="text" id="kC1" class="inp"></div>
                <div class="f-g"><label>Chế Từ 2</label><input type="text" id="kC2" class="inp"></div>
            </div>
            <div class="grid" style="padding:0;">
                <div class="f-g"><label>SL Chế</label><input type="number" id="kCh" class="inp" value="0" oninput="g()"></div>
                <div class="f-g"><label>GAP</label><input type="number" id="kGa" class="inp" value="0" readonly style="background:#f3f4f6;font-weight:bold;"></div>
            </div>
            <button class="btn btn-p" onclick="svK()">💾 Lưu</button>
        </div>
        <div class="card box">
            <h2>🔍 Tìm Kiếm Khuôn</h2>
            <input type="text" id="kTk" class="inp" oninput="tkK()">
            <div id="kGy" class="list"></div>
            <div id="kKq"></div>
        </div>
    </div>

    <!-- THÔNG SỐ DAO -->
    <div id="pD" class="page">
        <button class="b-btn" onclick="go('mainMenu')">⬅ Trở về</button>
        <div class="card">
            <h2>⚙ Nhập Thông Số</h2>
            <div class="f-g"><label>Tên Dao *</label><input type="text" id="dTe" class="inp"></div>
            <div class="f-g"><label>CCW *</label><input type="number" step="0.001" id="dCw" class="inp"></div>
            <button class="btn btn-p" onclick="svD()">💾 Lưu</button>
            <button class="btn btn-s" style="background:#059669;margin-top:8px;" onclick="document.getElementById('fTx').click()">📂 Nạp file TXT</button>
            <input type="file" id="fTx" accept=".txt" style="display:none" onchange="imp(this)">
        </div>
        <div class="card box">
            <h2>🔍 Tìm Kiếm & Lọc</h2>
            <div class="grid" style="padding:0;">
                <div class="f-g"><label>Theo Tên</label><input type="text" id="dTn" class="inp" oninput="tkD()"></div>
                <div class="f-g"><label>Theo CCW (≤ 0.05)</label><input type="number" step="0.001" id="dTc" class="inp" oninput="tkD()"></div>
            </div>
            <div id="dKq"></div>
        </div>
    </div>

    <!-- DAO THAY THẾ -->
    <div id="pR" class="page">
        <button class="b-btn" onclick="go('mainMenu')">⬅ Trở về</button>
        <div class="card">
            <h2>🔄 Dao Thay Thế</h2>
            <div class="f-g"><label>Tên Dao Gốc *</label><input type="text" id="rtG" class="inp"></div>
            <div class="f-g">
                <label>Dao Thay Thế (3 ô)</label>
                <input type="text" id="rt1" class="inp" style="margin-bottom:4px;">
                <input type="text" id="rt2" class="inp" style="margin-bottom:4px;">
                <input type="text" id="rt3" class="inp">
            </div>
            <div class="f-g"><label>Ngày Thay</label><input type="date" id="rtN" class="inp"></div>
            <input type="hidden" id="rtId" value="">
            <button class="btn btn-p" onclick="svR()">💾 Lưu</button>
        </div>
        <div class="card box">
            <h2>🔍 Tra Cứu</h2>
            <input type="text" id="rtTk" class="inp" oninput="tkR()">
            <div id="rtKq"></div>
        </div>
    </div>

    <!-- MÃ NVL -->
    <div id="pM" class="page">
        <button class="b-btn" onclick="go('mainMenu')">⬅ Trở về</button>
        <div class="card">
            <h2>🏷 Mã Nguyên Vật Liệu</h2>
            <div class="f-g"><label>Mã Gốc *</label><input type="text" id="mGc" class="inp" oninput="chM()"></div>
            <div class="f-g"><label>Mã Nội Bộ *</label><input type="text" id="mNb" class="inp" oninput="chM()"></div>
            <div id="mEr" class="err"></div>
            <div class="f-g"><label>Ghi Chú</label><input type="text" id="mNt" class="inp"></div>
            <button class="btn btn-p" onclick="svM()">💾 Lưu Mã</button>
        </div>
        <div class="card box">
            <h2>🔍 Tra Cứu (Gốc / Nội Bộ)</h2>
            <input type="text" id="mTk" class="inp" oninput="tkM()">
            <div id="mKq"></div>
        </div>
    </div>

    <script>
        // --- CORE ---
        function go(id) { document.querySelectorAll('.page').forEach(p=>p.classList.remove('active')); document.getElementById(id).classList.add('active'); window.scrollTo(0,0); }
        function bak() {
            const o = { k: localStorage.getItem('M_K')||'[]', d: localStorage.getItem('M_D')||'[]', r: localStorage.getItem('M_R')||'[]', m: localStorage.getItem('M_M')||'[]' };
            const a = document.createElement('a'); a.href = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(o));
            a.download = "ME_Backup_" + new Date().toISOString().slice(0,10) + ".json"; document.body.appendChild(a); a.click(); a.remove();
        }
        function res(i) {
            const r = new FileReader(); r.onload = function(){
                try {
                    const d = JSON.parse(r.result); if(d.k) localStorage.setItem('M_K', d.k); if(d.d) localStorage.setItem('M_D', d.d); if(d.r) localStorage.setItem('M_R', d.r); if(d.m) localStorage.setItem('M_M', d.m);
                    alert("Thành công!"); location.reload();
                } catch(e) { alert("Lỗi file!"); }
            }; if(i.files) r.readAsText(i.files);
        }

        // --- KHUÔN ---
        function g() { document.getElementById('kGa').value = (parseInt(document.getElementById('kTh').value)||0) - ((parseInt(document.getElementById('kNh').value)||0) + (parseInt(document.getElementById('kCh').value)||0)); }
        function svK() {
            const t = document.getElementById('kTe').value.trim().toUpperCase(); if(!t) return alert("Thiếu tên!");
            let db = JSON.parse(localStorage.getItem('M_K'))||[]; const c1 = document.getElementById('kC1').value.trim().toUpperCase(); const c2 = document.getElementById('kC2').value.trim().toUpperCase();
            [c1,c2].forEach(c => { if(c) { let o = db.find(x=>x.t===c); if(o) { o.th = Math.max(0, o.th-1); o.ct = o.ct ? o.ct+", "+t : t; } } });
            let i = db.findIndex(x=>x.t===t); const item = { t:t, th:parseInt(document.getElementById('kTh').value)||0, nh:parseInt(document.getElementById('kNh').value)||0, ch:parseInt(document.getElementById('kCh').value)||0, c1:c1, c2:c2, ga:parseInt(document.getElementById('kGa').value)||0, ct:i>=0?(db[i].ct||''):'' };
            if(i>=0) db[i]=item; else db.push(item); localStorage.setItem('M_K', JSON.stringify(db)); alert("Đã lưu!"); document.getElementById('kTe').value=''; document.getElementById('kC1').value=''; document.getElementById('kC2').value=''; g(); tkK();
        }
        function tkK() {
            const v = document.getElementById('kTk').value.trim().toUpperCase(); const db = JSON.parse(localStorage.getItem('M_K'))||[];
            const gy = document.getElementById('kGy'); const kq = document.getElementById('kKq'); gy.innerHTML=''; kq.innerHTML=''; if(!v) return gy.style.display='none';
            const res = db.filter(x=>x.t.includes(v));
            if(res.length>0) {
                gy.style.display='block'; res.forEach(x=>{ let d=document.createElement('div'); d.className='suggest-item'; d.innerText=x.t; d.onclick=()=>{ document.getElementById('kTk').value=x.t; gy.style.display='none'; shK(x.t); }; gy.appendChild(d); });
            } else gy.style.display='none';
            let ex = db.find(x=>x.t===v); if(ex) shK(ex.t);
        }
        function shK(t) {
            const x = (JSON.parse(localStorage.getItem('M_K'))||[]).find(x=>x.t===t); if(!x) return;
            document.getElementById('kKq').innerHTML = `<div class="item"><div class="tit">${x.t}</div><p>Thực tế: ${x.th} | Nhập: ${x.nh} | Chế: ${x.ch}<br>Chế từ: ${x.c1||'X'}, ${x.c2||'X'}<br><span style="color:var(--d)">Chế thành: ${x.ct||'X'}</span><br>GAP: <b>${x.ga}</b></p><div class="grid" style="padding:0;margin-top:4px;"><button class="btn btn-s" onclick="edK('${x.t}')">Sửa</button><button class="btn btn-d" onclick="delK('${x.t}')">Xóa</button></div></div>`;
        }
        function edK(t) {
            const x = (JSON.parse(localStorage.getItem('M_K'))||[]).find(x=>x.t===t); if(!x) return;
            document.getElementById('kTe').value=x.t; document.getElementById('kTh').value=x.th; document.getElementById('kNh').value=x.nh; document.getElementById('kCh').value=x.ch; document.getElementById('kC1').value=x.c1; document.getElementById('kC2').value=x.c2; document.getElementById('kGa').value=x.ga; window.scrollTo(0,0);
        }
        function delK(t) { if(confirm("Xóa lân 1?")) setTimeout(()=>{ if(confirm("Xóa vĩnh viễn?")) { let db=JSON.parse(localStorage.getItem('M_K'))||[]; db=db.filter(x=>x.t!==t); localStorage.setItem('M_K', JSON.stringify(db)); document.getElementById('kTk').value=''; tkK(); } },200); }

        // --- DAO ---
        function svD() {
            const t = document.getElementById('dTe').value.trim().toUpperCase(); const c = parseFloat(document.getElementById('dCw').value); if(!t||isNaN(c)) return alert("Nhập đủ!");
            let db = JSON.parse(localStorage.getItem('M_D'))||[]; let i = db.findIndex(x=>x.t===t); if(i>=0) db[i].c=c; else db.push({t:t, c:c});
            localStorage.setItem('M_D', JSON.stringify(db)); alert("Đã lưu!"); document.getElementById('dTe').value=''; document.getElementById('dCw').value=''; tkD();
        }
        function imp(i) {
            const r = new FileReader(); r.onload = function() {
                let db = JSON.parse(localStorage.getItem('M_D'))||[]; let lines = r.result.split('\n'); let cnt=0;
                lines.forEach(l=>{ if(!l.trim()) return; let p=l.split(/[,;\t\s]+/); if(p.length>=2) { let t=p[0].trim().toUpperCase(); let c=parseFloat(p[p.length-1]); if(t&&!isNaN(c)) { let idx=db.findIndex(x=>x.t===t); if(idx>=0) db[idx].c=c; else db.push({t:t,c:c}); cnt++; } } });
                localStorage.setItem('M_D', JSON.stringify(db)); alert("Đã nạp "+cnt); tkD();
            }; if(i.files) r.readAsText(i.files);
        }
        function tkD() {
            const tn = document.getElementById('dTn').value.trim().toUpperCase(); const tc = parseFloat(document.getElementById('dTc').value);
            const db = JSON.parse(localStorage.getItem('M_D'))||[]; const kq = document.getElementById('dKq'); kq.innerHTML=''; if(!tn&&isNaN(tc)) return;
            let f = []; if(tn) f = db.filter(x=>x.t.includes(tn)); else if(!isNaN(tc)) { f = db.filter(x=>Math.abs(x.c-tc)<=0.05); f.sort((a,b)=>Math.abs(a.c-tc)-Math.abs(b.c-tc)); }
            if(f.length>0) {
                let h=`<table class="tbl"><thead><tr><th>Tên Dao</th><th style="text-align:right">CCW</th></tr></thead><tbody>`;
                f.forEach(x=>h+=`<tr><td><b>${x.t}</b></td><td style="text-align:right;color:var(--s);font-weight:bold;">${x.c.toFixed(3)}</td></tr>`); kq.innerHTML=h+`</tbody></table>`;
            } else kq.innerHTML='<p style="text-align:center;color:#9ca3af;padding:5px;">Trống</p>';
        }

        // --- THAY DAO ---
        document.getElementById('rtN').value = new Date().toISOString().substring(0, 10);
        function svR() {
            const g = document.getElementById('rtG').value.trim().toUpperCase(); const t1 = document.getElementById('rt1').value.trim().toUpperCase(); const t2 = document.getElementById('rt2').value.trim().toUpperCase(); const t3 = document.getElementById('rt3').value.trim().toUpperCase();
            if(!g||(!t1&&!t2&&!t3)) return alert("Nhập đủ!"); let db = JSON.parse(localStorage.getItem('M_R'))||[]; let arr = []; if(t1) arr.push(t1); if(t2) arr.push(t2); if(t3) arr.push(t3);
            const id = document.getElementById('rtId').value; if(id) { let idx=db.findIndex(x=>x.id===id); if(idx>=0) { db[idx].g=g; db[idx].sub=arr; db[idx].d=document.getElementById('rtN').value; } document.getElementById('rtId').value=''; }
            else { db.push({id:'I_'+Date.now(), g:g, sub:arr, d:document.getElementById('rtN').value}); }
            localStorage.setItem('M_R', JSON.stringify(db)); alert("Đã lưu!"); document.getElementById('rtG').value=''; document.getElementById('rt1').value=''; document.getElementById('rt2').value=''; document.getElementById('rt3').value=''; tkR();
        }
        function tkR() {
            const v = document.getElementById('rtTk').value.trim().toUpperCase(); const db = JSON.parse(localStorage.getItem('M_R'))||[]; const kq = document.getElementById('rtKq'); kq.innerHTML=''; if(!v) return;
            db.filter(x=>x.g.includes(v)).forEach(x=>{
                let s = x.sub.map((d,idx)=>`<div>↳ Loại ${idx+1}: <b>${d}</b></div>`).join(''); let d=document.createElement('div'); d.className='item';
                d.innerHTML=`<div class="tit">GỐC: ${x.g}</div><div>${s}<div style="font-size:11px;color:#6b7280;margin-top:2px;">Ngày: ${x.d}</div></div><div class="grid" style="padding:0;margin-top:4px;"><button class="btn btn-s" onclick="edR('${x.id}')">Sửa</button><button class="btn btn-d" onclick="delR('${x.id}')">Xóa</button></div>`; kq.appendChild(d);
            });
        }
        function edR(id) {
            const x = (JSON.parse(localStorage.getItem('M_R'))||[]).find(x=>x.id===id); if(!x) return;
            document.getElementById('rtG').value=x.g; document.getElementById('rt1').value=x.sub[0]||''; document.getElementById('rt2').value=x.sub[1]||''; document.getElementById('rt3').value=x.sub[2]||''; document.getElementById('rtN').value=x.d; document.getElementById('rtId').value=x.id; window.scrollTo(0,0);
        }
        function delR(id) { if(confirm("Xóa lần 1?")) setTimeout(()=>{ if(confirm("Xóa vĩnh viễn?")) { let db=JSON.parse(localStorage.getItem('M_R'))||[]; db=db.filter(x=>x.id!==id); localStorage.setItem('M_R', JSON.stringify(db)); document.getElementById('rtTk').value=''; tkR(); } },200); }

        // --- NVL ---
        function chM() {
            const g = document.getElementById('mGc').value.trim().toUpperCase(); const n = document.getElementById('mNb').value.trim().toUpperCase();
            const db = JSON.parse(localStorage.getItem('M_M'))||[]; const er = document.getElementById('mEr'); er.style.display='none'; if(!g&&!n) return;
            let h = db.find(x=>(g&&x.g===g)||(n&&x.n===n)); if(h) { er.innerText=`⚠️ TRÙNG: [Gốc: ${h.g} - Nội bộ: ${h.n}]`; er.style.display='block'; }
        }
        function svM() {
            const g = document.getElementById('mGc').value.trim().toUpperCase(); const n = document.getElementById('mNb').value.trim().toUpperCase(); if(!g||!n) return alert("Nhập đủ!");
            let db = JSON.parse(localStorage.getItem('M_M'))||[]; let idx = db.findIndex(x=>x.g===g||x.n===n); const item = {g:g, n:n, nt:document.getElementById('mNt').value.trim()||'X'};
            if(idx>=0) db[idx]=item; else db.push(item); localStorage.setItem('M_M', JSON.stringify(db)); alert("Đã lưu mã NVL!"); document.getElementById('mGc').value=''; document.getElementById('mNb').value=''; document.getElementById('mNt').value=''; document.getElementById('mEr').style.display='none'; tkM();
        }
        function tkM() {
            const v = document.getElementById('mTk').value.trim().toUpperCase(); const db = JSON.parse(localStorage.getItem('M_M'))||[]; const kq = document.getElementById('mKq'); kq.innerHTML=''; if(!v) return;
            db.filter(x=>x.g.includes(v)||x.n.includes(v)).forEach(x=>{
                let d = document.createElement('div'); d.className='item';
                d.innerHTML=`<div class="tit">NVL KHỚP</div><p>Gốc: <b>${x.g}</b><br>Nội bộ: <b>${x.n}</b><br>Ghi chú: ${x.nt}</p><button class="btn btn-d" style="padding:4px;margin-top:4px;" onclick="delM('${x.g}')">🗑 Xóa</button>`; kq.appendChild(d);
            });
        }
        function delM(g) { if(confirm("Xóa mã này?")) { let db=JSON.parse(localStorage.getItem('M_M'))||[]; db=db.filter(x=>x.g!==g); localStorage.setItem('M_M', JSON.stringify(db)); document.getElementById('mTk').value=''; tkM(); } }
    </script>
</body>
</html>
