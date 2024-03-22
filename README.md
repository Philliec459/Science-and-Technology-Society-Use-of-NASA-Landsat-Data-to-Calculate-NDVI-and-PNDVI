![image](sts_nasa.png)

# Science-and-Technology-Society-Use-of-NASA-Landsat-Data-to-Calculate-NDVI-and-PNDVI
The Science and Technology Society of Sarasota-Manatee Counties use Landsat 9 data to calculate Normalized Difference Vegetative Index (NDVI) from Landsat NIR Band 5 and Red Band 4 as well as our testing of the Panchromatic Normalized Difference Vegetative Index (PNDVI) from Panchromatic Band 8 and NIR Band 4 for a high resolution (15m) NDVI to assess the health of Mangrove Forests in Sarasota Bay, Florida. 

The python code in this repository expects you to have downloaded your Landsat Level 2 processed Bands 4 (red), 5 (NIR) and Level 1 processing Band 8 (Panchromatic) spectral bands. 


# [STS](https://scienceandtechnologysociety.org/) Approach to Map Vegetative Habitats for a Proposed Study Areas in Robinson Preserve.

## Introduction:

In the Mangrove Heart Attach report supplied by Sherri Swanson, the authors give us some interesting methods to identify Mangrove Forests from our Landsat data. They calculated the Normalized Difference Vegetative Index (NDVI) from their Landsat-type data and then used simple NDVI cutoffs to differentiate and map their mangrove habitats. 

We were inspired so we applied some of the techniques mentioned in this report using python to handle the data. We first went to [Earth Explorer](https://earthexplorer.usgs.gov/) to download our Level 1 Landsat data with all the spectral channels, and then applied a Reflectance Correction per the advice from Mike Taylor of NASA. This made a huge difference with all the Landsat Band data corrected to Reflectance. The following is the equation we use to correct the raw Landsat Band data to Reflectance Band data:

    corrected_array = image_array * 0.0000275 - 0.2

The Landsat data is stored as integers and Mike's recommended correction converts the data to [Reflectance](https://www.usgs.gov/faqs/how-do-i-use-a-scale-factor-landsat-level-2-science-products). 

![image.png](attachment:425117b2-41ff-4ebf-9da4-a9f670565c6b.png)

The Landsat data is stored as integer values ranging from 7,273 to 43,636 per USGS documents, or a difference of 36,363 integer values within this range. This gives us a value of 0.0000275 for each integer value within this range. 

    1/36,363 =  0.0000275
    
Where does the -0.2 come from? Since  0.0000275 is the *Reflectance per Integer*, then 

    Apparent Reflectance of Integer 7,237 *  0.0000275 = 0.2
    
and

    Apparent Reflectance of Integer 43626 *  0.0000275 = 1.2
    
To convert our *Apparent Reflectance* to *Reflectance* as an index, we need to subtract 0.2 from the *Apparent Reflectance* and now *Reflectance* is scaled between 0 and 1. Great!

From the Reflectance corrected Band data we calculate NDVI using the following equation:

    NDVI = (near IR - Red)/(near IR + Red) = (Band 5 - Band 4)/(Band 5 + Band 4)

NDVI appears to be a great indicator that provides useful information in making vegetative identifications as suggested by the name. We were able calculated NDVI for the Robinson Preserve area including West Bradenton.  In some of the plots below, most of the warmer colors are actual mangrove habitat estimates. 

In the Mangrove Heart Attack report, they applied simple cutoffs to the NDVI index data to identify their Mangrove habitats, and in this report they went as far as to identify species of Mangroves too. For now, we are satisfied with being able to identify Mangroves in general.  

NDVI calculates an index ranging from our Landsat spectral data from -1 to 3. For our estimate we initiall colored our NDVI image from 0. to 2, where these cutoff values create the surface areas that correlates fairly well to the mapped mangrove locations as reported by [GeoData](https://geodata.myfwc.com/datasets/a78a27e02f9d4a71a3c3357aefc35baf/about). We probably have an 80% solution using this simple technique; and other than some lawns in Bradenton, we are doing pretty well. 

To refine our method, we created a histogram of the NDVI pixels that lie under the mapped GeoData Mangrove Habitats. This allowed us to refine our cutoffs to be from 0.1 to 1.79 and increasing the accuracy of our Mangrove Habitat estimates. 

![image.png](attachment:ece0481e-b8c3-4ebf-ba82-c6398898c236.png)

This is a start, but with a few rules applied (mangrove habitats must be adjacent to salt water, near sea-level elevations  ….) we can narrow down our mangrove habitat picks even more. In addition, we can reduce a lot of the false positives by requiring a certain number of pixels required in each habitat cluster. 

## These are the GeoData mapped Mangrove Habitats for our Study Area:

Using our cutoff estimations from 0.1 to 1.79, our Mangrove Habitats correlate fairly well to the mapped Mangrove Habitats as reported by [GeoData](https://geodata.myfwc.com/datasets/a78a27e02f9d4a71a3c3357aefc35baf/about) as shown in the bottom image below. 

![image.png](attachment:918dde58-f7fe-477c-a4fc-fa87f9ab1e1f.png)

The key to our STS study will be our on-site station studies that will allow us to confirm actual mangrove habitats from other species, rule out any false positives and obtain hard spectral data by species. We might even refine the GeoData picks. It appears that they might be using a similar method to identify mangrove habitats as to what we are doing, but we will be on-site to verify our picks in the end. 

Will any of this be publishable? There is probably much more to the study that we will better understand as the project proceeds, but just maybe we will have something worth writing about in at least a **Medium** article: Citizen Science Group Sponsored by [STS](https://scienceandtechnologysociety.org/) Helps in the Maintaining and Restoration of our Local Wetlands in Bradenton County, FL.

## Calculate *Normalized Difference Vegetation Index (NDVI)* from spectral Landsat data:

    NDVI = (Band 5 - Band 4)/(Band 5 + Band 4) 

## Calculate *Panchromatic Normalized Difference Vegetation Index (PNDVI)* from spectral Landsat data:

    PNDVI = (Band 5 - Band 8)/(Band 5 + Band 8) 

where Band 4 is the Red Spectral Band, Band 5 is the Near IR band and Band 8 is the Panchromatic band with increased resolution. For PNDVI we resolution match the Near IR Band 5 at 30m resolution to Band 8 at 15m resolution for a PNDVI that has higher resolution than NDVI. NDVI is the more traditional approach, but we are testing PNDVI to see if this might provide a similar product to NDVI, but with increased resolution. 

We typically download the Level 2 processed Bands 4 and 5, but Band 8 only comes with the Level 1 processing data. At this point we are using Level 2 Bands 4 and 5 in combination with Band 8 Level 1 data in our processing. 


**We always create histograms of NDVI and PNDVI to ensure that they have a range between -2.0 to around 2.0 to screen out any noise and .**

---
---
