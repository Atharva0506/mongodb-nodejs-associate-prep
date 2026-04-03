# MongoDB Practice Question Patterns

## 1. Coverage Snapshot (30 Questions)
- CRUD operations and command correctness: 13 questions
- BSON/document model fundamentals: 4 questions
- Query operators and cursor usage: 3 questions
- Aggregation framework: 3 questions
- Indexes and performance: 5 questions
- Atlas Search: 2 questions
- Schema design: 1 question
- Node.js driver and pooling: 3 questions

## 2. Repeated Concepts
- Exact method-name correctness (`findOne`, `updateMany`, `deleteMany`, `findOneAndDelete`).
- Correct options object usage (`upsert: true`, one-vs-many method selection).
- `_id` uniqueness and insert validity.
- Document-shape outcomes after `replaceOne` vs `$set` updates.
- Compound index matching for filter + sort, including reverse sort support.
- Aggregation stage composition (`$group` then `$match`, `$sort` then `$limit`).
- Atlas Search query keys (`path`, `query`) and autocomplete tokenization (`edgeGram`).

## 3. Frequently Tested Topics (By Weight)
- High priority:
- CRUD syntax and behavior
- Index design for query/sort performance
- Medium priority:
- Aggregation output reasoning
- Query operators and cursor handling
- Node.js driver API correctness
- Targeted priority:
- BSON/document basics
- Atlas Search syntax
- Schema design embedding choice

## 4. Question Pattern Types
- API-validity MCQ: choose syntactically and semantically valid command.
- Output-based reasoning: predict result documents after update/aggregation/query.
- Scenario-based design: pick best schema/index/query from a business requirement.
- Select-multiple pattern: choose exactly two correct options.
- Trap-based distractors:
- Fake method names (`findMany`, `updateMulti`, `runAggregation`).
- Wrong key names (`field` vs `path`, `$upsert` vs `upsert`).
- Invalid literals/syntax (`True`, `1KB`, assignment in object literals).

## 5. Difficulty Assessment
- Overall difficulty: Easy to Medium.
- Easy:
- Pure syntax recognition and method-name questions.
- Medium:
- Compound index + sort compatibility.
- Aggregation output calculations after grouping.
- Data-modeling decision questions based on access pattern.

## 6. Recommended Study Priority
1. CRUD command semantics and one-vs-many method behavior.
2. Compound indexes for equality + sort queries, plus `getIndexes()` interpretation.
3. Aggregation stage order and output prediction (`$group`, `$match`, `$sort`, `$limit`).
4. Query operators (`$and`, `$or`, `$nin`, comparison operators) and cursor materialization.
5. Node.js driver API essentials (`MongoClient`, `db`, `close`, `updateMany`, `aggregate`).
6. Atlas Search essentials (`$search`, `text`, `path`, `query`, `edgeGram`).
7. Schema design fundamentals for embedding bounded, frequently-read data.

## 7. Topics Not Covered in This Set
- Transactions
- Sharding
- Validation rules / JSON Schema validators
- Replication internals

These topics should be studied separately for full exam readiness, but they are not evidenced in this 30-question practice set.
