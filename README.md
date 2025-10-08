# QTM 350 — Assignment 05: Literate Programming with Quarto

This repository contains:
- `report.qmd`: Main Quarto report (renders to HTML & PDF)
- `slides.qmd`: Reveal.js presentation
- `references.bib`: BibTeX file with sources
- `wdi.csv`: **Place the dataset file here** (not committed in this starter unless you add it)

## Live links (fill these in after you publish)
- **Repository URL:** <ADD-YOUR-REPO-URL>
- **HTML Report (GitHub Pages or GitHack):** <ADD-PAGES-URL-TO-report.html>
- **Slides (Reveal.js HTML):** <ADD-PAGES-URL-TO-slides.html>

## Render
From the repo root:
```bash
quarto render report.qmd
quarto render slides.qmd
```

## Data
Obtain `wdi.csv` from the course GitHub or generate it with Python (requires internet + libraries):

```python
# pip install pandas wbgapi
import pandas as pd
import wbgapi as wb

indicators = {
    "gdp_per_capita": "NY.GDP.PCAP.CD",
    "gdp_growth_rate": "NY.GDP.MKTP.KD.ZG",
    "inflation_rate": "FP.CPI.TOTL.ZG",
    "unemployment_rate": "SL.UEM.TOTL.ZS",
    "total_population": "SP.POP.TOTL",
    "life_expectancy": "SP.DYN.LE00.IN",
    "adult_literacy_rate": "SE.ADT.LITR.ZS",
    "income_inequality": "SI.POV.GINI",
    "health_expenditure_gdp_share": "SH.XPD.CHEX.GD.ZS",
    "measles_immunisation_rate": "SH.IMM.MEAS",
    "education_expenditure_gdp_share": "SE.XPD.TOTL.GD.ZS",
    "primary_school_enrolment_rate": "SE.PRM.ENRR",
    "exports_gdp_share": "NE.EXP.GNFS.ZS",
}

country_codes = wb.region.members("WLD")
df = wb.data.DataFrame(indicators.values(), economy=country_codes, time=2022, skipBlanks=True, labels=True).reset_index()
df = df.drop(columns=["economy"], errors="ignore")
df = df.rename(columns=lambda x: {v:k for k,v in indicators.items()}.get(x, x).lower())
df = df.sort_values("country").reset_index(drop=True)
df.to_csv("wdi.csv", index=False)
```

## Cross-references & citations
See the Quarto source to learn how figure/table labels (e.g., `{#fig-scatter}`, `{#tbl-summary}`) and citations (e.g., `[@worldbank_wdi]`) are used.

## Presentation
`slides.qmd` uses `format: revealjs` with a fade transition and slide numbers. It reuses the same dataset and re-creates the core figures.

## Git workflow (minimal)
```bash
git init
git add .
git commit -m "Assignment 05: Quarto report + slides"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

## Publish
- **GitHub Pages**: enable Pages on the repo (root or `/docs`), then push the rendered HTML files (`report.html`, `slides.html`).
- **GitHack**: alternatively, use GitHack to serve raw HTML files (paste the raw file URL).
