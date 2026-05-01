# Common Networking Mistakes

## أشهر أخطاء الشبكات

هذا الملف يجمع أخطاء شائعة جدًا يقع فيها المبتدئون وحتى أحيانًا الفنيون أثناء التعامل مع الشبكات.

الهدف من الملف:

```text
تفهم الخطأ
تعرف سببه
تعرف كيف تفحصه
تعرف كيف تصلحه
```

---

# 1. الخلط بين IP و MAC Address

## الخطأ

بعض الناس تخلط بين:

```text
IP Address
MAC Address
```

---

## الصحيح

```text
IP Address = يستخدم في Layer 3
MAC Address = يستخدم في Layer 2
```

الـ IP يساعد في الوصول بين الشبكات.

الـ MAC يساعد في التوصيل داخل نفس الشبكة المحلية.

---

## مثال

```text
IP Address  = 192.168.1.10
MAC Address = AA:BB:CC:DD:EE:FF
```

---

## القاعدة

```text
داخل نفس الشبكة → MAC مهم
بين الشبكات → IP مهم
```

---

# 2. نسيان Default Gateway

## الخطأ

الجهاز لديه IP صحيح، لكنه لا يستطيع الدخول إلى الإنترنت.

---

## السبب المحتمل

لا يوجد Default Gateway أو هو مكتوب خطأ.

---

## مثال

```text
IP Address      = 192.168.1.10
Subnet Mask     = 255.255.255.0
Default Gateway = فارغ
```

---

## النتيجة

الجهاز يستطيع التواصل مع الأجهزة داخل نفس الشبكة فقط، لكنه لا يعرف كيف يخرج لشبكات أخرى.

---

## الحل

ضع Default Gateway الصحيح.

مثال:

```text
Default Gateway = 192.168.1.1
```

---

# 3. Subnet Mask خطأ

## الخطأ

جهازان يبدو أنهما في نفس الشبكة، لكن الاتصال لا يعمل بسبب Subnet Mask مختلف أو خطأ.

---

## مثال

```text
PC1 = 192.168.1.10/24
PC2 = 192.168.1.20/25
```

قد يسبب هذا مشاكل في تحديد هل الجهاز الآخر Local أو Remote.

---

## الحل

تأكد أن الأجهزة داخل نفس الشبكة تستخدم Subnet Mask مناسب ومتوافق.

مثال:

```text
192.168.1.10/24
192.168.1.20/24
```

---

# 4. استخدام IP خارج الشبكة

## الخطأ

إعطاء جهاز IP لا ينتمي لنفس شبكة الراوتر.

---

## مثال

```text
Router = 192.168.1.1/24
PC     = 192.168.2.10/24
```

الجهاز والراوتر هنا في شبكتين مختلفتين.

---

## النتيجة

الجهاز لن يستطيع الوصول إلى الراوتر مباشرة.

---

## الحل

اجعل الجهاز في نفس شبكة الراوتر:

```text
Router = 192.168.1.1/24
PC     = 192.168.1.10/24
```

---

# 5. IP Conflict

## الخطأ

جهازان في نفس الشبكة يستخدمان نفس IP.

---

## النتيجة

تظهر مشاكل مثل:

```text
انقطاع متكرر
Ping غير مستقر
رسالة IP conflict
الجهاز يخرج ويدخل الشبكة
```

---

## الحل

تأكد أن كل جهاز له IP مختلف.

استخدم DHCP أو نظم الـ Static IPs جيدًا.

---

# 6. الاعتماد على Ping فقط

## الخطأ

تقول:

```text
Ping لا يعمل، إذن السيرفر مغلق
```

---

## الصحيح

فشل Ping لا يعني دائمًا أن الجهاز مغلق.

قد يكون السبب:

```text
Firewall يمنع ICMP
السيرفر يمنع Ping
Packet Loss
مشكلة Routing
```

---

## مثال

قد يفشل:

```cmd
ping example.com
```

لكن يعمل:

```bash
curl https://example.com
```

---

## القاعدة

```text
Ping أداة مهمة، لكنها ليست الحكم النهائي
```

---

# 7. الخلط بين DNS والإنترنت

## الخطأ

المستخدم يقول:

```text
الإنترنت لا يعمل
```

لكن الحقيقة أن الإنترنت يعمل، والمشكلة DNS.

---

## مثال

هذا يعمل:

```cmd
ping 8.8.8.8
```

لكن هذا لا يعمل:

```cmd
ping google.com
```

---

## التفسير

الاتصال بالإنترنت موجود، لكن تحويل الأسماء إلى IP لا يعمل.

---

## الحل

افحص DNS:

```cmd
nslookup google.com
```

وجرب DNS آخر مثل:

```text
8.8.8.8
1.1.1.1
```

---

# 8. نسيان أن DNS يستخدم Cache

## الخطأ

تغير DNS Record وتتوقع أن التغيير يظهر فورًا في كل مكان.

---

## الصحيح

DNS يعتمد على Cache و TTL.

قد تحتاج بعض الوقت حتى يظهر التغيير في كل الأماكن.

---

## الحل

افحص من أكثر من DNS Resolver:

```bash
nslookup example.com 8.8.8.8
nslookup example.com 1.1.1.1
```

وامسح DNS Cache محليًا إذا لزم:

```cmd
ipconfig /flushdns
```

---

# 9. الخلط بين Public IP و Private IP

## الخطأ

تحاول الوصول من الإنترنت إلى IP داخلي مثل:

```text
192.168.1.10
```

---

## الصحيح

Private IP لا يتم توجيهه على الإنترنت العام.

أمثلة Private IP:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

---

## الحل

للوصول من الإنترنت لجهاز داخلي تحتاج غالبًا:

```text
Public IP
Port Forwarding
Firewall Rule
أو VPN
```

---

# 10. اعتبار NAT هو Firewall

## الخطأ

تقول:

```text
عندي NAT، إذن أنا محمي بالكامل
```

---

## الصحيح

NAT يترجم العناوين.

Firewall يسمح أو يمنع الترافيك.

---

## الفرق

```text
NAT = Address Translation
Firewall = Traffic Filtering
```

---

## القاعدة

```text
NAT ليس بديلًا عن Firewall
```

---

# 11. Port Forwarding بدون تأمين

## الخطأ

فتح بورتات من الإنترنت إلى جهاز داخلي بدون حماية.

مثال خطير:

```text
Public IP:3389 → PC داخلي RDP
```

---

## الخطر

قد يتعرض الجهاز لمحاولات اختراق مباشرة.

---

## الأفضل

استخدم:

```text
VPN
Firewall Allowlist
Strong Passwords
MFA
تحديثات مستمرة
Logs Monitoring
```

---

# 12. نسيان CGNAT

## الخطأ

تعمل Port Forwarding في الراوتر، لكنه لا يعمل من الخارج.

---

## السبب المحتمل

أنت خلف CGNAT عند مزود الخدمة.

---

## كيف تعرف؟

قارن بين:

```text
WAN IP في الراوتر
Public IP الظاهر على الإنترنت
```

لو مختلفين، أو WAN IP مثل:

```text
100.64.x.x
10.x.x.x
192.168.x.x
172.16.x.x
```

فقد تكون خلف CGNAT.

---

## الحل

```text
طلب Public IP من ISP
استخدام VPN مناسب
Cloud Tunnel
VPS كوسيط
```

---

# 13. الخلط بين TCP و UDP

## الخطأ

تفتكر أن كل البورتات تعمل بنفس الطريقة.

---

## الصحيح

```text
TCP = Connection-Oriented
UDP = Connectionless
```

TCP يحتاج Handshake.

UDP لا يحتاج Handshake.

---

## مثال

```text
HTTPS = TCP 443 غالبًا
DNS = UDP 53 غالبًا
HTTP/3 = UDP 443
```

---

# 14. اختبار UDP مثل TCP

## الخطأ

تستخدم نفس طريقة فحص TCP مع UDP وتتوقع نتيجة واضحة.

---

## الصحيح

UDP لا يرد دائمًا.

عدم الرد لا يعني دائمًا أن البورت مغلق.

---

## مثال

في nmap قد يظهر UDP:

```text
open|filtered
```

لأن الفحص لا يستطيع الجزم بسهولة.

---

# 15. نسيان أن نفس Port قد يعمل مع TCP و UDP

## الخطأ

تقول:

```text
Port 53 مفتوح
```

بدون تحديد البروتوكول.

---

## الصحيح

يجب أن تقول:

```text
TCP 53
UDP 53
```

لأنهما مختلفان.

---

## مثال

```text
DNS يستخدم UDP 53 غالبًا
وقد يستخدم TCP 53 أيضًا
```

---

# 16. الاعتقاد أن Port Number يثبت نوع الخدمة دائمًا

## الخطأ

تقول:

```text
Port 443 يعني HTTPS دائمًا
```

---

## الصحيح

Port 443 غالبًا HTTPS، لكن أي برنامج يمكن أن يعمل على أي Port إذا تم ضبطه.

---

## القاعدة

```text
Port يعطي مؤشرًا، لكنه ليس دليلًا نهائيًا دائمًا
```

---

# 17. فتح خدمات الإدارة للإنترنت

## الخطأ

فتح خدمات مثل:

```text
SSH
RDP
Router Admin Panel
Database
```

مباشرة على الإنترنت.

---

## الخطر

هذه الخدمات تتعرض لمحاولات كثيرة جدًا.

---

## الأفضل

```text
استخدم VPN
اسمح فقط لعناوين IP محددة
استخدم MFA
غير كلمات المرور الافتراضية
حدّث الخدمة باستمرار
```

---

# 18. استخدام Telnet بدل SSH

## الخطأ

استخدام Telnet للإدارة عن بعد.

---

## المشكلة

Telnet غير مشفر.

يعني يمكن رؤية:

```text
Username
Password
Commands
```

---

## الصحيح

استخدم:

```text
SSH
```

بدل:

```text
Telnet
```

---

# 19. تجاهل VLANs

## الخطأ

تعتقد أن كل الأجهزة على نفس Switch تستطيع التواصل دائمًا.

---

## الصحيح

لو الأجهزة في VLANs مختلفة، فهي مفصولة منطقيًا.

---

## مثال

```text
PC1 في VLAN 10
PC2 في VLAN 20
```

حتى لو على نفس السويتش، لن يتواصلا بدون:

```text
Inter-VLAN Routing
```

---

# 20. Access Port و Trunk Port خطأ

## الخطأ

توصيل راوتر أو سويتش آخر على Port مضبوط Access بدل Trunk.

---

## النتيجة

بعض VLANs لا تعمل.

---

## الصحيح

```text
Access Port = لجهاز عادي مثل PC
Trunk Port = بين Switch و Switch أو Switch و Router
```

---

# 21. Native VLAN Mistake

## الخطأ

Native VLAN مختلفة بين طرفي Trunk.

---

## النتيجة

مشاكل غريبة في الشبكة وقد يحدث VLAN Leakage.

---

## الحل

تأكد أن Native VLAN متطابقة على طرفي الـ Trunk.

---

# 22. نسيان تفعيل Interface

## الخطأ

إعداد IP على Interface لكن الواجهة لا تعمل.

---

## في Cisco

قد تحتاج:

```cisco
no shutdown
```

---

## افحص

```cisco
show ip interface brief
```

لو الواجهة:

```text
administratively down
```

فهي مغلقة إداريًا.

---

# 23. الكابل أو Layer 1 آخر شيء تفكر فيه

## الخطأ

تبدأ بتحليل Routing و DNS وتنسى الكابل أو الإشارة.

---

## الصحيح

ابدأ من الأساسيات:

```text
Cable
Wi-Fi Signal
Link Light
Interface Up/Down
```

---

## القاعدة

```text
Layer 1 أولًا
```

---

# 24. تجاهل Wi-Fi Interference

## الخطأ

تعتقد أن ضعف الإنترنت دائمًا من الراوتر أو الشركة.

---

## السبب المحتمل

قد تكون المشكلة:

```text
إشارة ضعيفة
قناة مزدحمة
تداخل من شبكات قريبة
المسافة بعيدة
جدران كثيرة
```

---

## الحل

```text
اقترب من الراوتر
غير Channel
استخدم 5GHz إذا مناسب
ضع الراوتر في مكان أفضل
استخدم كابل للاختبار
```

---

# 25. الخلط بين Bandwidth و Latency

## الخطأ

تعتقد أن سرعة 100 Mbps تعني دائمًا أن الاتصال سريع في كل شيء.

---

## الصحيح

```text
Bandwidth = كمية البيانات الممكن نقلها
Latency = زمن التأخير
```

---

## مثال

قد يكون عندك Bandwidth عالي، لكن Latency عالي أيضًا.

هذا يؤثر على:

```text
Online Gaming
Video Calls
Remote Desktop
```

---

# 26. تجاهل Packet Loss

## الخطأ

السرعة تبدو جيدة، لكن المكالمات والألعاب سيئة.

---

## السبب المحتمل

```text
Packet Loss
```

---

## افحص

```cmd
ping 8.8.8.8
```

أو أدوات مثل:

```text
mtr
pathping
```

---

# 27. الخلط بين Router و Switch

## الخطأ

تعتقد أن Switch و Router نفس الوظيفة.

---

## الصحيح

```text
Switch = Layer 2 غالبًا، يربط أجهزة داخل نفس الشبكة
Router = Layer 3، يربط شبكات مختلفة
```

---

## مثال

للتواصل بين:

```text
192.168.1.0/24
192.168.2.0/24
```

تحتاج Router أو Layer 3 Switch.

---

# 28. Route ذهاب بدون Route عودة

## الخطأ

تضيف Route في اتجاه واحد فقط.

---

## مثال

```text
R1 يعرف طريق LAN2
لكن R2 لا يعرف طريق LAN1
```

---

## النتيجة

الطلب يذهب، لكن الرد لا يرجع.

---

## القاعدة

```text
Routing must work both ways
```

---

# 29. الخلط بين Default Route و Default Gateway

## Default Gateway

يستخدم غالبًا على أجهزة End Devices.

مثال:

```text
PC Default Gateway = 192.168.1.1
```

---

## Default Route

توجد في Routing Table.

مثال:

```text
0.0.0.0/0 via 192.168.1.1
```

---

## الفكرة

كلاهما يشير لطريق افتراضي، لكن المصطلح يختلف حسب السياق.

---

# 30. نسيان Longest Prefix Match

## الخطأ

تعتقد أن الراوتر يختار Route عشوائيًا أو حسب أول Route.

---

## الصحيح

الراوتر يختار أولًا:

```text
Longest Prefix Match
```

يعني المسار الأكثر تحديدًا.

---

## مثال

لو عنده:

```text
192.168.1.0/24
192.168.1.128/25
```

والوجهة:

```text
192.168.1.150
```

سيختار:

```text
192.168.1.128/25
```

لأنه أكثر تحديدًا.

---

# 31. الخلط بين Administrative Distance و Metric

## الخطأ

تستخدم Metric و AD كأنهما نفس الشيء.

---

## الصحيح

```text
Administrative Distance = يختار بين مصادر مختلفة للـ Route
Metric = يختار أفضل طريق داخل نفس البروتوكول
```

---

## مثال

```text
Static Route AD = 1
OSPF AD = 110
```

سيختار Static لو نفس الشبكة موجودة من الاثنين.

---

# 32. استخدام RIP في شبكة كبيرة

## الخطأ

استخدام RIP في شبكة كبيرة أو حديثة.

---

## المشكلة

RIP محدود بـ:

```text
15 Hops
```

ويعتمد فقط على:

```text
Hop Count
```

---

## الأفضل غالبًا

```text
OSPF
EIGRP
IS-IS
BGP حسب الحالة
```

---

# 33. OSPF Area غير صحيحة

## الخطأ

إعداد OSPF Areas بدون فهم.

---

## القاعدة المهمة

```text
Area 0 = Backbone Area
```

وغالبًا يجب أن تتصل المناطق الأخرى بـ Area 0.

---

## افحص

```cisco
show ip ospf neighbor
show ip route ospf
```

---

# 34. BGP ليس أسرع طريق دائمًا

## الخطأ

تعتقد أن BGP يختار دائمًا أسرع مسار.

---

## الصحيح

BGP يعتمد على:

```text
Policies
AS Path
Local Preference
MED
Business Agreements
```

وليس السرعة فقط.

---

# 35. تجاهل Firewall أثناء التشخيص

## الخطأ

كل شيء مضبوط، لكن الاتصال لا يعمل، وتنسى Firewall.

---

## افحص

```text
Windows Firewall
Linux UFW/iptables/nftables
Router Firewall
Cloud Security Groups
Hosting Firewall
```

---

## مثال

السيرفر يعمل على Port 443، لكن Firewall يمنع الدخول.

---

# 36. الخلط بين Connection Refused و Timeout

## Connection Refused

غالبًا يعني:

```text
الجهاز وصلته المحاولة
لكن الخدمة غير موجودة أو رفضت الاتصال
```

---

## Timeout

غالبًا يعني:

```text
لا يوجد رد
Firewall
Routing Problem
Packet Loss
```

---

## القاعدة

```text
Refused = رد بالرفض
Timeout = لا يوجد رد
```

---

# 37. تجاهل TLS Certificate Warnings

## الخطأ

عندما يظهر تحذير شهادة، تضغط Continue بدون فهم.

---

## الخطر

قد تكون:

```text
شهادة منتهية
اسم الدومين غير مطابق
CA غير موثوقة
محاولة Man-in-the-Middle
تاريخ الجهاز خطأ
```

---

## القاعدة

```text
لا تتجاهل Certificate Warning في مواقع حساسة
```

---

# 38. الاعتقاد أن HTTPS يعني الموقع آمن 100%

## الخطأ

تعتقد أن القفل في المتصفح يعني أن الموقع مضمون.

---

## الصحيح

HTTPS يعني غالبًا:

```text
الاتصال مشفر
الشهادة صالحة للدومين
```

لكنه لا يعني أن الموقع نفسه صادق أو غير احتيالي.

---

## مثال

موقع تصيد يمكن أن يملك HTTPS.

---

# 39. نسيان أن DNS قد يكون غير مشفر

## الخطأ

تعتقد أن HTTPS يشفر كل شيء، بما في ذلك DNS.

---

## الصحيح

HTTPS لا يعني أن DNS مشفر.

DNS العادي قد يظهر لمن يراقب الشبكة.

---

## الحلول

```text
DoH = DNS over HTTPS
DoT = DNS over TLS
VPN في بعض الحالات
```

---

# 40. الخلط بين HTTP Status Codes

## الخطأ

تفسير كل Error على أنه مشكلة إنترنت.

---

## الصحيح

```text
404 = المسار غير موجود
403 = ممنوع
401 = يحتاج تسجيل دخول
500 = خطأ في السيرفر
502 = مشكلة Gateway/Proxy
503 = الخدمة غير متاحة
504 = Timeout من Gateway
```

---

# 41. 404 لا يعني أن السيرفر Down

## الخطأ

تقول:

```text
الموقع واقع لأنه يعطي 404
```

---

## الصحيح

404 يعني أن السيرفر رد عليك، لكن الصفحة أو المسار غير موجود.

---

# 42. 500 غالبًا ليس من جهازك

## الخطأ

تحاول تصلح جهازك لأن الموقع يعطي 500.

---

## الصحيح

500 غالبًا خطأ في السيرفر أو التطبيق.

---

# 43. تجاهل Proxy أو VPN

## الخطأ

الاتصال لا يعمل، وتنسى أنك تستخدم VPN أو Proxy.

---

## الحل

جرب:

```text
إيقاف VPN مؤقتًا
تغيير شبكة
فحص Proxy Settings
اختبار بدون Extension في المتصفح
```

---

# 44. الاعتماد على Cache أثناء الاختبار

## الخطأ

تعتقد أن الموقع يعمل، لكنه يظهر من Cache.

---

## الحل

استخدم:

```text
Hard Reload
Incognito Mode
Clear Cache
curl -v
DevTools Network Disable Cache
```

---

# 45. نسيان أن المتصفح قد يفتح اتصالات كثيرة

## الخطأ

تعتقد أن فتح صفحة واحدة يعني Request واحد فقط.

---

## الصحيح

صفحة واحدة قد تحتاج:

```text
HTML
CSS
JavaScript
Images
Fonts
API Requests
Analytics
Ads
```

يعني عشرات الطلبات.

---

# 46. عدم قراءة Logs

## الخطأ

تحاول تخمين المشكلة فقط.

---

## الصحيح

اقرأ Logs.

أمثلة:

```text
Web Server Logs
Application Logs
Firewall Logs
Router Logs
System Logs
DNS Logs
```

---

# 47. عدم توثيق التغييرات

## الخطأ

تغير أكثر من إعداد في نفس الوقت ولا تعرف ما الذي أصلح أو كسر الشبكة.

---

## الأفضل

```text
غيّر شيئًا واحدًا في كل مرة
اكتب ما غيرته
احفظ نسخة من الإعدادات
اختبر بعد كل تغيير
```

---

# 48. تجاهل Time and Date

## الخطأ

TLS أو Login أو VPN لا يعمل، وتنسى فحص وقت الجهاز.

---

## السبب

وقت خاطئ قد يسبب مشاكل في:

```text
Certificates
Kerberos
VPN
Logs
Authentication
```

---

## الحل

اضبط الوقت والتاريخ والمنطقة الزمنية.

---

# 49. استخدام نفس كلمة المرور الافتراضية

## الخطأ

ترك كلمة مرور الراوتر أو السويتش أو السيرفر كما هي.

---

## الخطر

أي شخص يعرف الإعداد الافتراضي قد يدخل.

---

## الحل

```text
غير كلمة المرور الافتراضية
استخدم كلمة قوية
استخدم MFA إذا متاح
عطل الإدارة من الإنترنت
```

---

# 50. عدم تحديث الأجهزة

## الخطأ

ترك Router أو Firewall أو Server بدون تحديثات.

---

## الخطر

قد توجد ثغرات معروفة.

---

## الحل

```text
حدّث Firmware
حدّث OS
حدّث التطبيقات
راجع Security Advisories
```

---

# 51. Troubleshooting عشوائي

## الخطأ

تبدأ من أي مكان بدون ترتيب.

---

## الصحيح

ابدأ من الأسفل للأعلى:

```text
Layer 1: Cable/Wi-Fi
Layer 2: MAC/VLAN/Switch
Layer 3: IP/Route/Gateway
Layer 4: TCP/UDP/Ports
Layer 5-7: TLS/HTTP/DNS/Application
```

---

# 52. عدم مقارنة جهاز بجهاز آخر

## الخطأ

تحلل المشكلة على جهاز واحد فقط.

---

## الأفضل

اسأل:

```text
هل المشكلة على جهاز واحد أم كل الأجهزة؟
هل نفس الموقع لا يعمل من شبكة أخرى؟
هل يعمل على الموبايل داتا؟
هل يعمل من VPN؟
```

---

# 53. نسيان اختبار الكابل بدل Wi-Fi

## الخطأ

تحلل Wi-Fi ساعات طويلة.

---

## الأفضل

جرب كابل Ethernet.

لو المشكلة اختفت، فغالبًا المشكلة Wi-Fi.

---

# 54. سوء فهم DHCP

## الخطأ

تعتقد أن DHCP يعطي إنترنت.

---

## الصحيح

DHCP يعطي إعدادات مثل:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
Lease Time
```

لكن الإنترنت نفسه يعتمد على Routing و NAT و ISP و DNS وغيرها.

---

# 55. عدم فهم Lease Time

## الخطأ

تتوقع أن IP من DHCP ثابت للأبد.

---

## الصحيح

DHCP يعطي IP لمدة اسمها:

```text
Lease Time
```

وقد يتغير لاحقًا.

لو تحتاج IP ثابت لسيرفر، استخدم:

```text
DHCP Reservation
أو Static IP منظم
```

---

# 56. Static IP داخل DHCP Pool

## الخطأ

تضع Static IP لجهاز، لكنه داخل مدى DHCP.

---

## النتيجة

قد يعطي DHCP نفس IP لجهاز آخر، ويحدث IP Conflict.

---

## الحل

```text
استخدم IP خارج DHCP Pool
أو اعمل DHCP Reservation
```

---

# 57. عدم فهم 0.0.0.0 و 127.0.0.1

## 0.0.0.0

في Listening غالبًا تعني:

```text
الخدمة تستمع على كل واجهات الجهاز
```

---

## 127.0.0.1

تعني:

```text
localhost
الجهاز نفسه فقط
```

---

## الخطأ الشائع

تشغل خدمة على:

```text
127.0.0.1:8080
```

ثم تحاول الوصول إليها من جهاز آخر.

لن تعمل لأنها تستمع محليًا فقط.

---

# 58. عدم فهم Binding

## الخطأ

الخدمة تعمل محليًا لكن لا تعمل من الشبكة.

---

## السبب المحتمل

الخدمة تستمع على:

```text
127.0.0.1
```

وليس:

```text
0.0.0.0
```

---

## الحل

اضبط الخدمة لتستمع على IP مناسب أو على كل الواجهات.

---

# 59. نسيان MTU

## الخطأ

بعض المواقع أو VPN لا تعمل جيدًا، وتنسى MTU.

---

## السبب المحتمل

Packet أكبر من المسموح في الطريق، ومشكلة في Fragmentation أو Path MTU Discovery.

---

## اختبار Windows

```cmd
ping google.com -f -l 1472
```

---

## اختبار Linux

```bash
ping -M do -s 1472 google.com
```

---

# 60. عدم استخدام Wireshark عند الحاجة

## الخطأ

تخمن بدل أن ترى الـ Packets.

---

## الصحيح

استخدم Wireshark عندما تحتاج تعرف:

```text
هل DNS خرج؟
هل الرد رجع؟
هل TCP Handshake تم؟
هل هناك RST؟
هل TLS فشل؟
هل HTTP رد بكود معين؟
```

---

# Troubleshooting Checklist

عند أي مشكلة شبكة، اسأل بالترتيب:

```text
1. هل الكابل أو Wi-Fi يعمل؟
2. هل الجهاز لديه IP صحيح؟
3. هل Default Gateway صحيح؟
4. هل DNS صحيح؟
5. هل Ping للـ Gateway يعمل؟
6. هل Ping لـ Public IP يعمل؟
7. هل DNS Resolution يعمل؟
8. هل TCP/UDP Port المطلوب مفتوح؟
9. هل Firewall يسمح؟
10. هل TLS Certificate صحيح؟
11. هل HTTP Status Code يوضح السبب؟
12. هل المشكلة من جهاز واحد أم كل الشبكة؟
```

---

# أوامر تساعدك تتجنب الأخطاء

## Windows

```cmd
ipconfig /all
ipconfig /flushdns
ping 8.8.8.8
ping google.com
tracert 8.8.8.8
nslookup google.com
route print
arp -a
netstat -ano
```

---

## PowerShell

```powershell
Test-NetConnection example.com -Port 443
Get-NetTCPConnection
```

---

## Linux

```bash
ip addr
ip route
ip neigh
ping 8.8.8.8
dig google.com
traceroute 8.8.8.8
ss -tuln
ss -tunap
curl -v https://example.com
nc -vz example.com 443
```

---

# ملخص سريع

```text
لا تعتمد على Ping فقط
افحص DNS إذا IP يعمل والاسم لا يعمل
Private IP لا يظهر على الإنترنت مباشرة
NAT ليس Firewall
Port Forwarding يحتاج تأمين
TCP و UDP مختلفان
VLAN تفصل الشبكات منطقيًا
Route العودة مهم مثل Route الذهاب
HTTPS لا يعني أن الموقع موثوق 100%
ابدأ التشخيص من Layer 1 ثم اصعد تدريجيًا
```

---

# أهم القواعد الذهبية

```text
Layer 1 first
```

```text
IP يحدد الجهاز
Port يحدد الخدمة
```

```text
DNS يحول الاسم إلى IP فقط
```

```text
Ping failure is not final proof
```

```text
Routing must work both ways
```

```text
Firewall can break everything even if routing is correct
```

```text
Change one thing at a time
```

```text
Read logs and capture packets when needed
```

---

## Navigation

| Previous | Home | Next |
|---|---|---|
| [Wireshark Basics](./Wireshark-Basics.md) | [README](./README.md) | [Networking-Glossary](./Networking-Glossary.md) |
