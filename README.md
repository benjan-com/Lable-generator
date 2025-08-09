<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nextwaves Industries - Tạo Mã Vạch & Tem Sản Phẩm</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <!-- Chụp DOM để dán vào PDF -->
  <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    *{margin:0;padding:0;box-sizing:border-box;}
    body{
      font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);
      min-height:100vh; padding:20px;
    }
    .container{max-width:1000px;margin:0 auto;background:#fff;border-radius:20px;box-shadow:0 20px 40px rgba(0,0,0,.1);overflow:hidden;}
    .header{background:linear-gradient(135deg,#4facfe 0%,#00f2fe 100%);color:#fff;padding:30px 40px;text-align:center;position:relative;}
    .logo{max-width:200px;height:auto;margin-bottom:15px;filter:brightness(0) invert(1);}
    .header h1{font-size:2.2rem;margin-bottom:10px;font-weight:700;}
    .header p{font-size:1.1rem;opacity:.9;}

    .tabs{display:flex;background:#f8f9fa;border-bottom:1px solid #dee2e6;}
    .tab{flex:1;padding:20px;text-align:center;background:none;border:none;font-size:1.1rem;font-weight:600;color:#6c757d;cursor:pointer;transition:.3s;position:relative;}
    .tab.active{color:#007bff;background:#fff;}
    .tab.active::after{content:'';position:absolute;bottom:0;left:0;right:0;height:3px;background:#007bff;}
    .tab-content{display:none;padding:40px;}
    .tab-content.active{display:block;}

    .form-group{margin-bottom:25px;}
    .form-row{display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:25px;}
    .form-row-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:15px;margin-bottom:25px;}
    label{display:block;margin-bottom:8px;font-weight:600;color:#333;font-size:.95rem;}
    input[type="text"],input[type="number"],select,input[type="date"]{
      width:100%;padding:12px 16px;border:2px solid #e9ecef;border-radius:8px;font-size:1rem;transition:border-color .3s;background:#fff;
    }
    input[type="text"]:focus,input[type="number"]:focus,select:focus,input[type="date"]:focus{
      outline:none;border-color:#007bff;box-shadow:0 0 0 3px rgba(0,123,255,.1);
    }
    /* Placeholder mờ, không chọn, ẩn khi focus */
    input::placeholder{color:#adb5bd;user-select:none;opacity:.9;}
    input:focus::placeholder{color:transparent;opacity:0;}

    input[type="checkbox"]{margin-right:8px;transform:scale(1.2);}
    .checkbox-group{display:flex;align-items:center;margin-top:10px;}

    .btn{background:linear-gradient(135deg,#007bff 0%,#0056b3 100%);color:#fff;padding:14px 28px;border:none;border-radius:8px;font-size:1rem;font-weight:600;cursor:pointer;transition:.3s;margin:8px;box-shadow:0 4px 15px rgba(0,123,255,.3);}
    .btn:hover{transform:translateY(-2px);box-shadow:0 6px 20px rgba(0,123,255,.4);}
    .btn-success{background:linear-gradient(135deg,#28a745 0%,#20c997 100%);box-shadow:0 4px 15px rgba(40,167,69,.3);}
    .btn-success:hover{box-shadow:0 6px 20px rgba(40,167,69,.4);}
    .btn-danger{background:linear-gradient(135deg,#dc3545 0%,#fd7e14 100%);box-shadow:0 4px 15px rgba(220,53,69,.3);}
    .btn-danger:hover{box-shadow:0 6px 20px rgba(220,53,69,.4);}

    .result-section{margin-top:30px;padding:30px;background:#f8f9fa;border-radius:12px;text-align:center;}
    .barcode-container{background:#fff;padding:30px;border-radius:12px;margin:20px 0;box-shadow:0 4px 15px rgba(0,0,0,.1);display:inline-block;}

    /* PREVIEW TEM */
    .label-preview{
      background:#fff;border:2px dashed #999;margin:20px auto;font-family:Arial, sans-serif;position:relative;display:inline-block;
      container-type:size; /* cho đơn vị cqh/cqw để co chữ theo khung */
    }
    .label-70x100{width:350px;height:500px;}
    .label-30x40{width:150px;height:200px;}

    /* Khung 9 khối theo % + khoảng cách 1% + top 2% + bottom 2% */
    .label-grid{
      height:100%;
      display:flex;flex-direction:column;
      gap:1%;
      padding-top:2%;
      padding-bottom:2%;
      padding-left:10px; /* Tất cả khối cách mép trái 10px */
    }
    .block{flex:0 0 auto;display:flex;align-items:center;}
    .block .inner{width:100%;height:100%;display:flex;align-items:center;padding-top:5px;padding-bottom:5px;}
    .block.center{justify-content:center;text-align:center;}
    /* Chiều cao từng khối */
    .block-1{flex-basis:13%;}
    .block-2{flex-basis:3%;}
    .block-3{flex-basis:4%;}
    .block-4{flex-basis:3%;}
    .block-5{flex-basis:25%;}
    .block-6{flex-basis:5%;}
    .block-7{flex-basis:3%;}
    .block-8{flex-basis:25%;}
    .block-9{flex-basis:7%;}
    /* Riêng khối 1: chỉ cách mép trên/dưới 1px */
    .block-1 .inner{padding-top:1px;padding-bottom:1px;}

    /* Logo khối 1 tự co giãn */
    .logo-in-label{max-height:100%;max-width:100%;object-fit:contain;margin:0 auto;}

    /* Text tự co giãn theo khung bằng cqh */
    .brand{font-weight:700;font-size:clamp(10px,3.5cqh,28px);margin:0 auto;}
    .prod-name{font-weight:800;font-size:clamp(12px,4.2cqh,34px);line-height:1.1;}
    .tech-name{font-weight:800;font-size:clamp(10px,3.2cqh,26px);}
    .company{font-size:clamp(9px,2.2cqh,18px);line-height:1.2;}
    .phone{font-weight:800;font-size:clamp(10px,3cqh,24px);}
    .madein{font-weight:800;font-size:clamp(10px,3cqh,24px);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;}
    .mfgdate{font-weight:700;font-size:clamp(10px,2.6cqh,22px);}

    /* Bảng khối 5 chiếm toàn bộ chiều cao khối */
    .spec-table{width:100%;height:100%;border:2px solid #000;border-collapse:collapse;table-layout:fixed;}
    .spec-table tr{height:50%;}
    .spec-table th,.spec-table td{
      border:2px solid #000;text-align:left;vertical-align:middle;
      padding:2px 6px;font-weight:700;font-size:clamp(10px,2.6cqh,22px);
      overflow:hidden;
    }
    .spec-table th{background:#f0f0f0;width:40%;}
    .spec-table td{width:60%;}

    /* Barcode khối 8 tự co dãn tối đa */
    .barcode-wrap{width:100%;height:100%;display:flex;flex-direction:column;align-items:center;justify-content:center;}
    .barcode-img{max-width:100%;max-height:80%;object-fit:contain;}
    .barcode-numbers{margin-top:3px;letter-spacing:1px;font-size:clamp(10px,2.4cqh,20px);}

    .download-options{margin-top:20px;padding:20px;background:#e3f2fd;border-radius:8px;text-align:center;}
    .download-options h4{margin-bottom:15px;color:#1976d2;}

    #barcodeCanvas,#labelBarcodeCanvas{display:none;}

    @media (max-width:768px){
      .form-row,.form-row-3{grid-template-columns:1fr;}
      .tabs{flex-direction:column;}
      .container{margin:10px;border-radius:15px;}
      .header{padding:20px;}
      .header h1{font-size:1.8rem;}
      .tab-content{padding:20px;}
    }
    @media print{
      body{margin:0;padding:0;background:#fff;}
      .container{box-shadow:none;margin:0;padding:0;}
      .header,.tabs,.tab-content:not(.active),.btn,.download-options{display:none;}
      .label-preview{border:1px solid #000;margin:0;}
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
          <input type="text" id="barcodeInput" placeholder="VD: 1234567890123">
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

      <div style="text-align:center;">
        <button class="btn" onclick="generateBarcode()">🎯 Tạo Mã Vạch</button>
        <button class="btn btn-success" onclick="downloadBarcode()">📥 Tải xuống PNG</button>
      </div>

      <div id="barcodeResult" class="result-section" style="display:none;">
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
          <input type="text" id="productName" placeholder="VD: ĂNG TEN MẶT ĐẤT TUYẾN TÍNH">
        </div>
        <div class="form-group">
          <label for="technicalName">🔧 Tên kỹ thuật:</label>
          <input type="text" id="technicalName" placeholder="VD: RA-UL-SFF-241401">
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="frequencyRange">📡 Dải tầng:</label>
          <input type="text" id="frequencyRange" placeholder="VD: 900 - 930 MHz">
        </div>
        <div class="form-group">
          <label for="gain">⚡ Độ lợi:</label>
          <input type="text" id="gain" placeholder="VD: 7.5 dBi">
        </div>
      </div>

      <div class="form-group">
        <label for="labelBarcodeText">📊 Mã vạch (EAN-13):</label>
        <input type="text" id="labelBarcodeText" placeholder="VD: 8936236710036">
      </div>

      <!-- Ngày sản xuất -->
      <div class="form-group">
        <label for="manufactureDate">📅 Ngày sản xuất:</label>
        <input type="date" id="manufactureDate">
      </div>

      <h4 style="margin:25px 0 15px 0;color:#333;">🎛️ Tùy chỉnh mã vạch trên tem:</h4>
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

      <div style="text-align:center;">
        <button class="btn" onclick="generateLabel()">🎯 Tạo Tem Sản Phẩm</button>
        <button class="btn" onclick="printLabel()">🖨️ In Tem</button>
      </div>

      <div class="download-options">
        <h4>📥 Tùy chọn tải xuống PDF</h4>
        <button class="btn btn-success" onclick="downloadLabelPDF()">📄 Tải xuống 1 tem PDF</button>
      </div>

      <div id="labelResult" class="result-section" style="display:none;">
        <h3>✅ Tem sản phẩm đã được tạo thành công!</h3>
        <div id="labelPreview" class="label-preview"></div>
      </div>
    </div>
  </div>

  <canvas id="barcodeCanvas" style="display:none;"></canvas>
  <canvas id="labelBarcodeCanvas" style="display:none;"></canvas>

  <script>
    // Global
    let currentTab = 'barcode';
    let currentBarcodeDataURL = '';

    function switchTab(tab){
      currentTab = tab;
      document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
      document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active'));
      if(tab==='barcode'){
        document.querySelector('.tab:first-child').classList.add('active');
        document.getElementById('barcode-tab').classList.add('active');
      }else{
        document.querySelector('.tab:last-child').classList.add('active');
        document.getElementById('label-tab').classList.add('active');
      }
    }

    // BARCODE TAB
    function generateBarcode(){
      const text = document.getElementById('barcodeInput').value;
      const format = document.getElementById('barcodeFormat').value;
      const width = parseFloat(document.getElementById('barcodeWidth').value);
      const height = parseInt(document.getElementById('barcodeHeight').value);
      const showText = document.getElementById('showText').checked;
      if(!text){ alert('Vui lòng nhập dãy số!'); return; }
      try{
        JsBarcode("#barcodeDisplay", text, {
          format,width,height,displayValue:showText,
          background:"#ffffff",lineColor:"#000000",margin:10,fontSize:14,
          textAlign:"center",textPosition:"bottom"
        });
        document.getElementById('barcodeResult').style.display='block';
      }catch(e){ alert('Lỗi tạo mã vạch: '+e.message); }
    }

    function downloadBarcode(){
      const barcodeElement = document.getElementById('barcodeDisplay');
      if(!barcodeElement || !barcodeElement.innerHTML){
        alert('Vui lòng tạo mã vạch trước khi tải xuống!'); return;
      }
      try{
        const svgElement = barcodeElement;
        const svgData = new XMLSerializer().serializeToString(svgElement);
        const canvas = document.getElementById('barcodeCanvas');
        const ctx = canvas.getContext('2d');
        const img = new Image();
        img.onload = function(){
          canvas.width = img.width; canvas.height = img.height;
          ctx.fillStyle='white'; ctx.fillRect(0,0,canvas.width,canvas.height);
          ctx.drawImage(img,0,0);
          canvas.toBlob(function(blob){
            if(blob.size>0){
              const link=document.createElement('a');
              link.download=`ma-vach-${document.getElementById('barcodeInput').value}-${Date.now()}.png`;
              link.href=URL.createObjectURL(blob);
              document.body.appendChild(link); link.click(); document.body.removeChild(link);
              URL.revokeObjectURL(link.href);
            }else{ alert('Lỗi tạo file. Vui lòng thử lại!'); }
          },'image/png');
        };
        img.src='data:image/svg+xml;base64,'+btoa(unescape(encodeURIComponent(svgData)));
      }catch(e){ alert('Lỗi tải xuống: '+e.message); }
    }

    // LABEL BARCODE
    function generateLabelBarcode(text, format, width, height, showText){
      return new Promise((resolve,reject)=>{
        try{
          const canvas = document.getElementById('labelBarcodeCanvas');
          JsBarcode(canvas, text, {
            format,width,height,displayValue:showText,
            background:"#ffffff",lineColor:"#000000",margin:5,fontSize:12,
            textAlign:"center",textPosition:"bottom"
          });
          const dataURL = canvas.toDataURL('image/png');
          resolve(dataURL);
        }catch(err){ reject(err); }
      });
    }

    // FORMATTERS
    function formatBarcodeNumbers(barcode){
      if(barcode.length>=13){ return `${barcode[0]} ${barcode.substring(1,7)} ${barcode.substring(7,13)}`; }
      return barcode;
    }
    function formatDateToDDMMYYYY(val){
      if(!val) return '';
      const d = new Date(val);
      if(isNaN(d)) return '';
      const dd = String(d.getDate()).padStart(2,'0');
      const mm = String(d.getMonth()+1).padStart(2,'0');
      const yyyy = d.getFullYear();
      return `${dd}${mm}${yyyy}`;
    }

    // GENERATE LABEL (chỉ khi bấm nút)
    async function generateLabel(){
      const size = document.getElementById('labelSize').value;
      const productName = document.getElementById('productName').value || '';
      const technicalName = document.getElementById('technicalName').value || '';
      const frequencyRange = document.getElementById('frequencyRange').value || '';
      const gain = document.getElementById('gain').value || '';
      const barcodeText = document.getElementById('labelBarcodeText').value || '';
      const mfgDate = document.getElementById('manufactureDate').value || '';
      const dateOut = formatDateToDDMMYYYY(mfgDate);

      // Barcode options
      const barcodeFormat = document.getElementById('labelBarcodeFormat').value;
      const barcodeWidth = parseFloat(document.getElementById('labelBarcodeWidth').value);
      const barcodeHeight = parseInt(document.getElementById('labelBarcodeHeight').value);
      const showText = document.getElementById('labelShowText').checked;

      try{
        currentBarcodeDataURL = barcodeText ? await generateLabelBarcode(barcodeText, barcodeFormat, barcodeWidth, barcodeHeight, showText) : '';
        const labelPreview = document.getElementById('labelPreview');
        labelPreview.className = `label-preview label-${size}`;
        labelPreview.innerHTML = `
          <div class="label-grid">
            <!-- Khối 1: Logo (13%) -->
            <div class="block block-1 center">
              <div class="inner">
                <img class="logo-in-label" src="https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png" alt="">
              </div>
            </div>
            <!-- Khối 2: Tên thương hiệu (3%) -->
            <div class="block block-2 center"><div class="inner"><div class="brand">Nextwaves</div></div></div>
            <!-- Khối 3: Tên sản phẩm (4%) -->
            <div class="block block-3"><div class="inner"><div class="prod-name">${productName}</div></div></div>
            <!-- Khối 4: Tên kỹ thuật (3%) -->
            <div class="block block-4"><div class="inner"><div class="tech-name">${technicalName}</div></div></div>
            <!-- Khối 5: Bảng thông số (25%) -->
            <div class="block block-5"><div class="inner">
              <table class="spec-table">
                <tr>
                  <th>DẢI TẦN</th><td>${frequencyRange}</td>
                </tr>
                <tr>
                  <th>ĐỘ LỢI</th><td>${gain}</td>
                </tr>
              </table>
            </div></div>
            <!-- Khối 6: Thông tin công ty (5%) -->
            <div class="block block-6"><div class="inner">
              <div class="company">
                Sản xuất bởi công ty TNHH Nextwaves Industries – 20/23 đường 35, Phường An Khánh, Thành Phố Hồ Chí Minh
              </div>
            </div></div>
            <!-- Khối 7: SĐT (3%) -->
            <div class="block block-7"><div class="inner"><div class="phone">0938888373</div></div></div>
            <!-- Khối 8: Mã vạch (25%) -->
            <div class="block block-8 center"><div class="inner">
              <div class="barcode-wrap">
                ${currentBarcodeDataURL ? `<img src="${currentBarcodeDataURL}" class="barcode-img" alt="Barcode">` : ''}
                ${showText && barcodeText ? `<div class="barcode-numbers">${formatBarcodeNumbers(barcodeText)}</div>` : ''}
              </div>
            </div></div>
            <!-- Khối 9: Made in + Ngày (7%) -->
            <div class="block block-9 center"><div class="inner">
              <div class="madein">
                <div>Made in Vietnam</div>
                <div class="mfgdate">${dateOut}</div>
              </div>
            </div></div>
          </div>
        `;
        document.getElementById('labelResult').style.display = 'block';
      }catch(err){
        console.error('Label generation error:', err);
        alert('Lỗi tạo tem: '+err.message);
      }
    }

    // DOWNLOAD PDF: chụp DOM và dán lên PDF, chỉ 350×500 hoặc 150×200 mm
    async function downloadLabelPDF(){
      const labelElement = document.getElementById('labelPreview');
      if(!labelElement || !labelElement.innerHTML){
        alert('Vui lòng tạo tem trước khi tải xuống!'); return;
      }
      const { jsPDF } = window.jspdf;
      const size = document.getElementById('labelSize').value;
      const pdfSize = (size==='70x100') ? [350,500] : [150,200]; // mm
      // Chụp DOM
      const canvas = await html2canvas(labelElement, {backgroundColor:'#ffffff', scale:2});
      const imgData = canvas.toDataURL('image/png');
      const pdf = new jsPDF({orientation:'portrait', unit:'mm', format:pdfSize});
      pdf.addImage(imgData, 'PNG', 0, 0, pdfSize[0], pdfSize[1]);
      pdf.save(`tem-${size}-${Date.now()}.pdf`);
    }

    // PRINT
    function printLabel(){ window.print(); }

    // Không gán input auto-update & không tự gọi generateLabel()
    // (Cố ý bỏ đoạn addEventListener('input', ...) và DOMContentLoaded auto-generate của bản cũ)
  </script>
</body>
</html>
