# 📄 BÀI THU HOẠCH / PHẢN HỒI 1 TRANG (ONE-PAGE REFLECTION)

> **Môn học:** Chuyên đề Lập trình Thiết bị Di động (Mobile Programming)  
> **Đề tài:** Topic 01 — Study Planner (Lịch học & Quản lý Deadline cho Sinh viên)  
> **Nhóm sinh viên thực hiện:**  
> - **1923050269** — Trưởng nhóm  
> - **1923050154** — Thành viên  
> - **1923050091** — Thành viên  

---

## 1. Những Điều Nhóm Sẽ Làm Khác Đi Nếu Được Thực Hiện Lại Dự Án Này

Nhìn lại quá trình xây dựng ứng dụng **Study Planner** từ giai đoạn phân tích yêu cầu, thiết kế kiến trúc cho tới kiểm thử và đóng gói, nếu có cơ hội làm lại dự án này từ đầu, nhóm sẽ thực hiện các cải tiến sau:

1. **Chuẩn hóa chiến lược Múi giờ (Timezone Strategy) và Ngày tháng ngay từ tầng DTO/Model:**  
   Ban đầu, nhóm phụ thuộc vào các chuỗi thời gian do AI sinh ra (chứa định dạng UTC `Z`), dẫn đến việc khi hiển thị và kiểm thử trên múi giờ Việt Nam (`Asia/Ho_Chi_Minh` - UTC+7), các mốc hạn nộp vào ban đêm bị nhảy sang ngày hôm sau. Nếu làm lại, nhóm sẽ quy ước lưu trữ chuẩn ISO 8601 kèm múi giờ rõ ràng và viết các helper chuyển đổi tập trung tại `utils/dates.ts` ngay trong Sprint đầu tiên.

2. **Thiết kế phân vùng dữ liệu Đa người dùng (Multi-tenant / User Isolation) ngay từ đầu:**  
   Ở các phiên bản đầu, ứng dụng sử dụng mảng tĩnh trong bộ nhớ và cache chung của TanStack Query. Khi bổ sung tính năng nhiều tài khoản, nhóm phải tái cấu trúc lại cơ chế lưu trữ phân vùng `@study_planner_assignments_${userId}` và reset TanStack Query cache khi đổi tài khoản. Việc dự phóng kiến trúc đa người dùng ngay từ đầu sẽ giúp tiết kiệm thời gian tái cấu trúc mã nguồn.

3. **Áp dụng kiểm thử tự động theo hướng TDD (Test-Driven Development):**  
   Thay vì hoàn thiện tính năng rồi mới viết Unit Test, nhóm nhận thấy việc viết test trước cho các logic nghiệp vụ quan trọng (tính toán tiến độ %, lọc hạn nộp 48h, validate email giáo dục `@dntu.vn`) sẽ giúp phát hiện sớm các trường hợp biên (edge cases) và ngăn ngừa lỗi hồi quy hiệu quả hơn.

---

## 2. Điểm / Lỗi Trợ Lý AI Đã Gợi Ý Sai Mà Nhóm Đã Tự Phát Hiện Và Sửa Lại

Trong quá trình sử dụng AI hỗ trợ lập trình, lỗi điển hình và nghiêm trọng nhất mà nhóm phát hiện là: **Hiện tượng rò rỉ dữ liệu giữa các tài khoản người dùng (User Data Leakage across Sessions)** và **Lỗi giả lập Unit Test trên AsyncStorage**.

- **Gợi ý sai của AI:**  
  Khi yêu cầu AI viết tính năng đăng nhập và quản lý danh sách bài tập/lịch học, AI đã khai báo một mảng dữ liệu mẫu dùng chung `let MOCK_ASSIGNMENTS = [...]` trong file `api/client.ts` và sử dụng TanStack Query với query key cố định `['assignments']`.  
  *Hậu quả:* Khi **Tài khoản A** (`sinhvien@dntu.vn`) đăng nhập và tạo một Lịch học mới, sau đó đăng xuất để **Tài khoản B** (`minhduy@student.edu.vn`) đăng nhập vào, Tài khoản B vẫn nhìn thấy toàn bộ Lịch học của Tài khoản A. Dữ liệu bị trộn lẫn hoàn toàn giữa các sinh viên, vi phạm nghiêm trọng tính toàn vẹn và riêng tư dữ liệu.

- **Cách nhóm sinh viên tự phát hiện và khắc phục:**  
  1. *Phát hiện:* Nhóm đã tiến hành kịch bản kiểm thử thực tế chuyển đổi qua lại giữa hai tài khoản và quan sát thấy tab Lịch học không được làm mới.
  2. *Giải pháp kỹ thuật của nhóm:*
     - Tái cấu trúc cơ sở dữ liệu cục bộ: Nhóm đã viết lại cơ chế lưu trữ phân vùng, gán `userId` vào tiền tố khóa lưu trữ riêng biệt: `@study_planner_assignments_${userId}`. Nhờ đó, mỗi người dùng sở hữu một không gian dữ liệu hoàn toàn độc lập.
     - Dọn dẹp bộ nhớ đệm: Nhóm đã tích hợp `queryClient.clear()` vào toàn bộ chu trình xác thực (`signIn`, `signUp`, `signOut`, `restoreSession`) trong `store/authStore.ts` để ép TanStack Query giải phóng toàn bộ cache trong RAM khi chuyển đổi tài khoản.
     - Viết bài kiểm thử tự động chứng minh: Nhóm đã tự tay thiết kế bộ unit test `__tests__/userIsolation.test.ts` (mô phỏng chính xác kịch bản Tài khoản A thêm lịch học $\rightarrow$ Tài khoản B đăng nhập xác minh không thấy $\rightarrow$ Tài khoản A đăng nhập lại và thấy dữ liệu toàn vẹn) để đảm bảo 100% không còn lỗi rò rỉ dữ liệu.

---

**Xác nhận của nhóm:** Toàn bộ nội dung trên phản ánh đúng thực tế quá trình nghiên cứu, phát triển và làm chủ công nghệ của cả 3 thành viên trong nhóm.
