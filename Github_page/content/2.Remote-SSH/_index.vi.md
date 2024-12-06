---
title : "Kết nối VSCode với AWS EC2 bằng SSH"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---
## 1.1. Giới thiệu kết nối SSH

- SSH (**Secure Shell**) là một giao thức mạng dùng để thiết lập kết nối mạng một cách bảo mật. SSH hoạt động ở lớp trên trong mô hình phân lớp TCP/IP. Các công cụ SSH (như OpenSSH) cung cấp cho người dùng cách thức để thiết lập kết nối mạng được mã hoá để tạo một kênh kết nối riêng tư.
- SSH tạo ra cơ chế xác thực qua mật khẩu mạnh, hình thành mối liên kết giao tiếp dữ liệu mã hóa ra giữa hai máy qua môi trường internet.
- Chức năng:
    - Hỗ trợ truy cập từ xa vào những hệ thống, thiết bị ứng dụng giao thức SSH.
    - Cho phép dịch chuyển file an toàn.
    - Thực thi lệnh bảo mật, an toàn trên hệ thống điều khiển từ xa.
    - Quản lý an toàn và hiệu quả thành phần hạ tầng mạng.

## 1.2. Mục đích sử dụng SSH

![Untitled](/images/part1/1-1.2-ssh.jpeg)

Mục đích SSH được tạo ra là để thay thế cho trình giả lập Terminal, cơ chế đăng nhập không an toàn (Telnet, Rlogin). Giao thức SSH hỗ trợ tính năng đăng nhập, khởi chạy Terminal Session thông qua hệ thống điều khiển từ xa.
Chức năng cơ bản nhất của giao thức SSH là liên kết với một host từ xa, ứng với một phiên Terminal bằng dòng lệnh "ssh [server.example.org](http://server.example.org/)". Dòng lệnh này có thể liên kết Client với một máy chủ [server.example.com](http://server.example.com/) thông qua ID người dùng UserName.
Trường hợp đó là lần kết nối đầu tiên giữa của Server và Host, người dùng phải được thông báo mã khóa của Host.

## 1.3. Giới thiệu Remote-SSH Extension trên VSCode

![Untitled](/images/part1/1-1.3-extension.png)

**Remote - SSH Extension** cho phép bạn sử dụng bất kỳ máy từ xa nào có máy chủ SSH làm môi trường phát triển của bạn. Điều này có thể đơn giản hóa đáng kể quá trình phát triển và khắc phục sự cố trong nhiều tình huống khác nhau, giúp bạn:

1. Phát triển trên cùng hệ điều hành mà bạn triển khai hoặc sử dụng phần cứng lớn hơn, nhanh hơn hoặc chuyên dụng hơn so với máy cục bộ của bạn.
2. Nhanh chóng chuyển đổi giữa các môi trường phát triển từ xa khác nhau và thực hiện cập nhật an toàn mà không lo ảnh hưởng đến máy cục bộ của bạn.
3. Truy cập môi trường phát triển hiện có từ nhiều máy hoặc vị trí.
4. Gỡ lỗi ứng dụng đang chạy ở nơi khác như trang web của khách hàng hoặc trên đám mây.
5. Không cần mã nguồn trên máy cục bộ của bạn để có được những lợi ích này vì tiện ích mở rộng chạy lệnh và các tiện ích mở rộng khác trực tiếp trên máy từ xa. Bạn có thể mở bất kỳ thư mục nào trên máy từ xa và làm 6. việc với nó giống như bạn sẽ làm nếu thư mục đó ở trên máy của riêng bạn.

## 1.4. Hướng dẫn khởi tạo Ubuntu EC2

### Bước 1: Tạo VPC

- Điều hướng tới trang dịch vụ VPC

  ![Untitled](/images/part1/1-1.4-step1-1.png)

- Chọn Create VPC

  ![Untitled](/images/part1/1-1.4-step1-2.png)

  ![Untitled](/images/part1/1-1.4-step1-3.png)

- Chọn: Create VPC, chờ trong giây lát ta sẽ thu được kết quả như này. Tiếp theo, ta có thể ấn View VPC để xem

  ![Untitled](/images/part1/1-1.4-step1-4.png)

  ![Untitled](/images/part1/1-1.4-step1-5.png)

### Bước 2: Điều hướng tới trang dịch vụ của EC2.

- Bạn có thể gõ từ khóa “EC2” vào thanh tìm kiếm để điều hướng tới trang dịch vụ.
- Tại đây, Bạn chọn Launch instance **để khởi tạo EC2 instance.**

  ![Untitled](/images/part1/1-1.4-step2-1.png)

### Bước 3: Khởi tạo EC2

- **Thiết lập các thông tin về Name và Application and OS Images:**
    - Name: **remote-connection**
    - **Application and OS Images**:  Ubuntu

  ![Untitled](/images/part1/1-1.4-step3-1.png)

- **Thiết lập các thông tin về AMI:**
    - AMI: Ubuntu Server 24.04 LTS (HVM), SSD Volume Type (mặc định)

  ![Untitled](/images/part1/1-1.4-step3-2.png)

- Chọn Instance Type:
    - Instance Type: t2.micro

  ![Untitled](/images/part1/1-1.4-step3-3.png)

- Tạo key pair:
    - Chọn: Create new key pair
    - Name: remote_connect
    - Type: RSA
    - Private key file: .pem
    - Chọn Create new key pair để tạo tạo key pair

  ![Untitled](/images/part1/1-1.4-step3-4.png)

  ![Untitled](/images/part1/1-1.4-step3-5.png)

- Ta sẽ nhận được 1 key pair:

  ![Untitled](/images/part1/1-1.4-step3-6.png)

- Tại phần Networking settings ta chọn theo mẫu bên dưới. Tại Firewall ta chọn: security group

  ![Untitled](/images/part1/1-1.4-step3-7.png)

- Cuối cùng chọn, Laucn instance để khởi tạo Ec2 đã vừa thiết lập thông tin. Kết quả thu được là

  ![Untitled](/images/part1/1-1.4-step3-8.png)


## 1.5. Hướng dẫn cấu hình kết nối Command Prompt với EC2

### Bước 1: Mở Command Prompt

- `Win + R` và gõ “cmd” để mở Command Prompt

  ![Untitled](/images/part1/1-1.5-step1-1.png)


- Bảng Command Prompt được mở, bạn cùng lệnh `cd Downloads` để điều hướng tới folder đang chứa file `testing-remote-connection.pem`

  ![Untitled](/images/part1/1-1.5-step1-2.png)

### Bước 2: Kết nối SSH

- Tại không gian làm việc của dịch vụ EC2, chọn EC2 bạn muốn kết nối SSH. Tiếp đó, chọn `Connect`

  ![Untitled](/images/part1/1-1.5-step2-1.png)


- Sao chép lệnh `ssh -i "testing-remote-connection.pem" [ubuntu@ec2-13-212-36-87.ap-southeast-1.compute.amazonaws.](mailto:ubuntu@ec2-13-212-36-87.ap-southeast-1.compute.amazonaws.com)com`

  ![Untitled](/images/part1/1-1.5-step2-2.png)

- Dán lệnh vừa sao chép vào Command Prompt. Tiếp đó, bạn nhập “yes” để xác nhận kết nối.

  ![Untitled](/images/part1/1-1.5-step2-3.png)

- Kết nối thành công

  ![Untitled](/images/part1/1-1.5-step2-4.png)
- Bạn có thể kiểm tra bằng cách tạo file [test.py](http://test.py) trong folder “test” bằng các dòng lệnh như hình bên dưới.

  ![Untitled](/images/part1/1-1.5-step2-5.png)

- Kết quả, nạn sẽ nhận được màn hình làm việc của file [test.py](http://test.py)

  ![Untitled](/images/part1/1-1.5-step2-6.png)

## 1.6. Hướng dẫn cấu hình kết nối VSCode với EC2

- Cài đặt tiện ích Remote-SSH để hỗ trợ việc kết nối vối SSH Host

  ![Untitled](/images/part1/1-1.6-1.png)

- Sau khi cài đặt tiện ích, bạn sẽ nhìn thấy icon ở phía dưới bên trái của màn hình.

  ![Untitled](/images/part1/1-1.6-2.png)

- Ấn vào icon để mở bảng chọn kết nối. Chọn “Connect to Host…”

  ![Untitled](/images/part1/1-1.6-3.png)

- Chọn “Add New SSH Host” để tạo một SSH Host mới

  ![Untitled](/images/part1/1-1.6-4.png)

- Chọn “C:\Users\Legion\.ssh\config” để mở fiel config

  ![Untitled](/images/part1/1-1.6-5.png)
- Tại không gian làm việc của EC2, bạn chọn “Details” để lấy các thông tin cần thiết cho việc thiết lập SSH Host

```
Host remote-connection
    HostName 13.212.36.87
    User ubuntu
    IdentityFile "C:\Users\Legion\Downloads\testing-remote-connection.pem"
```

- Sau khi thiết lập xong, bạn ấn vào icon để mở bạng chọn và điều hướng tới “remote-connection”

  ![Untitled](/images/part1/1-1.6-6.png)

- Tiếp theo, bạn chọn Platform details “Linux” và chọn “Continue”

  ![Untitled](/images/part1/1-1.6-7.png)

- Thành công khi bạn mình làm việc của “SSH Host: remote-connection” mở ra

  ![Untitled](/images/part1/1-1.6-8.png)

- Tiếp theo bạn có thể  điều hướng tới folder test của mình bằng cách chọn File, chọn Open folder  và điều hướng tới folder test.
  ![Untitled](/images/part1/1-1.6-9.png)