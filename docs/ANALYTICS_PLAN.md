# KinNai MVP1 — Analytics Plan

เครื่องมือ: PostHog · อ้างอิงการตัดสินใจ: D-001, D-003, D-004, A-001 ถึง A-003 ใน `DECISION_LOG.md`
เป้าทุกตัวเป็นสมมติฐานที่ยังไม่ได้พิสูจน์ ต้นแบบ HTML แสดง event ชุดหลักเหล่านี้ในแผง "Analytics events" (property บางตัว เช่น `time_to_criteria_ms` มีเฉพาะใน spec)

## 1. Event schema

Property ร่วมทุก event: `user_id` (anonymous UUID), `session_id`, `app_version`, `platform`, `lang`, `client_ts`, server timestamp ของ PostHog
ห้ามมีใน event: พิกัด, ชื่อ, เบอร์โทร, free-text

### Solo

| Event | เกิดเมื่อ | Properties |
|---|---|---|
| `session_start` | หน้าแรกแสดงและแอปอยู่ foreground | `location_status` (granted/denied/not_asked) |
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
| `user_data_deleted` | ลบข้อมูลสำเร็จ | `scope` |

### Kampai (เฉพาะต้นแบบใน MVP1 ใช้จริงใน MVP1.5)

`group_created`, `group_invite_shared`, `group_decision_start`, `group_shortlist_shown`, `group_vote_cast`, `group_vote_closed` (`votes`, `tie`, `abstained`), `group_tie_broken` (`by: organizer`), `group_vote_reopened`, `group_decision_confirmed` (`time_to_yes_ms`, `abstained`, `tie_broken_by_organizer`), `group_cancelled` (`at_step`, `elapsed_ms`)

## 2. นิยาม KPI ของ Solo

**Successful Food Decision (North Star)** = จำนวน `decision_confirmed` ที่ไม่ซ้ำ `decision_id` การดูผลโดยไม่ยืนยันไม่นับ

**Solo Time to Yes** = `decision_confirmed.ts − criteria_completed.ts` โดยใช้ `criteria_completed` ที่ `is_first_in_session = true` (D-001, A-001)
- รายงาน median และ P90 ต่อสัปดาห์ เป้า median ≤ 30 วินาที
- คำนวณจากช่วงเวลาที่วัดฝั่ง client (monotonic) และตรวจกับ server timestamp ถ้าต่างเกิน 5 วินาทีให้ติดธงและตัดออกจาก P90

**Decision completion rate** = sessions ที่มี `decision_confirmed` ÷ sessions ที่มี `criteria_completed`
- ต้องรายงานคู่กับ TTY ทุกครั้ง เพราะ TTY คำนวณเฉพาะ session ที่สำเร็จ (survivorship bias)

**Abandonment rate** = sessions ที่มี `session_abandoned` ÷ sessions ที่มี `criteria_completed`

**Session** เริ่มที่ `session_start` จบเมื่อ `decision_confirmed`, `session_abandoned` หรือไม่มี event 10 นาที (ค่า 10 นาทีเป็นข้อเสนอ)

## 3. นิยาม KPI ของ Kampai

**Group Time to Yes** = `group_decision_confirmed.ts − group_decision_start.ts` เป้า ≤ 5 นาที
**Group completion rate** = กลุ่มที่ยืนยัน ÷ กลุ่มที่มี `group_decision_start` (กลุ่มที่ยกเลิกอยู่ในตัวหาร)
**Participation rate** = สมาชิกที่โหวต ÷ สมาชิกทั้งหมดที่เข้ากลุ่ม ผู้งดออกเสียงอยู่ในตัวหาร (D-004, A-003)
**Tie rate** = กลุ่มที่ `tie = true` ÷ กลุ่มที่ปิดโหวต ใช้ติดตามว่า D-003 ถูกใช้บ่อยแค่ไหน

## 4. Guardrails

| Guardrail | นิยาม | เหตุผล |
|---|---|---|
| Time to Criteria | `criteria_completed.ts − session_start.ts` (median, P90) | D-001 ตัดช่วงนี้ออกจาก TTY จึงต้องดูแยก ไม่ให้เวลาย้ายไปซ่อนที่หน้าแรก |
| End-to-end time | `decision_confirmed.ts − session_start.ts` | ตัวเลขที่ตรงกับคำสัญญา "30 วินาที" ในมุมผู้ใช้ |
| Relevance | สัดส่วน 👍 ต่อ feedback ทั้งหมด, อัตรา reroll ต่อ session | ตัดสินใจเร็วแต่ไม่ถูกใจไม่ถือว่าสำเร็จ |
| Empty/insufficient rate | `shortlist_shown` ที่ `count < 3` ÷ ทั้งหมด | วัดความครอบคลุมของข้อมูลร้าน |
| Failure rate | `recommendation_failed` ÷ `recommendation_requested` | เสถียรภาพของ provider |
| Cost per decision | ค่า API ของวัน ÷ `decision_confirmed` ของวัน | H-006 |
| Crash-free sessions | จาก Sentry | NFR-02 |
| Founder hours | บันทึกเองรายสัปดาห์ | H-005 |

## 5. ตัวชี้วัดธุรกิจ (เตรียมโครงไว้ ยังไม่มีข้อมูลใน MVP1)

Retention D1/D7/D30 จาก `decision_confirmed` · Paid conversion, ARPU, churn เริ่มวัดเมื่อมี MON-001 (MVP1.5)

## 6. Data quality

- `decision_id` ซ้ำถูกตัดที่ server และไม่ส่ง event ซ้ำ
- Event ที่ส่งแบบ offline ใช้ `client_ts` เดิม
- บัญชีทดสอบและ build ภายในติด `is_internal = true` และตัดออกจาก dashboard
- ข้อมูลจากต้นแบบ HTML ไม่นำมารวมกับข้อมูล beta
