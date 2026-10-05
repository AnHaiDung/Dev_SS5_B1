# Báo cáo Bài 1: Khôi phục Commit đã mất bằng Git Reflog

## 1. Giới thiệu
Tài liệu này báo cáo quy trình truy vết và khôi phục thành công commit bị mất khỏi lịch sử làm việc cục bộ (local repository) sau khi vô tình thực hiện lệnh `git reset --hard HEAD~1`.

---

## 2. Quy trình thực hiện

### Bước 1: Khởi tạo môi trường và giả lập sự cố

1. **Khởi tạo file và commit tính năng:**
   ```bash
   echo "Day la tinh nang quan trong" > feature.txt
   git add .
   git commit -m "Them tinh nang quan trong"
   ```

2. **Giả lập sự cố lùi commit (Reset Hard):**
   ```bash
   git reset --hard HEAD~1
   ```

3. **Xác nhận commit bị mất:**
   ```bash
   git log --oneline
   ```
   *Kết quả:* Commit `"Them tinh nang quan trong"` không còn xuất hiện trong lịch sử `git log`.

---

### Bước 2: Tra cứu nhật ký Reflog

Thực hiện kiểm tra lịch sử thao tác của con trỏ `HEAD` bằng lệnh:
```bash
git reflog
```

*Kết quả tham chiếu:*
```text
a1b2c3d (HEAD -> main) HEAD@{0}: reset: moving to HEAD~1
e3a5b2c HEAD@{1}: commit: Them tinh nang quan trong
8f9e0d1 HEAD@{2}: commit: Initial commit
```

> **Ghi chú:** Mã hash `e3a5b2c` tại con trỏ `HEAD@{1}` chính là trạng thái chứa commit đã bị xóa.

---

### Bước 3: Khôi phục Commit về nhánh hiện tại

Sử dụng mã hash tìm được trong `reflog` để khôi phục lại trạng thái:
```bash
git reset --hard e3a5b2c
```
*(Lưu ý: Thay `e3a5b2c` bằng mã hash tương ứng thu được trên terminal thực tế).*

---

## 3. Kiểm tra kết quả

1. **Kiểm tra lại lịch sử commit (`git log`):**
   ```bash
   git log --oneline
   ```
   *Kết quả:* Commit `"Them tinh nang quan trong"` đã xuất hiện trở lại ở vị trí gần nhất (`HEAD`).

2. **Kiểm tra nội dung file:**
   ```bash
   cat feature.txt
   ```
   *Kết quả:* Nội dung `Day la tinh nang quan trong` đã được khôi phục đầy đủ.

---

## 4. Tổng kết bài học
- `git log` chỉ hiển thị các commit thuộc về nhánh hiện tại.
- `git reflog` đóng vai trò nhật ký theo dõi mọi chuyển động của `HEAD`. Khi lỡ tay thực hiện `git reset --hard`, `git reflog` giúp tìm lại mã SHA-1 của commit cũ để đưa dự án về trạng thái an toàn.