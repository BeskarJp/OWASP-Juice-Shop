## [Sensitive Data Exposure] - [Forgotten Developer Backup]

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

**Step 1 : Directory Discovery

In the application, when `f12` is pressed, you can access the main.js file in the Sources. You can find the information that there is a path to a directory named ftp. In this directory, you can find multiples files not accessible because of their type. The server enforces a restriction on file extensions (.png,  .jpg, ...) to ensure only images are accessed.

![[Pasted image 20260403124552.png]]

![[Pasted image 20260403124815.png]]

**Step 2 : Access Restriction Bypass**

To bypass this restriction, we will use the Null Byte Injection method. For this, we only have to add a null byte when trying to open the file.

```
https://ctf.juice.cyber.epitest.eu/ftp/package.json.bak%2500.md
```

## Risk Assessment

* **Business Impact :** Exposure of backup files can reveal internal application structure, dependencies, and potentially sensitive configuration data. This information can be leveraged to identify further vulnerabilities or plan targeted attacks.

* **Technical Description of the Impact :** The application fails to properly restrict access to backup files and is vulnerable to null byte injection. By combining directory enumeration with input manipulation, an attacker can bypass file extension restrictions and retrieve unintended resources.

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