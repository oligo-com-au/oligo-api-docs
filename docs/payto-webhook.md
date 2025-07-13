---
id: payto-webhook
title: PayTo Webhook
sidebar_label: PayTo Webhook
sidebar_position: 3
---

# Oligo PayTo API Overview

PayTo is a real-time payment mechanism that allows businesses to initiate payments from customer bank accounts using secure agreements.

---

## 📘 Introduction

MyOligo PayTo lets you create **agreements** with customers and trigger **payments** based on those agreements.

You must first create an agreement, then initiate payments against that agreement.

---

## 🔐 Authentication

All API calls must include an Authorization header:

```
Authorization: Bearer <access_token>
```

---

## 🔁 Create PayTo Agreement

**POST** `https://api.myoligo.com/payto/agreements`

### Request

```json
{
  "payer_name": "Jane Smith",
  "payer_bsb": "123456",
  "payer_account_number": "87654321",
  "description": "Recurring gym membership",
  "metadata": {
    "customer_id": "cust-001"
  }
}
```

### Response

```json
{
  "uid": "agr_abc123",
  "status": "pending",
  "created_at": "2025-06-13T12:00:00Z"
}
```

---

## 💸 Initiate PayTo Payment

**POST** `https://api.myoligo.com/payto/payments`

### Request

```json
{
  "agreement_uid": "agr_abc123",
  "amount": 2500,
  "description": "Weekly gym fee",
  "reference": "GYM-20250613"
}
```

### Response

```json
{
  "payment_uid": "pay_xyz789",
  "status": "processing",
  "amount": 2500,
  "created_at": "2025-06-13T12:30:00Z"
}
```

---

## 📄 Get Agreement by UID

**GET** `https://api.myoligo.com/payto/agreements/{uid}`

Returns detailed information about an agreement.

---

## 📄 Get Payment by UID

**GET** `https://api.myoligo.com/payto/payments/{uid}`

Returns the status and metadata of a PayTo payment.

---

## 📬 Webhooks

MyOligo notifies your server about changes to agreement or payment status via webhooks.

You can configure your webhook endpoint in the developer dashboard.

---

## 📞 Support

Contact us at **support@myoligo.com** for questions or help integrating MyOligo PayTo API.
