---
id: create-payment
title: Create Payment
sidebar_label: Create Payment
description: Create Payment
sidebar_position: 4
---

# Create Agreement

**POST** `/payto/payments`

## Try It

You can test this endpoint using the following cURL command:

```bash
curl --request POST \
  --url 'https://api-sandbox.oligo.com.au/v1/payto/payments' \
  --header 'accept: application/json' \
  --header 'authorization: Bearer <your_token>' \
  --header 'content-type: application/json' \
  --data '{
    "uid": "<your_payment_uid>",
    "agreement_uid": "<your_payment_uid",
    "amount": 2888,
    "priority": "unattended"
  }'
```
