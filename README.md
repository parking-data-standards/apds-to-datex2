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
4. [Different Rate for Reservation Customers](#4-different-rate-for-reservation-customers)
5. [Customer-only Parking](#5-customer-only-parking)

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
}
```
_(contents truncated - additional place attributes)_
#### Rates
The two _Rate Tables_ potentially applicable  
##### Daylight
```json
{
  "id": "{DaylightRateId}",
  "version": 1,
  "rateTableName": [{ "language": "en", "string": "public parking during daylight hours"}],
  "rateLineCollections": [ {} ],
  "rateResponsibleParty": "XYZ City Council"
}
```
_(contents truncated - details of the rate table)_
##### Evening  
```json
{
  "id": "{EveningRateId}",
  "version": 1,
  "rateTableName": [{ "language": "en", "string": "public parking during evening hours"}],
  "rateLineCollections": [ {} ],
  "rateResponsibleParty": "XYZ City Council"
}
```
_(contents truncated - details of the rate table)_
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
      "id": "e908a3ab-8931-46fb-9ef9-c49f1f262dcb",
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
      "id": "38745ba6-af98-4c7d-b14d-4115325a55ca",
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
}
```
_(contents truncated - further place details)_
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
  "rateEligibility": [
    {
      "id": "e86eb1ac-f7bb-44ed-91bd-2b1f854a18df",
      "version": 1,
      "rateTable": {
        "id": "{RateIdForZeroEmissionZone}",
        "version": 1
      },
      "eligibility": {
        "eligibilityName": [ { "language": "en", "string": "zero-emission only"}],
        "description": [ { "language": "en", "string": "only zero-emission vehicle qualify for this right"}],
        "qualifications": [
          {
            "emissions": {
              "emissionLevel": "freeOfEmission"
            }
          }
        ]
      }
    }
  ]
}
```

### 3. Vehicle-based Restrictions
A local authority operates a very old car park. The structure dates back to a time when vehicles were much smaller and lighter. As a consequence, usage of the car park needs to be restricted to vehicles that don't exceed certain dimensions and weight. In our example, vehicles must not exceed 1.75 tons of total weight (vehicle + load) and have dimensions (Length/Width/Height) within the boundaries of (4.5 metres/1.8 metres/1.9 metres).  

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
}
```
_(contents truncated - further place details)_

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
  "rateEligibility": [
    {
      "id": "f6e7fb78-0f9e-4844-875f-d6a50b784197",
      "version": 1,
      "rateTable": {
        "id": "{RateIdForOldCarPark}",
        "version": 1
      },
      "eligibility": {
        "description": [ { "language": "en", "string": "very old car park, size and weight restrictions apply"}],
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
    }
  ]
}
```

### 4. Different Rate for Reservation Customers
A car park operator wants to create incentives for customers who pre-book a space in a car park. Customers with a reservation will therefore be eligible for a different (cheaper) rate. Adhoc parkers have to pay the standard rate.
#### Place
```json
{
  "id": "{MixedUsePlaceId}",
  "version": 1,
  "type": "parkingPlace",
  "name": [ { "language": "en", "string": "City Centre Car Park"}],
  "rightSpecifications": [
    { "id": "{AdhocParkerRightId}", "version": 1},
    { "id": "{PreBookParkerRightId}", "version": 1}
  ]
}
```
_(contents truncated)_

#### Standard Right Specification
```json
{
  "id": "{AdhocParkerRightId}",
  "version": 1,
  "description": [ { "language": "en", "string": "standard (non-pre-book) parkers"}],
  "rateEligibility": [
    {
      "id": "4355949a-8f6a-450e-8013-c3edaddf979d",
      "version": 1,
      "rateTable": {
        "id": "{StandardRateId}",
        "version": 1
      }
    }
  ]
}
```

#### Right Specification for Pre-Bookers
```json
{
  "id": "{PreBookParkerRightId}",
  "version": 1,
  "description": [ { "language": "en", "string": "pre-bookers only"}],
  "rateEligibility": [
    {
      "id": "3334b1b7-9b36-4bb8-a0b5-906c65876b1e",
      "version": 1,
      "rateTable": {
        "id": "{ReducedPreBookerRateId}",
        "version": 1
      },
      "eligibility": {
        "eligibilityName": "Pre-Bookers",
        "qualifications": [
          {
            "withReservation": true
          }
        ]
      }
    } 
  ]
}
```

### 5. Customer-only Parking
A fitness studio has a small adjacent parking lot that shall only be used by members of the gym.

#### Place
```json
{
  "id": "{FitnessStudioLocationId}",
  "version": 1,
  "type": "parkingPlace",
  "name": [ { "language": "en", "string": "Fit24"}],
  "rightSpecifications": [
    { "id": "{MembersOnlyRightId}", "version":  1}
  ]
}
```
_(contents truncated - further place details)_

#### Right Specification
```json
{
  "id": "{MembersOnlyRightId}",
  "version": 1,
  "hierarchyElements": [
    { "id":  "{FitnessStudioLocationId}", "version": 1}
  ],
  "rateEligibility": [
    {
      "id": "1e3a3d07-47ea-4f72-8039-e8c817602dfc",
      "version": 1,
      "rateTable": {
        "id": "{RateTableIdForMembersOnly}",
        "version": 1
      },
      "eligibility": {
        "eligibilityName": [ { "language": "en", "string": "members only"}],
        "qualifications": [
          {
            "withMembership": true,
            "membershipNames": [ "Fit24"]
          }
        ]
      }
    }
  ]
}
```

