# Networking Practical Labs

## تطبيق عملي على مفاهيم الشبكات

هذا الملف يجمع Labs عملية تساعدك تطبق المفاهيم التي شرحناها سابقًا.

الفكرة هنا ليست الحفظ فقط، بل أن ترى بنفسك كيف تعمل الشبكة.

سنطبق على:

```text
IP Addressing
Subnetting
Ping
Traceroute
DNS
ARP
Ports
TCP
UDP
HTTP
TLS
Routing
VLANs
NAT
Troubleshooting
Wireshark
```

---

# قبل أن تبدأ

تحتاج واحد أو أكثر من الأدوات التالية:

```text
Windows CMD / PowerShell
Linux Terminal
macOS Terminal
Cisco Packet Tracer
Wireshark
Browser DevTools
curl
netcat / nc
```

لو لم تكن كل الأدوات متاحة، نفذ المتاح فقط.

---

# قاعدة مهمة

نفذ الأوامر والفحوصات فقط على:

```text
جهازك
شبكتك المنزلية
Lab خاص بك
سيرفرات تملك تصريحًا لفحصها
```

لا تستخدم فحص Ports أو Scanning على أجهزة أو شبكات لا تملكها.

---

# Lab 1: معرفة إعدادات الشبكة على جهازك

## الهدف

معرفة:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
MAC Address
```

---

## Windows

افتح CMD واكتب:

```cmd
ipconfig /all
```

---

## Linux

```bash
ip addr
ip route
```

---

## macOS

```bash
ifconfig
netstat -rn
```

---

## المطلوب منك

اكتب القيم التالية:

```text
My IP Address      =
Subnet Mask/Prefix =
Default Gateway    =
DNS Server         =
MAC Address        =
```

---

## ماذا تتعلم؟

لو جهازك أخذ IP مثل:

```text
192.168.1.10
```

والـ Gateway:

```text
192.168.1.1
```

فهذا يعني أن جهازك داخل شبكة محلية، وغالبًا الراوتر هو طريق الخروج للإنترنت.

---

# Lab 2: اختبار الاتصال بالترتيب الصحيح

## الهدف

تعرف أين المشكلة عندما الإنترنت لا يعمل.

---

## نفذ الأوامر بالترتيب

### 1. اختبار الجهاز نفسه

```cmd
ping 127.0.0.1
```

لو نجح، TCP/IP Stack يعمل.

---

### 2. اختبار IP جهازك

```cmd
ping YOUR_IP
```

مثال:

```cmd
ping 192.168.1.10
```

---

### 3. اختبار الراوتر

```cmd
ping 192.168.1.1
```

غير الرقم حسب Default Gateway عندك.

---

### 4. اختبار الإنترنت بدون DNS

```cmd
ping 8.8.8.8
```

---

### 5. اختبار DNS

```cmd
ping google.com
```

---

## تحليل النتائج

| النتيجة | المعنى غالبًا |
|---|---|
| Gateway لا يرد | مشكلة محلية أو Wi-Fi أو IP |
| 8.8.8.8 لا يرد | مشكلة Router أو ISP أو NAT |
| 8.8.8.8 يرد و google.com لا يرد | مشكلة DNS |
| كل شيء يرد | الاتصال الأساسي يعمل |

---

# Lab 3: DNS Resolution

## الهدف

تفهم كيف يتحول Domain Name إلى IP Address.

---

## Windows / Linux / macOS

```bash
nslookup google.com
```

أو:

```bash
nslookup github.com
```

---

## Linux / macOS

```bash
dig google.com
```

---

## المطلوب منك

سجل النتيجة:

```text
Domain =
Resolved IP =
DNS Server used =
```

---

## جرب أكثر من DNS Server

```bash
nslookup google.com 8.8.8.8
```

```bash
nslookup google.com 1.1.1.1
```

---

## ماذا تتعلم؟

DNS هو الذي يحول:

```text
google.com
```

إلى IP مثل:

```text
142.250.x.x
```

بدون DNS، قد يكون الإنترنت يعمل لكن المواقع بالأسماء لا تفتح.

---

# Lab 4: ARP Table

## الهدف

تفهم كيف يعرف جهازك MAC Address الخاص بالراوتر أو الأجهزة المحلية.

---

## Windows

```cmd
arp -a
```

---

## Linux

```bash
ip neigh
```

---

## macOS

```bash
arp -a
```

---

## خطوات عملية

اعمل Ping للراوتر:

```cmd
ping 192.168.1.1
```

ثم اعرض ARP Table:

```cmd
arp -a
```

---

## المتوقع

ستجد سطرًا يشبه:

```text
192.168.1.1    aa-bb-cc-dd-ee-ff
```

هذا يعني أن جهازك عرف MAC Address الخاص بالـ Default Gateway.

---

## ماذا تتعلم؟

داخل الشبكة المحلية:

```text
IP يحتاج MAC حتى يتم الإرسال
```

وهذا يتم باستخدام:

```text
ARP
```

---

# Lab 5: Traceroute

## الهدف

معرفة الطريق الذي تمر به Packets حتى تصل للوجهة.

---

## Windows

```cmd
tracert 8.8.8.8
```

---

## Linux / macOS

```bash
traceroute 8.8.8.8
```

أو:

```bash
tracepath 8.8.8.8
```

---

## المطلوب منك

اكتب أول 3 Hops:

```text
Hop 1 =
Hop 2 =
Hop 3 =
```

---

## ماذا يعني Hop 1؟

غالبًا Hop 1 هو:

```text
راوتر البيت
```

مثال:

```text
192.168.1.1
```

---

## ماذا تعني النجوم؟

لو ظهر:

```text
* * *
```

هذا لا يعني دائمًا أن الاتصال متوقف.

قد يكون الراوتر لا يرد على ICMP.

---

# Lab 6: فهم Ports

## الهدف

اختبار هل Port معين مفتوح ويمكن الوصول إليه.

---

## Windows PowerShell

```powershell
Test-NetConnection google.com -Port 443
```

---

## Linux / macOS

```bash
nc -vz google.com 443
```

---

## جرب Ports مختلفة

```bash
nc -vz google.com 443
nc -vz google.com 80
nc -vz google.com 22
```

---

## تحليل النتيجة

```text
Port 443 مفتوح غالبًا لأنه HTTPS
Port 80 قد يكون مفتوحًا أو يعمل Redirect
Port 22 غالبًا مغلق لأنه SSH غير متاح للعامة
```

---

## ماذا تتعلم؟

```text
IP يحدد الجهاز
Port يحدد الخدمة
```

مثال:

```text
google.com:443 = HTTPS Service
```

---

# Lab 7: عرض الاتصالات الحالية

## الهدف

ترى الاتصالات المفتوحة على جهازك.

---

## Windows

```cmd
netstat -ano
```

---

## Linux

```bash
ss -tunap
```

---

## Linux بدون صلاحيات root

```bash
ss -tuna
```

---

## المطلوب منك

ابحث عن اتصالات:

```text
ESTABLISHED
```

وسجل مثالًا:

```text
Local Address  =
Remote Address =
State          =
```

---

## ماذا تتعلم؟

عندما تفتح مواقع أو تطبيقات، جهازك ينشئ اتصالات كثيرة.

كل اتصال له:

```text
Source IP
Source Port
Destination IP
Destination Port
Protocol
```

---

# Lab 8: HTTP Request باستخدام curl

## الهدف

ترى HTTP Headers والـ Status Code.

---

## أمر بسيط

```bash
curl -I https://example.com
```

---

## أمر بتفاصيل أكثر

```bash
curl -v https://example.com
```

---

## المطلوب منك

سجل:

```text
Status Code =
Server Header =
Content-Type =
Redirect موجود؟ Yes/No
```

---

## مثال نتيجة

```http
HTTP/2 200
content-type: text/html
```

---

## ماذا تتعلم؟

curl يوضح لك ما يحدث بين العميل والسيرفر بدون واجهة المتصفح.

---

# Lab 9: اختبار Redirect من HTTP إلى HTTPS

## الهدف

تفهم كيف يحول الموقع من HTTP إلى HTTPS.

---

## نفذ:

```bash
curl -I http://example.com
```

أو جرّب موقعًا آخر تعرف أنه يحول إلى HTTPS.

---

## ابحث عن Header:

```http
Location: https://...
```

---

## ماذا تتعلم؟

لو السيرفر رد بـ:

```text
301
302
```

فهذا يعني أن هناك Redirect.

مثال:

```text
http://site.com → https://site.com
```

---

# Lab 10: فحص TLS Certificate

## الهدف

معرفة معلومات شهادة الموقع.

---

## OpenSSL

```bash
openssl s_client -connect example.com:443 -servername example.com
```

---

## المطلوب منك

ابحث عن:

```text
TLS Version
Cipher
Certificate Subject
Certificate Issuer
Verify return code
```

---

## عرض الشهادة فقط

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null | openssl x509 -noout -subject -issuer -dates
```

---

## ماذا تتعلم؟

TLS Certificate تثبت هوية السيرفر، والمتصفح يتحقق منها قبل الوثوق بالاتصال.

---

# Lab 11: Browser DevTools Network

## الهدف

تفهم HTTP Requests داخل المتصفح.

---

## الخطوات

افتح المتصفح.

ثم:

```text
Right Click
Inspect
Network
```

بعد ذلك افتح موقعًا مثل:

```text
https://example.com
```

---

## راقب الآتي

```text
Request URL
Method
Status Code
Type
Size
Time
Headers
Response
Cookies
Timing
```

---

## المطلوب منك

اكتب أول Request ظهر:

```text
URL =
Method =
Status Code =
Content-Type =
Remote Address =
```

---

## ماذا تتعلم؟

صفحة واحدة قد تحتاج عشرات الطلبات:

```text
HTML
CSS
JavaScript
Images
Fonts
API Requests
```

---

# Lab 12: Wireshark Capture - Ping

## الهدف

رؤية ICMP Echo Request و Echo Reply.

---

## الخطوات

1. افتح Wireshark.
2. اختر كرت الشبكة المستخدم.
3. ابدأ Capture.
4. نفذ:

```cmd
ping 8.8.8.8
```

5. أوقف Capture.

---

## فلتر Wireshark

```text
icmp
```

---

## ابحث عن

```text
Echo Request
Echo Reply
Source IP
Destination IP
TTL
```

---

## ماذا تتعلم؟

Ping يستخدم ICMP وليس TCP أو UDP.

---

# Lab 13: Wireshark Capture - DNS

## الهدف

رؤية DNS Query و DNS Response.

---

## الخطوات

ابدأ Capture ثم نفذ:

```bash
nslookup example.com
```

---

## فلتر Wireshark

```text
dns
```

---

## ابحث عن

```text
Query Name
Query Type
Response IP
DNS Server
```

---

## ماذا تتعلم؟

قبل فتح الموقع، الجهاز يحتاج معرفة IP الخاص بالدومين.

---

# Lab 14: Wireshark Capture - TCP Handshake

## الهدف

رؤية 3-Way Handshake.

---

## الخطوات

ابدأ Capture ثم افتح:

```text
https://example.com
```

أو نفذ:

```bash
curl https://example.com
```

---

## فلتر Wireshark

```text
tcp.port == 443
```

أو:

```text
tcp.flags.syn == 1
```

---

## ابحث عن

```text
SYN
SYN-ACK
ACK
```

---

## الشكل المتوقع

```text
Client → Server: SYN
Server → Client: SYN, ACK
Client → Server: ACK
```

---

# Lab 15: Wireshark Capture - TLS

## الهدف

رؤية TLS Handshake.

---

## فلتر Wireshark

```text
tls
```

---

## ابحث عن

```text
ClientHello
ServerHello
Certificate
Finished
Application Data
```

---

## ماذا تتعلم؟

بعد TLS Handshake، HTTP يظهر غالبًا كـ:

```text
Encrypted Application Data
```

وليس كنص واضح.

---

# Lab 16: مقارنة HTTP و HTTPS

## الهدف

ترى الفرق بين اتصال غير مشفر واتصال مشفر.

---

## افتح موقع HTTP للتجربة فقط

```text
http://example.com
```

ثم HTTPS:

```text
https://example.com
```

---

## في Wireshark

لـ HTTP:

```text
http
```

لـ HTTPS:

```text
tls
```

---

## المقارنة

| العنصر | HTTP | HTTPS |
|---|---|---|
| المحتوى واضح؟ | نعم غالبًا | لا |
| يستخدم TLS؟ | لا | نعم |
| Port | 80 | 443 |
| آمن للبيانات الحساسة؟ | لا | نعم غالبًا |

---

# Lab 17: Subnetting سريع

## الهدف

تحديد هل جهازان في نفس الشبكة أم لا.

---

## مثال 1

```text
PC1 = 192.168.1.10/24
PC2 = 192.168.1.20/24
```

الشبكة:

```text
192.168.1.0/24
```

النتيجة:

```text
Same Network
```

---

## مثال 2

```text
PC1 = 192.168.1.10/24
PC2 = 192.168.2.20/24
```

الشبكات:

```text
192.168.1.0/24
192.168.2.0/24
```

النتيجة:

```text
Different Networks
```

ويحتاجان:

```text
Router
```

---

# تدريب

حدد هل الأجهزة في نفس الشبكة:

```text
A = 10.0.0.10/24
B = 10.0.0.50/24
```

```text
A = 172.16.5.10/24
B = 172.16.6.10/24
```

```text
A = 192.168.10.5/25
B = 192.168.10.100/25
```

---

# Lab 18: Packet Tracer - شبكة بسيطة

## الهدف

بناء LAN بسيطة.

---

## الأجهزة

```text
2 PCs
1 Switch
```

---

## الإعداد

```text
PC1 IP = 192.168.1.10/24
PC2 IP = 192.168.1.20/24
```

لا تحتاج Gateway لأنهما في نفس الشبكة.

---

## الاختبار

من PC1:

```text
ping 192.168.1.20
```

---

## المتوقع

```text
Ping Success
```

---

## ماذا تتعلم؟

الأجهزة داخل نفس الشبكة يمكنها التواصل عبر Switch فقط.

---

# Lab 19: Packet Tracer - شبكتان مع Router

## الهدف

ربط شبكتين مختلفتين باستخدام Router.

---

## الأجهزة

```text
2 PCs
1 Router
2 Switches
```

---

## الشبكات

```text
Network 1 = 192.168.1.0/24
Network 2 = 192.168.2.0/24
```

---

## إعدادات PC1

```text
IP Address      = 192.168.1.10
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.1.1
```

---

## إعدادات PC2

```text
IP Address      = 192.168.2.10
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.2.1
```

---

## إعداد الراوتر

```text
G0/0 = 192.168.1.1/24
G0/1 = 192.168.2.1/24
```

---

## الاختبار

من PC1:

```text
ping 192.168.2.10
```

---

## المتوقع

```text
Ping Success
```

---

## ماذا تتعلم؟

التواصل بين شبكتين مختلفتين يحتاج Router أو جهاز Layer 3.

---

# Lab 20: Packet Tracer - Static Route

## الهدف

ربط أكثر من Router باستخدام Static Routes.

---

## التصميم

```text
PC1 --- R1 --- R2 --- PC2
```

---

## الشبكات

```text
PC1 Network = 192.168.1.0/24
R1-R2 Link  = 10.0.0.0/30
PC2 Network = 192.168.2.0/24
```

---

## Route على R1

```text
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

---

## Route على R2

```text
ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

---

## الاختبار

من PC1:

```text
ping 192.168.2.10
```

---

## ماذا تتعلم؟

لا يكفي Route في اتجاه واحد.

لازم:

```text
Route للذهاب
Route للعودة
```

---

# Lab 21: Packet Tracer - VLANs

## الهدف

فهم أن VLANs تفصل الشبكة حتى لو الأجهزة على نفس Switch.

---

## الأجهزة

```text
1 Switch
4 PCs
```

---

## VLANs

```text
VLAN 10 = IT
VLAN 20 = HR
```

---

## التوزيع

```text
PC1 و PC2 في VLAN 10
PC3 و PC4 في VLAN 20
```

---

## IPs

```text
PC1 = 192.168.10.10/24
PC2 = 192.168.10.20/24

PC3 = 192.168.20.10/24
PC4 = 192.168.20.20/24
```

---

## الاختبار

```text
PC1 ping PC2 → Success
PC3 ping PC4 → Success
PC1 ping PC3 → Fail
```

---

## ماذا تتعلم؟

VLANs تفصل Broadcast Domains.

للتواصل بين VLANs تحتاج:

```text
Inter-VLAN Routing
```

---

# Lab 22: Packet Tracer - Router on a Stick

## الهدف

تشغيل Inter-VLAN Routing باستخدام Router واحد.

---

## التصميم

```text
PCs → Switch → Router
```

الرابط بين Switch و Router يكون:

```text
Trunk
```

---

## VLANs

```text
VLAN 10 = 192.168.10.0/24
VLAN 20 = 192.168.20.0/24
```

---

## Router Subinterfaces

```text
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

---

## Default Gateway

لأجهزة VLAN 10:

```text
192.168.10.1
```

لأجهزة VLAN 20:

```text
192.168.20.1
```

---

## الاختبار

```text
PC in VLAN 10 ping PC in VLAN 20
```

---

## المتوقع

```text
Success
```

---

# Lab 23: Packet Tracer - OSPF بسيط

## الهدف

تشغيل Dynamic Routing بين راوترات.

---

## التصميم

```text
LAN1 --- R1 --- R2 --- LAN2
```

---

## OSPF على R1

```text
router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
```

---

## OSPF على R2

```text
router ospf 1
 network 192.168.2.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
```

---

## أوامر فحص

```text
show ip route
show ip ospf neighbor
```

---

## ماذا تتعلم؟

OSPF يجعل الراوترات تتعلم الشبكات تلقائيًا بدل Static Routes.

---

# Lab 24: NAT Concept Lab

## الهدف

فهم كيف يخرج أكثر من جهاز داخلي إلى الإنترنت باستخدام IP واحد.

---

## الفكرة

الأجهزة الداخلية:

```text
PC1 = 192.168.1.10
PC2 = 192.168.1.20
```

تخرج إلى الإنترنت عبر الراوتر باستخدام:

```text
Public IP واحد
```

---

## قبل NAT

```text
192.168.1.10:50001 → 8.8.8.8:53
192.168.1.20:50001 → 8.8.8.8:53
```

---

## بعد PAT

```text
41.33.20.5:62001 → 8.8.8.8:53
41.33.20.5:62002 → 8.8.8.8:53
```

---

## ماذا تتعلم؟

PAT يستخدم:

```text
IP + Port
```

حتى يميز الاتصالات القادمة من أجهزة مختلفة.

---

# Lab 25: Troubleshooting Challenge 1

## السيناريو

جهازك متصل بالواي فاي، لكن لا يوجد إنترنت.

الإعدادات:

```text
IP Address      = 169.254.10.20
Subnet Mask     = 255.255.0.0
Default Gateway =
DNS Server      =
```

---

## السؤال

ما المشكلة غالبًا؟

---

## الإجابة

الجهاز لم يحصل على IP من DHCP.

الدليل:

```text
169.254.x.x
```

هذا يسمى APIPA.

---

## خطوات الحل

```text
افحص DHCP
افصل واتصل بالشبكة
جرب ipconfig /renew
افحص الراوتر
افحص كلمة مرور Wi-Fi
افحص الكابل لو Ethernet
```

---

# Lab 26: Troubleshooting Challenge 2

## السيناريو

```text
ping 192.168.1.1 يعمل
ping 8.8.8.8 يعمل
ping google.com لا يعمل
```

---

## المشكلة غالبًا

```text
DNS Problem
```

---

## الحل

جرب:

```cmd
nslookup google.com
ipconfig /flushdns
```

غيّر DNS إلى:

```text
8.8.8.8
1.1.1.1
```

---

# Lab 27: Troubleshooting Challenge 3

## السيناريو

```text
ping google.com يعمل
لكن الموقع لا يفتح في المتصفح
```

---

## الاحتمالات

```text
Port 80/443 محجوب
Proxy Problem
TLS Certificate Problem
Browser Cache
Firewall
Server HTTP Problem
```

---

## الاختبار

```bash
curl -v https://google.com
```

أو:

```powershell
Test-NetConnection google.com -Port 443
```

---

# Lab 28: Troubleshooting Challenge 4

## السيناريو

موقع يعطي:

```text
404 Not Found
```

---

## المعنى

السيرفر يعمل ورد عليك، لكن المسار المطلوب غير موجود.

---

## ليس معناه

```text
الإنترنت لا يعمل
السيرفر مطفأ
DNS لا يعمل
```

---

## افحص

```text
URL
Path
Routing في التطبيق
Reverse Proxy Rules
```

---

# Lab 29: Troubleshooting Challenge 5

## السيناريو

موقع يعطي:

```text
502 Bad Gateway
```

---

## المعنى غالبًا

Reverse Proxy لا يستطيع الوصول إلى Backend Server.

مثال:

```text
Nginx يعمل
لكن Application Server متوقف
```

---

## افحص

```text
Backend Service
Proxy Config
Ports
Firewall
Logs
```

---

# Lab 30: Mini Project - ارسم رحلة فتح موقع

## الهدف

تجمع كل ما تعلمته في رسم واحد.

---

## المطلوب

ارسم رحلة فتح:

```text
https://example.com
```

واكتب الخطوات:

```text
1. DNS
2. ARP
3. TCP Handshake
4. TLS Handshake
5. HTTP Request
6. Server Processing
7. HTTP Response
8. Browser Rendering
```

---

## نموذج رسم

```text
Browser
  │
  ├── DNS Query → DNS Server
  │
  ├── ARP → Default Gateway MAC
  │
  ├── TCP SYN → Web Server:443
  │
  ├── TLS ClientHello
  │
  ├── Encrypted HTTP GET /
  │
  └── Encrypted HTTP Response
```

---

# Lab 31: Mini Project - Troubleshooting Report

## الهدف

تكتب تقرير تشخيص احترافي.

---

## النموذج

```text
Problem:
Website does not open.

Device IP:
192.168.1.10/24

Gateway Test:
ping 192.168.1.1 = Success

Internet IP Test:
ping 8.8.8.8 = Success

DNS Test:
nslookup example.com = Success

TCP Test:
Test-NetConnection example.com -Port 443 = Failed

Conclusion:
Network and DNS are working, but TCP 443 is blocked or service is down.

Next Action:
Check firewall, proxy, server port 443, or ISP filtering.
```

---

# Lab 32: Mini Project - احفظ أهم Ports

## الهدف

تحفظ أهم البورتات العملية.

---

## المطلوب

اكتب الخدمة المناسبة لكل Port:

```text
20/21 =
22 =
23 =
25 =
53 =
67/68 =
80 =
123 =
443 =
445 =
3306 =
3389 =
5432 =
8080 =
```

---

## الإجابة

```text
20/21 = FTP
22 = SSH
23 = Telnet
25 = SMTP
53 = DNS
67/68 = DHCP
80 = HTTP
123 = NTP
443 = HTTPS
445 = SMB
3306 = MySQL
3389 = RDP
5432 = PostgreSQL
8080 = Alternative HTTP
```

---

# Lab 33: Mini Project - OSI Mapping

## الهدف

تربط كل خطوة بطبقة OSI.

---

## المطلوب

ضع كل عنصر في الطبقة المناسبة:

```text
Ethernet
IP
TCP
UDP
TLS
HTTP
DNS
ARP
Switch
Router
Port
MAC Address
```

---

## الإجابة المختصرة

```text
Ethernet    = Layer 2
MAC Address = Layer 2
Switch      = Layer 2
ARP         = Layer 2/3 boundary
IP          = Layer 3
Router      = Layer 3
TCP         = Layer 4
UDP         = Layer 4
Port        = Layer 4
TLS         = بين Layer 4 و Layer 7 عمليًا
HTTP        = Layer 7
DNS         = Layer 7
```

---

# أهم أوامر Labs للحفظ

## Windows

```cmd
ipconfig /all
ipconfig /release
ipconfig /renew
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
icmp
dns
arp
tcp
udp
tls
http
tcp.port == 443
tcp.flags.syn == 1
ip.addr == 8.8.8.8
```

---

# ملخص سريع

```text
Labs تساعدك تحول النظري إلى عملي
ابدأ دائمًا من IP ثم Gateway ثم DNS ثم TCP ثم HTTP/TLS
Ping يختبر الوصول
Traceroute يوضح الطريق
DNS يحول الاسم إلى IP
ARP يعرف MAC داخل الشبكة المحلية
Ports تحدد الخدمات
Wireshark يريك Packets فعليًا
Packet Tracer ممتاز لتجارب Routing و VLANs
```

---

# أهم القواعد

```text
لا تفحص شبكة لا تملكها أو لا تملك تصريحًا عليها
```

```text
ابدأ التشخيص من الأساسيات
```

```text
لو 8.8.8.8 يعمل و google.com لا يعمل → DNS
```

```text
لو Gateway لا يعمل → مشكلة محلية
```

```text
لو Ping يعمل والموقع لا يفتح → افحص Ports و TLS و HTTP
```

```text
Wireshark يوضح لك الحقيقة داخل الشبكة
```

---

## Navigation

| Previous | Home | Next |
|---|---|---|
| [Network Troubleshooting Commands](./Network-Troubleshooting-Commands.md) | [README](./README.md) | [Wireshark-Basics](./Wireshark-Basics.md) |
