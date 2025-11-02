# 📘 دوكيومنتيشن الباك اند الشاملة

## 📑 فهرس المحتويات

1. [مراحل البناء والتاسكات](#مراحل-البناء-والتاسكات)
2. [مراجع قاعدة البيانات](#مراجع-قاعدة-البيانات)
3. [الراوتس الكاملة](#الراوتس-الكاملة)
4. [معايير التطوير](#معايير-التطوير)
5. [الأخطاء والحلول](#الأخطاء-والحلول)

---

# 🎯 مراحل البناء والتاسكات

## المرحلة الأولى: إعداد الأساسيات ✅

### Task 1.1: تنظيم هيكل المشروع الأساسي
- [x] إنشاء مجلدات `modules` منفصلة لكل إنتيتي
- [x] إعداد `config/database.js` و `config/env.js`
- [x] إعداد `app.js` الرئيسي
- [x] إعداد `Prisma schema`

**الملفات المطلوبة:**
```
src/modules/{entityName}/
├── {entity}.controller.js      # معالجة الطلبات
├── {entity}.service.js         # منطق الأعمال
├── {entity}.repository.js      # الوصول للبيانات
├── {entity}.routes.js          # تعريف الروتس
├── {entity}.schema.js          # التحقق من صحة البيانات
└── {entity}.calculations.js    # الحسابات الخاصة (إن وجدت)
```

---

## المرحلة الثانية: بناء الـ Entities الأساسية 🔨

### Task 2.1: Companies Module ⭐ أولوية عالية
**الوصف:** إدارة الشركات والنسخ وبيانات الاشتراك

**الحقول المطلوبة:**
```typescript
Company {
  id: Int (PK)
  name: String (required, unique per company)
  logo: String (URL)
  address: String
  email: String (required, unique)
  phone: String
  subscriptionExpiryDate: DateTime
  createdAt: DateTime (auto)
  updatedAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/companies` - الحصول على جميع الشركات
- `GET /api/companies/:id` - الحصول على شركة محددة
- `POST /api/companies` - إنشاء شركة جديدة
- `PUT /api/companies/:id` - تحديث بيانات الشركة
- `PUT /api/companies/:id/subscription` - تحديث تاريخ الاشتراك
- `DELETE /api/companies/:id` - حذف شركة

**الحسابات:**
- التحقق من صلاحية الاشتراك
- عداد الأيام المتبقية

---

### Task 2.2: Users Module ⭐ أولوية عالية
**الوصف:** إدارة المستخدمين والأدوار والصلاحيات

**الحقول المطلوبة:**
```typescript
User {
  id: Int (PK)
  companyId: Int (FK → Company)
  fullName: String (required)
  email: String (required, unique per company)
  passwordHash: String (required, bcrypt)
  role: Enum (manager, employee, admin, accountant)
  status: Enum (Active, Inactive, Suspended)
  lastLoginAt: DateTime (nullable)
  createdAt: DateTime (auto)
  updatedAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/users` - الحصول على المستخدمين
- `GET /api/users/:id` - الحصول على مستخدم محدد
- `GET /api/users/company/:companyId` - المستخدمين بالشركة
- `POST /api/users` - إنشاء مستخدم جديد
- `PUT /api/users/:id` - تحديث بيانات المستخدم
- `PUT /api/users/:id/password` - تغيير كلمة المرور
- `DELETE /api/users/:id` - حذف مستخدم

**المنطق:**
- التحقق من البريد الفريد لكل شركة
- هاش كلمة المرور باستخدام `bcrypt`
- التحقق من الأدوار المسموحة

---

### Task 2.3: Suppliers Module
**الوصف:** إدارة الموردين

**الحقول المطلوبة:**
```typescript
Supplier {
  id: Int (PK)
  companyId: Int (FK → Company)
  name: String (required)
  contactInfo: String (phones, emails)
  address: String (nullable)
  city: String (nullable)
  phone: String
  email: String
  notes: String (nullable)
  isActive: Boolean (default: true)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/suppliers` - الحصول على جميع الموردين
- `GET /api/suppliers/:id` - تفاصيل مورد
- `POST /api/suppliers` - إضافة مورد جديد
- `PUT /api/suppliers/:id` - تحديث بيانات مورد
- `DELETE /api/suppliers/:id` - حذف مورد

---

### Task 2.4: Products Module
**الوصف:** إدارة المنتجات والمخزون

**الحقول المطلوبة:**
```typescript
Product {
  id: Int (PK)
  companyId: Int (FK → Company)
  supplierId: Int (FK → Supplier)
  name: String (required)
  description: String (nullable)
  category: String (required)
  price: Decimal (required)
  stock: Int (default: 0)
  minStockLevel: Int (default: 10)
  sku: String (unique per company, nullable)
  image: String (URL, nullable)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/products` - جميع المنتجات
- `GET /api/products/:id` - تفاصيل منتج
- `GET /api/products/low-stock` - المنتجات قليلة المخزون
- `GET /api/products/categories` - جميع الفئات
- `POST /api/products` - إنشاء منتج
- `PUT /api/products/:id` - تحديث منتج
- `DELETE /api/products/:id` - حذف منتج

**الحسابات:**
- تنبيهات المخزون المنخفض (stock < minStockLevel)
- عداد المنتجات قليلة المخزون

---

### Task 2.5: Accessories Module
**الوصف:** إدارة الملحقات والإضافات

**الحقول المطلوبة:**
```typescript
Accessory {
  id: Int (PK)
  companyId: Int (FK → Company)
  supplierId: Int (FK → Supplier)
  name: String (required)
  price: Decimal (required)
  stock: Int (default: 0)
  category: String (nullable)
  sku: String (unique per company)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/accessories` - جميع الملحقات
- `GET /api/accessories/:id` - تفاصيل ملحق
- `POST /api/accessories` - إنشاء ملحق
- `PUT /api/accessories/:id` - تحديث ملحق
- `DELETE /api/accessories/:id` - حذف ملحق

---

### Task 2.6: Services Module
**الوصف:** إدارة الخدمات المقدمة

**الحقول المطلوبة:**
```typescript
Service {
  id: Int (PK)
  companyId: Int (FK → Company)
  name: String (required)
  description: String (nullable)
  price: Decimal (required)
  category: String (nullable)
  duration: Int (بالدقائق, nullable)
  isActive: Boolean (default: true)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/services` - جميع الخدمات
- `GET /api/services/:id` - تفاصيل خدمة
- `POST /api/services` - إنشاء خدمة
- `PUT /api/services/:id` - تحديث خدمة
- `DELETE /api/services/:id` - حذف خدمة

---

## المرحلة الثالثة: Entities الوسيطة 🔗

### Task 3.1: ProductAccessory Module
**الوصف:** الربط بين المنتجات والملحقات (علاقة Many-to-Many)

**الحقول:**
```typescript
ProductAccessory {
  productId: Int (FK)
  accessoryId: Int (FK)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/product-accessories` - جميع العلاقات
- `GET /api/products/:productId/accessories` - ملحقات منتج
- `POST /api/product-accessories` - إضافة علاقة (مع منع التكرار)
- `DELETE /api/product-accessories/:productId/:accessoryId` - حذف علاقة

---

## المرحلة الرابعة: Customers و Employees 👥

### Task 4.1: Customers Module ⭐ أولوية عالية
**الوصف:** إدارة العملاء والبيانات الشخصية

**الحقول المطلوبة:**
```typescript
Customer {
  id: Int (PK)
  companyId: Int (FK → Company)
  fullName: String (required)
  nationalId: String (required, unique per company)
  customerType: Enum (Installation, Maintenance)
  idCardImage: String (URL, nullable)
  primaryNumber: String (required)
  secondaryNumber: String (nullable)
  governorate: String (required)
  city: String (required)
  district: String (required)
  address: String (nullable)
  notes: String (nullable)
  isActive: Boolean (default: true)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/customers` - جميع العملاء
- `GET /api/customers/:id` - بيانات عميل
- `GET /api/customers/type/:customerType` - العملاء حسب النوع
- `GET /api/customers/governorates` - جميع المحافظات
- `GET /api/customers/cities/:governorate` - المدن بمحافظة
- `GET /api/customers/count` - عداد العملاء
- `POST /api/customers` - إضافة عميل جديد
- `PUT /api/customers/:id` - تحديث بيانات عميل
- `DELETE /api/customers/:id` - حذف عميل

**ملحوظات:**
- دعم رفع صور البطاقة (FormData)
- التحقق من الرقم القومي الفريد لكل شركة

---

### Task 4.2: Employees Module
**الوصف:** إدارة الموظفين والفنيين

**الحقول المطلوبة:**
```typescript
Employee {
  id: Int (PK)
  companyId: Int (FK → Company)
  fullName: String (required)
  nationalId: String (required, unique per company)
  role: Enum (SalesRep, Technician, Manager, Admin)
  primaryNumber: String (required)
  secondaryNumber: String (nullable)
  idCardImage: String (URL, nullable)
  governorate: String (required)
  city: String (required)
  district: String (required)
  address: String (nullable)
  salary: Decimal (nullable)
  hireDate: DateTime
  isEmployed: Boolean (default: true)
  notes: String (nullable)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/employees` - جميع الموظفين
- `GET /api/employees/:id` - بيانات موظف
- `GET /api/employees/role/:role` - الموظفين حسب الدور
- `GET /api/employees/roles` - جميع الأدوار المتاحة
- `POST /api/employees` - إضافة موظف جديد
- `PUT /api/employees/:id` - تحديث بيانات موظف
- `DELETE /api/employees/:id` - حذف موظف

---

## المرحلة الخامسة: Invoices و Items 🧾

### Task 5.1: Invoices Module ⭐ أولوية عالية جداً
**الوصف:** إدارة الفواتير والعقود

**الحقول المطلوبة:**
```typescript
Invoice {
  id: Int (PK)
  companyId: Int (FK → Company)
  customerId: Int (FK → Customer)
  salesRepId: Int (FK → Employee)
  technicianId: Int (FK → Employee, nullable)
  totalAmount: Decimal (required)
  discountAmount: Decimal (default: 0)
  discountPercentage: Decimal (default: 0)
  finalAmount: Decimal (محسوبة)
  saleType: Enum (Cash, Installment)
  maintenancePeriod: Int (شهور الضمان)
  paidAtContract: Decimal (المدفوع عند الإبرام)
  paidAtInstallation: Decimal (المدفوع عند التركيب)
  installationCostType: Enum (Fixed, Percentage)
  installationCostValue: Decimal
  contractDate: DateTime (required)
  installationDate: DateTime
  contractNotes: String (nullable)
  status: Enum (Draft, Confirmed, Completed, Cancelled)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/invoices` - جميع الفواتير
- `GET /api/invoices/:id` - تفاصيل فاتورة
- `GET /api/invoices/customer/:customerId` - فواتير عميل
- `GET /api/invoices/recent` - آخر 5 فواتير
- `GET /api/invoices/monthly-revenue` - إجمالي الإيرادات الشهري
- `GET /api/invoices/statistics` - إحصائيات الفواتير
- `POST /api/invoices` - إنشاء فاتورة جديدة
- `PUT /api/invoices/:id` - تحديث فاتورة
- `DELETE /api/invoices/:id` - حذف فاتورة

**الحسابات:**
```
finalAmount = totalAmount - discountAmount
installationCost = installationCostType === 'Fixed' 
                   ? installationCostValue 
                   : (totalAmount * installationCostValue) / 100
remainingPayment = totalAmount - paidAtContract - paidAtInstallation
```

---

### Task 5.2: InvoiceItems Module
**الوصف:** بنود الفاتورة (منتجات، خدمات، ملحقات)

**الحقول المطلوبة:**
```typescript
InvoiceItem {
  id: Int (PK)
  invoiceId: Int (FK → Invoice)
  companyId: Int (FK → Company)
  productId: Int (FK → Product, nullable)
  serviceId: Int (FK → Service, nullable)
  accessoryId: Int (FK → Accessory, nullable)
  quantity: Int (required)
  unitPrice: Decimal (required)
  subtotal: Decimal (محسوبة: quantity × unitPrice)
  notes: String (nullable)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/invoice-items` - جميع البنود
- `GET /api/invoices/:invoiceId/items` - بنود فاتورة
- `POST /api/invoice-items` - إضافة بند
- `PUT /api/invoice-items/:id` - تحديث بند
- `DELETE /api/invoice-items/:id` - حذف بند

**المنطق:**
- يجب تحديد واحد فقط من (productId, serviceId, accessoryId)
- الـ subtotal يُحسب تلقائياً
- تحديث totalAmount في الفاتورة عند إضافة/تعديل/حذف بند

---

## المرحلة السادسة: Maintenance 🔧

### Task 6.1: Maintenance Module
**الوصف:** إدارة الصيانة والخدمات

**الحقول المطلوبة:**
```typescript
Maintenance {
  id: Int (PK)
  companyId: Int (FK → Company)
  customerId: Int (FK → Customer)
  productId: Int (FK → Product, nullable)
  serviceId: Int (FK → Service)
  technicianId: Int (FK → Employee)
  maintenanceDate: DateTime (required)
  completionDate: DateTime (nullable)
  price: Decimal
  status: Enum (Pending, InProgress, Completed, Cancelled)
  notes: String (nullable)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/maintenances` - جميع الصيانات
- `GET /api/maintenances/:id` - تفاصيل صيانة
- `GET /api/maintenances/customer/:customerId` - صيانات عميل
- `GET /api/maintenances/upcoming` - الصيانات المعلقة
- `GET /api/maintenances/upcoming-count` - عداد الصيانات المعلقة
- `GET /api/maintenances/upcoming-list` - أول 5 صيانات معلقة
- `POST /api/maintenances` - إنشاء صيانة
- `PUT /api/maintenances/:id` - تحديث صيانة
- `DELETE /api/maintenances/:id` - حذف صيانة

---

## المرحلة السابعة: Installments و Payments 💳

### Task 7.1: Installments Module ⭐ أولوية عالية جداً
**الوصف:** إدارة خطط التقسيط

**الحقول المطلوبة:**
```typescript
Installment {
  id: Int (PK)
  invoiceId: Int (FK → Invoice, unique)
  numberOfMonths: Int (required)
  monthlyInstallment: Decimal (required)
  collectionStartDate: DateTime (required)
  collectionEndDate: DateTime (required)
  totalAmount: Decimal (numberOfMonths × monthlyInstallment)
  amountPaid: Decimal (default: 0)
  isCompleted: Boolean (default: false)
  notes: String (nullable)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/installments` - جميع خطط التقسيط
- `GET /api/installments/:id` - تفاصيل خطة
- `GET /api/installments/invoice/:invoiceId` - خطة فاتورة
- `GET /api/installments/pending-count` - عداد الدفعات المعلقة
- `POST /api/installments` - إنشاء خطة جديدة
- `PUT /api/installments/:id` - تحديث خطة
- `DELETE /api/installments/:id` - حذف خطة

---

### Task 7.2: InstallmentPayments Module ⭐ أولوية عالية جداً
**الوصف:** إدارة دفعات التقسيط والتتبع

**الحقول المطلوبة:**
```typescript
InstallmentPayment {
  id: Int (PK)
  installmentId: Int (FK → Installment)
  customerId: Int (FK → Customer)
  monthNumber: Int (رقم الشهر: 1-12)
  amountDue: Decimal (required)
  amountPaid: Decimal (default: 0)
  carryoverAmount: Decimal (default: 0) // المبلغ المتبقي من الشهر السابق
  overdueAmount: Decimal (default: 0)
  status: Enum (Pending, Partial, Paid, Overdue)
  dueDate: DateTime (required)
  paymentDate: DateTime (nullable)
  notes: String (nullable)
  createdAt: DateTime (auto)
}
```

**الراوتس:**
- `GET /api/installment-payments` - جميع الدفعات
- `GET /api/installment-payments/:id` - تفاصيل دفعة
- `GET /api/installments/:installmentId/payments` - دفعات تقسيط
- `GET /api/installment-payments/customer/:customerId` - دفعات عميل
- `GET /api/payments/overdue-count` - عداد الدفعات المتأخرة
- `GET /api/payments/overdue-list` - قائمة الدفعات المتأخرة
- `POST /api/installment-payments` - تسجيل دفعة جديدة
- `PUT /api/installment-payments/:id` - تحديث دفعة
- `DELETE /api/installment-payments/:id` - حذف دفعة

**منطق الدفعات المعقد:**
```
عند تسجيل دفعة:
1. إذا amountPaid === amountDue → Status = "Paid"
2. إذا 0 < amountPaid < amountDue:
   - Status = "Partial"
   - carryoverAmount = amountDue - amountPaid
   - أضف carryoverAmount للدفعة التالية
3. إذا amountPaid === 0 → Status = "Pending"
4. إذا paymentDate > dueDate → Status = "Overdue"

عند التعديل:
- احذف الـ carryoverAmount القديم من الدفعة التالية
- أضف الـ carryoverAmount الجديد
```

---

## المرحلة الثامنة: التحليلات والتقارير 📊

### Task 8.1: Analytics و Reports Module
**الوصف:** تقارير وإحصائيات المبيعات والأداء

**الراوتس:**
- `GET /api/analytics/dashboard` - ملخص لوحة التحكم
- `GET /api/analytics/revenue/monthly` - الإيرادات الشهرية
- `GET /api/analytics/revenue/yearly` - الإيرادات السنوية
- `GET /api/analytics/sales/by-rep` - المبيعات حسب الموظف
- `GET /api/analytics/products/top-selling` - أكثر المنتجات مبيعة
- `GET /api/analytics/customers/top` - أفضل العملاء
- `GET /api/analytics/payment-status` - حالة الدفعات

**البيانات المرجعة:**
```json
{
  "dashboard": {
    "totalCustomers": 150,
    "totalInvoices": 250,
    "pendingInstallments": 45,
    "overdueDuePayments": 12,
    "lowStockProducts": 8,
    "upcomingMaintenances": 20
  },
  "revenue": {
    "thisMonth": 45000,
    "lastMonth": 38000,
    "thisYear": 420000,
    "growth": "18.4%"
  }
}
```

---

# 📋 مراجع قاعدة البيانات

## العلاقات الرئيسية

```
Company (1) ──→ (Many) User
Company (1) ──→ (Many) Customer
Company (1) ──→ (Many) Employee
Company (1) ──→ (Many) Supplier
Company (1) ──→ (Many) Product
Company (1) ──→ (Many) Accessory
Company (1) ──→ (Many) Service
Company (1) ──→ (Many) Invoice
Company (1) ──→ (Many) Maintenance

Supplier (1) ──→ (Many) Product
Supplier (1) ──→ (Many) Accessory

Product (Many) ←→ (Many) Accessory (ProductAccessory)

Customer (1) ──→ (Many) Invoice
Customer (1) ──→ (Many) Maintenance
Customer (1) ──→ (Many) InstallmentPayment

Employee (1) ──→ (Many) Invoice (as SalesRep)
Employee (1) ──→ (Many) Invoice (as Technician)
Employee (1) ──→ (Many) Maintenance (as Technician)

Invoice (1) ──→ (Many) InvoiceItem
Invoice (1) ──→ (1) Installment

Installment (1) ──→ (Many) InstallmentPayment

Service (1) ──→ (Many) InvoiceItem
Service (1) ──→ (Many) Maintenance

Product (1) ──→ (Many) InvoiceItem
Product (1) ──→ (Many) Maintenance

Accessory (1) ──→ (Many) InvoiceItem
```

---

# 🔌 الراوتس الكاملة

## 1️⃣ Companies Routes

| الطريقة | الـ Endpoint | الوصف | المميزات |
|-------|-----------|-------|---------|
| GET | `/api/companies` | جميع الشركات | pagination, filtering |
| GET | `/api/companies/:id` | شركة محددة | مع العلاقات |
| POST | `/api/companies` | إنشاء شركة | validation |
| PUT | `/api/companies/:id` | تحديث بيانات | partial update |
| PUT | `/api/companies/:id/subscription` | تحديث الاشتراك | تاريخ الانتهاء |
| DELETE | `/api/companies/:id` | حذف شركة | cascade delete |

---

## 2️⃣ Users Routes

| الطريقة | الـ Endpoint | الوصف |
|-------|-----------|-------|
| GET | `/api/users` | جميع المستخدمين |
| GET | `/api/users/:id` | مستخدم محدد |
| GET | `/api/users/company/:companyId` | مستخدمي شركة |
| POST | `/api/users` | إنشاء مستخدم |
| PUT | `/api/users/:id` | تحديث المستخدم |
| PUT | `/api/users/:id/password` | تغيير كلمة المرور |
| DELETE | `/api/users/:id` | حذف مستخدم |

---

## 3️⃣ Customers Routes

| الطريقة | الـ Endpoint | الوصف |
|-------|-----------|-------|
| GET | `/api/customers` | جميع العملاء |
| GET | `/api/customers/:id` | عميل محدد |
| GET | `/api/customers/type/:type` | حسب النوع |
| GET | `/api/customers/count` | عداد العملاء |
| GET | `/api/customers/governorates` | قائمة المحافظات |
| GET | `/api/customers/cities/:governorate` | المدن |
| POST | `/api/customers` | إضافة عميل |
| PUT | `/api/customers/:id` | تحديث عميل |
| DELETE | `/api/customers/:id` | حذف عميل |

---

## 4️⃣ Employees Routes

| الطريقة | الـ Endpoint | الوصف |
|-------|-----------|-------|
| GET | `/api/employees` | جميع الموظفين |
| GET | `/api/employees/:id` | موظف محدد |
| GET | `/api/employees/role/:role` | حسب الدور |
| GET | `/api/employees/roles` | جميع الأدوار |
| POST | `/api/employees` | إضافة موظف |
| PUT | `/api/employees/:id` | تحديث موظف |
| DELETE | `/api/employees/:id` | حذف موظف |

---

## 5️⃣ Suppliers Routes

| الطريقة | الـ Endpoint | الوصف |
|-------|-----------|-------|
| GET | `/api/suppliers` | جميع الموردين |
| GET | `/api/suppliers/:id` | مورد محدد |
| POST | `/api/suppliers` | إضافة مورد |
| PUT | `/api/suppliers/:id` | تحديث مورد |
| DELETE | `/api/suppliers/:id` | حذف مورد |

---

## 6️⃣ Products Routes

| الطريقة | الـ Endpoint | الوصف |
|-------|-----------|-------|
| GET | `/api/products` | جميع المنتجات |
| GET |
