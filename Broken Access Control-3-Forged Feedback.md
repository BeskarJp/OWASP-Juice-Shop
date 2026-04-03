## [Broken Access Control] - [Forged Feedback]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6

* **Date of Resolution :** 25-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Improper Authorization

* **Affected Components :** Feedback Submission API (`/api/Feedbacks/`)

* **Severity  :** <span style="color:red"> High </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** HTTP Request Interception and Parameter
* **Tools used :** Web Browser, Burp Suite

#### 2. Reproduction Steps

**Step 1 : Intercept the Feedback Request**

Navigate to `Customer Feedback` page. Fill out the form with a random comment and rating. Before clicking Submit, enable "Intercept" in Burp Suite.

**Step 2 : Analyze and Modify the Payload**

In Burp Suite, examine the `POST` request. It is observed that the application explicitly sends a `UserId` within the JSON payload:

```JSON
{
  "UserId": 3,
  "captchaId": 8,
  "captcha": "4",
  "comment": "test",
  "rating": 1
}
```

**Step 3 : Exploit the Lack of Server-Side Validation**

Modify the value of the `UserId` field to another value. This test determines if the application verifies the ownership of the feedback.
Once the modification is complete, click to `Forward`.

**Step 4 : Verify Success**

The server returned a `201 Created` status code. The JSON response confirms that the feedback was registered with the modified User ID, proving that the server does not validate the consistency between the authenticated user and the ID provided in the request.

## Risk Assessment

* **Business Impact :** This vulnerability allows any user to post comments or reviews on behalf of others, including administrators or high-profile customers

* **Technical Description of the Impact :** The system uses client-side data to identify the author of an action instead of secure session data stored on the server. This allows an attacker to bypass basic access controls simply by changing a numerical value.

---

## Actions & Remediation

#### Mitigation Strategies

   - Never rely on user identifiers (`user_id, email`) sent directly in the body of POST/PUT requests if the user is already authenticated.

   - Identify the user exclusively via their Identity Token (JWT) or server-side Session data.

#### Recommended Fixes

   - When receiving a request, the backend must extract the User ID from the authentication token and ignore any ID provided in the payload.

#### Security Best Practices

  - A01:2021-Broken Access Control -> Implement the principle of least privilege and verify authorizations for every object access.

  - A04:2021-Insecure Design -> Design APIs that use secure tokens to maintain user state rather than modifiable parameters.
