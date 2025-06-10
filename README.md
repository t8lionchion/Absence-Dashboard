## 出缺席圖表（Ted）

本專案說明如何透過 **PHPMyAdmin 匯出 JSON**、搭配 **Chart.js + async/await** 製作三種類型圖表，以動態呈現學生 Ted 的出缺席情況。

---

### 📊 1-7. 出席狀況圓餅圖

**📌 對應 SQL 語法：**

```sql
SELECT
  SUM(class_hours) AS `總課程時數`,
  SUM(attended_hours) AS `實際上課時數`,
  SUM(absent_hours) AS `缺席時數`,
  SUM(late_hours) AS `遲到時間`,
  SUM(leave_early_hours) AS `早退時數`,
  SUM(attended_hours) / SUM(class_hours) AS `出勤比率`
FROM attendance_log
WHERE name = 'Ted';
```
建成view:
```SQL=
CREATE view attendance_log_view 
as select sum(class_hours) as `總課程時數`,
sum(attended_hours) as `實際上課時數`,
sum(absent_hours) as `缺席時數`,
sum(late_hours) as `遲到時間`,
sum(leave_early_hours) as `早退時數`,
sum(attended_hours)/sum(class_hours) as `出勤比率`
from attendance_log 
where name='Ted';
```

**📁 匯出 JSON 檔名：** `attendance_log.json`

**📈 圖表類型：** Pie / Doughnut Chart

---

### 📈 8. 每日上課時數折線圖

**📌 對應 SQL 語法：**

```sql
SELECT class_date, attended_hours
FROM attendance_log
WHERE name = 'Ted';
```
建成view:
```SQL=
CREATE view attendance_log2_view 
as select class_date,attended_hours from attendance_log
where name='Ted';
```

**📁 匯出 JSON 檔名建議：** `attendance_log2.json`

**📈 圖表類型：** Line Chart（折線圖）

---

### 📊 9. 每日在校時數長條圖（或散點圖）

**📌 對應 SQL 語法：**

```sql
SELECT class_date, raw_hours
FROM attendance_log
WHERE name = 'Ted';
```
建成view:
```SQL=
CREATE view attendclasstime as select class_date,raw_hours from attendance_log
where name='Ted';
```

**📁 匯出 JSON 檔名建議：** `attendclasstime.json`

**📈 圖表類型：** Bar Chart 

---

### 🧩 技術總結：

* 使用 `fetch()` + `async/await` 動態載入 JSON 資料。
* 使用 `Chart.js` 建立不同類型圖表。
* 將每個圖表放入 `<canvas>` 元素中，如：

```html
<canvas id="ex07"></canvas>
<canvas id="ex08"></canvas>
<canvas id="ex09"></canvas>
```

* 搭配 `Bootstrap` 佈局美化畫面。

---

### 📦 專案結構：

```bash
attendance-project/
├── 6張card對應總課程時數、實際上課時數、缺席時數、遲到時數、早退時數、出勤比率
├── attendance_log.json      #出缺席 (圓餅圖)
├── attendance_log2.json     #每日出席時數（折線圖）
├── attendclasstime.json     #每日在校時數（長條圖 ）
└── chart-attendance.html    #圖表渲染主頁

```



