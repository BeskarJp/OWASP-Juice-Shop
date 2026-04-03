## [Broken Access Control] - [Forged Review]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6

* **Date of Resolution :** 25-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Improper Authorization

* **Affected Components :** Product Review API (`/rest/products/{id}/reviews`)

* **Severity  :** <span style="color:orange"> Medium </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** HTTP Request Interception
* **Tools used :** Web Browser, Burp Suite

#### 2. Reproduction Steps

**Step 1 : Identify the Review Author Parameter**

When you write a review, the app sends a request with an author field. We found that I can manually change this email address to anyone else's email. The server does not check if the email belongs to the person logged in.

**Step 2 : Get the Review ID**

To edit a review that is already posted, we need its unique ID. We found this ID by intercepting a `Like` request on a comment. The server response showed the hidden ID : `"id": "HIDDEN_REVIEW_ID_123",`

**Step 3 : Change Someone Else's Review**

Using that ID, we sent a `PUT` request to the server. Because the server doesn't check who owns the review, we were able to change the message content (For example):

```JSON
{
  "id": "HIDDEN_REVIEW_ID_123",
  "message": "I don't know why I'm here"
}
```

**Step 4 : Confirm the Exploit**

We refreshed the product page. The original review was gone and replaced by my new message. This proves that any user can edit or fake any review.

## Risk Assessment

* **Business Impact :** It can destroy a shop's reputation. We can delete good reviews, write fake bad reviews as other people, or pretend to be an admin to trick customers.

* **Technical Description of the Impact :** The server processes `Edit` requests based only on the review ID. It does not check if the person sending the request is the actual owner of that review.

---

## Actions & Remediation

#### Mitigation Strategies

   - Never let the user choose the "Author" name in the request. The server should get the name automatically from the user's login session.

   - Always check if the user has permission to edit a specific item.

#### Recommended Fixes

   - Before saving changes, the server must ask if the `ReviewID` belong to the User who is currently logged in. If no, block the request.

   - Use secure ID checks on the backend for every `Edit` or `Delete` action.

#### Security Best Practices

  - A01:2021-Broken Access Control -> Check permissions on the server, not just on the website.

  - A04:2021-Insecure Design -> Don't trust data sent by the user for sensitive fields like user IDs.
