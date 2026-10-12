# Note

Ứng dụng ghi chú và kho tệp chạy ngay trên máy Windows của bạn. Dữ liệu nằm trong một file SQLite và một thư mục — không cần tài khoản, không cần internet. Tải một file `.exe` về là dùng được.

## Tính năng

- **Ghi chú markdown**: soạn / xem trước, mục lục tự sinh từ `# ## ###`, ghim, thẻ, tự động lưu, thùng rác giữ 30 ngày.
- **Tìm kiếm toàn văn** trên ghi chú và tệp, không phân biệt dấu tiếng Việt (gõ `duong` ra "đường").
- **Kho tệp**: thư mục lồng nhau, kéo-thả để tải lên và sắp xếp, ghi chú riêng cho từng tệp.
- **Import / Export**: nhập `txt` `md` `html`, xuất `md` `txt` `html`.
- **Đồng bộ ghi chú giữa hai máy** trong cùng mạng (giữ bản mới hơn, lệnh xóa cũng được đồng bộ).
- **MCP server** (`/mcp`): cho AI liệt kê, đọc, tạo, sửa ghi chú và quản lý tệp.
- **Giao diện** tiếng Việt / English, sáng / tối.
- **Mang đi được**: dữ liệu nằm cạnh file exe, chép sang USB hay ổ khác là xong.

## Cách dùng

1. Tải `Note-0.2.0-portable.exe`, đặt vào một thư mục bất kỳ (ví dụ `E:\Note`).
2. Bấm đúp để chạy. Dữ liệu được tạo trong thư mục `data` ngay cạnh file exe.
3. Muốn chuyển sang máy / ổ khác: đóng app, chép cả thư mục (exe + `data`).

Lần đầu Windows có thể hiện cảnh báo SmartScreen vì file chưa được ký số: chọn *More info → Run anyway*. App chỉ mở một cửa sổ; chạy lần hai sẽ đưa cửa sổ đang mở lên.

Mỗi lần mở, app tự sao lưu `note.db` vào `data\backups` (giữ 10 bản mới nhất).

## Phím tắt

| Phím | Việc |
|---|---|
| `Ctrl+N` | Ghi chú mới |
| `Ctrl+O` | Import file |
| `Ctrl+S` | Lưu ngay |
| `Ctrl+E` | Chuyển Soạn / Xem trước |
| `Ctrl+B` / `Ctrl+I` | Đậm / Nghiêng |
| `Alt+Z` | Bật / tắt xuống dòng |
| `/` | Tìm kiếm |

## Dùng qua mạng LAN và MCP

Mặc định app chỉ nhận kết nối từ chính máy bạn. Để máy khác trong mạng truy cập, đặt biến môi trường trước khi chạy (PowerShell):

```powershell
$env:NOTE_HOST = '0.0.0.0'
$env:NOTE_TOKEN = 'mã-bí-mật'
.\Note-0.2.0-portable.exe
```

Máy khác mở `http://<IP-máy>:4571`. **Hãy đặt `NOTE_TOKEN` khi mở ra mạng**, nếu không ai trong mạng cũng đọc và ghi được. Có token thì web hỏi mã một lần; MCP và script gửi `Authorization: Bearer <mã>`.

MCP endpoint: `http://<IP>:4571/mcp` (Streamable HTTP). Tools: `list_notes`, `get_note`, `create_note`, `update_note`, `trash_note`, `restore_note`, `list_folders`, `create_folder`, `list_files`, `get_file`, `set_file_note`, `move_file`, `search`, `sync_notes`.

## Đồng bộ hai máy

Mở **Mạng**, nhập địa chỉ máy kia (`http://<IP>:4571`), bấm **Đồng bộ**. Nên đặt cùng `NOTE_TOKEN` và dùng cùng phiên bản trên hai máy. Chỉ đồng bộ ghi chú, không đồng bộ tệp.

## Biến môi trường

Tất cả đều không bắt buộc:

| Biến | Mặc định | Ý nghĩa |
|---|---|---|
| `NOTE_PORT` | `4571` (tự đổi nếu bị chiếm) | Cổng lắng nghe |
| `NOTE_HOST` | `127.0.0.1` | `0.0.0.0` để mở ra mạng LAN |
| `NOTE_TOKEN` | (trống) | Mã truy cập web / API / MCP |
| `NOTE_DATA_DIR` | thư mục `data` cạnh exe | Nơi chứa `note.db`, `files`, `backups` |
| `NOTE_PEER` | (trống) | Địa chỉ máy kia cho tool MCP `sync_notes` |
| `NOTE_ALLOWED_HOSTS` | (trống) | Tên host được phép truy cập khi không có token, ngăn cách bằng dấu phẩy |

## Yêu cầu

Windows 10 / 11 64-bit.
