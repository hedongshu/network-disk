---
date: 2021-11-02 09:46:36
tags:
  - 教程
  - 小程序
title: 领券外卖CPS小程序项目教程
---

## 简介

先领券外卖 CPS 小程序项目|外卖红包 CPS 带好友返利佣金分销系统程序|饿了么美团京东拼多多唯品会联盟源码

## 介绍说明

- 惠聚本地生活系统集合了多平台 CPS、CPA 产品，目前支持的 CPS 业务有本地生活、美团、饿了么等业务。
- 惠聚本地生活系统推广业务不抽佣，不扣手续费。推广美团、饿了么、京东、拼多多、唯品会以及其他业务，推广佣金和官方一致。
- 支持用户 diy 页面，diy 功能，做到每个应用都有自己不一样的风格。

## 功能列表

| 功能                                 | 进度                     |
| :----------------------------------- | :----------------------- |
| 美团联盟（秒杀、酒店、生鲜商超）     | 已完成                   |
| 美团分销（外卖、点评吃喝玩乐、优选） | 已完成                   |
| 饿了么（外卖、生鲜商超）             | 已完成（订单模块进行中） |
| 京东联盟                             | 已完成                   |
| 拼多多联盟                           | 已完成                   |
| 唯品会联盟                           | 已完成                   |
| 话费充值                             | 已完成                   |
| 电影票折扣（蚂蚁星球插件）           | 已完成                   |
| 三级分销                             | 已完成                   |
| 海报分销（拉粉丝）                   | 已完成                   |
| 微信公众号点餐提醒（v1.0.2）         | 已完成                   |
| 小程序点餐提醒（v1.0.2）             | 已完成                   |

## 联系方式

1179601966 (qq)

# 搭建流程

## 前期准备

- 微信小程序（个人小程序不能做美团分销,不建议使用）
- 公众号（订阅号只能做入口，服务号可以做用户账号和小程序互通）
- 个体工商户或公司营业执照（申请美团联盟需要，我们无要求）

## 流程

- 创建小程序，公众号，微信开发平台等（个人只需要小程序创建，个人不能做电影票）
- 配置美团联盟账户和美团分销联盟账户
- 配置阿里妈妈账户
- 配置多多进宝账户
- 配置京东联盟账户
- 配置唯品会联盟账户
- 配置 92 折话费充值返利链接
- 配置火车票购买返利链接
- 把准备好的账户信息填写入后台，具体操作看目录操作指引
- 测试好交付
- 提交审核发布上线

- **本文中所描述均为必须配置，一个都不能落下**
- **请按照文中所说一步一步的配置完成，不会的联系客服！**
- **在配置过程中不要有过多的想法，只需要按照文中所说一步一步的配置完成，让程序走通**
- **程序走通后再尝试自己修改已经配置好的内容**

# 小程序

## 基本信息

- 微信小程序注册地址：[https://mp.weixin.qq.com](https://mp.weixin.qq.com/)
- 填写信息
  ![img](https://img.kancloud.cn/71/82/71828c9b950521e6fe09fdffbac4cfa9_1279x641.png)
- 填写小程序名称、头像、介绍、类目

## 程序类目选择

- 餐饮/菜谱
- 工具 > 报价/比价
- 生活服务-百货/超市便利店

## 配置域名

点击“ 开发管理 ” - “ 开发设置 ”，添加服务器域名

| 类型                  | 域名                                                                                                                                          |
| :-------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------- |
| request 合法域名      | [https://cps.7hds.com](https://cps.7hds.com/)                                                                                                 |
| uploadFile 合法域名   | [https://cps.7hds.com](https://cps.7hds.com/)                                                                                                 |
| downloadFile 合法域名 | [https://cps.7hds.com](https://cps.7hds.com/) [https://p0.meituan.net](https://p0.meituan.net/) [https://cdn.7hds.com](https://cdn.7hds.com/) |

![img](https://img.kancloud.cn/48/c0/48c0bb1e6fc1a5f77d7b285a3eab76ed_1575x904.png)

## 小程序对接

### 登录管理后台

- 后台地址：[https://cps.7hds.com](https://cps.7hds.com/)

### 进入管理后台，创建小程序并填写信息

- 创建，点击新建
  ![img](https://img.kancloud.cn/8e/db/8edb59eb29e1cfa6ddda854a4ecdaf50_1920x868.png)
- 填写资料信息
  ![img](https://img.kancloud.cn/dd/57/dd5703d30ddd27838892112205c402c5_1571x754.png)
  说明： 1.带\*号为必填项 2.小程序 AppId&小程序 AppSecret（获取链接：https://mp.weixin.qq.com/wxamp/devprofile/get_profile?token=1970986726&lang=zh_CN）
  ![img](https://img.kancloud.cn/1a/f9/1af985ef0c519d5b49d44b90d2d144e1_3348x1782.png)![img](https://img.kancloud.cn/1a/f9/1af985ef0c519d5b49d44b90d2d144e1_3348x1782.png) 3.审核版本必填，和你小程序上线的版本要对应上（每次上线审核都要修改，否则有可能审核不通过的情况。） 4.关于我们可填写或不填写，为用户中心做宣传

### 新增小程序首页轮播

![img](https://img.kancloud.cn/8c/99/8c99c3ad3ee8a248fd43d805210b7317_1920x867.png)
说明：
**1. 所属应用就是必须先新增了小程序才能做新增轮播图**
**2. 轮播类型，如下**
![img](https://img.kancloud.cn/f3/9d/f39d7f85fe58af7269a1e5edc095ff22_770x640.png)
**3. 跳转参数及页面活动路径说明**

| 类型           | 路径                                                                                        |
| :------------- | :------------------------------------------------------------------------------------------ |
| 9.9 包邮       | /pages/activity/index?activityType=nine&type=1&navName=9.9 包邮                             |
| 销量榜单       | /pages/activity/index?activityType=sales&type=1&navName=销量榜单                            |
| 高佣榜单       | /pages/activity/index?activityType=commission&type=1&navName=高佣榜单                       |
| 品牌清仓       | /pages/activity/index?activityType=brand&type=1&navName=品牌清仓                            |
| 大牌秒杀       | /pages/activity/index?activityType=seckill&type=1&navName=秒杀专场                          |
| 拼多多百亿补贴 | /pages/activity/platform?activityType=billion&navName=拼多多百亿补贴                        |
| 京东精选家电   | /pages/activity/platform?activityType=wiring&navName=精选家电                               |
| 京东超市       | /pages/activity/platform?activityType=jdsupermarket&navName=京东超市                        |
| 京东自营       | /pages/activity/platform?activityType=ziying&navName=京东自营                               |
| 京东图书聚汇   | /pages/activity/platform?activityType=jdbooks&navName=图书聚汇                              |
| 京东母婴玩具   | /pages/activity/platform?activityType=babyProduct&navName=母婴玩具                          |
| 京东美妆穿搭   | /pages/activity/platform?activityType=cosmetics&navName=美妆穿搭                            |
| 美团           | /pages/takeOut/meituan（/pages/takeOut/meituan?index={number}，激活 tab 的数字，从 0 开始） |
| 饿了么         | /pages/takeOut/elem                                                                         |
| 身边优惠       | /pages/nearby/index                                                                         |
| 邀请分享       | /pages/invitation/index                                                                     |
| 充值话费       | 具体请看充值话费链接获取目录                                                                |
| 火车票         | 具体请看火车票链接获取目录                                                                  |

**4. 标题/名称 区分作用**
**5. 状态 是否显示作用**
**6. 排序 数字越大越排在前面**

# 美团联盟

**提示：美团联盟不能做分销，所以不做订单的采集及订单的展示，需要的直接登录美团联盟后台数据中心查看（美团联盟不支持个人用户，只支持企业用户）**

## 1. 登录美团联盟

您可以通过https://union.meituan.com/登陆美团联盟，如果没有账号可以点击【免费注册】根据提示注册。
![img](https://img.kancloud.cn/8b/b7/8bb748ac943cb87caab4eccf90865561_1305x656.png)

## 2.完成资质认证

登录后，您需要在“账户管理>基本信息” 页面先进行企业资质认证。填写前请注意：

（1）认证提交后，账户性质无法再进行修改，请您谨慎选择。

（2）认证提交后，认证结果会立即返回，请您及时查看。

（3）在该页面可以直接查看《美团联盟服务合同》内容，如继续使用平台，则默认您已阅读并同意合同相关规定。

（4）美团联盟目前暂不支持个人推广者推广。
![img](https://img.kancloud.cn/59/d7/59d7944bb003489e639c0d0746d063d4_1366x619.png)
![img](https://img.kancloud.cn/78/33/7833b9e3d01d5e4bfde8d987858458e8_1366x624.png)
![img](https://img.kancloud.cn/4c/55/4c55dab679f115ea66cab9351e9beffc_1362x625.png)

## 3.创建媒体和推广位

### 3.1 美团联盟 UID

美团联盟 UID 是您使用的账号在美团联盟平台的标识码，可以登录后在页面右上角查看，“欢迎您”后的数字即为美团联盟 UID
![img](https://img.kancloud.cn/f2/d4/f2d4397246c18a9e35d7cd7203b4f69a_1241x614.png)

### 3.2 创建媒体

首先请先在“推广者备案>媒体管理”页面点击右上角【新增媒体】，填写信息后，点击【保存】或者【保存并新增推广位】。
操作成功后，您可以在媒体列表查看媒体对应的 APPKEY，APPKEY 是渠道标记，用于渠道效果追踪。
![img](https://img.kancloud.cn/ae/bb/aebbb1e88da2a3ce7a3e849fc8811685_1366x615.png)
![img](https://img.kancloud.cn/b5/9e/b59e333ebc6c7df0b92f9e192ae819ba_1362x623.png)
![img](https://img.kancloud.cn/ee/00/ee0088d69b1854fafef87416c0adfcc3_1360x616.png)

### 3.3 创建推广位

接下来，您可以在“推广者备案>推广位管理”页面，点击右上角【新增推广位】，填写信息后，点击【确定】。

操作成功后您可以在推广位列表查看媒体信息对应的 SID。

SID 是用来标记渠道二级，推广位最多只支持 200 个。

特别说明：目前，APPKEY 不具备接口创建能力，SID 可以通过自助取链接口进行赋值创建，但需要注意通过该拼接方式创建的 SID 仅在用户发生实际点击推广链接后生效！
![img](https://img.kancloud.cn/28/ab/28aba4f349f77fb3f9df82ee67ea61b9_1359x612.png)
![img](https://img.kancloud.cn/0b/4e/0b4e45c5c5afe9c25548f5dd6efb8179_1364x606.png)
![img](https://img.kancloud.cn/f4/db/f4db295ef5cf456c86be550f75ea0035_1359x577.png)

## 4. 获取推广活动链接

完成以上几步后，您可以到“我要推广>活动总览”页面查看当前美团联盟支持推广的活动。

对于您想要推广的活动，您可以点击【立即推广】，然后选择已经建好的媒体和推广位，点击【获取链接】，您就可以获取多种形式的推广链接，并且可以转成短链和生成二维码海报。
![img](https://img.kancloud.cn/79/91/7991daac8ba752301acdc9487fc3b81d_1363x615.png)
![img](https://img.kancloud.cn/d5/23/d523d27e84964157e23d6b0cf65ba96f_1366x622.png)
![img](https://img.kancloud.cn/28/d6/28d6d480acc48b068100869836aa35f5_1360x617.png)
![img](https://img.kancloud.cn/28/b6/28b62e9fa2b0e9e26c3645630fbded59_1366x626.png)
![img](https://img.kancloud.cn/2b/61/2b61c925cdaf3af6a254346cb61907f2_1366x617.png)

## 推广活动设置

### 1.设置美团-联盟推广活动（主要涉及到优选、商超及酒店），不可做分销。

![img](https://img.kancloud.cn/98/b6/98b624e7522e1914570c7e3f1288900c_1920x818.png)

### 1.1 获取推广链接说明

![img](https://img.kancloud.cn/60/df/60dfd402e68915d1dd0bcacf496594be_1530x846.png)
![img](https://img.kancloud.cn/10/15/10157c4d0bb38d91c9b64a08cfca5be3_1265x649.png)
![img](https://img.kancloud.cn/8a/c4/8ac401a64a76862d31bb619eec260a8f_943x509.png)

### 1.2 管理后台填写相对应的参数

![img](https://img.kancloud.cn/98/b6/98b624e7522e1914570c7e3f1288900c_1920x818.png)

### 1.设置美团-联盟系统参数

### 1.1 获取美团联盟 Appkey

![img](https://img.kancloud.cn/8c/30/8c303b81f8180d3773a9ee51bdd23aa6_1276x525.png)

### 1.2 获取美团联盟 secret

![img](https://img.kancloud.cn/9a/b5/9ab5d817d7ff3e5dab30d38682220119_1302x722.png)

### 1.3 获取美团联盟 sid

![img](https://img.kancloud.cn/b8/90/b89007c733960ea2fa31b9171f7b8b33_1126x639.png)

### 1.4 填写对应的参数到系统设置参数中

![img](https://img.kancloud.cn/3c/75/3c75b18319fd7e0932550ee15d5a964e_1173x786.png)

# 美团分销联盟

**提示：必须先在美团联盟后台注册申请入驻通过后的企业用户才能对接美团分销平台（美团分销平台不支持个人用户，只支持企业用户）**

## 1 设置美团-分销平台系统参数

![img](https://img.kancloud.cn/77/1f/771ff706b41e3c18f00a7f1b4b2f3463_1166x743.png)

### 1.1 获取 AppKey

![img](https://img.kancloud.cn/e0/f7/e0f74f9370312491a81e0d726061d931_1056x345.png)

### 1.2 获取 utmSource

![img](https://img.kancloud.cn/59/7b/597bdae9440e3984567bcd46c09a07b3_1168x382.png)

### 获取 pid

![img](https://img.kancloud.cn/60/ac/60acd793b873dc46a2ddc3b111afb022_1239x474.png)

## 1.设置美团-联盟推广活动（主要涉及到优选、商超及酒店），不可做分销。

![img](https://img.kancloud.cn/8c/5f/8c5f86a377605c3f3e148803ff3dc6b8_1639x779.png)

### 1.1 获取活动 ID

![img](https://img.kancloud.cn/7d/e5/7de590d8f58d9445b86be9c9a18ec1c1_1282x775.png)
复制活动 ID 写入到后台中。

### 1.2 管理后台填写相对应的参数

![img](https://img.kancloud.cn/8c/5f/8c5f86a377605c3f3e148803ff3dc6b8_1639x779.png)

# 淘宝联盟

## **提示：饿了么、淘宝客平台需要用到淘宝联盟**

## 1 登录淘宝联盟

- 没有联盟账号的话可以用自己淘宝账号，登录淘宝联盟认证成联盟账号
- 访问地址：https://pub.alimama.com/

## 2 登录之后点击进入后台

![img](https://img.kancloud.cn/c7/f3/c7f3499eb3016bab12d84945cae5909f_1704x618.png)

## 3 推广管理

- **3.1 新建媒体备案**
  ![img](https://img.kancloud.cn/e5/c2/e5c25196d7d93d475a0a2b5b4712ef4f_1266x638.png)
- **3.2 选择自由平台，网站应用**
  ![img](https://img.kancloud.cn/ae/5d/ae5d593d7de05bccee761019e857d838_1583x871.png)
- **3.3 根据提示需求填写，提交之后等待淘宝审核，审核完成之后才可以对接**
  ![img](https://img.kancloud.cn/a9/ee/a9ee0c7db939e80656fdeae35bdd645c_1271x927.png)
  提示：等待审核，如果审核失败重新提交审核

​ **提示：必须审核通过之后才能设置系统淘宝参数**

## 1.设置淘宝联盟系统参数

![img](https://img.kancloud.cn/87/1b/871bd0d80c7c99437c738f538790b261_1096x586.png)

### 1.1 获取淘宝联盟 Appkey，AppSecret

![img](https://img.kancloud.cn/fb/a6/fba6f42271d0b36cebbe32ff59dff3ac_1228x607.png)

- 点击查看
  ![img](https://img.kancloud.cn/34/01/340117942216d119ba6de75e220fd660_1068x469.png)
- 复制到后台中填写

### 1.2 获取淘宝联盟 PID

![img](https://img.kancloud.cn/ee/dd/eedd8a0cfa533878a7dbeee9dfc6fe53_1248x441.png)

- 提示：切记不要在联盟后台删除已填写过的 pid

### 1.3 获取 adzone_id

**例子：比如 PID 为：mm_132567491_430350191_111590150191
adzone_id 就为: “111590150191”**

### 1.4 填写相对应的参数到后台系统中

![img](https://img.kancloud.cn/87/1b/871bd0d80c7c99437c738f538790b261_1096x586.png)

### 1.0 获取活动 ID

![img](https://img.kancloud.cn/49/ff/49ffec8e3ac80a55a6a0a5fbb94c6b39_1541x1212.png)

### 2.0 填写后台参数

![img](https://img.kancloud.cn/5b/e4/5be4e0c983c9618cc15aa02cf2279b8c_1282x692.png)

---

**提示：提交好打开小程序测试**

## 3.0 如果小程序出现请求错误，请查看自己的 API 权限包是否都已经申请了

![img](https://img.kancloud.cn/e1/35/e1355d418fa6dadd7b3271c6549b402d_1080x2400.png)
![img](https://img.kancloud.cn/ed/51/ed5193b41777417df2bffd9c5593bdf2_2399x1236.png)

---

**提示：把需要申请的都申请上，否则不能使用哦~**
申请理由可以写：
做外卖 cps 的话，如果没有一个科学的玩法还真的十分容易迷失自己，作为一个普通人我们本来就没有很多的业余时间，所以当然是想要让自己的业余时间得到充分的利用了，因此我们十分需要在做每一件事之前先去好好的认识和了解自己想要去做的这件事该如何去做，以及做这件事的时候改注意一些什么问题。外卖 cps 对于普通人来说，算是一个比较友好的赚钱方式，因为他真的是一分耕耘一分收获的，所以即使你是一个人，也是可以去做外卖 cps 的。

# 京东联盟配置

## 1.注册京东联盟

- 注册地址：https://union.jd.com/index

## 2.获取联盟 ID

- 进入：https://union.jd.com/user
  ![img](https://img.kancloud.cn/03/2c/032cffaa711388efbfbccfb2935deab7_3088x1160.png)

## 3.获取 Appkey&Appsecret

![img](https://img.kancloud.cn/3e/b3/3eb3e55b4ac524e51baa2809639d95fe_1903x706.png)
![img](https://img.kancloud.cn/7d/d6/7dd6d07963aadfbfd6846a3b6576a9b6_1915x839.png)

## 4.获取京东联盟 key

![img](https://img.kancloud.cn/4c/ff/4cff3fa1727ba8fcb66c0fa07d39432f_3746x1478.png)

## 5.填写到系统后台

![img](https://img.kancloud.cn/e5/c5/e5c5e8510ddc2782ec23dc40372d233b_1920x803.png)

# 拼多多

## 1.进入多多进宝官方网站

访问：https://jinbao.pinduoduo.com/

## 2.多多进宝账号和拼多多账号不同，即使有拼多多账号也需要注册多多进宝

![img](https://box.kancloud.cn/c5b2759648ac4568e269bf4f790f8645_3198x1800.png)
![img](https://box.kancloud.cn/7d264dec5f74551b8bd7d45a1edbeb96_3198x1798.png)
多多进宝账号注册完成

## 3.最后完善你的账号信息

![img](https://box.kancloud.cn/d33ae0b3262a6310fb97626c087600ca_2492x958.png)

## 开发平台

## 1.注册账户（新用户）

- 注册地址：https://open.pinduoduo.com/application/app/list

## 2.创建应用

![img](https://img.kancloud.cn/11/b2/11b244cd544426cd95caaec9dc9e9f72_1920x336.png)
![img](https://img.kancloud.cn/c2/63/c263794e218ec3d452c3a1163e85937d_1906x672.png)

## 3.填写资料（回调地址）

回调地址填写：[https://wx-mini.jiaxianghui.cn/api/openapi/pddAuthCallBack](http://wx-mini.jiaxianghui.cn/api/openapi/pddAuthCallBack)

## 4.获取 client 和 secret

- 上面的步骤创建完应用之后，进入应用列表，查看详情（https://open.pinduoduo.com/application/app/list）
  ![img](https://img.kancloud.cn/cb/f1/cbf1b287099572d1e7cbea35121f63f2_1420x586.png)
  ![img](https://img.kancloud.cn/b7/99/b799937e121622a653184cdb9a48c8fb_1902x760.png)
  回调地址填写：[https://wx-mini.jiaxianghui.cn/api/openapi/pddAuthCallBack](http://wx-mini.jiaxianghui.cn/api/openapi/pddAuthCallBack)

## 5.【重要】 绑定接口权限， 否则无法推广

- 访问：https://jinbao.pinduoduo.com/third-party/rank
- 在以下的框内输入上面创建应用的的 ClientID。
  ![img](https://img.kancloud.cn/d3/72/d3722a649b4074a08267cb629b5e08e5_1619x360.png)

## 4 获取 PID

访问：https://jinbao.pinduoduo.com/promoter-audit
![img](https://img.kancloud.cn/bc/76/bc7623480fccbe6ed9f1cc058d2e6f5d_1493x882.png)

## 5 填写到后台

![img](https://img.kancloud.cn/fa/c2/fac2a1904158b42ae7cd2fee82506260_1174x708.png)
必须填写了 Client_Id 和 Client_Secret 才能前往授权获取 Access_Token

# 唯品会

**提示：唯品会走的是工具商模式（申请审批都比较麻烦，所以走工具商模式，方便使用）**

## 1.设置唯品会参数

### 1.1 前往授权

![img](https://img.kancloud.cn/d1/fb/d1fb4f7c75aa2c4660b2a0810e45ce4b_1218x653.png)
![img](https://img.kancloud.cn/4b/ee/4beea70a54f7d2136c589f724ddfe175_975x708.png)
![img](https://img.kancloud.cn/e9/e5/e9e5ea8d018c0dfa9d2e70cc92ba15b9_1105x657.png)

### 1.2 复制粘贴 access_token

![img](https://img.kancloud.cn/d1/b9/d1b90f5664983cfe785170183fc4d278_1786x91.png)

### 1.3 填写到系统后台

![img](https://img.kancloud.cn/d1/fb/d1fb4f7c75aa2c4660b2a0810e45ce4b_1218x653.png)

### 1.4 手机下载“唯享客”登录同意即可，必须下载登录刚注册的账号，否则使用不了。

提示：user has no union auth 出现此提示，请下载唯享客 APP 登录刚才注册的手机号码即可使用。

# 大淘客

**提示：大淘客主要用作于淘宝订单的采集和京东的商品详情和转链（考虑到京东申请商品详情和转链麻烦，所以选择使用大淘客平台）**

### 1.1 注册等待审核

访问：https://www.dataoke.com/

### 1.2 审核通过后

![img](https://img.kancloud.cn/7f/4f/7f4fad7859ba83511185c5524fa26ac2_1912x516.png)

- 进入开放平台，获取 Appkey
  ![img](https://img.kancloud.cn/99/1f/991f2369ec91374eda1f6a23445f6e2e_1805x462.png)

### 1.3 创建应用

![img](https://img.kancloud.cn/5d/ba/5dbaab6c4beb056460c02809f0776e81_1525x494.png)

### 1.4 淘宝京东授权，按照指引操作

![img](https://img.kancloud.cn/f0/d1/f0d17ab0782af6c14b0f6b8d5f08824a_1301x495.png)

### 1.5 把对应的参数填写入后台中

![img](https://img.kancloud.cn/e3/b5/e3b515c8f142b7517062f08b100b5b01_1339x747.png)
![img](https://img.kancloud.cn/5d/ba/5dbaab6c4beb056460c02809f0776e81_1525x494.png)

# 三级分销设置

**提示：本系统只支持到三级分销**

返利说明
其实分销的佣金说明已经介绍得很清楚了
总佣金 100.00，默认设置的返利比例为 30%，分销比例为：20%，15%，10%
则购买者 D 获取的佣金为：100*30%=30 元
他的上级 C 获取的佣金为：100*20%=20 元
C 的上级 B 获取的佣金为：100*15%=15 元
B 的上级 A 获取的佣金为：100*10%=10 元
平台获取的佣金为：100-30-20-15-10=25 元
用户购买最多仅有此 5 个收益方，至于 A 的上级或者上上级则不享受收益。
比例请自行设置，由于是按照总佣金进行分成，所以最终计算盈亏会很直观。
![img](https://img.kancloud.cn/92/ef/92ef870e93ad0d0573422da35e6b13d3_1047x661.png)

# 充值话费链接

# 话费链接获取

- 因为话费充值需要资质，所以选择了拼多多的跳转，这样我们本身也不需要操心更多
- 访问链接：https://jinbao.pinduoduo.com/
  ![img](https://img.kancloud.cn/03/3e/033ec32e1e0582663958e828c490a782_3246x676.png)
  ![img](https://img.kancloud.cn/b1/1d/b11d05a04c554702d50392d9efb753da_3170x1214.png)

---

# 跳转找不到页面和参数错误的，请看下面

**\*\*拼多多的 Appid 为：wx32540bd863b27570\*\***
**提示：内容参数为：/pages/web/web?specialUrl=1&src=充值链接**

# 火车票链接

## 火车票链接获取

- 访问链接：https://jinbao.pinduoduo.com/
  ![img](https://img.kancloud.cn/03/3e/033ec32e1e0582663958e828c490a782_3246x676.png)
  ![img](https://img.kancloud.cn/88/09/88097f2c2cfcff364f10cefa282a6126_2946x1592.png)

# 小程序搭建上线

## 1.0 下载小程序开发工具（已安装的忽略）

访问下载：https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html

## 2.0 下载小程序源码，解压并导入编辑器

源码在公众号发送 源码 ， 即可获取下载链接

![img](https://img.kancloud.cn/91/b4/91b4fdd0b9d5e6fbc3678793d592078a_995x748.png)

## 3.0 全局搜索并替换成自己的配置信息

![img](https://img.kancloud.cn/71/8d/718db97d26395d4315be28718d099462_810x777.png)
![img](https://img.kancloud.cn/93/8f/938f9dbf74b59ed22196bbd52f12e5eb_1240x369.png)
![img](https://img.kancloud.cn/e0/b3/e0b361c908356ddff97e53438ff30b2f_2548x836.png)

## 4.0 微信小程序后台开发设置完成之后，上传到微信小程序后台

访问地址：https://mp.weixin.qq.com/
![img](https://img.kancloud.cn/cf/08/cf083c9d2bdd6bcab9ff96e40e3323c1_2495x994.png)
**提示：可以扫码体验看看能不能出来数据，能出来的话就说明后台开发设置都设置好了。没有的请检查域名配置**

## 5.0 检查完毕提交审核

![img](https://img.kancloud.cn/8b/26/8b2625aca641f37846ad19a7acd9f7ff_2284x1164.png)

# 素材地址

## 1.0 美团

| 活动名称 | 图片链接                                                            |
| :------- | :------------------------------------------------------------------ |
| 外卖     | http://img2.hzhstb.com/16c53536106c9324f619f018ff9782ee_800x750.png |
| 优选     | http://img2.hzhstb.com/e9bfa17c087f75299e29bc21a6f40d8f_800x750.png |
| 商超     | http://img1.hzhstb.com/915b22a9508a7c360c872f3283d477f9_800x750.png |
| 酒店     | http://img1.hzhstb.com/4101eb97b881a08229bc5a30524bf9ba_800x750.png |

## 2.0 饿了么

| 活动名称   | 图片链接                                                               |
| :--------- | :--------------------------------------------------------------------- |
| 外卖       | http://img3.hzhstb.com/fdebfe5ea01fe6149f5b64bf93d4bf9b_1080x1125.png  |
| 饿了么果蔬 | https://iconfont.alicdn.com/t/2be1151b-d2ce-4ff9-a064-92dd197822a8.png |
| 下午茶     | https://iconfont.alicdn.com/t/8cc03132-b8f5-4448-99a9-1699a38b3f0f.png |

## 3.0 轮播管理

| 活动名称 | 图片链接                                                                 |
| :------- | :----------------------------------------------------------------------- |
| 吃喝玩乐 | https://img.yunzhanxinxi.com/static/home/banner/panda/chwl_banner.png    |
| 美团外卖 | https://img.yunzhanxinxi.com/static/mini/0.0.1/营销版/mtwm_banner@3x.png |
| 饿了么   | https://img.yunzhanxinxi.com/static/home/banner/panda/elm_banner.png     |
| 吃喝玩乐 | https://img.yunzhanxinxi.com/static/home/banner/panda/chwl_banner.png    |

**提示：把图片链接复制进去即可**

# 微信小程序，公众号点餐提醒

**提示：每天提醒的时间为上午 10：40 分，下午 6：10 分**

---

**1.0 小程序提醒配置**

## **1.1 小程序行业选择**

访问：https://mp.weixin.qq.com/wxamp/basicprofile/index?token=1585923914&lang=zh_CN

- 工具>报价/比价
- 餐饮>菜谱
- 生活服务>百货/超市/便利店

---

![img](https://img.kancloud.cn/ae/cc/aecc22687dd7260bbf464c0c25957d08_2422x204.png)

## **1.2** 选择订阅模板消息

![img](https://img.kancloud.cn/83/d6/83d6918e0981bcbf830439804e07456b_2242x972.png)
![img](https://img.kancloud.cn/03/b6/03b6bf45d7d092da5b8ed149c077d9c5_2087x901.png)

---

**提示：必须选择一样的模板操作，否则无效**

## 1.3 填写到后台中

![img](https://img.kancloud.cn/b4/94/b4941710d954515edf14fa9d7c2cbf1c_2555x1005.png)
![img](https://img.kancloud.cn/bd/32/bd32ff7e2f0e088d515f2afaad19ea88_1641x1087.png)

---

# **2.0 微信公众号提醒配置**

## 2.1 **公众号开发设置**

暂无
