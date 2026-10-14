# Riachuelo Com Brazil Scraper

Extract structured data from [riachuelo.com.br](https://riachuelo.com.br) — riachuelo.com.br — every product from a keyword search or category page of Brazil's Riachuelo fashion store. Get current and original prices with discounts, sizes with stock levels and colors, plus fabric and care details as clean JSON.

**[Riachuelo Com Brazil Scraper on Apify →](https://apify.com/blackfalcondata/riachuelo-com-br-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products. Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from riachuelo.com.br on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from riachuelo.com.br.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "camiseta",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Product search keywords in Portuguese, e.g. "camiseta", "vestido floral", "tenis masculino". Required if no startUrls provided. |
| `maxResults` | integer | `100` | Maximum total products (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw search or category URLs from riachuelo.com.br (e.g. https://www.riachuelo.com.br/feminino?q=vestido). Each URL becomes its own search task; results are merged and deduped by product URL across all URLs. When provided, startUrls REPLACE the search term (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch each product page for the description, material/composition, care instructions and the full catalogue attribute block. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state — returns only products that are new, or whose price, promo badge or size availability changed. stateKey is optional — defaults to a value derived from your search inputs (search terms, startUrls) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also return products that haven't changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental, also return products that are no longer listed. |
| `skipReposts` | boolean | `false` | When incremental, skip products whose content matches a product that disappeared in a prior run (same style relisted under a new URL). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED products. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of products included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every product returns the same 45-field schema. Missing values are `null` — never omitted.

- `description`
- `gender`
- `ageGroup`
- `category`
- `subCategory`
- `collection`
- `material`
- `careInstructions`
- `manufacturingCountry`
- `breadcrumb`
- `attributes`
- `soldOut`
- `productId`
- `variantGroup`
- `genericCode`
- `title`
- `brand`
- `url`
- `price`
- `originalPrice`
- `discountPercent`
- `currency`
- `isOnSale`
- `promoLabel`
- `imageUrl`
- `images`
- `colorsAvailable`
- `sizes`
- `variants`
- `inStock`
- `stockUnits`
- `sellerId`
- `sellerName`
- `isMarketplace`
- `categories`
- `searchQuery`
- `position`
- `scrapedAt`
- `listingId`
- `portalUrl`
- `contentHash`
- `contentQuality`
- `detailFetched`
- `source`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "description": "A Camiseta feminina oversized silk estonado - cinza é uma peça que traduz personalidade com elegância contemporânea, ideal para a mulher que acompanha tendências com autenticidade.…",
  "gender": "Feminino",
  "ageGroup": "Adulto",
  "category": "Coleção Feminina",
  "subCategory": "Camiseta",
  "collection": "Inverno",
  "material": "Algodão 100%",
  "careInstructions": "Lavar Em Temperatura Máxima De 30°; Não Usar Alvejante À Base De Cloro; Proibido Usar Secadora; Passar Em Temperatura Máxima De 110°; Não Limpar A Seco",
  "manufacturingCountry": "BR - Brasil",
  "breadcrumb": [
    "Feminino",
    "Coleção feminina",
    "Camiseta Feminina"
  ],
  "attributes": {
    "Instrução de Lavagem": "Lavar Em Temperatura Máxima De 30°; Não Usar Alvejante À Base De Cloro; Proibido Usar Secadora; Passar Em Temperatura Máxima De 110°; Não Limpar A Seco",
    "Gênero": "Feminino",
    "Sub Categoria": "Camiseta",
    "Padronagem": "Lisa / Silk",
    "Embalagem para presente": "true",
    "Categoria": "FEMININO",
    "Inverno": "Nao",
    "Composição Principal": "Algodão 100%",
    "Eixo": "Rom Gotico",
    "Divisão": "Top",
    "DCO": "220",
    "Medidas do Produto": "Peso: 0,500 Qui\n<br>",
    "Ocasião": "Casual,passeio Ao Ar Livre",
    "Descrição do Departamento": "Fem. Fast Response",
    "Compre junto": "16393872001;16314794001;15971350001;16397282001",
    "Estação": "Verao 26/27",
    "Modelo": "Oversized",
    "Código Genérico": "16481607",
    "Tema": "Fr013ff",
    "Palavra Chave": "Camiseta Estampada, Punk Barroco",
    "Department": "Feminino",
    "Marca": "Riachuelo",
    "Classificação do Grupo": "Top",
    "Altura": "2.0000",
    "Modelagem": "Camiseta",
    "Produto": "T-shirt",
    "Gola": "Careca",
    "GM": "220000",
    "Faixa Etária": "Adulto",
    "Comprimento": "37.0000",
    "Peso": "220.0g",
    "Coleção": "Inverno",
    "Idade do produto": "125 days",
    "Cor": "Cinza",
    "Estilo de Vida": "Casual",
    "Departamento": "Feminino",
    "Geek": "F",
    "Manga": "Curta",
    "Forro": "Sem Forro",
    "Descrição da Marca": "Riachuelo",
    "Largura": "58.0000",
    "Data de Atualização - IA": "2026-06-10T13:36:33",
    "Família": "PAREDE",
    "País de Fabricação": "BR - Brasil"
  },
  "soldOut": false
}
```

*Truncated — full records contain 45 fields. See Output fields for the complete schema.*

**[Try Riachuelo Com Brazil Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/riachuelo-com-br-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.003 |

See the [actor on Apify](https://apify.com/blackfalcondata/riachuelo-com-br-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape riachuelo.com.br?**
Use this actor on Apify to extract structured data from riachuelo.com.br. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get riachuelo.com.br data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape riachuelo.com.br?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check riachuelo.com.br's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Riachuelo Com Brazil Scraper](https://apify.com/blackfalcondata/riachuelo-com-br-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
