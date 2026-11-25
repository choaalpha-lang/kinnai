# KinNai — Product Roadmap (เดือน 0–24)

ที่มา: Blueprint V1.2 และ 10X Strategy v1.0 (ปรับ 2026-11-26) · ทำตาม stage gate เฉพาะ MVP1 ที่เข้า detailed specification (D-007) ระยะหลังจากนั้นเป็นทิศทาง ไม่ใช่คำมั่น

| ระยะ | ช่วง | ขอบเขต | Gate ที่ต้องผ่าน | สถานะ |
|---|---|---|---|---|
| MVP 1.0 Kampai Protocol + Solo (web app mobile-first, D-013, D-016) | M0–3 | ลิงก์กลุ่ม โหวตโดยไม่ต้อง login · การ์ดสรุปแชร์กลับ · ระบบแนะนำร่วมกับ Solo · analytics ของ K · โครงข้อมูล intent | วัน 1–30: K ≥ 1.2 และ ≥ 80% ได้ผลใน 5 นาที · วัน 31–60: organizer repeat ≥ 30% ใน 30 วัน | **อยู่ที่ Gate 1** |
| MVP 1.5 Revenue pilots | M4–6 | pilot manual: Intent Exchange กับแบรนด์ และ Workplace กับองค์กร (WPL-001 ถ้าผ่าน gate วัน 60) นอกตัวผลิตภัณฑ์ | วัน 61–90: จ่ายซ้ำหรือต่อสัญญา และพิสูจน์ ROI | ยังไม่ spec |
| MVP 2.0 Growth | M7–12 | Intent Exchange ขนาดเล็กในผลิตภัณฑ์ (offer ติดป้าย แยกจาก organic) ถ้าผ่านเกณฑ์ · personalization · retention | มีหลักฐานการใช้ซ้ำและ contribution | ยังไม่ spec |
| Growth 2.0 Monetization | M13–18 | Affiliate · sponsored result (ติดป้าย) · AI-assisted support | หลักฐาน breakeven รายเดือน | ยังไม่ spec |
| Scale 3.0 Automation | M19–24 | Merchant Pro candidate · self-service · ต้นทุนและการดำเนินงาน | ARPU, ภาระงาน, economics | ยังไม่ spec |

## Gates ภายใน MVP1

| Gate | สิ่งที่ Founder อนุมัติ | สถานะ |
|---|---|---|
| Gate 1 | PRD, user stories, AC, scope | รออนุมัติ: ดู `GATE1_DECISION_PACKAGE.md` |
| Gate 2 | สถาปัตยกรรม, dependency, ต้นทุน, test plan | ยังไม่เริ่ม |
| Gate 3 | Feature coverage และหลักฐานการทดสอบที่รันจริง | ยังไม่เริ่ม |
| Gate 4 | Web beta กับผู้ใช้จริงใน 2–3 เครือข่ายที่ไม่ทับซ้อน (D-022) ตัดสิน gate วัน 1–30 (K, 5 นาที) และวัน 31–60 (organizer repeat) · privacy และความพร้อมด้านกฎหมาย · ยังไม่ส่ง store (D-013) | ยังไม่เริ่ม |

## ลำดับงานของ MVP1

1. **Gate 1 → 2:** SPIKE-001 (Places provider), SPIKE-002 (PDPA), แผนสถาปัตยกรรมและต้นทุน
2. **Build (D-016):** PLAT-001/002, ANL-001/002 → PLAT-003, PRIV-001 → GRP-003, GRP-002, GRP-004, GRP-005 → SOLO-002/004 และ SOLO-001/003/005 → AUTH-001, INT-001, BRAND-001
3. **Gate 3:** หลักฐาน test ครบทุก AC
4. **Gate 4:** Web beta ในพื้นที่เปิดตัว เก็บข้อมูล TTY จริง แล้วตัดสินว่าจะทำ native app (App Store / Google Play) หรือไม่ โดยใช้ผลทดสอบนี้

ไม่มีการกำหนดวันที่รายสัปดาห์ในขั้นนี้ เพราะขึ้นกับผล SPIKE-001 และเวลาของ Founder (H-005)

## การจับคู่กับแผน 90 วันของ strategy

| ช่วง | งาน / ผลลัพธ์ | หมายเหตุ |
|---|---|---|
| ก่อนนับวันที่ 1 | SPIKE-001/002, Gate 2, สร้าง GRP-002 ถึง 005, ANL-001/002 | วันที่ 1 เริ่มเมื่อผลิตภัณฑ์ใช้งานได้ ไม่ใช่เริ่มตอนนี้ ระยะสร้างจริง UNKNOWN |
| วัน 1–30 | เปิดลิงก์ให้ 2–3 เครือข่าย วัด K และ completion ภายใน 5 นาที | H ของ K ต้องรอ 14 วันหลังเข้าร่วม ผลบางส่วนยังไม่ครบ |
| วัน 31–60 | วัด organizer repeat และคุณภาพข้อมูล เริ่ม WPL-001 ถ้าผ่าน | ต้องมีผู้จัดที่เริ่มตั้งแต่ช่วงแรก |
| วัน 61–90 | pilot จ่ายเงินแบบ manual (แบรนด์ องค์กร) นอก app | นอกขอบเขต MVP1 |

เป้า 10X (MAU 10 ล้าน กำไร ฿1,000 ล้านใน 12 เดือน) เป็นเป้าทิศทาง ไม่ใช่เงื่อนไขของ spec (D-021) เปิดงบเพิ่มเมื่อผ่าน gate รายขั้นเท่านั้น

## KPI และ guardrails

North Star: Successful Decisions (Kampai และ Solo รายงานแยก) · Gate: K, organizer repeat, group completion ≤ 5 นาที (D-020) · Solo TTY median/P90 (รอง) · retention
Guardrails: network saturation, join friction · ความพึงพอใจและความตรงของคำแนะนำ · ต้นทุน API ต่อการตัดสินใจ · crash-free sessions, privacy, abuse · ชั่วโมงทำงานของ Founder ต่อสัปดาห์
นิยามและสูตรอยู่ใน `ANALYTICS_PLAN.md`
