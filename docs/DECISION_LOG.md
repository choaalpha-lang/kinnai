# KinNai — Decision Log

อัปเดตล่าสุด: 2026-11-11 · ผู้ตัดสินใจ: Founder (คนเดียว) · แหล่งอ้างอิง: `docs/reference/kinnai_product_blueprint_v1.2.html` และคำตอบของ Founder ในรอบทบทวนต้นแบบ

สถานะ: **Confirmed** = Founder ตัดสินแล้ว · **Assumption** = การตีความของ Claude Code รอ Founder ยืนยัน · **Hypothesis** = ยังไม่มีหลักฐาน · **Open** = ยังไม่มีคำตอบ

## 1. Confirmed decisions

| ID | เรื่อง | การตัดสินใจ | ที่มา | ผลต่อ spec |
|---|---|---|---|---|
| D-001 | นิยาม Solo Time to Yes | เริ่มจับเวลาเมื่อผู้ใช้กำหนด criteria เสร็จ จบเมื่อยืนยันร้าน | Founder 2026-11-11 | ANALYTICS_PLAN §2, SOLO-003 |
| D-002 | ความหมายของงบ | งบเป็นเพดาน ร้านที่ถูกกว่าก็แสดง | Founder 2026-11-11 | SOLO-001, SOLO-002 |
| D-003 | Kampai โหวตเสมอ | ผู้ตั้งนัด (organizer) เป็นคนเลือกจากร้านที่คะแนนเท่ากัน | Founder 2026-11-11 | GRP-001, GRP-002 (อนาคต) |
| D-004 | คนไม่ตอบใน Kampai | นับเป็นงดออกเสียง | Founder 2026-11-11 | GRP-001, ANALYTICS_PLAN §3 |
| D-005 | Consent ตำแหน่งและ retention | ทำตามกฎหมายที่เกี่ยวข้อง (PDPA พ.ศ. 2562) | Founder 2026-11-11 | PRD §7, PRIV-001, SPIKE-002 |
| D-006 | Tech stack | Expo + React Native + TypeScript, Supabase, PostHog, Sentry, GitHub Actions + EAS | Blueprint V1.2 | PRD §6 |
| D-007 | ขอบเขต spec | เฉพาะ MVP1 เข้า detailed specification | Blueprint V1.2 | REQUIREMENTS |
| D-008 | สิ่งที่ไม่ทำใน MVP1 | Production payments, referral, merchant portal, LLM recommendation | Blueprint V1.2 | PRD §3 |
| D-009 | งบ MVP | เพดานวางแผน THB 45,000 (ทบทวนได้) | Blueprint V1.2 | GATE1 §5 |
| D-011 | Login | บังคับ login ตอนเปิดแอป (ก่อนหน้าแรก) ด้วย Sign in with Apple และ Google เท่านั้น LINE Login เลื่อนไปหลัง MVP1 แทนที่ A-006 | Founder 2026-11-11 | AUTH-001, PLAT-002, PRIV-001 |
| D-012 | Link account | หนึ่งบัญชี KinNai เชื่อมได้ทั้ง Apple และ Google ต้องเหลืออย่างน้อย 1 ช่องทาง | Founder 2026-11-11 | AUTH-001 |
| D-010 | Traceability | Requirement ID แยกจาก Linear issue ID ห้ามแต่ง Linear ID | Blueprint V1.2 | LINEAR_WORKFLOW |

## 2. Assumptions

**สถานะ 2026-11-11:** A-001 ถึง A-005 Founder ยืนยันแล้ว ถือเป็น Confirmed · A-006 **ถูกยกเลิก** แทนด้วย D-011

| ID | เกี่ยวกับ | สิ่งที่ Claude Code ตีความ | เหตุผล | ถ้าไม่ยืนยัน |
|---|---|---|---|---|
| A-001 | D-001 | นาฬิกาเริ่มที่ `criteria_completed` **ครั้งแรก** ของ session การแก้เงื่อนไขหรือขอชุดใหม่ไม่รีเซ็ตเวลา | ถ้ารีเซ็ตทุกครั้ง ค่า TTY จะต่ำเกินจริงสำหรับคนที่ต้องค้นหลายรอบ | ต้องแก้สูตรใน ANALYTICS_PLAN §2 |
| A-002 | D-001 | เก็บ `time_to_criteria` (เปิดหน้าแรก → กำหนด criteria เสร็จ) เป็น guardrail แยกต่างหาก | คำสัญญา "30 วินาที" ที่ผู้ใช้รับรู้รวมเวลากรอกด้วย ดู GATE1 Finding F-02 | เสี่ยง KPI ดูดีกว่าประสบการณ์จริง |
| A-003 | D-004 | งดออกเสียงไม่นับเป็นคะแนนให้ร้านใด แต่ยังอยู่ในตัวหารของ participation rate | สอดคล้องกับ blueprint ที่ให้บันทึกการไม่ตอบเพื่อไม่ให้ KPI ดูดีเกินจริง | ต้องนิยามตัวหารใหม่ |
| A-004 | D-003 | ต้องมีอย่างน้อย 2 เสียง (รวม organizer) จึงปิดโหวตได้ | ป้องกัน "group decision" ที่มีคนเดียวตัดสิน | Organizer ปิดได้ทันทีด้วยเสียงเดียว |
| A-005 | D-005 | ข้อเสนอ retention: ประวัติการตัดสินใจ 12 เดือน, ไม่เก็บพิกัดดิบ (ใช้ในคำขอแล้วทิ้ง), consent log เก็บตลอดอายุบัญชี + ระยะที่จำเป็นทางกฎหมาย | PDPA ไม่กำหนดตัวเลข แต่บังคับให้แจ้งระยะเวลา (ม.23) และลบเมื่อพ้นความจำเป็น (ม.37(3)) | ต้องมีตัวเลขอื่นก่อนเขียน privacy notice |
| ~~A-006~~ (ยกเลิก) | D-006 | MVP1 ใช้ Supabase anonymous sign-in ไม่มีหน้าสมัครสมาชิก | ลด friction ต่อ TTY และลดข้อมูลส่วนบุคคลที่เก็บ (data minimization ม.22) | ต้องเพิ่ม requirement เรื่อง Auth |

## 3. Hypotheses (ยังไม่มีหลักฐาน)

| ID | สมมติฐาน | หลักฐานที่ต้องมี | เมื่อไร |
|---|---|---|---|
| H-001 | Solo TTY median ≤ 30 วินาที | ข้อมูล beta จริง ไม่ใช่ต้นแบบ | Gate 3–4 |
| H-002 | Group TTY ≤ 5 นาที | MVP1.5 | หลัง MVP1 |
| H-003 | ผู้ใช้ยอมจ่าย Plus ฿99/เดือน และ Kampai ฿39/ครั้ง | การจ่ายเงินจริง | MVP1.5–2 |
| H-004 | 3 ร้านที่แนะนำ "ตรงใจ" พอให้ตัดสินใจได้ | อัตรา `decision_confirmed` และ feedback 👍 | Gate 3–4 |
| H-005 | Founder คนเดียวดูแลระบบได้ในเวลาที่รับได้ต่อสัปดาห์ | บันทึกชั่วโมงทำงานจริง | ต่อเนื่อง |
| H-006 | ต้นทุน API ต่อการตัดสินใจหนึ่งครั้งอยู่ในงบ | ราคาและโควตาของ provider (Q-001) | ก่อน Gate 2 |

## 4. Open questions

| ID | คำถาม | เจ้าของ | ต้องตอบก่อน | สถานะ |
|---|---|---|---|---|
| Q-001 | ผู้ให้บริการ Places และเงื่อนไข license ในการแสดงผลและ cache | Founder + SPIKE-001 | Gate 2 | **Open** (Founder ยังไม่ได้ตัดสินในรอบ 2026-11-11) |
| Q-002 | ตัวเลข retention ที่จะประกาศ (ดู A-005) | Founder + ที่ปรึกษากฎหมาย | Gate 4 | Open |
| Q-003 | อายุผู้ใช้ขั้นต่ำและวิธีจัดการผู้เยาว์ (PDPA ม.20) | Founder + ที่ปรึกษากฎหมาย | Gate 4 | Open |
| Q-004 | Region ของ Supabase และฐานการโอนข้อมูลไปต่างประเทศ (PDPA ม.28–29) | Founder | Gate 2 | Open |
| Q-005 | พื้นที่เปิดตัว (ย่านใดในกรุงเทพฯ) | Founder | Gate 2 | Open |
| Q-006 | การเชื่อม Linear workspace | Founder | ก่อนสร้าง issue จริง | Open: session นี้ไม่มี Linear tool |
| Q-007 | Default branch ของ repo | Founder | ก่อนเปิด PR | Open: repo ไม่มี `main` |
| Q-009 | ต้องขออีเมลจาก Apple/Google หรือใช้เฉพาะ account ID | Founder | Gate 2 | Open (ข้อเสนอ: ไม่ขออีเมล) |
| Q-008 | GRP-001 จะ port เข้าแอปจริงใน MVP1 หรือใช้ HTML prototype ที่มีแล้ว | Founder | Gate 2 | Open (ข้อเสนอ: ใช้ HTML ที่มีแล้ว) |

## 5. Risks

| ID | ความเสี่ยง | ระดับ | แนวทางลด |
|---|---|---|---|
| R-001 | ข้อมูล Places ไม่มีราคาต่อหัวเป็นบาท ทำให้ตัวกรองงบไม่แม่น | สูง | SPIKE-001 ทดสอบข้อมูลจริงในย่านเปิดตัว |
| R-002 | เงื่อนไข license ห้าม cache ทำให้ต้นทุนต่อคำขอสูง | สูง | เลือก provider จากเงื่อนไข cache, ตั้ง daily cap และ kill switch |
| R-003 | ภาระ PDPA ของ data controller ตกที่ Founder คนเดียว | กลาง | Data minimization, anonymous auth, ไม่เก็บพิกัดดิบ |
| R-004 | KPI TTY ดูดีกว่าประสบการณ์จริง | กลาง | A-001, A-002 และรายงานคู่กับ completion rate |
| R-006 | Apple ปฏิเสธแอปเพราะบังคับ login ทั้งที่ฟังก์ชันหลักไม่จำเป็นต้องใช้บัญชี (Guideline 5.1.1(v)) | สูง | เตรียมเหตุผลว่าบัญชีจำเป็นต่อการ sync ความชอบและประวัติ, มีทางถอย: ย้าย login ไปหลังผลแนะนำ (ค2) โดยไม่ต้องแก้โครงสร้าง |
| R-007 | Drop-off ที่หน้า login ไม่ปรากฏใน TTY และ completion rate | กลาง | Guardrail `auth_completion_rate` ใน ANALYTICS_PLAN §4 |
| R-005 | ขอบเขตบาน (Kampai จริง, Plus) ก่อนพิสูจน์ Solo | กลาง | Stage gate, GRP-001 เป็น Could |
