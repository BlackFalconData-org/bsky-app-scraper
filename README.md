# Bluesky Scraper - Posts, Search and Profile Feeds

Extract structured data from [bsky.app](https://bsky.app) — Bluesky posts by keyword search, @handle or profile URL. Each post includes its full text and author profile with like, repost and reply counts plus hashtags, links and media. Filter results by post language for clean exports.

**[Bluesky Scraper - Posts, Search and Profile Feeds on Apify →](https://apify.com/blackfalcondata/bsky-app-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword. Filter by 🧭 profile feed filter, 📝 description format, and more.

**Incremental mode** — Only get new or changed posts since your last run. Content hash per post — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how posts evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N posts (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every post returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured posts from bsky.app on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor posts, track trends, and analyze market dynamics with structured, deduplicated data from bsky.app.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per post is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "bluesky",
  "maxResults": 100,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | What to search for. Bluesky search syntax works (e.g. "climate from:nasa.gov", "#openai"). Paste a @handle, a DID or a bsky.app profile URL instead to pull that account's own posts. Required if no startUrls provided. |
| `language` | string | — | Optional ISO 639-1 code (e.g. "en", "da", "ja") — returns only posts written in that language. Bluesky has no location filter. |
| `maxResults` | integer | `100` | Maximum total posts (0 = unlimited). |
| `startUrls` | array | `[]` | Paste bsky.app search, profile, hashtag, post, custom-feed or list URLs. Each URL becomes its own task; results are merged and deduped across all URLs. A post URL (/profile/<handle>/post/<id>) delivers that post plus its replies (turn replies off with includeReplies); a feed URL (/profile/<handle>/feed/<id>) and a list URL (/profile/<handle>/lists/<id>) deliver that feed or list. When provided, startUrls REPLACE the search term and language fields (other URLs are skipped, and if none is supported the run stops with an input error). |
| `descriptionMaxLength` | integer | `0` | Truncate each post's text to N characters. 0 = full text. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from your search inputs (search terms, language, startUrls) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip posts whose text matches a post that disappeared in a prior run (cross-run duplicate detection). |
| `includeReplies` | boolean | `true` | Keep replies in author feeds, feeds, lists and search results. Turn off to get top-level posts only. For a post URL it also decides whether the post's reply tree is delivered with the post. |
| `includeReposts` | boolean | `true` | Keep native reposts (a post another account reposted) in author feeds and feeds. Turn off to see only posts the account wrote itself. |
| `authorFeedFilter` | enum | `"posts_with_replies"` | Which part of an account's feed a profile URL or @handle returns. Applies to profile feeds only. |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit posts whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit posts that were seen before but are no longer found (deleted or no longer matching). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED posts. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of posts included in each notification message (1–20). |
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

Every post returns the same 45-field schema. Missing values are `null` — never omitted.

- `type`
- `postType`
- `id`
- `uri`
- `cid`
- `url`
- `portalUrl`
- `text`
- `createdAt`
- `indexedAt`
- `langs`
- `authorHandle`
- `authorDisplayName`
- `authorDid`
- `authorAvatar`
- `authorUrl`
- `likeCount`
- `repostCount`
- `replyCount`
- `quoteCount`
- `bookmarkCount`
- `engagementTotal`
- `repostedByHandle`
- `replyParentUri`
- `replyRootUri`
- `quotedPost`
- `hashtags`
- `mentions`
- `links`
- `embedType`
- `hasMedia`
- `images`
- `imageCount`
- `video`
- `externalLink`
- `labels`
- `source`
- `input`
- `searchQuery`
- `scrapedAt`
- `contentQuality`
- `detailFetched`
- `contentHash`
- `changeType`
- `listingId`

---

## Sample output

One object per post. Here is a real example from a production run:

```json
{
  "type": "post",
  "postType": "reply",
  "id": "3mytib4g4nx2t",
  "uri": "at://did:plc:y7akbffdmjwyw4e7jjbktw7q/app.bsky.feed.post/3mytib4g4nx2t",
  "cid": "bafyreia46sxe4txizp3brskimpeltxmy2xvsgkpfakou6p3si3uxtayjja",
  "url": "https://bsky.app/profile/gunpowderindia.bsky.social/post/3mytib4g4nx2t",
  "portalUrl": "https://bsky.app/profile/gunpowderindia.bsky.social/post/3mytib4g4nx2t",
  "text": "The filing contains no period-over-period financial results, so no balanced performance comparison is available.\n\n🚀 Free alerts → https://gunpowderalerts.in/go?utm_source=bluesky&…",
  "createdAt": "2026-10-27T05:16:56Z",
  "indexedAt": "2026-10-27T05:16:56.677Z",
  "langs": [],
  "authorHandle": "gunpowderindia.bsky.social"
}
```

*Truncated — full records contain 45 fields. See Output fields for the complete schema.*

**[Try Bluesky Scraper - Posts, Search and Profile Feeds now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/bsky-app-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.0009 |

See the [actor on Apify](https://apify.com/blackfalcondata/bsky-app-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape bsky.app?**
Use this actor on Apify to extract structured data from bsky.app. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get bsky.app data as JSON, CSV, or Excel?**
The actor writes each post to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape bsky.app?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check bsky.app's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each post gets a content hash. On subsequent runs, only new or changed posts are emitted — saving time, compute, and storage. Expired posts can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Bluesky Scraper - Posts, Search and Profile Feeds](https://apify.com/blackfalcondata/bsky-app-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
