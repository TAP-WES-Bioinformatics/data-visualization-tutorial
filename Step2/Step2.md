# Step 2 - Lineage Composition Over Time

## Running the Notebook

To run this Jupyter Notebook you have two options:

### Running Locally
Create the conda environment from the repo's `environment.yaml`, then start Jupyter:

```
conda env create -f environment.yaml
conda activate data-visualization-tutorial
jupyter lab
```

Then open `Step2.ipynb` from the browser interface.

### GitHub Codespaces
Open the repository in your Codespace and click `Step2.ipynb` to open it in the VS Code notebook editor.


### Tutorial Overview

In this tutorial you'll be running a Jupyter Notebook to look at which SARS-CoV-2 lineages were circulating over time, in both the clinical and wastewater samples. You'll be:

- Plotting the lineages circulating in clinical genomic samples over time, first at full resolution, then simplified two different ways: a data-driven prevalence threshold, and a curated lineage-to-clade mapping
- Repeating both grouping approaches for the wastewater samples, using Freyja-deconvolved lineage abundances

## Plot 1: All Clinical Lineages Over Time

**Data:** `clinical_metadata.csv` — one row per clinical sample, with `sample_id`, `collection_date`, and the Pango `lineage` assigned to it.

**What it shows:** Samples are binned by week, and for each week we compute what percent of that week's samples belonged to each lineage. Stacking these percentages produces a filled area plot that always sums to 100%. Run as-is, this covers all 170 distinct lineages detected in the clinical data.

**What it lets you conclude:** Mostly, that raw lineage-level resolution is too fine-grained to read. With 170 categories, no legend can realistically list them all and the colors blur together — the plot demonstrates *why* some form of aggregation is necessary before you can spot a trend, which motivates Plot 2.

## Plot 2: Clinical Lineages Grouped by Curated Clade

**Data:** `clinical_metadata.csv` plus `curated_lineages.json` — outbreak.info's curated lineage-to-clade mapping, the same file Freyja uses internally to compute its `summarized` column.

**What it shows:** The same weekly, filled 0–100% stacked-area layout as Plot 1, but simplified by mapping each clinical sample's lineage onto a named curated clade (e.g. `EG.5* [Omicron (EG.5.X)]`) instead of a bare Pango lineage. Lookups work from broadest to most specific clade, so a lineage always lands in its most specific curated group; anything with no curated entry (mostly rare recombinants) falls into `Other`.

**What it lets you conclude:** This is a readable version of Plot 1 that groups lineages the same way Freyja's own `summarized` output does. You can see which curated clades (e.g. `EG.5.X`, `XBB.1.16.X`, `XBB.1.5.X`) actually made up a visible share of clinical sequencing and how their relative share shifted week to week — including replacement events, where one clade's share rises as another's falls. Because the grouping is curated rather than threshold-based, `Other` stays small (a few percent) even without tuning any cutoff.

## Plot 3: Wastewater Lineages That Ever Exceed 1%

**Data:** `wastewater_metadata.csv` (`sample_id`, `collection_date`, treatment plant `location`) plus `freyja_demix/` — one `.demix.tsv` file per wastewater sample, produced by [Freyja](https://github.com/andersen-lab/Freyja), each containing a `lineages` row and a matching `abundances` row (the estimated relative abundance of every lineage detected in that mixed sample).

**What it shows:** Each sample's lineage abundances are normalized to sum to 100%, then averaged across samples collected in the same week, giving a weekly average composition per lineage. Any lineage that reaches at least 1% of a week's average composition in at least one week keeps its own bucket and everything else is folded into `Other`.

**What it lets you conclude:** This is the same data-driven prevalence-threshold approach Plot 2 used before switching to curated-clade grouping, applied here to wastewater instead of clinical data. Wastewater is a pooled, community-level mixture rather than one lineage per sample, so it's inherently noisier and more diverse — at a 1% threshold, dozens of individually-rare lineages each clear the bar in some week, so `Other` stays modest but the legend is large. For a grouping-consistent, apples-to-apples comparison against the clinical data, see Plot 4 rather than Plot 2, since Plot 2 no longer uses a threshold at all.

## Plot 4: Wastewater Lineages Grouped by Curated Clade

**Data:** Same as Plot 3 (`wastewater_metadata.csv` + `freyja_demix/`), plus `curated_lineages.json`.

**What it shows:** The same weekly-averaged, renormalized wastewater lineage composition as Plot 3, but grouped using the identical curated lineage-to-clade mapping built for Plot 2, instead of a prevalence threshold. Each sample's raw per-lineage abundances are summed into their curated clade before renormalizing to 100% and averaging across samples collected in the same week.

**What it lets you conclude:** Because Plot 2 and Plot 4 share the exact same lineage-to-clade mapping, this is the most direct, apples-to-apples comparison of clinical vs. wastewater signal in this notebook. If a clade is genuinely dominant in the community, its share here should track the same rise-and-fall pattern seen in Plot 2 for the same weeks. Where the two plots agree, that's a real epidemiological signal corroborated by two independent data sources; where they diverge, that points to sampling bias, detection lag, or under-ascertainment in one stream relative to the other — which is the central question this project is trying to answer.

# Freyja plot

Additionally, you can use the ```freyja aggregate``` and ```freyja plot``` commands on the command line to make similar plots for wastewater data. Run the following on the command line in the Step2 directory.

```freyja aggregate ../data/freyja_demix/```

This should produce a file with aggregated freyja resulted called ```aggregated_result.tsv```.

Next, run the following on the command line in the Step2 directory.

```freyja plot --times wastewater_freyja_metadata.csv aggregated_result.tsv```

By default (without the ```--lineages``` flag), `freyja plot` groups lineages using the same curated, WHO-clade mapping (`curated_lineages.json`) as Plot 2 and Plot 4, so this should produce a plot similar to ```Plot 4``` from the Jupyter Notebook. Passing ```--lineages --thresh 0.01``` instead switches to raw per-lineage abundances with a flat prevalence cutoff, closer in spirit to Plot 3 — though Freyja's `--thresh` is a global frequency across all samples combined, while Plot 3's threshold is a per-week peak, so the two won't match exactly.


