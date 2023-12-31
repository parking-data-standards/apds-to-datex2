![](https://img.shields.io/badge/status-work%20in%20progress-blue)


1. [Different Charging Hours](#1-different-charging-hours)
2. [Zero-Emission Zone](#2-zero-emission-zone)
3. [Vehicle-based Restrictions](#3-vehicle-based-restrictions)
4. [Different Rate for Reservation Customers](#4-different-rate-for-reservation-customers)
5. [Customer-only Parking](#5-customer-only-parking)
6. [Interconnected Rights - Reduced Rate for Subsequent Visits](#6-interconnected-right-specifications)
7. [Differentiate Vehicle Type of Drive](#7-rights-per-vehicle-type-of-drive)
8. [Differentiate Payment Methods](#8-right-per-payment-method)
9. [Special Load Types](#9-special-load-types)

### 1. Different Charging Hours

```json
"hierarchyElementGeneral": {
    "id": "{PlaceId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:name": { "com:values":{"com:value":{ "lang": "en","#text": "Main Street"}}},
    "fac:rates": {"xsi:type": "fac:RatesByReference",
        "fac:rateTableReference": {"targetClass": "fac:RateTable","id": 0,"version": 0},
        "fac:rateMatrixReference": {"targetClass": "fac:RateMatrix","id": "{RateMatrixId}","version": 1}
    },
    "layer": 1,
    "type": "place",
    "commonComponents": {
        "characteristics": {
            "structureGrade": "groundLevel",
            "structureType": "onStreet",
            "openToPublic": true,
            "spacesTotal": 67
        }
    }
}

```

#### Corresponding RateMatrix (1)
```json
"rateMatrix": {
    "id": "{RateMatrixId}",
    "version": 1,
    "versionTime": "2023-07-07T10:13:51+02:00",
    "rateTable": [
        {
            "id": "{DaylightRateId}",
            "version": 1,
            "rateTableName": {"com:values": {"com:value": {"lang": "en","#text": "public parking during daylight hours"}}},
            "rateResponsibleParty": {"targetClass": "fac:Organisation","id": "{XYZCityCouncilId}","version": 1},
            "rateLineCollection": {
                "collectionSequence": 1,
                "rateLine": {
                    "sequence": 1,
                    "rateLineType": "incrementingRate",
                    "value": 2.8
                }
            },
            "rateEligibility": {
                "eligibility": "",
                "rightSpecification": {
                    "type": "oneTimeUseParking",
                    "description": {"com:values": {"com:value": {"lang": "en","#text": "daylight hours"}}},
                    "validity": {
                        "com:validityStatus": "definedByValidityTimeSpec",
                        "com:validityTimeSpecification": {
                            "com:overallStartTime": "2022-09-01T00:00:00Z",
                            "com:validPeriod": {
                                "com:periodName": {"com:values": {"com:value": {"lang": "en","#text": "during the day"}}},
                                "com:recurringTimePeriodOfDay": {
                                    "com:startTimeOfPeriod": "08:00:00",
                                    "com:endTimeOfPeriod": "18:30:00"
                                }
                            }
                        }
                    }
                }
            }
        },
        {
            "xsi:type": "RateTable",
            "id": "{EveningRateId}",
            "version": 1,
            "rateTableName": {"com:values": {"com:value": {"lang": "en","#text": "public parking during daylight hours"}}},
            "rateResponsibleParty": {"targetClass": "fac:Organisation","id": "{XYZCityCouncilId}","version": 1},
            "rateLineCollection": {
                "collectionSequence": 1,
                "rateLine": {
                    "sequence": 1,
                    "rateLineType": "incrementingRate",
                    "value": 2.8
                }
            },
            "rateEligibility": {
                "eligibility": "",
                "rightSpecification": {
                    "type": "oneTimeUseParking",
                    "description": {"com:values": {"com:value": {"lang": "en","text": "evening hours"}}},
                    "validity": {
                        "com:validityStatus": "definedByValidityTimeSpec",
                        "com:validityTimeSpecification": {
                            "com:overallStartTime": "2022-09-01T00:00:00Z",
                            "com:validPeriod": {
                                "com:periodName": {"com:values": {"com:value": {"lang": "en","#text": "during the day"}}},
                                "com:recurringTimePeriodOfDay": {
                                    "com:startTimeOfPeriod": "18:30:00",
                                    "com:endTimeOfPeriod": "22:00:00"
                                }
                            }
                        }
                    }
                }
            }
        }
    ]
}
```

### 2. Zero-Emission Zone

```json
"hierarchyElementGeneral": {
    "id": "{PlaceId}",
    "version": 1,
    "xsi:type": "SpecificArea",
    "fac:rates": {
        "xsi:type": "fac:RatesByReference",
        "fac:rateTableReference": {"targetClass": "fac:RateTable","id": "{RateIdforZeroEmissionZone}","version": 1}},
    "layer": 1,
    "type": "identifiedArea",
    "additionalCharacteristics": {
        "assignment": {
            "exclusivelyAssignedFor": {
                "fac:eligibilityName": {"com:values": {"com:value": {"lang": "en","#text": "zero-emission only"}}},
                "fac:description": {"com:values": {"com:value": {"lang": "en","#text": "only zero-emission vehicle qualify for this right"}}},
                "fac:qualification": {
                    "fac:vehicleCharacteristics": {
                        "com:emissions": {"com:emissionLevel": "freeOfEmission"}
                    }
                }
            }
        }
    }
}
```

### 3. Vehicle-based Restrictions

```json
"hierarchyElementGeneral": {
    "id": "{OldPlaceId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:name": {"com:values": {"com:value": {"lang": "en","#text": "The Old Shed"}}},
    "fac:rates": {"xsi:type": "fac:RatesByReference","fac:rateTableReference": {"targetClass": "fac:RateTable","id": "{RateIdforOldCarPark}","version": 1}
    },
    "layer": 1,
    "type": "place",
    "commonComponents": {
        "additionalCharacteristics": {
            "assignment": {
                "exclusivelyAssignedFor": {
                    "fac:description": {"com:values": {"com:value": {"lang": "en","#text": "very old car park, size and weight restrictions apply"}}},
                    "fac:qualification": {
                        "fac:vehicleCharacteristics": {
                            "com:grossWeightCharacteristic": {
                                "com:comparisonOperator": "lessThanOrEqualTo",
                                "com:grossVehicleWeight": 1.75,
                                "com:typeOfWeight": "maximumPermitted"
                            },
                            "com:heightCharacteristic": {
                                "com:comparisonOperator": "lessThanOrEqualTo",
                                "com:vehicleHeight": 1.9
                            },
                            "com:lengthCharacteristic": {
                                "com:comparisonOperator": "lessThanOrEqualTo",
                                "com:vehicleLength": 4.5
                            },
                            "com:widthCharacteristic": {
                                "com:comparisonOperator": "lessThanOrEqualTo",
                                "com:vehicleWidth": 1.8
                            }
                        }
                    }
                }
            }
        }
    }
```

### 4. Different Rate for Reservation Customers

```json
"hierarchyElementGeneral": {
    "id": "{MixedUsePlaceId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:name": {"com:values": {"com:value": {"lang": "en","#text": "City Centre Car Park"}}},
    "layer": 1,
    "type": "place",
    "commonComponents": {
        "additionalCharacteristics": {
            "assignment": [
                {
                    "applicableFor": {
                        "fac:description": {"com:values": {"com:value": {"lang": "en","#text": "standard (non-pre-book) parkers"}}},
                        "fac:qualification": {
                            "fac:rateTableMember": {"targetClass": "fac:RateTable", "id": "{StandardRateId}","version": 1}
                        }
                    }
                },
                {
                    "applicableFor": {
                        "fac:eligibilityName": {"com:values": {"com:value": {"lang": "en","#text": "Pre-Bookers"}}},
                        "fac:description": {"com:values": {"com:value": {"lang": "en","#text": "pre-bookers only"}}},
                        "fac:qualification": {
                            "fac:withReservation": true,
                            "fac:rateTableMember": {"targetClass": "fac:RateTable","id": "{ReducedPreBookerRateId}","version": 1}
                        }
                    }
                }
            ]
        }
    }
}
```

### 5. Customer-only Parking

```json
"hierarchyElementGeneral": {
    "id": "{FitnessStudioLocationId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:name": { "com:values": { "com:value": { "lang": "en", "#text": "Fit24" } } },
    "layer": 1,
    "type": "place",
    "commonComponents": {
        "additionalCharacteristics": {
            "assignment": {
                "exclusivelyAssignedFor": {
                    "fac:eligibilityName": { "com:values": { "com:value": { "lang": "en", "#text": "members only" } } },
                    "fac:qualification": {
                        "fac:withMembership": true,
                        "fac:membershipName": { "com:values": { "com:value": { "lang": "en", "#text": "Fit24" } } },
                        "fac:rateTableMember": { "targetClass": "fac:RateTable", "id": "{RateTableIdForMembersOnly}", "version": 1
                        }
                    }
                }
            }
        }
    }
}
```

### 6. Interconnected Right Specifications

```json
"hierarchyElementGeneral": {
    "id": "{PlaceWithConnectedRightsId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:rates": {
        "xsi:type": "fac:RatesByReference",
        "fac:rateTableReference": { "targetClass": "fac:RateTable", "id": 0, "version": 0 },
        "fac:rateMatrixReference": { "targetClass": "fac:RateMatrix", "id": "{RateMatrixStandardAndReducedId}", "version": 1 } },
    "layer": 1,
    "type": "place"
}
```

#### Corresponding RateMatrix (6)

```json
"rateMatrix": {
    "id": "{RateMatrixStandardAndReducedId}",
    "version": 1,
    "versionTime": "2023-07-07T10:13:51+02:00",
    "rateTable": [
        {
            "id": "{StandardRateId}",
            "version": 1,
            "rateTableName": { "com:values": { "com:value": { "lang": "en", "#text": "standard rates" } } },
            "rateResponsibleParty": { "targetClass": "fac:Organisation", "id": "{XYZCityCouncilId}", "version": 1 },
            "rateLineCollection": {
                "collectionSequence": 1,
                "rateLine": {
                    "sequence": 1,
                    "rateLineType": "incrementingRate",
                    "value": 2.8
                }
            }
        },
        {
            "xsi:type": "RateTable",
            "id": "{ReducedRateId}",
            "version": 1,
            "rateTableName": { "com:values": { "com:value": { "lang": "en", "#text": "Reduced rates" } } },
            "rateResponsibleParty": { "targetClass": "fac:Organisation", "id": "{XYZCityCouncilId}", "version": 1 },
            "rateLineCollection": {
                "collectionSequence": 1,
                "rateLine": {
                    "sequence": 1,
                    "rateLineType": "incrementingRate",
                    "value": 1
                }
            },
            "rateEligibility": {
                "eligibility": {
                    "qualification": {
                        "memberOfOtherRateTable": true,
                        "rateTableMember": { "targetClass": "fac:RateTable", "id": "{StandardRateId}", "version": 1 }
                    }
                },
                "rightSpecification": {
                    "description": { "com:values": { "com:value": { "lang": "en", "#text": "Can be only used if already {StandardRateId} was applied before during that day" } } }
                }
            }
        }
    ]
}
```

### 7. Rights per Vehicle Type of Drive

```json
"hierarchyElementGeneral": {
    "id": "{CamdenLocationId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:rates": {
        "xsi:type": "fac:RatesByReference",
        "fac:rateTableReference": { "targetClass": "fac:RateTable", "id": 0, "version": 0 },
        "fac:rateMatrixReference": { "targetClass": "fac:RateMatrix", "id": "{RateMatrixPerDriveTypeId}", "version": 1 }
    },
    "layer": 1,
    "type": "place"
}
```

#### Corresponding RateMatrix (7)

```json
"rateMatrix": {
    "id": "{RateMatrixPerDriveTypeId}",
    "version": 1,
    "versionTime": "2023-07-07T10:13:51+02:00",
    "rateTable": [
        {
            "id": "{ReducedRateId}",
            "version": 1,
            "rateTableName": { "com:values": { "com:value": { "lang": "en", "#text": "reduced rates" } } },
            "rateResponsibleParty": { "targetClass": "fac:Organisation", "id": "{XYZCityCouncilId}", "version": 1 },
            "rateLineCollection": {
                "collectionSequence": 1,
                "rateLine": {
                    "sequence": 1,
                    "rateLineType": "incrementingRate",
                    "value": 1
                }
            },
            "rateEligibility": {
                "priority": 2,
                "eligibility": {
                    "qualification": {
                        "propulsionEnergyType": [
                            "battery",
                            "ethanol",
                            "hydrogen",
                            "liquidGas",
                            "lpg",
                            "methane",
                            "petrol"
                        ]
                    }
                }
            }
        },
        {
            "xsi:type": "RateTable",
            "id": "{IncreasedRateId}",
            "version": 1,
            "rateTableName": { "com:values": { "com:value": { "lang": "en", "#text": "Increased rates" } } },
            "rateResponsibleParty": { "targetClass": "fac:Organisation", "id": "{XYZCityCouncilId}", "version": 1 },
            "rateLineCollection": {
                "collectionSequence": 1,
                "rateLine": {
                    "sequence": 1,
                    "rateLineType": "incrementingRate",
                    "value": 2
                }
            },
            "rateEligibility": {
                "priority": 1,
                "eligibility": {
                    "qualification": {
                        "propulsionEnergyType": [
                            "diesel",
                            "biodiesel"
                        ]
                    }
                }
            }
        }
    ]
}
```

### 8. Right per Payment Method

```json
"hierarchyElementGeneral": {
    "id": "{PlaceId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:rates": {
        "xsi:type": "fac:RatesByReference",
        "fac:rateTableReference": { "targetClass": "fac:RateTable", "id": 0, "version": 0 },
        "fac:rateMatrixReference": { "targetClass": "fac:RateMatrix", "id": "{RateMatrixPaymentMethodAwareId}", "version": 1 }
    },
    "layer": 1,
    "type": "place"
}
```

#### Corresponding RateMatrix (8)

```json
"rateMatrix": {
    "id": "{RateMatrixPaymentMethodAwareId}",
    "version": 1,
    "versionTime": "2023-07-07T10:13:51+02:00",
    "rateTable": [
        {
            "id": "{StandardRateId}",
            "version": 1,
            "rateTableName": { "com:values": { "com:value": { "lang": "en", "#text": "standard rates" } } },
            "rateResponsibleParty": { "targetClass": "fac:Organisation", "id": "{XYZCityCouncilId}", "version": 1 },
            "rateLineCollection": {
                "collectionSequence": 1,
                "rateLine": {
                    "sequence": 1,
                    "rateLineType": "incrementingRate",
                    "value": 2.8
                }
            },
            "rateEligibility": {
                "eligibility": {
                    "qualification": {
                        "paymentMethod": {
                            "paymentMeans": [
                                "cashBillsOnly",
                                "cashCoinsOnly",
                                "cashCoinsAndBills"
                            ]
                        }
                    }
                }
            }
        },
        {
            "xsi:type": "RateTable",
            "id": "{ReducedRateId}",
            "version": 1,
            "rateTableName": { "com:values": { "com:value": { "lang": "en", "#text": "Reduced rates" } } },
            "rateResponsibleParty": { "targetClass": "fac:Organisation", "id": "{XYZCityCouncilId}", "version": 1 },
            "rateLineCollection": {
                "collectionSequence": 1,
                "rateLine": {
                    "sequence": 1,
                    "rateLineType": "incrementingRate",
                    "value": 1
                }
            },
            "rateEligibility": {
                "eligibility": {
                    "qualification": {
                        "paymentMethod": {
                            "paymentMeans": [
                                "mobileAccount",
                                "paymentCreditCard",
                                "prepay"
                            ]
                        }
                    }
                }
            }
        }
    ]
}
```

### 9. Special Load Types

```json
"hierarchyElementGeneral": {
    "id": "{SpecialDeliveryZoneId}",
    "version": 1,
    "xsi:type": "SpecificArea",
    "fac:name": { "com:values": { "com:value": { "lang": "en", "#text": "special delivery unloading zone" } } },
    "layer": 1,
    "type": "identifiedArea",
    "additionalCharacteristics": {
        "assignment": {
            "exclusivelyAssignedFor": {
                "fac:description": { "com:values": { "com:value": { "lang": "en", "#text": "delivery of special goods only" } } },
                "fac:qualification": {
                    "fac:noFeeToUse": true,
                    "fac:vehicleCharacteristics": [
                        {"com:loadType": "refrigeratedGoods"},
                        {"com:loadType": "perishableProducts"}
                    ]
                }
            }
        }
    }
}
```
