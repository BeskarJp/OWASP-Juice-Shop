## [Broken Access Control] - [Manipulate Basket]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6

* **Date of Resolution :** 25-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** HTTP Parameter

* **Affected Components :** Shopping Basket API (`/api/BasketItems/`)

* **Severity  :** <span style="color:red"> High </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** HTTP Parameter
* **Tools used :** Web Browser, Burp Suite

#### 2. Reproduction Steps

**Step 1 : Identifying the Security Constraints**

Initially, trying to modify a basket directly results in a `400 Bad Request`. The server response indicates a `noUpdate` constraint on the `BasketId` field, preventing unauthorized users from changing the destination basket of an item.

**Step 2 : Attempting Parameter Overloading**

To bypass this logic, I used HTTP Parameter by duplicating the BasketId key in the JSON body :

```JSON
{
  "ProductId": 22,
  "BasketId": "1",
  "BasketId": "7",
  "quantity": 1
}
```

This triggered a `500 Internal Server Error` with a `SQLITE_CONSTRAINT: FOREIGN KEY constraint failed`. The server was processing the second `BasketId`, but the ID used (`7`) did not exist in the database.

**Step 3 : Successful Exploitation**

We repeated the process using a known valid `BasketId` for another user (`BasketId: 2`) :

```JSON
{
  "ProductId": 42,
  "BasketId": "1", 
  "BasketId": "2",
  "quantity": 1
}
```

**Step 4 : Verification**

The server returned a `200 OK`. The response body confirmed that the item (`ProductId: 42`) was successfully attached to `BasketId: 2`. The item was added to another user's basket.

## Risk Assessment

* **Business Impact :** Attackers can add unwanted items to any user's basket.

---

## Actions & Remediation

#### Mitigation Strategies

   - Implement a validator that rejects any request containing duplicate keys in the JSON body.

   - The `BasketId` should ideally be retrieved from the user's session on the server side, rather than being accepted as an editable parameter from the client.

#### Recommended Fixes

   - Configure a web server to handle multiple parameters with the same name, either by rejecting the request or by only considering the first validated instance.
