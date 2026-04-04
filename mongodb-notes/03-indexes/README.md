# Section 3: Indexes (17%)

[Home](../../README.md) | [Notes Index](../README.md) | [Previous: Section 2](../02-crud/README.md) | [Next: Section 4](../04-data-modeling/README.md)

Official references:
- https://www.mongodb.com/docs/manual/indexes/
- https://www.mongodb.com/docs/manual/reference/explain-results/
- https://www.mongodb.com/docs/manual/core/indexes/index-types/index-compound/

This section focuses on choosing index keys that match query and sort shape, then verifying behavior with explain.

## Objective 3.1: Improve query performance with correct index

For filter + sort queries, align compound index order to query shape.

Example query:

```javascript
db.people.find({ employer: "ABC" }).sort({ last_name: 1, job: 1 });
```

Recommended index:

```javascript
db.people.createIndex({ employer: 1, last_name: 1, job: 1 });
```

Expected output from createIndex:

```javascript
employer_1_last_name_1_job_1
```

## Objective 3.2: Equality match on array field

Index the queried array path. MongoDB creates a multikey index.

Syntax:

```javascript
db.collection.createIndex({ "objs.a": 1 });
db.collection.find({ "objs.a": 1 });
```

Expected behavior:
- Query can use IXSCAN instead of COLLSCAN when index is selective.

## Objective 3.3: No-filter query with two-field sort

If query is only sorting by product then price, index key order must match field order.

Syntax:

```javascript
db.coll.createIndex({ product: 1, price: 1 });
db.coll.find({}).sort({ product: 1, price: 1 });
```

Expected behavior:
- Index supports sort order without in-memory sort for matching direction.

## Index types you must know for exam and real projects

MongoDB exam questions mostly focus on single, compound, and multikey indexes, but you should also recognize other types and when they are used.

### 1) Single-field index

Syntax:

```javascript
db.users.createIndex({ email: 1 });
```

Best for:
- Equality/range/sort on one field.

### 2) Compound index

Syntax:

```javascript
db.people.createIndex({ employer: 1, last_name: 1, job: 1 });
```

Best for:
- Queries that filter and sort across multiple fields.
- Field order is critical.

### 3) Multikey index (array field)

Syntax:

```javascript
db.orders.createIndex({ tags: 1 });
db.orders.createIndex({ "items.sku": 1 });
```

Best for:
- Matching values inside arrays.

### 4) Text index

Syntax:

```javascript
db.articles.createIndex({ title: "text", body: "text" });
```

Best for:
- Keyword text search with $text.

### 5) Geospatial indexes

2dsphere example:

```javascript
db.places.createIndex({ location: "2dsphere" });
```

Best for:
- Location queries such as $near and geo intersections.

### 6) Hashed index

Syntax:

```javascript
db.events.createIndex({ userId: "hashed" });
```

Best for:
- Hash-based distribution and sharding use cases.

### 7) Wildcard index

Syntax:

```javascript
db.catalog.createIndex({ "$**": 1 });
```

Best for:
- Dynamic/unpredictable fields where indexing every path manually is difficult.

### 8) TTL index

Syntax:

```javascript
db.sessions.createIndex({ expiresAt: 1 }, { expireAfterSeconds: 0 });
```

Best for:
- Automatic deletion of expired documents.

### 9) Unique index

Syntax:

```javascript
db.users.createIndex({ username: 1 }, { unique: true });
```

Best for:
- Enforcing uniqueness constraint at database level.

### 10) Partial and sparse indexes

Partial syntax:

```javascript
db.orders.createIndex(
  { status: 1, createdAt: -1 },
  { partialFilterExpression: { archived: false } }
);
```

Sparse syntax:

```javascript
db.profiles.createIndex({ phone: 1 }, { sparse: true });
```

Best for:
- Indexing subsets of documents to reduce index size and write cost.

Exam reminder:
- Atlas Search index definitions are different from standard createIndex indexes.

## Objective 3.4: Identify number of indexes on a collection

Syntax:

```javascript
db.inventory.getIndexes();
```

Expected output shape:

```javascript
[
  { v: 2, key: { _id: 1 }, name: "_id_" },
  { v: 2, key: { sku: 1 }, name: "sku_1" }
]
```

Count index definitions in the returned array.

## Objective 3.5: Index trade-offs and drop impact

Read benefit:
- Faster reads for supported query patterns.

Write/storage cost:
- Extra disk usage.
- Additional memory use.
- Slower writes due to index maintenance.

Dropping an index:

```javascript
db.people.dropIndex("employer_1_last_name_1_job_1");
```

Expected output:

```javascript
{ nIndexesWas: 2, ok: 1 }
```

After dropping, queries that depended on that index may fall back to COLLSCAN.

## Objective 3.6: Explain output interpretation

Syntax:

```javascript
db.people
  .find({ employer: "ABC" })
  .sort({ last_name: 1, job: 1 })
  .explain("executionStats");
```

Fields to inspect:
- queryPlanner.winningPlan
- executionStats.nReturned
- executionStats.totalKeysExamined
- executionStats.totalDocsExamined

Signals:
- IXSCAN usually indicates index use.
- COLLSCAN indicates full collection scan.
- High totalDocsExamined compared to nReturned suggests poor selectivity/index fit.

Expected winningPlan examples:

```javascript
{ stage: "COLLSCAN", ... }
```

or

```javascript
{ stage: "FETCH", inputStage: { stage: "IXSCAN", indexName: "employer_1_last_name_1_job_1" } }
```

## Quick command toolbox

```javascript
db.people.createIndex({ employer: 1, last_name: 1, job: 1 });
db.people.getIndexes();
db.people.dropIndex("employer_1_last_name_1_job_1");
db.people.find({ employer: "ABC" }).sort({ last_name: 1, job: 1 }).explain("executionStats");
```

## Common exam traps
- Right fields but wrong compound key order.
- Assuming index direction and sort direction always match.
- Assuming more indexes always improve performance.
- Mixing Atlas Search index concepts with regular B-tree indexes.

## MCQ practice (with answers and reasoning)

### Q3.1
For query `db.people.find({ employer: "ABC" }).sort({ last_name: 1, job: 1 })`, best index is:
- A. `{ last_name: 1, job: 1, employer: 1 }`
- B. `{ employer: 1, last_name: 1, job: 1 }`
- C. `{ employer: 1 }`
- D. `{ job: 1, last_name: 1 }`
Answer: B
Reason: Equality field first, then sort fields in order.

### Q3.2
Best index for `db.collection.find({ "objs.a": 1 })`:
- A. `{ objs: 1 }`
- B. `{ a: 1 }`
- C. `{ "objs.a": 1 }`
- D. `{ objs: 1, a: 1 }`
Answer: C
Reason: Index key must match queried path.

### Q3.3
For `find({}).sort({ product: 1, price: 1 })`, which two indexes can support sort order?
- A. `{ product: 1, price: 1 }`
- B. `{ product: 1, price: -1 }`
- C. `{ product: -1, price: 1 }`
- D. `{ product: -1, price: -1 }`
Answer: A and D
Reason: Same field order is required; full reverse direction can also be used.

### Q3.4
Command that lists index definitions:
- A. `db.inventory.showIndexes()`
- B. `db.inventory.getIndexes()`
- C. `db.inventory.indexesList()`
- D. `db.inventory.displayIndexes()`
Answer: B
Reason: getIndexes() is the valid method.

### Q3.5
What does COLLSCAN in explain usually indicate?
- A. Query used an index efficiently
- B. Query scanned whole collection
- C. Query is sharded
- D. Query is cached
Answer: B
Reason: COLLSCAN means full collection scan.

### Q3.6
Which index type is created for an array field automatically when indexed?
- A. text
- B. wildcard
- C. multikey
- D. hashed
Answer: C
Reason: Array field indexing creates a multikey index.

### Q3.7
Which index type is used for `$text` queries?
- A. hashed
- B. text
- C. 2dsphere
- D. sparse
Answer: B
Reason: $text uses text indexes.

### Q3.8
Which index type supports auto-expiring old documents?
- A. wildcard
- B. unique
- C. TTL
- D. partial
Answer: C
Reason: TTL uses expireAfterSeconds to remove expired docs.

### Q3.9
Main write-side trade-off of adding many indexes:
- A. Writes can become slower
- B. Writes always become faster
- C. _id uniqueness is disabled
- D. BSON size decreases
Answer: A
Reason: Every write must maintain all affected indexes.

### Q3.10
If `totalDocsExamined` is much larger than `nReturned`, likely interpretation is:
- A. Efficient index targeting
- B. Potentially poor selectivity/index fit
- C. No index exists on collection
- D. Query failed
Answer: B
Reason: High scanned-to-returned ratio suggests inefficiency.

### Q3.11
Which command creates a unique index on username?
- A. `db.users.createIndex({ username: 1, unique: true })`
- B. `db.users.createIndex({ username: 1 }, { unique: true })`
- C. `db.users.createUnique({ username: 1 })`
- D. `db.users.addIndex({ username: 1 }, { unique: true })`
Answer: B
Reason: unique is an index option in the second argument.

### Q3.12
Atlas Search indexes are created with:
- A. `db.collection.createIndex(...)`
- B. `db.collection.searchIndex(...)`
- C. Atlas Search index definitions in Atlas UI/API
- D. `db.collection.createTextSearch(...)`
Answer: C
Reason: Atlas Search indexes are separate from regular B-tree indexes.

Detailed bank across all sections: [All-Sections Detailed MCQ Bank](../all-sections-detailed-mcq-bank.md#section-3-indexes)
Pattern analysis of official options: [Question Pattern Analysis](../question-pattern-analysis.md)

[Previous: Section 2](../02-crud/README.md) | [Back to Notes Index](../README.md) | [Next: Section 4](../04-data-modeling/README.md)
