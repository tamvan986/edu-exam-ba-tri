# HƯỚNG DẪN ĐƯA PHÒNG THI LÊN INTERNET ĐỂ HỌC SINH DÙNG ĐIỆN THOẠI

## Bước 1 – Tạo cơ sở dữ liệu
1. Vào https://supabase.com/dashboard, đăng ký/đăng nhập và tạo New project.
2. Trong SQL Editor, dán toàn bộ nội dung `supabase/schema.sql`, chạy và kiểm tra không có lỗi.
3. Vào Project Settings → API hoặc Connect, lấy **Project URL** và **publishable key** (hoặc anon key). **Không dùng service_role key**.
4. Trong Authentication → Providers, bật Email; kiểm tra cấu hình xác nhận email. Tạo tài khoản giáo viên bằng chức năng Đăng ký trên webapp.

## Bước 2 – Đưa lên Vercel
1. Giải nén gói ZIP này lên máy tính. Đưa thư mục dự án lên GitHub (repository riêng tư nếu cần).
2. Vào https://vercel.com/new → Import repository → chọn repository vừa tải.
3. Framework Preset: **Vite**; Build Command: `npm run build`; Output Directory: `dist`.
4. Environment Variables:
   - `VITE_SUPABASE_URL` = Project URL
   - `VITE_SUPABASE_ANON_KEY` = publishable/anon key
5. Nhấn Deploy. Vercel cấp địa chỉ dạng `https://ten-du-an.vercel.app`.
6. Trong Supabase Authentication → URL Configuration, cập nhật Site URL và Redirect URLs theo địa chỉ Vercel vừa nhận, nếu dùng luồng xác nhận email.

## Bước 3 – Giáo viên mời học sinh
1. Giáo viên mở website Vercel, đăng ký/đăng nhập.
2. Tạo phòng, chọn môn, khối, lớp, thời lượng; mở chi tiết phòng.
3. Sao chép link mời hoặc cho học sinh quét QR. Link phải bắt đầu bằng địa chỉ `https://...vercel.app`, **không phải localhost**.
4. Học sinh mở link trên Chrome/Safari điện thoại, điền tên, mã học sinh, lớp và nhấn Vào phòng chờ.
5. Giáo viên duyệt danh sách rồi nhấn Bắt đầu thi; học sinh làm bài, nộp và nhận điểm theo bản thử nghiệm.

## Kiểm tra nhanh trên 2 thiết bị
- Máy giáo viên: đăng nhập và mở phòng chờ.
- Điện thoại: dùng 4G (không chung Wi-Fi cũng được) mở link mời.
- Xác nhận học sinh xuất hiện trên bảng giáo viên, được duyệt, làm bài, nộp và hiện điểm.

## CẢNH BÁO
- Đây là bản thử nghiệm: bộ 22 câu hiện là câu hỏi **mẫu**, chưa có ngân hàng đề chính thức và chưa xác thực danh tính học sinh.
- Học sinh tự khai mã; không sử dụng cho kỳ thi quan trọng hoặc đánh giá chính thức trước khi bổ sung xác thực, kiểm thử bảo mật, kiểm thử đồng thời và quy trình xử lý sự cố.
- Cần mạng Internet và cấu hình Supabase + Vercel. Chỉ tải file HTML hoặc mở file trên máy giáo viên sẽ **không** tạo phòng thi nhiều thiết bị.
- Bản này lưu token phiên vào trình duyệt để hỗ trợ mở lại sau tải trang; học sinh không nên xóa dữ liệu trình duyệt khi đang thi.
- Chưa có tự động kết thúc bài trên máy chủ khi mất kết nối đúng thời điểm hết giờ; cần hoàn thiện trước khi sử dụng chính thức.
