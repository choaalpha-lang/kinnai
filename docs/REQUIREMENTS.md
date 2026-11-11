# KinNai MVP1 — Requirements

ขอบเขต: MVP1 (M0–3) เท่านั้น · ต้นแบบอ้างอิง: `prototype/index.html` · การตัดสินใจอ้างอิง: `docs/DECISION_LOG.md`

ทุก requirement ใช้ Definition of Done ร่วมกัน (§9) Event ทั้งหมดนิยามใน `docs/ANALYTICS_PLAN.md`

| ID | Capability | MoSCoW | ขึ้นกับ |
|---|---|---|---|
| SOLO-001 | เลือกงบ ประเภทอาหาร และระยะ | Must | PLAT-001, PRIV-001 |
| SOLO-002 | แนะนำไม่เกิน 3 ร้านพร้อมเหตุผล | Must | SOLO-001, PLAT-003 |
| SOLO-003 | ยืนยันการตัดสินใจและบันทึก Time to Yes | Must | SOLO-002, PLAT-002, ANL-001 |
| SOLO-004 | เปิดแผนที่นำทางภายนอก | Must | SOLO-003 |
| SOLO-005 | Feedback และการจำความชอบ | Should | SOLO-003, PLAT-002 |
| GRP-001 | Kampai clickable prototype (ข้อมูลจำลอง) | Could | ไม่มี (ส่งมอบแล้วเป็น HTML) |
| PLAT-001 | App shell, i18n, design tokens, CI | Must | Gate 2 |
| PLAT-002 | Supabase schema, anonymous auth, RLS | Must | PLAT-001 |
| PLAT-003 | Places adapter ฝั่ง server, rate limit, cost cap | Must | SPIKE-001, PLAT-002 |
| PRIV-001 | Consent ตำแหน่ง, privacy notice, ลบข้อมูล | Must | SPIKE-002, PLAT-002 |
| ANL-001 | Event instrumentation และ dashboard | Must | PLAT-001 |

---

## SOLO-001 — เลือกงบ ประเภทอาหาร และระยะ

**User story:** ในฐานะคนที่หิวและยังไม่รู้จะกินอะไร ฉันอยากบอกงบ แนวอาหาร และระยะที่ไปไหว ด้วยการแตะไม่กี่ครั้ง เพื่อให้แอปเลือกร้านให้

**Business value:** เป็น input ขั้นต่ำของการแนะนำ และเป็นจุดเริ่มนาฬิกา TTY (D-001)

**Acceptance criteria**

1. Given ผู้ใช้เปิดแอปครั้งแรก When หน้าแรกแสดง Then เห็นตัวเลือกงบ 4 ระดับ (≤฿100, ฿101–200, ฿201–400, ฿400+), ประเภทอาหาร (รวม "อะไรก็ได้") และระยะ (500 ม., 1, 2, 5 กม.) โดยมีค่าเริ่มต้นเลือกไว้แล้ว
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

**Business value:** สร้าง North Star "Successful Food Decision" ซึ่งนับเฉพาะการยืนยันจริง ไม่ใช่แค่ดูผล

**Acceptance criteria**

1. Given ผลแสดงอยู่ When ผู้ใช้กด "กินร้านนี้" Then ระบบบันทึก decision 1 รายการ และแสดงหน้ายืนยันพร้อมเวลา TTY
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

**Edge cases:** ร้านไม่มีพิกัด (ใช้ชื่อ + ย่านค้นหาแทน) · iOS กับ Android ใช้ URL scheme ต่างกัน

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

**User story:** ในฐานะ Founder ฉันอยากเห็นและให้คนลอง flow กลุ่มด้วยข้อมูลจำลอง เพื่อตัดสินว่าควรลงทุนสร้างจริงใน MVP1.5 หรือไม่

**สถานะ:** ส่งมอบแล้วใน `prototype/index.html` (แท็บ Kampai)

**Acceptance criteria**

1. Given เปิดแท็บ Kampai When ทำตาม 5 ขั้น (สร้าง, ชวน, จับคู่, โหวต, ยืนยัน) Then จบที่ร้านเดียวพร้อม Group TTY
2. Given โหวตเสมอ When ปิดโหวต Then organizer เลือกจากร้านที่เสมอ (D-003)
3. Given สมาชิกไม่ตอบ When ปิดโหวต Then นับเป็นงดออกเสียงและบันทึกจำนวนใน event (D-004)
4. Given อยู่ในขั้นใดก็ตาม When ดูหน้าจอ Then มีป้ายบอกชัดว่าเป็นข้อมูลจำลอง
5. Given ยกเลิกกลางทาง When กด "ยกเลิกนัดนี้" Then บันทึก `group_cancelled` พร้อมขั้นที่ยกเลิก

**ไม่รวม:** ลิงก์เชิญจริง, realtime, บัญชีผู้ใช้หลายคน, LINE share จริง (ทั้งหมดเป็น GRP-002 ใน MVP1.5)

---

## PLAT-001 — App shell

Expo + React Native + TypeScript (strict) · 3 แท็บ (กินไหน, Kampai, ฉัน) · i18n ไทย/อังกฤษ · design tokens จาก `DESIGN_SPEC.md` · CI: typecheck, lint, unit test ทุก PR · Sentry
**AC:** Given PR ใหม่ When CI รัน Then typecheck, lint และ test ต้องผ่านจึง merge ได้ · Given แอป crash Then Sentry ได้รับรายงานโดยไม่มีข้อมูลส่วนบุคคลใน payload

## PLAT-002 — Supabase schema, auth, RLS

ตาราง: `profiles`, `preferences`, `decision_sessions`, `decisions`, `feedback`, `consent_log` · Anonymous sign-in (A-006) · RLS ทุกตาราง
**AC:** Given ผู้ใช้ A When query ข้อมูลของ B ด้วย anon key Then ได้ 0 แถว (ทดสอบอัตโนมัติทุกตาราง) · Given migration ใหม่ Then ตารางที่ไม่มี RLS policy ทำให้ CI fail · service role key ไม่ปรากฏใน client bundle (ตรวจใน CI)

## PLAT-003 — Places adapter

Edge Function เดียวที่ห่อ provider หลัง interface `searchPlaces(criteria)` เพื่อเปลี่ยน provider ได้ · rate limit ต่อผู้ใช้ · daily cost cap พร้อม kill switch · cache ตามที่ license อนุญาตเท่านั้น
**AC:** Given เกิน rate limit Then ตอบ 429 และแอปแสดง error state · Given ถึง daily cap Then หยุดเรียก provider และแจ้งเตือน Founder · Given provider timeout 4 วินาที Then ตอบ error ที่แอปจัดการได้

## PRIV-001 — Consent, privacy notice, ลบข้อมูล

**AC**
1. Given ผู้ใช้ใหม่ When ก่อนขอสิทธิ์ตำแหน่งของระบบ Then เห็นคำอธิบายวัตถุประสงค์เป็นภาษาไทย และเลือก "เลือกย่านเอง" ได้โดยไม่เสียฟังก์ชันหลัก
2. Given ผู้ใช้เปิด privacy notice When อ่าน Then พบรายการตาม PDPA ม.23: วัตถุประสงค์, ข้อมูลที่เก็บ, ระยะเวลาเก็บ, ผู้รับข้อมูล, ช่องทางติดต่อ controller, สิทธิของเจ้าของข้อมูล
3. Given ผู้ใช้กด "ลบข้อมูลของฉัน" และยืนยัน When สำเร็จ Then ข้อมูลใน `preferences`, `decisions`, `feedback`, `decision_sessions` ของผู้ใช้ถูกลบ และ PostHog person ถูกลบ (ม.33)
4. Given ผู้ใช้ให้หรือถอน consent When เกิดขึ้น Then บันทึกใน `consent_log` พร้อมเวลาและเวอร์ชันของ notice (ม.19)
5. Given analytics When ผู้ใช้ยังไม่ยอมรับ Then ไม่ส่ง event ที่ไม่จำเป็นต่อการให้บริการ

ข้อความ notice และฐานทางกฎหมายต้องผ่าน SPIKE-002 ก่อน Gate 4

## ANL-001 — Event instrumentation

**AC:** event ทุกตัวใน ANALYTICS_PLAN §1 ถูกส่งพร้อม property ที่กำหนด · ไม่มีพิกัด ชื่อ หรือ free-text ใน event · มี dashboard: Solo TTY median/P90, completion rate, abandonment, cost per decision · unit test ตรวจ schema ของ event

---

## 8. Non-functional requirements

| ID | หมวด | ข้อกำหนด |
|---|---|---|
| NFR-01 | Performance | ผลแนะนำแสดงภายใน 2 วินาทีที่ P75 บน 4G · cold start ≤ 3 วินาทีบนเครื่องระดับกลาง |
| NFR-02 | Reliability | Crash-free sessions ≥ 99% ใน beta |
| NFR-03 | Accessibility | Contrast ตาม WCAG 2.1 AA สำหรับข้อความ · รองรับ Dynamic Type · เคารพ reduce motion · ปุ่มมี accessibility label ทั้งสองภาษา |
| NFR-04 | Security | ไม่มี privileged secret ใน client · RLS ทุกตาราง · authorization ฝั่ง server · dependency scan ใน CI |
| NFR-05 | Privacy | ตาม PRIV-001 · ไม่เก็บพิกัดดิบ · ROPA ตาม PDPA ม.39 (ตรวจข้อยกเว้นกิจการขนาดเล็กใน SPIKE-002) |
| NFR-06 | Cost | Daily cap ต่อ provider · แจ้งเตือนเมื่อใช้ถึง 50% และ 80% ของงบเดือน |
| NFR-07 | Operability | Founder ดูแลได้คนเดียว: ไม่มี server ที่ต้อง patch เอง, runbook 1 หน้าสำหรับ kill switch |
| NFR-08 | Localization | ไทยเป็นค่าเริ่มต้น อังกฤษครบทุกหน้าจอ |

## 9. Definition of Done (ใช้ร่วมทุก requirement)

- AC ทุกข้อมี test ที่รันได้และผ่านใน CI หรือมีหลักฐาน manual test แนบใน PR
- Event ตาม ANALYTICS_PLAN ถูกส่งและตรวจใน PostHog ของ environment ทดสอบ
- ครบสถานะ loading, empty, error, denied-location ตาม DESIGN_SPEC
- ข้อความครบทั้งไทยและอังกฤษ
- ไม่มี secret ใน client, RLS test ผ่าน
- PR อ้าง requirement ID และ Linear issue ID (เมื่อมี) และ Founder ยอมรับ
