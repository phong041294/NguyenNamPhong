#Linux and Unix dd command
##Giới thiệu về lệnh dd
Lệnh dd dùng để sao lưu và phục hồi dữ liệu,chuyển đổi định dạng của dữ liệu,tạo file với kích thước cố định,ngoài ra nó còn dùng để đo tốc độ đọc ghi của ổ cứng

**Cú pháp**

    dd OPTION 
    
    dd [Toán tử]...
    
Toán tử

| Toán tử |Ý nghĩa |
| ------------- |-----:|
| bs=BYTES | Quá trình đọc (ghi) bao nhiêu byte một lần đọc (ghi |
| cbs=BYTES |  Chuyển đổi bao nhiêu byte một lần |
| count=Blocks | Thực hiện bao nhiêu Block trong quá trình thực thi câu lệnh |
| if | Chỉ đường dẫn đọc đầu vào |
| of | Chỉ đường dẫn ghi đầu ra |
| ibs=bytes | Chỉ ra số byte một lần đọc |
| obs=bytes | Chỉ ra số byte một lần ghi |
| skip=blocks | Bỏ qua bao nhiêu block đầu vào |
| conv=Convs | Chỉ ra tác vụ cụ thể của câu lệnh, các tùy chọn được ghi dưới bảng sau đây |

**Chi tiết Conv**

| Tác vụ | Ý nghĩa |
| ------------- |-----:|
| ascii | Chuyển đôi từ mã EBCDIC sáng ASCII |
| ebcdic | Chuyển đổi từ mã ASCII sang EBCDIC |
| lcase | Chuyển đổi từ chữ thường lên hết thành chữ in hoa |
| ucase | Chuyển đổi từ chữ in hoa sang chữ thường |
| nocreat | Không tạo ra file đầu ra |
| noerror | Tiếp tục sao chép dữ liệu khi đầu vào bị lỗi |
| sync | Đồng bộ dữ liệu với ổ đang sao chép sang |

**Options**

| Option | Ý nghĩa |
| ------------- |-----:|
| --help | Hiển thị menu help và thoát |
| --version | Hiển thị thông tin phiên bản và thoát |

**Trường hậu tố**
Block và Bytes có thể thêm một số trường theo sau để khai báo định dạng khác
- c = 1 byte
- w = 2 byte
- b = 512 byte
- kB = 1000 byte
- K = 1024 byte
- MB = 1000000 byte
- M = (1024 * 1024) byte
- GB = (1000 * 1000 * 1000) byte
- G = (1024 * 1024 * 1024) byte

##Ví dụ về lệnh dd
**CHÚ Ý:** Sử dụng lệnh dd một cách thận trọng,sử dụng không đúng cách hoặc nhập sai giá trị có thể gây mất toàn bộ dữ liệu hoặc ghi đè lên ổ cứng của bạn

- Sao lưu toàn bộ dữ liệu ổ cứng sao ổ cứng khác:
 
    ```dd if=/dev/sda of=/dev/sdb conv=noerror,sync```

*Tùy chọn conv=noerror,sync có nghĩa là vẫn tiếp tục sao lưu nếu dữ liệu đầu vào bị lỗi và tự động đồng bộ với dữ liệu sdb

<img src="http://i.imgur.com/Z5QQDOa.png">

- Tạo 1 file img cho ổ SDA1:
 
    ```dd if=/dev/sda1 of=/root/sda1.img```
<img src="http://i.imgur.com/itpQEdW.png">
- Phục hồi dữ liệu
 
        dd if=/root/sda1.img of=/dev/sda1

<img src="http://i.imgur.com/jdIBQYN.png">
- Chuyển đổi chữ thường thành hoa và ngược lại:
 
<img src="http://i.imgur.com/4Dv91QD.png">

*Nếu muốn biến chữ hoa thành chữ thường,thay ucase bằng lcase*

- Tạo 1 file có dung lượng cố định:

<img src="http://i.imgur.com/er6sjjc.png">

- Đo tốc độ đọc/ghi của ổ cứng:

Cú pháp đo tốc độ đọc:

    dd if=/dev/zero of=/nas-mount-point/samplefile bs=1M count=1024 oflag=direct
    
<img src="http://i.imgur.com/ZRkCsip.png">

Cú pháp đo tốc độ ghi:

    dd if=/nas-mount-point/samplefile of=/dev/null bs=1M count=1024 iflag=direct
    
<img src="http://i.imgur.com/32N1ARz.png">

*Trong đó iflag/oflag=direct có nghĩa là sử dụng luồng Input/Output trực tiếp và bỏ qua buffer cache.Ngoài ra bạn có thể thêm thông số **conv=fdatasync**

*để chỉ ra rằng dữ liệu được ghi trên ổ cứng chứ không ghi trên RAM,như vậy kết quả sẽ chính xác hơn.*

##Tốc độ nào để đánh giá ổ cứng có chậm hay không?

*Kinh nghiệm của nhiều người chỉ ra rằng, tốc độ chuẩn là 50MB/s.

Tức là nếu ổ của bạn đạt trên 50MB/s là có thể dùng được, trên 100MB/s là tốt.

Nhưng nếu ổ của bạn đạt dưới 40MB/s thì quá tệ.*
