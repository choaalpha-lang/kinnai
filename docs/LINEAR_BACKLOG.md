# KinNai MVP1 — Backlog (พร้อม import เข้า Linear)

สถานะ: **ยังไม่ได้ sync กับ Linear** session นี้ไม่มี Linear tool คอลัมน์ "Linear ID" เว้นว่างจนกว่าจะสร้าง issue จริง ไฟล์ import: `LINEAR_IMPORT.csv`

ลำดับเรียงตาม dependency และความสำคัญตาม D-016 (Kampai-first) ปรับ 2026-11-26 เป็น 25 รายการ รายละเอียด AC อยู่ใน `REQUIREMENTS.md`

| # | Req ID | งาน | Priority | MoSCoW | ขึ้นกับ | Gate | Linear ID |
|---|---|---|---|---|---|---|---|
| 1 | SPIKE-001 | ประเมิน Places provider: ความครอบคลุม ข้อมูลราคา license ต้นทุน (desk research เสร็จ 2026-11-25 ดู `SPIKE-001_PLACES_PROVIDER.md` เหลือส่วนทดสอบข้อมูลจริง) ต้องรองรับต้นทุนต่อกลุ่ม Kampai | Urgent | Must | — | ก่อน Gate 2 | |
| 2 | SPIKE-002 | ทบทวน PDPA: notice, ฐานทางกฎหมาย, retention, ผู้เยาว์, โอนข้ามประเทศ + **ขยาย:** intent data แบบรวม และการนับผู้ใช้ไม่ซ้ำโดยไม่มีบัญชี (Q-018, F-09) | High | Must | — | ก่อน Gate 4 | |
| 3 | PLAT-001 | Web app shell: Next.js + TS, mobile-first, PWA manifest, bottom nav, i18n, tokens, CI + e2e smoke, Sentry | High | Must | Gate 2 | Build | |
| 4 | PLAT-002 | Supabase schema (รวม groups, group_members, group_votes, intent_records), anonymous sign-in, Auth providers (Apple, Google), RLS + RLS tests | High | Must | PLAT-001 | Build | |
| 5 | ANL-001 | Event instrumentation, schema test, dashboard | High | Must | PLAT-001 | Build | |
| 6 | ANL-002 | ตัวชี้วัดแบบ strategy: K (I × A × H), organizer repeat, saturation, completion ใน 5 นาที พร้อมช่วงความเชื่อมั่น | High | Must | ANL-001, GRP-002 | Build | |
| 7 | PLAT-003 | Places adapter (Edge Function), rate limit, daily cap, kill switch, 1 การค้นหาต่อกลุ่ม | High | Must | SPIKE-001, PLAT-002 | Build | |
| 8 | PRIV-001 | หน้าอธิบายก่อนขอตำแหน่ง, privacy notice, consent log, ลบข้อมูล, ลบการเข้าร่วมของผู้ร่วมกลุ่ม | High | Must | SPIKE-002, PLAT-002 | Build | |
| 9 | GRP-003 | ผู้ร่วมกลุ่มโดยไม่ต้อง login (anonymous session, ชื่อเล่นไม่บังคับ, ใช้ได้ใน LINE in-app browser) | High | Must | PLAT-002, PRIV-001 | Build | |
| 10 | GRP-002 | Kampai บนเว็บ: สร้างกลุ่ม ลิงก์ โหวต ปิดโหวต เสมอผู้จัดเลือก งดออกเสียง | High | Must | PLAT-001, PLAT-002, PLAT-003, SOLO-002, GRP-003 | Build | |
| 11 | GRP-004 | การ์ดสรุปที่แชร์กลับเข้ากลุ่ม (Web Share, ตัวอย่างลิงก์, ปุ่มจัดนัดของคุณเอง) | High | Must | GRP-002, SOLO-004 | Build | |
| 12 | GRP-005 | กติกาเวลาและเงื่อนไขการปิดโหวต | Medium | Should | GRP-002 | Build | |
| 13 | SOLO-002 | ผลแนะนำ ≤ 3 ร้าน + เหตุผล + 5 สถานะ (ระบบแนะนำที่ใช้ร่วมกับ Kampai) | High | Must | SOLO-001, PLAT-003 | Build | |
| 14 | SOLO-004 | เปิดแผนที่ภายนอก (ใช้ร่วมกับ Kampai) | Medium | Must | SOLO-002 | Build | |
| 15 | SOLO-001 | หน้าแรก Solo: งบ แนว ระยะ ตำแหน่ง/ย่าน | Medium | Should | PLAT-001, PRIV-001 | Build | |
| 16 | SOLO-003 | ยืนยันแบบ idempotent + TTY (ไม่ต้อง login) | Medium | Should | SOLO-002, PLAT-002, ANL-001 | Build | |
| 17 | SOLO-005 | Feedback, ความชอบ, เมนู "ฉัน" | Low | Should | SOLO-003 | Build | |
| 18 | AUTH-001 | Login Apple/Google หลังตัดสินใจสำเร็จ (ข้ามได้), ผูก anonymous เข้าบัญชี, เชื่อมบัญชี, ออกจากระบบ, ลบบัญชี + ตั้งค่า credentials ของ Apple Developer และ Google Cloud | Medium | Must | PLAT-001, PLAT-002 | Build | |
| 19 | INT-001 | โครงข้อมูล intent ของกลุ่มแบบรวมไม่ระบุตัวบุคคล (ไม่ขาย ไม่แสดง offer) | Medium | Should | GRP-002, SPIKE-002 | Build | |
| 20 | BRAND-001 | ชุดแบรนด์ตาม strategy: tokens, โลโก้ K + Fork เป็น vector, ไอคอน PWA | Medium | Should | PLAT-001 | Build | |
| 21 | GRP-001 | Kampai prototype (HTML) แบบอ้างอิงของ GRP-002 | Low | Could | — | **ส่งมอบแล้ว** (ต้นแบบยังไม่ปรับตาม D-016 ถึง D-022) | |
| 22 | REL-001 | เปิด web beta: โดเมน production, hosting, privacy policy, หน้าลบบัญชี, consent banner, ทดสอบบน LINE in-app browser | Medium | Must | ทุกรายการ Must | Gate 4 | |
| 23 | OPS-001 | Runbook: kill switch, cost alert, แจ้งเหตุละเมิดข้อมูล | Medium | Must | PLAT-003, PRIV-001 | Gate 4 | |
| 24 | UXR-001 | แผนทดสอบผู้ใช้จริง: 2–3 เครือข่ายที่ไม่ทับซ้อน (D-022), จำนวน, สคริปต์, เกณฑ์ผ่านของ H-007 ถึง H-009 และ H-001 | Medium | Should | — | ก่อน Gate 4 | |
| 25 | WPL-001 | ชุด pilot องค์กร: กลุ่มประจำ, งบต่อหัวล่วงหน้า, ส่งออก CSV ไม่มี integration (เริ่มหลัง gate วัน 60) | Low | Could | GRP-002, ผ่าน gate D-020 | หลัง Gate 4 | |

## Critical path

SPIKE-001 → PLAT-003 → SOLO-002 → GRP-002 → GRP-004 → ANL-002 → REL-001
SPIKE-002 → PRIV-001 → GRP-003 → GRP-002
PLAT-002 → GRP-003 / AUTH-001 (AUTH-001 ไม่บล็อก Kampai ต้องมีบัญชี Apple Developer ก่อนจึงทดสอบ Sign in with Apple ได้)

งานที่ทำขนานได้หลัง Gate 2: PLAT-001/002 และ ANL-001 ไม่รอ SPIKE

## ไม่อยู่ใน backlog MVP1

**ถอดออกแล้ว (D-018):** MON-001 (Plus), MON-002 (Kampai Premium), REF-001 (referral credit)
ระบบ Intent Exchange (offer ในผลิตภัณฑ์), Workplace ที่ต้อง integrate, การชำระเงินทุกชนิด ยังไม่เปิด issue จนกว่าจะผ่าน gate วัน 61–90
