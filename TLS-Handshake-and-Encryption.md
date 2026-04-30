# TLS Handshake and Encryption

## كيف يتم تشفير الاتصال؟

عندما تفتح موقعًا يبدأ بـ:

```text
https://
```

فهذا يعني أن الاتصال بين جهازك والسيرفر مشفر غالبًا باستخدام:

```text
TLS
```

مثال:

```text
https://example.com
```

هنا المتصفح لا يرسل HTTP Request عادي مباشرة.  
بل يحدث أولًا اتصال TCP، ثم TLS Handshake، وبعدها يتم إرسال HTTP داخل الاتصال المشفر.

---

# ما هو TLS؟

TLS اختصار لـ:

```text
Transport Layer Security
```

وهو بروتوكول يستخدم لتأمين الاتصال بين العميل والسيرفر.

TLS يوفر ثلاث نقاط مهمة:

```text
Encryption
Authentication
Integrity
```

---

# 1. Encryption

Encryption يعني تشفير البيانات.

أي أن البيانات لا تكون واضحة لأي شخص يراقب الاتصال.

مثال بدون تشفير:

```text
username=mahmoud&password=123456
```

مثال بعد التشفير:

```text
8fA91xZq29sP0aLm...
```

الشخص الذي يراقب الشبكة قد يرى أنك متصل بسيرفر معين، لكنه لا يستطيع قراءة محتوى البيانات بسهولة.

---

# 2. Authentication

Authentication يعني التأكد من هوية السيرفر.

عندما تدخل إلى:

```text
https://google.com
```

المتصفح يحتاج يتأكد أن السيرفر فعلًا تابع لـ Google وليس موقعًا مزيفًا.

هذا يتم باستخدام:

```text
Certificate
```

أو:

```text
Digital Certificate
```

---

# 3. Integrity

Integrity تعني التأكد أن البيانات لم يتم تعديلها أثناء الطريق.

يعني لو شخص حاول تغيير البيانات بينك وبين السيرفر، TLS يساعد على اكتشاف ذلك.

مثال:

```text
Original Message: Transfer $10
Modified Message: Transfer $1000
```

TLS يجعل هذا التلاعب قابلًا للاكتشاف.

---

# TLS vs SSL

قد تسمع كثيرًا كلمة:

```text
SSL
```

لكن SSL قديم وغير آمن الآن.

الاسم الصحيح الحديث هو:

```text
TLS
```

لكن الناس ما زالت تقول SSL أحيانًا بالمعنى العام.

مثال:

```text
SSL Certificate
```

غالبًا المقصود بها فعليًا:

```text
TLS Certificate
```

---

# أين يحدث TLS؟

TLS يحدث بعد TCP وقبل HTTP.

الترتيب عند فتح موقع HTTPS:

```text
1. DNS
2. TCP 3-Way Handshake
3. TLS Handshake
4. HTTP Request داخل TLS
5. HTTP Response داخل TLS
```

يعني:

```text
TCP ينشئ الاتصال
TLS يشفر الاتصال
HTTP ينقل طلبات وصفحات الويب
```

---

# HTTPS يعني ماذا؟

HTTPS هو:

```text
HTTP over TLS
```

يعني HTTP العادي لكن داخل قناة مشفرة باستخدام TLS.

```text
HTTP  = غير مشفر غالبًا
HTTPS = HTTP داخل TLS
```

---

# الفرق بين HTTP و HTTPS

| المقارنة | HTTP | HTTPS |
|---|---|---|
| التشفير | لا | نعم |
| البورت الشائع | TCP 80 | TCP 443 |
| الحماية من التجسس | ضعيفة | قوية |
| شهادة رقمية | لا يحتاج غالبًا | يحتاج Certificate |
| الاستخدام الحديث | غير مفضل | المفضل |

---

# ما هو TLS Handshake؟

TLS Handshake هو عملية تفاوض بين العميل والسيرفر قبل إرسال البيانات المشفرة.

فيه يتم الاتفاق على:

```text
TLS Version
Cipher Suite
Key Exchange
Server Certificate
Session Keys
```

بعد نجاح الـ Handshake، يبدأ الاتصال المشفر.

---

# ماذا يحدث قبل TLS Handshake؟

قبل TLS، يجب أن يكون هناك اتصال TCP.

مثال:

```text
Client → Server: SYN
Server → Client: SYN-ACK
Client → Server: ACK
```

بعدها فقط يبدأ TLS Handshake.

---

# رسم مبسط

```text
Client                              Server
  │                                    │
  │ TCP SYN                            │
  │───────────────────────────────────>│
  │ TCP SYN-ACK                        │
  │<───────────────────────────────────│
  │ TCP ACK                            │
  │───────────────────────────────────>│
  │                                    │
  │ TLS ClientHello                    │
  │───────────────────────────────────>│
  │ TLS ServerHello + Certificate      │
  │<───────────────────────────────────│
  │ Key Exchange / Finished            │
  │───────────────────────────────────>│
  │ Finished                           │
  │<───────────────────────────────────│
  │                                    │
  │ Encrypted HTTP                     │
  │───────────────────────────────────>│
```

---

# ClientHello

أول رسالة في TLS Handshake غالبًا تكون:

```text
ClientHello
```

يرسلها العميل إلى السيرفر.

تحتوي على معلومات مثل:

```text
TLS versions supported
Cipher suites supported
Random value
SNI
ALPN
Supported groups
Key share في TLS 1.3
```

---

# TLS Version

العميل يخبر السيرفر بالإصدارات التي يدعمها.

أمثلة:

```text
TLS 1.2
TLS 1.3
```

الإصدارات القديمة مثل SSL وTLS 1.0 وTLS 1.1 لم تعد مفضلة أمنيًا.

---

# Cipher Suites

Cipher Suite هي مجموعة خوارزميات يستخدمها TLS لتأمين الاتصال.

قد تشمل:

```text
Key Exchange Algorithm
Encryption Algorithm
Integrity Algorithm
```

مثال مبسط:

```text
TLS_AES_128_GCM_SHA256
```

الفكرة:

```text
العميل يقول للسيرفر:
أنا أدعم هذه الخوارزميات
اختر واحدة مناسبة
```

---

# Random Value

العميل يرسل قيمة عشوائية.

والسيرفر أيضًا يرسل قيمة عشوائية.

هذه القيم تساعد في إنشاء مفاتيح الجلسة.

```text
Client Random
Server Random
```

---

# SNI

SNI اختصار لـ:

```text
Server Name Indication
```

وهو يجعل العميل يخبر السيرفر باسم الموقع الذي يريد الوصول إليه.

مثال:

```text
example.com
```

لماذا هذا مهم؟

لأن نفس السيرفر قد يستضيف أكثر من موقع على نفس IP.

مثال:

```text
example.com
test.com
shop.com
```

كلهم قد يكونون على نفس IP، لكن لكل واحد Certificate مختلفة.

SNI يساعد السيرفر يختار الشهادة الصحيحة.

---

# ALPN

ALPN اختصار لـ:

```text
Application-Layer Protocol Negotiation
```

ويستخدم للاتفاق على بروتوكول التطبيق.

مثال:

```text
HTTP/1.1
HTTP/2
```

العميل يقول:

```text
أنا أدعم HTTP/2 و HTTP/1.1
```

والسيرفر يختار المناسب.

---

# ServerHello

السيرفر يرد برسالة:

```text
ServerHello
```

ويختار فيها الإعدادات النهائية.

تحتوي غالبًا على:

```text
Selected TLS Version
Selected Cipher Suite
Server Random
Key Share في TLS 1.3
```

---

# Certificate

السيرفر يرسل شهادة رقمية:

```text
Certificate
```

هذه الشهادة تثبت هوية السيرفر.

الشهادة تحتوي على معلومات مثل:

```text
Domain Name
Public Key
Issuer
Validity Period
Signature
```

---

# ما هي Certificate؟

Certificate هي ملف رقمي يربط بين:

```text
Domain Name
Public Key
```

مثال:

```text
example.com
Public Key الخاص بـ example.com
```

وتكون موقعة من جهة موثوقة تسمى:

```text
Certificate Authority
```

أو:

```text
CA
```

---

# Certificate Authority

CA هي جهة موثوقة تصدر وتوقع الشهادات.

أمثلة:

```text
DigiCert
Let's Encrypt
GlobalSign
Sectigo
Google Trust Services
```

المتصفح يحتوي مسبقًا على قائمة CAs موثوقة.

لو الشهادة موقعة من CA موثوقة، والمتصفح تحقق منها، يظهر القفل بجانب الموقع.

---

# Chain of Trust

المتصفح لا يثق بأي شهادة عشوائية.

هو يبحث عن سلسلة ثقة:

```text
Website Certificate
Intermediate Certificate
Root Certificate
```

مثال:

```text
example.com Certificate
        ↓ signed by
Intermediate CA
        ↓ signed by
Root CA
```

المتصفح يثق في Root CA، ومن خلالها يثق في الشهادة النهائية.

---

# ماذا يفحص المتصفح في الشهادة؟

المتصفح يفحص أشياء كثيرة:

```text
هل اسم الدومين مطابق؟
هل الشهادة انتهت؟
هل الشهادة موقعة من CA موثوقة؟
هل السلسلة صحيحة؟
هل الشهادة تم إلغاؤها؟
هل الخوارزميات قوية؟
```

---

# مشكلة الاسم غير مطابق

لو فتحت:

```text
https://example.com
```

لكن الشهادة صادرة لـ:

```text
another-site.com
```

سيظهر تحذير.

لأن المتصفح لا يستطيع التأكد أن السيرفر هو فعلًا example.com.

---

# مشكلة انتهاء الشهادة

الشهادة لها تاريخ بداية ونهاية.

مثال:

```text
Valid From: 2026-01-01
Valid To:   2026-04-01
```

لو انتهت الشهادة، المتصفح يظهر تحذيرًا.

---

# Public Key و Private Key

في التشفير غير المتماثل، يوجد مفتاحان:

```text
Public Key
Private Key
```

## Public Key

يمكن مشاركته مع الآخرين.

يوجد داخل الشهادة.

## Private Key

يجب أن يبقى سريًا عند السيرفر.

لا يتم إرساله للعميل.

---

# لماذا لا نستخدم Public Key لتشفير كل البيانات؟

التشفير غير المتماثل أبطأ من التشفير المتماثل.

لذلك TLS يستخدمه في البداية للمصادقة وتبادل المفاتيح.

بعد ذلك يستخدم:

```text
Symmetric Encryption
```

لأنه أسرع ومناسب لنقل البيانات الكثيرة.

---

# Symmetric Encryption

في التشفير المتماثل، نفس المفتاح يستخدم للتشفير وفك التشفير.

مثال:

```text
Session Key
```

العميل والسيرفر يتفقان على Session Keys أثناء TLS Handshake.

بعدها:

```text
Client encrypts data with session key
Server decrypts data with session key
```

---

# Asymmetric vs Symmetric Encryption

| المقارنة | Asymmetric | Symmetric |
|---|---|---|
| المفاتيح | Public + Private | نفس المفتاح للطرفين |
| السرعة | أبطأ | أسرع |
| الاستخدام في TLS | المصادقة وتبادل المفاتيح | تشفير البيانات |
| مثال | RSA, ECDSA | AES, ChaCha20 |

---

# Key Exchange

Key Exchange يعني طريقة يتفق بها العميل والسيرفر على مفاتيح الجلسة.

أشهر الطرق الحديثة تعتمد على:

```text
Diffie-Hellman
ECDHE
```

ECDHE تعطي ميزة مهمة اسمها:

```text
Forward Secrecy
```

---

# Forward Secrecy

Forward Secrecy تعني أن تسريب Private Key الخاص بالسيرفر في المستقبل لا يفك تشفير جلسات قديمة تم تسجيلها سابقًا.

مثال:

لو شخص سجل الترافيك اليوم، ثم سرق Private Key بعد سنة، لا يستطيع فك تشفير جلسات اليوم إذا كان الاتصال استخدم Forward Secrecy بشكل صحيح.

---

# TLS 1.2 vs TLS 1.3

## TLS 1.2

TLS 1.2 آمن إذا تم استخدام إعدادات قوية.

لكنه أبطأ نسبيًا في الـ Handshake.

---

## TLS 1.3

TLS 1.3 أحدث وأسرع وأكثر أمانًا.

يحذف خوارزميات قديمة وضعيفة، ويقلل عدد الرسائل المطلوبة في الـ Handshake.

---

# TLS 1.2 Handshake مبسط

```text
ClientHello
ServerHello
Certificate
ServerKeyExchange
ServerHelloDone
ClientKeyExchange
ChangeCipherSpec
Finished
ChangeCipherSpec
Finished
```

بعدها يبدأ الاتصال المشفر.

---

# TLS 1.3 Handshake مبسط

```text
ClientHello
ServerHello
EncryptedExtensions
Certificate
CertificateVerify
Finished
Finished
```

TLS 1.3 أسرع لأن جزءًا كبيرًا من الاتفاق يتم مبكرًا.

---

# 0-RTT في TLS 1.3

TLS 1.3 يدعم ميزة اسمها:

```text
0-RTT
```

في بعض الحالات، العميل يستطيع إرسال بيانات مبكرًا عند الرجوع لنفس السيرفر.

لكن 0-RTT له اعتبارات أمنية، مثل احتمال:

```text
Replay Attacks
```

لذلك لا يستخدم دائمًا لكل أنواع الطلبات.

---

# ماذا يرى شخص يراقب الشبكة؟

مع HTTPS، الشخص الذي يراقب الشبكة قد يرى أشياء مثل:

```text
IP Address للسيرفر
Port 443
حجم البيانات تقريبًا
وقت الاتصال
أحيانًا اسم الدومين عبر SNI إذا لم يكن مشفرًا
```

لكنه لا يرى عادة:

```text
URL الكامل
Cookies
Passwords
HTTP Headers
HTTP Body
محتوى الصفحة
```

---

# هل DNS مشفر؟

ليس دائمًا.

حتى لو الموقع HTTPS، طلب DNS قد يكون غير مشفر إذا كنت تستخدم DNS عادي.

مثال:

```text
DNS Query: what is IP of example.com?
```

لذلك ظهرت تقنيات مثل:

```text
DoH = DNS over HTTPS
DoT = DNS over TLS
```

---

# هل HTTPS يخفي اسم الموقع بالكامل؟

ليس دائمًا.

قد يظهر اسم الموقع من:

```text
DNS Query
SNI
IP Address
```

لكن محتوى الاتصال نفسه يكون مشفرًا.

تقنيات حديثة مثل:

```text
ECH = Encrypted ClientHello
```

تهدف إلى إخفاء معلومات أكثر مثل SNI، لكنها ليست مفعلة في كل مكان.

---

# ما الذي يحميه TLS؟

TLS يحمي:

```text
HTTP Headers
Cookies
Passwords
Form Data
API Requests
Page Content
Tokens
Session Data
```

يعني لو أرسلت كلمة مرور عبر HTTPS صحيح، فهي تكون داخل قناة مشفرة.

---

# ما الذي لا يحميه TLS؟

TLS لا يحمي من كل شيء.

أمثلة:

```text
موقع مزيف لكنه يملك شهادة صحيحة لدومينه
جهازك مصاب ب Malware
المتصفح مخترق
إدخال كلمة مرور في موقع تصيد
تسريب البيانات من السيرفر نفسه
```

يعني القفل في المتصفح لا يعني أن الموقع آمن 100%.

هو يعني أن الاتصال مشفر وأن الشهادة صالحة لهذا الدومين.

---

# Certificate Warning

لو المتصفح أظهر تحذير شهادة، لا تتجاهله.

أسباب التحذير قد تكون:

```text
الشهادة منتهية
الدومين غير مطابق
الشهادة Self-Signed
السلسلة غير موثوقة
محاولة Man-in-the-Middle
تاريخ الجهاز خطأ
```

---

# Self-Signed Certificate

Self-Signed Certificate هي شهادة يوقعها صاحبها بنفسه.

قد تستخدم في:

```text
Labs
Internal Servers
Testing
Development
```

لكن المتصفح لا يثق بها تلقائيًا لأنها ليست موقعة من CA موثوقة.

---

# Man-in-the-Middle و TLS

TLS مصمم لمنع هجمات:

```text
Man-in-the-Middle
```

مثال:

```text
Client ↔ Attacker ↔ Server
```

لو المهاجم حاول تقديم شهادة مزيفة، المتصفح يجب أن يكتشف ذلك ويظهر تحذيرًا.

---

# TLS و Wi-Fi العام

في Wi-Fi عام، قد يستطيع شخص مراقبة الترافيك.

لكن إذا كنت تستخدم HTTPS صحيح، محتوى الاتصال يكون مشفرًا.

مع ذلك، يجب الحذر من:

```text
مواقع HTTP غير مشفرة
تحذيرات الشهادات
شبكات Wi-Fi مزيفة
DNS Spoofing
Captive Portals
```

استخدام VPN قد يساعد في بعض السيناريوهات، لكنه لا يغني عن HTTPS.

---

# TLS و HSTS

HSTS اختصار لـ:

```text
HTTP Strict Transport Security
```

وهي سياسة تجعل المتصفح يستخدم HTTPS دائمًا مع الموقع.

لو موقع يستخدم HSTS، المتصفح يرفض الرجوع إلى HTTP غير المشفر.

هذا يقلل خطر هجمات مثل:

```text
SSL Stripping
```

---

# SSL Stripping

SSL Stripping هجوم يحاول إجبار المستخدم على استخدام HTTP بدل HTTPS.

مثال:

```text
User يريد https://example.com
Attacker يحاول تحويله إلى http://example.com
```

HSTS يساعد في منع هذا النوع من الهجمات.

---

# TLS و Cookies

Cookies الحساسة يجب أن تستخدم خصائص مثل:

```text
Secure
HttpOnly
SameSite
```

## Secure

تعني أن الـ Cookie لا ترسل إلا عبر HTTPS.

## HttpOnly

تمنع JavaScript من قراءة الـ Cookie.

## SameSite

تساعد في تقليل بعض هجمات CSRF.

---

# TLS و HTTP/2

HTTP/2 غالبًا يستخدم مع TLS في المتصفحات.

عند TLS Handshake، يتم استخدام ALPN لاختيار:

```text
h2
```

أي HTTP/2.

مثال:

```text
Client supports: h2, http/1.1
Server selects: h2
```

---

# TLS و HTTP/3

HTTP/3 يستخدم:

```text
QUIC
```

وQUIC يعمل فوق:

```text
UDP
```

وفيه التشفير مدمج بطريقة تعتمد على TLS 1.3.

لذلك في HTTP/3، لا يوجد TCP Handshake بنفس الشكل التقليدي.

---

# HTTPS مع TCP و HTTP/2

الترتيب غالبًا:

```text
DNS
TCP Handshake
TLS Handshake
HTTP/2
```

---

# HTTPS مع HTTP/3

الترتيب غالبًا:

```text
DNS
QUIC Handshake over UDP
TLS 1.3 داخل QUIC
HTTP/3
```

---

# كيف تعرف تفاصيل شهادة موقع؟

في المتصفح:

```text
اضغط على علامة القفل
ثم Connection is secure
ثم Certificate
```

ستجد معلومات مثل:

```text
Issued To
Issued By
Valid From / To
Public Key
Certificate Chain
```

---

# أوامر مفيدة

## OpenSSL

فحص شهادة موقع:

```bash
openssl s_client -connect example.com:443 -servername example.com
```

عرض تفاصيل الشهادة:

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null | openssl x509 -noout -text
```

---

## curl

عرض تفاصيل الاتصال:

```bash
curl -v https://example.com
```

اختبار TLS version:

```bash
curl --tlsv1.3 https://example.com
```

---

## nmap

فحص TLS Ciphers:

```bash
nmap --script ssl-enum-ciphers -p 443 example.com
```

استخدم هذه الأوامر فقط على أنظمة تملك تصريحًا لفحصها.

---

# Wireshark

في Wireshark يمكن رؤية TLS Handshake.

فلاتر مفيدة:

```text
tls
```

```text
tcp.port == 443
```

```text
tls.handshake.type == 1
```

ClientHello نوعه غالبًا:

```text
1
```

ServerHello نوعه غالبًا:

```text
2
```

---

# هل يمكن فك تشفير TLS في Wireshark؟

يمكن في بعض الحالات إذا كنت تملك مفاتيح الجلسة من جهازك.

مثال باستخدام:

```text
SSLKEYLOGFILE
```

لكن بدون مفاتيح الجلسة، لا يمكن قراءة المحتوى المشفر بسهولة.

---

# Troubleshooting TLS

## المشكلة 1: Certificate Expired

المتصفح يظهر أن الشهادة انتهت.

الحل:

```text
تجديد الشهادة على السيرفر
التأكد من تاريخ ووقت الجهاز
```

---

## المشكلة 2: Name Mismatch

الشهادة ليست لنفس الدومين.

مثال:

```text
تدخل إلى example.com
والشهادة تخص test.com
```

الحل:

```text
استخدام شهادة صحيحة للدومين
ضبط SNI بشكل صحيح
```

---

## المشكلة 3: Untrusted CA

الشهادة موقعة من جهة غير موثوقة.

الحل:

```text
استخدام شهادة من CA موثوقة
أو تثبيت Root CA داخلي في بيئات الشركات
```

---

## المشكلة 4: TLS Version غير مدعوم

عميل قديم يحاول الاتصال بسيرفر لا يدعم إلا TLS حديث.

أو سيرفر قديم لا يدعم TLS حديث.

الحل:

```text
تحديث المتصفح أو النظام
تحديث إعدادات السيرفر
تفعيل TLS 1.2 أو TLS 1.3 حسب الحاجة
```

---

## المشكلة 5: Cipher Suite غير متوافق

العميل والسيرفر لا يجدان Cipher Suite مشتركة.

الحل:

```text
ضبط Ciphers قوية ومتوافقة
تحديث العميل أو السيرفر
```

---

# أخطاء شائعة

## الخطأ الأول

```text
HTTPS يعني الموقع آمن دائمًا
```

غير صحيح.

HTTPS يعني أن الاتصال مشفر وأن الشهادة صالحة للدومين، لكنه لا يضمن أن الموقع نفسه صادق أو خالي من الاحتيال.

---

## الخطأ الثاني

```text
TLS يشفر IP Address
```

غير صحيح.

IP Address لازم يظهر حتى تصل البيانات للسيرفر.

TLS يشفر محتوى الاتصال، وليس عنوان IP.

---

## الخطأ الثالث

```text
Certificate تحتوي على Private Key
```

غير صحيح.

الشهادة تحتوي على Public Key.

Private Key يبقى سريًا عند السيرفر.

---

## الخطأ الرابع

```text
DNS دائمًا مشفر مع HTTPS
```

غير صحيح.

HTTPS لا يعني أن DNS مشفر.

DNS يحتاج DoH أو DoT أو تقنيات مشابهة حتى يكون مشفرًا.

---

## الخطأ الخامس

```text
لو عندي VPN لا أحتاج HTTPS
```

غير صحيح.

VPN يحمي جزءًا من الطريق، لكن HTTPS ما زال مهمًا جدًا بين المتصفح والموقع.

---

# مثال عملي كامل: فتح موقع HTTPS

أنت كتبت:

```text
https://example.com
```

الخطوات:

```text
1. المتصفح يحتاج IP الخاص بـ example.com
2. يتم DNS Query
3. الجهاز يحدد أن الوجهة خارج الشبكة
4. يستخدم ARP لمعرفة MAC الخاص بالـ Gateway
5. يبدأ TCP 3-Way Handshake مع example.com على Port 443
6. يبدأ TLS Handshake
7. السيرفر يرسل Certificate
8. المتصفح يتحقق من الشهادة
9. الطرفان يتفقان على Session Keys
10. يبدأ الاتصال المشفر
11. المتصفح يرسل HTTP Request داخل TLS
12. السيرفر يرسل HTTP Response داخل TLS
```

---

# ماذا يظهر في Packet Capture؟

قد ترى:

```text
DNS Query
TCP SYN
TCP SYN-ACK
TCP ACK
TLS ClientHello
TLS ServerHello
TLS Certificate
TLS Finished
Encrypted Application Data
```

بعد انتهاء الـ Handshake، ستظهر البيانات باسم:

```text
Application Data
```

لكن محتواها مشفر.

---

# ملخص سريع

```text
TLS = Transport Layer Security
HTTPS = HTTP over TLS
TLS يحدث بعد TCP وقبل HTTP
TLS يوفر Encryption و Authentication و Integrity
Certificate تثبت هوية السيرفر
CA تصدر وتوقع الشهادات
Public Key داخل الشهادة
Private Key يبقى سريًا
Session Keys تستخدم لتشفير البيانات
TLS 1.3 أحدث وأسرع من TLS 1.2
SNI يحدد اسم الموقع المطلوب
ALPN يحدد بروتوكول التطبيق مثل HTTP/2
```

---

# أهم القواعد

```text
TCP Handshake أولًا
TLS Handshake ثانيًا
HTTP Request بعد ذلك داخل TLS
```

```text
HTTPS لا يعني أن الموقع صادق 100%
لكنه يعني أن الاتصال مشفر والشهادة صالحة للدومين
```

```text
لا تتجاهل Certificate Warnings
```

```text
Private Key لا يتم إرساله للعميل
```

```text
TLS يشفر المحتوى وليس IP Address
```

```text
DNS قد يبقى غير مشفر حتى لو الموقع HTTPS
```

---

## Next Topic

اقرأ بعد ذلك:

[HTTP and HTTPS Request Lifecycle](./HTTP-and-HTTPS-Request-Lifecycle.md)
