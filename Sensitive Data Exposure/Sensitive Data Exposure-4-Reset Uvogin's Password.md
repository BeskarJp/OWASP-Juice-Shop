## [Sensitive Data Exposure] - [Reset Uvogin's Password]

## General Information

* **Difficulty Level :** ⭐⭐⭐⭐ / 6

* **Date of Resolution :** 25/03/2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Password Reset Mechanism / Internet Search

* **Affected Components :** Password Reset Functionality / Uvogin's account

* **Severity :** <span style="color:red"> High </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Social Engineering, Account Enumeration
* **Tools used :** Web Browser

#### 2. Reproduction Steps

**Step 1 : Target Identification**

On the admin account, there is a list of all the users and their emails address. Uvogin's password can be found there (`uvogin@juice-sh.op`). If you try to reset his password, a security question is asked : "Your favorite movie ?".

**Step 2 : External Profile Discovery**

By searching through social media with the target's email address, we are able to find a twitter account with a single post

![Uvogin's twitter account](../images/Uvogin.png)

One of the comment on this post talk about a movie named "Silence of the Lambs".

**Step 3 : Reset Uvogin's password**

We now have the answer to his security question and are now able to reset his password.

---
## Risk Assessment

* **Business Impact :** Unauthorized account takeover may lead to data exposure, fraudulent actions, and loss of user trust.

* **Technical Description of the Impact :** The application relies on weak, easily discoverable personal information for authentication.  This allows attackers to exploit publicly available data to bypass account security.

---

## Actions & Remediation

#### Mitigation Strategies

   - Avoid using security questions based on personal information easily found on social media
   - Do not share any personal information on social media

#### Recommended Fixes

   - Replace security questions with multi-factor authentication (MFA)
   - Use one-time tokens sent via email for password reset
   - Prevent account enumeration

#### Security Best Practices

  - A07:2021-Identification and Authentication Failures → Strengthen authentication mechanisms

  - A02:2021-Cryptographic Failures → Use secure reset tokens instead of static answers

  - A04:2021-Insecure Design → Avoid predictable security questions
