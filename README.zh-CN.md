# aicentos — 企业级大模型 API 接入网关

一个 API Key，调用 94 个模型、13 家厂商——OpenAI、Anthropic、Google、xAI、DeepSeek 等，全部走一个兼容 OpenAI、Anthropic、Gemini 协议的网关。

- **自建官方账号池**，多账号兜底：某个上游账号失败时，请求自动重试到同一模型的下一个账号。
- **正文不落日志**：用量日志只记模型、token 数、耗时等元数据，不存请求与响应正文。
- **密钥多维管控**：额度上限、模型白名单、IP 白名单、有效期，随时吊销重发。
- **按量计费，余额永不过期**。支持对公合同、开票与私有化部署。

📖 完整文档：**https://docs.aicentos.com** · 状态监控：**https://status.aicentos.com**

## 快速开始——三步

1. **注册**：打开 [aicentos.com](https://aicentos.com)，新账号送 ¥1 体验额度，不充值也能先试。
2. **创建 API 密钥**：控制台左侧「API 密钥」→「创建 API 密钥」。密钥形如 `sk-xxxxxxxxxxxx`，只在创建时完整显示一次，请立即复制保存。
3. **填上 Base URL 和 Key**：SDK 和客户端不用换，改个地址就能用。

### Base URL——最容易出错的一步

| 使用场景 | Base URL |
| --- | --- |
| Codex / OpenAI 兼容格式 | `https://aicentos.com/v1` |
| Claude Code / Anthropic 格式，以及其它客户端 | `https://aicentos.com` |

> Anthropic 协议只填根地址，**不要手动追加 `/v1/messages`**，客户端会自动拼接。

### curl 验证

```bash
export AICENTOS_API_KEY="sk-xxxxxxxxxxxx"

curl https://aicentos.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AICENTOS_API_KEY" \
  -d '{
    "model": "claude-sonnet-5",
    "messages": [
      { "role": "user", "content": "Hello, aicentos!" }
    ]
  }'
```

也可以先请求模型列表，只要返回 JSON，就说明密钥和 Base URL 都可用：

```bash
curl https://aicentos.com/v1/models \
  -H "Authorization: Bearer $AICENTOS_API_KEY"
```

### Python（OpenAI SDK）

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["AICENTOS_API_KEY"],
    base_url="https://aicentos.com/v1",
)

resp = client.chat.completions.create(
    model="claude-sonnet-5",
    messages=[{"role": "user", "content": "用一句话介绍你自己"}],
)
print(resp.choices[0].message.content)
```

## 客户端教程——69 款

Claude Code、Codex、Cursor、GitHub Copilot、Gemini CLI、Cline、Roo Code、Cherry Studio 等 69 款 AI 编程工具与聊天客户端的图文配置教程：

👉 **https://docs.aicentos.com/clients**

## 链接

- 文档：https://docs.aicentos.com
- 快速开始：https://docs.aicentos.com/quickstart
- 模型列表与价格：https://docs.aicentos.com/models
- 常见问题：https://docs.aicentos.com/faq
- 实时状态：https://status.aicentos.com
- 控制台：https://aicentos.com

## 安全提醒

- 不要把密钥硬编码到前端代码，也不要提交到公开仓库。
- 每个密钥都可以单独设额度、模型白名单、IP 白名单和有效期，按项目分开创建，泄露了也能单独吊销。
