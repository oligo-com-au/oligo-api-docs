---
id: create-payment
title: Create Payment
sidebar_label: Create Payment
description: Create Payment
sidebar_position: 4
---

# Create Payment

**POST** `/payto/payments`

## Overview

Once the NPP has received the PayTo Payment Initiation request, we should expect to find out the final payment status within a few seconds. Please note that the NPP has SLAs built in to around 30 seconds for any outliers.

Once we receive the final payment status report from the NPP, we will trigger a payment status webhook to your application containing the outcome: Payment Successful or Payment Failed.

:::tip

Once a PayTo Payment has been submitted onto the NPP Basic Infrastructure, it is irrevocable and can not be cancelled; funds are cleared & settled between the debtor and creditor account in real-time.

:::

## PayTo Payment Lifecycle
### Standard Payment
1. Created 
2. Submitted 
3. Settled or Failed
### Delayed Payment
1. Created 
2. Submitted 
3. Pending 
4. Settled or Failed
### Under Investigation
1. Created 
2. Submitted 
3. Pending 
4. Under Investigation 
5. Settled or Failed

## Request Headers

```http
Authorization: Bearer <access_token>
Content-Type: application/json
Accept: application/json
```

## Request Body

Here are some key fields in the request payload, please check the API Reference for more details.

| Field                       | Type     | Required | Description                                                                                                                                                                                                                                                                                                              |
|-----------------------------|----------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `uid`                       | string   | Yes      | The supplied unique identifier for the Agreement. <br/>This identifier ensures agreement uniqueness between integrator systems and Oligo.                                                                                                                                                                                |
| `agreement_uid`             | string   | Yes      | The unique PayTo Agreement UID to validate this payment against.                                                                                                                                                                                                                                                         |
| `amount`                    | integer  | Yes      | The amount (in cents) to be collected from the debtor account.                                                                                                                                                                                                                                                           |
| `priority`                  | string   | Yes      | Execution priority of payment. attended payments will be prioritised over unattended payments. <br/> For example: <li>`Attended: E-commerce style checkout experiences`.</li> <li> `Unattended: Loan collections`</li>                                                                                                   |

## Successful Response

```json
{
  "data": {
    "uid": "<your_payment_uid>",
    "agreement_uid": "<your_agreement_uid>",
    "state": "created",
    "amount": 2888,
    "priority": "unattended",
    "creditor": {
      "party_name": "The name of the creditor party for this agreement.",
      "ultimate_party_name": "The name of the creditor party for this agreement.",
      "account_identifier": {
        "type": "bban",
        "value": "123456-98765432"
      }
    },
    "debtor": {
      "ultimate_party_name": "John Doe",
      "party_name": "John Doe",
      "account_identifier": {
        "type": "bban",
        "value": "123456-78901234"
      }
    },
    "links": {
      "self": "/payto/payments/<your_payment_uid>",
      "agreement": "/payto/agreements/<your_agreement_uid>"
    }
  }
}
```


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
    "agreement_uid": "<your_agreement_uid>",
    "amount": 2888,
    "priority": "unattended"
  }'
```
