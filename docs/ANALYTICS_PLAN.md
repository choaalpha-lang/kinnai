# KinNai MVP1 — Analytics Plan

เครื่องมือ: PostHog · อ้างอิงการตัดสินใจ: D-001, D-003, D-004, D-016 ถึง D-022, A-001 ถึง A-003, A-007 ถึง A-012 ใน `DECISION_LOG.md`
เป้าทุกตัวเป็นสมมติฐานที่ยังไม่ได้พิสูจน์ ต้นแบบ HTML แสดง event ชุดหลักเหล่านี้ในแผง "Analytics events" (property บางตัว เช่น `time_to_criteria_ms` มีเฉพาะใน spec)

## 1. Event schema

Property ร่วมทุก event: `user_id` (Supabase user UUID ไม่ใช่ account ID ของ Apple/Google), `session_id`, `app_version`, `platform`, `lang`, `client_ts`, server timestamp ของ PostHog · event ของกลุ่มมี `group_id` และ `member_pseudo_id` (รหัสไม่ระบุตัวตนที่ระบบสร้างเอง ไม่ใช่ชื่อหรือ account ID)
ห้ามมีใน event: พิกัด, ชื่อ, เบอร์โทร, free-text

### Auth (D-017: ขอหลังตัดสินใจสำเร็จ)

| Event | เกิดเมื่อ | Properties |
|---|---|---|
| `auth_screen_shown` | ข้อเสนอ login แสดง | `platform`, `trigger` (post_decision_solo / post_decision_group / me_tab) |
| `auth_dismissed` | กด "ไว้ทีหลัง" | `trigger` |
| `auth_method_selected` | แตะปุ่ม Apple หรือ Google | `method` |
| `auth_completed` | เข้าสู่ระบบสำเร็จ | `method`, `is_new_user`, `duration_ms` |
| `auth_cancelled` / `auth_failed` | ยกเลิกหรือผิดพลาด | `method`, `reason` |
| `identity_linked` / `identity_unlinked` | เชื่อมหรือยกเลิกช่องทาง | `method` |
| `signed_out`, `account_deleted` | ออกจากระบบ, ลบบัญชี | `scope` |

ตาม D-017 ผู้ใช้ใช้งานและตัดสินใจได้ทั้งหมดโดยไม่ login ทุก event จึงเกิดได้ก่อน login ส่งด้วย anonymous user ของ Supabase และ ID ของ PostHog แล้วเชื่อมกับบัญชีเมื่อ login สำเร็จ การขอ login เกิดหลังตัดสินใจจึงไม่อยู่ในช่วงเวลาของ TTY (ไม่มี `included_auth` อีกแล้ว) ทั้งหมดอยู่ใต้ consent analytics เดียวกัน

### Solo

| Event | เกิดเมื่อ | Properties |
|---|---|---|
| `session_start` | หน้าแรกแสดงและแอปอยู่ foreground | `location_status` (granted/denied/not_asked), `ref_code` (ไม่บังคับ: รหัสกลุ่มต้นทางแบบกว้าง เช่น `friends-a` จากพารามิเตอร์ในลิงก์ ห้ามเป็นชื่อหรือ ID ของบุคคล) |
| `location_permission` | ผู้ใช้ตอบคำขอสิทธิ์ | `status` |
| `area_selected` | เลือกย่านเอง | `area` |
| `preference_set` | เปลี่ยนงบ/แนว/ระยะ | ค่าที่เปลี่ยน |
| `criteria_completed` | กด "ช่วยเลือกให้หน่อย" | `budget`, `cuisine`, `radius_km`, `location_source`, `is_first_in_session` |
| `recommendation_requested` | ส่งคำขอค้นหา | เหมือนข้างบน |
| `shortlist_shown` | ผลแสดง | `count` (0–3), `place_ids`, `latency_ms` |
| `shortlist_rerolled` | ขอชุดใหม่ | `reroll_index` |
| `filters_relaxed` | ขยายเงื่อนไข | ค่าใหม่ |
| `recommendation_failed` | error/timeout/cap | `reason` |
| `decision_confirmed` | กด "กินร้านนี้" | `decision_id`, `place_id`, `time_to_yes_ms`, `time_to_criteria_ms`, `rerolls`, `backgrounded`, `within_target` |
| `session_abandoned` | เริ่มมื้อใหม่โดยไม่ยืนยัน หรือเงียบเกิน 10 นาที | `at_screen`, `had_shortlist` |
| `map_opened` | เปิดแผนที่ | `decision_id`, `provider` |
| `feedback_submitted` | 👍/👎 | `decision_id`, `rating` |
| `feedback_reason` | เลือกเหตุผล | `decision_id`, `reason` |

### Kampai (D-016 ใช้จริงใน MVP1)

| Event | เกิดเมื่อ | Properties |
|---|---|---|
| `group_created` | ผู้จัดสร้างกลุ่ม | `group_id`, `budget`, `area`, `source_group_id` (มีเมื่อผู้จัดคนนี้เคยเป็นผู้ร่วมกลุ่มอื่นมาก่อน), `organizer_logged_in` |
| `group_invite_shared` | ผู้จัดแชร์ลิงก์ | `group_id`, `channel` (web_share / copy_link) |
| `group_link_opened` | มีคนเปิดลิงก์ | `group_id`, `member_pseudo_id`, `is_new_pseudo_id`, `is_existing_kinnai_user` (ใช้คำนวณ saturation), `in_app_browser` |
| `group_joined` | ผู้ร่วมกลุ่มกดเข้าร่วม | `group_id`, `member_pseudo_id` |
| `group_preference_set` | ผู้ร่วมกลุ่มเลือกประเภทอาหาร | `group_id`, `cuisine_count` |
| `group_decision_start` | ผู้จัดกด "เริ่มโหวต" หรือถึงเวลาเริ่มอัตโนมัติ | `group_id`, `members` |
| `group_shortlist_shown` | รายการโหวตแสดง | `group_id`, `count`, `latency_ms` |
| `group_vote_cast` | โหวต (นับการเปลี่ยนเสียงด้วย) | `group_id`, `member_pseudo_id`, `is_change` |
| `group_vote_closed` | ปิดโหวต | `group_id`, `votes`, `tie`, `abstained`, `closed_by` (organizer / deadline) |
| `group_tie_broken` | ผู้จัดเลือกเมื่อเสมอ | `group_id`, `by: organizer` |
| `group_tie_reminder_shown` | แสดงคำเตือนผู้จัดเมื่อเสมอ (D-023) | `group_id` |
| `group_closed_no_result` | ผู้จัดไม่เลือกภายใน 5 นาที กลุ่มปิดโดยไม่มีผล | `group_id`, `reason: tie_unresolved` |
| `group_vote_restarted` | ผู้จัดเริ่มโหวตใหม่ | `group_id` |
| `group_deadline_reached_below_quorum` | ถึงเวลาปิดแต่มีเสียงน้อยกว่า 2 | `group_id`, `voices` |
| `group_deadline_extended` | ผู้จัดขยายเวลา | `group_id` |
| `group_decision_confirmed` | ผู้จัดยืนยันผล | `group_id`, `time_to_yes_ms`, `within_5min`, `abstained`, `tie_broken_by_organizer` |
| `group_result_shared` | แชร์การ์ดสรุป | `group_id`, `channel` |
| `group_cta_create_clicked` | กด "จัดนัดของคุณเอง" | `group_id`, `source_group_id` |
| `group_cancelled` / `group_expired` | ยกเลิกหรือหมดอายุ | `group_id`, `at_step`, `elapsed_ms` |
| `group_member_left` | ผู้ร่วมกลุ่มลบการเข้าร่วมของตน | `group_id` |

## 2. นิยาม KPI ของ Solo (ตัวชี้วัดรอง ตาม D-016)

**Successful Solo Decision** = จำนวน `decision_confirmed` ที่ไม่ซ้ำ `decision_id` การดูผลโดยไม่ยืนยันไม่นับ (North Star รวมอยู่ใน §3)

**Solo Time to Yes** = `decision_confirmed.ts − criteria_completed.ts` โดยใช้ `criteria_completed` ที่ `is_first_in_session = true` (D-001, A-001)
- รายงาน median และ P90 ต่อสัปดาห์ เป้า median ≤ 30 วินาที
- การขอ login เกิดหลังตัดสินใจ (D-017) จึงไม่อยู่ในช่วงเวลานี้
- คำนวณจากช่วงเวลาที่วัดฝั่ง client (monotonic) และตรวจกับ server timestamp ถ้าต่างเกิน 5 วินาทีให้ติดธงและตัดออกจาก P90

**Decision completion rate** = sessions ที่มี `decision_confirmed` ÷ sessions ที่มี `criteria_completed`
- ต้องรายงานคู่กับ TTY ทุกครั้ง เพราะ TTY คำนวณเฉพาะ session ที่สำเร็จ (survivorship bias)

**Abandonment rate** = sessions ที่มี `session_abandoned` ÷ sessions ที่มี `criteria_completed`

**Session** เริ่มที่ `session_start` จบเมื่อ `decision_confirmed`, `session_abandoned` หรือไม่มี event 10 นาที (ค่า 10 นาทีเป็นข้อเสนอ)

## 3. นิยาม KPI ของ Kampai และ gate ของ MVP1 (D-020)

**North Star (D-020):** Successful Decisions = `group_decision_confirmed` ที่ไม่ซ้ำ `group_id` รวม `decision_confirmed` ของ Solo รายงานแยกสองชนิดเสมอ

**Group Time to Yes (Core Habit Metric)** = `group_decision_confirmed.ts − group_decision_start.ts` เป้า ≤ 5 นาที (A-012)
**End-to-end group time (guardrail)** = `group_decision_confirmed.ts − group_created.ts` ไม่ให้เวลารอเพื่อนเข้ากลุ่มซ่อนอยู่นอก KPI

**Group completion rate** = กลุ่มที่ยืนยัน ÷ กลุ่มที่มี `group_decision_start` (กลุ่มที่ยกเลิกหรือหมดอายุอยู่ในตัวหาร) กลุ่มที่ปิดโดยไม่มีผลแล้วเริ่มโหวตใหม่นับเป็นกลุ่มเดิมและนาฬิกา Group TTY ไม่รีเซ็ต (D-023) ส่วนกลุ่มที่ปิดโดยไม่มีผลและไม่ได้เริ่มใหม่ถือว่าไม่สำเร็จและอยู่ในตัวหาร
**Participation rate** = สมาชิกที่โหวต ÷ สมาชิกทั้งหมดที่เข้ากลุ่ม ผู้งดออกเสียงอยู่ในตัวหาร (D-004, A-003)
**Tie rate** = กลุ่มที่ `tie = true` ÷ กลุ่มที่ปิดโหวต ใช้ติดตามว่า D-003 ถูกใช้บ่อยแค่ไหน

### Gate วัน 1–30: Organic distribution

| ตัวชี้วัด | นิยามที่ใช้ | เกณฑ์ (strategy) |
|---|---|---|
| ผลภายใน 5 นาที | กลุ่มที่ `group_decision_confirmed` ภายใน 5 นาทีหลัง `group_decision_start` ÷ กลุ่มที่มี `group_decision_start` (ยกเลิกและหมดอายุอยู่ในตัวหาร) | ≥ 80% |
| **K = I × A × H** | ดูด้านล่าง | ≥ 1.2 |

**นิยามของ K** (เก็บต่อกลุ่มที่ผู้จัดคนหนึ่งสร้าง)
- **I** = จำนวนผู้เปิดลิงก์ที่ไม่ซ้ำ (`group_link_opened` นับ `member_pseudo_id` ที่ไม่ซ้ำ) ใช้เป็นตัวแทนของ "ผู้รับเชิญ" เพราะจำนวนคนที่เห็นข้อความในแชตวัดไม่ได้ ข้อจำกัด: ผู้ที่เห็นลิงก์แต่ไม่เปิดไม่ถูกนับ ทำให้ A ดูสูงกว่าที่ strategy คิด แต่ K ซึ่งเท่ากับ "จำนวนผู้จัดใหม่ที่เกิดจากกลุ่มหนึ่ง" ไม่ได้รับผลกระทบ
- **A** = ผู้ที่ `group_joined` ÷ I
- **H** = ผู้ร่วมกลุ่มที่ภายหลังเป็น "ผู้จัดใหม่" ÷ ผู้ร่วมกลุ่มที่ `group_joined`
- **ผู้จัดใหม่** = `member_pseudo_id` ที่ `group_created` ครั้งแรกภายใน 14 วันหลังเข้าร่วมกลุ่มต้นทาง (14 วันเป็นข้อเสนอ) โดยก่อนหน้านั้นไม่เคยเป็นผู้จัดและไม่เคยเป็นสมาชิกของกลุ่มอื่นใน KinNai
- กลุ่มที่เริ่มหลังวันที่ 16 ของช่วงทดสอบจะยังวัด H ไม่ครบเมื่อสิ้นวันที่ 30 ต้องรายงานว่ายังไม่ครบ

**กติกากัน False Virality**
- นับเฉพาะผู้ใช้และกลุ่มที่ไม่ซ้ำ หักผู้เชิญกันเองภายในเครือข่ายเดิม
- รายงาน **network saturation** = ผู้เปิดลิงก์ที่เป็นสมาชิกกลุ่มอื่นของ KinNai อยู่แล้ว ÷ ผู้เปิดลิงก์ทั้งหมด (`is_existing_kinnai_user`)
- รายงาน K พร้อมช่วงความเชื่อมั่นและจำนวนกลุ่มที่ใช้ ขนาดตัวอย่างขั้นต่ำยังไม่กำหนด (UNKNOWN) ไม่สรุป gate จากข้อมูลน้อยเกินไป (R-011)
- รายงานแยกตามเครือข่ายต้นทาง (`ref_code`) เพื่อดูว่ามีเครือข่ายเดียวที่ผลักค่า K หรือไม่ (D-022)
- บุคคลเดียวกันข้ามอุปกรณ์หรือหลังล้างเบราว์เซอร์รวมกันไม่ได้ (A-010) ต้องระบุข้อจำกัดนี้ทุกครั้งที่รายงาน

### Gate วัน 31–60: Repeat behavior

**Organizer repeat** = ผู้จัดที่สร้างกลุ่มที่สองภายใน 30 วันหลัง `group_decision_confirmed` ของกลุ่มแรก ÷ ผู้จัดที่กลุ่มแรกสำเร็จ เกณฑ์ ≥ 30% รายงานแยกผู้จัดที่ login กับไม่ login (R-007)

### Gate วัน 61–90: Willingness to pay

วัดนอก app ใน pilot manual (จ่ายซ้ำหรือต่อสัญญา และ ROI) ไม่อยู่ใน analytics ของ MVP1 (D-018)

## 4. Guardrails

| Guardrail | นิยาม | เหตุผล |
|---|---|---|
| Login prompt conversion | ผู้ที่ `auth_completed` ÷ ผู้ที่ `auth_screen_shown` แยกตาม trigger, platform, method และ `in_app_browser` | D-017 ขอ login หลังตัดสินใจ ถ้าต่ำ การวัด repeat จะไม่ครบ (R-007) |
| Join friction | ผู้ที่ `group_vote_cast` ภายใน 3 แตะหลัง `group_link_opened` ÷ ผู้ที่ `group_link_opened` (วัดจำนวนแตะ) | H-009 และพารามิเตอร์ A ของ K |
| Network saturation | ดู §3 | R-011 |
| End-to-end group time | ดู §3 | ไม่ให้เวลารอซ่อนอยู่นอก KPI |
| Cost per group | ค่า Places API ต่อกลุ่ม | R-014, H-006 |
| Time to Criteria | `criteria_completed.ts − session_start.ts` (median, P90) | D-001 ตัดช่วงนี้ออกจาก TTY จึงต้องดูแยก ไม่ให้เวลาย้ายไปซ่อนที่หน้าแรก |
| End-to-end time | `decision_confirmed.ts − session_start.ts` | ตัวเลขที่ตรงกับคำสัญญา "30 วินาที" ในมุมผู้ใช้ |
| Relevance | สัดส่วน 👍 ต่อ feedback ทั้งหมด, อัตรา reroll ต่อ session | ตัดสินใจเร็วแต่ไม่ถูกใจไม่ถือว่าสำเร็จ |
| Empty/insufficient rate | `shortlist_shown` ที่ `count < 3` ÷ ทั้งหมด | วัดความครอบคลุมของข้อมูลร้าน |
| Failure rate | `recommendation_failed` ÷ `recommendation_requested` | เสถียรภาพของ provider |
| Cost per decision | ค่า API ของวัน ÷ `decision_confirmed` ของวัน | H-006 |
| Crash-free sessions | จาก Sentry | NFR-02 |
| Founder hours | บันทึกเองรายสัปดาห์ | H-005 |

## 5. ตัวชี้วัดธุรกิจ (เตรียมโครงไว้ ยังไม่มีข้อมูลใน MVP1)

Retention D1/D7/D30 จาก `decision_confirmed` และ `group_decision_confirmed` · ตัวชี้วัดรายได้ (Intent Exchange, Workplace) วัดใน pilot manual นอกผลิตภัณฑ์ ไม่อยู่ใน analytics ของ MVP1 (D-018)

## 6. Data quality

- `decision_id` ซ้ำถูกตัดที่ server และไม่ส่ง event ซ้ำ
- Event ที่ส่งแบบ offline ใช้ `client_ts` เดิม
- บัญชีทดสอบและ build ภายในติด `is_internal = true` และตัดออกจาก dashboard
- ข้อมูลจากต้นแบบ HTML ไม่นำมารวมกับข้อมูล beta
- ผู้ทดสอบกลุ่มแรกเป็นคนรู้จักและมาจาก referral ใน 2–3 เครือข่ายที่ไม่ทับซ้อน (D-015, D-022) กลุ่มตัวอย่างเล็กและไม่เป็นตัวแทนของผู้ใช้ทั่วไป ตัวเลข median/P90 ของ Solo TTY ใช้ดูทิศทางและหาจุดติดขัด ไม่ใช้เป็นหลักฐานยืนยัน H-001 และผู้ทดสอบอาจให้ feedback ในทางบวกเกินจริง จึงควรดูพฤติกรรมจริง (completion, reroll, abandonment) ควบคู่กับ 👍/👎
- ขนาดกลุ่มตัวอย่างและเกณฑ์ผ่านกำหนดใน UXR-001 (Q-013)
