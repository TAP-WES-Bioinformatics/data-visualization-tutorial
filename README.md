# data-visualization-tutorial

Welcome to the tutorial for data integration and visualization! 

Use the left sidebar to browse files, and click on them to open them in the file viewer.

You'll find each step of the tutorial as a numbered directory in the sidebar, starting with Step1. Each directory contains all the instructions and data needed for that step. Go ahead and cd into Step1 and open Step1.md to get started.

This dataset contains real world clinical sequencing, wastewater sequencing, and case count data from San Diego County, California, United States from September 2023 to October 2023. Here we're going to walk through some standard data visualizations and compare what we can learn from each data modality.

Have fun!


# Running in GitHub Codespaces
This tutorial is designed to run in GitHub Codespaces. All dependencies are pre-installed in the container — no local installation required.

To launch a Codespace:

1. Click the green Code button at the top of this repository
2. Select the Codespaces tab
2. Click Create codespace on main

The container will build and open in your browser. This takes a few minutes the first time.

# Running Locally
If you prefer to run the tutorial locally, create a conda environment using the provided ```environment.yaml```:

```
conda env create -f environment.yaml
conda activate data-visualization-tutorial
```
Navigate the the ```data-visualization-tutorial``` directory and then run ```jupyter-notebook``` to launch.

# What You'll Learn

## Step 1 - Descriptive, Time-Resolved Plotting
- Plotting the number of clinical and wastewater samples over time
- Plotting case counts over time

## Step 2 - Lineage Composition Over Time
- Stacked lineplot of all lineages circulating in clinical and wastewater samples over time

## Step 3 - Spatial Resolution of Lineages
- Plotting the prevelance of a lineage over a region for clinical and wastewater samples


