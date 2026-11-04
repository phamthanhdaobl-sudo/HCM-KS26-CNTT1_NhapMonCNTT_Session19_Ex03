# HCM-KS26-CNTT1_NhapMonCNTT_Session19_BTVN3
Anh Đạo ơi, bài Session 18 – Exercise 03 này yêu cầu mình xây dựng một quy trình ứng phó khi máy tính bị nhiễm Infostealer và thông tin đăng nhập bị đánh cắp.

Điểm quan trọng nhất của bài là phải hiểu: Đổi mật khẩu thôi chưa đủ. Phải thu hồi cả những phiên đăng nhập và Token đã bị lộ.

Em sẽ soạn bài theo dạng báo cáo hoàn chỉnh để anh đưa lên GitHub, có đủ IPO, quy trình 4 bước, hướng dẫn thao tác thực tế trên GitHub và cấu trúc thư mục nộp bài.

# BÀI 3 – KHẮC PHỤC SỰ CỐ RÒ RỈ THÔNG TIN ĐĂNG NHẬP

Môn học: Nhập môn CNTT · Session 18 · Exercise 03

Thông tin sinh viên

- Họ và tên: Phạm Thành Đạo
- Lớp: HCM-KS26-CNTT1
- Chủ đề: Incident Response – Infostealer

## 1. Phân tích IPO (Input – Process – Output)

### 1.1. Input – Dữ liệu đầu vào

Máy tính xuất hiện những dấu hiệu bất thường, nghi ngờ bị nhiễm mã độc Infostealer.

Các dấu hiệu nhận biết:

- Tài khoản Facebook, Gmail liên tục bị đăng xuất hoặc có thông báo đăng nhập từ thiết bị lạ.
- Xuất hiện hoạt động bất thường như gửi tin nhắn, thay đổi thông tin tài khoản mà người dùng không thực hiện.
- Mật khẩu đã thay đổi nhưng kẻ tấn công vẫn có thể truy cập tài khoản.
- Trình duyệt có thể đã lưu mật khẩu, Cookie hoặc thông tin phiên đăng nhập bị đánh cắp.
- Có nguy cơ lộ GitHub Personal Access Token (PAT), SSH Key hoặc thông tin xác thực khác.

### 1.2. Process – Quy trình xử lý

Thực hiện quy trình cấp cứu gồm 4 bước:

| Bước | Công việc                                    | Mục đích                                      |
| ---- | -------------------------------------------- | --------------------------------------------- |
| 1    | Ngắt kết nối Internet                        | Cô lập máy tính bị nghi nhiễm                 |
| 2    | Sử dụng thiết bị sạch đổi mật khẩu           | Ngăn kẻ tấn công tiếp tục sử dụng mật khẩu cũ |
| 3    | Revoke Token và thu hồi phiên đăng nhập      | Vô hiệu hóa quyền truy cập đã bị đánh cắp     |
| 4    | Quét mã độc, làm sạch và bảo vệ lại hệ thống | Khôi phục môi trường an toàn                  |

### 1.3. Output – Kết quả đầu ra

Sau khi hoàn thành quy trình:

- Máy tính đã được kiểm tra và xử lý mã độc.
- Các mật khẩu bị lộ đã được thay đổi.
- Những phiên đăng nhập đáng ngờ đã bị đăng xuất.
- Token, API Key và SSH Key không còn an toàn đã được thu hồi hoặc thay thế.
- Tài khoản được kích hoạt xác thực hai yếu tố (2FA).
- Hệ thống được khôi phục và có thể tiếp tục sử dụng an toàn.

## 2. Triển khai quy trình cấp cứu 4 bước

### Bước 1. Ngắt kết nối Internet (Isolation)

Ưu tiên thực hiện ngay

Mục đích: Ngăn mã độc tiếp tục gửi dữ liệu ra bên ngoài hoặc giao tiếp với máy chủ điều khiển của kẻ tấn công.

Cách thực hiện:

1. Ngắt kết nối Wi-Fi trên máy tính.
2. Rút dây mạng LAN nếu đang sử dụng.
3. Không tiếp tục đăng nhập vào Facebook, Gmail, GitHub trên máy tính nghi nhiễm.
4. Không cắm USB hoặc thiết bị lưu trữ quan trọng vào máy tính.
5. Không sử dụng máy tính này để thay đổi mật khẩu.

Kết quả: Máy tính bị nghi nhiễm được cô lập khỏi Internet, hạn chế nguy cơ tiếp tục rò rỉ thông tin.

### Bước 2. Đổi mật khẩu từ thiết bị khác (Password Reset)

Mục đích: Ngăn kẻ tấn công tiếp tục sử dụng thông tin đăng nhập đã đánh cắp.

Cách thực hiện:

1. Sử dụng một thiết bị sạch khác, chẳng hạn điện thoại hoặc máy tính đã được kiểm tra an toàn.
2. Truy cập trang web chính thức của Gmail, Facebook và GitHub.
3. Đăng nhập và thay đổi mật khẩu của từng tài khoản.
4. Sử dụng mật khẩu mới, dài, khó đoán và không trùng lặp giữa các tài khoản.
5. Kiểm tra địa chỉ email, số điện thoại khôi phục và các phương thức xác thực.
6. Đăng xuất các thiết bị hoặc phiên đăng nhập không nhận diện được.

Lưu ý: Nếu email chính bị chiếm quyền, cần ưu tiên bảo vệ email và các phương thức khôi phục tài khoản. Nếu có nguy cơ bị kiểm soát cả thiết bị sạch, hãy sử dụng một thiết bị khác đáng tin cậy.

Kết quả: Mật khẩu cũ không còn được sử dụng để đăng nhập và quyền khôi phục tài khoản được kiểm tra lại.

### Bước 3. Thu hồi Token và quyền truy cập (Revoke Token)

Mục đích: Vô hiệu hóa các Token hoặc thông tin xác thực đã bị đánh cắp.

Infostealer không chỉ lấy mật khẩu mà còn có thể đánh cắp Cookie, Session Token, Personal Access Token và SSH Key.

Đặc biệt, đối với GitHub, cần kiểm tra và thu hồi những thông tin xác thực không còn an toàn.

Các loại thông tin cần kiểm tra:

- Personal Access Tokens (PAT).
- SSH Keys.
- GitHub Apps và OAuth Apps đã cấp quyền.
- Các phiên đăng nhập đang hoạt động.

Thực hiện trên GitHub:

1. Đăng nhập GitHub bằng thiết bị sạch.
2. Nhấn ảnh đại diện ở góc trên bên phải.
3. Chọn Settings.
4. Chọn Developer settings.
5. Mở Personal access tokens.
6. Kiểm tra các Token đã tạo.
7. Chọn Token nghi bị lộ và thực hiện Delete/Revoke.
8. Kiểm tra tiếp SSH Keys và các ứng dụng đã được cấp quyền.
9. Xóa hoặc thu hồi những quyền truy cập không nhận diện được.

Kết quả: Token bị lộ được thu hồi, ngăn việc tiếp tục sử dụng Token cũ để truy cập tài nguyên GitHub.

Lưu ý: Xóa Token không tự động xóa mọi phiên đăng nhập trên GitHub. Cần kiểm tra riêng mục Sessions và thu hồi các phiên đáng ngờ.

### Bước 4. Quét mã độc và khôi phục hệ thống (Recovery)

Mục đích: Loại bỏ nguyên nhân gây rò rỉ thông tin và đưa máy tính về trạng thái an toàn.

Cách thực hiện:

1. Sử dụng phần mềm bảo mật đáng tin cậy để quét toàn bộ hệ thống.
2. Kiểm tra các chương trình khởi động cùng Windows.
3. Gỡ bỏ phần mềm, tiện ích trình duyệt đáng ngờ.
4. Cập nhật hệ điều hành và trình duyệt lên phiên bản mới nhất.
5. Nếu xác nhận máy tính bị nhiễm Infostealer, cân nhắc cài đặt lại hệ điều hành từ nguồn chính thức để đảm bảo sạch hoàn toàn.
6. Bật xác thực hai yếu tố (2FA) cho Gmail, Facebook và GitHub.
7. Chỉ đăng nhập lại sau khi hệ thống được làm sạch và kiểm tra.

Kết quả: Máy tính được khôi phục, tài khoản được bảo vệ bằng các phương thức xác thực an toàn hơn.

## 3. Hướng dẫn chi tiết thao tác bảo mật trên GitHub

Đây là phần quan trọng để chứng minh anh hiểu và có thể triển khai thực tế.

### 3.1. Thu hồi Personal Access Token (Revoke Token)

[Setup Version Control | Fluxscape Docs](https://images.openai.com/static-rsc-4/woVyA-HjsWy76I63b_2UFW2b7d4fuaMdYO40Mua8dI8zm0eGfgSLyhQAy3JQpKCV3kDCIWGv-dhU1VVTucg-PRZARkFwHVb3urHUPpUYeYYUyuRNiTbBPnR7GvpYWlRzF03the4r3uxHgLetK23-wD2siGNjxb5tJ3iG3jpEuss?purpose=inline)

[GitHub Actions using AWS and OpenID](https://images.openai.com/static-rsc-4/AZi36s4nKdq0Cz3sWTaLFhzyrgxkRTL9OpsTMa4l3XyVN8nBWp4w-Z5azKV2smHrwLSDOhuxs4bOtqP9PHqaNksAANXOZwHsYvNzdAdIJCJJl0LBFIkMWPqmX7NqGnQfnh8FTsC-zpAlZzPpqXWalsv0JPsVb3SRzuGW8ARHn4Q?purpose=inline)

[GitHub Fine-grained Personal Access Tokens – S4 Help](https://images.openai.com/static-rsc-4/YaRtqaFTFsJx_uRgZgkaCaSx19HIapYgBj32JnMJhud8oBUE0hO2ZnVBssbH1Xk38_8nIWD-vlFIqEs7H-hoYY7RwMnpAQyGZj2ROI4BFDUCfpnAuRBMZ_5tvDBtPtxtFYuE-kp77EGlaf-UM6n9NN4-xnJsgE9F-BmAOKiehlk?purpose=inline)

4

Các bước thực hiện:

1. Truy cập [GitHub](https://github.com?utm_source=chatgpt.com).
2. Nhấn ảnh đại diện → Settings.
3. Chọn Developer settings ở cuối thanh điều hướng.
4. Chọn Personal access tokens.
5. Chọn Fine-grained tokens hoặc Tokens (classic).
6. Xem danh sách Token đang tồn tại.
7. Chọn Token nghi ngờ bị lộ.
8. Nhấn Delete hoặc Revoke để thu hồi.

Giải thích:

- Token là một dạng thông tin xác thực cho phép ứng dụng hoặc người dùng truy cập GitHub theo quyền được cấp.
- Khi Token bị lộ, người khác có thể sử dụng nó để truy cập những tài nguyên được cấp phép.
- Revoke Token sẽ vô hiệu hóa Token đó.
- Sau khi thu hồi, nếu ứng dụng vẫn cần truy cập GitHub thì phải tạo Token mới và cấp quyền tối thiểu cần thiết.



Lưu ý

&#x20;Không đăng Token mới lên GitHub Repository, README hoặc gửi Token cho người khác.



### 3.2. Thu hồi SSH Key

[SSH + Git: What, Why, How, and Where? | by Rajdips | Medium](https://images.openai.com/static-rsc-4/hSnVUxZkoqmD_WnsUpPUfTSPmVI8n7IYAk7bUoEogl4yN6I-DS7AdOPv5mkZM5gomba3yfSTVeGRGqga6UiCdpp3Pec8L5lJA9p9eyRIk_LZJwhoUdSlHD-zWrI1UTCjchCsHJmNUw7CPff64TYIpO2YxUV5QnjAZNkjAx0x4vg?purpose=inline)

[medium.com](https://medium.com/%40rajdips834/ssh-git-what-why-how-and-where-d115537645a8?utm_source=chatgpt.com)

Thực hiện:

1. Mở GitHub → Settings.
2. Chọn SSH and GPG keys.
3. Kiểm tra danh sách SSH Keys.
4. Xác định Key không nhận diện được hoặc có nguy cơ bị lộ.
5. Nhấn Delete để xóa Key.
6. Tạo SSH Key mới trên thiết bị sạch nếu vẫn cần sử dụng SSH.

### 3.3. Thu hồi phiên đăng nhập (Session)

[Log out of Github website on a different device - Stack Overflow](https://images.openai.com/static-rsc-4/p-yiruZW7AdEpJfWZQbpHxop0q-2qFySHqdYAe8F93616W7LwLGjlPZt-Jlu_EBWGIPgNZH_8-kQ7FN_uyg-KJ6LwF2RhKqyUXKP70U5dfqwZSfADMcOFYP3eKzDS3lyk-EjNy0LyNKjkvq66rBHEgMFLJD48T2lzuNCp60x5pQ?purpose=inline)

[stackoverflow.com](https://stackoverflow.com/questions/51369478/log-out-of-github-website-on-a-different-device?utm_source=chatgpt.com)

Thực hiện:

1. Mở GitHub → Settings.
2. Chọn Sessions.
3. Kiểm tra những thiết bị và phiên đăng nhập đang hoạt động.
4. Nhấn Sign out đối với phiên đăng nhập đáng ngờ.
5. Nếu nghi ngờ toàn bộ phiên đăng nhập bị đánh cắp, đăng xuất các phiên còn lại và đăng nhập lại trên thiết bị sạch.

Ngoài ra, kiểm tra mục Applications → Authorized OAuth Apps và GitHub Apps để thu hồi những ứng dụng không đáng tin cậy.

### 3.4. Bật xác thực hai yếu tố (2FA)

[How to Secure Your GitHub Account With Two-Factor Authentication](https://images.openai.com/static-rsc-4/DdPR6KIPgIziEBMzgy6MUMdWxvA5jcz2lPD7Oxb5Nb52kkEXUzpU33_HeJW11MkcKmrAYS4F5_mZq5TJuheLSQl1tmyYVufQJZbKyuhsXwU5dDD-_13IUkr13SRkivIZy5qXBzWf3qWX_nDxnB0cj2yqH4kMthNuE31VdbNtqT4?purpose=inline)

[techrepublic.com](https://www.techrepublic.com/article/how-to-secure-github-two-factor-authentication/?utm_source=chatgpt.com)

Thực hiện:

1. Vào GitHub → Settings.
2. Chọn Password and authentication.
3. Tìm mục Two-factor authentication.
4. Chọn Enable two-factor authentication.
5. Thiết lập ứng dụng xác thực hoặc phương thức bảo mật được GitHub hỗ trợ.
6. Lưu Recovery Codes ở nơi an toàn, không lưu chung với mật khẩu.

Ý nghĩa: Ngay cả khi mật khẩu bị lộ, kẻ tấn công vẫn cần yếu tố xác thực thứ hai để đăng nhập.

## 4. Bảng kiểm tra sau khi xử lý sự cố

| Hạng mục                      | Trạng thái mong muốn |
| ----------------------------- | -------------------- |
| Ngắt Internet máy nhiễm       | Hoàn thành           |
| Đổi mật khẩu từ thiết bị sạch | Hoàn thành           |
| Thu hồi PAT bị lộ             | Hoàn thành           |
| Kiểm tra và thu hồi SSH Key   | Hoàn thành           |
| Đăng xuất phiên đáng ngờ      | Hoàn thành           |
| Kiểm tra OAuth Apps           | Hoàn thành           |
| Quét và làm sạch máy tính     | Hoàn thành           |
| Bật xác thực 2FA              | Hoàn thành           |

Đây là bảng tiêu chí cần đạt, không có nghĩa là các thao tác đã được thực hiện trên tài khoản của anh.

## 5. Kết luận

Infostealer là loại mã độc nguy hiểm vì có thể đánh cắp mật khẩu, Cookie, Token và nhiều thông tin xác thực được lưu trữ trên máy tính.

Để xử lý sự cố hiệu quả, không thể chỉ thay đổi mật khẩu mà cần thực hiện đồng thời việc cô lập thiết bị, đổi mật khẩu từ thiết bị sạch, thu hồi Token và phiên đăng nhập, sau đó làm sạch hệ thống.

Thông qua bài tập này, em hiểu được tầm quan trọng của việc bảo vệ thông tin đăng nhập, quản lý quyền truy cập và sử dụng xác thực hai yếu tố để giảm thiểu nguy cơ bị chiếm quyền tài khoản.
