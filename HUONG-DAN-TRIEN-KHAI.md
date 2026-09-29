# Phần mềm quản lý Quầy Sức Khỏe: bản chạy trên Vercel với Firebase

Bản xuất ngày 29/9/2026, cùng tính năng với bản 21 trên Claude, đã nối Firebase dự án `phan---mem---quan---ly---quay`.

## Trong gói
| Tệp | Là gì |
|---|---|
| `index.html` | Toàn bộ phần mềm, đã nối Firebase (Firestore, đăng nhập ẩn danh). Không cần build. |
| `logo.png` | Logo quầy, đặt cùng thư mục với `index.html`. |
| `firestore.rules` | Quy tắc bảo mật Firestore dùng cho bản này. |

Thư viện tải từ CDN khi chạy: Firebase 8.10.1, Be Vietnam Pro (Google Fonts), qrcodejs 1.0.0, pdf.js 3.11.174 (chỉ tải khi mở PDF).

## Cài đặt trên Firebase Console (làm một lần)
1. **Authentication → Sign-in method:** bật **Anonymous**.
2. **Firestore Database:** tạo cơ sở dữ liệu (chế độ production), rồi vào **Rules**, dán nội dung `firestore.rules`, bấm Publish.
3. Nếu API key có giới hạn theo tên miền (Google Cloud Console → Credentials), thêm tên miền Vercel của quầy vào danh sách được phép.

## Đưa lên Vercel
1. Tạo dự án mới trên Vercel, chọn **Other** (trang tĩnh, không có lệnh build).
2. Tải lên thư mục chứa `index.html` và `logo.png` (kéo thả trên vercel.com hoặc lệnh `vercel deploy` trong thư mục này).
3. Mở tên miền Vercel. Lần đầu, cơ sở dữ liệu trống: màn hình sẽ mời tạo tài khoản chủ quầy đầu tiên.

## Lưu ý bảo mật
Quy tắc trong gói cho **mọi người mở trang** được đọc ghi toàn bộ dữ liệu, vì trang tự đăng nhập ẩn danh. Mã PIN nhân sự và mật khẩu hội viên chỉ được kiểm tra trên trình duyệt. Dùng được cho giai đoạn chạy thử nội bộ. Trước khi mở cho khách thật, cần: đăng nhập thật cho nhân sự và hội viên, quy tắc Firestore phân quyền theo vai, lưu tệp bệnh án sang Firebase Storage.

## Dữ liệu cũ
Dữ liệu đang dùng trên bản Claude (hội viên, thẻ, kho, giấy tờ sản phẩm, lịch sử thanh toán) chưa có trong Firebase. Cần chuyển sang nếu muốn dùng tiếp trên bản Vercel.
