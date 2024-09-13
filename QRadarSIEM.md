References:
https://www.ibm.com/docs/en/SS42VS_7.4/pdf/b_qradar_users_guide.pdf

## 1: Cách tạo và customize Dashboard trong QRadar Console

References: https://www.ibm.com/docs/en/qradar-on-cloud?topic=siem-dashboard-management

Dashboard là trang mặc định khi đăng nhập vào UI của QRadar, dùng để quan sát cụ thể trạng thái của hệ thống hiện tại.

Tại thẻ Dashboard trên giao diện của QRadar Console, chọn New Dashboard để tạo mới một Dashboard.
![image](https://hackmd.io/_uploads/SkURXJO2A.png)

Một cửa sổ popup hiện lên, nhập tên muốn đặt cho Dashboard này và nhấn OK.
![image](https://hackmd.io/_uploads/rJxQB1uhC.png)

Một Dashboard mới hiện ra với giao diện trống, ta cần chọn Add Item để thêm các widget cho Dashboard.
![image](https://hackmd.io/_uploads/HJwgK-d2A.png)

Một vài widget mặc định:
- System Summary: Hiển thị tình hình tổng quan về trạng thái của hệ thống QRadar SIEM như FPS, EPS hiện tại, các Flow, Event mới, ...
- System Notification: Hiển thị các thông báo của QRadar, bao gồm cảnh báo, lỗi hoặc các sự kiện liên quan đến hệ thống hoặc thành phần hạ tầng.
- Most Recent Offense: hiển thị các sự cố bảo mật gần nhất.

![image](https://hackmd.io/_uploads/SyCp5ZOnR.png)

Các chú ý:
- Các Widget có thể tùy chỉnh vị trí nhưng không thể chỉnh sửa kích cỡ.
- Một số widget có thể chỉnh số lượng kết quả trả về, ví dụ như top 5, top 10, ...
- Có một số kết quả có thể vẽ biểu đồ và tùy chỉnh dạng biểu đồ sao cho phù hợp.

Từ phiên bản 7.4.0 trở đi, QRadar bổ sung thêm Pulse App để tăng cường sự trực quan khi sử dụng Dashboard của QRadar.

## 2: Cách tạo và customize các rule của QRadar SIEM

References: https://www.ibm.com/docs/en/uax?topic=rules-detection-rule-properties

IBM QRadar cung cấp một bộ các rule để xác định một phạm vi hoạt động lớn của hệ thống mạng. Các rules này áp dụng cho các event, flow hoặc offense để tìm kiếm hoặc phát hiện bất thường trong QRadar.

Để xem và tạo các custom rule, ta truy cập vào tab Offense, chọn Rules
![image](https://hackmd.io/_uploads/B1GjuJh2C.png)

Một giao diện mới hiện ra bao gồm tất cả các rule hiện có.
![image](https://hackmd.io/_uploads/BJMkKknhC.png)

Có một số cột cần chú ý:
- Origin: cho biết nguồn gốc của 1 rule. (System - rule được tạo mặc định, Modified - rule mặc định nhưng đã được chỉnh sửa, User - rule được tạo bởi người dùng)
- Enabled: cho biết trạng thái của rule đó có được kích hoạt hay chưa.

Để tạo 1 custom rule, chọn Action sau đó chọn các Options phù hợp:
![image](https://hackmd.io/_uploads/BkFzQgn3A.png)

Một giao diện mới hiện ra để tiến hành config cho rule mới:
![image](https://hackmd.io/_uploads/BkZy4gnhA.png)

Chọn các Options phù hợp cho rules, sau đó chọn next:
![image](https://hackmd.io/_uploads/SyecHxh2C.png)

Tại giao diện này, gồm có các mục như sau:

##### Rule Action: Các hành động cần thực hiện khi một sự kiện xảy ra và kích hoạt các quy tắc này. Gồm:
- Serverity: độ nghiêm trọng của 1 sự kiện hoặc cảnh báo và các tác động có thể có đối với hệ thống.
    - VD: DDoS có khả năng làm sập toàn bộ hệ thống mạng nội bộ --> Serverity cao. Hành động quét cổng trên một máy endpoint bằng nmap chỉ là việc do thám, không ảnh hưởng quá nhiều --> Serverity thấp.
- Credibility: độ đáng tin cậy của thông tin về sự kiện đó. 
    - VD: Phát hiện hành động mạng bất thường từ log của IDS --> Credibility cao. Phát hiện hành động mạng bất thường từ NetFlow của Router --> Credibility thấp.
- Relevance: mức độ liên quan của sự kiện đối với hệ thống, đánh giá xem liệu sự kiện này có ảnh hưởng đến các tài nguyên quan trọng của tổ chức hay không.
    - VD: Endpoint bị tấn công --> Relevance thấp. DB bị tấn công --> Relevance cao.
- Từ các chỉ số trên, ta tính được thêm một chỉ số nữa là Magnitude: thể hiện mức độ nghiêm trọng tổng thể của một event
- Ensure the detected event is part of an offense: đảm bảo sự kiện đã phát hiện là một phần của một offense
- Annotate event: ghi chú sự kiện
- Bypass futher rule corelation event: khi tick tùy chọn này, các event khác sẽ không được kiểm tra để kiểm tra độ tương quan

##### Rule Response: Xác định các hành động phản hồi khi rule được kích hoạt. Gồm:
- Dispatch new event: tạo ra một event mới dựa trên event kích hoạt
- Email: gửi mail thông báo
- Send to Local Syslog: gửi event này tới Syslog cục bộ của QRadar
- Send to Forwarding Destination: gửi sự kiện đến các điểm chuyển tiếp
- Notify: Thông báo cho người dùng hoặc các hệ thống khác về event
- Add/Remove to a Reference Set: Thêm/Xóa event vào một tập tham chiếu đã được xác định
- Add/Remove to a Reference Data: Thêm/Xóa event vào dữ liệu tham chiếu
- Execute custom action: Thực hiện một hành động tùy chỉnh đã được lập trình sẵn.

##### Response Limiter: Giới hạn tần suất phản hồi của rule để tránh việc rule này phản ứng quá nhiều lần trong một thời gian ngắn.

Sau khi đã chọn xong các hành động cần thiết, Finish để hoàn thành việc tạo rule.

Sau khi tạo xong rule sẽ hiển thị trong danh sách các rule với Origin là User:
![image](https://hackmd.io/_uploads/BkKlclh3A.png)

References: https://www.ibm.com/docs/en/qsip/7.5?topic=rules-creating-custom-rule
## 3: Cách tạo và customize Device Support Module (DSM) trong QRadar SIEM

DSM (Device Support Module) trong IBM QRadar là một thành phần chịu trách nhiệm phân tích, chuẩn hóa và trích xuất thông tin quan trọng từ các event và log được gửi tới QRadar từ các thiết bị bảo mật, hệ thống mạng, ứng dụng, và các dịch vụ, ...

Để custom một DSM, truy cập vào Admin Tab và chọn DSM Editor ở mục Data Sources:
![image](https://hackmd.io/_uploads/Hk-oW7h3A.png)

Một tab mới hiện ra, chọn Create new để tiến hành tạo mới:
![image](https://hackmd.io/_uploads/SydqGmn3A.png)

Nhập các thông tin cần thiết và chọn DSM mới tạo, sẽ được giao diện như dưới đây:
![image](https://hackmd.io/_uploads/BJJ17723C.png)

Nhập một đoạn log vào ô Workspace để parsing như mẫu dưới đây:
![image](https://hackmd.io/_uploads/S1TTm7330.png)

Tại mục Properties bên trái, chọn thông tin muốn lọc từ đoạn log. Trong ví dụ này, chọn mục Event ID và tick vào ô Override system behavior:
![image](https://hackmd.io/_uploads/ry_vVXnhA.png)

Mục Expression sẽ là những phép toán để ta lọc được thông tin cần thiết trong đoạn log. Với ví dụ này, ta sẽ dùng Regex để lọc lấy các thông tin trong trường eventId:
![image](https://hackmd.io/_uploads/SJqiSQh20.png)

Giao diện Workspace cũng sẽ highlight những phần được lấy để ta dễ quan sát:
![image](https://hackmd.io/_uploads/B18CSm32A.png)

Nhấn OK và kết quả sẽ được hiển thị ở phần Log Activity Preview:
![image](https://hackmd.io/_uploads/H1AZL7h30.png)

Có thể thấy dữ liệu đã được lọc thành công. 
Làm tương tự với một vài trường nữa và ta được kết quả từ đoạn log trên như sau:
![image](https://hackmd.io/_uploads/B1Bx_Q33R.png)

Tiếp theo, ta cần thực hiện Mapping để ánh xạ các giá trị này cho phù hợp với QRadar. Chọn tab Event Mappings bên cạnh tab Properties và nhấn dấu (+), tab mới hiện ra như sau:
![image](https://hackmd.io/_uploads/HJI4tmh3C.png)

QRadar khá thông minh và sẽ tự lọc các trường cần thực hiện mapping. Ở ví dụ này, ta có Event ID và Event Category. Để tạo được một Event Mappings, ta cần cấp cho nó một QID Record. Đây là giá trị định danh DUY NHẤT để ánh xạ các loại event mà QRadar nhận được từ nhiều thiết bị và hệ thống khác nhau.

Nhấp vào Choose QID... và một giao diện mới hiện ra:
![image](https://hackmd.io/_uploads/Bk9qjQ220.png)

Chọn Create New QID Record, sau đó điền các thông tin cần thiết và Save để lưu lại:
![image](https://hackmd.io/_uploads/B1byaX22R.png)

Nhấn OK để chọn QID Record, giao diện như sau là đã chọn thành công:
![image](https://hackmd.io/_uploads/r1XZTQn2A.png)

Cuối cùng nhấn OK và kết quả sau khi Mapping thành công:
![image](https://hackmd.io/_uploads/H1zqRX230.png)

## Use case thực tế: Giám sát network traffic tại một Ubuntu host bằng QRadar

Trước tiên, cần xác định Flow Collector của QRadar SIEM đang hoạt động trên port nào. Tại UI của QRadar, chọn Admin --> Flow Sources
![image](https://hackmd.io/_uploads/S1FEYRy6C.png)

Một tab mới hiện ra, nháy đúp để xem thông tin
![image](https://hackmd.io/_uploads/ByUcKC16A.png)

Có thể thấy Flow Collector này đang hoạt động trên port 2055 với Source Type là NetFlow
![image](https://hackmd.io/_uploads/rJW3KAJTA.png)

Như vậy, ta sẽ dùng softflowd để thực hiện capture các traffic trên Ubuntu host và gửi nó đến QRadar.

Tại Ubuntu host, tiến hành cài đặt softflowd bằng các câu lệnh:
```
$ sudo apt update
$ sudo apt install softflowd
```
Sau đó kiểm tra tên interface mà card mạng đang kết nối:
```
$ sudo apt install ifconfig
$ ifconfig 
```
Kết quả sẽ tương tự như hình sau:
![image](https://hackmd.io/_uploads/r1r3jAk60.png)
 Như vậy interface chúng ta cần giám sát sẽ là ens160
 
 Chạy câu lệnh theo cú pháp dưới đây để softflowd thực hiện việc capture và gửi dữ liệu đến QRadar:
```
sudo softflowd -i [interface] -n [qradar_ip]:[port]
```

Kết quả sau khi thực hiện thành công trên tab Network Activity của QRadar:
![image](https://hackmd.io/_uploads/SywN30kT0.png)

## Use case thực tế: Chuyển log từ pfSense Firewall về QRadar, cảnh báo về các hoạt động mạng bị pfSense từ chối.

![image](https://hackmd.io/_uploads/BJwg1x-pR.png)

Để chuyển log từ pfSense Firewall, làm theo hướng dẫn ở link [này](https://www.manageengine.com/products/firewall/help/configure-pfsense-firewalls.html#:~:text=Log%20into%20the%20pfsense%20Web%20Interface.%20Navigate%20to,in%20the%20Remote%20log%20servers%20field.%20Click%20Save.)

Chọn các loại log muốn đẩy về QRadar và Save để lưu lại.
![image](https://hackmd.io/_uploads/HkiAklWpA.png)

Tại QRadar, tạo Log Source tương ứng từ Log Source Management. Bản thân QRadar đã có sẵn nguồn Log này qua giao thức Syslog:
![image](https://hackmd.io/_uploads/HkaHxgWTR.png)

![image](https://hackmd.io/_uploads/B1_IleWpA.png)

Thực hiện các cài đặt còn lại tương tự như khi đẩy log từ các host về QRadar. Sau đó Deploy Changes ở tab Admin và kiểm tra pfSense đã kết nối và đẩy Log sang QRadar hay chưa:
![image](https://hackmd.io/_uploads/BJphlxbpR.png)

Kết quả sau khi thực hiện thành công:
![image](https://hackmd.io/_uploads/rkigZg-6C.png)
