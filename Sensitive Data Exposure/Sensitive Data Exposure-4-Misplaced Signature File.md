## [Sensitive Data Exposure] - [Misplaced Signature File]

## General Information

* **Difficulty Level :** ⭐⭐⭐⭐ / 6

* **Date of Resolution :** 26/03/2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Backup File Disclosure

* **Affected Components :** Public FTP Directory (`/ftp`)

* **Severity :** <span style="color:yellow"> Low </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Directory Enumeration, File Discovery, Null Byte Injection
* **Tools used :** Web Browser

#### 2. Reproduction Steps

**Step 1 : Directory Discovery**

In the application, when `f12` is pressed, you can access the main.js file in the Sources. You can find the information that there is a path to a directory named ftp. In this directory, you can find multiples files not accessible because of their type. The server enforces a restriction on file extensions (.png,  .jpg, ...) to ensure only images are accessed.

![[Pasted image 20260403124552.png]]

![[Pasted image 20260403124815.png]]

**Step 2 : Access Restriction Bypass**

To bypass this restriction, we will use the Null Byte Injection method. For this, we only have to add a null byte when trying to open the file.

```
https://ctf.juice.cyber.epitest.eu/ftp/suspicious_errors.yml%2500.md
```

**Step 3 : Sensitive Data Exposure**

The file reveals internal data, such as file signature or integrity-related information.

```YAML
title: Suspicious error messages specific to the application
  description: Detects error messages that only occur from tampering with or attacking the application
author: Bjoern Kimminich
logsource:
    category: application
    product: nodejs
    service: errorhandler
detection:
    keywords:
        - 'Blocked illegal activity'
        - '* with id=* does not exist'
        - 'Only * files are allowed'
        - 'File names cannot contain forward slashes'
        - 'Unrecognized target URL for redirect: *'
        - 'B2B customer complaints via file upload have been deprecated for security reasons'
        - 'Infinite loop detected'
        - 'Detected an entity reference loop'
    condition: keywords
level: low
```

While not directly critical, this information should not be publicly exposed.

---

## Risk Assessment

* **Business Impact :** Limited direct impact, but may contribute to reconnaissance by exposing internal file structure or metadata.

* **Technical Description of the Impact :** The application exposed hidden or misplaced files without access control. This may assist attackers in understanding the system and chaining further attacks.

---

## Actions & Remediation

#### Mitigation Strategies

   - Remove or secure unnecessary or hidden files from public directories

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