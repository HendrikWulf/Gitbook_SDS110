---
description: Detecting traffic accident clusters
icon: burst
---

# Lab 4 - Analyzing Spatial Data

<figure><img src=".gitbook/assets/auto-accident-9_crop.jpg" alt=""><figcaption><p>Auto accident (<a href="https://picryl.com/media/auto-accident-9">image</a> <a href="https://www.loc.gov/">source</a>)</p></figcaption></figure>

## Introduction

Spatial data is more than just dots on a map, it’s a window into real-world patterns and behaviors. Whether we’re studying wildlife movements, disease outbreaks, or urban infrastructure, the ability to analyze where things happen is a cornerstone of spatial data science. In this lab, we explore methods to detect and interpret spatial clusters, using a real-world dataset of traffic accidents across Switzerland.

By analyzing the spatial distribution of accident locations, you will learn how to uncover patterns that are not always visible at first glance. Are certain intersections more dangerous than others? Do accidents cluster around specific roads or zones? Using a range of analytical techniques, from simple point counting in grid cells to heatmaps and clustering algorithms, you’ll discover how to turn raw spatial data into actionable insights.

This lab sharpens your technical skills in spatial analysis and challenges you to think critically about the results. Which patterns are real, and which might be artifacts of scale or method? Ultimately, you'll build your ability to choose appropriate spatial analysis tools and interpret their outcomes within a real-world context. Let’s dive in and explore how spatial data can reveal the hidden structure of our environments.

## Learning Objectives

By the end of the lab, you are able to:

* Explore point-based spatial data and interpret key attribute fields (e.g. location, category, time).
* Select and extract data for a specific region of interest using spatial queries and filters.
* Apply and compare different spatial clustering techniques, including:
  * Point count per grid cell
  * Kernel Density Estimation (heatmap)
  * DBSCAN clustering algorithm
* Visualize and interpret spatial clusters and discuss strengths and limitations of each method.
* Evaluate which clustering method is most appropriate for a given question and justify your choice based on data resolution, spatial scale, and analytical goal.

## Task

You’ve just joined the **Urban Insights Unit**, a team of spatial analysts tasked with making cities safer, smarter, and more resilient. Governments, citizen groups, and planning offices regularly approach your unit with pressing questions rooted in real-world challenges. Today’s request just landed in your inbox:

> _**"Where exactly are the danger zones for traffic accidents in our city and what might be causing them?"**_

Your job is to investigate traffic accident patterns in a city of your choice and uncover the spatial logic behind their occurrence. This work is more than academic: municipalities may use your results to improve road safety, redesign intersections, install signage, or adjust traffic patterns. Your findings could save lives.

### Your Mission

You’ll play the role of a local spatial analyst. Choose a place you care about (maybe it’s your hometown, a city you recently visited, or a region you've always found interesting) and apply spatial analysis methods to detect patterns in road accidents.

**Your core objectives:**

* **Identify hotspots**: Where do accidents concentrate, and can we detect meaningful clusters?
* **Compare methods**: Do different analytical techniques reveal the same patterns?
* **Interpret results**: What do these patterns suggest about road safety, infrastructure, or urban layout?

### Your Toolkit

To complete your mission, you’ll work with a variety of spatial analysis tools, including::

* Point grids to count accident occurrences in spatial cells
* Heatmaps to visualize density patterns
* DBSCAN clustering to statistically group spatial events
* Attribute filters to focus on specific types of accidents (e.g. bicycles, pedestrians, night-time incidents)

You'll combine these tools to explore where accidents occur.

### Your Case

Select a city or region in Switzerland (or [a similar dataset abroad](https://hendrik-wulf.gitbook.io/sds-110-fundamentals-of-spatial-data/lab-4-analyzing-spatial-data#resources)) and frame a research question that matters. See some examples below for inspiration:

* Where do most bicycle accidents happen in Bern?
* Which schools in Geneva show clusters of pedestrian accidents nearby?
* Which areas in Lugano see the highest accident density on weekends?

Let curiosity guide you. Your investigation can be broad or focused, depending on what sparks your interest.

### Your Deliverable

As part of your assignment, prepare a short spatial case report that includes:

* [x] Your case question and study area
* [x] Maps of accident distribution and clustering results
* [x] A comparison of methods used
* [x] Your interpretation of what the patterns suggest

Ready? Open QGIS, load your data, and start your spatial investigation. The city is counting on you.

## Workflow

### 1. Find and explore the data

Finding the right dataset is an essential skill. Try to locate the relevant files yourself using the [data portals](lab-3-creating-online-maps.md) we introduced in Lab 3. Navigating these resources builds your ability to source high-quality, authoritative data in real-world projects. That said, if your search turns up empty, you can use the direct download links below:

* Road Traffic Accident Locations – \[[download link](https://data.geo.admin.ch/ch.astra.unfaelle-personenschaeden_alle/unfaelle-personenschaeden_alle/unfaelle-personenschaeden_alle_2056.csv.zip)]
* swissBOUNDARIES3D (tlm\_hoheitsgebiet) – \[[download link](https://data.geo.admin.ch/ch.swisstopo.swissboundaries3d/swissboundaries3d_2025-04/swissboundaries3d_2025-04_2056_5728.gpkg.zip)]

Before diving into any analysis, it’s essential to get familiar with the data you’ll be working with. This step sets the stage for everything that follows.

* **Open QGIS and set up your project**.&#x20;
  * Create a new project and save it to your working directory (e.g., `lab4_analysis.qgz` ).
* **Load the Accident Data**
  * Go to `Layer > Add Layer > Add Delimited Text Layer`.
  * Browse and load to the downloaded dataset: `RoadTrafficAccidentLocations.csv` .
  * Right-click the layer > `Open Attribute Table` to view its structure.
    * This dataset contains point features representing traffic accidents from 2011 in Switzerland. Each record includes accident attributes such as: Time, Type, Severity, Participants (e.g., car, bicycle).
* **Load the Municipality Boundaries**
  * Add the dataset: `swissBOUNDARIES3D_1_5_LV95_LN02.gpkg` to load the layer `tlm_hoheitsgebiet`
  * Take a look at the attributes to get a sense of the content.
* **Add a Basemap**
  * In the Browser Panel, right-click XYZ Tiles > CartoDB Dark Matter / OSM > Add Layer to Project.
  * This will help you visually contextualize your accident data.
* **Style the Accident Data**
  * Open the **Layer Styling Panel**.
  * Set the accident layer Symbology to **Categorized** , using `AccidentType` as the classification field and `Classify`
  * Zoom in and pan across Switzerland to get a feel for where and how accidents are distributed.

{% hint style="success" %}
#### **Explore some patterns**:

* Where do most accidents seem to occur?
* Are there obvious clusters or outliers?
* Do accident types show different spatial trends?
{% endhint %}

<figure><img src=".gitbook/assets/Screenshot 2025-06-05 at 14.38.21.png" alt=""><figcaption><p>Road traffic accidents color-coded by accident type</p></figcaption></figure>

### 2. Define your study area

Before analyzing patterns in road accidents, you need to define the geographic scope of your investigation. For this, you’ll extract only the accidents that occurred within a specific region, Kanton, or municipality. In this case we will focus on the city of Zürich. Depending on your research question, you can apply suitable filters on attributes (e.g. AccidentInvolvingBicycle) or locations (e.g. spatial buffer around bike lanes).

* **Filter the Municipalities Layer**
  * Load the administrative boundaries `tlm_hoheitsgebiet`&#x20;
  * Right-click the `tlm_hoheitsgebiet` layer → **Filter...** to launch the the **Query Builder**&#x20;
  *   In the dialog, build a query like:

      ```
      "name" = 'Zürich'
      ```
  * Click OK. The map will now only show your chosen area.
* **Select Accidents by Location**
  * Go to Vector > Research Tools > Select by Location
    * Input layer: `RoadTrafficAccidentLocations`
    * Selecting features from: your filtered municipality layer
    * Geometric predicate: `intersect`
  * This selects only the accidents within your chosen area.
* **Export Selected Accidents**
  * Right-click on `RoadTrafficAccidentLocations` → `Export > Save Selected Features As...`
  * Save the file to your working directory with a meaningful name (e.g. `Accidents_Zurich.gpkg`).

{% hint style="success" %}
#### **Explore the data**:

* Where do you expect accident hotspots to appear?
  * Think about intersections, busy roads, or areas with mixed traffic (cars, bicycles, pedestrians). Do these expectations match what you see?
* Are the accident points precise?
  * Examine the distribution closely. Do the points seem to align exactly with roads and intersections, or do you notice repeating patterns, grid-like placement, or clusters that hint at aggregated or anonymized data?
{% endhint %}

<figure><img src=".gitbook/assets/Screenshot 2025-06-05 at 14.44.31.png" alt=""><figcaption><p>Road traffic accidents in Zurich.</p></figcaption></figure>

### 3. Grid-based analysis

To better understand the spatial distribution of accidents, we will create a grid over your study area and count the number of accidents within each cell. This approach transforms individual points into an [aggregated spatial pattern](https://h3geo.org/docs/highlights/aggregation/) and helps identify areas of higher or lower accident frequency.

* **Create the Grid**
  * Go to `Vector → Research Tools → Create Grid`.
  * In the dialog:&#x20;
    * set Grid type to `Hexagon (polygon)` for smoother visual patterns.
    * Choose Grid extent → select your study area layer (your municipality).
    * Define a grid spacing (e.g., 50 m, 100 m, 200 m). Try at least two different resolutions to compare outcomes.
    * Set the CRS to `EPSG:2056` (CH1903+ / LV95).
    * Save the output with a meaningful name (e.g., `ZH_grid_100m.gpkg`).
* **Count Accidents in Each Cell**
  * Go to `Vector → Analysis Tools → Count Points in Polygon`.
  * In the dialog:
    * Polygons: Select your grid layer.
    * Points: Select your filtered traffic accident points.
    * Save the result (e.g., `accidents_per_cell_100m.gpkg`).
  * Repeat this step for each grid size you created.
* **Style the Output**
  * Right-click on the output layer `→ Properties → Symbology.`
  * Choose `Graduated` as the style type.
  * Set the column to the point count field (e.g., `NUMPOINTS`).
  * Use a suitable sequential [color ramp](lab-5-modelling-spatial-data.md#add-scientific-color-maps) (e.g., `batlow`) to highlight spatial patterns.
  * Adjust the classification mode (e.g., `Equal Interval` or `Natural Breaks`) to improve readability.

{% hint style="success" %}
#### Reflect and Compare:

* Which grid size reveals patterns most clearly?
* What trade-offs do you observe between small and large cell sizes?
* Are there any areas where the point density seems misleading?
{% endhint %}

> 💡 _Grid-based analysis is great for detecting general patterns but may miss detailed local variation or create artificial borders. Use it as a first exploratory step before diving into more nuanced techniques._

<figure><img src=".gitbook/assets/Screenshot 2025-06-06 at 17.07.49.png" alt=""><figcaption><p>Gird-based analysis with 100 m grid cells</p></figcaption></figure>

### 4. Create a heatmap

A heatmap, or **Kernel Density Estimation (KDE)**, is a powerful method to interpolate point data into a continuous surface. It allows us to visualize where accidents are concentrated without being limited by administrative boundaries or artificial grid cells.

{% hint style="info" %}
#### [**Explainer**](https://youtu.be/DCgPRaIDYXA?si=MhaPOFMwiWRH4ABy)**: What is Kernel Density Estimation (KDE)?**

KDE is a spatial analysis technique used to create a smooth, continuous surface that shows where points are concentrated. Instead of displaying individual data points, KDE estimates the _density_ of events across space, helping reveal hotspots that may not be obvious from the raw points.

The method relies on two key settings:

* **Radius (bandwidth):** Defines how far each point “spreads” its influence.\
  A large radius produces a smooth, generalized surface; a small radius reveals fine-scale hotspots.
* **Kernel function:** Controls how the influence of each point decreases with distance (e.g., Gaussian).\
  Points closer to the center contribute more to density than those farther away.

KDE is excellent for visualizing _intensity patterns_ and understanding where events concentrate, especially when boundaries or discrete clusters are not the focus.

**Watch** [**this video**](https://youtu.be/DCgPRaIDYXA?si=MhaPOFMwiWRH4ABy) **to better understand KDE.**
{% endhint %}

* **Use the Interactive Heatmap Renderer**
  * Select your accident points layer in the Layers Panel.
  * Open the panel on `Layer Styling → Symbology`.
  * In the drop-down at the top, choose `Heatmap`.
  * Set the following options:
    * `Radius:` Start with \~10 m (adjust based on city size and zoom level).
    * `Weight:` Leave empty or select a field like `AccidentInvolvingBicycle` for a filtered map.
    * `Color ramp:`Choose a perceptually clear scheme (e.g., `Spectral` ). Consider inverting it so hotspots appear red.
    * Adjust the transparency (and color spread) to highlight high-density areas.
* **(Optional) Export the Heatmap as a Raster** if you want to keep a static version of your KDE:
  * Go to `Processing Toolbox → Heatmap (Kernel Density Estimation).`
  * Choose your accident layer as Input point layer.
  * Set the Radius (e.g., 100 m) and Pixel size (e.g., 10 m).
  * Choose Output raster file and save it in your working folder.

{% hint style="success" %}
#### Reflect and Compare:

* How does the heatmap differ from the grid-based method?
* Does it reveal more fluid or organic clusters?
* Are there locations with a high density that were not highlighted in the grid?
{% endhint %}

> 💡 _KDE is great for intuitive, smooth visuals of hotspots, but it’s still a statistical estimate. Radius size and color scales can strongly influence interpretation._

<figure><img src=".gitbook/assets/Screenshot 2025-06-06 at 17.32.59.png" alt=""><figcaption><p>Heatmap of bicycle accidents.</p></figcaption></figure>

### 5. Detect clusters using DBSCAN

To go beyond visualizations, we now apply a clustering algorithm to _automatically detect accident hotspots_ based on spatial proximity. DBSCAN (Density-Based Spatial Clustering of Applications with Noise) is a powerful clustering algorithm that groups points into clusters if they are densely packed together, while marking outliers as noise.

{% hint style="info" %}
[**Explainer**](https://www.youtube.com/watch?v=_A9Tq6mGtLI): What is DBSCAN?

DBSCAN is a popular unsupervised clustering algorithm used to identify clusters of data points in space, especially when clusters have arbitrary shapes and when there may be noise or outliers. The density-based clustering approach uses two key parameters:

* **ε (epsilon)**: Maximum distance between two points to be considered part of the same cluster.
* **minPts**: Minimum number of points required to form a cluster.

DBSCAN can detect clusters of arbitrary shape and doesn’t require knowing the number of clusters in advance (as needed for kMeans).\
**Watch** [**this video**](https://www.youtube.com/watch?v=_A9Tq6mGtLI) **to better understand DBSCAN.**
{% endhint %}



* **Open and run the DBSCAN Tool**
  * Go to `View > Panels > Processing Toolbox`.
  * Search for `DBSCAN clustering` and click to open the tool.
  * Set the Parameters and run the tool
    * `Input layer`: Your filtered accident dataset (e.g., accidents in your selected city).
    * `Minimum cluster size`: Start with `30` (adjust depending on point density).
    * `Maximum distance between clustered points`: Try `15` meters (adjust as needed).
      * You may need to experiment with different combinations to get meaningful results.
    * Choose an output path (e.g., `dbscan_clusters_minPts-30_maxDist-15.gpkg`) and click RunDMC.
* **Explore the Output**
  * The result will be a copy of your point layer, but with _two new fields_:
    * `cluster_id`: All points in a cluster share the same ID. The value `NULL` means the point is outside the `minimum size` and `maximum distance` criteria.
    * `cluster_size`: The number of points in each cluster.
  * Apply the `Filter ...` (right-click the layer) `"cluster_id" is not null` to hide points outside your analysis.
  * Style the layer for improved interpretation
    * Use `Graduated` symbology based on `cluster_id` to color-code different clusters.

<figure><img src=".gitbook/assets/Screenshot 2025-06-07 at 16.46.24.png" alt=""><figcaption></figcaption></figure>

* **Visualize Clusters with Polygons**
  * Use the `Minimum Bounding Geometry` tool:
    * Go to `Processing Toolbox` → Search for `“Minimum bounding geometry”`
    * Input layer: The DBSCAN result layer (`dbscan_clusters_minPts-30_maxDist-15.gpkg`)
    * Geometry type: `Convex Hull`
    * Field: `cluster_id`
    * Output: Save to file as e.g., `dbscan_clusters_hulls.gpkg`
  * Join `cluster_size` from the Point Layer to the Polygon Layer
    * Open _Layer Properties_ for `dbscan_cluster_hulls.gpkg` (double click)
    * Go to the _Joins_ tab and click the **+** button
    * Set the parameters:
      * Join layer: your DBSCAN point layer (e.g., `dbscan_clusters_minPts-30_maxDist-15`)
      * Join field: `cluster_id`
      * Target field: `cluster_id`&#x20;

<figure><img src=".gitbook/assets/Screenshot 2025-06-07 at 16.51.20.png" alt="" width="375"><figcaption></figcaption></figure>

* Style the new polygon layer:
  * Use **Categorized** symbology based on the field `cluster_size`
  * This lets you **visually compare the size** of clusters based on how many accidents they contain.

<figure><img src=".gitbook/assets/Screenshot 2025-06-08 at 10.38.41.png" alt=""><figcaption><p>DBSCAN clusters color-coded by the number of accidents within</p></figcaption></figure>

{% hint style="success" %}
#### Reflect and Compare:

* Which accident clusters are detected that were missed by KDE or grid-based analysis?
* Does DBSCAN capture cluster shape and intensity more precisely?
* How does changing the distance or minPts affect the output?
{% endhint %}

> 💡 Tip: You can visualize individual clusters further by changing the stroke color and enabling `Draw effects` like _Outer Glow_

<figure><img src=".gitbook/assets/Screenshot 2025-06-07 at 17.39.28.png" alt=""><figcaption><p>Layer styling effect to highlight bright objects against a dark background</p></figcaption></figure>

### 6. Summarize insights

Now that you've applied **three different spatial analysis methods,** grid-based point counts, heatmaps (KDE), and DBSCAN clustering, it's time to synthesize your findings. Use the following questions to guide your comparison:

**Point Count per Grid Cell**

* What resolution worked best to highlight accident-prone areas?
* Did this method reveal broad trends or specific hotspots?
* Were some high-accident areas missed due to the shape or alignment of the grid?

**Heatmap (Kernel Density Estimation)**

* How does the spatial smoothing compare to the grid?
* Which regions showed up as high-density in the heatmap but not in the other methods?
* Did the KDE surface reveal gradual changes or sharp boundaries?

**DBSCAN Clustering**

* Which clusters overlap with the hotspots detected by the grid or heatmap?
* Are there small, dense clusters that the other methods missed?
* Which parameters worked best to highlight accident-prone areas?

### Optional: The extra mile&#x20;

Completing this optional section can earn you +1 bonus point. Getting familiar with DataPlotly is **highly recommended** as you will need it anyway in Lab 6.

#### Add Non-Spatial Insights with DataPlotly

Spatial clustering shows _where_ accidents happen. But what about _when_, _how_, or _under what conditions_ they occur? In this part of the lab, you’ll explore attribute-based patterns using the _DataPlotly_ plugin to create interactive charts in QGIS.

* **Install and Open DataPlotly**
  * Go to `Plugins → Manage and Install Plugins`\
    Search for **“**_DataPlotly_**”** and install it.
  * Once installed, open it from `View → Panels → DataPlotly`.
* **Choose the Layer and Attributes in the DataPlotly panel**
  * _Layer_: Select your accident layer (filtered to your study area).
  * _Plot Type_: Choose a Histogram chart to start with
  * _X field_: Select a field like:
    * `AccidentHour` , `AccidentWeekDay` , `AccidentMonth` `AccidentYear` to explore changes over time&#x20;
    * `AccidentType_en` or `AccidentSeverityCategory_en` for causes or severity
  * _Manual bin size_: Choose a bin size according to the number of unique entries in that category.

<figure><img src=".gitbook/assets/Screenshot 2025-06-08 at 10.30.13.png" alt=""><figcaption><p>Histogram of hourly and annual road accidents in Zurich</p></figcaption></figure>

> 💡 Tip: Choose other plot types to explore intersections of attributes, e.g.:\
> “At what time are bicycle accidents most common?”

{% hint style="danger" %}
**Spoiler Alert**: A Google Earth Engine account will be required for the next lab 5. Follow [these steps](https://courses.spatialthoughts.com/gee-sign-up.html) to register in advance so you're all set up.
{% endhint %}

## Resources

**Traffic Accident Data**

* Swiss Traffic Accidents (2011–2024) **-** [Opendata.swiss Dataset](https://opendata.swiss/en/dataset/strassenverkehrsunfalle-mit-personenschaden)
* European Traffic Accidents Data - [European data portal](https://data.europa.eu/data/datasets?locale=en\&query=road+accidents\&page=1)
* US Traffic Accidents Data - [Crash Database 2025](https://www.arcgis.com/home/item.html?id=3ef625ffe7ce4ef38be81ee6d80a5385)

**Tools and Tutorials**

* DataPlotly Plugin
  * [Official Plugin Documentation](https://dataplotly-docs.readthedocs.io/en/latest/) and [GitHub Link](https://github.com/ghtmtt/DataPlotly)
* DBSCAN&#x20;
  * [Visualizing DBSCAN](https://www.naftaliharris.com/blog/visualizing-dbscan-clustering/) - Interactive explanation of how the DBSCAN clustering algorithm works
  * [StatQuest explainer](https://youtu.be/RDZUdRSDOok?feature=shared) - Clustering with DBSCAN
  * [QGIS documentation](https://docs.qgis.org/3.40/en/docs/user_manual/processing_algs/qgis/vectoranalysis.html#dbscan-clustering) - DBSCAN
  * [Wikipedia](https://en.wikipedia.org/wiki/DBSCAN) - Density-based spatial clustering of applications with noise
* &#x20;Heatmaps
  * [QGIS documentation](https://docs.qgis.org/3.40/en/docs/user_manual/processing_algs/qgis/interpolation.html#heatmap-kernel-density-estimation) - Heatmaps (kernel density estimation)
  * [Wikipedia](https://en.wikipedia.org/wiki/Kernel_density_estimation) - Kernel density estimation&#x20;

## References

**Road Traffic Accidents – ArcGIS StoryMap:** Interactive visual exploration of crash data

* [Analyzing Crashes – ](https://www.arcgis.com/apps/Cascade/index.html?appid=9a27635635c940539b96fb5ef954e4d5)[Overview](https://www.arcgis.com/apps/Cascade/index.html?appid=9a27635635c940539b96fb5ef954e4d5) & [Workflow](https://desktop.arcgis.com/en/analytics/case-studies/analyzing-crashes-2-pro-workflow.htm)\
  Interactive visual exploration of crash data, showcasing techniques used in spatial analysis.
