Category: [Network Forensics](https://cyberdefenders.org/blueteam-ctf-challenges/?categories=network-forensics)

# Scenario:
A suspicious file was identified on a company web server, raising alarms within the intranet. The Development team flagged the anomaly, suspecting potential malicious activity. To address the issue, the network team captured critical network traffic and prepared a PCAP file for review.
Your task is to analyze the provided PCAP file to uncover how the file appeared and determine the extent of any unauthorized activity.

Tool: Wireshark 
# Q1:
<img width="996" height="285" alt="image" src="https://github.com/user-attachments/assets/2a339614-f4ba-4420-a032-daef89197991" />

### Solution:
- Đề yêu cầu tìm xem cuộc tấn công xuất phát từ thành phố nào.
- Sau khi mở Wiresshark ta thấy rằng source IP là 117.11.88.124
 <img width="1862" height="728" alt="image-1" src="https://github.com/user-attachments/assets/96f3df58-e938-4a45-b441-6835ed9039f2" />

- Sử dụng whatismyipaddress để tra cứu địa chỉ IP này, ta thấy rằng nó xuất phát từ Tianjin,Trung Quốc.
<img width="1591" height="907" alt="image-2" src="https://github.com/user-attachments/assets/adc79d3e-1b90-42a2-94a8-76cb298bbe64" />

# Q2: 
<img width="1007" height="193" alt="image-3" src="https://github.com/user-attachments/assets/a1bc1fba-e8af-4bef-b9f8-ec05d520e1b3" />

### Solution:
- Đề yêu cầu tìm Full User-Agent của attacker.
<img width="1863" height="811" alt="image5" src="https://github.com/user-attachments/assets/59b1b0ed-7d37-4d0c-a8d8-65ee58fef0ea" />

 - Khi follow http stream, ta thấy được full user-agent của attacker là: User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0 
# Q3:
<img width="1001" height="213" alt="image-4" src="https://github.com/user-attachments/assets/888cf8e1-1bf4-41e4-91e0-aea6ab7744bb" />
  
### Solution:
- Đề yêu cầu tìm file độc hại mà attacker đã tải lên thành công .
- Ta có thể tìm các file được tải lên bằng cách lọc theo http.request.method == "POST"
<img width="1727" height="851" alt="image-6" src="https://github.com/user-attachments/assets/abc0f9e4-8c49-4cfe-8730-c7c1ec55efa5" />

- Sau khi và check các gói tin ta biết rằng attacker đã tải lên thành công file image.jpg.php content là một file độc hại và trước đó đã tải file image.php với cùng nội dung nhưng đã bị lỗi invalid file format.
<img width="1679" height="847" alt="image-5" src="https://github.com/user-attachments/assets/e6be93ed-5ee6-4709-8e2d-3dcd00fc73a4" />

# Q4:
<img width="995" height="208" alt="image-7" src="https://github.com/user-attachments/assets/d06a0aa5-77d8-47f2-a618-158b9426c9a5" />

### Solution:
- Đề yêu cầu tìm xem thư mục nào được trang web sử dụng để lưu trữ file độc hại.
- Ta có thể tìm được thư mục này bằng cách lọc và tìm trên packet details của gói tin có chứa file độc hại image.jpg.php 
 <img width="1817" height="818" alt="image-8" src="https://github.com/user-attachments/assets/c5698f13-7ec4-48f6-a946-caeacaff39ce" />

# Q5:
<img width="1013" height="217" alt="image-9" src="https://github.com/user-attachments/assets/2d1a6a96-9dd1-4ff0-9621-edab86a67ad6" />

### Solution:
- Đề yêu cầu tìm xem port nào đang được mở trên máy chủ của attcker và được đánh dấu bởi webshell độc hại để có thể truy cập từ xa trái phép.
- Nhìn vào nội dung của file độc hại image.jpg.php ta thấy rằng nó có chứa một webshell đơn giản và có thể truy cập từ xa thông qua cổng 8080.
<img width="930" height="838" alt="image-11" src="https://github.com/user-attachments/assets/20650b85-dc89-4d40-8a90-00aecc5826b0" />

# Q6: 
<img width="1007" height="211" alt="image-10" src="https://github.com/user-attachments/assets/90885700-5769-44c1-b9cc-59546774f3be" />

### Solution:
- Đề yêu cầu xác định xem attacker đang cố gắng để đánh cắp.
- Biết rằng attacker dùng nc để kết nối đến máy chủ của mình thông qua cổng 8080, ta có thể theo dõi các gói tin có liên quan đến cổng này để xem attacker đang cố gắng đánh cắp gì bằng cách lọc theo tcp.port == 8080(hoặc updp.port == 8080 nếu dùng udp) và theo dõi tcp stream để xem nội dung của các gói tin này.
<img width="886" height="673" alt="image-12" src="https://github.com/user-attachments/assets/271a7bdc-8abe-4285-b461-38e40dfcf776" />

<img width="920" height="526" alt="image-14" src="https://github.com/user-attachments/assets/2e77b3bc-df00-4cbb-b33c-643d601d63d5" />

- Nhìn vào nội dung của các gói tin này ta thấy rằng attacker đang cố gắng đánh cắp thông tin đăng nhập của người dùng trên máy chủ bằng cách sử dụng webshell để thực thi các lệnh và truy cập vào các file passwd chứa thông tin đăng nhập.
# (IOCs)(Dấu hiệu xâm nhập):
| Type | Indicator | Description |
|------|-----------|-------------|
| Attacker IPv4 | 117.11.88.124 |   Tianjin, China. |
| Victim IPv4 | 24.49.63.79 | Compromised Web Server |
| Malicious File | image.jpg.php | file php được nguỵ trang thành JPEG file image. |
| User-Agent | User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0  | Kali Linux default browsers. |
