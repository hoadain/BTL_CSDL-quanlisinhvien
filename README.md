# CHƯƠNG TRÌNH QUẢN LÍ SINH VIÊN
- Tác giả: Nguyễn Đình Hòa
- Mss: k215480106097
- lớp: k57kmt
- hoàn thành: 19-6-2024
  ## chức năng cơ bản
  ### 1. Quản lí
  1. quản lí sinh viên
     - thêm sinh viên : cho phép thêm sinh viên vào cơ sở dữ liệu
     - sửa sinh viên: cho phép sửa sinh viên vào cơ sở dữ liệu
     - xóa sinh viên: cho phép xóa sinh viên ra khỏi cơ sở dữ liệu
     - tìm kiếm sinh viên: hiển thị danh sách sinh viên
### 2. chức năng truy vấn
- tìm kiếm sinh viên
- xem thông tin sinh viên
- xem điểm của sinh viên
- xem môn học
### 3. Chức năng nâng cao
- cập nhập tính điểm trung bình: tự động tính điểm trung bình vào bảng

- tự động tính tích điểm của sinh viên và lưu vào bảng tương ứng
1. Báo cáo thống kê
    - hiển sinh viên và môn học trượt môn 
## Thiết kế chương trình trong Sql
### 1. Tạo các bảng
bảng sinh viên
- MaSinhVien🔑: Khóa chính được sử dụng để xác định mỗi sinh viên một cách duy nhất. Là một số nguyên (INT), giá trị này tự động tăng (AUTO_INCREMENT) để đảm bảo tính duy nhất và dễ quản lý của mỗi bản ghi sinh viên.
- HoTen: Tên đầy đủ của sinh viên (NVARCHAR(100)), không được NULL để đảm bảo mỗi sinh viên được lưu trữ đều có thông tin tên.
NgaySinh: Ngày sinh của sinh viên (DATE), lưu trữ thông tin về ngày tháng năm sinh của sinh viên.
- DiaChi: Địa chỉ của sinh viên (NVARCHAR(200)), lưu trữ thông tin chi tiết về địa chỉ nơi sinh viên ở.
- DienThoai: Số điện thoại của sinh viên (NVARCHAR(20)), để liên lạc và thông báo liên quan đến sinh viên.
- Email: Địa chỉ email của sinh viên (NVARCHAR(100)), để gửi thông tin quan trọng và liên lạc với sinh viên.
 ![z5555174365225_e4b5d69b09ab62fbc2356a8478bba3b2](https://github.com/hoadain/BTL_CSDL-quanlisinhvien/assets/168847370/a7d355aa-e635-4e5d-bdbd-319693558f62)
  Bảng môn học
-  MaMonHoc🔑: Khóa chính của bảng, là một số nguyên (INT) tự động tăng (AUTO_INCREMENT). Dùng để xác định mỗi môn học một cách duy nhất và dễ dàng quản lý.
- TenMonHoc: Tên của môn học (NVARCHAR(100)), không được NULL. Lưu trữ tên đầy đủ của môn học để dễ dàng nhận diện và hiển thị thông tin liên quan đến môn học này.
 ![z5555197536081_84dc1f71c117889bc3c98de7f8680210](https://github.com/hoadain/BTL_CSDL-quanlisinhvien/assets/168847370/c617a65c-08fb-42dc-9c40-06c0a7fedec5)
  Bảng Diem:
- MaSinhVien🔑: Đây là một phần của khóa chính (composite key) cùng với - -- MaMonHoc để xác định mỗi bản ghi một cách duy nhất.
- MaMonHoc🔑: Đây là một phần của khóa chính (composite key) cùng với - ---- MaSinhVien để xác định mỗi bản ghi một cách duy nhất.
- Diem: Điểm của sinh viên trong môn học tương ứng, được lưu trữ dưới dạng kiểu dữ liệu FLOAT.
- FOREIGN KEY (MaSinhVien): Tham chiếu đến bảng SinhVien với trường MaSinhVien, đảm bảo rằng chỉ có các MaSinhVien có sẵn trong bảng SinhVien mới có thể tồn tại trong bảng Diem.
- FOREIGN KEY (MaMonHoc): Chưa có định nghĩa cho bảng MonHoc, nhưng giả sử đây là một bảng khác lưu thông tin về các môn học, và MaMonHoc là khóa chính của bảng đó. ForeignKey này đảm bảo rằng chỉ có các MaMonHoc hợp lệ từ bảng MonHoc mới có thể tồn tại trong bảng Diem.
  ![z5555201773094_693c2e84eff28085af88ba6c0b1928b8](https://github.com/hoadain/BTL_CSDL-quanlisinhvien/assets/168847370/fe31f330-274a-4b35-8499-704fd179b37c)
### 2. Hàm thêm
--them sinh vien
```
CREATE PROCEDURE ThemSinhVien
    @MaSinhVien INT,
    @HoTen NVARCHAR(100),
    @NgaySinh DATE,
    @DiaChi NVARCHAR(200),
    @DienThoai NVARCHAR(20),
    @Email NVARCHAR(100)
AS
BEGIN
    INSERT INTO SinhVien (MaSinhVien, HoTen, NgaySinh, DiaChi, DienThoai, Email, DiemTrungBinh)
    VALUES (@MaSinhVien, @HoTen, @NgaySinh, @DiaChi, @DienThoai, @Email, NULL);
END;
```
-- them môn học
```
CREATE PROCEDURE ThemMonHoc
    @MaMonHoc INT,
    @TenMonHoc NVARCHAR(100)
AS
BEGIN
    INSERT INTO MonHoc (MaMonHoc, TenMonHoc)
    VALUES (@MaMonHoc, @TenMonHoc);
END;
```
--Theem điểm cho sv
```
CREATE PROCEDURE ThemDiem
    @MaSinhVien INT,
    @MaMonHoc INT,
    @Diem FLOAT
AS
BEGIN
    INSERT INTO Diem (MaSinhVien, MaMonHoc, Diem)
    VALUES (@MaSinhVien, @MaMonHoc, @Diem);
END;
```
### 2. Thêm dữ liệu vòa các bảng
-- Thêm sinh viên
```
EXEC ThemSinhVien 2, N'Trần Thị B', '2001-02-15', N'Hồ Chí Minh', '0901234567', 'ttb@example.com';
EXEC ThemSinhVien 3, N'Lê Văn C', '1999-09-30', N'Hà Nội', '0976543210', 'lvc@example.com';
EXEC ThemSinhVien 4, N'Phạm Thị D', '2002-05-20', N'Đà Nẵng', '0912345678', 'ptd@example.com';
EXEC ThemSinhVien 5, N'Nguyễn Thị E', '2003-03-05', N'Hà Nam', '0987654321', 'nte@example.com';
EXEC ThemSinhVien 6, N'Lý Văn F', '2000-06-12', N'Hải Phòng', '0976543210', 'lvf@example.com';
EXEC ThemSinhVien 7, N'Trần Thị ', '2002-10-25', N'Đồng Nai', '0912345678', 'ttg@example.com';
EXEC ThemSinhVien 8, N'Nguyễn Văn H', '2001-07-10', N'Hà Tĩnh', '0987654321', 'nvh@example.com';
EXEC ThemSinhVien 9, N'Trần Văn I', '1999-12-20', N'Bắc Giang', '0976543210', 'tvi@example.com';
EXEC ThemSinhVien 10, N'Lê Thị K', '2003-05-15', N'Quảng Ninh', '0912345678', 'ltk@example.com';
```
-- Thêm môn học
```
EXEC ThemMonHoc 2, N'Văn';
EXEC ThemMonHoc 3, N'Lý';
EXEC ThemMonHoc 4, N'Hóa';
EXEC ThemMonHoc 5, N'Tiếng Anh';
EXEC ThemMonHoc 6, N'Tin Học';
EXEC ThemMonHoc 7, N'Lịch Sử';
EXEC ThemMonHoc 8, N'Sinh Học';
EXEC ThemMonHoc 9, N'Địa Lý';
EXEC ThemMonHoc 10, N'Âm Nhạc';
```
-- Thêm điểm cho sinh viên
```
EXEC ThemDiem 2, 2, 8.0; -- Điểm 8.0 cho môn Văn của sinh viên có mã 2 (Trần Thị B)
EXEC ThemDiem 3, 3, 6.0; -- Điểm 6.0 cho môn Lý của sinh viên có mã 3 (Lê Văn C)
EXEC ThemDiem 4, 4, 7.2; -- Điểm 7.2 cho môn Hóa của sinh viên có mã 4 (Phạm Thị D)

EXEC ThemDiem 5, 5, 7.5; -- Điểm 7.5 cho môn Tiếng Anh của sinh viên có mã 5 (Nguyễn Thị E)
EXEC ThemDiem 6, 2, 9.0; -- Điểm 9.0 cho môn Văn của sinh viên có mã 6 (Lý Văn F)
EXEC ThemDiem 6, 6, 8.5; -- Điểm 8.5 cho môn Tin Học của sinh viên có mã 6 (Lý Văn F)
EXEC ThemDiem 7, 3, 7.0; -- Điểm 7.0 cho môn Lý của sinh viên có mã 7 (Trần Thị G)
EXEC ThemDiem 7, 7, 6.5; -- Điểm 6.5 cho môn Lịch Sử của sinh viên có mã 7 (Trần Thị G)
EXEC ThemDiem 8, 8, 7.5; -- Điểm 7.5 cho môn Sinh Học của sinh viên có mã 8 (Nguyễn Văn H)
EXEC ThemDiem 9, 2, 9.0; -- Điểm 9.0 cho môn Văn của sinh viên có mã 9 (Trần Văn I)3
EXEC ThemDiem 9, 9, 8.5; -- Điểm 8.5 cho môn Địa Lý của sinh viên có mã 9 (Trần Văn I)
EXEC ThemDiem 10, 3, 7.0; -- Điểm 7.0 cho môn Lý của sinh viên có mã 10 (Lê Thị K)
EXEC ThemDiem 10, 10, 6.5; -- Điểm 6.5 cho môn Âm Nhạc của sinh viên có mã 10 (Lê Thị K)
```
## Thiết lập chức năng
### 1. Một số chức năng cơ bản
1.1 Chức năng tìm kiếm thống tin
* Lấy thông tin sinh viên
  ```
  SELECT * FROM Sinhvien WHERE Masinhvien = (id của sinhvien cần tìm);
  ```
1.2 Chức năng thêm, sửa, xóa
```
CREATE PROCEDURE ThemSinhVien
    @MaSinhVien INT,
    @HoTen NVARCHAR(100),
    @NgaySinh DATE,
    @DiaChi NVARCHAR(200),
    @DienThoai NVARCHAR(20),
    @Email NVARCHAR(100),
    @MaKhoa INT
AS
BEGIN
    INSERT INTO SinhVien (MaSinhVien, HoTen, NgaySinh, DiaChi, DienThoai, Email, DiemTrungBinh, MaKhoa)
    VALUES (@MaSinhVien, @HoTen, @NgaySinh, @DiaChi, @DienThoai, @Email, NULL, @MaKhoa);
END;
GO
-- sua sinh vien
CREATE PROCEDURE SuaSinhVien
    @MaSinhVien INT,
    @HoTen NVARCHAR(100),
    @NgaySinh DATE,
    @DiaChi NVARCHAR(200),
    @DienThoai NVARCHAR(20),
    @Email NVARCHAR(100),
    @MaKhoa INT
AS
BEGIN
    UPDATE SinhVien
    SET HoTen = @HoTen,
        NgaySinh = @NgaySinh,
        DiaChi = @DiaChi,
        DienThoai = @DienThoai,
        Email = @Email,
        MaKhoa = @MaKhoa
    WHERE MaSinhVien = @MaSinhVien;
END;
GO
-- xoa sinh vien
CREATE PROCEDURE XoaSinhVien
    @MaSinhVien INT
AS
BEGIN
    DELETE FROM Diem WHERE MaSinhVien = @MaSinhVien;  -- Delete all grades related to the student
    DELETE FROM SinhVien WHERE MaSinhVien = @MaSinhVien;  -- Delete the student record
END;
GO
EXEC ThemSinhVien 11, N'Nguyễn Văn L', '2002-08-10', N'Thái Bình', '0909876543', 'nvl@example.com', 1;
EXEC SuaSinhVien 11, N'Nguyễn Văn L', '2002-08-10', N'Hải Dương', '0909876543', 'nvl_updated@example.com', 2;
EXEC XoaSinhVien 11;
```
### 2.CHức năng nâng cao
-- cập nhập tính điểm trung bình: tự động tính điểm trung bình vào bảng

-- Tạo trigger tính điểm trung bình của sinh viên khi có điểm mới được thêm vào bảng Diem
CREATE TRIGGER tinh_diem_trung_binh
ON Diem
AFTER INSERT, UPDATE
AS
BEGIN
    DECLARE @v_tong_diem FLOAT;
    DECLARE @v_so_mon INT;
    
    SELECT @v_tong_diem = SUM(Diem), @v_so_mon = COUNT(*)
    FROM Diem
    WHERE MaSinhVien = (SELECT MaSinhVien FROM inserted);

    IF @v_so_mon > 0
    BEGIN
        UPDATE SinhVien
        SET DiemTrungBinh = @v_tong_diem / @v_so_mon
        WHERE MaSinhVien = (SELECT MaSinhVien FROM inserted);
    END
END;
GO
```

![z5555277601365_61c016a2f05361a9a295abacc3f687d3](https://github.com/hoadain/BTL_CSDL-quanlisinhvien/assets/168847370/601ea6fb-2aea-4133-85bd-07f706fde585)

- tự động tính tích điểm của sinh viên và lưu vào bảng tương ứng
  -- Tạo bảng DiemChu để lưu các điểm chữ và khoảng điểm tương ứng
```
CREATE TABLE DiemChu (
    DiemChu NVARCHAR(10) PRIMARY KEY,
    DiemMin FLOAT,
    DiemMax FLOAT
);

Thêm dữ liệu cho bảng DiemChu, bao gồm cả "A+"
INSERT INTO DiemChu (DiemChu, DiemMin, DiemMax)
VALUES 
    ('A+', 9.0, 10),
    ('A', 8.5, 8.99),
    ('B+', 7.5, 8.49),
    ('B', 6.5, 7.49),
    ('C+', 5.5, 6.49),
    ('C', 4.0, 5.49),
    ('F', 0, 3.99);
```
-- Xóa trigger cũ nếu tồn tại

```
IF EXISTS (SELECT * FROM sys.triggers WHERE name = 'update_DiemChu')
BEGIN
    DROP TRIGGER update_DiemChu;
END;
GO
```
-- Tạo lại trigger mới để cập nhật cột DiemChu
```
CREATE TRIGGER update_DiemChu
ON Diem
AFTER INSERT	
AS
BEGIN
    DECLARE @MaSinhVien INT;
    DECLARE @MaMonHoc INT;
    DECLARE @Diem FLOAT;
    DECLARE @DiemChu NVARCHAR(10);

    -- Duyệt qua các bản ghi mới được thêm vào bảng Diem
    DECLARE cur CURSOR FOR
    SELECT MaSinhVien, MaMonHoc, Diem
    FROM inserted;

    OPEN cur;
    FETCH NEXT FROM cur INTO @MaSinhVien, @MaMonHoc, @Diem;

    WHILE @@FETCH_STATUS = 0
    BEGIN
        -- Tính điểm chữ
        IF @Diem >= 9.0 AND @Diem <= 10
            SET @DiemChu = 'A+';
        ELSE
        BEGIN
            SELECT TOP 1 @DiemChu = DiemChu
            FROM DiemChu
            WHERE @Diem >= DiemMin AND @Diem <= DiemMax
            ORDER BY DiemMin DESC; -- Lấy điểm chữ cao nhất thỏa điều kiện
        END

        -- Cập nhật cột DiemChu trong bảng Diem
        UPDATE Diem
        SET DiemChu = @DiemChu
        WHERE MaSinhVien = @MaSinhVien AND MaMonHoc = @MaMonHoc;

        FETCH NEXT FROM cur INTO @MaSinhVien, @MaMonHoc, @Diem;
    END

    CLOSE cur;
    DEALLOCATE cur;
  END;
GO
```

![z5555288375371_fbd279b9759e40e938cea8aef89f2488](https://github.com/hoadain/BTL_CSDL-quanlisinhvien/assets/168847370/2f9dd10e-7ca7-4e9f-bf65-fb98665918f5)

- hiển thị những sinh viên thi trượt có điểm tích F

```
CREATE VIEW SinhVien_MonHoc_F
AS
SELECT sv.MaSinhVien, sv.HoTen AS HoTenSV, sv.NgaySinh,
       mh.MaMonHoc, mh.TenMonHoc,
       dc.DiemChu
FROM SinhVien sv
JOIN Diem d ON sv.MaSinhVien = d.MaSinhVien
JOIN MonHoc mh ON d.MaMonHoc = mh.MaMonHoc
JOIN DiemChu dc ON d.DiemChu = dc.DiemChu
WHERE dc.DiemChu = 'F';
SELECT * FROM SinhVien_MonHoc_F; 
```

![z5555315296831_6cf4a9cd18c9201dc381ee57cbbd830e](https://github.com/hoadain/BTL_CSDL-quanlisinhvien/assets/168847370/0208a8b8-fc22-4bc4-ab84-e46fc027036f)




  
