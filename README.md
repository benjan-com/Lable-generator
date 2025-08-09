<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nextwaves Industries - Tạo Mã Vạch & Tem Sản Phẩm</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <!-- Chụp DOM -->
  <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh; padding: 20px;
    }
    .container {
      max-width: 1000px; margin: 0 auto; background: white; border-radius: 20px;
      box-shadow: 0 20px 40px rgba(0,0,0,.1); overflow: hidden;
    }
    .header {
      background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
      color: white; padding: 30px 40px; text-align: center; position: relative;
    }
    .logo { max-width: 200px; height: auto; margin-bottom: 15px; filter: brightness(0) invert(1); }
    .header h1 { font-size: 2.2rem; margin-bottom: 10px; font-weight: 700; }
    .header p { font-size: 1.1rem; opacity: .9; }

    .tabs { display: flex; background: #f8f9fa; border-bottom: 1px solid #dee2e6; }
    .tab { flex: 1; padding: 20px; text-align: center; background: none; border: none;
      font-size: 1.1rem; font-weight: 600; color: #6c757d; cursor: pointer; transition: .3s; position: relative; }
    .tab.active { color: #007bff; background: white; }
    .tab.active::after { content:''; position:absolute; bottom:0; left:0; right:0; height:3px; background:#007bff; }

    .tab-content { display: none; padding: 40px; }
    .tab-content.active { display: block; }

    .form-group { margin-bottom: 25px; }
    .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 25px; }
    .form-row-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px; margin-bottom: 25px; }

    label { display:block; margin-bottom:8px; font-weight:600; color:#333; font-size:.95rem; }
    input[type="text"], input[type="number"], select {
      width:100%; padding:12px 16px; border:2px solid #e9ecef; border-radius:8px; font-size:1rem;
      transition:border-color .3s; background:white;
    }
    input[type="text"]:focus, input[type="number"]:focus, select:focus {
      outline:none; border-color:#007bff; box-shadow:0 0 0 3px rgba(0,123,255,.1);
    }
    input[type="checkbox"] { margin-right:8px; transform:scale(1.2); }
    .checkbox-group { display:flex; align-items:center; margin-top:10px; }

    .btn {
      background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
      color:white; padding:14px 28px; border:none; border-radius:8px; font-size:1rem; font-weight:600;
      cursor:pointer; transition:.3s; margin:8px; box-shadow:0 4px 15px rgba(0,123,255,.3);
    }
    .btn:hover { transform: translateY(-2px); box-shadow:0 6px 20px rgba(0,123,255,.4); }
    .btn-success { background: linear-gradient(135deg, #28a745 0%, #20c997 100%); box-shadow:0 4px 15px rgba(40,167,69,.3); }
    .btn-success:hover { box-shadow:0 6px 20px rgba(40,167,69,.4); }
    .btn-danger { background: linear-gradient(135deg, #dc3545 0%, #fd7e14 100%); box-shadow:0 4px 15px rgba(220,53,69,.3); }
    .btn-danger:hover { box-shadow:0 6px 20px rgba(220,53,69,.4); }

    .result-section { margin-top:30px; padding:30px; background:#f8f9fa; border-radius:12px; text-align:center; }
    .barcode-container { background:white; padding:30px; border-radius:12px; margin:20px 0; box-shadow:0 4px 15px rgba(0,0,0,.1); display:inline-block; }

    /* Label preview surface */
    .label-preview {
      background:white; border:2px dashed #999; margin:20px auto; font-family: Arial, sans-serif;
      position: relative; display: block; overflow: hidden;
    }
    /* Kích thước xem trước đúng mm */
    .label-70x100 { width: 350mm; height: 500mm; }
    .label-30x40  { width: 150mm; height: 200mm; }

    /* Bề mặt và 9 khối (layout tuyệt đối bằng % chiều cao) */
    .label-surface { position: relative; width:100%; height:100%; }
    .block {
      position: absolute; left: 10px; /* cách mép trái 10px */
      right: 0; display: flex; align-items: center; justify-content: flex-start;
    }
    .block-inner {
      width: 100%; height: 100%; padding: 5px; display:flex; align-items:center; justify-content:flex-start;
      overflow: hidden; /* giữ nội dung không tràn */
    }
    /* Canh giữa cho các khối 1,2,8,9 */
    .center .block-inner { justify-content: center; text-align: center; }
    /* Khối 1: chỉ 1px trên/dưới, không bắt buộc 5px trái/phải */
    .block-1 .block-inner { padding-top:1px; padding-bottom:1px; padding-left:0; padding-right:0; }

    /* Hình ảnh co giãn */
    .fit-img { max-width:100%; max-height:100%; object-fit: contain; }

    /* Bảng TSKT khối 5 */
    .spec-table { width:100%; border-collapse: collapse; font-weight: 600; }
    .spec-table th, .spec-table td { border: 2px solid #000; padding: 4px; text-align: left; }

    /* Văn bản tự co cỡ – sẽ được JS tối ưu để không tràn */
    .auto-text { font-weight: 600; line-height: 1.15; word-break: break-word; }

    .download-options { margin-top:20px; padding:20px; background:#e3f2fd; border-radius:8px; text-align:center; }
    .download-options h4 { margin-bottom:15px; color:#1976d2; }

    #barcodeCanvas, #labelBarcodeCanvas { display:none; }

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
          <input autocomplete="off" type="text" id="barcodeInput" placeholder="VD: 1234567890123">
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
          <input autocomplete="off" type="number" id="barcodeWidth" min="1" max="5" step="0.1" placeholder="VD: 2">
        </div>
        <div class="form-group">
          <label for="barcodeHeight">📐 Chiều cao (px):</label>
          <input autocomplete="off" type="number" id="barcodeHeight" min="20" max="200" placeholder="VD: 100">
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
          <input autocomplete="off" type="text" id="productName" placeholder="VD: ĂNG TEN MẶT ĐẤT TUYẾN TÍNH">
        </div>
        <div class="form-group">
          <label for="technicalName">🔧 Tên kỹ thuật:</label>
          <input autocomplete="off" type="text" id="technicalName" placeholder="VD: RA-UL-SFF-241401">
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="frequencyRange">📡 Dải tầng:</label>
          <input autocomplete="off" type="text" id="frequencyRange" placeholder="VD: 900 - 930 MHz">
        </div>
        <div class="form-group">
          <label for="gain">⚡ Độ lợi:</label>
          <input autocomplete="off" type="text" id="gain" placeholder="VD: 7.5 dBi">
        </div>
      </div>

      <div class="form-group">
        <label for="labelBarcodeText">📊 Mã vạch (EAN-13):</label>
        <input autocomplete="off" type="text" id="labelBarcodeText" placeholder="VD: 8936236710036">
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
          <input autocomplete="off" type="number" id="labelBarcodeWidth" min="1" max="5" step="0.1" placeholder="VD: 2">
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="labelBarcodeHeight">📐 Chiều cao (px):</label>
          <input autocomplete="off" type="number" id="labelBarcodeHeight" min="20" max="100" placeholder="VD: 50">
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
        <button class="btn btn-success" onclick="downloadLabelPDF('single')">📄 Tải xuống 1 tem PDF</button>
      </div>

      <div id="labelResult" class="result-section" style="display: none;">
        <h3>✅ Tem sản phẩm đã được tạo thành công!</h3>
        <div id="labelPreview" class="label-preview"></div>
      </div>
    </div>
  </div>

  <canvas id="barcodeCanvas" style="display:none;"></canvas>
  <canvas id="labelBarcodeCanvas" style="display:none;"></canvas>

  <script>
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

    /* ---- Mã vạch (tab 1) chỉ chạy khi bấm nút ---- */
    function generateBarcode() {
      const text = document.getElementById('barcodeInput').value;
      const format = document.getElementById('barcodeFormat').value;
      const width = parseFloat(document.getElementById('barcodeWidth').value || '2');
      const height = parseInt(document.getElementById('barcodeHeight').value || '100');
      const showText = document.getElementById('showText').checked;

      if (!text) { alert('Vui lòng nhập dãy số!'); return; }

      try {
        JsBarcode("#barcodeDisplay", text, {
          format, width, height, displayValue: showText,
          background:"#ffffff", lineColor:"#000000", margin:10, fontSize:14,
          textAlign:"center", textPosition:"bottom"
        });
        document.getElementById('barcodeResult').style.display = 'block';
      } catch (error) {
        alert('Lỗi tạo mã vạch: ' + error.message);
      }
    }

    function downloadBarcode() {
      const barcodeElement = document.getElementById('barcodeDisplay');
      if (!barcodeElement || !barcodeElement.innerHTML) {
        alert('Vui lòng tạo mã vạch trước khi tải xuống!');
        return;
      }
      try {
        const svgElement = barcodeElement;
        const svgData = new XMLSerializer().serializeToString(svgElement);
        const canvas = document.getElementById('barcodeCanvas');
        const ctx = canvas.getContext('2d');
        const img = new Image();
        img.onload = function() {
          canvas.width = img.width; canvas.height = img.height;
          ctx.fillStyle = 'white'; ctx.fillRect(0,0,canvas.width,canvas.height);
          ctx.drawImage(img, 0, 0);
          canvas.toBlob(function(blob) {
            if (blob && blob.size > 0) {
              const link = document.createElement('a');
              link.download = `ma-vach-${(document.getElementById('barcodeInput').value || 'barcode')}-${Date.now()}.png`;
              link.href = URL.createObjectURL(blob);
              document.body.appendChild(link); link.click();
              document.body.removeChild(link); URL.revokeObjectURL(link.href);
            } else { alert('Lỗi tạo file. Vui lòng thử lại!'); }
          }, 'image/png');
        };
        img.src = 'data:image/svg+xml;base64,' + btoa(unescape(encodeURIComponent(svgData)));
      } catch (error) {
        alert('Lỗi tải xuống: ' + error.message);
      }
    }

    /* ---- Tạo PNG mã vạch cho khối 8 của tem ---- */
    function generateLabelBarcode(text, format, width, height, showText) {
      return new Promise((resolve, reject) => {
        try {
          const canvas = document.getElementById('labelBarcodeCanvas');
          JsBarcode(canvas, text, {
            format, width, height, displayValue: showText,
            background:"#ffffff", lineColor:"#000000", margin:5,
            fontSize:12, textAlign:"center", textPosition:"bottom"
          });
          resolve(canvas.toDataURL('image/png'));
        } catch (error) { reject(error); }
      });
    }

    /* ---- Auto fit text vào khối (giảm cỡ đến khi vừa) ---- */
    function fitText(el, maxPx, minPx) {
      let size = maxPx;
      el.style.fontSize = size + 'px';
      const parent = el.parentElement;
      // Giảm dần cho tới khi không tràn
      while ((el.scrollHeight > parent.clientHeight || el.scrollWidth > parent.clientWidth) && size > minPx) {
        size -= 1;
        el.style.fontSize = size + 'px';
      }
    }

    /* ---- Tạo Tem (chỉ khi bấm nút) ---- */
    async function generateLabel() {
      const size = document.getElementById('labelSize').value;
      const productName   = document.getElementById('productName').value || '';
      const technicalName = document.getElementById('technicalName').value || '';
      const frequencyRange= document.getElementById('frequencyRange').value || '';
      const gain          = document.getElementById('gain').value || '';
      const barcodeText   = document.getElementById('labelBarcodeText').value || '';

      // Thiết lập mã vạch – LUÔN có số ở dưới theo yêu cầu khối 8
      const barcodeFormat = document.getElementById('labelBarcodeFormat').value;
      const barcodeWidth  = parseFloat(document.getElementById('labelBarcodeWidth').value || '2');
      const barcodeHeight = parseInt(document.getElementById('labelBarcodeHeight').value || '50');
      const showText      = true;

      try {
        currentBarcodeDataURL = await generateLabelBarcode(barcodeText || '0000000000000', barcodeFormat, barcodeWidth, barcodeHeight, showText);

        const preview = document.getElementById('labelPreview');
        preview.className = `label-preview label-${size}`;
        // Bề mặt
        preview.innerHTML = `
          <div class="label-surface" id="labelSurface">
            <div class="block block-1 center"><div class="block-inner">
              <img class="fit-img" src="https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png" alt="logo">
            </div></div>
            <div class="block block-2 center"><div class="block-inner">
              <div class="auto-text" id="brandName">Nextwaves</div>
            </div></div>
            <div class="block block-3"><div class="block-inner">
              <div class="auto-text" id="prodName" style="font-weight:800;">${productName}</div>
            </div></div>
            <div class="block block-4"><div class="block-inner">
              <div class="auto-text" id="techName" style="font-weight:800;">${technicalName}</div>
            </div></div>
            <div class="block block-5"><div class="block-inner">
              <table class="spec-table">
                <tr><th style="width:40%;">DẢI TẦN</th><td style="width:60%;">${frequencyRange}</td></tr>
                <tr><th>ĐỘ LỢI</th><td>${gain}</td></tr>
              </table>
            </div></div>
            <div class="block block-6"><div class="block-inner">
              <div class="auto-text" id="companyInfo" style="font-weight:500;">
                Sản xuất bởi công ty TNHH Nextwaves Industries – 20/23 đường 35, Phường An Khánh, Thành Phố Hồ Chí Minh
              </div>
            </div></div>
            <div class="block block-7"><div class="block-inner">
              <div class="auto-text" id="phone" style="font-weight:800;">0938888373</div>
            </div></div>
            <div class="block block-8 center"><div class="block-inner" style="flex-direction:column;">
              <img src="${currentBarcodeDataURL}" alt="barcode" class="fit-img" style="flex:1;">
              <div class="auto-text" id="barcodeNumbers" style="margin-top:4px;">${formatBarcodeNumbers(barcodeText)}</div>
            </div></div>
            <div class="block block-9 center"><div class="block-inner" style="flex-direction:column;">
              <div class="auto-text" id="madeIn" style="font-weight:800; margin-bottom:4px;">Made in Vietnam</div>
              <div class="auto-text" id="yearLine" style="font-weight:600;">NĂM SẢN XUẤT: 2025</div>
            </div></div>
          </div>
        `;

        // Tính layout tuyệt đối theo % chiều cao (2% trên, 1% giữa, 2% dưới)
        applyBlocksLayout();

        // Co chữ cho các vùng văn bản để không tràn khối
        const maxMap = {
          brandName: 120, prodName: 120, techName: 110, companyInfo: 80,
          phone: 100, barcodeNumbers: 60, madeIn: 100, yearLine: 80
        };
        Object.keys(maxMap).forEach(id => {
          const el = document.getElementById(id);
          if (el) fitText(el, maxMap[id], 8);
        });

        document.getElementById('labelResult').style.display = 'block';
      } catch (error) {
        console.error('Label generation error:', error);
        alert('Lỗi tạo tem: ' + error.message);
      }
    }

    // Tính vị trí/chiều cao 9 khối theo phần trăm chiều cao
    function applyBlocksLayout() {
      const surface = document.getElementById('labelSurface');
      const H = surface.clientHeight;
      const topPad = 0.02 * H;
      const gap = 0.01 * H;
      const heights = [12,3,6,6,14,9,6,25,7].map(p => p/100 * H);

      let top = topPad;
      for (let i=0;i<9;i++){
        const b = surface.querySelector('.block-' + (i+1));
        b.style.top = Math.round(top) + 'px';
        b.style.height = Math.round(heights[i]) + 'px';
        // canh lề: khối 1,2,8,9 giữa
        if ([1,2,8,9].includes(i+1)) b.classList.add('center'); else b.classList.remove('center');
        top += heights[i];
        if (i<8) top += gap;
      }
      // Đệm dưới 2% đã bảo toàn do tổng 88% + 8*1% + 2% top = 98% -> còn 2% cuối
    }

    function formatBarcodeNumbers(barcode) {
      if (!barcode) return '';
      if (barcode.length >= 13) return `${barcode[0]} ${barcode.substring(1,7)} ${barcode.substring(7,13)}`;
      return barcode;
    }

    /* ---- Xuất PDF: chụp toàn bộ DOM rồi dán lên PDF, đúng khổ 350x500 hoặc 150x200 ---- */
    async function downloadLabelPDF() {
      const labelElement = document.getElementById('labelPreview');
      if (!labelElement || !labelElement.innerHTML) {
        alert('Vui lòng tạo tem trước khi tải xuống!');
        return;
      }
      const size = document.getElementById('labelSize').value;
      const pageW = (size === '70x100') ? 350 : 150; // mm
      const pageH = (size === '70x100') ? 500 : 200; // mm

      // Chụp DOM
      const canvas = await html2canvas(labelElement, { scale: 3, backgroundColor: '#ffffff', useCORS: true });
      const imgData = canvas.toDataURL('image/png');

      const { jsPDF } = window.jspdf;
      const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: [pageW, pageH] });
      pdf.addImage(imgData, 'PNG', 0, 0, pageW, pageH);
      pdf.save(`tem-${size}-${Date.now()}.pdf`);
    }

    function printLabel(){ window.print(); }

    /* LƯU Ý: Theo yêu cầu, KHÔNG gán sự kiện 'input' toàn trang, KHÔNG auto-generate. */
  </script>
</body>
</html>
