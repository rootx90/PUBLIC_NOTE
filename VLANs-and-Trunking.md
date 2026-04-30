# VLANs and Trunking

## تقسيم الشبكة الكبيرة إلى شبكات أصغر

في الشبكات العادية، لو كل الأجهزة متصلة بنفس السويتش وبدون تقسيم، غالبًا ستكون كلها داخل نفس:

```text
Broadcast Domain
```

يعني لو جهاز أرسل Broadcast، الرسالة قد تصل لكل الأجهزة.

مثال:

```text
ARP Request
DHCP Discover
```

هذا قد يكون مقبولًا في شبكة صغيرة، لكن في شبكة كبيرة يسبب مشاكل.

---

# المشكلة بدون VLAN

تخيل شركة فيها:

```text
Employees
Guests
Servers
Cameras
IP Phones
Printers
```

لو كلهم في نفس الشبكة:

```text
كل Broadcast يصل للجميع
أي جهاز قد يحاول الوصول لأي جهاز آخر
صعوبة في التنظيم
صعوبة في تطبيق السياسات الأمنية
```

مثال:

```text
جهاز ضيف Guest قد يكون في نفس الشبكة مع السيرفرات
```

وهذا غير آمن.

الحل هو:

```text
VLAN
```

---

# ما هي VLAN؟

VLAN اختصار لـ:

```text
Virtual Local Area Network
```

يعني شبكة محلية افتراضية.

الفكرة:

```text
نقسم السويتش الواحد إلى أكثر من شبكة منطقية
```

مثال:

```text
VLAN 10 = Employees
VLAN 20 = Guests
VLAN 30 = Servers
VLAN 40 = Cameras
```

رغم أن الأجهزة قد تكون متصلة بنفس السويتش، لكنها منطقيًا منفصلة.

---

# تشبيه بسيط

تخيل مبنى واحد فيه عدة أقسام:

```text
قسم الموظفين
قسم الزوار
قسم الإدارة
قسم السيرفرات
```

كلهم في نفس المبنى، لكن كل قسم له صلاحيات ومساحة خاصة.

السويتش هو المبنى.

والـ VLANs هي الأقسام الداخلية.

---

# لماذا نستخدم VLAN؟

نستخدم VLAN لأسباب مهمة:

```text
Security
Performance
Organization
Broadcast Control
Flexibility
```

---

## 1. Security

يمكن فصل الضيوف عن الشبكة الداخلية.

مثال:

```text
Guest Wi-Fi → VLAN 20
Company Devices → VLAN 10
Servers → VLAN 30
```

الضيف لا يجب أن يصل مباشرة إلى السيرفرات.

---

## 2. Performance

كل VLAN لها Broadcast Domain مستقل.

يعني Broadcast في VLAN 10 لا يصل إلى VLAN 20.

هذا يقلل الترافيك غير الضروري.

---

## 3. Organization

بدل تقسيم الشبكة حسب المكان فقط، يمكن تقسيمها حسب الوظيفة.

مثال:

```text
HR Department       → VLAN 10
IT Department       → VLAN 20
Accounting          → VLAN 30
Servers             → VLAN 40
```

حتى لو الأجهزة في أماكن مختلفة، يمكن وضعها في نفس VLAN.

---

## 4. Flexibility

لو موظف انتقل من مكتب إلى مكتب، يمكن فقط تغيير VLAN الخاصة بالمنفذ.

لا تحتاج تغيير الكابلات أو تصميم الشبكة بالكامل.

---

# VLAN ID

كل VLAN لها رقم يسمى:

```text
VLAN ID
```

أمثلة:

```text
VLAN 10
VLAN 20
VLAN 30
```

المدى الطبيعي للـ VLAN IDs في 802.1Q هو:

```text
1 إلى 4094
```

لكن في الاستخدام العملي، غالبًا تختار أرقام منظمة مثل:

```text
10
20
30
100
200
```

---

# Default VLAN

في كثير من السويتشات، الـ VLAN الافتراضية تكون:

```text
VLAN 1
```

كل المنافذ تكون داخل VLAN 1 بشكل افتراضي.

لكن في الشبكات المنظمة، لا يُفضل الاعتماد على VLAN 1 لكل شيء.

الأفضل إنشاء VLANs واضحة مثل:

```text
VLAN 10 = Users
VLAN 20 = Guests
VLAN 30 = Servers
```

---

# Broadcast Domain مع VLAN

كل VLAN تعتبر Broadcast Domain منفصل.

مثال:

```text
VLAN 10 = Broadcast Domain مستقل
VLAN 20 = Broadcast Domain مستقل
VLAN 30 = Broadcast Domain مستقل
```

لو جهاز في VLAN 10 أرسل Broadcast:

```text
FF:FF:FF:FF:FF:FF
```

سيصل فقط لأجهزة VLAN 10.

لن يصل إلى VLAN 20 أو VLAN 30.

---

# Access Port

Access Port هو منفذ عادي يوصل جهازًا نهائيًا.

أمثلة أجهزة:

```text
Laptop
Desktop PC
Printer
Camera
IP Phone
```

غالبًا Access Port يكون تابعًا لـ VLAN واحدة فقط.

مثال:

```text
Port 1 → VLAN 10
Port 2 → VLAN 10
Port 3 → VLAN 20
Port 4 → VLAN 30
```

الجهاز المتصل بالـ Access Port غالبًا لا يعرف شيئًا عن VLAN Tag.

السويتش هو الذي يعرف أن هذا المنفذ تابع لـ VLAN معينة.

---

# مثال Access Port

لو عندك جهاز موظف متصل على Port 1:

```text
Port 1 = Access VLAN 10
```

أي Frame يخرج من الجهاز إلى السويتش، يعتبره السويتش تابعًا لـ VLAN 10.

وأي Frame يخرج من السويتش إلى الجهاز، يخرج غالبًا بدون VLAN Tag.

---

# Trunk Port

Trunk Port هو منفذ يحمل أكثر من VLAN في نفس الوقت.

يستخدم غالبًا بين أجهزة الشبكة.

أمثلة:

```text
Switch ↔ Switch
Switch ↔ Router
Switch ↔ Firewall
Switch ↔ Access Point
Switch ↔ Layer 3 Switch
```

لو عندك سويتشان، وكل سويتش عليه VLAN 10 و VLAN 20، فأنت لا تريد تمديد كابل لكل VLAN.

بدل ذلك تستخدم Trunk واحد يحمل أكثر من VLAN.

---

# لماذا نحتاج Trunk؟

تخيل عندك سويتشين.

السويتش الأول فيه:

```text
VLAN 10
VLAN 20
VLAN 30
```

والسويتش الثاني فيه نفس الـ VLANs.

لو استخدمت Access Ports فقط، ستحتاج كابل لكل VLAN.

```text
Cable for VLAN 10
Cable for VLAN 20
Cable for VLAN 30
```

هذا غير عملي.

الحل:

```text
Trunk Link
```

كابل واحد يحمل كل الـ VLANs.

---

# 802.1Q Tagging

حتى يعرف السويتش المستقبل لأي VLAN ينتمي الـ Frame، يتم إضافة Tag داخل Ethernet Frame.

هذا يسمى:

```text
802.1Q Tag
```

الـ Tag يحتوي على VLAN ID.

مثال:

```text
Frame belongs to VLAN 10
Frame belongs to VLAN 20
```

---

# شكل Ethernet Frame بدون VLAN

```text
┌───────────────┬────────────┬───────────┬──────┬─────┐
│ Destination   │ Source     │ EtherType │ Data │ FCS │
│ MAC           │ MAC        │           │      │     │
└───────────────┴────────────┴───────────┴──────┴─────┘
```

---

# شكل Ethernet Frame مع 802.1Q Tag

```text
┌───────────────┬────────────┬───────────┬───────────┬──────┬─────┐
│ Destination   │ Source     │ 802.1Q    │ EtherType │ Data │ FCS │
│ MAC           │ MAC        │ Tag       │           │      │     │
└───────────────┴────────────┴───────────┴───────────┴──────┴─────┘
```

الـ 802.1Q Tag يحدد:

```text
VLAN ID
```

---

# Tagged و Untagged

## Untagged Frame

Frame بدون VLAN Tag.

غالبًا يستخدم على:

```text
Access Port
```

الجهاز العادي يستقبل ويرسل Frames بدون Tag.

---

## Tagged Frame

Frame يحتوي على VLAN Tag.

غالبًا يستخدم على:

```text
Trunk Port
```

بين السويتشات أو بين السويتش والراوتر.

---

# Native VLAN

في Trunk Port، هناك مفهوم اسمه:

```text
Native VLAN
```

وهي VLAN التي تمر على الـ Trunk بدون Tag.

في كثير من الأجهزة، Native VLAN الافتراضية تكون:

```text
VLAN 1
```

لكن لأسباب أمنية وتنظيمية، يفضل تغيير Native VLAN إلى VLAN غير مستخدمة.

مثال:

```text
Native VLAN = 999
```

---

# Access Port vs Trunk Port

| المقارنة | Access Port | Trunk Port |
|---|---|---|
| الاستخدام | جهاز نهائي | بين أجهزة الشبكة |
| عدد VLANs | VLAN واحدة غالبًا | أكثر من VLAN |
| VLAN Tag | غالبًا بدون Tag | غالبًا Tagged |
| أمثلة | PC, Printer | Switch, Router, AP |

---

# هل VLANs تتواصل مع بعضها مباشرة؟

لا.

الأجهزة داخل VLAN واحدة يمكنها التواصل مع بعضها مباشرة على Layer 2.

لكن جهاز في VLAN 10 لا يستطيع التواصل مباشرة مع جهاز في VLAN 20.

مثال:

```text
PC1 in VLAN 10
PC2 in VLAN 20
```

حتى لو هما على نفس السويتش، فهما في شبكتين منفصلتين منطقيًا.

للتواصل بين VLANs نحتاج:

```text
Inter-VLAN Routing
```

---

# Inter-VLAN Routing

Inter-VLAN Routing يعني السماح بالتواصل بين VLANs عن طريق جهاز Layer 3.

يمكن تنفيذه باستخدام:

```text
Router
Layer 3 Switch
Firewall
```

---

# طريقة 1: Router-on-a-Stick

في هذه الطريقة، نستخدم راوتر متصل بالسويتش بكابل Trunk واحد.

الراوتر يقسم المنفذ إلى Subinterfaces.

مثال:

```text
Router Interface: G0/0
Subinterface G0/0.10 → VLAN 10
Subinterface G0/0.20 → VLAN 20
Subinterface G0/0.30 → VLAN 30
```

كل Subinterface يكون لها IP وتكون Default Gateway للـ VLAN.

مثال:

```text
VLAN 10 Gateway = 192.168.10.1
VLAN 20 Gateway = 192.168.20.1
VLAN 30 Gateway = 192.168.30.1
```

---

# رسم Router-on-a-Stick

```text
      ┌────────────┐
      │   Router   │
      │ G0/0 Trunk │
      └─────┬──────┘
            │ Trunk
      ┌─────┴──────┐
      │   Switch   │
      ├─────┬──────┤
      │     │      │
    VLAN10 VLAN20 VLAN30
```

الراوتر يستقبل Frames tagged، ويفهم لأي VLAN تنتمي.

ثم يعمل Routing بين الشبكات.

---

# طريقة 2: Layer 3 Switch

Layer 3 Switch يستطيع عمل Switching وRouting.

يتم إنشاء واجهات افتراضية تسمى:

```text
SVI = Switch Virtual Interface
```

مثال:

```text
interface vlan 10
ip address 192.168.10.1 255.255.255.0

interface vlan 20
ip address 192.168.20.1 255.255.255.0
```

ثم يتم تفعيل:

```text
ip routing
```

بعدها يستطيع السويتش عمل Routing بين VLANs.

---

# Router-on-a-Stick vs Layer 3 Switch

| المقارنة | Router-on-a-Stick | Layer 3 Switch |
|---|---|---|
| الأداء | أقل غالبًا | أعلى غالبًا |
| الاستخدام | شبكات صغيرة أو تعليم | شبكات متوسطة وكبيرة |
| الكابل | Trunk واحد للراوتر | داخلي داخل السويتش |
| الاعتماد | Router خارجي | السويتش نفسه يعمل Routing |

---

# Default Gateway لكل VLAN

كل VLAN تحتاج Default Gateway خاص بها.

مثال:

```text
VLAN 10 Network = 192.168.10.0/24
Gateway         = 192.168.10.1

VLAN 20 Network = 192.168.20.0/24
Gateway         = 192.168.20.1

VLAN 30 Network = 192.168.30.0/24
Gateway         = 192.168.30.1
```

الجهاز داخل VLAN 10 يأخذ Gateway:

```text
192.168.10.1
```

والجهاز داخل VLAN 20 يأخذ Gateway:

```text
192.168.20.1
```

---

# VLAN و DHCP

لو كل VLAN لها شبكة مختلفة، فكل VLAN تحتاج IP Pool خاص بها.

مثال:

```text
VLAN 10 → 192.168.10.0/24
VLAN 20 → 192.168.20.0/24
VLAN 30 → 192.168.30.0/24
```

إذن DHCP Server يحتاج Pools مثل:

```text
Pool VLAN10
Pool VLAN20
Pool VLAN30
```

---

# DHCP داخل نفس VLAN

لو DHCP Server داخل نفس VLAN، الجهاز يرسل DHCP Discover كـ Broadcast، والسيرفر يرد عليه.

---

# DHCP في VLAN مختلفة

لو DHCP Server موجود في VLAN مختلفة، Broadcast لن يعبر الراوتر تلقائيًا.

الحل:

```text
DHCP Relay
```

أو في Cisco:

```text
ip helper-address
```

مثال:

```text
interface vlan 10
ip helper-address 192.168.99.10
```

هذا يجعل Layer 3 device يرسل طلبات DHCP إلى السيرفر.

---

# VLAN و ARP

ARP Broadcast لا يخرج خارج VLAN.

مثال:

```text
PC in VLAN 10 يسأل:
Who has 192.168.10.1?
```

هذا ARP Request يصل فقط إلى VLAN 10.

لن يصل إلى VLAN 20.

إذا أراد جهاز في VLAN 10 الوصول إلى جهاز في VLAN 20، سيعمل ARP للـ Default Gateway الخاص بـ VLAN 10، وليس للجهاز الموجود في VLAN 20.

---

# مثال عملي

لدينا:

```text
PC1:
IP   = 192.168.10.10
VLAN = 10
GW   = 192.168.10.1

PC2:
IP   = 192.168.20.10
VLAN = 20
GW   = 192.168.20.1
```

PC1 يريد إرسال بيانات إلى PC2.

الخطوات:

```text
1. PC1 يرى أن PC2 خارج شبكته
2. PC1 يرسل البيانات إلى Default Gateway
3. PC1 يعمل ARP لمعرفة MAC الخاص بـ 192.168.10.1
4. Layer 3 Switch أو Router يستقبل Packet
5. يعمل Routing إلى VLAN 20
6. يعمل ARP لمعرفة MAC الخاص بـ PC2
7. يرسل Frame إلى PC2 داخل VLAN 20
```

---

# أوامر Cisco مفيدة

## إنشاء VLAN

```cisco
vlan 10
name Employees

vlan 20
name Guests
```

---

## جعل منفذ Access

```cisco
interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
```

---

## جعل منفذ Trunk

```cisco
interface fastEthernet 0/24
switchport mode trunk
switchport trunk allowed vlan 10,20,30
```

---

## عرض VLANs

```cisco
show vlan brief
```

---

## عرض Trunk Ports

```cisco
show interfaces trunk
```

---

## عرض MAC Table

```cisco
show mac address-table
```

---

# أخطاء شائعة

## 1. جهاز في VLAN غلط

لو جهاز موظف دخل VLAN الضيوف، قد لا يصل للموارد الداخلية.

افحص:

```cisco
show vlan brief
```

---

## 2. Trunk لا يحمل VLAN المطلوبة

لو VLAN 20 غير مسموح بها على الـ Trunk، فلن تنتقل بين السويتشات.

افحص:

```cisco
show interfaces trunk
```

---

## 3. Native VLAN mismatch

لو Native VLAN مختلفة بين طرفي Trunk، قد تحدث مشاكل وتحذيرات.

مثال:

```text
Switch 1 Native VLAN = 1
Switch 2 Native VLAN = 999
```

يجب توحيد الإعدادات.

---

## 4. لا يوجد Inter-VLAN Routing

لو VLAN 10 لا تستطيع الوصول إلى VLAN 20، ربما لا يوجد Router أو Layer 3 Switch يعمل Routing بينهما.

---

## 5. DHCP لا يعمل في VLAN معينة

قد يكون السبب:

```text
لا يوجد DHCP Pool لهذه VLAN
ip helper-address غير مضبوط
Trunk لا يحمل هذه VLAN
الجهاز في VLAN خطأ
```

---

# VLAN في Wi-Fi

في الشبكات اللاسلكية، يمكن ربط كل SSID بـ VLAN مختلفة.

مثال:

```text
SSID: Company-WiFi → VLAN 10
SSID: Guest-WiFi   → VLAN 20
SSID: Cameras      → VLAN 30
```

الـ Access Point يتصل بالسويتش غالبًا عبر Trunk حتى يحمل أكثر من VLAN.

---

# VLAN في الأمن

VLAN ليست Firewall وحدها.

هي تفصل Broadcast Domains وتساعد في التنظيم، لكن لو تريد التحكم في من يستطيع الوصول لمن، تحتاج:

```text
ACL
Firewall Rules
Inter-VLAN Policies
```

مثال:

```text
Guests VLAN لا تصل إلى Servers VLAN
Employees VLAN تصل إلى Printer VLAN فقط
IT VLAN تصل إلى كل الشبكات للإدارة
```

---

# ملخص سريع

```text
VLAN = تقسيم منطقي للشبكة
كل VLAN = Broadcast Domain مستقل
Access Port = VLAN واحدة لجهاز عادي
Trunk Port = يحمل أكثر من VLAN
802.1Q = يضيف VLAN Tag داخل Frame
Native VLAN = VLAN تمر بدون Tag على Trunk
Inter-VLAN Routing = تواصل بين VLANs عبر Layer 3
DHCP Relay = نقل DHCP Requests بين VLANs
```

---

# أهم القواعد

```text
Broadcast لا يعبر بين VLANs
```

```text
ARP Request يبقى داخل نفس VLAN
```

```text
Access Port غالبًا Untagged
```

```text
Trunk Port غالبًا Tagged
```

```text
VLANs لا تتواصل مع بعضها إلا عن طريق Routing
```

```text
VLAN تساعد في التنظيم والأمان، لكنها ليست بديلًا عن Firewall
```

---

## Next Topic

اقرأ بعد ذلك:

[Router Layer 3](./Router-Layer-3.md)
