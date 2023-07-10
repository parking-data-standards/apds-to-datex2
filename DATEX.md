![](https://img.shields.io/badge/status-work%20in%20progress-blue)

### 1. Different Charging Hours

```json
"hierarchyElementGeneral": {
    "id": "{PlaceId}",
    "version": 1,
    "xsi:type": "Place",
    "fac:name": {
        "com:values": {
            "com:value": {
                "lang": "en",
                "#text": "Main Street"
            }
        }
    },
    "fac:rates": {
        "xsi:type": "fac:RatesByReference",
        "fac:rateTableReference": {
            "targetClass": "fac:RateTable",
            "id": 0,
            "version": 0
        },
        "fac:rateMatrixReference": {
            "targetClass": "fac:RateMatrix",
            "id": "{RateMatrixId}",
            "version": 1
        }
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
            "rateTableName": {
                "com:values": {
                    "com:value": {
                        "lang": "en",
                        "#text": "public parking during daylight hours"
                    }
                }
            },
            "rateResponsibleParty": {
                "targetClass": "fac:Organisation",
                "id": "{XYZCityCouncilId}",
                "version": 1
            },
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
                    "description": {
                        "com:values": {
                            "com:value": {
                                "lang": "en",
                                "#text": "daylight hours"
                            }
                        }
                    },
                    "validity": {
                        "com:validityStatus": "definedByValidityTimeSpec",
                        "com:validityTimeSpecification": {
                            "com:overallStartTime": "2022-09-01T00:00:00Z",
                            "com:validPeriod": {
                                "com:periodName": {
                                    "com:values": {
                                        "com:value": {
                                            "lang": "en",
                                            "#text": "during the day"
                                        }
                                    }
                                },
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
            "rateTableName": {
                "com:values": {
                    "com:value": {
                        "lang": "en",
                        "#text": "public parking during daylight hours"
                    }
                }
            },
            "rateResponsibleParty": {
                "targetClass": "fac:Organisation",
                "id": "{XYZCityCouncilId}",
                "version": 1
            },
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
                    "description": {
                        "com:values": {
                            "com:value": {
                                "lang": "en",
                                "#text": "evening hours"
                            }
                        }
                    },
                    "validity": {
                        "com:validityStatus": "definedByValidityTimeSpec",
                        "com:validityTimeSpecification": {
                            "com:overallStartTime": "2022-09-01T00:00:00Z",
                            "com:validPeriod": {
                                "com:periodName": {
                                    "com:values": {
                                        "com:value": {
                                            "lang": "en",
                                            "#text": "during the day"
                                        }
                                    }
                                },
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

