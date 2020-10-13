# Notes from meeting w/Amazon 16 January

* There is a need to display analyzed aggregate data about paths students have taken before that are anonymized.
  * Is transferring aggregate data in scope for a data model here?
  * If so, is that part of the IdealizedPathway object or is it a sanitized version of a PersonalizedPathway object?
  * Alex: describing Omid's intention he is definitely talking about individualized PersonalizedPathway objects, not aggregate data. "Show me the 10 student paths closest to my current progress or current recorded intentions".
* The Shortest Path effort may need to publish some data resulting from analysis of many PersonalizedPathways for a particular IdealizedPathway. This aggregate data will be interpreted in relevant tools.
  * There will be some analysis done on the raw data of PersonalizedPathways. This data will also be transferred between systems. That's probably out of scope for EdPlan Standard
  * When these analysis formats are created, there will be an effort to represent common elements in common ways.
  * These aggregate analyses may be used to generate additional recommended sequences that are in scope for IdealizedPathway.
* Omid: Ideally, this information about Courses requirements of a program, etc should already be in COCI database. Not just in PDFs.
* In IdealizedPathway, Course should be associated with CID designator. (also in PlannedCourse).
* CID designator and program completion requirements are already in scope for EdPlan IdealizedPathway.
* Sharing aggregate reported data to CCC Chancellor's Office data lake will be a small lift (easy). No need to bike shed it too much on this call. Data will exist in data lake for future analysis as well as consumable by Program Mapper.
* It is important that the vendors using EdPlan Standard are able to represent edplans in their system in the common standard.
