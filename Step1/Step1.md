# Step 1 - Descriptive, Time-Resolved Plotting

## Running the Notebook

### Running Locally
```
conda env create -f environment.yaml
conda activate data-visualization-tutorial
jupyter lab
```
Then open `Step1.ipynb`.

### GitHub Codespaces
Open the repository in your Codespace and click `Step1.ipynb`.

## Overview

This notebook is an introduction to descriptive, time-resolved plotting using San Diego COVID-19 case counts alongside the clinical and wastewater sample metadata used throughout the rest of this tutorial (Sept-Oct 2023).

- Plotting reported case counts over time, then smoothing out day-to-day noise with a weekly average
- Plotting how many clinical and wastewater samples were collected each week, to see how sampling effort varies over time
- Comparing sequencing counts against case counts, to check how representative the sequenced samples are of the underlying epidemic

## Plot 1: Daily Case Counts

**Data:** `covid19cases_test.csv` (`date`, `cases`), filtered to September-October 2023.

**What it shows:** A simple line plot of reported COVID-19 case counts by day.

**What it lets you conclude:** Daily case counts are noisy — reporting delays, weekends, and holidays create jagged, hard-to-read fluctuations, which motivates the smoothing in Plot 2.

## Plot 2: Weekly-Smoothed Case Counts

**Data:** Same as Plot 1.

**What it shows:** The daily case count line from Plot 1 (lightened), overlaid with a weekly rolling average.

**What it lets you conclude:** Aggregating by week removes most of the day-to-day noise and reveals the underlying trend — whether cases were rising, falling, or flat over the period — much more clearly than the daily view.

## Plot 3: Clinical Samples Over Time

**Data:** `clinical_metadata.csv` (`sample_id`, `collection_date`), binned into ISO weeks.

**What it shows:** A bar chart of how many clinical samples were collected each week.

**What it lets you conclude:** How clinical sequencing effort was distributed over the study period — whether it was roughly constant or concentrated in particular weeks, which matters for interpreting any lineage trend built on top of it.

## Plot 4: Wastewater Samples Over Time

**Data:** `wastewater_metadata.csv` (`sample_id`, `collection_date`), binned into ISO weeks.

**What it shows:** The same weekly bar chart as Plot 3, for wastewater samples instead of clinical.

**What it lets you conclude:** Whether wastewater sampling effort tracks clinical sampling effort over time, or the two data streams have different gaps and coverage.

## Plot 5: Sequencing Counts vs. Case Counts

**Data:** Weekly clinical sample counts (Plot 3) plus weekly-averaged case counts (Plot 2).

**What it shows:** Both series plotted together on the same weekly x-axis, so their shapes can be compared directly.

**What it lets you conclude:** Whether clinical sequencing kept pace with the actual case trend. If sequencing counts diverge from case counts (e.g. sequencing effort drops while cases keep rising), that's a sign the sequenced samples may be under-representing the true epidemic in that period — important context for any variant-prevalence conclusions drawn from the sequencing data alone.
