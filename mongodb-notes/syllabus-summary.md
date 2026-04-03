# MongoDB Associate Developer Exam Syllabus (Official PDF Aligned)

[Home](../README.md) | [Notes Index](README.md)

Source used for alignment in this repo:
- `MongodDBAssociateDeveloperExamGuide.docx3.pdf`

## Exam Format
- Questions: 53
- Types: Multiple Choice, Multiple Response
- Time: 75 minutes

## Domain Weights
- Section 1: MongoDB Overview and the Document Model: 8%
- Section 2: CRUD: 51%
- Section 3: Indexes: 17%
- Section 4: Data Modeling: 4%
- Section 5: Tools and Tooling: 2%
- Section 6: Drivers: 18%

## Jump to Sections
- [Section 1: Overview and Document Model](01-overview-document-model/README.md)
- [Section 2: CRUD](02-crud/README.md)
- [Section 3: Indexes](03-indexes/README.md)
- [Section 4: Data Modeling](04-data-modeling/README.md)
- [Section 5: Tools and Tooling](05-tools-and-tooling/README.md)
- [Section 6: Drivers - Node.js](06-drivers-nodejs/README.md)

## Objectives Map

### Section 1: MongoDB Overview and the Document Model
- 1.1 Identify BSON value types supported by MongoDB.
- 1.2 Identify which differently shaped documents can co-exist in one collection.

### Section 2: CRUD
- 2.1 Insert command correctness.
- 2.2 Replace-style update output and state change.
- 2.3 `$set` update output and state change.
- 2.4 Upsert command selection.
- 2.5 Multi-document update expression.
- 2.6 findAndModify concurrent scenario output.
- 2.7 Delete expression selection.
- 2.8 Single-document equality lookup expression.
- 2.9 Equality constraint on array field matching.
- 2.10 Relational operator matching.
- 2.11 `$in` matching.
- 2.12 `$elemMatch` matching.
- 2.13 Logical-operator expression matching.
- 2.14 Query output with sort and limit.
- 2.15 Incorrect projection identification.
- 2.16 Get all results from cursor.
- 2.17 Count expressions for query matches.
- 2.18 Define a search index.
- 2.19 Choose correct search query.
- 2.20 Aggregation output using `$match` and `$group`.
- 2.21 Aggregation output using `$lookup`.
- 2.22 Aggregation output using `$out`.

### Section 3: Indexes
- 3.1 Best index to remove collection scan.
- 3.2 Best index for equality match on array field.
- 3.3 Best index for no-constraint query with two-field sort.
- 3.4 Identify index count on a collection.
- 3.5 Index trade-offs and effects of deleting indexes.
- 3.6 Explain outputs indicating performance/index issues.

### Section 4: Data Modeling
- 4.1 Parent/children embed vs link decisions.
- 4.2 Identify anti-pattern data models.

### Section 5: Tools and Tooling
- 5.1 Load Atlas Sample Dataset and use Data Explorer to find first document in a collection.

### Section 6: Drivers (Node.js track for this repo)
- 6.1 Define what the Node.js driver is.
- 6.2 How an application connects/uses the driver.
- 6.3 URI components used by `MongoClient`.
- 6.4 Connection pooling and advantages.
- 6.5 Insert one/many syntax.
- 6.6 Update one/many syntax.
- 6.7 Delete one/many syntax.
- 6.8 Find one/many syntax.
- 6.9 Aggregation pipeline syntax.
- 6.10 MQL syntax vs Aggregation syntax differences.

## Out-of-Scope Cleanup
The previous repo content included topics not listed in this PDF guide (for example transactions, sharding, replication internals, validation domains). Those sections were removed to keep this repo strictly aligned to the provided official guide.
