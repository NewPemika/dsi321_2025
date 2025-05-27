# ⚡ ระบบดึงข้อมูลการผลิตไฟฟ้าเรียลไทม์จาก กฟผ. (EGAT Real-time Power Data Pipeline)

โปรเจกต์นี้มีวัตถุประสงค์เพื่อดึงข้อมูลการผลิตไฟฟ้าแบบเรียลไทม์จากเว็บไซต์ของการไฟฟ้าฝ่ายผลิตแห่งประเทศไทย (EGAT) และจัดเก็บข้อมูลเป็นไฟล์เวอร์ชันผ่านระบบ LakeFS โดยใช้ Prefect ในการควบคุม Workflow และทำงานแบบอัตโนมัติผ่าน Docker Compose

---

## 🚀 คุณสมบัติเด่น

- 🔄 **ดึงข้อมูลเรียลไทม์:** ใช้ Selenium เปิดหน้าเว็บไซต์และดึงข้อมูล MW และอุณหภูมิจาก Console log
- 🤖 **ควบคุมด้วย Prefect:** ใช้ Prefect ในการจัดการ Task และ Flow ของ pipeline
- 🐳 **ทำงานแบบ container:** รองรับการรันผ่าน Docker Compose เพื่อความสะดวกในการใช้งานและ deploy
- 📦 **จัดเก็บข้อมูลใน LakeFS:** บันทึกข้อมูลเป็นไฟล์ Parquet ลงใน S3 bucket ที่ใช้ LakeFS เพื่อการควบคุมเวอร์ชันของข้อมูล
- 💾 **รองรับการ Commit อัตโนมัติ:** มีการ commit ข้อมูลใหม่เข้า LakeFS อัตโนมัติในทุกครั้งที่มีข้อมูลใหม่

---

## 🗂️ โครงสร้างโปรเจกต์

```
egat-scraper/
├── egat_pipeline.py             # ไฟล์หลักของ Prefect flow และ task
├── egat_realtime_power.csv      # ตัวอย่างข้อมูลที่ดึงได้
├── docker-compose.yml           # ไฟล์สำหรับรัน container ต่างๆ
├── prefect.yaml                 # การตั้งค่า deployment ของ Prefect
└── README.md                    # คู่มือการใช้งาน
```

---

## 🧱 ความต้องการเบื้องต้น

- Python 3.7 ขึ้นไป
- ติดตั้ง Google Chrome
- Docker และ Docker Compose
- มี Prefect และ LakeFS ติดตั้งพร้อมใช้งาน

---

## ⚙️ การใช้งาน

### 1. ติดตั้งไลบรารีที่จำเป็น

```bash
pip install -r requirements.txt
```

หรือหากไม่มี `requirements.txt` ให้ติดตั้งดังนี้:

```bash
pip install pandas selenium webdriver-manager prefect lakefs-client pyarrow
```

### 2. กำหนด Environment Variables (ผ่าน `.env` หรือ export)

```env
LAKEFS_ACCESS_KEY_ID=your_access_key
LAKEFS_SECRET_ACCESS_KEY=your_secret_key
LAKEFS_ENDPOINT_URL=http://localhost:8001/
```

### 3. เรียกใช้งาน Prefect flow (ใน local)

```bash
python egat_pipeline.py
```

---

## 🐳 การใช้งานร่วมกับ Docker Compose

```bash
docker-compose up --build
```

> ตรวจสอบให้แน่ใจว่า Prefect agent และ LakeFS container ทำงานก่อน

---

## 📈 ข้อมูลที่บันทึก

- `display_date_id`: วันที่แสดงในระบบ
- `display_time`: เวลาที่แสดง
- `current_value_MW`: กำลังการผลิต (เมกะวัตต์)
- `temperature_C`: อุณหภูมิ
- `scrape_timestamp_utc`: เวลาที่ดึงข้อมูลใน UTC

---

