# ICMP, Ping, and Traceroute

## أدوات تشخيص الشبكات

عندما تكون هناك مشكلة في الشبكة، أول أدوات غالبًا نستخدمها هي:

```text
ping
traceroute
tracert
```

هذه الأدوات تعتمد بشكل كبير على بروتوكول اسمه:

```text
ICMP
```

---

# ما هو ICMP؟

ICMP اختصار لـ:

```text
Internet Control Message Protocol
```

وهو بروتوكول يستخدم لإرسال رسائل تحكم وتشخيص داخل الشبكة.

ICMP لا يستخدم لنقل بيانات التطبيقات مثل HTTP أو FTP.

هو يستخدم أكثر لمعرفة:

```text
هل الجهاز الآخر موجود؟
هل الطريق إليه يعمل؟
أين تتوقف الحزمة؟
هل هناك مشكلة في التوجيه؟
```

---

# ICMP يعمل في أي طبقة؟

ICMP يعمل مع:

```text
Layer 3 - Network Layer
```

لأنه مرتبط ببروتوكول IP.

مهم جدًا:

```text
ICMP لا يستخدم TCP أو UDP Ports
```

يعني لا يوجد:

```text
ICMP Port 80
ICMP Port 443
```

لأن ICMP ليس TCP ولا UDP.

---

# لماذا نحتاج ICMP؟

نحتاج ICMP لمعرفة حالة الشبكة.

مثال:

```text
هل أستطيع الوصول إلى 8.8.8.8؟
هل الراوتر يرد؟
هل السيرفر موجود؟
هل هناك Packet ضاعت؟
هل الطريق طويل؟
```

بدون ICMP، سيكون تشخيص مشاكل الشبكات أصعب.

---

# أشهر استخدامات ICMP

```text
Ping
Traceroute
Destination Unreachable
Time Exceeded
Path MTU Discovery
```

---

# Ping

أمر Ping يستخدم لاختبار الوصول إلى جهاز آخر.

مثال:

```cmd
ping 8.8.8.8
```

لو الجهاز الآخر يرد، سترى ردودًا مثل:

```text
Reply from 8.8.8.8: bytes=32 time=25ms TTL=117
```

---

# كيف يعمل Ping؟

Ping يستخدم نوعين من رسائل ICMP:

```text
ICMP Echo Request
ICMP Echo Reply
```

الجهاز الأول يرسل:

```text
Echo Request
```

والجهاز الآخر يرد:

```text
Echo Reply
```

---

# Ping بشكل مبسط

```text
Your PC ─── ICMP Echo Request ───> Server
Your PC <── ICMP Echo Reply ───── Server
```

لو وصل الرد، فهذا يعني أن هناك اتصالًا على مستوى IP بين الطرفين.

---

# ICMP Types في Ping

في IPv4، Ping يستخدم غالبًا:

```text
Echo Request = Type 8
Echo Reply   = Type 0
```

يعني:

```text
Type 8 → سؤال
Type 0 → رد
```

---

# مثال Ping

```cmd
ping google.com
```

قد تظهر نتيجة مثل:

```text
Pinging google.com [142.250.80.46] with 32 bytes of data:
Reply from 142.250.80.46: bytes=32 time=30ms TTL=117
Reply from 142.250.80.46: bytes=32 time=29ms TTL=117
Reply from 142.250.80.46: bytes=32 time=31ms TTL=117
Reply from 142.250.80.46: bytes=32 time=30ms TTL=117
```

---

# شرح نتيجة Ping

## bytes

```text
bytes=32
```

هذا حجم البيانات المرسلة في رسالة Ping.

---

## time

```text
time=30ms
```

هذا الوقت الذي استغرقته الرسالة للذهاب والعودة.

يسمى:

```text
Round Trip Time
RTT
```

كلما كان الرقم أقل، كان الاتصال أسرع.

---

## TTL

```text
TTL=117
```

TTL اختصار لـ:

```text
Time To Live
```

وهو رقم يقل بمقدار 1 عند كل راوتر تمر به الحزمة.

---

# ما هو TTL؟

TTL موجود داخل IP Header.

هدفه منع الحزم من الدوران إلى الأبد في حالة وجود Routing Loop.

مثال:

```text
TTL يبدأ بـ 64
يمر على راوتر → يصبح 63
يمر على راوتر آخر → يصبح 62
```

لو وصل TTL إلى صفر، يتم إسقاط الحزمة.

---

# ماذا يحدث عند وصول TTL إلى صفر؟

الراوتر الذي يجد TTL وصل إلى صفر يسقط الحزمة ويرسل رسالة ICMP إلى المصدر:

```text
Time Exceeded
```

وهذه هي الفكرة الأساسية التي يستخدمها Traceroute.

---

# Packet Loss

لو أرسلت Ping ولم ترجع بعض الردود، فهذا يسمى:

```text
Packet Loss
```

مثال:

```text
Packets: Sent = 4, Received = 3, Lost = 1
```

يعني هناك فقد بنسبة:

```text
25%
```

أسباب Packet Loss قد تكون:

```text
ازدحام في الشبكة
ضعف Wi-Fi
مشكلة في الكابل
Firewall
مشكلة في الراوتر
السيرفر لا يرد على ICMP
```

---

# Request Timed Out

لو ظهر:

```text
Request timed out
```

فهذا لا يعني دائمًا أن الجهاز غير موجود.

قد يكون السبب:

```text
Firewall يمنع ICMP
السيرفر لا يرد على Ping
مشكلة في الطريق
Packet Loss
الجهاز مغلق
```

إذن:

```text
No ping reply does not always mean host is down
```

---

# Destination Unreachable

رسالة ICMP Destination Unreachable تعني أن الحزمة لا تستطيع الوصول للوجهة.

قد تكون بسبب:

```text
Network unreachable
Host unreachable
Protocol unreachable
Port unreachable
Fragmentation needed
```

في IPv4، Destination Unreachable هو:

```text
ICMP Type 3
```

---

# أمثلة Destination Unreachable

## Network Unreachable

يعني لا يوجد طريق للشبكة.

```text
No route to destination network
```

---

## Host Unreachable

يعني الشبكة قد تكون معروفة، لكن الجهاز نفسه لا يمكن الوصول إليه.

---

## Port Unreachable

تظهر غالبًا مع UDP.

مثال:

لو أرسلت UDP إلى بورت مغلق، قد يرد الجهاز:

```text
ICMP Port Unreachable
```

---

## Fragmentation Needed

تظهر في Path MTU Discovery.

تعني أن الحزمة أكبر من الحد المسموح في الطريق، ولا يمكن تجزئتها بسبب DF Bit.

---

# Traceroute

Traceroute أداة لمعرفة الطريق الذي تسلكه الحزمة حتى تصل للوجهة.

في Windows اسمها:

```cmd
tracert
```

في Linux وmacOS غالبًا:

```bash
traceroute
```

---

# كيف يعمل Traceroute؟

Traceroute يعتمد على TTL.

يرسل حزمًا بقيم TTL مختلفة:

```text
TTL = 1
TTL = 2
TTL = 3
TTL = 4
...
```

كل راوتر يقلل TTL بمقدار 1.

عندما يصل TTL إلى صفر، الراوتر يرسل ICMP Time Exceeded.

بهذا يعرف Traceroute عنوان كل راوتر في الطريق.

---

# مثال مبسط

```text
Your PC → Router 1 → Router 2 → Router 3 → Server
```

Traceroute يعمل هكذا:

## المحاولة الأولى

```text
TTL = 1
```

تصل الحزمة إلى Router 1.

Router 1 يقلل TTL إلى صفر ويرسل:

```text
ICMP Time Exceeded
```

إذن نعرف Router 1.

---

## المحاولة الثانية

```text
TTL = 2
```

الحزمة تمر من Router 1 ثم تصل إلى Router 2.

Router 2 يجعل TTL صفر ويرسل:

```text
ICMP Time Exceeded
```

إذن نعرف Router 2.

---

## المحاولة الثالثة

```text
TTL = 3
```

نعرف Router 3.

وهكذا حتى نصل إلى السيرفر.

---

# رسم Traceroute

```text
TTL 1 → Router 1 returns ICMP Time Exceeded
TTL 2 → Router 2 returns ICMP Time Exceeded
TTL 3 → Router 3 returns ICMP Time Exceeded
TTL 4 → Destination reached
```

---

# مثال أمر Traceroute

## Windows

```cmd
tracert 8.8.8.8
```

## Linux / macOS

```bash
traceroute 8.8.8.8
```

أو باستخدام ICMP:

```bash
traceroute -I 8.8.8.8
```

---

# مثال نتيجة Traceroute

```text
1     1 ms     1 ms     1 ms   192.168.1.1
2    10 ms    12 ms    11 ms   10.10.0.1
3    20 ms    22 ms    21 ms   172.16.5.1
4    30 ms    29 ms    31 ms   8.8.8.8
```

---

# شرح النتيجة

كل سطر يمثل Hop.

مثال:

```text
1   192.168.1.1
```

هذا غالبًا الراوتر داخل بيتك.

```text
2   10.10.0.1
```

قد يكون راوتر داخل شبكة مزود الخدمة.

```text
4   8.8.8.8
```

هذه الوجهة النهائية.

---

# لماذا تظهر نجوم * * * ؟

أحيانًا تظهر النتيجة هكذا:

```text
5   *   *   *
```

هذا قد يعني:

```text
الراوتر لا يرد على ICMP
Firewall يمنع الرد
الحزمة ضاعت
الراوتر مشغول
```

لكن هذا لا يعني دائمًا أن الطريق متوقف.

قد تكمل الحزم بعده بشكل طبيعي.

---

# Ping vs Traceroute

| المقارنة | Ping | Traceroute |
|---|---|---|
| الهدف | هل الوجهة ترد؟ | ما الطريق إلى الوجهة؟ |
| يعتمد على | Echo Request/Reply | TTL + Time Exceeded |
| يعطيك | زمن الوصول وفقد الحزم | الراوترات في الطريق |
| الاستخدام | اختبار سريع | تحديد مكان المشكلة |

---

# ICMP و Firewall

بعض الشبكات تمنع ICMP لأسباب أمنية.

مثال:

```text
Firewall blocks Echo Request
```

لذلك قد لا يرد السيرفر على Ping رغم أن الموقع يعمل.

مثال:

```text
ping example.com fails
but https://example.com works
```

هذا طبيعي إذا كان ICMP محجوبًا.

---

# هل منع ICMP دائمًا جيد؟

ليس دائمًا.

منع ICMP بالكامل قد يسبب مشاكل في التشخيص وبعض وظائف الشبكة مثل:

```text
Path MTU Discovery
Troubleshooting
Network Monitoring
```

الأفضل عادة هو التحكم فيه بدل منعه بالكامل.

---

# ICMP و Path MTU Discovery

Path MTU Discovery يستخدم ICMP لمعرفة أكبر حجم Packet يمكن أن يعبر الطريق بدون Fragmentation.

لو Packet أكبر من المسموح، يرسل الراوتر رسالة:

```text
Fragmentation Needed
```

لو تم منع هذه الرسائل، قد تحدث مشكلة تسمى أحيانًا:

```text
PMTUD Black Hole
```

يعني الاتصال يتعطل أو يصبح بطيئًا لأن الجهاز لا يعرف أن عليه تقليل حجم الحزمة.

---

# ICMP و IPv6

في IPv6، ICMPv6 مهم جدًا.

ليس فقط للتشخيص، بل أيضًا لوظائف أساسية مثل:

```text
Neighbor Discovery
Router Advertisement
Router Solicitation
Path MTU Discovery
```

لذلك منع ICMPv6 بشكل كامل قد يكسر الشبكة.

---

# ICMP Security

رغم أن ICMP مفيد، يمكن استغلاله في هجمات.

أمثلة:

```text
ICMP Flood
Ping of Death
ICMP Tunneling
Network Reconnaissance
```

---

## ICMP Flood

المهاجم يرسل عددًا كبيرًا من ICMP Echo Requests لإرهاق الهدف.

---

## Ping of Death

هجوم قديم يعتمد على إرسال Ping بحجم غير طبيعي يسبب مشاكل في أنظمة قديمة.

الأنظمة الحديثة غالبًا محمية منه.

---

## ICMP Tunneling

استخدام ICMP لإخفاء بيانات داخل رسائل ICMP وتجاوز بعض القيود الأمنية.

---

# أوامر مفيدة

## Windows

```cmd
ping 8.8.8.8
ping google.com
tracert 8.8.8.8
pathping 8.8.8.8
```

---

## Linux / macOS

```bash
ping 8.8.8.8
ping google.com
traceroute 8.8.8.8
traceroute -I 8.8.8.8
mtr 8.8.8.8
```

---

# Troubleshooting عملي

## الحالة 1: لا يوجد إنترنت

جرب:

```cmd
ping 127.0.0.1
```

لو يعمل، TCP/IP Stack على جهازك غالبًا يعمل.

ثم:

```cmd
ping 192.168.1.1
```

لو لا يعمل، المشكلة قد تكون بينك وبين الراوتر.

ثم:

```cmd
ping 8.8.8.8
```

لو يعمل، الإنترنت موجود.

ثم:

```cmd
ping google.com
```

لو لا يعمل، المشكلة غالبًا DNS.

---

# ترتيب اختبار الاتصال

```text
1. Ping localhost
2. Ping your own IP
3. Ping Default Gateway
4. Ping public IP like 8.8.8.8
5. Ping domain name like google.com
```

---

# ماذا تفهم من النتائج؟

## Ping للـ Gateway لا يعمل

قد تكون المشكلة في:

```text
Wi-Fi
Cable
VLAN
IP Address
Subnet Mask
Firewall على الجهاز
```

---

## Ping للـ Gateway يعمل لكن 8.8.8.8 لا يعمل

قد تكون المشكلة في:

```text
Router
NAT
ISP
Default Route
Firewall
```

---

## Ping لـ 8.8.8.8 يعمل لكن google.com لا يعمل

غالبًا المشكلة في:

```text
DNS
```

---

# ملخص سريع

```text
ICMP = بروتوكول تشخيص وتحكم في Layer 3
Ping = يستخدم Echo Request و Echo Reply
Traceroute = يستخدم TTL لمعرفة الطريق
TTL يقل عند كل Router
Time Exceeded تظهر عندما يصل TTL إلى صفر
Destination Unreachable تعني أن الوجهة أو الطريق غير متاح
ICMP لا يستخدم TCP أو UDP Ports
```

---

# أهم القواعد

```text
Ping reply means IP reachability exists
```

```text
No ping reply لا يعني دائمًا أن الجهاز مغلق
```

```text
Traceroute يساعدك تعرف أين يتوقف الطريق
```

```text
TTL prevents packets from looping forever
```

```text
ICMP مهم للتشخيص، ومنعه بالكامل قد يسبب مشاكل
```

---

## Next Topic

اقرأ بعد ذلك:

[NAT and PAT](./NAT-and-PAT.md)
