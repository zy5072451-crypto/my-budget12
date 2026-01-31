<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="تطبيق الجرد الشهري للمصروفات - إدارة ميزانيتك الشخرية بسهولة">
  <title>💰 تطبيق الجرد الشهري للمصروفات</title>
  
  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700&display=swap" rel="stylesheet">
  
  <!-- Styles -->
 <link rel="stylesheet" href="./css/style.css">

</head>
<body>
  
  <!-- Header -->
  <header class="app-header">
    <div class="container">
      <div class="header-content">
        <h1 class="app-title">
          <span class="app-title-icon">💰</span>
          <span class="app-title-text">الجرد الشهري للمصروفات</span>
        </h1>
        
        <div class="header-actions">
          <button class="btn btn-secondary btn-sm" id="showReport" title="عرض التقرير الشهري">
            📊 التقرير الشهري
          </button>
          <div class="theme-toggle" id="themeToggle" title="تبديل الوضع الليلي"></div>
        </div>
      </div>
    </div>
  </header>

  <!-- Main Content -->
  <main class="app-wrapper">
    <div class="container">
      
      <!-- Month Navigation -->
      <div class="card mb-2">
        <div style="display: flex; justify-content: space-between; align-items: center;">
          <button class="btn btn-secondary btn-icon" id="nextMonth" title="الشهر التالي">◀</button>
          <h2 id="currentMonthName" style="margin: 0;">جاري التحميل...</h2>
          <button class="btn btn-secondary btn-icon" id="prevMonth" title="الشهر السابق">▶</button>
        </div>
      </div>
      
      <!-- Summary Cards -->
      <div class="summary-grid">
        <!-- Balance Card -->
        <div class="card summary-card balance">
          <div class="summary-card-header">
            <span class="summary-card-title">الرصيد الحالي</span>
            <span class="summary-card-icon">💰</span>
          </div>
          <div class="summary-card-amount" id="totalBalance">0 د.ع</div>
        </div>
        
        <!-- Income Card -->
        <div class="card summary-card income">
          <div class="summary-card-header">
            <span class="summary-card-title">إجمالي الدخل</span>
            <span class="summary-card-icon">📈</span>
          </div>
          <div class="summary-card-amount" id="totalIncome">0 د.ع</div>
        </div>
        
        <!-- Expense Card -->
        <div class="card summary-card expense">
          <div class="summary-card-header">
            <span class="summary-card-title">إجمالي المصروفات</span>
            <span class="summary-card-icon">📉</span>
          </div>
          <div class="summary-card-amount" id="totalExpense">0 د.ع</div>
        </div>
      </div>
      
      <!-- Add Transaction Form -->
      <section class="form-section">
        <div class="card">
          <h2>➕ إضافة عملية جديدة</h2>
          
          <form id="transactionForm">
            <div class="form-grid">
              <!-- Transaction Type -->
              <div class="form-group">
                <label for="transactionType" class="form-label">نوع العملية *</label>
                <select id="transactionType" name="type" class="form-select" required>
                  <option value="">اختر نوع العملية</option>
                  <option value="salary">💰 راتب</option>
                  <option value="expense">💸 مصروف</option>
                  <option value="deposit">📥 إيداع</option>
                  <option value="withdraw">📤 سحب</option>
                </select>
              </div>
              
              <!-- Amount -->
              <div class="form-group">
                <label for="transactionAmount" class="form-label">المبلغ (د.ع) *</label>
                <input 
                  type="number" 
                  id="transactionAmount" 
                  name="amount" 
                  class="form-input" 
                  placeholder="0"
                  min="0"
                  step="1"
                  required
                >
              </div>
              
              <!-- Date -->
              <div class="form-group">
                <label for="transactionDate" class="form-label">التاريخ *</label>
                <input 
                  type="date" 
                  id="transactionDate" 
                  name="date" 
                  class="form-input"
                  required
                >
              </div>
              
              <!-- Description -->
              <div class="form-group">
                <label for="transactionDescription" class="form-label">الوصف *</label>
                <input 
                  type="text" 
                  id="transactionDescription" 
                  name="description" 
                  class="form-input" 
                  placeholder="مثال: راتب شهر يناير، فاتورة كهرباء، إلخ..."
                  required
                >
              </div>
            </div>
            
            <!-- Notes (Full Width) -->
            <div class="form-group mt-1">
              <label for="transactionNotes" class="form-label">ملاحظات إضافية</label>
              <textarea 
                id="transactionNotes" 
                name="notes" 
                class="form-textarea" 
                placeholder="أي ملاحظات إضافية (اختياري)..."
              ></textarea>
            </div>
            
            <!-- Submit Button -->
            <div class="form-actions">
              <button type="submit" class="btn btn-primary">
                ✓ إضافة العملية
              </button>
              <button type="reset" class="btn btn-secondary">
                ✕ مسح النموذج
              </button>
            </div>
          </form>
        </div>
      </section>
      
      <!-- Transactions Table -->
      <section class="table-section">
        <div class="card">
          <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: var(--spacing-lg); flex-wrap: wrap; gap: var(--spacing-md);">
            <h2 style="margin: 0;">📋 سجل العمليات</h2>
            
            <!-- Filter -->
            <div class="filters">
              <div class="filter-group">
                <select id="filterType" class="form-select">
                  <option value="all">جميع العمليات</option>
                  <option value="salary">الرواتب فقط</option>
                  <option value="expense">المصروفات فقط</option>
                  <option value="deposit">الإيداعات فقط</option>
                  <option value="withdraw">السحوبات فقط</option>
                </select>
              </div>
            </div>
          </div>
          
          <div class="table-wrapper">
            <table class="table">
              <thead>
                <tr>
                  <th>التاريخ</th>
                  <th>النوع</th>
                  <th>الوصف</th>
                  <th>المبلغ</th>
                  <th>الملاحظات</th>
                  <th>إجراءات</th>
                </tr>
              </thead>
              <tbody id="transactionsTableBody">
                <tr>
                  <td colspan="6" class="empty-state">
                    <div class="empty-state-icon">📋</div>
                    <div class="empty-state-text">لا توجد عمليات لعرضها</div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </section>
      
      <!-- Export/Import Section -->
      <section class="form-section">
        <div class="card">
          <h3>💾 النسخ الاحتياطي</h3>
          <p style="color: var(--text-secondary); margin-bottom: var(--spacing-lg);">
            يمكنك تصدير بياناتك كنسخة احتياطية أو استيراد بيانات سابقة
          </p>
          <div class="form-actions">
            <button class="btn btn-primary" id="exportData">
              📤 تصدير البيانات
            </button>
            <button class="btn btn-secondary" id="importData">
              📥 استيراد البيانات
            </button>
          </div>
        </div>
      </section>
      
    </div>
  </main>
  
  <!-- Footer -->
  <footer style="text-align: center; padding: var(--spacing-xl); color: var(--text-tertiary);">
    <p>تطبيق الجرد الشهري للمصروفات © 2026</p>
  </footer>
  
  <!-- Scripts -->
  <script src="js/storage.js"></script>
  <script src="js/app.js"></script>
  
</body>
</html>
/* ===================================
   Design System & CSS Variables
   =================================== */
:root {
  /* Colors - Light Mode */
  --primary-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  --primary-color: #667eea;
  --primary-dark: #5568d3;
  --secondary-color: #764ba2;
  
  --success-color: #10b981;
  --success-light: #d1fae5;
  --danger-color: #ef4444;
  --danger-light: #fee2e2;
  --warning-color: #f59e0b;
  --warning-light: #fef3c7;
  --info-color: #3b82f6;
  --info-light: #dbeafe;
  
  --bg-primary: #f8fafc;
  --bg-secondary: #ffffff;
  --bg-tertiary: #f1f5f9;
  
  --text-primary: #1e293b;
  --text-secondary: #64748b;
  --text-tertiary: #94a3b8;
  
  --border-color: #e2e8f0;
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  
  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  --spacing-2xl: 3rem;
  
  /* Border Radius */
  --radius-sm: 0.375rem;
  --radius-md: 0.5rem;
  --radius-lg: 0.75rem;
  --radius-xl: 1rem;
  --radius-full: 9999px;
  
  /* Transitions */
  --transition-fast: 150ms ease-in-out;
  --transition-base: 250ms ease-in-out;
  --transition-slow: 350ms ease-in-out;
}

/* Dark Mode Variables */
[data-theme="dark"] {
  --bg-primary: #0f172a;
  --bg-secondary: #1e293b;
  --bg-tertiary: #334155;
  
  --text-primary: #f1f5f9;
  --text-secondary: #cbd5e1;
  --text-tertiary: #94a3b8;
  
  --border-color: #334155;
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.3);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.4), 0 2px 4px -1px rgba(0, 0, 0, 0.3);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.4), 0 4px 6px -2px rgba(0, 0, 0, 0.3);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.5), 0 10px 10px -5px rgba(0, 0, 0, 0.4);
}

/* ===================================
   Base Styles & Reset
   =================================== */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  font-size: 16px;
  scroll-behavior: smooth;
}

body {
  font-family: 'Cairo', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  background: var(--bg-primary);
  color: var(--text-primary);
  line-height: 1.6;
  direction: rtl;
  text-align: right;
  transition: background-color var(--transition-base), color var(--transition-base);
  min-height: 100vh;
}

/* ===================================
   Typography
   =================================== */
h1, h2, h3, h4, h5, h6 {
  font-weight: 700;
  line-height: 1.2;
  margin-bottom: var(--spacing-md);
}

h1 { font-size: 2.5rem; }
h2 { font-size: 2rem; }
h3 { font-size: 1.75rem; }
h4 { font-size: 1.5rem; }
h5 { font-size: 1.25rem; }
h6 { font-size: 1rem; }

p {
  margin-bottom: var(--spacing-md);
}

/* ===================================
   Layout Components
   =================================== */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-lg);
}

.app-wrapper {
  min-height: 100vh;
  padding: var(--spacing-xl) 0;
}

/* ===================================
   Header
   =================================== */
.app-header {
  background: var(--bg-secondary);
  border-bottom: 1px solid var(--border-color);
  padding: var(--spacing-lg) 0;
  margin-bottom: var(--spacing-2xl);
  box-shadow: var(--shadow-sm);
  position: sticky;
  top: 0;
  z-index: 100;
  backdrop-filter: blur(10px);
  background: rgba(255, 255, 255, 0.9);
}

[data-theme="dark"] .app-header {
  background: rgba(30, 41, 59, 0.9);
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: var(--spacing-lg);
}

.app-title {
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
  margin: 0;
}

.app-title-icon {
  font-size: 2.5rem;
  background: var(--primary-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.app-title-text {
  background: var(--primary-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-size: 1.75rem;
}

.header-actions {
  display: flex;
  gap: var(--spacing-md);
  align-items: center;
}

/* ===================================
   Buttons
   =================================== */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-sm);
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  font-family: 'Cairo', sans-serif;
  border: none;
  border-radius: var(--radius-lg);
  cursor: pointer;
  transition: all var(--transition-base);
  text-decoration: none;
  white-space: nowrap;
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
}

.btn:active {
  transform: translateY(0);
}

.btn-primary {
  background: var(--primary-gradient);
  color: white;
}

.btn-primary:hover {
  filter: brightness(1.1);
}

.btn-secondary {
  background: var(--bg-tertiary);
  color: var(--text-primary);
}

.btn-secondary:hover {
  background: var(--border-color);
}

.btn-icon {
  padding: 0.75rem;
  aspect-ratio: 1;
}

.btn-sm {
  padding: 0.5rem 1rem;
  font-size: 0.875rem;
}

/* ===================================
   Cards
   =================================== */
.card {
  background: var(--bg-secondary);
  border-radius: var(--radius-xl);
  padding: var(--spacing-xl);
  box-shadow: var(--shadow-md);
  border: 1px solid var(--border-color);
  transition: all var(--transition-base);
}

.card:hover {
  box-shadow: var(--shadow-lg);
}

.card-glass {
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}

[data-theme="dark"] .card-glass {
  background: rgba(30, 41, 59, 0.7);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

/* ===================================
   Summary Cards
   =================================== */
.summary-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--spacing-lg);
  margin-bottom: var(--spacing-2xl);
}

.summary-card {
  position: relative;
  overflow: hidden;
}

.summary-card::before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  width: 100%;
  height: 4px;
  background: var(--primary-gradient);
}

.summary-card.income::before {
  background: linear-gradient(135deg, #10b981 0%, #059669 100%);
}

.summary-card.expense::before {
  background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
}

.summary-card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: var(--spacing-md);
}

.summary-card-title {
  font-size: 1rem;
  font-weight: 600;
  color: var(--text-secondary);
  margin: 0;
}

.summary-card-icon {
  font-size: 2rem;
  opacity: 0.8;
}

.summary-card-amount {
  font-size: 2rem;
  font-weight: 700;
  margin: 0;
}

.summary-card.balance .summary-card-amount {
  background: var(--primary-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.summary-card.income .summary-card-amount {
  color: var(--success-color);
}

.summary-card.expense .summary-card-amount {
  color: var(--danger-color);
}

/* ===================================
   Forms
   =================================== */
.form-section {
  margin-bottom: var(--spacing-2xl);
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--spacing-lg);
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-sm);
}

.form-label {
  font-weight: 600;
  font-size: 0.875rem;
  color: var(--text-primary);
}

.form-input,
.form-select,
.form-textarea {
  padding: 0.75rem 1rem;
  font-size: 1rem;
  font-family: 'Cairo', sans-serif;
  border: 2px solid var(--border-color);
  border-radius: var(--radius-md);
  background: var(--bg-primary);
  color: var(--text-primary);
  transition: all var(--transition-base);
  direction: rtl;
}

.form-input:focus,
.form-select:focus,
.form-textarea:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.form-textarea {
  resize: vertical;
  min-height: 80px;
}

.form-actions {
  display: flex;
  gap: var(--spacing-md);
  margin-top: var(--spacing-lg);
}

/* ===================================
   Table
   =================================== */
.table-section {
  margin-bottom: var(--spacing-2xl);
}

.table-wrapper {
  overflow-x: auto;
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-md);
}

.table {
  width: 100%;
  border-collapse: collapse;
  background: var(--bg-secondary);
}

.table thead {
  background: var(--primary-gradient);
  color: white;
}

.table th {
  padding: 1rem;
  font-weight: 600;
  text-align: right;
  white-space: nowrap;
}

.table td {
  padding: 1rem;
  border-bottom: 1px solid var(--border-color);
}

.table tbody tr {
  transition: background-color var(--transition-fast);
}

.table tbody tr:hover {
  background: var(--bg-tertiary);
}

.table tbody tr:last-child td {
  border-bottom: none;
}

/* ===================================
   Badges & Tags
   =================================== */
.badge {
  display: inline-flex;
  align-items: center;
  gap: var(--spacing-xs);
  padding: 0.25rem 0.75rem;
  font-size: 0.875rem;
  font-weight: 600;
  border-radius: var(--radius-full);
  white-space: nowrap;
}

.badge-income {
  background: var(--success-light);
  color: var(--success-color);
}

.badge-expense {
  background: var(--danger-light);
  color: var(--danger-color);
}

.badge-deposit {
  background: var(--info-light);
  color: var(--info-color);
}

.badge-withdraw {
  background: var(--warning-light);
  color: var(--warning-color);
}

[data-theme="dark"] .badge-income {
  background: rgba(16, 185, 129, 0.2);
  color: #6ee7b7;
}

[data-theme="dark"] .badge-expense {
  background: rgba(239, 68, 68, 0.2);
  color: #fca5a5;
}

[data-theme="dark"] .badge-deposit {
  background: rgba(59, 130, 246, 0.2);
  color: #93c5fd;
}

[data-theme="dark"] .badge-withdraw {
  background: rgba(245, 158, 11, 0.2);
  color: #fcd34d;
}

/* ===================================
   Action Buttons in Table
   =================================== */
.action-btn {
  padding: 0.5rem;
  border: none;
  background: transparent;
  cursor: pointer;
  font-size: 1.25rem;
  transition: all var(--transition-fast);
  border-radius: var(--radius-sm);
}

.action-btn:hover {
  background: var(--bg-tertiary);
  transform: scale(1.1);
}

.action-btn-delete {
  color: var(--danger-color);
}

.action-btn-delete:hover {
  background: var(--danger-light);
}

/* ===================================
   Empty State
   =================================== */
.empty-state {
  text-align: center;
  padding: var(--spacing-2xl);
  color: var(--text-secondary);
}

.empty-state-icon {
  font-size: 4rem;
  opacity: 0.3;
  margin-bottom: var(--spacing-md);
}

.empty-state-text {
  font-size: 1.125rem;
}

/* ===================================
   Modal
   =================================== */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: var(--spacing-lg);
  opacity: 0;
  animation: fadeIn var(--transition-base) forwards;
}

@keyframes fadeIn {
  to { opacity: 1; }
}

.modal {
  background: var(--bg-secondary);
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-xl);
  max-width: 800px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  transform: scale(0.9);
  animation: scaleIn var(--transition-base) forwards;
}

@keyframes scaleIn {
  to { transform: scale(1); }
}

.modal-header {
  padding: var(--spacing-xl);
  border-bottom: 1px solid var(--border-color);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-title {
  margin: 0;
  font-size: 1.5rem;
}

.modal-close {
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: var(--text-secondary);
  transition: color var(--transition-fast);
  padding: 0.5rem;
}

.modal-close:hover {
  color: var(--text-primary);
}

.modal-body {
  padding: var(--spacing-xl);
}

/* ===================================
   Filters
   =================================== */
.filters {
  display: flex;
  gap: var(--spacing-md);
  flex-wrap: wrap;
  margin-bottom: var(--spacing-lg);
}

.filter-group {
  flex: 1;
  min-width: 200px;
}

/* ===================================
   Theme Toggle
   =================================== */
.theme-toggle {
  position: relative;
  width: 60px;
  height: 30px;
  background: var(--bg-tertiary);
  border-radius: var(--radius-full);
  cursor: pointer;
  transition: background-color var(--transition-base);
  border: 2px solid var(--border-color);
}

.theme-toggle::before {
  content: '🌙';
  position: absolute;
  top: 50%;
  right: 4px;
  transform: translateY(-50%);
  width: 22px;
  height: 22px;
  background: var(--primary-gradient);
  border-radius: 50%;
  transition: all var(--transition-base);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.75rem;
}

[data-theme="dark"] .theme-toggle::before {
  content: '☀️';
  right: calc(100% - 26px);
}

/* ===================================
   Animations
   =================================== */
@keyframes slideInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-in {
  animation: slideInUp var(--transition-base) ease-out;
}

/* ===================================
   Utilities
   =================================== */
.hidden {
  display: none !important;
}

.text-center {
  text-align: center;
}

.text-success {
  color: var(--success-color);
}

.text-danger {
  color: var(--danger-color);
}

.mt-1 { margin-top: var(--spacing-md); }
.mt-2 { margin-top: var(--spacing-lg); }
.mb-1 { margin-bottom: var(--spacing-md); }
.mb-2 { margin-bottom: var(--spacing-lg); }

/* ===================================
   Responsive Design
   =================================== */
@media (max-width: 768px) {
  html {
    font-size: 14px;
  }
  
  .header-content {
    flex-direction: column;
    text-align: center;
  }
  
  .summary-grid {
    grid-template-columns: 1fr;
  }
  
  .form-grid {
    grid-template-columns: 1fr;
  }
  
  .filters {
    flex-direction: column;
  }
  
  .table-wrapper {
    border-radius: var(--radius-md);
  }
  
  .table th,
  .table td {
    padding: 0.75rem 0.5rem;
    font-size: 0.875rem;
  }
  
  .btn {
    width: 100%;
  }
  
  .form-actions {
    flex-direction: column;
  }
}

@media (max-width: 480px) {
  .app-title-text {
    font-size: 1.25rem;
  }
  
  .summary-card-amount {
    font-size: 1.5rem;
  }
}

/* ===================================
   Print Styles
   =================================== */
@media print {
  .app-header,
  .form-section,
  .filters,
  .action-btn,
  .btn {
    display: none;
  }
  
  .table {
    box-shadow: none;
  }
}
