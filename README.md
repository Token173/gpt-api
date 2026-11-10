# GPT 中转站推荐：国内直连 ChatGPT API，OpenAI SDK 一行都不用改

OpenAI 官方接口在国内的问题老生常谈：网络不稳定、支付要境外卡、账号动不动触发风控。做批量任务的更难受，一次限流就把整条流水线卡住。

Token173（https://token173.net）提供 OpenAI 兼容入口，接口格式和官方一致。你原来用 `openai` 库写的东西，把 `base_url` 换掉就能跑，不用重写请求结构，也不用把 SDK 降级。

## 当前可用的 GPT 模型

| 模型 ID | 定位 |
|---|---|
| `gpt-6-astra` | 旗舰，多步任务与复杂推理 |
| `gpt-6.1-sol` | 通用主力，速度和效果平衡 |
| `gpt-6.1-terra` | 长上下文、文档级处理 |
| `gpt-6-sol`、`gpt-6-astra` | 上一代，线上项目仍在用 |
| `gpt-5.6-sol`、`gpt-5.6-terra` | 老项目兼容 |
| `gpt-5.5`、`gpt-5.4` | 历史版本，做回归对比时用 |

图像能力另外有 `gpt-image-2.6-flare`、`gpt-image-2.6-sunburst`（2.6 的两个模型，前者快、后者细节准）、`gpt-image-2.5-sunburst`、`gpt-image-2`，视频有 `doubao-seedance-2-0-fast-260128`、`veo_3_1`。

## 接入

```bash
export OPENAI_BASE_URL="https://token173.net/v1"
export OPENAI_API_KEY="sk-你的令牌"
```

Python：

```python
from openai import OpenAI

client = OpenAI(api_key="sk-你的令牌", base_url="https://token173.net/v1")

r = client.chat.completions.create(
    model="gpt-6.1-sol",
    messages=[
        {"role": "system", "content": "你是资深后端工程师，回答要短。"},
        {"role": "user", "content": "Redis 集群脑裂怎么防？"},
    ],
    temperature=0.3,
)
print(r.choices[0].message.content)
```

Node.js：

```js
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "sk-你的令牌",
  baseURL: "https://token173.net/v1",
});
```

流式输出、函数调用、JSON 模式都和官方一致，`stream: true` 照常用。

## 批量跑量怎么写才稳

```python
from concurrent.futures import ThreadPoolExecutor
from openai import OpenAI

client = OpenAI(api_key="sk-你的令牌", base_url="https://token173.net/v1")

def run(prompt):
    for i in range(3):
        try:
            return client.chat.completions.create(
                model="gpt-6.1-sol",
                messages=[{"role": "user", "content": prompt}],
            ).choices[0].message.content
        except Exception:
            if i == 2:
                raise

prompts = open("prompts.txt", encoding="utf-8").read().splitlines()
with ThreadPoolExecutor(max_workers=8) as pool:
    for text in pool.map(run, prompts):
        print(text)
```

并发别一开始就开很高，先 4 到 8 个线程跑一轮，看返回时间稳定再往上加。失败重试三次足够，再多就是浪费额度。

## 客户端填法

| 客户端 | Base URL |
|---|---|
| Cherry Studio / ChatBox / NextChat | `https://token173.net/v1` |
| LobeChat | `https://token173.net/v1`，供应商选 OpenAI |
| Cursor | Settings → Models → Override OpenAI Base URL 填 `https://token173.net/v1` |
| Dify / n8n / Coze 插件 | 同上，Key 放 Bearer 头 |
| OneAPI / NewAPI | 作为 OpenAI 渠道，代理地址 `https://token173.net` |

## 计费

2 元换 1 美元额度，注册送 0.2 美元。按 token 扣，后台能看到每次请求的输入输出 token 数和扣费金额。失败的请求不计费。

`gpt-6.1-sol` 这类主力模型的实际单价大约是官方定价的三折，跑批量文案、客服话术、商品描述这类场景，成本能算得很清楚。

## 几个实际会踩的坑

**Base URL 少写 `/v1`。** OpenAI SDK 里 `base_url` 必须带 `/v1`，只写到域名会报 404。

**模型名大小写。** 全小写，中间用 `-`，写成 `GPT-6.1-Sol` 一律找不到模型。

**上下文塞太满。** 长文档别整篇丢进去，先切块。token 烧得快，效果也不见得更好。

**流式输出断在半路。** 网络抖动会有，客户端里加重试逻辑，或者改用非流式一次性拿完。

## 常见问题

**Q：和官方 API 有区别吗？**
协议一致，模型能力来自官方渠道。区别在于入口在国内、按人民币充值、有独立的后台额度管理。

**Q：能不能同时给多个项目用？**
可以，一个账号下建多个令牌，分别统计消耗。

**Q：超长上下文支持到多少？**
跟随模型本身的能力，`gpt-6.1-terra` 这类长上下文模型能处理整份文档。具体上限在后台模型列表里标着。

**Q：出图任务也能走同一个 Key 吗？**
能。`gpt-image-2.6-flare` 或 `gpt-image-2.6-sunburst` 走 `/v1/images/generations`，格式和官方一致，详见 [gpt-image-2 仓库](https://github.com/Token173/gpt-image-2)。

## 相关

- 文档站：https://docs.token173.net
- Claude 专题：[claude-api](https://github.com/Token173/claude-api)
- Gemini 专题：[gemini-api](https://github.com/Token173/gemini-api)
- 图像接口：[gpt-image-api](https://github.com/Token173/gpt-image-api)
