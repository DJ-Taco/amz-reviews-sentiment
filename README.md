# Boba Protein Sentiment Analysis (Amazon Reviews)

## Overview
This project analyzes Amazon customer reviews for **Boba Protein** with the goal of answering two core questions:

1. **How do customers feel about the brand overall?** (sentiment)
2. **How do the two flavors compare?** (ratings + sentiment + themes)

I built a lightweight pipeline to collect review data from Amazon, clean and structure it in a dataset, and then apply **VADER sentiment analysis** + exploratory analysis to uncover trends over time and differences between products.

## Business Questions
- What is **overall customer sentiment** for Boba Protein?
- Are there meaningful differences between flavors in:
  - **average rating**
  - **sentiment**
  - common themes in review text?
- Does sentiment change across time (monthly)?
- Does sentiment meaningfully align with star ratings?

## Data Source
**Amazon product reviews** for ASIN: `B07HRJ2VPK`

The dataset used in the notebook contains:
- **100 reviews total**
- Two variants/flavors:
  - **Classic Milk Tea** (55 reviews)
  - **Vietnamese Coffee** (45 reviews)

Fields collected/created include:
- `rating`, `title`, `body`, `date_text`, `verified_purchase`, `helpful_votes`, `variant`
- Engineered columns: `Flavor`, `Date`, `Month`, `sent_compound`, `sent_label`

## Methods

### 1) Data Collection (Scraping)
- Implemented an Amazon review scraper with:
  - Requests-based parsing (where possible)
  - **Selenium fallback** (became the main driver due to dynamic content / banners)
- Parsed review elements including rating, review title/body, date, and variant info.
- Saved results to a CSV and reloaded for analysis.

### 2) Cleaning & Feature Engineering
- Converted rating strings → numeric ratings
- Extracted dates into `Date` and `Month`
- Normalized flavor labels into a clean `Flavor` field

### 3) Sentiment Analysis
- Used **NLTK VADER** to compute a sentiment score per review (`sent_compound`)
- Added a domain-tuned lexicon update to better capture food/drink language (e.g., “creamy,” “chalky,” “chemical”)
- Converted compound scores into labels using standard VADER thresholds:
  - `pos` ≥ 0.05  
  - `neg` ≤ -0.05  
  - otherwise `neu`

### 4) Exploratory Analysis & Visualization
- Rating distributions and summary stats by flavor
- Review volume over time (monthly)
- Monthly sentiment shifts overall and by flavor
- Word clouds (per flavor) after removing common/boilerplate terms and brand/category words

## Key Results

### Ratings Summary
**Overall**
- **Average rating:** **3.93**
- **Most common rating (mode):** **5.0**

**By Flavor (mean rating)**
| Flavor | Avg Rating |
|-------|------------|
| Classic Milk Tea | **4.15** |
| Vietnamese Coffee | **3.67** |

**Interpretation**
- Classic Milk Tea reviews are **higher on average** and appeared more consistently high.
- Vietnamese Coffee reviews are **lower on average** and more spread out (more mixed feedback).

### Sentiment Summary (VADER)
- `sent_compound` mean: **~0.63**
- `sent_compound` median: **~0.88**

**Interpretation**
- Overall sentiment is **positive**, with a strong positive median.
- A smaller number of negative reviews appear to pull the mean downward.

### Sentiment vs Rating Alignment
- **Correlation between VADER compound sentiment and star rating:** **~0.656**

**Interpretation**
- Sentiment and star ratings are **moderately positively correlated**, suggesting that sentiment scoring is generally consistent with how customers rated the product.

A quick cross-check using rating bins showed:
- High ratings were overwhelmingly associated with positive sentiment.
- Low/mid ratings included a larger share of negative/neutral sentiment.

### Trends Over Time (Monthly)
Monthly analysis suggests:
- Review volume increases during summer months.
- Sentiment shifts month-to-month and shows **declining sentiment during higher-volume periods**, which may reflect broader adoption and more mixed opinions as more people try the product (hypothesis, not causal).

Flavor-level monthly charts were also generated to compare:
- **Sentiment by flavor over time**
- **Review volume by flavor over time**

### Text Themes (Word Clouds)
After filtering out boilerplate and category/brand terms, the word clouds suggest:
- **Classic Milk Tea** is dominated by more consistently positive descriptors.
- **Vietnamese Coffee** still contains many positive descriptors, but surfaced a few more negative cues (e.g., “artificial,” “unfortunately”), aligning with its lower average rating and more mixed feedback.

## Conclusion
- Boba Protein’s reviews show **overall positive sentiment**, and the most common rating is **5 stars**.
- **Classic Milk Tea** outperforms Vietnamese Coffee in:
  - average star rating (**4.15 vs 3.67**)
  - consistency of ratings over time (observational)
- Sentiment scoring aligns reasonably well with customer star ratings (**corr ≈ 0.656**).

## Recommendations / Next Steps
To strengthen this analysis for product and marketing decision-making:
1. **Segment review themes** (topic modeling or clustering) by flavor to quantify recurring complaints/praises.
2. Compute **sentiment by flavor** with confidence intervals or bootstrapping to compare differences more formally.
3. Analyze review text for **specific drivers** of lower ratings in Vietnamese Coffee (sweetness, aftertaste, mixability, etc.).
4. If available, connect to **sales / return / repeat purchase** metrics to see if online sentiment reflects customer behavior.

## How to Run
1. Clone the repo
2. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib nltk wordcloud scipy statsmodels
