## [Broken Anti Automatisation] - [CAPTCHA Bypass]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6

* **Date of Resolution :** 25-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** CAPTCHA Reuse / Improper Validation

* **Affected Components :** Customer Feedback Form (`/contact`)

* **Severity  :** <span style="color:orange"> Medium </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Manual Request Replay, Exploiting Logical Flaws
* **Tools used :** Web Browser

#### 2. Reproduction Steps

**Step 1 : Initial Feedback Submission**

Navigate to the Customer Feedback page. Fill in the required fields and solve the CAPTCHA math problem. Submit the form to observe the standard behavior.

**Step 2 : Identifying the Logic Flaw**

The application is designed to prevent spam by requiring a CAPTCHA for each submission. However, the server-side validation fails to "consume" or invalidate the CAPTCHA solution once it has been used successfully.

**Step 3 : Exploitation (Manual Spamming)**

Instead of using automated tools like Burp Suite or Python scripts, this vulnerability can be exploited through manual coordination :

**Step 4 : Response Analysis**

The server returns a `HTTP/2 200 OK` response containing the JSON content. The file contains some words in Klingon, confirming the successful exploitation :

   1. Open the feedback form.
   2. Solve the CAPTCHA
   3. In coordination with a team mate, spam the "Submit" button repeatedly using the same validated CAPTCHA answer.
   4. By submitting more than 10 feedback entries within a 20-second window, the anti-automation threshold is crossed.

---

## Risk Assessment

* **Business Impact :** The feedback system can be flooded with spam, distorting customer satisfaction metrics and potentially overwhelming the database.

* **Technical Description of the Impact :** A CAPTCHA should be tied to a single-use token. By allowing reuse, the application renders its primary defense against bots completely ineffective.

---

## Actions & Remediation

#### Mitigation Strategies

   - Ensure the server-side logic invalidates a CAPTCHA ID immediately after it has been used for a successful request.

   - Implement IP-based rate limiting to prevent a high volume of requests from a single source within a short timeframe.

#### Recommended Fixes

   - Replace the simple math-based CAPTCHA with more robust solutions, which use behavioral analysis to detect bots.

#### Security Best Practices

  - A02:2021-Cryptographic Failures -> Implement secure CAPTCHA validation

  - A03:2021-Injection -> Prevent reuse of security tokens
