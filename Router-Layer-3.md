# Router Layer 3

## الراوتر وعالم الطبقة الثالثة

في الدروس السابقة عرفنا أن:

```text
Switch = Layer 2 = MAC Address
Router = Layer 3 = IP Address
```

السويتش يربط الأجهزة داخل نفس الشبكة المحلية.

أما الراوتر، فوظيفته الأساسية هي:

```text
ربط الشبكات المختلفة ببعض
```

مثال:

```text
شبكة البيت → الإنترنت
فرع شركة → فرع آخر
VLAN 10 → VLAN 20
LAN → WAN
```

---

# ما هو Router؟

الراوتر هو جهاز يعمل غالبًا في:

```text
Layer 3 - Network Layer
```

ويستخدم:

```text
IP Address
```

حتى يقرر أين يرسل الـ Packet.

الراوتر لا يعتمد على MAC Address لاتخاذ قرار التوجيه النهائي.  
هو يقرأ:

```text
Destination IP
```

ثم يبحث في:

```text
Routing Table
```

حتى يعرف أفضل طريق للوجهة.

---

# لماذا نحتاج Router؟

نحتاج الراوتر عندما نريد إرسال بيانات إلى شبكة مختلفة.

مثال:

جهازك:

```text
192.168.1.10
```

يريد الوصول إلى:

```text
8.8.8.8
```

الجهاز يرى أن 8.8.8.8 خارج الشبكة المحلية.

إذن يرسل البيانات إلى:

```text
Default Gateway
```

وهو غالبًا الراوتر.

---

# Switch vs Router

| المقارنة | Switch | Router |
|---|---|---|
| الطبقة | Layer 2 | Layer 3 |
| يستخدم | MAC Address | IP Address |
| ينقل | Frames | Packets |
| يربط | أجهزة داخل نفس الشبكة | شبكات مختلفة |
| الجدول | CAM / MAC Table | Routing Table |
| مثال | Laptop مع Printer | شبكة البيت مع الإنترنت |

---

# مثال بسيط

## داخل نفس الشبكة

```text
PC1 = 192.168.1.10
PC2 = 192.168.1.20
```

هنا PC1 وPC2 داخل نفس الشبكة:

```text
192.168.1.0/24
```

الاتصال يتم غالبًا عبر:

```text
Switch
```

باستخدام:

```text
MAC Address
```

---

## خارج الشبكة

```text
PC1    = 192.168.1.10
Google = 142.250.80.46
```

هنا Google خارج شبكة البيت.

إذن PC1 يرسل البيانات إلى:

```text
Default Gateway = Router
```

والراوتر يكمل الطريق إلى الإنترنت.

---

# ما هو Routing؟

Routing يعني:

```text
اختيار الطريق المناسب لإرسال Packet إلى Destination IP
```

الراوتر يقرأ Destination IP، ثم يسأل نفسه:

```text
هل أعرف طريقًا لهذه الشبكة؟
```

لو يعرف، يرسل Packet من المنفذ المناسب.

لو لا يعرف، قد يستخدم:

```text
Default Route
```

ولو لا توجد Default Route، يتم إسقاط الـ Packet.

---

# Routing Table

Routing Table هو جدول داخل الراوتر يحتوي على الطرق المعروفة.

مثال مبسط:

```text
Destination Network     Next Hop        Interface
192.168.1.0/24          Connected       G0/0
10.0.0.0/24             192.168.1.2     G0/0
0.0.0.0/0               41.33.20.1      G0/1
```

معنى الجدول:

```text
192.168.1.0/24 متصلة مباشرة بالراوتر
10.0.0.0/24 موجودة خلف راوتر آخر
0.0.0.0/0 هي الطريق الافتراضي إلى الإنترنت
```

---

# Directly Connected Routes

عندما يكون الراوتر متصلًا مباشرة بشبكة، يضيفها تلقائيًا إلى Routing Table.

مثال:

```text
Router Interface G0/0 = 192.168.1.1/24
```

إذن الراوتر يعرف مباشرة شبكة:

```text
192.168.1.0/24
```

وتظهر كـ:

```text
Connected Route
```

---

# Static Route

Static Route هو طريق تضيفه يدويًا.

مثال:

```text
ip route 10.10.10.0 255.255.255.0 192.168.1.2
```

معناه:

```text
للوصول إلى شبكة 10.10.10.0/24
أرسل إلى Next Hop 192.168.1.2
```

Static Route مفيد في الشبكات الصغيرة أو الطرق الثابتة.

---

# Dynamic Routing

في الشبكات الكبيرة، من الصعب كتابة كل Route يدويًا.

لذلك نستخدم Dynamic Routing Protocols.

أمثلة:

```text
RIP
OSPF
EIGRP
BGP
```

هذه البروتوكولات تجعل الراوترات تتبادل معلومات الشبكات تلقائيًا.

مثال:

```text
Router A يعرف شبكة 192.168.1.0/24
Router B يعرف شبكة 10.0.0.0/24

باستخدام OSPF، كل راوتر يخبر الآخر بالشبكات التي يعرفها
```

---

# Default Route

Default Route هو الطريق المستخدم عندما لا يوجد Route محدد للوجهة.

يكتب غالبًا بهذا الشكل:

```text
0.0.0.0/0
```

معناه:

```text
أي وجهة لا أعرفها، أرسلها من هنا
```

في راوتر البيت، الـ Default Route غالبًا يشير إلى مزود الخدمة ISP.

مثال:

```text
0.0.0.0/0 → ISP Gateway
```

---

# Longest Prefix Match

عندما يكون عند الراوتر أكثر من Route ينطبق على نفس الوجهة، يختار الأكثر تحديدًا.

هذه القاعدة تسمى:

```text
Longest Prefix Match
```

مثال Routing Table:

```text
10.0.0.0/8        → Route A
10.10.0.0/16      → Route B
10.10.10.0/24     → Route C
0.0.0.0/0         → Default Route
```

لو Destination IP هو:

```text
10.10.10.5
```

فالراوتر يختار:

```text
10.10.10.0/24
```

لأنه الأكثر تحديدًا.

---

# Administrative Distance

لو الراوتر تعلم نفس الشبكة من أكثر من مصدر، يستخدم قيمة اسمها:

```text
Administrative Distance
```

لتحديد أي مصدر أكثر ثقة.

أمثلة شائعة:

```text
Connected = 0
Static    = 1
OSPF      = 110
RIP       = 120
```

كلما كان الرقم أقل، كان المصدر أكثر تفضيلًا.

---

# Metric

لو نفس البروتوكول لديه أكثر من طريق لنفس الوجهة، يستخدم Metric لاختيار الأفضل.

مثال:

```text
RIP  → Hop Count
OSPF → Cost
EIGRP → Bandwidth + Delay
```

يعني:

```text
Administrative Distance يختار بين مصادر مختلفة
Metric يختار داخل نفس البروتوكول
```

---

# ماذا يحدث داخل الراوتر؟

عندما تصل Packet إلى الراوتر، يحدث الآتي:

```text
1. الراوتر يستقبل Frame على Interface معين
2. يزيل Layer 2 Header
3. يقرأ Destination IP داخل IP Header
4. يبحث في Routing Table
5. يختار أفضل Route
6. يقلل TTL بمقدار 1
7. يبني Layer 2 Frame جديد
8. يرسل Packet إلى Next Hop
```

---

# نقطة مهمة جدًا: الراوتر يغير MAC

عند كل راوتر، يتم تغيير Layer 2 Header.

يعني:

```text
Source MAC يتغير
Destination MAC يتغير
```

لكن غالبًا:

```text
Source IP لا يتغير
Destination IP لا يتغير
```

إلا في حالة وجود:

```text
NAT
```

---

# مثال MAC و IP أثناء الرحلة

## من اللابتوب إلى راوتر البيت

```text
Source MAC      = Laptop MAC
Destination MAC = Home Router MAC

Source IP       = 192.168.1.10
Destination IP  = 8.8.8.8
```

---

## من راوتر البيت إلى راوتر ISP

```text
Source MAC      = Home Router MAC
Destination MAC = ISP Router MAC

Source IP       = Public IP بعد NAT
Destination IP  = 8.8.8.8
```

لاحظ:

```text
MAC تغير
Source IP تغير بسبب NAT
Destination IP لم يتغير
```

---

# TTL

TTL اختصار لـ:

```text
Time To Live
```

وهو رقم داخل IP Header يقل بمقدار 1 عند كل راوتر.

مثال:

```text
TTL = 64
```

بعد أول راوتر:

```text
TTL = 63
```

بعد ثاني راوتر:

```text
TTL = 62
```

لو وصل TTL إلى صفر، يتم إسقاط الـ Packet.

والراوتر قد يرسل رسالة ICMP:

```text
Time Exceeded
```

وهذه الفكرة يستخدمها Traceroute.

---

# لماذا يوجد TTL؟

TTL يمنع الـ Packets من الدوران إلى الأبد داخل الإنترنت.

لو حصل Routing Loop، قد تظل Packet تدور بين الراوترات.

TTL يمنع ذلك.

```text
TTL reaches 0 → Drop Packet
```

---

# Router و ARP

الراوتر يستخدم ARP عندما يحتاج معرفة MAC الخاص بالـ Next Hop داخل نفس الشبكة.

مثال:

```text
Router wants to send Packet to Next Hop 192.168.2.2
```

لو لا يعرف MAC الخاص به، يعمل ARP:

```text
Who has 192.168.2.2?
```

ثم يبني Frame جديد.

---

# Router و NAT

راوتر البيت غالبًا يستخدم NAT.

الأجهزة داخل البيت تستخدم Private IP مثل:

```text
192.168.1.10
192.168.1.11
192.168.1.12
```

لكن الإنترنت لا يرى هذه العناوين.

الراوتر يحولها إلى Public IP.

مثال:

```text
192.168.1.10 → 41.33.20.5
```

لو أكثر من جهاز يستخدم نفس Public IP، يستخدم الراوتر PAT.

مثال:

```text
192.168.1.10:50000 → 41.33.20.5:62001
192.168.1.11:50000 → 41.33.20.5:62002
```

---

# Router و Firewall

كثير من الراوترات، خصوصًا في البيت أو الشركات، تحتوي أيضًا على Firewall.

لكن من المهم التفريق:

```text
Routing = اختيار الطريق
Firewall = السماح أو المنع
NAT = ترجمة العناوين
```

قد يكون الجهاز الواحد يقوم بالثلاثة معًا، لكن كل وظيفة مختلفة.

---

# Router و DHCP

راوتر البيت غالبًا يعمل كـ DHCP Server.

يعني يعطي الأجهزة:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
```

مثال:

```text
IP Address      = 192.168.1.50
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.1.1
DNS Server      = 8.8.8.8
```

---

# Router و DNS

الراوتر قد يكون DNS Forwarder.

يعني جهازك يسأل الراوتر:

```text
ما هو IP الخاص بـ google.com؟
```

والراوتر يرسل السؤال إلى DNS Server خارجي مثل:

```text
8.8.8.8
1.1.1.1
DNS الخاص بمزود الخدمة
```

---

# Inter-VLAN Routing

عندما تكون عندك أكثر من VLAN، كل VLAN تعتبر شبكة منفصلة.

مثال:

```text
VLAN 10 = 192.168.10.0/24
VLAN 20 = 192.168.20.0/24
```

لو جهاز في VLAN 10 يريد الوصول إلى جهاز في VLAN 20، يحتاج جهاز Layer 3 يعمل Routing.

يمكن أن يكون:

```text
Router
Layer 3 Switch
Firewall
```

---

# Router-on-a-Stick

Router-on-a-Stick يعني استخدام راوتر واحد ومنفذ واحد Trunk للتوجيه بين VLANs.

مثال:

```text
Router G0/0.10 → VLAN 10 → 192.168.10.1
Router G0/0.20 → VLAN 20 → 192.168.20.1
```

الراوتر يستقبل Frames Tagged بـ 802.1Q، ثم يوجه بينها.

---

# Layer 3 Switch

Layer 3 Switch يجمع بين وظيفة السويتش والراوتر.

يعني يستطيع:

```text
Switching using MAC
Routing using IP
```

يستخدم غالبًا في الشبكات المتوسطة والكبيرة لعمل Inter-VLAN Routing بسرعة أعلى.

---

# Route Summarization

Route Summarization يعني تلخيص أكثر من Route في Route واحد.

مثال:

بدل كتابة:

```text
192.168.0.0/24
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24
```

يمكن تلخيصها في:

```text
192.168.0.0/22
```

الفائدة:

```text
تقليل حجم Routing Table
تحسين الأداء
تبسيط الإدارة
```

---

# Routing Loop

Routing Loop يحدث عندما تدور Packet بين راوترات بدون أن تصل للوجهة.

مثال:

```text
Router A يرسل إلى Router B
Router B يرسل مرة أخرى إلى Router A
```

الحماية تكون بطرق مثل:

```text
TTL
Routing Protocol Loop Prevention
Split Horizon
Route Poisoning
STP في Layer 2 وليس Layer 3
```

---

# مثال عملي كامل: فتح google.com

جهازك:

```text
IP Address      = 192.168.1.10
Gateway         = 192.168.1.1
DNS Server      = 8.8.8.8
```

أنت كتبت:

```text
https://google.com
```

الخطوات:

```text
1. DNS يحول google.com إلى IP مثل 142.250.80.46
2. الجهاز يرى أن هذا IP خارج الشبكة المحلية
3. الجهاز يعمل ARP لمعرفة MAC الخاص بالـ Gateway
4. الجهاز يرسل Frame إلى الراوتر
5. الراوتر يزيل Layer 2 Header
6. الراوتر يقرأ Destination IP
7. الراوتر يبحث في Routing Table
8. الراوتر يستخدم Default Route إلى ISP
9. الراوتر ينفذ NAT/PAT
10. الراوتر يبني Frame جديد للـ Next Hop
11. Packet تكمل رحلتها عبر الإنترنت
```

---

# أوامر مفيدة

## Windows

عرض Default Gateway:

```cmd
ipconfig
```

عرض Routing Table:

```cmd
route print
```

تتبع الطريق:

```cmd
tracert 8.8.8.8
```

---

## Linux

عرض IP والـ Interfaces:

```bash
ip addr
```

عرض Routing Table:

```bash
ip route
```

تتبع الطريق:

```bash
traceroute 8.8.8.8
```

أو:

```bash
tracepath 8.8.8.8
```

---

## Cisco

عرض Routing Table:

```cisco
show ip route
```

عرض Interfaces:

```cisco
show ip interface brief
```

إضافة Static Route:

```cisco
ip route 10.10.10.0 255.255.255.0 192.168.1.2
```

إضافة Default Route:

```cisco
ip route 0.0.0.0 0.0.0.0 41.33.20.1
```

---

# مشاكل شائعة

## 1. لا يوجد Default Gateway

الأعراض:

```text
الجهاز يتواصل داخل الشبكة المحلية
لكن لا يدخل الإنترنت
```

السبب:

```text
Default Gateway غير مضبوط
```

---

## 2. Route غير موجود

الأعراض:

```text
شبكة معينة لا يمكن الوصول إليها
```

السبب:

```text
لا يوجد Route لهذه الشبكة
```

---

## 3. NAT غير مفعل

الأعراض:

```text
الأجهزة الداخلية لا تصل للإنترنت رغم وجود Route
```

السبب:

```text
Private IP لا يخرج مباشرة للإنترنت
```

---

## 4. Gateway خطأ

الأعراض:

```text
الجهاز يرسل البيانات لراوتر غير صحيح
```

السبب:

```text
DHCP أعطى Gateway خطأ
أو الإعداد اليدوي غير صحيح
```

---

## 5. Routing Loop

الأعراض:

```text
Traceroute يظهر تكرار نفس الراوترات
TTL ينتهي
الاتصال لا يصل للوجهة
```

---

# ملخص سريع

```text
Router = يعمل في Layer 3
Router uses IP Address
Router routes Packets between networks
Routing Table = جدول الطرق
Default Route = 0.0.0.0/0
Next Hop = الراوتر التالي
TTL decreases at every router
MAC changes at every hop
IP غالبًا يظل ثابتًا إلا مع NAT
NAT يغير Private IP إلى Public IP
```

---

# أهم القواعد

```text
Switch connects devices inside same network
Router connects different networks
```

```text
Switch uses MAC
Router uses IP
```

```text
IP = final destination
MAC = next hop
```

```text
Router removes old Layer 2 header
then creates a new Layer 2 header
```

```text
If no route exists:
Packet is dropped
```

```text
Default Route = الطريق لأي وجهة غير معروفة
```

---

## Next Topic

اقرأ بعد ذلك:

[Routing Protocols OSPF RIP BGP](./Routing-Protocols-OSPF-RIP-BGP.md)
