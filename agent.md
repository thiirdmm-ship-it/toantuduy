# AGENT.MD — MMABC Đông Mỹ Website
> Đọc file này TRƯỚC KHI làm bất cứ thứ gì. Không hỏi lại những gì đã có ở đây.

---

## 1. PROJECT SNAPSHOT

| Key | Value |
|-----|-------|
| Project | Website lớp toán tư duy MMABC – Đông Mỹ |
| Stack | HTML + TailwindCSS CDN + Vanilla JS |
| Repo | github.com/thiirdmm-ship-it/toantuduy |
| Deploy | Netlify (chưa live) |
| Form backend | Google Sheets + Apps Script |
| Chatbot | Gemini 2.0 Flash API |

---

## 2. FILE STRUCTURE

```
/
├── index.html                          # Landing page (trang chính)
├── ve-chung-toi.html                   # Giới thiệu giáo viên & phương pháp
├── goc-hoc-tap.html                    # Blog / góc học tập (danh sách bài)
├── ket-qua.html                        # Kết quả học sinh & social proof
├── cam-on.html                         # Thank you page (sau khi đăng ký)
├── bai-viet-mau.html                   # Template bài viết blog (tham khảo)
├── toan-tu-duy-mmabc-la-gi.html        # Bài viết: Toán tư duy MMABC là gì?
├── con-ghet-toan-ba-me-nen-lam-gi.html # Bài viết: Con nói "con ghét toán"
├── do-vui-toan-tu-duy.html             # Bài viết: 3 câu đố toán tư duy
├── netlify.toml                        # Netlify config (publish dir & redirect)
├── agent.md                            # File hướng dẫn cho AI Agent
└── images/
    ├── logo.png             # Logo trung tâm (chatbot avatar)
    ├── favicon-round.png    # Favicon tròn cho tab browser
    ├── hero_section.jpg     # Ảnh lớp học thật (hero section)
    └── co_giao.jpg          # Ảnh giáo viên
```

---

## 3. DESIGN SYSTEM — KHÔNG THAY ĐỔI

```css
/* Màu chính */
--primary:     #FF7A2F  /* cam chủ — buttons, accents */
--primary-dark:#C95B00  /* cam đậm — logo text */
--navy:        #1A2B3C  /* navy — headings, footer bg */
--ivory:       #FFF8F0  /* kem — page background */
--ivory-light: #FFF0E0  /* kem nhạt — card backgrounds */

/* Font */
font-family: "Public Sans", sans-serif  /* DUY NHẤT — không dùng font khác */

/* Border */
border: 0.5px solid #FFD9B0  /* orange tint border */
border-radius: 12px           /* card standard */
border-radius: 20px           /* pill buttons */
```

---

## 4. NAVBAR — ĐỒNG NHẤT TẤT CẢ TRANG

```html
<!-- CẤU TRÚC CHUẨN — thứ tự link: Trang chủ → Giới thiệu → Góc học tập → Học phí → Kết quả → CTA -->
<div class="site-nav-links">
  <a href="index.html">Trang chủ</a>           <!-- nav-active trên index.html -->
  <a href="ve-chung-toi.html">Giới thiệu</a>   <!-- nav-active trên ve-chung-toi.html -->
  <a href="goc-hoc-tap.html">Góc học tập</a>   <!-- nav-active trên goc-hoc-tap & bài viết con -->
  <a href="index.html#hoc-phi">Học phí</a>
  <a href="ket-qua.html">Kết quả</a>
  <a href="index.html#dang-ky" class="site-nav-cta">Đăng ký học thử</a>
</div>

<!-- ACTIVE STATE: thêm class="nav-active" vào link tương ứng với trang hiện tại -->
<!-- cam-on.html: không có active state -->
```

---

## 5. THÔNG TIN THẬT — DÙNG CHÍNH XÁC

```yaml
Giáo viên:    Nguyễn Thị Bích Liên
Kinh nghiệm:  3 năm giảng dạy toán tư duy
Độ tuổi:      4–12 tuổi
Sĩ số:        Tối đa 12 bạn / lớp
Học phí:      Từ 499.000đ / tháng
Lộ trình:     24 buổi · 3 tháng · 90 phút/buổi
Học thử:      Miễn phí buổi đầu — không cam kết

Địa chỉ:      Khu đấu giá Đông Mỹ, xã Nam Phù, Thành phố Hà Nội
Zalo/SĐT:     0825 797 789  →  https://zalo.me/0825797789
Email:        mmabcdongmy@gmail.com
Facebook:     https://www.facebook.com/mmabcdongmy
```

---

## 6. 3 CHƯƠNG TRÌNH CHÍNH

| Chương trình | Độ tuổi | Mô tả ngắn |
|---|---|---|
| Toán tư duy tích hợp toán trường MMABC | 4–12 tuổi | Tư duy logic + song song chương trình trường |
| Luyện chữ đẹp | 4–12 tuổi | Nét chữ chuẩn, đúng tư thế, thói quen viết tốt |
| Hành trang cho bé vào lớp 1 | 4–6 tuổi | Chuẩn bị toàn diện trước khi vào lớp 1 |

---

## 7. FORM ĐĂNG KÝ

```html
<!-- index.html — form section id="dang-ky" -->
<form id="registration-form">
  <!-- Fields: ten-phu-huynh, ten-hoc-sinh, so-dien-thoai, buoi-hoc -->
</form>

<!-- JS: fetch với mode no-cors → Apps Script URL -->
<!-- Apps Script: https://script.google.com/macros/s/AKfycbx4KNi0xYg91r6ugHnhy9coU_4FdC4SmRb2tESuUnMW8jg63MAhEIeIbjiVoAccA_N6/exec -->
<!-- Redirect sau submit: cam-on.html -->
<!-- LƯU Ý: Xóa mode:'no-cors' sau khi deploy Netlify -->
```

---

## 8. CHATBOT

```javascript
// Có trên tất cả trang (kể cả các trang bài viết) — id: mmabc-chatbot-bubble
// Gemini API Key: AIzaSyBqkGZf9TQZR-aHXmS73k17Kjcj0ffJGrg  ← CẦN CHUYỂN SANG BACKEND PROXY
// Model: gemini-2.0-flash (1500 req/ngày free)
// FAQ cố định: 6 câu (không gọi API)
// Fallback: Zalo 0825 797 789
// Avatar: images/logo.png (border-radius: 8px, không tròn)
```

---

## 9. SECTIONS QUAN TRỌNG TRONG INDEX.HTML

```
id="hero"       → hero section
id="gioi-thieu" → section giới thiệu (phương pháp/đặc điểm)
id="lich-hoc"   → section giáo viên chủ nhiệm & lịch học
id="hoc-phi"    → section học phí
id="dang-ky"    → form đăng ký học thử
```

---

## 10. QUY TẮC KHI SỬA CODE

```
✓ LUÔN dùng Public Sans — không thay bằng Inter, Roboto, Nunito, Quicksand
✓ LUÔN dùng đúng màu hex ở mục 3
✓ LUÔN output cả file hoàn chỉnh — không chỉ snippet
✓ Khi sửa 1 trang → kiểm tra navbar có đúng cấu trúc mục 4 không
✓ Khi thêm section mới → dùng bg-[#FFF8F0] hoặc bg-white xen kẽ
✓ Khi thêm button → dùng bg-[#FF7A2F] text-white rounded-full
✓ Bài viết mới → tạo file riêng, cập nhật card trong goc-hoc-tap.html

✗ KHÔNG thêm font mới
✗ KHÔNG dùng gradient (trừ hero đã có sẵn)
✗ KHÔNG thay đổi thông tin liên hệ trừ khi được yêu cầu rõ ràng
✗ KHÔNG xóa chatbot widget khi sửa file
✗ KHÔNG xóa favicon link trong <head>
```

---

## 11. GIT WORKFLOW

```bash
# Sau mỗi lần sửa xong:
git add .
git commit -m "mô tả ngắn thay đổi"
git push origin main
# Netlify tự deploy sau khi push
```

---

## 12. VIỆC CÒN LẠI (TODO)

- [ ] Xóa `mode: 'no-cors'` trong form fetch sau khi Netlify live
- [ ] Test form sau khi Netlify live
- [ ] Tạo _private/index.html — dashboard quản trị
- [ ] Điền thông tin lịch học cụ thể (thứ mấy, giờ nào)
- [ ] Chuyển Gemini API key sang backend proxy (bảo mật)
- [ ] Thêm ảnh thật vào 3 bài viết (placeholder hiện dùng emoji)
- [x] Thêm ảnh giáo viên Nguyễn Thị Bích Liên vào ve-chung-toi.html
- [x] Tạo bai-viet-mau.html — template bài viết blog
- [x] Đồng bộ font Public Sans cho index.html
- [x] Thêm OG meta tags cho tất cả trang
- [x] Sửa màu rogue #ec5b13 → #FF7A2F trong cam-on.html
- [x] Thêm "Trang chủ" làm link đầu tiên trong navbar tất cả trang
- [x] Tạo netlify.toml (publish dir + redirect rule)
- [x] Tạo toan-tu-duy-mmabc-la-gi.html
- [x] Tạo con-ghet-toan-ba-me-nen-lam-gi.html
- [x] Tạo do-vui-toan-tu-duy.html

---
*Cập nhật lần cuối: Tháng 3/2026 — v1.3*

## CHANGELOG

### v1.3 — Tháng 3/2026
- ✅ 3 bài viết mới, navbar thêm Trang chủ, netlify.toml

### v1.2 — Tháng 3/2026
- ✅ Audit fixes: font, màu, OG tags, stray text, cam-on.html

