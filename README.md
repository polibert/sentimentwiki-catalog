# SentimentWiki — Financial Sentiment Inversion Catalog

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![API](https://img.shields.io/badge/API-sentimentwiki.io%2Fv1-blue)](https://sentimentwiki.io/api)
[![Community](https://img.shields.io/badge/Contribute-sentimentwiki.io-green)](https://sentimentwiki.io/contribute/label)

An open, community-maintained catalog of **financial sentiment inversions** — phrases where a generic NLP model predicts the wrong direction for a specific asset.

## The Problem

Generic sentiment models like FinBERT read *"OPEC announces output cut"* as **bearish** (cutting = negative, right?).

For crude oil, it's **bullish**. Less supply → higher prices.

This happens constantly in commodity markets, forex, and macro-sensitive equities. The model doesn't know the asset. The community does.

## What's in This Dataset

| Field | Description |
|-------|-------------|
| `symbol` | Asset ticker (OIL, GOLD, NATGAS, BTC, etc.) |
| `full_name` | Full asset name |
| `asset_class` | energy / metals / agriculture / crypto / equity_index / forex |
| `phrase` | The trigger phrase |
| `naive_polarity` | What a generic model predicts (positive/negative) |
| `actual_direction` | Correct market direction (bullish/bearish) |
| `reason` | Why the inversion exists |
| `confidence` | 0.0–1.0 community confidence score |
| `status` | `active` (validated) or `hypothesis` (pending) |

## Coverage

35+ securities across 6 asset classes:

- **Energy:** OIL (WTI), BRENT, NATGAS (Henry Hub), GAS (TTF), LNG, ELEC
- **Metals:** GOLD, SILVER, COPPER, PLATINUM, PALLADIUM, ALUMINUM, STEEL_REBAR
- **Agriculture:** CORN, WHEAT, SOYA, COTTON, COFFEE, COCOA, SUGAR, RBOB
- **Crypto:** BTC, ETH
- **Equity Indices:** SPX, DAX
- **Forex:** USDJPY, EURUSD, AUDUSD, USDCAD, USDCHF

## Files

- `sentimentwiki_inversions.csv` — all inversions (active + hypotheses)
- `active_inversions.json` — validated inversions only, JSON format

## API

Free public API — no auth required for basic use:

```python
import requests

response = requests.post("https://sentimentwiki.io/v1/analyze", json={
    "text": "OPEC announces surprise production cut of 1 million barrels per day",
    "security": "OIL"
})

print(response.json())
# {
#   "direction": "BULLISH",
#   "relevance": 0.97,
#   "magnitude": 0.85,
#   "reasoning": "OPEC output cuts reduce supply → bullish for crude prices...",
#   "model_version": "haiku-4-5"
# }
```

Full API docs: [sentimentwiki.io/api](https://sentimentwiki.io/api)

## Contribute

The catalog is built by the community. You can:

- **Label headlines** — [sentimentwiki.io/contribute/label](https://sentimentwiki.io/contribute/label)
- **Validate hypotheses** — vote on AI-generated inversion candidates
- **Submit inversions** — highlight phrases in articles and tag their market direction
- **Push from your pipeline** — `POST /v1/inversions/suggest` (see API docs)

## Roadmap

- [ ] LoRA fine-tune on community labels (target: 500+ human labels)
- [ ] Per-security model adapters
- [ ] Python SDK (`pip install sentimentwiki`)
- [ ] HuggingFace dataset release
- [ ] arXiv benchmark paper

## License

Dataset: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — free to use, cite SentimentWiki.  
Platform code: MIT (coming soon)

## Citation

```bibtex
@misc{sentimentwiki2026,
  title={SentimentWiki: A Community-Maintained Financial Sentiment Inversion Catalog},
  author={SentimentWiki Contributors},
  year={2026},
  url={https://sentimentwiki.io}
}
```
