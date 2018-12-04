# Examples for requirement specification formats

## Hobsons chute format

Chute is a DSL for specifying requirements.  A simple program might looks like the following in the DSL

```
PLAN AAS-Geology "Associate of Science - Geology"
    WITH
    description = "Two Year Geology Program resulting in a Associate of Science upon completion.",
    catalogYear >= 2019,
    gpa >= 1.7.

    take ENGL101.

    take GEOL101 and (GEOL102 or GEOL113b) and (GEOL103 or GEOL113b)
    where grade >= 2.0,
    with description = "First Year Courses", targetYear = 1.

    take GEOL11*,GEOL12* where courses >= 2, credits >= 4, with description = "First Year Electives".

    take 2 FROM (
        BIOL101 and (BIO102a,BIO102b,BIO102c where courses >= 2, credits >= 4) and BIOL103
        with description = "Biology Series"
    ),
    (
        CHEM101 and CHEM102 and CHEM103
        with description = "Chemistry Series"
    ),
    (
        PHYS101 and PHYS102 and PHYS103
        with description = "Physics Series"
    )
    with description = "Two of Biology, Chemistry, or Physics Series".


    take (GEOL206 or GEOL208B) and GEOL221 and GEOL222 with description = "Second Year Courses".

    take GeologyElectives where courses >= 4, credits >= 12, with description = "Geology Electives".

    satisfy GenEd and satisfy Graduation.

END
```

It can be exported into JSON that would look similar to this

```json
{
  "requirements": {
    "format": "hobsons/chute",
    "version": "1.2",
    "contents": {
      "Geology": {
      "type": "PLAN",
      "name": "AAS-Geology",
      "display": "Associate of Science - Geology",
      "annotations": {
        "description": {
          "operator": "=",
          "value": "Two Year Geology Program resulting in a Associate of Science upon completion."
        },
        "catalogYear": {
          "operator": ">=",
          "value": "2019"
        },
        "gpa": {
          "operator": ">=",
          "value": "1.7"
        },
      },
      "statements": [
        {
          "take": {
            "id": "ENGL101"
          }
        },
        {
          "take": {
            "and": [
              {
                "and": [
                  {
                    "id": "GEOL101"
                  },
                  {
                    "or": [
                      {
                        "id": "GEOL102"
                      },
                      {
                        "id": "GEOL113b"
                      }
                    ]
                  }
                ]
              },
              {
                "or": [
                  {
                    "id": "GEOL103"
                  },
                  {
                    "id": "GEOL113b"
                  }
                ]
              }
            ],
            "with": [
              {
                "name": "description",
                "operator": "=",
                "value": "First Year Courses"
              },
              {
                "name": "targetYear",
                "operator": "=",
                "value": "1"
              }
            ],
            "where": [
              {
                "name": "grade",
                "operator": ">=",
                "value": "2.0"
              }
            ]
          }
        },
        {
          "take": {
            "list": [
              {
                "id": "GEOL11*"
              },
              {
                "id": "GEOL12*"
              }
            ],
            "with": [
              {
                "name": "description",
                "operator": "=",
                "value": "First Year Electives"
              }
            ],
            "where": [
              {
                "name": "courses",
                "operator": ">=",
                "value": "2"
              },
              {
                "name": "credits",
                "operator": ">=",
                "value": "4"
              }
            ]
          }
        },
        {
          "take": {
            "amount": "2",
            "list": [
              {
                "and": [
                  {
                    "and": [
                      {
                        "id": "BIOL101"
                      },
                      {
                        "list": [
                          {
                            "id": "BIO102a"
                          },
                          {
                            "id": "BIO102b"
                          },
                          {
                            "id": "BIO102c"
                          }
                        ],
                        "where": [
                          {
                            "name": "courses",
                            "operator": ">=",
                            "value": "2"
                          },
                          {
                            "name": "credits",
                            "operator": ">=",
                            "value": "4"
                          }
                        ]
                      }
                    ]
                  },
                  {
                    "id": "BIOL103"
                  }
                ],
                "with": [
                  {
                    "name": "description",
                    "operator": "=",
                    "value": "Biology Series"
                  }
                ]
              },
              {
                "and": [
                  {
                    "and": [
                      {
                        "id": "CHEM101"
                      },
                      {
                        "id": "CHEM102"
                      }
                    ]
                  },
                  {
                    "id": "CHEM103"
                  }
                ],
                "with": [
                  {
                    "name": "description",
                    "operator": "=",
                    "value": "Chemistry Series"
                  }
                ]
              },
              {
                "and": [
                  {
                    "and": [
                      {
                        "id": "PHYS101"
                      },
                      {
                        "id": "PHYS102"
                      }
                    ]
                  },
                  {
                    "id": "PHYS103"
                  }
                ],
                "with": [
                  {
                    "name": "description",
                    "operator": "=",
                    "value": "Physics Series"
                  }
                ]
              }
            ],
            "with": [
              {
                "name": "description",
                "operator": "=",
                "value": "Two of Biology, Chemistry, or Physics Series"
              }
            ]
          }
        },
        {
          "take": {
            "and": [
              {
                "and": [
                  {
                    "or": [
                      {
                        "id": "GEOL206"
                      },
                      {
                        "id": "GEOL208B"
                      }
                    ]
                  },
                  {
                    "id": "GEOL221"
                  }
                ]
              },
              {
                "id": "GEOL222"
              }
            ],
            "with": [
              {
                "name": "description",
                "operator": "=",
                "value": "Second Year Courses"
              }
            ]
          }
        },
        {
          "take": {
            "id": "GeologyElectives",
            "with": [
              {
                "name": "description",
                "operator": "=",
                "value": "Geology Electives"
              }
            ],
            "where": [
              {
                "name": "courses",
                "operator": ">=",
                "value": "4"
              },
              {
                "name": "credits",
                "operator": ">=",
                "value": "12"
              }
            ]
          }
        },
        {
          "and": [
            {
              "satisfy": {
                "name": "GenEd"
              }
            },
            {
              "satisfy": {
                "name": "Graduation"
              }
            }
          ]
        }
      ]
    }
  }
}
```