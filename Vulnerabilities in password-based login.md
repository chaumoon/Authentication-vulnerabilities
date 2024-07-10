I. Lý thuyết<br>
- đối với web login bằng mật khẩu, user đăng kí 1 tài khoản or đc gán 1 acc bởi admin
- acc gồm: 1 username đơn nhất và 1 password bí mật
- -> biết password -> xác thực danh tính ng dùng
- kẻ tấn công can tấn công bằng brute-force or lỗi trong việc bảo vệ brute-force để lấy or đoán password của tài khoản<br>

II. Brute-force attacks<br>
1. Lý thuyết<br>
- là cuộc tấn công và attacker dùng hệ thống thử và sai để đoán in4 login hợp lệ của user
- thg tự động hóa bằng cách xài list username và password với công cụ chuyên dùng để thực hiện số lượng lớn lần login với tốc độ cao
- ko phải lúc nào cx là đoán 1 cách hoàn toàn ngẫu nhiên, dựa vào logic cơ bản or in4 có sẵn công khai -> attacker tinh chỉnh tấn công brute-force để đưa ra phỏng đoán có cơ sở hơn
- web dùng xác thực ng dùng bằng password là phương pháp duy nhất can rất dễ bị tấn công nếu ko triển khai biện pháp bảo vệ brute-force mạnh mẽ<br>

2. Brute-forcing usernames<br>
- username đặc biệt dễ đoán vì thường dùng mẫu dễ nhận biết như email
- kể cả ko có mẫu rõ ràng thì thỉnh thoảng kể cả các acc đặc quyền cao cx có thể xài username dễ đoán như admin or adminitrator
- trong quá trình kiểm tra, check xem web có tiết lộ tên username tiềm năng ko
- VD: can truy cập vào profile mà ko cần login ko, thậm chí nội dung thực tế của profile là ẩn thì tên trong profile cx can là username
- nên check phản hồi HTTP xem có địa chỉ email nào bị lộ ko, đôi khi nó chứa email của acc đặc quyền cao như admin or IT support<br>

3. Brute-forcing passwords<br>
- brute-force password độ khó dựa trên độ mạnh của password
- một số web xài 1 số dạng chính sách password bắt buộc user phải tạo password có high-entropy -> khó để bẻ khóa chỉ bằng brute-force
- VD: số kí tự min, kết hợp hoa thường, xài kí tự đặc biệt
- trong khi password có high-entropy khó để crack chỉ bằng mt thì c.ta can xài kiến thức cơ bản về hành vi ng dùng để khai thác vul mà user vô tình đưa vào hệ thống
- nếu chính sách yêu cầu user thay đổi password thg xuyên thì cx chỉ có thay đổi nhỏ đối vs password ưa thích của họ
- -> brute-force phức tạp hơn nh và do đó hiệu quả hơn so với việc chỉ lặp lại mọi bộ tổ hợp kí tự<br>

4. Username enumeration (bảng liệt kê username)<br>
- là khi attacker can quan sát sự thay đổi trong hành vi của web để xác định username hợp lệ hay ko
- xảy ra trên trang login (nhập username hợp lệ nma sai password) or tren form đăng kí (nhập username đã đc dùng)
- -> giảm time và công sức brute-force vì đã tạo được 1 list các username hợp lệ
- trong khi brute-force nên đặc biệt chú ý sự khác biệt trong:<br>
+ status code: thường giống nhau đối vs đa số phán đoán, nếu HTTP trả về status code khác -> username chắc chắn đúng -> web cần trả về status code giống nhau bất kể kết quả ra sao nhưng ko phải lúc nào điều này cx đc thực hiện
+ Error messages: đôi khi nó tra về khác nhau nếu cả username và password sai hay chỉ password sai -> web cần xài thông báo chung, giống nhau cho cả 2 trh nhưng đôi khi vẫn có lỗi đánh máy khiến 2 thông báo khác nhau
+ Response times: nếu hầu hết yêu cầu đc xử lí với time tg tự nhau -> nếu có điều gì khác xảy ra tức là đã có dự khác biệt -> đoán đc username là đúng. VD web chỉ check password nếu username là hợp lệ -> khiến tăng nhẹ time xử lý nhưng attacker can tăng độ trễ băng cách nhập password quá dài để tăng thời gian xử lý<br>

5. Lab: Username enumeration via different responses<br>
- https://0a1b00dc03f4f07580c21c5000460094.web-security-academy.net/<br>

This lab is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:<br>

Candidate usernames: https://portswigger.net/web-security/authentication/auth-lab-usernames<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>
To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.<br>

5.1. Giải<br>
- B1: nhập bừa username và password và chặn truy cập<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/22218d3b-2a4c-4e66-9036-43e914c23c43)<br>

- B2: chuyển sang intruder và tạo payload bằng list đã cho<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/f159dd72-a526-4494-810d-48fe7d05e952)<br>

- B3: tấn công và thu kết quả: username: americas; password: 123456 -> sai -> mỗi username đúng -> check password<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/8f0bb3da-8eba-4b5a-89af-d81a4b3f7087)<br>

- B4: check password -> password: jennifer<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/7869ca43-715e-45bf-b77d-2717480d7f4d)<br>

6. Lab: Username enumeration via subtly different responses<br>
- https://0a16008f04a52a7f807beebf00dc001a.web-security-academy.net/<br>

This lab is subtly vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:<br>

Candidate usernames: https://portswigger.net/web-security/authentication/auth-lab-usernames<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>
To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.<br>

6.1: Giải<br>
- B1: ném vào intruder để brute-force<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/38fcefc6-d3ce-4332-9054-55a41ee7d375)<br>

- B2: dùng list đã cho để tạo payload<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/2f1e24b2-a744-46dc-b6a9-97144832193f)
<br>

- B3: thu kết quả: username: americas -> lỗi đánh thiếu dấu '.'<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/598dea80-d99f-486a-aef1-0bd5ee356e11)<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/ba1675f1-1454-44f6-a7fa-ade491696e96)<br>

- B4: thu kết quả: password: 1qaz2wsx

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/245a5cda-04d3-4ef7-8ff9-4c85480f56d0)<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/177575b0-bcd9-4536-ab3b-875bb79c0e7f)<br>

7. 
