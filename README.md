<!DOCTYPE html><html lang="vi">  
<head>  
    <meta charset="UTF-8">  
    <!-- Thêm: giữ nguyên -->  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <!-- Thêm: giữ nguyên -->  
    <title>Nextwaves Industries - Tạo Mã Vạch & Tem Sản Phẩm</title>  
    <!-- Thêm: giữ nguyên -->  
    <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>  
    <!-- Thêm: giữ nguyên -->  
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>  
    <!-- Thêm: giữ nguyên -->  
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">  
    <!-- Thêm: giữ nguyên -->  
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
            width: 264px; /* Giữ preview px tương ứng tỉ lệ 70x100 */  
            /* Thêm: giữ nguyên chiều rộng preview trên màn hình */  
            height: 378px; /* Giữ preview px tương ứng tỉ lệ 70x100 */  
            /* Thêm: giữ nguyên chiều cao preview trên màn hình */  
            padding: 0; /* Sửa: bỏ padding để dùng lưới 9 khối chiếm 100% */  
            /* Sửa: loại bỏ padding để lưới chiếm trọn 100% chiều cao theo yêu cầu */  
            --label-h: 378px; /* Thêm: biến CSS đại diện chiều cao để tính cỡ chữ tự thích ứng */  
            /* Thêm: dùng biến này cho công thức font-size theo chiều cao khối */  
        }  
        .label-30x40 {  
            width: 113px; /* Giữ preview px tương ứng tỉ lệ 30x40 */  
            /* Thêm: giữ nguyên chiều rộng preview trên màn hình */  
            height: 151px; /* Giữ preview px tương ứng tỉ lệ 30x40 */  
            /* Thêm: giữ nguyên chiều cao preview trên màn hình */  
            padding: 0; /* Sửa: bỏ padding để dùng lưới 9 khối chiếm 100% */  
            /* Sửa: loại bỏ padding để lưới chiếm trọn 100% chiều cao theo yêu cầu */  
            --label-h: 151px; /* Thêm: biến CSS đại diện chiều cao để tính cỡ chữ tự thích ứng */  
            /* Thêm: dùng biến này cho công thức font-size theo chiều cao khối */  
        }/* Thêm: Lưới 9 khối theo phần trăm chiều cao, có khoảng cách 1% và đệm trên 2%, dưới 2%, cách trái 10px */  
    .label-grid {  
        display: grid; /* Thêm: dùng grid để chia 9 hàng theo phần trăm */  
        grid-template-rows: 12% 3% 5% 5% 15% 17% 25% 1% 5%; /* Thêm: đúng tỷ lệ chiều cao 9 khối */  
        row-gap: 1%; /* Thêm: khoảng cách giữa các khối 1% */  
        height: 100%; /* Thêm: lưới chiếm toàn bộ chiều cao nhãn */  
        padding-top: 2%; /* Thêm: khối 1 cách mép trên 2% */  
        padding-bottom: 2%; /* Thêm: khối 9 cách mép dưới 2% */  
        padding-left: 10px; /* Thêm: toàn bộ khối cách mép trái 10px */  
        padding-right: 0; /* Thêm: không yêu cầu cách phải, giữ 0 */  
    }  
    .block {  
        padding: 1px; /* Thêm: phần tử trong khối cách 4 mép 1px */  
        overflow: hidden; /* Thêm: tránh tràn nội dung */  
        display: flex; /* Thêm: dùng flex để căn lề ngang theo yêu cầu */  
        align-items: flex-start; /* Thêm: nội dung bám mép trên trong khối */  
        justify-content: flex-start; /* Thêm: mặc định căn trái */  
        width: calc(100% - 10px); /* Thêm: trừ phần đệm trái 10px của lưới để không tràn */  
    }  
    .block.center {  
        justify-content: center; /* Thêm: khối cần căn giữa ngang */  
        text-align: center; /* Thêm: văn bản căn giữa */  
    }  
    .block.left {  
        justify-content: flex-start; /* Thêm: khối căn trái */  
        text-align: left; /* Thêm: văn bản căn trái */  
    }  
    .block img {  
        width: 100%; /* Thêm: ảnh tự co dãn theo chiều ngang khối */  
        height: 100%; /* Thêm: ảnh tự co dãn theo chiều cao khối */  
        object-fit: contain; /* Thêm: bảo toàn tỉ lệ ảnh trong khối */  
    }  
    .fit-text {  
        line-height: 1.05; /* Thêm: tăng độ khít chữ để tận dụng chiều cao khối */  
        word-break: break-word; /* Thêm: xuống dòng khi dài */  
        width: 100%; /* Thêm: chiếm đủ bề rộng để căn chỉnh chính xác */  
    }  
    /* Thêm: cỡ chữ tự thích ứng theo chiều cao nhãn cho từng khối chính */  
    .block-2 .fit-text { font-size: calc(var(--label-h) * 0.03 * 0.6); }  
    /* Thêm: tên thương hiệu trong khối 2, 60% chiều cao khối */  
    .block-3 .fit-text { font-size: calc(var(--label-h) * 0.05 * 0.45); }  
    /* Thêm: tên sản phẩm trong khối 3 */  
    .block-4 .fit-text { font-size: calc(var(--label-h) * 0.05 * 0.45); }  
    /* Thêm: tên kỹ thuật trong khối 4 */  
    .block-5 .fit-text { font-size: calc(var(--label-h) * 0.15 * 0.18); }  
    /* Thêm: thông số kỹ thuật, chữ nhỏ hơn để phù hợp bảng */  
    .block-6 .fit-text { font-size: calc(var(--label-h) * 0.17 * 0.16); }  
    /* Thêm: thông tin công ty */  
    .block-8 .fit-text { font-size: calc(var(--label-h) * 0.01 * 0.9); }  
    /* Thêm: dãy số dưới mã vạch, gần sát chiều cao khối 8 */  
    .block-9 .fit-text { font-size: calc(var(--label-h) * 0.05 * 0.5); }  
    /* Thêm: Made in Vietnam */  
  
    /* Sửa: tối ưu bảng thông số để nằm gọn trong khối 5 */  
    .spec-table {  
        width: 100%; /* Sửa: bảng chiếm 100% bề rộng khối */  
        border: 1px solid #000; /* Sửa: viền mảnh để rõ nhưng không chiếm chỗ */  
        border-collapse: collapse; /* Sửa: gộp viền */  
        height: 100%; /* Thêm: bảng co dãn theo chiều cao khối 5 */  
        table-layout: fixed; /* Thêm: tránh tràn cột */  
        font-weight: bold; /* Thêm: chữ đậm như yêu cầu ban đầu */  
    }  
    .spec-table th, .spec-table td {  
        border: 1px solid #000; /* Sửa: đồng bộ viền ô */  
        padding: 0; /* Sửa: bỏ padding để vừa chiều cao khối */  
        text-align: center; /* Giữ: canh giữa nội dung ô */  
        font-size: inherit; /* Thêm: cỡ chữ theo .fit-text của khối 5 */  
    }  
  
    /* Thêm: quy tắc in đảm bảo đúng khổ giấy 70x100mm hoặc 30x40mm, bỏ A4 */  
    @media print {  
        body { margin: 0; padding: 0; background: white; }  
        /* Giữ: reset margin/padding khi in */  
        .container { box-shadow: none; margin: 0; padding: 0; }  
        /* Giữ: bỏ bóng khi in */  
        .header, .tabs, .tab-content:not(.active), .btn, .download-options { display: none; }  
        /* Giữ: ẩn phần không liên quan khi in */  
        .label-preview { border: 1px solid #000; margin: 0; }  
        /* Giữ: viền mảnh khi in */  
        .label-70x100 { width: 70mm; height: 100mm; }  
        /* Thêm: đặt kích thước in thực tế cho 70x100mm */  
        .label-30x40 { width: 30mm; height: 40mm; }  
        /* Thêm: đặt kích thước in thực tế cho 30x40mm */  
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
            <!-- Giữ: nút chuyển tab mã vạch -->  
            <button class="tab" onclick="switchTab('label')">🏷️ Tạo Tem Sản Phẩm</button>  
            <!-- Giữ: nút chuyển tab tem -->  
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
                <h4>📥 Tải xuống PDF</h4>  
                <!-- Sửa: chỉ còn 1 nút tải PDF đúng khổ nhãn, bỏ nút A4 -->  
                <button class="btn btn-success" onclick="downloadLabelPDF()">📄 Tải xuống PDF theo khổ đã chọn</button>  
                <!-- Thêm: nút tải PDF theo kích thước 70x100 hoặc 30x40, không còn A4 -->  
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
        let currentBarcodeDataURL = '';// Tab switching  
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
  
    // Sửa: generateLabel tạo trước xem trước với lưới 9 khối và căn lề theo yêu cầu  
    async function generateLabel() {  
        const size = document.getElementById('labelSize').value; // Giữ: lấy kích thước  
        const productName = document.getElementById('productName').value || 'ĂNG TEN MẶT ĐẤT TUYẾN TÍNH'; // Giữ: tên sp  
        const technicalName = document.getElementById('technicalName').value || 'RA-UL-SFF-241401'; // Giữ: tên kỹ thuật  
        const frequencyRange = document.getElementById('frequencyRange').value || '900 - 930 MHz'; // Giữ: dải tần  
        const gain = document.getElementById('gain').value || '7.5 dBi'; // Giữ: độ lợi  
        const barcodeText = document.getElementById('labelBarcodeText').value || '8936236710036'; // Giữ: dãy số mã vạch  
  
        // Barcode settings  
        const barcodeFormat = document.getElementById('labelBarcodeFormat').value; // Giữ  
        const barcodeWidth = parseFloat(document.getElementById('labelBarcodeWidth').value); // Giữ  
        const barcodeHeight = parseInt(document.getElementById('labelBarcodeHeight').value); // Giữ  
        const showText = document.getElementById('labelShowText').checked; // Giữ  
        try {  
            // Generate barcode PNG  
            currentBarcodeDataURL = await generateLabelBarcode(barcodeText, barcodeFormat, barcodeWidth, barcodeHeight, showText); // Giữ: tạo ảnh mã vạch PNG  
            const labelPreview = document.getElementById('labelPreview');  
            labelPreview.className = `label-preview label-${size}`; // Giữ: set class kích thước nhãn  
  
            // Sửa: dựng HTML theo 9 khối với lưới và căn lề đúng quy định  
            labelPreview.innerHTML = `  
                <div class="label-grid">  
                    <!-- Khối 1: 12% logo thương hiệu, căn giữa -->  
                    <div class="block block-1 center">  
                        <img src="https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png" alt="Logo" onerror="this.style.display='none'">  
                    </div>  
                    <!-- Khối 2: 3% tên thương hiệu, căn giữa -->  
                    <div class="block block-2 center">  
                        <div class="fit-text">Nextwaves</div>  
                    </div>  
                    <!-- Khối 3: 5% tên sản phẩm, căn trái -->  
                    <div class="block block-3 left">  
                        <div class="fit-text">${productName}</div>  
                    </div>  
                    <!-- Khối 4: 5% tên kỹ thuật, căn trái -->  
                    <div class="block block-4 left">  
                        <div class="fit-text">${technicalName}</div>  
                    </div>  
                    <!-- Khối 5: 15% bảng thông số kỹ thuật, căn trái -->  
                    <div class="block block-5 left">  
                        <div class="fit-text" style="height:100%;">  
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
                        </div>  
                    </div>  
                    <!-- Khối 6: 17% thông tin công ty, căn trái -->  
                    <div class="block block-6 left">  
                        <div class="fit-text">  
                            Sản xuất bởi công ty TNHH Nextwaves Industries\n20/23 đường 35 Phường An Khánh\nThành Phố Hồ Chí Minh\nĐiện thoại: 0938888373\nNĂM SẢN XUẤT: 2025  
                        </div>  
                    </div>  
                    <!-- Khối 7: 25% mã vạch, căn trái theo yêu cầu -->  
                    <div class="block block-7 left">  
                        <img src="${currentBarcodeDataURL}" alt="Barcode">  
                    </div>  
                    <!-- Khối 8: 1% dãy số dưới mã vạch, căn giữa nếu bật hiển thị -->  
                    <div class="block block-8 center">  
                        ${showText ? `<div class=\"fit-text\">${formatBarcodeNumbers(barcodeText)}</div>` : ''}  
                    </div>  
                    <!-- Khối 9: 5% Made in Vietnam, căn giữa -->  
                    <div class="block block-9 center">  
                        <div class="fit-text">Made in Vietnam</div>  
                    </div>  
                </div>  
            `; // Sửa: kết thúc dựng lưới 9 khối  
  
            document.getElementById('labelResult').style.display = 'block'; // Giữ  
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
  
    // Sửa: Tải PDF theo đúng khổ nhãn người dùng chọn, bỏ hoàn toàn A4 và nhiều tem  
    function downloadLabelPDF() {  
        const labelElement = document.getElementById('labelPreview'); // Giữ  
        if (!labelElement || !labelElement.innerHTML) {  
            alert('Vui lòng tạo tem trước khi tải xuống!'); // Giữ  
            return; // Giữ  
        }  
        // Sửa: lấy jsPDF an toàn theo UMD và kiểm tra tồn tại  
        const jsPDF = window.jspdf && window.jspdf.jsPDF;  
        if (!jsPDF) {  
            alert('Không tìm thấy thư viện tạo PDF. Vui lòng kiểm tra kết nối mạng hoặc CDN jsPDF.');  
            return;  
        }  
        try {  
            const size = document.getElementById('labelSize').value; // Giữ: đọc kích thước đã chọn  
            // Thêm: chọn khổ PDF đúng 70x100mm hoặc 30x40mm  
            const format = (size === '70x100') ? [70, 100] : [30, 40];  
            // Thêm: khởi tạo PDF đúng khổ, không dùng A4  
            const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: format });  
            addLabelToPDF(pdf, size); // Sửa: vẽ 1 tem theo 9 khối lên trang PDF đã đúng khổ  
            pdf.save(`tem-${size}-${Date.now()}.pdf`); // Sửa: lưu 1 tem duy nhất đúng khổ đã chọn  
        } catch (err) {  
            alert('Lỗi khi tạo file PDF: ' + err.message);  
        }  
    }  
  
    // Sửa: Vẽ nội dung 9 khối với tỷ lệ phần trăm, căn lề và đệm 1px trong từng khối  
    function addLabelToPDF(pdf, size) {  
        const productName = document.getElementById('productName').value || 'ĂNG TEN MẶT ĐẤT TUYẾN TÍNH'; // Giữ  
        const technicalName = document.getElementById('technicalName').value || 'RA-UL-SFF-241401'; // Giữ  
        const frequencyRange = document.getElementById('frequencyRange').value || '900 - 930 MHz'; // Giữ  
        const gain = document.getElementById('gain').value || '7.5 dBi'; // Giữ  
        const barcodeText = document.getElementById('labelBarcodeText').value || '8936236710036'; // Giữ  
        const showText = document.getElementById('labelShowText').checked; // Giữ  
  
        const W = (size === '70x100') ? 70 : 30; // Thêm: chiều rộng mm theo khổ  
        const H = (size === '70x100') ? 100 : 40; // Thêm: chiều cao mm theo khổ  
        const gap = 0.01 * H; // Thêm: 1% khoảng cách giữa khối  
        const padTop = 0.02 * H; // Thêm: 2% mép trên  
        const padBottom = 0.02 * H; // Thêm: 2% mép dưới  
        const left10px = 2.6458; // Thêm: 10px ≈ 2.6458mm  
        const innerPadPx = 0.2646; // Thêm: 1px ≈ 0.2646mm cho đệm trong khối  
  
        const rowsPct = [0.12, 0.03, 0.05, 0.05, 0.15, 0.17, 0.25, 0.01, 0.05]; // Thêm: phần trăm 9 khối  
        const rows = rowsPct.map(p => p * H); // Thêm: chiều cao từng khối mm  
  
        let y = padTop; // Thêm: điểm bắt đầu khối 1  
  
        // Helper chuyển mm sang pt để tính font size gần đúng  
        const mmToPt = mm => mm / 0.3527777778; // Thêm: quy đổi mm → pt  
  
        // Khối 1: Logo, căn giữa ngang, 1px đệm trong  
        // Vẽ khung nội dung khối 1  
        // Dùng placeholder đen nếu không có ảnh cần nhúng  
        const h1 = rows[0] - 2 * innerPadPx; // Thêm: chiều cao vùng nội dung khối 1  
        const w1 = W - left10px - 2 * innerPadPx; // Thêm: chiều rộng vùng nội dung khối 1  
        // Vẽ placeholder logo ở giữa  
        const logoH = Math.min(h1, (size === '70x100') ? 20 : 6); // Thêm: chọn chiều cao logo hợp lý theo khổ  
        const logoW = logoH; // Thêm: logo vuông  
        const cx = left10px + innerPadPx + (w1 - logoW) / 2; // Thêm: căn giữa ngang  
        const cy = y + innerPadPx + (h1 - logoH) / 2; // Thêm: căn giữa dọc trong khối  
        pdf.setFillColor(0); // Giữ: màu đen cho placeholder  
        pdf.rect(cx, cy, logoW, logoH, 'F'); // Sửa: placeholder logo  
        y += rows[0] + gap; // Thêm: chuyển xuống khối 2  
  
        // Khối 2: Tên thương hiệu, căn giữa  
        const h2 = rows[1] - 2 * innerPadPx; // Thêm  
        const brandFont = Math.min(mmToPt(h2 * 0.7), 18); // Thêm: cỡ chữ ~70% chiều cao khối, giới hạn tối đa  
        pdf.setFont('helvetica', 'bold'); // Thêm: đậm cho thương hiệu  
        pdf.setFontSize(brandFont); // Thêm: đặt cỡ chữ  
        pdf.text('Nextwaves', left10px + (W - left10px) / 2, y + innerPadPx + h2 * 0.8, { align: 'center' }); // Sửa: căn giữa ngang  
        y += rows[1] + gap; // Thêm  
  
        // Khối 3: Tên sản phẩm, căn trái  
        const h3 = rows[2] - 2 * innerPadPx; // Thêm  
        const pFont = Math.min(mmToPt(h3 * 0.7), 12); // Thêm  
        pdf.setFont('helvetica', 'bold'); // Giữ  
        pdf.setFontSize(pFont); // Thêm  
        pdf.text(productName, left10px + innerPadPx, y + innerPadPx + h3 * 0.8, { align: 'left', maxWidth: W - left10px - 2 * innerPadPx }); // Sửa  
        y += rows[2] + gap; // Thêm  
  
        // Khối 4: Tên kỹ thuật, căn trái  
        const h4 = rows[3] - 2 * innerPadPx; // Thêm  
        const tFont = Math.min(mmToPt(h4 * 0.7), 11); // Thêm  
        pdf.setFont('helvetica', 'bold'); // Giữ  
        pdf.setFontSize(tFont); // Thêm  
        pdf.text(technicalName, left10px + innerPadPx, y + innerPadPx + h4 * 0.8, { align: 'left', maxWidth: W - left10px - 2 * innerPadPx }); // Sửa  
        y += rows[3] + gap; // Thêm  
  
        // Khối 5: Bảng thông số kỹ thuật, căn trái  
        const h5 = rows[4] - 2 * innerPadPx; // Thêm  
        const tableX = left10px + innerPadPx; // Thêm  
        const tableY = y + innerPadPx; // Thêm  
        const tableW = W - left10px - 2 * innerPadPx; // Thêm  
        const tableH = h5; // Thêm  
        // Vẽ khung bảng 2 hàng 2 cột  
        pdf.setLineWidth(0.2); // Thêm: viền mảnh  
        pdf.rect(tableX, tableY, tableW, tableH); // Thêm: khung ngoài  
        pdf.line(tableX, tableY + tableH / 2, tableX + tableW, tableY + tableH / 2); // Thêm: kẻ ngang  
        pdf.line(tableX + tableW * 0.4, tableY, tableX + tableW * 0.4, tableY + tableH); // Thêm: kẻ dọc 40%  
        const specFont = Math.min(mmToPt((tableH / 2) * 0.35), 8); // Thêm: cỡ chữ trong ô  
        pdf.setFont('helvetica', 'bold'); // Giữ  
        pdf.setFontSize(specFont); // Thêm  
        // Hàng 1  
        pdf.text('DẢI TẦN', tableX + tableW * 0.2, tableY + (tableH / 4), { align: 'center' }); // Thêm  
        pdf.text(frequencyRange, tableX + tableW * 0.7, tableY + (tableH / 4), { align: 'center' }); // Thêm  
        // Hàng 2  
        pdf.text('ĐỘ LỢI', tableX + tableW * 0.2, tableY + (3 * tableH / 4), { align: 'center' }); // Thêm  
        pdf.text(gain, tableX + tableW * 0.7, tableY + (3 * tableH / 4), { align: 'center' }); // Thêm  
        y += rows[4] + gap; // Thêm  
  
        // Khối 6: Thông tin công ty, căn trái  
        const h6 = rows[5] - 2 * innerPadPx; // Thêm  
        const infoFont = Math.min(mmToPt(h6 * 0.18), 7); // Thêm: chữ nhỏ vừa khối  
        pdf.setFont('helvetica', 'normal'); // Thêm  
        pdf.setFontSize(infoFont); // Thêm  
        const infoLines = [  
            'Sản xuất bởi công ty TNHH Nextwaves Industries',  
            '20/23 đường 35 Phường An Khánh',  
            'Thành Phố Hồ Chí Minh',  
            'Điện thoại: 0938888373',  
            'NĂM SẢN XUẤT: 2025'  
        ]; // Thêm: nội dung khối 6  
        let ly = y + innerPadPx + infoFont * 0.3528; // Thêm: vị trí dòng đầu tiên (xấp xỉ)  
        infoLines.forEach(line => {  
            pdf.text(line, left10px + innerPadPx, ly, { align: 'left', maxWidth: W - left10px - 2 * innerPadPx }); // Thêm  
            ly += (h6 / infoLines.length); // Thêm: phân bổ đều theo chiều cao khối  
        });  
        y += rows[5] + gap; // Thêm  
  
        // Khối 7: Mã vạch, căn trái  
        const h7 = rows[6] - 2 * innerPadPx; // Thêm  
        if (currentBarcodeDataURL) {  
            const bw = W - left10px - 2 * innerPadPx; // Thêm: bề rộng vùng mã vạch  
            const bh = h7; // Thêm: chiều cao vùng mã vạch  
            pdf.addImage(currentBarcodeDataURL, 'PNG', left10px + innerPadPx, y + innerPadPx, bw, bh); // Sửa: chèn ảnh mã vạch vừa khối  
        }  
        y += rows[6] + gap; // Thêm  
  
        // Khối 8: Dãy số dưới mã vạch, căn giữa nếu có  
        const h8 = rows[7] - 2 * innerPadPx; // Thêm  
        if (showText) {  
            const numFont = Math.min(mmToPt(h8 * 0.9), 7); // Thêm: cỡ chữ theo chiều cao khối 8  
            pdf.setFont('helvetica', 'normal'); // Thêm  
            pdf.setFontSize(numFont); // Thêm  
            pdf.text(formatBarcodeNumbers(barcodeText), left10px + (W - left10px) / 2, y + innerPadPx + h8 * 0.85, { align: 'center' }); // Sửa  
        }  
        y += rows[7] + gap; // Thêm  
  
        // Khối 9: Made in Vietnam, căn giữa  
        const h9 = rows[8] - 2 * innerPadPx; // Thêm  
        const madeFont = Math.min(mmToPt(h9 * 0.6), 9); // Thêm  
        pdf.setFont('helvetica', 'bold'); // Thêm  
        pdf.setFontSize(madeFont); // Thêm  
        pdf.text('Made in Vietnam', left10px + (W - left10px) / 2, y + innerPadPx + h9 * 0.8, { align: 'center' }); // Sửa  
  
        // Lưu ý: tổng 9 khối + khoảng cách + đệm trên/dưới = 100% chiều cao  
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
