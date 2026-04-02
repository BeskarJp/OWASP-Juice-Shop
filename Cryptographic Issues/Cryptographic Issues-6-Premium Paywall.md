## [Cryptographic Issues] - [Premium Paywall]

## General Information

* **Difficulty Level :** ⭐⭐⭐⭐⭐⭐ / 6

* **Date of Resolution :** 03-04-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Information Exposure

* **Affected Components :** Hidden Premium URL (`/this/page/is/hidden/...`) and Key Directory (`/encryptionkeys`)

* **Severity  :** <span style="color:red"> Hight </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Fuzzing, Cryptographic Analysis
* **Tools used :** Fuff, CyberChef, Web Browser.

#### 2. Reproduction Steps

**Step 1 : Fuzzing and Discovery of the Key**

We begin with an enumeration phase to find unlisted sensitive directories. Using ffuf, we discover a suspicious directory :

```bash
ffuf -u http://172.17.0.2:3000/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

The scan reveals the path `/encryptionkeys`. Accessing via the browser, we find a file named `premium.key`. It contents : `1337133713371337.EA99A61D92D2955B1E9285B55BF2AD42`

**Step 2 : Cryptographic Data Analysis**

Analyzing the string allows us to isolate two essential components for AES-128-CBC decryption :

   - **IV (Initialization Vector) :** `1337133713371337` (ASCII format) . `31333337313333373133333731333337` (Hex format).
   - **Key :** `EA99A61D92D2955B1E9285B55BF2AD42`

**Step 3 : Extraction of the Encrypted Payload**

By searching through the application's JavaScript files, we identify a Base64-encoded string representing the premium URL : `IvLuRfBJYlmStf9XfL6ckJFngyd9LfV1JaaN/KRTPQPidTuJ7FR+D/nkWJUF+0xUF07CeCeqYfxq+OJVVa0gNbqgYkUNvn//UbE7e95C+6e+7GtdpqJ8mqm4WcPvUGIUxmGLTTAC2+G9UuFCD1DUjg==`.

**Step 4 : Decryption with Cyber Chef**

For obtain the clear URL, we configure a Cyber Chef like :

```text
Operation : `From Base64`
Operation : `AES Decrypt`
   - Key : `EA99A61D92D2955B1E9285B55BF2AD42` (Type: Hex)
   - IV : `31333337313333373133333731333337` (Type: Hex)
   - Mode : `CBC`
   - Input/Output : `Raw`/`Hex`
```

**Step 5 : Accessing the Premium Content**

By navigating to the decrypted path : `http://localhost:3000/this/page/is/hidden/behind/an/incredibly/high/paywall/that/could/only/be/unlocked/by/sending/1btc/to/us`. The server validates the access.

---

## Risk Assessment

* **Business Impact :** High. Gaining free access to content that is normally paid completely bypasses the application's monetization.

* **Technical Description of the Impact :** Security here relies on hidden URL. The compromise is total because the secrets (Key and IV) are stored on the same server as the protected data and are accessible without authentication.

---

## Actions & Remediation

#### Mitigation Strategies

   - The `/encryptionkeys` directory must never be publicly accessible via the web server.

#### Recommended Fixes

   - Use a dedicated, authenticated logging platform rather than exposing flat files via a web interface.
