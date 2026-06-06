[Booking Reviews Scraper](https://apify.com/huggable_quote/booking-reviews-scraper?fpr=data)

Extract guest reviews from Booking.com hotel pages at scale. This Apify Actor uses Booking.com's internal GraphQL API to retrieve structured review data — including reviewer info, scores, positive/negative text, room type, and stay details — without browser rendering. It supports multi-language filtering, score-based filtering, and can extract up to 1,000 reviews per hotel.

## Why Use It

- **GraphQL-based extraction**: Fetches reviews via Booking.com's internal API instead of parsing HTML, returning structured JSON with 25 reviews per request. Falls back to HTML parsing automatically if the API structure changes.
- **Multi-language support**: Filter reviews by 12 languages (English, Japanese, Korean, Chinese, Spanish, French, German, Italian, Portuguese, Russian) or retrieve all languages at once.
- **Score filtering**: Extract only reviews within a specific score range (0–10 scale), useful for sentiment analysis or quality monitoring.
- **Anti-bot handling**: Uses Apify Residential Proxies by default, rotates User-Agent headers, and applies random delays between requests. Includes exponential backoff on rate-limited responses.

## How To Use

### 1. Create a new task

Go to the [Booking.com Reviews Scraper](https://apify.com/huggable_quote/booking-reviews-scraper) page on Apify and click **Try for free**.

### 2. Configure input

Add one or more Booking.com hotel URLs. Each URL should point to a hotel detail page:

```
https://www.booking.com/hotel/jp/the-okura-tokyo.html
https://www.booking.com/hotel/us/paramount-new-york.html
```

### 3. Set options

Choose the maximum number of reviews per hotel, language filter, sort order, and score range.

### 4. Run and download

Click **Start** and wait for the run to complete. Download results as JSON, CSV, or Excel from the Dataset tab.

## Input Configuration

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `startUrls` | array | *(required)* | List of Booking.com hotel page URLs. Min 1, max 100. |
| `maxReviewsPerHotel` | integer | `100` | Maximum reviews to extract per hotel (1–1,000). |
| `language` | string | `"all"` | Filter by review language. Options: `all`, `en`, `ja`, `ko`, `zh-cn`, `zh-tw`, `es`, `fr`, `de`, `it`, `pt`, `ru`. |
| `sortBy` | string | `"newest"` | Sort order. Options: `newest`, `oldest`, `highest_score`, `lowest_score`, `most_relevant`. |
| `minReviewScore` | number | `0` | Minimum review score (0–10). |
| `maxReviewScore` | number | `10` | Maximum review score (0–10). |
| `includeReviewerInfo` | boolean | `true` | Include reviewer name, country, room type, and traveler type. Set to `false` to exclude personal data. |
| `proxyConfiguration` | object | Residential | Proxy settings. Residential proxies are recommended — Booking.com blocks most datacenter IPs. |

### Example Input

```
{
  "startUrls": [
    { "url": "https://www.booking.com/hotel/jp/the-okura-tokyo.html" },
    { "url": "https://www.booking.com/hotel/us/paramount-new-york.html" }
  ],
  "maxReviewsPerHotel": 200,
  "language": "en",
  "sortBy": "newest",
  "minReviewScore": 7,
  "maxReviewScore": 10,
  "includeReviewerInfo": true,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

## Output Format

Each review is stored as a separate record in the dataset:

```
{
  "hotelUrl": "https://www.booking.com/hotel/jp/the-okura-tokyo.html",
  "hotelName": "The Okura Tokyo",
  "hotelLocation": "Tokyo, Japan",
  "reviewId": "abc123def456",
  "reviewerName": "John D.",
  "reviewerCountry": "United States",
  "reviewerCountryCode": "us",
  "reviewDate": "2026-03-15",
  "stayDate": "2026-02",
  "roomType": "Deluxe Twin Room",
  "lengthOfStay": 3,
  "travelerType": "Couple",
  "reviewScore": 9.0,
  "reviewTitle": "Excellent stay",
  "positiveText": "Great location and service",
  "negativeText": "Wifi was a bit slow",
  "language": "en",
  "isVerifiedStay": true,
  "scrapedAt": "2026-04-16T10:00:00Z"
}
```

### Output Fields

| Field | Type | Description |
| --- | --- | --- |
| `hotelUrl` | string | Original hotel page URL |
| `hotelName` | string | Hotel name as displayed on Booking.com |
| `hotelLocation` | string | Hotel city and country |
| `reviewId` | string | Booking.com internal review identifier |
| `reviewerName` | string | Reviewer display name (empty if `includeReviewerInfo` is false) |
| `reviewerCountry` | string | Reviewer's country name |
| `reviewerCountryCode` | string | ISO 3166-1 alpha-2 country code (lowercase) |
| `reviewDate` | string | Review submission date (YYYY-MM-DD) |
| `stayDate` | string | Month of stay (YYYY-MM) |
| `roomType` | string | Room type the reviewer stayed in |
| `lengthOfStay` | integer | Number of nights |
| `travelerType` | string | Traveler category (e.g., Couple, Family, Solo traveler) |
| `reviewScore` | number | Review score on a 0–10 scale |
| `reviewTitle` | string | Review headline |
| `positiveText` | string | Positive review comments |
| `negativeText` | string | Negative review comments |
| `language` | string | Review language code |
| `isVerifiedStay` | boolean | Whether the reviewer's stay was verified by Booking.com |
| `scrapedAt` | string | Timestamp when the review was extracted (ISO 8601 UTC) |

## Use Cases

- **Hotel operators**: Monitor guest feedback across properties, identify recurring complaints, and track satisfaction trends over time.
- **Travel metasearch platforms**: Aggregate review data from Booking.com alongside other OTAs for comparison features.
- **Sentiment analysis**: Feed review text into NLP pipelines to extract sentiment scores, topic clusters, or staff performance insights.
- **Market research**: Analyze competitor hotels' guest satisfaction, pricing perception, and seasonal review patterns.
- **Academic research**: Study traveler behavior, cross-cultural review differences, or hospitality service quality at scale.

## Pricing

This Actor uses the **pay-per-event** pricing model. You are only charged for the reviews actually extracted.

| Event | Price per unit | Example |
| --- | --- | --- |
| `review-extracted` | $0.0002 | 1,000 reviews = $0.20 |

You can set a **maximum spend per run** in the Apify Console to control costs. The Actor automatically stops extracting when your spending limit is reached and returns all reviews collected up to that point.

Residential proxy usage is billed separately by Apify based on data transfer. Typical usage: ~0.5 MB per 100 reviews.

## Limitations & FAQ

**Q: How many reviews can I extract per hotel?**
A: Up to 1,000 per run. Booking.com may have fewer reviews available for some properties.

**Q: Why do I need residential proxies?**
A: Booking.com blocks most datacenter IP ranges. Residential proxies route requests through consumer IP addresses, which are not blocked. Without residential proxies, requests will likely return CAPTCHA pages or 403 errors.

**Q: Can I scrape reviews without a Booking.com account?**
A: Yes. This Actor does not require any Booking.com credentials. Reviews are publicly accessible.

**Q: How often does the GraphQL API structure change?**
A: Booking.com updates their frontend periodically. This Actor includes automatic fallback to HTML parsing when the GraphQL structure changes, so it continues working across most updates. If both methods fail, an update to the Actor will be released.

**Q: Are there rate limits?**
A: The Actor includes built-in delays (1–3 seconds between requests) and exponential backoff on 429 responses. Up to 3 hotels are processed concurrently by default.

**Q: What happens if a hotel URL is invalid?**
A: Invalid URLs are skipped with a warning log. Other hotels in the same run continue processing normally.

## Author & Contact

**Author**: [huggable_quote](https://apify.com/huggable_quote)
**License**: Apache-2.0

For bug reports or feature requests, open an issue on the [GitHub repository](https://github.com/huggable_quote/booking-reviews-scraper) or contact via Apify.