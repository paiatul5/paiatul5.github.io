---
layout: post
title: Analysis of EV Trends in State of Washington
image: "/posts/credit_card.png"
tags: [Data Analysis, Vizualization, Seaborn, Geopandas, Python]
---

# Analysis of Electric Vehicle Adoption in State of Washington


Please refer to the github repository for notebook [GitHub](https://github.com/paiatul5/credit_card_delinquency)

This project is based on Electric Vehicles Data published on Government of Washington's website: https://data.wa.gov/Transportation/Electric-Vehicle-Population-Data/f6w7-q2d2/about_data

## Table of Contents
1. [Dataset](#dataset)
2. [Broad Overview](#overview)
3. [2024 Models](#2024)
4. [Pricing and Number of Models](#pricing)
5. [Good Performers - BMW and Kia](#performers)
6. [Registered Vehicles by Legislative District](#density)
7. [Summary](#summary)

---

## <a id="dataset"></a> Dataset

- **Registration Data for Vehicles:**
  
  - Registration data is as of 03-31-2024. All comparisons are done keeping in mind that we only have 3 months of data for 2024.
    - This is a snapshot of the data, there are 181458 vehicle registrations in the registry.
    
    - Below is an overview of the key columns:
    
      - **VIN (1-10):** First 10 digits of the Vehicle Identification Number.
      - **County, City, State, Postal Code:** Geographic details where the vehicle is registered.
      - **Model Year:** Year of the vehicle's model.
      - **Make, Model:** The manufacturer (e.g., Tesla, Audi) and model of the vehicle.
      - **Electric Vehicle Type:** Indicates whether the vehicle is a:
        - **Battery Electric Vehicle (BEV)**
        - **Plug-in Hybrid Electric Vehicle (PHEV)**
      - **CAFV Eligibility:** Specifies if the vehicle qualifies as a Clean Alternative Fuel Vehicle.
      - **Electric Range:** The vehicle's range on electric power alone (in miles).
      - **Base MSRP:** Manufacturer’s Suggested Retail Price.
      - **Legislative District:** Legislative district where the vehicle is registered.
      - **DOL Vehicle ID:** Unique ID for the vehicle from the Department of Licensing.
      - **Vehicle Location:** Geographical coordinates (longitude, latitude) of the vehicle's location.
      - **Electric Utility:** The electric utility provider servicing the vehicle’s location.
      - **2020 Census Tract:** The census tract where the vehicle is registered.

- **GeoJSON data for Legislative Districts:**

  File Information
      - **File Type**: GeoJSON
      - **Coordinate Reference System (CRS)**: EPSG::4269 (North American Datum 1983)
      - **Main Object Type**: `FeatureCollection`
      
      Features Overview
      Each feature represents a geographic boundary (polygon), likely for legislative districts, and contains the following information:
      
      Properties for Each Feature:
      - **STATEFP**: FIPS state code (e.g., "53" for Washington).
      - **SLDLST**: State Legislative District identifier.
      - **GEOID**: Geographic identifier.
      - **NAMELSAD**: Name of the legislative area (e.g., "State House District 1").
      - **ALAND**: Land area (in square meters).
      - **AWATER**: Water area (in square meters).
      - **INTPTLAT**: Latitude of the center point.
      - **INTPTLON**: Longitude of the center point.
      
      
      - **Type**: Polygon
      - **Coordinates**: A series of latitude and longitude points defining the polygon's boundaries.
      
      ## Example Feature:
      ```json
      {
        "type": "Feature",
        "properties": {
          "STATEFP": "53",
          "SLDLST": "001",
          "GEOID": "53001",
          "NAMELSAD": "State House District 1",
          "ALAND": 175590920,
          "AWATER": 6165627,
          "INTPTLAT": "+47.7952818",
          "INTPTLON": "-122.1635472"
        },
        "geometry": {
          "type": "Polygon",
          "coordinates": [
            [
              [-122.317652, 47.777618],
              [-122.317432, 47.778464],
              ...
            ]
          ]
        }
      }
- **Pricing Dataset** - self compiled for the top 6 automakers
    
  - **Total Entries**: 204 electric vehicles.
    - Make
    - Model
    - Model Year
    - Electric Vehicle Type
    - Price (for 195 entries)

## <a id="overview"></a> Broad Overview
- Key takeaways are as follows
  
   - <ins>**Top Left**</ins> : Number of Electric Vehicles registered are increasing. Just for the **first 3 months of 2024**, we see number of cars of 2024 model year are at 
                    **par with full year's data for 2020**. This signals a widespread adoption of Electric Vehicles in the State of Washington.
   - <ins>**Top Right**</ins>  : **Tesla** by far is the **most popular** Electric Vehicle registered in Washington. It is followed by the traditional automalers like **Nissan, 
                     Chevrolet, Ford, BMW and Kia**. We will consider them to be most popular makers from here on. These numbers are based on models from 1997 to 2024.
   - <ins>**Bottom Left**</ins>  : Market Share of **Tesla** has been fairly consistent over the years, but there is a **dip in 2024 models**(at least for the first 3 months). We see 
                    **Ford, Nissan, and Chevrolet lose** market share as well. **Kia and BMW** are **picking up** market share, however there seem to be a few **new entrants**
                    that are capitalizing on Tesla's dip.
   - <ins>**Bottom Right**</ins>  : We see the most popular EV manufacturers as a group have **increased their offerings over the years**. We see **BMW and Kia** have started to push 
                        **more models from 2015**, while Nissan, Ford, and Chevrolet have been more or less consistent in the number of offerings. Tesla too has 
                        increased it's offerings. So it is **not the case of lesser models that is causing the dip in their market share**.
![alt text](/img/posts/chart_1.png "Overview of Electric Vehicles")


## <a id="2024"></a>2024 Models:
- The chart on the **left** shows number of vehicles registered across model years from **1997-2024**. The one on the **right** is **only 2024** models.
- Key takeaway is : **Early entrants Nissan, Ford, and Chevrolet** who are **traditional** vehicle manufacturers are **not** even in the **top 15** in the **2024** models, despite being in the **top 5** overall. This could be a concern, as it seems like traditional manufactures are losing significant market share. With a push to Electric Vehicles all over the world, their loss in market share could be a big **neagtive** sign.
  
![alt text](/img/posts/chart_2.png "2024 EV")

## <a id="pricing"></a>Pricing and Models:

- Here we explore the **pricing** (base MSRP in USD), and **number of models** by top automakers to see if there is a consistent trend. The **top row** illustrates **pricing** and **bottom** row illustrates **number od models**. The **columns** differentiate between **Battery Electric Vehicles** and **Plug-in** varieties.
- We see **Tesla** has **decreased its price point** but still are **losing** market share. **Other** automakers are kept their prices **fairly consistent** except **BMW**. It looks like they are targeting the **luxury segment** of the market with their EV options.
- As for number of models, we see **Kia** and **BMW** both **increasing** the number of offerings in hybrid and battery vehicles varieties. This could explain their **increase** in their market market share. **Other** main automakers are have **decreased** or kept their offerings at **par** with previous years, probably causing a **dip** in their market share.

 ![alt text](/img/posts/chart_3.png "Pricing") 

## <a id="performers"></a> Good Performers - BMW and Kia

- Here we explore **sales** of BMW and KIA over the years by both **Plug-In** and **Battery Electric Vehicle** types.
- On taking a closer look at BMW and Kia's vehicle adoption in Washington, we see a **significant increase** in numbers.
- BMW was previously doing **better relatively** in the **plug-in** variety, now it's **Battery Vehicles** are also gaining traction. BMW's **2024** model sales are **almost at 2023** levels within **3** months of 2024!
- Kia has increased sales **both in BEV and Plug-Ins**, their sales for **2024** seem to be in line with **2023** figures considering we have data only for 3 months of 2024.

 ![alt text](/img/posts/chart_4.png "Performers") 

## <a id="desnity"></a> Registered Vehicles by Legislative District:

- Here we explore the **adoption** of Electric Vehicles by **Legislatve Districts**. The **hatched** legislative areas have **Republican** elected representatives.
- Legislative Districts with representatives elected from **Republican** Party have **lower adoption** of Electric Vehicles.
- Other factors could also affect EV adoption:
      - **Per Capita Income** - **Lower EV adoption** districs also have **lower per capita income** (not in the dataset, found from Washinton Sate's website)
      - **Proximity to Seattle** - It is observed that closer districts to **Seattle**(dark green area) have **better** adoption of EVs. This could be a function of **grid infrastructure** being better in the city and surrounding areas.

  ![alt text](/img/posts/chart_5.png "Density")

## <a id="summary"></a> Summary:

- **Tesla** is loosing market share. They were the dominant players in the market, since the **market** is also **growing** other automakers are trying to venture in to capitalize.
- **Ford, Nissan, and Chevrolet** are **cutting back** or **not emphazing** as much on the Electric Vehicle Market.
- **BMW and Kia** are trying to **capitalize** on the growth of the Electric Vehicle Market. **BMW** seems to be trying to capture the **luxury** segment, while **Kia** is targeting **retail**
- **Republican** legislatures have beenn **slow** to **embrace** Electric Vehicles. Other issues like **per capita income** and **grid infrastructure** could be at play.
  

