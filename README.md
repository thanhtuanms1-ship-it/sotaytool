
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Sổ tay bảo trì</title>
    <style>
        :root { --p: #2563eb; --s: #16a34a; --bg: #f4f6f9; --txt: #1f2937; }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: system-ui, sans-serif; font-size: 13px; }
        body { background: var(--bg); color: var(--txt); padding: 10px; width: 100vw; overflow-x: hidden; }
        .h { background: var(--p); color: #fff; padding: 12px; text-align: center; font-size: 16px; font-weight: bold; border-radius: 8px; margin-bottom: 10px; }
        .card { background: #fff; padding: 12px; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); margin-bottom: 10px; border: 1px solid #e5e7eb; width: 100%; }
        .f-g { margin-bottom: 8px; }
        .f-g label { display: block; font-size: 11px; font-weight: 600; margin-bottom: 4px; color: #4b5563; }
        .inp { width: 100%; padding: 8px; border: 1px solid #e5e7eb; border-radius: 6px; outline: none; }
        .btn { width: 100%; padding: 10px; border: none; border-radius: 6px; color: #fff; font-weight: bold; background: var(--p); margin-top: 4px; }
        .search-box { background: #e0e7ff; border: 1px solid #bfdbfe; }
        
        /* Khung cuộn dọc bảo vệ chiều ngang không bị tràn */
        .tbl-box { width: 100%; max-height: 350px; overflow-y: auto; overflow-x: hidden; border-radius: 6px; border: 1px solid #e5e7eb; background: #fff; }
        .tbl { width: 100%; border-collapse: collapse; table-layout: fixed; }
        
        /* Ghim cố định dòng tiêu đề khi cuộn dọc */
        .tbl thead th { position: sticky; top: 0; background: #e0e7ff; color: var(--p); z-index: 10; font-weight: bold; border-bottom: 2px solid #bfdbfe; padding: 8px 4px; font-size: 12px; }
        .tbl td { padding: 8px 4px; border-bottom: 1px solid #f3f4f6; font-size: 12px; text-align: center; word-break: break-all; }
        
        /* Tự động chia tỷ lệ các cột vừa khít 100% chiều ngang điện thoại */
        .col-date { width: 18%; text-align: left !important; padding-left: 6px !important; }
        .col-name { width: 30%; text-align: left !important; font-weight: 600; }
        .col-step { width: 14%; }
        .col-del  { width: 10%; }

        .ok-btn { width: 100%; background: var(--s); color: #fff; border: none; padding: 5px 2px; border-radius: 4px; font-weight: bold; font-size: 11px; }
        .done-date { color: var(--s); font-weight: bold; font-family: monospace; font-size: 10px; display: block; text-align: center; }
        .del-btn { background: #dc2626; color: #fff; border: none; padding: 4px; border-radius: 4px; font-size: 11px; width: 100%; }
    </style>
</head>
<body>
    <div class="h">Sổ tay bảo trì</div>
    
    <div class="card">
        <div class="f-g"><label>Ngày bảo trì *</label><input type="date" id="btNgay" class="inp"></div>
        <div class="f-g"><label>Tên thiết bị *</label><input type="text" id="btTen" class="inp" placeholder="Ví dụ: MÁY DẬP 01"></div>
        <button class="btn" onclick="luuBT()">💾 Lưu thiết bị</button>
    </div>

    <div class="card search-box">
        <div class="f-g">
            <label style="color: var(--p);">🔍 Tìm kiếm thiết bị nhanh</label>
            <input type="text" id="btTk" class="inp" placeholder="Nhập tên thiết bị..." oninput="timKiemBT()">
        </div>
    </div>

    <div class="card" style="padding: 4px;" id="viTriBang">
        <div class="tbl-box">
            <table class="tbl">
                <thead>
                    <tr>
                        <th class="col-date" style="text-align:left; padding-left:6px;">Ngày</th>
                        <th class="col-name" style="text-align:left;">Thiết bị</th>
                        <th class="col-step">M.Cắt</th>
                        <th class="col-step">B.Trì</th>
                        <th class="col-step">Form</th>
                        <th class="col-del">Xóa</th>
                    </tr>
                </thead>
                <tbody id="btList"></tbody>
            </table>
        </div>
    </div>
    <script>
        // Tự động điền ngày hôm nay
        document.getElementById('btNgay').value = new Date().toISOString().substring(0, 10);

        // Quản lý bộ nhớ điện thoại
        function layData() { return JSON.parse(localStorage.getItem('SO_TAY_BT')) || []; }
        function luuData(d) { localStorage.setItem('SO_TAY_BT', JSON.stringify(d)); }

        // Lưu thiết bị mới
        function luuBT() {
            const ngay = document.getElementById('btNgay').value;
            const ten = document.getElementById('btTen').value.trim().toUpperCase();
            if (!ngay || !ten) return alert("Vui lòng điền đủ Ngày và Tên thiết bị!");

            let db = layData();
            db.push({
                id: 'ID_' + Date.now(),
                ngay: ngay,
                ten: ten,
                matCat: '',
                baoTri: '',
                vietForm: ''
            });

            luuData(db);
            document.getElementById('btTen').value = ''; 
            hienThi('');
        }

        // Ghi nhận ngày tháng khi bấm OK
        function hoanThanh(id, buoc) {
            let db = layData();
            let o = db.find(x => x.id === id);
            if (o) {
                const bayGio = new Date();
                const d = String(bayGio.getDate()).padStart(2, '0');
                const m = String(bayGio.getMonth() + 1).padStart(2, '0');
                
                o[buoc] = `${d}/${m}`; 
                luuData(db);
                
                const tuKhoa = document.getElementById('btTk').value.trim().toUpperCase();
                hienThi(tuKhoa);
            }
        }

        // Tìm kiếm và cuộn màn hình mượt mà
        function timKiemBT() {
            const tuKhoa = document.getElementById('btTk').value.trim().toUpperCase();
            hienThi(tuKhoa);
            if(tuKhoa.length > 0) {
                document.getElementById('viTriBang').scrollIntoView({ behavior: 'smooth' });
            }
        }

        // Logic kích hoạt trạng thái xác nhận xóa (Không dùng confirm app)
        function thucHienXoa(e, id) {
            e.stopPropagation(); // Chặn sự kiện click lan ra ngoài body
            const btn = e.target;
            
            if (btn.innerText === "Xóa") {
                // Đổi nút hiện tại sang trạng thái chờ xác nhận
                btn.innerText = "OK?";
                btn.style.background = "#991b1b"; // Đỏ đậm hơn
                btn.style.fontSize = "10px";
            } else if (btn.innerText === "OK?") {
                // Tiến hành xóa vĩnh viễn khi bấm lần 2
                let db = layData();
                db = db.filter(x => x.id !== id);
                luuData(db);
                const tuKhoa = document.getElementById('btTk').value.trim().toUpperCase();
                hienThi(tuKhoa);
            }
        }

        // Khi bấm vào bất kỳ vị trí nào khác trên màn hình, tự hủy chế độ xóa
        document.body.addEventListener('click', function() {
            document.querySelectorAll('.del-btn').forEach(btn => {
                if (btn.innerText === "OK?") {
                    btn.innerText = "Xóa";
                    btn.style.background = "#dc2626"; // Trả về màu đỏ gốc
                    btn.style.fontSize = "11px";
                }
            });
        });

        // Hiển thị dữ liệu lên bảng thống kê co giãn 100% chiều ngang
        function hienThi(boLoc) {
            const db = layData();
            const tbody = document.getElementById('btList');
            tbody.innerHTML = '';

            let dsHienThi = db;
            if(boLoc) { dsHienThi = db.filter(x => x.ten.includes(boLoc)); }

            if (dsHienThi.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" style="text-align:center;color:#9ca3af;padding:15px;">Trống</td></tr>`;
                return;
            }

            // Sắp xếp ngày mới nhất lên đầu
            dsHienThi.sort((a, b) => new Date(b.ngay) - new Date(a.ngay));

            dsHienThi.forEach(x => {
                let tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="col-date"><b>${x.ngay.split('-').reverse().slice(0,2).join('/')}</b></td>
                    <td class="col-name">${x.ten}</td>
                    <td class="col-step">${x.matCat ? `<span class="done-date">✓ ${x.matCat}</span>` : `<button class="ok-btn" onclick="hoanThanh('${x.id}', 'matCat')">OK</button>`}</td>
                    <td class="col-step">${x.baoTri ? `<span class="done-date">✓ ${x.baoTri}</span>` : `<button class="ok-btn" onclick="hoanThanh('${x.id}', 'baoTri')">OK</button>`}</td>
                    <td class="col-step">${x.vietForm ? `<span class="done-date">✓ ${x.vietForm}</span>` : `<button class="ok-btn" onclick="hoanThanh('${x.id}', 'vietForm')">OK</button>`}</td>
                    <td class="col-del"><button class="del-btn" onclick="thucHienXoa(event, '${x.id}')">Xóa</button></td>
                `;
                tbody.appendChild(tr);
            });
        }

        // Khởi động bảng thống kê ban đầu
        hienThi('');
    </script>
</body>
</html>
