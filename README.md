# new_syp1
calculator for new and old Syrian pounds 
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حاسبة العملة السورية - الهوية الرسمية المطورة</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --state-green: #054239;
            --state-gold-light: #b9a779;
            --state-gold-dark: #988561;
            --white: #ffffff;
            --danger-red: #d93025;
            --light-gold: #fdfaf2;
        }

        body {
            font-family: 'Cairo', sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: var(--state-green);
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: var(--white);
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 15px 35px rgba(0,0,0,0.1);
            border: 1px solid rgba(185, 167, 121, 0.3);
            margin: 20px;
        }

        .header {
            background-color: var(--state-green);
            padding: 20px;
            text-align: center;
            border-bottom: 4px solid var(--state-gold-light);
        }

        .logo { width: 80px; margin-bottom: 10px; }
        h1 { color: var(--state-gold-light); margin: 0; font-size: 20px; font-weight: 900; }

        .content { padding: 20px; }

        .section {
            margin-bottom: 15px;
            padding: 15px;
            border-radius: 15px;
            background: #fcfcfc;
            border: 1px solid #f0f0f0;
        }

        .section-title { display: block; margin-bottom: 12px; font-weight: 700; font-size: 14px; color: var(--state-green); }
        
        .main-inputs { display: flex; gap: 10px; align-items: center; margin-bottom: 15px; }
        .input-wrapper { flex: 1; }
        label { display: block; font-size: 11px; color: var(--state-gold-dark); margin-bottom: 5px; font-weight: 700; text-align: center; }

        input {
            width: 100%;
            padding: 12px 5px;
            border: 2px solid #eee;
            border-radius: 12px;
            font-family: 'Cairo', sans-serif;
            font-size: 18px;
            font-weight: 700;
            color: var(--state-green);
            box-sizing: border-box;
            text-align: center;
            transition: all 0.2s;
        }

        input:focus { border-color: var(--state-gold-light); outline: none; background: #fff; }

        .usd-compact-row {
            display: flex;
            background: var(--light-gold);
            padding: 12px;
            border-radius: 12px;
            align-items: center;
            justify-content: space-between;
            border: 1px solid #e9e0c8;
        }

        .rate-link {
            font-size: 10px;
            color: var(--state-green);
            text-decoration: none;
            display: block;
            margin-top: 4px;
            font-weight: 600;
        }

        .reset-btn {
            width: 100%; padding: 15px; background-color: var(--state-green);
            color: var(--state-gold-light); border: 2px solid var(--state-gold-light); 
            border-radius: 15px; font-size: 16px; font-weight: 900; cursor: pointer; margin-top: 10px;
            transition: 0.3s;
        }

        .reset-btn:hover { background: var(--state-gold-light); color: var(--state-green); }

        .footer { background: #f8f8f8; padding: 15px; text-align: center; font-size: 11px; color: #777; }

        .error-glow { border-color: var(--danger-red) !important; background: #fff5f5 !important; }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <img src="https://syrian.zone/syid/logo.png" alt="شعار سوريا" class="logo">
        <h1>محول القيم النقدية الموحد</h1>
    </div>

    <div class="content">
        <div class="section">
            <span class="section-title">1. المبلغ المطلوب (السعر)</span>
            <div class="main-inputs">
                <div class="input-wrapper">
                    <label>بالعملة الجديدة</label>
                    <input type="text" id="totalNew" placeholder="0">
                </div>
                <div style="font-weight: bold; color: var(--state-gold-light);">↔</div>
                <div class="input-wrapper">
                    <label>بالعملة القديمة</label>
                    <input type="text" id="totalOld" placeholder="0">
                </div>
            </div>

            <div class="usd-compact-row">
                <div>
                    <label style="text-align:right">سعر الصرف ($1)</label>
                    <input type="text" id="exchangeRateInput" value="150" style="width:80px; height:30px; font-size:14px;">
                    <a href="https://sp-today.com/currency/us-dollar" target="_blank" class="rate-link">تحديث من الليرة اليوم 🔗</a>
                </div>
                <div>
                    <label>السعر بالدولار</label>
                    <input type="text" id="totalUSD" placeholder="0.00" style="width:100px; height:35px; font-size:16px;">
                </div>
            </div>
        </div>

        <div class="section" style="background: rgba(5, 66, 57, 0.02);">
            <span class="section-title">2. المبالغ المتوفرة معك</span>
            <div class="main-inputs">
                <div class="input-wrapper">
                    <label>معي بالجديدة</label>
                    <input type="text" id="paidNew" placeholder="0">
                </div>
                <div class="input-wrapper">
                    <label>معي بالقديمة</label>
                    <input type="text" id="paidOld" placeholder="0">
                </div>
            </div>
        </div>

        <button id="resetBtn" class="reset-btn">تصفير كافة الحقول ↺</button>
    </div>

    <div class="footer">
        1 ليرة جديدة = 100 ليرة قديمة | جميع الحقول مترابطة تلقائياً
    </div>
</div>

<script>
    const inputs = {
        totalNew: document.getElementById('totalNew'),
        totalOld: document.getElementById('totalOld'),
        totalUSD: document.getElementById('totalUSD'),
        paidNew: document.getElementById('paidNew'),
        paidOld: document.getElementById('paidOld'),
        rate: document.getElementById('exchangeRateInput')
    };

    // دالة تنظيف الرقم من الفواصل
    function clean(val) { return parseFloat(val.toString().replace(/,/g, '')) || 0; }

    // دالة إضافة الفواصل (1,000,000)
    function format(num, decimals = 0) {
        if (num === 0) return '';
        let parts = num.toFixed(decimals).split('.');
        parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        return parts.join('.').replace(/\.00$/, '');
    }

    // تحديث الحسابات بناءً على الحقل المتغير
    function update(source) {
        let r = clean(inputs.rate.value);
        let tN = clean(inputs.totalNew.value);
