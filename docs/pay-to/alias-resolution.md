---
id: alias-resolution
title: Alias Resolution
sidebar_label: Alias Resolution
description: Alias Resolution
sidebar_position: 4
---

# Alias Resolution

**POST** [/payto/alias_resolution](/api#tag/Alias-Resolution/paths/~1payto~1alias_resolution/post)

---
## Overview
The PayTo Alias Resolution endpoint is a compulsory service that must be utilised before establishing a PayTo agreement when PayID is used as an account_identifier type such as alias_phone, alias_email, alias_abn, and alias_organisation_identifier.

The service offers two primary benefits:

1. Enhanced confidence: Provide real-time PayID validation feedback during the checkout process, increasing confidence in the transaction's accuracy.
2. Fraud prevention: Mitigate illegitimate activity by leveraging our integrated fraud prevention measures.

## Implementation Guidelines
1. A PayTo agreement creation request should only be initiated once the PayID has successfully been validated (i.e. 200 response from POST /payto/alias_resolution)

2. A PayTo agreement creation request should not be initiated when the alias resolution request return any other response (i.e. 4XX or 5XX)

3. The "display_name" associated with a registered PayID is provided in the 200 response and can be used as validation against your customer record prior to creating a PayTo agreement

4. The "display_name" should not be displayed to the end customer


## Try It

You can test this endpoint using the following cURL command:

```bash
curl --request POST \
  --url 'https://api-sandbox.oligo.com.au/v1/payto/alias_resolution' \
  --header 'accept: application/json' \
  --header 'authorization: Bearer <your_token>' \
  --header 'content-type: application/json' \
  --data '{
    "type": "alias_phone",
    "value": "+61-411222333",
    "requester": {
      "id": "user_ACBDEFGHIJKLMNOP",
      "remote_ip": "192.0.2.146"
    }
  }'
```
