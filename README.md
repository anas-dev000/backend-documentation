# 📘 API Routes Documentation

---

## 🏢 **1. Companies Routes**

### **GET Routes**

```http
GET /api/companies
```

**Response:**

```json
{
  "data": [
    {
      "CompanyID": 1,
      "Name": "شركة الندى",
      "Logo": "url",
      "Address": "القاهرة، مصر الجديدة",
      "Email": "info@alnada.com",
      "Phone": "0224567890",
      "SubscriptionExpiryDate": "2025-01-15T10:00:00Z",
      "CreatedAt": "2025-01-15T10:00:00Z"
    }
  ]
}
```

---

### **PUT Routes**

```http
PUT /api/companies/{CompanyID}/expiry
```

**Body (FormData):**

```
SubscriptionExpiryDate: "2025-12-31T10:00:00Z"
```

**Response:**

```json
{
  "success": true,
  "updated": {
    /* Company Object */
  }
}
```

---

## 👥 **2. Users Routes**

### **GET Routes**

```http
GET /api/users
```

**Response:**

```json
{
  "data": [
    {
      "UserID": 1,
      "CompanyID": 1,
      "FullName": "أنس علي",
      "Email": "developer@alnada.com",
      "Role": "developer",
      "Status": "Active",
      "CreatedAt": "2023-01-15T10:00:00Z"
    }
  ]
}
```

---

### **POST Routes**

```http
POST /api/users
```

**Body:**

```json
{
  "CompanyID": 1,
  "FullName": "محمد أحمد",
  "Email": "user@example.com",
  "PasswordHash": "hashed_password",
  "Role": "employee",
  "Status": "Active"
}
```

---

### **PUT Routes**

```http
PUT /api/users/{UserID}
```

**Body:** Same as POST

---

### **DELETE Routes**

```http
DELETE /api/users/{UserID}
```

---

## 🧑‍💼 **3. Customers Routes**

### **GET Routes**

```http
GET /api/customers
GET /api/customers/count
GET /api/customerTypes
GET /api/governorates
GET /api/cities
```

**Response Examples:**

```json
// GET /api/customers
{
  "data": [
    {
      "CustomerID": 1,
      "FullName": "محمد علي عبدالله",
      "CustomerType": "Installation",
      "NationalID": "12345678901234",
      "IDCardImage": "url",
      "PrimaryNumber": "01234567890",
      "SecondaryNumber": "01098765432",
      "Address": {
        "Governorate": "القاهرة",
        "City": "مدينة نصر",
        "District": "الحي الأول"
      },
      "CompanyID": 1,
      "CreatedAt": "2024-01-10T10:00:00Z"
    }
  ]
}

// GET /api/customers/count
{ "data": 6 }

// GET /api/customerTypes
{ "data": ["Installation", "Maintenance"] }

// GET /api/governorates
{ "data": ["القاهرة", "الجيزة", "الإسكندرية"] }

// GET /api/cities
{ "data": ["مدينة نصر", "الدقي", "سموحة", ...] }
```

---

### **POST Routes**

```http
POST /api/customers
```

**Body (FormData):**

```
FullName: "محمد علي"
CustomerType: "Installation"
NationalID: "12345678901234"
IDCardImage: File
PrimaryNumber: "01234567890"
SecondaryNumber: "01098765432"
Address: '{"Governorate":"القاهرة","City":"مدينة نصر","District":"الحي الأول"}'
CompanyID: 1
```

---

### **PUT Routes**

```http
PUT /api/customers/{CustomerID}
```

**Body:** Same as POST

---

### **DELETE Routes**

```http
DELETE /api/customers/{CustomerID}
```

---

## 👷 **4. Employees Routes**

### **GET Routes**

```http
GET /api/employees
GET /api/roles
```

**Response:**

```json
// GET /api/employees
{
  "data": [
    {
      "EmployeeID": 1,
      "FullName": "محمود حسن",
      "NationalID": "22334455667788",
      "IDCardImage": "url",
      "Role": "SalesRep",
      "PrimaryNumber": "01087654321",
      "SecondaryNumber": "01233445566",
      "Address": {
        "Governorate": "القاهرة",
        "City": "الشروق",
        "District": "المنطقة الأولى"
      },
      "CompanyID": 1,
      "IsEmployed": true
    }
  ]
}

// GET /api/roles
{ "data": ["SalesRep", "Technician"] }
```

---

### **POST/PUT/DELETE Routes**

Same pattern as Customers with FormData support

---

## 🏭 **5. Suppliers Routes**

### **GET Routes**

```http
GET /api/suppliers
```

**Response:**

```json
{
  "data": [
    {
      "SupplierID": 1,
      "Name": "شركة فلاتر مصر",
      "ContactInfo": "01234567890 - filters@egypt.com",
      "CompanyID": 1
    }
  ]
}
```

---

### **POST Routes**

```http
POST /api/suppliers
```

**Body:**

```json
{
  "Name": "شركة جديدة",
  "ContactInfo": "0123456789",
  "CompanyID": 1
}
```

---

### **PUT/DELETE Routes**

```http
PUT /api/suppliers/{SupplierID}
DELETE /api/suppliers/{SupplierID}
```

---

## 📦 **6. Products Routes**

### **GET Routes**

```http
GET /api/products
GET /api/products/low-stock-count
GET /api/categories
```

**Response:**

```json
// GET /api/products
{
  "data": [
    {
      "ProductID": 1,
      "Name": "فلتر مياه 7 مراحل",
      "Category": "فلاتر المياه",
      "Price": 1500,
      "Stock": 25,
      "SupplierID": 1,
      "CompanyID": 1
    }
  ]
}

// GET /api/products/low-stock-count
{ "data": 1 }  // عدد المنتجات التي Stock < 10

// GET /api/categories
{ "data": ["فلاتر المياه", "تكييفات"] }
```

---

### **POST/PUT/DELETE Routes**

Standard CRUD operations

---

## 🔧 **7. Accessories Routes**

### **GET Routes**

```http
GET /api/accessories
```

**Response:**

```json
{
  "data": [
    {
      "AccessoryID": 1,
      "Name": "شمعة فلتر",
      "Price": 50,
      "Stock": 100,
      "SupplierID": 1,
      "CompanyID": 1
    }
  ]
}
```

---

### **POST/PUT/DELETE Routes**

Standard CRUD operations

---

## 🔗 **8. Product-Accessories Routes**

### **GET Routes**

```http
GET /api/productAccessories
```

**Response:**

```json
{
  "data": [
    {
      "ProductID": 1,
      "AccessoryID": 1
    }
  ]
}
```

---

### **POST Routes**

```http
POST /api/productAccessories
```

**Body:**

```json
{
  "ProductID": 1,
  "AccessoryID": 2
}
```

**Note:** يرفض التكرار (Duplicate detection)

---

### **DELETE Routes**

```http
DELETE /api/productAccessories/product/{ProductID}
```

يحذف **جميع** الـ Accessories المرتبطة بالمنتج

---

## 🛠️ **9. Services Routes**

### **GET Routes**

```http
GET /api/services
```

**Response:**

```json
{
  "data": [
    {
      "ServiceID": 1,
      "Name": "صيانة فلتر",
      "Description": "صيانة دورية للفلتر وتغيير الشمعات",
      "Price": 100,
      "CompanyID": 1
    }
  ]
}
```

---

### **POST/PUT/DELETE Routes**

Standard CRUD operations

---

## 🔧 **10. Maintenances Routes**

### **GET Routes**

```http
GET /api/maintenances
GET /api/maintenances/upcoming-count
GET /api/maintenances/upcoming-list
```

**Response:**

```json
// GET /api/maintenances
{
  "data": [
    {
      "MaintenanceID": 1,
      "CustomerID": 1,
      "ServiceID": 1,
      "ProductID": 1,
      "MaintenanceDate": "2025-11-04T10:00:00Z",
      "Notes": "صيانة دورية",
      "Price": 100,
      "CompanyID": 1,
      "Status": "Pending",
      "TechnicianID": 2
    }
  ]
}

// GET /api/maintenances/upcoming-count
{ "data": 2 }  // الصيانات المعلقة (Pending)

// GET /api/maintenances/upcoming-list
{
  "data": [/* أول 5 صيانات Pending خلال 7 أيام */]
}
```

---

### **POST/PUT/DELETE Routes**

Standard CRUD operations

---

## 🧾 **11. Invoices Routes**

### **GET Routes**

```http
GET /api/invoices
GET /api/invoices/recent
GET /api/invoices/monthly-revenue
```

**Response:**

```json
// GET /api/invoices
{
  "data": [
    {
      "InvoiceID": 1,
      "TechnicianID": 2,
      "SalesRepID": 1,
      "CustomerID": 1,
      "TotalAmount": 1800,
      "MaintenancePeriod": 1,
      "PaidAtInstallation": 0,
      "PaidAtContract": 1800,
      "InstallationDate": "2024-01-15T10:00:00Z",
      "ContractDate": "2024-01-10T10:00:00Z",
      "SaleType": "Cash",
      "CompanyID": 1,
      "DiscountAmount": 0,
      "InstallationCostType": "Percentage",
      "InstallationCostValue": 0,
      "ContractNotes": ""
    }
  ]
}

// GET /api/invoices/recent
{ "data": [/* آخر 5 فواتير مرتبة بالتاريخ */] }

// GET /api/invoices/monthly-revenue
{ "data": 11600 }  // مجموع TotalAmount لجميع الفواتير
```

---

### **POST/PUT/DELETE Routes**

Standard CRUD operations

---

## 📋 **12. Invoice Items Routes**

### **GET Routes**

```http
GET /api/invoiceItems
```

**Response:**

```json
{
  "data": [
    {
      "InvoiceItemID": 1,
      "InvoiceID": 1,
      "ProductID": 1,
      "AccessoryID": null,
      "ServiceID": null,
      "Quantity": 1,
      "UnitPrice": 0,
      "Subtotal": 0,
      "CompanyID": 1
    }
  ]
}
```

---

### **POST Routes**

```http
POST /api/invoiceItems
```

**Body:**

```json
{
  "InvoiceID": 1,
  "ProductID": 2,
  "AccessoryID": null,
  "ServiceID": null,
  "Quantity": 3,
  "UnitPrice": 150,
  "CompanyID": 1
}
```

**Auto-calculates:** `Subtotal = Quantity × UnitPrice`

---

### **PUT Routes**

```http
PUT /api/invoiceItems/{InvoiceItemID}
```

---

### **DELETE Routes**

```http
DELETE /api/invoiceItems/{InvoiceItemID}
```

---

## 💳 **13. Installments Routes**

### **GET Routes**

```http
GET /api/installments
GET /api/installments/pending-count
```

**Response:**

```json
// GET /api/installments
{
  "data": [
    {
      "InstallmentID": 1,
      "InvoiceID": 2,
      "NumberOfMonths": 12,
      "CollectionStartDate": "2025-02-01T00:00:00Z",
      "CollectionEndDate": "2026-01-31T00:00:00Z",
      "MonthlyInstallment": 291.67
    }
  ]
}

// GET /api/installments/pending-count
{ "data": 18 }  // عدد الدفعات المعلقة (Pending)
```

---

### **POST/PUT/DELETE Routes**

Standard CRUD operations

---

## 💰 **14. Installment Payments Routes**

### **GET Routes**

```http
GET /api/installmentPayments
GET /api/payments/overdue-count
```

**Response:**

```json
// GET /api/installmentPayments
{
  "data": [
    {
      "PaymentID": 1,
      "InstallmentID": 1,
      "CustomerID": 1,
      "Status": "Paid",
      "OverdueAmount": 0,
      "AmountPaid": 291.67,
      "AmountDue": 291.67,
      "CarryoverAmount": 0,
      "Notes": "دفع الشهر الأول",
      "PaymentDate": "2025-02-10T10:00:00Z",
      "DueDate": "2025-02-01T00:00:00Z"
    }
  ]
}

// GET /api/payments/overdue-count
{ "data": 0 }  // الدفعات المتأخرة (Overdue أو Pending بعد DueDate)
```

---

### **POST Routes**

```http
POST /api/installmentPayments
```

**Body:**

```json
{
  "InstallmentID": 1,
  "CustomerID": 1,
  "AmountDue": 291.67,
  "AmountPaid": 291.67,
  "DueDate": "2025-02-01T00:00:00Z",
  "PaymentDate": "2025-02-10T10:00:00Z",
  "Notes": "دفع كامل"
}
```

**Auto-calculates:**

- `Status`: `"Paid"` | `"Partial"` | `"Pending"`
- `OverdueAmount`: `AmountDue - AmountPaid` (if partial)
- `CarryoverAmount`: المبلغ المتبقي يُضاف للدفعة القادمة

**Logic:**

- إذا `AmountPaid === AmountDue` → Status = `"Paid"`
- إذا `0 < AmountPaid < AmountDue` → Status = `"Partial"` + CarryoverAmount يُضاف للدفعة القادمة
- إذا `AmountPaid === 0` → Status = `"Pending"`

---

### **PUT Routes**

```http
PUT /api/installmentPayments/{PaymentID}
```

**Body:** Same as POST

**Special Logic:**

- عند التعديل، يحذف الـ CarryoverAmount القديم من الدفعة التالية
- ثم يضيف الـ CarryoverAmount الجديد للدفعة التالية

---

### **DELETE Routes**

```http
DELETE /api/installmentPayments/{PaymentID}
```

---

## 📊 **Dashboard Aggregation Routes**

| Route                                  | Description                              | Response Type |
| -------------------------------------- | ---------------------------------------- | ------------- |
| `GET /api/customers/count`             | عدد العملاء                              | `number`      |
| `GET /api/installments/pending-count`  | عدد الدفعات المعلقة                      | `number`      |
| `GET /api/maintenances/upcoming-count` | عدد الصيانات القادمة                     | `number`      |
| `GET /api/products/low-stock-count`    | عدد المنتجات قليلة المخزون (< 10)        | `number`      |
| `GET /api/invoices/monthly-revenue`    | إجمالي إيرادات الفواتير                  | `number`      |
| `GET /api/payments/overdue-count`      | عدد الدفعات المتأخرة                     | `number`      |
| `GET /api/invoices/recent`             | آخر 5 فواتير                             | `array`       |
| `GET /api/maintenances/upcoming-list`  | أول 5 صيانات قادمة (Pending خلال 7 أيام) | `array`       |

---

## 🔐 **Notes & Best Practices**

### **1. FormData Handling**

Routes التي تستخدم `FormData`:

- `/api/customers` (POST/PUT) - لرفع `IDCardImage`
- `/api/employees` (POST/PUT) - لرفع `IDCardImage`
- `/api/companies/{id}/expiry` (PUT)

**Example:**

```javascript
const formData = new FormData();
formData.append('FullName', 'محمد');
formData.append('IDCardImage', file);
formData.append('Address', JSON.stringify({...}));
```

---

### **2. Auto-calculated Fields**

| Entity                | Auto-calculated Fields                                    |
| --------------------- | --------------------------------------------------------- |
| `InvoiceItems`        | `InvoiceItemID`, `Subtotal`                               |
| `InstallmentPayments` | `PaymentID`, `Status`, `OverdueAmount`, `CarryoverAmount` |
| All Entities          | `{Entity}ID` (auto-increment)                             |

---

### **3. Special Delete Behavior**

```http
DELETE /api/productAccessories/product/{ProductID}
```

يحذف **جميع** الـ Accessories المرتبطة بالمنتج (Cascade Delete)

---

### **4. Error Responses**

```json
{
  "error": "Item not found: 123"
}
```

```json
{
  "error": "Duplicate"
}
```

```json
{
  "error": "Unknown endpoint: /api/xyz"
}
```

---

## 🎯 **Summary Table**

| Entity              | GET | POST | PUT         | DELETE       | Special Routes                                         |
| ------------------- | --- | ---- | ----------- | ------------ | ------------------------------------------------------ |
| Companies           | ✅  | ❌   | ✅ (expiry) | ❌           | `/expiry`                                              |
| Users               | ✅  | ✅   | ✅          | ✅           | -                                                      |
| Customers           | ✅  | ✅   | ✅          | ✅           | `/count`, `/customerTypes`, `/governorates`, `/cities` |
| Employees           | ✅  | ✅   | ✅          | ✅           | `/roles`                                               |
| Suppliers           | ✅  | ✅   | ✅          | ✅           | -                                                      |
| Products            | ✅  | ✅   | ✅          | ✅           | `/low-stock-count`, `/categories`                      |
| Accessories         | ✅  | ✅   | ✅          | ✅           | -                                                      |
| ProductAccessories  | ✅  | ✅   | ❌          | ✅ (cascade) | -                                                      |
| Services            | ✅  | ✅   | ✅          | ✅           | -                                                      |
| Maintenances        | ✅  | ✅   | ✅          | ✅           | `/upcoming-count`, `/upcoming-list`                    |
| Invoices            | ✅  | ✅   | ✅          | ✅           | `/recent`, `/monthly-revenue`                          |
| InvoiceItems        | ✅  | ✅   | ✅          | ✅           | -                                                      |
| Installments        | ✅  | ✅   | ✅          | ✅           | `/pending-count`                                       |
| InstallmentPayments | ✅  | ✅   | ✅          | ✅           | `/overdue-count`                                       |

---

**Total Routes:** **~60 API endpoints**
