<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>💰 تطبيق إدارة الميزانية</title>
    <!-- بداية أكواد الـ CSS المدمجة -->
    <style>
        :root {
            --primary-color: #007bff;
            --secondary-color: #6c757d;
            --background-color: #f8f9fa;
            --card-bg: #ffffff;
            --text-color: #333;
            --income-color: #28a745;
            --expense-color: #dc3545;
        }

        body {
            font-family: sans-serif;
            background-color: var(--background-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            line-height: 1.6;
            direction: rtl; /* يدعم اللغة العربية */
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin-bottom: 20px;
        }

        .summary-cards {
            display: flex;
            justify-content: space-between;
            gap: 15px;
            margin-bottom: 20px;
        }

        .card {
            background-color: var(--card-bg);
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            flex: 1;
        }

        .balance { border-right: 5px solid var(--primary-color); }
        .income { border-right: 5px solid var(--income-color); color: var(--income-color); }
        .expense { border-right: 5px solid var(--expense-color); color: var(--expense-color); }

        form {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        input, select, button {
            padding: 10px;
            border-radius: 4px;
            border: 1px solid var(--secondary-color);
        }

        button {
            background-color: var(--primary-color);
            color: white;
            border: none;
            cursor: pointer;
        }

        #transaction-list {
            list-style: none;
            padding: 0;
        }

        #transaction-list li {
            background-color: var(--card-bg);
            margin-bottom: 10px;
            padding: 15px;
            border-radius: 5px;
            display: flex;
            justify-content: space-between;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }

        .delete-btn {
            background-color: var(--expense-color);
            padding: 5px 10px;
        }
    </style>
    <!-- نهاية أكواد الـ CSS المدمجة -->
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

    <!-- بداية أكواد الـ JavaScript المدمجة -->
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const balanceEl = document.getElementById('balance-amount');
            const incomeEl = document.getElementById('income-amount');
            const expenseEl = document.getElementById('expense-amount');
            const form = document.getElementById('transaction-form');
            const transactionList = document.getElementById('transaction-list');

            let transactions = JSON.parse(localStorage.getItem('transactions')) || [];

            function updateDOM() {
                transactionList.innerHTML = '';
                transactions.forEach(addTransactionDOM);
                updateSummary();
            }

            function addTransactionDOM(transaction) {
                const sign = transaction.type === 'income' ? '+' : '-';
                const li = document.createElement('li');
                li.classList.add(transaction.type);
                li.innerHTML = `
                    ${transaction.description} <span>${sign}${Math.abs(transaction.amount)} د.ع</span>
                    <button class="delete-btn" onclick="removeTransaction('${transaction.id}')">X</button>
                `;
                transactionList.appendChild(li);
            }

            function updateSummary() {
                const amounts = transactions.map(t => t.amount * (t.type === 'income' ? 1 : -1));
                const total = amounts.reduce((acc, item) => (acc += item), 0).toFixed(2);
                const incomeTotal = transactions.filter(t => t.type === 'income').map(t => t.amount).reduce((acc, item) => (acc += item), 0).toFixed(2);
                const expenseTotal = transactions.filter(t => t.type === 'expense').map(t => t.amount).reduce((acc, item) => (acc += item), 0).toFixed(2);

                balanceEl.innerText = `${total} د.ع`;
                incomeEl.innerText = `${incomeTotal} د.ع`;
                expenseEl.innerText = `${expenseTotal} د.ع`;
            }

            function addTransaction(e) {
                e.preventDefault();
                const description = document.getElementById('description').value;
                const amount = parseFloat(document.getElementById('amount').value);
                const type = document.getElementById('type').value;

                if (description && amount > 0) {
                    const transaction = {
                        id: Date.now().toString(),
                        description,
                        amount,
                        type
                    };

                    transactions.push(transaction);
                    saveTransactions();
                    updateDOM();
                    form.reset();
                }
            }

            function removeTransaction(id) {
                transactions = transactions.filter(transaction => transaction.id !== id);
                saveTransactions();
                updateDOM();
            }

            function saveTransactions() {
                localStorage.setItem('transactions', JSON.stringify(transactions));
            }

            window.removeTransaction = removeTransaction; 

            form.addEventListener('submit', addTransaction);
            updateDOM();
        });
    </script>
    <!-- نهاية أكواد الـ JavaScript المدمجة -->
</body>
</html>
