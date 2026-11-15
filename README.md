# Arrakis-DUNE Drone Survey Archive

## เกี่ยวกับโปรเจกต์

DUNE Drone Monitoring System เป็นระบบสำหรับดูข้อมูลและติดตามการทำงานของโดรน โดย API Server ทำหน้าที่รับคำขอจากเว็บไซต์ ติดต่อกับ Drone Config Server และ Drone Log Server เพื่อดึงข้อมูลและบันทึกข้อมูลอุณหภูมิ

API Server เป็นส่วนที่ทำหน้าที่เชื่อมต่อระหว่าง Web Frontend กับ Server ของอาจารย์ โดยเว็บไซต์จะเรียกใช้งานผ่าน API Server ของโปรเจกต์นี้

## การทำงานของ API Server

API Server รองรับการทำงานดังนี้

1. ดึงข้อมูลพื้นฐานของโดรนจาก Drone Config Server
2. ตรวจสอบสถานะของโดรนจาก Drone Config Server
3. ดึงประวัติการบันทึกอุณหภูมิจาก Drone Log Server โดยเรียงจากรายการล่าสุดไปเก่าที่สุด และแสดงข้อมูลไม่เกิน 12 รายการต่อหน้า
4. รับข้อมูลอุณหภูมิจากเว็บไซต์และส่งไปบันทึกใน Drone Log Server

## หมายเหตุ

-ต้องเปิด API Server และ Web Server พร้อมกันขณะใช้งานเว็บไซต์
- ตรวจสอบค่าในไฟล์ .env ให้ถูกต้องก่อนเริ่มเซิร์ฟเวอร์
- หากมีการแก้ไขค่าในไฟล์ .env ควรเริ่มเซิร์ฟเวอร์ใหม่เพื่อให้ระบบอ่านค่าที่เปลี่ยนแปลง
- ไฟล์ .env ใช้เก็บ URL และ API Token สำหรับเชื่อมต่อกับเซิร์ฟเวอร์จริง จึงไม่ให้นำไฟล์ขึ้น GitHub
- ในโฟลเดอร์จะมีไฟล์ .env.example เป็นตัวอย่างสำหรับตั้งค่าแทนการเผยแพร่ข้อมูลจริงเท่านั้นค่ะ

## URL สำหรับเข้าใช้งาน API

- API Server: https://arrakis-dune-drone-survey-archive.onrender.com/

หมายเหตุ: เนื่องจาก Render Free จะหยุดการทำงานของ Web Service เมื่อไม่มีการใช้งานเป็นระยะเวลาหนึ่ง เมื่อมีการเรียกใช้งานอีกครั้ง เซิร์ฟเวอร์อาจใช้เวลาประมาณ 30–60 วินาทีในการเริ่มทำงานค่ะ

## ติดตั้ง tools ก่อนรันบน VS Code

- Node.js เวอร์ชัน 18 ขึ้นไป
- npm ซึ่งติดตั้งมาพร้อมกับ Node.js

- ตรวจสอบเวอร์ชันได้ด้วยคำสั่ง

```bash
node -v
npm -v
```

## วิธีติดตั้งและเริ่มรัน บน VS Code
ระบบประกอบด้วย API Server และ Web Server ซึ่งต้องเปิดใช้งานพร้อมกัน โดยให้เปิด Terminal แยกกัน 2 terminal

### 1. ตั้งค่าและเปิด API Server

- เปิด Terminal แล้วเข้าไปยังโฟลเดอร์โปรเจกต์ API

```bash
cd dune-drone-api
```

- ติดตั้งแพ็กเกจที่จำเป็นด้วยคำสั่ง

```bash
npm install
```

- สร้างไฟล์ .env จากไฟล์ตัวอย่าง หากใช้ PowerShell บน Windows ให้ใช้คำสั่ง

```powershell
Copy-Item .env.example .env
```

- เปิดไฟล์ .env และกำหนดค่าตาม API ของตนเองที่ได้รับจาก server อาจารย์

```env
CONFIG_URL=URL ของ Drone Config Server

LOG_URL=URL ของ Drone Log Server

LOG_API_TOKEN=API TOKEN ที่อาจารย์กำหนด

PORT=3000
```

- เมื่อกำหนดค่าเรียบร้อยแล้ว ให้เริ่ม API Server ด้วยคำสั่ง

```bash
npm start
```

- หากเริ่มทำงานสำเร็จ จะเห็นข้อความ

```text
API listening on :3000
```

- API Server จะเปิดให้ใช้งานที่

```text
http://localhost:3000
```

## API Endpoints

GET /configs/:droneId ดึงข้อมูลพื้นฐานของโดรน

GET /status/:droneId ตรวจสอบสถานะของโดรน

GET /logs/:droneId?page=1 ดึงประวัติอุณหภูมิของโดรน

POST /logs บันทึกข้อมูลอุณหภูมิ

หมายเหตุ: API Server ทำหน้าที่รับคำขอจากเว็บไซต์ แล้วติดต่อกับ Drone Config Server หรือ Drone Log Server ตามการทำงานของแต่ละ API โดยเว็บไซต์จะเรียกใช้งานผ่าน API Server ของโปรเจกต์นี้ ไม่ได้เรียก Server ของอาจารย์โดยตรงตามที่อาจารย์ได้กำหนดไว้ค่ะ
