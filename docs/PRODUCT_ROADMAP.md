# KinNai — Product Roadmap (เดือน 0–24)

ที่มา: Blueprint V1.2 · ทำตาม stage gate เฉพาะ MVP1 ที่เข้า detailed specification (D-007) ระยะหลังจากนั้นเป็นทิศทาง ไม่ใช่คำมั่น

| ระยะ | ช่วง | ขอบเขต | Gate ที่ต้องผ่าน | สถานะ |
|---|---|---|---|---|
| MVP 1.0 Solo Decision | M0–3 | งบ/แนว/ระยะ · 3 ร้านแนะนำ · แผนที่และ decision event · Kampai clickable prototype | ทดลอง Time to Yes ≤ 30 วินาที | **อยู่ที่ Gate 1** |
| MVP 1.5 Group + Plus Beta | M4–6 | เชิญและความชอบของกลุ่ม · โหวตและสรุป · Plus entitlement beta | กลุ่มตัดสินใจ ≤ 5 นาที และมีผู้จ่ายเงินจริง | ยังไม่ spec |
| MVP 2.0 Growth | M7–12 | Kampai Premium · referral 3 เพื่อน · personalization · ทดสอบ retention/conversion | มีหลักฐานการใช้ซ้ำและ contribution | ยังไม่ spec |
| Growth 2.0 Monetization | M13–18 | Affiliate · sponsored result (ติดป้าย) · AI-assisted support | หลักฐาน breakeven รายเดือน | ยังไม่ spec |
| Scale 3.0 Automation | M19–24 | Merchant Pro candidate · self-service · ต้นทุนและการดำเนินงาน | ARPU, ภาระงาน, economics | ยังไม่ spec |

## Gates ภายใน MVP1

| Gate | สิ่งที่ Founder อนุมัติ | สถานะ |
|---|---|---|
| Gate 1 | PRD, user stories, AC, scope | รออนุมัติ: ดู `GATE1_DECISION_PACKAGE.md` |
| Gate 2 | สถาปัตยกรรม, dependency, ต้นทุน, test plan | ยังไม่เริ่ม |
| Gate 3 | Feature coverage และหลักฐานการทดสอบที่รันจริง | ยังไม่เริ่ม |
| Gate 4 | Beta, privacy, การส่ง store | ยังไม่เริ่ม |

## ลำดับงานของ MVP1

1. **Gate 1 → 2:** SPIKE-001 (Places provider), SPIKE-002 (PDPA), แผนสถาปัตยกรรมและต้นทุน
2. **Build:** PLAT-001/002, ANL-001 → PLAT-003, PRIV-001 → SOLO-001 ถึง 005
3. **Gate 3:** หลักฐาน test ครบทุก AC
4. **Gate 4:** Beta ในพื้นที่เปิดตัว เก็บข้อมูล TTY จริง

ไม่มีการกำหนดวันที่รายสัปดาห์ในขั้นนี้ เพราะขึ้นกับผล SPIKE-001 และเวลาของ Founder (H-005)

## KPI และ guardrails

North Star: Successful Food Decisions · Solo TTY median/P90 · Group completion ≤ 5 นาที · retention, paid conversion, ARPU
Guardrails: ความพึงพอใจและความตรงของคำแนะนำ · ต้นทุน API ต่อการตัดสินใจ · crash-free sessions, privacy, abuse · ชั่วโมงทำงานของ Founder ต่อสัปดาห์
นิยามและสูตรอยู่ใน `ANALYTICS_PLAN.md`
