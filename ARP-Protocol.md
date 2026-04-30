# ARP Protocol

## الجسر بين IP و MAC

في الدروس السابقة عرفنا أن:

```text
IP Address  = Layer 3
MAC Address = Layer 2
```

لكن هنا يظهر سؤال مهم جدًا:

```text
لو الجهاز يعرف IP الوجهة، كيف يعرف MAC Address الذي سيرسل له الـ Frame؟
```

الإجابة هي:

```text
ARP
```

---

# ما هو ARP؟

ARP اختصار لـ:

```text
Address Resolution Protocol
```

ومعناه بروتوكول يستخدم لمعرفة MAC Address الخاص بجهاز معين عند معرفة IP Address الخاص به.

ببساطة:

```text
ARP = يحول IP إلى MAC داخل الشبكة المحلية
```

مثال:

```text
أنا أعرف أن الراوتر IP الخاص به هو 192.168.1.1
لكن أحتاج أعرف MAC Address الخاص به
```

فيستخدم الجهاز ARP ليسأل:

```text
Who has 192.168.1.1?
```

والراوتر يرد:

```text
192.168.1.1 is at AA:BB:CC:DD:EE:FF
```

---

# لماذا نحتاج ARP؟

في Layer 3، الجهاز يتعامل مع IP.

لكن في Layer 2، السويتش وكرت الشبكة يتعاملان مع MAC.

عندما يريد الجهاز إرسال بيانات، لازم يبني:

```text
Frame
```

والـ Frame يحتاج:

```text
Source MAC
Destination MAC
```

الجهاز يعرف MAC الخاص به، لكنه يحتاج يعرف Destination MAC.

هنا يأتي دور ARP.

---

# مثال بسيط داخل نفس الشبكة

لدينا جهازان داخل نفس الشبكة:

```text
PC1 = 192.168.1.10
PC2 = 192.168.1.20
```

وPC1 يريد إرسال بيانات إلى PC2.

PC1 يعرف IP الخاص بـ PC2:

```text
192.168.1.20
```

لكنه لا يعرف MAC الخاص به.

إذن يرسل ARP Request:

```text
Who has 192.168.1.20?
Tell 192.168.1.10
```

PC2 يرد:

```text
192.168.1.20 is at BB:BB:BB:BB:BB:BB
```

الآن PC1 يستطيع بناء Frame:

```text
Source MAC      = PC1 MAC
Destination MAC = PC2 MAC
```

---

# ARP Request

ARP Request هو سؤال يرسله الجهاز داخل الشبكة المحلية.

لأنه لا يعرف MAC الوجهة، يرسل السؤال كـ Broadcast.

يعني Destination MAC يكون:

```text
FF:FF:FF:FF:FF:FF
```

وهذا يعني:

```text
أرسل هذه الرسالة لكل الأجهزة داخل نفس الشبكة المحلية
```

مثال:

```text
Who has 192.168.1.20?
```

كل الأجهزة تسمع السؤال، لكن الجهاز صاحب IP المطلوب فقط هو الذي يرد.

---

# ARP Reply

ARP Reply هو الرد على ARP Request.

يكون غالبًا Unicast، يعني يذهب للجهاز الذي سأل فقط.

مثال:

```text
192.168.1.20 is at BB:BB:BB:BB:BB:BB
```

بعدها الجهاز يحفظ النتيجة في:

```text
ARP Cache
```

---

# رسم مبسط

```text
┌──────────────┐                              ┌──────────────┐
│     PC1      │                              │     PC2      │
│ 192.168.1.10 │                              │ 192.168.1.20 │
└──────┬───────┘                              └──────┬───────┘
       │                                             │
       │ ARP Request - Broadcast                     │
       │ Who has 192.168.1.20?                       │
       │────────────────────────────────────────────>│
       │                                             │
       │ ARP Reply - Unicast                         │
       │ 192.168.1.20 is at BB:BB:BB:BB:BB:BB        │
       │<────────────────────────────────────────────│
       │                                             │
```

---

# ARP Cache

الجهاز لا يسأل ARP كل مرة.

بعد أن يعرف MAC الخاص بعنوان IP معين، يحفظه مؤقتًا في جدول يسمى:

```text
ARP Cache
```

مثال:

```text
IP Address      MAC Address
192.168.1.1     AA:AA:AA:AA:AA:AA
192.168.1.20    BB:BB:BB:BB:BB:BB
```

بعد فترة، يتم حذف هذه البيانات تلقائيًا حتى يتأكد الجهاز من المعلومات مرة أخرى.

---

# عرض ARP Cache

## Windows

```cmd
arp -a
```

## Linux

```bash
ip neigh
```

أو:

```bash
arp -a
```

## macOS

```bash
arp -a
```

---

# القاعدة الذهبية

ARP يعمل فقط داخل الشبكة المحلية.

```text
ARP does not cross routers
```

يعني ARP لا يسأل عن MAC الخاص بجوجل أو فيسبوك أو أي سيرفر بعيد على الإنترنت.

لو تريد فتح:

```text
google.com
```

وجوجل خارج شبكتك، جهازك لا يحتاج MAC الخاص بجوجل.

هو يحتاج MAC الخاص بـ:

```text
Default Gateway
```

يعني الراوتر.

---

# مثال: فتح موقع خارج الشبكة

جهازك:

```text
IP Address      = 192.168.1.10
Default Gateway = 192.168.1.1
```

تريد الوصول إلى:

```text
Google IP = 142.250.80.46
```

الجهاز يكتشف أن Google خارج الشبكة المحلية.

إذن القرار:

```text
Send to Default Gateway
```

الجهاز يعرف IP الراوتر:

```text
192.168.1.1
```

لكنه يحتاج MAC الخاص بالراوتر.

فيرسل ARP:

```text
Who has 192.168.1.1?
```

الراوتر يرد:

```text
192.168.1.1 is at AA:AA:AA:AA:AA:AA
```

الآن الجهاز يبني Frame:

```text
Source MAC      = Laptop MAC
Destination MAC = Router MAC
Source IP       = 192.168.1.10
Destination IP  = 142.250.80.46
```

لاحظ:

```text
Destination MAC = الراوتر
Destination IP  = Google
```

وهذه نقطة مهمة جدًا.

---

# IP النهائي و MAC للخطوة التالية

عندما ترسل بيانات لموقع بعيد:

```text
IP Address = الوجهة النهائية
MAC Address = الخطوة التالية فقط
```

مثال:

```text
Destination IP  = Google Server
Destination MAC = Home Router
```

الراوتر يستلم الـ Frame، يزيل MAC القديم، ثم يبني Frame جديد للخطوة التالية.

---

# لماذا يتغير MAC عند كل راوتر؟

لأن MAC Address يعمل داخل الشبكة المحلية فقط.

عند كل راوتر، يحدث الآتي:

```text
1. الراوتر يستقبل Frame
2. يزيل Ethernet Header القديم
3. يقرأ Destination IP
4. يحدد Next Hop من Routing Table
5. يبني Frame جديد بـ MAC جديد
```

مثال:

## من جهازك إلى الراوتر

```text
Source MAC      = Laptop MAC
Destination MAC = Home Router MAC
Source IP       = 192.168.1.10
Destination IP  = 142.250.80.46
```

## من الراوتر إلى راوتر ISP

```text
Source MAC      = Home Router MAC
Destination MAC = ISP Router MAC
Source IP       = Public IP بعد NAT
Destination IP  = 142.250.80.46
```

لاحظ:

```text
MAC تغير
IP الوجهة لم يتغير
```

---

# ARP و Switch

السويتش لا يستخدم ARP بنفس الطريقة التي يستخدمها الكمبيوتر.

السويتش يهتم بـ MAC Address ويبني جدولًا اسمه:

```text
CAM Table
```

عندما يمر ARP Request داخل السويتش، يحدث الآتي:

```text
1. السويتش يرى Source MAC ويتعلمه
2. لأن ARP Request Broadcast، يرسله لكل المنافذ داخل نفس VLAN
3. عندما يأتي ARP Reply، يتعلم MAC الجهاز الآخر
4. بعد ذلك يستطيع إرسال Frames مباشرة بدون Flooding
```

---

# ARP و VLAN

كل VLAN تعتبر Broadcast Domain مستقل.

يعني ARP Request داخل VLAN 10 لا يصل إلى VLAN 20.

مثال:

```text
VLAN 10 = Employees
VLAN 20 = Guests
```

لو جهاز في VLAN 10 أرسل ARP Broadcast، سيصل فقط لأجهزة VLAN 10.

ولكي يتواصل جهاز في VLAN 10 مع جهاز في VLAN 20، يحتاج المرور عبر:

```text
Router
أو Layer 3 Switch
```

---

# ARP و DHCP

العلاقة بينهم مهمة:

```text
DHCP يعطي الجهاز IP
ARP يساعد الجهاز يعرف MAC الخاص بالـ Gateway أو الأجهزة المحلية
```

مثال:

```text
1. DHCP يعطي الجهاز IP = 192.168.1.50
2. DHCP يعطيه Gateway = 192.168.1.1
3. الجهاز يستخدم ARP لمعرفة MAC الخاص بـ 192.168.1.1
```

---

# ARP و DNS

DNS وARP لا يقومان بنفس الوظيفة.

```text
DNS = Domain Name إلى IP
ARP = IP محلي إلى MAC
```

مثال عند فتح موقع:

```text
DNS:
google.com → 142.250.80.46

ARP:
192.168.1.1 → AA:AA:AA:AA:AA:AA
```

يعني DNS يعرفك عنوان السيرفر، وARP يعرفك عنوان الجهاز التالي داخل شبكتك.

---

# ARP و ICMP Ping

عندما تعمل Ping لجهاز داخل نفس الشبكة:

```cmd
ping 192.168.1.20
```

قبل إرسال ICMP Echo Request، جهازك يحتاج MAC الخاص بـ 192.168.1.20.

لو غير موجود في ARP Cache، سيحدث:

```text
ARP Request
ARP Reply
ثم ICMP Ping
```

---

# مثال عملي كامل داخل الشبكة

الجهاز A:

```text
IP  = 192.168.1.10
MAC = AA:AA:AA:AA:AA:AA
```

الجهاز B:

```text
IP  = 192.168.1.20
MAC = BB:BB:BB:BB:BB:BB
```

الجهاز A يريد إرسال Ping للجهاز B.

الخطوات:

```text
1. A يفحص ARP Cache
2. لا يجد MAC الخاص بـ 192.168.1.20
3. A يرسل ARP Request كـ Broadcast
4. السويتش يعمل Flooding للرسالة
5. B يرى أن السؤال عن IP الخاص به
6. B يرسل ARP Reply إلى A
7. A يحفظ IP/MAC في ARP Cache
8. A يرسل ICMP Echo Request داخل Frame إلى MAC الخاص بـ B
```

---

# مثال عملي خارج الشبكة

الجهاز:

```text
IP Address      = 192.168.1.10
Default Gateway = 192.168.1.1
```

الوجهة:

```text
8.8.8.8
```

الخطوات:

```text
1. الجهاز يعرف أن 8.8.8.8 خارج الشبكة المحلية
2. يقرر إرسال البيانات إلى Default Gateway
3. يفحص ARP Cache بحثًا عن MAC الخاص بـ 192.168.1.1
4. لو غير موجود، يرسل ARP Request
5. الراوتر يرد بـ ARP Reply
6. الجهاز يبني Frame إلى MAC الراوتر
7. الراوتر يستلم Packet ويوجهها للإنترنت
```

---

# Gratuitous ARP

Gratuitous ARP هو ARP يرسله الجهاز عن نفسه بدون أن يسأله أحد.

مثال:

```text
أنا 192.168.1.50 و MAC الخاص بي هو CC:CC:CC:CC:CC:CC
```

يستخدم في حالات مثل:

```text
اكتشاف IP Conflict
تحديث ARP Cache عند الأجهزة الأخرى
High Availability Failover
```

مثال:

لو راوتر احتياطي أصبح هو الراوتر الرئيسي، قد يرسل Gratuitous ARP ليخبر الأجهزة:

```text
IP الخاص بالـ Gateway أصبح على MAC الجديد
```

---

# ARP Spoofing

ARP بسيط جدًا، لكنه لا يحتوي على Authentication.

يعني الجهاز قد يصدق رد ARP حتى لو كان الرد مزيفًا.

هذا يؤدي إلى هجوم اسمه:

```text
ARP Spoofing
```

أو:

```text
ARP Poisoning
```

---

## فكرة الهجوم

المهاجم داخل نفس الشبكة يرسل رسائل ARP مزيفة.

مثال:

```text
أنا الراوتر 192.168.1.1
و MAC الخاص بي هو MAC المهاجم
```

الجهاز الضحية يصدق الكلام ويحفظه في ARP Cache.

بعدها يرسل البيانات إلى المهاجم بدل الراوتر.

---

# Man-in-the-Middle

ARP Spoofing قد يسمح بهجوم:

```text
MITM = Man-in-the-Middle
```

يعني المهاجم يصبح في المنتصف بينك وبين الراوتر.

```text
Victim → Attacker → Router → Internet
```

المهاجم قد يحاول:

```text
مراقبة الترافيك
تعديل الترافيك
قطع الاتصال
تحويل الضحية لمواقع مزيفة
```

---

# هل HTTPS يحمي من ARP Spoofing؟

HTTPS يحمي محتوى الاتصال بالتشفير.

يعني حتى لو مر الترافيك على المهاجم، فلن يستطيع قراءة محتوى HTTPS بسهولة.

لكن ARP Spoofing ما زال خطيرًا لأنه قد يسمح بـ:

```text
قطع الاتصال
إعادة التوجيه
محاولة خداع المستخدم بشهادة مزيفة
DNS Spoofing مع هجمات أخرى
```

لو ظهرت لك رسالة Certificate Warning في المتصفح، لا تتجاهلها.

---

# الحماية من ARP Spoofing

طرق الحماية تشمل:

```text
استخدام HTTPS دائمًا
عدم تجاهل تحذيرات الشهادات
استخدام شبكات موثوقة
تقسيم الشبكة بـ VLANs
تفعيل Dynamic ARP Inspection على السويتشات المدارة
استخدام DHCP Snooping
استخدام VPN في الشبكات العامة
```

في الشركات، من أهم التقنيات:

```text
Dynamic ARP Inspection = DAI
```

وهي تمنع رسائل ARP المزيفة اعتمادًا على معلومات موثوقة من DHCP Snooping.

---

# أوامر مفيدة

## Windows

عرض ARP Cache:

```cmd
arp -a
```

حذف ARP Cache:

```cmd
arp -d *
```

---

## Linux

عرض الجيران:

```bash
ip neigh
```

حذف Entry معين:

```bash
sudo ip neigh del 192.168.1.1 dev eth0
```

---

## macOS

عرض ARP Cache:

```bash
arp -a
```

حذف Entry معين:

```bash
sudo arp -d 192.168.1.1
```

---

# ملخص سريع

```text
ARP = يحول IP إلى MAC داخل الشبكة المحلية
ARP Request = Broadcast
ARP Reply = غالبًا Unicast
ARP Cache = جدول مؤقت لحفظ IP/MAC
ARP لا يعبر الراوتر
MAC يتغير عند كل Hop
IP يمثل الوجهة النهائية
```

---

# أهم القواعد

```text
DNS gives IP
ARP gives MAC
```

```text
If destination is inside my LAN:
ARP for destination device MAC
```

```text
If destination is outside my LAN:
ARP for Default Gateway MAC
```

```text
Switch forwards using MAC
Router routes using IP
```

```text
ARP is local only
```

---

## Next Topic

اقرأ بعد ذلك:

[Switch Layer 2](./Switch-Layer-2.md)
