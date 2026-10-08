
# 🗄️ Lesson 2 — Database Design with PostgreSQL & Prisma 8

في هذا الدرس انتقلنا من إعداد المشروع إلى بناء قاعدة البيانات الخاصة بمنصة السفر.

## 🛠️ التقنيات المستخدمة

- PostgreSQL
- Prisma 8
- NestJS
- TypeScript

## 🏗️ تصميم قاعدة البيانات

قاعدة البيانات تحتوي حاليًا على ثلاث كيانات رئيسية:

```text
Country
   │
   │ 1:N
   ▼
City
   │
   │ 1:N
   ▼
Destination
````

### Country

يمثل الدولة.

الحقول:

* id
* name
* code
* createdAt

`code` فريد لكل دولة.

### City

يمثل المدينة التابعة لدولة.

الحقول:

* id
* name
* countryId
* createdAt

كل مدينة مرتبطة بدولة واحدة.

### Destination

يمثل الوجهة السياحية الموجودة داخل مدينة.

الحقول:

* id
* name
* description
* imageUrl
* cityId
* createdAt

كل وجهة مرتبطة بمدينة واحدة.

## 🔗 العلاقات

العلاقات في قاعدة البيانات:

```text
Country 1 ──── N City
City    1 ──── N Destination
```

وعند حذف دولة يتم حذف المدن المرتبطة بها، وعند حذف مدينة يتم حذف الوجهات المرتبطة بها باستخدام:

```text
onDelete: Cascade
```

## ⚡ الفهارس

تم إنشاء Index على:

```text
City.countryId
Destination.cityId
```

وذلك لتحسين عمليات البحث عن المدن التابعة لدولة والوجهات التابعة لمدينة.

## 📁 Prisma

تم إنشاء:

```text
backend/prisma/
├── contract.prisma
├── contract.json
└── contract.d.ts
```

ملف `contract.prisma` يحتوي على تصميم قاعدة البيانات.

## ⚙️ Prisma Configuration

تم إنشاء:

```text
backend/prisma.config.ts
```

ويستخدم اتصال PostgreSQL الموجود في:

```text
DATABASE_URL
```

داخل ملف `.env`.

## 🗄️ قاعدة البيانات

اسم قاعدة البيانات:

```text
travel_db
```

## 🚀 تطبيق الـ Contract

في Prisma 8 استخدمنا Contract workflow.

أولًا قمنا بإنشاء الـ contract:

```bash
npx prisma contract emit
```

ثم تم تطبيق الـ contract على قاعدة البيانات:

```bash
npx prisma db update
```

نتيجة العملية:

```text
✔ Applied 8 operation(s) across 1 contract space
```

وتم إنشاء:

```text
Country
City
Destination
```

مع العلاقات والـ indexes والـ constraints.

## 🔐 التحقق من قاعدة البيانات

بعد تحديث قاعدة البيانات قمنا بالتحقق باستخدام:

```bash
npx prisma db sign
```

والنتيجة:

```text
✔ Database signed
```

وهذا يعني أن قاعدة البيانات الحالية مطابقة للـ Prisma Contract.

## 🔎 التحقق من حالة الـ migrations

تم تنفيذ:

```bash
npx prisma migration status
```

وكانت النتيجة:

```text
(no migrations)
```

في هذا المشروع نستخدم Prisma 8 Contract workflow، لذلك لا نستخدم أوامر Prisma القديمة مثل:

```bash
npx prisma migrate dev
```

## 📂 هيكل المشروع بعد الدرس

```text
travel-platform/
│
├── frontend/
│
├── backend/
│   ├── migrations/
│   ├── prisma/
│   │   ├── contract.prisma
│   │   ├── contract.json
│   │   └── contract.d.ts
│   │
│   ├── prisma.config.ts
│   ├── src/
│   ├── package.json
│   └── .env
│
├── docs/
│   └── lesson-02.md
│
└── .gitignore
```

> ملاحظة: ملف `.env` يحتوي على معلومات الاتصال بقاعدة البيانات ولا يجب رفعه إلى GitHub.

## 🎯 ماذا تعلمنا؟

في هذا الدرس تعلمنا:

1. إنشاء قاعدة PostgreSQL.
2. ربط Prisma 8 مع PostgreSQL.
3. إنشاء Prisma Contract.
4. تصميم Country / City / Destination.
5. إنشاء العلاقات بين الجداول.
6. إضافة Unique Constraint.
7. إضافة Database Indexes.
8. تطبيق الـ Contract على PostgreSQL.
9. التحقق من تطابق قاعدة البيانات مع الـ Contract.

## 🔜 الدرس القادم

في الدرس القادم سنبدأ بناء الـ REST API باستخدام NestJS.

سننشئ:

```text
POST   /countries
GET    /countries

POST   /cities
GET    /cities

POST   /destinations
GET    /destinations
GET    /destinations/:id
```

وسنبدأ بتطبيق:

```text
Controller
     ↓
Service
     ↓
Prisma
     ↓
PostgreSQL
```

````

