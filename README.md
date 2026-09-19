# Coastal Public Space Perceived Safety Survey — Pilot App

แอปต้นแบบสำหรับงานวิจัยเชิงทดลองด้วยภาพ (Image-based Experimental Survey)

## สิ่งที่แอปทำได้

- ขอความยินยอมก่อนเริ่มแบบสอบถาม
- สุ่มผู้ตอบเข้าสู่เงื่อนไข A / B / C แบบค่อนข้างสมดุล
- ผู้ตอบเห็นเพียง 1 ภาพ และไม่เห็นชื่อเงื่อนไข
- ล็อกเงื่อนไขเดิมเมื่อ refresh หน้าใน session เดียวกัน
- บังคับให้ดูภาพอย่างน้อย 12 วินาทีก่อนตอบ
- แบบสอบถามครบชุด: Perceived Safety, Social Trust, Social Comfort/Anxiety, Behavioral Intention, Familiarity/Contact, Manipulation Check, Confound Check, ข้อมูลทั่วไป และคำถาม pilot
- บันทึกคำตอบลง Google Sheets อัตโนมัติ
- ป้องกันการส่งซ้ำจาก participant_id เดิม

## ไฟล์ในชุดนี้

- `Code.gs` — ฝั่งเซิร์ฟเวอร์ Google Apps Script และการบันทึก Google Sheets
- `Index.html` — หน้าตาและตรรกะของแบบสอบถาม
- `Images.html` — ภาพทดลอง A/B/C ฝังเป็น base64 เพื่อให้ deploy ได้ทันที
- `assets/` — ภาพ JPG แยก 3 เงื่อนไขสำหรับตรวจดู/แก้ไขภายหลัง

> ภาพในชุดนี้เป็น “ภาพตัวอย่าง” ที่ครอปจากภาพต้นแบบเพื่อใช้ทดสอบระบบเท่านั้น ก่อนเก็บข้อมูลวิจัยจริงควรสร้าง stimulus ที่ควบคุมจำนวนคน ตำแหน่ง ท่าทาง แสง และกิจกรรมให้เทียบเท่ากัน และทำ stimulus validation ก่อน

## วิธีสร้างลิงก์แบบสอบถามด้วย Google Apps Script

### 1) สร้าง Google Sheet

สร้าง Google Sheet ใหม่ เช่น `Pilot_Coastal_Safety_Survey`

### 2) เปิด Apps Script

ใน Google Sheet เลือก **Extensions → Apps Script**

### 3) วางโค้ด

- เปิดไฟล์ `Code.gs` เดิมใน Apps Script แล้วแทนที่ด้วยโค้ดจาก `Code.gs`
- สร้างไฟล์ HTML ชื่อ `Index` แล้ววางโค้ดจาก `Index.html`
- สร้างไฟล์ HTML ชื่อ `Images` แล้ววางโค้ดจาก `Images.html`

ชื่อไฟล์ต้องตรงตามนี้ เพราะ `Index.html` เรียก `Images.html` ผ่าน `include('Images')`

### 4) ตั้งค่าครั้งแรก

ใน Apps Script เลือกฟังก์ชัน `setup` แล้วกด **Run** 1 ครั้ง

ระบบจะขอสิทธิ์เข้าถึง Google Sheet ให้กดยืนยัน จากนั้นระบบจะสร้างแท็บ:

- `Responses` — เก็บคำตอบ
- `_Assignments` — เก็บการสุ่ม condition และซ่อนไว้

### 5) Deploy เป็น Web App

เลือก **Deploy → New deployment → Web app**

ค่าที่แนะนำ:

- Execute as: **Me**
- Who has access: **Anyone** หรือระดับการเข้าถึงที่สถาบันอนุญาต

กด Deploy แล้วคัดลอก **Web app URL** นี่คือลิงก์ที่ส่งให้ผู้ตอบ

> บัญชี Google Workspace บางองค์กรอาจไม่อนุญาต “Anyone”. หากติดข้อจำกัด ต้องใช้การตั้งค่าที่หน่วยงานอนุญาต หรือ deploy หน้าเว็บบนบริการอื่นแล้วเชื่อม backend ภายหลัง

## การทดสอบก่อนส่งให้กลุ่มตัวอย่าง

1. เปิดลิงก์ใน Incognito/Private window
2. ตอบแบบสอบถามให้จบ
3. ตรวจดูแถวใหม่ใน `Responses`
4. เปิด Incognito ใหม่หลายครั้งและตรวจว่า A/B/C ถูกกระจายใกล้เคียงกัน
5. ทดสอบทั้งมือถือและคอมพิวเตอร์

## วิธีเปลี่ยนภาพ A/B/C ในอนาคต

วิธีที่ง่ายที่สุดคือแก้ `Images.html` โดยแทนค่า data URI ของ A, B และ C ด้วยภาพใหม่ที่แปลงเป็น base64

หากต้องการ ฉบับต่อไปสามารถแก้แอปให้โหลดภาพจาก Google Drive / Firebase Storage / GitHub Pages ได้แทน เพื่อลดขนาดไฟล์และเปลี่ยนภาพได้ง่ายขึ้น

## ข้อมูลที่บันทึก

ระบบเก็บ condition และคำตอบทุกข้อ รวมถึงเวลาเริ่ม/จบ เวลาเปิดดูภาพ user-agent และขนาดหน้าจอ เพื่อช่วยตรวจสอบคุณภาพข้อมูล

ระบบตัวอย่างนี้ไม่ได้เขียนโค้ดเพื่อบันทึก IP address ลงใน Sheet

## ข้อควรทำก่อนเก็บข้อมูลจริง

- ขอความเห็นชอบด้านจริยธรรมการวิจัยตามข้อกำหนดของสถาบัน (ถ้ามี)
- ตรวจ Content Validity ของแบบสอบถาม
- ทำ stimulus validation แยกก่อน main pilot
- กำหนด sample size/power analysis สำหรับการเปรียบเทียบ A/B/C
- กำหนดเกณฑ์จัดการข้อมูล missing / duplicate / failed manipulation check ล่วงหน้า
- ระบุว่า AI ใช้เพื่อสร้าง/ปรับ stimulus อย่างไรใน Methods
