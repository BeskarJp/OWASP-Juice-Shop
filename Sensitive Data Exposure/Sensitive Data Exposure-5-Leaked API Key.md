## [Sensitive Data Exposure] - [Leaked API Key]

## General Information

* **Difficulty Level :** ⭐⭐⭐⭐⭐ / 6

* **Date of Resolution :** 26-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Sensitive Data Exposure in Code Source

* **Affected Components :** Public Source Code (https://github.com/juice-shop/juice-shop)

* **Severity :** <span style="color:red"> Critical </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** OSINT, Secret Scanning.
* **Tools used :** GitHub Juice-Shop, GitHub Search

#### 2. Reproduction Steps

**Step 1 : Search in Code Source Repository**

In most cases, the leaked sensitive information is located either on a GitHub repository or in restricted files. Searching for the terms `apikey` or `secret` in the OWASP Juice Shop source code reveals configuration files and verification routes.

**Step 2 : Identify API Key in `routes/verify.ts`**

Analysis of the `routes/verify.ts` file (line 322) reveals a regular expression or direct comparison used by the system to validate whether a user reports the correct leaked key :

```typescript
{ [Op.like]: '%6PPi37DBxP41DwlriuaxP15HaDjpsUXY5TspVmie%' }
```

**Step 3 : Reporting via the Complaint System**

To exploit this and "inform the shop" as required by the challenge :

   - Navigate to the `Customer FeedBack`
   - Insert the exact key `6PPi37DBxP41DwlriuaxP15HaDjpsUXY5TspVmie` into the `Comment`.
   - Submit the form and the flag appear.

---

## Risk Assessment

* **Business Impact :** An API key leaked in source code can grant full access to a big part of services (Payments, Cloud infrastructure, User data). If this key belongs to an administrator, the attacker could potentially take control of the infrastructure.

* **Technical Description of the Impact :** Git maintains a full history of all changes. Even if a key is removed in a later version, it remains accessible in previous `commits`. This allows malicious actors to script the harvesting of secrets across thousands of public repositories simultaneously.

---

## Actions & Remediation

#### Mitigation Strategies

   - Store secrets outside of the source code (for example, in .env files that are strictly excluded from version control).

   - Use dedicated solutions such as an app which can manage Secrets Variables.

#### Recommended Fixes

   - Any exposed key must be immediately invalidated and rotated on the respective service.

   - Use tools like `git-filter-repo` to permanently purge the key from the entire repository history.
