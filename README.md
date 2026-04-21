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

This project was an early anchor in my analytical development, and I'd approach several things differently now.

Working through messy, real-world data taught me that data quality decisions are modeling decisions. Every imputation strategy, every type conversion, every dropped column is a hypothesis about what matters. Getting comfortable with that ambiguity and documenting the reasoning behind it  is a skill I've continued to build.

If I revisited this today, I'd layer in spatial features using listing coordinates, test regularised regression (Ridge/Lasso) to handle multicollinearity in the amenity features, and cross-validate more rigorously. The OLS foundation here, though, gave me exactly what I needed at the time: interpretable output and a forcing function to understand every variable in the model.

---

## Data Source

Inside Airbnb Amsterdam, April 2021  
[listings.csv.gz](http://data.insideairbnb.com/the-netherlands/north-holland/amsterdam/2021-04-09/data/listings.csv.gz)

---

