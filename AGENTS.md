# LingoEat  - System Skills & Scope

## 📌 ภาพรวมของระบบ (System Overview)
LingoEat (หรือ MenuSpeak) คือเว็บแอปพลิเคชันฝึกทักษะภาษาอังกฤษเชิงสถานการณ์สำหรับผู้ประกอบการร้านอาหารในชุมชน (แนวคิด "Duolingo สำหรับแม่ค้าร้านอาหารตามสั่ง") ระบบเน้นการเรียนรู้ผ่านสถานการณ์จำลองที่ใช้จริง ไม่เน้นไวยากรณ์ซับซ้อน และมีระบบตรวจจับเสียงเพื่อฝึกการพูด

## 🛠️ เทคโนโลยีหลัก (Tech Stack)
- **Frontend:** React, TypeScript, Tailwind CSS (รันบน Vite / TanStack Start)
- **Backend & Database:** Supabase (PostgreSQL, Authentication, Row Level Security)
- **Speech Tech:** Web Speech API (สำหรับการตรวจจับเสียงพูดและประเมินผล)

## ⚙️ ขอบเขตการทำงานหลัก (Core Features)

### 1. ระบบบทเรียนเชิงสถานการณ์ (Situational Lessons)
รองรับเนื้อหา 4 สถานการณ์หลักสำหรับการทำงานในร้านอาหาร:
- **Greetings & Seating:** การทักทายและการจัดที่นั่ง
- **Menus & Recommendations:** การอธิบายเมนูและแนะนำอาหาร
- **Orders & Special Requests:** การรับออเดอร์และเงื่อนไขพิเศษ
- **Billing & Payment:** การคิดเงินและการชำระเงิน

### 2. ระบบแบบฝึกหัด (Quiz & Exercises Module)
แต่ละบทเรียนมีแบบฝึกหัด 7 ข้อ แบ่งเป็น 3 ระดับความยากเพื่อไต่ระดับทักษะ:
- **Easy (3 ข้อ):** Multiple Choice (แบบเลือกตอบ) - เน้นจดจำคำศัพท์
- **Medium (2 ข้อ):** Fill in the blanks (แบบเติมคำในช่องว่าง) - เน้นบริบทและโครงสร้างประโยค
- **Hard (2 ข้อ):** Speech Recognition (แบบฝึกพูด) - ใช้ไมโครโฟนเพื่อตรวจสอบการออกเสียง

### 3. ระบบติดตามความก้าวหน้า (Gamification & Progress Tracking)
- บันทึกคะแนนและสถานะการเรียนรู้ของผู้ใช้ในตาราง `user_progress`
- แสดงสถิติ สรุปผลคะแนน และหน้า Dashboard สำหรับผู้เรียน

### 4. ระบบจัดการผู้ใช้และสิทธิ์ (Auth & Role Management)
- **User:** สมัครสมาชิก, ล็อกอิน, เข้าถึงบทเรียน, ทำแบบฝึกหัด, ดูคะแนนตัวเอง
- **Admin:** เข้าถึงหน้า Dashboard, เพิ่ม/แก้ไขข้อมูลบทเรียน (Lessons), คำศัพท์ (Vocabulary), และแบบฝึกหัด (Exercises)

## 🗄️ โครงสร้างฐานข้อมูล (Database Relational Mapping)
- `profiles`: ข้อมูลผู้ใช้และสิทธิ์ (Role: 'user' หรือ 'admin')
- `lessons`: เก็บข้อมูลบทเรียนหลัก
- `vocabulary`: คำศัพท์ที่เชื่อมโยงกับ `lessons.id`
- `exercises`: แบบฝึกหัดที่เชื่อมโยงกับ `lessons.id` (มีฟิลด์ type ระบุประเภท: choice, fill, speech)
- `user_progress` / `exercise_results`: เก็บข้อมูลความก้าวหน้าที่เชื่อม `user_id` และ `exercise_id`

## ⚠️ กฎและข้อควรระวังสำหรับ Editor / AI (Editor Instructions & Rules)
1. **Environment Variables:** โปรเจกต์นี้ใช้ **Vite** ดังนั้นการเรียกใช้ Environment Variables **ต้องใช้ `import.meta.env.VITE_...`** ห้ามใช้ `process.env` โดยเด็ดขาด
2. **Supabase Client:** ให้ใช้ไฟล์ Client ที่มีอยู่แล้วในโปรเจกต์ (เช่น `src/utils/supabaseClient.js` หรือ `.ts`) ในการเชื่อมต่อ
3. **TypeScript First:** เขียนโค้ดแบบ Strongly Typed เสมอ และตรวจสอบ Nullable data ที่ได้จากการดึงข้อมูลจาก Supabase 
4. **Speech API Fallbacks:** เนื่องจากระบบใช้ Web Speech API ให้ใส่ Logic สำหรับแจ้งเตือน (Error Handling) หรือ Fallback หากเปิดใช้งานผ่าน Browser ที่ไม่รองรับ หรือผู้ใช้ไม่ให้สิทธิ์การเข้าถึงไมโครโฟน
5. **Git / Lovable Sync:** ห้าม rewrite git history (force push, rebase) เนื่องจากอาจทำให้การ Sync กับแพลตฟอร์ม Lovable มีปัญหา