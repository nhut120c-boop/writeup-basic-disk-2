# writeup: basic disk 2

công cụ dùng: ftk imager, registry explorer, db browser for sqlite

## đề bài

bài cho file ảnh đĩa `.ad1`, cần tìm:

1. build number của windows trên máy nạn nhân

2. nội dung tài liệu bí mật bị khóa mà người bạn gửi cho user

format flag: `Win[version]-[build]_{nội_dung_tài_liệu}`

## câu 1: tìm build number

em biết build number nằm trong registry hive `SOFTWARE` vì windows lưu toàn bộ thông tin phần mềm và hệ điều hành vào đó, cụ thể là key `Microsoft\Windows NT\CurrentVersion`, 

b1: mở file ảnh đĩa trong ftk imager, vào:

```
[root]\Windows\System32\config\
```

b2: chuột phải vào file `SOFTWARE` → export file → lưu ra desktop

<img width="1410" height="701" alt="image" src="https://github.com/user-attachments/assets/4d2690aa-fc00-4e83-9127-08de32141348" />


b3: mở registry explorer, load file `SOFTWARE` vừa export vào


b4: tìm tới CurrentVersion trong:

```
ROOT\Microsoft\Windows NT\CurrentVersion
```

<img width="1547" height="875" alt="image" src="https://github.com/user-attachments/assets/2bc826d5-9b9d-40b2-b8bd-f01c6d94e1e0" />

b5: nhìn khung bên phải, tìm 2 dòng:

<img width="695" height="370" alt="image" src="https://github.com/user-attachments/assets/33bf5e22-c36f-43cf-804c-4f2a72ab0fd2" />


kết quả câu 1: `Win10-19043`

## câu 2: tìm tài liệu bí mật và mật khẩu

### tìm file tài liệu

quay lại ftk imager, bới các thư mục của user k137:

```
[root]\Users\k137\Downloads\
[root]\Users\k137\Desktop\
[root]\Users\k137\Documents\
```

tìm file nén hoặc tài liệu khả nghi → export ra ngoài

ảnh: [chèn ảnh thư mục downloads]

### tìm mật khẩu

em đoán mật khẩu lưu trong sticky notes vì đây là chỗ người dùng hay copy mật khẩu vào để nhớ tạm, và trong forensics windows thì sticky notes là artifact hay bị khai thác, nó lưu dữ liệu dưới dạng sqlite tại:

```
[root]\Users\k137\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\
```

b1: export file `plum.sqlite` ra ngoài

b2: mở bằng db browser for sqlite, vào tab browse data, chọn bảng `Note`, đọc nội dung

ảnh: [chèn ảnh db browser]

b3: lấy mật khẩu từ note, dùng để mở file tài liệu đã export

ảnh: [chèn ảnh nội dung tài liệu]

kết quả câu 2: `{nội_dung_tài_liệu}`

## flag

```
Win10-19043_{nội_dung_tài_liệu}
```
