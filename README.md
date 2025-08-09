<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nextwaves Industries - Tạo Mã Vạch & Tem Sản Phẩm</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <!-- Dùng html2canvas để chụp DOM -->
  <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      padding: 20px;
    }
    .container {
      max-width: 1000px;
      margin: 0 auto;
      background: white;
      border-radius: 20px;
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
      overflow: hidden;
    }
    .header {
      background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
      color: white;
      padding: 30px 40px;
      text-align: center;
      position: relative;
    }
    .logo { max-width: 200px; height: auto; margin-bottom: 15px; filter: brightness(0) invert(1); }
    .header h1 { font-size: 2.2rem; margin-bottom: 10px; font-weight: 700; }
    .header p { font-size: 1.1rem; opacity: 0.9; }

    .tabs { display: flex; background: #f8f9fa; border-bottom: 1px solid #dee2e6; }
    .tab {
      flex: 1; padding: 20px; text-align: center; background: none; border: none;
      font-size: 1.1rem; font-weight: 600; color: #6c757d; cursor: pointer;
      transition: all 0.3s ease; position: relative;
    }
    .tab.active { color: #007bff; background: white; }
    .tab.active::after {
      content: ''; position: absolute; bottom: 0; left: 0; right: 0; height: 3px; background: #007bff;
    }
    .tab-content { display: none; padding: 40px; }
    .tab-content.active { display: block; }

    .form-group { margin-bottom: 25px; }
    .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 25px; }
    .form-row-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px; margin-bottom: 25px; }
    label { display: block; margin-bottom: 8px; font-weight: 600; color: #333; font-size: 0.95rem; }

    input[type="text"], input[type="number"], select {
      width: 100%; padding: 12px 16px; border: 2px solid #e9ecef; border-radius: 8px;
      font-size: 1rem; transition: border-color 0.3s ease; background: white;
    }
    input[type="text"]:focus, input[type="number"]:focus, select:focus {
      outline: none; border-color: #007bff; box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
    }
    input[type="checkbox"] { margin-right: 8px; transform: scale(1.2); }
    .checkbox-group { display: flex; align-items: center; margin-top: 10px; }

    .btn {
      background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
      color: white; padding: 14px 28px; border: none; border-radius: 8px;
      font-size: 1rem; font-weight: 600; cursor: pointer; transition: all 0.3s ease;
      margin: 8px; box-shadow: 0 4px 15px rgba(0, 123, 255, 0.3);
    }
    .btn:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(0, 123, 255, 0.4); }
    .btn-success { background: linear-gradient(135deg, #28a745 0%, #20c997 100%); box-shadow: 0 4px 15px rgba(40, 167, 69, 0.3); }
    .btn-success:hover { box-shadow: 0 6px 20px rgba(40, 167, 69, 0.4); }
    .btn-danger { background: linear-gradient(135deg, #dc3545 0%, #fd7e14 100%); box-shadow: 0 4px 15px rgba(220, 53, 69, 0.3); }
    .btn-danger:hover { box-shadow: 0 6px 20px rgba(220, 53, 69, 0.4); }

    .result-section { margin-top: 30px; padding: 30px; background: #f8f9fa; border-radius: 12px; text-align: center; }
    .barcode-container {
      background: white; padding: 30px; border-radius: 12px; margin: 20px 0;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1); display: inline-block;
    }

    /* ----------- LABEL (9 KHỐI) ----------- */
    .label-preview {
      background: white;
      border: 2px dashed #999;
      margin: 20px auto;
      font-family: Arial, sans-serif;
      position: relative;
      display: inline-block;
      overflow: hidden;
    }
    /* Kích thước xem trước theo mm đúng yêu cầu */
    .label-70x100 { width: 350mm; height: 500mm; }
    .label-30x40  { width: 150mm; height: 200mm; }

    /* Bố cục khối */
    .block { width: calc(100% - 10px); margin-left: 10px; }
    .block:not(.block-9) { margin-bottom: 1%; }
    .block-1 { height: 12%; margin-top: 2%; }
    .block-2 { height: 3%; }
    .block-3 { height: 4%; }
    .block-4 { height: 4%; }
    .block-5 { height: 24%; }
    .block-6 { height: 5%; }
    .block-7 { height: 4%; }
    .block-8 { height: 25%; }
    .block-9 { height: 7%; margin-bottom: 2%; }

    .block-inner {
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: flex-start; /* mặc định lề trái */
      padding: 5px;                /* cách 4 mép 5px */
    }
    /* Khối 1: chỉ cách trên & dưới 1px, không yêu cầu trái/phải */
    .block-1 .block-inner { padding-top: 1px; padding-bottom: 1px; padding-left: 0; padding-right: 0; }

    /* Căn giữa cho khối 1,2,8,9 */
    .block-1 .block-inner,
    .block-2 .block-inner,
    .block-8 .block-inner,
    .block-9 .block-inner { justify-content: center; text-align: center; }

    /* Text tự co */
    .fit-text { display: inline-block; width: 100%; white-space: nowrap; overflow: hidden; }

    /* Ảnh logo tự co dãn */
    .brand-logo {
      max-height: 100%;
      max-width: 100%;
      object-fit: contain;
      display: block;
    }

    /* Bảng thông số (khối 5) chiếm 100% chiều cao khối */
    .block-5 .block-inner { align-items: stretch; }
    .spec-table {
      width: 100%;
      height: 100%;
      table-layout: fixed;
      border-collapse: collapse;
      border: 2px solid #000;
    }
    .spec-table th, .spec-table td {
      border: 2px solid #000;
      padding: 4px;
      text-align: left;         /* yêu cầu lề trái */
      vertical-align: middle;
      word-break: break-word;
    }
    .spec-table th { background: #f0f0f0; }

    /* Khối 8: cột dọc, ảnh mã vạch co dãn + số dưới */
    .block-8 .block-inner { flex-direction: column; }
    .barcode-img-dynamic {
      flex: 1;
      max-width: 100%;
      max-height: calc(100% - 24px); /* chừa chỗ cho số dưới */
      object-fit: contain;
      display: block;
    }
    .barcode-numbers { letter-spacing: 1px; }

    @media (max-width: 768px) {
      .form-row, .form-row-3 { grid-template-columns: 1fr; }
      .tabs { flex-direction: column; }
      .container { margin: 10px; border-radius: 15px; }
      .header { padding: 20px; }
      .header h1 { font-size: 1.8rem; }
      .tab-content { padding: 20px; }
    }

    @media print {
      body { margin: 0; padding: 0; background: white; }
      .container { box-shadow: none; margin: 0; padding: 0; }
      .header, .tabs, .tab-content:not(.active), .btn, .download-options { display: none; }
      .label-preview { border: 1px solid #000; margin: 0; }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <img src="https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png" alt="Nextwaves Industries" class="logo" onerror="this.style.display='none'">
      <h1>🏷️ Nextwaves Industries</h1>
      <p>Tạo Mã Vạch & Tem Sản Phẩm Chuyên Nghiệp</p>
    </div>

    <div class="tabs">
      <button class="tab active" onclick="switchTab('barcode')">📊 Tạo Mã Vạch</button>
      <button class="tab" onclick="switchTab('label')">🏷️ Tạo Tem Sản Phẩm</button>
    </div>

    <!-- Tab Tạo Mã Vạch -->
    <div id="barcode-tab" class="tab-content active">
      <div class="form-row">
        <div class="form-group">
          <label for="barcodeInput">📝 Nhập dãy số:</label>
          <!-- bỏ placeholder để không còn “chữ mờ”, giữ value thật -->
          <input type="text" id="barcodeInput" value="1234567890123">
        </div>
        <div class="form-group">
          <label for="barcodeFormat">📋 Định dạng mã vạch:</label>
          <select id="barcodeFormat">
            <option value="CODE128">CODE128 (Phổ biến nhất)</option>
            <option value="EAN13">EAN-13 (Quốc tế)</option>
            <option value="EAN8">EAN-8 (Ngắn gọn)</option>
            <option value="UPC">UPC-A (Mỹ)</option>
            <option value="CODE39">CODE39 (Công nghiệp)</option>
            <option value="ITF14">ITF-14 (Thùng carton)</option>
          </select>
        </div>
      </div>

      <div class="form-row-3">
        <div class="form-group">
          <label for="barcodeWidth">📏 Độ rộng thanh:</label>
          <input type="number" id="barcodeWidth" min="1" max="5" step="0.1" value="2">
        </div>
        <div class="form-group">
          <label for="barcodeHeight">📐 Chiều cao (px):</label>
          <input type="number" id="barcodeHeight" min="20" max="200" value="100">
        </div>
        <div class="form-group">
          <label for="showText">🔤 Hiển thị số:</label>
          <div class="checkbox-group">
            <input type="checkbox" id="showText" checked>
            <span>Hiển thị số bên dưới</span>
          </div>
        </div>
      </div>

      <div style="text-align: center;">
        <button class="btn" onclick="generateBarcode()">🎯 Tạo Mã Vạch</button>
        <button class="btn btn-success" onclick="downloadBarcode()">📥 Tải xuống PNG</button>
      </div>

      <div id="barcodeResult" class="result-section" style="display: none;">
        <h3>✅ Mã vạch đã được tạo thành công!</h3>
        <div class="barcode-container">
          <svg id="barcodeDisplay"></svg>
        </div>
      </div>
    </div>

    <!-- Tab Tạo Tem Sản Phẩm -->
    <div id="label-tab" class="tab-content">
      <div class="form-group">
        <label for="labelSize">📏 Kích thước tem:</label>
        <select id="labelSize">
          <option value="70x100">70 x 100 mm (Kích thước lớn)</option>
          <option value="30x40">30 x 40 mm (Kích thước nhỏ)</option>
        </select>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="productName">🏷️ Tên sản phẩm:</label>
          <input type="text" id="productName" value="ĂNG TEN MẶT ĐẤT TUYẾN TÍNH">
        </div>
        <div class="form-group">
          <label for="technicalName">🔧 Tên kỹ thuật:</label>
          <input type="text" id="technicalName" value="RA-UL-SFF-241401">
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="frequencyRange">📡 Dải tầng:</label>
          <input type="text" id="frequencyRange" value="900 - 930 MHz">
        </div>
        <div class="form-group">
          <label for="gain">⚡ Độ lợi:</label>
          <input type="text" id="gain" value="7.5 dBi">
        </div>
      </div>

      <div class="form-group">
        <label for="labelBarcodeText">📊 Mã vạch (EAN-13):</label>
        <input type="text" id="labelBarcodeText" value="8936236710036">
      </div>

      <h4 style="margin: 25px 0 15px 0; color: #333;">🎛️ Tùy chỉnh mã vạch trên tem:</h4>
      <div class="form-row">
        <div class="form-group">
          <label for="labelBarcodeFormat">📋 Định dạng mã vạch:</label>
          <select id="labelBarcodeFormat">
            <option value="EAN13">EAN-13 (Khuyến nghị)</option>
            <option value="CODE128">CODE128</option>
            <option value="EAN8">EAN-8</option>
            <option value="UPC">UPC-A</option>
            <option value="CODE39">CODE39</option>
            <option value="ITF14">ITF-14</option>
          </select>
        </div>
        <div class="form-group">
          <label for="labelBarcodeWidth">📏 Độ rộng thanh:</label>
          <input type="number" id="labelBarcodeWidth" min="1" max="5" step="0.1" value="2">
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="labelBarcodeHeight">📐 Chiều cao (px):</label>
          <input type="number" id="labelBarcodeHeight" min="20" max="100" value="50">
        </div>
        <div class="form-group">
          <label for="labelShowText">🔤 Hiển thị số:</label>
          <div class="checkbox-group">
            <input type="checkbox" id="labelShowText" checked>
            <span>Hiển thị số bên dưới mã vạch</span>
          </div>
        </div>
      </div>

      <div style="text-align: center;">
        <button class="btn" onclick="generateLabel()">🎯 Tạo Tem Sản Phẩm</button>
        <button class="btn" onclick="printLabel()">🖨️ In Tem</button>
      </div>

      <div class="download-options">
        <h4>📥 Tùy chọn tải xuống PDF</h4>
        <!-- Loại bỏ nút A4; chỉ còn 1 nút xuất PDF chụp DOM -->
        <button class="btn btn-success" onclick="downloadLabelPDF()">📄 Tải xuống 1 tem PDF</button>
      </div>

      <div id="labelResult" class="result-section" style="display: none;">
        <h3>✅ Tem sản phẩm đã được tạo thành công!</h3>
        <div id="labelPreview" class="label-preview"></div>
      </div>
    </div>
  </div>

  <canvas id="barcodeCanvas" style="display: none;"></canvas>
  <canvas id="labelBarcodeCanvas" style="display: none;"></canvas>

  <script>
    // Global
    let currentTab = 'barcode';
    let currentBarcodeDataURL = '';

    function switchTab(tab) {
      currentTab = tab;
      document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
      document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
      if (tab === 'barcode') {
        document.querySelector('.tab:first-child').classList.add('active');
        document.getElementById('barcode-tab').classList.add('active');
      } else {
        document.querySelector('.tab:last-child').classList.add('active');
        document.getElementById('label-tab').classList.add('active');
      }
    }

    // Generate barcode (giữ nguyên hành vi khi bấm nút)
    function generateBarcode() {
      const text = document.getElementById('barcodeInput').value;
      const format = document.getElementById('barcodeFormat').value;
      const width = parseFloat(document.getElementById('barcodeWidth').value);
      const height = parseInt(document.getElementById('barcodeHeight').value);
      const showText = document.getElementById('showText').checked;
      if (!text) { alert('Vui lòng nhập dãy số!'); return; }
      try {
        JsBarcode("#barcodeDisplay", text, {
          format, width, height, displayValue: showText,
          background: "#ffffff", lineColor: "#000000", margin: 10,
          fontSize: 14, textAlign: "center", textPosition: "bottom"
        });
        document.getElementById('barcodeResult').style.display = 'block';
      } catch (error) { alert('Lỗi tạo mã vạch: ' + error.message); }
    }

    function downloadBarcode() {
      const barcodeElement = document.getElementById('barcodeDisplay');
      if (!barcodeElement || !barcodeElement.innerHTML) {
        alert('Vui lòng tạo mã vạch trước khi tải xuống!'); return;
      }
      try {
        const svgElement = barcodeElement;
        const svgData = new XMLSerializer().serializeToString(svgElement);
        const canvas = document.getElementById('barcodeCanvas');
        const ctx = canvas.getContext('2d');
        const img = new Image();
        img.onload = function() {
          canvas.width = img.width; canvas.height = img.height;
          ctx.fillStyle = 'white'; ctx.fillRect(0, 0, canvas.width, canvas.height);
          ctx.drawImage(img, 0, 0);
          canvas.toBlob(function(blob) {
            if (blob.size > 0) {
              const link = document.createElement('a');
              link.download = `ma-vach-${document.getElementById('barcodeInput').value}-${Date.now()}.png`;
              link.href = URL.createObjectURL(blob);
              document.body.appendChild(link); link.click(); document.body.removeChild(link);
              URL.revokeObjectURL(link.href);
            } else { alert('Lỗi tạo file. Vui lòng thử lại!'); }
          }, 'image/png');
        };
        img.src = 'data:image/svg+xml;base64,' + btoa(unescape(encodeURIComponent(svgData)));
      } catch (error) { alert('Lỗi tải xuống: ' + error.message); }
    }

    // Tạo PNG mã vạch cho khối 8
    function generateLabelBarcode(text, format, width, height, showText) {
      return new Promise((resolve, reject) => {
        try {
          const canvas = document.getElementById('labelBarcodeCanvas');
          JsBarcode(canvas, text, {
            format, width, height, displayValue: false, /* chữ hiển thị sẽ render riêng */
            background: "#ffffff", lineColor: "#000000", margin: 0
          });
          const dataURL = canvas.toDataURL('image/png');
          resolve(dataURL);
        } catch (error) { reject(error); }
      });
    }

    // Hàm co chữ để vừa khối (giảm dần tới khi vừa)
    function fitTextToBlock(el, maxPx, minPx) {
      if (!el) return;
      let size = maxPx;
      el.style.fontSize = size + 'px';
      el.style.lineHeight = '1.1';
      const parent = el.parentElement;
      while (size > minPx && (el.scrollWidth > parent.clientWidth || el.scrollHeight > parent.clientHeight)) {
        size -= 1;
        el.style.fontSize = size + 'px';
      }
    }

    // Co chữ trong bảng khối 5 cho vừa 100% chiều cao
    function fitSpecTable() {
      const wrap = document.querySelector('.block-5 .block-inner');
      const tbl  = wrap ? wrap.querySelector('.spec-table') : null;
      if (!wrap || !tbl) return;
      let size = 28; // bắt đầu lớn với tem 350x500
      // nếu tem nhỏ 150x200 thì giảm điểm xuất phát
      const isSmall = document.getElementById('labelSize').value === '30x40';
      if (isSmall) size = 14;
      tbl.style.fontSize = size + 'px';
      while (size > 8 && (tbl.scrollHeight > wrap.clientHeight || tbl.scrollWidth > wrap.clientWidth)) {
        size -= 1;
        tbl.style.fontSize = size + 'px';
      }
    }

    // Tạo tem (9 khối)
    async function generateLabel() {
      const size = document.getElementById('labelSize').value;
      const productName   = document.getElementById('productName').value || '';
      const technicalName = document.getElementById('technicalName').value || '';
      const frequencyRange = document.getElementById('frequencyRange').value || '';
      const gain = document.getElementById('gain').value || '';
      const barcodeText = document.getElementById('labelBarcodeText').value || '';

      // Cài đặt mã vạch
      const barcodeFormat = document.getElementById('labelBarcodeFormat').value;
      const barcodeWidth  = parseFloat(document.getElementById('labelBarcodeWidth').value);
      const barcodeHeight = parseInt(document.getElementById('labelBarcodeHeight').value);
      const showText = document.getElementById('labelShowText').checked;

      try {
        currentBarcodeDataURL = await generateLabelBarcode(barcodeText, barcodeFormat, barcodeWidth, barcodeHeight, showText);

        const labelPreview = document.getElementById('labelPreview');
        labelPreview.className = `label-preview label-${size}`;

        // 9 khối đúng thứ tự & nội dung
        labelPreview.innerHTML = `
          <!-- 1: Logo thương hiệu -->
          <div class="block block-1">
            <div class="block-inner">
              <img class="brand-logo" src="https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png" alt="logo" onerror="this.style.display='none'">
            </div>
          </div>

          <!-- 2: Tên thương hiệu -->
          <div class="block block-2">
            <div class="block-inner">
              <div class="fit-text" id="brandNameTxt" style="font-weight:700;">Nextwaves</div>
            </div>
          </div>

          <!-- 3: Tên sản phẩm (đậm) -->
          <div class="block block-3">
            <div class="block-inner">
              <div class="fit-text" id="productNameTxt" style="font-weight:700;">${productName}</div>
            </div>
          </div>

          <!-- 4: Tên kỹ thuật (đậm) -->
          <div class="block block-4">
            <div class="block-inner">
              <div class="fit-text" id="technicalNameTxt" style="font-weight:700;">${technicalName}</div>
            </div>
          </div>

          <!-- 5: Bảng thông số (2 cột, lề trái, cao = 100%) -->
          <div class="block block-5">
            <div class="block-inner">
              <table class="spec-table">
                <tr>
                  <th style="width:40%;">DẢI TẦN</th>
                  <td style="width:60%;">${frequencyRange}</td>
                </tr>
                <tr>
                  <th>ĐỘ LỢI</th>
                  <td>${gain}</td>
                </tr>
              </table>
            </div>
          </div>

          <!-- 6: Thông tin công ty -->
          <div class="block block-6">
            <div class="block-inner">
              <div class="fit-text" id="companyTxt">
                Sản xuất bởi công ty TNHH Nextwaves Industries • 20/23 đường 35 Phường An Khánh • Thành Phố Hồ Chí Minh
              </div>
            </div>
          </div>

          <!-- 7: Số điện thoại công ty (đậm) -->
          <div class="block block-7">
            <div class="block-inner">
              <div class="fit-text" id="phoneTxt" style="font-weight:700;">0938888373</div>
            </div>
          </div>

          <!-- 8: Mã vạch có số ở dưới (căn giữa) -->
          <div class="block block-8">
            <div class="block-inner">
              <img src="${currentBarcodeDataURL}" class="barcode-img-dynamic" alt="Barcode">
              ${showText ? `<div class="barcode-numbers" id="barcodeNumTxt">${formatBarcodeNumbers(barcodeText)}</div>` : ''}
            </div>
          </div>

          <!-- 9: Made in Vietnam + Năm sản xuất (xuống dòng, căn giữa) -->
          <div class="block block-9">
            <div class="block-inner">
              <div style="width:100%;">
                <div class="fit-text" id="madeInTxt" style="font-weight:700;">Made in Vietnam</div>
                <div class="fit-text" id="yearTxt">NĂM SẢN XUẤT: 2025</div>
              </div>
            </div>
          </div>
        `;

        // Hiện bản xem trước
        document.getElementById('labelResult').style.display = 'block';

        // Co chữ cho các khối text
        fitTextToBlock(document.getElementById('brandNameTxt'),     80, 12);
        fitTextToBlock(document.getElementById('productNameTxt'),   70, 12);
        fitTextToBlock(document.getElementById('technicalNameTxt'), 60, 12);
        fitTextToBlock(document.getElementById('companyTxt'),       28, 10);
        fitTextToBlock(document.getElementById('phoneTxt'),         40, 12);
        fitTextToBlock(document.getElementById('madeInTxt'),        36, 12);
        fitTextToBlock(document.getElementById('yearTxt'),          28, 10);
        const numTxt = document.getElementById('barcodeNumTxt');
        if (numTxt) fitTextToBlock(numTxt, 28, 10);

        // Co chữ trong bảng khối 5
        fitSpecTable();

      } catch (error) {
        console.error('Label generation error:', error);
        alert('Lỗi tạo tem: ' + error.message);
      }
    }

    function formatBarcodeNumbers(barcode) {
      if (barcode.length >= 13) {
        return `${barcode[0]} ${barcode.substring(1, 7)} ${barcode.substring(7, 13)}`;
      }
      return barcode;
    }

    // Xuất PDF: chụp DOM và dán lên PDF, chỉ 2 khổ 350x500 hoặc 150x200
    async function downloadLabelPDF() {
      const labelElement = document.getElementById('labelPreview');
      if (!labelElement || !labelElement.innerHTML) {
        alert('Vui lòng tạo tem trước khi tải xuống!'); return;
      }
      const size = document.getElementById('labelSize').value;
      const pageW = (size === '70x100') ? 350 : 150; // mm
      const pageH = (size === '70x100') ? 500 : 200; // mm

      // Chụp DOM với độ phân giải cao
      const canvas = await html2canvas(labelElement, {
        backgroundColor: '#ffffff',
        scale: 2,
        useCORS: true
      });
      const imgData = canvas.toDataURL('image/png');
      const { jsPDF } = window.jspdf;
      const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: [pageW, pageH] });
      pdf.addImage(imgData, 'PNG', 0, 0, pageW, pageH);
      pdf.save(`tem-${size}-${Date.now()}.pdf`);
    }

    function printLabel() { window.print(); }

    // Khởi tạo: không gán input cho toàn trang, không auto gọi generateLabel
    document.addEventListener('DOMContentLoaded', function() {
      // cho phép người dùng tự bấm nút để tạo tem; riêng mã vạch có thể tạo sẵn
      generateBarcode();
    });

    // === BỎ TOÀN BỘ auto-update trên input/select theo yêu cầu ===
    // (Không gán sự kiện input để gọi generateLabel/generateBarcode tự động)

  </script>
</body>
</html>
