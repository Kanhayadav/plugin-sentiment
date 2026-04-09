# plugin-sentiment

A lightweight sentiment microservice for stock-related news.

This service fetches recent stock news, filters relevant headlines, applies source-based weighting, and returns a sentiment score with a simple label (`positive`, `neutral`, `negative`).

## What it does

- Fetches latest stock news using `yfinance`
- Filters relevant headlines for the requested stock/company
- Removes low-quality speculative headlines like prediction/forecast noise
- Applies source-based weighting
- Runs sentiment analysis using `VADER`
- Returns:
  - stock symbol
  - sentiment score
  - sentiment label
  - filtered headlines with source

## Project structure
```bash
plugin-sentiment/
│
├── .gitignore
└── sentiment-service/
    ├── main.py
    ├── sentiment.py
    └── requirements.txt
```

Requirements
Python 3.10+
pip

Setup
1. Clone the repository-----
git clone https://github.com/Kanhayadav/plugin-sentiment.git
cd plugin-sentiment/sentiment-service

2. Create a virtual environment-------
macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
Windows
python -m venv .venv
.venv\Scripts\activate

3. Install dependencies-----
pip install -r requirements.txt
Run the service
From inside sentiment-service/:
uvicorn main:app --reload --port 8001
Server will start at:
http://127.0.0.1:8001

4. API usage-----
Endpoint
GET /sentiment?symbol=<STOCK_SYMBOL>
Example
http://127.0.0.1:8001/sentiment?symbol=MSFT


Example symbols-----
AAPL
MSFT
TSLA
NVDA
TCS.NS
RELIANCE.NS

Example response-----
{
  "symbol": "MSFT",
  "score": 0.05,
  "label": "neutral",
  "headlines": [
{
"title": "Microsoft & Oracle have a 'tremendous opportunity' in the AI boom",
"source": "Yahoo Finance Video"
},
{
"title": "Why Microsoft is a 'screaming buy.'",
"source": "Yahoo Finance Video"
}
]
}

Response fields-----
symbol → requested stock symbol
score → weighted sentiment score
label → positive, neutral, or negative
headlines → filtered relevant headlines with source
reason → returned only when insufficient data is available

Notes----
This service is a sentiment signal generator, not a buy/sell engine
It is meant to be used as an input to a larger stock analysis or ML system
Sentiment alone should not be used for final trading decisions
News comes from external sources and may vary over time

Future improvements-----
Add timestamp-based decay
Add better financial NLP model like FinBERT
Add macro-event / war / sector impact layer
Add cache expiry instead of simple in-memory caching
Add confidence scoring
Integration idea

A main backend can call this service and combine the sentiment score with model output, for example:

final_score = (model_score * 0.7) + (sentiment_score * 0.3)


Author
Kanha
