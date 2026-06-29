# 🎵 Web Nghe Nhạc (Music Website)

Đồ án tốt nghiệp — Website nghe nhạc trực tuyến với đầy đủ chức năng quản lý (CRUD) cho Admin và trải nghiệm nghe nhạc cho người dùng.

---

## 📑 Mục lục

1. [Giới thiệu dự án](#-giới-thiệu-dự-án)
2. [Công nghệ sử dụng](#-công-nghệ-sử-dụng)
3. [Cấu trúc Database](#-cấu-trúc-database)
4. [Cài đặt môi trường](#-cài-đặt-môi-trường)
5. [Hướng dẫn cài SQL Server (SSMS)](#-hướng-dẫn-cài-sql-server-ssms)
6. [Khởi tạo Database](#-khởi-tạo-database)
7. [Chạy project bằng Visual Studio](#-chạy-project-bằng-visual-studio)
8. [Tài khoản mặc định](#-tài-khoản-mặc-định)
9. [Chức năng đồ án (Map)](#-chức-năng-đồ-án-map)
10. [Các lỗi thường gặp & Cách fix](#-các-lỗi-thường-gặp--cách-fix)

---

## 🎯 Giới thiệu dự án

**Web Nghe Nhạc** là ứng dụng web cho phép người dùng nghe nhạc trực tuyến theo chủ đề, thể loại, ca sĩ; đồng thời có hệ thống quản trị cho phép Admin quản lý bài hát, ca sĩ, chủ đề, thể loại và tài khoản người dùng.

Dự án được xây dựng bằng **ASP.NET MVC (C#)** kết nối **SQL Server** thông qua **LINQ to SQL (DBML)**. Thiết kế giao diện dùng Bootstrap 3 + jQuery.

---

## 🛠 Công nghệ sử dụng

| Hạng mục | Công nghệ |
|----------|-----------|
| Backend | ASP.NET MVC 5, C#, .NET Framework 4.7.2 |
| ORM | LINQ to SQL (`.dbml` — `DBcontext.dbml`) |
| Database | SQL Server 2019 / SQL Server Express |
| Frontend | Bootstrap 3.4.1, jQuery, Razor View |
| Auth | Forms Authentication |
| IDE | Visual Studio 2019 / 2022 |

---

## 🗂 Cấu trúc Database

Database tên: **`WebNhac`** (script tạo tại `scriptnhac.sql` ở thư mục gốc repo).

```
WebNhac
├── account       (Tài khoản: MaTK, Email, PassWord, Role, Ten)
├── CaSi          (Ca sĩ: MaCS, TenCS)
├── ChuDe         (Chủ đề: MaCD, TenCD, Picture, Color)
├── TheLoai       (Thể loại: MaTL, TenTL)
├── Nhac          (Bài hát: MaBH, TenBH, Files, image, NgayPh, MaCS, MaTL, MaCD)
└── PlayList      (Playlist: stt, Matk, MaBH, TenPL)
```

**Quan hệ:**
- `Nhac` ⟶ FK ⟶ `CaSi`, `TheLoai`, `ChuDe`
- `PlayList` ⟶ FK ⟶ `Nhac`, `account`

**Role tài khoản:** `1` = Admin, `0` = User thường

---

## ⚙️ Cài đặt môi trường

Yêu cầu trước khi bắt đầu:

1. **Visual Studio 2019/2022** (có workload "ASP.NET and web development")
2. **SQL Server 2019** hoặc **SQL Server Express** (≥ 2017)
3. **SQL Server Management Studio (SSMS)** — dùng để quản lý DB

---

## 📥 Hướng dẫn cài SQL Server (SSMS)

### Bước 1: Cài SQL Server Express

1. Tải SQL Server Express tại: https://www.microsoft.com/en-us/sql-server/sql-server-downloads
2. Chọn **Express** → tải và chạy installer.
3. Chọn **Basic** installation → đặt instance name là **`SQLEXPRESS`** (mặc định) hoặc đặt tên khác (ví dụ `SQLEXPRESS01`) — **nhớ chính xác instance name này vì sẽ dùng ở bước sau**.

> ⚠️ Nếu bạn clone project này về và `Web.config` đang dùng `localhost\SQLEXPRESS01`, bạn **BẮT BUỘC** phải đặt instance là `SQLEXPRESS01`, hoặc đổi connection string cho khớp.

### Bước 2: Cài SQL Server Management Studio (SSMS)

1. Tải SSMS tại: https://learn.microsoft.com/en-us/ssms/download-sql-server-management-studio-ssms
2. Chạy installer → cài đặt bình thường.
3. Mở SSMS, đăng nhập với:
   - **Server name:** `localhost\SQLEXPRESS01` (hoặc instance bạn vừa tạo)
   - **Authentication:** `Windows Authentication`
   - Nhấn **Connect**.

---

## 🗄 Khởi tạo Database

1. Mở SSMS → kết nối tới SQL Server của bạn.
2. **File → Open → File...** → chọn file `scriptnhac.sql` ở thư mục gốc repo.
3. Nhấn **Execute** (phím F5) để chạy toàn bộ script → DB `WebNhac` được tạo cùng dữ liệu mẫu.
4. Trong SSMS, click phải **Databases** → **Refresh** → thấy database `WebNhac` là OK.

> 💡 Nếu bị lỗi đường dẫn `.mdf` cứng trong script, xem mục [Lỗi #1](#-các-lỗi-thường-gặp--cách-fix).

---

## ▶️ Chạy project bằng Visual Studio

### Bước 1: Clone & mở project

```bash
git clone <url-repo-cua-ban>
cd Webnghenhac
```

Mở file **`src/Webnghenhac/WebApplication1.csproj`** bằng Visual Studio (hoặc mở solution nếu có).

> ⚠️ Project này gốc là `WebApplication1`. Nếu bạn đổi tên folder, file `.csproj` cần cập nhật cho khớp.

### Bước 2: Restore NuGet packages

- Chuột phải **Solution** → **Restore NuGet Packages**
- Hoặc mở **Package Manager Console** gõ:
  ```
  Update-Package -reinstall
  ```

### Bước 3: Sửa Connection String

Mở `src/Webnghenhac/Web.config`, tìm:

```xml
<add name="WebNhacConnectionString" 
     connectionString="Data Source=localhost\SQLEXPRESS01;Initial Catalog=WebNhac;Integrated Security=True;" 
     providerName="System.Data.SqlClient" />
```

Sửa `Data Source` cho đúng instance SQL Server của bạn:

| SQL Server của bạn | Sửa thành |
|--------------------|-----------|
| SQLEXPRESS (mặc định) | `localhost\SQLEXPRESS` |
| SQLEXPRESS01 (mặc định repo) | `localhost\SQLEXPRESS01` |
| Bản full SQL Server | `localhost` hoặc `.\MSSQLSERVER` |

### Bước 4: Build & Run

- Nhấn **F5** (Debug) hoặc **Ctrl + F5** (Run without debug).
- Trình duyệt mở trang chủ → dùng tài khoản admin bên dưới để đăng nhập.

---

## 🔑 Tài khoản mặc định

> Tài khoản này đã có sẵn trong `scriptnhac.sql` sau khi Execute.

### 👨‍💼 Admin

| Field | Value |
|-------|-------|
| Email | `admin@gmail.com` |
| Password | `admin123` |
| Role | `1` |
| Tên hiển thị | `Phước` |

Quyền: truy cập tất cả trang quản lý (QLTK, QLBH, Casi, ChuDe, TheLoai).

### 👤 Người dùng thường

| Email | Password | Role |
|-------|----------|------|
| `vohuuphuoc1@gmail.com` | `123` | User |
| `vohuuphuoc2@gmail.com` | `1234` | User |

Quyền: nghe nhạc, tạo playlist cá nhân.

> 📝 **Đăng ký user mới:** người dùng có thể tự đăng ký qua trang `/login/SignUp` — sẽ được tạo với `Role = 0`.

---

## 🗺 Chức năng đồ án (Map)

### Phía người dùng (User/Admin đều dùng được)

| # | Chức năng | Controller | View |
|---|-----------|------------|------|
| 1 | Đăng nhập | `loginController` | `/login/Login` |
| 2 | Đăng ký tài khoản mới | `loginController` | `/login/SignUp` |
| 3 | Đăng xuất | `loginController` | `/login/Logout` |
| 4 | Trang chủ — danh sách bài hát | `HomeController` | `/Home/Index` |
| 5 | Nghe bài hát chi tiết | `HomeController` | `/Home/Baihat` |
| 6 | Tìm kiếm bài hát | `HomeController` | `/Home/Search`, `/Home/TimKiem` |
| 7 | Bài hát yêu thích | `HomeController` | `/Home/Baihatyeuthich` |
| 8 | Thư viện cá nhân | `HomeController` | `/Home/Library` |
| 9 | Xem playlist | `HomeController` | `/Home/Playlist` |
| 10 | Thêm playlist | `HomeController` | `/Home/ThemPL` |

### Phía Admin (yêu cầu `Role = 1`)

| # | Chức năng | Controller | View |
|---|-----------|------------|------|
| 11 | **Quản lý bài hát** (CRUD) | `QLBHController` | `/QLBH/...` |
| 12 | **Quản lý ca sĩ** (CRUD) | `CasiController` | `/Casi/...` |
| 13 | **Quản lý chủ đề** (CRUD) | `ChudeController` | `/Chude/...` |
| 14 | **Quản lý thể loại** (CRUD) | `TheloaiController` | `/Theloai/...` |
| 15 | **Quản lý tài khoản** (CRUD) | `QLTKController` | `/QLTK/...` |

---

## ❗ Các lỗi thường gặp & Cách fix

### Lỗi #1: Không tạo được Database — lỗi đường dẫn `.mdf` cứng

**Triệu chứng:**
```
Msg 5133, Level 16, State 1, Line 1
Directory lookup for the file "C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\..." failed
```

**Nguyên nhân:** File `scriptnhac.sql` đang hard-code đường dẫn `.mdf` theo máy tác giả.

**Cách fix:** Mở `scriptnhac.sql`, tìm đoạn:

```sql
CREATE DATABASE [WebNhac]
 CONTAINMENT = NONE
 ON PRIMARY 
( NAME = N'WebNhac', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\DATA\WebNhac.mdf' , ...)
```

→ Đổi `FILENAME = N'C:\Program Files\...'` thành `FILENAME = N'WebNhac.mdf'` (để SQL Server tự chọn đường dẫn DATA mặc định).

---

### Lỗi #2: "A network-related or instance-specific error occurred" / không connect được SQL Server

**Triệu chứng khi chạy web:**
```
SqlException: A network-related or instance-specific error occurred...
Server not found / Cannot connect to localhost\SQLEXPRESS01
```

**Cách fix:**
1. Mở **SQL Server Configuration Manager** → **SQL Server Services** → chắc chắn service `SQL Server (SQLEXPRESS01)` đang **Running**.
2. Nếu instance của bạn tên khác (ví dụ `SQLEXPRESS` mặc định), sửa `Web.config`:
   ```xml
   Data Source=localhost\SQLEXPRESS
   ```
3. Mở **SQL Server Configuration Manager** → **SQL Server Network Configuration** → Protocols for `<instance>` → bật **TCP/IP**.

---

### Lỗi #3: "Cannot open database 'WebNhac' requested by the login"

**Nguyên nhân:** Chưa chạy `scriptnhac.sql` để tạo DB.

**Cách fix:** Quay lại mục [Khởi tạo Database](#-khởi-tạo-database).

---

### Lỗi #4: "Login failed for user" khi chạy web

**Triệu chứng:**
```
SqlException: Login failed for user '...'
```

**Nguyên nhân:** `Web.config` đang dùng `Integrated Security=True` nhưng user Windows hiện tại không có quyền truy cập SQL Server.

**Cách fix:**
- Mở SSMS → kết nối thành công bằng Windows Auth → chứng tỏ Windows Auth đang hoạt động.
- Thử chạy Visual Studio bằng **Run as Administrator**.
- Hoặc đổi connection string sang SQL Auth:
  ```xml
  Data Source=localhost\SQLEXPRESS01;Initial Catalog=WebNhac;User Id=sa;Password=yourPassword;
  ```

---

### Lỗi #5: Lỗi build — "Metadata file ... DLL could not be found"

**Cách fix:**
1. Chuột phải **Solution** → **Clean Solution**
2. **Rebuild Solution** (Ctrl + Shift + B)
3. Nếu vẫn lỗi: chuột phải **Solution** → **Restore NuGet Packages** rồi build lại.

---

### Lỗi #6: "The type or namespace 'WebApplication1' could not be found"

**Nguyên nhân:** Folder/namespace project bị đổi tên nhưng code vẫn ở namespace `WebApplication1` cũ.

**Cách fix:**
- Mở file `.csproj`, sửa `<RootNamespace>` và `<AssemblyName>` cho khớp với namespace hiện tại trong các file `.cs`.
- Hoặc giữ nguyên namespace `WebApplication1` cho mọi file (đơn giản nhất khi clone về).

---

### Lỗi #7: FormsAuth login không nhớ, cookie bị mất

**Triệu chứng:** Click vào trang admin → bị đá về `/login/Login`.

**Cách fix:** Đảm bảo `Web.config` có:
```xml
<authentication mode="Forms">
  <forms loginUrl="/login/Login"></forms>
</authentication>
```
(Nếu thiếu, vào thêm tay theo cú pháp trên.)

---

### Lỗi #8: Không upload được file nhạc (> vài MB)

**Cách fix:** Trong `Web.config` đã có:
```xml
<httpRuntime maxRequestLength="1073741824" />
```
Đây là giới hạn 1 GB. Nếu vẫn lỗi, mở IIS Express config hoặc `applicationhost.config` để tăng `maxAllowedContentLength`.

---

### Lỗi #9: "IDENTITY_INSERT already ON" khi chạy lại `scriptnhac.sql`

**Nguyên nhân:** Chạy script 2 lần → INSERT bị trùng ID.

**Cách fix:** Trước khi chạy lại, trong SSMS:
```sql
USE WebNhac;
DELETE FROM PlayList;
DELETE FROM Nhac;
DELETE FROM ChuDe;
DELETE FROM CaSi;
DELETE FROM TheLoai;
DELETE FROM account;
DBCC CHECKIDENT('account', RESEED, 999);
```
Rồi chạy lại script.

---

### Lỗi #10: Lỗi tiếng Việt — font bị lỗi khi insert từ SSMS

**Triệu chứng:** Tiếng Việt trong DB hiện thành `?` hoặc ký tự lạ.

**Cách fix:**
1. Trong SSMS, gõ lệnh trước khi chạy script:
   ```sql
   SET LANGUAGE Vietnamese;
   ```
2. Hoặc đảm bảo cột trong DB đang dùng kiểu `nvarchar` (đã đúng trong script này).

---

## 📁 Cấu trúc thư mục

```
Webnghenhac/
├── README.md                          ← File này
├── scriptnhac.sql                     ← Script tạo DB + dữ liệu mẫu
└── src/
    └── Webnghenhac/                   ← Project ASP.NET MVC
        ├── WebApplication1.csproj     ← File project (open bằng VS)
        ├── Web.config                 ← Connection string
        ├── packages.config
        ├── Global.asax / Global.asax.cs
        ├── App_Start/
        │   ├── BundleConfig.cs
        │   ├── FilterConfig.cs
        │   └── RouteConfig.cs
        ├── Controllers/
        │   ├── HomeController.cs       (Trang chủ + nghe nhạc)
        │   ├── loginController.cs      (Đăng nhập / Đăng ký)
        │   ├── QLBHController.cs       (CRUD bài hát - Admin)
        │   ├── CasiController.cs       (CRUD ca sĩ - Admin)
        │   ├── ChudeController.cs      (CRUD chủ đề - Admin)
        │   ├── TheloaiController.cs    (CRUD thể loại - Admin)
        │   └── QLTKController.cs       (CRUD tài khoản - Admin)
        ├── Models/
        │   ├── DBcontext.dbml          (LINQ to SQL mapping)
        │   └── DBcontext.designer.cs
        ├── Views/
        │   ├── Home/                   (Trang người dùng)
        │   ├── login/                  (Login + SignUp)
        │   ├── QLBH/ Casi/ Chude/ Theloai/ QLTK/  (Trang Admin)
        │   └── Shared/                 (Layout, Error)
        ├── Content/                    (CSS, Bootstrap)
        ├── Scripts/                    (jQuery, Bootstrap JS)
        └── fonts/
```

---

## 📝 Ghi chú thêm

- **Mật khẩu lưu plain text** trong DB (chưa hash) — đây là điểm trừ bảo mật, nếu làm đồ án nâng cao nên hash bằng `BCrypt` hoặc `PasswordHasher`.
- **Không phân quyền chi tiết** theo Role ở controller — chỉ dùng `[Authorize]` chung. Admin truy cập trang admin thông qua kiểm tra thủ công `Role == 1` trong view.
- **File nhạc `.mp3`** lưu ở thư mục riêng (xem cột `Files` trong bảng `Nhac`); cần cấu hình IIS để serve static file ngoài `Content/`.

---

## 👤 Tác giả



---

> 💬 Nếu gặp lỗi chưa có trong danh sách trên, vui lòng mở **Issue** trên GitHub kèm screenshot và message lỗi.
