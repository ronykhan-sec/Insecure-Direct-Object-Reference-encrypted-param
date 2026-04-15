# Insecure-Direct-Object-Reference-encrypted-param
Security write-up showing how improper server-side validation of encrypted parameters can result in unauthorized access to private user information in a real-world application.

# 🔐 Insecure Direct Object Reference via Encrypted Parameter Manipulation

## 📌 Summary

While testing an e-commerce application, I discovered a vulnerability that allowed unauthorized access to other users' address data by manipulating a client-side parameter called `encrypted_request`.

This issue occurs due to improper server-side validation, leading to sensitive data exposure.

---

## 🧠 Vulnerability Overview

The application uses an API endpoint to handle user address data. The request includes a parameter:
This parameter is expected to securely represent user-specific data. However, the server fails to validate whether the request actually belongs to the authenticated user.


## 🔍 Steps to Reproduce

1. Login to **Account A**
2. Intercept an address-related API request using a proxy tool
3. Login to **Account B** in another session
4. Capture the same API request
5. Copy the `encrypted_request` value from Account B
6. Replace Account A’s `encrypted_request` with Account B’s value
7. Send the modified request


## 📸 Proof of Concept

### 🧾 Request Manipulation

![Request](./IMG_20260415_144048.jpg)

### 📦 Unauthorized Response
![Response](./IMG_20260415_144103.jpg)

The response returns another user's address data, including:
- Name
- Phone number
- Full address
- Location details

## ⚠️ Root Cause

- Missing server-side authorization checks
- Trusting client-controlled encrypted parameters
- No validation of resource ownership


## 💥 Impact

- Unauthorized access to sensitive user data (PII)
- Exposure of addresses and phone numbers
- Cross-account data leakage
- Potential for large-scale data harvesting


## 🛡️ Recommended Fix

- Validate user ownership on the server side
- Bind request parameters to authenticated sessions
- Avoid trusting client-side encrypted data
- Implement strict access control checks

## ⚠️ Disclaimer

This report is for educational purposes only. Testing was performed on authorized accounts.
