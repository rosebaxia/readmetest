# Java SDK使用指南

本指南将会为您介绍如何使用Java SDK接入您的项目。如果您想要了解游戏行业应该收集哪些数据以及如何收集，请查阅[接入样例](/user_guide/guide_book.md)。如果您想获得API接口的详细说明，可以查看[Java SDK API文档](/technical_document/API_document/java_api_document.md)。

**最新版本为：**1.1.16

**更新时间为：**2019-05-30


## 1. 初始化SDK

1.使用Maven置入SDK，请在`pom.xml`文件中置入以下依赖信息：

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

2.初始化SDK

您可以通过三种方式获得SDK实例（其他Consumer构造器的重载请参考API文档）：

**(1)LoggerConsumer：**批量实时写本地文件，文件以天为分隔，需要搭配LogBus进行上传

```java
//使用LoggerConsumer
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.LoggerConsumer(LOG_DIRECTORY));
```

`LOG_DIRECTORY`为写入本地的文件夹地址，您只需将LogBus的监听文件夹地址设置为此处的地址，即可使用LogBus进行数据的监听上传。

**(2)ProduceKafka：**实时向Kafka写数据，需要搭配LogBus从Kafka中传输数据

```java
//使用ProduceKafka
ProduceKafka produce = new ProduceKafka("ip:port","topic");
//如果Kafka需要其他参数，请使用如下方法添加参数
//produce.setProps("key","value");
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(produce);
```


**(3)BatchConsumer：**批量实时地向TA服务器传输数据，不需要搭配传输工具，**<font color="red">不建议在生产环境中使用</font>**

```java
//使用BatchConsumer
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.BatchConsumer(SERVER_URI, APP_ID));
```

`SERVER_URI`为传输数据的uri，`APP_ID`为您的项目的APP ID

如果您使用的是云服务，请输入以下URL:

http://receiver.ta.thinkingdata.cn/logagent

如果您使用的是私有化部署的版本，请输入以下URL:

http://<font color="red">数据采集地址</font>/logagent



## 2. 发送事件

在SDK初始化完成之后，您就可以调用`track`来上传事件，一般情况下，您可能需要上传十几到上百个不同的事件，如果您是第一次使用TGA后台，我们推荐您先上传几个关键事件。

如果您对需要发送什么样的事件有疑惑，可以查看[快速使用指南](/getting_started/getting_started_menu.md)以及[接入样例](/user_guide/guide_book.md)了解更多信息。


### a\) 发送事件

您可以调用`track`来上传事件，建议您根据先前梳理的文档来设置事件的属性以及发送信息的条件，此处以玩家付费作为范例：

```java
//初始化SDK
ThinkingDataAnalytics tga = new ThinkingDataAnalytics(new ThinkingDataAnalytics.BatchConsumer(SERVER_URI, APP_ID));

//设置访客ID"ABCDEFG123456789"
String distinct_id = "ABCDEFG123456789";

//设置账号ID"TA_10001"
String account_id = "TA_10001";

//设置事件属性
Map<String,Object> properties = new HashMap<String,Object>();

// 设置事件发生的时间，如果不设置的话，则默认使用为当前时间，**注意** #time的类型必须是Date 
properties.put("#time", new Date());

// 设置用户的ip地址，TA系统会根据IP地址解析用户的地理位置信息，如果不设置的话，则默认不上报
properties.put("#ip", "192.168.1.1");

properties.put("Product_Name", "月卡");
properties.put("Price", 30);
properties.put("OrderId", "abc_123");

//
上传事件，包含用户的访客ID与账号ID，请注意账号ID与访客ID的顺序
tga.track(account_id,distinct_id,"payment",properties);
```
**注：**为了保证访客ID与账号ID能够顺利进行绑定，如果您的游戏中会用到访客ID与账号ID，我们极力建议您同时上传这两个ID，<font color="red">否则将会出现账号无法匹配的情况，导致用户重复计算</font>，具体的ID绑定规则可参考[用户识别规则](/user_guide/user_identify.md)一章。

* 事件的名称是`String`类型，只能以字母开头，可包含数字，字母和下划线“\_”，长度最大为50个字符，对字母大小写不敏感。
* 事件的属性是一个`Map<String,Object>`对象，其中每个元素代表一个属性。  
* Key的值为属性的名称，为`String`类型，规定只能是[预置属性](/user_guide/preset_properties.md)，或以字母开头，包含数字，字母和下划线“\_”，长度最大为50个字符，对字母大小写不敏感。  
* Value为该属性的值，支持`String`、`Number`、`Boolean`和`Date`。


### b\) 设置公共事件属性

对于一些需要出现在所有事件中的属性，您可以调用`setSuperProperties`将这些属性设置为公共事件属性，公共事件属性将会添加到所有使用`track`上传的事件中。
  
```java
Map<String, Object> superProperties = new HashMap<String,Object>();
//设置公共属性：服务器名称
superProperties.put("server_name", "S10001");
//设置公共属性：服务器版本
superProperties.put("server_version", "1.2.3");
//设置公共事件属性
tga.setSuperProperties(superProperties);

Map<String,Object> properties = new HashMap<String,Object>();
//设置事件属性
properties.put("Product_Name", "月卡");
properties.put("Price", 30);
//上传事件，此时事件中将带有公共属性以及该事件的属性
tga.track(account_id,distinct_id,"payment",properties);

/* 相当于在每个事件中加入这些属性
 *  properties.clear();
 *  properties.put("server_name", "S10001");
 *  properties.put("server_version", "1.2.3");
 *  properties.put("Product_Name", "月卡");
 *  properties.put("Price", 30);
 *  tga.track(account_id,distinct_id,"payment",properties);
*/
```  
  
* 公共事件属性同样也是一个`Map<String, Object>`对象，其中每个元素代表一个属性。  
* Key的值为属性的名称，为`String`类型，规定只能是[预置属性](/user_guide/preset_properties.md)，或以字母开头，包含数字，字母和下划线“\_”，长度最大为50个字符，对字母大小写不敏感。  
* Value为该属性的值，支持`String`、`Number`、`Boolean`和`Date`。

如果调用`setSuperProperties`设置先前已设置过的公共事件属性，则会覆盖之前的属性值。如果公共事件属性和`track`上传事件中的某个属性的Key重复，则该事件的属性会覆盖公共事件属性：
```java
Map<String, Object> superProperties = new HashMap<String,Object>();
superProperties.put("server_name", "S10001");
superProperties.put("server_version", "1.2.3");
//设置公共事件属性
tga.setSuperProperties(superProperties);


superProperties.clear();
superProperties.put("server_name", "Q12345");
//再次设置公共事件属性，此时"server_name"被覆盖，值为"Q12345"
tga.setSuperProperties(superProperties);

Map<String,Object> properties = new HashMap<String,Object>();
properties.put("Product_Name", "月卡");
//设置与公共事件属性重复的属性
superProperties.put("server_version", "1.2.4");   
//上传事件，此时"server_version"的属性值会被覆盖为"1.2.4"，"server_name"的值为"Q12345"
tga.track(account_id,distinct_id,"payment",properties);
```
如果您想要清空所有公共事件属性，可以调用`clearSuperProperties`。

## 3. 用户属性

TGA平台目前支持的用户属性设置接口为user\_set、user\_setOnce、user\_add、user\_del。

### a\) user\_set

对于一般的用户属性，您可以调用`user_set`来进行设置，使用该接口上传的属性将会覆盖原有的属性值，如果之前不存在该用户属性，则会新建该用户属性，类型与传入属性的类型一致，此处以玩家设置用户名为例：

```java
Map<String,Object> userSetProperties = new HashMap<String,Object>();
userSetProperties.put("user_name", "ABC");
userSetProperties.put("#time", new Date());
//上传用户属性
tga.user_set(account_id,distinct_id,userSetProperties);

userSetProperties.clear();
userSetProperties.put("user_name","abc");
userSetProperties.put("#time", new Date());
//再次上传用户属性，此时"user_name"的值会被覆盖为"abc"
tga.user_set(account_id,distinct_id,userSetProperties);
```

`user_set`设置的用户属性是一个`Map<String,Object>`对象，其中每个元素代表一个属性。  
Key的值为属性的名称，为`String`类型，规定只能以字母开头，包含数字，字母和下划线“\_”，长度最大为50个字符，对字母大小写不敏感。  
Value为该属性的值，支持`String`、`Number`、`Boolean`和`Date`。

### b\) user\_setOnce

如果您要上传的用户属性只要设置一次，则可以调用`user_setOnce`来进行设置，当该属性之前已经有值的时候，将会忽略这条信息，再以设置玩家用户名为例：

```java
Map<String,Object> userSetOnceProperties = new HashMap<String,Object>();
userSetOnceProperties.put("user_name", "ABC");
userSetOnceProperties.put("#time", new Date());
//上传用户属性，新建"user_name"，值为"ABC"
tga.user_set(account_id,distinct_id,userSetOnceProperties);

userSetOnceProperties.clear();
userSetOnceProperties.put("user_name","abc");
userSetOnceProperties.put("user_age",18);
userSetOnceProperties.put("#time", new Date());
//再次上传用户属性，此时"user_name"的值不会被覆盖，仍为"ABC"；"user_age"的值为18
tga.user_set(account_id,distinct_id,userSetOnceProperties);
```

`user_setOnce`设置的用户属性类型及限制条件与`user_set`一致。

### c\) user\_add

当您要上传数值型的属性时，您可以调用`user_add`来对该属性进行累加操作，如果该属性还未被设置，则会赋值0后再进行计算，可传入负值，等同于相减操作。此处以累计付费金额为例：

```java
Map<String,Object> userAddProperties = new HashMap<String,Object>();
userAddProperties.put("total_revenue",30);
userAddProperties.put("#time", new Date());
//上传用户属性，此时"total_revenue"的值为30
tga.user_add(account_id,distinct_id,userAddProperties);

userAddProperties.clear();
userAddProperties.put("total_revenue",60);
userAddProperties.put("#time", new Date());
//再次上传用户属性，此时"total_revenue"的值会累加为90
tga.user_add(account_id,distinct_id,userAddProperties);
```

`user_add`设置的用户属性类型及限制条件与`user_set`一致，<font color="red">但只支持传入数值型的用户属性。</font>

### d\) user\_del

如果您要删除某个用户，可以调用`user_del`将这名用户删除，您将无法再查询该名用户的用户属性，但该用户产生的事件仍然可以被查询到，<font color="red">该操作可能产生不可逆的后果，请慎用</font>

```java
tga.user_del(account_id,distinct_id);
```

## 4. 其他操作

### a) 立即提交数据

```java
tga.flush();
```

立即提交数据到相应的接收器

### b) 关闭sdk
	
```java
tga.close();
```

关闭并退出sdk，请在关闭服务器前调用本接口，以避免缓存内的数据丢失

## 5. 相关预置属性

### 5.1 所有事件带有的预置属性

以下预置属性，是Java SDK中所有事件（包括自动采集事件）都会带有的预置属性

属性名|中文名|说明
---|---|---
\#ip | IP地址|用户的IP地址，需要进行手动设置，TGA将以此获取用户的地理位置信息
\#country | 国家|用户所在国家，根据IP地址生成
\#country_code | 国家代码 | 用户所在国家的国家代码(ISO 3166-1 alpha-2，即两位大写英文字母)，根据IP地址生成
\#province  |省份|用户所在省份，根据IP地址生成
\#city | 城市|用户所在城市，根据IP地址生成
\#lib|  SDK类型|您接入SDK的类型，如Java等
\#lib\_version| SDK版本  |您接入Java SDK的版本


## ChangeLog

#### v1.1.14 2019/04/25

* 兼容Java 1.7
* 优化了loggerConsumer的上报机制

#### v1.1.13 2019/04/11

* 优化数据上报的性能及稳定性
* 调整了Consumer的默认参数


#### v1.1.9 2018/09/03

* BatchConsumer新增异步传输功能，详情查看API文档


