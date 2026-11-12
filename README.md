# aicentos — Enterprise LLM API Gateway

One API key. 94 models from 13 providers — OpenAI, Anthropic, Google, xAI, DeepSeek and more — behind a single OpenAI-, Anthropic- and Gemini-compatible gateway.

- **Self-built official account pool** with multi-account failover: if one upstream account fails, requests automatically retry on the next one.
- **No prompt bodies in logs** — usage logs record metadata only (model, tokens, latency).
- **Per-key controls**: spending caps, model allowlists, IP allowlists, expiry, instant revoke.
- **Pay as you go**, balance never expires. Enterprise contracts, invoicing and self-hosted deployment available.

📖 Full docs: **https://docs.aicentos.com** · Status: **https://status.aicentos.com**

## Quickstart — 3 steps

1. **Sign up** at [aicentos.com](https://aicentos.com) — new accounts get ¥1 free credit, no top-up needed to try.
2. **Create an API key** in the console under **API Keys**. Keys look like `sk-xxxxxxxxxxxx` and are shown in full only once at creation — copy it immediately.
3. **Point your client at the Base URL** and use the key. No SDK changes needed.

### Base URL — this is the step people get wrong

| Scenario | Base URL |
| --- | --- |
| Codex / OpenAI-compatible format | `https://aicentos.com/v1` |
| Claude Code / Anthropic format, and other clients | `https://aicentos.com` |

> For Anthropic, use the root address only — do **not** append `/v1/messages`; the client adds the path itself.

### Verify with curl

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

Or list the models — any JSON response means your key and Base URL work:

```bash
curl https://aicentos.com/v1/models \
  -H "Authorization: Bearer $AICENTOS_API_KEY"
```

### Python (OpenAI SDK)

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["AICENTOS_API_KEY"],
    base_url="https://aicentos.com/v1",
)

resp = client.chat.completions.create(
    model="claude-sonnet-5",
    messages=[{"role": "user", "content": "Introduce yourself in one sentence."}],
)
print(resp.choices[0].message.content)
```

## Client guides — 69 and counting

Step-by-step setup for Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Cline, Roo Code, Cherry Studio and 60+ more:

👉 **https://docs.aicentos.com/clients**

## Links

- Docs: https://docs.aicentos.com
- Quickstart: https://docs.aicentos.com/quickstart
- Model list & pricing: https://docs.aicentos.com/models
- FAQ: https://docs.aicentos.com/faq
- Live status: https://status.aicentos.com
- Console: https://aicentos.com

## Security notes

- Never hard-code a key into frontend code or commit it to a public repo.
- Keys support expiry, quotas, model and IP allowlists — set limits per project so a leaked key can't burn your balance.
