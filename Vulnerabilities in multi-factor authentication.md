I. Lý thuyết<br>
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

