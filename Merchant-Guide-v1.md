---
title: "Merchant Guide v1"
excerpt: "Sezzle Merchant Guide v1"
slug: "merchant-guide-v1"
category: 6102e1a0ab9a5c000f95e56f
---

# Introduction (v1)

<aside class="content warning">
You are viewing Version 1 of the Sezzle API. Check out the <a class="external-link" href="/">latest version</a>!
</aside>

The Sezzle API v1 is intended for merchants interested in accepting Sezzle as a payment option. The Sezzle [Integration Flow](#integration-flow) illustrates the user payment interaction.

Sezzle supports individually authorized transactions for a single purchase of goods or services.

Sezzle offers integrations with some of the most popular eCommerce platforms.<br/>
Note that <sup>v2</sup> indicates support for <a class="external-link" href="/">v2 API</a> only.<br/>
**Please choose your platform to see the relevant documentation:**<br/>
1. [3DCart](#3dcart)<br/>
2. [BigCommerce](/#bigcommerce) <sup>v2</sup><br/>
3. [Bold Cashier](#bold-cashier)<br/>
4. [BuyItLive](#buyitlive)<br/>
5. [CommentSold](#commentsold)<br/>
6. [Magento 1](#magento-1)<br/>
7. [Magento 2](/#magento-2) <sup>v2</sup><br/>
8. [Mojo](/#mojo) <sup>v2</sup><br/>
9. [NopCommerce](#nopcommerce)<br/>
10. [Salesforce Commerce Cloud](#salesforce-commerce-cloud) <sup>v2</sup><br/>
11. [Shopify](#shopify)<br/>
12. [Shopware 5](/#shopware-5) <sup>v2</sup><br/>
13. [Wix](/#wix) <sup>v2</sup><br/>
14. [WooCommerce](#woocommerce)<br/>
15. [Zoey](#zoey)<br/>

Field or header names in bold case followed by an asterisk are required. (For example, **this_is_required\*** is a required field whereas this_is_optional is not.)

If you have any questions regarding our API, please reach out to our team by email at dev@sezzle.com.

<aside class="notice">
You should have an approved Sezzle account to start the integration process. Please visit our <a class="external-link" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have a Sezzle account already.
</aside>

# Integration Flow

## Overview of Integration Flow
![payment flow](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/api-flow.png?raw=true)

## Explanation of payment flow

1. Merchant calls `/v1/checkouts` to send cart data to Sezzle.
2. Sezzle returns URL to redirect consumer to make payment at Sezzle checkout.
3. Merchant redirects the consumer to Sezzle.
4. When the consumer completes the Sezzle checkout flow, they are redirected back to merchant's website.
5. Alternatively, on approval, the consumer is redirected from Sezzle checkout to merchant's website and merchant captures the order by calling `/v1/complete`.

# Authentication

## Obtain Authentication Token

> To authorize, use the following format:

> ### Request Body

```json
{
    "public_key": "myPublicKey",
    "private_key": "myPrivateKey"
}
```

> Make sure to replace `keys` with your API keys from your Merchant Dashboard.

> ### Response Body

```json
{
    "token": "authToken",
    "expiration_date": "2017-01-01T01:30:25.388940303Z",
    "merchant_uuid": "merchant1234567890"
}
```

`POST https://gateway.sezzle.com/v1/authentication`

Sezzle uses scoped API keys to allow access to the API. You can find/generate these keys on your [merchant dashboard](https://dashboard.sezzle.com/merchant) once you have been approved by Sezzle.

Once you have a valid token, it must be used as a Header for subsequent requests to our API, using the format below.

`Authorization: Bearer authToken`


<aside class="notice">
You must replace the tokens you obtain from the Sezzle Authorization endpoint every 120 minutes. However, we do not restrict you to a single token, and you can obtain a new one at any time.
</aside>



# Configuration

## Setting your configuration

> ### Request Body

```json
{
    "webhook_url": "https://yourdomain.com/webhook"
}
```

> There is no response body for this request. If successful, we return an HTTP 200 Status OK.

`POST https://gateway.sezzle.com/v1/configuration`

At this time, Sezzle only allows configuration of the URL that we send our webhooks to.

Parameter | Type | Description
--------- | ---- | -----------
**webhook_url\*** | string | A URL for us to send our webhooks to.



# Checkouts

## Create a Checkout

> ### Request Body

```json
{
    "amount_in_cents": 12999,
    "currency_code": "USD",
    "order_reference_id": "Ref123456789",
    "order_description": "Order #1800",
    "checkout_cancel_url": "https://sezzle.com/cart",
    "checkout_complete_url": "https://sezzle.com/complete",
    "customer_details":
    {
        "first_name": "John",
        "last_name": "Doe",
        "email": "john.doe@sezzle.com",
        "phone": "5555045294"
    },
    "billing_address": {
        "name": "John Doe",
        "street": "123 W Lake St",
        "street2": "Unit 104",
        "city": "Minneapolis",
        "state": "MN",
        "postal_code": "55408",
        "country_code": "US",
        "phone_number": "5555045294"
    },
    "shipping_address": {
        "name": "John Doe",
        "street": "123 W Lake St",
        "street2": "Unit 104",
        "city": "Minneapolis",
        "state": "MN",
        "postal_code": "55408",
        "country_code": "US",
        "phone_number": "5555045294"
    },
    "requires_shipping_info": false,
    "items": [
        {
          "name": "widget",
          "sku": "sku123456",
          "quantity": 1,
          "price": {
              "amount_in_cents": 1000,
              "currency": "USD"
          }
        }
    ],
    "discounts": [
        {
          "name": "20% off",
          "amount": {
              "amount_in_cents": 1000,
              "currency": "USD"
          }
        }
    ],
    "tax_amount": {
        "amount_in_cents": 1000,
        "currency": "USD"
    },
    "shipping_amount": {
        "amount_in_cents": 1000,
        "currency": "USD"
    },
    "metadata": {
        "location_id": "123",
        "store_name": "Downtown Minneapolis",
        "store_manager": "Jane Doe"
    }
}
```

> ### Response Body

```json
{
    "checkout_url": "https://checkout.sezzle.com/?id=<checkout_id>",
    "checkout_uuid": "<checkout_id>"
}
```

### HTTP Request

`POST https://gateway.sezzle.com/v1/checkouts`

 This checkout endpoint creates a checkout in our system, and returns the URL that you should redirect the user to.
 We suggest you provide as much optional information about the user as you have available, since this will speed up our checkout process and increase conversion.

 Sezzle is able to handle the entire checkout process after a Checkout has been provided. However, if your flow requires that the user confirm their checkout on
 your site after being approved by Sezzle, you may include the `merchant_completes` parameter with the checkout request. In this flow, Sezzle will not complete the
 order unless you make a [checkout completion](#complete-a-checkout-optional) request.

### Checkout Object

Parameter | Type | Description
--------- | ------- | -----------
**amount_in_cents\*** | int | The amount of the checkout. Must be at least `100`
**currency_code\*** | string| The currency code of the checkout
**order_reference_id\*** | string | A reference to this checkout in your systems
**order_description\*** | string | A user-facing description for this checkout
**checkout_cancel_url\*** | string | The URL we should redirect the customer to in the case of a cancellation
**checkout_complete_url\*** | string | The URL we should redirect to in the case of a successful checkout
checkout_expiration | string | Checkout expiration in ISO 8601 date/time format
customer_details | object | The customer in the checkout
billing_address | object | The billing address of the checkout
shipping_address | object | The shipping address of the checkout
requires_shipping_info | boolean | Flag to indicate if you would like us to collect shipping information for this checkout from the customer. If omitted, will default to false
items | array | The items being purchased
discounts | array | The discounts applied. Must be included in the total
tax_amount | object | The taxes applied to this checkout. Must be included in the total
shipping_amount | object | The shipping fees applied to this checkout. Must be included in the total
merchant_completes | boolean | Flag to determine whether this checkout must be completed by the merchant. If omitted, will default to false
metadata | object | Object for any custom data you want to submit with the checkout. You are not limited to the key-value pairs shown in the example, and you may use any key-value pairs you like
send_checkout_url | object | A Notification object for sending checkout URL to the customer
locale | string | Localizes the checkout. Accepted values are `en-US` (English, United States), `en-CA` (English, Canada), and `fr-CA` (French, Canada). Defaults to `en-US` if not provided.

<aside class="notice">
Your order reference ID must be unique identifier, and may only contain 'a-Z', '0-9', '-', and '_'.
</aside>

### Customer Details Object

Parameter | Type | Description
--------- | ------- | -----------
first_name | string | The user's first name
last_name | string | The user's last name
email | string | The user's email address
phone | string | The user's phone number

### Address Object
This is used for both billing and shipping

Parameter | Type | Description
--------- | ------- | -----------
name | string | The name on the address
street | string | The street and number of the address
street2 | string | The apt or unit
city | string | The city
state | string | The 2 character state code
postal_code | string | The postal delivery code
country_code | string | The 2 character country code
phone_number | string | The phone number at the delivery location

### Item Object

Parameter | Type | Description
--------- | ------- | -----------
name | string | The name of the item
sku | string | The sku identifier
quantity | int | The quantity purchased
price | object | The price object

### Tax Amount Object

Parameter | Type | Description
--------- | ------- | -----------
tax_amount | object | A price object

### Shipping Amount Object

Parameter | Type | Description
--------- | ------- | -----------
shipping_amount | object | A price object

### Discount Object

Parameter | Type | Description
--------- | ------- | -----------
name | string | The description of the discount
amount | object | A price object

### Price Object

Parameter | Type | Description
--------- | ------- | -----------
amount_in_cents | int | The amount of the item in pennies
currency | string | The 3 character currency code as defined by ISO 4217

### Metadata Object
Use the metadata object for any additional information you would like to attach to the checkout. All values must be strings.

Parameter | Type | Description
--------- | ------- | -----------
**some_field_name** | string | Custom metadata field
**some_other_field_name** | string | Custom metadata field

### Notification Object
A valid notification object contains at a minimum a phone or an email.

Parameter | Type | Description
--------- | ------- | -----------
to_sms_phone | string | The SMS phone number of the notification
to_email | string | The email address of the notification
language | string | The 2-character ISO 639 langauge code of the notification. Acceptable values are "en" and "fr". Will default to English if not provided
## Complete a Checkout (optional)

> ### Success response

> Echoes the given Checkout.

> ### Rejection response

```json
{
	"status": 409,
	"id": "checkout_expired",
	"message": "checkout not completed within time limit"
}
```

`POST https://gateway.sezzle.com/v1/checkouts/{order_reference_id}/complete`

If you pass `true` to `merchant_completes` in our Create Checkout flow, then you must call our Complete Checkout endpoint.

For some checkouts, a merchant may need to have the user return to their site for additional steps before completing the purchase. If this is the case,
the order completion endpoint is used to complete the Checkout with Sezzle. From the time the user is redirected back to the Merchant's site, you must
make the request to complete the checkout within 30 minutes, or the checkout will be canceled by Sezzle. If the checkout has expired, we will return the
rejection response on the right, with a Status 409. The default expiration period for new orders may be extended up to 7 days in your [merchant dashboard](https://dashboard.sezzle.com/merchant/settings/ecommerce).

There are two non-error responses expected. Either an HTTP 200, which echoes all accepted fields given at Checkout creation, or a rejection message.

# Orders

## Order Details

> ### Order Details Response Body

```json
{
    "created_at": "2018-11-02T20:09:59Z",
    "captured_at": "2018-11-02T20:18:50Z",
    "capture_expiration": "2018-11-02T20:48:45Z",
    "description": "Description of order",
    "amount_in_cents": 20000,
    "usd_amount_in_cents": 20000,
    "customer_amount_in_cents": 20000,
    "currency_code": "USD",
    "customer_currency_code": "USD",
    "reference_id": "Ref123456789",
    "customer": {
        "first_name": "John",
        "last_name": "Doe",
        "email": "john.doe@sezzle.com",
        "phone": "5555045294"
    },
    "shipping_address": {
        "name": "John Doe",
        "phone_number": "5555045294",
        "street": "123 W Lake St",
        "street2": "Unit 104",
        "city": "Minneapolis",
        "state": "MN",
        "postal_code": "55408",
        "country_code": "US"
    },
    "billing_address": {
        "name": "John Doe",
        "phone_number": "5555045294",
        "street": "123 W Lake St",
        "street2": "Unit 104",
        "city": "Minneapolis",
        "state": "MN",
        "postal_code": "55408",
        "country_code": "US"
    },
    "refunds": [
      {
        "amount": {
          "amount_in_cents": 10000,
          "currency": "USD"
        },
        "created_at": "2018-11-02T20:09:59Z",
        "is_full_refund": false,
        "order_reference_id": "Ref123456789",
        "refund_id": "52b2O9Lv-8",
        "refund_reason": "broken"
      }
    ],
    "metadata": {
      "location_id": "123",
      "store_name": "Downtown Minneapolis",
      "store_manager": "Jane Doe"
    },
    "items": [
        {
          "name": "widget",
          "sku": "sku123456",
          "quantity": 1,
          "price": {
              "amount_in_cents": 1000,
              "currency": "USD"
          }
        }
    ]
}
```

`GET https://gateway.sezzle.com/v1/orders/{order_reference_id}`

Once an order is created, you can retrieve the details of the order using this endpoint.

### Optional Query Parameter(s)
Parameter | Type    | Values | Description
--------- | ------- | ----------------- | ---------------------
include-shipping-info | string | true or false | If your checkout post data required us to collect shipping information from the customer, then you can request that information alongside the order details.

## Order Refunds

> ### Refund Request Body

```json
{
	"refund_id": "41a2O9Lv-7",
	"amount": {
        "amount_in_cents": 500,
        "currency": "USD"
    },
	"refund_reason": "Item returned by user"
}
```


`POST https://gateway.sezzle.com/v1/orders/{order_reference_id}/refund`

Sezzle allows refunds for orders either through our Merchant Dashboard or through the API.
If the refund is processed through the dashboard, a [webhook](#order-webhooks) will be sent to your system.
In either case, Sezzle allows for either partial or complete refunds. Refund amounts are relative to the order
total, not the amount that has been paid by the shopper.

### Refund Request

Parameter | Type | Description
--------- | ---- | -----------
**amount\*** | object | A price object that defines the amount to be refunded. Amount may not be 0, negative, or exceed the total order amount. Currency must either be the order's currency or the customer's paying currency. This field is optional if the `is_full_refund` parameter is true.
refund_id | string | UUID for the Refund. Must be unique to a Merchant.
refund_reason | string | A reason for the refund.
is_full_refund | boolean | Overrides `amount`.  If true, the order will be fully refunded. If omitted, will default to false

# Reporting

## Settlement Reports

These endpoints allow you to view a list of payout summaries or a detailed report of an individual payout. 

>### Settlement Summaries Response Body

```json
[
    {
        "uuid": "b7916fbe-f30a-4435-b411-124634287a8ca",
        "payout_currency": "USD",
        "payout_date": "2019-12-09T15:52:33Z",
        "net_settlement_amount": 9370,
        "forex_fees": 0,
        "status": "Complete"
    },
    {
        "uuid": "c51343hba-d54b-5641-e341-15235523b3at",
        "payout_currency": "USD",
        "payout_date": "2019-12-10T15:52:33Z",
        "net_settlement_amount": 23470,
        "forex_fees": 0,
        "status": "Complete"
    }
]
```

### Settlement Summaries Request

`GET https://gateway.sezzle.com/v1/settlements/summaries`


Query Parameter | Description
--------------- | -----------
**start-date\*** | The UTC start date for the report. Must be in yyyy-mm-dd format.
end-date | The UTC end date for the report. Must be in yyyy-mm-dd format. If omitted, will default to the current date.
offset | The offset for the report. Limit is 20.
currency-code | The ISO-4217 currency code selected by users at checkout. If omitted, will default to USD.

>### Settlement Details Response

```text
total_order_amount,total_refund_amount,total_fee_amount,total_returned_fee_amount,total_chargeback_amount,total_chargeback_reversal_amount,total_interest_transfer_amount,total_correction_amount,total_referral_revenue_transfer_amount,total_bank_account_withdrawals,total_bank_account_withdrawal_reversals,forex_fees,net_settlement_amount,payment_uuid,settlement_currency,payout_date,payout_status
703.20,-5.00,-43.80,.30,0.00,0.00,-4.30,1.71,10.00,100.00,-100.00,0.00,693.61,a5c13qt1-4126-41d3-2fq8-9ca431f51431,USD,2019-11-02 00:05:00 +0000 UTC,Complete
type,order_capture_date,order_created_at,event_date,order_uuid,customer_order_id,external_reference_id,amount,posting_currency,type_code,chargeback_code,sezzle_order_id
ORDER,2019-11-01T19:09:50Z,2019-11-01T19:09:50Z,2019-10-22T19:09:50Z,bm99f-31vu1-kg00e-rae1g,1,12345,500.00,USD,001,,66d78e86-fd96-4266-9217-b769c102a0a0
ORDER,2019-11-01T19:09:50Z,2019-11-01T19:09:50Z,2019-10-22T19:09:50Z,va13d-474s9-3000e-nungg,13,12346,200.00,USD,001,,5e0d4886-8c3d-4d4e-901a-2046a06c1e0f
ORDER,2019-11-01T20:00:01Z,2019-11-01T00:00:01Z,2019-11-01T00:00:01Z,as41g-4v4s9-3000e-nunh0,1,12347,1.40,USD,001,,a2c1a142-96ad-48c9-93d2-1acaaee9f073
ORDER,2019-11-01T20:00:01Z,2019-11-01T20:00:01Z,2019-11-01T20:00:01Z,as62l-5ptqs-9g00e-pvk10,2,12348,1.80,USD,001,,3f62dcba-f5a4-41be-ad8f-53e938b5f310
FEE,2019-11-01T19:09:50Z,2019-11-01T19:09:50Z,2019-11-01T19:09:50Z,bm99f-31vu1-kg00e-rae1g,1,12345,-30.00,USD,003,,66d78e86-fd96-4266-9217-b769c102a0a0
FEE,2019-11-01T19:09:50Z,2019-11-01T19:09:50Z,2019-11-01T19:09:50Z,va13d-474s9-3000e-nungg,13,12346,-12.00,USD,003,,5e0d4886-8c3d-4d4e-901a-2046a06c1e0f
FEE,2019-11-01T20:00:01Z,2019-11-01T00:00:01Z,2019-11-01T20:00:01Z,as41g-4v4s9-3000e-nunh0,1,12347,-1.20,USD,003,,a2c1a142-96ad-48c9-93d2-1acaaee9f073
FEE,2019-11-01T20:00:01Z,2019-11-01T20:00:01Z,2019-11-01T20:00:01Z,as62l-5ptqs-9g00e-pvk10,2,12348,-0.60,USD,003,,3f62dcba-f5a4-41be-ad8f-53e938b5f310
REFUND,2019-10-22T19:09:50Z,2019-10-22T19:09:50Z,2019-11-01T19:09:50Z,bm5rm-vg2js-1tsky-c2dsky,8,12344,5.00,USD,002,,e4194956-de70-4958-9da4-6c05f276fdab
RETURNED_FEE,2019-10-22T19:09:50Z,2019-10-22T19:09:50Z,2019-11-01T19:09:50Z,bm5rm-vg2js-1tsky-c2dsky,7,12344,.30,USD,004,,e4194956-de70-4958-9da4-6c05f276fdab
CORRECTION,,,2019-11-01T17:00:01Z,,,,-1.29,,007,
CORRECTION,,,2019-11-01T17:00:01Z,,,,3.00,,007,
INTEREST_TRANSFER,,,2019-11-01T18:00:01Z,,,,-4.30,,008,
REFERRAL_REVENUE_TRANSFER,,,2019-11-01T15:00:01Z,,,,10.00,,009,
BANK_ACCOUNT_WITHDRAWAL,,,2019-11-02T00:05:00Z,,,,100.00,,010,
BANK_ACCOUNT_WITHDRAWAL_REVERSAL,,,2019-11-02T00:05:00Z,,,,-100.00,,011,
```

### Settlement Details Request
`GET https://gateway.sezzle.com/v1/settlements/details/{payout_uuid}`

<aside class="notice">
  The <code>payout_uuid</code> is the <code>uuid</code> returned by the
  Settlement Summaries Response.
</aside>

Query Parameter | Description
--------------- | -----------
metadata | An optional comma-separated list of metadata keys. To add a metadata key as a column to the report line items, include the key in this list.  When applicable, the value of the metadata key will be added to the line item.  If no line items contain the metadata key, the key will not be added as a column.

The settlement details response contains two sections. The first two rows are a summary of the payout. The remaining rows contain the individual line items that contributed to the payout. 

**Summary column definitions:**

Column Header | Description
------------- | -----------
Total order amount | The sum of all orders on this payout. 
Total refund amount | The sum of all refunds on this payout.
Total fee amount | The sum of all fees on this payout.
Total returned fee amount | The sum of all returned fees on this payout.
Total chargeback amount | The sum of all chargebacks on this payout.
Total chargeback reversal amount | The sum of all chargeback reversals on this payout.
Total interest transfer amount | The sum of all interest transfers on this payout. If you are not participating in the interest program, this field will be omitted. 
Total correction amount | The sum of all corrections on this payout. 
Total referral revenue transfer amount | The sum of all referral revenue transfers on this payout.
Total bank account withdrawal amount | The sum of all bank account withdrawals. 
Total bank account withdrawal reversal amount | The sum of all bank account withdrawal reversals, which reflect a bank account withdrawal that has failed. 
Forex fees | The cost of foreign exchange fees associated with this payout. 
Net settlement amount | Net amount of settlement.
Payment uuid | The UUID for this payout. 
Settlement currency | The currency in which this payout was sent.
Payout date | The date this payout was sent. 
Payout status | The current status of this payout. 

**Line item column definitions:**

Column Header | Description
------------- | -----------
Type | Describes the type of event (Order, Fee, Refund, etc.).
Order capture date | The date at which the order was captured. This field is empty if the order has not yet been captured. 
Order created at | The date at which the order was created. 
Event date | The date at which the event took place. 
Order uuid | The uuid associated with the order. 
Customer order id | The customer's order number.
External reference id | The external reference ID submitted with the order. 
Amount | The amount of the event. 
Posting currency | The customer's currency code. 
Type code| A numeric code that corresponds with the Type field.
Chargeback code | A numeric code that corresponds with the type of chargeback submitted. 
Sezzle order ID | The internal ID Sezzle has assigned to this order.

**Line item event type definitions:**

Type | Description | Type Code 
---- | ----------- | ---------
ORDER | A completed order with Sezzle. | 001 
REFUND | An order that has been refunded. | 002
FEE | The fee assessed by Sezzle for a given order. | 003
RETURNED_FEE | A fee refunded by Sezzle. | 004
CHARGEBACK | A chargeback resulting from a disputed order. | 005
CHARGEBACK_REVERSAL | A reversal of a chargeback resulting from a disputed order. | 006
CORRECTION | A manual correction to a payout. | 007
INTEREST_TRANSFER | A transfer from the Sezzle interest account. | 008
REFERRAL_REVENUE_TRANSFER | A payment earned from Sezzle's merchant referral program. | 009
BANK_ACCOUNT_WITHDRAWAL | A withdrawal of funds from your bank to cover a negative balance with Sezzle. | 010
BANK_ACCOUNT_WITHDRAWAL_REVERSAL | A failed BANK_ACCOUNT_WITHDRAWAL. | 011

## Interest Account Reports

Sezzle gives merchants the option to enroll in an interest account program. If you are enrolled in the interest account program, you can use these endpoints to get the current balance and activity on the interest account. Fractions of cents are tracked to properly calculate daily interest accrual even if the interest balance is low.

> ### Interest Account Balance Response Body

```json
{
  "interest_balance": 5183.4624
}
```

### Interest Account Balance Request
`GET https://gateway.sezzle.com/v1/interest/balance`

Query Parameter | Description
--------------- | -----------
currency-code | The ISO-4217 currency code of the interest account. If omitted, will default to USD.

> ### Interest Account Activity Response Body

```text
type,event_date,interest_account_change_amount,interest_account_balance_after_change
INTEREST_PAYOUT,2019-12-21T19:10:00Z,122.8718,5101.4676	
INTEREST_WITHDRAWAL,2019-12-21T19:20:00Z,-26.1000,5075.3676	
INTEREST_ACCRUAL,2019-12-21T19:15:00Z,1.0702,5182.3922
INTEREST_ACCRUAL,2019-12-22T19:15:00Z,1.0702,5183.4624
```


### Interest Account Activity Request
`GET https://gateway.sezzle.com/v1/interest/activity`

Query Parameter | Description
--------------- | -----------
**start-date\*** | The start date for the report. Must be in yyyy-mm-dd format. 
end-date | The end date for the report. Must be in yyyy-mm-dd format. If omitted, will default to the current date.
offset | The offset for the report.
currency-code | The ISO-4217 currency code of the interest account. If omitted, will default to USD.

# Webhooks

## Order Webhooks

> ### Webhook

```json
{
	"time": "2017-10-19T00:33:10.548372055Z",
	"uuid": "02c5a2a0-8394-4b45-80b3-52d40c494322",
	"type": "order_update",
	"event": "order_complete",
	"object_uuid": "Ref123456789",
	"refund_id": "szl-a0293Pn-3948-80b3-ao34JAia39zQ",
	"refund_amount": {
        "amount_in_cents": 500,
        "currency": "USD"
    }
}
```

Because the majority of a consumer's checkout process happens on Sezzle's pages, our API uses webhooks to communicate
information about checkout updates, completions, or refunds to your system.

We expect any response in the 200 range on submitting webhooks.

### Order Webhook Object
Parameter | Type | Description
--------- | ------- | -----------
time | string | The time (UTC) at which the Webhook was generated.
uuid | string | A unique identifier for the webhook.
type | string | The high-level category. For example, `order_update`
event | string | The specific action. For example, `order_complete`
object_uuid | string | The ID for the Checkout/Order.
refund_id | string `optional` | Unique ID for a refund. Included if the webhook event is order_refund.
refund_amount | object `optional` | Price object. Included if the webhook event is order_refund.

<aside class="notice">
For order update webhooks, the `object_uuid` returned is the reference ID provided during checkout creation by the merchant
</aside>

### Order Update Events/Types
Type | Event | Description
----- | ----- | -----
order_update | order_complete | The checkout was completed successfully
order_update | order_refund | The order was refunded from the Sezzle Merchant Dashboard

# Errors

## Error Details

> ### Response Error Body

```json
{
	"status": 400,
	"id": "error_id",
	"message": "Descriptive message"
}
```

Unless otherwise specified in our documentation, Sezzle returns a standard API error object.

We attempt to keep these errors as consistent as possible, and will announce any changes in advance if they are required.

### Error Object

Parameter | Type | Description
--------- | ------- | -----------
Status | int | Matches the HTTP Status code of the response
ID | string | A programmatic identifier for the error. These rarely (if at all) change.
Message | string | A human-friendly string. These may change, and are intended to assist in debugging rather than program logic.

# Javascript SDK

The Javascript SDK is documented in the latest [API v2 documentation](/#javascript-sdk). It is supported for users of the v1 API using the same loadable page script.

<aside class="content">When using the Javascript SDK with v1, use <code>apiVersion: "v1"</code> in the Checkout constructor as shown in <a class="external-link" href="/#checkout-configuration">Checkout configuration</a>.</aside>

>### Create a Checkout

```javascript
checkout.startCheckout({
    checkout_payload: {
        "amount_in_cents": 12999,
        "currency_code": "USD",
        "order_reference_id": "Ref123456789",
        "order_description": "Order #1800",
    }
});
```

When using the Javascript SDK with v1, a checkout is created using the <a class="external-link" href="#checkout-object">Checkout Object</a>. The checkout is completed using the <a class="external-link" href="#complete-a-checkout-optional">Complete a Checkout</a> endpoint. This endpoint differs from the v2 endpoint in that it captures the total order amount and does not require a request body. Because of this, do not use the payload object shown in the example capture.
>### Complete a Checkout

```javascript
checkout.capturePayment("Ref123456789");
```

# Widget SDK

## Purpose

The Widget SDK serves to load our sales widgets to web pages. The widgets will not show unless a config is provided before the script is loaded. The repository for this project can be found at [https://github.com/sezzle/sezzle-js](https://github.com/sezzle/sezzle-js).

View our <a class="external-link" href="/#configuring-the-widgets">latest documentation</a> for configuring widgets.

# Testing
While you are working on the integration, you should test it before going live. Please use this section for information on testing.

## Sandbox
**API Endpoint** `https://sandbox.gateway.sezzle.com/v1`<br/>
**Sandbox Dashboard** `https://sandbox.dashboard.sezzle.com/merchant`
<aside class="notice">
    Credentials to log in to the sandbox dashboard are the ones you use to log in to the <b><a class="external-link" href="https://dashboard.sezzle.com/merchant/">Sezzle Merchant Dashboard</a></b>. You can create your test API keys in the sandbox dashboard.
</aside>

## Test Data
You can use the following test data to test your integration

### Bank
**Bank** `Test Bank`<br/>
**Username** `demo`<br/>
**Password** `go`<br/>

### Card
**Card Number** `4242424242424242`<br/>
**CVV/CVC** `any (3 numbers)` <br/>
**Expiration Date** `any`<br/>
**Name** `any`<br/>
**Address** `any`<br/>

### Phone and other information
1. Please use any valid phone number.
2. The expected `OTP` is `123123`.
3. Personal information does not need to be real.

# Open API
The <a class="external-link" href="https://swagger.io/specification/">OpenAPI Specification (OAS)</a> defines a standard, language-agnostic interface to RESTful APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection.

## Specification
Download the Sezzle OpenAPI Specification for <a class="external-link" href="https://sandbox.gateway.sezzle.com/swagger.json?download">Sandbox</a> or <a class="external-link" href="https://gateway.sezzle.com/swagger.json?download">Production</a>.

## Client Generator
The Sezzle OpenAPI Specification can be imported into the `Swagger Editor` to easily generate a Sezzle client in a variety of programming languages. Generate a Sezzle client for <a class="external-link" href="https://editor.swagger.io/?url=https://sandbox.gateway.sezzle.com/swagger.json">Sandbox</a> or <a class="external-link" href="https://editor.swagger.io/?url=https://gateway.sezzle.com/swagger.json">Production</a>.

# Platform Integrations


## 3DCart

This guide describes how to integrate Sezzle into your 3DCart website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your 3DCart site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your 3DCart order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.

### Integration Steps Overview

1. [Install and configure the Sezzle](#install-the-sezzle-3dcart-extension) <a class="external-link" target="_blank" href="https://app-sezzle01.3dcart.com/Home/InstallApp">3DCart App</a>
2. [Test your integration](#3dcart-live-checkout)
3. **(Optional)** [Sandbox Testing](#3dcart-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle 3DCart Extension

1. Log in to your website's 3DCart admin.
2. Get the app <a class="external-link" target="_blank" href="https://app-sezzle01.3dcart.com/Home/InstallApp">here</a>.
3. Copy+paste your `Store URL` into the input area, then click `Proceed`.
![application_authorization](images/integrations/3dcart/application-authorization.png)<br/>
4. Check the PCI Compliance box, then click `Acknowledge and Authorize the App` to start the installation.

### Admin Configuration

1. In your 3DCart admin, go to `Settings` > `Payment`.<br/>
2. Click `Select Payment Methods`. <br/>
![select_payment_methods](images/integrations/3dcart/select-payment-methods.png)<br/>
3. Turn the Sezzle switch to `On`.<br/>
4. Copy your `Public Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste it into the corresponding field in the Sezzle configuration page of your 3DCart admin.<br/>
5. Next to `Private Key`, click `Change`. Then, copy your `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste it into the corresponding field in the Sezzle configuration page of your 3DCart admin.<br/>
6. Click `Save`.<br/>
![sezzle_payment_config](images/integrations/3dcart/payment-methods.png)<br/>
7. To restrict Sezzle usage by country, click the `Exclude List` hyperlink under the Sezzle switch. <br/>
8. Click `Add Location`.<br/>
9. Select the desired country, then click `Add`. <br/>
![exclude_list](images/integrations/3dcart/exclude-list.png)<br/>
10. Installation is complete.

### 3DCart Sandbox Testing

1. In the Sezzle configuration page of your 3DCart admin, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> and check the `Test Mode` checkbox, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to checkout and select `Sezzle` as the payment method.
3. Click `Place Order` and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![checkout](images/integrations/3dcart/checkout.png)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> to see the test order you just placed.

### 3DCart Live Checkout

1. In the Sezzle configuration page of your 3DCart admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and uncheck the `Test Mode` checkbox, then save the configuration. 
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
![checkout](images/integrations/3dcart/checkout.png)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](images/sezzle-checkout.png)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.

### Troubleshooting

If testing was unsuccessful, review the following:

* Sezzle 3DCart extension is the most updated version.
* Sezzle payment method is enabled.
* API Keys were entered correctly.
    * It is recommended to use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> to avoid typos or extra spaces.
    * If you have multiple accounts with Sezzle, the merchant ID and API Keys are tied to only one URL.
* Widget script is present on your website and reflects the `Merchant ID `from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>.
  * Go to a product page on your website.
  * Right-click then select `Inspect`.
  * In the `Elements` tab, search for `widget.sezzle`.

### Manual Theme Integration

If the 3DCart app fails to maintain the widget script on the product pages, or to add the script manually for additional pages, complete the following steps:

1. From your 3DCart admin, go to `Settings` > `Design` > `Themes & Styles`. 
2. In your `Current Theme`, click the button labeled `More` then select `Edit Template(HTML)`.
3. Find the copy of your theme under `Go to Folder`. 
4. Click the `gear` icon next to the `product_items.html` file, then click `Edit`.
5. When the dashboard asks if you want to edit your theme files, click the `Edit Theme Files` button.
6. In the `Source Code` text area, copy+paste the script at the very beginning of the file.
7. Click `Save`.

The script to be inserted into your webpage is as follows:

Template: 
```
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid={sezzle_merchant_uuid}"></script>
```

<aside class="notice">
  Update <code>{sezzle_merchant_uuid}</code> in the above script template with your site’s Merchant ID (removing the curly brackets), which can be found in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>.
</aside>

Example: 
```
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid=12a34bc5-6de7-890f-g123-4hi5678jk901"></script>
```

Instructions may vary slightly depending on your active plug-ins. For assistance with widget configuration, click `Request Addition of Widgets` in the widget step of your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist</a>.

### Uninstall Steps

1. Go to `Settings` > `Payment`.
2. Click `Select Payment Methods`.
2. Under the Sezzle App, click the `gear` icon then click `Delete`.

## Bold Cashier

This guide describes how to integrate Sezzle into your Bold Cashier website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your Bold Cashier site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your Bold Cashier order management system.
3. Authorize and capture payments.

#### This integration is currently only available on Shopify.

### Integration Steps Overview

1. [Install the Sezzle](#install-the-sezzle-bold-cashier-extension) Bold Cashier app
2. [Test your integration](#bold-cashier-live-checkout)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. You should generate API Keys using your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle Bold Cashier Extension

1. First you must install the Bold Cashier app to your platform and store url. You can do this from the <a class="external-link" target="_blank" href="https://boldcommerce.com/cashier">Bold Cashier site</a>.
2. Log in to your Shopify admin.
3. Go to `Apps` > `Bold Cashier`. 
![Bold cashier](images/integrations/bold/bold-apps.png)
4. In the Bold Cashier left sidebar, click `Marketplace`, then find `Sezzle` and click `Install`.
![Bold install](images/integrations/bold/bold-marketplace.png)
5. Click `Allow` to accept permissions and complete the installation. 
![Bold permissions](images/integrations/bold/bold-permissions.png)
6. Installation is complete.
![Bold marketplace](images/integrations/bold/bold-configure.png)

### Bold Cashier Live Checkout

1. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
2. Click `Complete Order`.
3. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](images/sezzle-checkout.png)
4. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.

### Uninstall Steps

1. Go to your Bold Cashier `Marketplace` and scroll to find `Sezzle`.
2. Click `Uninstall`. 
![Bold marketplace](images/integrations/bold/bold-configure.png)


## BuyItLive

This guide describes how to integrate Sezzle into your BuyItLive website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your BuyItLive site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your BuyItLive order management system.
3. Authorize and capture payments.

### Integration Steps Overview

1. [Add and configure the Sezzle BuyItLive App](#buyitlive-admin-configuration)
2. [Test your integration](#buyitlive-live-checkout)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### BuyItLive Admin Configuration

1. Log in to your website's BuyItLive admin.
2. Click `Add Payment Provider`.
3. Select `Connect with Sezzle`.
4. Go to `Sezzle Payments` > `Tools`.
5. Copy your `Private Key` and `Public Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle configuration page of your BuyItLive admin.
6. Click `Save Settings`.
7. Go to `Cart Settings` and ensure the Sezzle switch is in the `On` position.
7. Installation is complete.

### BuyItLive Live Checkout

1. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
2. Click `Place Order`.
3. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](images/sezzle-checkout.png)
4. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.


## CommentSold

This guide describes how to integrate Sezzle into your CommentSold website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your CommentSold site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your CommentSold order management system.
3. Authorize and capture payments.

### Integration Steps Overview

1. [Configure the Sezzle](#commentsold-admin-configuration) <a class="external-link" target="_blank" href="https://commentsold.com/admin/setup/billing">CommentSold App</a>
2. [Test your integration](#commentsold-live-checkout)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### CommentSold Admin Configuration

1. In your CommentSold admin, go to `Setup`.
2. Click `Payment Gateways`.
3. Copy your `Private Key` and `Public Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle configuration page of your CommentSold admin.
4. Click `Update Keys`.
![sezzle_payment_config](images/integrations/commentsold/payment-gateways.png)<br/>
5. Installation is complete.

### CommentSold Live Checkout

1. In the Sezzle configuration page of your CommentSold admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and uncheck the `Use Sandbox` checkbox, then save the configuration. 
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](images/sezzle-checkout.png)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.

## Magento 1

<aside class="content warning">Magento 1 documentation is provided for legacy reference only. Please use <a class="external-link" href="/#magento-2">Magento 2</a> for new projects or upgrades.
</aside>

This guide describes how to integrate Sezzle into your Magento 1 website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your Magento 1 site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your Magento 1 order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.
5. Offer instant and delayed capture.

### Integration Steps Overview

1. [Install and configure the Sezzle](#install-the-sezzle-magento-1-extension) <a class="external-link" target="_blank" href="https://github.com/sezzle/sezzle-magento">Magento 1 extension</a>
2. [Test your integration](#magento-1-live-checkout)
3. **(Optional)** [Sandbox Testing](#magento-1-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle Magento 1 Extension

#### For the below instructions, assume [Magento] represents your root Magento directory.

1. Download the .zip or tar.gz file from Sezzle's github repository.
2. Unzip the file and follow the following instructions.
3. Copy all files in the extracted folder's: `/app/code/community/` to: `[MAGENTO]/app/code/community`.
4. Copy all files in the extracted folder's `/app/design/frontend/base/default/layout/` to: `[MAGENTO]/app/design/frontend/base/default/layout`.
5. Copy all files in the extracted folder's `/app/design/frontend/base/default/template/` to: `[MAGENTO]/app/design/frontend/base/default/template`.
6. Copy all files in the extracted folder's: `/app/etc/` to:
`[MAGENTO]/app/etc`.
7. Copy all files in the extracted folder's: `/js` to: `[MAGENTO]/js`.
8. Log in to your Magento 1 admin and go to `System/Cache Management`.
9. Flush the cache storage by selecting `Flush Cache Storage`.

#### Note : To upgrade the extension, completely remove the previously added Sezzle Magento extension files, then repeat the above steps with the updated sezzle-magento repository.

### Admin Configuration

1. Go to `System` > `Configuration` > `Sales` > `Payment Methods` > `Sezzle`.
2.  Configure the extension as follows:
    1. Set `Enabled` to `Yes`.
    2. Copy your `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>, and paste it into the corresponding field in the Sezzle configuration page of your Magento 1 admin.
    3. Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle configuration page of your Magento 1 admin.
    4. Set `API Mode` to `Live`.
    5. If you want the widget script added to the Product Display Page, set `Add Widget Script in PDP` to `Yes`
    6. If you want the widget script added to the Cart Page, set `Add Widget Script in Cart Page` to `Yes`
    7. Set `Payment from Applicable Countries` to `Specific Countries`.
    8. Set `Payment from Specific Countries` to `United States` or `Canada` as applicable.
    9. Set `Payment Action` as `Authorize Only` to authorize the payment at the time the order is placed but capture payment later, or `Authorize and Capture` to both authorize and capture at the time the order is placed.
        * If `Authorize Only` is selected, then the capture expiry time will be visible in the `Order Details` page. You need to capture the payment before the given deadline by choosing `Capture Online` when you create the invoice.
    10. Save the configuration.
    ![admin sezzle](images/integrations/magento1/admin-pay.png)
3. Go to `System/Cache Management`.
4. Flush the cache storage by selecting `Flush Cache Storage`.
5. Installation is complete.

### Magento 1 Sandbox Testing

1. In the Sezzle configuration page of your Magento 1 admin, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> and set the `API Mode` to `Sandbox/Test`, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Continue` then `Place Order` and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![onepage movement](images/integrations/magento1/onepage-movement.png)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> to see the test order you just placed.

### Magento 1 Live Checkout

1. In the Sezzle configuration page of your Magento 1 admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and set the `API Mode` to `Live`, then save the configuration. 
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Continue` then `Place Order`.
![onepage movement](images/integrations/magento1/onepage-movement.png)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](images/sezzle-checkout.png)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete. 

### Troubleshooting

If testing was unsuccessful, review the following:

* Sezzle-Magento extension is the most updated version.
* Sezzle extension is enabled.
  * Go to `System` > `Configuration` > `Sales` > `Payment Methods` > `Sezzle` and ensure `Enabled` dropdown is reflecting `Yes`.
* `Merchant ID` was entered correctly.
* API Keys were entered correctly.
    * It is recommended to use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> to avoid typos or extra spaces.  
    * If you have multiple accounts with Sezzle, the merchant ID and API Keys are tied to only one URL.
* Cache Storage was flushed.
* Widget script is present on your website and reflects the `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>.
  * Go to a product page on your website.
  * Right-click then select `Inspect`.
  * In the `Elements` tab, search for `widget.sezzle`.

## NopCommerce

This guide describes how to integrate Sezzle into your NopCommerce website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your NopCommerce site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your NopCommerce order management system.
3. Authorize and capture payments.
4. Offer instant and delayed capture.

### Integration Steps Overview

1. [Install and configure the Sezzle](#install-the-sezzle-nopcommerce-extension) <a class="external-link" target="_blank" href="https://www.nopcommerce.com/sezzle">NopCommerce extension</a>
2. [Test your integration](#nopcommerce-live-checkout)
3. **(Optional)** [Sandbox Testing](#nopcommerce-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle NopCommerce Extension

Go to <a class="external-link" target="_blank" href="https://www.nopcommerce.com/sezzle">https://www.nopcommerce.com/sezzle</a> and click `Get Extension`.

### Admin Configuration

1. Go to `Configuration` > `Local Plugins`.
2. Click `Upload Plugin or Theme` and select the downloaded zipped file per the instructions given.
3. After the extension has been uploaded, click `Install`.
4. Under `Configuration`, go to `Payment Methods` and then click `Configure` under `Sezzle`.
![admin nopcommerce](images/integrations/nopcommerce/pay-methods.png)
5. Click `Edit` from the `Payment Method` list.
6. Copy your `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>, and paste it into the corresponding field in the Sezzle configuration page of your NopCommerce admin.
7. Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle configuration page of your NopCommerce admin.
8. Set `Transaction Mode` to either `Authorize` or `Authorize and Capture`.
9. Save the configuration.
![admin nopcommerce sezzlepay](images/integrations/nopcommerce/pay-configuration.png)
10. To restrict Sezzle usage based on billing country, go to `Configuration` > `Payment Restrictions`.
11. Choose the country you want to restrict for Sezzle. Please note that Sezzle is currently available for customers from `The United States` and `Canada`. You may wish to restrict all countries where Sezzle is not available.
![admin nopcommerce_sezzlepay restriction](images/integrations/nopcommerce/pay-restrictions.png)
12. Integration is complete.

### NopCommerce Sandbox Testing

1. In the Sezzle configuration page of your NopCommerce admin, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> and check the `Use Sandbox` checkbox, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Confirm` and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![onepage nopcommerce payment movement](images/integrations/nopcommerce/payment-meth.png)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> to see the test order you just placed.

### NopCommerce Live Checkout

1. In the Sezzle configuration page of your NopCommerce admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and uncheck the `Use Sandbox` checkbox, then save the configuration. 
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
![nopcommerce product page](images/integrations/nopcommerce/product-widget.png)
![nopcommerce product page](images/integrations/nopcommerce/cart-widget.png)
3. Click `Continue` then `Confirm`.
![onepage nopcommerce payment movement](images/integrations/nopcommerce/payment-meth.png)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](images/sezzle-checkout.png)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.


## Shopify

This guide describes how to integrate Sezzle into your Shopify website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your Shopify site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your Shopify order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.

### Integration Steps Overview

1. [Install and configure the Sezzle](#install-the-sezzle-shopify-extension) <a class="external-link" target="_blank" href="https://www.shopify.com/login?redirect=authorize_gateway%2F1041436">Shopify App</a>
2. [Test your integration](#shopify-live-checkout)
3. **(Optional)** [Sandbox Testing](#shopify-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle Shopify App

1. Log in to your website's Shopify admin.<br/>
2. In your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist</a>, click `Download Shopify App`.<br/>
3. Click `Get the App`.<br/>
4. Click `Install App`.  <br/>
![install_app](images/integrations/shopify/install-app.png)<br/>

### Configure Widgets

1. Within the Sezzle app, enter your Public API Key and click `link sezzle account`. <br/>
![add_public_key](images/integrations/shopify/link-account.png)<br/>
2. Once your account is linked, click `add widgets` to add widgets to your shop. This process may take a minute. <br/>
![add_widgets](images/integrations/shopify/add-widgets.png)<br/>
3. After widgets have been added, navigate to a product page to confirm that the Sezzle widget has been added. <br/>
![product_widget](images/integrations/shopify/widget-product.png)<br/>
4. If you ever need to remove Sezzle widgets from your shop, click the `remove widgets` button within the Sezzle app. <br/>
![remove_widgets](images/integrations/shopify/remove-widgets.png)

#### Note: If the Sezzle app is unable to automatically add widgets to your shop, one of our team members will automatically be notified and will work to manually add widgets to your shop within 7 business days.

### Install the Sezzle Payment Gateway

1. In your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist</a>, click `Connect Sezzle to Shopify`.<br/>
2. Click `Instructions`.<br/>
3. Click the first hyperlink on the new page to enable the gateway for your shop. If prompted, select your Shopify store.<br/>
4. Click `Install Payment Provider`. <br/>
![install_payment_provider](images/integrations/shopify/install-payment-provider.png)<br/>

### Admin Configuration

1. In your Shopify admin, go to `Settings` > `Payment Providers`. <br/>
![sezzle_payment_config](images/integrations/shopify/payment-providers.png)<br/>
2. Under `Alternative Payment Methods`, click `Choose Alternative Payment`. <br/>
![choose_alternative_payment](images/integrations/shopify/choose-alternative-payment.png)<br/>
3. Search for and click on `Sezzle`. <br/>
![select_sezzle](images/integrations/shopify/select-sezzle.png)<br/>
4. Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle configuration page of your Shopify admin.<br/>
5. Click the `Activate Sezzle` button. <br/>
![account_information](images/integrations/shopify/account-information.png)<br/>
6. Installation is complete.<br/>

### Shopify Live Checkout

1. In the Sezzle configuration page of your Shopify admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and uncheck the `Enable Test Mode` checkbox, then save the configuration. 
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
![checkout](images/integrations/shopify/checkout.png)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.

### Troubleshooting

If testing was unsuccessful, review the following:

* Sezzle Shopify extension is the most updated version.
  * Go to `Apps` > `Sezzle`, then click `About`. If there is an option to upgrade, do so now.
* Sezzle gateway is activated.
    * Go to `Settings` > `Payment Providers` and ensure "Sezzle is active" is listed under the `Alternative Payment Methods` section.
* API Keys were entered correctly.
    * It is recommended to use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> to avoid typos or extra spaces.
    * If you have multiple accounts with Sezzle, the merchant ID and API Keys are tied to only one URL.
* Widget script is present on your website and reflects the `Merchant ID `from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>.
  * Go to a product page on your website.
  * Right-click then select `Inspect`.
  * In the `Elements` tab, search for `widget.sezzle`.

### Manual Theme Integration

If the Shopify app fails to maintain the widget script on the product pages, or to add the script manually for additional pages, complete the following steps:

1. Go to `Sales Channels` > `Online Store` > `Themes`.
2. Click `Actions`, then select `Edit Code`.
3. In the `Code Explorer`, go to the `Templates` folder and select the `product.liquid` file.
4. Copy+paste the script to the very bottom of the file, then click `Save`.
<script src="https://widget.sezzle.com/v1/javascript/price-widget/initial?uuid=12a34bc5-6de7-890f-g123-4hi5678jk901"></script>
5. Repeat the previous step in the `cart.liquid` file.

Note: If you have additional custom product templates, step 4 will need to be repeated for each file, or the script added to a global file, such as `layout/theme.liquid`.

The script to be inserted into your webpage is as follows:

Template: 
```
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid={sezzle_merchant_uuid}"></script>
```

<aside class="notice">
  Update <code>{sezzle_merchant_uuid}</code> in the above script template with your site’s Merchant ID (removing the curly brackets), which can be found in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>.
</aside>

Example: 
```
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid=12a34bc5-6de7-890f-g123-4hi5678jk901"></script>
```

For assistance with widget configuration, click `Request Addition of Widgets` in the widget step of your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist</a>.

### Uninstall Steps

1. Go to `Apps`.
2. Under Sezzle, click `Delete`.
3. Go to `Settings` > `Payment Providers`.
4. Under `Alternative Payment Methods`, find `Sezzle` and click `Edit`.
5. Click `Deactivate Sezzle`.
6. Click `Deactivate Sezzle` to confirm.

### Inventory Locking

Our Shopify integration has an optional feature to prevent overselling while a customer is checking out with Sezzle. To enable this feature, please reach out to our Merchant Success team at <a href="mailto:accounts@sezzle.com">accounts@sezzle.com</a>. 

### Shopify Sandbox Testing

Sezzle offers an alternate sandbox payment gateway that can be used to test your integration. If you would like to install this gateway for testing, please reach out to our Merchant Success team at <a href="mailto:accounts@sezzle.com">accounts@sezzle.com</a>. 

## WooCommerce

This guide describes how to integrate Sezzle into your WooCommerce website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your WooCommerce site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your WooCommerce order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.

### Integration Steps Overview

1. [Install and configure the Sezzle](#install-the-sezzle-woocommerce-extension) <a class="external-link" target="_blank" href="https://wordpress.org/plugins/sezzle-woocommerce-payment/">WooCommerce extension</a>
2. [Test your integration](#woocommerce-live-checkout)
3. **(Optional)** [Sandbox Testing](#woocommerce-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle WooCommerce Extension

1. Log in to your website's Wordpress admin.
    * Ex: your-website.com/wp-admin
![wordpress login](images/integrations/woocommerce/admin-login.png)
2. In the left sidebar, click `Plugins` > `Add New`.
3. Search for `Sezzle`.
4. Click `Install Now`. 
![search sezzle](images/integrations/woocommerce/search-sezzle.png)
5. Click `Activate`.
![activate sezzle](images/integrations/woocommerce/activate-sezzle.png)

### Admin Configuration

1. In the left sidebar, click `WooCommerce` > `Settings` .
2. Select the `Payments` tab.
![payment settings](images/integrations/woocommerce/go-to-payment-settings.png)
3. Click the `Manage` button for `Sezzle`.
![select sezzle](images/integrations/woocommerce/select-sezzle.png)
4. Check the `Enable/Disable` checkbox for enabling Sezzle.
5. Check the `Payment option availability in other countries` if you want to allow Sezzle outside of `US` and `Canada`.
    - Note, Sezzle operates only in `US` and `Canada`. Be sure to check this option.
6. Set `Merchant ID` as received from the `Business` section of <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant">Sezzle Merchant Dashboard</a>.
7. Copy your `Private Key` and `Public Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields.
8. Set `Minimum Checkout Amount` if you want to restrict Sezzle based on a minimum order total.
9. Set the `Transaction Mode` as `Live` for production and `Sandbox` for sandbox testing mode.
10. Check the `Show Sezzle widget in product pages` checkbox for adding widget script in the Product Display Page, which allows enabling Sezzle Widget Modal in PDP.
11. Configure the installment plan widget under `Installment Plan Widget Configuration` settings
    1. Check the `Enable Installment Widget Plan in Checkout page` checkbox for enabling installment widget plan.
    2. Set the `Order Total Container Class Name`. Default is `woocommerce-Price-amount`.
    3. Set the `Order Total Container Parent Class Name`. Default is `order-total`.
12. Check the `Enable Logging` checkbox for logging Sezzle checkout related data. This is helpful for debugging issues, if encountered.
![sezzle page overview](images/integrations/woocommerce/sezzle-page-overview.png)
13. Click `Save Changes`.

### WooCommerce Sandbox Testing

1. In the `Sezzle` configuration page of your WooCommerce admin, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> and set the `Transaction Mode` to `Sandbox`, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`, and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![select sezzle payment](images/integrations/woocommerce/select-sezzle-payment.png)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> to see the test order you just placed.

### WooCommerce Live Checkout

1. In the `Sezzle` configuration page of your WooCommerce admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and set the `Transaction Mode` to `Live`, then save the configuration. 
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
![select sezzle payment](images/integrations/woocommerce/select-sezzle-payment.png)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.

### Important Note on Order Fulfillment

Before shipping the order, ensure that the Order Notes in the WooCommerce interface show the order status is **Processing** and include **Payment approved by Sezzle**. When these notes exist, you know that the customer completed the checkout at Sezzle, and you will be paid for the order.

WooCommerce will create an order regardless of whether the customer completes the checkout at Sezzle. Check the Order Notes and do not fulfill orders where the Sezzle checkout is not completed.

### Troubleshooting

If testing was unsuccessful, review the following:

* Sezzle WooCommerce extension is the most updated version.
    * Go to `Plugins` > `Installed Plugins`, then click `View Details` next to the `Sezzle WooCommerce Payment`. If there is an option to upgrade, do so now.
* Sezzle extension is activated.
    * Go to `WooCommerce` > `Settings` and ensure the switch is turned On.
* `Merchant ID` was entered correctly.
* API Keys were entered correctly.
    * It is recommended to use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> to avoid typos or extra spaces.
    * If you have multiple accounts with Sezzle, the merchant ID and API Keys are tied to only one URL.
* `Show Sezzle widget in product pages` box is checked.
* Widget script is present on your website and reflects the `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>.
    * Go to a product page on your website.
    * Right-click then select `Inspect`.
    * In the `Elements` tab, search for `widget.sezzle`.

### Uninstall Steps

1. Go to `Plugins` > `Installed Plugins`.
2. Under `Sezzle WooCommerce Payment`, click `Deactivate` then click `Delete`.


## Zoey

This guide describes how to integrate Sezzle into your Zoey website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your Zoey site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your Zoey order management system.
3. Authorize and capture payments.

### Integration Steps Overview

1. [Install and configure the Sezzle](#install-the-sezzle-zoey-extension) <a class="external-link" target="_blank" href="https://www.zoey.com/apps/sezzle/">Zoey extension</a>
2. [Test your integration](#zoey-live-checkout)
3. **(Optional)** [Sandbox Testing](#zoey-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account. 
    * Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
    * <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle Zoey Extension

Go to <a class="external-link" target="_blank" href="https://www.zoey.com/apps/sezzle/">https://www.zoey.com/apps/sezzle/</a> and click `Get App`.

### Admin Configuration

1. Go to `Set-up` > `Payment Methods` > `Sezzle`.
2. Click `Configure`.
![admin zoey](images/integrations/zoey/zoey-payment.png)
3.  Configure the extension as follows:
    1. Set `Enabled` to `Yes`.
    2. Copy your `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>, and paste it into the corresponding field in the Sezzle configuration page of your Zoey admin.
    3. Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle configuration page of your Zoey admin.
    4. If you want the widget script added to the Product Display Page, set `Add Widget Script in PDP` to `Yes`
    5. If you want the widget script added to the Cart Page, set `Add Widget Script in Cart Page` to `Yes`
    6. Set `Payment from Applicable Countries` to `Specific Countries`.
    7. Set `Payment from Specific Countries` to `United States` or `Canada` as applicable.
    8. Save the configuration.
    ![admin zoey sezzlepay](images/integrations/zoey/zoey-pay-fields.png)
4. Click `Advanced/Refresh Your Store`.
5. Installation is complete.

### Zoey Sandbox Testing

1. In the Sezzle configuration page of your Zoey admin, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> and set the `API Mode` to `Sandbox/Test`, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Continue` then `Place Order` and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![onepage zoey payment movement](images/integrations/zoey/zoey-sezzle-payment-page.png)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> to see the test order you just placed.

### Zoey Live Checkout

1. In the Sezzle configuration page of your Zoey admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and set the `API Mode` to `Live`, then save the configuration.
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Continue` then `Place Order`.
![onepage zoey payment movement](images/integrations/zoey/zoey-sezzle-payment-page.png)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](images/sezzle-checkout.png)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.
