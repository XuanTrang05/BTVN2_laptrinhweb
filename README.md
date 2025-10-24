# BTVN2_laptrinhweb
Bài tập 02: Lập trình web.
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
chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
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
## BÀI LÀM
2.1. Cài đặt Apache web server:
- Truy cập trang chủ của apache để cài đặt
- ấn download -> a number of  third party vendors
  <img width="1920" height="1080" alt="Screenshot 2025-10-21 210216" src="https://github.com/user-attachments/assets/ee423006-a7d9-407c-a381-99ed58166bc7" />
  <img width="1920" height="1080" alt="Screenshot 2025-10-21 210327" src="https://github.com/user-attachments/assets/bd4fb93a-475e-41b9-b228-32ebcedc97c2" />
-  Chọn bộ xử lí phù hợp với máy để tải xuống
<img width="1919" height="1079" alt="Screenshot 2025-10-21 210451" src="https://github.com/user-attachments/assets/9543eddf-8054-4bbc-9646-35f7d47b9098" />
- Giải nén tại ổ D
   <img width="1203" height="1079" alt="image" src="https://github.com/user-attachments/assets/bd076556-3e62-45e8-a86c-e94a35ffcd0d" />
- Cấu hình file D:\Apache24\conf\httpd.conf .
  <img width="940" height="509" alt="image" src="https://github.com/user-attachments/assets/3386369c-44aa-4809-9061-ec90bead6f59" />
  <img width="940" height="519" alt="image" src="https://github.com/user-attachments/assets/d906af51-2afb-4d92-8fd6-096c3f1aca11" />
- đổi SeverName www.example.com:80 thành ServerName localhost:80f
- <img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/a9c09495-32cd-41f6-a9d7-a7c1d769125a" />
- đổi Define SRVROOT "c:/Apache24 thành Define SRVROOT "D:/Apache24"
<img width="490" height="224" alt="image" src="https://github.com/user-attachments/assets/259fee55-e425-4681-bd8a-85e636ef382f" />
- xóa dấu # tại phần Include conf/extra/httpd-vhosts.conf
<img width="598" height="187" alt="image" src="https://github.com/user-attachments/assets/5db486fc-74c3-47f7-897f-30d2cea1a035" />
- Cấu hình D:Apache24\conf\extra\httpd-vhosts.conf
<img width="956" height="1079" alt="image" src="https://github.com/user-attachments/assets/caaff020-fd54-44dd-80e8-eae111de0334" />
-sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain
<img width="960" height="1079" alt="image" src="https://github.com/user-attachments/assets/17282224-229f-4528-8774-a3429cce2497" />
- tạo foder với họ và tên mình trong mục apache, sau đó tạo file index dươis dạng txt,
<img width="589" height="403" alt="image" src="https://github.com/user-attachments/assets/e9c5eb2d-47a0-42be-ba7a-fdc140555c30" />
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
- <img width="940" height="534" alt="image" src="https://github.com/user-attachments/assets/c9f67fb8-40d6-40ae-afaf-e4a1ecb3e0e7" />
<img width="955" height="1079" alt="image" src="https://github.com/user-attachments/assets/171f1992-8cbf-4737-915c-8aad5507d1ed" />
2.2 Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
<img width="1920" height="1080" alt="Screenshot 2025-10-22 225420" src="https://github.com/user-attachments/assets/bbc12b61-0f87-4755-bf91-3675f9951752" />
<img width="1920" height="1080" alt="Screenshot 2025-10-22 225435" src="https://github.com/user-attachments/assets/d56ba571-f5eb-4ecd-a525-752edf9570ec" />
<img width="1920" height="1080" alt="Screenshot 2025-10-22 225505" src="https://github.com/user-attachments/assets/3a67ae71-aa04-4255-8d64-895ed9db9978" />
- Chọn tải xuống ở ổ D
- <img width="1920" height="1080" alt="Screenshot 2025-10-22 225515" src="https://github.com/user-attachments/assets/cb544f58-f978-4094-b0a2-da02e09cf95d" />
<img width="1920" height="1080" alt="Screenshot 2025-10-22 225535" src="https://github.com/user-attachments/assets/4e2fdb22-8118-4d04-ac03-85496007ad3e" />
<img width="1920" height="1080" alt="Screenshot 2025-10-22 225547" src="https://github.com/user-attachments/assets/c5f6a43f-3d50-45e6-aa35-0092dfdc1bba" />
- Thư mục được cài đặt vào ổ D
<img width="956" height="1017" alt="image" src="https://github.com/user-attachments/assets/4ed7c12d-7102-4497-a35f-e51c96ad3737" />
2.3
-


-
-
-
2.4: Cài đặt thư viên trên nodered
- truy cập giao diện nodered bằng url: http://localhost:1880
<img width="1917" height="1079" alt="image" src="https://github.com/user-attachments/assets/e5b5c69d-e85a-48dc-8ea7-4819ca100583" />
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
<img width="1920" height="1080" alt="Screenshot 2025-10-23 003650" src="https://github.com/user-attachments/assets/d6606ed9-9f35-463e-8c15-0b0e3c8789ea" />
<img width="1920" height="1080" alt="Screenshot 2025-10-23 003756" src="https://github.com/user-attachments/assets/4115ef8a-e49c-40af-a68b-865ea471a282" />
<img width="1920" height="1080" alt="Screenshot 2025-10-23 003839" src="https://github.com/user-attachments/assets/467db682-4ed0-4d93-b243-bf015c08e274" />
<img width="1920" height="1080" alt="Screenshot 2025-10-23 003947" src="https://github.com/user-attachments/assets/4a6e714b-c11b-4b0f-b684-63c5e8adeef5" />
<img width="1920" height="1080" alt="Screenshot 2025-10-23 004030" src="https://github.com/user-attachments/assets/7853d875-9126-48ac-ba22-2f80be425452" />
<img width="1920" height="1080" alt="Screenshot 2025-10-23 004111" src="https://github.com/user-attachments/assets/a8dd26db-c598-434b-8231-28ad70d5bef8" />
<img width="1920" height="1080" alt="Screenshot 2025-10-23 004145" src="https://github.com/user-attachments/assets/b0ab977f-eca9-4e67-b702-44de4e4d1d15" />
<img width="1920" height="1080" alt="Screenshot 2025-10-23 004219" src="https://github.com/user-attachments/assets/af3cf941-ed79-48a9-bc67-30b7d7b6f643" />



