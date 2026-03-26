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
├── index.html              # Landing page (trang chính)
├── ve-chung-toi.html       # Giới thiệu giáo viên & phương pháp
├── goc-hoc-tap.html        # Blog / góc học tập
├── ket-qua.html            # Kết quả học sinh & social proof
├── cam-on.html             # Thank you page (sau khi đăng ký)
├── agent.md                # File hướng dẫn cho AI Agent
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

## 4. NAVBAR — ĐỒNG NHẤT 5 TRANG

```html
<!-- CẤU TRÚC CHUẨN — copy y hệt vào mọi trang -->
<header class="site-nav">
  <nav class="site-nav-inner">
    <a href="index.html" style="display:flex; align-items:center; gap:12px; text-decoration:none;">
      <span style="font-size:2rem; font-weight:700; color:#C95B00;">MMABC</span>
      <div style="width:1.5px; height:40px; background:#FFD9B0;"></div>
      <div style="display:flex; flex-direction:column; gap:2px;">
        <span style="font-size:14px; font-weight:600; color:#1A2B3C;">Đông Mỹ</span>
        <span style="font-size:11px; color:#FF7A2F; font-weight:500; max-width:280px;">
          Toán tư duy tích hợp toán trường MMABC · Luyện chữ đẹp · Hành trang cho bé vào lớp 1
        </span>
      </div>
    </a>
    <div class="site-nav-links">
      <a href="ve-chung-toi.html">Giới thiệu</a>
      <a href="goc-hoc-tap.html">Góc học tập</a>
      <a href="index.html#hoc-phi">Học phí</a>
      <a href="ket-qua.html">Kết quả</a>
      <a href="index.html#dang-ky" class="site-nav-cta">Đăng ký học thử</a>
    </div>
  </nav>
</header>

<!-- ACTIVE STATE: thêm class="nav-active" vào link tương ứng với trang hiện tại -->
<!-- cam-on.html: không có active state -->
```

---

## 5. THÔNG TIN THẬT — DÙNG CHÍNH XÁC

```yaml
Giáo viên:    Nguyễn Thị Bích Liên
Kinh nghiệm:  3 năm giảng dạy toán tư duy
Độ tuổi:      4–12 tuổi
Sĩ số:        Tối đa 8 học sinh / lớp
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
// Có trên cả 5 trang — id: mmabc-chatbot-bubble
// Gemini API Key: AIzaSyBqkGZf9TQZR-aHXmS73k17Kjcj0ffJGrg
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
- [x] Thêm ảnh giáo viên Nguyễn Thị Bích Liên vào ve-chung-toi.html
- [ ] Tạo _private/index.html — dashboard quản trị
- [ ] Tạo bai-viet-mau.html — template bài viết blog
- [ ] Điền thông tin lịch học cụ thể (thứ mấy, giờ nào)
- [ ] Test form sau khi Netlify live
- [ ] Đồng bộ font Public Sans cho index.html (hiện đang dùng Nunito và Quicksand)

---
*Cập nhật lần cuối: Tháng 3/2026 — v1.1*
