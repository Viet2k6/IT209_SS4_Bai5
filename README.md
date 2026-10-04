# Bài 5: Khôi phục trạng thái và Đảo ngược commit (Reset vs Revert)

## 1. Mục tiêu

Thực hành hai cơ chế `git reset` và `git revert`, đồng thời phân biệt cách sử dụng khi làm việc cá nhân và khi làm việc trên nhánh chung.

## 2. Trường hợp 1: Git Reset

Ban đầu tạo các commit:

```bash
git commit -m "Initial commit"
git commit -m "Add application code"
git commit -m "Add buggy code"
```

Sau đó sử dụng:

```bash
git reset --mixed HEAD~1
```

Kết quả:

* `HEAD` lùi lại 1 commit.
* Commit `Add buggy code` bị loại khỏi lịch sử hiện tại.
* File `app.js` vẫn được giữ lại trong thư mục làm việc.
* `app.js` chuyển sang trạng thái `Modified`.
* Không sử dụng `git reset --hard`, nên mã nguồn hiện hành không bị xóa.

Kiểm tra:

```bash
git status
git log --oneline -n 5
```

## 3. Trường hợp 2: Git Revert

Tạo lại commit chứa mã nguồn lỗi:

```bash
git add app.js
git commit -m "Add buggy code again"
```

Sau đó đảo ngược commit bằng:

```bash
git revert HEAD
```

Git tạo một commit mới:

```text
Revert "Add buggy code again"
```

Kiểm tra lịch sử:

```bash
git log --oneline -n 5
```

Kết quả:

```text
8c71171 Revert "Add buggy code again"
715e251 Add buggy code again
346324c Add application code
bccc5cb Initial commit
```

Điều này cho thấy commit cũ vẫn được giữ trong lịch sử và một commit mới được tạo để đảo ngược thay đổi.

## 4. So sánh Reset và Revert

### Git Reset

`reset` di chuyển con trỏ `HEAD` về một commit trước đó và có thể làm thay đổi lịch sử Git.

Phù hợp khi làm việc cá nhân hoặc với các commit chưa được chia sẻ lên remote.

Trong bài này sử dụng `--mixed` để giữ lại thay đổi trong Working Directory dưới dạng `Modified`.

### Git Revert

`revert` không xóa commit cũ mà tạo thêm một commit mới để đảo ngược thay đổi.

Phù hợp khi làm việc nhóm hoặc trên nhánh chung vì không viết lại lịch sử đã được chia sẻ.

## 5. Kết quả

* Thực hiện thành công `git reset --mixed HEAD~1`.
* Giữ lại mã nguồn sau khi reset.
* Thực hiện thành công `git revert`.
* Tạo được commit mới bắt đầu bằng `Revert`.
* Working Tree cuối cùng ở trạng thái sạch.
