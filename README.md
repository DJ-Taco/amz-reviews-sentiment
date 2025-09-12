# Amazon Reviews Sentiment

Scrapes Amazon product reviews (Requests + Selenium backup), parses review fields,
and runs sentiment analysis (VADER + light domain tuning). Includes EDA plots
(rating trends, monthly volume) and flavor-specific word clouds.
In this case, we are scraping and analyzing Boba Protein! and also giving out insights.

> Educational project. Respect websites' Terms of Service. Do not abuse scraping.

## Features
- Selenium fallback with manual login window; cookie-less and login flows both supported
- Robust parsing of reviews (date, rating, flavor/variant)
- Sentiment analysis (VADER) + monthly trends and volume
- Flavor-specific word clouds
