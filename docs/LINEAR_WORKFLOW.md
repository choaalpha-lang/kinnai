# KinNai — Linear Workflow

สถานะปัจจุบัน: **Linear ยังไม่ได้เชื่อม** (ไม่มี Linear tool ใน session ที่จัดทำเอกสารนี้) ทุกอย่างด้านล่างเป็นข้อเสนอ ยังไม่มี issue จริงและไม่มี Linear ID

## 1. โครงสร้างที่เสนอ

| ระดับ | ค่า |
|---|---|
| Workspace | Yimio |
| Team | Product Engineering |
| Project | KinNai |
| Milestones | Gate 1 Spec · Gate 2 Architecture · MVP1 Build · Gate 3 Evidence · Gate 4 Beta |
| Labels | `mvp1`, `solo`, `kampai`, `platform`, `privacy`, `security`, `analytics`, `cost`, `spike`, `prototype`, `release`, `ops` |

## 2. สถานะของ issue

Backlog → Ready → In Progress → In Review → Done (+ Blocked)

| สถานะ | เงื่อนไขเข้า | ใครย้าย |
|---|---|---|
| Backlog | มี requirement ID และคำอธิบาย | Claude Code เสนอ, Founder อนุมัติให้สร้าง |
| Ready | AC ได้รับอนุมัติ, dependency ปิดแล้ว, gate ที่เกี่ยวข้องผ่าน | Founder |
| In Progress | มี branch ที่อ้าง issue | Claude Code |
| In Review | PR เปิดแล้ว, CI ผ่าน, แนบหลักฐาน test | Claude Code |
| Done | Founder ยอมรับตาม Definition of Done | Founder |
| Blocked | ระบุสิ่งที่รอและเจ้าของ | ใครก็ได้ |

## 3. เนื้อหาที่ทุก issue ต้องมี

Business value · User story · AC แบบ Given/When/Then · Edge cases · Dependencies · Analytics · Test cases · Security · Definition of Done
แหล่งข้อมูลคือ `REQUIREMENTS.md` issue ลิงก์กลับมาที่หัวข้อนั้น ไม่คัดลอกจนเกิดสองฉบับที่ไม่ตรงกัน

## 4. Traceability

Requirement ID (คงที่ เช่น `SOLO-003`) ≠ Linear issue ID (Linear ออกให้)

```
Requirement → Linear Issue → GitHub PR → Test Evidence → Release
SOLO-003    → (รอสร้าง)    → PR #…     → CI run / manual → build …
```

- ชื่อ issue ขึ้นต้นด้วย `[REQ-ID]`
- ชื่อ branch: `<linear-id>-<req-id>-<slug>` เมื่อมี Linear ID; ก่อนหน้านั้นใช้ `<req-id>-<slug>`
- PR description ระบุ requirement ID, Linear ID และรายการ AC ที่ครอบคลุม
- ตาราง traceability อยู่ใน `LINEAR_BACKLOG.md` และอัปเดตเมื่อมี ID จริง

## 5. กติกาของ Claude Code

1. ตรวจข้อมูลที่มีใน Linear ก่อนสร้างหรือแก้ issue และขออนุมัติ Founder ก่อนทุกครั้ง
2. ไม่แต่ง Linear ID และไม่อ้างว่า sync แล้วถ้ายังไม่ได้ทำ
3. ถ้าไม่มี Linear tool ให้อัปเดต `LINEAR_BACKLOG.md` และ `LINEAR_IMPORT.csv` แทน
4. ไม่ย้าย issue เป็น Done เอง

## 6. การ import CSV

`LINEAR_IMPORT.csv` ใช้คอลัมน์ Title, Description, Priority, Status, Labels, Estimate ต้องตรวจการ map คอลัมน์กับตัว importer ของ Linear เวอร์ชันปัจจุบันตอน import จริง (ยังไม่ได้ทดสอบ import) หลัง import ให้กรอก Linear ID กลับใน `LINEAR_BACKLOG.md`
