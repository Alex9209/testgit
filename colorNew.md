---
title: 色彩标注服务
type: reference
order: 5
---

对图片内容进行色彩分析

## 调用说明

```Python
from productai import Client

cli = Client(ACCESS_KEY_ID, SECRET_KEY)

# 获取全图颜色分析API
api = cli.get_color_analysis_api(sub_type='everything')

# 通过url来分析图片颜色
resp = api.query(url='http://xxxx', granularity='major', return_type='w3c')

# 或者直接上传本地图片来分析图片颜色
with open("fashion.jpg") as f_image:
    resp = api.query(f_image, granularity='major', return_type='w3c')
```

```php
use ProductAI;
$product_ai = new ProductAI\API($access_key_id='xxxx', $secret_key='xxxx');

//获取全图颜色分析API，通过url来分析图片颜色
$result = $product_ai->imageColorAnalysis($image='http://xxxx', $type='everything', $granularity='major', $return_type= 'w3c');

//或者通过上传本地图片来分析图片颜色
$result = $product_ai->imageColorAnalysis($image='@C:/xxx.jpg', $type='everything', $granularity='major', $return_type= 'w3c');
```

```java
IProfile profile = new DefaultProfile();
profile.setAccessKeyId(ACCESS_KEY_ID);
profile.setSecretKey(SECRET_KEY);
profile.setVersion("1");
profile.setGlobalLanguage(LanguageType.Chinese);
IWebClient client = new DefaultProductAIClient(profile);

//获取全图颜色分析API
ClassifyByImageUrlRequest request = new ClassifyByImageUrlRequest("everything", " _0000072");
request.getOptions().put("granularity", "major");
request.getOptions().put("return_type", "basic");
request.setLanguage(LanguageType.Chinese);

//使用URL来分析图片颜色
request.setUrl("https://xxx");

//或者使用本地文件来分析图片颜色
request.setImageFile(new File(".\\xxx.jpg"));

ClassifyResponse response = client.getResponse(request);
String base64Json = response.getResponseBase64String();
String json = new String(new BASE64Decoder().decodeBuffer(base64Json));
```

```shell
//获取全图颜色分析API
curl -X POST -H 'x-ca-version: 1.0' -H 'x-ca-accesskeyid: YOUR_API_KEY' -d 'url=http://xxx.jpg&type=everything&granularity=major&return_type=basic' https://api.productai.cn/color/ _0000072
```


>注：关于sub_type的类型，请分别参见 [全图色彩分析](#%E5%85%A8%E5%9B%BE%E8%89%B2%E5%BD%A9%E5%88%86%E6%9E%90)，[人物色彩分析](#%E4%BA%BA%E7%89%A9%E8%89%B2%E5%BD%A9%E5%88%86%E6%9E%90)，[物品色彩分析](#%E7%89%A9%E5%93%81%E8%89%B2%E5%BD%A9%E5%88%86%E6%9E%90)

### 输入参数说明

|参数名称|类型|说明|必选|限制|
|:-|:-|:-|:-|:-
|url|字符串|Query图片的链接，query方法第一个参数|是|与f_image参数二选一|
|f_image|文件|Query图片文件内容，query方法第一个参数|是|与url参数二选一|
|granularity|字符串|分析粒度|是|必须是major, detailed, dominant三个值之一|
|return_type|字符串|返回值类型|是|必须是basic, w3c, ncs, cncs四个值之一|

#### granularity参数

以全图搜索为例，使用下面图片进行讲解
![示例图片1](https://cdn.malong.com/web/help/flight.png)

|选项|结果|说明|
|-|-|-|
|major|<span style="background-color: #5BA3D3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #D8D5C9">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>|只返回整个图片最主要的两种颜色
|detailed|<span style="background-color: #5BA3D3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: ##D8D5C9; border: 1px solid black">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #5D7669">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #AEC051">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #39AC4D">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>|返回图片内更多的颜色
|dominant|<span style="background-color: #5BA3D3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>|只返回一个图片主颜色

#### return_type参数

|选项|结果|说明|
|-|-|
|basic|<span style="background-color: #5BA3D3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>蓝|
|w3c|<span style="background-color: #5BA3D3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>深天蓝|[W3C](https://www.w3.org/TR/css-color-3/)的CSS工作组发布CSS颜色模块
|ncs|<span style="background-color: #5BA3D3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>1050-R90B|[NCS](http://ncscolour.com/)色卡是来自瑞典的色彩设计工具，它以眼睛看颜色的方式来描述颜色。表面颜色定义在NCS色卡中，同时给出一个色彩编号。
|cncs|<span style="background-color: #5BA3D3">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> { "chroma": "Medium", "cncs": "1196022", "color": "蓝紫色", "lightness": "Medium", "rgb": ["107", "160", "206"]}|[CNCS](http://www.cncscolor.com/)是由中国纺织信息中心联合国际国内色彩专家和机构，经多年精心调研而开发的色彩体系，力求为服装设计师和相关机构提供当前最时尚的色彩信息和色彩管理解决方案。

### 输出字段说明

#### 200 HTTP状态码

服务器端成功处理，并返回结果

```js
{
    "is_err": 0,
    "request_id": "d54dab1a-e098-11e7-bb18-eea500941bf4",
    "results": [
        {
            "cncs": {
                "chroma": "Medium",
                "cncs": "1196022",
                "color": "蓝紫色",
                "lightness": "Medium",
                "rgb": [
                    "107",
                    "160",
                    "206"
                ]
            },
            "freq": 1,
            "hex": "#5BA3D3",
            "rgb": [
                91,
                163,
                211
            ]
        }
    ],
    "time": 0.149
}
```di

|结果字段|类型|说明|
|:-|:---:|:-
|is_err| 整数　|１表示有错误，0表示没有错误
|request_id| 字符串|本次调用的唯一ID，可以用于和ProductAI团队进行联调分析
|results|数组|颜色分析结果集合
|cncs|字典|cncs相关结果信息
|ncs|字符串|ncs相关结果信息
|basic|字符串|基础颜色结果信息
|w3c|字符串|w3c标准的结果信息
|freq|整数|颜色比例
|hex|字符串|16进制颜色码
|rgb|数组|RGB颜色码
|time| 浮点数| 服务器端计算时间

#### 非200 HTTP状态码

服务器端遇到错误

```js
{
    "data": {
        "is_err": 1,
        "message": "Download image failed.",
        "request_id": "224b2a28-e08f-11e7-bb18-eea500941bf4",
        "time": 0.013
    },
    "status": 408
}
```

|结果字段|类型|说明|
|:-|:---:|:-
|data|字典|所有信息包装到data中
|status| 整数　|HTTP状态码
|is_err| 整数　|１表示有错误，0表示没有错误
|message| 字符串 |错误信息详情
|request_id| 字符串|本次调用的唯一ID，可以用于和ProductAI团队进行联调分析
|time| 浮点数| 服务器端计算时间

### 错误信息说明

API和SDK使用http状态码来通知客户端本次调用成功和失败

|http状态码|error_code|说明|
|:-|:-|:-
|200|N/a|数据提交成功
|400|N/a|参数granularity不在指定值范围，参数return_type不在指定值范围，参数loc不符合规范，Query图片为空
|408|N/a|Query图片下载失败，Query图片格式解析失败，图片颜色分析失败，上游计算服务超时

## 服务列表

目前有全图，前景物体，人物三种分析类型．三者之间的区别，使用下面图片进行讲解（granularity参数为major）
![示例图片1](https://cdn.malong.com/web/help/tooopen_sy_121404229348.jpg)

|分析类别|结果|说明|
|-|-|-|
|全图色彩分析|<span style="background-color: #F5F6FC">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #AA8B7F">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>| 灰色背景，和前景人物的暗红色主色调会被分析出来
|人物色彩分析|<span style="background-color: #E2DFE0">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #976E65">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #757179">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>| 可以看到，返回的颜色以人物身上服装颜色为主
|物品色彩分析|<span style="background-color: #BC9F96">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #FFFFFF; border: 1px solid black">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> <span style="background-color: #67656E">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>| 由于本图去除背景色后，物体即为人物，所以返回颜色与 **人物色彩分析**　结果类似

### 全图色彩分析

对整张图片进行色彩分析，依据输入参数返回对应体系的色彩。对应的sub_type参数填everything

```Python
from productai import Client

cli = Client(ACCESS_KEY_ID, SECRET_KEY)

# 获取全图颜色分析API
api = cli.get_color_analysis_api(sub_type='everything')
```

```php
use ProductAI;
$product_ai = new ProductAI\API($access_key_id='xxxx', $secret_key='xxxx');

//获取全图颜色分析API
$result = $product_ai->imageColorAnalysis($image='http://xxxx', $type='everything', $granularity='major', $return_type= 'w3c');
```

```java
IProfile profile = new DefaultProfile();
profile.setAccessKeyId(ACCESS_KEY_ID);
profile.setSecretKey(SECRET_KEY);
profile.setVersion("1");
profile.setGlobalLanguage(LanguageType.Chinese);
IWebClient client = new DefaultProductAIClient(profile);

//获取全图颜色分析API
ClassifyByImageUrlRequest request = new ClassifyByImageUrlRequest("everything", " _0000072");
request.getOptions().put("granularity", "xxx");
request.getOptions().put("return_type", "xxx");
```

```shell
//获取全图颜色分析API
curl -X POST -H 'x-ca-version: 1.0' -H 'x-ca-accesskeyid: YOUR_API_KEY' -d 'url=http://xxx.jpg&type=everything&granularity=xxx&return_type=xxx' https://api.productai.cn/color/ _0000072
```

### 人物色彩分析

对图片中的时尚衣物进行色彩分析，去除掉了背景色彩和肤色，并依据输入参数返回对应体系的色彩。对应的sub_type参数填person_outfit

```Python
from productai import Client

cli = Client(ACCESS_KEY_ID, SECRET_KEY)

# 获取人物颜色分析API
api = cli.get_color_analysis_api(sub_type='person_outfit')
```

```php
use ProductAI;
$product_ai = new ProductAI\API($access_key_id='xxxx', $secret_key='xxxx');

//获取人物颜色分析API
$result = $product_ai->imageColorAnalysis($image='http://xxxx', $type='person_outfit', $granularity='xxx', $return_type= 'xxx');
```

```java
IProfile profile = new DefaultProfile();
profile.setAccessKeyId(ACCESS_KEY_ID);
profile.setSecretKey(SECRET_KEY);
profile.setVersion("1");
profile.setGlobalLanguage(LanguageType.Chinese);
IWebClient client = new DefaultProductAIClient(profile);

//获取人物颜色分析API
ClassifyByImageUrlRequest request = new ClassifyByImageUrlRequest("person_outfit", " _0000074");
request.getOptions().put("granularity", "xxx");
request.getOptions().put("return_type", "xxx");
```

```shell
//获取人物颜色分析API
curl -X POST -H 'x-ca-version: 1.0' -H 'x-ca-accesskeyid: YOUR_API_KEY' -d 'url=http://xxx.jpg&type=person_outfit&granularity=xxx&return_type=xxx' https://api.productai.cn/color/ _0000074
```

### 物品色彩分析

去除掉了背景的颜色，并依据输入参数返回对应体系的色彩。对应的sub_type参数填foreground

```Python
from productai import Client

cli = Client(ACCESS_KEY_ID, SECRET_KEY)

# 获取前景物体颜色分析API
api = cli.get_color_analysis_api(sub_type='everything')
```

```php
use ProductAI;
$product_ai = new ProductAI\API($access_key_id='xxxx', $secret_key='xxxx');

// 获取前景物体颜色分析API
$result = $product_ai->imageColorAnalysis($image='http://xxxx', $type='foreground', $granularity='xxx', $return_type= 'xxx');
```

```java
IProfile profile = new DefaultProfile();
profile.setAccessKeyId(ACCESS_KEY_ID);
profile.setSecretKey(SECRET_KEY);
profile.setVersion("1");
profile.setGlobalLanguage(LanguageType.Chinese);
IWebClient client = new DefaultProductAIClient(profile);

//获取前景物体颜色分析API
ClassifyByImageUrlRequest request = new ClassifyByImageUrlRequest("foreground", " _0000073");
request.getOptions().put("granularity", "xxx");
request.getOptions().put("return_type", "xxx");
```

```shell
//获取前景物体颜色分析API
curl -X POST -H 'x-ca-version: 1.0' -H 'x-ca-accesskeyid: YOUR_API_KEY' -d 'url=http://xxx.jpg&type=foreground&granularity=xxx&return_type=xxx' https://api.productai.cn/color/ _0000073
```

#modify colorNew