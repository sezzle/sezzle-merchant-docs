
---
title: "Canada In-Store"
excerpt: "Canada In-Store"
slug: "canada-in-store"
category: 6102e1a0ab9a5c000f95e56f
---



# Canada In-Store

## In-Store Direct API, for Canadian Merchants

### Overview

The integrated POS checkout API enables merchants to send a Sezzle Checkout link to a customer via text or email and then receive payment updates on their POS when the checkout has been completed.
<div style="display:flex;align-items:flex-start;margin-bottom:10px;padding-left:28px">
<img src="https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/canada_instore/Text_Invoice.jpeg?raw=true" alt="sezzle text invoice" width="40%" />
<div style="display:inline;width:20px;"></div>
<img src="https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/canada_instore/Email_Invoice.jpeg?raw=true" alt="sezzle email invoice" width="40%" />
</div>
The customer can click on the Sezzle URL in the text or email to complete the checkout:
### Customer experience for new Sezzle users<br/>
<img src="https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/canada_instore/New User Experience.png?raw=true" alt="new Sezzle user experience" width="75%" /><br/><br/>
Customer experience for existing Sezzle users<br/>
<img src="https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/canada_instore/Existing User Experience.png?raw=true" alt="existing Sezzle user experience" width="60%" /><br/>

## Direct API

### Checkout Steps

1. Merchant Merchant [creates a session](https://sezzle-group.readme.io/sezzle-integration/reference/postv2session)
* Set order `intent` to `AUTH`
* The customer checkout is texted or emailed as set in `send_checkout_url`
2. Sezzle returns order uuid and checkout URL
3. Customer receives Sezzle URL via text or email to check out
4. Customer completes the Sezzle checkout
5. Merchant receives order-complete notification
* You can either [get order details](https://sezzle-group.readme.io/sezzle-integration/reference/getv2order) or subscribe to [webhooks](https://sezzle-group.readme.io/sezzle-integration/reference/postv2webhooks)
6. (optional) Update the order [reference](https://sezzle-group.readme.io/sezzle-integration/reference/patchv2checkout) 
7. Capture the [payment](https://sezzle-group.readme.io/sezzle-integration/reference/postv2capturebyorder)
7. (optional) Expire the checkout manually
* [Delete the checkout](https://sezzle-group.readme.io/sezzle-integration/reference/deletev2deletecheckoutbyorder) if the customer has not completed the checkout and the merchant has canceled the order
8. (optional) Refund the [order](https://sezzle-group.readme.io/sezzle-integration/reference/postv2refundbyorder)
