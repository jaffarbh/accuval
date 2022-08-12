# accuval
## AccuVal REST API

[AccuVal](https://accuval.co.uk/) is an AI based valuation platform for properties in the United Kingdom. It can be accessed interactively with any modern web browser, or programmatically using a REST API tool. This repo explains how AccuVal can be accessed via API with examples.

## Method 1: UPRN Code
UPRN stands for _Unique Property Reference Number_. It’s becoming the de-facto to reference properties across the different government bodies. As the name suggests, UPRN is unique for each property. As of mid 2022, please keep in mind that not all properties have UPRN assigned in all databases.

To value a property using it’s UPRN code, the following command can be used:

`curl -H "apikey: replace-with-your-key" "https://accuval.co.uk/api/?uprn=202215263"`

Which would return:

`{"Type": "Flat/Maisonette", "NUMBER_HABITABLE_ROOMS": 4, "TOTAL_FLOOR_AREA": 72, "CURRENT_ENERGY_RATING": "B", "POTENTIAL_ENERGY_RATING": "B", "CONSTRUCTION_AGE_BAND": "2012 Onwards", "Duration": "Leasehold", "New": "No", "Condition": "Average", "Price": 553430, "Confidence": "High confidence", "POSTCODE": "HA9 0FR", "ADDRESS": "Flat 1 Archery Court, 31, Olympic Way", "Rent": 2075}`

in JSON format. Obviously, the UPRN is required.

Please [contact us](https://accuval.co.uk/contact/) for pricing and API keys.

## Method 2: Property Details
If UPRN is unavailable or unknown, AccuVal can still value properties based on their Postcode and basic information. This method is also useful to value properties that have changed or do not exist (yet)!

To value a property using it’, the following command can be used:

`curl -H "apikey: replace-with-your-key" "https://accuval.co.uk/api/?postcode=HA9+0FR&address=Flat+1+Archery+Court,+31,+Olympic+Way&type=Flat/Maisonette&rooms=4&area=72&uom=m&epc_current=B&epc_potential=B&age=2012+Onwards&duration=Leasehold&new=No&condition=Average"`

Which would return:

`{"Type": "Flat/Maisonette", "NUMBER_HABITABLE_ROOMS": 4, "TOTAL_FLOOR_AREA": 72, "CURRENT_ENERGY_RATING": "B", "POTENTIAL_ENERGY_RATING": "B", "CONSTRUCTION_AGE_BAND": "2012 Onwards", "Duration": "Leasehold", "New": "No", "Condition": "Average", "Price": 553430, "Confidence": "High confidence", "POSTCODE": "HA9 0FR", "ADDRESS": "Flat 1 Archery Court, 31, Olympic Way", "Rent": 2075}`

### Required parameters

#### `postcode`
A valid UK postcode, i.e. HA9 0FR. Currently only Britain and Wales are supported. **Please note that whitespaces aren't allowed, so in the example above using curl, whitespaces are replaced with the '+' symbol.**

#### `type`
- The property type,  which can one of:
- `Flat/Maisonette`
- `Terraced/End-of-Terrace`
- `Semi-Detached`
- `Detached/Bungalow`

#### `area`
The total floor area. This can be obtained from the EPC certificate.

### Optional parameters and defaults

#### `rooms`
The total number of habitable rooms. This includes bedrooms, living rooms and receptions. Kitchens and bathrooms are not considered habitable rooms and should not be included. Must be a positive integer. _defaults to 3 if missing_.

#### `address`
The first line address.

#### `uom`
Unit of measure for the area, can be:
- `m` square meters (_default_)
- `f` square feet

#### `epc_current`
The current energy rating. This can be obtained from the EPC certificate. Possible values:
- `A`
- `B`
- `C`
- `D` (_default_)
- `E`
- `F`

#### `epc_potential`
The potential energy rating. This can be obtained from the EPC certificate. Possible values:
- `A`
- `B`
- `C` (_default_)
- `D`
- `E`
- `F`

#### `age`
The construction age band. Possible values:
- `Before 1900`
- `1900-1929`
- `1930-1949` (_default_)
- `1950-1966`
- `1967-1975`
- `1976-1982`
- `1983-1990`
- `1991-1995`
- `1996-2002`
- `2003-2006`
- `2007-2011`
- `2012 Onwards`

#### `duration`
The type of tenure for the property. Possible values:
- `Leasehold`
- `Freehold`

If missing, _defaults to Leasehold for Flat/Maisonette, otherwise Freehold_

#### `new`
Whether the property is a new build (i.e. no prior records with Land Registry). Possible values:
- `Yes`
- `No` (_default_)

#### `condition`
The overall condition of the property in relations to similar properties in the area. Possible values:
- `Outstanding`
- `Above Average`
- `Average` (_default_)
- `Below Average`
- `Poor`
