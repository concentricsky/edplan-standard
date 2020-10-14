---
layout: home
---
# EdPlan Standard
{{site.description}}

View the EdPlan Standard API Definition [here](./edplan-standard.yaml). This
file is compatible with OpenAPI 3.0.

Table of contents
=================


 * [Design Principles](#design-principles)
    * [Acceptance Criteria](#acceptance-criteria)
 * [Data Model](#data-model)
    * [IdealizedPathway](#idealizedpathway)
      * [AwardGoal](#awardgoal)
      * [ProgramPlan](#programplan)
      * [RequirementsSpec](#requirementspec)
      * [AcademicSequence](#academicsequence)
      * [PlannedCourse](#plannedcourse)
      * [PlannedActivity](#plannedactivity)
      * [PersonalizedPathway](#personlizedpathway)
      * [PersonalizedAwardGoal](#personalizedawardgoal)
      * [PersonalizedProgramPlan](#personalizedprogramplan)
      * [StudentRecord](#studentrecord)
      * [CourseRecord](#courserecord)
      * [TestRecord](#testrecord)
      * [OtherRecord](#otherrecord)
 * [Example Requirements](#example-requirements)
 * [API](#api)
    * [Get Idealized EdPlans](#get-idealized-edplans)
    * [Get Student EdPlans](#get-student-edplans)


## Design Principles
We want to create an initial cut of the EdPlan Standard that is immediately usable. This data should be able to move between the relevant services that implement edplans. It must be a practical implementation to hold minimum viable versions of the edplan data created and understood in the participating vendors' products. This means the data model should be compatible with the data models in existing products from the start.

### Acceptance Criteria
* Is this model workable to move a minimum viable version of our edplan data into and out of a repository?

## Data Model

### IdealizedPathway {#IdealizedPathway}

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|awardGoal|[AwardGoal](#schemaawardgoal)|false|none|none|
|programPlan|[ProgramPlan](#schemaprogramplan)|false|none|none|

```json
{
  "awardGoal": {
    "programID": "AAS-Geology-2019",
    "status": {
      "active": true,
      "lastUpdated": "2018-11-21T22:45:50.493Z"
    },
    "awardName": "Associate of Science",
    "awardId": "AAS-2019",
    "issuer": "4cd",
    "anticipatedTimeToCompletion": {
      "count": 2,
      "unit": "years"
    }
  },
  "issuers": [
    {
      "id": "https://school.edu",
      "ope": 123456,
      "duns": 123456789,
      "name": "School of Edu"
    }
  ],
  "programPlan": {
    "issuer": "https://school.edu",
    "requirements": {
      "format": "https://example.com/HobsonsChuteFormat",
      "version": "1.2",
      "contents": {}
    },
    "incrementalObjectives": [
      {}
    ],
    "suggestedSequences": [
      [
        {
          "number": "MATH101",
          "unique": "MATH101.A.2019-2",
          "section": "1c",
          "termId": "Spring2019"
        }
      ]
    ]
  }
}

```

#### AwardGoal {#AwardGoal}

*Issues*:
* It would be good to create a IRI for identifier values if we're delivering a JSON-LD spec. ProgramID is the first such value.
* Should ProgramID be a URL, or can we get a URL for every program ID?
  * Probably we cannot do this.
  * We could create a URL structure ending in the chancellor's office assigned COCI id.
  * Job skills certificates offered aren't in COCI, so that method might not work for all programs to translate a string into a URL.
* We could use a different format IRI, such as `urn:uuid`. Schools are the responsible party for creating this. Could they be responsible for tracking a locally unique identifier that is in an IRI format.
  * Hearing that schools can track a unique id, but not yet hearing that they could do so in an IRI format consistently.
  * Identifier does need to be scoped to the school.
* `name` might be able to be used instead of `awardName`
* Identify cases for there being variation between program and award. Think through how these would be expressed.
* Need to figure out how Issuers are identified. COCI college identifier is one thing to look at. e.g. `bakersfield`
  * One case: There are multiple campuses for a particular institution. The locations have variations on requirements for degree goals based on availability at those different campuses. In that case, the issuer entity is probably the broader institution, but the programID might come from the subsidiary campus.
* Possible sources for program/institutional IDs: DUNS, OPE, FICE (sp); There are 12-15 options in XML approved format for electronic transcripts
* Consider coordinated format between estimatedTimeToCompletion and issuer identification.


```json
{
  "id": "https://school.edu/awards/someaward123",
  "name": "Associate in Science for Transfer Somewhere",
  "programId": "https://school.edu/programs/9fa84564-e9f5-49a3-a0ae-a0f80b534f6e",
  "status": {
    "active": true,
    "lastUpdated": "2018-11-21T22:45:50.493Z"
  },
  "issuer": "https://school.edu",
  "anticipatedTimeToCompletion": {
    "count": 10,
    "unit": "terms"
  }
}
```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Claim/Award/Certificate ID|
|name|string|false|none|Claim/Award/Certificate Name|
|programId|string|false|none|none|
|status|object|false|none|none|
|» active|boolean|false|none|none|
|» lastUpdated|string(date-time)|false|none|none|
|issuer|string|false|none|none|
|anticipatedTimeToCompletion|object|false|none|none|
|» count|number|false|none|none|
|» unit|string|false|none|none|

##### unit: Enumberated Values

*Issues*
* How do short summer or winter sessions work into this?
* Goal: keep it simple and enable comparison. Perfect time accuracy is not the point of this component.
* Should we have a Term object that has more metadata? That would add complexity and undercut the goal.
* Option: identify the type of terms specifically. "A term of length 10weeks followed by a term of length 3weeks"
* Should hours be added? (actual hours, less than a day. "A 4 hour certificate". Not comparable to 4 credit hours)

|Property|Value|
|----|----|
|unit|hours|
|unit|days|
|unit|weeks|
|unit|months|
|unit|years|
|unit|quarters|
|unit|semesters|
|unit|trimester|
|unit|terms|

#### Program Plan {#ProgramPlan}
**Issues**
* The types of subsidiary achievements that might be
* How to represent an elective that doesn't have a defined course (like a pick at least 6 credits from this category of courses)
* Additional requirements
* How can requirements for incremental objectives be expressed
* There are three types of requirements:
  - Course requirements,
  - non-course requirements (GPA, residency), non-academic requirements (things that happen outside of the classroom that are still required)
  - Test scores: maybe it is time to call them out specifically
* Suggestion that for suggestedSequences, you might as well not leave the boxes open to be able to describe different options. Let the student drop their own version in, but for the suggestion, offer a concrete course. Might present information overload for students otherwise. Suggesting an actual concrete "default" suggestion cuts away information overload.
* It definitely makes sense to describe the purpose for having "suggestedSequences" in this spec. It should not recapitulate data that is already in "requirements".
  - It may still make sense to have "open-ended course descriptors" in this section to suggest "It is suggested to take a music elective before your higher ed math requirement".
* Suggested sequences might vary depending on start term. But how?
  - There seem to be strong reasons to have placeholder/open-ended course descriptors in the suggestedSequences area.
* The intent of this part of the document is to expose the completion requirements (not tied to specific terms) and successful path(s) through the requirements.



```json
{
  "issuer": "https://school.edu",
  "requirements": {
    "format": "hobsons/chute",
    "version": "1.2",
    "contents": {}
  },
  "incrementalObjectives": [
    {}
  ],
  "suggestedSequences": [
    [
      {
        "number": "MATH101",
        "unique": "MATH101.A.2019-2",
        "section": "1c",
        "termId": "Spring2019"
      }
    ]
  ]
}

```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|issuer|string|false|none|none|
|requirements|[RequirementsSpec](#schemarequirementsspec)|false|none|none|
|incrementalObjectives|[[IncrementalObjective](#schemaincrementalobjective)]|false|none|none|
|suggestedSequences|[[AcademicSequence](#schemaacademicsequence)]|false|none|none|

#### RequirementsSpec
*Academic and non-academic requirements expressed in one of potentially multiple json formats / versions.*

```json
{
  "format": "hobsons/chute",
  "version": "1.2",
  "contents": {}
}
```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|format|string|false|none|none|
|version|string|false|none|none|
|contents|object|false|none|none|

#### AcademicSequence {#AcademicSequence}

```json
[
  {
    "number": "MATH101",
    "unique": "MATH101.A.2019-2",
    "section": "1c",
    "termId": "Spring2019"
  }
]

```

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[PlannedCourse](#schemaplannedcourse)|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[PlannedActivity](#schemaplannedactivity)|false|none|none|

#### PlannedCourse

```json
{
  "number": "MATH101",
  "unique": "MATH101.A.2019-2",
  "section": "1c",
  "termId": "Spring2019"
}

```

**Issues**:
* CID should be referenced for Course.

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|number|string|false|none|none|
|unique|string|false|none|none|
|section|string|false|none|none|
|termId|string|false|none|none|

#### PlannedActivity

```json
{
  "name": "Graduation Paperwork",
  "value": "Complete",
  "termId": "Spring2019"
}

```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|false|none|none|
|value|string|false|none|none|
|termId|string|false|none|none|


### PersonalizedPathway {#PersonalizedPathway}

```json
{
  "transcriptData": [
    {
      "number": "MATH101",
      "unique": "MATH101.A.2019-2",
      "section": "1c",
      "grade": "B",
      "numericGrade": 3,
      "credits": 5,
      "termId": "Spring2017"
    }
  ],
  "idealizedPlanReference": {
    "id": "4cd.AAS-Geology-Ideal",
    "name": "AAS - Geology Pathway"
  },
  "awardGoal": {
    "programID": "AAS-Geology-2019",
    "status": {
      "active": true,
      "lastUpdated": "2018-11-21T22:45:50.493Z"
    },
    "awardName": "Associate of Science",
    "awardId": "AAS-2019",
    "issuer": "4cd",
    "anticipatedTimeToCompletion": {
      "count": 2,
      "unit": "years"
    },
    "anticipatedAwardCompletionDate": "2018-11-21T22:45:50.493Z",
    "credentialAwardIssueDate": "2018-11-21T22:45:50.493Z"
  },
  "personalizedProgramPlan": {
    "additionalProperties": [
      {
        "number": "MATH101",
        "unique": "MATH101.A.2019-2",
        "section": "1c",
        "termId": "Spring2019"
      }
    ]
  },
  "remainingSteps": {
    "format": "hobsons/chute",
    "version": "1.2",
    "contents": {}
  }
}

```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|transcriptData|[[StudentRecord](#schemastudentrecord)]|false|none|none|
|idealizedPlanReference|object|false|none|none|
|» id|string|false|none|none|
|» name|string|false|none|none|
|awardGoal|[PersonalizedAwardGoal](#schemapersonalizedawardgoal)|false|none|none|
|personalizedProgramPlan|[PersonalizedProgramPlan](#schemapersonalizedprogramplan)|false|none|none|
|remainingSteps|[RequirementsSpec](#schemarequirementsspec)|false|none|none|

### PersonalizedAwardGoal

```json
{
  "programID": "AAS-Geology-2019",
  "status": {
    "active": true,
    "lastUpdated": "2018-11-21T22:45:50.493Z"
  },
  "awardName": "Associate of Science",
  "awardId": "AAS-2019",
  "issuer": "4cd",
  "anticipatedTimeToCompletion": {
    "count": 2,
    "unit": "years"
  },
  "anticipatedAwardCompletionDate": "2018-11-21T22:45:50.493Z",
  "credentialAwardIssueDate": "2018-11-21T22:45:50.493Z"
}

```

*allOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[AwardGoal](#schemaawardgoal)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» anticipatedAwardCompletionDate|string(date-time)|false|none|none|
|» credentialAwardIssueDate|string(date-time)|false|none|none|

### PersonalizedProgramPlan

```json
{
  "additionalProperties": [
    {
      "number": "MATH101",
      "unique": "MATH101.A.2019-2",
      "section": "1c",
      "termId": "Spring2019"
    }
  ]
}

```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|additionalProperties|[oneOf]|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[PlannedCourse](#schemaplannedcourse)|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[PlannedActivity](#schemaplannedactivity)|false|none|none|

### StudentRecord

```json
{
  "number": "MATH101",
  "unique": "MATH101.A.2019-2",
  "section": "1c",
  "grade": "B",
  "numericGrade": 3,
  "credits": 5,
  "termId": "Spring2017"
}

```

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[CourseRecord](#schemacourserecord)|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[TestRecord](#schematestrecord)|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[OtherRecord](#schemaotherrecord)|false|none|none|

### CourseRecord

```json
{
  "number": "MATH101",
  "unique": "MATH101.A.2019-2",
  "section": "1c",
  "grade": "B",
  "numericGrade": 3,
  "credits": 5,
  "termId": "Spring2017"
}

```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|number|string|false|none|none|
|unique|string|false|none|none|
|section|string|false|none|none|
|grade|string|false|none|none|
|numericGrade|number|false|none|none|
|credits|number|false|none|none|
|termId|string|false|none|none|

### TestRecord

```json
{
  "name": "Math Placement 1",
  "score": 74,
  "value": "MATH101 placement"
}

```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|false|none|none|
|score|number|false|none|none|
|value|string|false|none|none|

### OtherRecord

```json
{
  "name": "Met With Counselor",
  "value": "Met 7/1/2018",
  "operator": "="
}

```

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|false|none|none|
|value|string|false|none|none|
|operator|string|false|none|none|

## Example Requirements

### Hobsons chute format

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


## API
The API consists of *n* endpoints. Clients get data about idealized EdPlans
published by institutions and

### Get Idealized EdPlans
A client may get idealized EdPlans that are published on the server. These are
assumed to be publicly available objects.

...

### Get Student EdPlans
This endpoint returns a set of one or more EdPlans associated with the
authenticated or identified students.

...
