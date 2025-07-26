---
id: get-agreement-status
title: Get Agreement Status
sidebar_label: Get Agreement Status
description: Get Agreement Status
sidebar_position: 3
---

# Get Agreement Status

## Overview

The agreement approval is an async flow, so your system needs to build a mechanism to check the status of the agreement after it has been created.

There are 2 ways to get the status of the agreement:


### 1. Listen to the webhook event `payto_agreement.*`
You can find more details about the webhook events in the [PayTo Webhook](../payto-webhook.md) documentation.

### 2. Poll the show agreement endpoint

**GET** [/payto/agreements](/api#tag/Agreements/paths/~1payto~1agreements~1%7Bagreement_uid%7D/get)


## Try It

You can test this endpoint using the following cURL command:

```bash
curl --request GET \
  --url 'https://api-sandbox.oligo.com.au/v1/payto/agreements/<your_agreement_uid>' \
  --header 'accept: application/json' \
  --header 'authorization: Bearer <your_token>' \
  --header 'content-type: application/json'
```
