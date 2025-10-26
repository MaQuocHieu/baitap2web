# Bài tập 02: Lập trình web.
==============================
NGÀY GIAO: 19/10/2025
==============================
DEADLINE: 26/10/2025
==============================
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.
2. NỘI DUNG BÀI TẬP:
2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Đỗ Duy Cốp` thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
2.7. Nhận xét bài làm của mình:
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
- đã hiểu cách frond-end tương tác với back-end ra sao?
==============================
TIÊU CHÍ CHẤM ĐIỂM:
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ
==============================
GHI CHÚ:
1. yêu cầu trên cài đặt trên ổ D, nếu máy ko có ổ D có thể linh hoạt chuyển sang ổ khác, path khác.
2. có thể thực hiện trực tiếp trên máy tính windows, hoặc máy ảo
3. vì csdl là tuỳ ý: sv cần mô tả rõ db chứa dữ liệu gì, nhập nhiều dữ liệu test có nghĩa, json trả về sẽ có dạng như nào cũng cần mô tả rõ.
4. có thể xây dựng nhiều API cùng cơ chế, khác tính năng: như tìm kiếm, thêm, sửa, xoá dữ liệu trong DB.
5. bài làm phải có dấu ấn cá nhân, nghiêm cấm mọi hình thức sao chép, gian lận (sẽ cấm thi nếu bị phát hiện gian lận).
6. bài tập thực làm sẽ tốn nhiều thời gian, sv cần chứng minh quá trình làm: save file `readme.md` mỗi khoảng 15-30 phút làm : lịch sử sửa đổi sẽ thấy quá trình làm này!
7. nhắc nhẹ: github ko fake datetime được.
8. sv được sử dụng AI để tham khảo.
==============================
DEADLINE: 26/10/2025
==============================
# Bài làm
2.1. Cài đặt Apache web server
- các bước:
- Truy cập trang chủ của apache để cài đặt
- Ấn download -> a number of third party vendors

  <img width="897" height="579" alt="image" src="https://github.com/user-attachments/assets/3eeb02ff-ae6c-49a1-a3cf-9747a649c4c2" />

  <img width="923" height="909" alt="image" src="https://github.com/user-attachments/assets/7c2e6f5a-0acc-4a22-9c54-b99f9010b502" />
- Chọn bộ xử lí phù hợp với máy để tải xuống
- Giải nén tại ổ I
  
  <img width="937" height="646" alt="image" src="https://github.com/user-attachments/assets/d18d15bc-3838-4e85-8c6b-78b1584c0662" />
  
- Cấu hình file I

  <img width="935" height="618" alt="image" src="https://github.com/user-attachments/assets/42d0e5e8-ee0c-4ecd-b54f-87b2816f5459" />

- Đổi SeverName thành ServerName localhost:80
  
  <img width="1361" height="908" alt="image" src="https://github.com/user-attachments/assets/9f67f305-56ea-40f2-80b3-d2f56ee73bbf" />

-Cấu hình I:Apache24\conf\extra\httpd-vhosts.conf

<img width="946" height="622" alt="image" src="https://github.com/user-attachments/assets/a60215df-6b45-47fe-a74c-e19aa73aea62" />

<img width="940" height="645" alt="image" src="https://github.com/user-attachments/assets/2d806d93-02b2-4978-a6dc-de48c6d6851f" />

- Tạo foder với họ và tên mình trong mục apache, sau đó tạo file index dưới dạng txt
  <img width="728" height="560" alt="image" src="https://github.com/user-attachments/assets/f2a0bcbf-5688-4aa9-9582-dc2ef6dc71c2" />

<img width="762" height="512" alt="image" src="https://github.com/user-attachments/assets/0a065c11-7c89-44df-8047-59feb48ee2ee" />

- Thao tác dòng lệnh trên file I:\Apache24\bin\httpd.exe với các tham số -k install và -k start để cài đặt và khởi động web server apache.
<img width="1103" height="638" alt="Screenshot 2025-10-25 215106" src="https://github.com/user-attachments/assets/ee08cc0a-ac61-4cc8-8c9a-b1b1600ae6b9" />

<img width="1920" height="1080" alt="Screenshot 2025-10-25 221220" src="https://github.com/user-attachments/assets/461e30c5-5e20-48d7-bfd0-7ce4dd0f6e54" />

2.2 Cài đặt nodejs và nodered => Dùng làm backend: - Cài đặt nodejs:

<img width="836" height="592" alt="Screenshot 2025-10-25 222240" src="https://github.com/user-attachments/assets/a1e7c549-bce6-4071-a026-3ee0e352d3a9" />

- chọn ổ cần lưu (ổ I) sau đó bấm next
  <img width="1143" height="774" alt="Screenshot 2025-10-25 222347" src="https://github.com/user-attachments/assets/65072e32-1266-4cde-87e3-7575502220b1" />
  
<img width="911" height="657" alt="Screenshot 2025-10-25 222604" src="https://github.com/user-attachments/assets/508c2edc-f231-4964-a41b-a403e5489c72" />

- Thư mục được cài đặt vào ổ I
  
  <img width="919" height="852" alt="image" src="https://github.com/user-attachments/assets/e0a5817f-e5a2-415e-95bd-019e08597dfc" />
  
- Tiến hành cài đặt nodered
  <img width="948" height="519" alt="Screenshot 2025-10-25 223433" src="https://github.com/user-attachments/assets/bb881e50-1775-497f-b25d-0f2e0f88ed5a" />
  
- Khởi chạy
  <img width="1902" height="1023" alt="Screenshot 2025-10-25 224701" src="https://github.com/user-attachments/assets/81dccda6-c50f-4ff4-8dd1-09826d5fa9ac" />
  
  2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022)

 <img width="1741" height="886" alt="Screenshot 2025-10-26 100646" src="https://github.com/user-attachments/assets/90a24fbf-7318-4c17-ae6e-98528994a987" />
 
  2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880
  <img width="1891" height="901" alt="image" src="https://github.com/user-attachments/assets/9356c23a-2a0a-46d6-8b7c-271c9351bbd5" />

- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
  <img width="903" height="934" alt="image" src="https://github.com/user-attachments/assets/0222a7fa-970a-47c7-9f6e-d94b4bd854a2" />

- Sửa file `I:\nodejs\nodered\work\settings.js`:
  
  <img width="1111" height="677" alt="image" src="https://github.com/user-attachments/assets/de501859-15c2-4113-bfb1-ed7c517248e3" />
- reset lại nodered
  <img width="656" height="269" alt="Screenshot 2025-10-26 101755" src="https://github.com/user-attachments/assets/f02ca0d7-ca2b-4f65-98ac-aaab56ddd3f6" />
- Đăng nhập vào nodered
  <img width="1286" height="861" alt="image" src="https://github.com/user-attachments/assets/411bb994-976f-49d2-b561-180e6278e755" />
  
2.5. tạo api back-end bằng nodered:

  <img width="1898" height="961" alt="image" src="https://github.com/user-attachments/assets/e8a99182-991e-458a-b6d5-639be4771f34" />
  
  <img width="1920" height="1080" alt="Screenshot 2025-10-26 104531" src="https://github.com/user-attachments/assets/374abbc9-fa0c-414e-9ac6-84558c0d211c" />
  
2.6. Tạo giao diện front-end - html form gồm các file : index.html, fullname.js, fullname.css

- HTML
<img width="978" height="593" alt="image" src="https://github.com/user-attachments/assets/f1111313-38ee-42ee-9854-205919b93450" />
- mqquochieu.css
  <img width="1488" height="981" alt="image" src="https://github.com/user-attachments/assets/18da2549-1291-4c1d-a63a-e4905220f5fb" />
- mqquochieu.js
  <img width="1114" height="818" alt="image" src="https://github.com/user-attachments/assets/0b402109-98c0-4666-9d9e-8ccc6ed08e09" />

- Kết quả
  <img width="1920" height="1080" alt="Screenshot 2025-10-26 112247" src="https://github.com/user-attachments/assets/03a1acad-a265-44f4-a61c-dd6cd990f27f" />

2.7  Nhận xét bài làm của mình:

- Về cài đặt phần mềm và thư viện:
Mình đã hiểu rõ quy trình cài đặt Node.js, Node-RED, SQL Server và Apache, cũng như cách thêm các thư viện mở rộng trong Node-RED. Biết được mục đích của từng công cụ và cách chúng liên kết với nhau trong hệ thống.

- Về tạo API back-end bằng Node-RED:
Đã nắm được cách sử dụng các node cơ bản như HTTP In, Function, MSSQL, và HTTP Response để xây dựng API. Hiểu được cách Node-RED xử lý dữ liệu từ client, truy vấn CSDL và trả kết quả về.

- Về giao diện front-end và tương tác với back-end:
Hiểu được cách giao diện HTML/JS gửi yêu cầu đến API bằng fetch(), nhận dữ liệu JSON trả về và hiển thị kết quả lên web.
Biết được mối liên hệ giữa front-end và back-end trong một ứng dụng hoàn chỉnh.





  

