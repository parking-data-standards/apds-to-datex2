![](https://img.shields.io/badge/status-work%20in%20progress-blue)
# APDS to DATEX II

> This is an explorative playground to be used by the _APDS_ and _DATEX II_ initiatives. It is meant to help examining representability aspects between the two standards based of selected use cases. For now, the focus here was placed on the _APDS_ `Rights` domain. The collection of examples in here is not exhaustive in terms of the `Rights` domain. Moreover, it focuses on classes, not the _APDS_ API.

## Preparatory Remarks
In _APDS_, elements in the `Place` hierarchy and _Right Specifications_ from the `Rights` domain are bidirectionally linked. That way, data readers can do both,

- find out which _Right Specifications_ are applicable to a given _Place_ and
- determine the list of one or more _Places_ to which a particular _Right_ is applicable

The _APDS_ model uses _Rights_ to define eligibility criteria by means of time-based constraints and so-called qualifications. Only users/vehicles matching those criteria are then eligible to make use of a particular _Right Specification_.  


## Examples
Note: while _APDS_ recommends GUID-formatted identifiers, the examples in this repository are using "logical" ids for better readability.

1. [Different Charging Hours](#1-different-charging-hours)
2. [Zero-Emission Zone](#2-zero-emission-zone)
3. [Vehicle-based Restrictions](#3-vehicle-based-restrictions)

### 1. Different Charging Hours
A local authority offers on-street parking in selected locations. Depending on the time of day, different rates apply: there is a "daylight hours" rate, and there is an "evening hours" rate. The applicability is defined via two corresponding _Right Specifications_.

#### Place
The parking location where the two rates apply (_Place_ object):
```json
{
  "id": "{PlaceId}",
  "version": 1,
  "type": "parkingPlace",
  "layer": 1, 
  "name": [{ "language": "en", "string": "Main Street"}],
  "characteristics": {
    "structureType": "onStreet",
    "structureGrade": "groundLevel",
    "openToPublic": true,
    "spacesTotal": 67
  },
  "rightSpecifications": [
    { "id": "{DaylightHoursRightId}", "version": 1},
    { "id": "{EveningHoursRightId}", "version": 1}
  ]
  
  // contents truncated (additional place attributes)
}
```
#### Rates
The two _Rate Tables_ potentially applicable  
##### Daylight
```json
{
  "id": "{DaylightRateId}",
  "version": 1,
  "rateTableName": [{ "language": "en", "string": "public parking during daylight hours"}],
  "rateLineCollections": [
    // contents truncated (details of the rate table)
  ],
  "rateResponsibleParty": "XYZ City Council"
}
```
##### Evening  
```json
{
  "id": "{EveningRateId}",
  "version": 1,
  "rateTableName": [{ "language": "en", "string": "public parking during evening hours"}],
  "rateLineCollections": [
    // contents truncated (details of the rate table)
  ],
  "rateResponsibleParty": "XYZ City Council"
}

```
#### Right Specifications
Everything then comes together via the two corresponding _Right Specifications_.
##### Daylight Hours Right
```json
{
  "id": "{DaylightHoursRightId}",
  "version": 1,
  "type": "oneTimeUseParking",
  "description": [ { "language": "en", "string": "daylight hours"}],
  "hierarchyElements": [
    { "id": "{PlaceId}", "version": 1}
  ],
  "rateEligibility": [
    {
      "id": "",
      "version": 1,
      "rate": {
        "id": "{DaylightRateId}",
        "version": 1
      }
    }
  ],
  "validity": {
    "validityStatus": "definedByValidityTimeSpec",
    "validityTimeSpecification": {
      "overallStartTime": "2022-09-01T00:00:00Z",
      "validPeriods": [
        {
          "periodName": [ { "language": "en", "string": "during the day"}],
          "recurringTimePeriodOfDay": [
            {
              "startTimeOfPeriod": "08:00",
              "endTimeOfPeriod": "18:30"
            }
          ]
        }
      ]
    }
  }
}
```
##### Evening Hours Right
```json
{
  "id": "{EveningHoursRightId}",
  "version": 1,
  "type": "oneTimeUseParking",
  "description": [ { "language": "en", "string": "evening hours"}],
  "hierarchyElements": [
    { "id": "{PlaceId}", "version": 1}
  ],
  "rateEligibility": [
    {
      "id": "",
      "version": 1,
      "rate": {
        "id": "{EveningRateId}",
        "version": 1
      }
    }
  ],
  "validity": {
    "validityStatus": "definedByValidityTimeSpec",
    "validityTimeSpecification": {
      "overallStartTime": "2022-09-01T00:00:00Z",
      "validPeriods": [
        {
          "periodName": [ { "language": "en", "string": "evening hours"}],
          "recurringTimePeriodOfDay": [
            {
              "startTimeOfPeriod": "18:30",
              "endTimeOfPeriod": "22:00"
            }
          ]
        }
      ]
    }
  }
}
```
### 2. Zero-Emission Zone
In the city centre, a local authority has demarcated a zero-emission zone. For this area, only vehicles meeting this requirement are eligible.

#### Place
In this example, there is just one _Right Specification_ limiting the use of the location to specific vehicles only.
```json
{
  "id": "{ZeroEmissionLocationId}",
  "version": 1,
  "type": "identifiedArea",
  "layer": 1,
  "rightSpecifications": [
    { 
      "id": "{ZeroEmissionRightId}", 
      "version": 1
    }
  ]
  // contents truncated
}
```
#### Right Specification
Eligibility is defined via a corresponding _Right Specification_ with one _Qualification_ criterion:
```json
{
  "id": "{ZeroEmissionRightId}",
  "version": 1,
  "type": "oneTimeUseParking",
  "description": [ { "language": "en", "string": "zero-emission vehicles only"}],
  "hierarchyElements": [
    {
      "id": "{ZeroEmissionLocationId}",
      "version": 1
    }
  ],
  "qualifications": [
    {
      "emissions": {
        "emissionLevel": "freeOfEmission"
      }
    }
  ]
}
```

### 3. Vehicle-based Restrictions
A local authority operates a very old car park. The structure dates back to a time when vehicles were much smaller and lighter. As a consequence, usage of the car park needs to be restricted to vehicles that don't exceed certain dimensions and weight. In our examples, vehicles must not exceed 1.75 tons of total weight (vehicle + load) and have dimensions (Length/Width/Height) within the boundaries of (4.5 metres/1.8 metres/1.9 metres).  

#### Place
```json
{
  "id": "{OldPlaceId}",
  "version": 1,
  "type": "parkingPlace",
  "layer": 1,
  "name": [ { "language": "en", "string": "The Old Shed"}],
  "rightSpecifications": [
    {
      "id": "{SmallAndLightVehiclesOnlyRightId}",
      "version": 1
    }
  ]
  // contents truncated
}
```

#### Right Specification
```json
{
  "id": "{SmallAndLightVehiclesOnlyRightId}",
  "version": 1,
  "hierarchyElements": [
    {
      "id": "{OldPlaceId}",
      "version": 1
    }
  ],
  "qualifications": [
    {
      "grossWeightCharacteristics": [
        { 
          "grossVehicleWeight": 1.75,
          "typeOfWeight": "maximumPermitted",
          "comparisonOperator": "lessThanOrEqualTo"
        }
      ],
      "heightCharacteristics": [
        {
          "vehicleHeight": 1.9,
          "comparisonOperator": "lessThanOrEqualTo"
        }
      ],
      "lengthCharacteristics": [
        {
          "vehicleLength": 4.5,
          "comparisonOperator": "lessThanOrEqualTo"
        }
      ],
      "widthCharacteristics": [
        {
          "vehicleWidth": 1.8,
          "comparisonOperator": "lessThanOrEqualTo"
        }
      ]
    }
  ]
}
```