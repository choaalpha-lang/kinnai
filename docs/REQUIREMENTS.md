# KinNai MVP1 — Requirements

ขอบเขต: MVP1 (M0–3) เท่านั้น · แก้ไขล่าสุด 2026-11-26: ปรับตาม 10X Strategy (D-016 ถึง D-022) · 2026-11-11: เพิ่ม AUTH-001 · ต้นแบบอ้างอิง: `prototype/index.html` · การตัดสินใจอ้างอิง: `docs/DECISION_LOG.md`

ทุก requirement ใช้ Definition of Done ร่วมกัน (§9) Event ทั้งหมดนิยามใน `docs/ANALYTICS_PLAN.md`

| ID | Capability | MoSCoW | ขึ้นกับ |
|---|---|---|---|
| AUTH-001 | เข้าสู่ระบบด้วย Apple หรือ Google (ขอหลังตัดสินใจสำเร็จ ข้ามได้) และเชื่อมบัญชี | Must | PLAT-001, PLAT-002 (ไม่บล็อก Kampai) |
| SOLO-001 | เลือกงบ ประเภทอาหาร และระยะ | Should | PLAT-001, PRIV-001 |
| SOLO-002 | แนะนำไม่เกิน 3 ร้านพร้อมเหตุผล | Must | SOLO-001, PLAT-003 |
| SOLO-003 | ยืนยันการตัดสินใจและบันทึก Time to Yes | Should | SOLO-002, PLAT-002, ANL-001 |
| SOLO-004 | เปิดแผนที่นำทางภายนอก | Must | SOLO-003 |
| SOLO-005 | Feedback และการจำความชอบ | Should | SOLO-003, PLAT-002 |
| GRP-001 | Kampai clickable prototype (ข้อมูลจำลอง) ส่งมอบแล้ว ใช้เป็นแบบอ้างอิงของ GRP-002 | Could | ไม่มี |
| GRP-002 | Kampai บนเว็บ: สร้างกลุ่ม โหวต และสรุป | Must | PLAT-001, PLAT-002, PLAT-003, SOLO-002, GRP-003 |
| GRP-003 | ผู้ร่วมกลุ่มโดยไม่ต้อง login (anonymous session) | Must | PLAT-002, PRIV-001 |
| GRP-004 | การ์ดสรุปที่แชร์กลับเข้ากลุ่ม | Must | GRP-002, SOLO-004 |
| GRP-005 | กติกาเวลาและเงื่อนไขการปิดโหวต | Should | GRP-002 |
| PLAT-001 | Web app shell (mobile-first), i18n, design tokens, CI | Must | Gate 2 |
| PLAT-002 | Supabase schema, anonymous user, Auth (Apple, Google), RLS | Must | PLAT-001 |
| PLAT-003 | Places adapter ฝั่ง server, rate limit, cost cap | Must | SPIKE-001, PLAT-002 |
| PRIV-001 | Consent ตำแหน่ง, privacy notice, ลบข้อมูล | Must | SPIKE-002, PLAT-002 |
| ANL-001 | Event instrumentation และ dashboard | Must | PLAT-001 |
| ANL-002 | ตัวชี้วัดแบบ strategy: K, organizer repeat, saturation | Must | ANL-001, GRP-002 |
| INT-001 | โครงข้อมูล intent ของกลุ่ม (ไม่ขาย ไม่แสดง offer) | Should | GRP-002, SPIKE-002 |
| BRAND-001 | ไอคอน PWA และโลโก้ (ใช้ชุดสีเดิมของแอป D-019) | Should | PLAT-001 |
| WPL-001 | ชุด pilot สำหรับองค์กร (เริ่มหลัง gate วัน 60) | Could | GRP-002, ผ่าน gate D-020 |

**ลำดับความสำคัญตาม D-016:** Kampai (GRP-002 ถึง 005) มาก่อน ส่วน Solo ใช้ระบบแนะนำร่วม (SOLO-002 และ SOLO-004 ยังเป็น Must เพราะใช้ร่วมกับ Kampai) SOLO-001, 003, 005 เป็น Should MON-001, MON-002 และ REF-001 ถูกถอดออก (D-018)

---

## AUTH-001 — เข้าสู่ระบบด้วย Apple หรือ Google (ขอหลังตัดสินใจสำเร็จ) และเชื่อมบัญชี

**User story:** ในฐานะผู้จัดหรือผู้ใช้ Solo ฉันอยากใช้งานได้ทันทีโดยไม่ต้องสมัคร แล้วเลือกบันทึกประวัติและกลุ่มของฉันไว้กับบัญชีที่มีอยู่แล้วหลังตัดสินใจสำเร็จ ด้วยการแตะไม่เกิน 2 ครั้ง

**Business value:** ไม่ขวางการแพร่กระจายของ Kampai (D-017) และให้ตัวตนถาวรสำหรับวัด organizer repeat การ sync ข้ามเครื่อง และ pilot ภายหลัง แทนที่ D-011

**Acceptance criteria**

1. Given ผู้ใช้ใหม่ (ผู้จัด Kampai หรือ Solo) When เปิดแอปและใช้งานจนตัดสินใจสำเร็จ Then ทำได้ครบโดยไม่เห็นหน้า login (ใช้ Supabase anonymous user)
2. Given ผู้ร่วมกลุ่มที่เปิดลิงก์ Kampai When เข้าร่วมและโหวต Then ไม่เห็นการขอ login ในทุกขั้นตอน
3. Given ผู้จัดหรือผู้ใช้ Solo ตัดสินใจสำเร็จแล้วและยังไม่ login When หน้าผลสรุปแสดง Then เห็นข้อเสนอ "บันทึกไว้จัดครั้งหน้า" พร้อมปุ่ม "ดำเนินการต่อด้วย Apple" และ "ดำเนินการต่อด้วย Google" (iOS แสดง Apple ก่อน Android แสดง Google ก่อน) และปุ่ม "ไว้ทีหลัง" โดยไม่บล็อกการดูผลหรือการเปิดแผนที่
4. Given ผู้ใช้แตะช่องทางหนึ่งและยืนยันในหน้า OAuth When สำเร็จ Then ข้อมูลที่สร้างขณะเป็น anonymous (ความชอบ ประวัติ กลุ่มที่เป็นผู้จัด) ผูกเข้ากับบัญชีเดียวกันโดยไม่หาย รวมไม่เกิน 2 แตะ ไม่มีฟอร์มหรือหน้าตั้งโปรไฟล์ (ความสามารถ linkIdentity ของ Supabase ต้องยืนยันใน Gate 2)
5. Given ผู้ใช้กด "ไว้ทีหลัง" When ผ่านไป Then ไม่ขอซ้ำภายใน 7 วัน (A-007) แต่เข้าสู่ระบบได้ตลอดจากเมนู "ฉัน"
6. Given ผู้ใช้ยกเลิกหรือ login ล้มเหลว When กลับมาที่แอป Then อยู่หน้าผลเดิม เห็นข้อความบอกสาเหตุ เลือกช่องทางอื่นหรือข้ามได้ โดยไม่เสียผลที่ตัดสินไว้
7. Given ผู้ใช้ login ด้วยช่องทางหนึ่ง When กด "เชื่อมบัญชี" อีกช่องทางในเมนู "ฉัน" และยืนยัน Then เข้าสู่ระบบด้วยช่องทางใดก็ได้แล้วเห็นข้อมูลชุดเดียวกัน (D-012)
8. Given ช่องทางที่จะเชื่อมผูกกับบัญชี KinNai อื่นอยู่แล้ว When พยายามเชื่อม Then แจ้งให้ทราบและไม่รวมข้อมูลอัตโนมัติ
9. Given เหลือช่องทางเดียว When ดูรายการบัญชีที่เชื่อม Then ไม่มีปุ่มยกเลิกการเชื่อมของช่องทางนั้น
10. Given ผู้ใช้ (รวมผู้ใช้ anonymous) กด "ลบบัญชีและข้อมูล" และยืนยัน When สำเร็จ Then ข้อมูลทั้งหมดของผู้ใช้ถูกลบ token ของ Apple ถูก revoke (ถ้ามี) และผู้ใช้กลับสู่สถานะใหม่
11. Given ผู้ใช้กด "ออกจากระบบ" When เข้าสู่ระบบใหม่ด้วยช่องทางเดิม Then ความชอบและประวัติยังอยู่

**Edge cases:** ไม่มีเครือข่ายตอน login (แจ้งและให้ลองใหม่) · ผู้ใช้ Apple เลือกซ่อนอีเมล (ต้องทำงานได้ปกติ) · ผู้ใช้ถอนสิทธิ์ KinNai จากหน้าตั้งค่าของ Apple/Google · refresh token หมดอายุ · Android ที่ไม่มีบัญชี Google ในเครื่อง · **ผู้ใช้ anonymous ที่ยังไม่ผูกบัญชีล้างข้อมูลเบราว์เซอร์** (ข้อมูลฝั่งผู้ใช้เข้าถึงไม่ได้ และนับเป็นผู้ใช้ใหม่ R-007 หน้าผลสรุปต้องบอกเหตุผลที่ควรบันทึก) · **เปิดใน in-app browser (LINE)** แล้วเปลี่ยนไป Safari/Chrome เพื่อ login: เบราว์เซอร์ใหม่เป็น anonymous session ใหม่ ข้อมูลที่สร้างในเบราว์เซอร์เดิมไม่ตามไป ต้องบอกผู้ใช้ก่อนและให้เลือกทำต่อในเบราว์เซอร์เดิมได้ (R-008)

**Security / privacy:** OAuth แบบ authorization code + PKCE ผ่าน Supabase Auth · Sign in with Apple บนเว็บต้องมี Services ID, key และการยืนยันโดเมน จึงต้องมี Apple Developer Program (D-014) · ขอ scope ต่ำสุด ไม่ขออีเมลถ้าไม่จำเป็น (Q-009, PDPA ม.22) · session ใช้กลไกของ Supabase client ตั้ง cookie แบบ Secure/SameSite ถ้าใช้ server rendering · การรับทราบ privacy notice อยู่ในหน้าขอ login แต่ consent ตำแหน่งและ analytics ขอแยก (ม.19) · ลบบัญชีได้ในเมนู "ฉัน" ตามสิทธิ PDPA (ม.33) · ข้อกำหนด App Store 4.8 และ 5.1.1(v) ใช้เมื่อขึ้น store (D-013) · anonymous user ยังเป็น identifier ที่ต้องอยู่ใต้ privacy notice (R-012)

**ข้อจำกัดบนเว็บ:** ถ้าตรวจพบ in-app browser (เช่น LINE) ที่หน้าเสนอ login ให้แสดงข้อความและปุ่ม "เปิดใน Safari/Chrome" เพราะ Google ไม่อนุญาต OAuth ใน embedded webview (R-008) · Redirect URI ต้องเป็น HTTPS บนโดเมนจริง

**Analytics:** `auth_screen_shown` (trigger: post_decision_solo / post_decision_group / me_tab), `auth_dismissed`, `auth_method_selected`, `auth_completed` (method, is_new_user), `auth_cancelled`, `auth_failed` (reason), `identity_linked`, `identity_unlinked`, `signed_out`, `account_deleted`

**Tests:** unit (ลำดับปุ่มตาม platform, กติกาช่องทางสุดท้าย, กฎไม่ขอซ้ำ 7 วัน) · integration (anonymous → link identity แล้วข้อมูลคงอยู่, RLS หลัง link) · manual บนอุปกรณ์จริงทั้งสองค่าย รวมกรณียกเลิก ลบบัญชี และเปิดจาก LINE

**ความเสี่ยงที่รู้แล้ว:** R-007 (ไม่ login จนวัด repeat ไม่ได้), R-008 (OAuth ใน in-app browser) R-006 (App Store review) ยังไม่เกิดจนกว่าจะทำ native

---

## SOLO-001 — เลือกงบ ประเภทอาหาร และระยะ

**User story:** ในฐานะคนที่หิวและยังไม่รู้จะกินอะไร ฉันอยากบอกงบ แนวอาหาร และระยะที่ไปไหว ด้วยการแตะไม่กี่ครั้ง เพื่อให้แอปเลือกร้านให้

**Business value:** เป็น input ขั้นต่ำของการแนะนำ และเป็นจุดเริ่มนาฬิกา TTY (D-001)

**Acceptance criteria**

1. Given ผู้ใช้เปิดแอปครั้งแรก (ยังไม่ต้องเข้าสู่ระบบ) When หน้าแรกแสดง Then เห็นตัวเลือกงบ 4 ระดับ (≤฿100, ฿101–200, ฿201–400, ฿400+), ประเภทอาหาร (รวม "อะไรก็ได้") และระยะ (500 ม., 1, 2, 5 กม.) โดยมีค่าเริ่มต้นเลือกไว้แล้ว
2. Given ผู้ใช้เคยใช้งานแล้ว When เปิดแอปอีกครั้ง Then ค่าที่เลือกครั้งก่อนถูกเลือกไว้ให้
3. Given ผู้ใช้เลือกงบระดับใดระดับหนึ่ง When ระบบค้นหา Then ร้านที่ราคาต่ำกว่าเพดานของระดับนั้นทั้งหมดเข้าเกณฑ์ (D-002) และร้านที่อยู่ในช่วงของระดับนั้นได้คะแนนจัดอันดับเพิ่ม
4. Given ผู้ใช้อนุญาตตำแหน่ง When กด "ช่วยเลือกให้หน่อย" Then ระบบใช้ตำแหน่งปัจจุบันเป็นจุดศูนย์กลาง และส่ง event `criteria_completed`
5. Given ผู้ใช้ปฏิเสธตำแหน่ง When หน้าแรกแสดง Then เห็นตัวเลือกย่านให้เลือกเอง และปุ่มค้นหาใช้ไม่ได้จนกว่าจะเลือกย่าน
6. Given ผู้ใช้เปลี่ยนภาษาเป็น EN When หน้าแรกแสดง Then ข้อความทั้งหมดเป็นภาษาอังกฤษ

**Edge cases:** สิทธิ์ตำแหน่งถูกถอนระหว่างใช้งาน · GPS ไม่แม่น (accuracy > 500 ม.) ให้แจ้งและเสนอเลือกย่าน · อยู่นอกพื้นที่ให้บริการ (Q-005) · ผู้ใช้กดค้นหาซ้ำเร็ว ๆ ต้องไม่ยิงคำขอซ้อน

**Security / privacy:** ขอสิทธิ์ตำแหน่งแบบ "ขณะใช้แอป" เท่านั้น ไม่ขอ background · ไม่บันทึกพิกัดดิบ (A-005) · ความชอบเก็บใต้ RLS `user_id = auth.uid()`

**Analytics:** `session_start`, `preference_set`, `location_permission`, `area_selected`, `criteria_completed`

**Tests:** unit (ตรรกะเพดานงบ, ค่าเริ่มต้น) · component (สถานะ granted/denied) · E2E (เลือกครบแล้วค้นหา ≤ 3 แตะเมื่อใช้ค่าเริ่มต้น)

---

## SOLO-002 — แนะนำไม่เกิน 3 ร้านพร้อมเหตุผล

**User story:** ในฐานะคนที่ไม่อยากเลื่อนดูรายการยาว ฉันอยากได้ร้านไม่เกิน 3 ร้านพร้อมเหตุผลสั้น ๆ เพื่อเลือกได้ทันที

**Acceptance criteria**

1. Given มีร้านเข้าเกณฑ์ ≥ 3 When ผลแสดง Then เห็น 3 ร้านเรียงตามคะแนน ร้านแรกมีป้าย "ตรงที่สุด" แต่ละร้านแสดงชื่อ ราคาโดยประมาณ ระยะ คะแนน และเหตุผลอย่างน้อย 2 ข้อ
2. Given มีร้านเข้าเกณฑ์ 1–2 ร้าน When ผลแสดง Then แสดงเท่าที่มี พร้อมข้อความว่าเจอกี่ร้านและปุ่มขยายเงื่อนไข
3. Given ไม่มีร้านเข้าเกณฑ์ When ผลแสดง Then เห็น empty state ที่บอกวิธีแก้ และปุ่ม "ขยายเงื่อนไขแล้วหาใหม่" ซึ่งขยายระยะหนึ่งขั้น หรือเปลี่ยนเป็น "อะไรก็ได้" เมื่อระยะสูงสุดแล้ว
4. Given ผู้ใช้กด "ขอชุดใหม่" When ผลแสดง Then ไม่ซ้ำร้านที่เห็นแล้วใน session นี้ และเมื่อหมดให้แจ้งว่าวนกลับชุดแรก
5. Given provider ไม่ตอบภายใน timeout หรือ error When กำลังโหลด Then เห็น error state ที่บอกว่าเกิดอะไรและปุ่มลองอีกครั้ง โดยเงื่อนไขที่เลือกไว้ไม่หาย
6. Given ผู้ใช้เคยกด 👎 ร้านใด When ค้นหา Then ร้านนั้นไม่ถูกแนะนำ
7. Given คำขอใช้เวลานาน When กำลังโหลด Then แสดง skeleton และหลัง 2 วินาทีแสดงข้อความว่าช้ากว่าปกติ

**Edge cases:** ร้านปิดอยู่ ณ เวลาค้นหา (ต้องกรองออกถ้า provider มีข้อมูลเวลาเปิด) · ข้อมูลราคาไม่มี (R-001) ต้องแสดงว่า "ไม่ทราบราคา" ไม่เดา · ร้านซ้ำจาก provider · ถึง daily cost cap ให้แสดง error state แบบเดียวกับข้อ 5

**Security:** เรียก provider ผ่าน Edge Function เท่านั้น API key ไม่อยู่ในแอป · rate limit ต่อผู้ใช้ (ค่าเริ่มต้นเสนอ 20 คำขอ/ชั่วโมง) · แสดง attribution ตามเงื่อนไข provider

**Analytics:** `recommendation_requested`, `shortlist_shown` (count, ids), `shortlist_rerolled`, `filters_relaxed`, `recommendation_failed` (reason)

**Tests:** unit (การจัดอันดับ, ไม่ซ้ำ, ตัดร้านที่ 👎) · contract test กับ adapter ด้วย fixture · E2E ครบ 5 สถานะ (ปกติ, 1 ร้าน, 0 ร้าน, ช้า, error)

---

## SOLO-003 — ยืนยันการตัดสินใจและบันทึก Time to Yes

**User story:** ในฐานะผู้ใช้ ฉันอยากกดยืนยันร้านที่เลือกครั้งเดียวแล้วจบ เพื่อไปกินได้เลย

**Business value:** สร้างส่วน Solo ของ North Star "Successful Decisions" (D-020) ซึ่งนับเฉพาะการยืนยันจริง ไม่ใช่แค่ดูผล

**Acceptance criteria**

1. Given ผลแสดงอยู่ (ผู้ใช้จะ login หรือไม่ก็ได้ D-017) When ผู้ใช้กด "กินร้านนี้" Then ระบบบันทึก decision 1 รายการใต้ผู้ใช้ anonymous หรือบัญชีที่ login แล้ว และแสดงหน้ายืนยันพร้อมเวลา TTY และข้อเสนอ AUTH-001 ข้อ 3 (ไม่อยู่ในช่วงเวลา TTY)
2. Given D-001 และ A-001 When คำนวณ TTY Then TTY = เวลาของ `decision_confirmed` − เวลาของ `criteria_completed` ครั้งแรกใน session เดียวกัน
3. Given คำขอยืนยันถูกส่งซ้ำด้วย `decision_id` เดิม (กดซ้ำ, retry เครือข่าย) When server รับ Then มี decision เพียง 1 รายการ และตอบสำเร็จเหมือนครั้งแรก
4. Given เครือข่ายหลุดขณะยืนยัน When กลับมาออนไลน์ Then แอปส่งซ้ำอัตโนมัติด้วย `decision_id` เดิม และผู้ใช้เห็นหน้ายืนยันทันทีโดยไม่ต้องรอ
5. Given ผู้ใช้ออกจากผลโดยไม่ยืนยัน When เริ่ม session ใหม่หรือไม่มี event เกิน 10 นาที Then ระบบบันทึก `session_abandoned`

**Edge cases:** นาฬิกาเครื่องผิด (ใช้ monotonic clock ฝั่ง client คำนวณช่วงเวลา และเก็บ server timestamp คู่กัน) · แอปถูกพับระหว่างตัดสินใจ (เวลาที่พับนับรวม และติดธง `backgrounded`) · ยืนยันสองร้านใน session เดียว (ไม่อนุญาต ต้องเริ่มมื้อใหม่)

**Security:** `decision_id` เป็น UUID สร้างฝั่ง client · unique constraint ที่ `(user_id, session_id)` · RLS insert/select เฉพาะเจ้าของ

**Analytics:** `decision_confirmed` (decision_id, time_to_yes_ms, within_target, rerolls, backgrounded), `session_abandoned`

**Tests:** unit (สูตร TTY, A-001) · integration (idempotency กับฐานข้อมูลจริงใน test project) · E2E offline แล้ว retry

---

## SOLO-004 — เปิดแผนที่นำทางภายนอก

**User story:** หลังยืนยันร้าน ฉันอยากเปิดเส้นทางในแอปแผนที่ที่ใช้อยู่ด้วยการแตะครั้งเดียว

**Acceptance criteria**

1. Given หน้ายืนยันแสดง When กด "เปิดแผนที่นำทาง" Then เปิดแอปแผนที่ภายนอกที่ปลายทางเป็นร้านนั้น
2. Given เครื่องไม่มีแอปแผนที่ When กดปุ่ม Then เปิดแผนที่ในเบราว์เซอร์แทน
3. Given เปิดแผนที่ไม่สำเร็จ When กดปุ่ม Then แสดงชื่อและที่อยู่ร้านให้คัดลอกได้

**Edge cases:** ร้านไม่มีพิกัด (ใช้ชื่อ + ย่านค้นหาแทน) · iOS กับ Android ใช้ URL scheme ต่างกัน (บนเว็บใช้ https URL แบบ universal ของ Google Maps ที่ระบบเปิดแอปแผนที่ให้ถ้ามี ไม่ใช้ custom scheme)

**Security:** ส่งเฉพาะปลายทาง ไม่ส่ง user identifier ใน URL

**Analytics:** `map_opened` (decision_id, provider)

**Tests:** unit (สร้าง URL) · manual บนอุปกรณ์จริงทั้ง iOS และ Android

---

## SOLO-005 — Feedback และการจำความชอบ

**User story:** ฉันอยากบอกว่าร้านที่แนะนำตรงใจไหม เพื่อให้ครั้งหน้าแนะนำได้ดีขึ้น

**Acceptance criteria**

1. Given หน้ายืนยันแสดง When กด 👍 Then ร้านนั้นได้คะแนนจัดอันดับเพิ่มในครั้งต่อไป และเห็นข้อความยืนยันการบันทึก
2. Given กด 👎 When เลือกเหตุผล (ไกลไป, แพงไป, ไม่อยากกินแนวนี้) หรือไม่เลือก Then ร้านนั้นไม่ถูกแนะนำอีก
3. Given ผู้ใช้เปลี่ยนใจ When กดอีกฝั่ง Then ค่าล่าสุดแทนค่าเดิม
4. Given ผู้ใช้เปิดแท็บ "ฉัน" When ดูความชอบ Then เห็นร้านที่ถูกใจ ร้านที่ซ่อน และประวัติล่าสุด และลบข้อมูลทั้งหมดได้ (ดู PRIV-001)

**Edge cases:** Feedback โดยไม่ออนไลน์ (เข้าคิวส่งภายหลัง) · ร้านที่ซ่อนทั้งหมดจนไม่เหลือผล (เสนอให้ล้างรายการซ่อน)

**Security:** RLS เฉพาะเจ้าของ · ไม่มี free-text ใน MVP1 เพื่อเลี่ยงข้อมูลส่วนบุคคลที่ไม่ตั้งใจเก็บ

**Analytics:** `feedback_submitted` (decision_id, rating), `feedback_reason`

**Tests:** unit (ผลต่อการจัดอันดับ) · integration (RLS: ผู้ใช้ A อ่านของ B ไม่ได้)

---

## GRP-001 — Kampai clickable prototype

**User story:** ในฐานะ Founder ฉันอยากเห็นและให้คนลอง flow กลุ่มด้วยข้อมูลจำลอง เพื่อตัดสินว่าควรลงทุนสร้างจริง (ตัดสินแล้วใน D-016) และใช้เป็นแบบอ้างอิงของ GRP-002

**สถานะ:** ส่งมอบแล้วใน `prototype/index.html` (แท็บ Kampai)

**Acceptance criteria**

1. Given เปิดแท็บ Kampai When ทำตาม 5 ขั้น (สร้าง, ชวน, จับคู่, โหวต, ยืนยัน) Then จบที่ร้านเดียวพร้อม Group TTY
2. Given โหวตเสมอ When ปิดโหวต Then organizer เลือกจากร้านที่เสมอ (D-003)
3. Given สมาชิกไม่ตอบ When ปิดโหวต Then นับเป็นงดออกเสียงและบันทึกจำนวนใน event (D-004)
4. Given อยู่ในขั้นใดก็ตาม When ดูหน้าจอ Then มีป้ายบอกชัดว่าเป็นข้อมูลจำลอง
5. Given ยกเลิกกลางทาง When กด "ยกเลิกนัดนี้" Then บันทึก `group_cancelled` พร้อมขั้นที่ยกเลิก

**ไม่รวม:** ลิงก์เชิญจริง, realtime, บัญชีผู้ใช้หลายคน, LINE share จริง (ทั้งหมดอยู่ใน GRP-002 ถึง 005 ของ MVP1 ตาม D-016)

---

## GRP-002 — Kampai บนเว็บ: สร้างกลุ่ม โหวต และสรุป

**User story:** ในฐานะผู้จัด ฉันอยากสร้างกลุ่ม ส่งลิงก์เข้าแชต และให้ทั้งกลุ่มเลือกร้านได้ภายใน 5 นาที โดยเพื่อนไม่ต้องลงแอปหรือสมัคร

**Business value:** เป็น growth loop หลักของ strategy (D-016) และเป็นแหล่งข้อมูลของ gate วัน 1–30

**Acceptance criteria**

1. Given ผู้จัด (ไม่ต้อง login) เปิดแท็บ Kampai When ตั้งชื่อนัด งบเป็นเพดาน (D-002) ย่าน และกด "สร้างกลุ่ม" Then ได้กลุ่มใหม่พร้อมลิงก์ที่เดายาก ลิงก์หมดอายุใน 24 ชั่วโมงและกลุ่มรับสมาชิกได้สูงสุด 20 คน (A-008)
2. Given ผู้จัดมีลิงก์ When กดแชร์ Then เปิดเมนูแชร์ของเครื่อง (Web Share) หรือคัดลอกลิงก์ได้ และบันทึก `group_invite_shared`
3. Given สมาชิกเปิดลิงก์ (GRP-003) และตอบความชอบ When มีผู้ตอบรวมผู้จัดอย่างน้อย 2 คน (A-004) Then ผู้จัดกด "เริ่มโหวต" ได้ ถ้าตั้งเวลาปิดไว้ ระบบเริ่มโหวตอัตโนมัติเมื่อถึงเวลา
4. Given เริ่มโหวต When ระบบสร้างรายการ Then แสดง 3–5 ร้านให้ทุกคนจากการค้นหาหนึ่งครั้งต่อกลุ่ม ใช้เกณฑ์: ราคาไม่เกินเพดานงบ, อยู่ในย่านที่ผู้จัดเลือก, จัดอันดับตามจำนวนสมาชิกที่ประเภทอาหารตรงกับความชอบ แล้วตามคะแนนและระยะ
5. Given สมาชิกโหวต When แตะร้าน Then ได้หนึ่งเสียงต่อคน เปลี่ยนเสียงได้จนกว่าจะปิดโหวต ผลรวมอัปเดตถึงทุกคนภายใน 5 วินาทีที่ P90 (NFR-10)
6. Given ถึงเวลาปิดหรือผู้จัดกด "ปิดโหวต" When ปิด Then ผู้ที่ไม่โหวตนับเป็นงดออกเสียง (D-004) ร้านที่ได้คะแนนสูงสุดชนะ ถ้าเสมอให้ผู้จัดเลือกเอง (D-003) และบันทึกจำนวนผู้งดออกเสียง
7. Given ปิดโหวตแล้วคะแนนเสมอ When ผู้จัดยังไม่เลือก Then ระบบเตือนผู้จัดบนหน้าจอพร้อมตัวนับ 5 นาที และแจ้งทุกคนว่ากำลังรอผู้จัด · Given ผู้จัดไม่เลือกภายใน 5 นาที When หมดเวลา Then กลุ่มปิดโดยไม่มีผล ทุกคนเห็นข้อความ และผู้จัดเริ่มโหวตใหม่ได้ (D-023) นาฬิกา Group TTY ไม่รีเซ็ตเมื่อเริ่มโหวตใหม่ · การเตือนทำงานเมื่อผู้จัดเปิดหน้าอยู่ MVP1 ไม่มี push notification ผู้จัดที่ปิดหน้าเว็บจะเห็นคำเตือนและเวลาที่เหลือเมื่อกลับมา
8. Given กลุ่มได้ผล When ผู้จัดยืนยัน Then แสดงการ์ดสรุป (GRP-004) และบันทึก `group_decision_confirmed` พร้อม Group TTY
9. Given ผู้จัดกด "ยกเลิกนัด" When ยืนยัน Then สมาชิกเห็นว่านัดถูกยกเลิกและบันทึก `group_cancelled`
10. Given ลิงก์หมดอายุหรือกลุ่มปิดแล้ว When มีคนเปิดลิงก์ Then เห็นผลสรุป (ถ้ามี) แบบอ่านอย่างเดียว หรือข้อความว่าหมดอายุพร้อมปุ่ม "จัดนัดของคุณเอง"

**Edge cases:** ผู้จัดปิดหน้าเว็บ (สถานะอยู่ฝั่งเซิร์ฟเวอร์ ผู้จัดกลับมาจัดการต่อได้ในเบราว์เซอร์เดิม) · ผู้จัดเปลี่ยนเบราว์เซอร์แล้วเสียสิทธิ์ผู้จัด (ดู AUTH-001) · สมาชิกครบ 20 คน · ร้านที่ได้น้อยกว่า 3 (แจ้งผู้จัดให้ขยายย่านหรืองบ) · ผู้ใช้โหวตซ้ำเร็ว ๆ (idempotent) · เครือข่ายหลุดกลางโหวต (ส่งซ้ำอัตโนมัติ) · ผลสรุปขัดกันถ้าผู้จัดสองเครื่อง (ใช้สถานะบนเซิร์ฟเวอร์เป็นหลัก)

**Security / privacy:** สิทธิ์เข้าถึงกลุ่มด้วยโทเคนในลิงก์และ RLS ที่ผูกกับสมาชิก · ผู้ไม่ใช่สมาชิกอ่านข้อมูลกลุ่มไม่ได้ · rate limit การสร้างกลุ่มต่อ anonymous user และต่อ IP, จำกัดกลุ่มต่อวัน (R-014) · ไม่มีพิกัดดิบ ใช้ย่านที่ผู้จัดเลือก · ข้อมูลกลุ่มและชื่อเล่นเก็บ 30 วันหลังปิดแล้วลบ (A-011)

**Analytics:** ดู ANALYTICS_PLAN §1 กลุ่ม Kampai

**Tests:** unit (กติกาปิดโหวต เสมอ งดออกเสียง ขั้นต่ำ 2 เสียง) · integration (RLS ของกลุ่ม, idempotent vote, หมดอายุ) · E2E หลายเบราว์เซอร์จำลองสมาชิก 5 คนจนได้ผลภายใน 5 นาที · manual บน LINE in-app browser

---

## GRP-003 — ผู้ร่วมกลุ่มโดยไม่ต้อง login

**User story:** ในฐานะเพื่อนที่ได้รับลิงก์ ฉันอยากแตะลิงก์แล้วโหวตได้เลย ไม่ต้องลงแอปและไม่ต้องสมัคร

**Acceptance criteria**

1. Given ผู้ร่วมกลุ่มเปิดลิงก์ที่ยังไม่หมดอายุ When หน้าโหลด Then เห็นชื่อนัด ผู้จัด (ชื่อเล่น) ตัวนับเวลา และปุ่มเข้าร่วม โดยไม่มีหน้า login หรือ tab bar
2. Given กดเข้าร่วม When สำเร็จ Then ได้ anonymous session ผูกกับกลุ่มนั้น ตั้งชื่อเล่นได้ (ไม่บังคับ ค่าเริ่มต้น "เพื่อน #n" ไม่เกิน 20 ตัวอักษร)
3. Given เข้าร่วมแล้ว When เลือกประเภทอาหาร 1–2 อย่างและโหวต Then ทำเสร็จภายใน 3 แตะหลังเปิดลิงก์ (ค่าเป็นข้อเสนอ H-009)
4. Given เปิดลิงก์ซ้ำในเบราว์เซอร์เดิม When หน้าโหลด Then ยังเป็นสมาชิกคนเดิม ไม่นับเป็นสมาชิกใหม่
5. Given ผู้ร่วมกลุ่มกด "ลบการเข้าร่วมของฉัน" When ยืนยัน Then ชื่อเล่นและความชอบของคนนั้นถูกลบ และเสียงที่โหวตไปถูกถอนก่อนปิดโหวต
6. Given เปิดจาก in-app browser ของ LINE When ใช้งาน Then เข้าร่วมและโหวตได้ครบโดยไม่ต้องเปลี่ยนเบราว์เซอร์
7. Given banner ขอ consent analytics แสดง When ผู้ใช้ยังไม่ตอบ Then banner ไม่บังปุ่มโหวต และการโหวตทำงานได้ (R-010)

**Edge cases:** ลิงก์ที่ถูกแชร์ต่อนอกกลุ่มเป้าหมาย (นับตามจำนวนสมาชิกสูงสุด) · ผู้ใช้ปิดคุกกี้หรือเบราว์เซอร์โหมดส่วนตัว (แจ้งว่าอาจกลับมาเป็นสมาชิกเดิมไม่ได้) · ชื่อเล่นไม่เหมาะสม (จำกัดความยาว และผู้จัดลบสมาชิกได้)

**Security / privacy:** ไม่เก็บชื่อจริง อีเมล หรือพิกัด · ชื่อเล่นเป็น free-text จึงมีความเสี่ยงข้อมูลส่วนบุคคลที่ไม่ตั้งใจ ให้จำกัดและลบพร้อมกลุ่ม (A-011) · แสดงลิงก์ privacy notice บนหน้าเข้าร่วม

**Analytics:** `group_link_opened`, `group_joined`, `group_preference_set`, `group_vote_cast`

**Tests:** E2E บนอุปกรณ์จริง iOS และ Android รวม in-app browser ของ LINE · integration (anonymous session ผูกกับกลุ่ม, ลบการเข้าร่วม)

---

## GRP-004 — การ์ดสรุปที่แชร์กลับเข้ากลุ่ม

**User story:** ในฐานะผู้จัด ฉันอยากส่งผลสรุปกลับเข้าแชตให้ทุกคนเห็นพร้อมปุ่มเปิดแผนที่

**Acceptance criteria**

1. Given กลุ่มได้ผล When แสดงหน้าผลสรุป Then เห็นชื่อร้าน ย่าน ราคาโดยประมาณ เวลาที่ใช้ตัดสิน ปุ่มเปิดแผนที่ (ลิงก์ universal ตาม SOLO-004) และปุ่มแชร์
2. Given กดแชร์ When ใช้ Web Share ได้ Then เปิดเมนูแชร์ของเครื่อง ถ้าไม่ได้ให้คัดลอกข้อความสรุปพร้อมลิงก์
3. Given ลิงก์ถูกวางในแอปแชต When แสดงตัวอย่างลิงก์ Then เห็นชื่อนัด สถานะ ("ได้ร้านแล้ว") และภาพแบรนด์ โดยไม่เปิดเผยชื่อเล่นของสมาชิก
4. Given ผู้ร่วมกลุ่มดูผลสรุป When เลื่อนถึงท้ายหน้า Then เห็นปุ่ม "จัดนัดของคุณเอง" ซึ่งไม่บังการดูผลและบันทึก `group_cta_create_clicked` พร้อม `source_group_id`
5. Given หน้าผลสรุป When ผู้จัดยังไม่ login Then เห็นข้อเสนอ AUTH-001 ข้อ 3 ใต้ปุ่มหลัก

**Edge cases:** เปิดในเบราว์เซอร์ที่ไม่รองรับ Web Share · ร้านถูกลบจาก provider หลังปิดโหวต (แสดงข้อมูลที่บันทึกไว้เฉพาะที่ license อนุญาต อ้าง SPIKE-001)

**Security / privacy:** ตัวอย่างลิงก์ (Open Graph) ห้ามมีข้อมูลบุคคล · ต้องแสดง attribution ตามเงื่อนไข provider

**Analytics:** `group_result_shared` (channel), `group_cta_create_clicked`

**Tests:** manual วางลิงก์ใน LINE และแอปแชตหลักอื่นเพื่อดูตัวอย่าง (Q-017) · unit (ข้อความสรุป)

---

## GRP-005 — กติกาเวลาและเงื่อนไขการปิดโหวต

**User story:** ฉันอยากให้โหวตปิดเองตามเวลา เพื่อไม่ต้องรอคนที่ไม่ตอบ

**Acceptance criteria**

1. Given ผู้จัดสร้างกลุ่ม When ไม่ตั้งค่าเวลา Then เวลาปิดโหวตค่าเริ่มต้น 5 นาทีหลังเริ่มโหวต (A-008) ปรับได้ 2–15 นาที
2. Given เหลือน้อยกว่า 1 นาที When หน้าโหวตแสดง Then ทุกคนเห็นตัวนับเวลาถอยหลัง
3. Given ถึงเวลาปิดแต่มีเสียงรวมน้อยกว่า 2 When ปิด Then ไม่ปิดอัตโนมัติ แจ้งผู้จัดให้ขยายเวลาหรือยกเลิก
4. Given ผู้จัดขยายเวลา When กดขยาย Then บันทึก `group_deadline_extended` และไม่รีเซ็ตนาฬิกา Group TTY (ยังไม่อยู่ในต้นแบบ)
5. Given คะแนนเสมอ When ผู้จัดยังไม่เลือก Then ใช้ตัวนับ 5 นาทีตาม GRP-002 ข้อ 7 และเมื่อหมดเวลากลุ่มปิดโดยไม่มีผล (D-023)

**หมายเหตุ:** ตัวเลขเวลาเป็นข้อเสนอ ต้องทดสอบกับผู้ใช้จริง

---

## ANL-002 — ตัวชี้วัดแบบ strategy (K, repeat, saturation)

**AC**
1. ทุก event ใน ANALYTICS_PLAN กลุ่ม Kampai ถูกส่งพร้อม `group_id` และ `member_pseudo_id` และไม่มีชื่อ ชื่อเล่น พิกัด หรือ free-text
2. มี dashboard: I, A, H, K พร้อมช่วงความเชื่อมั่นและจำนวนกลุ่มที่ใช้คำนวณ · network saturation · Group completion และสัดส่วนที่ได้ผลภายใน 5 นาที · organizer repeat ภายใน 30 วัน (แยกผู้ที่ login กับไม่ login) · Participation rate
3. การนับ "บุคคลที่ไม่ซ้ำ" ใช้ `member_pseudo_id` ตาม A-010 และ dashboard ระบุข้อจำกัดว่าบุคคลเดียวกันข้ามอุปกรณ์หรือหลังล้างเบราว์เซอร์รวมกันไม่ได้
4. บัญชีและกลุ่มทดสอบภายในติด `is_internal` และถูกตัดออก
5. unit test ตรวจสูตร K และการตัดการเชิญวนซ้ำในเครือข่ายเดิมด้วยข้อมูลตัวอย่าง

**เหตุผล:** gate วัน 1–30 และ 31–60 (D-020) ตัดสินด้วยตัวเลขเหล่านี้ ถ้าไม่เก็บตั้งแต่วันแรกจะวัดย้อนหลังไม่ได้

---

## INT-001 — โครงข้อมูล intent ของกลุ่ม (ไม่ขาย ไม่แสดง offer)

**User story:** ในฐานะ Founder ฉันอยากมีข้อมูลความต้องการของกลุ่มในรูปแบบรวมที่ไม่ระบุตัวบุคคล เพื่อใช้ทดลองคุยกับแบรนด์ใน pilot โดยไม่ต้องรื้อโครงข้อมูลภายหลัง

**Acceptance criteria**

1. Given กลุ่มได้ผล (`group_decision_confirmed`) When บันทึก Then สร้าง intent record 1 รายการ: ระดับงบ, ย่าน, ช่วงเวลา (ชั่วโมง), จำนวนสมาชิกเป็นช่วง (2–3, 4–6, 7+ ตาม A-009), ผลโหวตรวมตามประเภทอาหาร, ร้านที่เลือก
2. Given ตรวจ record When อ่าน Then ไม่มี `member_pseudo_id` ชื่อเล่น พิกัด หรือ identifier ที่ย้อนไปหาบุคคลได้
3. Given client ทุกชนิด When พยายามอ่านตาราง intent Then อ่านไม่ได้ (เข้าถึงได้เฉพาะ service role ฝั่งเซิร์ฟเวอร์)
4. Given ผู้ใช้อ่าน privacy notice When ดูหัวข้อการใช้ข้อมูล Then เห็นคำอธิบายการใช้ข้อมูลรวมไม่ระบุตัวบุคคล และ consent ที่เกี่ยวข้องตามผล SPIKE-002
5. Given ระบบ When ทำงาน Then ไม่ส่งออกให้บุคคลที่สาม ไม่ปรากฏใน UI และไม่มีการแสดง offer ของแบรนด์ใน MVP1

**ข้อควรระวัง:** ฐานทางกฎหมายและความเสี่ยงการระบุตัวตนซ้ำจากข้อมูลรวมที่เล็กมาก (F-09, R-012) ต้องผ่าน SPIKE-002 ก่อนเริ่ม pilot กับแบรนด์ ห้ามขายข้อมูลส่วนบุคคล ต้องแยก Sponsored ออกจาก Organic เมื่อมี offer ในอนาคต

---

## BRAND-001 — ไอคอน PWA และโลโก้ (ใช้ชุดสีเดิมของแอป)

**หมายเหตุ:** Founder ตัดสินให้ใช้ชุดสีเดิมของแอป (D-019) จึงไม่ต้องเปลี่ยน design tokens ตามที่ strategy ระบุ

**Acceptance criteria**
1. Design tokens ใน DESIGN_SPEC §2 (ชุดเดิม) ถูกนำไปใช้ทั้งแอป
2. ไอคอน PWA ครบขนาดที่ manifest ต้องการ รวมแบบ maskable และ theme color ตรงแบรนด์ (Cream / Coral)
3. ข้อความขาวบน coral ใช้กับปุ่มตัวหนาและตัวอักษรใหญ่เท่านั้น ตามกติกา contrast ใน DESIGN_SPEC §2
4. โลโก้ที่ใช้เป็นไอคอน: ยังไม่ตัดสิน ระหว่างโลโก้ K + Fork ของ strategy กับโลโก้เดิม (Q-019) ภาพโลโก้ใน strategy เป็น concept preview ต้องสร้างเป็น vector สะอาดและทดสอบการอ่านที่ขนาดเล็กก่อนใช้
5. ตรวจเครื่องหมายการค้าก่อนสร้างไอคอนและจดโดเมน (Q-016)

---

## WPL-001 — ชุด pilot สำหรับองค์กร (Could เริ่มหลัง gate วัน 60)

**เงื่อนไขเริ่มงาน:** ผ่าน gate organizer repeat (D-020) เท่านั้น และไม่ต้อง customize หรือ integrate ระบบรายองค์กร (kill criterion ของ strategy 03)

**Acceptance criteria**
1. Given ผู้จัดในองค์กร When สร้างกลุ่มประจำ (เช่น Office Lunch) Then กลุ่มคงอยู่และเริ่มรอบใหม่ได้ด้วยการแตะครั้งเดียว
2. Given ตั้งงบต่อหัวล่วงหน้า When สร้างรอบ Then ใช้เป็นเพดานงบ (D-002) อัตโนมัติ
3. Given ผู้จัดกด "ส่งออกสรุป" When สำเร็จ Then ได้ไฟล์ CSV ที่มีจำนวนรอบ ระดับงบ และร้านที่เลือกต่อสัปดาห์ โดยไม่มีชื่อบุคคล
4. Given pilot When ดำเนินการ Then ไม่มีงาน integrate ERP หรือพัฒนาเฉพาะรายองค์กร

---

## PLAT-001 — Web app shell

Next.js (React) + TypeScript (strict) บน Vercel (D-013) · mobile-first responsive ออกแบบที่ 360–430px และแสดงเป็นคอลัมน์กลางบน desktop · PWA manifest เพื่อเพิ่มลงหน้าจอหลัก (service worker ไม่บังคับใน MVP1) · bottom navigation 3 รายการ (กินไหน, Kampai, ฉัน) · i18n ไทย/อังกฤษ · design tokens จาก `DESIGN_SPEC.md` · CI: typecheck, lint, unit test และ e2e smoke บนเบราว์เซอร์ ทุก PR · Sentry browser SDK
**AC:** Given PR ใหม่ When CI รัน Then typecheck, lint และ test ต้องผ่านจึง merge ได้ · Given แอป crash Then Sentry ได้รับรายงานโดยไม่มีข้อมูลส่วนบุคคลใน payload

## PLAT-002 — Supabase schema, auth, RLS

ตาราง: `profiles`, `preferences`, `decision_sessions`, `decisions`, `feedback`, `consent_log`, `groups`, `group_members`, `group_votes`, `intent_records` · Supabase anonymous sign-in สำหรับทุกคนที่ยังไม่ login และ Auth ด้วย Apple และ Google (D-014, D-017) · RLS ทุกตาราง
**AC:** Given ผู้ใช้ A When query ข้อมูลของ B ด้วย anon key Then ได้ 0 แถว (ทดสอบอัตโนมัติทุกตาราง) · Given ผู้ที่ไม่ใช่สมาชิกกลุ่ม When query กลุ่มนั้น Then ได้ 0 แถว · Given ตาราง `intent_records` When client ทุกชนิด query Then ได้ 0 แถว · Given anonymous user ผูกบัญชีสำเร็จ Then ข้อมูลเดิมยังเข้าถึงได้ (ยืนยันความสามารถของ Supabase ใน Gate 2) · Given migration ใหม่ Then ตารางที่ไม่มี RLS policy ทำให้ CI fail · service role key ไม่ปรากฏใน client bundle (ตรวจใน CI)

## PLAT-003 — Places adapter

Edge Function เดียวที่ห่อ provider หลัง interface `searchPlaces(criteria)` เพื่อเปลี่ยน provider ได้ · rate limit ต่อผู้ใช้ · daily cost cap พร้อม kill switch · cache ตามที่ license อนุญาตเท่านั้น
**AC:** Given เกิน rate limit Then ตอบ 429 และแอปแสดง error state · Given ถึง daily cap Then หยุดเรียก provider และแจ้งเตือน Founder · Given provider timeout 4 วินาที Then ตอบ error ที่แอปจัดการได้

## PRIV-001 — Consent, privacy notice, ลบข้อมูล

**AC**
1. Given ผู้ใช้ใหม่ When ก่อนขอสิทธิ์ตำแหน่งของระบบ Then เห็นคำอธิบายวัตถุประสงค์เป็นภาษาไทย และเลือก "เลือกย่านเอง" ได้โดยไม่เสียฟังก์ชันหลัก
2. Given ผู้ใช้เปิด privacy notice When อ่าน Then พบรายการตาม PDPA ม.23: วัตถุประสงค์, ข้อมูลที่เก็บ, ระยะเวลาเก็บ, ผู้รับข้อมูล, ช่องทางติดต่อ controller, สิทธิของเจ้าของข้อมูล
3. Given ผู้ใช้กด "ลบบัญชีและข้อมูล" และยืนยัน When สำเร็จ Then บัญชีใน Supabase Auth และข้อมูลใน `preferences`, `decisions`, `feedback`, `decision_sessions` ของผู้ใช้ถูกลบ และ PostHog person ถูกลบ (ม.33)
4. Given ผู้ใช้ให้หรือถอน consent When เกิดขึ้น Then บันทึกใน `consent_log` พร้อมเวลาและเวอร์ชันของ notice (ม.19)
5. Given analytics When ผู้ใช้ยังไม่ยอมรับ Then ไม่ส่ง event ที่ไม่จำเป็นต่อการให้บริการ และบนเว็บไม่ตั้ง cookie หรือ localStorage ของ PostHog จนกว่าจะยอมรับ (R-010) ข้อยกเว้น: ข้อมูลที่จำเป็นต่อการทำงานของกลุ่ม (anonymous session, การโหวต) ใช้ฐานสัญญา/ความจำเป็นของบริการ ตามผล SPIKE-002
6. Given ผู้ร่วมกลุ่มที่ไม่ login When เข้าร่วม Then เห็นลิงก์ privacy notice และปุ่ม "ลบการเข้าร่วมของฉัน" และ banner ขอ consent ต้องไม่บังการโหวต

ข้อความ notice และฐานทางกฎหมายต้องผ่าน SPIKE-002 ก่อน Gate 4

## ANL-001 — Event instrumentation

**AC:** event ทุกตัวใน ANALYTICS_PLAN §1 ถูกส่งพร้อม property ที่กำหนด · ไม่มีพิกัด ชื่อ หรือ free-text ใน event · มี dashboard: Solo TTY median/P90, completion rate, abandonment, cost per decision · unit test ตรวจ schema ของ event

---

## 8. Non-functional requirements

| ID | หมวด | ข้อกำหนด |
|---|---|---|
| NFR-01 | Performance | ผลแนะนำแสดงภายใน 2 วินาทีที่ P75 บน 4G · LCP ≤ 2.5 วินาทีที่ P75 บน 4G ตามเกณฑ์ Core Web Vitals · งบขนาด JS ของหน้าแรกกำหนดใน Gate 2 |
| NFR-02 | Reliability | Sessions ที่ไม่มี JavaScript error ที่ไม่ได้จัดการ ≥ 99% ใน beta (วัดจาก Sentry) |
| NFR-03 | Accessibility | Contrast ตาม WCAG 2.1 AA สำหรับข้อความ · รองรับการขยายตัวอักษรของเบราว์เซอร์ถึง 200% (WCAG 1.4.4) · เคารพ reduce motion · ปุ่มมี accessibility label ทั้งสองภาษา |
| NFR-04 | Security | ไม่มี privileged secret ใน client · RLS ทุกตาราง · authorization ฝั่ง server · dependency scan ใน CI |
| NFR-05 | Privacy | ตาม PRIV-001 · ไม่เก็บพิกัดดิบ · ROPA ตาม PDPA ม.39 (ตรวจข้อยกเว้นกิจการขนาดเล็กใน SPIKE-002) |
| NFR-06 | Cost | Daily cap ต่อ provider · แจ้งเตือนเมื่อใช้ถึง 50% และ 80% ของงบเดือน |
| NFR-07 | Operability | Founder ดูแลได้คนเดียว: ไม่มี server ที่ต้อง patch เอง, runbook 1 หน้าสำหรับ kill switch |
| NFR-08 | Localization | ไทยเป็นค่าเริ่มต้น อังกฤษครบทุกหน้าจอ |
| NFR-09 | Browser support | iOS Safari และ Chrome Android (เวอร์ชันหลักล่าสุด 2 รุ่น) · in-app browser ของ LINE ตรวจแยก (R-008) · Geolocation API ใช้ได้เฉพาะ HTTPS และขอเมื่อผู้ใช้กดปุ่มเท่านั้น |
| NFR-10 | Realtime | ผลโหวตอัปเดตถึงสมาชิกทุกคนภายใน 5 วินาทีที่ P90 · รองรับกลุ่มสูงสุด 20 คน (A-008) · วิธีทำ (Supabase Realtime หรือ polling) เลือกใน Gate 2 |
| NFR-11 | Abuse & cost | จำกัดการสร้างกลุ่มต่อผู้ใช้และต่อ IP · หนึ่งการค้นหา Places ต่อกลุ่ม · daily cap และ kill switch (R-014) |

## 9. Definition of Done (ใช้ร่วมทุก requirement)

- AC ทุกข้อมี test ที่รันได้และผ่านใน CI หรือมีหลักฐาน manual test แนบใน PR
- Event ตาม ANALYTICS_PLAN ถูกส่งและตรวจใน PostHog ของ environment ทดสอบ
- ครบสถานะ loading, empty, error, denied-location ตาม DESIGN_SPEC
- ข้อความครบทั้งไทยและอังกฤษ
- ไม่มี secret ใน client, RLS test ผ่าน
- PR อ้าง requirement ID และ Linear issue ID (เมื่อมี) และ Founder ยอมรับ
