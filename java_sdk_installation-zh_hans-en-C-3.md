# Java SDK User Guide

This guide will show you how to access your project using the Java SDK. If you want to know what data the gaming industry should collect and how it should be collected, check out the access example. If you want to get a detailed description of the API interface, you can check out the Java SDK API documentation.

\*\*The latest version is: \*\*1.1.16

\*\*Updated on: \*\*2019-05-30


## 1\. Initialize the SDK

1\. To install the SDK using Maven, please place the following dependency information in the pom.xml file:

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

You can get an SDK instance in three ways (please refer to the API documentation for overloading other Consumer constructors):

\** (1) LoggerConsumer: \** Write local files in batch in real time. Files are separated by days and need to be uploaded with LogBus

```java
//Use LoggerConsumer
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.LoggerConsumer(LOG_DIRECTORY));
```

LOG_DIRECTORY is a local folder address. You only need to set the LOGBus listening folder address to this address, and you can use LogBus to listen and upload data.

\** (2) ProduceKafka: \** Write data to Kafka in real time and need to transfer data from Kafka with LogBus

```java
//Use ProduceKafka
ProduceKafka produce = new ProduceKafka("ip:port","topic");
//If Kafka requires other parameters, use the following method to add parameters
//produce.setProps("key","value");
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(produce);
```


**(3) BatchConsumer: BatchConsumer transmits data to TA server in real time in batches, does not need to be matched with transmission tools, and is not recommended for use in production environments**

```java
//Use BatchConsumer
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.BatchConsumer(SERVER_URI, APP_ID));
```

Server_URI is the URI to transfer the data, and APP_ID is the APP ID of your project

If you are using a cloud service, enter the following URL:

http://receiver.ta.thinkingdata.cn/logagent

If you are using a privatized deployment version, enter the following URL:

http://Data collection address/logagent



## 2\. Sending events

After the SDK is initialized, you can call track to upload events. Generally, you may need to upload a dozen or hundreds of different events. If you are using the TGA background for the first time, we recommend that you upload a few key events first.

If you have questions about what kind of event you need to send, check out the Quick Use Guide and Access Samples for more information.


### a) Sending events

You can call track to upload events. It is recommended that you set the attributes of the event and the conditions for sending information according to the previously combed documents. Here is an example of player fees:

```java
//Initialize the SDK
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.BatchConsumer(SERVER_URI, APP_ID));

//Set the visitor ID “ABCDEFG123456789"
String distinct_id = "ABCDEFG123456789";

//Set the account ID “TA_10001"
String account_id = "TA_10001";

//Set event properties
Map<String,Object> properties = new HashMap<String,Object>();

//Set the time the event occurred. If not set, the current time will be used by default, **Note** #time的类型必须是Date 
properties.put("#time", new Date());

//Set the IP address of the user. The TA system will resolve the user's geographical location information based on the IP address. If not set, it will not be reported by default
properties.put("#ip", "192.168.1.1");

properties.put (“Product_Name”, “Monthly Card”);
properties.put("Price", 30);
properties.put("OrderId", "abc_123");

//
The upload event contains the user's visitor ID and account ID. Please note the order of the account ID and visitor ID
tga.track(account_id,distinct_id,"payment",properties);
```
\*\*Note: \** In order to ensure the smooth binding of guest ID and account ID, if you use guest ID and account ID in your game, we strongly recommend that you upload both IDs at the same time. Otherwise, accounts will not match, which will cause users to be counted repeatedly. For specific ID binding rules, please refer to the chapter on User Identification Rules.

* The name of the event is a String type, can only start with a letter, can contain numbers, letters, and underscore “_”, up to 50 characters in length, and is not sensitive to case of letters.
* The property of an event is a Map object in<String, Object> which each element represents a property.  
* The value of Key is the name of the property and is of type String. It can only be a preset property, or starts with a letter, contains numbers, letters, and underscores “_”. The maximum length is 50 characters, and is not sensitive to letter case.  
* Value is the value of this property and supports String, Number, Boolean, and Date.


### b) Set public event properties

For some properties that need to appear in all events, you can call setSuperProperties to set them as public event properties, which will be added to all events uploaded using track.
  
```java
Map<String, Object> superProperties = new HashMap<String,Object>();
//Set public property: server name
superProperties.put("server_name", "S10001");
//Set public properties: server version
superProperties.put("server_version", "1.2.3");
//Set public event properties
tga.setSuperProperties(superProperties);

Map<String,Object> properties = new HashMap<String,Object>();
//Set event properties
properties.put (“Product_Name”, “Monthly Card”);
properties.put("Price", 30);
//Upload the event, in which case the event will have the public properties and the properties of the event
tga.track(account_id,distinct_id,"payment",properties);

/* Equivalent to adding these attributes to each event
 *  properties.clear();
 *  properties.put("server_name", "S10001");
 *  properties.put("server_version", "1.2.3");
 * properties.put (“Product_Name”, “Monthly Card”);
 *  properties.put("Price", 30);
 *  tga.track(account_id,distinct_id,"payment",properties);
*/
```  
  
* The public event property is also a Map object<String, Object>, where each element represents a property.  
* The value of Key is the name of the property and is of type String. It can only be a preset property, or starts with a letter, contains numbers, letters, and underscores “_”. The maximum length is 50 characters, and is not sensitive to letter case.  
* Value is the value of this property and supports String, Number, Boolean, and Date.

If you call setSuperProperties to set a public event property that was previously set, the previous property value will be overwritten. If a public event property and a property in the Track upload event have a duplicate key, the event's properties override the public event property:
```java
Map<String, Object> superProperties = new HashMap<String,Object>();
superProperties.put("server_name", "S10001");
superProperties.put("server_version", "1.2.3");
//Set public event properties
tga.setSuperProperties(superProperties);


superProperties.clear();
superProperties.put("server_name", "Q12345");
//Set the public event property again. At this time, “server_name” is overwritten and the value is “Q12345"
tga.setSuperProperties(superProperties);

Map<String,Object> properties = new HashMap<String,Object>();
properties.put (“Product_Name”, “Monthly Card”);
//Set an attribute that duplicates a common event property
superProperties.put("server_version", "1.2.4");   
//Upload event, the attribute value of “server_version” will be overwritten with “1.2.4", and the value of “server_name” will be “Q12345"
tga.track(account_id,distinct_id,"payment",properties);
```
If you want to clear all public event properties, you can call clearSuperProperties.

## 3\. User properties

The user property setting interfaces currently supported by the TGA platform are user_set, user_setOnce, user_add, and user_del.

### a) user_set

For general user properties, you can call user_set to set them. The property uploaded using this interface will overwrite the original attribute value. If the user attribute does not exist before, the user attribute will be created, and the type is the same as the type of the passed attribute.

```java
Map<String,Object> userSetProperties = new HashMap<String,Object>();
userSetProperties.put("user_name", "ABC");
userSetProperties.put("#time", new Date());
//upload user attributes
tga.user_set(account_id,distinct_id,userSetProperties);

userSetProperties.clear();
userSetProperties.put("user_name","abc");
userSetProperties.put("#time", new Date());
//Upload the user attribute again. At this time, the value of “user_name” will be overwritten with “abc”
tga.user_set(account_id,distinct_id,userSetProperties);
```

The user property set by user_set is a Map object<String, Object> where each element represents a property.  
The value of Key is the name of the property and is of type String. It is specified that it can only start with a letter, contain numbers, letters, and underscore “_”. The maximum length is 50 characters, and is not sensitive to letter case.  
Value is the value of this property and supports String, Number, Boolean, and Date.

### b) user_setOnce

If the user property you want to upload is set only once, you can call user_setOnce to set it. If the property has a value before, this information will be ignored, and then take the player username as an example:

```java
Map<String,Object> userSetOnceProperties = new HashMap<String,Object>();
userSetOnceProperties.put("user_name", "ABC");
userSetOnceProperties.put("#time", new Date());
//Upload user attributes and create a new “user_name” with the value “ABC”
tga.user_set(account_id,distinct_id,userSetOnceProperties);

userSetOnceProperties.clear();
userSetOnceProperties.put("user_name","abc");
userSetOnceProperties.put("user_age",18);
userSetOnceProperties.put("#time", new Date());
//Upload the user attribute again. At this time, the value of “user_name” will not be overwritten, it will still be “ABC”; the value of “user_age” is 18
tga.user_set(account_id,distinct_id,userSetOnceProperties);
```

The user property types and restrictions set by user_setOnce are the same as user_set.

### c) user_add

When you want to upload a numeric property, you can call user_add to add the property. If the property is not set yet, it will be calculated after assigning a value of 0\. You can pass in a negative value, which is equivalent to subtracting. Here is an example of the accumulated payment amount:

```java
Map<String,Object> userAddProperties = new HashMap<String,Object>();
userAddProperties.put("total_revenue",30);
userAddProperties.put("#time", new Date());
//Upload user attributes. At this time, the value of “total_revenue” is 30
tga.user_add(account_id,distinct_id,userAddProperties);

userAddProperties.clear();
userAddProperties.put("total_revenue",60);
userAddProperties.put("#time", new Date());
//Upload the user attributes again. At this time, the value of “total_revenue” will add up to 90
tga.user_add(account_id,distinct_id,userAddProperties);
```

The user property types and restrictions set by user_add are the same as user_set, but only numeric user attributes are supported. </font>

### d) user_del

If you want to delete a user, you can call `user_del` to delete the user. You will no longer be able to query the user properties of that user, but events generated by that user can still be queried.<font color="red">This operation may have irreversible consequences, so please use caution</font>  

```java
tga.user_del(account_id,distinct_id);
```

## 4\. Other actions

### a) Immediate submission of data

```java
tga.flush();
```

Immediately submit data to the appropriate receiver

### b) Close sdk
	
```java
tga.close();
```

Close and exit the SDK. Please call this interface before shutting down the server to avoid data loss in the cache

## 5\. Related Preset Properties

### 5.1 Preset properties for all events

The following preset properties are included in all events (including automatic collection events) in the Java SDK

Attribute name|Chinese name|stating
---|---|---
\#ip | IP address|The IP address of the user needs to be set manually. The TGA will use this to obtain the user's geographical location information
\#country | country|The user's country, based on the IP address
\#country_code | Country code | The country code of the user's country (ISO 3166-1 alpha-2, two uppercase English letters), generated from the IP address
\#province  |provinces|The province where the user is located, based on the IP address
\#city | city|The city where the user is located, based on the IP address
\#lib|  SDK Type|The type of SDK you have access to, such as Java
\#lib_version| SDK version  |The version of the Java SDK you are connected to


## ChangeLog

#### v1.1.14 2019/04/25

* Compatible with Java 1.7
* Optimized the reporting mechanism of LoggerConsumer

#### v1.1.13 2019/04/11

* Optimize the performance and stability of data reporting
* Adjusted the default parameters of Consumer


#### v1.1.9 2018/09/03

* BatchConsumer adds asynchronous transmission function. See API documentation for details


