# Routing Protocols - OSPF, RIP, BGP

## كيف تتعلم الراوترات الطرق؟

في الدرس السابق عرفنا أن الراوتر يستخدم:

```text
Routing Table
```

حتى يعرف أين يرسل الـ Packet.

لكن السؤال المهم:

```text
كيف امتلأ هذا الجدول بالطرق؟
```

الإجابة تكون غالبًا بإحدى طريقتين:

```text
Static Routing
Dynamic Routing
```

---

# ما هو Routing Protocol؟

Routing Protocol هو بروتوكول يجعل الراوترات تتبادل معلومات الشبكات مع بعضها.

بدل ما تضيف كل Route يدويًا، الراوترات تخبر بعضها:

```text
أنا أعرف هذه الشبكة
هذه الشبكة خلفي
هذا الطريق أفضل
هذا الطريق لم يعد متاحًا
```

أمثلة مشهورة:

```text
RIP
OSPF
EIGRP
BGP
```

---

# Static Routing

Static Routing يعني أنك تكتب الطريق يدويًا داخل الراوتر.

مثال Cisco:

```cisco
ip route 192.168.20.0 255.255.255.0 192.168.10.2
```

المعنى:

```text
للوصول إلى شبكة 192.168.20.0/24
أرسل إلى Next Hop 192.168.10.2
```

---

## مميزات Static Routing

```text
سهل في الشبكات الصغيرة
لا يستهلك موارد كثيرة
واضح ومباشر
أكثر تحكمًا
```

---

## عيوب Static Routing

```text
صعب في الشبكات الكبيرة
لو الطريق تعطل لا يتغير تلقائيًا
يحتاج تعديل يدوي
قابل للأخطاء البشرية
```

---

# Dynamic Routing

Dynamic Routing يعني أن الراوترات تتعلم الطرق تلقائيًا باستخدام Routing Protocol.

مثال:

```text
Router A يعرف شبكة 192.168.1.0/24
Router B يعرف شبكة 10.0.0.0/24
```

باستخدام OSPF أو RIP، كل راوتر يخبر الآخر بالشبكات التي يعرفها.

بعد ذلك، كل راوتر يبني Routing Table تلقائيًا.

---

## مميزات Dynamic Routing

```text
مناسب للشبكات الكبيرة
يتعامل مع الأعطال تلقائيًا
يجد مسارات بديلة
يقلل الإدارة اليدوية
```

---

## عيوب Dynamic Routing

```text
أكثر تعقيدًا
يستهلك CPU وRAM وBandwidth
يحتاج تخطيط جيد
قد يسبب مشاكل إذا تم ضبطه خطأ
```

---

# Static vs Dynamic Routing

| المقارنة | Static Routing | Dynamic Routing |
|---|---|---|
| الإعداد | يدوي | تلقائي |
| مناسب لـ | شبكات صغيرة | شبكات متوسطة وكبيرة |
| التعامل مع الأعطال | يدوي | تلقائي |
| التعقيد | بسيط | أعلى |
| استهلاك الموارد | قليل | أعلى |
| أمثلة | ip route | OSPF, RIP, BGP |

---

# أنواع Routing Protocols

يمكن تقسيم Routing Protocols إلى نوعين رئيسيين:

```text
IGP
EGP
```

---

## IGP

IGP اختصار لـ:

```text
Interior Gateway Protocol
```

يستخدم داخل نفس المؤسسة أو نفس النظام الإداري.

يعني داخل شبكة شركة واحدة.

أمثلة:

```text
RIP
OSPF
EIGRP
IS-IS
```

---

## EGP

EGP اختصار لـ:

```text
Exterior Gateway Protocol
```

يستخدم بين شبكات كبيرة مختلفة أو بين مزودي خدمة الإنترنت.

أشهر مثال:

```text
BGP
```

BGP هو البروتوكول الأساسي الذي يساعد الإنترنت كله على تبادل المسارات بين الشبكات الكبيرة.

---

# Administrative Distance

عندما يعرف الراوتر نفس الشبكة من أكثر من مصدر، يحتاج يقرر أي مصدر يثق به أكثر.

هنا يستخدم قيمة اسمها:

```text
Administrative Distance
```

كلما كان الرقم أقل، كان المصدر أكثر ثقة.

---

## أمثلة Administrative Distance

```text
Connected Route = 0
Static Route    = 1
eBGP            = 20
EIGRP           = 90
OSPF            = 110
RIP             = 120
Unknown         = 255
```

مثال:

لو الراوتر عرف نفس الشبكة من Static Route ومن OSPF:

```text
Static AD = 1
OSPF AD   = 110
```

سيختار Static Route لأنه أقل.

---

# Metric

بعد اختيار البروتوكول أو المصدر، قد يكون هناك أكثر من طريق داخل نفس البروتوكول.

هنا يستخدم الراوتر:

```text
Metric
```

Metric هو رقم أو طريقة حساب تحدد أفضل طريق.

كل بروتوكول له Metric مختلف.

---

## أمثلة Metric

| Protocol | Metric |
|---|---|
| RIP | Hop Count |
| OSPF | Cost |
| EIGRP | Bandwidth + Delay |
| BGP | Attributes / Policies |

---

# الفرق بين AD و Metric

```text
Administrative Distance:
يختار بين مصادر مختلفة للـ Route
```

```text
Metric:
يختار أفضل طريق داخل نفس البروتوكول
```

مثال:

```text
نفس الشبكة موجودة من Static و OSPF
→ استخدم AD
```

```text
نفس الشبكة موجودة من طريقين داخل OSPF
→ استخدم Metric
```

---

# RIP

RIP اختصار لـ:

```text
Routing Information Protocol
```

وهو من أقدم بروتوكولات التوجيه.

RIP يعتبر:

```text
Distance Vector Protocol
```

يعني كل راوتر يرسل لجيرانه معلومات عن الشبكات التي يعرفها.

---

## Metric في RIP

RIP يستخدم:

```text
Hop Count
```

يعني عدد الراوترات التي يجب المرور بها للوصول للوجهة.

مثال:

```text
Network A تحتاج 1 Hop
Network B تحتاج 3 Hops
```

RIP يفضل الأقل.

---

## حد RIP

أكبر عدد Hops في RIP هو:

```text
15
```

لو الطريق يحتاج 16 Hop، يعتبر غير قابل للوصول.

```text
16 = Unreachable
```

لذلك RIP غير مناسب للشبكات الكبيرة.

---

## مميزات RIP

```text
سهل الفهم
سهل الإعداد
مناسب للتعليم والشبكات الصغيرة جدًا
```

---

## عيوب RIP

```text
بطيء في التحديث
يعتمد فقط على عدد الـ Hops
لا يهتم بسرعة الرابط
حده الأقصى 15 Hop
غير مناسب للشبكات الكبيرة
```

---

## مثال مشكلة في RIP

قد يكون عندك طريقان:

```text
Path A = 2 Hops بسرعة 1 Mbps
Path B = 3 Hops بسرعة 1 Gbps
```

RIP قد يختار Path A لأنه أقل Hops، رغم أنه أبطأ جدًا.

لذلك الاعتماد على Hop Count فقط ليس مثاليًا.

---

# OSPF

OSPF اختصار لـ:

```text
Open Shortest Path First
```

وهو بروتوكول شائع جدًا داخل الشركات.

OSPF يعتبر:

```text
Link-State Protocol
```

يعني كل راوتر يحاول بناء خريطة للشبكة، ثم يحسب أفضل طريق.

---

# كيف يعمل OSPF؟

الفكرة بشكل مبسط:

```text
1. الراوترات تكتشف الجيران
2. تتبادل معلومات عن الروابط والشبكات
3. كل راوتر يبني خريطة للشبكة
4. يستخدم خوارزمية SPF لاختيار أفضل طريق
```

الخوارزمية المستخدمة تسمى:

```text
Dijkstra Algorithm
```

---

# Metric في OSPF

OSPF يستخدم Metric اسمه:

```text
Cost
```

والـ Cost يعتمد غالبًا على Bandwidth.

يعني الرابط الأسرع يكون Cost أقل.

مثال:

```text
Fast Link  → Low Cost
Slow Link  → High Cost
```

OSPF يختار الطريق ذو الـ Cost الأقل.

---

# OSPF Areas

OSPF يستخدم مفهوم:

```text
Areas
```

لتقسيم الشبكة الكبيرة إلى أجزاء.

أهم Area هي:

```text
Area 0
```

وتسمى:

```text
Backbone Area
```

في تصميم OSPF، المناطق الأخرى غالبًا يجب أن تتصل بـ Area 0.

---

# لماذا يستخدم OSPF Areas؟

لأن الشبكات الكبيرة جدًا قد تكون معقدة.

تقسيمها إلى Areas يساعد على:

```text
تقليل حجم الجداول
تقليل التحديثات
تحسين الأداء
تنظيم الشبكة
```

---

# OSPF Neighbor

راوترات OSPF تحتاج تكوين علاقة جيرة تسمى:

```text
Neighbor Adjacency
```

قبل أن تتبادل معلومات التوجيه.

تستخدم رسائل:

```text
Hello Packets
```

لاكتشاف الجيران والتأكد أنهم ما زالوا موجودين.

---

# معلومات يجب أن تتطابق في OSPF

حتى يصبح راوتران جيران في OSPF، يجب أن تتطابق أشياء مثل:

```text
Area ID
Subnet Mask
Hello Timer
Dead Timer
Authentication
Network Type
```

لو لم تتطابق، قد لا تتكون الجيرة.

---

# مميزات OSPF

```text
سريع في Convergence
مناسب للشركات
يدعم الشبكات الكبيرة
يختار الطرق حسب Cost
يدعم تقسيم الشبكة إلى Areas
لا يعتمد فقط على Hop Count
```

---

# عيوب OSPF

```text
أكثر تعقيدًا من RIP
يحتاج فهم جيد للتصميم
قد يستهلك موارد أكثر
إعداد Areas يحتاج تخطيط
```

---

# BGP

BGP اختصار لـ:

```text
Border Gateway Protocol
```

وهو البروتوكول الأساسي المستخدم بين الشبكات الكبيرة على الإنترنت.

BGP لا يستخدم عادة داخل شبكة بيت أو شركة صغيرة.

يستخدم بين:

```text
Internet Service Providers
Large Companies
Data Centers
Cloud Providers
Autonomous Systems
```

---

# ما هو Autonomous System؟

Autonomous System أو AS هو شبكة كبيرة تحت إدارة واحدة.

كل AS له رقم يسمى:

```text
ASN = Autonomous System Number
```

مثال:

```text
ISP A = AS100
ISP B = AS200
Cloud Provider = AS300
```

BGP يستخدم لتبادل المسارات بين هذه الأنظمة.

---

# BGP Path Vector

BGP يعتبر:

```text
Path Vector Protocol
```

يعني لا يقول فقط:

```text
أعرف هذه الشبكة
```

بل يقول أيضًا:

```text
هذا هو المسار عبر AS numbers للوصول إليها
```

مثال:

```text
To reach 8.8.8.0/24:
AS Path = 100 200 300
```

---

# BGP يستخدم TCP

BGP يستخدم:

```text
TCP Port 179
```

وهذا مختلف عن بروتوكولات كثيرة في التوجيه.

وجود TCP يساعد على اتصال موثوق بين BGP Peers.

---

# eBGP و iBGP

هناك نوعان مهمان:

```text
eBGP
iBGP
```

---

## eBGP

eBGP يعني:

```text
External BGP
```

ويستخدم بين AS مختلفين.

مثال:

```text
AS100 ↔ AS200
```

---

## iBGP

iBGP يعني:

```text
Internal BGP
```

ويستخدم داخل نفس AS.

مثال:

```text
Router A داخل AS100
Router B داخل AS100
```

---

# BGP لا يبحث دائمًا عن الأسرع

BGP ليس هدفه فقط اختيار أسرع طريق.

BGP يعتمد جدًا على:

```text
Policies
Agreements
Business Rules
Traffic Engineering
```

يعني أحيانًا يختار طريقًا معينًا لأنه مفضل تجاريًا أو إداريًا، وليس لأنه الأقصر.

---

# BGP Attributes

BGP يستخدم Attributes كثيرة لاختيار الطريق.

أمثلة مهمة:

```text
AS_PATH
NEXT_HOP
LOCAL_PREF
MED
Origin
Community
```

---

## AS_PATH

AS_PATH يوضح قائمة الـ AS التي سيمر بها الترافيك.

مثال:

```text
AS_PATH = 100 200 300
```

غالبًا BGP يفضل المسار الأقصر في AS_PATH، لكن هذا ليس العامل الوحيد.

---

## LOCAL_PREF

LOCAL_PREF يستخدم داخل نفس AS لتحديد الطريق المفضل للخروج.

كلما كان الرقم أعلى، كان الطريق أفضل.

مثال:

```text
LOCAL_PREF 200 أفضل من LOCAL_PREF 100
```

---

## MED

MED اختصار لـ:

```text
Multi-Exit Discriminator
```

يستخدم للتأثير على كيفية دخول الترافيك إلى شبكة معينة من AS آخر.

كلما كان MED أقل، كان مفضلًا غالبًا.

---

# مقارنة RIP و OSPF و BGP

| المقارنة | RIP | OSPF | BGP |
|---|---|---|---|
| النوع | Distance Vector | Link State | Path Vector |
| الاستخدام | شبكات صغيرة | داخل الشركات | بين الشبكات الكبيرة والإنترنت |
| Metric | Hop Count | Cost | Attributes |
| السرعة في التكيف | بطيء | سريع | أبطأ لكنه مستقر |
| الحد الأقصى | 15 Hop | كبير | الإنترنت كله |
| التعقيد | بسيط | متوسط/عالي | عالي |

---

# Convergence

Convergence تعني:

```text
وصول كل الراوترات إلى رؤية صحيحة ومحدثة للشبكة
```

مثال:

لو رابط بين راوترين وقع، يجب أن تعرف باقي الراوترات وتختار طرقًا بديلة.

---

## Convergence في البروتوكولات

```text
RIP  = بطيء نسبيًا
OSPF = سريع
BGP  = أبطأ من OSPF غالبًا، لكنه مصمم للاستقرار على مستوى الإنترنت
```

---

# Routing Updates

## RIP

يرسل تحديثات دورية كل فترة.

غالبًا:

```text
كل 30 ثانية تقريبًا
```

---

## OSPF

لا يرسل جدول التوجيه كاملًا كل فترة مثل RIP.

يرسل تحديثات عند حدوث تغييرات في الشبكة.

---

## BGP

يرسل Updates عند تغير المسارات أو السياسات.

ولا يرسل كل الإنترنت كاملًا كل مرة بشكل عشوائي.

---

# Load Balancing

بعض البروتوكولات تسمح باستخدام أكثر من طريق متساوٍ.

هذا يسمى:

```text
ECMP = Equal-Cost Multi-Path
```

مثال:

```text
Path A Cost = 10
Path B Cost = 10
```

يمكن للراوتر استخدام الطريقين معًا.

OSPF يدعم ECMP.

---

# Route Summarization

Route Summarization يعني تلخيص عدة شبكات في Route واحد.

مثال:

```text
192.168.0.0/24
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24
```

يمكن تلخيصها إلى:

```text
192.168.0.0/22
```

الفائدة:

```text
تقليل حجم Routing Table
تقليل التحديثات
تحسين التنظيم
```

---

# مشكلة Routing Loop

Routing Loop تحدث عندما تدور Packet بين راوترات بدون أن تصل للوجهة.

مثال:

```text
Router A يرسل إلى Router B
Router B يرجعها إلى Router A
```

البروتوكولات تستخدم آليات لتقليل هذه المشكلة.

أمثلة:

```text
TTL
Split Horizon
Route Poisoning
Link-State Database
AS_PATH في BGP
```

---

# Split Horizon

Split Horizon فكرة تستخدم في بروتوكولات Distance Vector مثل RIP.

معناها:

```text
لا تعلن Route من نفس الواجهة التي تعلمته منها
```

مثال:

لو Router A تعلم شبكة من Router B، لا يرجع يقول لـ Router B عن نفس الشبكة من نفس الطريق.

---

# Route Poisoning

Route Poisoning يعني أن الراوتر يعلن أن شبكة معينة أصبحت غير متاحة.

في RIP، يمكن إعلانها بـ Metric:

```text
16
```

لأن 16 في RIP تعني:

```text
Unreachable
```

---

# مثال عملي: شركة صغيرة

شركة عندها 3 فروع:

```text
Branch A = 192.168.10.0/24
Branch B = 192.168.20.0/24
Branch C = 192.168.30.0/24
```

لو استخدمت Static Routes، ستحتاج تكتب Routes يدويًا في كل راوتر.

لو الفرع زاد أو تغير الطريق، ستعدل يدويًا.

لكن باستخدام OSPF:

```text
كل راوتر يعلن شبكته
والباقي يتعلم تلقائيًا
```

---

# مثال عملي: الإنترنت

مزود خدمة لديه AS100.

مزود آخر لديه AS200.

شركة كبيرة لديها AS300.

كل واحد يعلن الشبكات التي يملكها باستخدام BGP.

مثال:

```text
AS300 يعلن:
203.0.113.0/24

AS100 و AS200 يتعلمان الطريق إلى هذه الشبكة عبر BGP
```

بهذا الشكل، الإنترنت يعرف كيف يصل إلى الشبكات المختلفة.

---

# متى تستخدم كل بروتوكول؟

## استخدم Static Routing عندما:

```text
الشبكة صغيرة
الطرق قليلة وثابتة
تريد تحكمًا مباشرًا
```

---

## استخدم RIP عندما:

```text
بيئة تعليمية
شبكة صغيرة جدًا
تريد فهم أساسيات Dynamic Routing
```

لكن في الواقع العملي الحديث، RIP قليل الاستخدام.

---

## استخدم OSPF عندما:

```text
شركة متوسطة أو كبيرة
تحتاج Dynamic Routing داخلي
تحتاج Convergence سريع
تحتاج تصميم Areas
```

---

## استخدم BGP عندما:

```text
تتعامل مع أكثر من ISP
لديك AS Number
تدير شبكة كبيرة أو Data Center
تحتاج التحكم في Internet Routing
```

---

# أوامر Cisco مفيدة

## عرض Routing Table

```cisco
show ip route
```

---

## عرض OSPF Neighbors

```cisco
show ip ospf neighbor
```

---

## عرض OSPF Database

```cisco
show ip ospf database
```

---

## عرض BGP Summary

```cisco
show ip bgp summary
```

---

## عرض BGP Table

```cisco
show ip bgp
```

---

## عرض RIP Database

```cisco
show ip route rip
```

---

# أمثلة إعداد مختصرة

## Static Route

```cisco
ip route 192.168.20.0 255.255.255.0 192.168.10.2
```

---

## RIP بسيط

```cisco
router rip
version 2
network 192.168.10.0
network 192.168.20.0
no auto-summary
```

---

## OSPF بسيط

```cisco
router ospf 1
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
```

---

## BGP بسيط

```cisco
router bgp 65001
neighbor 203.0.113.2 remote-as 65002
network 198.51.100.0 mask 255.255.255.0
```

---

# مشاكل شائعة

## 1. Route لا يظهر في الجدول

افحص:

```text
هل البروتوكول مفعّل؟
هل الشبكة معلنة؟
هل الواجهة Up؟
هل هناك Filter أو ACL؟
```

---

## 2. OSPF Neighbor لا يتكون

افحص:

```text
Area ID
Subnet Mask
Hello/Dead Timers
Authentication
Network Type
Firewall
```

---

## 3. BGP Peer لا يعمل

افحص:

```text
IP Reachability
TCP Port 179
Remote AS
Neighbor IP
Authentication
Route Policies
```

---

## 4. الطريق ليس الأفضل

افحص:

```text
Administrative Distance
Metric
Prefix Length
BGP Attributes
Policy / Filtering
```

---

# ملخص سريع

```text
Routing Protocol = يجعل الراوترات تتعلم الطرق تلقائيًا
Static Routing = طرق يدوية
Dynamic Routing = طرق تلقائية
IGP = داخل مؤسسة واحدة
EGP = بين مؤسسات أو AS مختلفة
RIP = بسيط ويستخدم Hop Count
OSPF = قوي ويستخدم Cost و Areas
BGP = بروتوكول الإنترنت بين ASs
Administrative Distance = ثقة المصدر
Metric = اختيار أفضل طريق داخل البروتوكول
Convergence = اتفاق الراوترات على حالة الشبكة
```

---

# أهم القواعد

```text
Router uses Routing Table
```

```text
Longest Prefix Match أولًا
ثم Administrative Distance
ثم Metric
```

```text
RIP مناسب للتعليم والشبكات الصغيرة
OSPF مناسب للشركات
BGP مناسب للإنترنت والشبكات الكبيرة
```

```text
AD يختار بين مصادر مختلفة
Metric يختار داخل نفس المصدر
```

```text
BGP لا يختار دائمًا الطريق الأسرع
بل الطريق المناسب حسب السياسات
```

---

## Next Topic

اقرأ بعد ذلك:

[ICMP Ping and Traceroute](./ICMP-Ping-and-Traceroute.md)
