# SEO Image AI — `seo_image_generate`

**MCP tool name:** `seo_image_generate`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/seo-image/` (action: `generate`)
**sv-cli command:** `sv seo-image` (aliases: image)
**API reference:** [SEO Image AI](https://seovendor.co/api/seo-image.html)
**Type:** Synchronous

## What it does

Generate on-brand, SEO-optimized images from a keyword, with control over image type, theme, background, colors, and dimensions.

## When to use this

Use when an article or landing page needs a supporting image optimized for image search / alt-text relevance.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `kw` | string | required | Keyword text, 4-300 characters. |
| `brand` | string | optional | Brand text, max 60 characters. |
| `web` | string | optional | Website URL, max 300 characters. |
| `url` | string | optional | Fallback website URL used when `web` is not sent. |
| `location` | string | optional | Location text, max 200 characters. |
| `notes` | string | optional | Notes text, max 200 characters. |
| `country` | string | optional | Country text, max 100 characters. |
| `brandpersona` | string | optional | Brand persona text, max 2000 characters. |
| `lang` | integer | optional | Language ID, same 40-language set. |
| `imagetype` | integer | optional | ~150 options (0=Auto/AI, then Social Media Post, Blog Header Image (33), Website Hero Image, Logo, Icon, Ad creative formats, and many more). CLI flag is `--type` (not `--imagetype`). See `sv image types`. |
| `imagetheme` | integer | optional | ~50 options (0=Auto/AI, Urban Cityscape, Futuristic, Cyberpunk, Medieval, Steampunk, etc). CLI flag is `--theme` (not `--imagetheme`). See `sv image themes --search <term>`. |
| `background` | integer | optional | ~52 options (0=Auto/AI, Gradient, Bokeh, Dark, Light, Office, Outdoors, etc). CLI discovery command is `sv image backgrounds`; exact generate-time flag name not confirmed in the quick sheet's examples — check `sv seo-image generate --help`. |
| `imagebackground` | integer | optional | Same enum as `background` — appears to be a duplicate/alias field; confirm which one the current schema honors via `sv options seo-image`. |
| `primarycolor` | integer | optional | 0=Auto (AI), then 20 named colors (Blue, Black, Red, Gold, Teal, etc). CLI discovery command is `sv image colors`; exact generate-time flag name not confirmed — check `sv seo-image generate --help`. |
| `secondarycolor` | integer | optional | Same 20-color enum as `primarycolor`. |
| `imagesize` | integer | optional | 0=1536x1024 Wide, 1=1024x1024 Square, 2=1792x1024 Tall. CLI flag is `--size` (not `--imagesize`). See `sv image sizes`. |
| `engine` | integer | optional | 0=Image GPT 1.0 (Simple), 1=Image GPT 2.0 (Complex), 2=Image GPT 1 Mini (Fast). |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options seo-image <field>` or `sv options seo-image <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show seo-image` shows the full current schema.

## Example request

```json
{
  "action": "generate",
  "kw": "white label seo",
  "web": "https://example.com",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`data.image_url` (full URL to the file), plus `filename`, `fileformat`, `size`, `compression`, and echoed labels for `imagetype`/`imagetheme`/`background`/`primarycolor`/`secondarycolor`/`notes`/`country`/`engine`. Synchronous, HTTP 200.

## Chaining with other tools

Pairs naturally with `seogpt_generate` or `seogpt2_create_task` — generate the article, then generate a matching hero image from the same target keyword.

## Gotchas

`background` and `imagebackground` both appear in the schema with the same enum — verify with `sv definitions show seo-image` which one is actually honored before relying on it in an automated pipeline.

