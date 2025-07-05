---
id: create-agreement
title: Create Agreement
sidebar_label: Create PayTo Agreement
description: Create a PayTo Agreement
sidebar_position: 2
---

# Create Agreement

**POST** `/payto/agreements`

Create a new PayTo agreement for initiating recurring or one-time payments between a debtor and a creditor.

---

## Request Headers

```http
Authorization: Bearer <access_token>
Content-Type: application/json
Accept: application/json
```

---

## Request Body

| Field                         | Type   | Required | Description                                                                                                                                                                                                                                                                                                              |
|-------------------------------|--------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `uid`                         | string | Yes      | The supplied unique identifier for the Agreement. <br/>This identifier ensures agreement uniqueness between integrator systems and Oligo.                                                                                                                                                                                |
| `purpose`                     | string | Yes      | Select from 12 purposes to define the nature of the PayTo Agreement with your customer: <br/> <li> mortgage, utility, loan, dependant_support, gambling, retail, salary, personal, government, pension, tax, other </li> <br/> The purpose should reflect the reason the PayTo agreement is being proposed.              |
| `description`                 | string | Yes      | A short description for the agreement.                                                                                                                                                                                                                                                                                   |
| `debtor`                      | object | Yes      | The party from whom the payment is taken.                                                                                                                                                                                                                                                                                |
| `payment_terms`               | object | Yes      | Specifies the type, frequency, and amount of the agreement.                                                                                                                                                                                                                                                              |
| `resolution_requested_before` | string | No       | Requested resolution (accept/decline) deadline for this agreement. It will be provided in any notification sent to the debtor. Value must be an ISO8601 date-time in UTC timezone. This time is for informational purposes only and does not affect the expiry time which occurs after 5 days of inaction by the debtor. |
| `validity_start_date`         | string | No       | Start date for the validity of the agreement. If specified, the agreement will be valid from 00:00:00 Australia Sydney time on the specified date. If no date is specified, the current date will be populated.                                                                                                          |

### Some detailed fields explanation:

#### 1. debtor: account_identifier type
Select from 5 account identifier types:

- bban (BSB and account number)
- alias_email (Email PayID)
- alias_phone (Phone PayID)
- alias_abn (ABN PayID)
- alias_organisation_identifier (Org. ID PayID)

:::danger[Take care]

1. There is a limit of 6 agreement creation attempts within 24 hours per debtor account.

2. You have to call the [Alias Resolution](./alias-resolution.md) endpoint to validate the name associated with a PayID before using the alias in the account_identifier. You don't need to call it if you use bban (BSB and account number) as the debtor.account_identifier.type.

:::


#### 2. payment_terms: frequency
The frequency at which payments will be made. Frequency can be used in conjunction with ‘count’ to allow more than one payment per period.

The frequency must be one of the following:

- adhoc: A payment on request or as necessary.
- daily: A payment daily. Valid until 23:59:59.999 Australia Sydney time the same day.
- weekly: A payment once per week. Valid until 23:59:59.999 Australia Sydney time on the 7th day.
- fortnightly: A payment every 2 weeks. Valid until 23:59:59.999 Australia Sydney time on the 14th day.
- monthly: A payment once per month. Valid for the month, up until 23:59:59.999 Australia Sydney time the day before the day noted in the `validity_start_date`.
- quarterly: A payment every 3 months. Valid for 3 months, up until 23:59:59.999 Australia Sydney time the day before the day noted in the `validity_start_date`.
- semi_annual: A payment twice per year. Valid for 6 months, up until 23:59:59.999 Australia Sydney time the day before the day noted in the `validity_start_date`.
- annual: A payment once per year. Valid for 12 months, up until 23:59:59.999 Australia Sydney time the day before the day noted in the `validity_start_date`.

NOTE: For periods starting at the end of a month with 31 days, when calculating a period during a shorter month, the end of the shorter month will be the period end.

### Example Request

```json
{
  "uid": "<your_agreement_uid>",
  "purpose": "loan",
  "description": "Monthly subscription for premium plan",
  "debtor": {
    "party_name": "John Doe",
    "account_identifier": {
      "type": "bban",
      "value": "123456-78901234"
    }
  },
  "payment_terms": {
    "type": "fixed",
    "frequency": "monthly",
    "amount": 5000,
    "first_payment_date": "2025-07-01",
    "last_payment_date": "2026-06-01"
  }
}
```

---

## Successful Response

Returns a JSON object containing the unique ID of the created agreement and its initial status.

```json
{
  "data": {
    "uid": "<your_agreement_uid>",
    "state": "pending",
    "created_at": "2025-07-02T19:29:39+10:00",
    "purpose": "loan",
    "payment_terms": {
      "type": "fixed",
      "frequency": "monthly",
      "count": 1,
      "max_amount": null,
      "amount": 5000,
      "first_payment_date": "2025-08-01",
      "last_payment_date": "2026-06-01"
    },
    "debtor": {
      "party_name": "John Doe",
      "ultimate_party_name": "John Doe",
      "account_identifier": {
        "type": "bban",
        "value": "123456-78901234"
      }
    },
    "creditor": {
      "party_name": "The name of the creditor party for this agreement.",
      "ultimate_party_name": "The name of the creditor party for this agreement.",
      "account_identifier": {
        "type": "bban",
        "value": "123456-98765432"
      }
    },
    "initiator": {
      "name": "initiator name",
      "legal_name": "initiator legal_name",
      "abn": "56192755287"
    },
    "description": "Monthly subscription for premium plan",
    "validity_start_date": "2025-07-02",
    "validity_end_date": null,
    "state_caused_by": "initiator",
    "links": {
      "self": "/payto/agreements/<your_agreement_uid>"
    }
  }
}
```

---

## Agreement Status Lifecycle

| Status      | Description                                                                               |
|-------------|-------------------------------------------------------------------------------------------|
| `Pending`   | Agreement has been created but is not yet active.                                         |
| `Created`   | Agreement was created successfully and awaits debtor authorization.                       |
| `Active`    | The Debtor has accepted the terms of the Agreement.                                       |
| `Declined`  | The Debtor has declined the terms of the Agreement.                                       |
| `Expired`   | The Debtor has ignored the Agreement for 5 days (expires on day 6)                        |
| `Cancelled` | The Agreement has been cancelled.                                                         |
| `Suspended` | Agreement is temporarily suspended and cannot be used.                                    |

_Note: `declined`, `expired`, and `cancelled` are terminal states and cannot be reactivated._

---

## Webhook Events

Once the agreement is created, the following webhook events may be triggered:

- `payto_agreement.activated`
- `payto_agreement.declined`
- `payto_agreement.expired`
- `payto_agreement.failed`

Each event includes the agreement details and updated status.

---

## Try It

You can test this endpoint using the following cURL command:

```bash
curl --request POST \
  --url 'https://api-sandbox.oligo.com.au/v1/payto/agreements' \
  --header 'accept: application/json' \
  --header 'authorization: Bearer <your_token>' \
  --header 'content-type: application/json' \
  --data '{
    "uid": "<your_agreement_uid>",
    "purpose": "loan",
    "description": "Monthly subscription for premium plan",
    "debtor": {
      "party_name": "John Doe",
      "account_identifier": {
        "type": "bban",
        "value": "123456-78901234"
      }
    },
    "payment_terms": {
      "type": "fixed",
      "frequency": "monthly",
      "amount": 5000,
      "first_payment_date": "2026-07-01",
      "last_payment_date": "2027-06-01"
    }
  }'
```
