# Indexes and Query Performance

## 1. Overview
- Index-focused questions are heavily represented and emphasize compound indexes for filter + sort performance.
- MongoDB uses indexes to avoid collection scans and to satisfy sort operations efficiently.
- The exam repeatedly tests index key order and direction compatibility with sort patterns.
- Reference docs: https://www.mongodb.com/docs/manual/indexes/ and https://www.mongodb.com/docs/manual/core/indexes/index-types/index-compound/sort-order/

## 2. Core Concepts
- Single-field index: One field key.
- Compound index: Multiple keys in a defined order.
- Prefix rule: Compound index can support queries that use its leading fields.
- Sort support rule:
- Compound index can support a sort in index order.
- It can also support complete reverse-order traversal.
- Multikey index: Created when indexing array field values.
- Index metadata inspection: `getIndexes()`.

## 3. Commands / Syntax
- Create filter + sort compound index:

```javascript
db.people.createIndex({ employer: 1, last_name: 1, job: 1 });
```

- Reverse-direction equivalent sort-support index:

```javascript
db.people.createIndex({ employer: 1, last_name: -1, job: -1 });
```

- Index on nested array field path:

```javascript
db.collection.createIndex({ "objs.a": 1 });
```

- List indexes:

```javascript
db.inventory.getIndexes();
```

- Node.js driver example:

```javascript
await db.collection("people").createIndex({ employer: 1, last_name: 1, job: 1 });
const indexes = await db.collection("people").indexes();
```

## 4. Key Rules / Limits
- Field order in a compound index is significant.
- For sort support, direction across sort keys must align with index order or exact reverse order.
- Indexes consume memory and disk; extra indexes increase write overhead.
- Array field indexes can support dot-notation queries into array elements.

## 5. Exam Patterns
- Pick two best indexes for a given `find(...).sort(...)` query.
- Determine whether index field order is correct for sort fields.
- Choose valid index for dotted-path query (for example `"objs.a": 1`).
- Identify command to list collection indexes.
- Common traps:
- Right fields but wrong order.
- Mixed sort directions that do not match usable index traversal.
- Confusing index name metadata with key pattern suitability.

## 6. Practice Questions
- MCQ: Which two compound indexes support `sort({ product: 1, price: 1 })`?
- MCQ: Which index supports query `find({"objs.a": 1})`?
- Scenario-based: A query filters by employer and sorts by two fields. Which index avoids collection scan?
- Scenario-based: How do you inspect index definitions for one collection in mongosh?
- Output-based: Given four index definitions, predict which ones are usable for a specific sort.

## 7. Best Practices
- Build indexes from actual query shapes: filter fields first, then sort fields.
- Prefer minimal index set that covers critical read paths.
- Re-check index usage with explain plans during optimization.
- Remove redundant indexes that do not support real workloads.
