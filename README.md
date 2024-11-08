# Riverbed DTM Toolkit

QGIS Processing tools for the creation of a complete river Digital Terrain Model (DTM) and for rectifying the riverbed to enable the extraction of features for morphodynamic analysis.

## Description

This plugin enables the creation of a river Digital Terrain Model (DTM), starting from remote sensing data. By integrating these informations, it produces a final representation that encompasses both the emerged and submerged areas of the river. The resulting digital model can be utilized to extract channel features, valuable for morphodynamic evaluations.

Additionally, the plugin includes an algorithm for rectifying channel classification using the centerline, facilitating temporal analyses.

## Uploaded content
In the uploaded directory you are going to find a .zip folder, carrying all needed files to get the plugin running on your QGIS installation. 

## Installation

Download the .zip folder and install it in your QGIS software. You can follow two alternative paths: 

### Install from ZIP File

Start QGIS on your computer. In the top menu, click on **Plugins** and then on **Manage and Install Plugins...**. A window will open, allowing you to manage QGIS plugins.
In the Plugin Manager window, at the bottom left, click on **Install from ZIP**. A new window will open, prompting you to select the ZIP file of the plugin.
Click on **Browse** (**...**) and find the ZIP file you downloaded. Once you’ve selected the ZIP file, click on **Open** and then **Install Plugin**.
After installation, the plugin should appear in the list of installed plugins. If needed, check the box next to the plugin's name to activate it.
Now the plugin is installed and activated: you can find it in the Processing Toolbox.

### Copy the content in the QGIS plugin folder

Start QGIS on your computer. In the top menu, click on **Settings**>**User Profiles**>**Open Active Profile Folder**. Your QGIS installation directory opens up and you can click on the folder **python** and then on the subfolder **plugins**.
You can copy or extract the content of the downloaded .zip at this position. The plugin is installed.
Restart QGIS. The plugin should appear in the in the Processing Toolbox. If it doesn't, in the top menu, click on **Plugins** and then on **Manage and Install Plugins...**. On the left click on **Installed**, find the plugin and check the box next to the plugin's name to activate it.
Now it shows in the Processing Toolbox.


## Instructions

This QGIS plugin includes two algorithms. The first one (***DTM creator from remote sensing data***) is designed to integrate remote sensing data, creating a complete DTM.
This algorithm uses a series of input data from remote sensing sources to integrate them into a single Digital Terrain Model (DTM) that includes both the exposed and submerged portions of a riverbed. To make it work, the following inputs are required:

- A raster with bathymetry values set to positive (e.g., if the depth is 2m, it should be indicated as 2m and not -2m).
- A classification with three classes (Sand, Vegetation, Water) in raster format.
- The same classification also in vector format.
- A mask of the riverbed, represented as a polygon vector encompassing the riverbed from bank to bank (continuous, without interruptions from bridges or similar).
- A centerline of the river.
- A DSM from photogrammetry in raster format.

Additionally, the following parameters must be set:

- `Filtering value for final output`: The DTM calculation is based on creating a sloped plane that follows the elevation increase of the riverbed from the mouth to the source. This plane is an approximation, and the resulting surface is filtered using the SAGA Simple Filter [Filter= 'Smooth', Kernel Type= 'Circle']. The value entered corresponds to the filter radius, which defaults to **30 meters**. This value does not refer to filtering of the final DTM.
- `Vegetation buffer distance`: To find points below vegetation, adjacent water and sand points are used. These points are identified within a buffer, the extent of which can be specified with this value. By default, this distance is set to **1 meter** from the vegetation boundary.
- `Water buffer distance`: To create the sloped plane for depth grading, DSM points immediately adjacent to water are used. These are identified within an immediately adjacent buffer area. The extent can be modified, with the default value set to **0.5 meters**.

The final result consists of:

- The final DTM (if not saved to a file, the default output name is 'Clipped Mask').
- The inclined plane representing the elevation increase from mouth to source due to topography, raw and without smoothing by the Simple Filter (if not saved to a file, the default output name is 'Rasterized').
- The filtered inclined plane, actually used to dimension the bathymetry (if not saved to a file, the default output name is 'Filtered Grid').
- A vector with interpolated point values beneath the vegetation (if not saved to a file, the default output name is 'Sampled').


The second algorithm (***River Classification Rectifier***) leverages river classification and centerline data to generate a vector layer that can be exported as .csv format. This file allows the creation of a rectified channel visualization of classifications, making it easier to conduct temporal and migration analyses of the classes.

The following inputs are required:

- Polygon vector layers that include **sand**, **water**, and **vegetation** (optional if only water and sand classes are desired in the final result). 
- Points along the river centerline created using the QGIS plugin [***QChainage***](https://plugins.qgis.org/plugins/qchainage/), with small variable spacing depending on the working scale (in this case, rasters have 1 m pixels, and points are spaced at 0.1 m). The 'cngmeters' column in these points represents the metric progression of each point. 
- A mask is required to identify the riverbed area, creating a continuous surface without interruptions from structures like bridges. 
- A line representing the watercourse centerline.

When choosing output destinations, it’s recommended to set each output to `CREATE TEMPORARY LAYER` and only save specific layers to disk as needed. Some outputs are labeled **'NO_Inversion**,' indicating the dataset is processed correctly using the standard right-left convention. However, if the final result does not follow this convention (typically due to creation issues with the point layers), you can use the outputs labeled **'WITH_INVERSION.'** To ensure all needed data is saved, you may choose to set destinations for all outputs and then retain only the useful ones.

The final result consists of two point vectors to export as .csv files for creating a plot, where the **'value'** column corresponds to the class, the **'cngmeters'** column represents the x-axis, and the **'distance'** column represents the y-axis. Be mindful of the negative sign on y-values, which allows to distinguish between the right and left sides of the points.


## License

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License.
This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. 
See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License along with this program.  If not, see https://www.gnu.org/licenses/.

If you need to contact us: matteo.bozzano at edu.unige.it, nicoletta.tambroni at unige.it, bianca.federici at unige.it

Created on Fri Nov 4th 2024

@authors: Matteo Bozzano, Nicoletta Tambroni, Bianca Federici

(C) 2024 by Bozzano - University of Genova









