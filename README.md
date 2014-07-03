*This project is a billing provider plugin to [android-store](https://github.com/soomla/android-store).*

## android-store-nokia-store

This project is billing service plugin for [android-store](https://github.com/soomla/android-store) that support the Nokia Store for the Nokia X device family. It should be working right now, but may require some in-depth testing. See [Contribution](##Contribution) for more details.

The modifications made to the original Google Play billing plugin have been done according [to the procedure described here](http://developer.nokia.com/resources/library/nokia-x/nokia-in-app-payment/nokia-in-app-payment-porting-guide.html).

## Getting Started

In order to work with this plugin you first need to go over android-store's [Getting Started](https://github.com/soomla/android-store#getting-started).

The steps to integrate this billing service are also in android-store's [Selecting Billing Service](https://github.com/soomla/android-store#google-play) but we will also write them here for convenience:

1. Clone the repo

    ```
    git clone https://github.com/Marneus68/android-store-nokia-store.git
    ```

2. Run the build script

    ```
    cd android-store-nokia-store
    ./build_all
    ```

3. Add `AndroidStoreNokiaStore.jar` from the `build` folder to your project.
4. Make the following changes in AndroidManifest.xml:

  Add the following permission (for the Nokia Store):

    ```xml
    <uses-permission android:name="com.nokia.payment.BILLING" />
    ```

    Add the IabActivity to your `application` element, the plugin will spawn a transparent activity to make purchases. Also, you need to tell us what plugin you're using so add a meta-data tag for that:

    ```xml
    <activity android:name="com.soomla.store.billing.nokia.NokiaStoreIabService$IabActivity"
        android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen"/>
    <meta-data android:name="billing.service" android:value="nokia.NokiaStoreIabService" />
    ```

5. Since the Nokia Store doesn't work with a public key like Google Play, the `setPublicKey` method won't do anything. However, to maintain a full compatibility with the Google Play Iab Plugin, this method still exists. No matter what you provide it with, this will not impact the use of the billing plugin in any way.

    ```Java
    NokiaStoreIabService.getInstance().setPublicKey("foo");
    ```


6. The Nokia Store doesn't allow test purchases per say. The only way for you to test out the In App elements is to assign specified test ID's to your elements as described [here](http://developer.nokia.com/resources/library/nokia-x/nokia-in-app-payment/nokia-in-app-payment-porting-guide.html#toc_TestingInAppPurchases). The complete list of ID's available for testing can be found [here](http://developer.nokia.com/resources/library/nokia-x/nokia-in-app-payment/nokia-in-app-payment-developer-guide/product-ids-for-testing-purposes.html).

    For the Nokia Store, it is recommend that you open the IAB Service and keep it open in the background in cases where you have an in-game storefront. This is how you do that:

    When you open the store, call:  

    ```Java
    StoreController.getInstance().startIabServiceInBg();
    ```

    When the store is closed, call:  

    ```Java
    StoreController.getInstance().stopIabServiceInBg();
    ```


## Contribution

This plugin may require some additionnal testing. You're welcome to test, report and create feature requests for this project ! If you find and fix a bug, I'b be more than happy to accept pull requests !

## Licenses

This work is based on the [android-store-google-play](https://github.com/soomla/android-store-google-play) project which is part of the SOOMLA project. As such, I'm sharing it with the very same license:  

MIT License.
+ http://www.opensource.org/licenses/MIT

Part of the code present in this repository can be under a different licenses, see the header of each file for more information. Namely [INokiaIAPService.aidl](https://github.com/Marneus68/android-store-nokia-store/blob/master/src/com/nokia/payment/iap/aidl/INokiaIAPService.aidl) is distributed by Microsoft Mobile under the Apache License, Version 2.0.
+ http://www.apache.org/licenses/LICENSE-2.0

