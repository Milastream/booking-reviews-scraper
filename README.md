[Booking Reviews Scraper](https://apify.com/sovereigntaylor/booking-reviews-scraper?fpr=data)

# Booking.com Reviews Scraper

Extract hotel reviews from Booking.com with full depth. Get positive and negative comments, reviewer details, scores, room types, stay duration, traveler types, and complete hotel metadata.

## What data does it extract?

### Per review

- **Review text** -- both positive ("Liked") and negative ("Disliked") sections
- **Review title** -- the headline summary written by the guest
- **Overall score** -- numeric score (1-10 scale)
- **Review date** -- when the review was written
- **Stay date** -- month and year of the actual stay
- **Reviewer name** and **country** of origin
- **Room type** -- the specific room booked (e.g., "Deluxe King Room")
- **Nights stayed** -- duration of the stay
- **Traveler type** -- solo, couple, family, or group
- **Review language** -- original language of the review

### Per hotel

- **Hotel name**, **address**, **city**, **country**
- **Star rating** (1-5)
- **Overall score** and **total review count**
- **Category scores** -- Staff, Facilities, Cleanliness, Comfort, Value for Money, Location, Free WiFi
- **Facilities list** -- parking, pool, spa, restaurant, etc.
- **Photo URLs** -- hotel and room images
- **Description** -- full hotel description text
- **Check-in/Check-out** times
- **Property type** -- hotel, apartment, hostel, etc.

## Use cases

- **Hospitality research** -- analyze guest sentiment across hotels, chains, or destinations
- **Hotel benchmarking** -- compare review scores and category ratings between competing properties
- **Travel analytics** -- track seasonal patterns, identify trending destinations, measure satisfaction trends
- **Reputation management** -- monitor what guests say about your property vs. competitors
- **Investment due diligence** -- evaluate hotel performance through guest satisfaction data before acquisition
- **Content marketing** -- generate data-driven travel guides and destination comparisons
- **Service improvement** -- identify the most common complaints and praised features across properties

## Input examples

### Scrape reviews from specific hotels

```
{
  "hotelUrls": [
    "https://www.booking.com/hotel/us/the-plaza-new-york.html",
    "https://www.booking.com/hotel/gb/the-savoy.html"
  ],
  "maxReviews": 500,
  "sortBy": "recent",
  "language": "en"
}
```

### Search a destination and scrape top hotels

```
{
  "searchQuery": "Paris",
  "maxHotelsFromSearch": 20,
  "maxReviews": 200,
  "sortBy": "recent",
  "guestType": "couple"
}
```

### Get only negative reviews for competitor analysis

```
{
  "hotelUrls": ["https://www.booking.com/hotel/fr/ritz-paris.html"],
  "maxReviews": 1000,
  "sortBy": "lowest",
  "language": "all"
}
```

### Filter by guest type for family travel research

```
{
  "hotelUrls": ["https://www.booking.com/hotel/us/disney-contemporary-resort.html"],
  "maxReviews": 500,
  "guestType": "family",
  "sortBy": "recent"
}
```

## Output example

```
{
  "hotelName": "The Plaza",
  "hotelUrl": "https://www.booking.com/hotel/us/the-plaza-new-york.html",
  "hotelStarRating": 5,
  "hotelOverallScore": 8.9,
  "hotelTotalReviews": 4521,
  "reviewerName": "Sarah",
  "reviewerCountry": "United Kingdom",
  "overallScore": 9.2,
  "reviewTitle": "Unforgettable luxury experience",
  "positiveText": "The rooms are stunning, staff was incredibly attentive, and the location on Central Park is unbeatable. Afternoon tea in the Palm Court was magical.",
  "negativeText": "The valet parking fee was quite steep at $75/night.",
  "reviewDate": "2025-12-15",
  "stayDate": "November 2025",
  "roomType": "Deluxe King Room with Park View",
  "nightsStayed": 3,
  "travelerType": "Couple",
  "reviewLanguage": "en",
  "scrapedAt": "2026-03-02T12:00:00.000Z"
}
```

## Pricing

This actor uses pay-per-event pricing. You are charged $0.004 per review extracted. Hotel metadata is included at no extra cost.

| Reviews | Cost |
| --- | --- |
| 100 | $0.40 |
| 500 | $2.00 |
| 1,000 | $4.00 |
| 10,000 | $40.00 |

## Tips for best results

1. **Use proxies** -- Booking.com has anti-bot protections. Apify proxies are recommended.
2. **Start with fewer reviews** -- test with `maxReviews: 50` before running large extractions.
3. **Language filter** -- set `language: "all"` if you want reviews in every language, or specify a language code for targeted results.
4. **Sort by recent** -- gives you the freshest data for trend analysis.
5. **Combine with hotel details** -- keep `includeHotelDetails: true` for complete property context.

## Integrations

Connect this scraper to your workflow:

- **Google Sheets** -- export reviews directly to spreadsheets
- **Webhooks** -- get notified when scraping completes
- **API** -- integrate with your own applications
- **Zapier / Make** -- automate review monitoring pipelines

## Support

Built by Sovereign AI. For questions or feature requests, open an issue on GitHub or contact [ricardo.yudi@gmail.com](mailto:ricardo.yudi@gmail.com).