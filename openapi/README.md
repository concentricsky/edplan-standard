<h1 id="Portable-Education-Plan-schema-definitions">Portable Education Plan schema definitions v0.0.1</h1>

# Schemas

<h2 id="tocSidealizedpathway">IdealizedPathway</h2>

<a id="schemaidealizedpathway"></a>

```json
{
  "awardGoal": {
    "programID": "string",
    "status": {
      "active": true,
      "lastUpdated": "2018-12-04T05:59:06Z"
    },
    "awardName": "string",
    "awardId": "string",
    "issuer": "string",
    "anticipatedTimeToCompletion": {
      "count": 0,
      "unit": "days"
    }
  },
  "programPlan": {
    "issuer": "string",
    "requirements": {
      "format": "string",
      "version": "string",
      "contents": {}
    },
    "incrementalObjectives": [
      {}
    ],
    "suggestedSequences": [
      [
        {
          "number": "string",
          "unique": "string",
          "section": "string",
          "termId": "string"
        }
      ]
    ]
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|awardGoal|[AwardGoal](#schemaawardgoal)|false|none|none|
|programPlan|[ProgramPlan](#schemaprogramplan)|false|none|none|

<h2 id="tocSawardgoal">AwardGoal</h2>

<a id="schemaawardgoal"></a>

```json
{
  "programID": "string",
  "status": {
    "active": true,
    "lastUpdated": "2018-12-04T05:59:06Z"
  },
  "awardName": "string",
  "awardId": "string",
  "issuer": "string",
  "anticipatedTimeToCompletion": {
    "count": 0,
    "unit": "days"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|programID|string|false|none|none|
|status|object|false|none|none|
|» active|boolean|false|none|none|
|» lastUpdated|string(date-time)|false|none|none|
|awardName|string|false|none|Claim/Award/Certificate Name|
|awardId|string|false|none|Claim/Award/Certificate ID|
|issuer|string|false|none|none|
|anticipatedTimeToCompletion|object|false|none|none|
|» count|number|false|none|none|
|» unit|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|unit|days|
|unit|weeks|
|unit|months|
|unit|years|
|unit|quarters|
|unit|semesters|

<h2 id="tocSprogramplan">ProgramPlan</h2>

<a id="schemaprogramplan"></a>

```json
{
  "issuer": "string",
  "requirements": {
    "format": "string",
    "version": "string",
    "contents": {}
  },
  "incrementalObjectives": [
    {}
  ],
  "suggestedSequences": [
    [
      {
        "number": "string",
        "unique": "string",
        "section": "string",
        "termId": "string"
      }
    ]
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|issuer|string|false|none|none|
|requirements|[RequirementsSpec](#schemarequirementsspec)|false|none|none|
|incrementalObjectives|[[IncrementalObjective](#schemaincrementalobjective)]|false|none|none|
|suggestedSequences|[[AcademicSequence](#schemaacademicsequence)]|false|none|none|

<h2 id="tocSrequirementsspec">RequirementsSpec</h2>

<a id="schemarequirementsspec"></a>

```json
{
  "format": "string",
  "version": "string",
  "contents": {}
}

```

*Academic and non-academic requirements expressed in one of potentially multiple json formats / versions.
*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|format|string|false|none|none|
|version|string|false|none|none|
|contents|object|false|none|none|

<h2 id="tocSincrementalobjective">IncrementalObjective</h2>

<a id="schemaincrementalobjective"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocSacademicsequence">AcademicSequence</h2>

<a id="schemaacademicsequence"></a>

```json
[
  {
    "number": "string",
    "unique": "string",
    "section": "string",
    "termId": "string"
  }
]

```

### Properties

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[PlannedCourse](#schemaplannedcourse)|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[PlannedActivity](#schemaplannedactivity)|false|none|none|

<h2 id="tocSplannedcourse">PlannedCourse</h2>

<a id="schemaplannedcourse"></a>

```json
{
  "number": "string",
  "unique": "string",
  "section": "string",
  "termId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|number|string|false|none|none|
|unique|string|false|none|none|
|section|string|false|none|none|
|termId|string|false|none|none|

<h2 id="tocSplannedactivity">PlannedActivity</h2>

<a id="schemaplannedactivity"></a>

```json
{
  "name": "string",
  "value": "string",
  "termId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|false|none|none|
|value|string|false|none|none|
|termId|string|false|none|none|

<h2 id="tocSpersonalizedpathway">PersonalizedPathway</h2>

<a id="schemapersonalizedpathway"></a>

```json
{
  "transcriptData": [
    {
      "number": "string",
      "unique": "string",
      "section": "string",
      "grade": "string",
      "numericGrade": 0,
      "credits": 0,
      "termId": "string"
    }
  ],
  "idealizedPlanReference": {
    "id": "string",
    "name": "string"
  },
  "awardGoal": {
    "programID": "string",
    "status": {
      "active": true,
      "lastUpdated": "2018-12-04T05:59:06Z"
    },
    "awardName": "string",
    "awardId": "string",
    "issuer": "string",
    "anticipatedTimeToCompletion": {
      "count": 0,
      "unit": "days"
    },
    "anticipatedAwardCompletionDate": "2018-12-04T05:59:06Z",
    "credentialAwardIssueDate": "2018-12-04T05:59:06Z"
  },
  "personalizedProgramPlan": {
    "additionalProperties": [
      {
        "number": "string",
        "unique": "string",
        "section": "string",
        "termId": "string"
      }
    ]
  },
  "remainingSteps": {
    "format": "string",
    "version": "string",
    "contents": {}
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|transcriptData|[[StudentRecord](#schemastudentrecord)]|false|none|none|
|idealizedPlanReference|object|false|none|none|
|» id|string|false|none|none|
|» name|string|false|none|none|
|awardGoal|[PersonalizedAwardGoal](#schemapersonalizedawardgoal)|false|none|none|
|personalizedProgramPlan|[PersonalizedProgramPlan](#schemapersonalizedprogramplan)|false|none|none|
|remainingSteps|[RequirementsSpec](#schemarequirementsspec)|false|none|none|

<h2 id="tocSpersonalizedawardgoal">PersonalizedAwardGoal</h2>

<a id="schemapersonalizedawardgoal"></a>

```json
{
  "programID": "string",
  "status": {
    "active": true,
    "lastUpdated": "2018-12-04T05:59:06Z"
  },
  "awardName": "string",
  "awardId": "string",
  "issuer": "string",
  "anticipatedTimeToCompletion": {
    "count": 0,
    "unit": "days"
  },
  "anticipatedAwardCompletionDate": "2018-12-04T05:59:06Z",
  "credentialAwardIssueDate": "2018-12-04T05:59:06Z"
}

```

### Properties

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

<h2 id="tocSpersonalizedprogramplan">PersonalizedProgramPlan</h2>

<a id="schemapersonalizedprogramplan"></a>

```json
{
  "additionalProperties": [
    {
      "number": "string",
      "unique": "string",
      "section": "string",
      "termId": "string"
    }
  ]
}

```

*List of courses and activities by termId value as key*

### Properties

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

<h2 id="tocSstudentrecord">StudentRecord</h2>

<a id="schemastudentrecord"></a>

```json
{
  "number": "string",
  "unique": "string",
  "section": "string",
  "grade": "string",
  "numericGrade": 0,
  "credits": 0,
  "termId": "string"
}

```

### Properties

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

<h2 id="tocScourserecord">CourseRecord</h2>

<a id="schemacourserecord"></a>

```json
{
  "number": "string",
  "unique": "string",
  "section": "string",
  "grade": "string",
  "numericGrade": 0,
  "credits": 0,
  "termId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|number|string|false|none|none|
|unique|string|false|none|none|
|section|string|false|none|none|
|grade|string|false|none|none|
|numericGrade|number|false|none|none|
|credits|number|false|none|none|
|termId|string|false|none|none|

<h2 id="tocStestrecord">TestRecord</h2>

<a id="schematestrecord"></a>

```json
{
  "name": "string",
  "score": 0,
  "value": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|false|none|none|
|score|number|false|none|none|
|value|string|false|none|none|

<h2 id="tocSotherrecord">OtherRecord</h2>

<a id="schemaotherrecord"></a>

```json
{
  "name": "string",
  "value": "string",
  "operator": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|false|none|none|
|value|string|false|none|none|
|operator|string|false|none|none|

