---
title: "Plugin Guides N-Z"
excerpt: "Plugin Guides N-Z"
slug: "plugin-guides-n-z"
category: 6102e1a0ab9a5c000f95e56f
---



# Plugin integration

This page contains guides for Sezzle plugins from L to Z. For guides to plugins from A to M (for example, the Lightspeed plugin) see [Plugin Guides A-M](https://docs.sezzle.com/sezzle-integration/docs/plugin-guides-a-m)

### Need help?

If you have any questions about integration, please <a href="https://merchant-help.sezzle.com/hc/en-us/categories/360002649692-Integrating-with-Sezzle" target="_blank">go here for US/CA</a> and <a href="https://help.eu.sezzle.com/hc/en-us/sections/360011033912-Integrating-with-Sezzle" target="_blank">go here for EU</a>


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
* Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/signup">signup page (EU)</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
#### US/CA
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>

#### EU
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle NopCommerce Extension

Go to <a class="external-link" target="_blank" href="https://www.nopcommerce.com/sezzle">https://www.nopcommerce.com/sezzle</a> and click `Get Extension`.

### Admin Configuration

1. Go to `Configuration` > `Local Plugins`.
2. Click `Upload Plugin or Theme` and select the downloaded zipped file per the instructions given.
3. After the extension has been uploaded, click `Install`.
4. Under `Configuration`, go to `Payment Methods` and then click `Configure` under `Sezzle`.
![admin nopcommerce](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/nopcommerce/pay-methods.png?raw=true)
5. Click `Edit` from the `Payment Method` list.
6. Copy your `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (EU)</a>, and paste it into the corresponding field in the Sezzle configuration page of your NopCommerce admin.
7. Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a>, and paste them into the corresponding fields in the Sezzle configuration page of your NopCommerce admin.
8. Set `Transaction Mode` to either `Authorize` or `Authorize and Capture`.
9. Save the configuration.
![admin nopcommerce sezzlepay](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/nopcommerce/pay-configuration.png?raw=true)
10. To restrict Sezzle usage based on billing country, go to `Configuration` > `Payment Restrictions`.
11. Choose the country you want to restrict for Sezzle. Please note that Sezzle is currently available for customers from `The United States` and `Canada`. You may wish to restrict all countries where Sezzle is not available.
![admin nopcommerce_sezzlepay restriction](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/nopcommerce/pay-restrictions.png?raw=true)
12. Integration is complete.

### NopCommerce Sandbox Testing

1. In the Sezzle configuration page of your NopCommerce admin, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://sandbox.eu.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (EU)</a> and check the `Use Sandbox` checkbox, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Confirm` and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![onepage nopcommerce payment movement](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/nopcommerce/payment-meth.png?raw=true)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://sandbox.dashboard.eu.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (EU)</a> to see the test order you just placed.

### NopCommerce Live Checkout

1. In the Sezzle configuration page of your NopCommerce admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a> and uncheck the `Use Sandbox` checkbox, then save the configuration.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
![nopcommerce product page](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/nopcommerce/product-widget.png?raw=true)
![nopcommerce product page](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/nopcommerce/cart-widget.png?raw=true)
3. Click `Continue` then `Confirm`.
![onepage nopcommerce payment movement](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/nopcommerce/payment-meth.png?raw=true)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.




## Salesforce Commerce Cloud

This guide describes how to integrate Sezzle into your Salesforce Commerce Cloud website so that you can provide Sezzle as a payment option for your customers. This integration supports sites built with both the SiteGenesis and Reference Architecture. After integrating Sezzle, your site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.

Please contact our team at dev@sezzle.com to request installation documentation for the Salesforce cartridge.



## Shift4Shop

This guide describes how to integrate Sezzle into your Shift4Shop website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your Shift4Shop site will:

- offer Sezzle as a payment option on the checkout page.
- refund Sezzle payments from your Shift4Shop order management system.
- display Sezzle promotional messaging.
- authorize and capture payments.

### Integration Steps Overview

1. [Install and configure the Sezzle](#install-the-sezzle-shift4shop-extension) <a class="external-link" target="_blank" href="https://app-sezzle01.3dcart.com/Home/InstallApp">Shift4Shop App</a>
2. [Test your integration](#shift4shop-live-checkout)
3. **(Optional)** [Sandbox Testing](#shift4shop-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account.
* Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/signup">signup page (EU)</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.

<strong>US/US</strong>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a><br>

<strong>EU</strong>
<ul></li><a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Merchant ID</a></li>
<li><a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Public API Key</a></li>
<li><a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Private API Key</a></li></ul>

3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle Shift4Shop Extension

1. Log in to your website's Shift4Shop admin.
2. Get the app <a class="external-link" target="_blank" href="https://app-sezzle01.3dcart.com/Home/InstallApp">here</a>.
3. Copy+paste your `Store URL` into the input area, then click `Proceed`.
![application_authorization](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shift4shop/application-authorization.png?raw=true)<br/>
4. Check the PCI Compliance box, then click `Acknowledge and Authorize the App` to start the installation.

### Admin Configuration

1. In your Shift4Shop admin, go to `Settings` > `Payment`.<br/>
2. Click `Select Payment Methods`. <br/>
![select_payment_methods](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shift4shop/select-payment-methods.png?raw=true)<br/>
3. Turn the Sezzle switch to `On`.<br/>
4. Copy your `Public Key` from your <a href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a>, and paste it into the corresponding field in the Sezzle configuration page of your Shift4Shop admin.<br/>
5. Next to `Private Key`, click `Change`. Then, copy your `Private Key` from your <a href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a>, and paste it into the corresponding field in the Sezzle configuration page of your Shift4Shop admin.<br/>
6. Click `Save`.<br/>
![sezzle_payment_config](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shift4shop/payment-methods.png?raw=true)<br/>
7. To restrict Sezzle usage by country, click the `Exclude List` hyperlink under the Sezzle switch. <br/>
8. Click `Add Location`.<br/>
9. Select the desired country, then click `Add`. <br/>
![exclude_list](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shift4shop/exclude-list.png?raw=true)<br/>
10. Installation is complete.

### Shift4Shop Sandbox Testing

1. In the Sezzle configuration page of your Shift4Shop admin, enter the [Sandbox API Keys](#sandbox) from your <a href="https://dashboard.sezzle.com/merchant" target="_blank">Sezzle Merchant Dashboard (US/CA)</a> or <a href="https://dashboard.eu.sezzle.com/merchant" target="_blank">Sezzle Merchant Dashboard (EU)</a> and check the `Test Mode` checkbox, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to checkout and select `Sezzle` as the payment method.
3. Click `Place Order` and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shift4shop/checkout.png?raw=true)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a href="https://sandbox.dashboard.sezzle.com/merchant" target="_blank">Sezzle Merchant Sandbox Dashboard (US/CA)</a> or <a href="https://sandbox.dashboard.eu.sezzle.com/merchant" target="_blank">Sezzle Merchant Sandbox Dashboard (EU)</a> to see the test order you just placed.

### Shift4Shop Live Checkout

1. In the Sezzle configuration page of your Shift4Shop admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a> and uncheck the `Test Mode` checkbox, then save the configuration.
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
![checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shift4shop/checkout.png?raw=true)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.

### Troubleshooting

If testing was unsuccessful, review the following:

* Sezzle Shift4Shop extension is the most updated version.
* Sezzle payment method is enabled.
* API Keys were entered correctly.
* It is recommended to use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> to avoid typos or extra spaces.
* If you have multiple accounts with Sezzle, the merchant ID and API Keys are tied to only one URL.
* Widget script is present on your website and reflects the `Merchant ID `from your <a href="https://dashboard.sezzle.com/merchant/settings/business" target="_blank" class="external-link">Sezzle Merchant Dashboard (US/CA)</a> or <a href="https://dashboard.eu.sezzle.com/merchant/settings/business" target="_blank" class="external-link">Sezzle Merchant Dashboard (EU)</a>.
* Go to a product page on your website.
* Right-click then select `Inspect`.
* In the `Elements` tab, search for `widget.sezzle`.

### Manual Theme Integration

If the Shift4Shop app fails to maintain the widget script on the product pages, or to add the script manually for additional pages, complete the following steps:

1. From your Shift4Shop admin, go to `Settings` > `Design` > `Themes & Styles`.
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

Note: Update `{sezzle_merchant_uuid}` in the above script template to reflect your site’s Merchant ID (removing the curly brackets), which can be found in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>.

Example:
```
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid=12a34bc5-6de7-890f-g123-4hi5678jk901"></script>
```

Instructions may vary slightly depending on your active plug-ins. For assistance with widget configuration, click `Request Addition of Widgets` in the widget step of your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist</a>.

### Uninstall Steps

1. Go to `Settings` > `Payment`.
2. Click `Select Payment Methods`.
2. Under the Sezzle App, click the `gear` icon then click `Delete`.



## Shopware 5

This guide describes how to integrate Sezzle into your Shopware 5 website so that you can provide Sezzle as a payment option for your customers. Additionally, it includes steps to upgrade existing integrations to the latest version of the plugin.

Sezzle's Shopware 5 plugin can be [Download from github](https://github.com/sezzle/sezzle-shopware5).

After integrating Sezzle, your site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your Shopware 5 backend.
3. Authorize and capture payments.
4. Offer instant and delayed capture.

### Integration Steps Overview

1. [Install and configure](#install-the-sezzle-shopware-5-plugin) the Sezzle <a class="external-link" target="_blank" href="#">Shopware 5 Plugin</a>
2. [Test your integration](#shopware-5-live-checkout)
3. **(Optional)** [Sandbox Testing](#shopware-5-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account.
* Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.

#### US/CA
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>

#### EU
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Private API Key</a>
<br><br>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#order-payment-flow).

### Install the Sezzle Shopware 5 Plugin

In the following section, `[Shopware]` refers to your Shopware 5 root directory.

#### Using the Composer

Go to the Shopware 5 installation directory, then run the below commands:

1. Open terminal and navigate to `Shopware` root path.
2. Run the below command for the adding the plugin into your codebase:
```composer require sezzle/shopware5```

#### Manual Method

1. Download the .zip or tar.gz file from `Sezzle's` github repository.
2. Unzip the file.
3. Navigate to `Shopware` `[Shopware]/custom/plugins/` either through `SFTP` or `SSH`.
4. Copy `SwagPaymentSezzle` directory from unzipped folder to `[Shopware]/custom/plugins/`.
5. Log in to `Shopware 5` Backend and navigate to `Configuration > Plugin Manager > Management > Installed`.
6. Find `Sezzle` from the `Uninstalled` list and click on the `+` button to install the plugin.
7. Once installed, you will see `Sezzle` under `Inactive` list. Click on the `x` button to activate the plugin.
8. After successful activation, you will be able to see `Sezzle` under `Configuration > Payment Methods`.

### Upgrade the Shopware 5 Plugin
#### Using the Composer
1. Change the version number of the `sezzle/sezzlepay` inside `composer.json`.
2. Open terminal and navigate to `Shopware` root path.
3. Run the following command for the updating the plugin to a newer version:
```composer update sezzle/sezzlepay```

#### Manual Method

1. Download the .zip or tar.gz file from `Sezzle's` github repository.
2. Unzip the file.
3. Delete the contents from `[Shopware]/custom/plugins/SwagPaymentSezzle`.
4. Copy the contents of `SwagPaymentSezzle` directory from unzipped folder to `[Shopware]/custom/plugins/SwagPaymentSezzle/`.
5. Log in to `Shopware` Backend and navigate to `Configuration > Cache/performance`.
6. Flush the cache storage by selecting `Clear shop cache`.


### Configure Sezzle

#### Payment Configuration

* Make sure you have the `Merchant UUID` and the `API Keys` from the [Sezzle Merchant Dashboard (US/CA)](https://dashboard.sezzle.com/merchant/) or [Sezzle Merchant Dashboard (EU)](https://dashboard.sezzle.com/merchant/). You must be registered with [Sezzle (US/CA)](https://dashboard.sezzle.com/merchant/signup) or [Sezzle (EU)](https://dashboard.eu.sezzle.com/merchant/signup) to access the Merchant Dashboard.
* Navigate to `Customers > Payments > Sezzle > Settings` in your `Shopware` Backend.
* Enable `Sezzle` by checking the `Enable for this shop` checkbox.
* Set the `Public Key` and `Private Key`.
* For testing, enable the Sandbox mode by checking the `Enable sandbox` checkbox.
* You can also verify your `API Keys` by clicking on the `Test API Settings` button.
* Set the `Merchant UUID`.
* Set the `Merchant Location` as per the store origin.
* Check the `Enable Tokenization` checkbox to enable customer tokenization in the Sezzle checkout. If the customer agrees to be tokenized, then future checkouts for this customer will not require a redirect to Sezzle. See [Customer tokenization](#customer-tokenization)
* Set `Payment Action` as `Authorize only` for doing payment authorization only and `Authorize and Capture` for doing instant capture.
* Check the `Enable Widget in PDP` checkbox to add the widget script and the `Sezzle Widget` Modal to the Product Display Page.
* Check the `Enable Widget in Cart` checkbox to add the widget script and the `Sezzle Widget` Modal to the Cart Page.
* Check the `Display errors` checkbox for showing up `Sezzle` related error code in the web URL on failure.
* Set `Logging` to `ERROR` to log only error messages or `ALL` to log all messages, including errors, warnings, and notices.
* Save the settings and clear the cache.
![sezzle settings](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopware5/settings.png?raw=true)

####Your store is now ready to accept payments through Sezzle.

### Frontend Functionality

* If you have successfully installed the Sezzle plugin, then Sezzle will be included as a payment method in the checkout page.
* Select `Sezzle` and continue.
* Once you click `Complete Payment`, you will be redirected to `Sezzle Checkout` to complete the checkout. Note: If your account is already tokenized, skip the next two steps as you will not be redirected to Sezzle.
* **[Optional]** On the final page of Sezzle Checkout, check the `Approve {Store Name} to process payments from your Sezzle account for future transactions. You may revoke this authorization at any time in your Sezzle Dashboard` to tokenize your account.
* Finally, click on `Complete Order` to complete your purchase.
* On successful order placement, you will be redirected to the order confirmation page.
![shopware5 checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopware5/checkout.png?raw=true)

### Capture Payment

* If `Payment Action` is set to `Authorize and Capture`, capture will be performed instantly from the plugin after order is created and validated in `Shopware`.
* If `Payment Action` is set to `Authorize`, capture needs to be performed manually from the `Shopware` backend. Follow the below steps to capture.
* Go the order and click on `Sezzle` tab.
* Enter a value in `Amount` field and click on `Capture` to capture the payment in `Sezzle`.

### Refund Payment

* Go the order and click on `Sezzle` tab.
* Enter a value in `Amount` field and click on `Refund` to refund the payment in `Sezzle`.

### Release Payment

* Go the order and click on `Sezzle` tab.
* Enter a value in `Amount` field and click on `Release` to release the payment in `Sezzle`.

### Order Verification in Shopware Backend

Merchants should always check the payment status and amount of all orders. The following steps ensure that each action has been completed as expected.

* Log in to `Shopware` admin and navigate to `Customers > Orders`.
![shopware5 orders](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopware5/orders.png?raw=true)
* Proceed into the corresponding order.
* Payment is successfully captured by `Sezzle` when:
* Current Payment Status is `Completely Paid`.
* `Capture Amount` equals the `Auth Amount`.
* Payment is only authorized when:
* Current Payment Status is `Open`.
* `Auth Amount` equals the `Order Amount`.
* `Capture Amount` equals `0`.
* Payment is refunded when:
* Current Payment Status is `Re-crediting`.
* `Refund Amount` is equal to or less than the `Capture Amount`.
* Payment is released when:
* Current Payment Status is `The process is cancelled for a full release or Open for a partial release`.
* Amount will be deducted from `Auth Amount` and should appear in `Released Amount`.
![shopware5 order_sezzle_tab](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopware5/order-sezzle-tab.png?raw=true)

### Order Verification in Sezzle Merchant Dashboard

* Log in to `Sezzle Merchant Dashboard` and navigate to `Orders`.
* Proceed into the corresponding order.
* Payment successfully captured has a status of `Approved`.
* Payment authorized but not captured has a status of `Authorized, uncaptured`.
* Payment refunded has a status of `Refunded` or `Partially refunded`.
* Payment released or not captured before the authorization expired has a status of  `Deleted due to checkout not being captured before expiration`.

### Customer Tokenization Details

* Log in to `Shopware` Backend and navigate to `Customers > Customers`.
* Select customer to view tokenization details.
* `Sezzle Customer UUID`, `Sezzle Customer UUID Expiry` and `Sezzle Customer UUID Status` will appear under `Free text fields`.
![shopware5 customer](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopware5/customer.png?raw=true)

### Shopware 5 Sandbox Testing

* In the `Sezzle` settings page of your `Shopware` Backend, enter the `Sandbox` `API Keys` from your Sezzle [Sezzle Merchant Dashboard (US/CA)](https://sandbox.dashboard.sezzle.com/merchant/) or [Sezzle Merchant Dashboard (EU)](https://sandbox.eu.dashboard.sezzle.com/merchant/) and check the `Enable sandbox` checkbox, then save the configuration. Make sure you are doing this on your `dev/staging` website.
* On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
* Once you click `Complete Payment`, you will be redirected to `Sezzle Checkout` to complete the checkout. Note: If your account is already tokenized, skip the next four steps as you will not be redirected to Sezzle.
* Sign In or Sign Up to continue.
* Enter the payment details using test data, then move to final page.
* **[Optional]** Check the `Approve {Store Name} to process payments from your Sezzle account for future transactions. You may revoke this authorization at any time in your Sezzle Dashboard` to tokenize your account.
* Finally, click on `Complete Order` to complete your purchase.
* `Sandbox` testing is complete. You can log in to your `Sezzle Merchant Sandbox Dashboard` to see the test order you just placed.

### Shopware 5 Live Checkout

1. In the `Sezzle` settings page of your `Shopware` Backend, enter the `API Keys` from your Sezzle <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Merchant Dashboard (EU)</a> and uncheck the `Enable sandbox` checkbox, then save the configuration.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Complete Payment`.
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete it.

### Troubleshooting

* `Sezzle` plugin creates logs of `Sezzle` action.
* In the event that `Merchant Success` and `Support` teams are unable to resolve any issue, the Merchant may request that these logs be forwarded to the `Platform Integrations` team for further troubleshooting..
* Logs are named as `plugin_dev-<current-date>.log`. To facilitate troubleshooting, we recommend sending the` core_dev-<current-date>.log` as well..
* Logs can be find in `[Shopware]/var/log/`.



## Shopify

<aside class="notice">
Download the Sezzle Shopify app from <a href="https://dashboard.sezzle.com/merchant/settings/ecommerce" target="_blank">Sezzle US/CA Dashboard</a> or <a href="https://dashboard.eu.sezzle.com/merchant/settings/ecommerce" target="_blank">Sezzle EU Dashboard</a>.<br>

When configuring the app, use the API keys from your Dashboard account <a href="https://dashboard.sezzle.com/" target="_blank">Sezzle US/CA Dashboard</a> or <a href="https://dashboard.eu.sezzle.com/" target="_blank">Sezzle EU Dashboard</a>.
</aside>

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
#### US/CA
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>

#### EU
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Private API Key</a>

3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle Shopify App

1. Log in to your website's Shopify admin.<br/>
2. In your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist (EU)</a>, click `Download Shopify App`.<br/>
3. Click `Get the App`.<br/>
4. Click `Install App`.  <br/>
![install_app](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/install-app.png?raw=true)<br/>

### Configure Widgets

1. Within the Sezzle app, enter your Public API Key and click `link sezzle account`. <br/>
![add_public_key](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/link-account.png?raw=true)<br/>
2. Once your account is linked, click `add widgets` to add widgets to your shop. This process may take a minute. <br/>
![add_widgets](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/add-widgets.png?raw=true)<br/>
3. After widgets have been added, navigate to a product page to confirm that the Sezzle widget has been added. <br/>
![product_widget](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/widget-product.png?raw=true)<br/>
4. If you ever need to remove Sezzle widgets from your shop, click the `remove widgets` button within the Sezzle app. <br/>
![remove_widgets](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/remove-widgets.png?raw=true)

#### Note: If the Sezzle app is unable to automatically add widgets to your shop, one of our team members will automatically be notified and will work to manually add widgets to your shop within 7 business days.

### Install the Sezzle Payment Gateway

1. In your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist (EU)</a>, click `Connect Sezzle to Shopify`.<br/>
2. Click `Instructions`.<br/>
3. Click the first hyperlink on the new page to enable the gateway for your shop. If prompted, select your Shopify store.<br/>
4. Click `Install Payment Provider`. <br/>
![install_payment_provider](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/install-payment-provider.png?raw=true)<br/>

### Admin Configuration

1. In your Shopify admin, go to `Settings` > `Payment Providers`. <br/>
![sezzle_payment_config](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/payment-providers.png?raw=true)<br/>
2. Under `Alternative Payment Methods`, click `Choose Alternative Payment`. <br/>
![choose_alternative_payment](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/choose-alternative-payment.png?raw=true)<br/>
3. Search for and click on `Sezzle`. <br/>
![select_sezzle](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/select-sezzle.png?raw=true)<br/>
4. Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a>, and paste them into the corresponding fields in the Sezzle configuration page of your Shopify admin.<br/>
5. Click the `Activate Sezzle` button. <br/>
![account_information](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/account-information.png?raw=true)<br/>
6. Installation is complete.<br/>

### Shopify Live Checkout

1. In the Sezzle configuration page of your Shopify admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a> and uncheck the `Enable Test Mode` checkbox, then save the configuration.
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
![checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/shopify/checkout.png?raw=true)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.

### Troubleshooting

If testing was unsuccessful, review the following:

* Sezzle Shopify extension is the most updated version.
* Go to `Apps` > `Sezzle`, then click `About`. If there is an option to upgrade, do so now.
* Sezzle gateway is activated.
* Go to `Settings` > `Payment Providers` and ensure "Sezzle is active" is listed under the `Alternative Payment Methods` section.
* API Keys were entered correctly.
* It is recommended to use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a> to avoid typos or extra spaces.
* If you have multiple accounts with Sezzle, the merchant ID and API Keys are tied to only one URL.
* Widget script is present on your website and reflects the `Merchant ID `from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (EU)</a>.
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
Update <code>{sezzle_merchant_uuid}</code> in the above script template with your site’s Merchant ID (removing the curly brackets), which can be found in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (EU)</a>.
</aside>

Example:
```
<script src="https://widget.sezzle.com/v1/javascript/price-widget?uuid=12a34bc5-6de7-890f-g123-4hi5678jk901"></script>
```

For assistance with widget configuration, click `Request Addition of Widgets` in the widget step of your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist (EU)</a>.

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

## Prestashop

Download the Sezzle module from <a href="" target="_blank">the PrestaShop Marketplace</a>.<br>
- In your PrestaShop back office, go to `Modules > Module Manager`.<br>
- Select 'Upload a module' , and select the .zip file that you downloaded on
your computer.<br>
- Go to `Payment > Payment methods`.<br>
- Find 'Sezzle', and select 'Enable Module'.<br>
- The plugin is now ready to be configured.<br>
- To learn how to install the plugin after you've downloaded it, you can also
watch the official <a href="https://www.youtube.com/watch?v=nG3VSMQ593s" target="_blank">PrestaShop video tutorial</a>.<br>

### Configuration
In your PrestaShop back office, go to `Modules > Module Manager`.<br>
In the Payment section, find Sezzle and select Configure. Fill out the following fields:<br>
- Live Mode : Enable or Disable for testing.<br>
- Merchant Id : Enter the Merchant Id that you got from Merchant
Dashboard.<br>
- Public Key : Enter the Public Key that you got from Merchant
Dashboard.<br>
- Private Key : Enter the Private Key that you got from Merchant
Dashboard.<br>
- Payment Action : Authorize and Capture for instant capture and
Authorize Only for just authorization(needs to be captured manually
from backoffice).<br>
- Allow Customer Tokenization : Enable to offer returning shoppers a
faster checkout experience by saving their card details.<br>
- Enable Widget : Enable for showing Sezzle widget in PDP and Cart
Page.<br>

<img src="https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/prestashop.jpg?raw=true" alt="prestashop Configuration" />

### Payment Capture
Payment will be automatically captured during the checkout process.<br>
- Payment Action as `Authorize Only`<br>
- In your PrestaShop backoffice, go to `Orders > Orders`.<br>
- Select the order for which you want to capture the payment.<br>
- In the `Payment` section right below, enter and below information
and click `Add` <br>
-  Date<br>
- Payment method<br>
- Amount<br>
- Change the Order Status to `Payment Accepted` if capture is
successful.<br>

<img src="https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/prestashop2.jpg?raw=true" alt="prestashop Payment Capture" />

### Payment Refund
- In your PrestaShop backoffice, go to `Orders > Orders`.<br>
- Select the order for which you want to refund the payment.<br>
- Click on `Partial Refund`, enter the amount and click on `Partial Refund` right
below<br>
- For Full Refund, you have to fill up all the order related amounts in
respective places.<br>
- Change the Order Status to `Refunded` only if full refund is
successful.<br>

<img src="https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/prestashop3.jpg?raw=true" alt="prestashop Payment Capture" />

### Payment Release
- In your PrestaShop backoffice, go to `Orders > Orders`.<br>
- Select the order for which you want to refund the payment.<br>
- Change the Order status to `Cancelled`, and click on `Update Status`.<br>

<img src="https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/prestashop4.jpg?raw=true" alt="prestashop Payment Release" />

### Upgrading the module
If you are already using an older Sezzle PrestaShop module version earlier and want to use the latest version, proceed as follows:<br>
- Uninstall the existing Sezzle module.<br>
- In the Upgrade drop-down menu select Uninstall.<br>
- Manually remove the /sezzle folder from the /modules folder(if not removed during uninstall action).<br>
- Install the latest plugin version as described in Installation step.<br>
- Configure the latest plugin version as described in Configuration step.<br>

### Reinstallation
When you reinstall the plugin, the plugin takes care of most of its configurations and functions except for the items listed below. We recommend that you follow the steps here to make sure that when you reinstall the plugin, it won't pick up settings from a previous installation.<br><br>
<strong>Order status</strong><br>
- The plugin keeps the `Awaiting Sezzle Payment` order status<br>
because existing orders might still use them. If you would also like to remove these statuses, first make sure that these are no longer in use.<br><br>
- To check if the status are is in use:<br>
- Go to the `Orders` page in your Prestashop admin panel.<br>
- Filter the orders for the `Awaiting Sezzle Payment` order status.<br>
- If there are any, move them to another status that you would like to use.<br>
When there are no more orders using this status, you can delete the status from your order status list. - Configurations<br>
- The plugin keeps the `AWAITING_SEZZLE_PAYMENT` configuration fields in your database in case there are still orders in your system with the corresponding statuses. If you have already removed the status, you can also remove these leftover configurations.<br>



## Wix

This guide describes how to integrate Sezzle into your Wix website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your Wix order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.

### Integration Steps Overview

1. Add Sezzle to your store as a payment method
2. [Test your integration](#wix-live-checkout)

### Before You Begin

1. You should have a Sezzle merchant account.
2. Make sure you have the following Sezzle details handy.
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#order-payment-flow).

### Add Payment Method

1. From your Wix store dashboard, select `Settings`, and `Accept Payments`.
2. Choose the `See More Payment Options` link at the bottom of the page.
3. Locate Sezzle in the list, and choose `Connect`. This takes you to the Connect Sezzle page.
4. Enter your API Keys from the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and choose **Connect**. Sezzle will now be available as a payment method at checkout.

### Wix Live Checkout

1. On your store website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
2. Follow the Wix checkout process to the final step and choose `Place Order`.
3. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
<br/>**Warning:** Don't complete the payment. Your checkout is now live, so you will be charged if you complete it.

### Troubleshooting
For account connection problems, check the following:

* API Keys were entered correctly.
* To avoid typos or extra spaces, use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>.
* If you have multiple accounts with Sezzle, each store has its own merchant ID and API Keys that are tied to the store's URL.

If Sezzle does not appear as a payment option during live testing:

* Ensure that Sezzle is activated as a payment provider on your site.
* Go to `Settings` > `Accept Payments` and ensure that "Accepting Payments" is displayed for Sezzle.


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
* Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/signup">signup page (EU)</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.
#### US/CA
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>

#### EU
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Private API Key</a>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### Install the Sezzle WooCommerce Extension

1. Log in to your website's Wordpress admin.
* Ex: your-website.com/wp-admin
![wordpress login](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/woocommerce/admin-login.png?raw=true)
2. In the left sidebar, click `Plugins` > `Add New`.
3. Search for `Sezzle`.
4. Click `Install Now`.
![search sezzle](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/woocommerce/search-sezzle.png?raw=true)
5. Click `Activate`.
![activate sezzle](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/woocommerce/activate-sezzle.png?raw=true)

### Admin Configuration

1. In the left sidebar, click `WooCommerce` > `Settings` .
2. Select the `Payments` tab.
![payment settings](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/woocommerce/go-to-payment-settings.png?raw=true)
3. Click the `Manage` button for `Sezzle`.
![select sezzle](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/woocommerce/select-sezzle.png?raw=true)
4. Check the `Enable/Disable` checkbox for enabling Sezzle.
5. Check the `Payment option availability in other countries` if you want to allow Sezzle outside of `US` and `Canada`.
- Note, Sezzle operates only in `US` and `Canada`. Be sure to check this option.
6. Set `Merchant ID` as received from the `Business` section of <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant">Sezzle Merchant Dashboard</a>.
7. Copy your `Private Key` and `Public Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a>, and paste them into the corresponding fields.
8. Set `Minimum Checkout Amount` if you want to restrict Sezzle based on a minimum order total.
9. Set the `Transaction Mode` as `Live` for production and `Sandbox` for sandbox testing mode.
10. Check the `Show Sezzle widget in product pages` checkbox for adding widget script in the Product Display Page, which allows enabling Sezzle Widget Modal in PDP.
11. Configure the installment plan widget under `Installment Plan Widget Configuration` settings
1. Check the `Enable Installment Widget Plan in Checkout page` checkbox for enabling installment widget plan.
2. Set the `Order Total Container Class Name`. Default is `woocommerce-Price-amount`.
3. Set the `Order Total Container Parent Class Name`. Default is `order-total`.
12. Check the `Enable Logging` checkbox for logging Sezzle checkout related data. This is helpful for debugging issues, if encountered.
![sezzle page overview](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/woocommerce/sezzle-page-overview.png?raw=true)
13. Click `Save Changes`.

### WooCommerce Sandbox Testing

1. In the `Sezzle` configuration page of your WooCommerce admin, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://sandbox.dashboard.eu.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (EU)</a> and set the `Transaction Mode` to `Sandbox`, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`, and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![select sezzle payment](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/woocommerce/select-sezzle-payment.png?raw=true)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> to see the test order you just placed.

### WooCommerce Live Checkout

1. In the `Sezzle` configuration page of your WooCommerce admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a> and set the `Transaction Mode` to `Live`, then save the configuration.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
![select sezzle payment](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/woocommerce/select-sezzle-payment.png?raw=true)
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
* It is recommended to use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a> to avoid typos or extra spaces.
* If you have multiple accounts with Sezzle, the merchant ID and API Keys are tied to only one URL.
* `Show Sezzle widget in product pages` box is checked.
* Widget script is present on your website and reflects the `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (EU)</a>.
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
![admin zoey](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/zoey/zoey-payment.png?raw=true)
3.  Configure the extension as follows:
1. Set `Enabled` to `Yes`.
2. Copy your `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard</a>, and paste it into the corresponding field in the Sezzle configuration page of your Zoey admin.
3. Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle configuration page of your Zoey admin.
4. If you want the widget script added to the Product Display Page, set `Add Widget Script in PDP` to `Yes`
5. If you want the widget script added to the Cart Page, set `Add Widget Script in Cart Page` to `Yes`
6. Set `Payment from Applicable Countries` to `Specific Countries`.
7. Set `Payment from Specific Countries` to `United States` or `Canada` as applicable.
8. Save the configuration.
![admin zoey sezzlepay](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/zoey/zoey-pay-fields.png?raw=true)
4. Click `Advanced/Refresh Your Store`.
5. Installation is complete.
### Zoey Sandbox Testing

1. In the Sezzle configuration page of your Zoey admin, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> and set the `API Mode` to `Sandbox/Test`, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Continue` then `Place Order` and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![onepage zoey payment movement](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/zoey/zoey-sezzle-payment-page.png?raw=true)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> to see the test order you just placed.

### Zoey Live Checkout

1. In the Sezzle configuration page of your Zoey admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and set the `API Mode` to `Live`, then save the configuration.
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Continue` then `Place Order`.
![onepage zoey payment movement](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/zoey/zoey-sezzle-payment-page.png?raw=true)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.
