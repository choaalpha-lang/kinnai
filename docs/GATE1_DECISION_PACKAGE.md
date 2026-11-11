# KinNai — Gate 1 Decision Package (Product Specification Approval)

วันที่: 2026-11-11 · จัดทำโดย: Claude Code (Discover + Specify) · ผู้อนุมัติ: Founder
ขอบเขตงานที่ทำ: เอกสารและต้นแบบ HTML เท่านั้น ไม่มีการติดตั้ง dependency, สร้าง app code, เชื่อม credential, สร้าง Linear issue หรือ deploy

## 1. Executive Summary

- **ข้อเสนอ: CONDITIONAL GO** สู่ Gate 2 (Architecture & Plan) โดยมีเงื่อนไข 4 ข้อใน §6
- Spec ของ MVP1 พร้อม: 6 requirement ตาม blueprint + 5 requirement สนับสนุน มี AC แบบ Given/When/Then, edge case, analytics และ test ครบ
- Founder ตัดสินแล้ว 5 เรื่อง (D-001 ถึง D-005) และต้นแบบปรับตามแล้ว
- ยังเปิดอยู่ 1 เรื่องที่กระทบ scope และต้นทุนโดยตรง: **Places provider (Q-001)** ซึ่งกำหนดว่าตัวกรองงบทำได้จริงแค่ไหน
- เป้า TTY ≤ 30 วินาทียังเป็นสมมติฐาน ต้นแบบพิสูจน์ได้แค่ว่า flow ใช้งานได้ ไม่ได้พิสูจน์ตัวเลข

## 2. สิ่งที่ตรวจพบใน Discover

| รายการ | ผล | หลักฐาน |
|---|---|---|
| Repository | ว่างเปล่าก่อนงานนี้ ไม่มี default branch | `git ls-remote` พบเฉพาะ `ccr-4db9404f-j4j6ny` |
| Linear tools | ไม่มีใน session นี้ | จึงจัดทำ `LINEAR_BACKLOG.md` และ `LINEAR_IMPORT.csv` ไม่มีการสร้างหรืออ้าง Linear ID |
| App code | ไม่มี | มีเฉพาะ `prototype/` (HTML เดี่ยว) |
| ราคา/โควตา Places provider | UNKNOWN | ยังไม่ได้ตรวจกับเอกสารของผู้ให้บริการ (SPIKE-001) |
| ความเห็นทางกฎหมาย PDPA | UNKNOWN | ยังไม่มีการทบทวนโดยผู้เชี่ยวชาญ (SPIKE-002) |

## 3. พร้อมแล้ว

| เอกสาร | เนื้อหา |
|---|---|
| `PRODUCT_VISION.md`, `BMC.md`, `PRODUCT_ROADMAP.md` | สรุปจาก blueprint V1.2 แยกสิ่งที่ยืนยันแล้วกับสมมติฐาน |
| `PRD.md` | ขอบเขต, flow, ตรรกะแนะนำ, สถาปัตยกรรมระดับสูง, PDPA |
| `REQUIREMENTS.md` | AC รายข้อ, NFR, Definition of Done |
| `DESIGN_SPEC.md` | Tokens, หน้าจอ, สถานะ, accessibility |
| `ANALYTICS_PLAN.md` | Event schema, สูตร KPI, guardrails |
| `LINEAR_WORKFLOW.md`, `LINEAR_BACKLOG.md`, `LINEAR_IMPORT.csv` | โครงสร้างและ backlog 15 รายการพร้อม import |
| `DECISION_LOG.md` | 10 decisions, 6 assumptions, 6 hypotheses, 8 open questions, 5 risks |
| `prototype/` | ต้นแบบคลิกได้ ปรับตาม D-001 ถึง D-004 |

## 4. Independent challenge (5C)

### F-01 — ตัวกรองงบอาจทำไม่ได้ตามที่ออกแบบ (สูง)

- **Condition:** SOLO-001 ให้เลือกงบเป็นช่วงบาทต่อคน และ SOLO-002 แสดง "ประมาณ ฿X ต่อคน" ต้นแบบทำได้เพราะใช้ราคาสมมติ
- **Criteria:** Blueprint กำหนด "relevant shortlist" และ value proposition "คำนึงถึงงบ"; D-002 กำหนดงบเป็นเพดาน
- **Cause:** ยังไม่ได้เลือก provider (Q-001) ผู้ให้บริการ Places ทั่วไปมักให้ระดับราคาแบบหยาบ ไม่ใช่ราคาต่อหัวเป็นบาท ความครอบคลุมของข้อมูลนี้สำหรับร้านข้างทางในกรุงเทพฯ: UNKNOWN
- **Effect:** ร้านเกินงบหลุดเข้ามา หรือร้านราคาถูกที่ไม่มีข้อมูลถูกตัดออก กระทบ H-004 และความเชื่อมั่นตั้งแต่ครั้งแรกที่ใช้
- **Recommendation:** ทำ SPIKE-001 ก่อน Gate 2: ดึงข้อมูลจริง 3 ย่านเปิดตัว วัดสัดส่วนร้านที่มีข้อมูลราคา แล้วเลือกระหว่าง (ก) map ระดับราคาเป็นช่วงบาท (ข) ลดตัวกรองเป็น 3 ระดับ ถูก/กลาง/แพง (ค) เก็บราคาเองในย่านนำร่อง

### F-02 — นิยาม TTY ตาม D-001 ไม่ตรงกับคำสัญญาที่ผู้ใช้รับรู้ (กลาง)

- **Condition:** TTY เริ่มนับหลังกำหนด criteria เสร็จ เวลาที่ใช้เลือกงบ แนว ระยะ ไม่ถูกนับ
- **Criteria:** คำสัญญาใน blueprint คือ "Solo: ตัดสินใจภายใน 30 วินาที"; blueprint เตือนเรื่อง KPI ที่ "ดูดีเกินจริง"
- **Cause:** นิยามเลือกวัดคุณภาพของการแนะนำ ไม่ใช่ทั้งประสบการณ์
- **Effect:** รายงานว่าเข้าเป้าได้ทั้งที่ผู้ใช้ใช้เวลารวมเกิน 30 วินาที การออกแบบอาจย้ายความซับซ้อนไปไว้หน้าแรกโดย KPI ไม่สะท้อน
- **Recommendation:** คง D-001 เป็น KPI หลัก และเก็บ `time_to_criteria` กับ end-to-end time เป็น guardrail (A-002) รายงานสามตัวคู่กัน ขอ Founder ยืนยัน A-001 และ A-002

### F-03 — เงื่อนไข license ของ provider กำหนดต้นทุนต่อการตัดสินใจ (สูง)

- **Condition:** ยังไม่มีการเลือก provider และไม่มีตัวเลขต้นทุนต่อคำขอ
- **Criteria:** Blueprint: "select provider after pricing, coverage and licensing evaluation", guardrail "API cost per successful decision", งบ MVP THB 45,000 (D-009)
- **Cause:** Q-001 ยังไม่ถูกตัดสินในรอบนี้
- **Effect:** ถ้า license ห้าม cache ผลค้นหา ทุกครั้งที่กด "ขอชุดใหม่" คือค่าใช้จ่ายใหม่ ต้นทุนโตตามการใช้งานโดยตรง และ H-006 พิสูจน์ไม่ได้
- **Recommendation:** SPIKE-001 เปรียบเทียบอย่างน้อย 3 ทางเลือกด้วยเกณฑ์ใน §5 และออกแบบ PLAT-003 ให้เปลี่ยน provider ได้ พร้อม daily cap และ kill switch ตั้งแต่วันแรก

### F-04 — ภาระ data controller ตกที่ Founder คนเดียว (กลาง)

- **Condition:** แอปประมวลผลตำแหน่งและพฤติกรรมการกิน ส่งข้อมูลไปผู้ให้บริการต่างประเทศอย่างน้อย 4 ราย
- **Criteria:** PDPA พ.ศ. 2562 ม.19, 23, 28–29, 33, 37 (รวมการแจ้งเหตุละเมิดภายใน 72 ชั่วโมง); D-005
- **Cause:** One-person company ไม่มีฝ่ายกฎหมายหรือ DPO
- **Effect:** ความเสี่ยงด้านกฎหมายและการถูกปฏิเสธจาก App Store / Google Play หาก privacy disclosure ไม่ครบ
- **Recommendation:** ลดข้อมูลตั้งแต่ออกแบบ (anonymous auth, ไม่เก็บพิกัดดิบ, ไม่มี free-text) ทำ SPIKE-002 ให้ผู้เชี่ยวชาญทบทวน notice, ฐานทางกฎหมาย, retention (Q-002), ผู้เยาว์ (Q-003) และการโอนข้ามประเทศ (Q-004) ก่อน Gate 4

### F-05 — ความซับซ้อนก่อนเวลา (ต่ำ)

- **Condition:** Blueprint ให้มี Kampai prototype ใน MVP1 และ roadmap มี 5 ระยะ
- **Criteria:** หลักการ stage gate ของ blueprint
- **Cause:** แรงดึงให้ port Kampai เข้าแอปจริง
- **Effect:** เวลา Founder ถูกแบ่งจากการพิสูจน์ Solo
- **Recommendation:** ถือว่า GRP-001 เสร็จแล้วด้วย HTML prototype ไม่ port เข้าแอปใน MVP1 (Q-008)

## 5. หลักฐานที่ต้องมีก่อน Gate 2

**SPIKE-001 Places provider** เกณฑ์เปรียบเทียบ: (1) ความครอบคลุมร้านในย่านเปิดตัว รวมร้านข้างทาง (2) มีข้อมูลราคา เวลาเปิด ประเภทอาหารหรือไม่ (3) เงื่อนไขการ cache และการแสดงผล/attribution (4) ราคาต่อคำขอและ free tier (5) อนุญาตให้ใช้คู่กับแผนที่ของผู้ให้บริการอื่นหรือไม่
ทางเลือกที่ควรประเมิน: Google Places API, Foursquare Places, HERE, OpenStreetMap (ODbL) และข้อมูลร้านอาหารในไทยที่เปิด API (ถ้ามี) ตัวเลขราคาและเงื่อนไขปัจจุบันของทุกราย: UNKNOWN ต้องอ่านจากเอกสารทางการ ณ วันประเมิน

**ประมาณการงบ** โครงรายการ (ตัวเลขต้องยืนยันจากหน้าราคาปัจจุบัน): Apple Developer Program, Google Play Console, Supabase, Expo EAS, PostHog, Sentry, Places API, โดเมน, ค่าทบทวนกฎหมาย เทียบกับเพดาน THB 45,000

## 6. เงื่อนไขของ CONDITIONAL GO

| # | เงื่อนไข | เจ้าของ | ปิดเมื่อ |
|---|---|---|---|
| C-1 | ตัดสิน Q-001 ด้วยผล SPIKE-001 และเลือกแนวทางตัวกรองงบ (F-01) | Founder | ก่อนอนุมัติ Gate 2 |
| C-2 | ยืนยันหรือแก้ A-001 ถึง A-006 | Founder | ก่อนเริ่ม Gate 2 |
| C-3 | สร้าง default branch และเชื่อม Linear (หรือยืนยันใช้ CSV) | Founder | ก่อนเริ่ม Build |
| C-4 | กำหนดพื้นที่เปิดตัว (Q-005) และ Supabase region (Q-004) | Founder | ก่อนอนุมัติ Gate 2 |

## 7. ทางเลือกของ Founder

| ทางเลือก | ความหมาย |
|---|---|
| **GO** | ยอมรับ spec ตามที่เป็น และรับความเสี่ยง F-01, F-03 ไปแก้ใน Gate 2 |
| **CONDITIONAL GO** (เสนอ) | อนุมัติ spec และให้ทำ SPIKE-001, SPIKE-002 กับแผนสถาปัตยกรรมของ Gate 2 โดยยังไม่เขียน app code |
| **REVISE** | แก้ scope ก่อน เช่น ลดตัวกรองงบ หรือเปลี่ยนนิยาม KPI |
| **STOP** | หยุดโครงการ |

## 8. Next Steps

1. Founder เลือก GO / CONDITIONAL GO / REVISE / STOP และตอบ C-2 (A-001 ถึง A-006)
2. เมื่ออนุมัติ: Claude Code ทำ SPIKE-001 (เปรียบเทียบ provider จากเอกสารทางการ) และร่าง privacy notice สำหรับ SPIKE-002
3. Founder สร้าง `main` ใน repo และเชื่อม Linear หรือ import `LINEAR_IMPORT.csv`
4. จัดทำ Gate 2 package: schema, dependency, ประมาณการต้นทุน, test plan

**หยุดที่ Gate 1 รอการอนุมัติจาก Founder**
