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
  <link rel="stylesheet" href="css/style.css">
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
