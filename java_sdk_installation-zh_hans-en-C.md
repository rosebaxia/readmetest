# Java SDK User Guide

This guide will show you how to access your project using the Java SDK. If you want to know what data the gaming industry should collect and how it should be collected, check out the access example. If you want to get a detailed description of the API interface, you can check out the Java SDK API documentation.

\*\*The latest version is: \*\*1.1.16

\*\*Updated on: \*\*2019-05-30


## 1\. Initialize the SDK

1\. To install the SDK using Maven, please place the following dependency information in the `pom.xml` file:

```xml <dependencies> // others... <dependency> <groupId>cn.thinkingdata</groupId> <artifactId>thinkingdatasdk</artifactId> <version>1.1.16</version> </dependency> </dependencies> ```

2\. Initialize the SDK

You can get an SDK instance in three ways (please refer to the API documentation for overloading other Consumer constructors):

\** (1) LoggerConsumer: \** Write local files in batch in real time. Files are separated by days and need to be uploaded with LogBus

```java//Use LoggerConsumer ThinkingDataAnalytics tga = new ThinkingDataAnalytics (new ThinkingDataAnalytics.loggerConsumer (LOG_DIRECTORY) ); ```

LOG\_DIRECTORY is a local folder address. You only need to set the LOGBus listening folder address to this address, and you can use LogBus to listen and upload data.

\** (2) ProduceKafka: \** Write data to Kafka in real time and need to transfer data from Kafka with LogBus

```java//use produceKaFka produceKaFka produce = new produceKaFka (“ip:port”, "topic”); //If Kafka requires other parameters, use the following method to add the parameter //produce.setProps (“key “, "value”); ThinkingDataAnalytics tga = new ThinkingDataAnalytics (produce); ```


**(3) BatchConsumer: BatchConsumer transmits data to TA server in real time in batches, does not need to be matched with transmission tools, and is not recommended for use in production environments**

```java//Use BatchConsumer ThinkingDataAnalytics tga = new ThinkingDataAnalytics (new ThinkingDataAnalytics.batchConsumer (SERVER_URI, APP_ID)); ```

Server\_URI is the URI to transfer the data, and APP\_ID is the APP ID of your project

If you are using a cloud service, enter the following URL:

http://receiver.ta.thinkingdata.cn/logagent

If you are using a privatized deployment version, enter the following URL:

http://数据采集地址/logagent



## 2\. Sending events

After the SDK is initialized, you can call track to upload events. Generally, you may need to upload a dozen or hundreds of different events. If you are using the TGA background for the first time, we recommend that you upload a few key events first.

If you have questions about what kind of event you need to send, check out the Quick Use Guide and Access Samples for more information.


### a) Sending events

You can call track to upload events. It is recommended that you set the attributes of the event and the conditions for sending information according to the previously combed documents. Here is an example of player fees:

\`\`\`java //initialize SDK ThinkingDataAnalytics tga = new ThinkingDataAnalytics (new ThinkingDataAnalytics.batchConsumer (SERVER\_URI, APP\_ID));

//Set the guest ID “ABCDEFG123456789" String distinct \_id = “ABCDEFG123456789";

//Set account ID “TA\_10001" String account\_id = “TA\_10001";

//Set the event property Map<String, Object> properties = new HashMap<String, Object> ();

//Set the time when the event occurs. If not set, it will be used as the current time by default. Note #time的类型必须是Date properties.put (” #time “, new Date ());

//Set the user's IP address. The TA system will parse the user's geolocation information according to the IP address. If it is not set, it will not report properties.put (” #ip “, “192.168.1.1") by default;

properties.put (“Product\_Name”, “Monthly Card”); properties.put (“Price”, 30); properties.put (“OrderID”, “abc\_123");

//The upload event contains the user's visitor ID and account ID. Please note the order of the account ID and guest ID tga.track (account\_id, distingut\_id, “payment”, properties); \`\`\`\*\*Note: \*\*In order to ensure that the guest ID and account ID can be bound smoothly, if your game Visitor ID and account ID will be used. We strongly recommend that you upload both IDs at the same time. Otherwise, there will be cases where accounts cannot be matched, resulting in repeated calculation of users. For specific ID binding rules, please refer to the chapter on User Identification Rules.

* The name of the event is a String type, can only start with a letter, can contain numbers, letters, and underscore “\_”, up to 50 characters in length, and is not sensitive to case of letters.
* The property of an event is a Map object in<String, Object> which each element represents a property.  
* The value of Key is the name of the property and is of type String. It can only be a preset property, or starts with a letter, contains numbers, letters, and underscores “\_”. The maximum length is 50 characters, and is not sensitive to letter case.  
* Value is the value of this property and supports String, Number, Boolean, and Date.


### b) Set public event properties

For some properties that need to appear in all events, you can call setSuperProperties to set them as public event properties, which will be added to all events uploaded using track.
  
\`\`java Map<String, Object> superProperties= new HashMap<String, Object> (); //Set public property: server name superProperties.put (“server\_name”, “S10001 “);//Set public property: server version superProperties.put (“server\_version”, “1.2.3");//Set public event property tga.setSuperProperties (superProperties);

Map<String, Object> properties = new HashMap<String, Object> (); //Set event property properties.put (“Product\_Name”, “Monthly Card”); properties.put (“Price”, 30); //Upload the event, at which time the event will have the public attribute and the event's property tga.track (account\_id, distinct \_id, “payment”, properties);

/* is equivalent to adding these attributes to each event * properties.clear (); * properties.put (“server\_name”, “S10001"); * properties.put (“server\_version”, “1.2.3"); * properties.put (“Product\_Name”, “Monthly Card”); * properties.put (“Price”, 30); * tga.track (account\_id, distinct \_id, "payment”, properties); \*/ \`\`\`  
  
* The public event property is also a Map object<String, Object>, where each element represents a property.  
* The value of Key is the name of the property and is of type String. It can only be a preset property, or starts with a letter, contains numbers, letters, and underscores “\_”. The maximum length is 50 characters, and is not sensitive to letter case.  
* Value is the value of this property and supports String, Number, Boolean, and Date.

If you call setSuperProperties to set a public event property that was previously set, the previous property value will be overwritten. If the public event property and the key of a property in the track upload event are duplicated, the property of that event overrides the public event property: “java Map<String, Object> superProperties= new HashMap<String, Object> (); superProperties.put (“server\_name”, “S10001"); superProperties.put (“server\_version”, “1.2.3"); //set the public event property tga.setSuperProperties ( SuperProperties);


superProperties.clear (); superProperties.put (“server\_name”, “Q12345");//Set the public event property again. At this time, “server\_name” is overwritten and the value is “Q12345" TGA.setSuperProperties (SuperProperties);

Map<String, Object> properties = new HashMap<String, Object> (); properties.put (“Product\_Name”, “Month Card”); //Set a property that duplicates the public event attribute SuperProperties.put (“server\_version”, “1.2.4");   
//Upload the event, the attribute value of “server\_version” will be overwritten with “1.2.4", and the value of “server\_name” will be “Q12345" tga.track (account\_id, distingut\_id, “payment”, properties); \`\`\`if you want to clear All public event properties are empty, and clearSuperProperties can be called.

## 3\. User properties

The user property setting interfaces currently supported by the TGA platform are user\_set, user\_setOnce, user\_add, and user\_del.

### a) user\_set

For general user properties, you can call user\_set to set them. The property uploaded using this interface will overwrite the original attribute value. If the user attribute does not exist before, the user attribute will be created, and the type is the same as the type of the passed attribute.

\`\`java Map<String, Object> UserSetProperties = new HashMap<String, Object> (); UserSetProperties.put (“user\_name”, “ABC”); userSetProperties.put (” #time “, new Date ()); //upload user attributes tga.user\_set (account\_id, distinct \_id, userSetProperties);

UserSetProperties.clear (); UserSetProperties.put (“user\_name”, "abc”); UserSetProperties.put (” #time “, new Date ()); //Upload user properties again, at this time” The value of “user\_name” will be overwritten with “abc” tga.user\_set (account\_id, discrimint\_id, userSetProperties); \`\`\`

The user property set by user\_set is a Map object<String, Object> where each element represents a property.  
The value of Key is the name of the property and is of type String. It is specified that it can only start with a letter, contain numbers, letters, and underscore “\_”. The maximum length is 50 characters, and is not sensitive to letter case.  
Value is the value of this property and supports String, Number, Boolean, and Date.

### b) user\_setOnce

If the user property you want to upload is set only once, you can call user\_setOnce to set it. If the property has a value before, this information will be ignored, and then take the player username as an example:

\`\`java Map<String, Object> UserSetOnceProperties = new HashMap<String, Object> (); userSetOnceProperties.put (“user\_name”, “ABC”); userSetOnceProperties.put (” #time “, new Date ()); //Upload the user property, create a new “user\_name”, and the value is “ABC” tga.user\_set (account\_id, UserSetOnceProperties);

UserSetOnceProperties.clear (); UserSetOnceProperties.put (“user\_name”, "abc”); UserSetOnceProperties.put (“user\_age” ,18); userSetOnceProperties.put (” #time “, new Date ());//Upload the user property again. At this time, the value of “user\_name” will not be overwritten and will still be “ABC”; the value of “user\_age” is 18 tga.user\_set (account\_id, distinct \_id, UserSetOnceProperties); \`\`\`

The user property types and restrictions set by user\_setOnce are the same as user\_set.

### c) user\_add

When you want to upload a numeric property, you can call user\_add to add the property. If the property is not set yet, it will be calculated after assigning a value of 0\. You can pass in a negative value, which is equivalent to subtracting. Here is an example of the accumulated payment amount:

\`\`java Map<String, Object> userAddProperties = new HashMap<String, Object> (); userAddProperties.put (“total\_revenue” ,30); userAddProperties.put (” #time “, new Date ()); //upload user properties, at which time the value of “total\_revenue” is 30 tga.user\_add (account\_id, differentit\_id, userAddProperties);

userAddProperties.clear (); userAddProperties.put (“total\_revenue” ,60); userAddProperties.put (” #time “, new Date ()); //Upload user properties again, at this time” The value of “total\_revenue” will add up to 90 tga.user\_add (account\_id, differentit\_id, userAddProperties); \`\`\`

The user property types and restrictions set by user\_add are the same as user\_set, but only numeric user attributes are supported. </font>

### d) user\_del

If you want to delete a user, you can call user\_del to delete the user. You will no longer be able to query the user properties of that user, but events generated by that user can still be queried. This operation may have irreversible consequences, so please use caution

```java tga.user_del(account_id,distinct_id); ```

## 4\. Other actions

### a) Immediate submission of data

```java tga.flush(); ```

Immediately submit data to the appropriate receiver

### b) Close sdk
	
```java tga.close(); ```

Close and exit the SDK. Please call this interface before shutting down the server to avoid data loss in the cache

## 5\. Related Preset Properties

### 5.1 Preset properties for all events

The following preset properties are included in all events (including automatic collection events) in the Java SDK

Attribute name|Chinese name|Description —|—|— #ip | IP address|user's IP address, need to be set manually. TGA will use this to get the user's geolocation information #country | Country|User's country, generate #country\_code based on IP address | Country code | Country code of user's country (ISO 3166-1 alpha-2, two uppercase English letters), generate #province | province | user's province according to IP address, #city | city | user's city, generate #lib according to IP address | SDK type|type of SDK you access, such as Java, etc. #lib\_version | SDK Version | The version of the Java SDK you have connected to


## ChangeLog

#### v1.1.14 2019/04/25

* Compatible with Java 1.7
* Optimized the reporting mechanism of LoggerConsumer

#### v1.1.13 2019/04/11

* Optimize the performance and stability of data reporting
* Adjusted the default parameters of Consumer


#### v1.1.9 2018/09/03

* BatchConsumer adds asynchronous transmission function. See API documentation for details


