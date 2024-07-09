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
- web dùng xác thực ng dùng bằng password là phương pháp duy nhất can rất dễ bị tấn công nếu ko triển khai biện pháp bảo vệ brute-force mạnh mẽ
