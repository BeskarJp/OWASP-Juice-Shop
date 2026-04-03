## [Sensitive Data Exposure] - [Login Bender]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6

* **Date of Resolution :** 25-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Weak Security Answer

* **Affected Components :** Password Reset Module (`/forgot-password`)

* **Severity :** <span style="color:red"> High </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** OSINT
* **Tools used :** Web Browser

#### 2. Reproduction Steps

**Step 1 : Identifying the Target**

The objective is to reset the password for Bender. Navigating to the `Forgot Password` page and entering the email bender@juice-sh.op reveals his security question : `Company you first work as adult`

**Step 2 : OSINT & Research**

//

## Risk Assessment

* **Business Impact :** An attacker can gain full control of an user accounts without knowing the original password. This leads to data breaches, unauthorized orders.

* **Technical Description of the Impact :** Security concerns are often irrelevant when it comes to authentication. When these questions rely on public knowledge, they bypass the security provided by a strong password.

---

## Actions & Remediation

#### Mitigation Strategies

   - Do not use questions where the answer is a matter of public record or can be found via a simple search.

   - Implement strict rate limiting on security question attempts to prevent brute-forcing answers.

#### Recommended Fixes

   - Replace security questions with cryptographically secure, time-limited reset tokens sent via email.

   - If security questions must be used, allow users to create their own unique questions and answers.
