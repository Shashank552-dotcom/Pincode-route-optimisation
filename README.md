India Pincode Route Optimisation Tool
Excel + Power Query + Mapbox API + ORS + OSM

The Problem
Logistics managers were manually looking up source-to-destination distances one pincode at a time on Google Maps — entering each pair individually, recording distance and time manually. For a single warehouse covering 200–1,000 delivery pincodes, this process took 2–3 days per location.
With 17+ warehouse locations across India, this was unsustainable.

The Solution
An Excel-based route optimisation tool built entirely in Power Query using:

Mapbox Directions API — for distance and travel time between coordinates
OpenRouteService (ORS) — alternative routing engine
OpenStreetMap (OSM) — for latitude/longitude lookup from pincode
India Post Pincode Master Data — for area/city name resolution

The tool processes 200–1,000+ destination pincodes per warehouse run in under 25 minutes — reducing a 2–3 day manual process by over 90%.

How It Works — Step by Step
The tool uses 7 custom Power Query functions that must be invoked in sequence:
Queries in this file:
QueryTypePurposeMapbox Distance AKI...TableMain output table — source to destination distancesfnGetLatLong_OSMFunction (fx)Gets latitude & longitude for any pincode using OSMfnGetRoute_ORSFunction (fx)Gets route distance using OpenRouteServicefxMapboxGeoFunction (fx)Geocoding via Mapbox — pincode to coordinatesfxMapboxTimeFunction (fx)Gets travel time between two coordinates via MapboxfnGetCityFromPincod...Function (fx)Returns city/area name from pincode using Pincode MasterMapbox DISTANCETableCore distance calculation table

Usage Instructions
Prerequisites

A free Mapbox account — get your API key at mapbox.com
(Free tier: 100,000 API calls/month)
Microsoft Excel with Power Query (Excel 2016 or later)

Step 1 — Add Your API Key

Open Power Query Editor: Data → Get Data → Launch Power Query Editor
Click on fxMapboxTime in the left panel
Find the step containing YOUR_MAPBOX_API_KEY
Replace with your actual Mapbox API key
Repeat for fxMapboxGeo and Mapbox DISTANCE queries

Step 2 — Prepare Your Source Table

Select the sheet for your warehouse location
Create a table with two columns:

Source_Pincode — your warehouse pincode (single value)
Destination_Pincode — list of all delivery pincodes



Step 3 — Get Coordinates (Lat/Long)

In your table, add a Custom Column
Invoke fnGetLatLong_OSM for the Source Pincode
→ Returns Source Latitude and Source Longitude
Add another Custom Column
Invoke fnGetLatLong_OSM for the Destination Pincode
→ Returns Destination Latitude and Destination Longitude

Step 4 — Get Distance

Add a Custom Column
Invoke Mapbox DISTANCE passing:

Source Latitude, Source Longitude
Destination Latitude, Destination Longitude
→ Returns road distance in kilometres



Step 5 — Get Travel Time

Add a Custom Column
Invoke fxMapboxTime passing the same coordinates
→ Returns estimated travel time in minutes

Step 6 — Get City/Area Name

Add a Custom Column
Invoke fnGetCityFromPincod... passing the Destination Pincode
→ Returns area name and city from India Post master data

Step 7 — Expand and Load

Expand all custom columns to individual fields
Click Close & Load
Output table is ready with full route data


Output Columns
ColumnDescriptionSource PincodeWarehouse origin pincodeDestination PincodeDelivery destination pincodeSource LatitudeCoordinates of source warehouseSource LongitudeCoordinates of source warehouseDestination LatitudeCoordinates of destinationDestination LongitudeCoordinates of destinationDistance (km)Road distance between source and destinationTravel Time (mins)Estimated travel time by roadCity / AreaLocality name for destination pincode

Coverage
This file includes pre-built output sheets for 19 warehouse locations across India:
Delhi NCR - ( WH_Delhi, WH_Delhi_West, WH_Gurugram, WH_Noida, WH_GBN) 
West - ( WH_Pune, WH_Bhiwandi (Mumbai), WH_Indore) 
South - (WH_Bangalore, WH_Hyderabad, WH_Chennai) 
East - ( WH_Kolkata, WH_Howrah, WH_Bhubaneswar) 
North - ( WH_Lucknow, WH_Jaipur, WH_Dehradun) 
Central - ( WH_Bhopal, WH_Raipur) 
Others - (WH_Bartana, WH_Malkapur, WH_Khairpur)

Data Source
India_Post_Pincode_Master sheet — Public data from India Post (indiapost.gov.in).
Contains all Indian pincodes with state, district, taluk, and locality name.
Included to enable automatic area name resolution without external lookup.

Tech Stack

Microsoft Excel — Power Query (M Language)
Mapbox Directions API — Primary routing engine
OpenRouteService (ORS) — Secondary routing engine
OpenStreetMap (OSM) — Geocoding (pincode to coordinates)
India Post Open Data — Pincode master reference

No Python. No external scripts. Runs entirely within Excel.

Real-World Impact
Built during active logistics operations to solve a real business problem:

Covered 20+ warehouse locations across India
Processed 200–1,000+ pincodes per warehouse per run
Reduced route analysis time from 2–3 days to under 25 minutes
Accuracy of ~95% compared to manual Google Maps lookup
Used operationally for same-day delivery route planning


Repository Structure
pincode-route-optimisation/
│
├── README.md
├── Route_Optimisation_API.xlsx      ← Main tool file
└── images/
    ├── output_sample.png            ← Sample output screenshot
    └── powerquery_steps.png         ← Power Query editor screenshot

License
This tool was built independently as a personal project.
The Mapbox API, ORS, and OSM integrations are subject to their respective terms of service.
India Post pincode data is publicly available government data.
