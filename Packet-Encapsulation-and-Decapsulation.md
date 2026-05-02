

# Packet Encapsulation and Decapsulation

## الخيط الذهبي الذي يربط كل شيء

في الدروس السابقة عرفنا أن كل طبقة في نموذج OSI لها وظيفة مختلفة:

```text
Layer 4 → تضيف Ports
Layer 3 → تضيف IP Addresses
Layer 2 → تضيف MAC Addresses
Layer 1 → تحول البيانات إلى Bits
```

لكن السؤال المهم:

**كيف تجتمع كل هذه الإضافات في رسالة واحدة تخرج من جهازك؟**

الإجابة هي:

```text
Encapsulation
```

يعني **التغليف**.

وعندما تصل البيانات لجهاز المستقبل، تحدث العملية العكسية:

```text
Decapsulation
```

يعني **فك التغليف**.

---

## ما معنى Encapsulation؟

التغليف هو عملية نزول البيانات من الطبقات العليا إلى الطبقات السفلى.

كل طبقة تضيف جزءًا خاصًا بها يسمى غالبًا:

```text
Header
```

وبعض الطبقات تضيف أيضًا:

```text
Trailer
```

مثال بسيط:

```text
Data
↓
TCP Header + Data
↓
IP Header + TCP Header + Data
↓
Ethernet Header + IP Header + TCP Header + Data + FCS
↓
Bits
```

يعني البيانات لا تسافر وحدها، بل تسافر داخل أغلفة متعددة.

---

# أسماء البيانات في كل طبقة

كل طبقة تسمي البيانات باسم مختلف.  
هذه الأسماء تسمى:

```text
PDU = Protocol Data Unit
```

## جدول PDU

| Layer | Name | PDU | What is added? |
|---|---|---|---|
| Layer 7 | Application | Data | بيانات التطبيق مثل HTTP Request |
| Layer 6 | Presentation | Data | تشفير أو ضغط أو Encoding |
| Layer 5 | Session | Data | إدارة الجلسة |
| Layer 4 | Transport | Segment / Datagram | Ports + TCP/UDP Header |
| Layer 3 | Network | Packet | Source IP + Destination IP |
| Layer 2 | Data Link | Frame | Source MAC + Destination MAC + FCS |
| Layer 1 | Physical | Bits | تحويل البيانات إلى إشارات |

---

# رحلة الإرسال: Encapsulation

تخيل أنك فتحت المتصفح وكتبت:

```text
https://google.com
```

بعد أن يعرف جهازك IP الخاص بجوجل، تبدأ رحلة تجهيز البيانات للإرسال.

---

## 1. Application Layer - Data

المتصفح يجهز طلب HTTP.

مثال:

```http
GET / HTTP/1.1
Host: google.com
User-Agent: Chrome
```

في هذه المرحلة اسم البيانات:

```text
Data
```

---

## 2. Presentation Layer - Encryption

بما أنك تستخدم HTTPS، يتم تشفير البيانات باستخدام TLS.

يعني طلب HTTP لا يسافر كنص واضح، بل يسافر مشفرًا.

```text
HTTP Data
↓
Encrypted Data
```

---

## 3. Transport Layer - Segment

الآن يأتي دور Layer 4.

لو الاتصال HTTPS تقليدي، غالبًا سيستخدم TCP.

الـ TCP يضيف Header يحتوي على معلومات مهمة مثل:

```text
Source Port
Destination Port
Sequence Number
Acknowledgment Number
Flags
Window Size
Checksum
```

مثال:

```text
Source Port      = 50000
Destination Port = 443
```

النتيجة تصبح:

```text
TCP Segment
```

شكل مبسط:

```text
[TCP Header][Encrypted Data]
```

> **مقارنة سريعة: شكل الـ Segment في TCP مقابل UDP:**
> ```text
> [TCP Header (20 Bytes)][Data] 
> → ثقيل، يحتوي على أرقام الـ Sequence و Acknowledgment والتحكم بالتدفق.
> 
> [UDP Header (8 Bytes)][Data] 
> → خفيف وسريع، لا يوجد به أي تحقق من وصول البيانات أو ترتيبها.
> (ملاحظة: في UDP نسميه Datagram وليس Segment).
> ```

---

## 4. Network Layer - Packet

الآن يأتي دور Layer 3.

الـ IP يضيف Header يحتوي على:

```text
Source IP
Destination IP
TTL
Protocol
```

مثال:

```text
Source IP      = 192.168.1.10
Destination IP = 142.250.80.46
```

النتيجة تصبح:

```text
IP Packet
```

شكل مبسط:

```text
[IP Header][TCP Header][Encrypted Data]
```

---

## 5. Data Link Layer - Frame

الآن يأتي دور Layer 2.

الجهاز يحتاج يعرف MAC Address للخطوة التالية.

لو الموقع خارج شبكتك المحلية، فالخطوة التالية ليست جوجل مباشرة، بل:

```text
Default Gateway
```

يعني الراوتر.

هنا يستخدم الجهاز ARP لمعرفة MAC Address الخاص بالراوتر.

بعد ذلك يضيف Ethernet Header يحتوي على:

```text
Source MAC
Destination MAC
EtherType
```

وفي نهاية الإطار يضيف:

```text
FCS = Frame Check Sequence
```

> **ما هو الـ FCS (Frame Check Sequence)؟**
> هو جزء يُضاف في نهاية الـ Frame (Trailer) وظيفته **اكتشاف الأخطاء (Error Detection)**.
> يعمل كـ "ختم أمان"، عندما يصل الـ Frame للجهاز التالي، يعيد حساب قيمة الـ FCS، 
> لو اختلف الرقم، يعني أن الإشارات تعرضت لتشويه (Interference) في الكابل أو الواي فاي، 
> فيقوم الجهاز بإهمال الـ Frame بالكامل (Drop the Frame) ولا يرسله للطبقات العليا.

النتيجة تصبح:

```text
Frame
```

شكل مبسط:

```text
[Ethernet Header][IP Header][TCP Header][Encrypted Data][FCS]
```

---

## 6. Physical Layer - Bits

في Layer 1، يتم تحويل الـ Frame إلى Bits:

```text
010101010101010101
```

ثم يتم إرسالها عبر:

```text
Ethernet Cable
Wi-Fi
Fiber
Radio Waves
```

---

# الشكل النهائي قبل الخروج من جهازك

قبل أن تغادر البيانات جهازك، تكون تقريبًا بهذا الشكل:

```text
┌─────────────────────────────────────────────────────────────┐
│                         Frame                               │
├───────────────┬────────────┬────────────┬──────────┬───────┤
│ Ethernet Hdr  │ IP Header  │ TCP Header │   Data   │  FCS  │
│ Layer 2       │ Layer 3    │ Layer 4    │ Layer 7  │ L2    │
└───────────────┴────────────┴────────────┴──────────┴───────┘
```

لو كان هناك VLAN، قد يظهر أيضًا:

```text
802.1Q VLAN Tag
```

في Layer 2 داخل الـ Frame.

---

# رحلة الاستقبال: Decapsulation

عند جهاز المستقبل، تحدث العملية بالعكس.

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

---

## 1. Physical Layer

كرت الشبكة يستقبل الإشارات.

```text
Electrical Signals / Wi-Fi Signals
↓
Bits
```

---

## 2. Data Link Layer

الجهاز يقرأ الـ Frame.

يتحقق من:

```text
Destination MAC
FCS
```

لو الـ MAC صحيح والـ FCS سليم، يكمل.

ثم يزيل:

```text
Ethernet Header
FCS
```

ويبقى:

```text
IP Packet
```

---

## 3. Network Layer

الجهاز يقرأ IP Header.

يتحقق من:

```text
Destination IP
```

لو الـ IP يخصه، يكمل.

ثم يزيل:

```text
IP Header
```

ويبقى:

```text
TCP Segment
```

---

## 4. Transport Layer

الـ TCP يقرأ:

```text
Destination Port
Sequence Number
Acknowledgment Number
```

ثم يرسل البيانات للتطبيق الصحيح حسب البورت.

مثال:

```text
Port 443 → HTTPS Service
```

ثم يزيل:

```text
TCP Header
```

ويبقى:

```text
Encrypted Data
```

---

## 5. Presentation / Application Layers

TLS يفك التشفير باستخدام Session Key.

ثم التطبيق يستلم البيانات الأصلية.

مثال:

```text
HTTP Response
```

ويعرض المتصفح صفحة الموقع.

---

# مشكلة الحجم: MTU و Fragmentation

بما أن كل طبقة تضيف Header، فإن حجم البيانات يكبر.
لكن الكابلات والراوترات لها حد أقصى لحجم الـ Frame يمكنها حمله، يسمى:

```text
MTU = Maximum Transmission Unit
```

في شبكات الإيثرنت العادية (Ethernet):
الحد الأقصى لحجم الـ Frame هو **1500 Bytes** (هذا يخص الـ Headers + Data بدون FCS).

**ماذا لو كانت البيانات المرسلة أكبر من 1500 Byte؟ (مثل تنزيل ملف كبير)**

يحدث شيء اسمه:

```text
Fragmentation (تقسيم)
```

في Layer 3، الراوتر يقوم بتقسيم الـ Packet الكبير إلى عدة Packets أصغر، 
كل واحد منها لا يتجاوز الـ MTU المسموح به، ويرسلها على شكل Frames منفصلة.

عندما تصل لجهاز المستقبل في Layer 3 مرة أخرى، يقوم الـ IP بتجميع هذه القطع (Reassembly) 
باستخدام أرقام وضعها في الـ IP Header لتعود البيانات كما كانت.

> **ملاحظة هامة:** يُفضل دائماً تجنب الـ Fragmentation لأنه يستهلك موارد الراوتر والجهاز،
> ولذلك في العصر الحديث نستخدم تقنية MSS (Maximum Segment Size) في Layer 4 
> حتى يقوم الـ TCP بتقسيم البيانات بحجم مناسب من البداية قبل أن تصل للراوتر!

---

# القاعدة الذهبية: IP ثابت و MAC يتغير

من أهم قواعد الشبكات:

```text
IP Address  = End-to-End
MAC Address = Hop-to-Hop
```

---

## IP Address

الـ IP يمثل العنوان النهائي.

مثال:

```text
Source IP      = 192.168.1.10
Destination IP = 142.250.80.46
```

غالبًا يظل Destination IP ثابتًا حتى يصل للسيرفر.

لكن يوجد استثناء مهم:

```text
NAT
```

عندما تخرج من شبكة البيت إلى الإنترنت، الراوتر يغير الـ Private IP إلى Public IP.

مثال:

```text
Before NAT:
Source IP = 192.168.1.10

After NAT:
Source IP = 41.33.20.5
```

---

## MAC Address

الـ MAC يستخدم فقط داخل نفس الشبكة المحلية أو بين كل Hop والتالي.

يعني عند كل راوتر، يتم التخلص من Frame القديم وبناء Frame جديد.

مثال:

```text
Laptop → Home Router
Source MAC      = Laptop MAC
Destination MAC = Router MAC
```

بعد الراوتر:

```text
Home Router → ISP Router
Source MAC      = Home Router MAC
Destination MAC = ISP Router MAC
```

إذن:

```text
MAC changes at every hop
```

---

# ماذا يتغير في كل Hop؟

| Field | هل يتغير؟ | السبب |
|---|---|---|
| Source MAC | نعم | يتغير عند كل وصلة |
| Destination MAC | نعم | يمثل الجهاز التالي فقط |
| Source IP | غالبًا لا، إلا مع NAT | NAT يغير Private IP إلى Public IP |
| Destination IP | غالبًا لا | يمثل الوجهة النهائية |
| Source Port | قد يتغير مع PAT | الراوتر قد يغيره أثناء NAT/PAT |
| Destination Port | غالبًا لا | يمثل خدمة السيرفر مثل 443 |
| TTL | نعم | يقل بمقدار 1 عند كل راوتر |

---

# Master Scenario

## فتح موقع HTTPS خطوة بخطوة

القصة:

```text
أنت فتحت اللابتوب واتصلت بالواي فاي وكتبت:
https://google.com
```

---

## المرحلة الأولى: الحصول على إعدادات الشبكة

### 1. DHCP

الجهاز لا يعرف IP الخاص به.

فيرسل:

```text
DHCP Discover
```

الراوتر أو DHCP Server يرد عليه بإعدادات مثل:

```text
IP Address      = 192.168.1.10
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.1.1
DNS Server      = 8.8.8.8
```

---

## المرحلة الثانية: معرفة IP الموقع

### 2. DNS

المتصفح يحتاج يعرف IP الخاص بـ:

```text
google.com
```

فيرسل DNS Query.

الـ DNS يرد مثلًا:

```text
google.com = 142.250.80.46
```

---

## المرحلة الثالثة: قرار التوجيه

الجهاز يقارن:

```text
My IP        = 192.168.1.10
My Network   = 192.168.1.0/24
Google IP    = 142.250.80.46
```

الجهاز يكتشف أن جوجل خارج الشبكة المحلية.

إذن القرار:

```text
Send to Default Gateway
```

يعني يرسل للراوتر.

---

## المرحلة الرابعة: معرفة MAC الراوتر

### 4. ARP

الجهاز يعرف IP الراوتر:

```text
192.168.1.1
```

لكن يحتاج MAC Address الخاص به.

فيرسل ARP Request:

```text
Who has 192.168.1.1?
```

الراوتر يرد:

```text
192.168.1.1 is at AA:AA:AA:AA:AA:AA
```

الجهاز يحفظ هذه المعلومة في:

```text
ARP Cache
```

---

## المرحلة الخامسة: TCP Handshake

بما أن HTTPS يستخدم TCP في HTTP/1.1 وHTTP/2، يبدأ الجهاز بعمل TCP 3-Way Handshake مع السيرفر.

```text
Client → Server: SYN
Server → Client: SYN-ACK
Client → Server: ACK
```

الآن الاتصال جاهز لنقل البيانات.

---

## المرحلة السادسة: TLS Handshake

بعد TCP، يبدأ TLS.

الغرض:

```text
Encryption
Authentication
Integrity
```

الخطوات المبسطة:

```text
Client Hello
Server Hello + Certificate
Key Exchange
Finished
```

بعدها يصبح الاتصال مشفرًا.

---

## المرحلة السابعة: HTTP Request

المتصفح يجهز طلب HTTP.

مثال:

```http
GET / HTTP/1.1
Host: google.com
```

ثم يتم تشفيره بواسطة TLS.

---

# المرحلة الثامنة: Encapsulation الحقيقي

## Layer 4

TCP يضيف:

```text
Source Port      = 50000
Destination Port = 443
```

النتيجة:

```text
Segment
```

---

## Layer 3

IP يضيف:

```text
Source IP      = 192.168.1.10
Destination IP = 142.250.80.46
```

النتيجة:

```text
Packet
```

---

## Layer 2

Ethernet يضيف:

```text
Source MAC      = Laptop MAC
Destination MAC = Router MAC
```

النتيجة:

```text
Frame
```

---

## Layer 1

يتم تحويل الإطار إلى Bits.

```text
010101010101
```

وتخرج عبر Wi-Fi أو الكابل.

---

# المرحلة التاسعة: داخل السويتش

السويتش يستقبل Frame.

ينظر إلى:

```text
Destination MAC
```

ويستخدم:

```text
CAM Table
```

ليعرف من أي Port يخرج الإطار.

السويتش لا يقرأ HTTP ولا TCP ولا IP للتوجيه العادي.

هو يهتم غالبًا بـ:

```text
MAC Address
```

---

# المرحلة العاشرة: عند الراوتر

الراوتر يستقبل Frame.

يفعل الآتي:

```text
1. يزيل Ethernet Header
2. يقرأ Destination IP
3. يبحث في Routing Table
4. يقلل TTL بمقدار 1
5. قد ينفذ NAT/PAT
6. يبني Frame جديد للـ Next Hop
```

مثال NAT:

```text
192.168.1.10:50000
↓
41.33.20.5:62001
```

ثم يرسل Packet للإنترنت داخل Frame جديد.

---

# المرحلة الحادية عشرة: عبر الإنترنت

الحزمة تمر على راوترات كثيرة.

في كل راوتر:

```text
MAC changes
TTL decreases by 1
Routing Table is checked
New Frame is created
```

لكن غالبًا:

```text
Destination IP remains the same
Destination Port remains 443
```

---

# المرحلة الثانية عشرة: عند سيرفر جوجل

السيرفر يستقبل البيانات ويفك التغليف:

```text
Bits
↓
Frame
↓
Packet
↓
Segment
↓
Encrypted Data
↓
HTTP Request
```

ثم يجهز Response ويرسله بنفس الفكرة في الاتجاه العكسي.

---

# ملخص سريع

```text
Encapsulation:
Data → Segment → Packet → Frame → Bits

Decapsulation:
Bits → Frame → Packet → Segment → Data
```

---

# أهم القواعد

```text
Layer 4 adds Ports
Layer 3 adds IP Addresses
Layer 2 adds MAC Addresses
Layer 1 sends Bits
```

```text
IP = final destination
MAC = next hop only
Port = application/service
```

```text
Router changes Layer 2 header at every hop
Switch forwards based on MAC
NAT may change Source IP and Source Port
TLS encrypts the Application Data
```

---

## Next Topic


اقرأ بعد ذلك:

[IP Addressing and Subnetting](./IP-Addressing-and-Subnetting.md)
