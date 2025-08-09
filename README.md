<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nextwaves Industries - Tạo Mã Vạch & Tem Sản Phẩm</title>
    <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
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
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
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
            font-size: 1.1rem; font-weight: 600; color: #6c757d; cursor: pointer; transition: all 0.3s ease; position: relative;
        }
        .tab.active { color: #007bff; background: white; }
        .tab.active::after { content: ''; position: absolute; bottom: 0; left: 0; right: 0; height: 3px; background: #007bff; }
        .tab-content { display: none; padding: 40px; }
        .tab-content.active { display: block; }

        .form-group { margin-bottom: 25px; }
        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 25px; }
        .form-row-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px; margin-bottom: 25px; }
        label { display: block; margin-bottom: 8px; font-weight: 600; color: #333; font-size: 0.95rem; }

        input[type="text"], input[type="number"], select {
            width: 100%; padding: 12px 16px; border: 2px solid #e9ecef; border-radius: 8px; font-size: 1rem;
            transition: border-color 0.3s ease; background: white;
        }
        input[type="text"]::placeholder { color: #999; }
        input[type="text"]:focus, input[type="number"]:focus, select:focus {
            outline: none; border-color: #007bff; box-shadow: 0 0 0 3px rgba(0,123,255,0.1);
        }
        input[type="checkbox"] { margin-right: 8px; transform: scale(1.2); }
        .checkbox-group { display: flex; align-items: center; margin-top: 10px; }

        .btn {
            background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
            color: white; padding: 14px 28px; border: none; border-radius: 8px; font-size: 1rem; font-weight: 600;
            cursor: pointer; transition: all 0.3s ease; margin: 8px; box-shadow: 0 4px 15px rgba(0,123,255,0.3);
        }
        .btn:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(0,123,255,0.4); }
        .btn-success { background: linear-gradient(135deg, #28a745 0%, #20c997 100%); box-shadow: 0 4px 15px rgba(40,167,69,0.3); }
        .btn-success:hover { box-shadow: 0 6px 20px rgba(40,167,69,0.4); }
        .btn-danger { background: linear-gradient(135deg, #dc3545 0%, #fd7e14 100%); box-shadow: 0 4px 15px rgba(220,53,69,0.3); }
        .btn-danger:hover { box-shadow: 0 6px 20px rgba(220,53,69,0.4); }

        .result-section { margin-top: 30px; padding: 30px; background: #f8f9fa; border-radius: 12px; text-align: center; }
        .barcode-container { background: white; padding: 30px; border-radius: 12px; margin: 20px 0; box-shadow: 0 4px 15px rgba(0,0,0,0.1); display: inline-block; }

        /* Xem trước tem: đúng 350x500 hoặc 150x200, bố cục 9 khối */
        .label-preview {
            background: white; border: 2px dashed #999; margin: 20px auto; font-family: Arial, sans-serif;
            position: relative; display: inline-block; overflow: hidden;
        }
        .label-350x500 { width: 350px; height: 500px; }
        .label-150x200 { width: 150px; height: 200px; }

        .label-content {
            position: absolute; inset: 0;
            display: grid;
            grid-template-rows: 12% 3% 5% 5% 15% 14% 3% 25% 5%;
            row-gap: 1%;
            padding-top: 2%;
            padding-bottom: 2%;
            padding-left: 10px;
            padding-right: 0;
        }
        .block { padding: 5px; overflow: hidden; display: flex; align-items: center; }
        .center { justify-content: center; text-align: center; }
        .left { justify-content: flex-start; text-align: left; }
        .bold { font-weight: 700; }

        .brand-logo, .barcode-img { width: 100%; height: 100%; object-fit: contain; display: block; }

        .spec-table { width: 100%; height: 100%; border-collapse: collapse; table-layout: fixed; }
        .spec-table th, .spec-table td {
            border: 1px solid #000; padding: 4px; text-align: left; vertical-align: middle;
            overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
        }
        .spec-table th { background: #f0f0f0; font-weight: bold; }

        .download-options { margin-top: 20px; padding: 20px; background: #e3f2fd; border-radius: 8px; text-align: center; }
        .download-options h4 { margin-bottom: 15px; color: #1976d2; }

        #barcodeCanvas, #labelBarcodeCanvas { display: none; }

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
                    background: "#ffffff", lineColor: "#000000", margin: 10, fontSize: 14, textAlign: "center", textPosition: "bottom"
                });
                document.getElementById('barcodeResult').style.display = 'block';
            } catch (error) {
                alert('Lỗi tạo mã vạch: ' + (error && error.message ? error.message : 'không xác định'));
            }
        }

        function downloadBarcode() {
            const barcodeElement = document.getElementById('barcodeDisplay');
            if (!barcodeElement || !barcodeElement.innerHTML) { alert('Vui lòng tạo mã vạch trước khi tải xuống!'); return; }
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
                        if (blob && blob.size > 0) {
                            const link = document.createElement('a');
                            link.download = `ma-vach-${document.getElementById('barcodeInput').value}-${Date.now()}.png`;
                            link.href = URL.createObjectURL(blob);
                            document.body.appendChild(link); link.click(); document.body.removeChild(link);
                            URL.revokeObjectURL(link.href);
                        } else { alert('Lỗi tạo file. Vui lòng thử lại!'); }
                    }, 'image/png');
                };
                img.src = 'data:image/svg+xml;base64,' + btoa(unescape(encodeURIComponent(svgData)));
            } catch (error) {
                alert('Lỗi tải xuống: ' + (error && error.message ? error.message : 'không xác định'));
            }
        }

        function generateLabelBarcode(text, format, width, height) {
            return new Promise((resolve, reject) => {
                try {
                    const canvas = document.getElementById('labelBarcodeCanvas');
                    JsBarcode(canvas, text, { format, width, height, displayValue: false, background: "#ffffff", lineColor: "#000000", margin: 0 });
                    resolve(canvas.toDataURL('image/png'));
                } catch (error) { reject(error); }
            });
        }

        function adjustTextToFit(el, minPx = 6, maxPx = 200) {
            if (!el) return;
            const parent = el.parentElement;
            const maxH = Math.max(1, parent.clientHeight - 10);
            let size = minPx;
            el.style.lineHeight = '1.1';
            el.style.whiteSpace = 'nowrap';
            while (size <= maxPx) {
                el.style.fontSize = size + 'px';
                if (el.scrollHeight > maxH) { size--; break; }
                size++;
            }
            if (size < minPx) size = minPx;
            el.style.fontSize = size + 'px';
            while (el.scrollHeight > maxH && size > minPx) {
                size--; el.style.fontSize = size + 'px';
            }
        }
        function adjustTableToFit(tableEl, minPx = 6, startPx = 16) {
            if (!tableEl) return;
            const parent = tableEl.parentElement;
            const maxH = Math.max(1, parent.clientHeight - 10);
            let size = startPx;
            tableEl.style.fontSize = size + 'px';
            while (tableEl.scrollHeight > maxH && size > minPx) {
                size--; tableEl.style.fontSize = size + 'px';
            }
        }

        async function generateLabel() {
            const size = document.getElementById('labelSize').value;
            const productName = document.getElementById('productName').value || '';
            const technicalName = document.getElementById('technicalName').value || '';
            const frequencyRange = document.getElementById('frequencyRange').value || '';
            const gain = document.getElementById('gain').value || '';
            const barcodeText = document.getElementById('labelBarcodeText').value || '';
            const barcodeFormat = document.getElementById('labelBarcodeFormat').value;
            const barcodeWidth = parseFloat(document.getElementById('labelBarcodeWidth').value);
            const barcodeHeight = parseInt(document.getElementById('labelBarcodeHeight').value);

            if (!barcodeText) { alert('Vui lòng nhập mã vạch.'); return; }
            if (barcodeFormat === 'EAN13' && !/^\d{13}$/.test(barcodeText)) {
                alert('Vui lòng nhập mã vạch EAN-13 gồm 13 chữ số.');
                return;
            }

            try {
                currentBarcodeDataURL = await generateLabelBarcode(barcodeText, barcodeFormat, barcodeWidth, barcodeHeight);

                const labelPreview = document.getElementById('labelPreview');
                const previewClass = (size === '70x100') ? 'label-350x500' : 'label-150x200';
                labelPreview.className = `label-preview ${previewClass}`;

                labelPreview.innerHTML = `
                    <div class="label-content">
                        <div class="block center" id="khoi1">
                            <img class="brand-logo" alt="Logo thương hiệu"
                                 src="https://i.postimg.cc/GmHBH7mz/LOGO-BLACK-EMPTY-2x.png"
                                 onerror="this.style.display='none'">
                        </div>
                        <div class="block center" id="khoi2">
                            <div class="brandName">Nextwaves</div>
                        </div>
                        <div class="block left" id="khoi3">
                            <div class="productName bold">${productName}</div>
                        </div>
                        <div class="block left" id="khoi4">
                            <div class="technicalName bold">${technicalName}</div>
                        </div>
                        <div class="block left" id="khoi5">
                            <table class="spec-table" id="specTable">
                                <tr><th style="width:40%">Dải tần</th><td style="width:60%">${frequencyRange}</td></tr>
                                <tr><th>Độ lợi</th><td>${gain}</td></tr>
                            </table>
                        </div>
                        <div class="block left" id="khoi6">
                            <div class="companyInfo">
                                Sản xuất bởi công ty TNHH Nextwaves Industries<br>
                                20/23 đường 35 Phường An Khánh<br>
                                Thành Phố Hồ Chí Minh
                            </div>
                        </div>
                        <div class="block left" id="khoi7">
                            <div class="phone bold">0938888373</div>
                        </div>
                        <div class="block center" id="khoi8">
                            <img src="${currentBarcodeDataURL}" class="barcode-img" alt="Barcode">
                        </div>
                        <div class="block center" id="khoi9">
                            <div class="madeIn">Made in Vietnam<br>Năm sản xuất: 2025</div>
                        </div>
                    </div>
                `;

                adjustTextToFit(document.querySelector('#khoi2 .brandName'), 8, 200);
                adjustTextToFit(document.querySelector('#khoi3 .productName'), 8, 200);
                adjustTextToFit(document.querySelector('#khoi4 .technicalName'), 8, 200);
                adjustTableToFit(document.getElementById('specTable'), 6, 18);
                adjustTextToFit(document.querySelector('#khoi6 .companyInfo'), 6, 200);
                adjustTextToFit(document.querySelector('#khoi7 .phone'), 8, 200);
                adjustTextToFit(document.querySelector('#khoi9 .madeIn'), 8, 200);

                document.getElementById('labelResult').style.display = 'block';
            } catch (error) {
                alert('Lỗi tạo tem: ' + (error && error.message ? error.message : 'không xác định'));
            }
        }

        function downloadLabelPDF() {
            const labelElement = document.getElementById('labelPreview');
            if (!labelElement || !labelElement.innerHTML) { alert('Vui lòng tạo tem trước khi tải xuống!'); return; }
            const size = document.getElementById('labelSize').value;
            const page = (size === '70x100') ? [350, 500] : [150, 200];

            html2canvas(labelElement, { backgroundColor: null, scale: 4, useCORS: true })
                .then(canvas => {
                    const imgData = canvas.toDataURL('image/png');
                    const { jsPDF } = window.jspdf;
                    const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: page });
                    pdf.addImage(imgData, 'PNG', 0, 0, page[0], page[1], undefined, 'FAST');
                    pdf.save(`tem-${size}-${Date.now()}.pdf`);
                })
                .catch(e => alert('Lỗi tạo PDF: ' + (e && e.message ? e.message : 'không xác định')));
        }

        function printLabel() { window.print(); }

        // Không tự gọi generateLabel hay generateBarcode khi tải trang
        document.addEventListener('DOMContentLoaded', function() {});
    </script>
</body>
</html>
