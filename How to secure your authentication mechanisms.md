I. Lý thuyết<br>
- xác thực là 1 chủ đề phức tạp và rất dễ để điểm yếu hay sai sót xảy ra
- có 1 số nguyên tắt chung phải luôn tuân theo để bảo vệ web<br>

II. Take care with user credentials<br>
- ngay cả cơ chế xác thực mạnh nhất cx ko hiệu quả nếu vô tình tiết lộ bộ in4 login hợp lệ cho attacker
- ko nên gửi bất kì data login nào qua kết nối ko mã hóa
- mặc dù đã triển khai HTTPS cho các yêu cầu login nhưng phải đảm bảo điều này được thực thi bằng cách chuyển hướng all yêu cầu HTTP sang HTTPS
- web phải đảm bảo ko có username or địa chỉ email nào bị tiết lộ thông qua profile đc truy cập công khai or được phản ánh trong phản hồi HTTP<br>

III. Don't count on users for security<br>
- một số biện pháp bảo mật nghiêm ngặt yêu cầu thêm ở user nhưng có 1 số user sẽ gây ra lỗi -> cần thực hiện hành vi an toàn bất cứ khi nào có thể
- thực hiện chính sách password hiệu quả: ko xài password dễ đoán
- triển khai 1 loại trình check password đơn giản nào đó,  cho phép user thử nghiệm và cung cấp feedback về độ mạnh password trong time thực
- VD: thư viện Javascript: zxcvbn
- bằng cách chỉ cho phép các password đc trình check password đánh giá cao -> thực thi password an toàn hiệu quả hơn chính sách truyền thống<br>

IV. Prevent username enumeration<br>
- attacker dễ phá vỡ cơ chế xác thực nếu web có tiết lộ rằng user tồn tại trên hệ thống
- có 1 số web nhất định, việc bt 1 ng cụ thể có acc đã là in4 nhạy cảm
- bất kể username đã thử hợp lệ ko -> phải xài thông báo lỗi chung, giống nhau, trả về status code giống nhau và lm cho time phản hồi trong các tình huống khác nhau càng ko phân bt đc càng tốt<br>

V. Implement robust brute-force protection<br>
- do xây dựng brute-force attack đơn giản -> cần phải thực hiện các bc để ngăn chặn or ít nhất lm gián đoạn mọi brute-force login
- một cách hiệu quả: lim tỉ lệ user dựa trên IP (bao gồm biện pháp ngăn chặn attacker thao túng địa chỉ IP của họ) -> yêu cầu user lm test CAPTCHA với mỗi login sau khi đạt đến 1 lim nhất định
- việc khiến quá trình tẻ nhạt và thủ công nhất có thể -> tăng khả năng attacker bỏ cuộc và tìm kiếm mục tiêu nhẹ nhàng hơn<br>

VI. Triple-check your verification logic<br>
- các lỗi logic cơ bản rất dễ xâm nhập vào code và trong trh xác thực nó can lm tổng hại hoàn toàn web và user của bn
- -> cần check kĩ lưỡng mọi logic xác thực và xác minh để loại bỏ sai sót<br>

VII. Don't forget supplementary functionality (c.năng bổ sung)<br>
- đảm bảo ko chỉ tập trung vào các trang login trung tâm mà bỏ qua các chức năng bro sung liên quan đến xác thực
- đặc bt quan trọng trong trh attacker can tự do đăng kí acc và khai thác các chức năng này
- việc reset or thay đổi password cx là bề mặt tấn công hợp lệ như cơ chế login chính của nó -> cần bảo vệ như nhau<br>

VIII. Implement proper multi-factor authentication<br>
- xác thực đa yếu tố can ko thực tế vs mọi web nhưng nếu đc triển khai đúng cách nó sẽ an toàn hơn nh so vs login chỉ dựa vào password
- gửi mã xác minh qua email về cơ bản chỉ là 1 hình thức của xác thực đơn yếu tố dài dòng hơn
- 2FA dựa trên SMS (có và bt) là kĩ thuật xác thực 2 yêu tố -> hoán đổi SIM -> ko đáng tin cậy
- 2FA phải đc triển khai xài thiết bị or app chuyên dụng để tạo mã xác minh trực tiếp (chúng đc xây dựng nhằm aim bảo mật -> an toàn hơn)
