# Switch Layer 2

## السويتش الذكي داخل الشبكة المحلية

في الشبكات المحلية، الأجهزة تحتاج تتواصل مع بعضها داخل نفس المكان، مثل:

```text
Laptop
Phone
Printer
Server
Access Point
```

الجهاز الذي ينظم هذا الاتصال غالبًا هو:

```text
Switch
```

السويتش يعمل أساسًا في:

```text
Layer 2 - Data Link Layer
```

ويستخدم:

```text
MAC Address
```

حتى يعرف أين يرسل الـ Frames.

---

# لماذا نحتاج Switch؟

لو عندك أكثر من جهاز في نفس الشبكة، تحتاج جهاز يربطهم ببعض.

قديمًا كان يوجد جهاز اسمه:

```text
Hub
```

لكن الـ Hub كان غبيًا جدًا.

لو جهاز أرسل بيانات، الـ Hub يرسلها لكل الأجهزة.

```text
PC1 sends data to PC2
Hub sends it to PC2, PC3, PC4, PC5...
```

هذا يسبب:

```text
ازدحام
تصادمات
ضعف أمان
بطء في الشبكة
```

السويتش جاء ليحل هذه المشكلة.

---

# الفرق بين Hub و Switch

## Hub

الـ Hub يعمل بطريقة بسيطة:

```text
أي شيء يدخل من منفذ
يرسله لكل المنافذ الأخرى
```

يعني لا يفهم MAC Address ولا يعرف مكان الأجهزة.

مثال:

```text
PC1 → Hub → Everyone
```

---

## Switch

السويتش أذكى.

هو يتعلم مكان كل جهاز من خلال MAC Address.

مثال:

```text
PC1 موجود على Port 1
PC2 موجود على Port 2
Printer موجودة على Port 5
```

بعدها لو PC1 يريد إرسال بيانات إلى PC2، السويتش يرسلها فقط إلى Port 2.

```text
PC1 → Switch → PC2 فقط
```

---

# التشبيه البسيط

تخيل شركة فيها موظفين.

## Hub

عامل الاستقبال الغبي:

```text
أي جواب يصل له، يصوره ويوزعه على كل الموظفين
```

## Switch

عامل استقبال ذكي:

```text
يعرف مكتب كل موظف
ويرسل الجواب للشخص المطلوب فقط
```

---

# السويتش يعمل بأي عنوان؟

السويتش يستخدم:

```text
MAC Address
```

وليس IP Address في عملية التحويل العادية.

مثال MAC Address:

```text
AA:BB:CC:DD:EE:FF
```

MAC Address هو عنوان كرت الشبكة في Layer 2.

---

# ما هو Frame؟

في Layer 2، اسم وحدة البيانات هو:

```text
Frame
```

الـ Frame يحتوي على معلومات مثل:

```text
Source MAC
Destination MAC
EtherType
Data
FCS
```

شكل مبسط:

```text
┌───────────────┬──────────────────┬──────────────┬──────┬─────┐
│ Destination   │ Source           │ EtherType    │ Data │ FCS │
│ MAC           │ MAC              │              │      │     │
└───────────────┴──────────────────┴──────────────┴──────┴─────┘
```

السويتش يقرأ أهم جزء:

```text
Destination MAC
```

حتى يعرف أين يرسل الـ Frame.

---

# ما هو CAM Table؟

السويتش يحتفظ بجدول داخلي اسمه:

```text
CAM Table
```

أو أحيانًا يسمى:

```text
MAC Address Table
```

هذا الجدول يربط بين:

```text
MAC Address
Port Number
VLAN
```

مثال:

```text
┌───────────────────┬───────┬──────┐
│ MAC Address       │ Port  │ VLAN │
├───────────────────┼───────┼──────┤
│ AA:AA:AA:AA:AA:AA │ 1     │ 10   │
│ BB:BB:BB:BB:BB:BB │ 2     │ 10   │
│ CC:CC:CC:CC:CC:CC │ 5     │ 20   │
└───────────────────┴───────┴──────┘
```

---

# كيف يتعلم السويتش؟

السويتش يتعلم من:

```text
Source MAC Address
```

وليس من Destination MAC.

يعني عندما يستقبل Frame من جهاز، ينظر إلى Source MAC ويقول:

```text
هذا الـ MAC جاء من هذا الـ Port
إذن هذا الجهاز موجود هنا
```

مثال:

```text
Frame دخل من Port 1
Source MAC = AA:AA:AA:AA:AA:AA
```

السويتش يحفظ:

```text
AA:AA:AA:AA:AA:AA → Port 1
```

---

# القواعد الثلاثة للسويتش

السويتش يعمل بثلاث قواعد مهمة:

```text
Learning
Forwarding
Flooding
```

---

## 1. Learning

عندما يدخل Frame إلى السويتش، يتعلم السويتش Source MAC.

مثال:

```text
Source MAC = AA:AA:AA:AA:AA:AA
Input Port = 1
```

السويتش يضيف إلى CAM Table:

```text
AA:AA:AA:AA:AA:AA → Port 1
```

---

## 2. Forwarding

بعد أن يتعلم Source MAC، ينظر إلى Destination MAC.

لو Destination MAC موجود في CAM Table، يرسل الـ Frame إلى المنفذ الصحيح فقط.

مثال:

```text
Destination MAC = BB:BB:BB:BB:BB:BB
CAM Table says = Port 2
```

إذن:

```text
Forward to Port 2 only
```

---

## 3. Flooding

لو Destination MAC غير موجود في CAM Table، السويتش لا يعرف أين الجهاز المطلوب.

في هذه الحالة يعمل Flooding.

يعني يرسل الـ Frame لكل المنافذ داخل نفس VLAN، ما عدا المنفذ الذي جاء منه الـ Frame.

مثال:

```text
Unknown Destination MAC
↓
Send to all ports in same VLAN except incoming port
```

---

# مثال عملي

لدينا 3 أجهزة على سويتش:

```text
PC1 on Port 1
PC2 on Port 2
PC3 on Port 3
```

في البداية، CAM Table فارغ.

---

## الخطوة 1: PC1 يرسل إلى PC2

PC1 يرسل Frame:

```text
Source MAC      = AA:AA:AA:AA:AA:AA
Destination MAC = BB:BB:BB:BB:BB:BB
```

السويتش يستقبل الـ Frame من Port 1.

أول شيء يتعلم:

```text
AA:AA:AA:AA:AA:AA → Port 1
```

ثم يبحث عن Destination MAC:

```text
BB:BB:BB:BB:BB:BB
```

لا يجده.

إذن يعمل Flooding إلى Port 2 و Port 3.

---

## الخطوة 2: PC2 يرد على PC1

PC2 يستقبل الرسالة ويرد.

Frame الجديد:

```text
Source MAC      = BB:BB:BB:BB:BB:BB
Destination MAC = AA:AA:AA:AA:AA:AA
```

السويتش يستقبل الرد من Port 2.

يتعلم:

```text
BB:BB:BB:BB:BB:BB → Port 2
```

ثم يبحث عن Destination MAC:

```text
AA:AA:AA:AA:AA:AA
```

يجده في CAM Table على Port 1.

إذن يرسل الرد إلى Port 1 فقط.

---

## بعد التعلم

CAM Table أصبح:

```text
AA:AA:AA:AA:AA:AA → Port 1
BB:BB:BB:BB:BB:BB → Port 2
```

بعد ذلك، أي اتصال بين PC1 وPC2 سيكون مباشرًا من خلال السويتش بدون Flooding.

---

# Unknown Unicast

لو Destination MAC غير معروف، يسمى هذا:

```text
Unknown Unicast
```

والسويتش يتعامل معه بـ:

```text
Flooding
```

داخل نفس VLAN فقط.

---

# Broadcast

Broadcast يعني رسالة موجهة لكل الأجهزة داخل نفس Broadcast Domain.

Destination MAC للـ Broadcast هو:

```text
FF:FF:FF:FF:FF:FF
```

مثال مشهور:

```text
ARP Request
```

عندما يرسل جهاز ARP Request:

```text
Who has 192.168.1.1?
```

يكون Destination MAC:

```text
FF:FF:FF:FF:FF:FF
```

السويتش يرسل هذا الـ Frame لكل المنافذ داخل نفس VLAN.

---

# السويتش و ARP

السويتش لا يحول IP إلى MAC.

هذه وظيفة ARP في الأجهزة.

لكن السويتش ينقل ARP Frames داخل الشبكة.

عند ARP Request:

```text
1. الجهاز يرسل Broadcast
2. السويتش يتعلم Source MAC
3. السويتش يعمل Flooding داخل نفس VLAN
4. الجهاز المطلوب يرد بـ ARP Reply
5. السويتش يتعلم MAC الجهاز المطلوب
6. بعد ذلك التواصل يصبح مباشرًا
```

---

# السويتش و IP

السويتش العادي في Layer 2 لا يوجه بناءً على IP.

هو يهتم بـ:

```text
MAC Address
```

لكن قد يكون للسويتش IP خاص للإدارة فقط.

مثال:

```text
Switch Management IP = 192.168.1.2
```

هذا الـ IP لا يعني أن السويتش يوجه الترافيك مثل الراوتر.

هو فقط لإدارته عن بعد.

---

# Switch vs Router

## Switch

```text
Layer 2
Uses MAC Address
Connects devices inside same LAN
Forwards Frames
```

## Router

```text
Layer 3
Uses IP Address
Connects different networks
Routes Packets
```

مثال:

```text
داخل البيت بين اللابتوب والطابعة → Switch
من البيت إلى الإنترنت → Router
```

---

# Collision Domain

قديمًا مع Hub، كل الأجهزة كانت داخل Collision Domain واحد.

يعني لو جهازان أرسلا في نفس الوقت، قد يحدث تصادم.

مع السويتش:

```text
كل Port يعتبر Collision Domain مستقل
```

هذا يقلل التصادمات ويزيد الأداء.

مثال:

```text
Port 1 = Collision Domain
Port 2 = Collision Domain
Port 3 = Collision Domain
```

---

# Broadcast Domain

Broadcast Domain هو النطاق الذي تنتشر داخله رسائل Broadcast.

السويتش بدون VLANs يكون غالبًا Broadcast Domain واحد.

يعني لو جهاز أرسل Broadcast، سيصل لكل الأجهزة على نفس السويتش.

لكن باستخدام VLANs، نقسم السويتش إلى Broadcast Domains منفصلة.

مثال:

```text
VLAN 10 = Broadcast Domain
VLAN 20 = Broadcast Domain
```

Broadcast في VLAN 10 لا يصل إلى VLAN 20.

---

# VLANs باختصار

VLAN تعني:

```text
Virtual LAN
```

وهي طريقة لتقسيم السويتش الواحد إلى شبكات منطقية متعددة.

مثال:

```text
VLAN 10 = Employees
VLAN 20 = Guests
VLAN 30 = Servers
```

كل VLAN منفصلة عن الأخرى.

للتواصل بين VLANs، نحتاج:

```text
Router
أو Layer 3 Switch
```

---

# Access Port

Access Port هو منفذ يوصل جهاز عادي مثل:

```text
Laptop
Printer
Camera
IP Phone
```

غالبًا يكون تابعًا لـ VLAN واحدة فقط.

مثال:

```text
Port 1 → VLAN 10
Port 2 → VLAN 10
Port 5 → VLAN 20
```

الجهاز المتصل عادة لا يعرف شيئًا عن VLAN Tag.

---

# Trunk Port

Trunk Port هو منفذ يحمل أكثر من VLAN.

غالبًا يستخدم بين:

```text
Switch ↔ Switch
Switch ↔ Router
Switch ↔ Access Point
Switch ↔ Firewall
```

حتى يعرف السويتش لأي VLAN ينتمي الـ Frame، يستخدم Tag اسمه:

```text
802.1Q
```

---

# 802.1Q Tag

802.1Q هو Tag يضاف داخل Ethernet Frame على الـ Trunk.

وظيفته:

```text
تحديد VLAN ID
```

مثال:

```text
Frame belongs to VLAN 10
Frame belongs to VLAN 20
```

عندما يخرج Frame إلى Access Port، يتم إزالة الـ Tag غالبًا قبل وصوله للجهاز العادي.

---

# MAC Aging

السويتش لا يحتفظ بالـ MAC Addresses إلى الأبد.

لو جهاز اختفى أو انتقل لمنفذ آخر، يجب أن يحدث الجدول.

لذلك يستخدم السويتش:

```text
MAC Aging Timer
```

يعني بعد فترة بدون أي Traffic من هذا MAC، يتم حذف Entry من CAM Table.

في كثير من الأجهزة، الوقت الافتراضي يكون تقريبًا:

```text
300 seconds
```

يعني حوالي 5 دقائق.

---

# ماذا يحدث لو نقلت جهاز من Port إلى Port؟

لو جهاز كان على Port 1 ثم نقلته إلى Port 5، السويتش سيتعلم من جديد.

عندما يرسل الجهاز أي Frame من Port 5، السويتش يرى:

```text
Source MAC = نفس الجهاز
Input Port = 5
```

فيحدث CAM Table:

```text
MAC → Port 5
```

---

# Full-Duplex و Half-Duplex

## Half-Duplex

الجهاز لا يستطيع الإرسال والاستقبال في نفس الوقت.

كان شائعًا مع Hub.

```text
إما إرسال أو استقبال
```

## Full-Duplex

الجهاز يستطيع الإرسال والاستقبال في نفس الوقت.

هذا هو الوضع الطبيعي مع السويتشات الحديثة.

```text
Send and Receive at the same time
```

---

# Loop Problem

لو وصلت سويتشين ببعض بأكثر من كابل بدون حماية، قد يحدث Loop.

المشكلة أن Broadcast Frames قد تدور إلى ما لا نهاية.

هذا قد يسبب:

```text
Broadcast Storm
بطء شديد
انهيار الشبكة
ارتفاع CPU في الأجهزة
```

---

# Spanning Tree Protocol

لحل مشكلة الـ Loops، تستخدم السويتشات بروتوكول:

```text
STP = Spanning Tree Protocol
```

وظيفته:

```text
منع Layer 2 Loops
```

يعمل عن طريق تعطيل بعض المسارات الزائدة مؤقتًا، ويترك مسارًا آمنًا.

لو المسار الأساسي فشل، يمكنه تفعيل مسار بديل.

---

# السويتش في رحلة فتح موقع

عندما تفتح:

```text
https://google.com
```

جهازك غالبًا يرسل Frame إلى الراوتر.

الـ Frame يكون مثلًا:

```text
Source MAC      = Laptop MAC
Destination MAC = Router MAC
Source IP       = Laptop IP
Destination IP  = Google IP
Destination Port = 443
```

السويتش يقرأ:

```text
Destination MAC = Router MAC
```

ثم يرسل الـ Frame إلى المنفذ المتصل بالراوتر.

السويتش لا يهتم بأن الموقع هو google.com، ولا يهتم بـ HTTP أو TLS.

هو فقط ينقل الـ Frame حسب MAC.

---

# أوامر مفيدة على Cisco

عرض جدول MAC:

```cisco
show mac address-table
```

عرض حالة المنافذ:

```cisco
show interfaces status
```

عرض VLANs:

```cisco
show vlan brief
```

عرض Trunk Ports:

```cisco
show interfaces trunk
```

عرض STP:

```cisco
show spanning-tree
```

---

# أوامر مفيدة على جهازك

## Windows

عرض MAC Address:

```cmd
ipconfig /all
```

أو:

```cmd
getmac
```

## Linux

```bash
ip link
```

أو:

```bash
ifconfig
```

## macOS

```bash
ifconfig
```

---

# مشاكل شائعة

## 1. الجهاز لا يرى الشبكة

قد يكون السبب:

```text
الكابل مفصول
Port مغلق
VLAN خطأ
مشكلة في كرت الشبكة
```

---

## 2. الجهاز أخذ IP لكن لا يصل للراوتر

افحص:

```text
هل الجهاز في VLAN صحيحة؟
هل السويتش تعلم MAC الجهاز؟
هل الراوتر في نفس VLAN؟
هل هناك Port Security؟
```

---

## 3. الشبكة بطيئة جدًا فجأة

قد يكون السبب:

```text
Broadcast Storm
Loop
كابل تالف
Duplex Mismatch
جهاز يرسل Traffic عالي
```

---

## 4. جهاز لا يستطيع التواصل مع VLAN أخرى

هذا طبيعي إذا لا يوجد:

```text
Inter-VLAN Routing
```

تحتاج:

```text
Router
أو Layer 3 Switch
```

---

# ملخص سريع

```text
Switch = يعمل غالبًا في Layer 2
Switch uses MAC Address
Switch forwards Frames
CAM Table = جدول MAC Address إلى Port
Learning = يتعلم Source MAC
Forwarding = يرسل لـ Destination MAC المعروف
Flooding = يرسل للجميع إذا Destination MAC غير معروف أو Broadcast
Broadcast = FF:FF:FF:FF:FF:FF
VLAN = تقسيم الشبكة إلى Broadcast Domains
Trunk = يحمل أكثر من VLAN
STP = يمنع Layer 2 Loops
```

---

# أهم القواعد

```text
Switch uses MAC
Router uses IP
```

```text
Switch learns from Source MAC
Switch forwards based on Destination MAC
```

```text
Unknown Unicast = Flooding
Broadcast = Flooding
Known Unicast = Forwarding
```

```text
ARP Request is Broadcast
ARP Reply is usually Unicast
```

```text
VLAN separates Broadcast Domains
```

---

## Next Topic

اقرأ بعد ذلك:

[VLANs and Trunking](./VLANs-and-Trunking.md)
