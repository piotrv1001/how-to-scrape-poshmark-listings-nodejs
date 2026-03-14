# How to Scrape Poshmark Listings in Node.js

This example shows how to scrape Poshmark listings in Node.js using the [Poshmark Listings Scraper](https://apify.com/piotrv1001/poshmark-listings-scraper) actor on Apify. Instead of building a scraper from scratch, this code calls a ready-made actor via the Apify API client — you get structured listing data in minutes with no browser automation setup required.

## What this example does

- Calls the Poshmark Listings Scraper actor with a list of search URLs
- Passes an input configuration (brand pages, category pages, or search queries)
- Waits for the actor run to complete
- Fetches the results from Apify's dataset storage
- Prints each listing object to the console

## Prerequisites

- [Node.js](https://nodejs.org/) v18 or higher
- An [Apify account](https://console.apify.com/sign-up) (free tier available)
- An [Apify API token](https://console.apify.com/settings/integrations)

## Installation

```bash
npm install
```

## Environment setup

Copy `.env.example` to `.env` and add your Apify API token:

```bash
cp .env.example .env
```

Then open `.env` and replace `your_apify_token_here` with your actual token from the [Apify console](https://console.apify.com/settings/integrations).

## Usage

```bash
npm start
```

The script will run the actor, wait for results, and print each listing to the console. It also logs a direct link to your dataset in Apify Console where you can export to CSV, JSON, or Excel.

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    "searchUrls": [
        "https://poshmark.com/brand/Nike-Men",
        "https://poshmark.com/brand/Nike-Women"
    ]
};

// Run the Actor and wait for it to finish
const run = await client.actor("piotrv1001/poshmark-listings-scraper").call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

See [`sample-output.json`](./sample-output.json) for a full example. Each listing item contains:

| Field | Description |
|---|---|
| `link` | Direct URL to the Poshmark listing |
| `image` | Listing thumbnail image URL |
| `title` | Listing title |
| `price` | Current asking price |
| `oldPrice` | Original / crossed-out price |
| `size` | Item size |
| `brand` | Brand name |
| `sellerName` | Poshmark username of the seller |
| `sellerAvatar` | Seller's profile picture URL |
| `sellerLink` | URL to the seller's Poshmark closet |

## Use cases

- **Resale market research** — monitor pricing trends for specific brands or categories across thousands of listings
- **Competitive pricing** — compare your own listings against similar items to price competitively
- **Inventory sourcing** — identify underpriced items for resale by scraping brand or category pages at scale
- **Price drop tracking** — periodically scrape the same search URLs to detect when sellers lower their prices
- **Data analysis** — build datasets of Poshmark listings for analysis in Python, Excel, or BI tools

## Try the actor on Apify

**[Open the Poshmark Listings Scraper on Apify](https://apify.com/piotrv1001/poshmark-listings-scraper)**

You can run the actor directly in the browser with no code required, or integrate it into your workflow using the API.

## License

MIT
