# Quầy Sức Khỏe — bản deploy lên Vercel

Đây là bản chuyển đổi từ Claude Artifact sang một trang web tĩnh dùng **Firebase Firestore** để lưu dữ liệu, có thể deploy lên Vercel với tên miền riêng.

## 1. Điền cấu hình Firebase

Mở file `index.html`, tìm đoạn:

```js
var firebaseConfig = {
  apiKey: "ĐIỀN_API_KEY",
  authDomain: "ĐIỀN_PROJECT.firebaseapp.com",
  projectId: "ĐIỀN_PROJECT_ID",
  storageBucket: "ĐIỀN_PROJECT.appspot.com",
  messagingSenderId: "ĐIỀN_SENDER_ID",
  appId: "ĐIỀN_APP_ID"
};
```

Thay các giá trị `ĐIỀN_...` bằng thông tin lấy từ Firebase Console → Project settings → Your apps → SDK setup and configuration.

## 2. Bật Firestore + Anonymous Authentication

Trong Firebase Console:
- Build → Firestore Database → Create database → **Production mode**.
- Build → Authentication → Get started → tab Sign-in method → bật **Anonymous**.

## 3. Dán quy tắc bảo mật (Firestore Rules)

Vào Firestore Database → tab **Rules**, dán nội dung file `firestore.rules` trong thư mục này vào, rồi bấm **Publish**.

Quy tắc này chặn mọi truy cập đọc/ghi trực tiếp vào kho dữ liệu nếu không đi qua trang web (trang web tự đăng nhập ẩn danh khi mở). Đây là mức chặn cơ bản — không thay được việc kiểm soát vai trò (chủ quầy / nhân viên / hội viên), việc đó vẫn do chính phần mềm xử lý ở giao diện.

## 4. Đẩy code lên GitHub

```bash
git init
git add .
git commit -m "Chuyển sang Firebase để deploy Vercel"
git branch -M main
git remote add origin <URL_REPO_GITHUB_CUA_BAN>
git push -u origin main
```

## 5. Deploy lên Vercel

1. Vào https://vercel.com/new
2. Chọn **Import Git Repository**, chọn repo vừa đẩy lên.
3. Vercel sẽ tự nhận đây là trang tĩnh (không cần chọn framework, không cần build command) → bấm **Deploy**.
4. Sau khi deploy xong, vào Project → Settings → Domains để gắn tên miền riêng của bạn.

## 6. Kiểm tra

Mở link Vercel vừa deploy:
- Trang phải hiện màn hình đăng nhập "Nhân sự quầy / Hội viên" (không hiện "Chưa mở được kho dữ liệu").
- Tạo tài khoản chủ quầy đầu tiên, đăng nhập thử, thêm một hội viên thử xem có lưu được không.
- Vào Firebase Console → Firestore Database → Data để xem dữ liệu vừa tạo có xuất hiện không.

## Về dữ liệu cũ

Bản Artifact cũ trên claude.ai và bản Firebase mới này là **hai kho dữ liệu độc lập** — dữ liệu hội viên, nhân sự... đã nhập trước đây trên artifact sẽ KHÔNG tự động có ở bản Vercel này. Nếu cần chuyển dữ liệu cũ sang, báo lại để được hỗ trợ.
