<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تطبيق الميزانية البسيط</title>
    <!-- رابط ملف الستايل (CSS) -->
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>💰 إدارة الميزانية</h1>
        </header>

        <div class="summary-cards">
            <div class="card balance">
                <h3>الرصيد</h3>
                <p id="balance-amount">0 د.ع</p>
            </div>
            <div class="card income">
                <h3>الدخل</h3>
                <p id="income-amount">0 د.ع</p>
            </div>
            <div class="card expense">
                <h3>المصروفات</h3>
                <p id="expense-amount">0 د.ع</p>
            </div>
        </div>

        <section class="form-section">
            <h2>إضافة عملية جديدة</h2>
            <form id="transaction-form">
                <input type="text" id="description" placeholder="الوصف (فاتورة كهرباء)" required>
                <input type="number" id="amount" placeholder="المبلغ (د.ع)" required min="0">
                <select id="type">
                    <option value="expense">مصروف</option>
                    <option value="income">دخل</option>
                </select>
                <button type="submit">إضافة</button>
            </form>
        </section>

        <section class="list-section">
            <h2>سجل العمليات</h2>
            <ul id="transaction-list">
                <!-- العمليات ستضاف هنا بواسطة JavaScript -->
            </ul>
        </section>
    </div>

    <!-- رابط ملف الجافاسكريبت (JS) -->
    <script src="script.js"></script>
</body>
</html>
