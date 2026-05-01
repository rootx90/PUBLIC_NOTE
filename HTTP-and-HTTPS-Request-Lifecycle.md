# HTTP and HTTPS Request Lifecycle

## ماذا يحدث عندما تفتح موقعًا؟

عندما تكتب في المتصفح:

```text
https://example.com
```

وتضغط Enter، لا يتم تحميل الصفحة مباشرة.

وراء هذه اللحظة تحدث خطوات كثيرة جدًا في الشبكة.

بشكل عام:

```text
DNS
ARP
TCP
TLS
HTTP
Server Processing
HTTP Response
Rendering
```

هذا الدرس يجمع أغلب ما تعلمناه في الملفات السابقة في رحلة واحدة كاملة.

---

# الفكرة العامة

المتصفح يحتاج أن يعرف:

```text
1. ما هو IP الخاص بالموقع؟
2. كيف أصل إلى هذا IP؟
3. هل أحتاج اتصال TCP؟
4. هل الاتصال مشفر؟
5. ما هو الطلب الذي سأرسله؟
6. ماذا سيرد السيرفر؟
```

---

# URL Example

لو كتبت:

```text
https://www.example.com/products?id=10
```

هذا الرابط يحتوي على أجزاء مهمة:

```text
https          = Scheme / Protocol
www.example.com = Domain Name
/products     = Path
id=10         = Query String
```

---

# HTTP vs HTTPS

## HTTP

HTTP اختصار لـ:

```text
HyperText Transfer Protocol
```

غالبًا يستخدم:

```text
TCP Port 80
```

ولا يكون مشفرًا افتراضيًا.

---

## HTTPS

HTTPS يعني:

```text
HTTP over TLS
```

غالبًا يستخدم:

```text
TCP Port 443
```

ويكون الاتصال مشفرًا باستخدام TLS.

---

# الفرق الأساسي

```text
HTTP  = بيانات واضحة غالبًا
HTTPS = HTTP داخل قناة مشفرة
```

في HTTPS، المتصفح لا يرسل HTTP Request مباشرة بعد TCP.

بل يحدث أولًا:

```text
TLS Handshake
```

ثم يتم إرسال HTTP Request داخل TLS.

---

# رحلة فتح موقع HTTPS

عندما تفتح:

```text
https://example.com
```

غالبًا تحدث هذه الخطوات:

```text
1. Browser يفحص Cache
2. DNS يحول domain إلى IP
3. الجهاز يحدد هل IP داخل الشبكة أم خارجها
4. ARP لمعرفة MAC الخاص بالـ Gateway أو الوجهة
5. TCP 3-Way Handshake مع السيرفر
6. TLS Handshake لتشفير الاتصال
7. HTTP Request داخل TLS
8. Server يعالج الطلب
9. HTTP Response يرجع للمتصفح
10. المتصفح يعرض الصفحة
```

---

# رسم عام

```text
User
 │
 ▼
Browser
 │
 │ DNS Query
 ▼
DNS Resolver
 │
 │ IP Address
 ▼
Browser
 │
 │ TCP Handshake
 ▼
Web Server
 │
 │ TLS Handshake
 ▼
Secure Connection
 │
 │ HTTP Request
 ▼
Application Server
 │
 │ HTTP Response
 ▼
Browser Rendering
```

---

# Step 1: Browser Cache

قبل أن يسأل المتصفح الشبكة، يفحص أشياء كثيرة.

مثال:

```text
هل الصفحة محفوظة في Cache؟
هل DNS محفوظ؟
هل هناك اتصال مفتوح بالفعل؟
هل هناك HSTS يجبر HTTPS؟
```

لو المتصفح عنده نسخة صالحة من الملف في Cache، قد لا يطلبه من السيرفر مرة أخرى.

---

# Step 2: DNS Resolution

المتصفح يحتاج IP الخاص بالدومين.

مثال:

```text
example.com → 93.184.216.34
```

لو DNS Cache لا يحتوي على الإجابة، يتم سؤال DNS Resolver.

النتيجة:

```text
Domain Name becomes IP Address
```

---

# Step 3: تحديد هل الوجهة داخل الشبكة أم خارجها

الجهاز يقارن Destination IP مع شبكته المحلية.

مثال:

```text
My IP      = 192.168.1.10/24
Target IP  = 93.184.216.34
Gateway    = 192.168.1.1
```

بما أن الهدف خارج الشبكة المحلية، الجهاز سيرسل البيانات إلى:

```text
Default Gateway
```

---

# Step 4: ARP

الجهاز يحتاج MAC Address للخطوة التالية.

لو الوجهة خارج الشبكة:

```text
ARP يسأل عن MAC الخاص بالـ Default Gateway
```

مثال:

```text
Who has 192.168.1.1?
```

الراوتر يرد:

```text
192.168.1.1 is at AA:BB:CC:DD:EE:FF
```

الآن الجهاز يستطيع إرسال Frame إلى الراوتر.

---

# Step 5: TCP 3-Way Handshake

بما أن HTTPS التقليدي يستخدم TCP، يجب إنشاء اتصال TCP أولًا.

الخطوات:

```text
Client → Server: SYN
Server → Client: SYN-ACK
Client → Server: ACK
```

بعدها يصبح الاتصال:

```text
ESTABLISHED
```

مثال اتصال:

```text
192.168.1.10:51544 → 93.184.216.34:443 TCP
```

---

# Step 6: TLS Handshake

لأن الرابط يبدأ بـ:

```text
https://
```

يحدث TLS Handshake بعد TCP.

TLS يتفق على:

```text
TLS Version
Cipher Suite
Certificate
Session Keys
ALPN
SNI
```

بعد نجاح TLS، يصبح الاتصال مشفرًا.

---

# Step 7: HTTP Request

بعد أن يصبح الاتصال جاهزًا ومشفرًا، يرسل المتصفح HTTP Request.

مثال:

```http
GET /products?id=10 HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Accept-Language: en-US,en;q=0.9
Cookie: sessionid=abc123
```

في HTTPS، هذا الطلب يكون داخل TLS، لذلك لا يظهر واضحًا لمن يراقب الشبكة.

---

# ما هو HTTP Request؟

HTTP Request هو رسالة من العميل إلى السيرفر يطلب فيها شيئًا.

قد يطلب:

```text
صفحة HTML
صورة
ملف CSS
ملف JavaScript
API Data
تحميل ملف
```

---

# أجزاء HTTP Request

HTTP Request يتكون غالبًا من:

```text
Request Line
Headers
Body اختياري
```

---

## Request Line

مثال:

```http
GET /products?id=10 HTTP/1.1
```

وتحتوي على:

```text
Method
Path
HTTP Version
```

---

## Headers

Headers هي معلومات إضافية عن الطلب.

مثال:

```http
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: sessionid=abc123
```

---

## Body

Body يوجد غالبًا في طلبات مثل:

```text
POST
PUT
PATCH
```

مثال Login Request:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "mahmoud",
  "password": "secret"
}
```

---

# HTTP Methods

أشهر HTTP Methods:

```text
GET
POST
PUT
PATCH
DELETE
HEAD
OPTIONS
```

---

## GET

يستخدم لجلب بيانات.

مثال:

```http
GET /articles HTTP/1.1
```

غالبًا لا يحتوي على Body.

---

## POST

يستخدم لإرسال بيانات أو إنشاء شيء جديد.

مثال:

```http
POST /login HTTP/1.1
```

أو:

```http
POST /users HTTP/1.1
```

---

## PUT

يستخدم غالبًا لتحديث مورد كامل.

مثال:

```http
PUT /users/10 HTTP/1.1
```

---

## PATCH

يستخدم لتحديث جزء من مورد.

مثال:

```http
PATCH /users/10 HTTP/1.1
```

---

## DELETE

يستخدم لحذف مورد.

مثال:

```http
DELETE /users/10 HTTP/1.1
```

---

## HEAD

يشبه GET لكن بدون Body في الرد.

يستخدم لمعرفة معلومات عن المورد.

---

## OPTIONS

يستخدم لمعرفة ما يسمح به السيرفر.

يظهر كثيرًا في CORS.

---

# Important Headers

## Host

```http
Host: www.example.com
```

يخبر السيرفر أي موقع يريده العميل.

مهم جدًا لأن نفس IP قد يستضيف أكثر من موقع.

---

## User-Agent

```http
User-Agent: Mozilla/5.0
```

يخبر السيرفر عن المتصفح أو العميل.

---

## Accept

```http
Accept: text/html,application/json
```

يخبر السيرفر بأنواع المحتوى التي يقبلها العميل.

---

## Content-Type

```http
Content-Type: application/json
```

يخبر السيرفر بنوع البيانات الموجودة في Body.

---

## Authorization

```http
Authorization: Bearer token_here
```

يستخدم لإرسال بيانات الدخول أو التوكن.

---

## Cookie

```http
Cookie: sessionid=abc123
```

يرسل Cookies محفوظة من الموقع.

---

# Step 8: Server Processing

السيرفر يستقبل الطلب ويفهمه.

قد تمر الرحلة داخل السيرفر عبر مكونات كثيرة:

```text
Load Balancer
Reverse Proxy
Web Server
Application Server
Database
Cache
File Storage
```

مثال:

```text
Nginx → Node.js / PHP / Python / Java → Database
```

---

# Reverse Proxy

Reverse Proxy يستقبل الطلبات قبل التطبيق.

أمثلة:

```text
Nginx
Apache
HAProxy
Traefik
Cloudflare
```

يمكن أن يقوم بـ:

```text
TLS Termination
Load Balancing
Caching
Compression
Security Filtering
Routing
```

---

# Load Balancer

Load Balancer يوزع الطلبات على أكثر من سيرفر.

مثال:

```text
Request 1 → Server A
Request 2 → Server B
Request 3 → Server C
```

الفائدة:

```text
تحمل ضغط أكبر
تقليل الأعطال
تحسين الأداء
```

---

# Application Server

Application Server هو الجزء الذي يحتوي على منطق التطبيق.

مثال:

```text
تسجيل الدخول
عرض حساب المستخدم
البحث في المنتجات
إرسال رسالة
قراءة بيانات من Database
```

---

# Database

لو الصفحة تحتاج بيانات، التطبيق قد يسأل Database.

أمثلة:

```text
MySQL
PostgreSQL
MongoDB
Redis
```

مثال:

```sql
SELECT * FROM products WHERE id = 10;
```

---

# Step 9: HTTP Response

بعد معالجة الطلب، السيرفر يرسل HTTP Response.

مثال:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1250
Set-Cookie: sessionid=abc123; Secure; HttpOnly

<html>
  <body>Hello</body>
</html>
```

---

# أجزاء HTTP Response

HTTP Response يتكون من:

```text
Status Line
Headers
Body
```

---

## Status Line

مثال:

```http
HTTP/1.1 200 OK
```

تحتوي على:

```text
HTTP Version
Status Code
Status Text
```

---

## Response Headers

مثال:

```http
Content-Type: text/html
Cache-Control: max-age=3600
Set-Cookie: sessionid=abc123
```

---

## Response Body

هو المحتوى نفسه.

قد يكون:

```text
HTML
CSS
JavaScript
JSON
Image
PDF
Video
```

---

# HTTP Status Codes

Status Code يوضح نتيجة الطلب.

الفئات الرئيسية:

```text
1xx = Informational
2xx = Success
3xx = Redirection
4xx = Client Error
5xx = Server Error
```

---

# 2xx Success

## 200 OK

الطلب نجح.

```http
HTTP/1.1 200 OK
```

---

## 201 Created

تم إنشاء مورد جديد.

مثال:

```text
إنشاء حساب جديد
إنشاء مقال جديد
```

---

## 204 No Content

الطلب نجح لكن لا يوجد Body في الرد.

---

# 3xx Redirection

## 301 Moved Permanently

تحويل دائم.

مثال:

```text
http://example.com → https://example.com
```

---

## 302 Found

تحويل مؤقت.

---

## 304 Not Modified

المتصفح لديه نسخة صالحة في Cache، والسيرفر يقول:

```text
الملف لم يتغير
استخدم النسخة الموجودة عندك
```

---

# 4xx Client Error

## 400 Bad Request

الطلب غير صحيح.

---

## 401 Unauthorized

تحتاج تسجيل دخول أو Authorization.

---

## 403 Forbidden

أنت معروف، لكن ليس لديك صلاحية.

---

## 404 Not Found

المورد غير موجود.

مثال:

```text
/page-not-exist
```

---

## 429 Too Many Requests

تم إرسال طلبات كثيرة جدًا.

غالبًا بسبب Rate Limiting.

---

# 5xx Server Error

## 500 Internal Server Error

خطأ عام داخل السيرفر.

---

## 502 Bad Gateway

Proxy أو Gateway استلم ردًا غير صحيح من السيرفر الخلفي.

---

## 503 Service Unavailable

الخدمة غير متاحة مؤقتًا.

قد يكون بسبب ضغط أو صيانة.

---

## 504 Gateway Timeout

Proxy أو Gateway انتظر ردًا من السيرفر الخلفي ولم يصل في الوقت المحدد.

---

# Cookies

Cookie هي بيانات صغيرة يحفظها المتصفح ويرسلها مع الطلبات القادمة لنفس الموقع.

مثال:

```http
Set-Cookie: sessionid=abc123; Secure; HttpOnly
```

بعد ذلك يرسل المتصفح:

```http
Cookie: sessionid=abc123
```

---

# لماذا نحتاج Cookies؟

HTTP في الأصل Stateless.

يعني كل Request مستقل.

بدون Cookies، السيرفر لا يعرف أن الطلب الجديد من نفس المستخدم السابق.

Cookies تساعد في:

```text
Login Sessions
Preferences
Shopping Cart
Tracking
CSRF Protection
```

---

# Cookie Security Flags

## Secure

```text
Secure
```

تعني أن Cookie لا ترسل إلا عبر HTTPS.

---

## HttpOnly

```text
HttpOnly
```

تمنع JavaScript من قراءة Cookie.

تفيد في تقليل خطر سرقة الجلسة عبر XSS.

---

## SameSite

```text
SameSite=Lax
SameSite=Strict
SameSite=None
```

تساعد في تقليل هجمات CSRF.

---

# Caching

Caching يعني حفظ نسخة من الملفات لتقليل التحميل وتسريع الموقع.

قد يحدث في:

```text
Browser
Proxy
CDN
Server
```

---

# Cache-Control

Header مهم للتحكم في الكاش:

```http
Cache-Control: max-age=3600
```

معناه:

```text
يمكن حفظ الملف لمدة 3600 ثانية
```

---

# ETag

ETag يستخدم لمعرفة هل الملف تغير أم لا.

مثال:

```http
ETag: "abc123"
```

المتصفح قد يرسل لاحقًا:

```http
If-None-Match: "abc123"
```

لو الملف لم يتغير، السيرفر يرد:

```http
304 Not Modified
```

---

# Compression

السيرفر قد يضغط الرد لتقليل الحجم.

أمثلة:

```text
gzip
br
deflate
```

العميل يرسل:

```http
Accept-Encoding: gzip, br
```

والسيرفر يرد:

```http
Content-Encoding: br
```

---

# CDN

CDN اختصار لـ:

```text
Content Delivery Network
```

وهو شبكة سيرفرات موزعة عالميًا.

تستخدم لتسريع تحميل الملفات مثل:

```text
Images
CSS
JavaScript
Videos
Static Files
```

مثال:

```text
User in Egypt gets files from nearby CDN server
instead of far origin server
```

---

# HTTP/1.1

HTTP/1.1 يستخدم TCP.

مميزات:

```text
Persistent Connections
Host Header
Chunked Transfer
```

لكن فيه قيود مثل أن الطلبات على نفس الاتصال قد تتأثر بالانتظار.

---

# HTTP/2

HTTP/2 غالبًا يعمل فوق TLS في المتصفحات.

مميزاته:

```text
Multiplexing
Header Compression
Binary Framing
Better performance
```

Multiplexing يعني إرسال أكثر من طلب ورد على نفس الاتصال في نفس الوقت تقريبًا.

---

# HTTP/3

HTTP/3 يستخدم:

```text
QUIC
```

وQUIC يعمل فوق:

```text
UDP
```

HTTP/3 يحاول تقليل التأخير وتحسين الأداء، خصوصًا مع الشبكات التي فيها فقد أو تغير اتصال.

---

# مقارنة HTTP Versions

| المقارنة | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| النقل | TCP | TCP | QUIC over UDP |
| Multiplexing | محدود | نعم | نعم |
| التشفير | اختياري نظريًا | غالبًا TLS | مدمج مع QUIC |
| الأداء | جيد | أفضل | أفضل في حالات كثيرة |

---

# What Happens with HTTP Only?

لو فتحت:

```text
http://example.com
```

غالبًا يحدث:

```text
1. DNS
2. ARP
3. TCP Handshake على Port 80
4. HTTP Request واضح
5. HTTP Response واضح
```

لا يوجد TLS.

يعني شخص على نفس الشبكة قد يستطيع رؤية أو تعديل البيانات بسهولة إذا لم توجد حماية أخرى.

---

# What Happens with HTTPS?

لو فتحت:

```text
https://example.com
```

يحدث:

```text
1. DNS
2. ARP
3. TCP Handshake على Port 443
4. TLS Handshake
5. HTTP Request مشفر
6. HTTP Response مشفر
```

---

# ماذا يرى المراقب في HTTP؟

في HTTP العادي، المراقب قد يرى:

```text
URL
Headers
Cookies
Body
Passwords
Forms
Response Content
```

لذلك HTTP غير مناسب لتسجيل الدخول أو البيانات الحساسة.

---

# ماذا يرى المراقب في HTTPS؟

في HTTPS، المراقب قد يرى غالبًا:

```text
IP Address
Port 443
حجم الترافيك تقريبًا
وقت الاتصال
DNS Query إذا لم يكن مشفرًا
SNI أحيانًا
```

لكنه لا يرى عادة:

```text
Full URL
Cookies
Passwords
HTTP Body
HTTP Headers
Page Content
```

---

# Mixed Content

Mixed Content يحدث عندما تكون الصفحة HTTPS لكنها تحمل بعض الموارد عبر HTTP.

مثال:

```html
<img src="http://example.com/image.png">
```

هذا خطر لأن جزءًا من الصفحة غير مشفر.

المتصفحات الحديثة تمنع أو تحذر من Mixed Content.

---

# HSTS

HSTS اختصار لـ:

```text
HTTP Strict Transport Security
```

يجعل المتصفح يستخدم HTTPS دائمًا مع الموقع.

Header مثال:

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

الفائدة:

```text
منع الرجوع إلى HTTP
تقليل خطر SSL Stripping
```

---

# Redirect من HTTP إلى HTTPS

كثير من المواقع عندما تدخل:

```text
http://example.com
```

ترد بـ Redirect إلى:

```text
https://example.com
```

مثال:

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.com/
```

---

# Same-Origin Policy

المتصفح يطبق سياسة أمنية اسمها:

```text
Same-Origin Policy
```

الفكرة أن صفحة من موقع معين لا تستطيع قراءة بيانات موقع آخر بسهولة.

Origin يتكون من:

```text
Scheme
Host
Port
```

مثال:

```text
https://example.com:443
```

---

# CORS

CORS اختصار لـ:

```text
Cross-Origin Resource Sharing
```

وهو نظام يسمح لموقع أن يطلب موارد من موقع آخر إذا السيرفر سمح بذلك.

مثال Header:

```http
Access-Control-Allow-Origin: https://app.example.com
```

لو الإعداد خطأ، المتصفح قد يمنع الطلب.

---

# API Requests

الكثير من المواقع الحديثة تعتمد على API.

مثال:

```http
GET /api/products HTTP/1.1
Host: example.com
Accept: application/json
```

الرد يكون JSON:

```json
{
  "id": 10,
  "name": "Laptop",
  "price": 500
}
```

---

# Authentication في HTTP

طرق شائعة:

```text
Cookies
Bearer Tokens
Basic Auth
OAuth
API Keys
```

مثال Bearer Token:

```http
Authorization: Bearer eyJhbGciOi...
```

---

# Sessions

عند تسجيل الدخول، السيرفر قد ينشئ Session.

ثم يرسل Cookie:

```http
Set-Cookie: sessionid=abc123; Secure; HttpOnly
```

المتصفح يرسل Cookie في الطلبات التالية.

هكذا يعرف السيرفر المستخدم.

---

# REST APIs

REST أسلوب شائع لتصميم APIs.

مثال:

```http
GET /users
GET /users/10
POST /users
PUT /users/10
DELETE /users/10
```

يستخدم HTTP Methods بوضوح للتعامل مع الموارد.

---

# Idempotent Methods

بعض HTTP Methods تسمى Idempotent.

يعني تكرار نفس الطلب يعطي نفس النتيجة تقريبًا.

أمثلة:

```text
GET
PUT
DELETE
```

أما POST غالبًا ليس Idempotent.

مثال:

```text
POST /orders
```

لو كررته مرتين، قد ينشئ طلبين شراء.

---

# Safe Methods

Safe Method يعني أنه لا يجب أن يغير حالة السيرفر.

أشهر مثال:

```text
GET
HEAD
OPTIONS
```

GET يجب أن يستخدم لجلب البيانات، وليس لحذف أو تعديل بيانات.

---

# Request Lifecycle داخل المتصفح

بعد استلام HTML، المتصفح يبدأ تحليل الصفحة.

قد يجد ملفات إضافية:

```html
<link rel="stylesheet" href="/style.css">
<script src="/app.js"></script>
<img src="/logo.png">
```

فيبدأ إرسال طلبات أخرى لهذه الملفات.

يعني تحميل صفحة واحدة قد ينتج عنه عشرات أو مئات HTTP Requests.

---

# Rendering

المتصفح يستخدم:

```text
HTML
CSS
JavaScript
Images
Fonts
```

حتى يبني الصفحة المعروضة.

الخطوات بشكل مبسط:

```text
Parse HTML
Build DOM
Load CSS
Build CSSOM
Run JavaScript
Layout
Paint
Composite
```

---

# Waterfall في DevTools

في Developer Tools داخل المتصفح، يمكن رؤية Network Waterfall.

سترى لكل Request:

```text
DNS
Initial Connection
TLS
Request Sent
Waiting TTFB
Content Download
```

---

# TTFB

TTFB اختصار لـ:

```text
Time To First Byte
```

يعني الوقت من إرسال الطلب حتى وصول أول بايت من الرد.

لو TTFB عالي، قد تكون المشكلة في:

```text
Server Processing
Database
Network Latency
Backend Delay
CDN Miss
```

---

# Common Performance Factors

أداء الموقع يتأثر بـ:

```text
DNS Latency
TCP Handshake Time
TLS Handshake Time
Server Response Time
File Size
Compression
Caching
CDN
Number of Requests
JavaScript Execution
```

---

# Troubleshooting Website Loading

لو الموقع لا يفتح، افحص بالترتيب:

```text
1. DNS
2. Ping أو Reachability
3. TCP Port
4. TLS Certificate
5. HTTP Status Code
6. Server Logs
7. Browser Console
8. Firewall / Proxy
```

---

# أوامر مفيدة

## DNS

```bash
nslookup example.com
dig example.com
```

---

## TCP Port

```bash
nc -vz example.com 443
```

على Windows:

```powershell
Test-NetConnection example.com -Port 443
```

---

## HTTP Headers

```bash
curl -I https://example.com
```

---

## HTTP Verbose

```bash
curl -v https://example.com
```

---

## Follow Redirects

```bash
curl -L https://example.com
```

---

## Test HTTP Status

```bash
curl -o /dev/null -s -w "%{http_code}\n" https://example.com
```

---

## OpenSSL TLS Test

```bash
openssl s_client -connect example.com:443 -servername example.com
```

---

# أمثلة Curl

## GET Request

```bash
curl https://example.com
```

---

## POST JSON

```bash
curl -X POST https://example.com/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"mahmoud","password":"secret"}'
```

---

## Send Header

```bash
curl https://example.com \
  -H "Authorization: Bearer TOKEN_HERE"
```

---

# DevTools

في المتصفح افتح Developer Tools ثم Network.

يمكنك رؤية:

```text
Request URL
Request Method
Status Code
Remote Address
Response Headers
Request Headers
Cookies
Timing
Response Body
```

هذه أداة مهمة جدًا لفهم HTTP.

---

# مثال عملي كامل

أنت تفتح:

```text
https://shop.example.com/products?id=10
```

الخطوات:

```text
1. المتصفح يفحص Cache و HSTS
2. DNS يحول shop.example.com إلى IP
3. الجهاز يقرر أن IP خارج الشبكة
4. ARP لمعرفة MAC الخاص بالـ Gateway
5. TCP Handshake مع IP السيرفر على Port 443
6. TLS Handshake مع SNI = shop.example.com
7. المتصفح يتحقق من Certificate
8. يتم الاتفاق على Session Keys
9. المتصفح يرسل HTTP Request مشفر
10. Reverse Proxy يستقبل الطلب
11. Application Server يعالج /products?id=10
12. التطبيق يسأل Database عن المنتج
13. السيرفر يرسل HTTP Response
14. المتصفح يستقبل HTML
15. المتصفح يطلب CSS و JS و Images
16. المتصفح يبني الصفحة ويعرضها
```

---

# مثال Request و Response

## Request

```http
GET /products?id=10 HTTP/1.1
Host: shop.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: sessionid=abc123
```

## Response

```http
HTTP/1.1 200 OK
Content-Type: text/html
Cache-Control: no-cache
Set-Cookie: tracking=xyz; Secure; SameSite=Lax

<html>
  <head>
    <title>Product</title>
  </head>
  <body>
    <h1>Laptop</h1>
  </body>
</html>
```

---

# HTTP Security Headers

بعض Headers تساعد في الأمان.

أمثلة:

```http
Strict-Transport-Security: max-age=31536000
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Referrer-Policy: no-referrer
```

---

# Content-Security-Policy

CSP تساعد على تقليل خطر XSS.

مثال:

```http
Content-Security-Policy: default-src 'self'
```

يعني المتصفح يسمح بتحميل الموارد من نفس الموقع فقط إلا لو تم السماح بغير ذلك.

---

# X-Frame-Options

يساعد في منع Clickjacking.

مثال:

```http
X-Frame-Options: DENY
```

يعني لا تسمح بعرض الصفحة داخل iframe.

---

# X-Content-Type-Options

مثال:

```http
X-Content-Type-Options: nosniff
```

يمنع المتصفح من تخمين نوع الملف بطريقة قد تكون خطيرة.

---

# Referrer-Policy

يتحكم في مقدار المعلومات التي يرسلها المتصفح في Header اسمه:

```http
Referer
```

مثال:

```http
Referrer-Policy: no-referrer
```

---

# Common HTTP Problems

## 404 Not Found

الصفحة أو المسار غير موجود.

افحص:

```text
URL
Routing
File Path
Reverse Proxy Rules
```

---

## 500 Internal Server Error

خطأ داخل التطبيق أو السيرفر.

افحص:

```text
Application Logs
Database
Permissions
Environment Variables
Backend Errors
```

---

## 502 Bad Gateway

غالبًا Reverse Proxy لا يستطيع التواصل مع Backend.

افحص:

```text
Backend running?
Port صحيح؟
Proxy config صحيح؟
Firewall؟
```

---

## 503 Service Unavailable

الخدمة غير متاحة.

قد يكون السبب:

```text
Maintenance
Overload
Backend Down
Rate Limit
```

---

## 504 Gateway Timeout

Proxy انتظر Backend طويلًا ولم يحصل على رد.

افحص:

```text
Slow Database Query
Backend Timeout
Network Issue
High Load
```

---

# أخطاء شائعة

## الخطأ الأول

```text
DNS يجلب صفحة الموقع
```

غير صحيح.

DNS فقط يحول الاسم إلى IP.

---

## الخطأ الثاني

```text
HTTPS يحدث قبل TCP
```

غير صحيح في HTTPS التقليدي.

TCP Handshake يحدث أولًا، ثم TLS Handshake.

---

## الخطأ الثالث

```text
Port 443 يعني أن كل شيء آمن
```

ليس دائمًا.

يجب أن تكون شهادة TLS صحيحة، والإعدادات آمنة، والموقع نفسه موثوق.

---

## الخطأ الرابع

```text
404 معناها السيرفر لا يعمل
```

غير صحيح.

404 يعني السيرفر رد، لكن المسار غير موجود.

---

## الخطأ الخامس

```text
500 مشكلة من المستخدم
```

غالبًا 500 مشكلة في السيرفر أو التطبيق.

---

# ملخص سريع

```text
DNS = Domain إلى IP
ARP = IP محلي إلى MAC
TCP = إنشاء اتصال موثوق
TLS = تشفير الاتصال
HTTP = طلب ورد بين المتصفح والسيرفر
HTTPS = HTTP داخل TLS
Status Code = نتيجة الطلب
Headers = معلومات إضافية
Body = محتوى الطلب أو الرد
Cookies = حفظ حالة المستخدم
Cache = تسريع وتقليل الطلبات
CDN = تقريب المحتوى للمستخدم
```

---

# أهم القواعد

```text
HTTP Request لا يحدث إلا بعد معرفة IP والوصول للسيرفر
```

```text
في HTTPS التقليدي:
DNS → TCP → TLS → HTTP
```

```text
DNS لا ينقل محتوى الصفحة
```

```text
TLS يشفر HTTP Headers و Body
```

```text
Status Code يساعدك تفهم نتيجة الطلب
```

```text
404 = المورد غير موجود
500 = خطأ في السيرفر
301/302 = Redirect
200 = نجاح
```

```text
المتصفح قد يرسل عشرات الطلبات لتحميل صفحة واحدة
```

---

## Navigation

| Previous | Home | Next |
|---|---|---|
| [TLS Handshake and Encryption](./TLS-Handshake-and-Encryption.md) | [README](./README.md) | End |
