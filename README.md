# 🛍️ Lazada Data Scraper (AJAX-Based)

สำหรับดึงข้อมูลสินค้า (Products), รายละเอียดเชิงลึก (Product Descriptions) และรีวิว (Reviews) จาก Lazada Thailand โดยใช้เทคนิค **AJAX Scraping** เพื่อความเร็วและความถูกต้องของข้อมูลสูงสุด เหมาะสำหรับงานด้าน Data Science และ Market Intelligence

## ✨ Features
*   **Speed & Efficiency:** ดึงข้อมูลผ่าน AJAX API โดยตรง ไม่ต้องรอ Render หน้าเว็บทั้งหมด ลดภาระการใช้งาน CPU และ RAM
*   **Persistent Session:** ระบบเก็บ Session (Cookies/Login) ผ่าน Persistent Context ทำให้ไม่ต้อง Login ใหม่ทุกครั้งที่รัน
*   **Manual Captcha Handling:** ระบบหยุดรอ (Pause) อัจฉริยะเมื่อตรวจพบ Captcha หรือระบบ Verification เพื่อให้ผู้ใช้จัดการด้วยตัวเองก่อนทำงานต่อ
*   **Robust Data Cleaning:** 
    *   จัดการข้อมูลตัวเลข (Sold Count, Price, Discount) ให้อยู่ในรูปแบบที่พร้อมนำไปวิเคราะห์
    *   ระบบแปลงวันที่ภาษาไทย (ม.ค. - ธ.ค.) และเวลาสัมพัทธ์ (เช่น "2 วันที่แล้ว") ให้เป็นรูปแบบสากล (YYYY-MM-DD)
*   **Lazy Load Handling:** ระบบจำลองการ Scroll อัตโนมัติเพื่อ Trigger ข้อมูลที่ซ่อนอยู่ (Specifications, Qualification, Descriptions)
*   **Fallback Image Retrieval:** ดึง URL รูปภาพจากหลาย Attributes (`src`, `data-src`, `data-lazy`) เพื่อให้มั่นใจว่าจะได้รูปภาพจริงที่คมชัดที่สุด
*   **Auto Export:** บันทึกข้อมูลเป็นไฟล์ `.xlsx` (Excel) แยกตามประเภทงานโดยอัตโนมัติ พร้อมระบบป้องกันข้อมูลซ้ำ (Deduplication)

---

## 🛠️ Prerequisites

โปรเจกต์นี้ทำงานบนระบบ **Python 3.12+** และ **Windows** (แนะนำให้รันบน Jupyter Notebook)

### Required Libraries:
```bash
pip install playwright pandas openpyxl nest_asyncio
playwright install chromium
