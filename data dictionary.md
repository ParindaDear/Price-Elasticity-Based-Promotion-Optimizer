# Data Dictionary

## sales_transaction

| Column         | Data Type | Example             | Description                   |
| -------------- | --------- | ------------------- | ----------------------------- |
| datetime       | datetime  | 22-5-2024  19:33:00 | วันที่และเวลาที่เกิดรายการขาย |
| product_id     | string    | P049            | รหัสสินค้า                    |
| price          | float     | 103.25              | ราคาขายต่อหน่วย               |
| qty            | integer   | 13                   | จำนวนสินค้าที่ขาย             |
| customer_id    | string    | C0179            | รหัสลูกค้า                    |
| promotion_id   | string    | PROMO002            | รหัส Promotion ที่ใช้ (ถ้ามี)   |
| store_id       | string    | S20            | รหัสสาขา                      |
| po_id   | string    | PO00001            | อ้างอิง purchase order   |

## product_master

| Column             | Data Type | Example  | Description          |
| ------------------ | --------- | -------- | -------------------- |
| product_id         | string    | P001 | รหัสสินค้า           |
| price              | float     | 140.47   | ราคาปกติของสินค้า    |
| product_taxonomies | string    | commodity | ประเภทสินค้า         |
| cogs               | float     | 48.18    | ต้นทุนสินค้าต่อหน่วย (mock เพิ่มขึ้นมา) |


## promotion_master

| Column         | Data Type | Example             | Description            |
| -------------- | --------- | ------------------- | ---------------------- |
| promotion_id   | string    | PROMO001            | รหัส Promotion          |
| discount       | float     | 0.5                | อัตราส่วนลด (ตีความเป็น % เช่น 50%) |
| product_id     | string  | P042,P008          | สินค้าที่ใช้ Promotion นี้   |
| start_date     | datetime  | 2024-05-18          | วันที่เริ่ม Promotion   |
| end_date       | datetime  | 2024-07-06          | วันที่สิ้นสุด Promotion |

## customer_master

| Column              | Data Type | Example  | Description  |
| ------------------- | --------- | -------- | ------------ |
| customer_id         | string    | C0001 | รหัสลูกค้า   |
| customer_taxonomies | string    | Loyal    | ประเภทลูกค้า |

## store_master

| Column           | Data Type | Example     | Description |
| ---------------- | --------- | ----------- | ----------- |
| store_id         | string    | S01    | รหัสสาขา    |
| store_taxonomies | string    | hypermarket | ประเภทสาขา  |

## Engineered Features

| Feature Name | Data Type | Example | Description |
| :--- | :--- | :--- | :--- |
| `revenue` | float | 388.50 | รายได้จากการขายของรายการนั้นๆ ก่อนหักส่วนลด (`price * qty`) |
| `cogs` | float | 52.24 | มูลค่าต้นทุนรวมของรายการขายนั้นๆ (`price * cogs_ratio`) |
| `effective_price` | float | 110.08 | ราคาขายสุทธิต่อชิ้นหลังหักส่วนลดเรียบร้อยแล้ว |
| `discount` | float | 0.17 | อัตราส่วนลดที่ได้รับจาก Promotion (ค่าทศนิยมอยู่ระหว่าง 0.00 ถึง 1.00) |
| `gross_profit` | float | 163.50 | กำไรขั้นต้นรวมจากรายการขายนั้นๆ |
| `profit_margin_pct` | float | 42.1 | อัตรากำไรคิดเป็น Percent เทียบกับรายได้สุทธิ |
| `has_promotion` | boolean | False | สถานะการร่วม Promotion ใน transaction (`True` = มีโปร, `False` = ไม่มีโปร) |
| `cogs_ratio` | float | 0.58 | อัตราส่วนต้นทุนต่อราคากลางสินค้า ($\frac{\text{COGS}}{\text{Price}}$) |
| `month` | integer | 5 | ตัวเลขระบุเดือนที่เกิดรายการขาย (ค่าอยู่ระหว่าง 1 - 12) |
| `dayofweek` | string | Wednesday | ชื่อวันในสัปดาห์ที่เกิดรายการขาย |
| `elasticity` | float | -1.35 | ค่าความยืดหยุ่นของอุปสงค์ต่อราคา (Price Elasticity of Demand) จากโมเดล |
| `elasticity_type` | string | Elastic | ประเภทความไวต่อราคาของสินค้า แยกเป็น Elastic หรือ Inelastic |
| `recommended_discount_pct` | float | 0.20 | อัตราส่วนลดที่เหมาะสมที่สุดในการสร้างกำไรสูงสุดที่ Model แนะนำ |
| `simulated_qty` | float | 14.25 | ปริมาณยอดขายคาดการณ์ภายใต้กลยุทธ์ส่วนลดใหม่จาก Model |
| `simulated_revenue` | float | 528.40 | รายได้รวมคาดการณ์ภายใต้กลยุทธ์ส่วนลดใหม่จาก Model |
| `simulated_profit` | float | 221.60 | กำไรสุทธิคาดการณ์ภายใต้กลยุทธ์ส่วนลดใหม่จาก Model |
| `estimated_discount_cost` | float | 45.20 | มูลค่าเม็ดเงินส่วนลดรวมที่ต้องจ่ายไปในกลยุทธ์จำลองใหม่ |
| `expected_revenue_change_pct`| float | 12.4 | Percent การเปลี่ยนแปลงของรายได้คาดการณ์เมื่อเทียบกับยอดขายเดิม |
| `expected_profit_change_pct` | float | 18.7 | Percent การเปลี่ยนแปลงของกำไรคาดการณ์เมื่อเทียบกับยอดกำไรเดิม |
| `action` | string | Increase Discount | คำแนะนำในการปรับเปลี่ยนกลยุทธ์ราคา (Reduce Discount / Keep Current / Increase Discount) |
