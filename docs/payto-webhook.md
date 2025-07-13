---
id: payto-webhook
title: PayTo Webhook
sidebar_label: PayTo Webhook
sidebar_position: 3
---

# Webhook

Webhooks are used to notify your application of changes to the state of PayTo Agreements and PayTo Payments as and when they are processed through the system.

By subscribing to our webhook events you can monitor the status of each agreement and payment in order to trigger the appropriate next steps.


## Our Delivery Promises
1. We only consider a webhook event delivery as failed if we don't receive any success http response code (2xx, 3xx).
1. We will auto-retry failed deliveries every 5 minutes for 1 hour. (Note: In the sandbox environment, webhook deliveries will only be retried once, to allow for easier testing of failure scenarios.)
1. Delivery order for webhook events is not guaranteed.
1. We guarantee at least 1 delivery attempt.

## Request ID
We provide a Split-Request-ID header in the form of a UUID which uniquely identifies a webhook event.

If the webhook event is retried/retransmitted, the UUID will remain the same. This allows you to check if a webhook event has been previously handled/processed.

## PayTo Agreement Events
| Event Name                 | Description                                           |
|----------------------------|-------------------------------------------------------|
| payto_agreement.activated  | The Debtor has accepted the Agreement.                |
| payto_agreement.declined   | The Debtor has declined the Agreement.                |
| payto_agreement.expired    | The Debtor has ignored the Agreement for 5 days.      |
| payto_agreement.failed     | The Agreement creation has failed.                    |
| payto_agreement.cancelled  | The Debtor or Initiator has cancelled the Agreement.  |

## PayTo Payment Events
| Event Name                         | Description                                                                                   |
|------------------------------------|-----------------------------------------------------------------------------------------------|
| payto_payment.settled              | A payment is settled once the funds have successfully cleared and settled from/to the nominated account.|
| payto_payment.failed               | A payment has failed once the funds were not successfully debited or credited from/to the nominated account. |
| payto_payment.under_investigation  | A payment enters the ‘under investigation’ state when the payer bank is not responding with a final payment status. |

## Sample payload
Once you register your webhook endpoint to Oligo, you will receive the POST webhook request with a payload similar to the following:

```json
{
  "data": {
    "id": "01888a1b-cf5c-94d9-eea6-be9209e47197",
    "body": {
      "mms_agreement_id": "d1c34076e5r014301e8b2947d9322222"
    },
    "type": "payto_agreement.activated",
    "published_at": "2020-05-05T15:15:15.150+10:00",
    "resource_uid": "biz_agreement_000123",
    "resource_type": "payto_agreement"
  },
  "links": {
    "resource": "/payto/agreements/biz_agreement_000123"
  }
}
```
