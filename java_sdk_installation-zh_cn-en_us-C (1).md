# Java SDK User's Guide

This guide will show you how to use the Java SDK to access your project. If you want to know what data should be collected in the gaming industry and how to collect it, check out the [access sample](/user_guide/guide_book.md). If you want to get a detailed description of the API interface, you can check the [Java SDK API documentation](/technical_document/API_document/java_api_document.md).

\*\*The latest version is:\** 1.1.16

\*\*Updated on:\*\*2019-05-30


## 1\. Initialize the SDK

1\. Use Maven to place the SDK, please place the following dependency information in the `pom.xml` file.

```xml
<dependencies>
    // others...
    <dependency>
        <groupId>cn.thinkingdata</groupId>
        <artifactId>thinkingdatasdk</artifactId>
        <version>1.1.16</version>
    </dependency>
</dependencies>
```

2\. Initialize the SDK

You can get SDK instances in three ways (see the API documentation for overloading of other Consumer constructors).

\*\*(1) LoggerConsumer:\** batch real-time write local files, files separated by days, need to be paired with LogBus for upload

```java
// Use LoggerConsumer
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.LoggerConsumer(LOG_DIRECTORY));
```

`LOG_DIRECTORY` is the address of the folder to which you write locally. You can use LogBus to listen for data uploads by simply setting the LogBus listening folder address to the address here.

\*\*(2)ProduceKafka:** Write data to Kafka in real time, need to be paired with LogBus to transfer data from Kafka

```java
// Using ProduceKafka
ProduceKafka produce = new ProduceKafka("ip:port", "topic");
// If Kafka requires additional parameters, please use the following method to add them
//produce.setProps("key","value");
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(produce);
```


**(3) BatchConsumer: **batch real-time data transfer to the TA server, no need to match the transfer tool,**<font color="red">not recommended for use in production environments</font>**

```java
// Use BatchConsumer
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.BatchConsumer(SERVER_URI, APP_ID));
```

`SERVER_URI` is the uri of the transferred data, `APP_ID` is the APP ID of your project

If you are using a cloud service, please enter the following URL:

http://receiver.ta.thinkingdata.cn/logagent

If you are using the private deployment version, please enter the following URL:

http://<font color="red">Data collection address/logagent</font>



## 2\. Sending events

After the SDK initialization is complete, you can call `track` to upload events. In general, you may need to upload a dozen to hundreds of different events, if it is your first time to use TGA backend, we recommend you to upload a few key events first.

If you're confused about what events to send, you can check out [the quick guide](/getting_started/getting_started_menu.md) and [access samples](/user_guide/guide_book.md) for more information.


### a) Sending events

You can call `track` to upload an event, and it is recommended that you set the properties of the event and the conditions for sending the message according to the documentation you combed through earlier, using player payment as an example here.

```java
// Initialize the SDK
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.BatchConsumer(SERVER_URI, APP_ID));

// set the visitor ID "ABCDEFG123456789"
String distinct_id = "ABCDEFG123456789";

// Set the account ID "TA_10001"
String account_id = "TA_10001";

// Set event properties
Map<String,Object> properties = new HashMap<String,Object>();

// Set the time of the event, if not set, the default is the current time, **Note** #time must be of type Date 
properties.put("#time", new Date());

// Set the user's ip address, TA system will parse the user's geolocation information according to the IP address, if not set, it will not report by default.
properties.put("#ip", "192.168.1.1");

properties.put("Product_Name", "Monthly Card");
properties.put("Price", 30);
properties.put("OrderId", "abc_123");

//
Upload events, including the user's visitor ID and account ID, please note the order of account ID and visitor ID
tga.track(account_id,distinct_id,"payment",properties);
```
\*\*Note:** In order to ensure that the visitor ID and account ID can be bound smoothly, if your game will use the visitor ID and account ID, we strongly recommend that you upload both IDs at the same time, <font color="red">otherwise there will be a situation where the account cannot be matched, resulting in double counting of users</font>, please refer to the chapter on [user identification](/user_guide/user_identify.md) rules for specific ID binding rules.

* The name of the event is of type `String`, can only start with a letter, can contain numbers, letters and underscores "_", has a maximum length of 50 characters and is case insensitive.
* The property of the event is a `Map<String,Object>` object, where each element represents a property.  
* The value of Key is the name of the property, of type `String`, specifying that it can only be a [preset property](/user_guide/preset_properties.md), or start with a letter, contain numbers, letters and underscores "_", have a maximum length of 50 characters, and are case insensitive to letters.  
* Value is the value of this property, supporting `String`, `Number`, `Boolean` and `Date`.


### b) Set public event properties

For some properties that need to appear in all events, you can call `setSuperProperties` to set these properties to public event properties, which will be added to all events that use `track` upload.
  
```java
Map<String, Object> superProperties = new HashMap<String,Object>();
// Set public attribute: server name
superProperties.put("server_name", "S10001");
// Set public attribute: server version
superProperties.put("server_version", "1.2.3");
// Set public event properties
tga.setSuperProperties(superProperties);

Map<String,Object> properties = new HashMap<String,Object>();
// Set event properties
properties.put("Product_Name", "Monthly Card");
properties.put("Price", 30);
//Upload event, where the event will have public properties and the properties of the event
tga.track(account_id,distinct_id,"payment",properties);

/* Equivalent to adding these properties to each event
 *  properties.clear();
 * properties.put("server_name", "S10001");
 *  properties.put("server_version", "1.2.3");
 * properties.put("Product_Name", "Monthly Card");
 *  properties.put("Price", 30);
 *  tga.track(account_id,distinct_id,"payment",properties);
*/
```  
  
* A public event property is also a `Map<String, Object>` object, where each element represents a property.  
* The value of Key is the name of the property, of type `String`, specifying that it can only be a [preset property](/user_guide/preset_properties.md), or start with a letter, contain numbers, letters and underscores "_", have a maximum length of 50 characters, and are case insensitive to letters.  
* Value is the value of this property, supporting `String`, `Number`, `Boolean` and `Date`.

If `setSuperProperties` is called to set a public event property that has been previously set, the previous property value is overwritten. If the public event attribute and the Key of an attribute in the `track` upload event duplicate, the attribute of that event overrides the public event attribute: the
```java
Map<String, Object> superProperties = new HashMap<String,Object>();
superProperties.put("server_name", "S10001");
superProperties.put("server_version", "1.2.3");
// Set public event properties
tga.setSuperProperties(superProperties);


superProperties.clear();
superProperties.put("server_name", "Q12345");
// Set the public event property again, at this time "server_name" is overridden and the value is "Q12345"
tga.setSuperProperties(superProperties);

Map<String,Object> properties = new HashMap<String,Object>();
properties.put("Product_Name", "Monthly Card");
// Set properties that duplicate public event properties
superProperties.put("server_version", "1.2.4");   
//Upload event, the property value of "server_version" will be overwritten to "1.2.4" and the value of "server_name" will be "Q12345".
tga.track(account_id,distinct_id,"payment",properties);
```
If you want to clear all public event properties, you can call `clearSuperProperties`.

## 3\. user attributes

The user property setting interfaces currently supported by the TGA platform are user_set, user_setOnce, user_add, and user_del.

### a) user_set

For general user attributes, you can call `user_set` to set them. The attributes uploaded using this interface will overwrite the original attribute values, and if the user attribute does not exist before, it will be newly created with the same type as the incoming attribute, here is an example of a player setting a user name.

```java
Map<String,Object> userSetProperties = new HashMap<String,Object>();
userSetProperties.put("user_name", "ABC");
userSetProperties.put("#time", new Date());
// Upload user attributes
tga.user_set(account_id,distinct_id,userSetProperties);

userSetProperties.clear();
userSetProperties.put("user_name","abc");
userSetProperties.put("#time", new Date());
//Upload the user attribute again, the value of "user_name" will be overwritten with "abc"
tga.user_set(account_id,distinct_id,userSetProperties);
```

The user property set by `user_set` is a `Map<String,Object>` object, where each element represents a property.  
The value of Key is the name of the property, of type `String`, which can only start with a letter, contain numbers, letters and underscores "_", has a maximum length of 50 characters, and is case-insensitive to letters.  
Value is the value of this property, supporting `String`, `Number`, `Boolean` and `Date`.

### b) user_setOnce

If the user attribute you want to upload is set only once, you can call `user_setOnce` to set it. When the attribute already has a value before, this message will be ignored. Again, to set the player username for example.

```java
Map<String,Object> userSetOnceProperties = new HashMap<String,Object>();
userSetOnceProperties.put("user_name", "ABC");
userSetOnceProperties.put("#time", new Date());
//Upload user attributes, create a new "user_name" with the value "ABC"
tga.user_set(account_id,distinct_id,userSetOnceProperties);

userSetOnceProperties.clear();
userSetOnceProperties.put("user_name","abc");
userSetOnceProperties.put("user_age",18);
userSetOnceProperties.put("#time", new Date());
// upload the user attributes again, at this time the value of "user_name" will not be overwritten, it is still "ABC"; the value of "user_age" is 18
tga.user_set(account_id,distinct_id,userSetOnceProperties);
```

`user_setOnce` sets the same type of user attributes and restrictions as `user_set`.

### c) user_add

When you want to upload a numeric attribute, you can call `user_add` to accumulate the attribute, and if the attribute is not yet set, it will be assigned a value of 0 and then calculated, you can pass in a negative value, which is equivalent to a subtraction operation. Here is an example of cumulative payment amount.

```java
Map<String,Object> userAddProperties = new HashMap<String,Object>();
userAddProperties.put("total_revenue",30);
userAddProperties.put("#time", new Date());
//Upload user properties, the value of "total_revenue" is 30 at this time
tga.user_add(account_id,distinct_id,userAddProperties);

userAddProperties.clear();
userAddProperties.put("total_revenue",60);
userAddProperties.put("#time", new Date());
//Upload the user properties again, and the value of "total_revenue" will add up to 90
tga.user_add(account_id,distinct_id,userAddProperties);
```

`user_add` sets the same types of user attributes and restrictions as `user_set`, <font color="red">but only supports passing in numeric user attributes. </font>

### d) user_del

If you want to delete a user, you can call `user_del` to delete this user, you will not be able to query the user properties of this user, but the events generated by this user can still be queried, <font color="red">this operation may have irreversible consequences, please use caution</font>

```java
tga.user_del(account_id,distinct_id);
```

## 4\. Other operations

### a) Submit data immediately

```java
tga.flush();
```

Immediate submission of data to the appropriate receiver

### b) Close the sdk
	
```java
tga.close();
```

Close and exit the sdk, please call this interface before closing the server to avoid data loss in the cache

## 5\. Related preset properties

### 5.1 All events with preset properties

The following preset properties are preset properties that come with all events in the Java SDK, including auto-capture events

Property Name|Chinese Name|Description
---|---|---
\#ip | IP Address|The user's IP address, which needs to be set manually, will be used by TGA to obtain the user's geographic location information
\#country | Country|Country of the user, generated from IP address
\#country_code | Country Code | Country code of the user's country (ISO 3166-1 alpha-2, i.e. two uppercase letters), generated based on the IP address
\#province  |Province|User's province, generated based on IP address
\#city | City|User's city, generated based on IP address
\#lib|  SDK Type|The type of SDK you have access to, such as Java, etc.
\#lib_version| SDK Version  |The version of the Java SDK you have access to


## ChangeLog

#### v1.1.14 2019/04/25

* Java 1.7 compatible
* Optimized the reporting mechanism of loggerConsumer

#### v1.1.13 2019/04/11

* Optimize the performance and stability of data reporting
* Adjusted the default parameters of the Consumer


#### v1.1.9 2018/09/03

* BatchConsumer adds asynchronous transfer function, see API documentation for details


