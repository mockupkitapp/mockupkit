# MockupKit API

<a href="https://mockupkit.app/?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api" target="_blank">MockupKit</a> helps you auto-generate product mockups at scale from Photoshop templates via a <a href="https://mockupkit.app/mockup-api?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api" target="_blank">REST Mockup API</a>.

## Introduction

MockupKit API is a product mockup generator built for developers, print-on-demand platforms, and e-commerce tools. Upload PSD templates with smart objects, then generate photorealistic listing images programmatically — single renders, batch jobs across multiple templates, or entire curated collections in one request.

Browse 1,000+ ready-made templates or bring your own PSDs. Pass a public artwork URL and template ID to receive a high-resolution JPG or PNG in seconds.

**Try it free in the browser:** [mockupkit.app/demo](https://mockupkit.app/demo?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)

## Key Features

- **Automated mockup generation** — Replace smart objects in Photoshop templates with your artwork via API.
- **Bulk & batch rendering** — Render one design across multiple templates with `template_ids[]`, or an entire collection with `collection_id`.
- **Colour fill overrides** — Override PSD solid colour layers (garment colours, backgrounds) with `color_fills` on render requests.
- **Template catalogue API** — Filter templates by product type, frame, size, and more with `GET /v1/filter-config` and `GET /v1/templates?fields=summary`.
- **Collections** — Group templates and render an entire product line with a single API call.
- **Integration-ready** — Built for Shopify, Etsy, WooCommerce, Amazon, and custom POD platforms.
- **Batch mode (no scripts)** — Upload your PSD to the [dashboard](https://dash.mockupkit.app/signup?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api) and bulk-generate mockups visually — no ExtendScript or Photoshop actions. [Learn more →](https://mockupkit.app/bulk-psd-mockups?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)

[![Try a mockup free](https://mockupkit.app/images/heroImage.png)](https://mockupkit.app/demo?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)

## Getting Started

### Prerequisites

Before you begin, make sure you have:

- A valid MockupKit API key — create a free account and generate one on the [API Keys page](https://dash.mockupkit.app/api-keys?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api).
- A publicly accessible artwork URL (PNG or JPG on your CDN or object storage).
- Optional: your own PSD templates uploaded to MockupKit, or use templates from the [public catalogue](https://mockupkit.app/templates?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api).

### Configuration

Authenticate with a Bearer token on every request:

```javascript
const headers = {
  Authorization: 'Bearer YOUR_API_KEY',
  'Content-Type': 'application/json',
};
```

Base URL: `https://api.mockupkit.app/v1`

Full reference: [mockupkit.app/api-docs](https://mockupkit.app/api-docs?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)

### Usage

**Single template render**

```javascript
const response = await fetch('https://api.mockupkit.app/v1/render', {
  method: 'POST',
  headers: {
    Authorization: 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    template_id: '01773337-3a38-44d5-9486-9756b590a1de',
    artwork_url: 'https://your-cdn.com/design.png',
    output_format: 'jpg',
  }),
});

const data = await response.json();
// data.results[0].render_url → https://render.mockupkit.app/...
```

**Render with colour overrides** (apparel, mugs, frames with PSD colour fill layers)

```javascript
await fetch('https://api.mockupkit.app/v1/render', {
  method: 'POST',
  headers: {
    Authorization: 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    template_id: 'b56e03e2-05e4-4847-bec9-941680dcd444',
    artwork_url: 'https://your-cdn.com/design.png',
    output_format: 'jpg',
    color_fills: {
      'Sleeve Color': '#2563EB',
      'Collar Color': '#111827',
    },
  }),
});
```

**Batch render** (same artwork, multiple templates)

```javascript
await fetch('https://api.mockupkit.app/v1/render', {
  method: 'POST',
  headers: {
    Authorization: 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    template_ids: ['template-id-1', 'template-id-2', 'template-id-3'],
    artwork_url: 'https://your-cdn.com/design.png',
    output_format: 'jpg',
  }),
});
```

**Discover templates**

```bash
# Filter taxonomy for building catalogue UIs
curl -s https://api.mockupkit.app/v1/filter-config \
  -H "Authorization: Bearer YOUR_API_KEY"

# Lightweight template catalogue
curl -s "https://api.mockupkit.app/v1/templates?fields=summary&product_type=apparel" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/v1/templates` | List templates (use `?fields=summary` for catalogue) |
| `GET` | `/v1/templates/:templateId` | Full template detail including colour fills |
| `GET` | `/v1/filter-config` | Filter taxonomy for template discovery |
| `GET` | `/v1/collections` | List template collections |
| `GET` | `/v1/collections/:collectionId/templates` | Templates in a collection |
| `POST` | `/v1/render` | Single, batch, or collection render |

## Bulk generate from your PSD (no Photoshop scripts)

Not every team wants to write ExtendScript. If you have a PSD with smart objects and need dozens or hundreds of outputs:

1. **Upload your PSD** to the [MockupKit dashboard](https://dash.mockupkit.app/signup?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)
2. **Add artwork once** and adjust placement
3. **Run batch mode** — select multiple templates or variants and generate every mockup in one run
4. **Download** high-resolution JPG/PNG files

Try multi-template batch generation free in the [live demo](https://mockupkit.app/demo?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api) using **“Also generate on”**. Full guide: [mockupkit.app/bulk-psd-mockups](https://mockupkit.app/bulk-psd-mockups?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)

## Use Cases

- **Print-on-demand** — Generate listing images when a seller uploads a design.
- **E-commerce catalogues** — Batch-render one artwork across apparel, mugs, posters, and home goods.
- **Marketplace automation** — Keep Shopify, Etsy, and Amazon product images consistent at scale.
- **Custom design tools** — Power mockup pickers and configurators in your own app.
- **PSD owners without scripts** — Bulk composite artwork into your own templates via dashboard batch mode.

## Support

- **Documentation:** [mockupkit.app/api-docs](https://mockupkit.app/api-docs?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)
- **Live demo:** [mockupkit.app/demo](https://mockupkit.app/demo?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)
- **Bulk PSD batch mode:** [mockupkit.app/bulk-psd-mockups](https://mockupkit.app/bulk-psd-mockups?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)
- **API overview:** [mockupkit.app/mockup-api](https://mockupkit.app/mockup-api?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)
- **Email:** [support@mockupkit.app](mailto:support@mockupkit.app)
- **Sign up:** [dash.mockupkit.app/signup](https://dash.mockupkit.app/signup?utm_source=github&utm_medium=readme&utm_campaign=mockup-generator-api)

---

This repository contains the [MockupKit](https://mockupkit.app) marketing site and API documentation frontend (Next.js). For local development: `npm install && npm run dev`.
