## [Sensitive Data Exposure] - [Forgotten Sales Backup]

## General Information

* **Difficulty Level :** ⭐⭐⭐⭐ / 6

* **Date of Resolution :** 26/03/2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Backup File Disclosure

* **Affected Components :** Public FTP Directory (`/ftp`)

* **Severity :** <span style="color:orange"> Medium </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Directory Enumeration, File Discovery, Null Byte Injection
* **Tools used :** Web Browser

#### 2. Reproduction Steps

**Step 1 : Directory Discovery**

In the application, when `f12` is pressed, you can access the main.js file in the Sources. You can find the information that there is a path to a directory named ftp. In this directory, you can find multiples files not accessible because of their type. The server enforces a restriction on file extensions (.png,  .jpg, ...) to ensure only images are accessed.

![ftp_folder](../images/ftp_folder.png)

![ftp_error](../images/ftp_error.png)

**Step 2 : Access Restriction Bypass**

To bypass this restriction, we will use the Null Byte Injection method. For this, we only have to add a null byte when trying to open the file.

```
https://ctf.juice.cyber.epitest.eu/ftp/coupons_2013.md.bak%2500.md
```

**Step 3 : Sensitive Data Exposure**

The server return the content of the backup file:

```
n<MibgC7sn
mNYS#gC7sn
o*IVigC7sn
k#pDlgC7sn
o*I]pgC7sn
n(XRvgC7sn
n(XLtgC7sn
k#*AfgC7sn
q:<IqgC7sn
pEw8ogC7sn
pes[BgC7sn
l}6D$gC7ss
```

This confirms access to internal business data not intended for public exposure.

---
## Risk Assessment

* **Business Impact :** Exposure of sales-related files may leak discount codes, promotional strategies, or historical business data. This can lead to financial loss, abuse of coupons, and reputational damage.

* **Technical Description of the Impact :** The vulnerability is caused by improper access control combined with unsafe handling of encoded input. By leveraging double encoding (`%2500`), an attacker bypasses file extension validation and retrieves restricted backup files.

---

## Actions & Remediation

#### Mitigation Strategies

   - Remove or secure publicly accessible backup files

   - Restrict access to sensitive directories like `/ftp`

   - Block null byte injection patterns (`%00`, `%2500`)

#### Recommended Fixes

   - Never expose backup or temporary files in production

   - Implement proper access control on all directories

   - Validate and sanitize user input after full decoding

   - Disable directory listing on sensitive paths

#### Security Best Practices

  - A02:2021-Cryptographic Failures -> Avoid leaking sensitive configuration data

  - A05:2021-Security Misconfiguration -> Disable directory listing and protect sensitive paths

  - A03:2021-Injection -> Sanitize all inputs
