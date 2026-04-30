





# OSI Layers

## المقدمة

نموذج **OSI Model** هو طريقة لفهم كيف تنتقل البيانات داخل الشبكات.

بدل ما نقول إن الإنترنت يعمل مرة واحدة بشكل غامض، نموذج OSI يقسم العملية إلى **7 طبقات**.  
كل طبقة لها وظيفة محددة، وكل طبقة تساعد الطبقة التي فوقها أو تحتها.

الفكرة ببساطة:

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
````


---

## لماذا نحتاج OSI Model؟

نحتاجه لأنه يساعدنا على:

* فهم الشبكات بطريقة منظمة.
* معرفة أين تحدث المشكلة عند وجود عطل.
* فهم دور كل بروتوكول مثل TCP وIP وARP وDNS.
* التفريق بين الأجهزة مثل Switch وRouter.
* فهم عملية Encapsulation وDecapsulation.

مثال بسيط:

```text
لو الموقع لا يفتح:
هل المشكلة في DNS؟
هل المشكلة في TCP؟
هل المشكلة في IP؟
هل المشكلة في Wi-Fi أو الكابل؟
```

نموذج OSI يساعدك تحدد مكان المشكلة بدل التخمين.

---

# طبقات OSI السبع

## Layer 7 - Application Layer

طبقة التطبيق هي أقرب طبقة للمستخدم.

هذه الطبقة لا تعني تطبيقات مثل Chrome أو WhatsApp نفسها فقط، لكنها تعني البروتوكولات التي يستخدمها التطبيق للتواصل عبر الشبكة.

أمثلة:

```text
HTTP
HTTPS
DNS
FTP
SMTP
IMAP
POP3
```

مثال عملي:

عندما تكتب:

```text
https://google.com
```

المتصفح يستخدم بروتوكول HTTPS في طبقة التطبيق حتى يطلب صفحة الموقع.

---

## Layer 6 - Presentation Layer

طبقة العرض مسؤولة عن شكل البيانات.

وظائفها الأساسية:

```text
Encoding
Encryption
Compression
```

يعني مثلًا:

* تحويل النصوص إلى شكل مفهوم.
* تشفير البيانات.
* ضغط البيانات لتقليل الحجم.

أمثلة:

```text
ASCII
Unicode
JPEG
PNG
MP3
MPEG
TLS/SSL
ZIP
GZIP
```

مثال عملي:

عندما تستخدم HTTPS، يتم تشفير البيانات حتى لا يستطيع أحد قراءتها أثناء انتقالها.

---

## Layer 5 - Session Layer

طبقة الجلسة مسؤولة عن إنشاء وإدارة وإنهاء الجلسات بين الأجهزة.

يعني هي التي تساعد على تنظيم الاتصال بين طرفين.

مثال:

```text
تسجيل الدخول إلى حسابك البنكي
فتح جلسة بين جهازك والسيرفر
الحفاظ على الجلسة أثناء الاستخدام
إنهاء الجلسة عند تسجيل الخروج
```

أمثلة بروتوكولات أو تقنيات مرتبطة:

```text
NetBIOS
RPC
PPTP
```

---

## Layer 4 - Transport Layer

طبقة النقل مسؤولة عن نقل البيانات بين التطبيقات على الأجهزة.

أهم شيء هنا:

```text
TCP
UDP
Ports
```

### TCP

TCP بروتوكول موثوق.

يعني:

* يتأكد أن البيانات وصلت.
* يرتب البيانات.
* يعيد إرسال البيانات إذا فُقدت.
* يبدأ الاتصال عن طريق TCP 3-Way Handshake.

مثال:

```text
HTTPS
SSH
FTP
Email
```

### UDP

UDP أسرع، لكنه لا يضمن وصول البيانات.

يعني:

* لا يوجد Handshake.
* لا يوجد تأكيد وصول.
* لا يوجد ترتيب مضمون.
* مناسب للسرعة.

مثال:

```text
DNS
DHCP
VoIP
Online Gaming
Streaming
```

### Ports

البورت يحدد التطبيق أو الخدمة داخل الجهاز.

مثال:

```text
HTTP  = 80
HTTPS = 443
DNS   = 53
SSH   = 22
```

---

## Layer 3 - Network Layer

طبقة الشبكة مسؤولة عن التوجيه بين الشبكات المختلفة.

أهم شيء هنا:

```text
IP Address
Router
Routing
ICMP
```

### IP Address

عنوان IP هو العنوان المنطقي للجهاز داخل الشبكة.

مثال:

```text
192.168.1.10
8.8.8.8
142.250.80.46
```

### Router

الراوتر يعمل في Layer 3.

وظيفته:

```text
ينقل Packet من شبكة إلى شبكة أخرى
```

مثال:

```text
من شبكة البيت إلى الإنترنت
من شركة إلى فرع آخر
من بلد إلى بلد آخر
```

### Routing

الراوتر يستخدم Routing Table حتى يعرف أفضل طريق للوجهة.

---

## Layer 2 - Data Link Layer

طبقة ربط البيانات مسؤولة عن النقل داخل نفس الشبكة المحلية.

أهم شيء هنا:

```text
MAC Address
Switch
Frame
ARP
VLAN
```

### MAC Address

عنوان MAC هو العنوان الفيزيائي لكرت الشبكة.

مثال:

```text
AA:BB:CC:DD:EE:FF
```

### Switch

السويتش يعمل غالبًا في Layer 2.

وظيفته:

```text
يوصل الأجهزة داخل نفس الشبكة المحلية باستخدام MAC Address
```

### Frame

في Layer 2، اسم وحدة البيانات هو:

```text
Frame
```

### ARP

بروتوكول ARP يربط بين IP وMAC داخل نفس الشبكة المحلية.

مثال:

```text
من لديه IP 192.168.1.1؟
أريد معرفة MAC Address الخاص به
```

---

## Layer 1 - Physical Layer

الطبقة الفيزيائية مسؤولة عن إرسال البيانات فعليًا على شكل إشارات.

أمثلة:

```text
Electrical Signals
Light Signals
Radio Waves
Cables
Fiber
Wi-Fi
```

في هذه الطبقة، البيانات تكون عبارة عن:

```text
Bits
0 و 1
```

أجهزة مرتبطة:

```text
Cable
Hub
Repeater
Network Card
Fiber
```

---

# أسماء البيانات في كل طبقة

كل طبقة تسمي البيانات باسم مختلف.

```text
Layer 7, 6, 5  = Data
Layer 4        = Segment / Datagram
Layer 3        = Packet
Layer 2        = Frame
Layer 1        = Bits
```

## جدول PDU

| Layer   | Name         | PDU                |
| ------- | ------------ | ------------------ |
| Layer 7 | Application  | Data               |
| Layer 6 | Presentation | Data               |
| Layer 5 | Session      | Data               |
| Layer 4 | Transport    | Segment / Datagram |
| Layer 3 | Network      | Packet             |
| Layer 2 | Data Link    | Frame              |
| Layer 1 | Physical     | Bits               |

---

# Encapsulation

التغليف هو عملية نزول البيانات من الطبقات العليا إلى الطبقات السفلى.

مثال:

```text
Data
↓
Segment
↓
Packet
↓
Frame
↓
Bits
```

يعني كل طبقة تضيف Header خاص بها.

مثال عملي:

```text
Application Data
+ TCP Header
+ IP Header
+ Ethernet Header
= Frame ready to be sent
```

---

# Decapsulation

فك التغليف هو العملية العكسية عند جهاز المستقبل.

```text
Bits
↓
Frame
↓
Packet
↓
Segment
↓
Data
```

كل طبقة تزيل الـ Header الخاص بها وتسلم البيانات للطبقة التي فوقها.

---

# مثال عملي: فتح موقع HTTPS

عندما تفتح موقع مثل:

```text
https://google.com
```

يحدث الآتي بشكل مبسط:

```text
1. المتصفح يستخدم HTTPS في Layer 7
2. TLS يشفر البيانات في Layer 6
3. TCP يفتح اتصال في Layer 4
4. IP يحدد عنوان السيرفر في Layer 3
5. MAC يحدد الخطوة التالية داخل الشبكة في Layer 2
6. Wi-Fi أو الكابل يرسل Bits في Layer 1
```

---

# خريطة سريعة للطبقات

```text
Layer 7: Application  → HTTP, HTTPS, DNS
Layer 6: Presentation → Encryption, Encoding, Compression
Layer 5: Session      → Sessions
Layer 4: Transport    → TCP, UDP, Ports
Layer 3: Network      → IP, Router, Routing, ICMP
Layer 2: Data Link    → MAC, Switch, ARP, VLAN
Layer 1: Physical     → Cable, Wi-Fi, Signals, Bits
```

---

# مقارنة مهمة

```text
Switch = Layer 2 = MAC Address
Router = Layer 3 = IP Address
TCP/UDP = Layer 4 = Ports
HTTP/DNS = Layer 7 = Application Protocols
```

---

# الخلاصة

نموذج OSI يقسم عملية الاتصال في الشبكات إلى 7 طبقات.

كل طبقة لها وظيفة محددة:

```text
Application  → ماذا يريد المستخدم؟
Presentation → كيف تظهر أو تُشفّر البيانات؟
Session      → كيف تُدار الجلسة؟
Transport    → كيف تنتقل البيانات بين التطبيقات؟
Network      → أين الوجهة؟
Data Link    → ما هو الجهاز التالي داخل الشبكة؟
Physical     → كيف تتحول البيانات إلى إشارات؟
```

أهم قاعدة:

```text
IP Address  = للوصول إلى الشبكة أو الجهاز النهائي
MAC Address = للوصول إلى الجهاز التالي داخل نفس الشبكة
Port        = للوصول إلى التطبيق الصحيح داخل الجهاز
```

---

## Next Topic

اقرأ بعد ذلك:

[Packet Encapsulation and Decapsulation](./Packet-Encapsulation-and-Decapsulation.md)



