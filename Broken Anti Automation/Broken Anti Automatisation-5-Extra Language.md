## [Broken Anti Automatisation] - [Extra Language]

## General Information

* **Difficulty Level :** ⭐⭐⭐⭐⭐ / 6

* **Date of Resolution :** 30-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Improper Access Control

* **Affected Components :** Front-end Assets (`/assets/i18n/`)

* **Severity  :** <span style="color:orange"> Medium </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Fuzzing, Path Manipulation
* **Tools used :** Burp Suite (Repeater), Exegol and Web Browser.

#### 2. Reproduction Steps

**Step 1 : Initial Observation**

The application supports multiple languages by fetching specific JSON files from the `/assets/i18n/` directory. Standard files like `en_GB.json` or `fr_FR.json` are called when the user changes the language of the website.

**Step 2 : Fuzzing & Discovery**

By performing a fuzzing attack on the `/assets/i18n/` directory or using all ISO languages:

``` bash
ffuf -u http://172.17.0.2:3000/assets/i18n/FUZZ -w iso-639-x.txt
ffuf -u http://172.17.0.2:3000/assets/i18n/tlh_FUZZ.json -w letters.txt
```

After that, an unlisted file was discovered: **`tlh_AA.json`** (Klingon). This file is not available in the language selection drop-down menu.

**Step 3 : Traffic Manipulation**

Intercept `GET` request with Burp Suite and modified to point to the newly discovered endpoint.

```HTTP
GET /assets/i18n/tlh_AA.json HTTP/2
Host: ctf.juice.cyber.epitest.eu
Cookie: language=ca_ES; continueCode=...
```

**Step 4 : Response Analysis**

The server returns a `HTTP/2 200 OK` response containing the JSON content. The file contains some words in Klingon, confirming the successful exploitation :

```JSON
{
  "LANGUAGE": "tlhIngan",
  "NAV_SEARCH": "tu'",
  "SEARCH_PLACEHOLDER": "tu'...",
  "NAV_COMPLAIN": "bep",
  "TITLE_LOGIN": "yi'el",
  "MANDATORY_EMAIL": "Yes.",
  ...
}
```

---

## Risk Assessment

* **Business Impact :** Low in a standard production environment, but it reveals a risk in resource management. In other scenarios, this could lead to the discovery of hidden features or sensitive misconfiguration files.

* **Technical Description of the Impact :** The lack of restrictions on static file access within the assets directory allows the discovery of resources that were not intended to be public. This is a form of unauthorized enumeration of unprotected resources.

---

## Actions & Remediation

#### Mitigation Strategies

   - Restrict access to language files.

   - Remove all development or testing files from production environments.

#### Recommended Fixes

   - Configure the web server to disable directory listing and strictly limit access to necessary files.

   - Implement a CI/CD pipeline that filters out non-essential assets before deploying to the production environment.

#### Security Best Practices

  - A02:2021-Cryptographic Failures -> Protect sensitive assets from unauthorized access

  - A05:2021-Security Misconfiguration -> Disable directory listing and restrict access to language files

  - A03:2021-Injection -> Prevent path manipulation to hidden resources
