![image](https://github.com/user-attachments/assets/dbf3e5ed-7035-4b46-a12a-148ea09335af)I. Lý thuyết<br>
- nh web hoàn toàn dựa vào xác thực đơn yếu tố (password) để xác thực user
- một số web yêu cầu user xác minh danh tính dùng đa yếu tố xác thực
- ngày càng phổ biến loại xác thực 2 yếu tố bắt buộc và tuy chọn (2FA) dựa trên những gì bạn bt và những gì bn có
- điều này thg yêu cầu user nhập cả password truyền thống và mã xác minh tạm thời từ thiết bị ngoài băng tần họ sở hữu
- -> việc đồng thời lấy đc cả password và yếu tố từ nguồn ngoài băng tần là ít khả năng
- nhưng triển khai kém thì vẫn có thể bỏ qua hoặc vượt qua hoàn toàn giống như xác thực đơn yếu tố
- toàn bộ lợi ích của xác thực đa yếu tố chỉ đạt đc khi xác thực nhiều yếu tố khác nhau
- VD: email 2FA cần bn cung cấp password và mã xác minh, bn chỉ can truy cập mã nếu bt thông tin login của tài khoản<br>

II. Two-factor authentication tokens<br>
- mã xác minh thg đc đọc từ 1 loại thiết bị vật lý
- nh web bảo mật cao hiện cung cấp cho user 1 thiết bị chuyên dụng cho mục đích này như: thiết bị thông báo mã RSA hay bàn phím để bn truy cập ngân hàng trực tuyến or laptop lm việc
- các thiệt bị chuyên dụng này ngoài xây dựng cho mục đích bảo mật còn có ưu điểm là tạo mã xác minh trực tiếp
- một số web xài app chuyên dụng cho đt di động như Google Authenticator cho mục đích tương tự
- một số web gửi mã đến điện thoại di động user dưới dạng văn bản -> vẫn là xác minh yếu tố (cái bạn có) nma nó có nguy cơ bị lạm dụng
- mã đc gửi qua SMS chứ ko phải đc tạo bởi chính thiết bị -> mã có thể bị chặn
- hay attacker tráo đổi SIM -> lấy đc mã xác minh gửi đến nạn nhân<br>

III. Bypassing two-factor authentication<br>
1. Lý thuyết<br>
- nếu user đc nhắc nhập password sau đó nhắc nhập mã xác minh trên 1 web riêng thì thực tế user đã ở trạng thái login trước khi nhập mã xác minh
- -> nên check xem can chuyển trực tiếp đến trang chỉ login sau khi hoàn thành bước xác thực đầu tiên không
- đôi khi bạn sẽ thấy web ko thực sự kiểm tra liệu bn đã hoàn thành bước thứ 2 chưa trước khi tải trang<br>

2. Lab: 2FA simple bypass<br>
- https://0ab0003e039cc51a83a3bf1100fd007c.web-security-academy.net/<br>

This lab's two-factor authentication can be bypassed. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, access Carlos's account page.<br>

Your credentials: wiener:peter<br>
Victim's credentials carlos:montoya<br>

3. Giải<br>
- B1: login vào your acc và vào mail client để lấy mã code -> login thành công<br>

![image](https://github.com/user-attachments/assets/7827e6e3-8660-407b-b753-a3e13f1c3677)<br>

- B2: lưu URL ở trang mà nãy vừa login thành công: https://0ab0003e039cc51a83a3bf1100fd007c.web-security-academy.net/my-account?id=wiener<br>

- B3: login vào victim acc -> sau khi login thì truy cập vào URL đã sửa đôi sau để vào trang login: https://0ab0003e039cc51a83a3bf1100fd007c.web-security-academy.net/my-account?id=carlos<br>

![image](https://github.com/user-attachments/assets/909d815b-1ade-4cb2-8fe4-6aeb0b15cb33)<br>

IV. Flawed two-factor verification logic<br>
1. Lý thuyết<br>
- nghĩa là đôi khi user hoàn thiện bước xác minh đầu tiên thì web ko xác minh đầy đủ rằng chính ng đó đang hoàn thành bước thứ 2
- các bước của web (VD):<br>
+ user login tài khoản thông thường (bước xác thực 1)
+ họ đc gán 1 cookie liên quan đến acc trước khi chuyên qua bước 2
+ khi gửi mã xác minh, yêu cầu dùng cookie để xác minh acc nào user đang cố truy cập<br>
-> attacker can login tài khoản của mk nhưng sau đó thay đổi giá trị cookie thành username bất kì khi gửi mã xác minh<br>
- -> cực kì nguy hiểm nếu attacker can brute-force mã xác minh -> can login acc user tùy ý dựa trên username mà ko cần bt password<br>

2. Lab: 2FA broken logic<br>
- https://0a2e001603671fe6834b2e7300d00048.web-security-academy.net/<br>

This lab's two-factor authentication is vulnerable due to its flawed logic. To solve the lab, access Carlos's account page.<br>

Your credentials: wiener:peter<br>
Victim's username: carlos<br>
You also have access to the email server to receive your 2FA verification code.<br>

 Hint<br>
Carlos will not attempt to log in to the website himself.<br>

3. Giải<br>
- B1: thực hiện login và logout your acc
- B2: gửi get/login2 tới repeater sửa username để tạo mã xác minh tạm thời cho victim's username<br>

![image](https://github.com/user-attachments/assets/27a1e720-44ed-4f8a-a2a9-082c9708d78e)<br>

![image](https://github.com/user-attachments/assets/78616204-7f81-4641-8c3b-1ee7edd6bc3d)<br>

- B3: thực hiện login lại như bước 1 (ko logout)
- B4: gửi post/login2 tới intruder để brute-force<br>

![image](https://github.com/user-attachments/assets/780f5ff6-f036-4dd4-8c6d-ebe041adab1c)<br>

![image](https://github.com/user-attachments/assets/bf06ded4-8855-45cd-ac36-16bf19170a41)<br>

- B5: thu kết quả<br>

![image](https://github.com/user-attachments/assets/20cdd1ef-35c9-4043-8c9a-8afc233c2b55)<br>

![image](https://github.com/user-attachments/assets/ab0b08b8-870b-47d9-8dd9-81f30047dcd9)<br>

- copy url và load trong browser để login<br>

![image](https://github.com/user-attachments/assets/57bfb4b9-a4e8-4061-8bcc-e60ea7293fa6)<br>

V. Brute-forcing 2FA verification codes<br>
1. Lý thuyết<br>
- cũng như password, web cx cần các bc để ngăn chặn brute-force mã xác minh 2FA
- mã thường chỉ có 4-6 số -> nếu ko đc bảo vệ brute-force đầy đủ thì việc crack đc mã xác minh rất dễ dàng
- một số web ngăn chặn bằng cách tự động logout user nếu họ nhập 1 số lượng nhất định mã xác minh ko chính xác
- ko hiệu quả vì attacker can tự động tiến trình nh bc này bằng cách tạo macros cho intruder, tiện ích Turbo Intruder cx đc xài cho mục đích này<br>

2. Macros<br>
- https://portswigger.net/burp/documentation/desktop/settings/sessions#macros
- macro là 1 chuỗi đc xác định trước của 1 or nh yêu cầu -> dùng trong quy tắc xử lý phiên để thực hiện nh tác vụ khác nhau:<br>
+ tìm nạp 1 trang của app (user's home page) để check xem phiên hiện tại còn hiệu lực ko
+ thực hiện login để lấy phiên hợp lệ mới
+ lấy mã thông báo or nonce để dùng làm tham số trong yêu cầu khác
+ khi quét or lm mở 1 yêu cầu trong tiến trình đa bc, macro can thực hiện bất kì yêu cầu cần thiết nào để đưa app vào trạng thái nơi yêu cầu đích đc chấp nhận
+ trong tiến trình đa bc, macro can hoàn thành các bc còn lại sau yêu cầu attack. nó can xác nhận hành động or kết thúc quá trình<br>
- ngoài trình tự yêu cầu, mỗi macro chỉ định cách xử lý cookie và tham số trong trình tự cũng như mọi sự phụ thuộc lẫn nhau giữa các mục
- thêm mới macro bằng cách nhấn Add để hiển thị hộp thoại Macro Editor
- can chỉnh sửa các macro hiện có và thay đổi vị trí chúng trong list<br>

3. Turbo Intruder<br>
- https://portswigger.net/bappstore/9abaa233088242e8be252cd4ff534988
- tiện ích giúp gửi lượng lớn yêu cầu HTTP và phân tích kết quả
- nó bổ sung cho intruder bằng cách xử lý các attack tốc độ cực cao or phức tạp
- các tính năng:<br>
+ nhanh: dùng ngăn xếp HTTP đc mã hóa thủ công từ đầu vs tốc độ cao -> trên nh mục tiêu, no can vượt xa thâm chí cả các tập lệnh Go ko đồng bộ đang thịnh hành
+ linh hoạt: attack đc cấu hình bằng python -> xử lý các yêu cầu phức tạp như yêu cầu đã đánh dấu và chuỗi tấn công nh bước; ngăn xếp HTTP tùy chỉnh giúp nó xử lý các yêu cầu ko đúng định dạng lm hỏng các thư viện khác
+ can mở rộng: can xài bộ nhớ phẳng -> cho phép các cuộc tấn công nh ngày đáng tin cậy, nó cx can chạy ko môi trg headless thông qua dòng lệnh
+ thuận tiện: kết quả ko hữu ích đc lọc bằng thuật toán phân biệt nâng cao đc điều chỉnh từ Backslash Powered Scanner<br>
- cách dùng: đánh dấu vùng bn muốn chèn và chọn Send to Turbo Intruder -> mở của sổ có đoạn mã python can tùy chỉnh trước khi attack<br>

4. Lab: 2FA bypass using a brute-force attack<br>
- https://0a9a00ea03b6654b803663d500a60028.web-security-academy.net/<br>

This lab's two-factor authentication is vulnerable to brute-forcing. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, brute-force the 2FA code and access Carlos's account page.<br>

Victim's credentials: carlos:montoya<br>

Note<br>
As the verification code will reset while you're running your attack, you may need to repeat this attack several times before you succeed. This is because the new code may be a number that your current Intruder attack has already attempted.<br>
 Hint<br>
You will need to use Burp macros in conjunction with Burp Intruder to solve this lab. For more information about macros, please refer to the Burp Suite documentation. Users proficient in Python might prefer to use the Turbo Intruder extension, which is available from the BApp store.<br>

5. Giải<br>
- B1: thực hiện login và nhập mã xác minh -> sai 2 lần sẽ tự động logout<br>

![image](https://github.com/user-attachments/assets/b338c2aa-b6b1-4c57-b818-f73ad8a2e1fe)<br>

- B2: c.bi các bước để tạo macro<br>

![image](https://github.com/user-attachments/assets/4c4e43bb-91f4-4eb5-b14e-cdfb4f5c0266)<br>

![image](https://github.com/user-attachments/assets/c63c0f93-6aec-4c6f-b838-f03207471225)<br>

![image](https://github.com/user-attachments/assets/8d407e61-7c88-417b-956c-de5aed2536eb)<br>

![image](https://github.com/user-attachments/assets/e4e4d703-a896-4886-811c-8570777bd1c6)<br>

![image](https://github.com/user-attachments/assets/0bd1be5b-d663-4a00-8a3e-9dca6d8dd1ef)<br>

- B3: có đc 1 bộ macro<br>

![image](https://github.com/user-attachments/assets/a5fb4a45-c283-4f49-b41f-3a52ec700274)<br>

- B4: bắt đầu brute-force<br>

![image](https://github.com/user-attachments/assets/517a0192-eed1-40ad-a21a-daf6e519e0ea)<br>

![image](https://github.com/user-attachments/assets/66a36378-e2a1-4f36-a048-2599126bfbd7)<br>

- B5: thu kết quả - lọc payload cho phản hồi 302(can phải attack vài lần)<br>




