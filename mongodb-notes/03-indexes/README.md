# Section 3: Indexes (17%)

Official references:
- https://www.mongodb.com/docs/manual/indexes/
- https://www.mongodb.com/docs/manual/reference/explain-results/
- https://www.mongodb.com/docs/manual/core/indexes/index-types/index-compound/

## Objective 3.1: Improve collection scan query performance with correct index

Match index shape to query shape:
- Equality/filter fields first
- Sort fields next (in useful order)

Practical pattern (ESR mindset):
- Equality predicates first
- Sort keys next
- Range predicates after sort when possible

Example:
```javascript
db.people.find({ employer: "ABC" }).sort({ last_name: 1, job: 1 });
db.people.createIndex({ employer: 1, last_name: 1, job: 1 });
```

## Objective 3.2: Equality match on array field

Index the queried array path (multikey index behavior).

Example:
```javascript
db.collection.find({ "objs.a": 1 });
db.collection.createIndex({ "objs.a": 1 });
```

Why it matters:
- Without index, query often degrades to collection scan.
- With multikey index, element-level lookups are faster.

## Objective 3.3: No-constraint query with two-field sort

If query is `find({}).sort({ product: 1, price: 1 })`, index should follow same key order.

Example:
```javascript
db.coll.createIndex({ product: 1, price: 1 });
```

Reverse traversal note:
- Complete reverse direction can also be usable for sort.

Sort compatibility trap:
- Same field order is mandatory.
- Mixed direction support depends on index direction alignment across sort keys.

## Objective 3.4: Identify number of indexes on a collection

```javascript
db.inventory.getIndexes();
```

Count the returned index definitions.

## Objective 3.5: Index trade-offs and index deletion impact

Benefits:
- Faster supported reads

Costs:
- More disk usage
- More memory pressure
- Slower writes (index maintenance)

Deleting useful index can cause:
- Query fallback to collection scan
- Higher response latency

Operational note:
- Removing rarely used indexes can improve write performance.
- Removing heavily used indexes hurts read performance.

## Objective 3.6: Explain outputs indicating potential issue

```javascript
db.people.find({ employer: "ABC" }).sort({ last_name: 1, job: 1 }).explain("executionStats");
```

Signals:
- `COLLSCAN`: potential index issue
- `IXSCAN`: index usage exists
- High docs examined vs returned: inefficiency

Additional explain fields worth reading:
- `nReturned`
- `totalKeysExamined`
- `totalDocsExamined`

Heuristic:
- If `totalDocsExamined` is much larger than `nReturned`, investigate indexing/filter selectivity.

## Index command toolbox

```javascript
// Create
db.people.createIndex({ employer: 1, last_name: 1, job: 1 });

// List
db.people.getIndexes();

// Drop by name
db.people.dropIndex("employer_1_last_name_1_job_1");

// Explain
db.people.find({ employer: "ABC" }).sort({ last_name: 1, job: 1 }).explain("executionStats");
```

## Common exam traps
- Correct fields, wrong compound order
- Sort direction mismatch
- Assuming more indexes always improves everything
- Confusing Atlas Search index definitions with regular B-tree indexes

## MCQ Practice (Section 3)

### Q3.1
For filter by employer and sort by last_name/job, best index is:
- A. `{ last_name: 1, job: 1, employer: 1 }`
- B. `{ employer: 1, last_name: 1, job: 1 }`
- C. `{ employer: 1 }`
- D. `{ job: 1, last_name: 1 }`
Answer: B

### Q3.2
Best index for query `{ "objs.a": 1 }`:
- A. `{ objs: 1 }`
- B. `{ a: 1 }`
- C. `{ "objs.a": 1 }`
- D. `{ objs: 1, a: 1 }`
Answer: C

### Q3.3
For `find({}).sort({product:1, price:1})`, which index is directly aligned?
- A. `{ price: 1, product: 1 }`
- B. `{ product: 1, price: 1 }`
- C. `{ product: 1 }`
- D. `{ price: -1 }`
Answer: B

### Q3.4
Command to list collection indexes:
- A. `showIndexes()`
- B. `indexesList()`
- C. `getIndexes()`
- D. `displayIndexes()`
Answer: C

### Q3.5
Common index trade-off:
- A. More indexes always speed writes
- B. More indexes can slow writes
- C. Indexes eliminate storage use
- D. Indexes are only for aggregation
Answer: B

### Q3.6
Explain output showing potential missing index pattern:
- A. `IXSCAN`
- B. `FETCH`
- C. `COLLSCAN`
- D. `LIMIT`
Answer: C

### Q3.7
If a useful index is deleted, likely effect is:
- A. Faster filtered reads
- B. Slower queries that depended on that index
- C. Automatic index recreation immediately
- D. BSON type changes
Answer: B

### Q3.8
Which statement is true about compound key order?
- A. Order is cosmetic only
- B. Order affects query and sort support
- C. MongoDB auto-fixes order at runtime
- D. Order matters only for unique indexes
Answer: B

### Q3.9
If explain shows `COLLSCAN`, most likely first action is:
- A. Add a query-aligned index
- B. Delete all indexes
- C. Increase document size
- D. Switch to aggregation only
Answer: A

### Q3.10
For `find({ employer:"ABC" }).sort({ last_name:1, job:1 })`, better index order is:
- A. `{ job:1, last_name:1, employer:1 }`
- B. `{ employer:1, last_name:1, job:1 }`
- C. `{ employer:1 }` only
- D. `{ last_name:1, employer:1, job:1 }`
Answer: B

### Q3.11
High `totalDocsExamined` with low `nReturned` usually indicates:
- A. Efficient targeting
- B. Potentially poor selectivity/index fit
- C. Index is definitely unique
- D. Sharding required immediately
Answer: B

### Q3.12
`getIndexes()` returns:
- A. Number of documents
- B. Index definitions on the collection
- C. Query plan history only
- D. Shard key metadata only
Answer: B

### Q3.13
Which statement about index trade-offs is true?
- A. More indexes always improve writes
- B. More indexes increase write maintenance work
- C. Indexes reduce all storage usage
- D. Indexes are used only by aggregation framework
Answer: B

### Q3.14
Best index for array-path equality filter `{"objs.a": 1}`:
- A. `{ objs: 1 }`
- B. `{ "objs.a": 1 }`
- C. `{ a: 1 }`
- D. `{ objs: 1, a: 1 }`
Answer: B

### Q3.15
If a query only sorts by `product` then `price`, which index is directly aligned?
- A. `{ price: 1, product: 1 }`
- B. `{ product: 1, price: 1 }`
- C. `{ product: -1 }`
- D. `{ price: -1 }`
Answer: B
