# KinNai — Business Model Canvas

ที่มา: Blueprint V1.2 · ราคา รายได้ และต้นทุนทั้งหมดเป็น**ข้อเสนอเพื่อทดลอง** ไม่ใช่ผลที่ยืนยันแล้ว
รูปแบบ: Consumer-first freemium, เติบโตด้วย referral, รายได้จากร้านค้าเป็นทางเลือกในอนาคต ไม่ใช่เงื่อนไขของ MVP

| ช่อง | เนื้อหา | สถานะหลักฐาน |
|---|---|---|
| Customer Segments | Solo diners (พนักงานออฟฟิศ นักศึกษา) · Group organizers · Group participants | สมมติฐาน ยังไม่มี user research ใน repo |
| Value Proposition | Solo: ได้ร้านใน 30 วินาที · Group: สรุปใน 5 นาที · คำนึงถึงงบ สถานที่ ความชอบ | H-001, H-002, H-004 |
| Channels | App Store / Google Play · LINE share + referral · content และ word of mouth | ยังไม่ทดสอบ |
| Customer Relationships | Self-service · คำแนะนำเฉพาะบุคคล · automated support | — |
| Revenue Streams | Plus ฿99/เดือน (เสนอ) · Kampai Premium ฿39/ครั้ง (เสนอ) · ภายหลัง: affiliate, sponsorship, Merchant Pro | H-003 ไม่มีรายได้ใน MVP1 |
| Key Resources | แอป, ตรรกะแนะนำ, ข้อมูลและ analytics, Founder, AI tools, แบรนด์ | — |
| Key Activities | Ship และทดสอบฟีเจอร์ · ปรับ Time to Yes · ติดตาม retention และ unit economics | — |
| Key Partners | Places/maps data provider (ยังไม่เลือก Q-001) · cloud, app stores, billing | Q-001 เปิดอยู่ |
| Cost Structure | Cloud, maps, AI usage · ค่า store และ payment · growth, support, security | ตัวเลข UNKNOWN รอ SPIKE-001 และ Gate 2 |

## Growth loop (MVP2, ไม่อยู่ใน MVP1)

ชวนเพื่อน 3 คนที่สมัครและใช้งานจริง → ได้ Kampai Premium ฟรี 1 ครั้ง ต้องออกแบบ anti-fraud และเพดานงบรางวัลก่อนเปิด

## ข้อสังเกตต่อโมเดล

1. รายได้ทั้งสองสายผูกกับฟีเจอร์ที่ยังไม่มีใน MVP1 ดังนั้น MVP1 พิสูจน์ได้เฉพาะคุณค่าและการใช้ซ้ำ ไม่ได้พิสูจน์ความยินดีจ่าย
2. ต้นทุนผันแปรหลักคือค่า Places API ต่อการค้นหา ซึ่งเกิดกับผู้ใช้ฟรีด้วย ต้องรู้ต้นทุนต่อการตัดสินใจก่อนกำหนดขอบเขตของ free tier
3. Sponsored result ในอนาคตขัดกับคำสัญญา "ร้านที่ตรงที่สุด" ได้ blueprint กำหนดให้ติดป้ายชัดเจน และควรแยกออกจาก 3 ร้านที่แนะนำ
