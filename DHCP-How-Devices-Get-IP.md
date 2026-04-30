# DHCP - How Devices Get IP

## كيف يحصل جهازك على IP تلقائيًا؟

عندما توصل جهازك بالواي فاي أو بالكابل، الجهاز لا يعرف من البداية:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
```

ومع ذلك، بعد ثواني قليلة يصبح الجهاز متصلًا بالشبكة.

السؤال هنا:

```text
من أعطى الجهاز هذه الإعدادات؟
```

الإجابة:

```text
DHCP
```

---

# ما هو DHCP؟

DHCP اختصار لـ:

```text
Dynamic Host Configuration Protocol
```

يعني بروتوكول يعطي الأجهزة إعدادات الشبكة تلقائيًا.

بدل ما تكتب يدويًا:

```text
IP Address      = 192.168.1.10
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.1.1
DNS Server      = 8.8.8.8
```

الـ DHCP يعطيها للجهاز بشكل تلقائي.

---

# تشبيه بسيط

تخيل أنك دخلت فندق.

أنت لا تعرف:

```text
رقم الغرفة
مكان المصعد
مكان المطعم
مدة الإقامة
```

فتذهب إلى موظف الاستقبال.

موظف الاستقبال يعطيك:

```text
رقم الغرفة
مفتاح الغرفة
تعليمات الفندق
مدة الإقامة
```

في الشبكات:

```text
DHCP Server = موظف الاستقبال
Device      = الضيف
IP Address  = رقم الغرفة
Gateway     = باب الخروج
DNS         = دليل الأماكن
Lease Time  = مدة الإقامة
```

---

# لماذا نحتاج DHCP؟

بدون DHCP، ستحتاج أن تضبط كل جهاز يدويًا.

لو عندك 5 أجهزة، الأمر بسيط.

لكن لو عندك شركة فيها:

```text
100 جهاز
500 جهاز
1000 جهاز
```

سيكون الأمر صعبًا جدًا.

الـ DHCP يحل مشاكل كثيرة:

```text
توفير الوقت
منع تكرار نفس IP
سهولة تغيير DNS أو Gateway
إدارة الشبكة بشكل مركزي
```

---

# ما الذي يعطيه DHCP للجهاز؟

عندما ينجح DHCP، يحصل الجهاز غالبًا على:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
Lease Time
```

مثال:

```text
IP Address      = 192.168.1.50
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.1.1
DNS Server      = 8.8.8.8
Lease Time      = 24 hours
```

---

# أين يوجد DHCP Server؟

في شبكة البيت غالبًا يكون DHCP داخل الراوتر.

```text
Home Router = DHCP Server
```

في الشركات الكبيرة، قد يكون DHCP Server جهازًا مستقلًا أو سيرفر ويندوز/لينكس.

```text
Windows Server
Linux Server
Firewall
Router
Layer 3 Switch
```

---

# ما هو IP Pool؟

الـ DHCP Server لا يعطي أي IP عشوائي.

هو يملك مجموعة عناوين جاهزة تسمى:

```text
IP Pool
```

مثال:

```text
192.168.1.50 - 192.168.1.200
```

يعني DHCP يستطيع إعطاء الأجهزة عناوين من هذا النطاق فقط.

مثال:

```text
Laptop  → 192.168.1.50
Phone   → 192.168.1.51
Printer → 192.168.1.52
```

---

# رحلة DHCP المشهورة: DORA

عملية DHCP الأساسية تسمى:

```text
DORA
```

وهي اختصار لـ:

```text
Discover
Offer
Request
ACK
```

---

## 1. DHCP Discover

الجهاز الجديد لا يملك IP.

لذلك يرسل رسالة Broadcast في الشبكة يقول فيها:

```text
هل يوجد DHCP Server هنا؟
```

اسم الرسالة:

```text
DHCP Discover
```

لأن الجهاز لا يعرف عنوان DHCP Server، يرسل الرسالة إلى:

```text
255.255.255.255
```

وهذا Broadcast يعني:

```text
إرسال لكل الأجهزة داخل الشبكة المحلية
```

---

## 2. DHCP Offer

الـ DHCP Server يسمع رسالة Discover.

فيرد على الجهاز بعرض:

```text
أستطيع أن أعطيك هذا IP
```

مثال:

```text
Offered IP      = 192.168.1.50
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.1.1
DNS Server      = 8.8.8.8
Lease Time      = 24 hours
```

اسم الرسالة:

```text
DHCP Offer
```

---

## 3. DHCP Request

الجهاز يستلم العرض.

ثم يرسل رسالة يقول فيها:

```text
أنا موافق على هذا العرض وأريد استخدام هذا IP
```

اسم الرسالة:

```text
DHCP Request
```

هذه الرسالة غالبًا تكون Broadcast حتى يعرف أي DHCP Server آخر أن الجهاز اختار هذا العرض.

---

## 4. DHCP ACK

الـ DHCP Server يرسل تأكيد نهائي.

```text
تم تأكيد العنوان، يمكنك استخدامه
```

اسم الرسالة:

```text
DHCP ACK
```

بعد هذه الرسالة، الجهاز يبدأ استخدام إعدادات الشبكة.

---

# DORA بشكل مختصر

```text
Client → Broadcast: DHCP Discover
Server → Client:    DHCP Offer
Client → Broadcast: DHCP Request
Server → Client:    DHCP ACK
```

---

# رسم مبسط

```text
┌──────────┐                                ┌──────────────┐
│  Client  │                                │ DHCP Server  │
└────┬─────┘                                └──────┬───────┘
     │                                             │
     │ DHCP Discover                               │
     │────────────────────────────────────────────>│
     │                                             │
     │ DHCP Offer                                  │
     │<────────────────────────────────────────────│
     │                                             │
     │ DHCP Request                                │
     │────────────────────────────────────────────>│
     │                                             │
     │ DHCP ACK                                    │
     │<────────────────────────────────────────────│
     │                                             │
```

---

# لماذا DHCP يستخدم UDP وليس TCP؟

DHCP يعمل قبل أن يحصل الجهاز على IP.

والـ TCP يحتاج اتصال واضح بين طرفين، ويحتاج عناوين جاهزة.

لذلك DHCP يستخدم UDP لأنه أخف وأسهل في هذه المرحلة.

DHCP يستخدم:

```text
UDP Port 67 = Server
UDP Port 68 = Client
```

يعني:

```text
DHCP Server listens on UDP 67
DHCP Client uses UDP 68
```

---

# لماذا أول رسالة تكون Broadcast؟

الجهاز في البداية لا يعرف:

```text
IP الخاص به
IP الخاص بالراوتر
IP الخاص بـ DHCP Server
```

لذلك لا يستطيع إرسال رسالة مباشرة إلى سيرفر معين.

فيصرخ في الشبكة:

```text
أنا جهاز جديد، هل يوجد DHCP Server؟
```

وهذا هو معنى Broadcast.

---

# ما هو Lease Time؟

الـ IP الذي يعطيه DHCP ليس ملكًا دائمًا للجهاز.

هو مثل عقد إيجار.

اسمه:

```text
Lease
```

مثال:

```text
Lease Time = 24 hours
```

يعني الجهاز يستطيع استخدام IP لمدة 24 ساعة.

قبل انتهاء المدة، الجهاز يطلب تجديد الإيجار.

---

# متى يجدد الجهاز الـ Lease؟

غالبًا عندما يمر نصف الوقت.

مثال:

```text
Lease Time = 24 hours
```

بعد حوالي:

```text
12 hours
```

الجهاز يحاول تجديد العنوان مع DHCP Server.

لو وافق السيرفر، يستمر الجهاز بنفس IP.

---

# ماذا يحدث لو انتهى Lease Time؟

لو انتهت مدة الإيجار ولم يستطع الجهاز التجديد، قد يفقد الـ IP أو يحاول الحصول على IP جديد.

في الشبكات الطبيعية، هذا غالبًا لا يسبب مشكلة، لأن التجديد يحدث تلقائيًا قبل انتهاء المدة.

---

# DHCP داخل نفس الشبكة

لو الجهاز وDHCP Server داخل نفس الشبكة، الموضوع سهل.

```text
Client and DHCP Server in same LAN
```

الجهاز يرسل Broadcast، والسيرفر يسمعه ويرد.

---

# DHCP مع VLANs أو شبكات مختلفة

Broadcast لا يعبر الراوتر بشكل طبيعي.

يعني لو عندك VLAN 10 وفيها أجهزة، وDHCP Server موجود في VLAN أخرى، فالـ Discover لن يصل له مباشرة.

الحل هو:

```text
DHCP Relay
```

أو في أجهزة Cisco:

```text
ip helper-address
```

وظيفته:

```text
يأخذ Broadcast من الجهاز
ويرسله Unicast إلى DHCP Server
```

---

# مثال DHCP Relay

```text
Client in VLAN 10
DHCP Server in VLAN 99
```

الجهاز يرسل:

```text
DHCP Discover
```

الراوتر أو Layer 3 Switch يستقبل الرسالة، ثم يرسلها إلى DHCP Server.

ويخبره أيضًا من أي شبكة جاءت الرسالة، حتى يعطي IP من الـ Pool الصحيح.

---

# DHCP Reservation

أحيانًا تريد جهازًا معينًا يحصل دائمًا على نفس IP.

مثال:

```text
Printer
Camera
Server
NAS
```

يمكن عمل:

```text
DHCP Reservation
```

يعني DHCP Server يربط بين:

```text
MAC Address
IP Address
```

مثال:

```text
Printer MAC = AA:BB:CC:DD:EE:FF
Reserved IP = 192.168.1.20
```

كل مرة تتصل الطابعة بالشبكة، تأخذ نفس الـ IP.

---

# Static IP vs DHCP Reservation

## Static IP

تكتب IP يدويًا داخل الجهاز نفسه.

مثال:

```text
IP Address      = 192.168.1.20
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.1.1
DNS Server      = 8.8.8.8
```

## DHCP Reservation

تترك الجهاز على DHCP، لكن السيرفر يعطيه نفس IP دائمًا بناءً على MAC Address.

غالبًا DHCP Reservation أفضل في الشبكات المنظمة، لأن الإدارة تكون من مكان واحد.

---

# ماذا يحدث لو فشل DHCP؟

لو الجهاز لم يستطع الوصول إلى DHCP Server، قد يأخذ عنوانًا تلقائيًا من نوع:

```text
169.254.x.x
```

هذا يسمى:

```text
APIPA
```

مثال:

```text
169.254.20.15
```

هذا يعني غالبًا:

```text
الجهاز لم يحصل على IP من DHCP
```

في هذه الحالة، الجهاز قد يتواصل فقط مع أجهزة أخرى في نفس نطاق APIPA، لكنه غالبًا لن يستطيع الوصول للإنترنت.

---

# DHCP و ARP

قبل أن يعطي DHCP Server عنوانًا لجهاز، قد يتأكد أن العنوان غير مستخدم.

كما أن الجهاز بعد أخذ IP قد يستخدم ARP للتأكد أو للتواصل مع الـ Gateway.

العلاقة المهمة:

```text
DHCP يعطي IP
ARP يعرف MAC
```

مثال:

```text
DHCP:
خذ IP = 192.168.1.50

ARP:
من لديه 192.168.1.1؟
أريد MAC الراوتر
```

---

# DHCP و DNS

الـ DHCP لا يعطي IP فقط.

هو غالبًا يعطي DNS Server أيضًا.

مثال:

```text
DNS Server = 8.8.8.8
```

وبدون DNS، قد يكون عندك إنترنت من ناحية IP، لكن المواقع لا تفتح بالأسماء.

مثال:

```text
google.com لا يفتح
لكن 8.8.8.8 يعمل Ping
```

هنا قد تكون المشكلة في DNS.

---

# DHCP و Default Gateway

الـ DHCP يعطي الجهاز عنوان الـ Default Gateway.

مثال:

```text
Default Gateway = 192.168.1.1
```

بدون Default Gateway، الجهاز قد يتواصل مع الأجهزة داخل نفس الشبكة فقط، لكنه لا يستطيع الخروج للإنترنت.

---

# DHCP في رحلة فتح موقع

عندما تفتح جهازك وتتصل بالواي فاي:

```text
1. DHCP يعطيك IP
2. DHCP يعطيك Subnet Mask
3. DHCP يعطيك Default Gateway
4. DHCP يعطيك DNS Server
5. بعدها تستطيع استخدام DNS لمعرفة IP الموقع
6. بعدها تستخدم ARP لمعرفة MAC الراوتر
7. بعدها تخرج للإنترنت عبر الراوتر
```

---

# أوامر مفيدة

## Windows

عرض إعدادات الشبكة:

```cmd
ipconfig /all
```

تجديد DHCP:

```cmd
ipconfig /release
ipconfig /renew
```

---

## Linux

عرض الإعدادات:

```bash
ip addr
ip route
```

طلب IP جديد:

```bash
sudo dhclient -r
sudo dhclient
```

---

## macOS

عرض الإعدادات:

```bash
ifconfig
networksetup -getinfo Wi-Fi
```

تجديد DHCP غالبًا من إعدادات الشبكة:

```text
System Settings → Network → Wi-Fi → Details → TCP/IP → Renew DHCP Lease
```

---

# مشاكل شائعة

## 1. الجهاز أخذ 169.254.x.x

السبب المحتمل:

```text
DHCP Server لا يرد
الكابل غير متصل
مشكلة في Wi-Fi
VLAN غلط
DHCP Relay غير مضبوط
```

---

## 2. الجهاز أخذ IP لكن لا يوجد إنترنت

افحص:

```text
Default Gateway
DNS Server
Firewall
NAT
ISP
```

---

## 3. بعض الأجهزة تأخذ IP وبعضها لا

قد يكون السبب:

```text
IP Pool امتلأ
DHCP Server متوقف
مشكلة VLAN
مشكلة Access Point
```

---

## 4. جهاز يأخذ IP خطأ

قد يكون هناك:

```text
Rogue DHCP Server
```

يعني DHCP Server غريب داخل الشبكة يعطي إعدادات خاطئة.

---

# ملخص سريع

```text
DHCP = يعطي إعدادات الشبكة تلقائيًا
DORA = Discover, Offer, Request, ACK
DHCP uses UDP 67/68
IP Pool = مجموعة العناوين التي يوزعها DHCP
Lease Time = مدة استخدام IP
DHCP Reservation = نفس IP لنفس MAC
DHCP Relay = نقل طلبات DHCP بين VLANs أو شبكات مختلفة
```

---

# أهم القواعد

```text
بدون DHCP:
ستحتاج تكتب IP وGateway وDNS يدويًا
```

```text
بدون Gateway:
الجهاز لن يخرج خارج الشبكة المحلية
```

```text
بدون DNS:
المواقع قد لا تفتح بالأسماء
```

```text
DHCP يعطي IP
DNS يحول الاسم إلى IP
ARP يحول IP المحلي إلى MAC
Gateway يخرجك إلى الإنترنت
```

---

## Next Topic

اقرأ بعد ذلك:

[DNS How Domain Names Work](./DNS-How-Domain-Names-Work.md)
