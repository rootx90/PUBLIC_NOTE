# Network Troubleshooting Commands

## أوامر تشخيص مشاكل الشبكة

هذا الملف يجمع أهم الأوامر التي تستخدمها عندما تواجه مشكلة في الشبكة.

مثال مشاكل:

```text
الإنترنت لا يعمل
الموقع لا يفتح
DNS لا يعمل
الجهاز لا يأخذ IP
البورت مغلق
Ping لا يرد
Wi-Fi متصل لكن لا يوجد إنترنت
```

---

# ترتيب التشخيص الصحيح

عندما تواجه مشكلة، لا تبدأ عشوائيًا.

اتبع هذا الترتيب:

```text
1. افحص كرت الشبكة
2. افحص IP Address
3. افحص Default Gateway
4. افحص DNS
5. افحص Ping
6. افحص Route
7. افحص Port
8. افحص Firewall
9. افحص HTTP/TLS
```

---

# 1. فحص IP Address

## Windows

```cmd
ipconfig
```

تفاصيل أكثر:

```cmd
ipconfig /all
```

---

## Linux

```bash
ip addr
```

أو:

```bash
ifconfig
```

---

## macOS

```bash
ifconfig
```

---

# ماذا تفحص في IP؟

ابحث عن:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
DHCP Enabled
MAC Address
```

مثال جيد:

```text
IP Address      = 192.168.1.10
Subnet Mask     = 255.255.255.0
Default Gateway = 192.168.1.1
DNS Server      = 8.8.8.8
```

---

# مشكلة APIPA

لو وجدت IP مثل:

```text
169.254.x.x
```

فهذا غالبًا يعني أن الجهاز لم يحصل على IP من DHCP.

أسباب ممكنة:

```text
DHCP Server لا يعمل
الكابل مفصول
Wi-Fi فيه مشكلة
VLAN خطأ
DHCP Pool انتهى
```

---

# 2. تجديد IP من DHCP

## Windows

إزالة IP الحالي:

```cmd
ipconfig /release
```

طلب IP جديد:

```cmd
ipconfig /renew
```

---

## Linux

```bash
sudo dhclient -r
sudo dhclient
```

---

# 3. فحص الاتصال المحلي

أول اختبار:

```cmd
ping 127.0.0.1
```

هذا يفحص TCP/IP Stack داخل جهازك.

بعدها افحص IP جهازك:

```cmd
ping 192.168.1.10
```

ثم افحص الراوتر:

```cmd
ping 192.168.1.1
```

---

# 4. فحص الإنترنت بدون DNS

استخدم IP مباشر مثل:

```cmd
ping 8.8.8.8
```

لو هذا يعمل، فالإنترنت غالبًا موجود.

لو:

```cmd
ping 8.8.8.8
```

يعمل لكن:

```cmd
ping google.com
```

لا يعمل، فالمشكلة غالبًا DNS.

---

# 5. فحص DNS

## Windows

```cmd
nslookup google.com
```

---

## Linux / macOS

```bash
nslookup google.com
```

أو:

```bash
dig google.com
```

---

# مثال DNS جيد

```text
google.com
142.250.185.78
```

لو ظهر IP، فهذا يعني أن DNS يعمل.

---

# تغيير DNS للاختبار

يمكن تجربة DNS مشهور:

```text
Google DNS:
8.8.8.8
8.8.4.4
```

```text
Cloudflare DNS:
1.1.1.1
1.0.0.1
```

---

# 6. مسح DNS Cache

## Windows

```cmd
ipconfig /flushdns
```

---

## Linux

حسب النظام:

```bash
sudo systemd-resolve --flush-caches
```

أو:

```bash
sudo resolvectl flush-caches
```

---

## macOS

```bash
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```

---

# 7. فحص Routing Table

## Windows

```cmd
route print
```

---

## Linux

```bash
ip route
```

---

## macOS

```bash
netstat -rn
```

---

# أهم شيء في Routing Table

ابحث عن Default Route.

مثال:

```text
0.0.0.0/0 via 192.168.1.1
```

أو:

```text
default via 192.168.1.1
```

لو لا يوجد Default Route، الجهاز قد يتصل بالشبكة المحلية فقط ولا يدخل الإنترنت.

---

# 8. Traceroute

Traceroute يوضح الطريق إلى الوجهة.

## Windows

```cmd
tracert 8.8.8.8
```

---

## Linux / macOS

```bash
traceroute 8.8.8.8
```

أو:

```bash
tracepath 8.8.8.8
```

---

# متى تستخدم Traceroute؟

استخدمه عندما:

```text
Ping لا يصل
موقع معين لا يفتح
تريد معرفة أين تتوقف الحزمة
تريد معرفة عدد الـ Hops
```

---

# 9. فحص ARP Table

## Windows

```cmd
arp -a
```

---

## Linux

```bash
ip neigh
```

---

## macOS

```bash
arp -a
```

---

# ماذا يفيد ARP؟

يعرض الربط بين:

```text
IP Address
MAC Address
```

مثال:

```text
192.168.1.1 → aa:bb:cc:dd:ee:ff
```

لو لا ترى MAC الخاص بالراوتر، قد تكون هناك مشكلة Layer 2.

---

# 10. فحص Ports

## Windows PowerShell

```powershell
Test-NetConnection google.com -Port 443
```

---

## Linux / macOS

```bash
nc -vz google.com 443
```

مثال:

```bash
nc -vz example.com 80
nc -vz example.com 443
```

---

# معنى النتيجة

لو ظهر:

```text
succeeded
```

يعني البورت مفتوح ويمكن الوصول إليه.

لو ظهر:

```text
timed out
```

قد يكون Firewall أو مشكلة طريق.

لو ظهر:

```text
connection refused
```

غالبًا الجهاز موجود لكن الخدمة لا تعمل على هذا البورت.

---

# 11. عرض الاتصالات الحالية

## Windows

```cmd
netstat -ano
```

---

## Linux

```bash
ss -tunap
```

---

## macOS

```bash
netstat -an
```

---

# عرض Listening Ports

## Linux

```bash
ss -tuln
```

مع البرامج:

```bash
sudo ss -tulnp
```

---

# 12. معرفة البرنامج الذي يستخدم Port

## Windows

اعرض الاتصالات:

```cmd
netstat -ano
```

ثم خذ PID وابحث عنه:

```cmd
tasklist | findstr PID
```

---

## Linux

```bash
sudo lsof -i :80
```

أو:

```bash
sudo lsof -i :443
```

---

# 13. فحص HTTP

## عرض Headers فقط

```bash
curl -I https://example.com
```

---

## عرض تفاصيل الطلب

```bash
curl -v https://example.com
```

---

## تتبع Redirects

```bash
curl -L https://example.com
```

---

## عرض Status Code فقط

```bash
curl -o /dev/null -s -w "%{http_code}\n" https://example.com
```

---

# Status Codes مهمة

```text
200 = OK
301 = Permanent Redirect
302 = Temporary Redirect
400 = Bad Request
401 = Unauthorized
403 = Forbidden
404 = Not Found
429 = Too Many Requests
500 = Server Error
502 = Bad Gateway
503 = Service Unavailable
504 = Gateway Timeout
```

---

# 14. فحص TLS Certificate

```bash
openssl s_client -connect example.com:443 -servername example.com
```

---

# ماذا تفحص في TLS؟

ابحث عن:

```text
Certificate chain
Verify return code
TLS version
Cipher
Expiration date
Domain name
```

لو الشهادة منتهية أو الاسم غير مطابق، المتصفح قد يظهر تحذير.

---

# 15. فحص Wi-Fi

## Windows

```cmd
netsh wlan show interfaces
```

يعرض:

```text
SSID
Signal
Radio type
Channel
Authentication
```

---

## Linux

```bash
iwconfig
```

أو:

```bash
nmcli dev wifi list
```

---

# مشاكل Wi-Fi شائعة

```text
إشارة ضعيفة
قناة مزدحمة
كلمة مرور خطأ
IP غير صحيح
DNS لا يعمل
Router بعيد
Interference
```

---

# 16. فحص Firewall

## Windows

فتح إعدادات Firewall:

```cmd
wf.msc
```

أو عرض الحالة:

```cmd
netsh advfirewall show allprofiles
```

---

## Linux UFW

```bash
sudo ufw status
```

---

## Linux iptables

```bash
sudo iptables -L -n -v
```

---

# 17. فحص NAT

لو الإنترنت لا يعمل من الشبكة الداخلية، افحص:

```text
هل الجهاز لديه Private IP صحيح؟
هل Default Gateway صحيح؟
هل الراوتر لديه Public/WAN IP؟
هل NAT مفعّل؟
هل توجد Default Route على الراوتر؟
```

---

# 18. معرفة Public IP

```bash
curl ifconfig.me
```

أو:

```bash
curl ipinfo.io/ip
```

---

# ملاحظة عن CGNAT

لو WAN IP داخل الراوتر مختلف عن Public IP الظاهر على الإنترنت، قد تكون خلف:

```text
CGNAT
```

وهذا قد يمنع Port Forwarding من العمل بشكل طبيعي.

---

# 19. فحص MTU

أحيانًا بعض المواقع لا تفتح بسبب MTU.

## Windows

```cmd
ping google.com -f -l 1472
```

لو فشل، جرب رقمًا أقل.

---

## Linux

```bash
ping -M do -s 1472 google.com
```

---

# 20. خطوات تشخيص مشكلة لا يوجد إنترنت

اتبع هذا الترتيب:

```text
1. ipconfig / ip addr
2. ping 127.0.0.1
3. ping IP الجهاز نفسه
4. ping Default Gateway
5. ping 8.8.8.8
6. ping google.com
7. nslookup google.com
8. tracert / traceroute
9. Test-NetConnection أو nc
10. curl -v
```

---

# سيناريوهات سريعة

## الحالة 1: الجهاز لا يأخذ IP

علامة:

```text
169.254.x.x
```

افحص:

```text
DHCP
Cable
Wi-Fi
VLAN
Router
```

---

## الحالة 2: Ping للراوتر لا يعمل

المشكلة غالبًا في:

```text
Layer 1 Cable
Layer 2 Switch/Wi-Fi
IP/Subnet
VLAN
Firewall على الجهاز
```

---

## الحالة 3: Ping للراوتر يعمل لكن 8.8.8.8 لا يعمل

المشكلة غالبًا في:

```text
Router
NAT
ISP
Default Route
Firewall
```

---

## الحالة 4: Ping 8.8.8.8 يعمل لكن google.com لا يعمل

المشكلة غالبًا:

```text
DNS
```

الحل:

```text
تغيير DNS
مسح DNS Cache
فحص nslookup
```

---

## الحالة 5: الموقع لا يفتح لكن Ping يعمل

قد تكون المشكلة في:

```text
TCP Port 80/443
Firewall
TLS Certificate
Proxy
HTTP Error
```

اختبر:

```bash
curl -v https://example.com
```

أو:

```bash
nc -vz example.com 443
```

---

# أوامر مختصرة للحفظ

## Windows

```cmd
ipconfig /all
ipconfig /release
ipconfig /renew
ipconfig /flushdns
ping 8.8.8.8
tracert 8.8.8.8
nslookup google.com
route print
arp -a
netstat -ano
```

---

## PowerShell

```powershell
Test-NetConnection google.com -Port 443
Get-NetTCPConnection
```

---

## Linux

```bash
ip addr
ip route
ip neigh
ping 8.8.8.8
traceroute 8.8.8.8
dig google.com
ss -tunap
ss -tuln
curl -v https://example.com
nc -vz example.com 443
```

---

## macOS

```bash
ifconfig
netstat -rn
arp -a
ping 8.8.8.8
traceroute 8.8.8.8
dig google.com
curl -v https://example.com
nc -vz example.com 443
```

---

# ملخص سريع

```text
IP Address يوضح هل الجهاز أخذ عنوان صحيح
Gateway يوضح طريق الخروج من الشبكة
DNS يحول Domain إلى IP
Ping يفحص الوصول
Traceroute يوضح الطريق
ARP يربط IP مع MAC داخل الشبكة المحلية
Route Table يوضح الطرق
Ports تحدد الخدمات
curl يفحص HTTP/HTTPS
openssl يفحص TLS
```

---

# أهم القواعد

```text
لو 8.8.8.8 يعمل و google.com لا يعمل → DNS Problem
```

```text
لو Gateway لا يرد → مشكلة محلية
```

```text
لو Gateway يرد والإنترنت لا يعمل → افحص Router/NAT/ISP
```

```text
لو Ping يعمل والموقع لا يفتح → افحص TCP Port و TLS و HTTP
```

```text
ابدأ دائمًا من الأسفل للأعلى:
Cable/Wi-Fi → IP → Gateway → DNS → TCP → TLS → HTTP
```

---

## Navigation

| Previous | Home | Next |
|---|---|---|
| [HTTP and HTTPS Request Lifecycle](./HTTP-and-HTTPS-Request-Lifecycle.md) | [README](./README.md) | [Networking Practical Labs](./Networking-Practical-Labs.md) |
