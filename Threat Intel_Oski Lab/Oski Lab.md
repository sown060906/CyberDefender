Category: [Threat Intel](https://cyberdefenders.org/blueteam-ctf-challenges/?categories=threat-intel)

# Scenario 

The accountant at the company received an email titled "Urgent New Order" from a client late in the afternoon. When he attempted to access the attached invoice, he discovered it contained false order information. Subsequently, the SIEM solution generated an alert regarding downloading a potentially malicious file. Upon initial investigation, it was found that the PPT file might be responsible for this download. Could you please conduct a detailed examination of this file?

Tool: VirusTotal,ANY.RUN
File txt chứa hash của file độc hại và dùng online threat intel platform như VirusTotal, Hybrid Analysis để làm.
  
Hash: `a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb`

Unzip the file with password:`cyberdefenders.org`
# Q1:
<img width="1006" height="217" alt="image" src="https://github.com/user-attachments/assets/665fc7fc-9438-4ad3-afee-07ae87034885" />

- Đề hỏi thời điểm malware được tạo ra,check details trên [VirusTotal](https://www.virustotal.com/gui/file/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb/details) ta được đáp án: `2022-09-28 17:40`
  <img width="1512" height="834" alt="image" src="https://github.com/user-attachments/assets/d0a05659-247d-4275-bd02-3cfcf6a5313c" />
# Q2:
<img width="1012" height="202" alt="image" src="https://github.com/user-attachments/assets/1bd51d71-6d2f-4e88-8241-9633a1ad398c" />

- Đề yêu cầu tìm máy chủ C2 (command and control) giao tiếp với file độc hại,check [Relation](https://www.virustotal.com/gui/file/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb/relations) ta có đáp án: `http://171.22.28.221/5c06c05b7b34e8e6.php `

# Q3:
<img width="1009" height="219" alt="image" src="https://github.com/user-attachments/assets/22e9e3fd-0a2a-41c7-85bc-8135f0d2bd7e" />

- Đề yêu cầu tìm file thư viện đầu tiên mà mã độc yêu cầu, check [Behavior](https://www.virustotal.com/gui/file/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb/behavior) dựa vào network communication và files dropped ta biết rằng file thư viện được yêu cầu là `sqlite3.dll`
  <img width="1793" height="269" alt="image" src="https://github.com/user-attachments/assets/a3f34076-4df9-4f7a-8b3f-7f370307b498" />
  <img width="1754" height="248" alt="image" src="https://github.com/user-attachments/assets/bcebb369-3ccf-4547-bf07-dae2d3ea8d02" />
# Q4:
<img width="998" height="208" alt="image" src="https://github.com/user-attachments/assets/dc72c005-33e1-469d-8630-67a246132e4d" />

- Kiểm tra báo cáo [Any](https://any.run/report/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb/d55e2294-5377-4a45-b393-f5a8b20f7d44#Behavior).run được cung cấp, khóa RC4 nào được mã độc sử dụng để giải mã chuỗi được mã hóa base64 của nó
  <img width="1428" height="356" alt="image" src="https://github.com/user-attachments/assets/a3c8f5a6-29c4-44a4-a65e-f638a783a05a" />
- Check Malware configuration ta có đáp án là: `5329514621441247975720749009`
# Q5:
<img width="1004" height="205" alt="image" src="https://github.com/user-attachments/assets/56119127-193a-480a-9f23-6342a73d4837" />

- Đề yêu cầu xem xét các kỹ thuật MITRE ATT&CK được hiển thị trong báo cáo [sandbox Any.run](https://app.any.run/tasks/d55e2294-5377-4a45-b393-f5a8b20f7d44), hãy xác định kỹ thuật MITRE chính mà mã độc sử dụng để đánh cắp mật khẩu người dùng.
<img width="627" height="393" alt="image" src="https://github.com/user-attachments/assets/2a90b1c8-71ba-4e12-809b-bc21e2225176" />

- Trong process details, chúng ta có thể thấy mã độc sử dụng kỹ thuật: `T1555` 'Steals credentials from Web Browsers'. Kỹ thuật này liên quan đến việc kẻ tấn công nhắm vào các vị trí lưu trữ mật khẩu phổ biến, chẳng hạn như trình duyệt web, để đánh cắp thông tin xác thực. Bằng cách trích xuất thông tin xác thực được lưu trữ trong trình duyệt hoặc các công cụ quản lý mật khẩu, kẻ tấn công có thể truy cập vào các tài khoản và hệ thống nhạy cảm của người dùng. 
# Q6:
<img width="1014" height="208" alt="image" src="https://github.com/user-attachments/assets/fbad0228-b257-417a-b676-5fb61ae01392" />

- Đề yêu cầu xem xét các kỹ thuật MITRE ATT&CK được hiển thị trong báo cáo [sandbox Any.run](https://app.any.run/tasks/d55e2294-5377-4a45-b393-f5a8b20f7d44),thư mục nào bị mã độc nhắm đến để xoá tất cả các file dll
  <img width="1012" height="410" alt="image" src="https://github.com/user-attachments/assets/8067b501-10ac-4e4c-a1c0-ff65fa9cf578" />

- Nhìn vào process details của tiến trình con cmd.exe ta thấy ``` "C:\Windows\system32\cmd.exe" /c timeout /t 5 & del /f /q "C:\Users\admin\AppData\Local\Temp\VPN.exe" & del "C:\ProgramData\*.dll"" & exit ``` nó sẽ chờ 5 giây sau đó xóa mã độc cùng với bất kỳ DLL liên quan bên trong `C:\ProgramData` nào có thể đã được tải trong quá trình thực thi.
- Đây là một kỹ thuật tự xóa (self-deletion) thường thấy ở malware để:
  - Che giấu bằng chứng - Xóa dấu vết sau khi đã hoàn thành mục đích
  - Tránh bị phát hiện - Không để lại file độc hại trên máy
  - Khó phân tích - Làm khó khi không còn file gốc
# Q7:
<img width="1011" height="233" alt="image" src="https://github.com/user-attachments/assets/1a56ed8a-12e8-4054-80bb-b32fda09ec31" />

- Hiểu được hành vi của mã độc sau khi đánh cắp dữ liệu có thể cung cấp cái nhìn sâu sắc về kỹ thuật tàng hình của nó.
Bằng cách phân tích các tiến trình con, sau khi đánh cắp dữ liệu người dùng thành công, mã độc mất bao nhiêu giây để tự xóa?
- Nhìn vào câu trước ta biết rằng sau `5` giây mã độc sẽ tự xoá 


  

   











  

  
  


