# Ports and Sockets

## كيف يعرف الجهاز أي برنامج يستقبل البيانات؟

في الدروس السابقة عرفنا أن:

```text
IP Address = يحدد الجهاز أو السيرفر
MAC Address = يحدد الجهاز داخل الشبكة المحلية
```

لكن يوجد سؤال مهم:

```text
لو وصلت البيانات إلى جهازك، كيف يعرف الجهاز أي برنامج يستقبلها؟
```

مثال:

في نفس اللحظة أنت قد تستخدم:

```text
Browser
WhatsApp
Telegram
Game
DNS Client
VPN
```

كل هذه البرامج تستخدم الإنترنت.

إذن كيف يعرف نظام التشغيل أن هذه البيانات تخص المتصفح وليس واتساب؟

الإجابة هي:

```text
Port Number
```

---

# ما هو Port؟

الـ Port هو رقم يستخدم في Layer 4 لتحديد التطبيق أو الخدمة داخل الجهاز.

يعني:

```text
IP Address يوصلك إلى الجهاز
Port Number يوصلك إلى التطبيق داخل الجهاز
```

مثال:

```text
142.250.80.46:443
```

هنا:

```text
142.250.80.46 = IP Address
443           = Port Number
```

يعني الاتصال ذاهب إلى خدمة تعمل على Port 443.

---

# تشبيه بسيط

تخيل أن IP Address هو عنوان عمارة.

داخل العمارة يوجد شقق كثيرة.

```text
IP Address = عنوان العمارة
Port       = رقم الشقة
Application = الشخص الموجود داخل الشقة
```

لو أرسلت رسالة إلى عنوان العمارة فقط، لن يعرف البواب لأي شقة يرسلها.

لازم يكون معك رقم الشقة.

في الشبكات:

```text
IP يحدد الجهاز
Port يحدد البرنامج أو الخدمة
```

---

# Ports تعمل في أي طبقة؟

الـ Ports تعمل في:

```text
Layer 4 - Transport Layer
```

وتستخدم مع:

```text
TCP
UDP
```

مثال:

```text
TCP Port 443 = HTTPS
UDP Port 53  = DNS غالبًا
UDP Port 67/68 = DHCP
```

مهم جدًا:

```text
ICMP لا يستخدم Ports
```

لذلك لا يوجد شيء اسمه:

```text
ICMP Port 8
```

---

# Source Port و Destination Port

أي اتصال في TCP أو UDP غالبًا يحتوي على:

```text
Source Port
Destination Port
```

---

## Destination Port

هو البورت الخاص بالخدمة التي تريد الوصول إليها.

مثال عند فتح موقع HTTPS:

```text
Destination Port = 443
```

لأن السيرفر يستمع على Port 443 لخدمة HTTPS.

---

## Source Port

هو بورت مؤقت يفتحه جهازك للاتصال.

مثال:

```text
Source Port = 51544
```

هذا الرقم يختاره نظام التشغيل مؤقتًا حتى يميز اتصالك عن باقي الاتصالات.

---

# مثال عملي

أنت فتحت:

```text
https://google.com
```

بعد DNS، جهازك عرف IP الخاص بجوجل.

ثم بدأ اتصال مثل:

```text
Source IP        = 192.168.1.10
Source Port      = 51544
Destination IP   = 142.250.80.46
Destination Port = 443
Protocol         = TCP
```

معنى ذلك:

```text
جهازي 192.168.1.10 يستخدم بورت مؤقت 51544
ويتصل بسيرفر جوجل على بورت 443
باستخدام TCP
```

---

# لماذا نحتاج Source Port؟

لأن جهازك قد يفتح أكثر من اتصال في نفس الوقت.

مثال:

```text
Tab 1 في المتصفح يفتح google.com
Tab 2 في المتصفح يفتح youtube.com
Tab 3 في المتصفح يفتح github.com
```

كلها قد تستخدم Destination Port 443.

لكن جهازك يميزها باستخدام Source Port مختلف لكل اتصال.

مثال:

```text
192.168.1.10:51544 → google.com:443
192.168.1.10:51545 → youtube.com:443
192.168.1.10:51546 → github.com:443
```

---

# شكل Layer 4 Header

في TCP أو UDP Header يوجد:

```text
Source Port
Destination Port
```

مثال مبسط:

```text
┌─────────────┬──────────────────┬──────────────┐
│ Source Port │ Destination Port │ Data         │
└─────────────┴──────────────────┴──────────────┘
```

ثم يتم تغليف هذا داخل IP Packet:

```text
┌───────────┬─────────────┬──────────────────┬──────┐
│ IP Header │ Source Port │ Destination Port │ Data │
└───────────┴─────────────┴──────────────────┴──────┘
```

---

# عدد البورتات

Port Number يتكون من 16 bit.

لذلك عدد البورتات يكون:

```text
0 إلى 65535
```

يعني إجمالي:

```text
65536 Ports
```

لكنها مقسمة إلى فئات.

---

# تصنيفات Ports

## 1. Well-Known Ports

هذه البورتات من:

```text
0 إلى 1023
```

تستخدمها الخدمات المشهورة والأساسية.

أمثلة:

| Port | Protocol | Service |
|---|---|---|
| 20/21 | TCP | FTP |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67/68 | UDP | DHCP |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 123 | UDP | NTP |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |

---

## 2. Registered Ports

هذه البورتات من:

```text
1024 إلى 49151
```

تستخدمها تطبيقات وخدمات كثيرة.

أمثلة:

| Port | Service |
|---|---|
| 1433 | Microsoft SQL Server |
| 3306 | MySQL |
| 3389 | RDP |
| 5432 | PostgreSQL |
| 6379 | Redis |
| 8080 | Alternative HTTP |
| 27017 | MongoDB |

---

## 3. Dynamic / Ephemeral Ports

هذه البورتات من:

```text
49152 إلى 65535
```

غالبًا يستخدمها جهاز العميل كـ Source Ports مؤقتة.

مثال:

```text
Laptop Source Port = 51544
Phone Source Port  = 53012
```

يعني جهازك لا يستخدم Port 443 كـ Source Port عادة عند فتح موقع HTTPS.

هو يستخدم Port مؤقت، ويرسل إلى Destination Port 443.

---

# Client Port vs Server Port

## Server Port

السيرفر يستمع على Port معروف.

مثال:

```text
Web Server listens on TCP 80 and TCP 443
DNS Server listens on UDP/TCP 53
SSH Server listens on TCP 22
```

---

## Client Port

العميل يستخدم Port مؤقت.

مثال:

```text
Client 192.168.1.10:51544 → Server 142.250.80.46:443
```

هنا:

```text
51544 = Source Port مؤقت
443   = Destination Port ثابت للخدمة
```

---

# Listening Port

عندما تكون خدمة تعمل على جهاز وتنتظر اتصالات، نقول إنها:

```text
Listening
```

مثال:

```text
Web Server listening on 0.0.0.0:80
SSH Server listening on 192.168.1.20:22
```

معنى:

```text
0.0.0.0:80
```

أن الخدمة تستمع على كل عناوين الجهاز على Port 80.

---

# Established Connection

عندما يتم الاتصال بين جهازين بنجاح، يصبح الاتصال:

```text
Established
```

مثال:

```text
192.168.1.10:51544 → 142.250.80.46:443 ESTABLISHED
```

يعني يوجد اتصال نشط بين جهازك والسيرفر.

---

# ما هو Socket؟

Socket هو نقطة اتصال بين IP و Port.

ببساطة:

```text
Socket = IP Address + Port Number
```

مثال:

```text
192.168.1.10:51544
142.250.80.46:443
```

لكن في الفهم الأدق، يجب أيضًا معرفة البروتوكول:

```text
TCP أو UDP
```

لأن:

```text
TCP 53
```

ليس نفس:

```text
UDP 53
```

---

# Socket Pair

الاتصال الكامل بين جهازين يمكن وصفه بزوج من الـ Sockets.

مثال:

```text
Client Socket = 192.168.1.10:51544
Server Socket = 142.250.80.46:443
```

الاتصال الكامل:

```text
192.168.1.10:51544 → 142.250.80.46:443
```

---

# ما هو 5-Tuple؟

لتمييز أي اتصال بشكل دقيق، نستخدم 5 عناصر تسمى:

```text
5-Tuple
```

وهي:

```text
Source IP
Source Port
Destination IP
Destination Port
Protocol
```

مثال:

```text
Source IP        = 192.168.1.10
Source Port      = 51544
Destination IP   = 142.250.80.46
Destination Port = 443
Protocol         = TCP
```

هذه الخمسة عناصر تميز الاتصال عن غيره.

---

# لماذا 5-Tuple مهم؟

لأن جهازك أو الراوتر قد يتعامل مع آلاف الاتصالات في نفس الوقت.

مثال:

```text
اتصال مع Google
اتصال مع YouTube
اتصال مع GitHub
اتصال مع Discord
اتصال مع DNS
```

كل اتصال له 5-Tuple مختلف.

الراوتر والفايروول وNAT يستخدمون هذه المعلومات لفهم الترافيك.

---

# مثال أكثر من اتصال لنفس السيرفر

تخيل أن جهازك فتح اتصالين مع نفس السيرفر:

```text
192.168.1.10:51544 → 142.250.80.46:443 TCP
192.168.1.10:51545 → 142.250.80.46:443 TCP
```

نفس:

```text
Source IP
Destination IP
Destination Port
Protocol
```

لكن Source Port مختلف.

لذلك النظام يعرف أن هذان اتصالان مختلفان.

---

# نفس Port مع TCP و UDP

نفس رقم البورت يمكن استخدامه مع TCP وUDP.

مثال:

```text
TCP 53
UDP 53
```

كلاهما DNS، لكنهما ليسا نفس الاتصال.

مثال آخر:

```text
TCP 443 = HTTPS
UDP 443 = HTTP/3 / QUIC
```

إذن لازم دائمًا تعرف:

```text
Port + Protocol
```

وليس Port فقط.

---

# Ports و DNS

DNS غالبًا يستخدم:

```text
UDP Port 53
```

لأن DNS Query صغيرة وسريعة.

لكن قد يستخدم:

```text
TCP Port 53
```

في حالات مثل:

```text
Zone Transfer
Large DNS Responses
DNSSEC
```

مثال:

```text
Client: 192.168.1.10:53000
DNS:    8.8.8.8:53
Protocol: UDP
```

---

# Ports و DHCP

DHCP يستخدم UDP.

البورتات:

```text
DHCP Server = UDP 67
DHCP Client = UDP 68
```

في بداية الاتصال، الجهاز لا يملك IP.

لذلك يرسل DHCP Discover كـ Broadcast.

مثال:

```text
Source IP        = 0.0.0.0
Source Port      = UDP 68
Destination IP   = 255.255.255.255
Destination Port = UDP 67
```

---

# Ports و HTTP/HTTPS

## HTTP

HTTP غير مشفر غالبًا يستخدم:

```text
TCP Port 80
```

مثال:

```text
http://example.com
```

---

## HTTPS

HTTPS يستخدم غالبًا:

```text
TCP Port 443
```

مثال:

```text
https://example.com
```

في HTTPS، HTTP يكون داخل TLS.

يعني المحتوى مشفر.

---

# Ports و SSH

SSH يستخدم غالبًا:

```text
TCP Port 22
```

مثال:

```bash
ssh user@192.168.1.20
```

SSH يستخدم للدخول الآمن على السيرفرات.

---

# Ports و RDP

RDP يستخدم غالبًا:

```text
TCP Port 3389
```

ويستخدم للدخول إلى Windows Remote Desktop.

فتح هذا البورت للإنترنت مباشرة خطر جدًا إذا لم يكن مؤمنًا جيدًا.

الأفضل استخدام:

```text
VPN
Firewall Rules
Strong Passwords
MFA
```

---

# Ports و NAT/PAT

في درس NAT عرفنا أن PAT يستخدم البورتات لتمييز الأجهزة الداخلية.

مثال:

```text
192.168.1.10:51544 → 41.33.20.5:62001
192.168.1.11:51544 → 41.33.20.5:62002
```

لاحظ أن الجهازين استخدما نفس Source Port داخليًا، لكن الراوتر غير البورت الخارجي حتى يميز الردود.

---

# Ports و Port Forwarding

لو تريد أن يصل شخص من الإنترنت إلى خدمة داخل شبكتك، تحتاج غالبًا Port Forwarding.

مثال:

```text
Public IP:80 → 192.168.1.20:80
```

أو:

```text
Public IP:2222 → 192.168.1.20:22
```

هذا يعني:

```text
أي اتصال يأتي إلى Public IP على Port 2222
أرسله إلى السيرفر الداخلي على Port 22
```

---

# Ports و Firewall

Firewall يمكنه السماح أو المنع بناءً على:

```text
Source IP
Destination IP
Source Port
Destination Port
Protocol
Direction
```

مثال Rules:

```text
Allow TCP 443 outbound
Block TCP 3389 inbound
Allow UDP 53 to DNS Server
Block TCP 23 Telnet
```

---

# Inbound و Outbound

## Outbound Traffic

يعني اتصال يبدأ من جهازك إلى الخارج.

مثال:

```text
Laptop → Google:443
```

غالبًا مسموح في الشبكات المنزلية.

---

## Inbound Traffic

يعني اتصال يبدأ من الخارج إلى جهازك.

مثال:

```text
Internet → Your PC:3389
```

غالبًا يتم منعه إلا إذا كان هناك Port Forwarding أو Rule واضحة.

---

# Open, Closed, Filtered Ports

عند فحص Port، قد يظهر بثلاث حالات:

## Open

يعني توجد خدمة تستمع على هذا البورت.

مثال:

```text
TCP 22 open
```

يعني SSH يعمل.

---

## Closed

يعني الجهاز موجود، لكن لا توجد خدمة تستمع على هذا البورت.

مثال:

```text
TCP 25 closed
```

---

## Filtered

يعني لا نعرف هل البورت مفتوح أو مغلق لأن Firewall يمنع الرد.

مثال:

```text
TCP 3389 filtered
```

---

# Port Scanning

Port Scanning يعني فحص جهاز لمعرفة البورتات المفتوحة.

يستخدم في:

```text
Troubleshooting
Security Auditing
Pentesting
Inventory
```

لكن يجب استخدامه فقط على الشبكات والأجهزة التي تملك تصريحًا لفحصها.

مثال أداة مشهورة:

```text
nmap
```

مثال:

```bash
nmap -sS 192.168.1.10
```

---

# لماذا البورتات المفتوحة مهمة أمنيًا؟

كل Port مفتوح يعني خدمة يمكن الوصول إليها.

لو الخدمة قديمة أو بها ثغرة، قد يتم استغلالها.

أمثلة خدمات يجب الحذر معها:

```text
Telnet 23
SMB 445
RDP 3389
Databases مثل MySQL 3306
Admin Panels على 8080 أو 8443
```

قاعدة مهمة:

```text
افتح فقط البورتات التي تحتاجها
وأغلق أي شيء غير مستخدم
```

---

# Port ليس دائمًا معناه الخدمة الحقيقية

رغم أن Port 443 غالبًا HTTPS، لكن يمكن لأي برنامج أن يعمل على أي Port إذا تم ضبطه.

مثال:

```text
SSH يمكن تشغيله على Port 2222
Web Server يمكن تشغيله على Port 8080
```

لذلك Port يعطيك مؤشرًا، لكنه ليس دليلًا نهائيًا دائمًا.

---

# أكثر Ports يجب حفظها

| Port | Protocol | Service |
|---|---|---|
| 20/21 | TCP | FTP |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67 | UDP | DHCP Server |
| 68 | UDP | DHCP Client |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 123 | UDP | NTP |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP |
| 389 | TCP/UDP | LDAP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |
| 993 | TCP | IMAPS |
| 995 | TCP | POP3S |
| 1433 | TCP | MSSQL |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP |
| 5432 | TCP | PostgreSQL |
| 6379 | TCP | Redis |
| 8080 | TCP | Alternative HTTP |

---

# مثال عملي: فتح موقع HTTPS

جهازك:

```text
IP Address = 192.168.1.10
```

يريد فتح:

```text
https://example.com
```

الخطوات:

```text
1. DNS يحول example.com إلى IP
2. المتصفح يطلب اتصال TCP مع Destination Port 443
3. نظام التشغيل يختار Source Port مؤقت مثل 51544
4. يتم TCP 3-Way Handshake
5. يبدأ TLS Handshake
6. يتم إرسال HTTP Request داخل TLS
```

الاتصال قد يبدو هكذا:

```text
192.168.1.10:51544 → 93.184.216.34:443 TCP
```

---

# مثال عملي: DNS Query

جهازك يريد معرفة IP الخاص بـ:

```text
google.com
```

الاتصال:

```text
192.168.1.10:53000 → 8.8.8.8:53 UDP
```

هنا:

```text
53000 = Source Port مؤقت
53    = Destination Port الخاص بالـ DNS
UDP   = البروتوكول
```

---

# مثال عملي: DHCP

جهاز جديد دخل الشبكة ولا يملك IP.

يرسل:

```text
0.0.0.0:68 → 255.255.255.255:67 UDP
```

معناه:

```text
أنا جهاز جديد
أحتاج DHCP Server يعطيني إعدادات الشبكة
```

---

# أوامر مفيدة

## Windows

عرض الاتصالات:

```cmd
netstat -ano
```

عرض الاتصالات مع أسماء البرامج:

```powershell
Get-NetTCPConnection
```

اختبار Port معين:

```powershell
Test-NetConnection google.com -Port 443
```

---

## Linux

عرض Listening Ports:

```bash
ss -tuln
```

عرض الاتصالات:

```bash
ss -tunap
```

معرفة البرنامج الذي يستخدم Port معين:

```bash
sudo lsof -i :443
```

اختبار اتصال ببورت:

```bash
nc -vz google.com 443
```

---

## macOS

عرض Listening Ports:

```bash
lsof -i -P -n
```

اختبار Port:

```bash
nc -vz google.com 443
```

---

# كيف تعرف البرنامج الذي يستخدم Port؟

## Windows

```cmd
netstat -ano
```

سيظهر PID.

بعدها:

```cmd
tasklist | findstr <PID>
```

---

## Linux

```bash
sudo lsof -i :80
```

أو:

```bash
sudo ss -ltnp
```

---

# مشاكل شائعة

## 1. الخدمة لا تعمل

قد يكون السبب:

```text
البرنامج لا يستمع على البورت
Firewall يمنع البورت
الخدمة توقفت
IP خطأ
Port خطأ
```

---

## 2. Port مستخدم بالفعل

لو حاولت تشغيل Web Server على Port 80، وظهر خطأ:

```text
Address already in use
```

فهذا يعني أن برنامجًا آخر يستخدم نفس Port.

الحل:

```text
إغلاق البرنامج الآخر
أو تغيير Port الخدمة
```

---

## 3. الخدمة تعمل محليًا لكن لا تعمل من الخارج

افحص:

```text
Firewall
NAT / Port Forwarding
Public IP
CGNAT
Service Binding
```

قد تكون الخدمة تستمع فقط على:

```text
127.0.0.1
```

وهذا يعني أنها متاحة من نفس الجهاز فقط.

---

## 4. الموقع لا يفتح رغم أن IP صحيح

قد تكون المشكلة في:

```text
Port 443 محجوب
Firewall
Proxy
TLS مشكلة
Server لا يستمع
```

---

## 5. DNS يعمل لكن HTTP لا يعمل

قد ترى أن:

```text
nslookup google.com
```

يعمل، لكن الموقع لا يفتح.

هذا يعني أن DNS نجح، لكن المشكلة قد تكون في:

```text
TCP 443
Firewall
Proxy
TLS
Routing
```

---

# 127.0.0.1 و localhost

العنوان:

```text
127.0.0.1
```

يعني جهازك نفسه.

يسمى:

```text
localhost
```

مثال:

```text
127.0.0.1:3000
```

يعني خدمة تعمل على جهازك أنت على Port 3000.

هذا شائع في البرمجة والتجارب المحلية.

---

# 0.0.0.0 في Listening

لو خدمة تستمع على:

```text
0.0.0.0:80
```

هذا يعني أنها تستقبل الاتصالات على كل عناوين الجهاز.

لكن لو تستمع على:

```text
127.0.0.1:80
```

فهي تستقبل من نفس الجهاز فقط.

---

# Ports في البرمجة

عندما يكتب المبرمج Server، غالبًا يعمل:

```text
Listen on a Port
```

مثال Web App:

```text
Server listens on port 3000
```

ثم تفتح:

```text
http://localhost:3000
```

معناه:

```text
اتصل بجهازي نفسه على Port 3000
```

---

# Ports في Docker

في Docker، قد يكون التطبيق داخل Container يستمع على Port معين.

مثال:

```text
Container Port = 80
Host Port      = 8080
```

التشغيل قد يكون:

```bash
docker run -p 8080:80 nginx
```

معناه:

```text
افتح Port 8080 على الجهاز
وحوله إلى Port 80 داخل الـ Container
```

---

# Ports و Security Best Practices

نصائح مهمة:

```text
أغلق أي Port غير مستخدم
لا تفتح RDP أو SSH للإنترنت بدون حماية
استخدم Firewall
استخدم VPN للإدارة عن بعد
استخدم كلمات مرور قوية
حدّث الخدمات باستمرار
راقب Logs
لا تعتمد على تغيير رقم Port فقط كحماية
```

تغيير Port قد يقلل الضوضاء، لكنه لا يعتبر حماية كافية.

---

# ملخص سريع

```text
Port = رقم يحدد التطبيق أو الخدمة داخل الجهاز
Ports تعمل في Layer 4
TCP و UDP يستخدمان Ports
IP يحدد الجهاز
Port يحدد الخدمة
Socket = IP + Port
5-Tuple = Source IP + Source Port + Destination IP + Destination Port + Protocol
Server يستخدم Listening Port
Client يستخدم Ephemeral Source Port
Firewall يمكنه السماح أو المنع حسب Ports
NAT/PAT يستخدم Ports لتمييز الاتصالات
```

---

# أهم القواعد

```text
IP Address يوصلك للجهاز
Port Number يوصلك للتطبيق
```

```text
Destination Port يحدد الخدمة المطلوبة
Source Port يميز اتصال العميل
```

```text
نفس رقم Port يمكن استخدامه مع TCP و UDP
```

```text
Port مفتوح = خدمة تستمع
Port مغلق = لا توجد خدمة
Port filtered = Firewall يمنع أو يخفي الرد
```

```text
لا تفتح Ports للإنترنت إلا عند الحاجة ومع تأمين جيد
```

---

## Next Topic

اقرأ بعد ذلك:

[TCP 3-Way Handshake](./TCP-3-Way-Handshake.md)
