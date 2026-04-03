## [Sensitive Data Exposure] - [Access Log]

## General Information

* **Difficulty Level :** ⭐⭐⭐⭐ / 6

* **Date of Resolution :** 02-04-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Broken Access Control

* **Affected Components :** Server-side Log Directory (`/support/logs`)

* **Severity  :** <span style="color:red"> High </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Fuzzing, Path Manipulation
* **Tools used :** Exegol and Web Browser.

#### 2. Reproduction Steps

**Step 1 : Initial Observation**

The application suggests that admin roles have access to specific diagnostic tools. After gaining access to a Support account, we investigate potential hidden paths that wouldn't be linked in the standard navigation menu.

**Step 2 : Fuzzing & Discovery**

Using `ffuf` with a common wordlist, we have found the `/support/` directory to find unprotected sub-directories or files. :

``` bash
ffuf -u http://172.17.0.2:3000/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt
ffuf -u http://172.17.0.2:3000/support/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

The tool identifies a hit on the `/logs` endpoint, which returns a 200 OK status.

**Step 3 : Direct Access & Directory Listing**

By visiting the discovered URL `https://ctf.juice.cyber.epitest.eu/support/logs/` directly in the browser, the server reveals a directory listing. This interface displays a .log files stored on the server.

**Step 4 : Sensitive Data Extraction**

We select the file `access.log.2026-04-02`. The server allows the download without any further authorization check.

---

## Risk Assessment

* **Business Impact :** High. The exposure of access logs provides a roadmap for future attacks. It leaks internal system architecture and user activity data.

* **Technical Description of the Impact :** By allowing Directory Listing and failing to restrict the `/support/logs` path to internal networks or strict admin roles, the server permits any user who guesses the URL to exfiltrate critical system informations.

---

## Actions & Remediation

#### Mitigation Strategies

   - Disable Directory Indexing on the web server to prevent file enumeration.

   - Implement strict Role Check to ensure only verified administrators can access the `/support hierarchy`.

#### Recommended Fixes

   - Move log files to a secure location outside the public folder in web.

   - Use a dedicated, authenticated logging platform rather than exposing flat files via a web interface.
