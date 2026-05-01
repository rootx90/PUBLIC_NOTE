# Wireshark Basics

## ما هو Wireshark؟

Wireshark هو برنامج يستخدم لمراقبة وتحليل الـ Packets داخل الشبكة.

يعني تستطيع أن ترى الترافيك الذي يمر من جهازك أو من كرت الشبكة.

يساعدك في فهم:

```text
ARP
DNS
ICMP
TCP
UDP
TLS
HTTP
DHCP
```

ويستخدم كثيرًا في:

```text
Network Troubleshooting
Cybersecurity
Pentesting Labs
Protocol Analysis
Learning Networking
```

---

# لماذا نستخدم Wireshark؟

Wireshark يجعلك ترى ما يحدث فعليًا.

بدل أن تقول:

```text
الموقع لا يفتح
```

يمكنك أن ترى:

```text
هل DNS رد؟
هل TCP Handshake تم؟
هل TLS بدأ؟
هل السيرفر رد؟
هل هناك Retransmission؟
هل هناك Packet Loss؟
```

---

# قاعدة مهمة قبل الاستخدام

استخدم Wireshark فقط على:

```text
جهازك
شبكتك الخاصة
Lab خاص بك
بيئة لديك تصريح عليها
```

لا تستخدمه للتجسس على شبكات أو أشخاص بدون إذن.

---

# Capture vs Analysis

هناك فرق بين:

```text
Capture
Analysis
```

## Capture

يعني تسجيل الـ Packets من كرت الشبكة.

مثال:

```text
Wi-Fi Interface
Ethernet Interface
Loopback Interface
```

## Analysis

يعني قراءة وتحليل الـ Packets بعد تسجيلها.

يمكنك فتح ملف Capture جاهز بصيغة:

```text
.pcap
.pcapng
```

---

# اختيار Network Interface

عند فتح Wireshark ستجد أكثر من Interface.

أمثلة:

```text
Wi-Fi
Ethernet
Loopback
VPN Adapter
VirtualBox Adapter
VMware Adapter
```

اختر الواجهة التي يمر منها الترافيك.

مثال:

لو أنت متصل بالواي فاي، اختر:

```text
Wi-Fi
```

لو أنت متصل بسلك، اختر:

```text
Ethernet
```

---

# كيف تعرف الواجهة الصحيحة؟

قبل بدء Capture، ستلاحظ أن بعض الواجهات يظهر عليها نشاط.

الواجهة التي عليها حركة غالبًا هي الصحيحة.

مثال:

```text
Wi-Fi يظهر عليها موجات Traffic
Ethernet لا يظهر عليها شيء
```

إذن اختر Wi-Fi.

---

# أول Capture بسيط

## الخطوات

```text
1. افتح Wireshark
2. اختر Wi-Fi أو Ethernet
3. اضغط Start Capture
4. افتح موقع أو نفذ ping
5. اضغط Stop
```

بعد ذلك ستظهر Packets كثيرة.

---

# واجهة Wireshark

Wireshark يحتوي غالبًا على 3 أجزاء رئيسية:

```text
Packet List
Packet Details
Packet Bytes
```

---

## Packet List

الجزء العلوي.

يعرض قائمة الـ Packets.

أعمدة مهمة:

```text
No.
Time
Source
Destination
Protocol
Length
Info
```

---

## Packet Details

الجزء الأوسط.

يعرض تفاصيل الـ Packet حسب الطبقات.

مثال:

```text
Frame
Ethernet
IP
TCP
TLS
HTTP
```

---

## Packet Bytes

الجزء السفلي.

يعرض البيانات الخام بالـ Hex و ASCII.

مفيد للفهم المتقدم.

---

# معنى الأعمدة المهمة

## No.

رقم الـ Packet داخل الـ Capture.

---

## Time

وقت ظهور الـ Packet.

يمكن أن يكون من بداية التسجيل أو وقت حقيقي.

---

## Source

عنوان المصدر.

قد يكون:

```text
MAC Address
IP Address
```

حسب نوع الـ Packet.

---

## Destination

عنوان الوجهة.

---

## Protocol

البروتوكول الذي تعرف عليه Wireshark.

أمثلة:

```text
ARP
DNS
ICMP
TCP
UDP
TLS
HTTP
DHCP
```

---

## Length

حجم الـ Packet بالـ Bytes.

---

## Info

ملخص سريع عن محتوى الـ Packet.

مثال:

```text
Who has 192.168.1.1?
Standard query A google.com
Echo request
SYN
Client Hello
```

---

# Display Filter vs Capture Filter

من أهم الأشياء في Wireshark معرفة الفرق بين:

```text
Display Filter
Capture Filter
```

---

## Display Filter

يستخدم بعد تسجيل الـ Packets لإظهار جزء معين منها.

مثال:

```text
dns
```

هذا لا يحذف Packets، فقط يخفي غير المطلوب من العرض.

---

## Capture Filter

يستخدم قبل التسجيل لتسجيل نوع معين فقط من الترافيك.

مثال:

```text
host 8.8.8.8
```

هذا يعني أن Wireshark لن يسجل إلا الترافيك الخاص بهذا الـ IP.

---

# الأفضل للمبتدئ

في البداية استخدم:

```text
Display Filters
```

لأنها أسهل وأكثر أمانًا أثناء التعلم.

لا تستخدم Capture Filters إلا عندما تفهم جيدًا ماذا تريد تسجيله.

---

# أهم Display Filters

## عرض ARP

```text
arp
```

---

## عرض DNS

```text
dns
```

---

## عرض ICMP

```text
icmp
```

---

## عرض TCP

```text
tcp
```

---

## عرض UDP

```text
udp
```

---

## عرض TLS

```text
tls
```

---

## عرض HTTP

```text
http
```

---

## عرض IP معين

```text
ip.addr == 8.8.8.8
```

---

## عرض مصدر معين

```text
ip.src == 192.168.1.10
```

---

## عرض وجهة معينة

```text
ip.dst == 8.8.8.8
```

---

## عرض Port معين

```text
tcp.port == 443
```

أو:

```text
udp.port == 53
```

---

# دمج الفلاتر

يمكن دمج أكثر من شرط.

## AND

```text
ip.addr == 8.8.8.8 and dns
```

---

## OR

```text
tcp.port == 80 or tcp.port == 443
```

---

## NOT

```text
not arp
```

---

# Lab 1: رؤية ARP

## الهدف

رؤية ARP Request و ARP Reply.

---

## الخطوات

ابدأ Capture ثم نفذ:

```cmd
ping 192.168.1.1
```

استخدم IP الراوتر عندك.

---

## الفلتر

```text
arp
```

---

## ماذا ستجد؟

قد ترى:

```text
Who has 192.168.1.1? Tell 192.168.1.10
192.168.1.1 is at aa:bb:cc:dd:ee:ff
```

---

## المعنى

جهازك يسأل:

```text
من يملك IP الراوتر؟
```

والراوتر يرد:

```text
أنا، وهذا هو MAC Address الخاص بي
```

---

# Lab 2: رؤية DNS

## الهدف

رؤية تحويل Domain إلى IP.

---

## الخطوات

ابدأ Capture ثم نفذ:

```cmd
nslookup example.com
```

أو افتح موقع في المتصفح.

---

## الفلتر

```text
dns
```

---

## ماذا ستجد؟

```text
Standard query A example.com
Standard query response A 93.184.216.34
```

---

## Query Type

أشهر أنواع DNS Records:

```text
A     = IPv4 Address
AAAA  = IPv6 Address
CNAME = Alias
MX    = Mail Server
TXT   = Text Record
NS    = Name Server
```

---

# Lab 3: رؤية Ping

## الهدف

رؤية ICMP Echo Request و Echo Reply.

---

## الخطوات

ابدأ Capture ثم نفذ:

```cmd
ping 8.8.8.8
```

---

## الفلتر

```text
icmp
```

---

## ماذا ستجد؟

```text
Echo request
Echo reply
```

---

## المعنى

جهازك يرسل:

```text
ICMP Echo Request
```

والسيرفر يرد:

```text
ICMP Echo Reply
```

---

# Lab 4: رؤية TCP 3-Way Handshake

## الهدف

رؤية بداية اتصال TCP.

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

## الفلتر

```text
tcp.port == 443
```

أو:

```text
tcp.flags.syn == 1
```

---

## الشكل المتوقع

```text
Client → Server: SYN
Server → Client: SYN, ACK
Client → Server: ACK
```

---

# كيف تميز SYN؟

في عمود Info ستجد:

```text
[SYN]
```

ثم:

```text
[SYN, ACK]
```

ثم:

```text
[ACK]
```

---

# معنى الأرقام في TCP

داخل TCP Details ستجد:

```text
Source Port
Destination Port
Sequence Number
Acknowledgment Number
Flags
Window Size
Checksum
```

هذه تساعد في فهم حالة الاتصال.

---

# Lab 5: رؤية TLS Handshake

## الهدف

رؤية بداية التشفير في HTTPS.

---

## الخطوات

ابدأ Capture وافتح:

```text
https://example.com
```

---

## الفلتر

```text
tls
```

---

## ماذا ستجد؟

```text
Client Hello
Server Hello
Certificate
Finished
Application Data
```

---

# Client Hello

رسالة من العميل إلى السيرفر.

تحتوي على معلومات مثل:

```text
TLS Version
Cipher Suites
SNI
ALPN
Key Share
```

---

# Server Hello

رد من السيرفر يختار فيه الإعدادات.

---

# Certificate

السيرفر يرسل الشهادة الرقمية الخاصة بالموقع.

---

# Application Data

بعد انتهاء TLS Handshake، يصبح المحتوى مشفرًا ويظهر غالبًا باسم:

```text
Application Data
```

---

# Lab 6: رؤية HTTP واضح

## الهدف

فهم الفرق بين HTTP و HTTPS.

---

## افتح موقع HTTP للتجربة فقط

```text
http://example.com
```

---

## الفلتر

```text
http
```

---

## ماذا قد ترى؟

```http
GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
```

في HTTP العادي، يمكن رؤية الـ Headers والطلب بشكل أوضح.

---

# HTTPS لا يظهر HTTP واضحًا

لو فتحت:

```text
https://example.com
```

غالبًا لن ترى:

```text
GET /
Host:
Cookie:
```

بل سترى:

```text
TLS Application Data
```

لأن HTTP داخل TLS مشفر.

---

# Lab 7: تتبع اتصال كامل

## الهدف

تتبع رحلة فتح موقع من البداية.

---

## الخطوات

1. ابدأ Capture.
2. افتح المتصفح.
3. افتح:

```text
https://example.com
```

4. أوقف Capture.

---

## استخدم الفلاتر بالترتيب

```text
dns
```

ثم:

```text
tcp.port == 443
```

ثم:

```text
tls
```

---

## التسلسل المتوقع

```text
DNS Query
DNS Response
TCP SYN
TCP SYN-ACK
TCP ACK
TLS ClientHello
TLS ServerHello
TLS Certificate
TLS Finished
Encrypted Application Data
```

---

# Follow TCP Stream

خاصية مهمة جدًا في Wireshark.

تستخدم لمتابعة اتصال TCP كامل.

## الطريقة

اضغط Right Click على Packet ثم:

```text
Follow
TCP Stream
```

---

# متى تستخدم Follow TCP Stream؟

تستخدمها مع:

```text
HTTP
Raw TCP Protocols
Telnet في Labs
Unencrypted Traffic
```

لكن مع HTTPS، ستجد البيانات مشفرة غالبًا.

---

# Follow UDP Stream

نفس الفكرة لكن مع UDP.

مفيد مع بعض البروتوكولات التي تستخدم UDP.

---

# Coloring Rules

Wireshark يستخدم ألوانًا لتسهيل القراءة.

أمثلة:

```text
TCP Packets
DNS Packets
ARP Packets
Errors
Retransmissions
```

الألوان تساعدك تلاحظ المشاكل بسرعة، لكنها لا تكفي وحدها للحكم.

---

# TCP Retransmission

قد ترى في Wireshark:

```text
TCP Retransmission
```

معناها أن TCP أعاد إرسال Segment لأن ACK لم يصل في الوقت المتوقع.

الأسباب المحتملة:

```text
Packet Loss
Network Congestion
Weak Wi-Fi
Firewall
Bad Cable
High Latency
```

---

# TCP Duplicate ACK

قد ترى:

```text
TCP Dup ACK
```

معناه أن المستقبل يكرر ACK لأنه لم يستلم جزءًا متوقعًا من البيانات.

قد يشير إلى:

```text
Packet Loss
Out-of-order packets
Network issue
```

---

# TCP Out-of-Order

معناه أن Packets وصلت بترتيب مختلف.

قد يحدث بسبب:

```text
مسارات مختلفة
ازدحام
Packet Loss
```

ليس دائمًا مشكلة كبيرة، لكن لو تكرر كثيرًا قد يؤثر على الأداء.

---

# TCP RST

قد ترى:

```text
RST
```

معناه أن الاتصال تم رفضه أو قطعه.

أسباب محتملة:

```text
Port مغلق
Application رفض الاتصال
Firewall أرسل Reset
Connection closed unexpectedly
```

---

# TCP FIN

```text
FIN
```

يعني إنهاء اتصال TCP بشكل طبيعي.

---

# Wireshark و DNS Troubleshooting

لو الموقع لا يفتح، افحص DNS.

## فلتر

```text
dns
```

## ابحث عن

```text
No response
NXDOMAIN
SERVFAIL
Wrong IP
Timeout
```

---

# NXDOMAIN

تعني أن الدومين غير موجود.

مثال:

```text
Name Error
```

---

# SERVFAIL

تعني أن DNS Server فشل في جلب الإجابة.

---

# Wireshark و TCP Troubleshooting

لو DNS يعمل لكن الموقع لا يفتح، افحص TCP.

## فلتر

```text
tcp.port == 443
```

## ابحث عن

```text
SYN بدون SYN-ACK
RST
Retransmission
Timeout
```

---

# SYN بدون SYN-ACK

قد يعني:

```text
Firewall يمنع
السيرفر لا يرد
مشكلة Routing
Packet Loss
```

---

# SYN ثم RST

قد يعني:

```text
البورت مغلق
الخدمة لا تعمل
السيرفر يرفض الاتصال
```

---

# SYN ثم SYN-ACK ثم ACK

يعني:

```text
TCP Handshake نجح
```

لو الموقع لا يفتح بعد ذلك، افحص TLS أو HTTP.

---

# Wireshark و TLS Troubleshooting

## فلتر

```text
tls
```

## مشاكل ممكنة

```text
TLS Alert
Handshake Failure
Certificate Problem
Unsupported Version
Cipher mismatch
```

---

# TLS Alert

لو رأيت:

```text
Alert
```

افتح التفاصيل وشاهد نوع الـ Alert.

قد يكون:

```text
Handshake Failure
Bad Certificate
Unknown CA
Protocol Version
```

---

# Wireshark و HTTP Troubleshooting

مع HTTP غير المشفر، استخدم:

```text
http
```

ابحث عن:

```text
GET
POST
Status Code
Headers
Redirects
Cookies
```

أمثلة Status Codes:

```text
200 OK
301 Moved Permanently
302 Found
403 Forbidden
404 Not Found
500 Internal Server Error
502 Bad Gateway
```

---

# فلاتر مفيدة جدًا

## IP معين

```text
ip.addr == 192.168.1.10
```

---

## محادثة بين جهازين

```text
ip.addr == 192.168.1.10 and ip.addr == 8.8.8.8
```

---

## TCP Port معين

```text
tcp.port == 443
```

---

## UDP Port معين

```text
udp.port == 53
```

---

## DNS Query معين

```text
dns.qry.name == "example.com"
```

---

## HTTP Method GET

```text
http.request.method == "GET"
```

---

## HTTP Status Code

```text
http.response.code == 404
```

---

## TCP SYN فقط

```text
tcp.flags.syn == 1 and tcp.flags.ack == 0
```

---

## TCP RST

```text
tcp.flags.reset == 1
```

---

## TCP Retransmission

```text
tcp.analysis.retransmission
```

---

## TCP Problems

```text
tcp.analysis.flags
```

---

# Capture Filters أمثلة

استخدمها قبل بدء التسجيل.

## Host معين

```text
host 8.8.8.8
```

---

## شبكة معينة

```text
net 192.168.1.0/24
```

---

## Port معين

```text
port 53
```

---

## TCP فقط

```text
tcp
```

---

## UDP فقط

```text
udp
```

---

# حفظ Capture

يمكنك حفظ التسجيل بصيغة:

```text
.pcapng
```

من:

```text
File → Save As
```

هذا مفيد لو تريد تحليله لاحقًا أو إرساله لشخص يساعدك.

---

# فتح Capture قديم

من:

```text
File → Open
```

ثم اختر ملف:

```text
.pcap
.pcapng
```

---

# Export Objects

Wireshark يستطيع استخراج ملفات من بعض الترافيك غير المشفر.

مثال:

```text
File → Export Objects → HTTP
```

لكن هذا غالبًا يعمل مع HTTP غير المشفر، وليس HTTPS المشفر.

---

# Statistics

قائمة Statistics مفيدة جدًا.

أمثلة:

```text
Protocol Hierarchy
Conversations
Endpoints
IO Graphs
Flow Graph
```

---

# Protocol Hierarchy

يعرض توزيع البروتوكولات داخل الـ Capture.

مثال:

```text
Ethernet
IPv4
TCP
TLS
DNS
ICMP
```

---

# Conversations

يعرض المحادثات بين الأجهزة.

مفيد لمعرفة:

```text
من يتكلم مع من؟
أي اتصال أكبر؟
أي IP يرسل أكثر؟
```

---

# Endpoints

يعرض الأجهزة أو العناوين الموجودة في الـ Capture.

---

# Flow Graph

يعرض تسلسل الاتصال بشكل رسومي.

مفيد جدًا لرؤية:

```text
DNS
TCP Handshake
TLS Handshake
HTTP Request/Response
```

---

# Wireshark و Promiscuous Mode

Promiscuous Mode يجعل كرت الشبكة يحاول التقاط Packets ليست موجهة له فقط.

لكن في الشبكات الحديثة، خصوصًا مع Switches و Wi-Fi، قد لا ترى كل الترافيك الخاص بالأجهزة الأخرى.

غالبًا سترى:

```text
Traffic الخاص بجهازك
Broadcast
Multicast
بعض الترافيك المحلي
```

---

# لماذا لا أرى ترافيك كل الأجهزة؟

لأن الـ Switch لا يرسل كل الترافيك لكل المنافذ.

هو يرسل الـ Frames للمنفذ الصحيح فقط.

لرؤية ترافيك أجهزة أخرى تحتاج أشياء مثل:

```text
Port Mirroring
SPAN Port
Network TAP
Router Capture
```

وهذا يكون في بيئات تملك تصريحًا عليها.

---

# Wireshark على Wi-Fi

على Wi-Fi قد تحتاج Monitor Mode لرؤية تفاصيل 802.11 الكاملة.

لكن Monitor Mode ليس مدعومًا بسهولة على كل كروت الشبكة أو الأنظمة.

بدون Monitor Mode، غالبًا ترى الترافيك بعد اتصاله بالشبكة بشكل عادي.

---

# أخطاء شائعة

## الخطأ الأول

```text
Wireshark يظهر كل شيء في الشبكة دائمًا
```

غير صحيح.

غالبًا يظهر الترافيك الذي يصل إلى كرت الشبكة الخاص بك.

---

## الخطأ الثاني

```text
HTTPS يمكن قراءته بسهولة في Wireshark
```

غير صحيح.

HTTPS مشفر، وسترى غالبًا TLS Application Data فقط.

---

## الخطأ الثالث

```text
أي Packet لونها أسود تعني هجوم
```

غير صحيح.

الألوان تساعد، لكنها لا تعني وحدها وجود هجوم.

---

## الخطأ الرابع

```text
Retransmission دائمًا مشكلة خطيرة
```

ليس دائمًا.

القليل من Retransmissions طبيعي أحيانًا، لكن كثرتها قد تشير لمشكلة.

---

## الخطأ الخامس

```text
No ping reply يعني الجهاز مغلق
```

ليس دائمًا.

قد يكون ICMP ممنوعًا من Firewall.

---

# سيناريو عملي: الموقع لا يفتح

## الخطوات في Wireshark

ابدأ Capture ثم افتح الموقع.

افحص بالترتيب:

```text
1. dns
2. tcp.port == 443
3. tls
4. http إذا الموقع HTTP
```

---

## التفسير

لو لا يوجد DNS Response:

```text
DNS Problem
```

لو DNS موجود لكن لا يوجد SYN-ACK:

```text
TCP/Firewall/Routing Problem
```

لو TCP Handshake تم لكن TLS فشل:

```text
TLS/Certificate/Cipher Problem
```

لو TLS تم لكن الموقع يعطي 404 أو 500:

```text
HTTP/Application Problem
```

---

# سيناريو عملي: الإنترنت بطيء

ابحث عن:

```text
TCP Retransmissions
Duplicate ACKs
High latency
Large downloads
DNS delays
TLS delays
```

فلاتر مفيدة:

```text
tcp.analysis.retransmission
```

```text
tcp.analysis.duplicate_ack
```

```text
dns
```

---

# سيناريو عملي: DNS بطيء

فلتر:

```text
dns
```

راقب الوقت بين:

```text
DNS Query
DNS Response
```

لو الرد متأخر، جرب DNS آخر.

---

# سيناريو عملي: Port مغلق

لو حاولت الاتصال ببورت معين، راقب TCP.

## SYN ثم RST

```text
Port Closed / Connection Refused
```

## SYN ثم لا شيء

```text
Filtered / Firewall / Timeout
```

## SYN ثم SYN-ACK

```text
Port Open
```

---

# Wireshark مع Nmap

يمكنك تشغيل Wireshark أثناء فحص جهاز في Lab خاص بك باستخدام nmap.

مثال داخل شبكتك أو Lab:

```bash
nmap -sS 192.168.1.10
```

في Wireshark قد ترى:

```text
SYN
SYN-ACK
RST
```

استخدمه فقط على أجهزة تملك تصريحًا لفحصها.

---

# Mini Checklist للتحليل

عند فتح Capture جديد، اسأل نفسك:

```text
ما المشكلة؟
ما IP جهاز العميل؟
ما IP السيرفر؟
هل DNS نجح؟
هل TCP Handshake نجح؟
هل TLS نجح؟
هل HTTP رد؟
هل يوجد Retransmission؟
هل يوجد RST؟
هل يوجد ICMP Error؟
```

---

# أهم فلاتر للحفظ

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
tcp.analysis.flags
http.response.code == 404
dns.qry.name == "example.com"
```

---

# ملخص سريع

```text
Wireshark يحلل Packets
Capture يعني تسجيل الترافيك
Display Filter يفلتر العرض فقط
Capture Filter يحدد ما يتم تسجيله من البداية
ARP يربط IP مع MAC
DNS يحول Domain إلى IP
ICMP يظهر Ping
TCP يظهر Handshake و ACK و RST و FIN
TLS يظهر Handshake ثم Application Data
HTTP يظهر واضحًا فقط إذا غير مشفر
```

---

# أهم القواعد

```text
ابدأ دائمًا بفلاتر بسيطة
```

```text
DNS أولًا ثم TCP ثم TLS ثم HTTP
```

```text
SYN-ACK يعني غالبًا أن TCP Port مفتوح
```

```text
RST يعني غالبًا رفض أو Port مغلق
```

```text
Timeout قد يعني Firewall أو مشكلة طريق
```

```text
HTTPS لا يظهر محتواه بوضوح لأنه مشفر
```

```text
لا تحلل ترافيك لا تملك تصريحًا عليه
```

---

## Navigation

| Previous | Home | Next |
|---|---|---|
| [Networking Practical Labs](./Networking-Practical-Labs.md) | [README](./README.md) | [Common-Networking-Mistakes](./Common-Networking-Mistakes.md) |
