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

[Previous: Section 2](../02-crud/README.md) | [Back to Notes Index](../README.md) | [Next: Section 4](../04-data-modeling/README.md)
