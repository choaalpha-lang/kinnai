# KinNai — Business Model Canvas

ที่มา: Blueprint V1.2 และ 10X Strategy v1.0 · ราคา รายได้ และต้นทุนทั้งหมดเป็น**ข้อเสนอเพื่อทดลอง** ไม่ใช่ผลที่ยืนยันแล้ว
รูปแบบ (แก้ 2026-11-26 ตาม D-016, D-018): Group-first, Kampai ฟรีเป็น growth loop, รายได้ทดลองจาก verified intent และ workplace เป็น pilot นอกผลิตภัณฑ์ ไม่ใช่ freemium subscription

| ช่อง | เนื้อหา | สถานะหลักฐาน |
|---|---|---|
| Customer Segments | Group organizers · Group participants (ทีมงาน นักศึกษา ครอบครัว) · Solo diners (รอง) · ผู้ซื้อในอนาคต: แบรนด์ (Intent Exchange), องค์กร (Workplace) | สมมติฐาน ยังไม่มี user research ใน repo |
| Value Proposition | Solo: ได้ร้านใน 30 วินาที · Group: สรุปใน 5 นาที · คำนึงถึงงบ สถานที่ ความชอบ | H-001, H-002, H-004 |
| Channels | Kampai Link ที่ผู้ใช้แชร์เข้ากลุ่มแชต (growth loop หลัก) · referral ใน 2–3 เครือข่ายที่ไม่ทับซ้อน (D-022) · หลังผล web beta: App Store / Google Play · content | H-007 ยังไม่ทดสอบ |
| Customer Relationships | Self-service · คำแนะนำเฉพาะบุคคล · automated support | — |
| Revenue Streams | Kampai ฟรี · Intent Exchange: ขาย verified outcome ให้แบรนด์ (฿40 ต่อ outcome เป็นสมมติฐานของ strategy) · Workplace: องค์กรจ่ายรายเดือน (฿5,000 ต่อองค์กรต่อเดือน สมมติฐาน) · ทั้งสองเริ่มเป็น pilot manual นอกผลิตภัณฑ์ (วัน 61–90) · **ถอดแล้ว:** Plus ฿99, Kampai Premium ฿39, referral credit (D-018) | H-010, H-011 ยังไม่มีหลักฐาน ไม่มีรายได้ใน MVP1 |
| Key Resources | แอป, ตรรกะแนะนำ, ข้อมูลและ analytics, Founder, AI tools, แบรนด์ | — |
| Key Activities | Ship และทดสอบฟีเจอร์ · ปรับ Time to Yes · ติดตาม retention และ unit economics | — |
| Key Partners | Places/maps data provider (ยังไม่เลือก Q-001) · cloud, app stores, billing | Q-001 เปิดอยู่ |
| Cost Structure | Cloud, maps, AI usage · ค่า store และ payment · growth, support, security | ตัวเลข UNKNOWN รอ SPIKE-001 และ Gate 2 |

## Growth loop (MVP1) — Kampai Protocol

เปิด Kampai Link → ผู้ร่วมกลุ่มโหวตบนเว็บโดยไม่ต้องลงแอปและไม่ต้องสมัคร → ระบบตัดสินด้วยงบ ย่าน และความชอบจริง → แชร์ผลกลับเข้ากลุ่ม → สมาชิกบางคนนำไปจัดกลุ่มของตัวเอง

K = I × A × H (invitations × join rate × new-organizer rate) ตัวอย่างใน strategy: 6 × 60% × 40% = 1.44 เป็นตัวอย่างสมมติ ไม่ใช่ผลที่วัดได้ นิยามและเงื่อนไขการนับอยู่ใน `ANALYTICS_PLAN.md` §3
Kill criterion ของ strategy: K ต่ำกว่า 1 อย่างต่อเนื่อง หรือผู้จัดไม่กลับมาจัดรอบใหม่

## ข้อสังเกตต่อโมเดล

1. MVP1 พิสูจน์ได้เฉพาะ growth loop และการใช้ซ้ำ ไม่พิสูจน์ความยินดีจ่าย รายได้ทั้งสองสายต้องรอ pilot วัน 61–90
2. ต้นทุนผันแปรหลักคือค่า Places API ต่อการค้นหา เกิดกับทุกกลุ่มที่สร้าง ต้องคุมด้วย 1 การค้นหาต่อกลุ่มและ daily cap (R-014)
3. Intent Exchange ขัดกับคำสัญญา "ร้านที่ตรงที่สุด" ได้ ต้องแยก Sponsored ออกจาก Organic เสมอ ห้ามซื้ออันดับผลโหวต และไม่ขายข้อมูลส่วนบุคคล
4. Workplace ไม่เหมาะถ้าต้อง integrate ระบบรายองค์กรหรือมี sales engineer ทุกดีล (kill criterion ของ strategy)
5. เป้า 10X เป็นทิศทาง ไม่ใช่เงื่อนไข (D-021)
