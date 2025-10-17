# Buổi 2: 

## A. Lý thuyết thiết kế cơ sở dữ liệu (Database Design)

### I. Mục tiêu của việc thiết kế cơ sở dữ liệu

Thiết kế cơ sở dữ liệu (Database Design) không chỉ là “tạo bảng và cột”, mà là **quá trình mô hình hóa thế giới thực** thành một cấu trúc dữ liệu có thể lưu trữ, truy vấn, và bảo toàn thông tin một cách **chính xác, an toàn, hiệu quả**. Mục tiêu của việc thiết kế cơ sở dữ liệu được chia thành 4 nhóm lớn:

#### 1. Đảm bảo **tính đúng đắn và toàn vẹn dữ liệu**

1. **Phản ánh trung thực thế giới thực**

   * Dữ liệu trong CSDL phải mô tả chính xác các thực thể (ví dụ: Sinh viên, Môn học, Đơn hàng, Khách hàng) và mối quan hệ giữa chúng.
   * Mọi ràng buộc nghiệp vụ (business rules) đều được thể hiện rõ trong mô hình dữ liệu.
   * Ví dụ: “Một sinh viên có thể đăng ký nhiều môn học, nhưng một môn học chỉ thuộc về một khoa” → cần có quan hệ 1–N và ràng buộc khóa ngoại tương ứng.

2. **Giữ toàn vẹn dữ liệu (Data Integrity)**
   Có ba dạng toàn vẹn cơ bản:

   * **Toàn vẹn thực thể (Entity integrity):** Mỗi bản ghi có danh tính duy nhất → dùng **Primary Key (PK)**.
   * **Toàn vẹn tham chiếu (Referential integrity):** Dữ liệu giữa các bảng phải khớp nhau → dùng **Foreign Key (FK)**.
   * **Toàn vẹn miền giá trị (Domain integrity):** Dữ liệu hợp lệ trong phạm vi, kiểu dữ liệu phù hợp

   *Ví dụ:*
   * Không thể có điểm lớn hơn 10 (`CHECK (diem <= 10)`),
   * Không thể tồn tại bản ghi đăng ký mà sinh viên đó đã bị xóa khỏi bảng `SinhVien`.

#### 2. Tránh **dư thừa và bất nhất dữ liệu (redundancy & anomalies)**

1. **Dư thừa dữ liệu** gây ra:

   * **Tốn bộ nhớ** do lặp thông tin (một sinh viên xuất hiện 10 lần trong 10 môn học).
   * **Mâu thuẫn dữ liệu**: cùng một người nhưng có hai địa chỉ khác nhau trong hai dòng.
   * **Lỗi cập nhật**: sửa một nơi quên sửa chỗ khác → dữ liệu sai lệch.

2. **Giải pháp:**

   * **Chuẩn hóa dữ liệu (Normalization):** Tách bảng theo nguyên tắc 1NF, 2NF, 3NF để loại bỏ phụ thuộc bộ phận và phụ thuộc bắc cầu.
   * **Tạo quan hệ hợp lý** giữa các bảng để dữ liệu không lặp mà vẫn truy vấn dễ dàng.

   *Ví dụ:*
   Ban đầu:

   ```
   DonHang(ma_dh, ten_khach, dia_chi_khach, san_pham, so_luong)
   ```

   Sau chuẩn hóa:

   ```
   KhachHang(ma_kh, ten_khach, dia_chi_khach)
   DonHang(ma_dh, ma_kh, ngay_dh)
   ChiTietDH(ma_dh, ma_sp, so_luong)
   ```

   => Thông tin khách hàng chỉ lưu một lần, không lặp lại.

#### 3. Cải thiện **hiệu năng truy vấn và khả năng mở rộng**

1. **Tối ưu hiệu năng truy xuất dữ liệu**

   * Một thiết kế tốt giúp truy vấn nhanh mà không phải viết SQL quá phức tạp.
   * Biết **khi nào nên chuẩn hóa**, và **khi nào nên phi chuẩn hóa** để tối ưu truy vấn thường xuyên (ví dụ: báo cáo tổng hợp doanh thu).

2. **Cấu trúc mở rộng dễ dàng**

   * Khi nghiệp vụ thay đổi (thêm thuộc tính mới, thêm bảng mới, thêm loại người dùng…), CSDL có thể mở rộng mà không phải sửa lại toàn bộ lược đồ.
   * Ví dụ: thêm `GiangVien` vào hệ thống `SinhVien` có thể dùng mô hình kế thừa (ISA), không cần viết lại toàn bộ.

3. **Khả năng chịu tải (Scalability)**

   * Một thiết kế chặt chẽ giúp dễ **phân mảnh dữ liệu (sharding)**, **sao lưu (replication)**, và **chia module** cho nhiều hệ thống.


#### 4. Đảm bảo **bảo mật, khả năng kiểm soát và bảo trì lâu dài**

1. **Phân quyền hợp lý**

   * Tách người quản trị, người nhập liệu, và người chỉ đọc dữ liệu.
   * Giảm rủi ro thao tác nhầm hoặc rò rỉ dữ liệu nhạy cảm.

2. **Dễ kiểm soát thay đổi (auditing)**

   * Có thể ghi nhận lịch sử thay đổi: ai cập nhật, khi nào, thay đổi giá trị gì.
   * Cấu trúc có sẵn `created_at`, `updated_at`, `created_by`, `updated_by` để dễ tracking.

3. **Khả năng bảo trì và phục hồi dữ liệu**

   * Thiết kế tốt giúp **backup/restore** đơn giản, tránh mất mát khi hệ thống gặp sự cố.
   * Lược đồ có tài liệu rõ ràng giúp người khác dễ tiếp quản, phát triển tiếp.


### II. Quy trình 6 bước thiết kế cơ sở dữ liệu

1. **Thu thập yêu cầu**: phỏng vấn, use-cases, quy tắc nghiệp vụ.
2. **Thiết kế khái niệm** (conceptual): mô hình **E-R** (thực thể, thuộc tính, quan hệ, bội số 1-1/1-N/N-N, ràng buộc).
3. **Thiết kế logic** (logical): ánh xạ E-R → **mô hình quan hệ** (bảng/khóa/khóa ngoại).
4. **Chuẩn hoá**: tinh chỉnh lược đồ về **1NF→2NF→3NF** (và cân nhắc BCNF nếu cần).
5. **Thiết kế vật lý**: chọn kiểu dữ liệu, chỉ mục (index), phân mảnh/partition, chiến lược lưu trữ.
6. **Ràng buộc & bảo trì**: khóa, unique, foreign key, check, trigger; backup/restore; theo dõi hiệu năng và (có chủ đích) **phi chuẩn hóa** một số chỗ nếu thực sự cần cho hiệu năng.

### III. Nguyên tắc vàng

* **Mô hình hóa theo quy tắc nghiệp vụ**, không theo màn hình UI.
* **Đặt tên nhất quán** (snake_case hay camelCase, nhưng thống nhất).
* **Khóa chính rõ ràng** (tự nhiên hay thay thế/surrogate như `BIGINT IDENTITY`).
* **Mọi quan hệ 1-N phải có FK ở phía N**; quan hệ N-N dùng bảng nối.
* **Ràng buộc > niềm tin**: dùng `PRIMARY KEY`, `UNIQUE`, `FOREIGN KEY`, `CHECK`, không trông chờ ứng dụng tự giữ kỷ luật.

---

## B. Lược đồ E-R (Entity–Relationship)

### I. Khái niệm cốt lõi

* **Thực thể (Entity)**: thứ “có danh tính” trong thế giới nghiệp vụ (VD: `SinhVien`, `MonHoc`).
* **Thuộc tính (Attribute)**: đặc trưng của thực thể (VD: `ho_ten`, `email`).

  * Đơn trị (atomic), đa trị (multi-valued), phức hợp (composite), dẫn xuất (derived).
* **Khóa (Key)**: thuộc tính/cụm thuộc tính xác định duy nhất thực thể (candidate key); chọn một làm **primary key**.
* **Quan hệ (Relationship)**: liên kết giữa thực thể (VD: `DangKy` giữa `SinhVien` và `MonHoc`).

  * **Bội số/cardinarlity**: 1–1, 1–N, N–N.
  * **Mức tham gia**: toàn phần (bắt buộc) hay bộ phận (tùy chọn).
* **Thực thể yếu (Weak entity)**: phụ thuộc danh tính vào “chủ” (owner).
* **Kế thừa/ISA**: thực thể con thừa hưởng thuộc tính của thực thể cha (VD: `NhanVien` ISA `Nguoi`).

### II. Ví dụ E-R (đăng ký học phần)

* Thực thể:

  * `SinhVien(ma_sv, ho_ten, email, nganh)`
  * `MonHoc(ma_mh, ten_mh, so_tc)`
* Quan hệ:

  * `DangKy` giữa `SinhVien` và `MonHoc` là **N–N** (mỗi SV đăng ký nhiều môn, mỗi môn có nhiều SV).
  * Thuộc tính quan hệ `DangKy`: `hoc_ky`, `nam_hoc`, `diem_qua_trinh`, `diem_cuoi_ky`, `diem_tong_ket`.

**Gợi ý ràng buộc nghiệp vụ**

* Mỗi `DangKy` thuộc **một** `hoc_ky` và `nam_hoc`.
* `diem_tong_ket` có thể **dẫn xuất** từ Q.T + C.K theo công thức.

### III. Khi nào tách/ghép trong E-R?

* Quan hệ **1–1**: cân nhắc gộp thành một thực thể nếu cùng vòng đời.
* Thuộc tính **đa trị**: tách thành thực thể/bảng riêng.
* Thuộc tính **phức hợp** (VD: địa chỉ): có thể giữ “phẳng” ở mô hình quan hệ.

---

## C. Mô hình dữ liệu quan hệ (Relational Model)

### I. Thuật ngữ nhanh

* **Quan hệ (Relation)** ~ **Bảng**; **Bộ (Tuple)** ~ **Hàng**; **Thuộc tính (Attribute)** ~ **Cột**; **Miền (Domain)** ~ kiểu dữ liệu hợp lệ.
* **Ràng buộc toàn vẹn**:

  * Miền (Domain): giá trị đúng kiểu/phạm vi.
  * Khóa (Key): duy nhất/không rỗng cho PK.
  * Đối tượng (Entity integrity): PK không NULL.
  * Tham chiếu (Referential integrity): FK phải tham chiếu giá trị tồn tại.
* **Đại số quan hệ** (ý niệm): chọn σ, chiếu π, kết ⋈, hợp ∪, trừ −, tích ×. SQL hiện thực hóa các phép này.

### II. Ánh xạ E-R → Quan hệ (quy tắc chuẩn)

1. **Thực thể thường** → 1 bảng: cột = thuộc tính, PK = khóa đã chọn.
2. **Thực thể yếu** → bảng chứa PK = (PK chủ + partial key).
3. **Quan hệ 1–N** → thêm **FK** ở phía N (có thể `UNIQUE` ở 1–1).
4. **Quan hệ N–N** → **bảng nối** (junction): PK thường là (FK1, FK2, …) + thuộc tính riêng của quan hệ.
5. **Thuộc tính đa trị** → bảng riêng (FK về chủ).
6. **ISA/kế thừa** → ba chiến lược (chọn 1):

   * Một bảng (single-table + `type`),
   * Mỗi lớp một bảng (class-table),
   * Bảng dùng chung PK (shared-PK subclassing).

### III. Từ ví dụ E-R sang SQL (DDL minh họa)

```sql
-- 1) Thực thể
CREATE TABLE SinhVien (
  ma_sv      VARCHAR(15)  PRIMARY KEY,
  ho_ten     NVARCHAR(100) NOT NULL,
  email      VARCHAR(100)  UNIQUE NOT NULL,
  nganh      NVARCHAR(100) NOT NULL
);

CREATE TABLE MonHoc (
  ma_mh      VARCHAR(15)   PRIMARY KEY,
  ten_mh     NVARCHAR(200) NOT NULL,
  so_tc      TINYINT       NOT NULL CHECK (so_tc BETWEEN 1 AND 6)
);

-- 2) Quan hệ N–N: bảng nối
CREATE TABLE DangKy (
  ma_sv          VARCHAR(15)  NOT NULL,
  ma_mh          VARCHAR(15)  NOT NULL,
  hoc_ky         TINYINT      NOT NULL CHECK (hoc_ky BETWEEN 1 AND 3),
  nam_hoc        SMALLINT     NOT NULL CHECK (nam_hoc BETWEEN 2000 AND 2100),
  diem_qua_trinh DECIMAL(4,2) NULL CHECK (diem_qua_trinh BETWEEN 0 AND 10),
  diem_cuoi_ky   DECIMAL(4,2) NULL CHECK (diem_cuoi_ky   BETWEEN 0 AND 10),
  diem_tong_ket  DECIMAL(4,2) NULL CHECK (diem_tong_ket  BETWEEN 0 AND 10),
  CONSTRAINT PK_DangKy PRIMARY KEY (ma_sv, ma_mh, hoc_ky, nam_hoc),
  CONSTRAINT FK_DangKy_SV FOREIGN KEY (ma_sv) REFERENCES SinhVien(ma_sv),
  CONSTRAINT FK_DangKy_MH FOREIGN KEY (ma_mh) REFERENCES MonHoc(ma_mh)
);

-- 3) Chỉ mục gợi ý
CREATE INDEX IX_DangKy_mh ON DangKy(ma_mh);
CREATE INDEX IX_DangKy_sem ON DangKy(nam_hoc, hoc_ky);
```

---

## D. Chuẩn hoá dữ liệu: 1NF, 2NF, 3NF

### I. Vì sao cần chuẩn hoá?

Để tránh **anomalies**:

* **Cập nhật**: sửa một giá trị nhưng bỏ sót bản sao ở nơi khác.
* **Chèn**: muốn thêm dữ liệu A nhưng bị “kẹt” vì buộc kèm dữ liệu B chưa có.
* **Xoá**: xoá một dòng lại vô tình mất thông tin quan trọng khác.

Chuẩn hoá dùng **phụ thuộc hàm (Functional Dependency – FD)** và **khóa** để phân rã bảng thành các bảng “tốt” hơn, vẫn **bảo toàn thông tin** và **không mất mát** khi kết lại.

> Khái niệm nhanh:
>
> * **FD X → Y**: giá trị X quyết định duy nhất giá trị Y.
> * **Khóa ứng viên (candidate key)**: tập thuộc tính tối thiểu xác định duy nhất một bộ.
> * **Thuộc tính prime**: thuộc tính nằm trong **ít nhất một** khóa ứng viên.

### II. 1NF (First Normal Form)

**Định nghĩa:** Mọi thuộc tính là **đơn trị/atomic**, không có nhóm lặp hay mảng trong một ô.

**Ví dụ kém 1NF**

```
DonHang(ma_dh, ngay, khach_hang, danh_sach_mat_hang, tong_tien)
-- danh_sach_mat_hang = "SP1x2;SP5x1;SP9x3"
```

**Sửa về 1NF**: tách **chi tiết đơn hàng** thành bảng dòng:

```
DonHang(ma_dh, ngay, khach_hang, tong_tien)
ChiTietDH(ma_dh, ma_sp, so_luong, don_gia)
```

### III. 2NF (Second Normal Form)

**Định nghĩa:** Ở **1NF** và **không có phụ thuộc bộ phận** từ thuộc tính không-prime vào **một phần** của **khóa ghép**.

Áp dụng khi **khóa chính là tổ hợp**.

**Ví dụ (đăng ký học phần, chưa chuẩn)**

```
DK(ma_sv, ma_mh, hoc_ky, nam_hoc, ho_ten_sv, ten_mh, so_tc)
-- Giả sử PK = (ma_sv, ma_mh, hoc_ky, nam_hoc)
```

* `ho_ten_sv` **phụ thuộc** vào `ma_sv` (một phần của PK) ⇒ **vi phạm 2NF**.
* `ten_mh`, `so_tc` phụ thuộc vào `ma_mh` ⇒ vi phạm.

**Chuẩn về 2NF**

```
SinhVien(ma_sv, ho_ten_sv, ... )
MonHoc(ma_mh, ten_mh, so_tc, ... )
DangKy(ma_sv, ma_mh, hoc_ky, nam_hoc, ...)
```

### IV. 3NF (Third Normal Form)

**Định nghĩa thực dụng:** Ở **2NF** và **không có phụ thuộc bắc cầu** (transitive) từ khóa → thuộc tính không-prime → thuộc tính không-prime khác.
(Định nghĩa đầy đủ: với mọi FD X→A, hoặc A ∈ X, hoặc X là **siêu khóa**, hoặc A là **thuộc tính prime**.)

**Ví dụ vi phạm 3NF**

```
DonHang(ma_dh, ma_kh, ten_kh, dia_chi_kh)
-- Khóa: ma_dh
-- ma_dh → ma_kh và ma_kh → (ten_kh, dia_chi_kh)  ⇒ bắc cầu
```

**Chuẩn 3NF**

```
KhachHang(ma_kh, ten_kh, dia_chi_kh)
DonHang(ma_dh, ma_kh, ngay, ...)
```

### V. Chuẩn hoá từng bước với ví dụ “Đăng ký học phần”

**Bảng ban đầu (phi chuẩn, gom hết):**

```
AllInOne(
  ma_sv, ho_ten, email, nganh,
  ma_mh, ten_mh, so_tc,
  hoc_ky, nam_hoc,
  diem_qua_trinh, diem_cuoi_ky, diem_tong_ket
)
-- Dòng lặp lại thông tin SV và Môn cho mỗi đăng ký
```

**1NF**: không nhóm lặp ⇒ bảng vẫn 1NF vì các cột là atomic.
**2NF**: phát hiện thuộc tính sinh viên phụ thuộc bộ phận vào `ma_sv`, thuộc tính môn phụ thuộc vào `ma_mh` ⇒ tách:

```
SinhVien(ma_sv PK, ho_ten, email UNIQUE, nganh)
MonHoc(ma_mh PK, ten_mh, so_tc)
DangKy(ma_sv FK→SinhVien, ma_mh FK→MonHoc, hoc_ky, nam_hoc,
       diem_qua_trinh, diem_cuoi_ky, diem_tong_ket,
       PK(ma_sv, ma_mh, hoc_ky, nam_hoc))
```

**3NF**: nếu `diem_tong_ket` = 0.4*Q.T + 0.6*C.K thì là thuộc tính **dẫn xuất**. Bạn có thể **không lưu** (tính khi truy vấn) hoặc lưu + `CHECK`/trigger để đồng bộ. Không có bắc cầu không-prime ⇒ đạt 3NF.

**Một số truy vấn mẫu**

```sql
-- Điểm tổng kết tính động
SELECT dk.ma_sv, sv.ho_ten, dk.ma_mh, mh.ten_mh,
       ROUND(0.4*dk.diem_qua_trinh + 0.6*dk.diem_cuoi_ky, 2) AS diem_tong_ket
FROM DangKy dk
JOIN SinhVien sv ON sv.ma_sv = dk.ma_sv
JOIN MonHoc  mh ON mh.ma_mh = dk.ma_mh
WHERE dk.nam_hoc = 2025 AND dk.hoc_ky = 1;

-- Thống kê số tín chỉ SV đã đăng ký trong một học kỳ
SELECT dk.ma_sv, sv.ho_ten, SUM(mh.so_tc) AS tong_tin_chi
FROM DangKy dk
JOIN MonHoc mh ON mh.ma_mh = dk.ma_mh
JOIN SinhVien sv ON sv.ma_sv = dk.ma_sv
WHERE dk.nam_hoc = 2025 AND dk.hoc_ky = 1
GROUP BY dk.ma_sv, sv.ho_ten;
```

### VI. Lưu ý

* **Chuẩn hoá là mặc định**; **phi chuẩn hoá** (gộp bảng, lưu cột dẫn xuất) chỉ khi có chứng cứ nghẽn hiệu năng và **biết rõ** cách giữ toàn vẹn.
* **Chỉ mục (index)** trên FK/cột tìm kiếm giúp JOIN nhanh; nhưng đừng lạm dụng.
* **NULL**: thiết kế cẩn thận (có thể dùng bảng riêng thay vì nhiều cột nullable).
* **CHECK**/trigger để cưỡng chế quy tắc nghiệp vụ (VD: thang điểm 0–10).
* **Khóa tự nhiên vs surrogate**: tự nhiên rõ nghĩa (VD: `ma_sv`) nhưng có thể thay đổi; surrogate (số tự tăng/UUID) ổn định hơn cho FK dài hạn.

