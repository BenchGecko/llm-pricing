# LLM API Pricing

**Up-to-date pricing for 300+ LLM APIs in a single JSON file.**

Powered by [BenchGecko](https://benchgecko.ai) -- The Data Layer of the AI Economy.

![Last Updated](https://img.shields.io/badge/last_updated-2026-04-27brightgreen)
![Models](https://img.shields.io/badge/models-360-blue)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Why This Exists

Every LLM provider has different pricing pages, different formats, different units. This repo gives you **one canonical JSON file** with standardized pricing for every major LLM API, updated weekly from the [OpenRouter](https://openrouter.ai) aggregator.

Use it to build cost calculators, comparison tools, billing dashboards, or just to answer "how much does GPT-4o cost again?"

For full benchmark scores, performance comparisons, and provider analytics, see [benchgecko.ai](https://benchgecko.ai).

---

## Quick Start

### Fetch the latest pricing

```bash
# Raw JSON (346 models, ~200KB)
curl -sL https://raw.githubusercontent.com/BenchGecko/llm-pricing/main/pricing.json | python3 -m json.tool | head -50

# Grouped by provider
curl -sL https://raw.githubusercontent.com/BenchGecko/llm-pricing/main/pricing-by-provider.json
```

### JavaScript / TypeScript

```js
const res = await fetch(
  'https://raw.githubusercontent.com/BenchGecko/llm-pricing/main/pricing.json'
);
const { models } = await res.json();

// Find cheapest model with 100K+ context
const cheap = models
  .filter(m => !m.is_free && m.context_window >= 100000)
  .sort((a, b) => a.input_per_million - b.input_per_million);

console.log(cheap[0]);
// { id: "...", name: "...", input_per_million: 0.08, ... }
```

### Python

```python
import requests

data = requests.get(
    "https://raw.githubusercontent.com/BenchGecko/llm-pricing/main/pricing.json"
).json()

# Calculate cost for 1M input + 100K output tokens
for m in data["models"]:
    if "gpt-4" in m["id"]:
        cost = m["input_per_million"] + (m["output_per_million"] * 0.1)
        print(f"{m['name']}: ${cost:.2f}")
```

### Install via npm

```bash
npm install @benchgecko/llm-pricing
```

```js
const pricing = require('@benchgecko/llm-pricing');
console.log(pricing.models.length); // 346
console.log(pricing.models.find(m => m.id === 'openai/gpt-4o'));
```

> Note: For always-fresh data, use the raw GitHub URL. The npm package is updated weekly.

---

## Top Models by Price

| Model | Provider | Input ($/1M tokens) | Output ($/1M tokens) | Context |
|-------|----------|--------------------:|---------------------:|--------:|
| Claude Opus 4.6 | Anthropic | $5.00 | $25.00 | 1000K |
| Claude Sonnet 4.6 | Anthropic | $3.00 | $15.00 | 1000K |
| Claude Sonnet 4.5 | Anthropic | $3.00 | $15.00 | 1000K |
| Claude Haiku 4.5 | Anthropic | $1.00 | $5.00 | 200K |
| DeepSeek V3 0324 | DeepSeek | $0.20 | $0.77 | 164K |
| R1 | DeepSeek | $0.70 | $2.50 | 64K |
| Gemini 2.5 Pro | Google DeepMind | $1.25 | $10.00 | 1049K |
| Gemini 2.5 Flash | Google DeepMind | $0.30 | $2.50 | 1049K |
| Gemini 2.0 Flash | Google DeepMind | $0.10 | $0.40 | 1049K |
| Llama 4 Maverick | Meta | $0.15 | $0.60 | 1049K |
| Llama 4 Scout | Meta | $0.08 | $0.30 | 328K |
| Mistral Large 2411 | Mistral AI | $2.00 | $6.00 | 131K |
| GPT-4.1 | OpenAI | $2.00 | $8.00 | 1048K |
| GPT-4.1 Mini | OpenAI | $0.40 | $1.60 | 1048K |
| GPT-4.1 Nano | OpenAI | $0.10 | $0.40 | 1048K |
| GPT-4o | OpenAI | $2.50 | $10.00 | 128K |
| GPT-4o-mini | OpenAI | $0.15 | $0.60 | 128K |
| o3 | OpenAI | $2.00 | $8.00 | 200K |
| o3 Mini | OpenAI | $1.10 | $4.40 | 200K |
| o4 Mini | OpenAI | $1.10 | $4.40 | 200K |

> Full pricing for all 346 models available in [`pricing.json`](pricing.json).

**Want to compare performance too?** See how these models score on 40+ benchmarks at [benchgecko.ai/compare](https://benchgecko.ai/compare)

---

## Data Schema

Each model in `pricing.json` has this structure:

```json
{
  "id": "openai/gpt-4o",
  "name": "GPT-4o",
  "provider": "OpenAI",
  "input_per_million": 2.5,
  "output_per_million": 10.0,
  "context_window": 128000,
  "max_output": 16384,
  "is_free": false,
  "modalities": ["file", "image", "text"],
  "type": "chat"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | OpenRouter model identifier (e.g., `openai/gpt-4o`) |
| `name` | string | Human-readable model name |
| `provider` | string | Company/org that created the model |
| `input_per_million` | number | Cost in USD per 1 million input tokens |
| `output_per_million` | number | Cost in USD per 1 million output tokens |
| `context_window` | number \| null | Maximum context length in tokens |
| `max_output` | number \| null | Maximum output/completion tokens |
| `is_free` | boolean | Whether the model is free to use |
| `modalities` | string[] | Supported modalities (text, image, audio, video, file) |
| `type` | string | Model type: `chat`, `image`, `embedding`, `speech` |

### Files

| File | Description |
|------|-------------|
| [`pricing.json`](pricing.json) | Flat list of all models, sorted by provider then name |
| [`pricing-by-provider.json`](pricing-by-provider.json) | Same data grouped by provider name |

---

## Related Resources

- [BenchGecko](https://benchgecko.ai) — Full model rankings, benchmark scores, and provider analytics
- [BenchGecko Compare](https://benchgecko.ai/compare) — Side-by-side model comparison with pricing + performance
- [BenchGecko API](https://benchgecko.ai/api-docs) — Free REST API for model data, benchmarks, and pricing
- [Awesome LLM Benchmarks](https://github.com/BenchGecko/awesome-llm-benchmarks) — Curated list of 60+ AI benchmarks

---

## Data Source

Pricing data is sourced from the [OpenRouter API](https://openrouter.ai/api/v1/models) and updated weekly. OpenRouter aggregates pricing from all major LLM providers into a single API.

Prices reflect the OpenRouter pass-through rates, which match or closely track the official provider pricing.

For full benchmark comparisons, performance scores, and provider analytics, visit **[benchgecko.ai](https://benchgecko.ai)**.

---

## License

MIT -- use this data however you want. Attribution appreciated but not required.

If you build something cool with this data, let us know at [benchgecko.ai](https://benchgecko.ai) or [@BenchGecko](https://twitter.com/BenchGecko) on Twitter.
