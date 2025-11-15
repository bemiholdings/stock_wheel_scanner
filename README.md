# stock_wheel_scanner

A lightweight browser-based tool that scans Cboe's delayed option chains to surface cash-secured put opportunities without needing an API key.

## Features

- Pulls delayed (≈15 minutes) stock quotes and complete option chains from Cboe's public delayed quote API—no sign-up required.
- Calculates premium/strike yield, annualized return, break-even price, and discount to the underlying.
- Lets you control filters for maximum days to expiration, minimum yield, minimum discount, and how many strikes per expiry to keep.
- Presents the highest-yielding candidates in a sortable table for quick review.

## Usage

1. Open `index.html` in a modern browser.
2. Enter the tickers you want to scan (comma separated) and adjust the filters as needed.
3. Click **Scan for Opportunities** to fetch the latest delayed data and view ranked cash-secured put ideas.

> **Note:** Data comes from Cboe's delayed quote feed and may be rate-limited or change without notice. Calculations are approximations—validate everything with your broker before placing trades. This tool is for educational and entertainment purposes only.

## Verifying the data feed

You can quickly confirm that the upstream API is reachable with curl:

```bash
curl -sS "https://cdn.cboe.com/api/global/delayed_quotes/options/AAPL.json" | jq '.data.options[:5]'
```

The response includes a JSON payload with `options`, `current_price`, and other delayed metrics that the scanner filters and ranks in the browser.

## Notes on Twelve Data

Earlier revisions of this project used Twelve Data, but their option-chain endpoint now responds with an upgrade notice unless you supply a paid-tier API key:

```bash
curl -sS "https://api.twelvedata.com/options/chain?symbol=AAPL" | jq '.message'
```

Because that requirement conflicts with the goal of avoiding sign-ups, the scanner now defaults to Cboe's freely accessible delayed feed.
