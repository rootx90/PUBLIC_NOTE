# TCP 3-Way Handshake

## كيف يبدأ اتصال TCP؟

عندما تفتح موقعًا أو تتصل بسيرفر باستخدام TCP، الاتصال لا يبدأ بإرسال البيانات مباشرة.

قبل إرسال البيانات، لازم الطرفين يتفقوا أن الاتصال جاهز.

هذه العملية اسمها:

```text
TCP 3-Way Handshake
```

ومعناها:

```text
مصافحة من 3 خطوات لإنشاء اتصال TCP
```

---

# ما هو TCP؟

TCP اختصار لـ:

```text
Transmission Control Protocol
```

وهو بروتوكول يعمل في:

```text
Layer 4 - Transport Layer
```

TCP يستخدم عندما نحتاج اتصال موثوق.

يعني يهتم بـ:

```text
وصول البيانات
ترتيب البيانات
إعادة إرسال البيانات المفقودة
التحكم في سرعة الإرسال
إنشاء اتصال قبل نقل البيانات
```

---

# لماذا نحتاج Handshake؟

لأن TCP بروتوكول Connection-Oriented.

يعني قبل نقل البيانات، لازم يتم إنشاء اتصال بين:

```text
Client
Server
```

الـ Handshake يتأكد من:

```text
العميل موجود وجاهز
السيرفر موجود وجاهز
الطرفان يعرفان أرقام Sequence Numbers
يمكن بدء إرسال البيانات
```

---

# خطوات TCP 3-Way Handshake

الخطوات الثلاثة هي:

```text
1. SYN
2. SYN-ACK
3. ACK
```

بشكل مبسط:

```text
Client → Server : SYN
Server → Client : SYN-ACK
Client → Server : ACK
```

بعدها يصبح الاتصال:

```text
Established
```

---

# رسم مبسط

```text
Client                                Server
  │                                      │
  │ SYN                                  │
  │─────────────────────────────────────>│
  │                                      │
  │ SYN-ACK                              │
  │<─────────────────────────────────────│
  │                                      │
  │ ACK                                  │
  │─────────────────────────────────────>│
  │                                      │
  │ Connection Established               │
```

---

# الخطوة الأولى: SYN

العميل يبدأ الاتصال ويرسل Packet فيها Flag اسمها:

```text
SYN
```

SYN معناها:

```text
Synchronize
```

أي أن العميل يريد مزامنة الاتصال وبدء جلسة TCP.

مثال:

```text
Client → Server
SYN
```

العميل يقول للسيرفر:

```text
أريد فتح اتصال TCP معك
وهذا هو Sequence Number الخاص بي
```

---

## مثال SYN

```text
Source IP        = 192.168.1.10
Source Port      = 51544
Destination IP   = 142.250.80.46
Destination Port = 443
TCP Flag         = SYN
Sequence Number  = 1000
```

معنى هذا:

```text
جهازي يريد الاتصال بالسيرفر على Port 443
وسأبدأ Sequence Number من 1000
```

---

# الخطوة الثانية: SYN-ACK

السيرفر يستقبل SYN.

إذا كان السيرفر مستعدًا لقبول الاتصال، يرد برسالة تحتوي على:

```text
SYN
ACK
```

وتسمى:

```text
SYN-ACK
```

السيرفر يقول:

```text
وصلني طلبك
وأنا أيضًا أريد مزامنة الاتصال معك
وهذا هو Sequence Number الخاص بي
```

---

## مثال SYN-ACK

```text
Source IP        = 142.250.80.46
Source Port      = 443
Destination IP   = 192.168.1.10
Destination Port = 51544
TCP Flag         = SYN, ACK
Sequence Number  = 5000
Acknowledgment   = 1001
```

لاحظ:

```text
Acknowledgment = 1001
```

لأن العميل أرسل:

```text
Sequence Number = 1000
```

والسيرفر يقول:

```text
أنا استلمت رقم 1000
وأنتظر الرقم التالي 1001
```

---

# الخطوة الثالثة: ACK

العميل يستقبل SYN-ACK.

ثم يرسل ACK أخير إلى السيرفر.

```text
Client → Server
ACK
```

العميل يقول:

```text
وصلني ردك
وأنا جاهز لبدء الاتصال
```

بعد هذه الخطوة، يصبح الاتصال مفتوحًا.

---

## مثال ACK

```text
Source IP        = 192.168.1.10
Source Port      = 51544
Destination IP   = 142.250.80.46
Destination Port = 443
TCP Flag         = ACK
Sequence Number  = 1001
Acknowledgment   = 5001
```

لاحظ:

```text
Acknowledgment = 5001
```

لأن السيرفر أرسل:

```text
Sequence Number = 5000
```

والعميل يقول:

```text
استلمت 5000
وأنتظر 5001
```

---

# مثال كامل بالأرقام

```text
Client                                Server
  │                                      │
  │ SYN, Seq=1000                       │
  │─────────────────────────────────────>│
  │                                      │
  │ SYN-ACK, Seq=5000, Ack=1001          │
  │<─────────────────────────────────────│
  │                                      │
  │ ACK, Seq=1001, Ack=5001              │
  │─────────────────────────────────────>│
  │                                      │
  │ TCP Connection Established           │
```

---

# معنى Sequence Number

Sequence Number هو رقم يستخدمه TCP لترتيب البيانات.

TCP لا يرسل البيانات ككتلة واحدة دائمًا.

قد يتم تقسيم البيانات إلى أجزاء.

Sequence Number يساعد الطرف الآخر يعرف:

```text
ترتيب البيانات
هل هناك جزء ناقص؟
ما هو الجزء التالي المتوقع؟
```

---

# معنى Acknowledgment Number

Acknowledgment Number يعني:

```text
أنا استلمت البيانات حتى رقم معين
وأنتظر الرقم التالي
```

مثال:

```text
Seq = 1000
Data Length = 500
```

إذن الطرف الآخر يرد:

```text
Ack = 1500
```

معناه:

```text
استلمت حتى 1499
وأنتظر 1500
```

---

# لماذا SYN يستهلك رقم Sequence؟

في TCP، رسالة SYN تعتبر تستهلك رقم Sequence واحد.

لذلك:

```text
Client SYN Seq = 1000
Server ACK     = 1001
```

رغم أن SYN لا يحمل بيانات تطبيق، لكنه يدخل في حساب Sequence Numbers.

---

# TCP Flags المهمة

داخل TCP Header توجد Flags.

أهمها:

```text
SYN
ACK
FIN
RST
PSH
URG
```

---

## SYN

تستخدم لبدء الاتصال.

```text
SYN = Start connection
```

---

## ACK

تستخدم لتأكيد الاستلام.

```text
ACK = Acknowledgment
```

---

## FIN

تستخدم لإنهاء الاتصال بشكل طبيعي.

```text
FIN = Finish
```

---

## RST

تستخدم لقطع الاتصال فورًا أو رفضه.

```text
RST = Reset
```

مثال:

لو حاولت الاتصال ببورت مغلق، قد يرد الجهاز:

```text
RST
```

---

## PSH

تطلب من الطرف الآخر تمرير البيانات للتطبيق بسرعة.

```text
PSH = Push
```

---

## URG

تستخدم للبيانات العاجلة.

```text
URG = Urgent
```

استخدامها قليل في الشبكات الحديثة.

---

# TCP State أثناء Handshake

عند إنشاء الاتصال، يمر الطرفان بحالات مختلفة.

---

## على Client

```text
CLOSED
SYN-SENT
ESTABLISHED
```

---

## على Server

```text
LISTEN
SYN-RECEIVED
ESTABLISHED
```

---

# شرح الحالات

## LISTEN

السيرفر ينتظر اتصالات على Port معين.

مثال:

```text
Web Server listening on TCP 443
```

---

## SYN-SENT

العميل أرسل SYN وينتظر SYN-ACK.

---

## SYN-RECEIVED

السيرفر استلم SYN وأرسل SYN-ACK وينتظر ACK النهائي.

---

## ESTABLISHED

الاتصال تم بنجاح ويمكن نقل البيانات.

---

# مثال من netstat

قد ترى اتصالًا بهذا الشكل:

```text
TCP    192.168.1.10:51544    142.250.80.46:443    ESTABLISHED
```

معناه:

```text
يوجد اتصال TCP نشط بين جهازك والسيرفر
```

---

# TCP Header أثناء Handshake

TCP Header يحتوي على معلومات مهمة مثل:

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

# أهم TCP Options في Handshake

أثناء الـ Handshake، الطرفان قد يتبادلان بعض الخيارات.

أمثلة:

```text
MSS
Window Scale
SACK Permitted
Timestamps
```

---

## MSS

MSS اختصار لـ:

```text
Maximum Segment Size
```

وهو أكبر حجم بيانات TCP يمكن إرساله داخل Segment واحد بدون حساب IP Header و TCP Header.

مثال:

```text
MSS = 1460 bytes
```

هذا شائع في Ethernet مع MTU 1500.

---

## Window Scale

TCP Window يحدد كمية البيانات التي يمكن إرسالها قبل انتظار ACK.

Window Scale يسمح بتكبير حجم الـ Window للاتصالات الحديثة والسريعة.

---

## SACK

SACK اختصار لـ:

```text
Selective Acknowledgment
```

يساعد TCP على إعادة إرسال الأجزاء المفقودة فقط بدل إعادة إرسال كمية كبيرة من البيانات.

---

# ماذا يحدث بعد Handshake؟

بعد نجاح 3-Way Handshake، يبدأ نقل البيانات.

مثال عند فتح موقع HTTPS:

```text
1. TCP 3-Way Handshake
2. TLS Handshake
3. HTTP Request داخل TLS
4. HTTP Response داخل TLS
```

يعني TCP يحدث قبل TLS وقبل HTTP.

---

# ترتيب فتح موقع HTTPS

عندما تفتح:

```text
https://example.com
```

الترتيب غالبًا:

```text
1. DNS Query لمعرفة IP
2. ARP لمعرفة MAC الخاص بالـ Gateway
3. TCP 3-Way Handshake مع السيرفر على Port 443
4. TLS Handshake لتشفير الاتصال
5. HTTP Request داخل الاتصال المشفر
6. HTTP Response
```

---

# مثال عملي كامل

جهازك:

```text
IP Address = 192.168.1.10
Source Port = 51544
```

السيرفر:

```text
IP Address = 93.184.216.34
Destination Port = 443
```

الاتصال:

```text
192.168.1.10:51544 → 93.184.216.34:443 TCP
```

الخطوات:

```text
1. Client يرسل SYN
2. Server يرد SYN-ACK
3. Client يرسل ACK
4. الاتصال يصبح ESTABLISHED
5. يبدأ TLS Handshake
6. يتم إرسال HTTP Request
```

---

# لماذا اسمها 3-Way؟

لأن إنشاء الاتصال يحتاج ثلاث رسائل:

```text
Message 1 = SYN
Message 2 = SYN-ACK
Message 3 = ACK
```

ولا يكفي رسالتان فقط، لأن كل طرف يجب أن يؤكد أنه يستطيع الإرسال والاستقبال.

---

# ماذا لو لم يرد السيرفر؟

لو أرسل العميل SYN ولم يرجع SYN-ACK، سيعيد المحاولة عدة مرات.

قد يكون السبب:

```text
السيرفر مغلق
البورت مغلق
Firewall يمنع الاتصال
مشكلة Routing
Packet Loss
السيرفر لا يستمع على هذا Port
```

---

# ماذا لو البورت مغلق؟

لو البورت مغلق، قد يرد السيرفر أو الجهاز بـ:

```text
RST
```

مثال:

```text
Client → Server: SYN
Server → Client: RST
```

معناه:

```text
لا توجد خدمة تستمع على هذا البورت
```

---

# ماذا لو Firewall يمنع الاتصال؟

لو Firewall يمنع الاتصال بصمت، قد لا يرجع شيء.

مثال:

```text
Client → SYN
No Reply
```

عندها يظهر غالبًا:

```text
Connection timed out
```

وهذا يختلف عن RST.

---

# RST vs Timeout

| الحالة | المعنى غالبًا |
|---|---|
| RST | الجهاز موجود لكن البورت مغلق أو الاتصال مرفوض |
| Timeout | لا يوجد رد، قد يكون Firewall أو مشكلة طريق |
| SYN-ACK | البورت مفتوح والسيرفر مستعد |
| ICMP Unreachable | الطريق أو الوجهة غير متاحة |

---

# TCP Reliability بعد الاتصال

بعد إنشاء الاتصال، TCP يوفر الموثوقية من خلال:

```text
Sequence Numbers
Acknowledgments
Retransmission
Flow Control
Congestion Control
Checksum
```

---

## Retransmission

لو جزء من البيانات لم يتم تأكيده، TCP يعيد إرساله.

مثال:

```text
Segment lost
No ACK received
TCP retransmits segment
```

---

## Flow Control

Flow Control يمنع المرسل من إرسال بيانات أكثر مما يستطيع المستقبل تحمله.

يستخدم:

```text
TCP Window Size
```

---

## Congestion Control

Congestion Control يحاول منع ازدحام الشبكة.

TCP يقلل أو يزيد سرعة الإرسال حسب حالة الشبكة.

---

# TCP 3-Way Handshake و NAT

عند الخروج من شبكة البيت، الراوتر قد يعمل NAT/PAT.

مثال قبل NAT:

```text
192.168.1.10:51544 → 93.184.216.34:443
```

بعد NAT:

```text
41.33.20.5:62001 → 93.184.216.34:443
```

الـ Handshake يحدث بين العميل والسيرفر، لكن الراوتر يترجم العناوين والبورتات في الطريق.

---

# TCP 3-Way Handshake و Firewall

Firewall يمكنه متابعة حالة الاتصال.

هذا يسمى:

```text
Stateful Firewall
```

مثال:

```text
يسمح بخروج SYN من الداخل
يسمح برجوع SYN-ACK من الخارج
يسمح بباقي الاتصال لأنه عرف أنه اتصال بدأ من الداخل
```

---

# SYN Flood Attack

SYN Flood هو هجوم يستغل TCP Handshake.

الفكرة:

```text
المهاجم يرسل عددًا كبيرًا من SYN
السيرفر يرد SYN-ACK
لكن المهاجم لا يرسل ACK النهائي
```

فيظل السيرفر محتفظًا باتصالات نصف مفتوحة.

هذا قد يستهلك موارد السيرفر.

---

# Half-Open Connection

الاتصال نصف المفتوح يحدث عندما:

```text
Server استلم SYN
Server أرسل SYN-ACK
Server لم يستلم ACK النهائي
```

الحالة تكون غالبًا:

```text
SYN-RECEIVED
```

---

# الحماية من SYN Flood

طرق الحماية تشمل:

```text
SYN Cookies
Rate Limiting
Firewall Rules
DDoS Protection
تقصير Timeout للاتصالات نصف المفتوحة
Load Balancer
```

---

# SYN Cookies

SYN Cookies تجعل السيرفر لا يحتاج تخزين كل SYN بشكل كامل في الذاكرة.

بدل ذلك، يضع معلومات مشفرة داخل Sequence Number.

لو رجع ACK صحيح، يكمل الاتصال.

هذا يساعد في مقاومة SYN Flood.

---

# TCP Handshake في Wireshark

في Wireshark، يمكن رؤية الخطوات بوضوح:

```text
1  Client → Server  TCP SYN
2  Server → Client  TCP SYN, ACK
3  Client → Server  TCP ACK
```

فلتر مفيد:

```text
tcp.flags.syn == 1
```

أو لعرض Handshake مع IP معين:

```text
ip.addr == 93.184.216.34 and tcp
```

---

# أوامر مفيدة

## Windows

اختبار اتصال TCP ببورت معين:

```powershell
Test-NetConnection google.com -Port 443
```

عرض الاتصالات:

```cmd
netstat -ano
```

---

## Linux

اختبار Port:

```bash
nc -vz google.com 443
```

عرض الاتصالات:

```bash
ss -tan
```

عرض الاتصالات مع البرامج:

```bash
sudo ss -tunap
```

---

## macOS

اختبار Port:

```bash
nc -vz google.com 443
```

عرض الاتصالات:

```bash
netstat -an | grep ESTABLISHED
```

---

# الفرق بين TCP Handshake و TLS Handshake

## TCP Handshake

يحدث في Layer 4.

وظيفته:

```text
إنشاء اتصال TCP موثوق
```

ويستخدم:

```text
SYN
SYN-ACK
ACK
```

---

## TLS Handshake

يحدث بعد TCP.

وظيفته:

```text
تشفير الاتصال
التحقق من شهادة السيرفر
الاتفاق على مفاتيح التشفير
```

مثال:

```text
TCP Handshake أولًا
ثم TLS Handshake
ثم HTTP Request
```

---

# TCP vs UDP في بداية الاتصال

TCP يحتاج Handshake قبل نقل البيانات.

لكن UDP لا يحتاج Handshake.

مثال:

```text
TCP:
SYN → SYN-ACK → ACK → Data
```

```text
UDP:
Data مباشرة
```

لذلك UDP أسرع في البداية، لكنه لا يقدم نفس ضمانات TCP.

---

# أخطاء شائعة

## الخطأ الأول

```text
TCP يبدأ بإرسال HTTP مباشرة
```

غير صحيح.

قبل HTTP يحدث TCP Handshake، ولو الاتصال HTTPS يحدث TLS Handshake أيضًا.

---

## الخطأ الثاني

```text
Port 443 يعني أن الاتصال بدأ مباشرة بـ TLS
```

ليس بالضرورة.

أولًا يجب إنشاء TCP Connection على Port 443، ثم يبدأ TLS.

---

## الخطأ الثالث

```text
SYN-ACK يعني أن الموقع فتح بالكامل
```

غير دقيق.

SYN-ACK يعني أن TCP Port يرد، لكن قد تفشل TLS أو HTTP بعد ذلك.

---

## الخطأ الرابع

```text
Timeout يعني السيرفر مغلق دائمًا
```

ليس دائمًا.

قد يكون Firewall يمنع الرد أو هناك مشكلة في الطريق.

---

## الخطأ الخامس

```text
RST يعني أن الجهاز غير موجود
```

غير صحيح.

RST غالبًا يعني أن الجهاز وصلته الرسالة، لكنه رفض الاتصال أو البورت مغلق.

---

# Troubleshooting

## الحالة 1: الموقع لا يفتح

اختبر:

```text
DNS
Ping
TCP Port
TLS
HTTP
```

مثال:

```bash
nslookup example.com
ping example.com
nc -vz example.com 443
```

---

## الحالة 2: Ping يعمل لكن الموقع لا يفتح

قد يكون السبب:

```text
TCP Port 80/443 محجوب
Firewall
Proxy
TLS Problem
Web Server Down
```

---

## الحالة 3: Port 443 لا يرد

قد يكون السبب:

```text
السيرفر لا يستمع
Firewall يمنع
Routing Problem
NAT Problem
Service Down
```

---

## الحالة 4: SYN يخرج ولا يرجع SYN-ACK

قد يكون السبب:

```text
Firewall
Packet Loss
Server Down
Wrong IP
ISP Blocking
```

---

## الحالة 5: SYN-ACK يرجع لكن الاتصال لا يكتمل

قد يكون السبب:

```text
مشكلة في جهاز العميل
Firewall يمنع ACK
مشكلة NAT
Packet Loss
```

---

# ملخص سريع

```text
TCP = بروتوكول موثوق في Layer 4
TCP يحتاج اتصال قبل نقل البيانات
3-Way Handshake = SYN, SYN-ACK, ACK
SYN = طلب بدء اتصال
SYN-ACK = قبول ومزامنة من السيرفر
ACK = تأكيد نهائي من العميل
بعدها يصبح الاتصال ESTABLISHED
Sequence Number = ترتيب البيانات
Acknowledgment Number = تأكيد الاستلام
RST = رفض أو قطع اتصال
FIN = إنهاء اتصال طبيعي
```

---

# أهم القواعد

```text
TCP Handshake يحدث قبل إرسال بيانات التطبيق
```

```text
HTTPS يحتاج TCP Handshake ثم TLS Handshake
```

```text
SYN-ACK غالبًا يعني أن البورت مفتوح ويرد
```

```text
RST غالبًا يعني البورت مغلق أو الاتصال مرفوض
```

```text
Timeout قد يعني Firewall أو مشكلة في الطريق
```

```text
TCP يستخدم Sequence و ACK لضمان وصول وترتيب البيانات
```

---

## Next Topic

اقرأ بعد ذلك:

[TCP vs UDP](./TCP-vs-UDP.md)
