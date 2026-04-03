## [Broken Authentication] - [Reset Jim's Password]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6

* **Date of Resolution :** 03-04-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Security Question Bypass

* **Affected Components :** Password Reset Page (`/forgot-password`)

* **Severity :** <span style="color:red"> High </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** OSINT, Information Identity Spy.
* **Tools used :** Web Browser, Burp Suite (Reapeater).

#### 2. Reproduction Steps

**Step 1 : Contextual Analysis**

The challenge implies that `Jim` is a celebrity whose personal details are publicly available. A key clue came from the `Extra Language` challenge, which involves **Klingon**. This strongly suggests a Star Trek theme. By crossing the name `Jim` with the Star Trek universe, we can identify the user as `Captain James Tiberius Kirk`.

**Step 2 : Analyzing the Security Question**

When attempting to reset the password for `jim@juice-sh.op`, the application prompts with the following question : `Your eldest siblings middle name ?`

**Step 3 : Information Gathering and Testing**

Researching James Tiberius Kirk's family reveals he has an older brother named `George Samuel Kirk`. We will try all names with Repeater of Burp Suite until we have `200 OK` Request.

**Step 4 : Reconstructing the Final Request**

The correct answer to the "middle name" is indeed Samuel. The POST request to the reset endpoint was captured and validated as follows :

```JSON
{
  "email": "jim@juice-sh.op",
  "answer": "Samuel",
  "new": "jim@juice-sh.op",
  "repeat": "jim@juice-sh.op"
}
```

**Step 5 : Final Exploitation**

The server returns a 200 OK status along with the user’s details (ID: 2, role: customer), confirming the password has been successfully reset.


## Risk Assessment

* **Business Impact :** An attacker can lock the legitimate user out and access sensitive personal data, including order history and delivery addresses.

* **Technical Description of the Impact :** This vulnerability highlights the inherent weakness of security questions based on biographical facts. The answers can be retrieved via a simple search.

---

## Actions & Remediation

#### Mitigation Strategies

   - Do not use questions where the answer is public or easily guessable.

   - Implement strict rate limiting or account lockouts after multiple failed security answer attempts to prevent brute-forcing.

#### Recommended Fixes

   - Replace security questions with a time-sensitive reset tokens sent via email or SMS.

#### Security Best Practices

  - A02:2021-Cryptographic Failures -> Implement secure password reset with tokens

  - A05:2021-Security Misconfiguration -> Avoid security questions based on public information
