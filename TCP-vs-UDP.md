# TCP vs UDP

## الفرق بين TCP و UDP

في Layer 4 يوجد بروتوكولان مهمان جدًا:

```text
TCP
UDP
```

كلاهما يعمل في:

```text
Transport Layer
```

وكلاهما يستخدم:

```text
Port Numbers
```

لكن طريقة العمل مختلفة جدًا.

---

# الفكرة الأساسية

## TCP

TCP يهتم بالموثوقية.

يعني يحاول التأكد أن البيانات:

```text
وصلت
وصلت بالترتيب الصحيح
لم تضيع
لم تتكرر بشكل خاطئ
```

---

## UDP

UDP يهتم بالسرعة والبساطة.

يرسل البيانات بدون إنشاء اتصال مسبق، وبدون ضمان قوي أن البيانات وصلت.

```text
UDP = Send and forget
```

---

# TCP اختصار لـ

```text
Transmission Control Protocol
```

TCP يستخدم عندما تكون الدقة والموثوقية أهم من السرعة اللحظية.

أمثلة:

```text
Web Browsing
HTTPS
SSH
Email
File Transfer
Databases
```

---

# UDP اختصار لـ

```text
User Datagram Protocol
```

UDP يستخدم عندما تكون السرعة أو الزمن الحقيقي أهم من إعادة إرسال كل شيء.

أمثلة:

```text
DNS
DHCP
VoIP
Online Games
Video Streaming
Live Calls
QUIC / HTTP/3
```

---

# مقارنة سريعة

| المقارنة | TCP | UDP |
|---|---|---|
| نوع الاتصال | Connection-Oriented | Connectionless |
| يحتاج Handshake؟ | نعم | لا |
| الموثوقية | عالية | أقل |
| ترتيب البيانات | يضمن الترتيب | لا يضمن |
| إعادة الإرسال | نعم | لا بشكل افتراضي |
| السرعة | أبطأ نسبيًا | أسرع نسبيًا |
| Header | أكبر | أصغر |
| الاستخدام | الدقة أهم | السرعة أهم |

---

# Connection-Oriented vs Connectionless

## TCP Connection-Oriented

TCP ينشئ اتصالًا قبل إرسال البيانات.

يبدأ بـ:

```text
3-Way Handshake
```

```text
SYN → SYN-ACK → ACK
```

بعدها يصبح الاتصال:

```text
ESTABLISHED
```

ثم يبدأ نقل البيانات.

---

## UDP Connectionless

UDP لا ينشئ اتصالًا.

يرسل البيانات مباشرة.

```text
Client → Server: Data
```

لا يوجد:

```text
SYN
SYN-ACK
ACK
```

لذلك UDP أبسط وأسرع في البداية.

---

# مثال TCP

عندما تفتح موقع:

```text
https://example.com
```

غالبًا يحدث:

```text
1. DNS لمعرفة IP
2. TCP 3-Way Handshake على Port 443
3. TLS Handshake
4. HTTP Request
5. HTTP Response
```

TCP هنا مهم لأن المتصفح يحتاج الصفحة كاملة وبترتيب صحيح.

---

# مثال UDP

عندما جهازك يسأل DNS:

```text
ما هو IP الخاص بـ google.com؟
```

غالبًا يرسل UDP Query:

```text
Client → DNS Server: Query
DNS Server → Client: Response
```

لا يحتاج اتصال طويل.

لو لم يأت الرد، يمكن للجهاز أن يعيد السؤال مرة أخرى.

---

# TCP Header

TCP Header يحتوي على معلومات كثيرة لأنه يقدم موثوقية وتحكم.

أمثلة:

```text
Source Port
Destination Port
Sequence Number
Acknowledgment Number
Flags
Window Size
Checksum
Options
```

---

# UDP Header

UDP Header أبسط بكثير.

يحتوي غالبًا على:

```text
Source Port
Destination Port
Length
Checksum
```

لذلك UDP أخف من TCP.

---

# TCP يستخدم Sequence Numbers

TCP يستخدم:

```text
Sequence Number
```

حتى يعرف ترتيب البيانات.

مثال:

```text
Segment 1 = bytes 0-999
Segment 2 = bytes 1000-1999
Segment 3 = bytes 2000-2999
```

لو وصل Segment 3 قبل Segment 2، يستطيع TCP ترتيبهم بشكل صحيح.

---

# UDP لا يرتب البيانات

UDP لا يضمن وصول البيانات بالترتيب.

لو أرسلت:

```text
Packet 1
Packet 2
Packet 3
```

قد تصل هكذا:

```text
Packet 1
Packet 3
Packet 2
```

وقد لا تصل Packet 2 أصلًا.

التطبيق نفسه هو الذي يتعامل مع هذا إذا كان يحتاج ترتيبًا.

---

# TCP يستخدم ACK

TCP يستخدم:

```text
Acknowledgment
```

يعني الطرف المستقبل يقول:

```text
استلمت البيانات
أرسل التالي
```

مثال:

```text
Client sends data
Server sends ACK
```

لو لم يصل ACK، قد يعيد TCP إرسال البيانات.

---

# UDP لا يستخدم ACK افتراضيًا

UDP نفسه لا يحتوي على ACK مثل TCP.

يعني لو أرسلت UDP Packet، البروتوكول لا يؤكد لك أنها وصلت.

لكن التطبيق يمكن أن يبني نظام تأكيد خاص به فوق UDP.

مثال:

```text
Game Protocol
Voice App
QUIC
```

---

# TCP Retransmission

لو فقدت بيانات في TCP، يتم إعادة إرسالها.

مثال:

```text
Segment 1 وصل
Segment 2 ضاع
Segment 3 وصل
```

TCP يلاحظ أن Segment 2 مفقود، فيطلبه أو يعيد إرساله.

هذا يجعل TCP موثوقًا، لكنه قد يزيد التأخير.

---

# UDP بدون Retransmission افتراضي

في UDP، لو Packet ضاعت، غالبًا لا يتم إعادة إرسالها من البروتوكول نفسه.

هذا مفيد في الصوت والفيديو المباشر.

مثال:

لو أنت في مكالمة صوتية وضاع جزء صغير، الأفضل غالبًا تجاهله بدل إعادة إرساله بعد فوات الوقت.

---

# مثال مكالمة صوتية

في VoIP، لو ضاعت Packet صوتية صغيرة:

```text
أفضل أن تضيع لحظة صغيرة
بدل أن تتأخر المكالمة كلها
```

لذلك UDP مناسب للمكالمات.

---

# مثال تحميل ملف

لو تحمل ملف PDF أو برنامج، لا ينفع تضيع أجزاء من الملف.

لازم يصل كاملًا وبترتيب صحيح.

لذلك TCP مناسب لتحميل الملفات.

---

# TCP Flow Control

TCP يستخدم:

```text
Flow Control
```

حتى لا يرسل بيانات أسرع مما يستطيع الطرف الآخر استقبالها.

يعتمد على:

```text
Window Size
```

يعني المستقبل يخبر المرسل:

```text
أستطيع استقبال كمية معينة من البيانات
```

---

# TCP Congestion Control

TCP يحاول أيضًا عدم إغراق الشبكة.

هذا يسمى:

```text
Congestion Control
```

لو الشبكة مزدحمة، TCP يقلل سرعة الإرسال.

لو الشبكة جيدة، يزيد السرعة تدريجيًا.

---

# UDP لا يملك Congestion Control افتراضيًا

UDP لا يملك نفس نظام TCP للتحكم في الازدحام.

لذلك التطبيقات التي تستخدم UDP يجب أن تكون حذرة.

لو تطبيق UDP أرسل بسرعة كبيرة جدًا، قد يسبب ضغطًا على الشبكة.

بعض البروتوكولات الحديثة فوق UDP، مثل QUIC، تضيف Congestion Control داخل التطبيق أو البروتوكول الأعلى.

---

# TCP مناسب لماذا؟

TCP مناسب عندما تحتاج:

```text
موثوقية
ترتيب
إعادة إرسال
اتصال مستقر
نقل بيانات بدون فقد
```

أمثلة:

```text
HTTPS
SSH
FTP
SMTP
IMAP
POP3
Databases
APIs
File Downloads
```

---

# UDP مناسب لماذا؟

UDP مناسب عندما تحتاج:

```text
سرعة
تأخير قليل
بساطة
زمن حقيقي
عدم انتظار إعادة إرسال كل Packet
```

أمثلة:

```text
DNS
DHCP
VoIP
Video Calls
Online Gaming
Live Streaming
NTP
QUIC / HTTP/3
```

---

# أشهر Ports مع TCP

| Port | Service |
|---|---|
| 21 | FTP |
| 22 | SSH |
| 25 | SMTP |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 445 | SMB |
| 3389 | RDP |

---

# أشهر Ports مع UDP

| Port | Service |
|---|---|
| 53 | DNS |
| 67/68 | DHCP |
| 69 | TFTP |
| 123 | NTP |
| 161 | SNMP |
| 500 | IKE / VPN |
| 51820 | WireGuard |
| 443 | QUIC / HTTP/3 |

---

# DNS يستخدم TCP أم UDP؟

DNS غالبًا يستخدم:

```text
UDP Port 53
```

لأن الاستعلامات صغيرة وسريعة.

لكن DNS يمكن أن يستخدم:

```text
TCP Port 53
```

في حالات مثل:

```text
Zone Transfer
Large DNS Responses
DNSSEC
```

---

# HTTP/3 و QUIC

قديمًا HTTP/1.1 و HTTP/2 يعتمدان غالبًا على:

```text
TCP
```

لكن HTTP/3 يعتمد على:

```text
QUIC
```

وQUIC يعمل فوق:

```text
UDP
```

لكن QUIC يضيف مميزات كثيرة مثل:

```text
Reliability
Encryption
Congestion Control
Stream Management
```

يعني استخدام UDP لا يعني دائمًا أن التطبيق غير موثوق.  
قد يبني التطبيق أو البروتوكول الأعلى موثوقية فوق UDP.

---

# TCP في HTTPS

عند فتح موقع HTTPS تقليديًا:

```text
TCP Handshake
ثم TLS Handshake
ثم HTTP Request
```

مثال:

```text
Client → Server: SYN
Server → Client: SYN-ACK
Client → Server: ACK
```

بعدها يبدأ TLS.

---

# UDP في QUIC / HTTP/3

في HTTP/3، يتم استخدام UDP لتقليل التأخير وتحسين الأداء في بعض الحالات.

بدل الاعتماد على TCP + TLS بشكل منفصل، QUIC يدمج أشياء كثيرة في تصميمه.

لكن من المهم فهم الأساس:

```text
HTTP/3 uses QUIC
QUIC uses UDP
```

---

# TCP و NAT

TCP سهل التتبع بالنسبة للراوتر والفايروول لأن له حالات واضحة.

مثال:

```text
SYN
SYN-ACK
ACK
ESTABLISHED
FIN
RST
```

Stateful Firewall يستطيع معرفة حالة الاتصال.

---

# UDP و NAT

UDP لا يملك اتصالًا واضحًا، لذلك الراوتر ينشئ NAT Entry مؤقت.

مثال DNS:

```text
192.168.1.10:53000 → 8.8.8.8:53 UDP
```

الراوتر يحفظ هذا لفترة قصيرة.

لو لم يأت رد خلال وقت معين، يحذف الـ Entry.

---

# TCP و Firewall

Firewall يمكنه السماح بناءً على حالة الاتصال.

مثال:

```text
Allow outbound TCP 443
Allow return traffic for established connections
Block inbound TCP 3389
```

TCP واضح لأنه يبدأ بـ Handshake.

---

# UDP و Firewall

UDP أصعب قليلًا في التتبع لأنه لا يوجد Handshake.

Firewall يعتمد على:

```text
Source IP
Destination IP
Ports
Timeout
State Table
```

مثال:

```text
Allow outbound UDP 53
Allow reply from DNS server
```

---

# TCP Latency

TCP قد يكون أبطأ في البداية لأنه يحتاج:

```text
3-Way Handshake
```

ولو HTTPS:

```text
TCP Handshake + TLS Handshake
```

هذا يزيد زمن البداية.

لكن بعد إنشاء الاتصال، TCP يكون قويًا وموثوقًا.

---

# UDP Latency

UDP يبدأ بسرعة لأنه لا يحتاج Handshake.

لذلك مناسب للتطبيقات الحساسة للتأخير.

مثال:

```text
Online Gaming
Voice Calls
Live Video
```

---

# TCP Head-of-Line Blocking

في TCP، لو جزء من البيانات ضاع، قد ينتظر التطبيق حتى يتم إعادة إرسال الجزء المفقود.

هذا يسمى:

```text
Head-of-Line Blocking
```

يعني فقد جزء واحد قد يؤخر تسليم الأجزاء التي بعده للتطبيق.

---

# UDP وتجاوز بعض التأخير

UDP لا يجبر التطبيق على انتظار Packet مفقودة.

هذا مفيد في الصوت والألعاب.

لكن لو التطبيق يحتاج موثوقية، يجب أن يبنيها بنفسه.

---

# هل UDP غير آمن؟

UDP ليس غير آمن بذاته.

هو فقط أبسط من TCP.

الأمان يعتمد على البروتوكول الأعلى.

مثال:

```text
QUIC يستخدم UDP لكنه مشفر
DNS العادي غير مشفر غالبًا
DoH يستخدم HTTPS
DoT يستخدم TLS
```

---

# هل TCP دائمًا أفضل؟

لا.

TCP ممتاز للموثوقية، لكنه ليس الأفضل لكل شيء.

لو عندك مكالمة مباشرة، إعادة إرسال Packet قديمة قد لا تفيد.

مثال:

```text
Packet صوتية من 3 ثواني فاتت لم تعد مهمة
```

هنا UDP قد يكون أفضل.

---

# هل UDP دائمًا أسرع؟

UDP أخف وأسرع في البداية، لكنه ليس دائمًا أفضل أداء.

لو التطبيق فوق UDP غير مصمم جيدًا، قد يسبب فقدًا أو ازدحامًا.

الأداء يعتمد على:

```text
التطبيق
الشبكة
Packet Loss
Latency
Congestion Control
```

---

# مثال عملي: تحميل ملف

عند تحميل ملف:

```text
file.zip
```

نحتاج:

```text
كل البيانات
بنفس الترتيب
بدون فقد
```

لذلك TCP مناسب.

لو ضاع جزء، TCP يعيده.

---

# مثال عملي: لعبة أونلاين

في لعبة أونلاين، معلومات موقع اللاعب تتغير بسرعة.

لو ضاعت Packet قديمة فيها مكان اللاعب قبل ثانية، قد لا يكون مهمًا إعادة إرسالها.

الأفضل إرسال التحديث الأحدث.

لذلك UDP مناسب لكثير من الألعاب.

---

# مثال عملي: DNS

DNS Query صغيرة.

لو ضاعت، الجهاز يعيد السؤال.

لذلك UDP مناسب.

```text
Query → Response
```

بدل إنشاء TCP Connection كامل لكل سؤال صغير.

---

# مثال عملي: SSH

SSH يحتاج اتصال مستقر وآمن.

لا ينفع تضيع أوامر أو تظهر بترتيب خاطئ.

لذلك SSH يستخدم:

```text
TCP Port 22
```

---

# TCP Termination

عند إنهاء اتصال TCP بشكل طبيعي، يستخدم:

```text
FIN
ACK
```

مثال مبسط:

```text
Client → Server: FIN
Server → Client: ACK
Server → Client: FIN
Client → Server: ACK
```

هذا إنهاء منظم.

---

# TCP Reset

لو الاتصال تم رفضه أو قطعه فجأة، قد يظهر:

```text
RST
```

مثال:

```text
Port closed
Connection refused
Application crashed
Firewall sends reset
```

---

# UDP لا يوجد Termination

لأن UDP لا ينشئ اتصالًا أصلاً، لا يوجد إنهاء اتصال مثل TCP.

لا يوجد:

```text
FIN
RST
```

التطبيق فقط يتوقف عن إرسال البيانات.

---

# Troubleshooting TCP

لو خدمة TCP لا تعمل، افحص:

```text
هل DNS يعمل؟
هل السيرفر reachable؟
هل البورت مفتوح؟
هل يوجد SYN-ACK؟
هل Firewall يمنع؟
هل TLS بعد TCP يفشل؟
```

أدوات مفيدة:

```bash
nc -vz example.com 443
```

```powershell
Test-NetConnection example.com -Port 443
```

---

# Troubleshooting UDP

UDP أصعب في الفحص لأن عدم الرد لا يعني دائمًا أن البورت مغلق.

مثال:

```text
UDP packet sent
No response
```

قد يكون السبب:

```text
الخدمة لا ترد
Firewall يمنع
Packet ضاعت
البروتوكول يحتاج Payload صحيح
```

أداة مثل nmap قد تظهر:

```text
open|filtered
```

لأن UDP لا يرد دائمًا.

---

# أوامر مفيدة

## Windows

اختبار TCP Port:

```powershell
Test-NetConnection google.com -Port 443
```

عرض الاتصالات:

```cmd
netstat -ano
```

---

## Linux

اختبار TCP:

```bash
nc -vz google.com 443
```

عرض TCP و UDP Sockets:

```bash
ss -tunap
```

عرض Listening Ports:

```bash
ss -tuln
```

---

## macOS

اختبار TCP:

```bash
nc -vz google.com 443
```

عرض الاتصالات:

```bash
netstat -an
```

---

# مقارنة في جملة واحدة

```text
TCP = موثوق لكنه أثقل
UDP = أسرع وأبسط لكنه لا يضمن الوصول افتراضيًا
```

---

# أخطاء شائعة

## الخطأ الأول

```text
UDP لا يستخدم Ports
```

غير صحيح.

UDP يستخدم Ports مثل TCP.

---

## الخطأ الثاني

```text
TCP آمن و UDP غير آمن
```

غير دقيق.

TCP و UDP لا يعنيان التشفير.  
الأمان يعتمد على بروتوكولات مثل TLS أو QUIC أو VPN.

---

## الخطأ الثالث

```text
UDP دائمًا أفضل لأنه أسرع
```

غير صحيح.

لو تحتاج موثوقية وترتيب، TCP أفضل غالبًا.

---

## الخطأ الرابع

```text
TCP لا يضيع منه أي شيء أبدًا
```

TCP قد يحدث فيه Packet Loss في الشبكة، لكنه يكتشف الفقد ويعيد الإرسال.

---

## الخطأ الخامس

```text
DNS يستخدم UDP فقط
```

غير صحيح.

DNS غالبًا UDP، لكنه يمكن أن يستخدم TCP أيضًا.

---

# ملخص سريع

```text
TCP = Transmission Control Protocol
UDP = User Datagram Protocol

TCP:
Connection-Oriented
Reliable
Ordered
Uses ACK
Uses Retransmission
Uses Flow Control
Uses Congestion Control

UDP:
Connectionless
No built-in reliability
No built-in ordering
No handshake
Lower overhead
Good for real-time traffic
```

---

# أهم القواعد

```text
لو البيانات لازم توصل كاملة وبالترتيب → TCP
```

```text
لو السرعة والزمن الحقيقي أهم من إعادة الإرسال → UDP
```

```text
TCP يحتاج 3-Way Handshake
```

```text
UDP يرسل مباشرة بدون Handshake
```

```text
TCP و UDP كلاهما يستخدم Ports
```

```text
ICMP لا يستخدم Ports
```

```text
UDP يمكن أن يكون موثوقًا إذا البروتوكول الأعلى أضاف الموثوقية مثل QUIC
```

---

## Next Topic

اقرأ بعد ذلك:

[TLS Handshake and Encryption](./TLS-Handshake-and-Encryption.md)
