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
