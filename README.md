<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="Proshop Scraper API by Piloterr – scrape Proshop data as JSON" width="100%"></a>
</p>

# Proshop Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr Proshop Scraper API** lets you extract public data from Proshop as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **Proshop Product API** — Read one proshop.dk product page.
- **Proshop Search API** — Search proshop.dk by keyword or search URL.

Use it for price monitoring, assortment tracking, competitor analysis, product research or catalog enrichment.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [Proshop Product API](#proshop-product-api) | `/v2/proshop/product` | 1 | Read one proshop.dk product page. Flat JSON with DKK price, EAN, specs, and stock text. |
| [Proshop Search API](#proshop-search-api) | `/v2/proshop/search` | 1 | Search proshop.dk by keyword or search URL. Returns product cards with DKK prices and pagination. |

## Table of contents

- [How it works](#how-it-works)
- [Proshop Product API](#proshop-product-api)
- [Proshop Search API](#proshop-search-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Official SDKs](#official-sdks)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## Proshop Product API

Read one proshop.dk product page. Flat JSON with DKK price, EAN, specs, and stock text.

`GET | POST https://api.piloterr.com/v2/proshop/product` · **1 credit per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Proshop product id, or a product URL. The id is the last path segment. |
| `return_page_source` | boolean | — | When true, return the raw HTML as {"result": "..."}. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/proshop/product",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "3071602",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type ProshopProduct = {
  id?: string;
  ean?: string;
  mpn?: string;
  sku?: string;
  url?: string;
  name?: string;
  brand?: string;
  price?: number;
  images?: string[];
  summary?: string;
  currency?: string;
  in_stock?: boolean;
  image_url?: string;
  breadcrumbs?: {
    url?: string;
    name?: string;
  }[];
  availability?: string;
  price_ex_vat?: number;
  price_display?: string;
  specifications?: {
    name?: string;
    value?: string;
  }[];
  price_ex_vat_display?: string;
};

const params = new URLSearchParams({
  "query": "3071602",
});

const response = await fetch(`https://api.piloterr.com/v2/proshop/product?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as ProshopProduct;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/proshop/product
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "3071602"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "id": "3071602",
  "ean": "0023942493525",
  "mpn": "49352",
  "sku": "3071602",
  "url": "https://www.proshop.dk/3071602",
  "name": "Verbatim Vi550 S3 SSD - 512GB - SATA-600 - 2.5\"",
  "brand": "Verbatim",
  "price": 580,
  "images": [
    "https://www.proshop.dk/Images/915x900/3071602_95234c5c86d9.jpg",
    "https://www.proshop.dk/Images/915x900/3071602_349c4c4c7a0f.jpg"
  ],
  "summary": "SSD (Solid State Drive), 512 GB, intern, overførselshastighed: 520 MB/s (læs) / 500 MB/s (skriv), IOPS: 75413 IOPS (læs) / 86756 IOPS (sk…",
  "currency": "DKK",
  "in_stock": true,
  "image_url": "https://www.proshop.dk/Images/915x900/3071602_95234c5c86d9.jpg",
  "breadcrumbs": [
    {
      "url": "https://www.proshop.dk/",
      "name": "Forside"
    },
    {
      "url": "https://www.proshop.dk/Hardware",
      "name": "Hardware"
    }
  ],
  "availability": "15+ stk. på lager – Levering i morgen (bestil inden kl. 22)",
  "price_ex_vat": 464,
  "price_display": "580,00 kr.",
  "specifications": [
    {
      "name": "Producent",
      "value": "Verbatim"
    },
    {
      "name": "Varenummer",
      "value": "3071602"
    }
  ],
  "price_ex_vat_display": "464,00 kr."
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `id` | string |  |
| `ean` | string |  |
| `mpn` | string |  |
| `sku` | string |  |
| `url` | string |  |
| `name` | string |  |
| `brand` | string |  |
| `price` | integer |  |
| `images` | string[] |  |
| `summary` | string |  |
| `currency` | string |  |
| `in_stock` | boolean |  |
| `image_url` | string |  |
| `breadcrumbs[]` | object[] |  |
| `breadcrumbs[].url` | string |  |
| `breadcrumbs[].name` | string |  |
| `availability` | string |  |
| `price_ex_vat` | integer |  |
| `price_display` | string |  |
| `specifications[]` | object[] |  |
| `specifications[].name` | string |  |
| `specifications[].value` | string |  |
| `price_ex_vat_display` | string |  |

## Proshop Search API

Search proshop.dk by keyword or search URL. Returns product cards with DKK prices and pagination.

`GET | POST https://api.piloterr.com/v2/proshop/search` · **1 credit per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Keyword or a full Proshop search URL with s=. A product URL is rejected. |
| `page` | number | — | Page number starting at 1. Proshop sends this as pn. |
| `return_page_source` | boolean | — | When true, return the raw HTML as {"result": "..."}. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/proshop/search",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "ssd",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type ProshopSearch = {
  results?: {
    id?: string;
    url?: string;
    name?: string;
    price?: number;
    summary?: string;
    currency?: string;
    in_stock?: boolean;
    image_url?: string;
    availability?: string;
    price_ex_vat?: number;
    price_display?: string;
    price_ex_vat_display?: string;
  }[];
  pagination?: {
    next?: string;
    page?: number;
    per_page?: number;
    total_pages?: number;
    total_results?: number;
  };
};

const params = new URLSearchParams({
  "query": "ssd",
});

const response = await fetch(`https://api.piloterr.com/v2/proshop/search?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as ProshopSearch;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/proshop/search
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "ssd"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "results": [
    {
      "id": "3071602",
      "url": "https://www.proshop.dk/SSD/Verbatim-Vi550-S3-SSD-512GB-SATA-600-25/3071602",
      "name": "Verbatim Vi550 S3 SSD - 512GB - SATA-600 - 2.5\"",
      "price": 580,
      "summary": "SSD (Solid State Drive), 512 GB, intern, overførselshastighed: 520 MB/s (læs) / 500 MB/s (skriv), IOPS: 75413 IOPS (læs) / 86756 IOPS (sk…",
      "currency": "DKK",
      "in_stock": true,
      "image_url": "https://www.proshop.dk/Images/174x116/3071602_95234c5c86d9.jpg",
      "availability": "På lager – Levering i morgen (bestil inden kl. 22)",
      "price_ex_vat": 464,
      "price_display": "580,00 kr.",
      "price_ex_vat_display": "464,00 kr."
    }
  ],
  "pagination": {
    "next": "https://www.proshop.dk/?s=ssd&pn=2",
    "page": 1,
    "per_page": 25,
    "total_pages": 95,
    "total_results": 2364
  }
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `results[]` | object[] |  |
| `results[].id` | string |  |
| `results[].url` | string |  |
| `results[].name` | string |  |
| `results[].price` | integer |  |
| `results[].summary` | string |  |
| `results[].currency` | string |  |
| `results[].in_stock` | boolean |  |
| `results[].image_url` | string |  |
| `results[].availability` | string |  |
| `results[].price_ex_vat` | integer |  |
| `results[].price_display` | string |  |
| `results[].price_ex_vat_display` | string |  |
| `pagination` | object |  |
| `pagination.next` | string |  |
| `pagination.page` | integer |  |
| `pagination.per_page` | integer |  |
| `pagination.total_pages` | integer |  |
| `pagination.total_results` | integer |  |

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Product | Search |
|---|---|---|
| cURL (Bash) | [`product`](examples/curl/product.sh) | [`search`](examples/curl/search.sh) |
| PHP | [`product`](examples/php/product.php) | [`search`](examples/php/search.php) |
| C# | [`product`](examples/csharp/Product.cs) | [`search`](examples/csharp/Search.cs) |
| Go | [`product`](examples/go/product.go) | [`search`](examples/go/search.go) |
| Java | [`product`](examples/java/ProductExample.java) | [`search`](examples/java/SearchExample.java) |
| Ruby | [`product`](examples/ruby/product.rb) | [`search`](examples/ruby/search.rb) |
| Node.js | [`product`](examples/nodejs/product.mjs) | [`search`](examples/nodejs/search.mjs) |

## Practical use cases

- **Price monitoring** — Track prices, promotions and availability over time to react to competitors.
- **Assortment & catalog tracking** — Follow new products, delisted items and category changes.
- **Competitor analysis** — Benchmark ranges, brands and positioning across retailers.
- **Product research** — Spot best-sellers and trends from search results.
- **Catalog enrichment** — Pull titles, images and attributes into your PIM or marketplace listings.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — Proshop plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape Proshop?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/proshop/product` **1**, `/v2/proshop/search` **1**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the Proshop API return?

Each endpoint returns structured JSON: **Proshop Product** — `id`, `ean`, `mpn`, `sku`, `url`, `name`; **Proshop Search** — `results[]`, `results[].id`, `results[].url`, `results[].name`, `results[].price`, `results[].summary`.

### How do I paginate Proshop results?

Use the `page` parameter and keep requesting while the response indicates more results.

### Can I get the raw HTML instead of JSON?

Yes. Set `return_page_source=true` and the response becomes `{"result": "..."}` with the page source.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Official SDKs

Use Piloterr from your own language with a typed client:

| SDK | Package | Install |
| --- | --- | --- |
| Node.js | [`@piloterr/sdk`](https://www.npmjs.com/package/@piloterr/sdk) | `npm install @piloterr/sdk` |
| Python | [`piloterr`](https://pypi.org/project/piloterr/) | `pip install piloterr` |
| Go | [`piloterr-go`](https://pkg.go.dev/github.com/piloterr/piloterr-go) | `go get github.com/piloterr/piloterr-go` |
| .NET | [`Piloterr`](https://www.nuget.org/packages/Piloterr) | `dotnet add package Piloterr` |
| Ruby | [`piloterr`](https://rubygems.org/gems/piloterr) | `gem install piloterr` |
| PHP | [`piloterr/sdk`](https://packagist.org/packages/piloterr/sdk) | `composer require piloterr/sdk` |
| Java | [`piloterr-java`](https://github.com/piloterr/piloterr-java) | build from source, see its README |
| CLI | [`@piloterr/cli`](https://www.npmjs.com/package/@piloterr/cli) | `npm install -g @piloterr/cli` |
| MCP server | [`@piloterr/mcp`](https://www.npmjs.com/package/@piloterr/mcp) | `npx -y @piloterr/mcp` |

## Related scrapers

- [About You Scraper API](https://github.com/piloterr/aboutyou-scraper)
- [Action Scraper API](https://github.com/piloterr/action-scraper)
- [Aldi Scraper API](https://github.com/piloterr/aldi-scraper)
- [Alibaba Scraper API](https://github.com/piloterr/alibaba-scraper)
- [AliExpress Scraper API](https://github.com/piloterr/aliexpress-scraper)
- [Allegro Scraper API](https://github.com/piloterr/allegro-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>Proshop is a trademark of its respective owner. This repository is not affiliated with or endorsed by Proshop. Examples are released under the MIT license.</sub>
