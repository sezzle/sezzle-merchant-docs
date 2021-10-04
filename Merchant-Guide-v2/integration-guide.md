---
title: "Integration Guide"
excerpt: "Sezzle Integration Guide"
slug: "integration-guide"
category: 6102e1a0ab9a5c000f95e56f
---


# Using the Sezzle API

## Obtain Authentication Token


US/CA: `POST https://gateway.sezzle.com/v2/authentication`

EU: `POST https://gateway.eu.sezzle.com/v2/authentication`

Sezzle uses scoped API keys to allow access to the API. You can find/generate these keys on your merchant dashboard [(US/CA)](https://dashboard.sezzle.com/merchant) or [(EU)](https://dashboard.eu.sezzle.com/merchant) once you have been approved by Sezzle.

Once you have a valid token, it must be used as a Header for subsequent requests to our API, using the format below.

`Authorization: Bearer authToken`

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

<aside class="notice">
You must replace the tokens you obtain from the Sezzle Authorization endpoint every 120 minutes. However, we do not restrict you to a single token, and you can obtain a new one at any time.
</aside>

## Open API
The <a class="external-link" href="https://swagger.io/specification/" target="_blank">OpenAPI Specification (OAS)</a> defines a standard, language-agnostic interface to RESTful APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection.
## Specification
View the <a class="external-link" href="https://gateway.sezzle.com/v2api.yaml" target="_blank">Sezzle v2 OpenAPI Specification</a>.

## Client Generator
The OpenAPI Specification can be imported into the <a class="external-link" href="https://editor.swagger.io/?url=https://gateway.sezzle.com/v2api.yaml" target="_blank">Swagger Editor</a> to easily generate a Sezzle client in a variety of programming languages.  If your language is not supported by Swagger, an alternate tool is <a class="external-link" href="https://openapi-generator.tech/">OpenAPI Generator</a>.

### Testing
While you are working on the integration, you should test it before going live. Please use this section for information on testing.

## Sandbox Testing
<strong>(US)</strong>
**API Endpoint** `https://sandbox.gateway.sezzle.com/v2`<br/>
**Sandbox Dashboard** `https://sandbox.dashboard.sezzle.com/merchant`

<strong>(EU)</strong>
**API Endpoint** `https://sandbox.gateway.eu.sezzle.com/v2`<br/>
**Sandbox Dashboard** `https://sandbox.dashboard.eu.sezzle.com/merchant`
<aside class="notice">
Credentials to log in to the sandbox dashboard are the ones you use to log in to the <b><a class="external-link" href="https://dashboard.sezzle.com/merchant/">Sezzle Merchant Dashboard (US/CA)</a></b> or <b><a class="external-link" href="https://dashboard.eu.sezzle.com/merchant/">Sezzle Merchant Dashboard (EU)</a></b>. You can create your test API keys in the sandbox dashboard.
</aside>

## Test Data
You can use the following test data to test your integration

### Card
**Card Number** `4242424242424242`<br/>
**CVV/CVC** `any (3 numbers)` <br/>
**Expiration Date** `any`<br/>
**Name** `any`<br/>
**Address** `any`<br/>

### Phone and other information
1. Please use any valid US or CA phone number.
2. The expected `OTP` is `123123`.
3. Personal information does not need to be real.

### Debit card
**IBAN Number** `DE89370400440532013000`<br/>
**Email Address** `any`<br/>
**Name** `any`<br/>

<a href="https://stripe.com/docs/testing#cards" target="_blank">Click here</a> to test more countries.


## Errors

v2 endpoints will return an array of standardized error objects.

We attempt to keep these errors as consistent as possible, and will announce any changes in advance if they are required.

> ### Error Response Body

```json
[
{
"code": "invalid",
"location": "order.amount.amount_in_cents",
"message": "Order amount must be greater than $0.99",
"debug_uuid": "919f40d0-874b-4d98-810d-ed2246a8ad77"
}
]
```

# Sessions

A session represents an order, a tokenization of a customer, or both. Use the session endpoints to post a new session or get the details of an existing session.
## Create a session

US/CA: `POST https://gateway.sezzle.com/v2/session`

EU: `POST https://gateway.eu.sezzle.com/v2/session`

This endpoint creates a session in our system, and it returns the URL that you should redirect the user to. You can use a session to create an order, tokenize a customer, or both.

We suggest you provide as much optional information about the user as you have available, since this will speed up our checkout process and increase conversion.

If you submit an order with a session, then Sezzle is able to handle the entire checkout process after an order has been provided. However, if your flow requires that the user confirm their checkout on your site after being approved by Sezzle, you may set the `intent` parameter to `AUTH` with the session request. In this flow, Sezzle will not complete the transaction unless you make a capture request. Capture requests can be made to capture all or part of the original order amount. You must send the capture request before the authorization expires. By default, authorizations expire within in 30 minutes, but the expiration period for new authorizations may be extended up to 7 days in your [merchant dashboard US/CN](https://dashboard.sezzle.com/merchant/settings/ecommerce) or [merchant dashboard EU](https://dashboard.eu.sezzle.com/merchant/settings/ecommerce).

If you tokenize a customer, then the customer will have the option to agree to allow you to process future transactions on their behalf. This gives you the ability to preapprove, authorize, and capture future transactions on behalf of the customer.

> ### Request Body

```json
{
"cancel_url": {
"href": "https://sezzle.com/cart",
"method": "GET"
},
"complete_url": {
"href": "https://sezzle.com/complete",
"method": "GET"
},
"customer": {
"tokenize": true,
"email": "john.doe@sezzle.com",
"first_name": "John",
"last_name": "Doe",
"phone": "5555045294",
"dob": "1990-02-25",
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
}
},
"order": {
"intent": "CAPTURE",
"reference_id": "ord_12345",
"description": "sezzle-store - #12749253509255",
"requires_shipping_info": true,
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
"metadata": {
"location_id": "123",
"store_name": "Downtown Minneapolis",
"store_manager": "Jane Doe"
},
"shipping_amount": {
"amount_in_cents": 1000,
"currency": "USD"
},
"tax_amount": {
"amount_in_cents": 1000,
"currency": "USD"
},
"order_amount": {
"amount_in_cents": 10000,
"currency": "USD"
}
}
}
```

> ### Response Body

```json
{
"uuid": "fadbc642-05a4-4e38-9e74-80e325623af9",
"links": [
{
"href": "https://gateway.sezzle.com/v2/session/fadbc642-05a4-4e38-9e74-80e325623af9",
"method": "GET",
"rel": "self"
}
],
"order": {
"uuid": "12a34bc5-6de7-890f-g123-4hi1238jk902",
"checkout_url": "https://checkout.sezzle.com/?id=12a34bc5-6de7-890f-g123-4hi1238jk902",
"intent": "CAPTURE",
"links": [
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902",
"method": "GET",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902",
"method": "PATCH",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/release",
"method": "POST",
"rel": "release"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/capture",
"method": "POST",
"rel": "capture"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/refund",
"method": "POST",
"rel": "refund"
}
]
},
"tokenize": {
"token": "7ec98824-67cc-469c-86ab-f9e047f9cf1a",
"expiration": "2020-04-27T14:46:59Z",
"approval_url": "https://dashboard.sezzle.com/customer/checkout-approval?merchant-request-id=3f3244fd-78ce-4994-af0c-b8c760d47794",
"links": [
{
"href": "https://gateway.sezzle.com/v2/token/7ec98824-67cc-469c-86ab-f9e047f9cf1a/session",
"method": "GET",
"rel": "token"
}
]
}
}
```
### Session Object
A valid session object contains at a minimum an Order object _or_ a Customer object with tokenize set to true.

Parameter | Type | Description
--------- | ---- | -----------
**cancel_url\*** | object | The HTTP request information used to redirect the customer in the case of a cancellation
**complete_url\*** | object | The HTTP request infromation used to redirect the customer upon completion of the session
customer | object | The customer for this session
order | object | The order for this session

### URL Object
This is used for both the cancel and complete URL objects

Parameter | Type | Description
--------- | ------- | -----------
**href\*** | string | The URL used when redirecting a customer
method | string | The HTTP request method used when redirecting a customer. Currently only the GET method is supported. If omitted, will default to GET.

### Customer object

Parameter | Type | Description
--------- | ------- | -----------
tokenize | boolean | Determines whether to tokenize customer. If omitted, will default to false.
email | string | The customer's email address
first_name | string | The customer's first name
last_name | string | The customer's last name
phone | string | The customer's phone number
dob | string | The customer's date of birth in YYYY-MM-DD format (parameter is input only)
billing_address | object | The customer's billing address
shipping_address | object | The customer's shipping address


### Address Object
This format is used for both billing_address and shipping_address in the Customer object.

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

### Order object

Parameter | Type | Description
--------- | ------- | -----------
**intent\*** | string | Accepted values are "AUTH" or "CAPTURE". If your checkout flow requires the user to confirm their checkout on your site after being approved by Sezzle, use "AUTH" as your intent. If you prefer the checkout be captured immediately, use "CAPTURE".
**reference_id\*** | string | Your reference ID for this order
**description\*** | string | Your description for this order
**order_amount\*** | object | A Price object containing the amount of the order, which must be at least `100`. All fields of the Price object are required.
requires_shipping_info | boolean | Flag to indicate if you would like us to collect shipping information for this checkout from the customer. If omitted, defaults to false.
items | object | The items being purchased
discounts | object | The discounts applied to this order. Must be included in total
metadata | object | Object for any custom data you want to submit with the checkout. You are not limited to the key-value pairs shown in the example, and you may use any key-value pairs you like
shipping_amount | object | The shipping fees applied to this order. Must be included in the total
tax_amount | object | The taxes applied to this order. Must be included in the total
checkout_expiration | string | The expiration for the order checkout in ISO 8601 date/time format
send_checkout_url | object | A Notification object for sending checkout URL to the customer
locale | string | Localizes the checkout. Accepted values are `en-US` (English, United States), `en-CA` (English, Canada), `fr-CA` (French, Canada) and `de-DE` (German, Germany), Defaults to `en-US` if not provided.

<aside class="notice">
Your order reference ID must be a unique identifier, and may only contain 'a-Z', '0-9', '-', and '_'.
</aside>

### Item Object

Parameter | Type | Description
--------- | ------- | -----------
name | string | The name of the item
sku | string | The sku identifier
quantity | int | The quantity purchased
price | object | The price object

### Discount Object

Parameter | Type | Description
--------- | ------- | -----------
name | string | The description of the discount
amount | object | A price object

### Metadata Object
Use the metadata object for any additional information you would like to attach to the checkout. All values must be strings.

Parameter | Type | Description
--------- | ------- | -----------
some_field_name | string | Custom metadata field
some_other_field_name | string | Custom metadata field

### Shipping Amount Object
A price object

### Tax Amount Object
A price object

### Price Object
The price object is used for items, discounts, shipping amount, tax amount, and order amount.

Parameter | Type | Description
--------- | ------- | -----------
amount_in_cents | int | The amount of the item in cents
currency | string | The 3 character currency code as defined by ISO 4217
### Notification Object
A valid notification object contains at a minimum a phone or an email.

Parameter | Type | Description
--------- | ------- | -----------
to_sms_phone | string | The SMS phone number of the notification
to_email | string | The email address of the notification
language | string | The 2-character ISO 639 langauge code of the notification. Acceptable values are "en" and "fr". Will default to English if not provided

## Get a session

US/CA: `GET https://gateway.sezzle.com/v2/session/{session_uuid}`

EU: `GET https://gateway.eu.sezzle.com/v2/session/{session_uuid}`

You can retrieve the details of an existing session using this endpoint.


> ### Response Body

```json
{
"uuid": "fadbc642-05a4-4e38-9e74-80e325623af9",
"links": [
{
"href": "https://gateway.sezzle.com/v2/session/fadbc642-05a4-4e38-9e74-80e325623af9",
"method": "GET",
"rel": "self"
}
],
"order": {
"uuid": "12a34bc5-6de7-890f-g123-4hi1238jk902",
"intent": "CAPTURE",
"checkout_url": "https://checkout.sezzle.com/?id=12a34bc5-6de7-890f-g123-4hi1238jk902",
"links": [
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902",
"method": "GET",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902",
"method": "PATCH",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/release",
"method": "POST",
"rel": "release"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/capture",
"method": "POST",
"rel": "capture"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/refund",
"method": "POST",
"rel": "refund"
}
]
},
"tokenize": {
"token": "7ec98824-67cc-469c-86ab-f9e047f9cf1a",
"expiration": "2020-04-27T14:46:59Z",
"approval_url": "https://dashboard.sezzle.com/customer/checkout-approval?merchant-request-id=3f3244fd-78ce-4994-af0c-b8c760d47794",
"links": [
{
"href": "https://gateway.sezzle.com/v2/token/7ec98824-67cc-469c-86ab-f9e047f9cf1a/session ",
"method": "GET",
"rel": "token"
}
]
}
}
```

# Orders

Use the orders endpoints to get order details, update an order, release an amount by order, capture an amount by order, or refund an amount by order.

<aside class="notice">
The {order_uuid} is the value returned from <i>Create session</i> (order.uuid) or <i>Create order by customer</i> (uuid). It is a different value than the Order ID displayed in the Sezzle Merchant Dashboard. It is important to note the Merchant Dashboard Order ID is not generated until a checkout is completed at Sezzle, whereas the {order_uuid} can represent an order before and after checkout completion.
</aside>

## Order payment flow

![order flow](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/v2-order.jpg?raw=true)

1. Merchant calls `/v2/session` with order and intent of `AUTH` or `CAPTURE`. Optionally, the merchant can send customer information.
2. Sezzle returns order uuid and checkout URL.
3. Merchant redirects customer to Sezzle checkout URL.
4. Customer completes the Sezzle checkout and is redirected to the session complete URL.
* If the intent was to `CAPTURE`, Sezzle will capture the total order amount.
* If the intent was to `AUTH`, Sezzle will only authorize the total order amount and the merchant can call `/v2/order` later to release or capture amounts using the order uuid.

## Get an order

US/CA: `GET https://gateway.sezzle.com/v2/order/{order_uuid}`

EU: `GET https://gateway.eu.sezzle.com/v2/order/{order_uuid}`

Use this endpoint to get details on an existing order

> ### Response Body

```json
{
"uuid": "12a34bc5-6de7-890f-g123-4hi1238jk902",
"links": [
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902",
"method": "GET",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902",
"method": "PATCH",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/release",
"method": "POST",
"rel": "release"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/capture",
"method": "POST",
"rel": "capture"
},
{
"href": "https://gateway.sezzle.com/v2/order/12a34bc5-6de7-890f-g123-4hi1238jk902/refund",
"method": "POST",
"rel": "refund"
}
],
"intent": "AUTH",
"reference_id": "ord_12345",
"description": "sezzle-store - #12749253509255",
"metadata": {
"location_id": "123",
"store_name": "Downtown Minneapolis",
"store_manager": "Jane Doe"
},
"order_amount": {
"amount_in_cents": 10000,
"currency": "USD"
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
],
"customer": {
"email": "john.doe@sezzle.com",
"first_name": "John",
"last_name": "Doe",
"phone": "5555045294",
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
}
},
"authorization": {
"authorization_amount": {
"amount_in_cents": 10000,
"currency": "USD"
},
"approved": true,
"expiration":"2020-04-23T16:13:44Z" ,
"financing_option": "4-pay-biweekly",
"releases": [
{
"uuid": "4b4d217c-18f1-4bfb-996e-767470c04661",
"amount": {
"amount_in_cents": 3000,
"currency": "USD"
}
}
],
"captures": [
{
"uuid": "f4415e94-e562-47cc-94f3-92279f27dc20",
"amount": {
"amount_in_cents": 7000,
"currency": "USD"
}
}
],
"refunds": [
{
"uuid": "83162d2f-d5f1-43ad-91ed-e8231920ce6d",
"amount": {
"amount_in_cents": 1000,
"currency": "USD"
}
}
]
}
}
```

## Update an order

US/CA: `PATCH https://gateway.sezzle.com/v2/order/{order_uuid}`

EU: `PATCH https://gateway.eu.sezzle.com/v2/order/{order_uuid}`

Use this endpoint to update an existing order. Only the reference ID can be updated.
### Update Order Object
Parameter | Type | Description
--------- | ------- | -----------
**reference_id\*** | string | Your reference ID for this order

> ### Request Body

```json
{
"reference_id": "ord_9876"
}
```

> There is no response body for this request. If successful, we return an HTTP 204 Status No Content.

## Release amount by order

US/CA: `POST https://gateway.sezzle.com/v2/order/{order_uuid}/release`

EU: `POST https://gateway.eu.sezzle.com/v2/order/{order_uuid}/release`

Use this endpoint to release an amount by order.

### Header Parameters
| <div style="width:120px">Parameter</div> | Type | Description
| --------- | ------- | -----------
| Sezzle-Request-Id | string | A unique, merchant-generated ID. Use this header to enforce idempotency when releasing an authorization.

### Release Order Object

A price object

### Price Object
Parameter | Type | Description
--------- | ------- | -----------
**amount_in_cents\*** | number | The amount you would like to release on this order, in cents
**currency\*** | string | The 3 character currency code as defined by ISO 4217

> ### Request Body

```json
{
"amount_in_cents": 5000,
"currency": "USD"
}
```

> ### Response Body

```json
{
"uuid": "6c9db5d4-d09a-4224-860a-b5438ac32ca8"
}
```

## Capture amount by order

US/CA: `POST https://gateway.sezzle.com/v2/order/{order_uuid}/capture`

EU: `POST https://gateway.eu.sezzle.com/v2/order/{order_uuid}/capture`

Use this endpoint to capture an amount by order.

### Header Parameters
| <div style="width:120px">Parameter</div> | Type | Description
| --------- | ------- | -----------
| Sezzle-Request-Id | string | A unique, merchant-generated ID. Use this header to enforce idempotency when capturing an order.

### Capture Amount By Order Object
Parameter | Type | Description
--------- | ------- | -----------
**capture_amount\*** | object | Details the amount and currency being captured
partial_capture | boolean | Determines whether this capture is a partial capture.

### Capture Amount Object

A price object.

### Price Object
Parameter | Type | Description
--------- | ------- | -----------
**amount_in_cents\*** | string | The amount to be captured, in cents
**currency\*** | string | The 3 character currency code as defined by ISO 4217

> ### Request Body

```json
{
"capture_amount": {
"amount_in_cents": 5000,
"currency": "USD"
},
"partial_capture": true
}
```
> ### Response Body

```json
{
"uuid": "6c9db5d4-d09a-4224-860a-b5438ac32ca8"
}
```

<aside class="notice">
The {uuid} returned from this operation is the capture transaction uuid, but there are no
endpoints that use this value. You may retrieve an order's capture transactions using the
<a href="#get-an-order">Get an order</a> endpoint.
</aside>

## Refund amount by order

US/CA: `POST https://gateway.sezzle.com/v2/order/{order_uuid}/refund`

EU: `POST https://gateway.eu.sezzle.com/v2/order/{order_uuid}/refund`

Use this endpoint to refund an amount by order

### Header Parameters
| <div style="width:120px">Parameter</div> | Type | Description
| --------- | ------- | -----------
| Sezzle-Request-Id | string | A unique, merchant-generated ID. Use this header to enforce idempotency when refunding an order.

### Refund Amount Object

A price object.

### Price Object
Parameter | Type | Description
--------- | ------- | -----------
**amount_in_cents\*** | string | The amount in cents to be refunded, in cents
**currency\*** | string | The 3 character currency code as defined by ISO 4217

> ### Request Body

```json
{
"amount_in_cents": 5000,
"currency": "USD"
}
```
> ### Response Body

```json
{
"uuid": "6c9db5d4-d09a-4224-860a-b5438ac32ca8"
}
```

<aside class="notice">
The {uuid} returned from this operation is the refund transaction uuid, but there are no
endpoints that use this value. You may retrieve an order's refund transactions using the
<a href="#get-an-order">Get an order</a> endpoint.
</aside>

## Delete checkout by order

`DELETE https://gateway.sezzle.com/v2/order/{order_uuid}/checkout`

Use this endpoint to delete a checkout for an order. The request fails if the checkout has already
been successfully completed by the customer.

If you have redirected the customer to the Sezzle checkout and subsequently cancel the order in
your ecommerce platform, then you should immediately call this endpoint to prevent the possibility
of the customer completing the Sezzle checkout.

> There is no response body for this request. If successful, we return an HTTP 204 Status No Content.

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

US/CA: `GET https://gateway.sezzle.com/v2/settlements/summaries`

EU: `GET https://gateway.eu.sezzle.com/v2/settlements/summaries`


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
US/CA: `GET https://gateway.sezzle.com/v2/settlements/details/{payout_uuid}`

EU: `GET https://gateway.eu.sezzle.com/v2/settlements/details/{payout_uuid}`


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

### Interest Account Balance Request
US/CA: `GET https://gateway.sezzle.com/v2/interest/balance`

EU: `GET https://gateway.eu.sezzle.com/v2/interest/balance`

Query Parameter | Description
--------------- | -----------
currency-code | The ISO-4217 currency code of the interest account. If omitted, will default to USD.

### Interest Account Activity Request
US/CA: `GET https://gateway.sezzle.com/v2/interest/activity`

EU: `GET https://gateway.eu.sezzle.com/v2/interest/activity`

Query Parameter | Description
--------------- | -----------
**start-date\*** | The start date for the report. Must be in yyyy-mm-dd format.
end-date | The end date for the report. Must be in yyyy-mm-dd format. If omitted, will default to the current date.
offset | The offset for the report. Limit is 20.
currency-code | The ISO-4217 currency code of the interest account. If omitted, will default to USD.


> ### Interest Account Balance Response Body

```json
{
"interest_balance": 5183.4624
}
```
> ### Interest Account Activity Response Body

```text
type,event_date,interest_account_change_amount,interest_account_balance_after_change
INTEREST_PAYOUT,2019-12-21T19:10:00Z,122.8718,5101.4676
INTEREST_WITHDRAWAL,2019-12-21T19:20:00Z,-26.1000,5075.3676
INTEREST_ACCRUAL,2019-12-21T19:15:00Z,1.0702,5182.3922
INTEREST_ACCRUAL,2019-12-22T19:15:00Z,1.0702,5183.4624
```

# Customers


Use the customers endpoints to get a list of customers, get details on an existing customer, delete a customer, preapprove an amount for the customer, or create an order for a customer.

## Get a list of customers

US/CA: `GET https://gateway.sezzle.com/v2/customer`

EU: `GET https://gateway.eu.sezzle.com/v2/customer`

You can retrieve a list of existing customers using this endpoint.

> ### Response Body

```json
[
{
"uuid": "a9d8e15c-5e4a-4201-aa8f-7540f934a9a2",
"expiration":  "2020-04-28T17:58:11Z",
"links": [
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2",
"method": "GET",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2",
"method": "DELETE",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2/preapprove",
"method": "POST",
"rel": "preapprove"
},
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2/order",
"method": "POST",
"rel": "order"
}
]
}
]
```

## Get a customer

US/CA: `GET https://gateway.sezzle.com/v2/customer/{customer_uuid}`

EU: `GET https://gateway.eu.sezzle.com/v2/customer/{customer_uuid}`

You can use this endpoint to get details on an existing customer

> ### Response Body

```json
{
"uuid": "a9d8e15c-5e4a-4201-aa8f-7540f934a9a2",
"links": [
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2",
"method": "GET",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2",
"method": "DELETE",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2/preapprove",
"method": "POST",
"rel": "preapprove"
},
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2/order",
"method": "POST",
"rel": "order"
}
],
"email": "john.doe@sezzle.com",
"first_name": "John",
"last_name": "Doe",
"phone": "5555045294",
"dob": "1990-02-25",
"token_expiration": "2020-04-27T14:46:59Z",
"billing_address": {
"name": "John Doe",
"street": "123 W Lake St",
"street2": "Unit 104",
"city": "Minneapolis",
"state": "MN",
"postal_code": "55408",
"country_code": "US",
"phone_number": "5555045294"
}
}
```

## Delete a customer

US/CA: `DELETE https://gateway.sezzle.com/v2/customer/{customer_uuid}`

EU: `DELETE https://gateway.eu.sezzle.com/v2/customer/{customer_uuid}`

You can use this endpoint to delete an existing customer

> There is no response body for this request. If successful, we return an HTTP 204 Status No Content.

## Preapprove amount by customer

US/CA: `POST https://gateway.sezzle.com/v2/customer/{customer_uuid}/preapprove`

EU: `POST https://gateway.eu.sezzle.com/v2/customer/{customer_uuid}/preapprove`

You can use this endpoint to preapprove an amount for a customer

> ### Request Body

```json
{
"amount_in_cents": 5000,
"currency": "USD"
}
```

### Preapprove Object
A price Object

### Price Object
Parameter | Type | Description
--------- | ------- | -----------
**amount_in_cents\*** | number | The amount you would like to preapprove, in cents
**currency\*** | string | The 3 character currency code as defined by ISO 4217

> ### Response Body

```json
{
"uuid": "6c9db5d4-d09a-4224-860a-b5438ac32ca8",
"approved": true
}
```

## Create order by customer

US/CA: `POST https://gateway.sezzle.com/v2/customer/{customer_uuid}/order`

EU: `POST https://gateway.eu.sezzle.com/v2/customer/{customer_uuid}/order`

You can use this endpoint to create an order for a customer

### Header Parameters
| <div style="width:120px">Parameter</div> | Type | Description
| --------- | ------- | -----------
| Sezzle-Request-Id | string | A unique, merchant-generated ID. Use this header to enforce idempotency when authorizing order payment.

> ### Request Body

```json
{
"intent": "AUTH",
"reference_id": "monthly_sub_123",
"order_amount": {
"amount_in_cents": 5000,
"currency": "USD"
},
"financing_options": ["6-pay-monthly"]
}
```

### Order Payment Object
Parameter | Type | Description
--------- | ------- | -----------
**intent\*** | string | Accepted values are "AUTH" or "CAPTURE"
**reference_id\*** | string | A reference ID for the order
**order_amount\*** | object | The amount and currency of the order
**financing_options** | array | The financing option of the order. Accepted values are "4-pay-biweekly" and "6-pay-monthly". If more that one options are supplied, the first option will be taken into account

### Order Amount Object
A price object. The amount must be greater than 99.

### Price Object
Parameter | Type | Description
--------- | ------- | -----------
**amount_in_cents\*** | number | The amount you would like to authorize, in cents
**currency\*** | string | The 3 character currency code as defined by ISO 4217

> ### Response Body

```json
{
"uuid": "6c9db5d4-d09a-4224-860a-b5438ac32ca8",
"links": [
{
"href": "https://gateway.sezzle.com/v2/order/6c9db5d4-d09a-4224-860a-b5438ac32ca8",
"method": "GET",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/order/6c9db5d4-d09a-4224-860a-b5438ac32ca8",
"method": "PATCH",
"rel": "self"
},
{
"href": "https://gateway.sezzle.com/v2/order/6c9db5d4-d09a-4224-860a-b5438ac32ca8/release",
"method": "POST",
"rel": "release"
},
{
"href": "https://gateway.sezzle.com/v2/order/6c9db5d4-d09a-4224-860a-b5438ac32ca8/capture",
"method": "POST",
"rel": "capture"
},
{
"href": "https://gateway.sezzle.com/v2/order/6c9db5d4-d09a-4224-860a-b5438ac32ca8/refund",
"method": "POST",
"rel": "refund"
},
],
"intent": "AUTH",
"reference_id": "monthly_sub_123",
"order_amount": {
"amount_in_cents": 5000,
"currency": "USD"
},
"authorization": {
"authorization_amount": {
"amount_in_cents": 5000,
"currency": "USD"
},
"approved": true,
"expiration":"2020-04-23T16:13:44Z"
}
}
```

# Tokenization

Use the token endpoints to get the status of a customer tokenization


## Customer tokenization

![customer tokenization](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/v2-tokenize-customer.jpg?raw=true)

1. Merchant calls `/v2/session` with customer tokenize of `true`. Optionally, the merchant can send customer information.
2. Sezzle returns the tokenize session token and approval URL.
3. Merchant redirects customer to Sezzle approval URL.
4. Customer can agree (or disagree) to allow future Sezzle transactions by the merchant and is redirected to the session complete URL. If the customer agrees to be tokenized, Sezzle will add a query parameter to the complete URL named `customer-uuid`, allowing the merchant to get the uuid of the customer.
* Alternatively, the merchant can call `/v2/token` with this tokenize session token to get the uuid of the customer.
5. Merchant can subsequently charge the customer by calling `/v2/customer` to create an order using the `customer-uuid`.
* If successful, merchant can call `/v2/order` to release, capture, or refund the order.

<aside class="notice">
The merchant has the option to create an order payment and tokenize the customer in a single session. In this instance, the merchant should redirect the customer to the Sezzle checkout URL. During checkout, the customer can agree to allow future Sezzle transactions by the merchant. Again, if the customer agrees to be tokenized, Sezzle will add a query parameter to the complete URL named `customer-uuid`. *Note: Sezzle still returns an approval URL on create session. If the customer does not agree to be tokenized during checkout, the merchant can use the approval URL at a later time.*
</aside>

> ### Response Body

```json
{
"token": "4f8cf865-2089-4423-85fd-ea833a16b62d",
"expiration": "2020-04-29T19:31:54Z",
"links": [
{
"href": "https://gateway.sezzle.com/v2/token/4f8cf865-2089-4423-85fd-ea833a16b62d/session",
"method": "GET",
"rel": "self"
}
],
"customer": {
"uuid": "a9d8e15c-5e4a-4201-aa8f-7540f934a9a2",
"expiration": "2020-10-13T14:29:41Z",
"links": [
{
"href": "https://gateway.sezzle.com/v2/customer/a9d8e15c-5e4a-4201-aa8f-7540f934a9a2",
"method": "GET",
"rel": "self"
}
]
}
}
```


## Get session tokenization


US/CA: `GET https://gateway.sezzle.com/v2/token/{token}/session`

EU: `GET https://gateway.eu.sezzle.com/v2/token/{token}/session`

You can use this endpoint to get the current state of a session (tokenize) token

If the customer is not tokenized, then the customer object will be omitted.

# Webhooks

You can use these endpoints to configure your webhooks

## Create webhooks

US/CA: `POST https://gateway.sezzle.com/v2/webhooks`

EU: `POST https://gateway.eu.sezzle.com/v2/webhooks`

This endpoint can be used to subscribe to webhooks

> ### Request Body

```json
{
"url": "https://example.com/webhooks",
"events": [
"customer.tokenized"
]
}
```

### Webhooks Object
Parameter | Type | Description
--------- | ------- | -----------
**url\*** | string | The url you are using to receive webhooks
**events\*** | array | An array of events to subscribe to

> ### Response Body

```json
{
"uuid": "747cf28a-bb5c-46a8-a288-d9b006fd6113",
"links": [
]
}
```

### Valid Webhook Events

We accept the following Webhook events

Event | Description
----- | -----------
customer.tokenized | This webhook is called when a customer is tokenized
order.authorized | This webhook is called when an order is authorized by Sezzle
order.captured | This webhook is called when an order is captured by Sezzle
order.refunded | This webhook is called when an order is refunded by Sezzle

## List webhooks

US/CA: `GET https://gateway.sezzle.com/v2/webhooks`

EU: `GET https://gateway.eu.sezzle.com/v2/webhooks`

You can get a list of your webhooks using this endpoint

> ### Response Body

```json
[
{
"uuid": "747cf28a-bb5c-46a8-a288-d9b006fd6113",
"links": [
{}
],
"url": "https://example.com/webhooks",
"events": [
"customer.tokenized"
]
}
]
```

## Delete webhooks

US/CA: `DELETE https://gateway.sezzle.com/v2/webhooks/{webhooks_uuid}`

EU: `DELETE https://gateway.eu.sezzle.com/v2/webhooks/{webhooks_uuid}`

You can delete your webhooks using this endpoint

> There is no response body for this request. If successful, we return an HTTP 204 Status No Content.

## Test Webhooks

`POST https://gateway.sezzle.com/v2/webhooks/test`

You can trigger a test event using this endpoint

### Test Webhooks Object
Parameter | Type | Description
--------- | ------- | -----------
**event\*** | string | One of the [Valid Webhook Events](#valid-webhook-events)
url | string | A url to receive the test webhook. If omitted, the test webhook is sent to all urls subscribed to that event (see [List Webhooks](#list-webhooks))

> There is no response body for this request. If successful, we return an HTTP 201 Status Created.

## Webhook Signature

Webhooks are signed with an HMAC using the SHA256 algorithm. The header `Sezzle-Signature` value is a hash of the webhook's body with your merchant private key. You should always verify that the signature matches the webhook data to ensure that the webhook came from Sezzle.

## Webhook Acceptance and Retries

A webhook has been successfully sent when we receive an HTTP 200 Status OK response. Any other response will queue the webhook to be retried. We will retry several times within the first hour, and a few times for the remainder of that day. The final two attempts are made one day later, and then 3 days later, for a total elapsed time of five days. If the final retry fails, then that subscribed webhook will be deleted for all events. You will need to create the webhook again to resubscribe, if desired.

It is possible that new webhooks will arrive before old webhooks have been retried, so webhooks are not guaranteed to be received in cronological order.

Webhooks are signed using the current merchant private key, not the private key at the time of their creation, so a retried webhook may have a different signature if the keys are changed after its originating event.

# SDK

## Javascript SDK

The Javascript SDK is for creating an in-context checkout by hosting the Sezzle checkout in a modal iframe or pop-up window.

## Javascript SDK Features

* Create Checkout
* Capture Payment
* Supports Sezzle Checkout in an iframe, pop-up window, or redirect to Sezzle
* Handle Payment Success
* Handle Payment Cancel
* Handle Payment Failure
* Render Sezzle Button

## Installing the Javascript SDK

* Include `https://checkout-sdk.sezzle.com/checkout.min.js` in the `<head>` section of the page.

## Implement the Javascript SDK

### Button Configuration

* Create a placeholder element for the Sezzle Button to be rendered on the page(s).

* Use HTML attributes to customize the button.

Attribute | Description
--- | -----
templateText | Text that will prepended with the Sezzle logo. Default is `Checkout with %%logo%%`
borderType | Options are `square` and `semi-rounded`
customClass | Custom classes to be applied
paddingX | X-axis padding. Default is `13px`
paddingY | Y-axis padding. Default is `7px`
width | Width of the button
height | Height of the button. Default is `20%`

>### Button Placeholder

```html
<div id="sezzle-smart-button-container"></div>
```

<div></div>

>### Button Customization

```html
<div id="sezzle-smart-button-container" style="text-align: center"
templateText="Pay with %%logo%%"
borderType="semi-rounded"
customClass="action,primary,checkout">
</div>
```

### Checkout Configuration

* Edit the page file to implement the SDK.
* Configure the SDK with the following options:<br>

Attribute | Options
----- | ----
mode | `iframe`&#124;`popup`&#124;`redirect`
apiMode | `sandbox`&#124;`live`
apiVersion | `v2`&#124;`v1`
**publicKey\*** | `xxxx` (used only when creating a checkout or capturing payment)

>### Configure Checkout

```javascript
const checkout = new Checkout({
'mode': "iframe",
'publicKey': "xxxx",
'apiMode': "sandbox",
'apiVersion': "v2"
});
```

<aside class="notice">
For security reasons, hosting the Sezzle checkout in an iframe is not allowed. To enable <span class="code">iframe</span> mode, please send an email to <a href="mailto:dev@sezzle.com?subject=Checkout SDK iframe Request">dev@sezzle.com</a> with your Sezzle Merchant UUID and a list of domains to be allowed per environment (production and sandbox).
</aside>

### Render the Sezzle Button

Call `renderSezzleButton` passing the `id` of the placeholder element defined in Button Configuration, above.

>### Render Button

```javascript
checkout.renderSezzleButton("sezzle-smart-button-container");
```

### Initialize the Checkout

The SDK requires the following event handlers:

Event | Description
--- | ----
**onClick\*** | Sezzle Button is clicked by the user
**onComplete\*** | Sezzle payment is successfully completed
**onCancel\*** | Sezzle payment is cancelled
**onFailure\*** | Sezzle payment has failed

>### Initialize Checkout

```javascript
checkout.init({
onClick: function () {},
onComplete : function (event) {},
onCancel: function () {},
onFailure: function (event) {}
});
```
### Hosting the Checkout

To be implemented in the `onClick` handler. There are two methods for hosting a checkout.

* Use an existing checkout url.

* Use a checkout payload as detailed in the [Session Object](#create-a-session).
* The cancel and complete urls are not required for `iframe` and `popup` mode.
* Tokenization is not supported in the SDK.

>### Start Checkout with URL

```javascript
checkout.startCheckout({
checkout_url: "https://checkout.sezzle.com/?id=example"
});
```
>### Start Checkout with Payload

```javascript
checkout.startCheckout({
checkout_payload: {
"order": {
"intent": "AUTH",
"reference_id": "ord_12345",
"description": "sezzle-store - #12749253509255",
"order_amount": {
"amount_in_cents": 10000,
"currency": "USD"
}
}
}
});
```


### Checkout Events

* A successfully completed Sezzle checkout will trigger an event to the `onComplete` handler.  The event should include a `data` object with the `session_uuid` and `order_uuid`.
* If the user exits the Sezzle checkout for any reason, the `onCancel` handler will be executed.
* Any error will trigger an event to the `onFailure` handler. The event should include a `data` object with the error details.

>### Checkout Completed

```javascript
function onCompleteHandler(event) {
var data = event.data || Object.create(null);

console.log('checkout data:',
data.session_uuid,
data.order_uuid);
}

checkout.init({
onComplete : onCompleteHandler
});
```


### Capture the Order

Typically implemented in the `onComplete` handler.  Capturing an order is not required if the `CAPTURE` intent was used when creating the checkout.

The capture payment method requires two parameters, the `order_uuid` and the payload as detailed in the [Capture Amount By Order Object](#capture-amount-by-order).

>### Capture Order

```javascript
var payload = {
capture_amount: {
amount_in_cents: 5000,
currency: "USD"
},
partial_capture: false
};

checkout.capturePayment(data.order_uuid, payload);
```

## Mobile SDK
The Mobile SDK is for integrating the Sezzle checkout and widget into a native mobile app.

### Android

The [Android SDK](https://github.com/sezzle/sezzlemerchantsdkandroid) is available in Gradle or Maven.


### iOS

The [iOS SDK](https://github.com/sezzle/SezzleMerchantsSDKiOS) is available in [CocoaPods](https://cocoapods.org/pods/SezzleMobileSDK).

## Widget SDK
### Purpose

The Widget SDK serves to load our sales widgets to web pages. The widgets will not show unless a config is provided before the script is loaded. The repository for this project can be found at [https://github.com/sezzle/sezzle-js](https://github.com/sezzle/sezzle-js).


## Configuring the Widgets

The config must be specified in a property of the `document` object called `document.sezzleConfig`. The SDK also provides various options to customize the widget's position and appearance. The explanation for all the options can be found below:


`targetXPath` (required)

**Purpose**: Path to the element in the webpage where the product price text value will be detected.<br/>
**Type**: string, or array of strings<br/>
**Default**: ''<br/>
**Additional Details**: Specify one path if only one price element is targeted. Specify multiple paths in an array if multiple price elements are targeted. The path may contain multiple subpaths. All subpaths need to be separated by the '/' character. IDs need to be preceded by a '#' character. Classes needed to be preceded by a '.' character. Tag names need to be followed by the applicable index. The format of a tagname is as follows: *tagName-Index* (e.g. *'SPAN-2'*). The indexes are zero-based, such that the first element of the specified type within the parent element is at index 0.

Example: *'#ProductSection/.product-price/SPAN-1'* would target the 2nd *'SPAN'* element contained within elements that contain the *'product-price'* class which are contained within the element with an ID of *'ProductSection'*.

<div></div>

> An example of how to configure the widget:

```javascript
document.sezzleConfig =
{
targetXPath: ['.price'],
renderToPath: ['..'],
urlMatch: 'product',
theme: 'light',
scaleFactor: 1.0,
logoSize: 1.00,
logoStyle: {'margin': '0px 0px -4px 1px','transformOrigin': 'center top'},
altVersionTemplate: {
'en': 'or 4 interest-free payments of %%price%% with %%logo%% %%info%%',
'fr': 'ou 4 paiements de %%price%% sans intérêts avec %%logo%% %%info%%'
},
splitPriceElementsOn: '-',
ignoredPriceElements: ['.Price-compareAt'],
ignoredFormattedPriceText: ['Subtotal', 'Total:', 'Sold Out'],
hideClasses: ['.afterpay-paragraph'],
fontFamily: 'inherit',
fontSize: 12,
fontWeight: 300,
color: 'inherit',
alignment: 'auto',
alignmentSwitchMinWidth: 768,
alignmentSwitchType: 'center',
lineHeight: '13px',
maxWidth: 400,
marginTop: 0,
marginBottom: 0,
marginLeft: 0,
marginRight: 0,
language: navigator.language,
forcedShow: false
}
```

> Only the targetXPath is required in the local config. Each key using the default value may be removed from the sezzleConfig, and the default values will be used automatically.<br/>


> An example of targetXPath:

```html
<div id="ProductSection">
<div class="product-price">
<span>Price: </span>
<span>$15.99</span>
</div>
</div>
<script type="text/javascript">
document.sezzleConfig = {
targetXPath: '#ProductSection/.product-price/SPAN-1'
}
</script>
```

`renderToPath` (optional)

**Purpose**: Path to the element in the webpage relative to `targetXPath` where the Sezzle widget should be rendered.<br/>
**Type**: string, or array of strings<br/>
**Default**: '..'<br/>
**Additional Details**: Path to the element below which the widget should render (widget's previous element sibling). If you wish to place widgets in multiple places, you can pass multiple paths in an array. The price path at the *nth* index of the `targetXPath` array will be rendered at the path given at the *nth* index of the `renderToPath` array. If you do not pass anything to the *nth* index of the `renderToPath` array but there is a path at the *nth* index of `targetXPath`, then the widget will default to be rendered directly below the parent of the corresponding target element. <br/>
**'./'** will place the widget as the next element sibling of the target element.<br/>
**'../'**  means go up one parent element.<br/>
As with targetXPath, prepend IDs with '#', classes with '.', and append tag names with the index (tagName-Index). It is recommended to keep the `renderToPath` as simple as possible to maximize compatibility.

`urlMatch` (optional)

**Purpose**: Specific word appearing in the url of pages where the widget config should be applied.<br/>
**Type**: string<br/>
**Default**: ''<br/>
**Additional Details**: Typical values are *'product'* or *'cart'*, as applicable

`theme` (optional)

**Purpose**: Updates the logo color to coordinate and contrast with different background colors of websites.<br/>
**Type**: string<br/>
**Options**: dark, light, grayscale, black-flat, white, white-flat<br/>
**Default**: 'light'

`scaleFactor` (optional)

**Purpose**: Ratio at which to scale the Sezzle widget.<br/>
**Type**: number<br/>
**Default**: 1.0

`logoSize` (optional)

**Purpose**: Ratio at which to scale the Sezzle logo.<br/>
**Type**: number<br/>
**Default**: 1.00<br/>
**Additional Details**: The space the logo occupies between the widget text and the More Info link/icon is determined by the font size. When dramatically scaling the widget, it may be necessary to override the styling to adjust the left and right margins of the logo using `logoStyle`.

`logoStyle` (optional)

**Purpose**: Custom styling to apply to the Sezzle logo within the widget, particularly when using `logoSize`.<br/>
**Type**: object<br/>
**Default**: {}<br/>
**Additional Details**: The object will accept any CSS styling in JSON format. Keys must be surrounded by '', given in camelCase instead of kebob-case, and separated from the following key by a comma instead of a semi-colon.

`altVersionTemplate` (optional)

**Purpose**: Text content of the widget. Also changes the arrangement of price, logo, and the info/learn-more icon within the widget.<br/>
**Type**: string, or object<br/>
**Default**: {en: 'or 4 interest-free payments of %%price%% with %%logo%% %%info%%', fr: 'ou 4 paiements de %%price%% sans intérêts avec %%logo%% %%info%%'}<br/>
**Additional Details**: Currently available templates: %%price%%, %%logo%%, %%link%%, %%info%%, %%question-mark%%, %%line-break%%

`splitPriceElementsOn` (optional)

**Purpose**: Character or string at which to split the price elements (for elements with price ranges).<br/>
**Type**: string<br/>
**Default**: ''<br/>
**Additional Details**: Certain websites, especially WooCommerce websites, have price ranges as their price element (e.g. $650 - $1000). Setting this field to the character or string which separates the prices (e.g. in the case above, it is ’-’) enables the widgets to parse the price elements separately. For instance, setting this field to ’-’ would cause the widget to render the widget price above as *$162.50 - $250.00*.

> An example of splitPriceElementsOn:

```html
<div class="price">
<span class="woocommerce-Price-amount">$12</span>
" - "
<span class="woocommerce-Price-amount">$15</span>
</div>
<script type="text/javascript">
document.sezzleConfig = {
targetXPath: '.price/.woocommerce-Price-amount',
splitPriceElementsOn: '-',
relatedElementActions: [{
relatedPath: '.',
initialAction:(e, t)=> {
if(e.nextSibling&&" – "===e.nextSibling.textContent) {
let i=(e.nextElementSibling.innerText.replace(",", "").substr(1)/4).toFixed(2);
t.getElementsByClassName("sezzle-price-split")[0].innerText+=" - $"+i
}
}
},
{
relatedPath: '.',
initialAction:(e, t)=> {
e.previousSibling&&" –" ===e.previousSibling.textContent&&(t.style="display: none")
}
}]
}
</script>
```

`ignoredPriceElements` (optional)

**Purpose**: Child elements of `targetXPath` to be disregarded when detecting the price and rendering the widget.<br/>
**Type**: array of strings<br/>
**Default**: []<br/>
**Additional Details**: `ignoredPriceElements` can be used to solve `targetXPath` variations between sale and regular-priced items. In this case, `targetXPath` should point to the parent element surrounding the old and the new prices, then `ignoredPriceElements` will specify the old/compare-at price element. As with `targetXPath`, prepend IDs with '#', classes with '.', and append tag names with the index (*tagName-Index*).

> An example of ignoredPriceElements:

```html
<span class="ProductMeta__PriceList">
<span class="product__price--regular">$20</span>
<span class="product__price--sale">$15</span>
</span>
<script type="text/javascript">
document.sezzleConfig = {
targetXPath: '.ProductMeta__PriceList',
ignoredPriceElements: ['.product__price--regular']
}
</script>
```

`ignoredFormattedPriceText` (optional)

**Purpose**: Text strings within the `targetXPath` to be disregarded when detecting the price and rendering the widget.<br/>
**Type**: array of strings<br/>
**Default**: ['Subtotal', 'Total:', 'Sold Out']

`hideClasses` (optional)

**Purpose**: Classes of elements that should be hidden when Sezzle's logo is showing. This is useful for hiding a product similar to Sezzle that is not available in a country where Sezzle is.<br/>
**Type**: array of strings<br/>
**Default**: []

`fontFamily` (optional)

**Purpose**: Font family of the widget text.<br/>
**Type**: string<br/>
**Default**: 'inherit'

`fontSize` (optional)

**Purpose**: Font size of the widget text in pixels.<br/>
**Type**: number<br/>
**Default**: 12<br/>
**Additional Details**: Enter numbers only. Do not enter the unit (e.g. *px*)!

`fontWeight` (optional)

**Purpose**: Boldness of the widget text.<br/>
**Type**: number<br/>
**Default**: 300<br/>
**Additional Details**: 100 is the lightest, 900 is the boldest.

`color` (optional)

**Purpose**: Color of the widget text.<br/>
**Type**: string<br/>
**Default**:  'inherit'<br/>
**Additional Details**: Accepts all kinds of values (hexadecimal, rgb(), hsl(), etc...)

`alignment` (optional)

**Purpose**: Alignment of the widget relative to the parent element.<br/>
**Type**: string<br/>
**Options**: left, center, right, auto<br/>
**Default**: 'auto'

`alignmentSwitchMinWidth` (optional)

**Purpose**: Screen width in pixels below which the alignment switches to `alignmentSwitchType` instead of `alignment`.<br/>
**Type**: number<br/>
**Default**: 0<br/>
**Additional Details**: The most common breakpoint is *768* (handheld vs desktop). `alignmentSwitchMinWidth` is typically only necessary when alignment is not auto.

`alignmentSwitchType` (optional)

**Purpose**: Alignment of the widget relative to the parent element to be applied when the viewport width is narrower than `alignmentSwitchMinWidth`.<br/>
**Type**: string<br/>
**Options**: left, center, right, auto<br/>
**Default**: 'auto'

`lineHeight` (optional)

**Purpose**: Content height of the widget.<br/>
**Type**: string<br/>
**Default**: '13px'<br/>
**Additional Details**: Include unit (e.g.: *px*)

`maxWidth` (optional)

**Purpose**: Maximum width of the widget element in pixels.<br/>
**Type**: number<br/>
**Default**: 400<br/>
**Additional Details**: 200 to render the widget nicely on 2 lines, 120 for 3 lines.

`marginTop` (optional)

**Purpose**: Amount of space above the widget in pixels.<br/>
**Type**: number<br/>
**Default**: 0

`marginBottom` (optional)

**Purpose**: Amount of space below the widget in pixels.<br/>
**Type**: number<br/>
**Default**: 0

`marginLeft` (optional)

**Purpose**: Amount of space left of the widget in pixels.<br/>
**Type**: number<br/>
**Default**: 0

`marginRight` (optional)

**Purpose**: Amount of space right of the widget in pixels.<br/>
**Type**: number<br/>
**Default**: 0

`language` (optional)

**Purpose**: Language in which the widget text should be rendered.<br/>
**Type**: string<br/>
**Options**: 'en', 'fr'<br/>
**Default**: navigator.language<br/>
**Additional Details**: To match the selected language in the window instead of the user's default browser language, use document.querySelector('html').lang. Currently, the SDK only supports 'en' and 'fr'

`forcedShow` (optional)

**Purpose**: Shows the widget in every country if true. Shows the widget in only the United States and Canada if false.<br/>
**Type**: boolean<br/>
**Options**: false, true<br/>
**Default**: false


## Uploading the Widgets

After building up the config, it is time to upload the widgets to your webpages! The SDK needs to be called in the HTML of the webpage in order for the widgets to be rendered. Place the config and the widget script at the bottom of the code file for the HTML page, and you should be all set!

> Placing the following code snippet into your HTML loads the SDK into the webpage.

> US/CA
```html
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid={{replace_with_your_merchant_id}}"></script>
```

> EU
```html
<script src="https://widget.eu.sezzle.com/v1/javascript/price-widget?uuid={{replace_with_your_merchant_id}}"></script>
```

> The script reflecting the applicable merchant ID **must** be called only after the config is specified (`document.sezzleConfig` is defined). Here is a complete example:

```html
<script type="text/javascript">
document.sezzleConfig =
{
targetXPath: ['.price'],
renderToPath: ['..'],
urlMatch: 'product',
theme: 'light',
scaleFactor: 1.0,
logoSize: 1.00,
logoStyle: {'margin': '0px 0px -4px 1px','transformOrigin': 'center top'},
altVersionTemplate: {
'en': 'or 4 interest-free payments of %%price%% with %%logo%% %%info%%',
'fr': 'ou 4 paiements de %%price%% sans intérêts avec %%logo%% %%info%%'
},
splitPriceElementsOn: '-',
ignoredPriceElements: ['.Price-compareAt'],
ignoredFormattedPriceText: ['Subtotal', 'Total:', 'Sold Out'],
hideClasses: ['.afterpay-paragraph'],
fontFamily: 'inherit',
fontSize: 12,
fontWeight: 300,
color: 'inherit',
alignment: 'auto',
alignmentSwitchMinWidth: 768,
alignmentSwitchType: 'center',
lineHeight: '13px',
maxWidth: 400,
marginTop: 0,
marginBottom: 0,
marginLeft: 0,
marginRight: 0,
language: navigator.language,
forcedShow: false
}
</script>
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid=12a34bc5-6de7-890f-g123-4hi5678jk901"></script>
```

> The "simple" configuration above is intended for a single template of code (product or cart page) individually. Alternatively, a single configuration could be created for the entire webpage using an array of objects called configGroups. The script will determine which configGroup(s) applies to the active webpage and render the widget(s) accordingly.

```html
<script type="text/javascript">
document.sezzleConfig = {
configGroups: [
{
targetXPath: ['.price'],
renderToPath: ['..'],
urlMatch: 'product',
theme: 'light',
scaleFactor: 1.0,
logoSize: 1.00,
logoStyle: {'margin': '0px 0px -4px 1px','transformOrigin': 'center top'},
altVersionTemplate: {
'en': 'or 4 interest-free payments of %%price%% with %%logo%% %%info%%',
'fr': 'ou 4 paiements de %%price%% sans intérêts avec %%logo%% %%info%%'
},
splitPriceElementsOn: '-',
ignoredPriceElements: ['.Price-compareAt'],
ignoredFormattedPriceText: ['Subtotal', 'Total:', 'Sold Out'],
hideClasses: ['.afterpay-paragraph'],
fontFamily: 'inherit',
fontSize: 12,
fontWeight: 300,
color: 'inherit',
alignment: 'auto',
alignmentSwitchMinWidth: 768,
alignmentSwitchType: 'center',
lineHeight: '13px',
maxWidth: 400,
marginTop: 0,
marginBottom: 0,
marginLeft: 0,
marginRight: 0
},
{
targetXPath: '.cart__subtotal',
renderToPath: '../..',
urlMatch: 'cart',
}
],
forcedShow: false,
language: document.querySelector('html').lang || navigator.language,
}
</script>
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid=12a34bc5-6de7-890f-g123-4hi5678jk901"></script>
```

