# PROJECT STATUS — ASACLEAN Thailand

> ไฟล์นี้คือ "สมุดบันทึกสถานะงาน" สำหรับเว็บไซต์ ASACLEAN Thailand
> ใช้เพื่อทำงานต่อข้ามเครื่อง (PC ↔ Notebook) — เปิดอ่านไฟล์นี้ก่อนเริ่มงานทุกครั้ง
> **กติกา:** ทุกครั้งที่มีการแก้ไข/เพิ่มเนื้อหา ให้บันทึกลงหัวข้อ [Changelog](#changelog) ด้านล่างเสมอ

- **เว็บไซต์:** https://www.asaclean-thailand.com
- **โดเมน:** asaclean-thailand.com
- **โฮสติ้ง:** Hostinger (อัปโหลดเข้า `public_html`)
- **อัปเดตล่าสุด:** 2026-06-12 (sitemap)
- **เจ้าของ:** Jack — บริษัท จิตตรงเคมีภัณฑ์ จำกัด (CSC)

---

## 1. เป้าหมายงาน (Goals)

ทำให้เว็บไซต์ติดอันดับ 1 บน Google และ search engine อื่นๆ สำหรับคีย์เวิร์ดหลัก เช่น
"เม็ดล้างสกรู", "เม็ดล้างเครื่องฉีดพลาสติก", "ASACLEAN", "Purging Compound"
โดยเน้น SEO และ Sitemap ที่ถูกต้องเสมอ

---

## 2. โครงสร้างโฟลเดอร์ & Workflow การ Deploy

ทำงานบน PC ในโฟลเดอร์โปรเจกต์ แล้วอัปโหลดเฉพาะโฟลเดอร์ `deploy-hostinger/` ขึ้น Hostinger

```
asaclean-thailand_v02/          ← โฟลเดอร์โปรเจกต์หลัก (ต้นทาง)
├── PROJECT-STATUS.md           ← ไฟล์นี้ (สถานะงาน)
├── deploy-hostinger/           ← *** โฟลเดอร์ที่อัปโหลดขึ้น public_html ***
│   ├── index.html              ← หน้าหลัก (Landing page)
│   ├── articles/               ← บทความทั้งหมด (SEO content)
│   ├── sitemap.xml             ← แผนผังเว็บไซต์
│   ├── robots.txt              ← คำสั่งสำหรับ search bot
│   ├── .htaccess               ← Cache / Gzip / Force HTTPS
│   └── (รูปภาพ, วิดีโอ, favicon, og-image)
├── client/ server/ shared/     ← ซอร์ส React (template เดิม — ไม่ได้ใช้ deploy ตรง)
└── dist/                       ← build output (React)
```

**ขั้นตอน deploy:**
1. แก้ไฟล์ในโฟลเดอร์ `deploy-hostinger/` (หน้าหลัก) หรือ `deploy-hostinger/articles/` (บทความ)
2. อัปเดต `sitemap.xml` (วันที่ `lastmod`) ทุกครั้งที่มีการเปลี่ยนเนื้อหา
3. อัปโหลดไฟล์ที่แก้ขึ้น Hostinger → `public_html`
4. บันทึกลง [Changelog](#changelog)

> **หมายเหตุไฟล์ binary (รูป/วิดีโอ/.ico):** เมื่อเขียนทับไฟล์รูปภาพใน OneDrive ต้อง Read เพื่อ download ก่อน;
> ไฟล์ `.ico` มักต้องอัปโหลดเอง manual

---

## 3. ไฟล์ที่ Deploy แล้ว (Inventory)

### หน้าหลัก — `deploy-hostinger/`

| ไฟล์ | หน้าที่ | สถานะ |
|------|---------|--------|
| `index.html` | Landing page (157 KB) | ✅ Live |
| `sitemap.xml` | Sitemap 8 URLs | ✅ Live |
| `robots.txt` | อนุญาต bot + ชี้ sitemap | ✅ Live |
| `.htaccess` | Cache, Gzip, Force HTTPS | ✅ Live |
| `og-image.jpg` | รูป OG 1200×630 | ✅ Live |
| `logo-nav.webp` | โลโก้ navbar | ✅ Live |
| `screw-before.webp` / `screw-after.webp` | รูปเปรียบเทียบ ก่อน–หลัง | ✅ Live |
| `video-poster.jpg` + `rheology_asacleanR.mp4` | วิดีโอ rheology | ✅ Live |
| `favicon.ico` / `favicon-32.png` / `favicon-48.png` / `apple-touch-icon.png` | ไอคอน | ✅ Live |

### บทความ — `deploy-hostinger/articles/`

| ไฟล์ | หัวข้อ | priority |
|------|--------|----------|
| `index.html` | หน้ารวมบทความ | 0.9 |
| `carbon-black-specks-purging.html` | คราบดำ/จุดคาร์บอน | 0.85 |
| `mechanical-purging-explained.html` | Mechanical Purging | 0.85 |
| `purge-vs-rinse-asaclean.html` | Purge vs Rinse | 0.85 |
| `routine-purging-sealing.html` | Routine purging / sealing | 0.85 |
| `extrusion-purging-guide.html` | การล้างเครื่องรีด (Extrusion) | 0.85 |
| `oee-roi-injection-molding.html` | OEE / ROI | 0.85 |
| `style.css` | สไตล์บทความ | — |

---

## 4. สถานะ SEO

| รายการ | สถานะ | หมายเหตุ |
|--------|--------|----------|
| Title / Meta description | ✅ | ภาษาไทย มีคีย์เวิร์ดหลัก |
| Canonical URL | ✅ | https://asaclean-thailand.com/ |
| Open Graph + Twitter Card | ✅ | og-image 1200×630 |
| Schema.org (JSON-LD) | ✅ | Organization, LocalBusiness, WebSite, Product, FAQPage |
| Sitemap.xml | ✅ | 8 URLs + image sitemap |
| robots.txt | ✅ | Allow all + sitemap |
| HTTPS | ✅ | Force HTTPS ผ่าน .htaccess |
| Mobile / viewport | ✅ | responsive |

**ข้อมูลธุรกิจใน Schema:** โทร +66-83-590-7389 · info@asaclean-thailand.com · เปิด จ–ศ 08:30–17:30 · กรุงเทพฯ
**Social:** Facebook /AsacleanThailand · YouTube @asacsc · cscth.com

---

## 5. งานที่ต้องทำต่อ (TODO / Next steps)

- [x] ~~อัปเดต `lastmod` ใน `sitemap.xml` ให้ตรงกับวันที่แก้บทความ~~ (เสร็จ 2026-06-12 → ทุก URL = 2026-06-11)
- [ ] ส่ง sitemap เข้า Google Search Console + ขอ index หน้าใหม่ (อย่าลืม re-submit หลังอัปโหลด sitemap ใหม่)
- [ ] ตรวจ Core Web Vitals / PageSpeed (รูปใหญ่, วิดีโอ 2.4 MB)
- [ ] เพิ่ม internal links ระหว่างบทความ ↔ หน้าหลัก
- [ ] พิจารณาเพิ่มบทความใหม่ตามคีย์เวิร์ดที่ยังไม่ครอบคลุม

---

## 6. Changelog

> เพิ่มรายการใหม่ไว้บนสุด · รูปแบบ: `YYYY-MM-DD — สิ่งที่เปลี่ยน — ไฟล์ที่กระทบ`

- **2026-06-12** — แก้ `sitemap.xml`: ปรับ `lastmod` ทุก URL จาก 2026-06-10 → 2026-06-11 ให้ตรงกับวันแก้บทความ — `deploy-hostinger/sitemap.xml` (ต้องอัปโหลดขึ้น Hostinger)
- **2026-06-12** — สร้างไฟล์ `PROJECT-STATUS.md` เพื่อบันทึกสถานะงานข้ามเครื่อง
- **2026-06-11** — อัปเดตบทความทั้ง 6 หน้า + หน้ารวมบทความ, อัปเดต `index.html` และ `logo-nav.webp`
- **2026-06-10** — ตั้งค่า sitemap.xml (8 URLs), robots.txt, og-image, รูป screw before/after, favicon set
