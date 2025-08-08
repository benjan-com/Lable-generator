<!DOCTYPE html>

<html lang="vi">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Nextwaves Industries - Tạo Mã Vạch & Tem Sản Phẩm</title>

    <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">

    <style>

        * {

            margin: 0;

            padding: 0;

            box-sizing: border-box;

        }


        body {

            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;

            background: linear-gradient(135deg, #667eea 0%, #764ba2 100% );

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


        .logo {

            max-width: 200px;

            height: auto;

            margin-bottom: 15px;

            filter: brightness(0) invert(1);

        }


        .header h1 {

            font-size: 2.2rem;

            margin-bottom: 10px;

            font-weight: 700;

        }


        .header p {

            font-size: 1.1rem;

            opacity: 0.9;

        }


        .tabs {

            display: flex;

            background: #f8f9fa;

            border-bottom: 1px solid #dee2e6;

        }


        .tab {

            flex: 1;

            padding: 20px;

            text-align: center;

            background: none;

            border: none;

            font-size: 1.1rem;

            font-weight: 600;

            color: #6c757d;

            cursor: pointer;

            transition: all 0.3s ease;

            position: relative;

        }


        .tab.active {

            color: #007bff;

            background: white;

        }


        .tab.active::after {

            content: '';

            position: absolute;

            bottom: 0;

            left: 0;

            right: 0;

            height: 3px;

            background: #007bff;

        }


        .tab-content {

            display: none;

            padding: 40px;

        }


        .tab-content.active {

            display: block;

        }


        .form-group {

            margin-bottom: 25px;

        }


        .form-row {

            display: grid;

            grid-template-columns: 1fr 1fr;

            gap: 20px;

            margin-bottom: 25px;

        }


        .form-row-3 {

            display: grid;

            grid-template-columns: 1fr 1fr 1fr;

            gap: 15px;

            margin-bottom: 25px;

        }


        label {

            display: block;

            margin-bottom: 8px;

            font-weight: 600;

            color: #333;

            font-size: 0.95rem;

        }


        input[type="text"], input[type="number"], select {

            width: 100%;

            padding: 12px 16px;

            border: 2px solid #e9ecef;

            border-radius: 8px;

            font-size: 1rem;

            transition: border-color 0.3s ease;

            background: white;

        }


        input[type="text"]:focus, input[type="number"]:focus, select:focus {

            outline: none;

            border-color: #007bff;

            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);

        }


        input[type="checkbox"] {

            margin-right: 8px;

            transform: scale(1.2);

        }


        .checkbox-group {

            display: flex;

            align-items: center;

            margin-top: 10px;

        }


        .btn {

            background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);

            color: white;

            padding: 14px 28px;

            border: none;

            border-radius: 8px;

            font-size: 1rem;

            font-weight: 600;

            cursor: pointer;

            transition: all 0.3s ease;

            margin: 8px;

            box-shadow: 0 4px 15px rgba(0, 123, 255, 0.3);

        }


        .btn:hover {

            transform: translateY(-2px);

            box-shadow: 0 6px 20px rgba(0, 123, 255, 0.4);

        }


        .btn-success {

            background: linear-gradient(135deg, #28a745 0%, #20c997 100%);

            box-shadow: 0 4px 15px rgba(40, 167, 69, 0.3);

        }


        .btn-success:hover {

            box-shadow: 0 6px 20px rgba(40, 167, 69, 0.4);

        }


        .btn-danger {

            background: linear-gradient(135deg, #dc3545 0%, #fd7e14 100%);

            box-shadow: 0 4px 15px rgba(220, 53, 69, 0.3);

        }


        .btn-danger:hover {

            box-shadow: 0 6px 20px rgba(220, 53, 69, 0.4);

        }


        .result-section {

            margin-top: 30px;

            padding: 30px;

            background: #f8f9fa;

            border-radius: 12px;

            text-align: center;

        }


        .barcode-container {

            background: white;

            padding: 30px;

            border-radius: 12px;

            margin: 20px 0;

            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);

            display: inline-block;

        }


        /* Label Styles */

        .label-preview {

            background: white;

            border: 2px dashed #999;

            margin: 20px auto;

            font-family: Arial, sans-serif;

            position: relative;

            display: inline-block;

        }


        .label-70x100 {

            width: 264px;

            height: 378px;

            padding: 15px;

        }


        .label-30x40 {

            width: 113px;

            height: 151px;

            padding: 8px;

        }


        .label-70x100 .logo-section {

            text-align: center;

            margin-bottom: 10px;

        }


        .label-70x100 .logo-img {

            width: 60px;

            height: 60px;

            background: url('https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png' ) center/contain no-repeat;

            margin: 0 auto;

        }


        .label-70x100 .brand-name {

            text-align: center;

            font-size: 20px;

            font-weight: bold;

            margin-bottom: 15px;

        }


        .label-70x100 .product-name {

            text-align: center;

            font-size: 14px;

            font-weight: bold;

            margin-bottom: 8px;

            line-height: 1.2;

        }


        .label-70x100 .technical-name {

            text-align: center;

            font-size: 12px;

            font-weight: bold;

            margin-bottom: 15px;

        }


        .label-70x100 .spec-table {

            width: 100%;

            border: 2px solid #000;

            border-collapse: collapse;

            margin-bottom: 12px;

            font-size: 10px;

        }


        .label-70x100 .spec-table th,

        .label-70x100 .spec-table td {

            border: 2px solid #000;

            padding: 4px;

            text-align: center;

            font-weight: bold;

        }


        .label-70x100 .spec-table th {

            background: #f0f0f0;

        }


        .label-70x100 .company-info {

            font-size: 9px;

            line-height: 1.2;

            margin-bottom: 8px;

            text-align: left;

        }


        .label-70x100 .phone {

            font-size: 12px;

            font-weight: bold;

            margin: 3px 0;

            text-align: left;

        }


        .label-70x100 .year {

            font-size: 9px;

            margin-bottom: 10px;

            text-align: left;

        }


        .label-70x100 .barcode-section {

            text-align: center;

            margin-bottom: 8px;

        }


        .label-70x100 .barcode-img {

            max-width: 200px;

            height: auto;

            margin: 0 auto;

        }


        .label-70x100 .barcode-numbers {

            font-size: 10px;

            margin-top: 3px;

            letter-spacing: 1px;

        }


        .label-70x100 .made-in {

            text-align: center;

            font-size: 12px;

            font-weight: bold;

        }


        /* 30x40 styles */

        .label-30x40 .logo-section {

            text-align: center;

            margin-bottom: 5px;

        }


        .label-30x40 .logo-img {

            width: 25px;

            height: 25px;

            background: url('https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png' ) center/contain no-repeat;

            margin: 0 auto;

        }


        .label-30x40 .brand-name {

            text-align: center;

            font-size: 8px;

            font-weight: bold;

            margin-bottom: 5px;

        }


        .label-30x40 .product-name {

            text-align: center;

            font-size: 6px;

            font-weight: bold;

            margin-bottom: 3px;

            line-height: 1.1;

        }


        .label-30x40 .technical-name {

            text-align: center;

            font-size: 5px;

            font-weight: bold;

            margin-bottom: 5px;

        }


        .label-30x40 .spec-table {

            width: 100%;

            border: 1px solid #000;

            border-collapse: collapse;

            margin-bottom: 5px;

            font-size: 4px;

        }


        .label-30x40 .spec-table th,

        .label-30x40 .spec-table td {

            border: 1px solid #000;

            padding: 1px;

            text-align: center;

            font-weight: bold;

        }


        .label-30x40 .spec-table th {

            background: #f0f0f0;

        }


        .label-30x40 .company-info {

            font-size: 4px;

            line-height: 1.1;

            margin-bottom: 3px;

            text-align: left;

        }


        .label-30x40 .phone {

            font-size: 5px;

            font-weight: bold;

            margin: 1px 0;

            text-align: left;

        }


        .label-30x40 .year {

            font-size: 4px;

            margin-bottom: 5px;

            text-align: left;

        }


        .label-30x40 .barcode-section {

            text-align: center;

            margin-bottom: 3px;

        }


        .label-30x40 .barcode-img {

            max-width: 80px;

            height: auto;

            margin: 0 auto;

        }


        .label-30x40 .barcode-numbers {

            font-size: 4px;

            margin-top: 1px;

            letter-spacing: 0.5px;

        }


        .label-30x40 .made-in {

            text-align: center;

            font-size: 5px;

            font-weight: bold;

        }


        .download-options {

            margin-top: 20px;

            padding: 20px;

            background: #e3f2fd;

            border-radius: 8px;

            text-align: center;

        }


        .download-options h4 {

            margin-bottom: 15px;

            color: #1976d2;

        }


        #barcodeCanvas, #labelBarcodeCanvas {

            display: none;

        }


        @media (max-width: 768px) {

            .form-row, .form-row-3 {

                grid-template-columns: 1fr;

            }

            

            .tabs {

                flex-direction: column;

            }

            

            .container {

                margin: 10px;

                border-radius: 15px;

            }

            

            .header {

                padding: 20px;

            }

            

            .header h1 {

                font-size: 1.8rem;

            }

            

            .tab-content {

                padding: 20px;

            }

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

            <button class="tab active" onclick="switchTab('barcode' )">📊 Tạo Mã Vạch</button>

            <button class="tab" onclick="switchTab('label')">🏷️ Tạo Tem Sản Phẩm</button>

        </div>


        <!-- Tab Tạo Mã Vạch -->

        <div id="barcode-tab" class="tab-content active">

            <div class="form-row">

                <div class="form-group">

                    <label for="barcodeInput">📝 Nhập dãy số:</label>

                    <input type="text" id="barcodeInput" placeholder="VD: 1234567890123" value="1234567890123">

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

                    <input type="text" id="productName" placeholder="VD: ĂNG TEN MẶT ĐẤT TUYẾN TÍNH" value="ĂNG TEN MẶT ĐẤT TUYẾN TÍNH">

                </div>

                <div class="form-group">

                    <label for="technicalName">🔧 Tên kỹ thuật:</label>

                    <input type="text" id="technicalName" placeholder="VD: RA-UL-SFF-241401" value="RA-UL-SFF-241401">

                </div>

            </div>


            <div class="form-row">

                <div class="form-group">

                    <label for="frequencyRange">📡 Dải tầng:</label>

                    <input type="text" id="frequencyRange" placeholder="VD: 900 - 930 MHz" value="900 - 930 MHz">

                </div>

                <div class="form-group">

                    <label for="gain">⚡ Độ lợi:</label>

                    <input type="text" id="gain" placeholder="VD: 7.5 dBi" value="7.5 dBi">

                </div>

            </div>


            <div class="form-group">

                <label for="labelBarcodeText">📊 Mã vạch (EAN-13):</label>

                <input type="text" id="labelBarcodeText" placeholder="VD: 8936236710036" value="8936236710036">

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

                <button class="btn btn-success" onclick="downloadLabelPDF('single')">📄 Tải xuống 1 tem PDF</button>

                <button class="btn btn-danger" onclick="downloadLabelPDF('multiple')">📄 Tải xuống 6 tem PDF (A4)</button>

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

        // Global variables

        let currentTab = 'barcode';

        let currentBarcodeDataURL = '';


        // Tab switching

        function switchTab(tab) {

            currentTab = tab;

            

            // Update tab buttons

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


        // Generate barcode (original function)

        function generateBarcode() {

            const text = document.getElementById('barcodeInput').value;

            const format = document.getElementById('barcodeFormat').value;

            const width = parseFloat(document.getElementById('barcodeWidth').value);

            const height = parseInt(document.getElementById('barcodeHeight').value);

            const showText = document.getElementById('showText').checked;


            if (!text) {

                alert('Vui lòng nhập dãy số!');

                return;

            }


            try {

                JsBarcode("#barcodeDisplay", text, {

                    format: format,

                    width: width,

                    height: height,

                    displayValue: showText,

                    background: "#ffffff",

                    lineColor: "#000000",

                    margin: 10,

                    fontSize: 14,

                    textAlign: "center",

                    textPosition: "bottom"

                });


                document.getElementById('barcodeResult').style.display = 'block';

            } catch (error) {

                alert('Lỗi tạo mã vạch: ' + error.message);

            }

        }


        // Download barcode as PNG (original function)

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

                    canvas.width = img.width;

                    canvas.height = img.height;

                    

                    // Fill white background

                    ctx.fillStyle = 'white';

                    ctx.fillRect(0, 0, canvas.width, canvas.height);

                    

                    // Draw barcode

                    ctx.drawImage(img, 0, 0);


                    // Download

                    canvas.toBlob(function(blob) {

                        if (blob.size > 0) {

                            const link = document.createElement('a');

                            link.download = `ma-vach-${document.getElementById('barcodeInput').value}-${Date.now()}.png`;

                            link.href = URL.createObjectURL(blob);

                            document.body.appendChild(link);

                            link.click();

                            document.body.removeChild(link);

                            URL.revokeObjectURL(link.href);

                        } else {

                            alert('Lỗi tạo file. Vui lòng thử lại!');

                        }

                    }, 'image/png');

                };


                img.src = 'data:image/svg+xml;base64,' + btoa(unescape(encodeURIComponent(svgData)));

            } catch (error) {

                alert('Lỗi tải xuống: ' + error.message);

            }

        }


        // Generate barcode for label (create PNG and return data URL)

        function generateLabelBarcode(text, format, width, height, showText) {

            return new Promise((resolve, reject) => {

                try {

                    const canvas = document.getElementById('labelBarcodeCanvas');

                    

                    // Generate barcode on canvas

                    JsBarcode(canvas, text, {

                        format: format,

                        width: width,

                        height: height,

                        displayValue: showText,

                        background: "#ffffff",

                        lineColor: "#000000",

                        margin: 5,

                        fontSize: 12,

                        textAlign: "center",

                        textPosition: "bottom"

                    });


                    // Convert to data URL

                    const dataURL = canvas.toDataURL('image/png');

                    resolve(dataURL);

                } catch (error) {

                    reject(error);

                }

            });

        }


        // Generate label

        async function generateLabel() {

            const size = document.getElementById('labelSize').value;

            const productName = document.getElementById('productName').value || 'ĂNG TEN MẶT ĐẤT TUYẾN TÍNH';

            const technicalName = document.getElementById('technicalName').value || 'RA-UL-SFF-241401';

            const frequencyRange = document.getElementById('frequencyRange').value || '900 - 930 MHz';

            const gain = document.getElementById('gain').value || '7.5 dBi';

            const barcodeText = document.getElementById('labelBarcodeText').value || '8936236710036';

            

            // Barcode settings

            const barcodeFormat = document.getElementById('labelBarcodeFormat').value;

            const barcodeWidth = parseFloat(document.getElementById('labelBarcodeWidth').value);

            const barcodeHeight = parseInt(document.getElementById('labelBarcodeHeight').value);

            const showText = document.getElementById('labelShowText').checked;


            try {

                // Generate barcode PNG

                currentBarcodeDataURL = await generateLabelBarcode(barcodeText, barcodeFormat, barcodeWidth, barcodeHeight, showText);


                const labelPreview = document.getElementById('labelPreview');

                labelPreview.className = `label-preview label-${size}`;


                let logoSection = '';

                if (size === '70x100') {

                    logoSection = `

                        <div class="logo-section">

                            <div class="logo-img"></div>

                        </div>

                        <div class="brand-name">Nextwaves</div>

                    `;

                } else {

                    logoSection = `

                        <div class="logo-section">

                            <div class="logo-img"></div>

                        </div>

                        <div class="brand-name">Nextwaves</div>

                    `;

                }


                labelPreview.innerHTML = `

                    ${logoSection}

                    <div class="product-name">${productName}</div>

                    <div class="technical-name">${technicalName}</div>

                    

                    <table class="spec-table">

                        <tr>

                            <th style="width: 40%">DẢI TẦN</th>

                            <td style="width: 60%">${frequencyRange}</td>

                        </tr>

                        <tr>

                            <th>ĐỘ LỢI</th>

                            <td>${gain}</td>

                        </tr>

                    </table>

                    

                    <div class="company-info">

                        Sản xuất bởi công ty TNHH Nextwaves Industries  


                        20/23 đường 35 Phường An Khánh  


                        Thành Phố Hồ Chí Minh

                    </div>

                    

                    <div class="phone">0938888373</div>

                    <div class="year">NĂM SẢN XUẤT: 2025</div>

                    

                    <div class="barcode-section">

                        <img src="${currentBarcodeDataURL}" class="barcode-img" alt="Barcode">

                        ${showText ? `<div class="barcode-numbers">${formatBarcodeNumbers(barcodeText)}</div>` : ''}

                    </div>

                    

                    <div class="made-in">Made in Vietnam</div>

                `;


                document.getElementById('labelResult').style.display = 'block';

            } catch (error) {

                console.error('Label generation error:', error);

                alert('Lỗi tạo tem: ' + error.message);

            }

        }


        function formatBarcodeNumbers(barcode) {

            // Format EAN-13: 8 936236 710036

            if (barcode.length >= 13) {

                return `${barcode[0]} ${barcode.substring(1, 7)} ${barcode.substring(7, 13)}`;

            }

            return barcode;

        }


        // Download label as PDF

        function downloadLabelPDF(type) {

            const labelElement = document.getElementById('labelPreview');

            if (!labelElement || !labelElement.innerHTML) {

                alert('Vui lòng tạo tem trước khi tải xuống!');

                return;

            }


            const { jsPDF } = window.jspdf;

            const pdf = new jsPDF({

                orientation: 'portrait',

                unit: 'mm',

                format: 'a4'

            });


            const size = document.getElementById('labelSize').value;


            if (type === 'single') {

                // Single label PDF

                addLabelToPDF(pdf, size, 10, 10);

                pdf.save(`tem-${size}-${Date.now()}.pdf`);

            } else {

                // Multiple labels on A4

                if (size === '70x100') {

                    // 2 labels per page for 70x100mm

                    addLabelToPDF(pdf, size, 10, 10);

                    addLabelToPDF(pdf, size, 10, 120);

                } else {

                    // 6 labels per page for 30x40mm (2 columns x 3 rows)

                    const positions = [

                        {x: 10, y: 10},

                        {x: 110, y: 10},

                        {x: 10, y: 60},

                        {x: 110, y: 60},

                        {x: 10, y: 110},

                        {x: 110, y: 110}

                    ];

                    

                    positions.forEach(pos => {

                        addLabelToPDF(pdf, size, pos.x, pos.y);

                    });

                }

                pdf.save(`tem-${size}-multiple-${Date.now()}.pdf`);

            }

        }


        function addLabelToPDF(pdf, size, x, y) {

            const productName = document.getElementById('productName').value || 'ĂNG TEN MẶT ĐẤT TUYẾN TÍNH';

            const technicalName = document.getElementById('technicalName').value || 'RA-UL-SFF-241401';

            const frequencyRange = document.getElementById('frequencyRange').value || '900 - 930 MHz';

            const gain = document.getElementById('gain').value || '7.5 dBi';

            const barcodeText = document.getElementById('labelBarcodeText').value || '8936236710036';

            const showText = document.getElementById('labelShowText').checked;


            if (size === '70x100') {

                // Draw 70x100mm label

                let currentY = y;

                

                // Border

                pdf.setDrawColor(0);

                pdf.setLineWidth(0.5);

                pdf.rect(x, y, 70, 100);

                

                // Logo placeholder

                pdf.setFillColor(0);

                pdf.rect(x + 25, currentY + 5, 20, 20, 'F');

                currentY += 30;

                

                // Brand name

                pdf.setFontSize(14);

                pdf.setFont('helvetica', 'bold');

                pdf.text('Nextwaves', x + 35, currentY, { align: 'center' });

                currentY += 10;

                

                // Product name

                pdf.setFontSize(10);

                pdf.text(productName, x + 35, currentY, { align: 'center', maxWidth: 60 });

                currentY += 8;

                

                // Technical name

                pdf.setFontSize(8);

                pdf.text(technicalName, x + 35, currentY, { align: 'center' });

                currentY += 10;

                

                // Table

                pdf.setLineWidth(0.3);

                pdf.rect(x + 5, currentY, 60, 15);

                pdf.line(x + 5, currentY + 7.5, x + 65, currentY + 7.5);

                pdf.line(x + 25, currentY, x + 25, currentY + 15);

                

                pdf.setFontSize(6);

                pdf.text('DẢI TẦN', x + 15, currentY + 5, { align: 'center' });

                pdf.text(frequencyRange, x + 45, currentY + 5, { align: 'center' });

                pdf.text('ĐỘ LỢI', x + 15, currentY + 12, { align: 'center' });

                pdf.text(gain, x + 45, currentY + 12, { align: 'center' });

                currentY += 20;

                

                // Company info

                pdf.setFont('helvetica', 'normal');

                pdf.setFontSize(5);

                pdf.text('Sản xuất bởi công ty TNHH Nextwaves Industries', x + 5, currentY);

                currentY += 3;

                pdf.text('20/23 đường 35 Phường An Khánh', x + 5, currentY);

                currentY += 3;

                pdf.text('Thành Phố Hồ Chí Minh', x + 5, currentY);

                currentY += 5;

                

                // Phone

                pdf.setFont('helvetica', 'bold');

                pdf.setFontSize(7);

                pdf.text('0938888373', x + 5, currentY);

                currentY += 5;

                

                // Year

                pdf.setFont('helvetica', 'normal');

                pdf.setFontSize(5);

                pdf.text('NĂM SẢN XUẤT: 2025', x + 5, currentY);

                currentY += 8;

                

                // Barcode image

                if (currentBarcodeDataURL) {

                    pdf.addImage(currentBarcodeDataURL, 'PNG', x + 10, currentY, 50, 10);

                    currentY += 12;

                }

                

                // Barcode numbers

                if (showText) {

                    pdf.setFontSize(6);

                    pdf.text(formatBarcodeNumbers(barcodeText), x + 35, currentY, { align: 'center' });

                    currentY += 5;

                }

                

                // Made in Vietnam

                pdf.setFont('helvetica', 'bold');

                pdf.setFontSize(8);

                pdf.text('Made in Vietnam', x + 35, currentY, { align: 'center' });

                

            } else {

                // Draw 30x40mm label

                let currentY = y;

                

                // Border

                pdf.setDrawColor(0);

                pdf.setLineWidth(0.3);

                pdf.rect(x, y, 30, 40);

                

                // Logo placeholder

                pdf.setFillColor(0);

                pdf.rect(x + 12, currentY + 2, 6, 6, 'F');

                currentY += 10;

                

                // Brand name

                pdf.setFontSize(5);

                pdf.setFont('helvetica', 'bold');

                pdf.text('Nextwaves', x + 15, currentY, { align: 'center' });

                currentY += 4;

                

                // Product name

                pdf.setFontSize(3);

                pdf.text(productName, x + 15, currentY, { align: 'center', maxWidth: 25 });

                currentY += 3;

                

                // Technical name

                pdf.setFontSize(2.5);

                pdf.text(technicalName, x + 15, currentY, { align: 'center' });

                currentY += 4;

                

                // Specs (simplified)

                pdf.setFontSize(2);

                pdf.text(`DẢI TẦN: ${frequencyRange}`, x + 15, currentY, { align: 'center' });

                currentY += 2.5;

                pdf.text(`ĐỘ LỢI: ${gain}`, x + 15, currentY, { align: 'center' });

                currentY += 4;

                

                // Company info (simplified)

                pdf.setFont('helvetica', 'normal');

                pdf.setFontSize(2);

                pdf.text('Nextwaves Industries', x + 2, currentY);

                currentY += 2;

                pdf.text('0938888373', x + 2, currentY);

                currentY += 3;

                

                // Barcode image

                if (currentBarcodeDataURL) {

                    pdf.addImage(currentBarcodeDataURL, 'PNG', x + 5, currentY, 20, 4);

                    currentY += 5;

                }

                

                // Barcode numbers

                if (showText) {

                    pdf.setFontSize(2);

                    pdf.text(formatBarcodeNumbers(barcodeText), x + 15, currentY, { align: 'center' });

                    currentY += 3;

                }

                

                // Made in Vietnam

                pdf.setFont('helvetica', 'bold');

                pdf.setFontSize(3);

                pdf.text('Made in Vietnam', x + 15, currentY, { align: 'center' });

            }

        }


        // Print function

        function printLabel() {

            window.print();

        }


        // Initialize

        document.addEventListener('DOMContentLoaded', function() {

            generateBarcode();

        });


        // Auto-update when form changes

        document.querySelectorAll('input, select').forEach(input => {

            input.addEventListener('input', function() {

                if (currentTab === 'barcode') {

                    generateBarcode();

                } else {

                    generateLabel();

                }

            });

        });

    </script>

</body>

</html>

