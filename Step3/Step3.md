# Step 3 - Spatial Resolution of Lineages

## Running the Notebook

### Running Locally
```
conda env create -f environment.yaml
conda activate data-visualization-tutorial
jupyter lab
```
Then open `Step3.ipynb`.

### GitHub Codespaces
Open the repository in your Codespace and click `Step3.ipynb`.

## Overview

This notebook tracks one variant family, `EG.5*`, across San Diego County during weeks 35-38 of 2023, using the clinical and wastewater data from Step 2.

- Lineages are grouped into curated variant families (`curated_lineages.json`), resolving overlapping/nested clades by keeping the most specific match — the same rule Freyja uses internally.
- Clinical zip codes are validated against San Diego County's actual boundary (fetched via `pygris`/Census TIGER-Line), dropping missing, malformed, or out-of-county entries.
- Both plots use the same 4-week window and the same red color scale (0-100%), so they can be compared directly.

## Plot 1: EG.5* Share by Zip Code (Clinical)

**Data:** `clinical_metadata.csv` (`sample_id`, `collection_date`, `lineage`, `zipcode`) plus `curated_lineages.json` for variant grouping, plus zip code boundary polygons fetched via `pygris`.

**What it shows:** A choropleth of the percent of clinical samples that were `EG.5*` in each San Diego County zip code, weeks 35-38 of 2023. Zip codes below `MIN_SAMPLES` are grayed out as unreliable rather than colored as if confident.

**What it lets you conclude:** Where `EG.5*` had a real foothold in the county versus where the data is too thin to say anything — the gray zip codes are as informative as the colored ones, since they mark where a single sample would otherwise be mistaken for a trend.

## Plot 2: EG.5* Share by Treatment Plant (Wastewater)

**Data:** `wastewater_metadata.csv` (`sample_id`, `collection_date`, `location`) plus `freyja_demix/` (`.demix.tsv` per sample) for lineage abundances, grouped with the same `curated_lineages.json` mapping as Plot 1.

**What it shows:** A bar chart of the percent of wastewater signal that was `EG.5*` at each treatment plant (Encina, Point Loma, South Bay), same time window and color scale as Plot 1, each bar labeled with its sample count (`n`).

**What it lets you conclude:** Whether the community-level wastewater signal agrees with the clinical picture in Plot 1 for the same weeks — since both use the same lineage grouping and color scale, a similar `EG.5*` share in a plant's service area and its overlapping zip codes is corroborating evidence, while a mismatch points to sampling bias or under-ascertainment in one stream.
