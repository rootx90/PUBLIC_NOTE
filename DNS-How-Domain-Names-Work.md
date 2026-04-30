# DNS - How Domain Names Work

## كيف تعمل أسماء النطاقات؟

عندما تفتح المتصفح وتكتب:

```text
google.com
```

أنت كبشر تفهم الاسم بسهولة.

لكن أجهزة الكمبيوتر والراوترات لا تتعامل مع الأسماء بهذه الطريقة.  
هي تحتاج عنوان IP.

مثال:

```text
142.250.80.46
```

هنا يأتي دور DNS.

---

# ما هو DNS؟

DNS اختصار لـ:

```text
Domain Name System
```

ومعناه نظام تحويل أسماء النطاقات إلى عناوين IP.

ببساطة:

```text
DNS = دليل التليفونات الخاص بالإنترنت
```

أنت تعرف الاسم:

```text
google.com
```

والـ DNS يعطيك الرقم أو العنوان:

```text
142.250.80.46
```

---

# لماذا نحتاج DNS؟

تخيل أنك تريد حفظ IP لكل موقع تستخدمه:

```text
Google
Facebook
YouTube
GitHub
Gmail
ChatGPT
```

سيكون الأمر صعبًا جدًا.

بدل ذلك، نستخدم أسماء سهلة مثل:

```text
google.com
github.com
youtube.com
```

والـ DNS يحول هذه الأسماء إلى IP.

---

# الفكرة الأساسية

```text
User types domain name
        ↓
DNS resolves domain to IP
        ↓
Browser connects to that IP
```

مثال:

```text
google.com
        ↓
DNS
        ↓
142.250.80.46
```

---

# من يسأل من؟

عندما تكتب موقعًا في المتصفح، جهازك لا يسأل السيرفر النهائي مباشرة.

هناك رحلة DNS تمر غالبًا بعدة مراحل.

أهم الأطراف:

```text
Client
Recursive Resolver
Root Server
TLD Server
Authoritative Server
```

---

# 1. Client

الـ Client هو جهازك أو المتصفح أو نظام التشغيل.

مثال:

```text
Laptop
Phone
Browser
Operating System
```

هو الذي يبدأ السؤال:

```text
ما هو IP الخاص بـ google.com؟
```

---

# 2. Recursive Resolver

الـ Recursive Resolver هو السيرفر الذي يبحث نيابة عنك.

غالبًا يكون من:

```text
ISP
Google DNS
Cloudflare DNS
Quad9 DNS
```

أمثلة مشهورة:

```text
Google DNS     = 8.8.8.8
Cloudflare DNS = 1.1.1.1
Quad9 DNS      = 9.9.9.9
```

وظيفته:

```text
يأخذ سؤالك
ويبحث في نظام DNS
ثم يرجع لك الإجابة النهائية
```

---

# 3. Root Server

الـ Root Servers هي أعلى نقطة في شجرة DNS.

هي لا تعرف غالبًا IP الخاص بـ google.com مباشرة.

لكنها تعرف من المسؤول عن الامتدادات مثل:

```text
.com
.org
.net
.de
.eg
```

مثال:

لو سألت Root Server عن:

```text
google.com
```

قد يقول لك:

```text
لا أعرف IP الخاص بـ google.com
لكن اسأل TLD Server المسؤول عن .com
```

---

# 4. TLD Server

TLD اختصار لـ:

```text
Top Level Domain
```

وهو مسؤول عن الامتداد.

مثال:

```text
.com
.org
.net
.edu
.de
.eg
```

لو الموقع:

```text
google.com
```

فالـ TLD هو:

```text
.com
```

الـ TLD Server لا يعطيك غالبًا IP النهائي للموقع، لكنه يعرف من هو الـ Authoritative Server المسؤول عن النطاق.

يعني يقول:

```text
اسأل السيرفر المسؤول عن google.com
```

---

# 5. Authoritative Server

الـ Authoritative Server هو السيرفر الذي يملك الإجابة النهائية.

هو المسؤول عن DNS Records الخاصة بالدومين.

مثال:

```text
google.com → 142.250.80.46
```

هذا السيرفر هو مصدر الحقيقة بالنسبة للنطاق.

---

# رحلة DNS كاملة

لو كتبت في المتصفح:

```text
google.com
```

تحدث الخطوات التالية:

```text
1. المتصفح يسأل نظام التشغيل
2. نظام التشغيل يسأل Recursive Resolver
3. Resolver يسأل Root Server
4. Root Server يقول: اسأل .com TLD
5. Resolver يسأل .com TLD Server
6. TLD يقول: اسأل Authoritative Server الخاص بـ google.com
7. Resolver يسأل Authoritative Server
8. Authoritative Server يرجع IP
9. Resolver يرجع IP لجهازك
10. المتصفح يبدأ الاتصال بهذا IP
```

---

# رسم مبسط

```text
┌──────────┐
│ Browser  │
└────┬─────┘
     │ What is IP of google.com?
     ▼
┌────────────────────┐
│ Recursive Resolver │
└────┬───────────────┘
     │ Ask Root
     ▼
┌─────────────┐
│ Root Server │
└────┬────────┘
     │ Ask .com TLD
     ▼
┌────────────┐
│ TLD Server │
└────┬───────┘
     │ Ask Authoritative Server
     ▼
┌──────────────────────┐
│ Authoritative Server │
└────┬─────────────────┘
     │ IP = 142.250.80.46
     ▼
┌────────────────────┐
│ Recursive Resolver │
└────┬───────────────┘
     │ Return IP
     ▼
┌──────────┐
│ Browser  │
└──────────┘
```

---

# DNS Cache

حتى لا تحدث هذه الرحلة الطويلة كل مرة، يتم تخزين الإجابات مؤقتًا في أماكن مختلفة.

هذا يسمى:

```text
DNS Cache
```

---

## أماكن DNS Cache

قد يتم حفظ نتيجة DNS في:

```text
Browser Cache
Operating System Cache
Router Cache
Recursive Resolver Cache
```

مثال:

لو فتحت:

```text
google.com
```

ثم أغلقته وفتحته مرة أخرى بعد دقيقة، غالبًا لن تحدث رحلة DNS كاملة.  
الجهاز أو المتصفح قد يستخدم الإجابة المحفوظة.

---

# TTL

TTL اختصار لـ:

```text
Time To Live
```

في DNS، معناه مدة بقاء الإجابة في الكاش.

مثال:

```text
google.com = 142.250.80.46
TTL = 300 seconds
```

يعني يمكن حفظ هذه الإجابة لمدة 300 ثانية.

بعد انتهاء الـ TTL، يجب السؤال مرة أخرى للتأكد من أن الـ IP لم يتغير.

---

# DNS Records

الـ DNS لا يحتوي فقط على IP.  
هو يحتوي على أنواع مختلفة من السجلات تسمى:

```text
DNS Records
```

أهم السجلات:

```text
A
AAAA
CNAME
MX
NS
TXT
SOA
```

---

## A Record

الـ A Record يحول الدومين إلى IPv4 Address.

مثال:

```text
example.com → 93.184.216.34
```

يعني:

```text
Domain Name → IPv4
```

---

## AAAA Record

الـ AAAA Record يحول الدومين إلى IPv6 Address.

مثال:

```text
example.com → 2606:2800:220:1:248:1893:25c8:1946
```

يعني:

```text
Domain Name → IPv6
```

---

## CNAME Record

CNAME اختصار لـ:

```text
Canonical Name
```

وهو يجعل اسمًا يشير إلى اسم آخر.

مثال:

```text
www.example.com → example.com
```

يعني `www.example.com` ليس له IP مباشر، لكنه يشير إلى `example.com`.

ملاحظة مهمة:

```text
CNAME points to another name, not directly to an IP
```

---

## MX Record

MX اختصار لـ:

```text
Mail Exchange
```

وهو يحدد سيرفرات البريد الخاصة بالدومين.

مثال:

```text
example.com mail is handled by mail.example.com
```

هذا مهم للإيميلات مثل:

```text
user@example.com
```

---

## NS Record

NS اختصار لـ:

```text
Name Server
```

وهو يحدد السيرفرات المسؤولة عن DNS Zone الخاصة بالدومين.

مثال:

```text
example.com uses ns1.example-dns.com
example.com uses ns2.example-dns.com
```

---

## TXT Record

TXT Record يخزن نصوصًا داخل DNS.

يستخدم كثيرًا في:

```text
Domain Verification
SPF
DKIM
DMARC
Security Policies
```

مثال:

```text
v=spf1 include:_spf.google.com ~all
```

---

## SOA Record

SOA اختصار لـ:

```text
Start of Authority
```

وهو يحتوي على معلومات إدارية عن الـ DNS Zone.

مثل:

```text
Primary Name Server
Responsible Email
Serial Number
Refresh Time
Retry Time
Expire Time
```

---

# أنواع DNS Queries

هناك أكثر من طريقة للسؤال في DNS.

أشهر الأنواع:

```text
Recursive Query
Iterative Query
Non-Recursive Query
```

---

## Recursive Query

في Recursive Query، العميل يقول للسيرفر:

```text
أريد الإجابة النهائية
```

يعني جهازك يسأل Recursive Resolver، والـ Resolver هو الذي يبحث في كل مكان ثم يرجع لك الإجابة.

مثال:

```text
Client → Resolver:
Give me the IP of google.com
```

---

## Iterative Query

في Iterative Query، السيرفر لا يعطي الإجابة النهائية دائمًا، لكنه يعطيك الخطوة التالية.

مثال:

```text
Root Server:
لا أعرف IP النهائي، لكن اسأل .com Server
```

ثم:

```text
TLD Server:
اسأل Authoritative Server الخاص بـ google.com
```

---

## Non-Recursive Query

في Non-Recursive Query، السيرفر يرد مباشرة من الكاش أو من المعلومات التي يعرفها بالفعل.

مثال:

```text
Resolver already has google.com in cache
```

فيرد مباشرة بدون رحلة كاملة.

---

# DNS يستخدم UDP أم TCP؟

DNS غالبًا يستخدم:

```text
UDP Port 53
```

لأن أغلب استعلامات DNS صغيرة وسريعة.

لكن DNS يمكن أن يستخدم أيضًا:

```text
TCP Port 53
```

في حالات مثل:

```text
Zone Transfer
Large Responses
DNSSEC
بعض الحالات التي تتطلب موثوقية أكبر
```

---

# لماذا DNS غالبًا يستخدم UDP؟

لأن DNS يحتاج إجابة سريعة.

لو كل DNS Query استخدم TCP، سيحتاج:

```text
TCP Handshake
DNS Query
DNS Response
TCP Close
```

وهذا يزيد الوقت.

لذلك UDP مناسب لأنه خفيف وسريع.

---

# DNS و DHCP

في أغلب الشبكات، جهازك يعرف DNS Server من DHCP.

مثال DHCP يعطيك:

```text
IP Address      = 192.168.1.50
Default Gateway = 192.168.1.1
DNS Server      = 8.8.8.8
```

إذن:

```text
DHCP يعطيك DNS Server
DNS يحول أسماء المواقع إلى IP
```

---

# DNS و HTTPS

عندما تفتح:

```text
https://example.com
```

الترتيب غالبًا يكون:

```text
1. DNS لمعرفة IP
2. TCP Handshake مع السيرفر
3. TLS Handshake لتشفير الاتصال
4. HTTP Request داخل TLS
```

يعني DNS يحدث قبل TCP وTLS وHTTP Request.

---

# DNS و ARP

DNS يعطيك IP الخاص بالموقع.

لكن لو الموقع خارج شبكتك، جهازك لا يحتاج MAC الخاص بالموقع مباشرة.

هو يحتاج MAC الخاص بـ:

```text
Default Gateway
```

يعني:

```text
DNS gives destination IP
ARP gives next-hop MAC
```

مثال:

```text
DNS:
google.com = 142.250.80.46

ARP:
Who has 192.168.1.1?
```

---

# DNS Troubleshooting

لو الموقع لا يفتح، قد تكون المشكلة في DNS.

مثال:

```text
google.com لا يفتح
لكن 8.8.8.8 يعمل Ping
```

هذا غالبًا يعني:

```text
الإنترنت موجود
لكن DNS لا يعمل
```

---

# أوامر مفيدة

## nslookup

على Windows وLinux وmacOS:

```bash
nslookup google.com
```

مثال نتيجة:

```text
Name:    google.com
Address: 142.250.80.46
```

---

## dig

على Linux وmacOS غالبًا:

```bash
dig google.com
```

عرض نوع معين من السجلات:

```bash
dig google.com A
dig google.com AAAA
dig google.com MX
dig google.com TXT
dig google.com NS
```

استخدام DNS Server معين:

```bash
dig @8.8.8.8 google.com
dig @1.1.1.1 google.com
```

---

## flush DNS cache

### Windows

```cmd
ipconfig /flushdns
```

### macOS

```bash
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```

### Linux systemd

```bash
sudo resolvectl flush-caches
```

أو في بعض الأنظمة القديمة:

```bash
sudo systemd-resolve --flush-caches
```

---

# مشاكل DNS شائعة

## 1. DNS Server لا يرد

الأعراض:

```text
المواقع لا تفتح بالأسماء
لكن الإنترنت قد يعمل بالأرقام
```

الحل المحتمل:

```text
تغيير DNS Server إلى 8.8.8.8 أو 1.1.1.1
```

---

## 2. DNS Cache قديم

قد يكون الموقع غير IP الخاص به، لكن جهازك ما زال يستخدم IP القديم.

الحل:

```text
Flush DNS Cache
```

---

## 3. CNAME خطأ

لو `www.example.com` يشير إلى اسم غير صحيح، الموقع قد لا يفتح.

---

## 4. MX Record خطأ

لو MX Record غير صحيح، قد لا تصل الإيميلات.

---

## 5. Propagation Delay

عند تغيير DNS Records، قد يحتاج التغيير وقتًا حتى يظهر للجميع بسبب الكاش وTTL.

---

# مثال عملي كامل

أنت كتبت في المتصفح:

```text
https://github.com
```

الخطوات:

```text
1. المتصفح يفحص Browser DNS Cache
2. نظام التشغيل يفحص OS DNS Cache
3. لو لا توجد نتيجة، يسأل Recursive Resolver
4. Resolver يسأل Root Server
5. Root يوجهه إلى .com TLD
6. TLD يوجهه إلى Authoritative Server
7. Authoritative يرجع IP الخاص بـ github.com
8. Resolver يحفظ الإجابة حسب TTL
9. جهازك يحصل على IP
10. يبدأ TCP Handshake مع هذا IP
11. يبدأ TLS Handshake
12. يرسل HTTP Request
```

---

# DNS في Bug Bounty و Pentest

DNS مهم جدًا في الاستطلاع.

يمكن استخدام DNS لمعرفة:

```text
Subdomains
Mail Servers
Name Servers
TXT Records
Cloud Providers
Misconfigurations
```

أمثلة:

```bash
dig example.com NS
dig example.com MX
dig example.com TXT
dig sub.example.com A
```

ممكن أيضًا البحث عن Subdomains مثل:

```text
api.example.com
admin.example.com
dev.example.com
test.example.com
```

---

# ملخص سريع

```text
DNS = يحول Domain Name إلى IP Address
Recursive Resolver = يبحث نيابة عنك
Root Server = يوجهك إلى TLD
TLD Server = يوجهك إلى Authoritative Server
Authoritative Server = يملك الإجابة النهائية
TTL = مدة بقاء الإجابة في الكاش
A Record = Domain to IPv4
AAAA Record = Domain to IPv6
CNAME = Alias to another domain
MX = Mail Server
NS = Name Server
TXT = Text data for verification/security
```

---

# أهم القواعد

```text
DNS happens before TCP connection
```

```text
DNS gives IP
ARP gives MAC
```

```text
DNS usually uses UDP 53
but can use TCP 53
```

```text
If domain name fails but IP works:
check DNS
```

---

## Next Topic

اقرأ بعد ذلك:

[ARP Protocol](./ARP-Protocol.md)
