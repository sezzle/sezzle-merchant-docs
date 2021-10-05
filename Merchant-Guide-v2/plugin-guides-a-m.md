---
title: "Plugin Guides A-M"
excerpt: "Plugin Guides A-M"
slug: "plugin-guides-a-m"
category: 6102e1a0ab9a5c000f95e56f
---




# Plugin integration

This page contains guides for Sezzle plugins from A to M. For guides to plugins from N to Z (for example, the Zoey plugin) see [Plugin Guides N-Z](doc:plugin-guides-n-z)

### Need help?

If you have any questions about integration, please <a href="https://merchant-help.sezzle.com/hc/en-us/categories/360002649692-Integrating-with-Sezzle" target="_blank">go here for US/CA</a> and <a href="https://help.eu.sezzle.com/hc/en-us/sections/360011033912-Integrating-with-Sezzle" target="_blank">go here for EU</a>

## BigCommerce

This guide describes how to integrate Sezzle into your BigCommerce website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your BigCommerce site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your BigCommerce order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.

#### This integration is only available with BigCommerce's <a class="external-link" target="_blank" href="https://support.bigcommerce.com/s/article/Optimized-Single-Page-Checkout">Optimized One Page Checkout</a>.

### Integration Steps Overview

1. [Enable Sezzle as an online payment method](#enable-sezzle-as-an-online-payment-method)
2. [Install and configure the Sezzle](#install-the-sezzle-bigcommerce-app) <a class="external-link" target="_blank" href="https://www.bigcommerce.com/apps/sezzle/">BigCommerce App</a>
3. [Test your integration](#bigcommerce-live-checkout)
4. **(Optional)** [Sandbox Testing](#bigcommerce-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account.
* Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.<br>
<strong>US/US</strong>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a><br>

<strong>EU</strong>
<ul></li><a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Merchant ID</a></li>
<li><a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Public API Key</a></li>
<li><a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Private API Key</a></li></ul>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#order-payment-flow).

### Enable Sezzle as an online payment method

1. Go to `Store Setup` > `Payments`. <br/>
![payment_methods](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/payment-methods.png?raw=true)
2. Go to `Online Payment Methods`, find `Sezzle` and click `Set up`. <br/>
![offline-payment_methods](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/online-payment-methods.png?raw=true)
3. Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields.<br/>
4. Select the `Transaction Type`.
* **Note:** If you select `Authorize only`, payment will only be authorized and will have to be captured later in BigCommerce. <br/>
5. Select the `Test Mode`: `No(Recommended)`, or `Yes` for testing. <br/>
6. Click on `Save`. <br/>

### Install the Sezzle BigCommerce App

1. Log in to your website's BigCommerce store admin page.
2. In the left sidebar, click `Apps` > `Marketplace`.
3. Click `BigCommerce.com/Apps`.
![bigcommerce_apps](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/bigcommerce-apps.png?raw=true)
4. Search for `Sezzle`.
![search_sezzle](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/app-search.png?raw=true)
5. Click `Sezzle`, then click `Get This App`.
![sezzle_app_bigcommerce_marketplace](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/app-marketplace.png?raw=true)
6. Click `Install`.
![sezzle_app_install_step1](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/install.png?raw=true)
7. Check the PCI Compliance box, then click `Confirm` to start the installation.
![sezzle_app_install_step2](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/pci.png?raw=true)

### BigCommerce Sandbox Testing

1. Select the `Test Mode` as `Yes` from `Sezzle Settings` in `Payments` section of BigCommerce admin. Make sure you are doing this on your `dev`/`staging` website.
![bigcommerce sandbox testing](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/sandbox-testing.png?raw=true)
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order` and you should be redirected to the Sezzle checkout page. If prompted, sign in.
![onepage bigcommerce payment movement](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/checkout.png?raw=true)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://sandbox.dashboard.eu.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (EU)</a> to see the test order you just placed.

### BigCommerce Live Checkout

1. Select the `Test Mode` as `No(Recommended)` from `Sezzle Settings` in `Payments` section of BigCommerce admin.
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
![bigcommerce checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/cart.png?raw=true)
3. Click `Place Order`.
![onepage bigcommerce payment movement](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/checkout.png?raw=true)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete it.

### Widget Pre-Configuration

1. Make sure you have the `Sezzle` App installed in your store. <br/>
2. Go to `Apps` > `Sezzle`.<br/>
3. Copy your `Merchant ID` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle App of your BigCommerce admin.<br/>
4. Check the `Add Sezzle Widget Script` box. This will inject the widget script into the product and cart pages of your store.<br/>
5. Save the configuration.<br/>
![sezzle_app_control_panel](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/control-panel.png?raw=true)
6. Go to `Storefront` > `Script Manager`. Confirm that the Sezzle Widget scripts appear in the list of installed scripts. <br/>
![sezzle_app_script_manager](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/script-manager.png?raw=true)
7. Widget is now ready to be configured.<br/>
8. For finalizing the widget configuration, click `Request Addition of Widgets` in the widget step of your <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/checklist">Sezzle Merchant Dashboard Setup Checklist (EU)</a>.

### Troubleshooting

If testing was unsuccessful, review the following:

* Site has Optimized One-Page Checkout enabled.
* Go to `Advanced Settings` > `Checkout`.
* API Keys were entered correctly.
* To avoid typos or extra spaces, use the Copy icon in the <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu/sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard (EU)</a>.
* If you have multiple accounts with Sezzle, each store has its own merchant ID and API Keys that are tied to the store's URL.
* `Add Sezzle Widget Script` box is checked.
* Widget script is present on your website and reflects the `Merchant ID `from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Sezzle Merchant Dashboard (EU)</a>.
* Go to a product page on your website.
* Right-click then select `Inspect`.
* In the `Elements` tab, search for `widget.sezzle`.

### Manual Theme Integration

If the Sezzle App fails to maintain the widget script on the product pages, or to add the script manually for additional pages, complete the following steps:

1. Go to `Storefront` > `Script Manager`.
2. Click the `Create a Script` button.
3. Set `Name of script` to `Sezzle Widget`.
4. Set `Location on page` to `Footer`.
5. Set `Select pages where script will be added` to `All pages`.
6. Set `Script category` to `Essential`.
7. Set `Script type` to `Script`.
8. In the Script content area, copy+paste the script, then click Save.

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

Instructions may vary slightly depending on your active plug-ins. If the issue persists after completing the above steps, look for other available features that allow the addition of a custom HTML code snippet to the site footer. If no such feature is found, the below steps may be followed as a last resort:

1. Go to `Storefront` > `My Themes`.
2. In your `Current Theme`, click `Advanced` then select `Edit Theme Files`.
3. In the confirmation window, click `Edit Theme Files`.
4. In the file list, go to `templates` > `pages`, then select `product.html`.
5. Copy+paste the script into the very bottom of the file, then click `Save and Apply Files`.
6. Repeat the previous step for the cart.html file.

For any kind of assistance, reach out to `merchantsupport@sezzle.com`.

### Uninstall Steps

1. Go to `Apps` > `My Apps`.
2. Under the Sezzle App, click `Uninstall`.
![sezzle uninstall](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bigcommerce/myapps.png?raw=true)
3. Toggle the button against `Sezzle` under `Store Setup`>`Payments`>`Online Payment Methods` to disable `Sezzle` as a payment option.

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
![Bold cashier](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bold/bold-apps.png?raw=true)
4. In the Bold Cashier left sidebar, click `Marketplace`, then find `Sezzle` and click `Install`.
![Bold install](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bold/bold-marketplace.png?raw=true)
5. Click `Allow` to accept permissions and complete the installation.
![Bold permissions](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bold/bold-permissions.png?raw=true)
6. Installation is complete.
![Bold marketplace](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bold/bold-configure.png?raw=true)

### Bold Cashier Live Checkout

1. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
2. Click `Complete Order`.
3. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
4. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.

### Uninstall Steps

1. Go to your Bold Cashier `Marketplace` and scroll to find `Sezzle`.
2. Click `Uninstall`.
![Bold marketplace](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/bold/bold-configure.png?raw=true)


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
#### (US/CA)
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>

#### (EU)
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Private API Key</a>
<br><br>
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
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
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
#### (US/CA)
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>

#### (EU)
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
<br><br>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#overview-of-integration-flow).

### CommentSold Admin Configuration

1. In your CommentSold admin, go to `Setup`.
2. Click `Payment Gateways`.
3. Copy your `Private Key` and `Public Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a>, and paste them into the corresponding fields in the Sezzle configuration page of your CommentSold admin.
4. Click `Update Keys`.
![sezzle_payment_config](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/commentsold/payment-gateways.png?raw=true)<br/>
5. Installation is complete.

### CommentSold Live Checkout

1. In the Sezzle configuration page of your CommentSold admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and uncheck the `Use Sandbox` checkbox, then save the configuration.
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete.


## Lightspeed

This guide describes how to integrate Sezzle into your Lightspeed website so that you can provide Sezzle as a payment option for your customers.

Sezzle's Lightspeed App is certified and available [here](https://www.lightspeedhq.com/ecommerce/store/apps/sezzle/) in the App Store.

After integrating Sezzle, your Lightspeed site will:

* Offer Sezzle as a payment option on the checkout page.
* Refund Sezzle payments from your Lightspeed backend.
* Display Sezzle promotional messaging.
* Authorize and capture payments.

### Integration Steps Overview

1. [Install and configure](#install-the-sezzle-lightspeed-app) the Sezzle <a class="external-link" target="_blank" href="https://www.lightspeedhq.com/ecommerce/store/apps/sezzle/">Lightspeed App</a>
2. [Test your integration](#lightspeed-live-checkout)
3. **(Optional)** [Sandbox Testing](#lightspeed-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account.
* Please visit our <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/signup">signup page</a> if you don't have an account.
2. Make sure you have the following Sezzle details handy.

#### (US/CA)
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>

#### (EU)
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/business">Merchant ID</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>
<br><br>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#order-payment-flow).

### Install the Sezzle Lightspeed App

* Log in to your Lightspeed back office.
* Navigate to `Apps > App Store`.
* Search for `Sezzle` in the search bar and click on the Sezzle app.
![sezzle lightspeed_app_search](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/app_search.png?raw=true)
* Click on `Install App` in the top right corner.
![sezzle lightspeed_app_install_page](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/app_install.png?raw=true)
* Grant permission by clicking on `Grant access`.
![sezzle lightspeed_app_grant_page](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/app_grant.png?raw=true)
* On successful install, you will be redirected to Sezzle App.
![sezzle lightspeed_app](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/app.png?raw=true)

### Configure Sezzle

Navigate to the Sezzle App by clicking on `Go to App` located at `Apps > Purchased Apps > Sezzle`.
![sezzle lightspeed_app_navigate](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/app_navigation.png?raw=true)

#### Account Verification

* Copy your `Public Key` and `Private Key` from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/">Sezzle Merchant Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/">Sezzle Merchant Dashboard (EU)</a>, and paste them into the corresponding fields.
* Click on `link sezzle account` to link your Sezzle account with your Lightspeed store.
* The message `Account successfully linked` is displayed when the account is verified.

#### Settings

* Toggle the `Sezzle at Checkout` checkbox to enable (`On`) or disable (`Off`) Sezzle as a payment option in the checkout page.
![sezzle lightspeed_checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/checkout.png?raw=true)
* Toggle the `Sezzle Widgets` checkbox to enable (`On`) or disable (`Off`) Sezzle widget in the PDP and Cart pages.
![sezzle lightspeed_widget](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/widget.png?raw=true)

### Payment Refund

* In your Lightspeed back office, go to `Orders > Orders`.
* Select the order to be refunded.
* Click `Add Credit Invoice`.
* Select the item and enter the quantity.
* Ensure that Status is set to `Not Paid`.
* Click `Add`.
* Cancelling a `Paid` order will also refund the payments for the `Paid` Invoices.
* On successful refund:
* An invoice with the status of `Not Paid` will be created in the `Orders > Invoices` section.
* The Order Status will be displayed as `Refunded` in your Sezzle Merchant Dashboard.

### Lightspeed Sandbox Testing

1. A Lightspeed test store must be used for Sandbox testing.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Buy` to be redirected to the Sezzle checkout page. If prompted, sign in.
![lightspeed checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/checkout.png?raw=true)
4. Enter the payment details using [test data](#test-data), then click `Complete Order`.
5. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
6. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (US/CA)</a> or <a class="external-link" target="_blank" href="https://sandbox.eu.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard (EU)</a> to see the test order you just placed.

### Lightspeed Live Checkout

1. Installing the Sezzle App in a live Lightspeed store will result in live checkouts.
2. On your website, add an item to your cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Buy`.
![lightspeed](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/lightspeed/checkout.png?raw=true)
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete it.

## Magento 2

This guide describes how to integrate Sezzle into your Magento 2 website so that you can provide Sezzle as a payment option for your customers.

[Sezzle's Magento 2 extension](https://marketplace.magento.com/sezzle-sezzlepay.html) is certified in the marketplace and can also be [downloaded from github](https://github.com/sezzle/sezzle-magento2).

After integrating Sezzle, your site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your Magento 2 order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.
5. Offer instant and delayed capture.

### Integration Steps Overview

1. [Install and configure](#install-the-sezzle-magento-2-extension) the Sezzle <a class="external-link" target="_blank" href="https://marketplace.magento.com/sezzle-sezzlepay.html">Magento 2 extension</a>
2. [Test your integration](#magento-2-live-checkout)
3. **(Optional)** [Sandbox Testing](#magento-2-sandbox-testing)

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

And familiarize yourself with [the transaction flow when buying with Sezzle](#order-payment-flow).

### Install the Sezzle Magento 2 Extension

In the following section, `[magento]` refers to your Magento 2 root directory.

#### Using the Composer

Go to the Magento 2 installation directory, then run the below commands:

1. `composer require sezzle/sezzlepay`
2. `php bin/magento setup:upgrade`
3. `php bin/magento setup:di:compile`
4. `php bin/magento setup:static-content:deploy`
5. `php bin/magento cache:clean`

#### Manual Method

1. Download the .zip file from [Sezzle's github repository](https://github.com/sezzle/sezzle-magento2).
2. Unzip the file
3. Navigate to `Magento` `[Magento]/app/code/` using `SFTP` or `SSH`.
4. Copy `Sezzle` directory from unzipped folder to `[Magento]/app/code/`.
5. Open a terminal window and run the following command to enable `Sezzle`:
* ```php bin/magento module:enable Sezzle_Sezzlepay```
6. Run the `Magento` setup upgrade:
* ```php bin/magento setup:upgrade```
7. Run the `Magento` Dependencies Injection Compile:
* ```php bin/`magento` setup:di:compile```
8. Run the `Magento` Static Content deployment:
* ```php bin/magento setup:static-content:deploy```
9. Log in to `Magento` Admin and navigate to `System > Cache Management`.
10. Flush the cache storage by selecting `Flush Cache Storage`.

You can now directly navigate from the Configuration Page to get signed up for `Sezzle`. To do so, you need to click on `Register for Sezzle` which will redirect you to the `Sezzle Merchant Signup` Page. If you have the details already, you can simply click on `I've already set up Sezzle, I want to edit my settings` to move ahead.

1. In your Magento 2 `[Magento]/app/code/` directory, create a directory named `Sezzle`.
2. Inside the new `Sezzle` directory, create a directory named `Sezzlepay`.
3. Inside the new `Sezzlepay` directory, extract the files from <a class="external-link" target="_blank" href="https://github.com/sezzle/sezzle-magento2">this repository</a>.
4. Open the command line and run these commands:
* `php bin/magento module:enable Sezzle_Sezzlepay`
* `php bin/magento setup:upgrade`
* `php bin/magento setup:di:compile`
* `php bin/magento setup:static-content:deploy`
5. Log in to your Magento 2 admin and go to `System/Cache Management`.
6. Flush the cache storage by selecting `Flush Cache Storage`.

### Upgrade the Magento 2 Extension
#### Using the Composer
1. Open terminal and navigate to `Magento` root path.
2. Execute the following commands in the terminal:
* `composer update sezzle/sezzlepay`
* `php bin/magento setup:upgrade`
* `php bin/magento setup:di:compile`
* `php bin/magento setup:static-content:deploy`
* `php bin/magento cache:clean`

### Configure Sezzle

#### Payment Configuration

* In the `Magento` admin site, navigate to `Stores > Configuration > Sales > Payment Methods > Sezzle > Payment Settings`
* Select the Payment Mode: `Live`, or `Sandbox` for testing.
* Enter your `Merchant UUID`, `Public Key` and `Private Key`. These can be found at the [Sezzle Merchant Dashboard (US/CA)](https://dashboard.sezzle.com/merchant/) or [Sezzle Merchant Dashboard (EU)](https://dashboard.eu.sezzle.com/merchant/).
* Select the `Payment Action`. `Authorize only` will only authorize payment and requires that the payment be captured later. `Authorize and Capture` will perform both in a single step.
* Set the `Merchant Country` as per the origin.
* Set `Min Checkout Amount` to restrict Sezzle payment method below that amount.
* Set `Payment from Applicable Countries` to `Specific Countries`.
* Set `Payment from Specific Countries` to `United States` or `Canada` as Sezzle is currently available for US and Canada only.
* Select `Enable Customer Tokenization`. `Yes` prompts the customer to allow their account to be tokenized. See [Customer tokenization](#customer-tokenization)
* Save the configuration and clear the cache.

#### In-Context Configuration

* Set `Enable In-Context Solution` to `Yes` to enable In-Context Checkout.
* Set `In-Context Checkout Mode` to `IFrame` or `PopUp` to select how Sezzle Checkout is hosted.

#### Settlement Report Configuration

* Set `Enable Settlement Reports` to `Yes` to enable the Settlement Reports Dashboard.
* Set `Range` to a value based on which you want to fetch the Settlement Reports.
* Set `Enable Automatic Syncing` to fetch the Settlement Reports asynchronously. (Note that this requires `cron` to be enabled.)
* Set Schedule and Time of Day for the automatic sync to run.

#### Widget Configuration

* Set `Enable Static Widget Module` to `Yes` to load the Sezzle Widget from your server, or `No` to load it from the Sezzle server.
* Set `Enable Widget in PDP` to `Yes` when adding the widget script in the Product Display Page, helping to enable the `Sezzle Widget` Modal in PDP.
* Set `Enable Widget in Cart Page` to `Yes` when adding the widget script in the Cart Page, helping to enable `Sezzle Widget` Modal in the Cart Page.
* Save the configuration and clear the cache.
<aside class="notice">
Make sure to add <code>&lt;div id="sezzle-widget"/></code> after the price element in the PDP and Cart theme files once you have enabled the Static Widget module.
</aside>

#### Developer Configuration

* Enable the log tracker to trace the `Sezzle` checkout process.
* Set `Send Logs to Sezzle` to `Yes` to send the logs to Sezzle on a periodic basic. (Note that this requires cron to be enabled.)
* You may download the latest logs by clicking on `Sezzle Log`.
* Save the configuration and clear the cache.

####Your store is now ready to accept payments through Sezzle.

### Frontend Functionality

* When you have correctly set up `Sezzle`, you will see `Sezzle` as a payment method in the checkout page.
* Select `Sezzle` and continue.
* Click `Continue to Sezzle` or `Place Order` to be redirected to the `Sezzle Checkout` to complete the checkout. If your account is tokenized, skip the next two steps, you will not be redirected to Sezzle.
* **[Optional]** To tokenize your account in the Sezzle checkout, check the box `Approve {Store Name} to process payments from your Sezzle account for future transactions. You may revoke this authorization at any time in your Sezzle Dashboard`.
* Click `Complete Order` to complete your purchase.
* On successful order placement, you will be redirected to the order confirmation page.

### Capture Payment

* If `Payment Action` is set to `Authorize and Capture`, capture will be performed instantly from the extension after order is created and validated in `Magento`.
* If `Payment Action` is set to `Authorize`, you will need to capture the payment manually from the `Magento` admin using the following steps:
* Go the order and click on `Invoice`.
* Verify your input in the `Create Invoice` page and click on `Save` to create the invoice.
* This will automatically capture the payment in `Sezzle`.

### Refund Payment

* Go to `Sales > Orders` in the `Magento` admin.
* Select the order for which you want to refund the payment.
* Click on `Credit Memo` and verify your input in the `Create Credit Memo` page.
* Save it to initiate the refund in `Sezzle`.
* Check the `Order Status` in the `Sezzle Merchant Dashboard`. `Refunded` indicates that payment has been fully refunded while `Partially Refunded` indicates that the payment has been partially refunded.

### Release Payment

* Go to `Sales > Orders` in the `Magento` admin.
* Select the order for which you want to release the payment.
* Click on `Void` and confirm your action.
* Check the `Order Status` in `Sezzle Merchant Dashboard`. `Deleted due to checkout not being captured before expiration` indicates that the payment has been fully released. Magento does not support partial releases.

### Order Verification in Magento Admin

* Log in to `Magento` admin, navigate to `Sales > Orders`, and select the order to verify.
* If Order Status is `Processing` and `Total Paid` equals `Grand Total`, then payment was successfully captured by `Sezzle`.
* If Order Status is `Pending` and `Total Paid` does not equal `Grand Total`, then payment is authorized but yet not captured.
* If Order Status is `Closed`, then payment has been refunded.
* If Order Status is `Canceled`, then payment has been released.

### Order Verification in Sezzle Merchant Dashboard

* Log in to `Sezzle Merchant Dashboard`, navigate to `Orders`, and select the order to veriy.
* `Approved` status indicates that payment was successfully captured by `Sezzle`.
* `Authorized, uncaptured` indicates that payment was authorized but yet not captured.
* `Refunded` status indicates that the payment was refunded.
* `Deleted due to checkout not being captured before expiration` status indicates that either the payment was not captured before the authorization expired, or that the payment has been released.

### Customer Tokenization Details

* Log in to `Magento` admin, navigate to `Customers > All Customers`, and select the customer to view tokenization details.
* If the customer is tokenizard, the `Sezzle` tab will appear.
* The `Customer UUID`, `Expiration Date` and `Status` will appear on the tab.

### Settlement Reports

* Log in to `Magento` admin and navigate to `Reports > Sales > Sezzle Settlement`. A list of recent Settlement Reports will be shown.
* To make a quick sync, enter the `From` and `To` Date and click on `Sync`.
* Click on `Download` from the `Action` column to download a Settlement Report.
* To view details of a particular Settlement Report, click on `View` from the `Action` column. You may also download the Settlement Report details from the Settlement Report view.
* You can download the Settlement Report in `CSV` or `Excel` format.

### Magento 2 Sandbox Testing

* In the `Sezzle` configuration page of your `Magento` admin, enter the `Sandbox` `API Keys` from your [Sezzle Merchant Sandbox Dashboard](https://sandbox.dashboard.sezzle.com/merchant/) and set the `Payment Mode` to `Sandbox`, and save the configuration. Make sure you are doing this on your `dev/staging` website.
* On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
* To pay with Sezzle:
* If customer is not tokenized, click `Continue to Sezzle`.
* If customer is tokenized, click `Place Order`.
* If In-Context checkout, click `Pay with Sezzle`.
* For In-Context checkout, the Sezzle checkout will be hosted in the configured mode, `iFrame` or `Popup`. Otherwise, you will be redirected to the Sezzle checkout.
* Sign In or Sign Up to continue.
* Enter the payment details using test data, then advance to the final page.
* Check the `Approve {Website Name} to process payments from your Sezzle account for future transactions. You may revoke this authorization at any time in your Sezzle Dashboard` to tokenize your account.
* If your account is tokenized, the order will be placed without redirection.  Otherwise you will be redirected to Sezzle Checkout to complete the purchase.
* After payment is completed at Sezzle, you will be directed to your site's successful payment page.
* `Sandbox` testing is complete. You can log in to your `Sezzle Merchant Sandbox Dashboard` to see the test order you just placed.

### Magento 2 Live Checkout

1. In the Sezzle configuration page of your Magento 2 admin, enter the API Keys from your <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Sezzle Merchant Dashboard</a> and disable `Sandbox/Test` as the `API Mode`, then save the configuration.
2. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
3. Click `Place Order`.
4. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
![sezzle checkout](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/sezzle-checkout.png?raw=true)
5. **Warning** Don't complete the payment. Your checkout is now live, so you will be charged if you complete it.

### Troubleshooting

If testing was unsuccessful, review the following:

* Sezzle-Magento2 extension is the latest version.
* Sezzle extension is enabled.
* Go to `System` > `Configuration` > `Sales` > `Payment Methods` > `Sezzle` and ensure `Enabled` dropdown is reflecting `Yes`.
* `Merchant ID` was entered correctly.
* API Keys were entered correctly.
* It is recommended to use the Copy icon in the [Sezzle Merchant Dashboard (US/CA)](https://dashboard.sezzle.com/merchant/settings/apikeys) or [Sezzle Merchant Dashboard (EU)](https://dashboard.eu.sezzle.com/merchant/settings/apikeys) to avoid typos or extra spaces.
* If you have multiple accounts with Sezzle, the merchant ID and API Keys are tied to only one URL.
* Cache Storage was flushed.
* Widget script is present on your website and reflects the `Merchant ID` from your
[Sezzle Merchant Dashboard (US/CA)](https://dashboard.sezzle.com/merchant/settings/business) or [Sezzle Merchant Dashboard (EU)](https://dashboard.eu.sezzle.com/merchant/settings/business).
* Go to a product page on your website.
* Right-click then select `Inspect`.
* In the `Elements` tab, search for `widget.sezzle`.

## Mojo

This guide describes how to integrate Sezzle into your Mojo website so that you can provide Sezzle as a payment option for your customers. After integrating Sezzle, your site will:

1. Offer Sezzle as a payment option on the checkout page.
2. Refund Sezzle payments from your Mojo order management system.
3. Display Sezzle promotional messaging.
4. Authorize and capture payments.

### Integration Steps Overview

1. Add Sezzle to your store as a payment method
2. [Test your integration](#mojo-sandbox-testing)

### Before You Begin

1. You should have a Sezzle merchant account.
2. Make sure you have the following Sezzle details handy.<br>
#### US/CA
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.sezzle.com/merchant/settings/apikeys">Private API Key</a>

#### EU
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Public API Key</a>
* <a class="external-link" target="_blank" href="https://dashboard.eu.sezzle.com/merchant/settings/apikeys">Private API Key</a>
<br><br>
3. Familiarize yourself with [the transaction flow when buying with Sezzle](#order-payment-flow).

### Add Payment Method

1. Log into your Mojo admin account and navigate to the `Integrations` tab.
2. Select Payment Systems in the dropdown menu and click on `Sezzle` in the next window.
3. Toggle Enable to ‘ON’.
4. Next, toggle Test Mode to ‘OFF’ unless  you are testing your integration in Sandbox. For Sandbox testing instructions, see [Mojo sandbox testing](#mojo-sandbox-testing).
5. Next, enter your Sezzle Public and Private Key within the appropriate fields.
![mojo_integration](https://github.com/jeffreySezzle/test-category/blob/main/Merchant-Guide-v2/images/integrations/mojo/mojo-integration.png?raw=true)
6. In order to ensure consistency between your webpage offers and payment options upon final checkout, take a moment to navigate to your Site Settings to enable or disable those features that you want enabled with Sezzle.
* NOTE: the Hide pay-over-time solutions switch will be automatically hidden for those customers that choose the multi-pay order option.
7. Once satisfied with your transaction flow, click the blue Save Changes button.
<aside class="notice">Mojo will use customer tokenization for upsells. See [customer tokenization](#customer-tokenization) or contact Mojo for more information. </aside>

### Mojo Sandbox testing

1. In the `Sezzle` Payment Systems configuration page of your `Integrations` tab, enter the [Sandbox API Keys](#sandbox) from your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a>.
2. Next, toggle Test Mode to ‘ON’, then save the configuration. Make sure you are doing this on your `dev`/`staging` website.
3. On your website, add an item to the cart, then proceed to `Checkout` and select `Sezzle` as the payment method.
4. Complete the Mojo checkout process, and you should be redirected to the Sezzle checkout page. If prompted, sign in.
5. Enter the payment details using [test data](#test-data), then click `Complete Order`.
6. After the payment is completed on Sezzle, you should be redirected back to your website and see a successful payment page.
7. **Sandbox testing is complete**. You can log in to your <a class="external-link" target="_blank" href="https://sandbox.dashboard.sezzle.com/merchant">Sezzle Merchant Sandbox Dashboard</a> to see the test order you just placed.

### Mojo Live Checkout

1. On your store website, add an item to your cart, then proceed to check out and select `Sezzle` as the payment method.
2. Follow the Mojo checkout process until you are redirected to Sezzle.
3. If you are redirected to the Sezzle checkout page, your integration is complete. **Congratulations!**
<br/>**Warning:** Don't complete the payment. Your checkout is now live, so you will be charged if you complete it.
