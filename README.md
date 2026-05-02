
# Networking Notes

A structured collection of networking notes for learning computer networks step by step, starting from the fundamentals and moving toward practical troubleshooting, infrastructure concepts, and network security.

هذا المستودع يحتوي على ملاحظات منظمة لتعلم أساسيات الشبكات خطوة بخطوة، بدايةً من المفاهيم الأساسية وحتى التطبيق العملي، مع التركيز على أمن الشبكات والبنية التحتية المتقدمة.

---

## Table of Contents

- [Main Topics](#main-topics)
- [Practical and Extra Topics](#practical-and-extra-topics)
- [Suggested Learning Path](#suggested-learning-path)
- [What You Will Learn](#what-you-will-learn)
- [Important Commands](#important-commands)
- [Repository Structure](#repository-structure)
- [Security Note](#security-note)

---

## Main Topics

| # | File | Topic |
|---|------|-------|
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

## Practical and Extra Topics

### Practical Labs and Troubleshooting

| # | File | Topic |
|---|------|-------|
| 18 | [Network Troubleshooting Commands](./Network-Troubleshooting-Commands.md) | أوامر تشخيص مشاكل الشبكة |
| 19 | [Networking Practical Labs](./Networking-Practical-Labs.md) | تطبيقات عملية و Labs |
| 20 | [Wireshark Basics](./Wireshark-Basics.md) | أساسيات Wireshark وتحليل Packets |
| 21 | [Common Networking Mistakes](./Common-Networking-Mistakes.md) | أشهر أخطاء الشبكات وطريقة تجنبها |

### Network Security

| # | File | Topic |
|---|------|-------|
| 23 | [Network Security Basics](./Network-Security-Basics.md) | مفاهيم الحماية، CIA Triad، والتهديدات |
| 24 | [Firewall Basics](./Firewall-Basics.md) | أنواع الجدران النارية، Stateful vs Stateless، ACLs |
| 25 | [VPN Basics](./VPN-Basics.md) | كيف تعمل الشبكات الخاصة الافتراضية، Tunneling و Encryption |

### Enterprise Infrastructure

| # | File | Topic |
|---|------|-------|
| 26 | [Proxy and Reverse Proxy](./Proxy-and-Reverse-Proxy.md) | الفرق بين Proxy و Reverse Proxy |
| 27 | [Load Balancer Basics](./Load-Balancer-Basics.md) | Load Balancers، Algorithms، L4 vs L7 |
| 28 | [CDN Basics](./CDN-Basics.md) | شبكات توصيل المحتوى وكيف يعمل Cloudflare كمثال |

### Operating System Commands and Scanning Tools

| # | File | Topic |
|---|------|-------|
| 30 | [Linux Networking Commands](./Linux-Networking-Commands.md) | أوامر الشبكات في Linux |
| 31 | [Windows Networking Commands](./Windows-Networking-Commands.md) | أوامر الشبكات في Windows |
| 32 | [Nmap Basics](./Nmap-Basics.md) | أساسيات أداة Nmap لفحص الشبكات |

### Scenarios and References

| # | File | Topic |
|---|------|-------|
| 29 | [DHCP DNS ARP Practical Scenarios](./DHCP-DNS-ARP-Practical-Scenarios.md) | سيناريوهات عملية تربط DHCP و DNS و ARP |
| 33 | [Common Ports and Services](./Common-Ports-and-Services.md) | دليل شامل لأشهر البورتات والخدمات |
| 34 | [Web Networking for Bug Bounty](./Web-Networking-for-Bug-Bounty.md) | فهم الشبكات من منظور Bug Bounty |
| 22 | [Networking Glossary](./Networking-Glossary.md) | قاموس مصطلحات الشبكات |

### Final Review

| # | File | Topic |
|---|------|-------|
| 35 | [Final Networking Cheat Sheet](./Final-Networking-Cheat-Sheet.md) | ورقة مراجعة نهائية تجمع أهم المفاهيم |

---

## Suggested Learning Path

To build a strong foundation, it is recommended to study the topics in the following order:

### Stage 1: Fundamentals

```text
OSI Layers → Encapsulation → IP Addressing → DHCP → DNS → ARP
````

### Stage 2: Network Devices and Routing

```text
Switch → VLANs → Router → Routing → ICMP → NAT → Ports
```

### Stage 3: Protocols and Applications

```text
TCP Handshake → TCP vs UDP → TLS → HTTP/HTTPS Lifecycle
```

### Stage 4: Practical Troubleshooting

```text
Troubleshooting Commands → Windows/Linux Commands → Wireshark → Labs → Common Mistakes
```

### Stage 5: Enterprise Infrastructure

```text
Proxy/Reverse Proxy → Load Balancer → CDN
```

### Stage 6: Network Security and Ethical Hacking

```text
Network Security → Firewall → VPN → Nmap → Web Networking for Bug Bounty
```

### Stage 7: Final Review

```text
Practical Scenarios → Glossary → Final Cheat Sheet
```

---

## What You Will Learn

After studying these notes, you should be able to understand:

### Networking Fundamentals

* How data moves through a network using Encapsulation and Decapsulation.
* The difference between IP Address and MAC Address.
* How devices get an IP address using DHCP.
* How domain names are resolved into IP addresses using DNS.
* How ARP is used to discover MAC addresses inside a local network.

### Network Devices and Routing

* How switches and routers work.
* The difference between Layer 2 and Layer 3 devices.
* How VLANs and Trunk Ports are used to separate networks.
* How routing protocols such as OSPF, RIP, and BGP work.
* How NAT and PAT allow private networks to access the internet.
* What ports and sockets are and why they are important.

### Protocols

* How a TCP connection starts using the 3-Way Handshake.
* The difference between TCP and UDP.
* How TLS encrypts communication in HTTPS.
* What happens step by step when you open a website in a browser.

### Practical Skills and Security

* How to troubleshoot network problems from Layer 1 to Layer 7.
* How to use common Windows and Linux networking commands.
* How to use Wireshark to analyze network traffic.
* Common mistakes made in networking and how to avoid them.
* How firewalls, VPNs, proxies, load balancers, and CDNs work.
* How to look at networks from a Bug Bounty and ethical hacking perspective.

---

## Important Commands

### Windows CMD

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

### PowerShell

```powershell
Test-NetConnection google.com -Port 443
Get-NetTCPConnection
```

### Linux

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

### Wireshark Filters

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

## Notes

* Each file can be read independently, but following the suggested order is recommended.
* The examples are practical and suitable for labs using tools such as Packet Tracer, GNS3, or a local virtual lab.
* This repository is useful for beginners, students, junior network engineers, and people interested in cybersecurity fundamentals.

---

## Security Note

Use scanning tools such as Nmap only on networks you own or networks where you have clear written permission.

Unauthorized scanning or testing may be illegal and unethical.

---

## Repository Structure

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
├── Common-Networking-Mistakes.md
├── Networking-Glossary.md
├── Network-Security-Basics.md
├── Firewall-Basics.md
├── VPN-Basics.md
├── Proxy-and-Reverse-Proxy.md
├── Load-Balancer-Basics.md
├── CDN-Basics.md
├── DHCP-DNS-ARP-Practical-Scenarios.md
├── Linux-Networking-Commands.md
├── Windows-Networking-Commands.md
├── Nmap-Basics.md
├── Common-Ports-and-Services.md
├── Web-Networking-for-Bug-Bounty.md
└── Final-Networking-Cheat-Sheet.md
```
