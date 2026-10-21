# Phòng thi trực tuyến – GDNN–GDTX Ba Tri

React + Vite + Supabase. Giao diện tiếng Việt, 3 môn Toán / Tin học / Công nghệ, tạo phòng, mã mời, QR, phòng chờ, duyệt học sinh, bắt đầu thi, 22 câu, lưu đáp án, chấm điểm.

## Cài đặt

1. Cài Node.js 20+.
2. Mở terminal tại thư mục dự án: `npm install`.
3. Tạo dự án Supabase tại https://supabase.com, mở SQL Editor và chạy toàn bộ `supabase/schema.sql`.
4. Sao chép `.env.example` thành `.env.local`, nhập Project URL và Publishable/anon key từ Supabase. **Không bao giờ đưa service_role key vào frontend.**
5. `npm run dev` và mở địa chỉ localhost được thông báo.
6. Đăng ký giáo viên ở giao diện. Nếu Supabase bật xác nhận email, xác nhận trước khi đăng nhập.
7. Tạo phòng, sao chép link hoặc QR; học sinh vào từ máy khác. Link cần dùng tên miền công khai, không dùng localhost khi mời học sinh ở máy khác.
8. Deploy lên Vercel, thêm hai biến môi trường `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, chọn framework Vite và deploy.

## Phạm vi bản đầu tiên

- Mỗi phòng tự tạo **22 câu hỏi mẫu** để thử luồng: 12 chọn A–D, 4 đúng/sai, 6 trả lời ngắn. Đây **không phải ngân hàng câu hỏi chuyên môn đạt chuẩn**. Giáo viên cần thay bằng câu hỏi đã thẩm định trước khi thi thật.
- Mã học sinh do người tham gia tự khai, chưa có xác thực danh tính học sinh; không dùng cho kỳ thi chính thức có yêu cầu chống thi hộ.
- Chế độ Theo lịch và thời điểm lịch hiện mới lưu cấu hình; chưa tự động mở/đóng theo lịch. Phòng vẫn do giáo viên điều khiển.
- Đồng hồ tính theo mốc máy chủ, nhưng nộp tự động cần trình duyệt còn kết nối. Nếu mất mạng đúng lúc hết giờ, cần cơ chế máy chủ quét và kết thúc lượt thi bổ sung cho triển khai chính thức.
- Chưa có giao diện chỉnh sửa ngân hàng câu hỏi, xuất Excel/PDF, nhập Word/PDF, đăng nhập học sinh, thống kê chuyên sâu hoặc chống gian lận.
- QR hiện dùng dịch vụ bên ngoài để tạo ảnh, vì vậy cần Internet và không nên dùng cho mã mời bí mật. Có thể thay bằng thư viện QR chạy nội bộ.
- Thang điểm: phần I 3 điểm, phần II 4 điểm, phần III 3 điểm. So khớp câu trả lời ngắn chỉ chuẩn hóa chữ hoa/thường, dấu phẩy thập phân và khoảng trắng; không suy diễn tương đương toán học.

## Bảo mật

- Các bảng bật RLS. Học sinh truy cập bài qua token ngẫu nhiên được băm trong cơ sở dữ liệu và RPC có kiểm tra trạng thái.
- Đáp án đúng chỉ nằm trong cơ sở dữ liệu và không trả về từ RPC lấy đề.
- Đây là **bản thử nghiệm có backend thực**, chưa đạt tiêu chuẩn vận hành kỳ thi chính thức. Cần thêm rate limiting, xác thực học sinh, kiểm tra bảo mật, nhật ký, backup, xử lý đồng thời và bài kiểm thử tải.

## Kiểm thử thủ công

1. Giáo viên tạo phòng và xác nhận phòng xuất hiện.
2. Mở link mời bằng trình duyệt ẩn danh, nhập học sinh.
3. Giáo viên duyệt học sinh; học sinh thấy trạng thái được cập nhật.
4. Giáo viên bắt đầu thi; học sinh thấy 22 câu và đồng hồ.
5. Học sinh chọn đáp án, tải lại trang: phiên hiện tại cần giữ token trong state; có thể dùng cùng phiên khi không tải lại. **Phục hồi sau refresh chưa được triển khai hoàn chỉnh**.
6. Nộp bài, kiểm tra điểm và trạng thái trên dashboard.
