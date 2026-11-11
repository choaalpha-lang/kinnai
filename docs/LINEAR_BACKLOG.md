# KinNai MVP1 — Backlog (พร้อม import เข้า Linear)

สถานะ: **ยังไม่ได้ sync กับ Linear** session นี้ไม่มี Linear tool คอลัมน์ "Linear ID" เว้นว่างจนกว่าจะสร้าง issue จริง ไฟล์ import: `LINEAR_IMPORT.csv`

ลำดับเรียงตาม dependency รายละเอียด AC อยู่ใน `REQUIREMENTS.md`

| # | Req ID | งาน | Priority | MoSCoW | ขึ้นกับ | Gate | Linear ID |
|---|---|---|---|---|---|---|---|
| 1 | SPIKE-001 | ประเมิน Places provider: ความครอบคลุม ข้อมูลราคา license ต้นทุน | Urgent | Must | — | ก่อน Gate 2 | |
| 2 | SPIKE-002 | ทบทวน PDPA: notice, ฐานทางกฎหมาย, retention, ผู้เยาว์, โอนข้ามประเทศ | High | Must | — | ก่อน Gate 4 | |
| 3 | PLAT-001 | App shell: Expo + TS, 3 แท็บ, i18n, tokens, CI, Sentry | High | Must | Gate 2 | Build | |
| 4 | PLAT-002 | Supabase schema, Auth providers (Apple, Google), RLS + RLS tests | High | Must | PLAT-001 | Build | |
| 4a | AUTH-001 | หน้า login Apple/Google, เชื่อมบัญชี, ออกจากระบบ, ลบบัญชี + ตั้งค่า credentials ของ Apple Developer และ Google Cloud | High | Must | PLAT-001, PLAT-002 | Build | |
| 5 | ANL-001 | Event instrumentation, schema test, dashboard | High | Must | PLAT-001 | Build | |
| 6 | PLAT-003 | Places adapter (Edge Function), rate limit, daily cap, kill switch | High | Must | SPIKE-001, PLAT-002 | Build | |
| 7 | PRIV-001 | หน้าอธิบายก่อนขอตำแหน่ง, privacy notice, consent log, ลบข้อมูล | High | Must | SPIKE-002, PLAT-002 | Build | |
| 8 | SOLO-001 | หน้าแรก: งบ แนว ระยะ ตำแหน่ง/ย่าน | High | Must | PLAT-001, PRIV-001, AUTH-001 | Build | |
| 9 | SOLO-002 | ผลแนะนำ ≤ 3 ร้าน + เหตุผล + 5 สถานะ | High | Must | SOLO-001, PLAT-003 | Build | |
| 10 | SOLO-003 | ยืนยันแบบ idempotent + TTY | High | Must | SOLO-002, PLAT-002, ANL-001 | Build | |
| 11 | SOLO-004 | เปิดแผนที่ภายนอก | Medium | Must | SOLO-003 | Build | |
| 12 | SOLO-005 | Feedback, ความชอบ, แท็บ "ฉัน" | Medium | Should | SOLO-003 | Build | |
| 13 | GRP-001 | Kampai prototype (HTML) | Low | Could | — | **ส่งมอบแล้ว** รอ Founder ยอมรับ | |
| 14 | REL-001 | เตรียม beta: TestFlight, Play internal testing, store privacy disclosure | Medium | Must | #3–#12 | Gate 4 | |
| 15 | OPS-001 | Runbook: kill switch, cost alert, แจ้งเหตุละเมิดข้อมูล | Medium | Must | PLAT-003, PRIV-001 | Gate 4 | |

## Critical path

SPIKE-001 → PLAT-003 → SOLO-002 → SOLO-003 → REL-001
SPIKE-002 → PRIV-001 → SOLO-001
PLAT-002 → AUTH-001 → SOLO-001 (ต้องมีบัญชี Apple Developer ก่อนจึงทดสอบ Sign in with Apple ได้)

งานที่ทำขนานได้หลัง Gate 2: PLAT-001/002 และ ANL-001 ไม่รอ SPIKE

## ไม่อยู่ใน backlog MVP1

GRP-002 (Kampai จริง), MON-001, MON-002, REF-001 อยู่ใน roadmap ระยะถัดไป ไม่เปิด issue จนกว่าจะผ่าน gate ของ MVP1
