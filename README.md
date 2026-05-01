# Networking Notes

## المحتوى

هذا المستودع يحتوي على ملاحظات منظمة لتعلم أساسيات الشبكات خطوة بخطوة.


---

# Main Topics

| # | File | Topic |
|---|---|---|
| 1 | [OSI Layers](./OSI-Layers.md) | شرح طبقات OSI |
| 2 | [Packet Encapsulation and Decapsulation](./Packet-Encapsulation-and-Decapsulation.md) | تغليف وفك تغليف البيانات |
| 3 | [IP Addressing and Subnetting](./IP-Addressing-and-Subnetting.md) | IP Address و Subnetting |
| 4 | [DHCP - How Devices Get IP](./DHCP-How-Devices-Get-IP.md) | كيف يحصل الجهاز على IP |
| 5 | [DNS - How Domain Names Work](./DNS-How-Domain-Names-Work.md) | كيف يعمل DNS |
| 6 | [ARP Protocol](./ARP-Protocol.md) | كيف يعرف الجهاز MAC Address |
| 7 | [Switch Layer 2](./Switch-Layer-2.md) | شرح Switch و MAC Table |
| 8 | [VLANs and Trunking](./VLANs-and-Trunking.md) | VLANs و Trunk Ports |
| 9 | [Router Layer 3](./Router-Layer-3.md) | شرح Router و Layer 3 |
| 10 | [Routing Protocols OSPF RIP BGP](./Routing-Protocols-OSPF-RIP-BGP.md) | Routing Protocols |
| 11 | [ICMP Ping and Traceroute](./ICMP-Ping-and-Traceroute.md) | Ping و Traceroute |
| 12 | [NAT and PAT](./NAT-and-PAT.md) | NAT و PAT |
| 13 | [Ports and Sockets](./Ports-and-Sockets.md) | Ports و Sockets |
| 14 | [TCP 3-Way Handshake](./TCP-3-Way-Handshake.md) | بداية اتصال TCP |
| 15 | [TCP vs UDP](./TCP-vs-UDP.md) | الفرق بين TCP و UDP |
| 16 | [TLS Handshake and Encryption](./TLS-Handshake-and-Encryption.md) | TLS و HTTPS Encryption |
| 17 | [HTTP and HTTPS Request Lifecycle](./HTTP-and-HTTPS-Request-Lifecycle.md) | رحلة فتح موقع HTTP/HTTPS |

---

# Practical and Extra Topics

| # | File | Topic |
|---|---|---|
| 18 | [Network Troubleshooting Commands](./Network-Troubleshooting-Commands.md) | أوامر تشخيص مشاكل الشبكة |
| 19 | [Networking Practical Labs](./Networking-Practical-Labs.md) | تطبيقات عملية و Labs |
| 20 | [Wireshark Basics](./Wireshark-Basics.md) | أساسيات Wireshark وتحليل Packets |
| 21 | [Common Networking Mistakes](./Common-Networking-Mistakes.md) | أشهر أخطاء الشبكات وطريقة تجنبها |

---

# Suggested Learning Path

ابدأ بالترتيب التالي:

```text
OSI Layers
↓
Encapsulation
↓
IP Addressing
↓
DHCP
↓
DNS
↓
ARP
↓
Switch
↓
VLANs
↓
Router
↓
Routing
↓
ICMP
↓
NAT
↓
Ports
↓
TCP Handshake
↓
TCP vs UDP
↓
TLS
↓
HTTP/HTTPS Lifecycle
↓
Troubleshooting Commands
↓
Practical Labs
↓
Wireshark
↓
Common Mistakes
```

---

# What You Will Learn

بعد قراءة هذه الملفات، ستكون قادرًا على فهم:

```text
كيف تتحرك البيانات داخل الشبكة
كيف يعمل IP و MAC Address
كيف يحصل الجهاز على IP من DHCP
كيف يتحول Domain Name إلى IP عن طريق DNS
كيف تعمل Switches و Routers
كيف تعمل VLANs
كيف تعمل Routing Protocols
كيف يعمل NAT و PAT
ما معنى Ports و Sockets
كيف يبدأ اتصال TCP
الفرق بين TCP و UDP
كيف يعمل TLS و HTTPS
ماذا يحدث عند فتح موقع
كيف تشخص مشاكل الشبكة
كيف تستخدم Wireshark لفهم الترافيك
أشهر الأخطاء التي يجب تجنبها
```

---

# Important Commands

## Windows

```cmd
ipconfig /all
ipconfig /flushdns
ping 8.8.8.8
tracert 8.8.8.8
nslookup google.com
route print
arp -a
netstat -ano
```

---

## PowerShell

```powershell
Test-NetConnection google.com -Port 443
Get-NetTCPConnection
```

---

## Linux

```bash
ip addr
ip route
ip neigh
ping 8.8.8.8
traceroute 8.8.8.8
dig google.com
ss -tunap
ss -tuln
curl -v https://example.com
nc -vz example.com 443
```

---

## Wireshark Filters

```text
arp
dns
icmp
tcp
udp
tls
http
ip.addr == 8.8.8.8
tcp.port == 443
udp.port == 53
tcp.flags.syn == 1
tcp.flags.reset == 1
tcp.analysis.retransmission
```

---

# Notes

```text
كل ملف مستقل ويمكن قراءته وحده
لكن الأفضل قراءة الملفات بالترتيب
الأمثلة عملية ومناسبة للتطبيق على جهازك أو Lab خاص بك
استخدم أوامر الفحص فقط على شبكات تملكها أو لديك تصريح عليها
```

---

# Repository Structure

```text
.
├── README.md
├── OSI-Layers.md
├── Packet-Encapsulation-and-Decapsulation.md
├── IP-Addressing-and-Subnetting.md
├── DHCP-How-Devices-Get-IP.md
├── DNS-How-Domain-Names-Work.md
├── ARP-Protocol.md
├── Switch-Layer-2.md
├── VLANs-and-Trunking.md
├── Router-Layer-3.md
├── Routing-Protocols-OSPF-RIP-BGP.md
├── ICMP-Ping-and-Traceroute.md
├── NAT-and-PAT.md
├── Ports-and-Sockets.md
├── TCP-3-Way-Handshake.md
├── TCP-vs-UDP.md
├── TLS-Handshake-and-Encryption.md
├── HTTP-and-HTTPS-Request-Lifecycle.md
├── Network-Troubleshooting-Commands.md
├── Networking-Practical-Labs.md
├── Wireshark-Basics.md
└── Common-Networking-Mistakes.md
```
