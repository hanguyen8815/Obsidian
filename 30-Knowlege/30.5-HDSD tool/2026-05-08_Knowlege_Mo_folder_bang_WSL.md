# Knowledge Note - Mở thư mục bằng WSL trên VS Code

## Câu hỏi cần trả lời

- Mở một thư mục bằng WSL trên VS Code như thế nào?
- Làm sao biết VS Code đang mở đúng ở chế độ WSL?

## Tóm tắt ngắn

- Nếu mở đúng bằng WSL, góc trái dưới của VS Code sẽ hiện `WSL: <ten distro>`.
- Nếu đang ở trong VS Code thường, bấm `F1` và dùng lệnh `WSL: Connect to WSL`.

## Điều đã xác nhận

- Fact: WSL ánh xạ ổ đĩa Windows theo dạng `/mnt/<ten_o>/`.
- Fact: Thư mục vault hiện tại có thể đi tới bằng đường dẫn WSL:
  `/mnt/d/OneDrive - CONG TY CO PHAN TAP DOAN IVI/Ha.NT/3. Documents/0. Obsidian`
- Fact: Khi kết nối đúng, VS Code sẽ hiển thị trạng thái WSL ở góc trái dưới.

## Giả định

- Assumption: Máy đang dùng Windows, đã cài WSL và đã cài VS Code.
- Assumption: VS Code đã có tiện ích mở rộng WSL của Microsoft.

## Cách áp dụng

- Bước 1: Mở VS Code.
- Bước 2: Bấm `F1`.
- Bước 3: Gõ `WSL: Connect to WSL` hoặc `WSL: Connect to WSL using Distro`.
- Bước 4: Sau khi vào cửa sổ WSL, chọn `File` -> `Open Folder`.
- Bước 5: Chọn thư mục theo đường dẫn Linux, ví dụ:

```text
/mnt/d/OneDrive - CONG TY CO PHAN TAP DOAN IVI/Ha.NT/3. Documents/0. Obsidian
```

- Cách đổi nhanh từ đường dẫn Windows sang WSL:

```text
D:\Thu muc\Con -> /mnt/d/Thu muc/Con
```

- Ví dụ với vault hiện tại:

```text
D:\OneDrive - CONG TY CO PHAN TAP DOAN IVI\Ha.NT\3. Documents\0. Obsidian
-> /mnt/d/OneDrive - CONG TY CO PHAN TAP DOAN IVI/Ha.NT/3. Documents/0. Obsidian
```

## Ví dụ hoặc tình huống dùng

- Muốn kết nối nhanh WSL ngay trong VS Code mà không cần mở terminal trước:

```text
F1 -> WSL: Connect to WSL -> File -> Open Folder
```

- Muốn mở đúng vault hiện tại sau khi đã vào cửa sổ WSL:

```text
/mnt/d/OneDrive - CONG TY CO PHAN TAP DOAN IVI/Ha.NT/3. Documents/0. Obsidian
```

## Phím tắt dùng nhanh

- Trong VS Code: `F1` -> gõ `WSL: Connect to WSL`.

## Rủi ro hoặc giới hạn

- Nếu đường dẫn có khoảng trắng, cần đặt cả đường dẫn trong dấu `"`.
- Nếu mở VS Code nhưng không thấy nhãn `WSL:` ở góc trái dưới, nhiều khả năng đang mở ở chế độ Windows thường.
- Nếu chưa cài tiện ích mở rộng WSL, lệnh kết nối WSL trong VS Code có thể không xuất hiện.

## Nguồn tham chiếu

- [[Vault Workflow]]

## Note liên quan

- [[00-General/Vault Workflow]]
