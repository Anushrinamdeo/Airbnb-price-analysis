# Amsterdam Airbnb Pricing
> What actually drives the price of a listing in one of Europe's most competitive short-term rental markets?  
> This project digs into that question using real listing data from Amsterdam and the answers are more nuanced than you'd expect.

---

## The Question Worth Asking

Amsterdam's Airbnb market is notoriously dense. Thousands of listings compete across a city where space is limited, tourism is relentless, and pricing decisions can make or break a host's return. Yet most hosts price based on gut feel.

This project treats that as a data problem. Using a snapshot of live listing data from [Inside Airbnb](http://data.insideairbnb.com/the-netherlands/north-holland/amsterdam/2021-04-09/data/listings.csv.gz), I set out to understand which variables genuinely move the needle on price and built an OLS regression model to quantify it.

---

## What's Under the Hood

The raw dataset was far from model-ready. A significant portion of the analytical work happened before a single coefficient was estimated.

**Data Preparation**
- Parsed and cleaned 90+ listing attributes from a compressed CSV source
- Converted price fields from string format (with currency symbols) to float, a small but critical step that broke several naive approaches
- Downcast numeric columns to memory-efficient dtypes, reducing the working dataset size by ~40%
- Imputed null values with a strategy informed by feature distribution, not blanket mean-fill

**Feature Engineering**
- Extracted host tenure from `host_since` timestamps to create a continuous seniority variable
- Derived binary flags from amenity text fields (e.g., presence of WiFi, kitchen, parking)
- Encoded categorical neighbourhood data to capture location premium without leaking data

**Modeling**
- Ran Ordinary Least Squares regression using `statsmodels` for interpretable, coefficient-level output
- Evaluated model fit using R², residual distribution, and p-values per feature
- Deliberately avoided black-box approaches for transparency over marginal accuracy

---

## What the Data Revealed

A few findings stood out as genuinely business-relevant:

- **Location is not just a cliché.** Neighbourhood alone explained a disproportionate share of price variance; listings in the Canal Ring commanded premiums that no amenity could fully offset.
- **Host tenure has diminishing returns.** Newer hosts with well-maintained listings weren't priced out but highly experienced hosts did price higher on average, suggesting that platform reputation gets baked into the rate.
- **Amenities cluster, not accumulate.** Adding individual amenities had modest independent effects, but listings with a coherent "premium" amenity bundle (WiFi + kitchen + private entrance) showed a compounding price lift.
- **Review scores had a weaker direct price effect than expected.** This likely reflects that reviews influence conversion (occupancy), not rate, a distinction that matters for revenue strategy.

---

## Technical Stack

| Tool | Purpose |
|------|---------|
| `pandas` | Data wrangling and transformation |
| `numpy` | Numerical operations and dtype management |
| `statsmodels` | OLS regression and diagnostics |
| `matplotlib` / `seaborn` | Exploratory visualisation |

---

## Growth & Reflection

This was one of my first end-to-end data projects, and it taught me 
things no course assignment could.

Working with real listing data, inconsistent formats, missing values, and 
columns that look clean until they aren't forced me to make judgment 
calls that textbooks don't prepare you for. I learned that cleaning data 
isn't a chore before the "real work" begins. It *is* the real work.

The OLS model gave me something I genuinely value at this stage: 
interpretability. Every coefficient had to mean something. That 
constraint pushed me to understand the data deeply rather than throw 
features at a model and hope for accuracy.

If I built this again today, I'd test regularised regression to handle 
correlated amenity features, and bring in spatial data using listing 
coordinates. The instinct to reach for those tools came directly from 
doing this project the simpler way first.

I'm early in my analytical journey — but I'm building it on a foundation 
of understanding, not just execution.
---

## Data Source

Inside Airbnb Amsterdam, April 2021  
[listings.csv.gz](http://data.insideairbnb.com/the-netherlands/north-holland/amsterdam/2021-04-09/data/listings.csv.gz)

---

