<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Nextwaves Industries - Tạo Mã Vạch & Tem Sản Phẩm</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    *{margin:0;padding:0;box-sizing:border-box}
    body{
      font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);
      min-height:100vh;padding:20px;
      -webkit-print-color-adjust:exact;
      print-color-adjust:exact;
    }
    .container{max-width:1000px;margin:0 auto;background:#fff;border-radius:20px;box-shadow:0 20px 40px rgba(0,0,0,.1);overflow:hidden}
    .header{background:linear-gradient(135deg,#4facfe 0%,#00f2fe 100%);color:#fff;padding:30px 40px;text-align:center;position:relative}
    .logo{max-width:200px;height:auto;margin-bottom:15px;filter:brightness(0) invert(1)}
    .header h1{font-size:2.2rem;margin-bottom:10px;font-weight:700}
    .header p{font-size:1.1rem;opacity:.9}

    .tabs{display:flex;background:#f8f9fa;border-bottom:1px solid #dee2e6}
    .tab{flex:1;padding:20px;text-align:center;background:none;border:none;font-size:1.1rem;font-weight:600;color:#6c757d;cursor:pointer;transition:.3s;position:relative}
    .tab.active{color:#007bff;background:#fff}
    .tab.active::after{content:'';position:absolute;bottom:0;left:0;right:0;height:3px;background:#007bff}

    .tab-content{display:none;padding:40px}
    .tab-content.active{display:block}

    .form-group{margin-bottom:25px}
    .form-row{display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:25px}
    .form-row-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:15px;margin-bottom:25px}

    label{display:block;margin-bottom:8px;font-weight:600;color:#333;font-size:.95rem}
    input[type="text"],input[type="number"],input[type="date"],select{
      width:100%;padding:12px 16px;border:2px solid #e9ecef;border-radius:8px;font-size:1rem;transition:border-color .3s;background:#fff
    }
    input::placeholder{color:#9ca3af;opacity:1}
    input:focus,select:focus{outline:none;border-color:#007bff;box-shadow:0 0 0 3px rgba(0,123,255,.1)}
    input[type="checkbox"]{margin-right:8px;transform:scale(1.2)}
    .checkbox-group{display:flex;align-items:center;margin-top:10px}

    .btn{background:linear-gradient(135deg,#007bff 0%,#0056b3 100%);color:#fff;padding:14px 28px;border:none;border-radius:8px;font-size:1rem;font-weight:600;cursor:pointer;transition:.3s;margin:8px;box-shadow:0 4px 15px rgba(0,123,255,.3)}
    .btn:hover{transform:translateY(-2px);box-shadow:0 6px 20px rgba(0,123,255,.4)}
    .btn-success{background:linear-gradient(135deg,#28a745 0%,#20c997 100%);box-shadow:0 4px 15px rgba(40,167,69,.3)}
    .btn-success:hover{box-shadow:0 6px 20px rgba(40,167,69,.4)}

    .result-section{margin-top:30px;padding:30px;background:#f8f9fa;border-radius:12px;text-align:center}
    .barcode-container{background:#fff;padding:30px;border-radius:12px;margin:20px 0;box-shadow:0 4px 15px rgba(0,0,0,.1);display:inline-block}

    /* -------- XEM TRƯỚC TEM: đúng 2 kích thước thực tế -------- */
    .label-preview{background:#fff;border:2px dashed #999;margin:20px auto;font-family:Arial,sans-serif;position:relative;display:inline-block;overflow:hidden}
    .label-70x100{width:70mm;height:100mm}
    .label-30x40{width:30mm;height:40mm}

    /* Lưới 9 khối + khoảng cách 1%, top 2%, bottom 2% */
    .label-canvas{
      display:grid;width:100%;height:100%;
      grid-template-rows:
        2%  /* top */
        12% /* b1 */
        1%  /* gap */
        3%  /* b2 */
        1%  /* gap */
        4%  /* b3 */
        1%  /* gap */
        3%  /* b4 */
        1%  /* gap */
        25% /* b5 */
        1%  /* gap */
        5%  /* b6 */
        1%  /* gap */
        3%  /* b7 */
        1%  /* gap */
        25% /* b8 */
        1%  /* gap */
        7%  /* b9 */
        2%  /* bottom */
      ;
      padding-left:10px;        /* mép trái giữ nguyên */
      padding-right:15px;       /* mép phải lùi vào thêm 5px như yêu cầu */
      --h:1000px;               /* set sau khi render */
      overflow:hidden;
    }

    /* KHỐI 1: logo */
    .b1{grid-row:2;display:flex;align-items:center;justify-content:center;text-align:center}
    .b1-inner{width:100%;height:100%;padding-top:1px;padding-bottom:1px}
    .b1-inner img{max-width:100%;max-height:100%;object-fit:contain;display:block;margin:0 auto}

    /* KHỐI 2: tên công ty dưới logo */
    .b2{grid-row:4;display:flex;align-items:center;justify-content:center;text-align:center;overflow:hidden}
    .b2-inner{width:100%;height:100%}
    .b2-inner .brand{
      font-weight:700;
      font-size:calc(var(--h)*0.018);   /* nhỏ hơn để không đè b3 */
      line-height:1.05;white-space:nowrap;letter-spacing:.5px
    }

    /* KHỐI 3: sản phẩm */
    .b3{grid-row:6;display:flex;align-items:center;justify-content:flex-start;text-align:left;overflow:hidden}
    .b3-inner{width:100%;height:100%}
    .b3-inner .product{
      font-weight:800;font-size:calc(var(--h)*0.03);line-height:1.05;
      white-space:nowrap;overflow:hidden;text-overflow:ellipsis
    }

    /* KHỐI 4: kỹ thuật */
    .b4{grid-row:8;display:flex;align-items:center;justify-content:flex-start;text-align:left;overflow:hidden}
    .b4-inner{width:100%;height:100%}
    .b4-inner .tech{font-weight:800;font-size:calc(var(--h)*0.022);line-height:1.05;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}

    /* KHỐI 5: grid 2x2, hai cột bằng nhau, viền 2px */
    .b5{grid-row:10;display:flex;align-items:stretch}
    .b5-inner{width:100%;height:100%;padding:0}
    .spec-grid{
      display:grid;width:100%;height:100%;
      grid-template-columns:1fr 1fr;   /* hai cột bằng nhau */
      grid-template-rows:1fr 1fr;
      border:2px solid #000
    }
    .cell{
      display:flex;align-items:center;
      padding:2px 4px;font-weight:700;
      font-size:calc(var(--h)*0.018);line-height:1.1;
      border:2px solid #000               /* tất cả nét 2px */
    }
    .cell.head{background:#f3f4f6}

    /* KHỐI 6: thông tin công ty căn trái */
    .b6{grid-row:12;display:flex;align-items:center;justify-content:flex-start;text-align:left;overflow:hidden}
    .b6-inner{width:100%;height:100%}
    .b6-inner .company{font-size:calc(var(--h)*0.018);line-height:1.2}

    /* KHỐI 7: điện thoại */
    .b7{grid-row:14;display:flex;align-items:center;justify-content:flex-start;text-align:left;overflow:hidden}
    .b7-inner{width:100%;height:100%}
    .b7-inner .phone{font-weight:800;font-size:calc(var(--h)*0.024);line-height:1.05;white-space:nowrap}

    /* KHỐI 8: mã vạch */
    .b8{grid-row:16;display:flex;align-items:center;justify-content:center;text-align:center;overflow:hidden}
    .b8-inner{width:100%;height:100%}
    .b8-inner img{width:100%;height:100%;object-fit:contain;display:block;margin:0 auto}

    /* KHỐI 9: Made in + ngày có cùng cỡ chữ, nhỏ hơn để không bị cắt */
    .b9{grid-row:18;display:flex;align-items:center;justify-content:center;text-align:center;overflow:hidden}
    .b9-inner{width:100%;height:100%}
    .b9-inner .madein,
    .b9-inner .mfgdate{
      font-weight:700;                 /* cùng trọng lượng cho đồng nhất thị giác */
      font-size:calc(var(--h)*0.028);  /* hai dòng cộng line-height < 7% chiều cao */
      line-height:1.05
    }

    #barcodeCanvas,#labelBarcodeCanvas{display:none}

    @media(max-width:768px){
      .form-row,.form-row-3{grid-template-columns:1fr}
      .tabs{flex-direction:column}
      .container{margin:10px;border-radius:15px}
      .header{padding:20px}
      .header h1{font-size:1.8rem}
      .tab-content{padding:20px}
    }
    @media print{
      body{margin:0;padding:0;background:#fff}
      .container{box-shadow:none;margin:0;padding:0}
      .header,.tabs,.tab-content:not(.active),.btn,.download-options{display:none}
      .label-preview{border:1px solid #000;margin:0}
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <img src="https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png" alt="Nextwaves Industries" class="logo" crossorigin="anonymous" onerror="this.style.display='none'">
      <h1>🏷️ Nextwaves Industries</h1>
      <p>Tạo Mã Vạch & Tem Sản Phẩm Chuyên Nghiệp</p>
    </div>

    <div class="tabs">
      <button class="tab active" onclick="switchTab('barcode')">📊 Tạo Mã Vạch</button>
      <button class="tab" onclick="switchTab('label')">🏷️ Tạo Tem Sản Phẩm</button>
    </div>

    <!-- TAB 1 -->
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
          <input type="number" id="barcodeWidth" min="1" max="5" step="0.1" placeholder="2">
        </div>
        <div class="form-group">
          <label for="barcodeHeight">📐 Chiều cao (px):</label>
          <input type="number" id="barcodeHeight" min="20" max="200" placeholder="100">
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

    <!-- TAB 2 -->
    <div id="label-tab" class="tab-content">
      <div class="form-group">
        <label for="labelSize">📏 Kích thước tem:</label>
        <select id="labelSize">
          <option value="70x100">70 x 100 mm</option>
          <option value="30x40">30 x 40 mm</option>
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

      <div class="form-row">
        <div class="form-group">
          <label for="labelBarcodeText">📊 Mã vạch (EAN-13):</label>
          <input type="text" id="labelBarcodeText" placeholder="VD: 8936236710036">
        </div>
        <div class="form-group">
          <label for="manufactureDate">📅 Ngày sản xuất:</label>
          <input type="date" id="manufactureDate">
        </div>
      </div>

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
          <input type="number" id="labelBarcodeWidth" min="1" max="5" step="0.1" placeholder="2">
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="labelBarcodeHeight">📐 Chiều cao (px):</label>
          <input type="number" id="labelBarcodeHeight" min="20" max="200" placeholder="100">
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
        <h4>📥 Tải xuống PDF</h4>
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
    let currentTab='barcode';
    let currentBarcodeDataURL='';

    function switchTab(tab){
      currentTab=tab;
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

    function generateBarcode(){
      const text=(document.getElementById('barcodeInput').value||'').trim();
      const format=document.getElementById('barcodeFormat').value;
      const width=parseFloat(document.getElementById('barcodeWidth').value||'2');
      const height=parseInt(document.getElementById('barcodeHeight').value||'100');
      const showText=document.getElementById('showText').checked;
      if(!text){alert('Vui lòng nhập dãy số!');return;}
      try{
        JsBarcode("#barcodeDisplay", text, {
          format, width, height,
          displayValue: showText,
          background:"#ffffff",lineColor:"#000000",
          margin:10,fontSize:14,textAlign:"center",textPosition:"bottom"
        });
        document.getElementById('barcodeResult').style.display='block';
      }catch(e){alert('Lỗi tạo mã vạch: '+e.message);}
    }

    function downloadBarcode(){
      const barcodeElement=document.getElementById('barcodeDisplay');
      if(!barcodeElement || !barcodeElement.innerHTML){alert('Vui lòng tạo mã vạch trước khi tải xuống!');return;}
      try{
        const svgData=new XMLSerializer().serializeToString(barcodeElement);
        const canvas=document.getElementById('barcodeCanvas');
        const ctx=canvas.getContext('2d'); const img=new Image();
        img.onload=function(){
          canvas.width=img.width; canvas.height=img.height;
          ctx.fillStyle='white'; ctx.fillRect(0,0,canvas.width,canvas.height);
          ctx.drawImage(img,0,0);
          canvas.toBlob(function(blob){
            if(blob && blob.size>0){
              const link=document.createElement('a');
              link.download=`ma-vach-${(document.getElementById('barcodeInput').value||'')}-${Date.now()}.png`;
              link.href=URL.createObjectURL(blob);
              document.body.appendChild(link); link.click();
              document.body.removeChild(link); URL.revokeObjectURL(link.href);
            }else alert('Lỗi tạo file. Vui lòng thử lại!');
          },'image/png');
        };
        img.src='data:image/svg+xml;base64,'+btoa(unescape(encodeURIComponent(svgData)));
      }catch(e){alert('Lỗi tải xuống: '+e.message);}
    }

    function generateLabelBarcode(text,format,width,height,showText){
      return new Promise((resolve,reject)=>{
        try{
          const canvas=document.getElementById('labelBarcodeCanvas');
          JsBarcode(canvas, text, {
            format, width, height, displayValue: showText,
            background:"#ffffff", lineColor:"#000000",
            margin:5, fontSize:12, textAlign:"center", textPosition:"bottom"
          });
          resolve(canvas.toDataURL('image/png'));
        }catch(err){reject(err);}
      });
    }

    async function generateLabel(){
      const size=document.getElementById('labelSize').value;

      const productName=(document.getElementById('productName').value||'').trim();
      const technicalName=(document.getElementById('technicalName').value||'').trim();
      const frequencyRange=(document.getElementById('frequencyRange').value||'').trim();
      const gain=(document.getElementById('gain').value||'').trim();
      const barcodeText=(document.getElementById('labelBarcodeText').value||'').trim();
      const dateRaw=document.getElementById('manufactureDate').value;
      const dateDDMMYYYY = (function(d){
        if(!d) return '';
        const [y,m,day]=d.split('-'); return `${day}${m}${y}`;
      })(dateRaw);

      const barcodeFormat=document.getElementById('labelBarcodeFormat').value;
      const barcodeWidth=parseFloat(document.getElementById('labelBarcodeWidth').value||'2');
      const barcodeHeight=parseInt(document.getElementById('labelBarcodeHeight').value||'100');
      const showText=document.getElementById('labelShowText').checked;

      if(!barcodeText){alert('Vui lòng nhập dãy số mã vạch cho tem!');return;}

      try{
        currentBarcodeDataURL = await generateLabelBarcode(barcodeText, barcodeFormat, barcodeWidth, barcodeHeight, showText);

        const labelPreview=document.getElementById('labelPreview');
        labelPreview.className = `label-preview ${size==='70x100'?'label-70x100':'label-30x40'}`;

        const grid=document.createElement('div');
        grid.className='label-canvas';

        const b1=document.createElement('div'); b1.className='b1';
        b1.innerHTML=`<div class="b1-inner"><img alt="logo" crossorigin="anonymous" src="https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png"></div>`;

        const b2=document.createElement('div'); b2.className='b2';
        b2.innerHTML=`<div class="b2-inner"><div class="brand">NEXTWAVES INDUSTRIES</div></div>`;

        const b3=document.createElement('div'); b3.className='b3';
        b3.innerHTML=`<div class="b3-inner"><div class="product">${productName}</div></div>`;

        const b4=document.createElement('div'); b4.className='b4';
        b4.innerHTML=`<div class="b4-inner"><div class="tech">${technicalName}</div></div>`;

        const b5=document.createElement('div'); b5.className='b5';
        b5.innerHTML=`<div class="b5-inner">
          <div class="spec-grid">
            <div class="cell head">DẢI TẦN</div><div class="cell">${frequencyRange}</div>
            <div class="cell head">ĐỘ LỢI</div><div class="cell">${gain}</div>
          </div>
        </div>`;

        const b6=document.createElement('div'); b6.className='b6';
        b6.innerHTML=`<div class="b6-inner"><div class="company">
          Sản xuất bởi công ty TNHH Nextwaves Industries, 20/23 đường 35, Phường An Khánh, Thành Phố Hồ Chí Minh
        </div></div>`;

        const b7=document.createElement('div'); b7.className='b7';
        b7.innerHTML=`<div class="b7-inner"><div class="phone">0938888373</div></div>`;

        const b8=document.createElement('div'); b8.className='b8';
        b8.innerHTML=`<div class="b8-inner"><img src="${currentBarcodeDataURL}" alt="Barcode"></div>`;

        const b9=document.createElement('div'); b9.className='b9';
        b9.innerHTML=`<div class="b9-inner">
          <div class="madein">Made in Vietnam</div>
          <span class="mfgdate">${dateDDMMYYYY}</span>
        </div>`;

        grid.append(b1,b2,b3,b4,b5,b6,b7,b8,b9);
        labelPreview.innerHTML=''; labelPreview.appendChild(grid);

        const exactH = grid.clientHeight || 1000;
        grid.style.setProperty('--h', exactH+'px');

        document.getElementById('labelResult').style.display='block';
      }catch(error){
        console.error(error);
        alert('Lỗi tạo tem: '+error.message);
      }
    }

    function printLabel(){
      const size=document.getElementById('labelSize').value;
      const styleId='print-page-style';
      let style=document.getElementById(styleId);
      if(!style){
        style=document.createElement('style'); style.id=styleId;
        document.head.appendChild(style);
      }
      const dims = size==='70x100' ? '70mm 100mm' : '30mm 40mm';
      style.innerHTML = `@page { size: ${dims}; margin: 0 }`;
      window.print();
    }

    async function downloadLabelPDF(){
      const labelElement=document.getElementById('labelPreview');
      if(!labelElement || !labelElement.innerHTML){alert('Vui lòng tạo tem trước khi tải xuống!');return;}

      const size=document.getElementById('labelSize').value;
      const page = (size==='70x100') ? [70,100] : [30,40];

      const { jsPDF } = window.jspdf;
      const pdf=new jsPDF({orientation:'portrait',unit:'mm',format:page});

      const canvas=await html2canvas(labelElement,{scale:3,backgroundColor:'#ffffff',useCORS:true});
      const imgData=canvas.toDataURL('image/png');
      pdf.addImage(imgData,'PNG',0,0,page[0],page[1]);
      pdf.save(`tem-${size}-${Date.now()}.pdf`);
    }
  </script>
</body>
</html>
