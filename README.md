# 2026版 高效的Open AI、Midjourney API代理、Claude代理、Suno代理等供应商

Token173聚合中转API（token173.net）是一个高效的Open AI、Midjourney API代理、Claude代理、Suno代理等供应商
我们致力于提供优质的 API 接入服务，让您可以轻松集成先进的AI模型至您的产品和服务。通过 API 综合管理平台，无缝整合OpenAl最尖端的人工智能模型。借助我们可靠且易于使用的API解决方案，升级您的产品与服务。

[[点击查看](https://token173.net)]

让您能够轻松一站式接入各种 AI 服务

价格优惠，仅需 2 元即可购买1美刀额度

注册即送0.2刀，不限时间，按量计费

明细可查，每一笔消耗都公开透明

支持 OpenAI 官方所有模型，以及其他市面上常见模型！

注意！我们的价格是 2 = 1 刀额度！！

只需设置模型名，即可开始使用模型！

模型名称即下面的模型列中的内容

除3.5、4.0模型以外，其他模型价格与官方完全一致(注意我们是2元人民币买1刀)

# 这里是使用教程

### 新用户注册赠送 0.2 刀额度，前往令牌页复制令牌，在聊天页设置中填入即可体验！

### 目录

```
· 分组区别
· 网站使用教程
  · GPT接入教程
  · Midjourney 接入教程
· 常见项目配置方式
· 常见问题
```

<br/>

## 分组区别

## Token173聚合中转API（token173.net）网站使用教程

🔥 均为高速回复渠道非低价普通渠道

💰 为方便计费，一个账号通用所有模型，兑换比例固定为 2 元兑换 1美金
其中GPT-4全模型（含32K、Dalle3）后台为官方计费倍率 2 倍，相当于4元兑换1美金（约等于官方的2.4折）

后台日志计费规则解析：假如提示是2000，补全是1000，使用的gpt-4，则您后台的扣费公式=(2000/1000)x0.03（提问价格）+(1000/1000)x0.06（回答价格）=$0.12

### GPT接入教程

1. 注册账号，免费体验，注册即送0.2美金额度，1刀起充
2. 前往[令牌页](/token)，添加令牌
3. 修改应用 BASE_URL为中转接口调用地址： https://token173.net  设置 API Key 为添加的令牌

不同的客户端需要填写不同的BASE_URL, 请尝试如下地址
https://token173.net
https://token173.net/v1
https://token173.net/v1/chat/completions

模型名在[首页](/) -> 支持模型中的第一列 模型 中
可在[聊天页](/chat) 进行测试或使用

<br/>

### Midjourney 接入教程

Midiourney-Proxy主机：https://token173.net

Midiourney-Proxy Secret ：自己后台生成的令牌

同时支持 Mid journey proxy Plus 以及 Mid journey proxy 接口协议

上面为MJ Proxy快捷接入方式，如果你的项目不支持以上方式，请点击查看[接入文档](https://gpt-best.apifox.cn/doc-3530863)

以下参数均可在令牌设置中进行设置
切换 MJ mode:

1. 通过 URL 路径切换
默认 /mj 是 fast mode
/mj-fast/mj 是 fast mode
/mj-turbo/mj 是 turbo mode
/mj-relax/mj 是 relax mode
例如： https://token173.net/mj-turbo/mj/submit/imagine
2. prompt 中通过 mj 参数指定： --relax\--fast\--turbo

切换 MJ 返回的图片地址：
1. 通过 URL 路径切换
默认 /mj 是管理员设置默认方式
/mj-{mode}-relay/mj 是使用服务转发地址，图片国内访问较快
/mj-{mode}-origin/mj 是使用discord 原地址，图片国外访问很快
/mj-{mode}-proxy/mj 是使用管理员设置的代理地址，图片国内访问较快
例如： https://token173.net/

**价格表**

|Mode 模式|类型1端点(根据mode调整价格)|类型2端点 (固定价格)|类型3端点(免费)|
|--|--|--|--|
|Relax|0.12/次|0.07/次|0/次|
|Fast|0.2/次|0.07/次|0/次|
|Turbo|0.4/次|0.07/次|0/次|

<br/>

类型1端点：

1. 类型 1 端点包括：Imagine/Variation/Outpaint/Pan/Blend/Inpaint/Upscale 2x/Upscale 4x/PicReader/Reroll
   1. ```
      中文释义：绘图/变换/变焦/焦点移动/混图/局部重绘/放大2倍/放大4倍/图生文后生成图片/重新生成
      ```
2. 类型 2 端点包括：Upscale/Describe/PicReaderRetry/FaceSwap/Shorten
   1. ```
      中文释义：放大/图生文/图生文重新生成/换脸/prompt分析
      ```
3. 类型 3 端点包括：Fetch/ListByIDs/Modal/Seed
   1. ```
      中文释义：获取单个任务进度/批量获取任务/确认提交弹窗任务/获取Seed
      ```

<br/>

## 常见项目配置方式

### **python openai官方库（使用AutoGPT，langchain等）**

示例代码请参考[demo.py](https://token173.net/demo.py)

***方法一***

```python
import openai
openai.api_base = "https://token173.net/v1"
```

***方法二（方法一不起作用用这个）***

修改环境变量OPENAI_API_BASE，各个系统怎么改环境变量请自行搜索，修改环境变量后不起作用请重启系统。

```sh
OPENAI_API_BASE=https://token173.net/v1
```

### **开源gpt_academic**

找到`config.py`文件中的`API_URL_REDIRECT`配置并修改为以下内容：

```python
API_URL_REDIRECT = {"https://api.openai.com/v1/chat/completions": "https://token173.net/v1/chat/completions"}
```

### **ChatBox(推荐使用)**

ChatGPT开源桌面应用，支持全部桌面平台。

下载链接：https://github.com/Bin-Huang/chatbox/releases

使用方法：如图在设置中填入购买的密钥，并将代理设置为`https://token173.net`即可
