![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/c9dd4daa-4c9e-4312-bb22-0bbd81f63b45)![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/44f82c9f-67f2-4dc1-a1e5-e26d4b0ff386)![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/19ad9390-ad7e-49b2-bb0d-e3472064a98b)<br>

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

7. Lab: Username enumeration via response timing<br>
- https://0a7b002503b466d781e78e29004f00b6.web-security-academy.net/<br>

This lab is vulnerable to username enumeration using its response times. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.<br>

Your credentials: wiener:peter<br>
Candidate usernames: https://portswigger.net/web-security/authentication/auth-lab-usernames<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>
 Hint
To add to the challenge, the lab also implements a form of IP-based brute-force protection. However, this can be easily bypassed by manipulating HTTP request headers.<br>

7.1: Giải<br>
- B1: brute-force ussername -> thông báo bị chặn login -> đc bảo vệ dựa trên IP<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/dfea7646-4c18-41ff-9f85-06ea311ebdbb)<br>

- B2: dùng X-Forwarded-For để vượt qua chặn kết hợp password dài (tầm 100 kí tự) để check time phản hồi<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/c56007ca-98a6-4f9d-8506-0699e90c5146)<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/57eb72ff-c216-4cdd-9cd1-b11fd336bac6)<br>

- B3: thu kết quả -> username: arlington<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/582fa85e-a5f7-44d5-8058-c1572e75dedb)<br>

- B4: brute-force tìm password -> password: qwertyuiop<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/22ed2acd-da0d-406a-93c4-06987438ca38)<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/f4db7663-40bf-4d08-8795-198a7286f999)<br>

8. Flawed brute-force protection<br>
8.1: Lý thuyết<br>
- bảo vệ brute-force là lm cho quá trình tự động hóa tiến trình là phức tạp nhất có thể và lm chậm tốc độ attacker thử login
- 2 cách phổ biến nhất để ngăn chặn brute-force là:<br>
+ khóa tài khoản remote user nếu họ login sai quá nh lần
+ chặn địa chỉ IP remote user nếu họ login quá nh lần liên tiếp<br>
- 2 cách có mức độ bảo vệ khác nhau nhưng nó can tránh attack đặc biệt nếu đc triển khai sử dụng login thiếu sót
- VD: địa chỉ IP bị chặn nếu login quá nh lần nhưng trong 1 số triển khai, bộ đếm số lần login này sẽ đc reset nếu bn login đúng -> trước khi đạt đến só lần bị khóa login tài khoản của attacker vào để reset số lần là đc
- -> đưa in4 login của attacker vào wordlist một cách phù hợp là vô hiệu hóa đc bảo vệ này<br>

8.2: Lab: Broken brute-force protection, IP block<br>
- https://0abd006903fb355381b97fbf003900a9.web-security-academy.net/<br>

This lab is vulnerable due to a logic flaw in its password brute-force protection. To solve the lab, brute-force the victim's password, then log in and access their account page.<br>

Your credentials: wiener:peter<br>
Victim's username: carlos<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>
 Hint
Advanced users may want to solve this lab by using a macro or the Turbo Intruder extension. However, it is possible to solve the lab without using these advanced features.<br>

8.3: Giải<r>
- B1: chặn login và chuyển sang intruder để brute-force<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/098e6ee9-dec7-42bc-a7c0-ab4ab2efdb1b)<br>

- B2: tạo payload<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/5f3c2a5a-767d-4ac5-953d-96f34a732f96)<br>

+ lặp lại > 100 lần:<br> ![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/7a1fbe51-5b24-4c49-8b77-5d36c980e1d8)<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/1be60c22-9830-40bb-bc17-7714aa0ea787)<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/f68112fc-2893-4022-97f8-d1fad4f9fc4b)<br>

- B3: tấn công và thu kết quả: password: mustang<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/778784ec-608d-4a97-8bb7-cabb76b626f1)<br>

8.4: Account locking<br>
- một số cách để ngăn chặn brute-force mà web xài là khóa acc khi đáp ứng 1 số tiêu chí đáng ngờ nhất định (thường là số lần login sai)
- cx như các lỗi login thông thg, phản hồi từ server cho bt acc đã bị khóa cx giúp attacker liệt kê username
- khóa acc cung cấp 1 mức độ bảo vệ nhất định khỏi brute-force đối vs 1 tài khoản cụ thể
- nó ko ngăn ngừa 1 cách thỏa đáng attack trong trh attacker chỉ cố truy cập vào bất kì acc ngẫn nhiên nào mà chúng can
- phương pháp giải quyết loại bảo vệ này:<br>
+ liệt kê list username ứng viên có khả năng hợp lệ, dựa vào liệt kê username or list username phổ biến
+ xác định 1 list rất nhỏ danh sách password mà có ít nhất 1 cái là đúng (số lượng này ko đượt vượt quá số lần login cho phép)
+ dùng tool để brute-force mật khẩu<br>
- khóa acc cx ko bảo vệ khỏi credential stuffing attacks (tấn công nhồi in4 xác thực)
- nó bao gồm lượng lớn cặp username:password bao gồm in4 login chính hãng bị đánh cắp trong các vụ vi phạm data
- Credential stuffing dựa trên thực tế nh ng xài cùng 1 username và password cho nh web -> nó can đúng cho web mục tiêu
- khóa acc ko bảo vệ khỏi credential stuffing vì mỗi username chỉ đc thử 1 lần -> đặc bt nguy hiển vì attacker can xâm phạm cùng lúc nh tài khoản chỉ bằng 1 cuộc tấn côngtự động<br>

8.4.1: Lab: Username enumeration via account lock<br>
- https://0add00f70487913d85ab3f82006c0069.web-security-academy.net/<br>

This lab is vulnerable to username enumeration. It uses account locking, but this contains a logic flaw. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.<br>

Candidate usernames: https://portswigger.net/web-security/authentication/auth-lab-usernames<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>

8.4.2: Giải<br>
- B1: Tạo payload<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/0738b953-398d-4e0c-9e70-4ff462e3df35)<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/81cefeff-21d2-47aa-b563-e4a0f37cd900)<br>

- B2: thu kết quả: username = antivirus<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/0aa34553-c2f6-4b73-895d-aebefbdc6203)<br>

- B3: brute-force password = 2000<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/3273eef0-1ea9-4f64-b47e-630a8963437d)<br>

8.5: User rate limiting<br>
- bảo vệ khỏi brute-force không qua lim tỉ lệ user, nếu thực hiện quá nh login trong 1 khoảng time ngắn -> IP bị chặn -> mở khóa bằng các cách<br>
+ tự động sau 1 khoảng time nhất định
+ thủ công bởi admin
+ thủ công bởi user sau khi hoàn tất CAPTCHA thành công<br>
- nó đc ưu tiên hơn khóa acc vì ít bị liệt kê username và tấn công dos
- nhưng ko hoàn toàn an toàn vì attacker can thao túng IP đê vượt qua bảo vệ
- do lim này dựa trên tốc độ gửi yêu cầu HTTP nên can vượt qua bằng cách đoán nh password chỉ trong 1 yêu cầu<br>

8.5.1: Lab: Broken brute-force protection, multiple credentials per request<br>
- https://0ad200fc04fa1a54811ee377005e0019.web-security-academy.net/<br>

This lab is vulnerable due to a logic flaw in its brute-force protection. To solve the lab, brute-force Carlos's password, then access his account page.<br>

Victim's username: carlos<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>

8.5.2: Giải<br>
- B1: thực hiện login bình thường -> bị chặn<br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/8fdb61e0-1252-4e60-95bc-8a9a32bc5fdf)<br>

- B2: thử đoán nhiều password trong 1 lần login <br>

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/2eef0e13-c1e9-4a8e-914b-f47ba0a8d323)<br>

- B3: copu URL và load -> login thành công

![image](https://github.com/bangngoc116/Authentication-vulnerabilities/assets/127403046/6ef129f3-48b2-4533-9822-22d7b46f1853)<br>

III. HTTP basic authentication<br>
1. Lý thuyết<br>
- khá cũ nhưng vì đơn giản và khá dễ triển khai nên xác thực dựa trên HTTP đôi khi vẫn đc xài
- client nhận mã xác thực từ server bằng cách nối username và password rồi mã hóa bằng Base64
- mã này đc lưu trữ và quản lý bởi browser, browser sẽ tự động thêm nó vào tiêu đề Authorization cho mọi yêu cầu tiếp theo: Authorization: Basic base64(username:password)
- đây ko đc coi là 1 phương thức xác thực an toàn vì:<br>
+ gửi in4 login của user với mọi yêu cầu
+ trừ khi web triển khai HSTS, nếu ko in4 login của user sẽ bị đánh cắp bởi a man-in-the-middle attack<br>
- nó ko đc hỗ trợ bảo vệ brute-force attack, mã chỉ bao gồm các giá trị tĩnh -> can bị brute-force
- nó dễ bị tấn công bởi biện pháp khai thác liên quan đến session đặc biệt là CSRF vì nó ko có biện pháp bảo vệ nào
- có thể cách này chỉ đưa attacker đến 1 trang web ko thú vị nhưng nó cung cấp thêm bề mặt tấn công và in4 thu thập đc can xài cho các trh khác
