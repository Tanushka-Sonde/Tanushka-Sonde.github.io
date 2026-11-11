---
layout: page
title: "Box Office Bomb Data Pipeline"
description: "Scraped, validated and enriched 138 box-office bomb records into an analytics-ready dataset."
category: Data Engineering
importance: 2
date_range: "Sep 2025"
---

**Sep 2025** · Software Tools and Techniques for AI Assignment, Prof. Nipun Batra

<a class="btn btn-sm z-depth-0" role="button" href="https://colab.research.google.com/drive/1FcTi6BDUjNgkldIlLcHMOux2kElmMvzJ?usp=sharing"><i class="fa-solid fa-book-open"></i> Colab notebook</a>

## What I did

- Built an end-to-end **data ingestion pipeline** to scrape and parse 138 box-office bomb records from Wikipedia using **BeautifulSoup**, handling nested table structures and noisy HTML data.
- Designed a **Pydantic-based validation layer** to clean titles, parse numeric ranges, and standardize financial fields (budget/loss) with automated error handling.
- Integrated the **OMDb API** to enrich records with metadata (director, language, IMDb rating, metascore) and implemented robustness to missing or failed API responses.
- Implemented **data consistency checks** between Wikipedia and OMDb release years and produced a structured dataset categorizing films by financial loss severity.
- Generated a finalized analytics-ready dataset (**138 films**) with financial statistics and loss categories, exported as a cleaned CSV for downstream analysis.

**Tech:** BeautifulSoup, Pydantic, OMDb API
