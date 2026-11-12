# KinNai MVP1 — Setup Checklist (บัญชีและเครื่องมือที่ต้องสมัคร)

อัปเดต: 2026-11-12 · เจ้าของ: Founder · อ้างอิง: `DECISION_LOG.md` (D-006, D-009, D-011), `GATE1_DECISION_PACKAGE.md`

ราคาและเงื่อนไขที่ไม่ได้ระบุตัวเลข ยังไม่ได้ตรวจกับหน้า pricing ปัจจุบันของผู้ให้บริการ ให้ถือเป็น **UNKNOWN** จนกว่าจะตรวจ

## 0. ต้องตัดสินก่อน

- [ ] ลงทะเบียน store ในนาม **บุคคล** หรือ **นิติบุคคล (Yimio)**
  - กำหนดชื่อผู้ขายที่แสดงใน App Store และ Google Play
  - นิติบุคคลต้องมีเลข D-U-N-S ทั้ง Apple และ Google ซึ่งใช้เวลาขอ
- [ ] Region ของ Supabase และ PostHog (Q-004 มีผลต่อการโอนข้อมูลข้ามประเทศ PDPA ม.28–29)
- [ ] Places provider (Q-001 รอ SPIKE-001)

## 1. ทำได้เลย (ฟรี ไม่ผูกมัด ไม่ต้องรอ Gate)

- [ ] อีเมลกลางของโครงการ (เช่น `dev@<โดเมน>`) ใช้สมัครทุกบริการ ไม่ใช้อีเมลส่วนตัว
- [ ] Password manager เก็บ credential และ recovery code ทั้งหมด
- [ ] เปิด 2FA ทุกบัญชีที่สมัคร
- [ ] จดโดเมน สำหรับหน้า privacy policy, หน้าลบบัญชี และอีเมลติดต่อ
- [ ] GitHub
  - [ ] สร้าง `main` ใน `choaalpha-lang/kinnai`
  - [ ] เปิด branch protection บน `main` (ต้องผ่าน PR และ CI)
- [ ] Linear
  - [ ] สร้าง workspace Yimio, team Product Engineering, project KinNai
  - [ ] Import `docs/LINEAR_IMPORT.csv` แล้วกรอก Linear ID กลับใน `LINEAR_BACKLOG.md`
  - [ ] เชื่อม Linear กับ GitHub

## 2. สมัครก่อนเริ่ม Build (หลังอนุมัติ Gate 2)

### Apple
- [ ] สมัคร Apple Developer Program (USD 99 ต่อปี) จำเป็นตั้งแต่ต้นเพราะทดสอบ Sign in with Apple ไม่ได้ถ้าไม่มี
- [ ] สร้าง App ID (bundle identifier) และเปิด capability "Sign in with Apple"
- [ ] สร้าง Services ID และ key สำหรับ Supabase Auth

### Google
- [ ] สมัคร Google Play Console (USD 25 ครั้งเดียว) และยืนยันตัวตน
- [ ] ตรวจเงื่อนไข closed testing สำหรับบัญชีบุคคลใหม่ (จำนวนผู้ทดสอบและจำนวนวันขั้นต่ำก่อนขึ้น production) และเริ่มหาผู้ทดสอบ
- [ ] สร้าง Google Cloud project
- [ ] ตั้งค่า OAuth consent screen
- [ ] สร้าง OAuth client สำหรับ iOS, Android และ web (ใช้กับ Google Sign-In)

### Backend และ build
- [ ] Supabase: สร้าง project `dev` และ `production` (เลือก region ก่อนสร้าง)
- [ ] Supabase: ตรวจว่า free tier หยุด project ที่ไม่มีการใช้งานหรือไม่ ก่อนเลือกแผนของ production
- [ ] Supabase: เปิด Auth provider Apple และ Google
- [ ] Expo: สร้างบัญชีและ organization สำหรับ EAS Build / Submit ตรวจโควตา build ของ free tier
- [ ] เก็บ secret ทั้งหมดใน GitHub Actions secrets และ EAS secrets ไม่ใส่ใน repo

### Analytics และ monitoring
- [ ] PostHog: สร้าง project (เลือก region ของข้อมูลตอนสมัคร)
- [ ] Sentry: สร้าง project สำหรับ React Native

### Places (หลัง SPIKE-001 เท่านั้น)
- [ ] สมัคร provider ที่เลือก
- [ ] ตั้ง budget alert และ quota cap ทันทีที่ได้ API key
- [ ] จำกัดสิทธิ์ของ key ให้เรียกได้จาก server เท่านั้น

## 3. ก่อนเปิด Beta (Gate 4)

- [ ] ที่ปรึกษากฎหมายทบทวน privacy notice, ฐานทางกฎหมาย, retention, ผู้เยาว์ (SPIKE-002, Q-002, Q-003)
- [ ] Privacy policy ภาษาไทยขึ้นบนโดเมน
- [ ] หน้าเว็บขอลบบัญชี (Google Play บังคับให้มี URL)
- [ ] App Store Connect
  - [ ] สร้าง app record
  - [ ] กรอก App Privacy (ประเภทข้อมูลที่เก็บ)
  - [ ] เปิด TestFlight และเพิ่มผู้ทดสอบ
- [ ] Play Console
  - [ ] กรอก Data safety form
  - [ ] Content rating
  - [ ] ระบุ URL privacy policy และหน้าลบบัญชี
  - [ ] เปิด internal / closed testing
- [ ] บัญชีทดสอบสำหรับ reviewer ของ Apple และ Google พร้อมคำอธิบายว่าทำไมต้อง login ตอนยืนยันร้าน (R-006)
- [ ] Runbook: kill switch, cost alert, ขั้นตอนแจ้งเหตุละเมิดข้อมูลภายใน 72 ชั่วโมง (PDPA ม.37(4))

## 4. ยังไม่ต้องสมัคร

| เครื่องมือ | ใช้เมื่อ |
|---|---|
| RevenueCat / store billing | MVP1.5 (Plus) |
| LINE Developers | หลัง MVP1 (LINE Login, LINE share) |
| Vercel | เมื่อมี web หรือ admin; ใช้วางหน้า privacy policy ได้ถ้าไม่มีที่อื่น |
| AI API | ไม่มี AI call ใน MVP1 |

## 5. ค่าใช้จ่ายเทียบเพดาน THB 45,000 (D-009)

| รายการ | จำนวน | สถานะ |
|---|---|---|
| Apple Developer Program | USD 99 ต่อปี | ทราบแล้ว |
| Google Play Console | USD 25 ครั้งเดียว | ทราบแล้ว |
| โดเมน | UNKNOWN | ขึ้นกับชื่อและนามสกุลโดเมน |
| Supabase | เริ่มที่ free tier | เพดาน free tier และราคาแผนถัดไป: UNKNOWN |
| Expo EAS | เริ่มที่ free tier | โควตา build: UNKNOWN |
| PostHog | เริ่มที่ free tier | เพดาน event: UNKNOWN |
| Sentry | เริ่มที่ free tier | เพดาน event: UNKNOWN |
| Places API | UNKNOWN | รอ SPIKE-001 |
| ทบทวนกฎหมาย | UNKNOWN | — |
| Linear, GitHub | เริ่มที่ free tier | — |

ตัวแปรใหญ่ที่ยังไม่รู้คือ Places API และค่าทบทวนกฎหมาย

## 6. บันทึกบัญชีที่สมัครแล้ว

กรอกเมื่อสมัครเสร็จ ห้ามใส่รหัสผ่าน key หรือ token ในไฟล์นี้

| บริการ | อีเมลที่ใช้ | แผน | วันที่สมัคร | วันต่ออายุ | 2FA |
|---|---|---|---|---|---|
| Apple Developer | | | | | |
| Google Play Console | | | | | |
| Google Cloud | | | | | |
| Supabase | | | | | |
| Expo | | | | | |
| PostHog | | | | | |
| Sentry | | | | | |
| Places provider | | | | | |
| Linear | | | | | |
| โดเมน | | | | | |
