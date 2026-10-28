# 📚 Study Planner — Ứng dụng Quản lý Lịch học & Deadline cho Sinh viên

> **Đồ án Cuối kỳ — Chuyên đề Lập trình Thiết bị Di động (Mobile Programming)**  
> **Topic 01:** Study Planner (Lịch học & Quản lý Deadline thông minh)  
> **Đối tượng:** Sinh viên năm 4 — Khoa Công nghệ Thông tin  

---

## 👥 Nhóm Sinh viên Thực hiện

| STT | Mã số Sinh viên (MSSV) | Họ và Tên | Vai trò & Phân công nhiệm vụ |
| :---: | :---: | :---: | :--- |
| **1** | **1923050269** | Trưởng nhóm | Thiết kế UI/UX Tokens, Xây dựng hệ thống màn hình (Screens), Tối ưu Accessibility (WCAG AA), Kiểm thử trải nghiệm |
| **2** | **1923050154** | Thành viên | Kiến trúc Quản lý trạng thái (Zustand), TanStack Query Cache, Xác thực bảo mật SecureStore, Xử lý ảnh đính kèm & Thông báo |
| **3** | **1923050091** | Thành viên | Thiết kế Mock API & Xử lý ngoại tuyến (Offline), Viết Automated Tests (Jest 22 tests), Kiểm thử Timezone & Build app |

---

## 🎯 Bảng Đối chiếu Tính năng & Thang điểm Đánh giá

| Phân loại | Tính năng yêu cầu | Trạng thái | Vị trí Source Code / Chi tiết kỹ thuật |
| :--- | :--- | :---: | :--- |
| **Cơ bản** | Đăng nhập / Đăng xuất an toàn, lưu phiên | ✅ Đạt | `store/authStore.ts`, `app/sign-in.tsx`, dùng `expo-secure-store` |
| **Cơ bản** | CRUD Môn học & Bài tập từ API | ✅ Đạt | `api/client.ts`, `api/courses.ts`, `api/assignments.ts` |
| **Cơ bản** | 2 Chế độ xem Lịch (Tuần & Danh sách) | ✅ Đạt | `app/(tabs)/calendar.tsx`, `components/WeeklyCalendar.tsx` |
| **Cơ bản** | Lọc theo môn học và trạng thái | ✅ Đạt | Lọc 'all' / 'pending' / 'completed' kết hợp môn học trong `calendar.tsx` |
| **Cơ bản** | Nhắc nhở hạn nộp thông báo cục bộ | ✅ Đạt | `utils/notifications.ts` (lên lịch nhắc trước 24h & 2h) |
| **Cơ bản** | Thống kê tiến độ theo môn (Computed) | ✅ Đạt | `app/(tabs)/courses.tsx`, `components/CourseCard.tsx` (tính động) |
| **Cơ bản** | Hỗ trợ Ngoại tuyến & Offline Banner | ✅ Đạt | `hooks/useNetworkStatus.ts`, `components/ui/OfflineBanner.tsx` |
| **Mở rộng** | Đính kèm ảnh chụp ghi chú bài tập | ✅ Đạt | `app/modal/new-assignment.tsx`, `app/assignment/[id].tsx` (Camera & Library) |
| **Mở rộng** | Widget tóm tắt deadline 48 giờ | ✅ Đạt | `components/WidgetSummary.tsx` hiển thị ngay đầu màn hình Trang chủ |
| **Mở rộng** | Cập nhật trạng thái bài tập mượt mà | ✅ Đạt | `api/assignments.ts` và `app/assignment/[id].tsx` khi hoàn thành bài |
| **Giao diện** | 4 Trạng thái (Loading, Empty, Error, Content) | ✅ Đạt | Đầy đủ tại các màn hình (`components/ui/*`) |
| **Giao diện** | Dark & Light Mode dễ nhìn | ✅ Đạt | `constants/tokens.ts` (hỗ trợ chuyển đổi giao diện sáng/tối) |
| **Giao diện** | Tối ưu trải nghiệm bấm và đọc màn hình | ✅ Đạt | Áp dụng kích thước nút bấm chuẩn, nhãn thông tin rõ ràng |
| **Cấu trúc** | Cấu trúc định tuyến Expo Router (5+ màn hình) | ✅ Đạt | Dynamic Route (`[id]`), Modal Route (`modal/new-assignment`), Tabs |
| **Kiểm thử** | Kiểm thử tự động (Unit Tests) | ✅ Đạt | Các bộ unit test chạy với Jest kiểm tra logic cơ bản |

---

## 🛠️ Công nghệ Sử dụng

- **Nền tảng:** React Native 0.88 + Expo SDK 58 (Expo Router 58, File-based routing)
- **Ngôn ngữ:** TypeScript 6.0 (Cấu hình `strict: true`, không dùng `any`)
- **State Management:** Zustand v5 (Store phân tầng, selector tối ưu re-render)
- **Data Fetching & Cache:** TanStack Query v5 (`@tanstack/react-query`) hỗ trợ cache, auto-retry, optimistic updates & rollback
- **Bảo mật:** `expo-secure-store` mã hóa token xác thực
- **Xử lý Thời gian:** `date-fns` v4 chuẩn hóa theo múi giờ Việt Nam (UTC+7)
- **Device Features:** `expo-image-picker` (Camera & Thư viện), `expo-notifications` (Thông báo đẩy cục bộ), `expo-network` (Giám sát kết nối mạng)
- **Testing:** Jest + `jest-expo` + `@react-native/jest-preset` + `@testing-library/react-native`

---

## 📂 Cấu trúc Thư mục Dự án

```
./ (file cuối kỳ)
├── __tests__/                  # Thư mục kiểm thử tự động (22 tests)
│   ├── appStore.test.ts        # Test quản lý trạng thái bộ lọc & theme
│   ├── authStore.test.ts       # Test đăng nhập, đăng xuất, SecureStore session
│   ├── dates.test.ts           # Test tiện ích ngày giờ, hạn 48h, format VN
│   └── progress.test.ts        # Test tính toán tiến độ môn học động
├── api/                        # Tầng kết nối API & TanStack Query Hooks
│   ├── client.ts               # REST API mock client (có độ trễ mạng giả lập)
│   ├── courses.ts              # Hooks: useCourses, useCourse
│   └── assignments.ts          # Hooks: useAssignments, useUpdateAssignment (Optimistic)
├── app/                        # Expo Router — Định tuyến dựa trên file
│   ├── (tabs)/                 # Nhóm màn hình Bottom Tabs
│   │   ├── _layout.tsx         # Tab Bar điều hướng (Home, Calendar, Courses, Profile)
│   │   ├── index.tsx           # Tab 1: Trang chủ Dashboard & Widget 48h
│   │   ├── calendar.tsx        # Tab 2: Lịch học (Weekly View & List View)
│   │   ├── courses.tsx         # Tab 3: Danh sách môn & Thống kê tiến độ
│   │   └── profile.tsx         # Tab 4: Thông tin sinh viên, Đổi theme, Cài đặt
│   ├── assignment/
│   │   └── [id].tsx            # Dynamic Route: Xem chi tiết bài tập, ảnh ghi chú
│   ├── modal/
│   │   └── new-assignment.tsx  # Modal Route: Form Thêm / Sửa bài tập & Chụp ảnh
│   ├── +not-found.tsx          # Màn hình 404 Not Found
│   ├── _layout.tsx             # Root Layout (Provider, Auth Guard)
│   └── sign-in.tsx             # Màn hình Đăng nhập (Email sinh viên)
├── components/                 # Các Component tái sử dụng
│   ├── ui/                     # Bộ 4 trạng thái giao diện chuẩn mực
│   │   ├── LoadingState.tsx    # Trạng thái đang tải dữ liệu
│   │   ├── EmptyState.tsx      # Trạng thái danh sách rỗng
│   │   ├── ErrorState.tsx      # Trạng thái lỗi mạng kèm nút thử lại
│   │   └── OfflineBanner.tsx   # Banner cảnh báo ngoại tuyến
│   ├── AssignmentCard.tsx      # Thẻ bài tập kèm deadline badge & checkbox
│   ├── CourseCard.tsx          # Thẻ môn học kèm thanh tiến độ phần trăm
│   ├── WeeklyCalendar.tsx      # Thanh chọn 7 ngày trong tuần
│   └── WidgetSummary.tsx       # Widget tóm tắt bài tập gấp trong 48 giờ
├── constants/
│   └── tokens.ts               # Nguồn duy nhất chứa Bảng màu, Typography, WCAG Tokens
├── hooks/
│   ├── useNetworkStatus.ts     # Hook theo dõi online/offline mạng
│   └── useTheme.ts             # Hook cung cấp màu sắc theme sáng/tối
├── store/
│   ├── appStore.ts             # Zustand store lưu filter, themePreference, offline
│   └── authStore.ts            # Zustand store lưu user, token, SecureStore sync
├── types/
│   └── index.ts                # TypeScript Interfaces (Course, Assignment, User, ...)
├── utils/
│   ├── dates.ts                # Xử lý thời gian, hạn nộp, tuần học
│   └── notifications.ts        # Quản lý quyền và lên lịch local notifications
├── AI_LOG.md                   # Báo cáo Nhật ký sử dụng AI bắt buộc
├── ONE_PAGE_REFLECTION.md      # Bài thu hoạch / Phản hồi 1 trang (Deliverable 4)
├── jest.config.js              # Cấu hình Jest Testing
├── package.json
├── tsconfig.json
└── README.md                   # Hướng dẫn chi tiết chạy dự án
```

---

## 🔑 Tài Khoản Đăng Nhập Mẫu (Demo Credentials)

Ứng dụng hỗ trợ đăng nhập nhanh bằng 2 nút bấm trên giao diện hoặc nhập thủ công:
- **Tài khoản A:** `sinhvien@dntu.vn` | Mật khẩu: `123456`
- **Tài khoản B:** `minhduy@student.edu.vn` | Mật khẩu: `123456`
*(Hệ thống hỗ trợ mọi email có đuôi `.edu`, `.edu.vn`, `@dntu.vn` và `@gmail.com`, có chức năng Đăng ký tài khoản mới)*

---

## 🚀 Hướng dẫn Cài đặt & Chạy Ứng dụng

### 1. Yêu cầu Môi trường
- Node.js >= 18.x
- Trình quản lý gói npm hoặc yarn
- Ứng dụng **Expo Go** trên điện thoại (iOS / Android) hoặc trình duyệt Web.

### 2. Cài đặt Thư viện
Mở terminal tại thư mục dự án:
```bash
npm install
```

### 3. Chạy Ứng dụng (Development)
```bash
npx expo start
```
- Quét mã QR hiển thị trên Terminal bằng **Expo Go** (Android) hoặc Camera (iOS).
- Nhấn `w` để mở giao diện Web trên trình duyệt máy tính.
- Nhấn `a` để chạy trên giả lập Android Emulator.

### 4. Chạy Kiểm thử Tự động (Automated Tests)
Toàn bộ 4 bộ test suites (22 tests) được chạy bằng Jest:
```bash
npm test
```
*Kết quả mẫu:*
```text
PASS __tests__/progress.test.ts
PASS __tests__/appStore.test.ts
PASS __tests__/authStore.test.ts
PASS __tests__/dates.test.ts

Test Suites: 4 passed, 4 total
Tests:       22 passed, 22 total
Snapshots:   0 total
Time:        2.953 s
```

### 5. Kiểm tra TypeScript (Type Check)
Đảm bảo 100% không có lỗi kiểu dữ liệu:
```bash
npx tsc --noEmit
```

---

## 📱 Hướng Dẫn Thao Tác & Trải Nghiệm Ứng Dụng (User & Demo Guide)

### 1. Đăng Nhập & Xác Thực Tài Khoản
- **Đăng nhập nhanh (1-Click):** Tại màn hình Đăng nhập, nhấn vào một trong 2 nút:
  - `⚡ Đăng nhập Tài khoản A (sinhvien@dntu.vn)` — Mật khẩu: `123456`
  - `⚡ Đăng nhập Tài khoản B (minhduy@student.edu.vn)` — Mật khẩu: `123456`
- **Nhập thủ công:** Nhập email bất kỳ có đuôi trường học (`@dntu.vn`, `.edu.vn`, `.edu`) hoặc tài khoản Google cá nhân (`@gmail.com`).
- **Thử nghiệm thông báo lỗi (Validation):**
  - Thử gõ sai mật khẩu: Màn hình lập tức hiện popup Native Alert và dải thông báo màu đỏ cảnh báo *"Mật khẩu không chính xác!"*.
  - Thử gõ email không hợp lệ (ví dụ: `test@yahoo.com`): Hệ thống sẽ từ chối và hướng dẫn dùng email trường cấp hoặc Gmail.
- **Đăng ký tài khoản mới:** Chuyển sang tab `📝 Đăng ký mới`, nhập họ tên, email sinh viên và mật khẩu (tối thiểu 6 ký tự) để tự tạo tài khoản riêng.

---

### 2. Thêm Mới Bài Tập & Lịch Học
1. Nhấn nút **`+`** ở góc trên bên phải thanh Header (tại tab **Trang chủ** hoặc tab **Lịch học**).
2. **Chọn loại nội dung:** Nhấn chuyển đổi giữa:
   - `📅 Lịch học / Tiết học`: Dành cho buổi học trên lớp, phòng học, ca học.
   - `📝 Bài tập / Deadline`: Dành cho đồ án, bài tập về nhà, tiểu luận.
3. **Nhập thông tin:**
   - **Tiêu đề:** Ví dụ: *"Báo cáo Cuối kỳ Lập trình Mobile"* hoặc *"Học lý thuyết Phòng A2-101"*.
   - **Môn học:** Chọn môn học tương ứng từ danh mục có sẵn.
   - **Hạn nộp / Ngày học:** Hệ thống tự động điền sẵn ngày hiện tại dạng `YYYY-MM-DD`, có thể sửa ngày theo ý muốn.
   - **Ghi chú:** Điền yêu cầu đề tài hoặc số phòng học.
4. **Đính kèm ảnh minh chứng / ghi chú:** Nhấn nút `📸 Chụp ảnh` hoặc `🖼️ Chọn ảnh từ thư viện` để đính kèm ảnh bài làm viết tay hoặc đề cương.
5. Nhấn nút **`Lưu bài tập`** (hoặc nút to **`💾 Tạo bài tập mới`** ở cuối trang). Bài tập lập tức được đồng bộ và hiển thị trên màn hình.

---

### 3. Theo Dõi Deadline & Tương Tác Danh Sách
- **Widget tóm tắt 48 Giờ (Tab Trang chủ):** Tự động lọc và hiển thị bài tập có hạn nộp khẩn cấp hôm nay và ngày mai.
- **Xem Lịch học (Tab Lịch học):**
  - Nhấn nút chuyển đổi giữa chế độ **Xem theo tuần (Weekly View)** (chọn từng ngày trong 7 ngày) và **Xem toàn bộ (List View)**.
  - Lọc nhanh theo môn học và theo trạng thái: *Tất cả*, *Chưa xong*, *Đã hoàn thành*.
- **Hoàn thành bài tập tức thì (Optimistic Updates):** Nhấn trực tiếp vào vòng tròn Checkbox trên thẻ bài tập. Bài tập sẽ được gạch ngang và đổi trạng thái ngay lập tức (0ms độ trễ).

---

### 4. Xem Chi Tiết, Chỉnh Sửa & Xóa
- **Xem chi tiết:** Nhấn vào thẻ bài tập bất kỳ để mở trang chi tiết: xem thông tin môn, ngày hết hạn, xem ảnh chụp ghi chú kích thước lớn.
- **Sửa bài tập:** Nhấn nút `✏️ Sửa bài tập` để cập nhật lại tiêu đề, ghi chú hoặc đổi ảnh.
- **Xóa an toàn:** Nhấn nút `🗑️ Xóa bài tập` $\rightarrow$ Hộp thoại xác nhận nội bộ hiện ra với hai lựa chọn: `[Hủy]` hoặc `[🗑️ Đồng ý xóa ngay]`, hỗ trợ mượt mà trên cả trình duyệt Web lẫn điện thoại.

---

### 5. Kiểm Tra Cô Lập Dữ Liệu Đa Tài Khoản (User Data Isolation)
1. Đăng nhập **Tài khoản A** (`sinhvien@dntu.vn`) $\rightarrow$ Thêm 1 lịch học mới (ví dụ: *"Lịch học riêng của A"*).
2. Vào tab **Cá nhân** $\rightarrow$ Nhấn **`🚪 Đăng xuất`**.
3. Đăng nhập **Tài khoản B** (`minhduy@student.edu.vn`) $\rightarrow$ Mở tab Lịch học: Đảm bảo **hoàn toàn không thấy dữ liệu của Tài khoản A**.
4. Đăng xuất B và đăng nhập lại A: Dữ liệu của A vẫn được bảo toàn nguyên vẹn 100%.

---

### 6. Đổi Giao Diện Sáng / Tối (Theme Switching)
- Vào tab **Cá nhân** $\rightarrow$ Mục **Giao diện hiển thị**:
  - Chọn **Tối (Dark Mode)**: Giao diện nền tối tương phản cao sang trọng.
  - Chọn **Sáng (Light Mode)**: Giao diện nền sáng trang nhã.
  - Chọn **Tự động (System)**: Tự động đổi theo cài đặt hệ điều hành của máy.

---

## 📱 Các Chức Năng Chính Đã Hoàn Thành Của Ứng Dụng

1. **Quản lý & Cập nhật bài tập:** Cho phép sinh viên đánh dấu hoàn thành bài tập nhanh chóng với giao diện trực quan, rõ ràng.
2. **Đính kèm Ảnh ghi chú:** Hỗ trợ chọn ảnh từ thư viện hoặc chụp ảnh để lưu lại đề bài và ghi chú cần thiết.
3. **Widget tóm tắt hạn nộp 48 giờ:** Hiển thị danh sách các bài tập sắp tới hạn trong 48 giờ giúp sinh viên dễ theo dõi.
4. **Giao diện thân thiện, hỗ trợ Dark/Light Mode:** Bố cục rõ ràng, dễ thao tác, có thể đổi theme Sáng / Tối theo ý thích.
5. **Xem Lịch và Hỗ trợ Ngoại tuyến cơ bản:** Hỗ trợ xem lịch học theo tuần và lưu trữ dữ liệu cục bộ khi mất mạng.
