# Thiết Kế và Triển Khai Hệ Thống Tường Lửa Thế Hệ Mới (NGFW) – Palo Alto

Triển khai hệ thống bảo mật mạng doanh nghiệp sử dụng Palo Alto Firewall (PA-VM), cấu hình HA, phân vùng bảo mật và các chính sách kiểm soát lưu lượng toàn diện.

---

## Kiến Trúc Mạng

- 2 thiết bị Palo Alto PA-VM chạy HA (Active/Passive)
- Phân vùng: `Outside` (WAN) · `Inside` (LAN) · `DMZ` · `VPN`
- VLAN 10 – Server nội bộ (192.168.10.0/24)
- VLAN 20 – DMZ (192.168.20.0/24)
- VLAN 30 – LAN HCM (192.168.30.0/24)
- VLAN 40 – LAN Hà Nội (192.168.40.0/24)

---

## Các Tính Năng Đã Triển Khai

**Routing & NAT** – Cấu hình interface Layer 3, DHCP server cho từng zone, NAT động cho phép mạng nội bộ ra Internet.

**Security Policy** – Quy tắc allow/deny theo zone nguồn/đích, áp dụng profile bảo mật kèm theo từng policy.

**IPS (Zone Protection Profile)** – Bảo vệ zone khỏi TCP Flood, UDP Flood, ICMP Flood, port scan. Thử nghiệm với nmap xác nhận hệ thống phát hiện và chặn thành công.

**SSL Decryption** – Giải mã lưu lượng HTTPS (Forward Proxy) bằng chứng chỉ nội bộ, cho phép các tính năng bảo mật kiểm tra được traffic mã hóa.

**File & Malware Blocking** – Chặn tải lên/xuống file thực thi nguy hiểm (.exe), tích hợp Antivirus profile quét mã độc. Thử nghiệm với file EICAR xác nhận chặn thành công.

**URL Filtering** – Tạo danh sách đen (blacklist) theo URL, áp dụng vào Security Policy với action Deny. Xác nhận chặn Facebook, YouTube trong khi các web khác vẫn truy cập bình thường.

**Application Control (App-ID)** – Nhận diện và chặn ứng dụng theo danh tính thực (Facebook, YouTube, Gmail) bất kể cổng hay giao thức sử dụng.

**Remote Access VPN** – Cấu hình GlobalProtect cho phép nhân viên từ xa kết nối vào mạng nội bộ, NAT và Security Policy riêng cho zone VPN → Inside.

**Site-to-Site VPN (IPsec IKEv2)** – Tunnel giữa PA1 (HCM, 192.168.10.0/24) và PA2 (Hà Nội, 192.168.40.0/24), mã hóa AES-128-CBC / SHA1. Ping thông hai chiều xác nhận tunnel hoạt động.
