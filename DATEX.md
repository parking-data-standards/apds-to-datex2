![](https://img.shields.io/badge/status-work%20in%20progress-blue)

### 1. Different Charging Hours

```json
"hierarchyElementGeneral": {
    "id": "{PlaceId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:name": { "com:values":{"com:value":{ "lang": "en","#text": "Main Street"}}},
    "fac:rates": {
        "xsi:type": "fac:RatesByReference", "fac:rateTableReference": {
            "targetClass": "fac:RateTable","id": 0,"version": 0},
        "fac:rateMatrixReference": {
            "targetClass": "fac:RateMatrix","id": "{RateMatrixId}","version": 1}
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

#### Corresponding RateMatrix:
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
    "fac:name": {
        "com:values": {
            "com:value": {
                "lang": "en",
                "#text": "The Old Shed"
            }
        }
    },
    "fac:rates": {
        "xsi:type": "fac:RatesByReference",
        "fac:rateTableReference": {
            "targetClass": "fac:RateTable",
            "id": "{RateIdforOldCarPark}",
            "version": 1
        }
    },
    "layer": 1,
    "type": "place",
    "commonComponents": {
        "additionalCharacteristics": {
            "assignment": {
                "exclusivelyAssignedFor": {
                    "fac:description": {
                        "com:values": {
                            "com:value": {
                                "lang": "en",
                                "#text": "very old car park, size and weight restrictions apply"
                            }
                        }
                    },
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

