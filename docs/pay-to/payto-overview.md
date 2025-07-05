---
id: payto-overview
title: PayTo API
sidebar_label: PayTo Overview
sidebar_position: 1
---

# Oligo PayTo API Overview

PayTo is a real-time payment mechanism that allows businesses to initiate payments from customer bank accounts using secure agreements.

---

## 📘 Introduction

Oligo PayTo lets you create **agreements** with customers and trigger **payments** based on those agreements.

You must first create an agreement, then initiate payments against that agreement.

---

## 🔐 Authentication

All API calls must include an Authorization header and content type set to JSON:

```
authorization: Bearer <access_token>
content-type: application/json
```

You will receive the access_token after signing contract with Oligo.

---

## Environments
We have 2 environments for testing and production:
- **Sandbox**: `https://api-sandbox.oligo.com.au/v1`
- **Production**: `https://api-prod.oligo.com.au/v1`



## 📄 The main endpoints you might use for the first integration

### 1. Create PayTo Agreement

Once you sign the contract with Oligo and get the access_token, you can create a PayTo agreement to start with your customer.

### 2. Check Agreement Status

### 3. Initiate PayTo Payment

After the agreement is authorized from the customer's bank account, you can initiate a payment against that agreement.
This allows you to charge the customer based on the terms defined in the agreement.

### 4. Check Payment Status

### 5. Webhooks

Oligo notifies your server about changes to agreement or payment status via webhooks.
You can initialize your webhook by providing your own webhook endpoint to Oligo.

