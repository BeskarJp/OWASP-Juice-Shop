## [Improper Input Validation] - [Payback Time]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6
* **Date of Resolution :** 25-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** Insecure Business Logic / Improper Input Validation
* **Affected Components :** Payment Process / User's Wallet
* **Severity :** <span style="color:red"> Critical </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** Business Logic Abuse, Request Interception, Parameter Manipulation
* **Tools used :** Web Browser, Burp Suite (Proxy)

#### 2. Reproduction Steps

**Step 1 : Accessing the Deluxe Portal**

A product is added to the cart and the checkout process is initiated. Using Burp Suite, the request sent during the payment process is intercepted. 

**Step 2 : Manipulation the Price**

Before forwarding the request to the server, the Price parameter is modified to a negative number. The modified request is accepted by the backend without validation. The transaction is processed using the negative amount.

---

## Risk Assessment

* **Business Impact :** Attackers can exploit this flaw to generate money by manipulating transaction values.  This can lead to severe financial losses and abuse of the payment system.

* **Technical Description of the Impact :** The application relies on client-side data for critical financial operations. The absence of server-side validation allows attackers to tamper with transaction values and alter the outcome of payments.

---

## Actions & Remediation

#### Mitigation Strategies

   - Validate all financial values on the server side

   - Never trust client-supplied data

#### Recommended Fixes

   -  Enforce strict validation (e.g., price > 0)
   
   -  Recalculate total price on the server instead of trusting client input

   -  Log and monitor suspicious payment activity

#### Security Best Practices

  - A04:2021-Insecure Design -> Ensure business logic prevents abuse

  - A01:2021-Broken Access Control -> Protect privileged functionalities

  - A05:2021-Security Misconfiguration -> Secure sensitive endpoints
