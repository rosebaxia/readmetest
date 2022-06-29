# Java SDK User Guide

This guide will show you how to use the Java SDK to plug into your project. If you want to know what data the gaming industry should collect and how, check out[the Access Samples](/user_guide/guide_book.md). If you want to get a detailed description of the API interface, you can check the[Java SDK API documentation](/technical_document/API_document/java_api_document.md).

The latest version is: \*\*1.1.16

\*\*Updated on:\*\*2019-05-30


## 1\. Initialize the SDK

1\. To place the SDK using Maven, place the`following dependency information in the pom .xml`file:

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

You can obtain an SDK instance in three ways (see API documentation for overloading other Consumer constructors):

\*\*(1) LoggerConsumer: \** Batch real-time write local files, files are separated by days, you need to upload with LogBus

```java
Use LoggerConsumer
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.LoggerConsumer(LOG\_DIRECTORY));
```

``LOG\_DIRECTORY is the folder address written locally, you only need to set the listening folder address of LogBus to the address here, and you can use LogBus to listen to and upload data.

\*\*(2) ProduceKafka: \** Write data to Kafka in real time, you need to transfer data from Kafka with LogBus

```java
Use ProductKafka
ProduceKafka produce = new ProduceKafka("ip:port","topic");
If Kafka requires additional parameters, use the following method to add parameters
//produce.setProps("key","value");
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(produce);
```


**(3) BatchConsumer:**Transfer data to TA servers in real time in batches, without the need for transfer tools, and**<font color="red">is not recommended for use in production environments</font>**

```java
Use The BatchConsumer
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.BatchConsumer(SERVER\_URI, APP\_ID));
```

``SERVER\_URI is the uri of the transferred data,`APP_ID`the APP ID of your project

If you are using a cloud service, enter the following URL:

http://receiver.ta.thinkingdata.cn/logagent

If you are using a version of the privatized deployment, enter the following URL:

<font color="red">http:// data collection address</font>/logagent



## 2\. Send the event

After the SDK initialization is complete, you can call`the track`to upload events, in general, you may need to upload a dozen to hundreds of different events, if you are using the TGA background for the first time, we recommend that you upload a few key events first.

If you have questions about what kind of events you need to send, you can check out[the Quick Usage Guide](/getting_started/getting_started_menu.md)and[Access Samples](/user_guide/guide_book.md)for more information.


### a) Send an event

You can call`a track`to upload an event, and it is recommended that you set the properties of the event and the conditions for sending information based on the documentation you have previously combed, using the player's payment as an example here:

\`\`\`java
Initialize the SDK
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.BatchConsumer(SERVER\_URI, APP\_ID));

Set the guest ID "ABCDEFG123456789"
String distinct\_id = "ABCDEFG123456789";

Set the account ID "TA\_10001"
String account\_id = "TA\_10001";

Set the event properties
Map<String,Object> properties = new HashMap<String,Object>();

Set the time when the event occurs, if not set, the default use is the current time,**note that** the #time的类型必须是Date
properties.put("#time", new Date());

Set the user's IP address, the TA system will resolve the user's geographical location information according to the IP address, if not set, it will not be reported by default
properties.put("#ip", "192.168.1.1");

properties.put("Product\_Name", "monthly card");
properties.put("Price", 30);
properties.put("OrderId", "abc\_123");

//
Upload the event, including the user's guest ID and account ID, please pay attention to the order of account ID and guest ID
tga.track(account\_id,distinct\_id,"payment",properties);
\`\`\`
\*\*Note:** In order to ensure that the guest ID and account ID can be successfully bound, if you will use the guest ID and account ID in your game, we strongly recommend that you upload both IDs at the same time,<font color="red">otherwise there will be an account that cannot be matched, resulting in user double calculation</font>and the specific ID binding rules can refer[to the chapter of user identification rules](/user_guide/user_identify.md).

* The name of the event is`of the String`type, can only start with a letter, can contain numbers, letters, and an underscore "\_", with a maximum length of 50 characters, and is not sensitive to letter casing.
* The properties of an event are a`Map< String, and object >`object, where each element represents a property.  
* The value of Key is the name of the property, of`type String`which can only be preset properties](/user_guide/preset_properties.md)or starts with a[letter, contains numbers, letters, and underscores "\_", is up to 50 characters long, and is not sensitive to letter case.  
* Value is the value of this property and supports`String``Number`Boolean``and`Date`.


### b) Set the public event properties

For some properties that need to appear in all events, you can call`setSuperProperties`to set these properties to public event properties, and the public event properties will be added to all events uploaded using`track`.
  
\`\`\`java
Map<String, Object> superProperties = new HashMap<String,Object>();
Set the public property: Server name
superProperties.put("server\_name", "S10001");
Set the public property: Server Version
superProperties.put("server\_version", "1.2.3");
Set public event properties
tga.setSuperProperties(superProperties);

Map<String,Object> properties = new HashMap<String,Object>();
Set the event properties
properties.put("Product\_Name", "monthly card");
properties.put("Price", 30);
Upload an event with public properties and properties of the event
tga.track(account\_id,distinct\_id,"payment",properties);

/* is equivalent to adding these properties to each event
 \*  properties.clear();
 \*  properties.put("server\_name", "S10001");
 \*  properties.put("server\_version", "1.2.3");
 \* properties.put("Product\_Name", "monthly card");
 \*  properties.put("Price", 30);
 \*  tga.track(account\_id,distinct\_id,"payment",properties);
\*/
\`\`\`  
  
* The public event property is also a`Map< String, Object>`object, where each element represents a property.  
* The value of Key is the name of the property, of`type String`which can only be preset properties](/user_guide/preset_properties.md)or starts with a[letter, contains numbers, letters, and underscores "\_", is up to 50 characters long, and is not sensitive to letter case.  
* Value is the value of this property and supports`String``Number`Boolean``and`Date`.

If you call`setSuperProperties`to set a public event property that has been set previously, the previous property value is overwritten. If the key of a public event property and`a property in a track`upload event duplicates, the properties of that event override the public event properties:
\`\`\`java
Map<String, Object> superProperties = new HashMap<String,Object>();
superProperties.put("server\_name", "S10001");
superProperties.put("server\_version", "1.2.3");
Set public event properties
tga.setSuperProperties(superProperties);


superProperties.clear();
superProperties.put("server\_name", "Q12345");
Set the public event properties again, at which point "server\_name" is overwritten with a value of "Q12345"
tga.setSuperProperties(superProperties);

Map<String,Object> properties = new HashMap<String,Object>();
properties.put("Product\_Name", "monthly card");
Sets properties that duplicate public event properties
superProperties.put("server\_version", "1.2.4");   
Upload an event, at which point the property value of "server\_version" will be overwritten as "1.2.4" and the value of "server\_name" as "Q12345"
tga.track(account\_id,distinct\_id,"payment",properties);
\`\`\`
If you want to empty all public event properties, you can call`clearSuperProperties`.

## 3\. User Attributes

The user attribute setting interfaces currently supported by the TGA platform are user\_set, user\_setOnce, user\_add, and user\_del.

### a) user\_set

For general user attributes, you can call`user_set`to set, using the interface uploaded attributes will overwrite the original attribute values, if the user attribute does not exist before, the user attribute will be created, the type is consistent with the type of the incoming attribute, here to set the user name of the player as an example:

\`\`\`java
Map<String,Object> userSetProperties = new HashMap<String,Object>();
userSetProperties.put("user\_name", "ABC");
userSetProperties.put("#time", new Date());
Upload user attributes
tga.user\_set(account\_id,distinct\_id,userSetProperties);

userSetProperties.clear();
userSetProperties.put("user\_name","abc");
userSetProperties.put("#time", new Date());
Upload the user attribute again, at which point the value of "user\_name" will be overwritten as "abc"
tga.user\_set(account\_id,distinct\_id,userSetProperties);
\`\`\`

``The user property user\_set set is a`Map< String, object>`object, where each element represents a property.  
The value of Key is the name of the property, of`type String`which states that it can only start with a letter, contains numbers, letters, and an underscore "\_", is up to 50 characters long, and is not sensitive to letter casing.  
Value is the value of this property and supports`String``Number`Boolean``and`Date`.

### b) user\_setOnce

If you want to upload a user attribute as long as you set it once, you can call`the user_setOnce`to set it, when the attribute has a value before, this message will be ignored, and then set the player username as an example:

\`\`\`java
Map<String,Object> userSetOnceProperties = new HashMap<String,Object>();
userSetOnceProperties.put("user\_name", "ABC");
userSetOnceProperties.put("#time", new Date());
Upload user attributes, create a new "user\_name" with a value of "ABC"
tga.user\_set(account\_id,distinct\_id,userSetOnceProperties);

userSetOnceProperties.clear();
userSetOnceProperties.put("user\_name","abc");
userSetOnceProperties.put("user\_age",18);
userSetOnceProperties.put("#time", new Date());
Upload the user attribute again, at this time the value of "user\_name" will not be overwritten, it will still be "ABC"; The value of "user\_age" is 18
tga.user\_set(account\_id,distinct\_id,userSetOnceProperties);
\`\`\`

``The user attribute types and restrictions set user\_setOnce are consistent with`the user_set`.

### c) user\_add

When you want to upload a numeric attribute, you can call`user_add`to perform an accumulation operation on the attribute, and if the attribute has not been set, it will be assigned a value of 0 and then calculated, and a negative value can be passed in, which is equivalent to the subtraction operation. Here's an example of the cumulative payment amount:

\`\`\`java
Map<String,Object> userAddProperties = new HashMap<String,Object>();
userAddProperties.put("total\_revenue",30);
userAddProperties.put("#time", new Date());
Upload the user attribute, at this point the value of "total\_revenue" is 30
tga.user\_add(account\_id,distinct\_id,userAddProperties);

userAddProperties.clear();
userAddProperties.put("total\_revenue",60);
userAddProperties.put("#time", new Date());
Upload the user attribute again, and the value of "total\_revenue" adds up to 90
tga.user\_add(account\_id,distinct\_id,userAddProperties);
\`\`\`

``The user attribute types and restrictions set user\_add are consistent with`user_set`<font color="red">but only the user attributes of the incoming numeric type are supported. </font>

### d) user\_del

If you want to delete a user, you can call`user_del`delete this user, you will no longer be able to query the user's user attributes, but the events generated by the user can still be queried,<font color="red">the operation may have irreversible consequences, please use with</font>caution

```java
tga.user\_del(account\_id,distinct\_id);
```

## 4\. Other operations

### a) Submit data immediately

```java
tga.flush();
```

Submit the data to the appropriate receiver immediately

### b) Turn off sdk
	
```java
tga.close();
```

Before shutting down and exiting sdk, call this interface before shutting down the server to avoid data loss in the cache

## 5\. Related preset properties

### 5.1 Preset properties for all events

The following preset properties are preset properties that all events in the Java SDK , including automatic collection of events ) 

Property name |Chinese name | description
---|---|---
\#ip | THE IP address | the user's IP address, which needs to be set manually, and TGA will obtain the user's geolocation information
\#country | The country | the user's country, generated based on the IP address
\#country\_code | Country code | The country code of the user's country (ISO 3166-1 alpha-2, i.e. two capital letters), generated from the IP address
\#province | province | the user's province, generated based on the IP address
\#city | City | the user's city and is generated based on the IP address
\#lib|  SDK type | the type of SDK you access, such as Java
\#lib\_version| SDK Version | the version of the Java SDK you access


## ChangeLog

#### v1.1.14 2019/04/25

* Compatible with Java 1.7
* Optimized the reporting mechanism of loggerConsumer

#### v1.1.13 2019/04/11

* Optimize the performance and stability of data reporting
* Adjusted the default parameters for Consumer


#### v1.1.9 2018/09/03

* BatchConsumer adds asynchronous transfer function, please view API documentation for details


