---
description: Generating Relative Elevation Models of Floodplains
icon: water
---

# Lab 5 - Modelling Spatial Data

<figure><img src=".gitbook/assets/Screenshot 2025-12-26 at 11.16.18.png" alt=""><figcaption><p>Relative Elevation Model (30 m) showing meandering scars of the Ucayali River, Peru</p></figcaption></figure>

## Introduction

Elevation data doesn’t just tell us how high a place is, it tells a story about how landscapes are shaped, how rivers move, and where water might return. In this lab, we step into the low-lying world of floodplains and river corridors, where even subtle changes in elevation matter. Our goal: to build a _Relative Elevation Model (REM)_ that visualizes how the terrain rises above the river’s surface, revealing features like abandoned channels, natural levees, and hidden flood pathways.

Through this workflow, you'll learn how to extract river networks from digital elevation models (DEMs), generate elevation samples along the river, interpolate a continuous water surface, and subtract it from the terrain. The result is a striking visualization of floodplain topography, one that can inform flood risk analysis, habitat mapping, and restoration planning.

This lab strengthens your geospatial data processing and terrain analysis skills, and invites you to think like a river: Where has it been? Where might it go next? Let’s find out.

## Learning Objectives

By the end of the lab, you are able to:

* Acquire and visualize high-resolution DEM data from global, national, or regional sources.
* Extract elevation values from the DEM using sampling points along a river network
* Interpolate a smooth water surface using three methods (TIN, IDW and Spline)&#x20;
* Create, style and interpret a Relative Elevation Model (REM)&#x20;
* Reflect on the implications of your results for floodplain mapping, river restoration, or landscape analysis.

## Task

You’ve just joined the Floodplain Intelligence Unit, a team of spatial analysts, hydrologists, and terrain detectives who get called in when flood risks are underestimated and river plans need sharper eyes. Your first assignment is in:

> _“Where is the land still shaped by the river, even if the water isn’t there anymore?”_

Using high-resolution elevation data and your QGIS skills, your job is to reveal the subtle topography of floodplains: where rivers once flowed, where they might flow again, and which areas quietly wait for the next flood. To do this, you’ll build a _Relative Elevation Model (REM),_ a map that doesn't just show elevation, but elevation _relative to the river surface_.

REMs are used in floodplain restoration, wetland protection, and hazard mapping. And let’s be honest, watching floodplain features emerge from a grayscale DEM is pretty satisfying.&#x20;

### **Your Mission**

Create a Relative Elevation Model (REM) for a river system of your choice. This model will help visualize how the terrain rises above the active channel and highlight geomorphic features such as:

* Abandoned meanders and oxbows
* Natural levees and floodplain terraces
* Point bars along meander bends
* Subtle topographic lows that may flood again

You’re free to choose your study area, from wide alluvial valleys in China to dynamic lowland rivers in the Amazon or anywhere else floodplains exist. As long as it has a defined river and some floodplain, it’s fair game.

### **Your Toolkit**

To complete your mission, you’ll build a spatial analysis workflow:

* **Download a high-resolution DEM** using Google Earth Engine, USGS 3DEP, or similar
* **Visualize the DEM** using color ramps, hillshading, and blending techniques
* **Extract the river network** using hydrological modeling tools
* **Sample network elevation points** using the QChainage and Point Sampling Tool plugins
* **Interpolate a river surface** using TIN, IDW and Spline methods
* **Create the REM** by subtracting the interpolated surface from the DEM
* **Style the REM** to make floodplain features pop

There’s a lot to do, but don’t worry, you’ve got this. And remember: the river doesn’t forget. It just waits.

### **Your Deliverable**

Your submission should include:

* [x] A clearly styled REM map using an suitable color ramp
* [x] A short description of your study area and workflow
* [x] A reflection comparing TIN vs. IDW vs. Spline interpolation: which method better captured the river surface in your case, and why?
* [x] Annotate any interesting floodplain features revealed in your REM (e.g., relict channels, terraces)

This lab strengthens your skills in hydrological terrain modeling, interpolation, and cartographic interpretation; all key abilities for anyone working in environmental analysis, water management, or spatial storytelling.

## Workflow

In this lab, you will create a Relative Elevation Model (REM) to reveal how the terrain rises above the surface of a river. This is an effective technique for studying floodplains, terraces, and historic river dynamics. The workflow begins with downloading a high-resolution DEM. After visualizing the terrain with enhanced color ramps and hillshading, you will extract the river network and sample elevation values along it. These elevation samples are then used to interpolate a smooth water surface. Finally, by subtracting this interpolated surface from the original DEM, you create and style the REM, producing a map that highlights subtle variations in relative elevation across the floodplain.

### 1. Download the DEM

To create a Relative Elevation Model (REM), you first need a high-quality Digital Elevation Model (DEM) **of a floodplain**. This choice is critical, as all subsequent steps and the interpretability of your REM depend on the quality and suitability of the DEM.

Visualize and style your DEM. Is it a wide, natural floodplain? Are river channels, levees, and abandoned channels likely visible? This helps you anticipate which fluvial features your REM may reveal.

Below are three options with different levels of flexibility and suitability.

#### **Option 1: Download a DEM using the Google Earth Engine (recommended)**

This option offers maximum flexibility and is ideal for global study areas. Once you [registered](https://courses.spatialthoughts.com/gee-sign-up.html) to the Earth Engine [this script](https://code.earthengine.google.com/80fdd99c3b758f7b7c28396a1db10b7f) (explained in [this video](https://youtu.be/d-SGrEAs7rU)) allows you to:

* Define a custom _Area of Interest (AOI)_
* Select from various global and national high-resolution DEM sources (0.5 - 30 m spatial resolution)
* Export the DEM to your _Google Drive_ in GeoTIFF format
* The DEM is projected to local UTM, **scaled by a factor of 100** (for file size efficiency), and exported as a `uint16` raster. \
  This method is suitable for any region on Earth.

#### **Option 2: Download a DEM from the US using the USGS 3DEP Lidar Explorer**

If you prefer a _study area in the United States_, the USGS 3DEP Lidar Explorer provides access to 1-meter resolution lidar-derived DEMs.

* Use the [interactive map](https://apps.nationalmap.gov/lidar-explorer/#/) to locate and define your area of interest
* Download relevant DEM tiles as GeoTIFFs
* Merge the tiles into a single raster using QGIS\
  This method follows [Daniel Coe’s tutorial](https://dancoecarto.com/downloading-and-preparing-lidar-dems-for-rem-processing).

#### **Option 3: Download a DEM from Switzerland using swisstopo**

Switzerland is not ideal for large, natural floodplains due to extensive human modification. However, you may still explore smaller or more constrained river systems.

* use the federal [map viewer](https://map.geo.admin.ch/#/map?lang=en\&center=2657107.38,1184572.92\&z=2.272\&topic=ech\&layers=ch.bafu.bundesinventare-auen,,1;ch.bafu.bundesinventare-auen_anhang2,,1\&bgLayer=ch.swisstopo.swissimage) to locate suitable floodplains
* download respective tiles of the [swissALTI3D](https://www.swisstopo.admin.ch/en/height-model-swissalti3d) digital elevation model
  * best to use the GeoTiff format in a 2 m spatial resolution

{% hint style="info" %}
💡 **Tip**: If you are unsure where to find a suitable floodplain, Option 1 (Arctic or Amazon) is your safest and most reliable choice.
{% endhint %}

### 2. Visualize the DEM

Once you've downloaded and prepared your DEM, the next step is to explore and enhance its visual appearance to better understand the terrain and river landscape.

#### **Load and Inspect the DEM**

* Load your DEM into QGIS.
* Use the `Identify Features` tool (<img src=".gitbook/assets/Screenshot 2025-06-12 at 16.16.40 (1).png" alt="" data-size="line">) to click along your river and note the range of elevation values (e.g., upstream vs. downstream).

#### **Correct DEM Elevation Values (Scaling)**&#x20;

DEMs downloaded via Google Earth Engine were exported with elevation values scaled by a factor of 100 to reduce file size. Before further analysis, we should convert the values back to meters.

* Open **Raster → Raster Calculator**
* Use the following expression: `"DEM@1" / 100`  (replace `DEM` with the name of your raster layer)
* Set the Output raster name (e.g. DEM\_meters) and save the output as a GeoTIFF

#### **Apply Color Visualization**

* Open the `Layer Styling` panel for the DEM and go to the `Symbology` tab.
* Set the `Render type` to _Singleband pseudocolor_.
* Enter a suitable _Min/Max value range_ **or** choose the `Cumulative count cut`.
* Choose a color ramp that clearly displays terrain differences. A smooth gradient from dark to light, like the `batlow` color map, often works well. But first need to add it (see next step).

<figure><img src=".gitbook/assets/Screenshot 2025-12-27 at 13.08.55.png" alt="" width="375"><figcaption><p>Suitable Layer Styling Settings</p></figcaption></figure>

#### **Add Scientific Color Maps**&#x20;

For very effective and perceptually accurate visualizations:

* Download [Scientific Colour Maps](https://www.fabiocrameri.ch/colourmaps/) by Fabio Crameri from [Zenodo](https://zenodo.org/records/8409685).
* In QGIS, go to `Settings > Style Manager`.
* In the Style Manager, click `Import` (bottom left <img src=".gitbook/assets/Screenshot 2025-06-13 at 10.47.13.png" alt="" data-size="line">) and select the `*_QGIS.xml` file you downloaded.
* The new color ramps will now appear in the `Singleband pseudocolor > Color ramp` options.

#### **Enhance with Hillshade**

To highlight terrain features:

* _Duplicate_ your DEM layer (right-click option) and apply _Hillshade_ as the render type in `Layer Styling` panel.
* Set the _Z-factor_ (e.g., 10) to exaggerate elevation changes&#x20;
* Optional: Check if _Multidirectional_ results in better relief shading.
* Under the _Resampling_ section, set both zoom-in and zoom-out modes to _bilinear_.

#### **Blend Layers for Best Effect**

* Place the hillshade layer _above the color DEM_ in the `Layers` panel.
* In the `Symbology > Layer Rendering` section, try different _blending modes_ (e.g. Overlay) to create an intuitive and visually rich terrain map.

<figure><img src=".gitbook/assets/Screenshot 2025-12-27 at 13.08.14.png" alt=""><figcaption><p>DEM (5 m) of the Kuskokwim River (Alaska), colour-coded using the BatlowW colour ramp and a hillshade overlay.</p></figcaption></figure>

### 3. Extract the river network

In this step, you’ll use the DEM to _automatically extract the river network_ using hydrological modeling tools in QGIS. Here, you extract the stream network as a raster layer, convert it to polygons and then to lines to obtain the vector format needed for elevation sampling along the river network (next section).

#### **Extract Stream Raster**&#x20;

Use the _GRASS tool_ **`r.stream.extract`** from the _Processing Toolbox_:

* Open the tool from:\
  `Processing Toolbox > GRASS > r.stream.extract`
* In the dialog:
  * Elevation map: your DEM (e.g., `DEM_Ucayali.tif`)
  * Minimum flow accumulation for streams: set to `100000` (adjust this value to change the density of your river network) &#x20;
  * Leave other parameters at default, but save the output file:
    * `Unique stream ids (rast)`
    * _Skip the other outputs_ to speed up the process
  * Click `Run`\
    The processing may take a few minutes depending on DEM size and threshold settings.

#### **Convert Stream Raster to Vector Polygons**

Once the stream raster is generated:

* Go to: `Raster > Conversion > Polygonize (Raster to Vector)`
* Select the _stream raster_ output from `r.stream.extract` as input
* Use the default settings
* Save the resulting _polygon layer_ as a GeoPackage
* _Zoom in_ and select features to inspect the quality and structure of the extracted stream network

<figure><img src=".gitbook/assets/Screenshot 2025-12-27 at 13.30.59.png" alt=""><figcaption><p>Subset of the river network polygon (blue) of the study area</p></figcaption></figure>

#### **Convert Polygons to Lines**

To convert your river network to line features:

* Go to: `Vector > Geometry Tools > Polygon to Lines`
* Input: the _polygon layer_ from the previous step
* Output: a new _vector line layer_ representing your river network

Notice the large number of line segments. Each segment encapsulates _4-connected pixels_ of the river network.

<figure><img src=".gitbook/assets/Screenshot 2025-12-27 at 13.34.28.png" alt=""><figcaption><p>A close-up of the line segments of the river network. All "rook-connected" pixels form one segment.</p></figcaption></figure>

#### **Dissolve Segments to a Network**

To combine the multiple individual line segments into one coherent river network:

* Go to: `Vector > Geoprocessing Tools > Dissolve`
* Input: the _line layer_ from the previous step
* Output: a dissolved _vector line layer_ representing your river network

<figure><img src=".gitbook/assets/Screenshot 2025-12-27 at 14.05.52.png" alt=""><figcaption><p>River network vector lines (yellow) overlaid on the color-coded DEM</p></figcaption></figure>

### 4. Sample river elevations

In this step, you’ll create evenly spaced points along your river line and sample the underlying DEM to extract elevation values at each point. This data will later be used to interpolate a water surface for generating the Relative Elevation Model (REM).

#### **Generate Points Along the River**

* First, install the _QChainage plugin_:\
  Go to `Plugins > Manage and Install Plugins`, search for _QChainage_, and install it.
* Use the `Measure Line` tool (<img src=".gitbook/assets/Screenshot 2025-06-13 at 14.20.50.png" alt="" data-size="line">) to estimate a suitable _sampling interval_ (e.g., 100 m).
* Open the QChainage plugin from the Plugins Toolbar (<img src=".gitbook/assets/Screenshot 2025-06-13 at 14.20.59.png" alt="" data-size="line">)\
  `View > Toolbars > Plugins Toolbar` (just ensure it is checked)
* Input:
  * **Line layer**: your digitized river line
  * **Chainage every**: the _sampling interval_
  * **Output Layername**: define a meaningful name

This will create regularly spaced points along the river network.

#### **Sample DEM Elevations**&#x20;

* Install the _Point Sampling Tool_:\
  Go to `Plugins > Manage and Install Plugins`, search for _Point Sampling Tool_, and install it.
* Select the DEM and open the Point Sampling Tool from the Plugins Toolbar (<img src=".gitbook/assets/Screenshot 2025-06-13 at 14.34.03.png" alt="" data-size="line">):\
  `Plugins > Analyses > Point Sampling Tool`
* Input:
  * _Layer containing sampling points_: the output from QChainage
  * _Layer with values to sample_: your DEM layer
  * _Output_: save the resulting point layer to your working directory
* The output point layer will now include an elevation value at each river point.

#### **Limit the Elevation Data to the Floodplain**

* Visualize Elevation Values
  * Go to the _Symbology_ tab _Layer Styling_ panel
  * Set _Render type_ to _Graduated_
  * Select the elevation field as the Value
  * Choose a meaningful color ramp (e.g., batlow)
  * Click _Classify_ and _Apply_
  * Adjust value ranges (e.g. 0) and un/check symbols (higher elevations) to explore suitable filter thresholds
* Use _attribute filtering_ in the attribute table or QGIS expression editor to:
  * Remove points with elevation values of 0 (often caused by no-data values)
  * Exclude points that are upstream outside the main river bed
    * Example expression: "elevation" > 0 AND "elevation" < 20
* Export the filtered point layer as a new file to use in the next interpolation step.

<figure><img src=".gitbook/assets/Screenshot 2025-12-27 at 14.14.14 (1).png" alt=""><figcaption><p>Visualizing different point sample elevation ranges (highlighted eye symbol) overlaid on the hillshade image</p></figcaption></figure>

### 5. Interpolate the water surface

In this section, you will use the cleaned river elevation points to generate a continuous water surface that follows the river’s flow. This interpolated surface will later be subtracted from the original DEM to create the Relative Elevation Model (REM).

In the Processing Toolbox, the following common interpolation method options are available: Inverse Distance Weighting (IDW) and Triangulated Irregular Network (TIN). With the SAGA Next Gen plugin, you can also explore Splines and Kriging (however, Kriging has long computational times).

_Inverse Distance Weighting (IDW)_

* A simple method that assumes points closer to each other have more similar values.
* Tends to produce smooth surfaces, but can struggle with sharp breaks in terrain or irregular point spacing.

_Triangulated Irregular Network (TIN)_

* Creates a surface by connecting points into non-overlapping triangles.
* Preserves local variations and edges, but may produce a more angular surface.

_Spline (Multilevel B-Spline)_

* Fits a smooth surface through the input points using spline functions.
* Captures gradual elevation changes well, but may slightly smooth small-scale floodplain features.

#### Installing SAGA Tools in QGIS (needed for Spline interpolation)

Since QGIS 3.30, SAGA tools are no longer included by default.\
To use SAGA-based tools (e.g. spline interpolation, terrain analysis), you must follow these steps:

{% stepper %}
{% step %}
**Install the&#x20;**_**Processing SAGA NextGen Provider**_**&#x20;plugin**

* Go to Plugins → Manage and Install Plugins
* Search for “Processing SAGA NextGen Provider”
* Install the plugin
{% endstep %}

{% step %}
**Download SAGA GIS**

* Visit: [https://sourceforge.net/projects/saga-gis/files/](https://sourceforge.net/projects/saga-gis/files/)
* Download SAGA the latest version (9.11 or newer) for your operating system
* Unzip the downloaded folder to a location you can easily find (e.g. `Documents/GIS/SAGA/`)
{% endstep %}

{% step %}
**Link SAGA to QGIS**

1. In QGIS, go to Settings → Options
2. Open the Processing tab
3. Select Providers
4. Expand SAGANG (SAGA NextGen)
5. Set the SAGA folder path to the unzipped SAGA directory\
   (the folder containing the SAGA binaries: win: `saga_cmd.exe`, mac: `saga_cmd`)

<figure><img src=".gitbook/assets/Screenshot 2025-12-26 at 21.52.45 (1).png" alt=""><figcaption><p>Example of a SAGA folder path on a Mac: <code>your_path/SAGA.app/contents/MacOS</code> </p></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2025-12-26 at 13.08.07.png" alt=""><figcaption><p>Example of a SAGA folder path on Windows: <code>your_path/saga-9.11.0_msw/saga-9.11.0_msw</code><br><br><br></p></figcaption></figure>

Click OK and restart QGIS (if needed).

<details>

<summary><strong>macOS: Allow QGIS to Access SAGA</strong></summary>

On macOS, Apple uses a security system called Gatekeeper.\
Because SAGA GIS is an open-source scientific tool and not signed by an “identified Apple developer”, macOS may block it by default.

You may see this warning:

> **“Apple could not verify ‘SAGA.app’ is free of malware.”**

<figure><img src=".gitbook/assets/Screenshot 2025-12-24 at 09.30.26.png" alt="" width="248"><figcaption></figcaption></figure>

This does not mean the software is unsafe. It simply means Apple has not certified it.

To allow SAGA on macOS follow these steps:

1. Double-click `SAGA.app` once
   * The warning message appears
   * Click **Done**
2. Open Apple menu → System Settings
3. Go to Privacy & Security
4. Scroll down to the Security section
5.  You should see a message like:

    > “SAGA.app was blocked from use because it is not from an identified developer.”
6. Click Open Anyway
7. Confirm by clicking Open
8. Enter your administrator password if requested

</details>
{% endstep %}
{% endstepper %}

#### **Run the Interpolation Methods**

Use **TIN** and **IDW** with the following general settings:

* _Input point layer_: your filtered elevation point layer
* _Interpolation attribute_: the field containing the elevation values
* Add the chosen inputs to the interpolation by clicking the green + symbol (<img src=".gitbook/assets/Screenshot 2025-06-13 at 16.10.52.png" alt="" data-size="line">)
* _Distance coefficient P:_ For IDW, test different settings next to the default value 2.
* _Extent_: calculate from your original DEM layer
* _Pixel size:_ Original; For IDW you can use a coarser resolution (e.g., 100 m) to speed up processing
* _Output raster_: save the .tif file to your working directory

Use **Multilevel B-Spline** with the following general settings:

* _Points_: your filtered elevation point layer
* _Attribute_: the field containing the elevation values
* _Extent_: calculate from your original DEM layer
* _Cellsize:_ Original spatial resolution of the DEM
* Maximum Level: Explore different levels (min:1, max: 14) with different smoothing levels
* _Target Grid_: save the .sdat file to your working directory

{% hint style="success" %}
**Reflection**: Based on your visual comparison and the spatial structure of the river, which interpolation method (TIN, IDW or Spline) produced a more realistic and continuous water surface for your river section?\
Consider smoothness, continuity, artifacts, and how well the surface follows the river’s flow.
{% endhint %}

#### **Resample the IDW Output (Optional)**

If you chose a coarse pixel size (e.g., 100 m) to speed up processing:

* Go to `Raster > Projections > Warp (Reproject)`
* Resample the interpolated surface to match your original DEM resolution (e.g., 5 m)
* Set the _resampling method_ to `Bilinear`
* Save the output with a clear name (e.g., `Water_surface_IDW_10m.tif`)

<figure><img src=".gitbook/assets/Screenshot 2025-12-27 at 14.53.40.png" alt=""><figcaption><p>Hillshade image of the water surface interpolation. Which interpolation method was applied here?</p></figcaption></figure>

### 6. Create and style the REM

Now that you have both your original DEM and the interpolated water surface, you can create the Relative Elevation Model (REM). The REM reveals how the landscape rises above the river, allowing you to visualize features like floodplains, terraces, and former river channels with striking clarity.

#### **Subtract Water Surface from DEM**

* Go to `Raster > Raster Calculator`
*   Create a simple subtraction expression:

    ```
    "DEM" - "Water_Surface"
    ```

    * Replace `"DEM"` and `"Water_Surface"` with the actual names of your original DEM and interpolated surface layers.
* This calculates the relative height above the river surface for each pixel.
* Set the output file location and name (e.g., `REM.tif`)
* Choose _GeoTIFF_ as the format and confirm that the _CRS_ matches your project
* Click _OK_ to generate the REM

{% hint style="success" %}
**Reflection:** Inspect the value range of the output raster.

* How did the value range change compared to the original DEM?
* Why are negative values, zero values, or small positive values expected in the REM?
* What do high and low REM values represent in relation to the river channel and floodplain?
{% endhint %}

#### **Style the REM**

* In the _Layer Styling_ panel Set the render type to _Singleband pseudocolor_
* Test some _monochromatic color ramps_ (e.g., `batlow, devon, lajolla, davos`) and their inversions
* Set the _Min / Max values_ manually to highlight fine-scale terrain variation
* [Enhance with a hillshade](https://hendrik-wulf.gitbook.io/sds-110-fundamentals-of-spatial-data/lab-5-modelling-spatial-data#enhance-with-hillshade) and [blend layers](https://hendrik-wulf.gitbook.io/sds-110-fundamentals-of-spatial-data/lab-5-modelling-spatial-data#blend-layers-for-best-effect) (see above).
* Use the _Identify Features_ tool (<img src=".gitbook/assets/Screenshot 2025-06-12 at 16.16.40 (2).png" alt="" data-size="line">) to explore relative elevation values in oxbows, levees, and terraces

<figure><img src=".gitbook/assets/Screenshot 2025-12-27 at 15.05.56.png" alt=""><figcaption><p>Color-coded relative elevation model of the Kuskokwim river.</p></figcaption></figure>

#### **Outcome**

The styled REM provides a powerful visualization of the landscape as seen from the river’s perspective, _elevation values now represent height above the water surface_, not above sea level. This helps reveal geomorphological features critical for flood studies, habitat assessments, and historical river analysis.

### Optional: The extra mile

Curious to push your workflow even further? For those ready to go beyond QGIS and dive into Python-based automation, this bonus challenge is for you.

Explore the [**RiverREM Python package**](https://github.com/OpenTopography/RiverREM)**,** a powerful tool that creates high-resolution relative elevation models with nothing but a DEM and an internet connection. It automatically fetches river centerlines from OpenStreetMap, samples elevations intelligently based on river sinuosity, and generates smooth, artifact-free REM visualizations with just a few lines of code.

This is your chance to compare your manual REM with a fully automated one and reflect on the strengths, weaknesses, and flexibility of each approach.

Read more about the science and inspiration behind REMs and the RiverREM project in this [**OpenTopography blog post**](https://opentopography.org/blog/new-package-automates-river-relative-elevation-model-rem-generation).

Whether you’re learning Python, expanding your geospatial toolbox, or just chasing beautiful visualizations, this extra step is worth exploring!

If you’re new to Python or encounter installation issues, you can explore this ready-to-use [**Colab notebook**](https://colab.research.google.com/github/HendrikWulf/SDS110_Lab-5/blob/main/SDS110_Lab_5_RiverREM_Colab_v2.ipynb) instead.

## Resources

**Digital Elevation Models (DEMs)**\
[Google Earth Engine DEM Catalog](https://developers.google.com/earth-engine/datasets/tags/dem) – Global collection of elevation datasets.\
[OpenTopography Portal](https://portal.opentopography.org/datasets) – High-resolution topographic data from sources around the world.\
[SwissALTI3D](https://www.swisstopo.admin.ch/en/height-model-swissalti3d) – National elevation model of Switzerland.

**REM Inspiration and Tutorials**\
[Daniel Coe’s REM Presentation (PDF)](https://www.dnr.wa.gov/publications/ger_presentations_dmt_2016_coe.pdf) – It brought REMs to life for the cartographic community.\
[Creating REMs in QGIS](https://dancoecarto.com/creating-rems-in-qgis-the-idw-method) – Daniel Coe’s hands-on blog tutorial for making beautiful REMs in QGIS.\
[OpenTopography Blog: Automating REM Creation](https://opentopography.org/blog/new-package-automates-river-relative-elevation-model-rem-generation) – Overview of the Python-based RiverREM tool\
[RiverREM GitHub Repository](https://github.com/OpenTopography/RiverREM) – Python package for automatic REM creation

**Cartographic Styling**\
[Scientific Colour Maps by Fabio Crameri](https://www.fabiocrameri.ch/colourmaps/) – Perceptually uniform, colorblind-friendly color maps&#x20;

**QGIS Tools and Plugins**\
[QChainage Plugin](https://plugins.qgis.org/plugins/qchainage/) – Generate equidistant points along a line (used for sampling river elevations).\
[Point Sampling Tool](https://plugins.qgis.org/plugins/pointsamplingtool/) – Sample raster values at point locations and store results as attributes.\
[QGIS Documentation](https://docs.qgis.org/3.40/en/docs/index.html) – Official user guide and manuals to support your workflow.

## References

Coe, D. (2019) _Tutorials_, Daniel Coe Cartography. Available at: [https://dancoecarto.com/tutorials](https://dancoecarto.com/tutorials) (Accessed: 10 June 2025).
