---
loai_note: template
nhom_mau: jira-bug-log
du_an: sakura
ngay_tao: {{date:YYYY-MM-DD}}
tags:
  - template
  - jira
  - bug-log
  - sakura
---

# JIRA Bug Log - SAKURA

## Mục tiêu

- Dùng để ghi nhanh lỗi và chuyển thành ticket JIRA theo đúng văn phong team SAKURA.
- Giúp thông tin đủ rõ để Dev, QA, BA và triển khai cùng hiểu và xử lý.

## Summary

```text
FE (Mã cơ sở) [Phân hệ/Màn hình] Mô tả lỗi ngắn gọn
BE (Mã cơ sở) [Phân hệ/Màn hình] Mô tả lỗi ngắn gọn
APP (Mã cơ sở) [Phân hệ/Màn hình] Mô tả lỗi ngắn gọn
INT (Mã cơ sở) [Phân hệ/Màn hình] Mô tả lỗi ngắn gọn
```

## Description

```markdown
FE/BE/APP/INT (Mã cơ sở) [Phân hệ/Màn hình] Mô tả lỗi ngắn gọn

**Hiện tại:**
-

**Link server:**
-

**Step:**
#
#
#

**Mong muốn:**
-

**Dữ liệu kiểm tra:**
- Mã Người bệnh:
- Mã hồ sơ:
- Mã phiếu thu / mã chỉ định / mã đợt điều trị:
- Tài khoản test:
- Vai trò / Mã quyền:
- Cấu hình / tham số liên quan:

**API:**
-

**Message lỗi / phản hồi BE:**
-

**Lý do thực hiện:**
-

**Spec:**
- Ticket cũ:
- Link confluence:
```

## Mẫu điền nhanh

- Loại: FE/BE/APP/INT
- Cơ sở:
- Phân hệ/Màn hình:
- Tên lỗi:

### Hiện tại

-

### Link server

-

### Step

1.
2.
3.

### Mong muốn

-

### Dữ liệu kiểm tra

- Mã Người bệnh:
- Mã hồ sơ:
- Mã phiếu/chỉ định:
- Tài khoản:
- Mã quyền:
- Cấu hình liên quan:

### API

-

### Message lỗi

-

### Lý do thực hiện

-

### Spec hoặc tài liệu liên quan

- Ticket cũ:
- Link confluence:

## Lưu ý khi dùng

- Thêm `Mã quyền` khi lỗi liên quan phân quyền.
- Thêm `API` khi cần đối chiếu giữa FE và BE.
- Thêm `Lý do thực hiện` khi đây là nhu cầu đặc thù của Cơ sở y tế.
- Thêm `Spec` hoặc `ticket cũ` khi Dev cần đối chiếu logic cũ.
- Không ghi dữ liệu nhạy cảm thật của Người bệnh nếu note cần chia sẻ rộng.

## Note liên quan

- [[00-General/Vault Workflow]]

