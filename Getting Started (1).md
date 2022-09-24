Using AIQUA, you can engage your customers across various channels from a single platform. In a nutshell, AIQUA allows you to:
* **[Collect User Data](doc:user-data-collection)** including user behaviors on your websites and apps as well as your own set of user data (e.g. your CRM system).
* **[Segment Audience](doc:audiences)** using the data collected as conditions.
* Create customized **[Campaigns](doc:campaigns)** to target different audience segments.
* Access **[Performance Reports](doc:campaign-performance-page)** to understand how your campaigns are performing. 

<br>

## What Can I Know About My Users?
Having user data is crucial in creating campaigns that are relevant to your users. By integrating with Appier Enterprise Service SDK ("Appier SDK"), you will be able to collect the following data about your online users.
* User attributes from your websites and apps (e.g. email submitted through account registration)
* User events from your websites and apps (e.g. a user added products to cart)

In addition, you may have your own set of data for your registered members (e.g. your CRM system) that you can upload to AIQUA.
* Offline user attributes (e.g. user's birthday from your CRM system)
* Offline user events (e.g. purchase data of a registered member in your physical stores)

By linking the above online and offline data, AIQUA helps you gain a holistic view of your users across platforms. See [User Data Collection](doc:user-data-collection) for more details.

<br>

## What Can I Do with AIQUA?
Once you have data about your users, you can segment audience and create personalized campaigns. Here's an example on how you can use AIQUA to reduce cart abandonment.

* **Collect User Data**: Some users logged in to your website and added products to their shopping cart. By having purchase data from your online and offline platforms on AIQUA, you can single out the users who did not proceed to checkout.

* **Segment Audience**: On AIQUA dashboard, you can create an audience segment to include users who have added products to cart, and then exclude those who have purchased. You now have a segment of users who left products in the cart without checking out.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0ee98e3-Screen_Shot_2021-01-08_at_10.23.02_AM.png",
        "Screen Shot 2021-01-08 at 10.23.02 AM.png",
        1650,
        546,
        "#f3f6f8"
      ]
    }
  ]
}
[/block]
* **Create Customized Campaign**: Now you can create an email campaign to remind users about products forgotten in their carts and offer free shipping. By utilizing AIQUA's [Dynamic Content](doc:dynamic-content-for-personalizing-creatives) feature, you can include each user's name and information about the actual products in each user's cart.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9c218cd-Screen_Shot_2021-01-08_at_1.10.58_AM.png",
        "Screen Shot 2021-01-08 at 1.10.58 AM.png",
        1734,
        838,
        "#edeeed"
      ]
    }
  ]
}
[/block]
<br>

---------------

To be able to collect user data and create campaigns, you will need to complete the following integrations.

[block:api-header]
{
  "title": "1. Integrate Platforms with Appier SDK"
}
[/block]
First, you need to integrate your website and app with the Appier Enterprise Service SDK ("Appier SDK"). This enables your website and app to show campaign notifications to users, and to collect basic user data such as cookies, device types, and site visits. 

See instructions on how to integrate different platforms:
  * **Website:** See [Integrating with the Appier Web SDK](doc:integrating-with-the-aiqua-web-sdk).
  * **Android App:** See [Android SDK Overview](doc:versions-for-android-sdk-integration).
  * **iOS App:** See [iOS SDK Overview](doc:ios-sdk-integration-overview).

[block:callout]
{
  "type": "info",
  "title": "Note:",
  "body": "* If your website is using multiple domains (e.g. www.abc.com, www.abc2.com) or subdomains (e.g. blog.abc.com, shop.abc.com), refer to [Cross-Domain Integration](doc:cross-domain-integration).\n\n* If your mobile app uses React Native framework, refer to:\n**React Native:** See [Installing the SDK via React Native](doc:installing-the-sdk-via-react-native)."
}
[/block]
<br>
[block:api-header]
{
  "title": "2. Collect Custom User Data"
}
[/block]
The basic user data Appier SDK can automatically collect by default are called **default user data**. In addition to default user data, you need to decide what other data you want to collect about your users, and set your website and app to collect these **custom user data**.

* [User Data Collection](doc:user-data-collection): Read the overview on how user data is tracked and used in AIQUA.
* [Default user data](doc:default-aiqua-parameters): See the list of default events and attributes collected by Appier SDK.
* [Custom User Data](doc:custom-user-data): See how to collect custom events and attributes.








<br>
[block:api-header]
{
  "title": "3. Upload Offline User Data"
}
[/block]
If you have additional user data in your database (e.g. your CRM system), you can upload these data to AIQUA using these methods:
* [Upload via dashboard as a CSV file](doc:updating-user-profiles)  
* [Upload via API](ref:bulk-upload-offline-users)

<br>

[block:api-header]
{
  "title": "4. Third-Party Integrations"
}
[/block]
If you want to send campaigns via email, SMS, or LINE, you need to integrate AIQUA with these third-party services.
  * **Email:** See [Email Integration](doc:email-integration).
  * **SMS:** See [SMS Integration](doc:sms-integration). 
  * **LINE:** See [LINE Integration](doc:line-integration).


<br>