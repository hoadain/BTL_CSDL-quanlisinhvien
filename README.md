# CHƯƠNG TRÌNH QUẢN LÍ SINH VIÊN
Mô tả bài toán quản lý: là một bài toán quan trọng trong nhiều tổ chức giáo dục, có thể quản lí được thông tin và học tập của sinh viên, tạo điều kiện thuận lợi cho việc tra cứu và báo cáo thông tin, hỗ trợ công tác giảng dạy và quản lí khóa học và theo dõi tiến độ học tập của sinh viên

## Những chức năng xây dụng quản lí sinh viên:
1. Thêm, sửa, xóa sinh viên
2. Tìm kiếm, lọc và tìm kiếm sinh viên theo ý của người dùng như tên, id...
3. Xem số lượng sinh viên
4. Quản lí điểm số
## Các thông tin liên quan đến sinh viên
1. Thông tin cá nhân
   - Mã sinh viên: mã định danh duy nhất của mỗi sinh viên
   - Họ và tên: họ và tên đầy đủ của sinh viên
   - Ngày sinh: ngày, tháng, năm sinh cảu sinh viên
   - Giới tính: nam hoặc nữ
   - Địa chỉ: địa chỉ hiện tại của sinh viên
   - Dân tộc: kinh hoặc dân tộc khác
2. Chương trình
   - Mã chương trình
   - Tên chương trình
3. Môn học
   - Mã môn học
   - tên môn học: tên môn học của sinh viên
   - Ma khoa
4. kết quả
   - Mã sinh viên: chỉ có một mã định danh duy nhất
   - Mã môn học
   - lần thi: số lần thi của sinh viên
   - điểm: số điểm mà sinh viên thi được

1. khoa: 🔑Mã khoa, tên khoa, năm thành lập
2. khóa học: 🔑 Mã khóa học, năm bắt đàu, năm kết thúc
3. chương trình học:🔑 mã chương trình, tên chương trình
4. lớp: 🔑 mã lớp, mã khoa, mã khóa học, mã chương trình, số tiến chỉ
5. sinh viên:🔑mã sinh viên, họ tên, năm sinh, dân tộc, mã lớp
6. Môn học:🔑 mã môn học, mã khoa, tên môn học
7. kết quả:🔑 mã sinh viên, mã môn học, lần thi, điể
8. giảng khoa: 🔑 mã chương trình, mã khoa, mã môn học, năm học, học kì

Như vậy, dựa theo những thông tin mà ta đã thu thập được chúng ta sẽ xây dựng các bảng đáp ứng yếu cầu quản lí sinh viên
Tạo các bảng như mô tả trong SQL sever:
1. Bảng khoa
- create table Khoa
- (
	- Ma_Khoa varchar(10) primary key, -- 13248   KTCC126  123
	- Ten_Khoa nvarchar(100), -- thêm n để viết được unicode
	- Nam_Thanh_Lap int
- )
go
2. Bảng khóa học
- create table Khoa_Hoc
- (
  	- Ma_Khoa_Hoc varchar(10) primary key, 
	- Nam_Bat_Dau int,
	- Nam_Ket_Thuc int
- )
3. Bảng chương trình học
- create table Chuong_Trinh_Hoc
- (
	- Ma_CT varchar(10) primary key,
	- Ten_CT nvarchar(100)
- )
go
4. bảng lớp
- create table Lop
- (
	- Ma_Lop varchar(10) primary key,
	- Ma_Khoa varchar(10) not null,
	- Ma_Khoa_Hoc varchar(10) not null,
	- Ma_CT varchar(10) not null,
	- STT int

	- foreign key(Ma_Khoa) references Khoa(Ma_Khoa),
	- foreign key(Ma_Khoa_Hoc) references Khoa_Hoc(Ma_Khoa_Hoc),
	- foreign key(Ma_CT) references Chuong_Trinh_Hoc(Ma_CT)
- )
go
5. Bảng sinh viên
- create table Sinh_Vien
- (
	- MaSV varchar(10) primary key,
	- Ho_Ten nvarchar(100),
	- Nam_Sinh int,
	- Dan_Toc nvarchar(20),
	- Ma_Lop varchar(10) not null

	- foreign key(Ma_Lop) references Lop(Ma_Lop)
- )
go
6. Bảng môn học
- create table Mon_Hoc
- (
	- MaMH varchar(10) primary key,
	- Ma_Khoa varchar(10) not null,
	- TenMH nvarchar(100)

	- foreign key(Ma_Khoa) references Khoa(Ma_Khoa)
- )
go
7. Bảng kết quả
- create table Ket_Qua
- (
	- MaSV varchar(10) not null,
	- MaMH varchar(10) not null,	
	- Lan_Thi int not null,
	- Diem_Thi float

	- primary key(MaSV, MaMH, Lan_Thi),

	- foreign key(MaSV) references Sinh_Vien(MaSV),
	- foreign key(MaMH) references Mon_Hoc(MaMH)
- )
go
8. Bảng giảng khoa
- create table Giang_Khoa
- (
	- Ma_CT varchar(10) not null,
	- Ma_Khoa varchar(10) not null,	
	- MaMH varchar(10) not null,
	- Nam_Hoc int not null,
	- Hoc_Ky int,
	- STLT int,
	- STTH int,
	- So_Tin_Chi int

	- primary key(Ma_CT, Ma_Khoa, MaMH,Nam_Hoc),

	- foreign key(Ma_CT) references Chuong_Trinh_Hoc(Ma_CT),
	- foreign key(Ma_Khoa) references Khoa(Ma_Khoa),
	- foreign key(MaMH) references Mon_Hoc(MaMH)
- )

go
   Tạo sơ đồ thực thể liên kết giữa các bảng
   ![z5540598903266_9ae1ed7324d68ee2f510b5b90ba8b6bd](https://github.com/hoadain/BTL_CSDL-quanlisinhvien/assets/168847370/a736c2f4-2c4c-47be-9962-91d7a29ca335)
Thêm dữ liệu vào các bảng
1. dữ liệu bảng khoa
 - Insert into Khoa(Ma_Khoa, Ten_Khoa, Nam_Thanh_Lap) values( 'CNTT', N'Công nghệ thông tin',1995)
go
- Insert into Khoa values('VL', N'Vật Lý' , 1970)

- Insert into Khoa values('TH', N'Triết học' , 1978)
go
2. dữ liệu bảng khóa học
- Insert into Khoa_Hoc(Nam_Bat_Dau, Ma_Khoa_Hoc, Nam_Ket_Thuc) values( 2002, 'K2002',2006)
go
- Insert into Khoa_Hoc values('K2003', 2003, 2007)
go
- Insert into Khoa_Hoc values('K2004', 2004, 2008)
go
3. nhập dữ liệu sinh viên
- Insert into Sinh_Vien values('0212001', N'Nguyễn Vĩnh An', 1984, N'Kinh', 'TH2002/01')
go
- Insert into Sinh_Vien values('0212002', N'Nguyễn Thanh Bình', 1985, N'Kinh', 'TH2002/01')
go
- Insert into Sinh_Vien values('0212003', N'Nguyễn Thanh Cường', 1984, N'Kinh', 'TH2002/02')
go
- Insert into Sinh_Vien values('0212004', N'Nguyễn Quốc Duy', 1983, N'Kinh', 'TH2002/02')
go
- Insert into Sinh_Vien values('0311001', N'Phan Tuấn Anh', 1985, N'Kinh', 'TH2003/01')
go
- Insert into Sinh_Vien values('0311002', N'Huỳnh Thanh Sang', 1984, N'Kinh', 'TH2003/01')
go
4. Nhập dữ liệu cho chương trình
- Insert into Chuong_Trinh_Hoc values('CQ', N'Chính Quy')
go
5. Nhập dữ liệu cho Môn học
- Insert into Mon_Hoc values('THT01', 'CNTT', N'Toán cao cấp A1')
go
- Insert into Mon_Hoc values('VLT01', 'VL', N'Toán cao cấp A1')
go
- Insert into Mon_Hoc values('THT02', 'CNTT', N'Toán rời rạc')
go
- Insert into Mon_Hoc values('THCS01', 'CNTT', N'Cấu trúc dữ liệu 1')
go
- Insert into Mon_Hoc values('THCS02', 'CNTT', N'Hệ điều hành')
go
6. Nhâp dữ liệu cho kết quả
- Insert into Ket_Qua values('0212001', 'THT01', 1,4)
go
- Insert into Ket_Qua values('0212001', 'THT01', 2,7)
go
- Insert into Ket_Qua values('0212002', 'THT01', 1,8)
go
- Insert into Ket_Qua values('0212003', 'THT01', 1,6)
go
- Insert into Ket_Qua values('0212004', 'THT01', 1,9)
go
- Insert into Ket_Qua values('0212001', 'THT02', 1,8)
go
- Insert into Ket_Qua values('0212002', 'THT02', 1,5.5)
go
- Insert into Ket_Qua values('0212003', 'THT02', 1,4)
go
- Insert into Ket_Qua values('0212003', 'THT02', 2,6)
go
- Insert into Ket_Qua values('0212001', 'THCS01', 1,6.5)
go
- Insert into Ket_Qua values('0212002', 'THCS01', 1,4)
go
- Insert into Ket_Qua values('0212003', 'THCS01', 1,7)
go
7. Nhập dữ liệu cho giảng khoa
- Insert into Giang_Khoa values('CQ', 'CNTT', 'THT01',2003, 1, 60, 30, 5)
go
- Insert into Giang_Khoa values('CQ', 'CNTT', 'THT02',2003, 2, 45, 30, 4)
go
- Insert into Giang_Khoa values('CQ', 'CNTT', 'THCS01',2004, 1, 45, 30, 4)
go
8. nhập dữ liệu cho lớp
- Insert into Lop values('TH2002/01', 'CNTT','K2002', 'CQ', 1)
go
- Insert into Lop values('TH2002/02', 'CNTT','K2002', 'CQ', 2)
go
- Insert into Lop values('TH2003/01', 'VL','K2003', 'CQ', 1)
go
## XÂY DỰNG CÁC THỦ TỤC THOE CHỨC NĂNG MONG MUỐN 
