# KinNai MVP1 — Setup Checklist (web app, mobile-first)

อัปเดต: 2026-11-24 · เจ้าของ: Founder · อ้างอิง: `DECISION_LOG.md` (D-009, D-011, D-013), `PRD.md` §6

MVP1 เป็น web app (D-013) จึงยังไม่ต้องสมัคร App Store / Google Play / Expo ไว้ในหัวข้อ 4
ราคาและเงื่อนไขที่ไม่ได้ระบุตัวเลข ยังไม่ได้ตรวจกับหน้า pricing ปัจจุบันของผู้ให้บริการ ให้ถือเป็น **UNKNOWN** จนกว่าจะตรวจ

## 0. ต้องตัดสินก่อน

- [ ] **Sign in with Apple บนเว็บ:** คงไว้หรือเริ่มด้วย Google อย่างเดียว (Q-010)
  - บนเว็บ Apple ไม่บังคับให้ต้องมี (ข้อ 4.8 ใช้กับ App Store)
  - ถ้าคงไว้ ต้องมี Apple Developer Program (USD 99 ต่อปี) เพื่อสร้าง Services ID และ key
  - สัดส่วนผู้ใช้ iPhone ในกลุ่มเป้าหมาย: UNKNOWN
- [ ] ลงทะเบียนบัญชีต่าง ๆ ในนาม **บุคคล** หรือ **นิติบุคคล (Yimio)** (มีผลกับใบเสร็จ ภาษี และชื่อที่แสดงในหน้า OAuth consent ของ Google)
- [ ] Region ของ Supabase และ PostHog (Q-004 มีผลต่อการโอนข้อมูลข้ามประเทศ PDPA ม.28–29)
- [ ] Places provider (Q-001 รอ SPIKE-001)
- [ ] ช่องทางแจกลิงก์ทดสอบ (Q-012) เพราะ LINE in-app browser มีผลต่อ Google login (R-008)

## 1. ทำได้เลย (ฟรี ไม่ผูกมัด ไม่ต้องรอ Gate)

- [ ] อีเมลกลางของโครงการ (เช่น `dev@<โดเมน>`) ใช้สมัครทุกบริการ
- [ ] Password manager เก็บ credential และ recovery code ทั้งหมด
- [ ] เปิด 2FA ทุกบัญชี
- [ ] **จดโดเมนก่อนอย่างอื่น** เพราะ web app ต้องใช้ตั้งแต่วันแรก: ที่อยู่ของแอป, Redirect URI ของ Google/Apple OAuth, หน้า privacy policy และอีเมลติดต่อ
- [ ] GitHub
  - [ ] สร้าง `main` ใน `choaalpha-lang/kinnai`
  - [ ] เปิด branch protection บน `main` (ต้องผ่าน PR และ CI)
- [ ] Linear
  - [ ] สร้าง workspace Yimio, team Product Engineering, project KinNai
  - [ ] Import `docs/LINEAR_IMPORT.csv` (17 รายการ) แล้วกรอก Linear ID กลับใน `LINEAR_BACKLOG.md`
  - [ ] เชื่อม Linear กับ GitHub

## 2. สมัครก่อนเริ่ม Build (หลังอนุมัติ Gate 2)

### Hosting และโดเมน
- [ ] Vercel: สร้างบัญชีและเชื่อม GitHub
- [ ] ตรวจเงื่อนไขของแผนฟรี ว่าอนุญาตการใช้งานเชิงพาณิชย์และการทดสอบกับผู้ใช้จริงหรือไม่ (Q-011) ถ้าไม่ ต้องใช้แผนเสียเงินหรือ host อื่น
- [ ] ผูกโดเมนกับ Vercel และตรวจว่าเป็น HTTPS (Geolocation API ต้องใช้ HTTPS)
- [ ] แยก environment: preview (ต่อ PR) และ production

### Backend
- [ ] Supabase: สร้าง project `dev` และ `production` (เลือก region ก่อนสร้าง)
- [ ] Supabase: ตรวจว่าแผนฟรีหยุด project ที่ไม่มีการใช้งานหรือไม่ ก่อนเลือกแผนของ production
- [ ] Supabase: เปิด Auth provider Google (และ Apple ถ้า Q-010 ตัดสินให้คงไว้) และใส่ Redirect URL ของโดเมนจริง

### Login
- [ ] Google Cloud project
  - [ ] ตั้งค่า OAuth consent screen (ชื่อแอป โลโก้ โดเมน privacy policy)
  - [ ] สร้าง OAuth client ชนิด web
  - [ ] ตรวจสถานะ publishing ของ consent screen ว่าผู้ใช้ทั่วไปเข้าได้ ไม่ใช่เฉพาะ test users
- [ ] Apple (เฉพาะถ้าคง Sign in with Apple)
  - [ ] สมัคร Apple Developer Program (USD 99 ต่อปี)
  - [ ] สร้าง App ID และ Services ID, เปิด "Sign in with Apple"
  - [ ] ยืนยันโดเมนและตั้ง Return URL
  - [ ] สร้าง key สำหรับ Supabase Auth
  - [ ] จดวันหมดอายุของ client secret ที่ Supabase ใช้และตั้งเตือนต่ออายุ (UNKNOWN: ระยะเวลาปัจจุบัน ตรวจตอนตั้งค่า)

### Analytics และ monitoring
- [ ] PostHog: สร้าง project (เลือก region ของข้อมูลตอนสมัคร)
- [ ] Sentry: สร้าง project ชนิด browser JavaScript / Next.js

### Places (หลัง SPIKE-001 เท่านั้น)
- [ ] สมัคร provider ที่เลือก
- [ ] ตั้ง budget alert และ quota cap ทันทีที่ได้ API key
- [ ] จำกัด key ให้เรียกได้จาก server เท่านั้น ไม่ใส่ในโค้ดฝั่งเบราว์เซอร์

### Secret
- [ ] เก็บ secret ทั้งหมดใน Vercel environment variables, Supabase secrets และ GitHub Actions secrets ไม่ใส่ใน repo
- [ ] ตรวจว่าไม่มี secret ใน bundle ฝั่งเบราว์เซอร์ (ตัวแปรที่ขึ้นต้นด้วย `NEXT_PUBLIC_` ถูกส่งถึงผู้ใช้ทุกคน)

## 3. ก่อนเปิดทดสอบกับผู้ใช้จริง (Gate 4)

- [ ] ที่ปรึกษากฎหมายทบทวน privacy notice, ฐานทางกฎหมาย, retention, ผู้เยาว์ (SPIKE-002, Q-002, Q-003)
- [ ] Privacy policy ภาษาไทยขึ้นบนโดเมน
- [ ] หน้าลบบัญชีในแอป ("ฉัน" → ลบบัญชีและข้อมูล) ทำงานจริงและลบ PostHog person ด้วย
- [ ] Consent banner: ไม่เริ่ม PostHog จนกว่าจะยอมรับ (R-010)
- [ ] ทดสอบบนเครื่องจริง: iOS Safari, Chrome Android และเปิดลิงก์จาก LINE (R-008)
- [ ] ทดสอบ Geolocation ในกรณีอนุญาต, ปฏิเสธ และปฏิเสธแล้วต้องการเปิดใหม่
- [ ] แผนทดสอบผู้ใช้ (UXR-001): กลุ่มเป้าหมาย จำนวน สคริปต์ และเกณฑ์ผ่านของ H-001, H-004
- [ ] Dashboard Time to Yes, completion rate, auth completion rate พร้อมใช้ก่อนเปิดทดสอบ
- [ ] Runbook: kill switch, cost alert, ขั้นตอนแจ้งเหตุละเมิดข้อมูลภายใน 72 ชั่วโมง (PDPA ม.37(4))

## 4. เลื่อนไปก่อน (ทำเมื่อตัดสินทำ native app)

| รายการ | เมื่อไร | หมายเหตุ |
|---|---|---|
| Google Play Console (USD 25 ครั้งเดียว) | ตัดสินทำแอป Android | บัญชีบุคคลใหม่มีเงื่อนไข closed testing ก่อนขึ้น production ตรวจเงื่อนไขปัจจุบัน |
| App Store Connect, TestFlight | ตัดสินทำแอป iOS | ใช้ Apple Developer Program เดียวกับ Sign in with Apple |
| Expo / EAS | ตัดสินทำด้วย React Native | หรือห่อเว็บด้วย Capacitor ยังไม่ได้ประเมิน |
| ข้อกำหนด store: 4.8, 5.1.1(v), Data safety, App Privacy | ตอนส่ง store | R-006 |
| RevenueCat / store billing | MVP1.5 (Plus) | |
| LINE Developers | หลัง MVP1 | LINE Login, LINE share |
| AI API | ไม่มีใน MVP1 | |

## 5. ค่าใช้จ่ายเทียบเพดาน THB 45,000 (D-009)

| รายการ | จำนวน | สถานะ |
|---|---|---|
| Apple Developer Program | USD 99 ต่อปี | ทราบแล้ว ต้องจ่ายเฉพาะถ้าคง Sign in with Apple (Q-010) |
| โดเมน | UNKNOWN | ขึ้นกับชื่อและนามสกุลโดเมน |
| Vercel | UNKNOWN | แผนฟรีอาจไม่ครอบคลุมการใช้เชิงพาณิชย์ (Q-011) |
| Supabase | เริ่มที่แผนฟรี | เพดานและราคาแผนถัดไป: UNKNOWN |
| PostHog | เริ่มที่แผนฟรี | เพดาน event: UNKNOWN |
| Sentry | เริ่มที่แผนฟรี | เพดาน event: UNKNOWN |
| Places API | UNKNOWN | รอ SPIKE-001 |
| ทบทวนกฎหมาย | UNKNOWN | — |
| Linear, GitHub, Google Cloud (OAuth) | เริ่มที่แผนฟรี | — |
| Google Play (USD 25) | เลื่อนไป | ไม่ใช้ใน MVP1 |

ตัวแปรใหญ่ที่ยังไม่รู้คือ Places API, hosting และค่าทบทวนกฎหมาย

## 6. บันทึกบัญชีที่สมัครแล้ว

กรอกเมื่อสมัครเสร็จ ห้ามใส่รหัสผ่าน key หรือ token ในไฟล์นี้

| บริการ | อีเมลที่ใช้ | แผน | วันที่สมัคร | วันต่ออายุ | 2FA |
|---|---|---|---|---|---|
| โดเมน | | | | | |
| Vercel | | | | | |
| Supabase | | | | | |
| Google Cloud | | | | | |
| Apple Developer (ถ้าคงไว้) | | | | | |
| PostHog | | | | | |
| Sentry | | | | | |
| Places provider | | | | | |
| Linear | | | | | |
