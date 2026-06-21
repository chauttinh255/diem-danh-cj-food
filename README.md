# Điểm Danh Chuyền Sản Xuất — CJ FOOD

App điểm danh nhân sự hằng ngày cho các chuyền: Kimchi, K-Sauce, RB Snack,
Gim Jaban, Rong Biển 4g, Rollgim/Myoek, và Sinh Viên (SVTT/SVPT/OEM PSAS/ĐHTV-OEM).

Đây là **1 file HTML tĩnh** (`index.html`), không cần server/backend riêng —
nhưng cần một nơi lưu dữ liệu dùng chung cho nhiều người, nên app dùng
**Firebase Firestore** (miễn phí) làm nơi lưu trữ. Vì vậy trước khi deploy,
bạn cần làm bước cấu hình Firebase (chỉ làm 1 lần, mất khoảng 5–10 phút).

---

## Bước 1 — Tạo Firebase project & bật Firestore

1. Vào https://console.firebase.google.com → **Add project** → đặt tên (vd `cj-food-diem-danh`) → bỏ qua Google Analytics nếu không cần → **Create project**.
2. Trong menu trái: **Build → Firestore Database → Create database**.
   - Chọn **Start in production mode**.
   - Chọn region gần Việt Nam, vd `asia-southeast1 (Singapore)`.
   - **Enable**.
3. Vào **Firestore Database → Rules**, xoá nội dung mặc định, dán nội dung
   trong file `firestore.rules` đi kèm vào, rồi bấm **Publish**.
   > Rules này cho phép đọc/ghi tự do (không cần đăng nhập) — phù hợp công cụ
   > nội bộ. Chỉ chia sẻ link app trong nội bộ công ty, không đăng public.
4. Vào **Project settings** (biểu tượng bánh răng cạnh "Project Overview")
   → tab **General** → cuộn xuống mục **Your apps** → bấm icon Web `</>`.
   → Đặt tên app (vd `diem-danh-web`) → **Register app**.
   → Firebase sẽ hiện đoạn code `const firebaseConfig = { apiKey: "...", ... }` — copy đoạn này.

## Bước 2 — Dán config vào file index.html

1. Mở file `index.html` bằng trình soạn thảo bất kỳ (VS Code, Notepad...).
2. Tìm đoạn (gần phần `FIREBASE SETUP` trong thẻ `<script>`):
   ```js
   var firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_PROJECT.firebaseapp.com",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_PROJECT.appspot.com",
     messagingSenderId: "YOUR_SENDER_ID",
     appId: "YOUR_APP_ID"
   };
   ```
3. Thay toàn bộ object này bằng đoạn config thật bạn vừa copy ở Bước 1.4.
4. Lưu file. (Nếu quên bước này, app vẫn chạy nhưng sẽ hiện banner đỏ cảnh
   báo và không lưu được dữ liệu.)

## Bước 3 — Đẩy code lên GitHub

**Cách dùng giao diện web (không cần cài git):**
1. Vào https://github.com → **New repository** → đặt tên (vd `diem-danh-cj-food`) → **Create repository**.
2. Trong repo vừa tạo → **Add file → Upload files** → kéo thả `index.html`
   (và `firestore.rules`, `README.md` nếu muốn lưu lại) → **Commit changes**.

**Hoặc dùng dòng lệnh git:**
```bash
git init
git add index.html firestore.rules README.md
git commit -m "Init: app diem danh CJ FOOD"
git branch -M main
git remote add origin https://github.com/<ten-cua-ban>/diem-danh-cj-food.git
git push -u origin main
```

## Bước 4 — Deploy bằng Netlify

1. Vào https://app.netlify.com → đăng nhập bằng GitHub.
2. **Add new site → Import an existing project → Deploy with GitHub**
   → chọn repo `diem-danh-cj-food` vừa tạo.
3. Ở phần build settings: để **Build command** trống, **Publish directory**
   để trống hoặc `/` (vì đây là file tĩnh, không cần build).
4. Bấm **Deploy site**. Sau khoảng 30 giây, Netlify cấp 1 link dạng:
   `https://ten-ngau-nhien-123abc.netlify.app`
5. (Tuỳ chọn) Đổi tên cho dễ nhớ: **Site settings → Change site name**
   → vd `diem-danh-cj-food.netlify.app`.

Gửi link này cho các bạn phụ trách từng chuyền — ai cũng vào cùng 1 link,
chọn ngày, điền chuyền của mình, bấm **Lưu điểm danh**. Tab **Tổng Quan**
sẽ tự tổng hợp số liệu của tất cả các chuyền trong ngày.

> Mỗi khi bạn `git push` thay đổi mới lên GitHub, Netlify sẽ **tự động
> deploy lại** bản mới — không cần thao tác gì thêm.

---

## Test nhanh không cần GitHub (tuỳ chọn)

Muốn xem thử trước khi làm đủ các bước trên: vào https://app.netlify.com/drop
và kéo thả file `index.html` vào — Netlify tạo ngay 1 link tạm để xem giao
diện (nhưng nhớ vẫn phải làm Bước 1–2 để lưu được dữ liệu thật).

## Ghi chú

- Kế hoạch (Kế hoạch nhân sự) của chuyền **Rollgim/Myoek** hiện để mặc định
  là 0/0 vì chưa có số liệu — bạn sửa trực tiếp trong ô "Kế hoạch" trên app,
  số sẽ được lưu lại cùng dữ liệu điểm danh từ lần lưu đầu tiên.
- Nếu một ngày có nhiều người cùng điểm danh các chuyền khác nhau cùng lúc,
  mỗi người chỉ ghi đè dữ liệu của đúng chuyền mình đang sửa — không ảnh
  hưởng tới chuyền khác.
