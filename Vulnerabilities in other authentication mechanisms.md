I. Lý thuyết<br>
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
- phương pháp này can đc xài để vượt qua lim số lần login nếu chúng ko đc xài cho việc đoán cookie<br>

2. Lab: Brute-forcing a stay-logged-in cookie<br>
This lab allows users to stay logged in even after they close their browser session. The cookie used to provide this functionality is vulnerable to brute-forcing.

To solve the lab, brute-force Carlos's cookie to gain access to his "My account" page.<br>

Your credentials: wiener:peter<br>
Victim's username: carlos<br>
Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords<br>

3. Giải<br>
- B1: 
