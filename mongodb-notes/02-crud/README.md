# Section 2: CRUD (51%)

[Home](../../README.md) | [Notes Index](../README.md) | [Previous: Section 1](../01-overview-document-model/README.md) | [Next: Section 3](../03-indexes/README.md)

Official references:
- https://www.mongodb.com/docs/manual/crud/
- https://www.mongodb.com/docs/manual/reference/operator/query/
- https://www.mongodb.com/docs/manual/aggregation/
- https://www.mongodb.com/docs/atlas/atlas-search/

This section carries the largest weight, so questions often combine syntax validation plus output reasoning.

## In-Depth Objective Guide

### 2.1 Insert command correctness
What is tested:
- Valid method name
- Correct literal values and JSON-like structure
- Understanding `_id` uniqueness

Core command:
```javascript
db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: true });
```

Driver equivalent:
```javascript
await coll.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: true });
```

### 2.2 Replace-style update output
What is tested:
- Replace vs update-operator behavior

`replaceOne` replaces the whole matched document content, except immutable `_id`.

```javascript
db.coll.replaceOne({ _id: 1 }, { a: "ten", b: "five" });
```

If original doc is `{ _id:1, a:"one", b:"four", c:"three" }`, after replace it becomes `{ _id:1, a:"ten", b:"five" }`.

### 2.3 `$set` update output
What is tested:
- Field-level update behavior

```javascript
db.coll.updateOne({ _id: 1 }, { $set: { b: 2 } });
```

`$set` updates/adds only target path; unrelated fields remain.

### 2.4 Upsert command selection
Use `upsert: true` in options object.

```javascript
db.cakeFlavors.updateOne(
  { flavor: "strawberry" },
  { $set: { number: 100 } },
  { upsert: true }
);
```

Common trap: `$upsert` is invalid.

### 2.5 Multi-document update expression
Use `updateMany` when requirement says all matching docs.

```javascript
db.movie.updateMany({ year: { $lt: 2000 } }, { $set: { classic: true } });
```

### 2.6 findAndModify concurrent scenario
Single-document atomic methods:
- `findOneAndUpdate`
- `findOneAndDelete`
- `findOneAndReplace`

```javascript
db.loans.findOneAndDelete({ book: "EFF", name: "T.B." });
```

Exam pattern: choose API that both changes state and returns the affected document.

### 2.7 Delete expression selection
- Delete one: `deleteOne`
- Delete all matches: `deleteMany`

```javascript
db.inventory.deleteMany({ status: "C" });
```

### 2.8 Single-document equality lookup
Use `findOne`.

```javascript
db.ratings.findOne({ hotel: "CCC" });
```

### 2.9 Array-field equality matching
Equality on array field matches element membership.

```javascript
db.products.find({ color: "purple" });
```

### 2.10 Relational operators
Use comparison operators directly on target field.

```javascript
db.products.find({ price: { $lte: 800 } });
```

### 2.11 `$in` matching

```javascript
db.users.find({ status: { $in: ["A", "B"] } });
```

### 2.12 `$elemMatch`
Use to bind multiple conditions to the same array element.

```javascript
db.orders.find({
  items: { $elemMatch: { sku: "ABC", qty: { $gte: 2 } } }
});
```

### 2.13 Logical operators

```javascript
db.products.find({
  $and: [
    { price: { $lte: 800 } },
    { $or: [{ color: "purple" }, { storage: 1024 }] }
  ]
});
```

Pattern tested: identify matched output documents from compound conditions.

### 2.14 Sort + limit output

```javascript
db.restaurants.find({}).sort({ rating: -1 }).limit(1);
```

Output reasoning:
- Sort descending first
- Take first document after sort

### 2.15 Incorrect projection detection
Projection rule:
- Include fields (`1`) OR exclude fields (`0`), not both.
- Exception: `_id` can be excluded alongside include projections.

Valid example:
```javascript
db.users.find({}, { name: 1, email: 1, _id: 0 });
```

### 2.16 Cursor materialization

```javascript
db.inventory.find({}).toArray();
```

`find()` returns cursor; `toArray()` loads all results to memory.

### 2.17 Count matched docs

```javascript
db.orders.countDocuments({ status: "OPEN" });
```

Exam distinction:
- `countDocuments(filter)` for filtered count.

### 2.18 Search index definition
Atlas Search index mappings are configured separately from regular `createIndex`.

Autocomplete concept example:
```json
{
  "mappings": {
    "dynamic": false,
    "fields": {
      "name": [
        { "type": "autocomplete", "tokenization": "edgeGram" }
      ]
    }
  }
}
```

### 2.19 Search query selection
`$search` text query uses `path` + `query`.

```javascript
db.restaurants.aggregate([
  {
    $search: {
      text: {
        path: "name",
        query: "cuban"
      }
    }
  }
]);
```

### 2.20 Aggregation output with `$match` and `$group`

```javascript
db.scores.aggregate([
  { $group: { _id: "$player", score: { $avg: "$score" } } },
  { $match: { score: { $gt: 70 } } }
]);
```

If `$match` is after `$group`, it filters grouped results, not raw documents.

### 2.21 Aggregation output with `$lookup`

```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer"
    }
  }
]);
```

### 2.22 Aggregation output with `$out`

```javascript
db.sales.aggregate([
  { $match: { status: "PAID" } },
  { $out: "sales_paid" }
]);
```

`$out` writes final pipeline output into target collection.

## High-yield exam checklist
- One vs many method selection
- Replace vs `$set`
- Correct options object (`upsert: true`)
- Array matching behavior (`$in`, `$elemMatch`, equality)
- Projection include/exclude rule
- Cursor vs document APIs
- Aggregation stage order reasoning

## Common traps
- Fake methods: `findMany`, `updateMulti`, `updateBulk`
- Wrong upsert key: `$upsert`
- Wrong Atlas Search keys: `field` instead of `path`
- Misplacing `limit` in wrong query shape

## MCQ Practice (Section 2)

### Q2.1
Valid insert command is:
- A. `db.files.insertOne({ size: 1KB, deleted: True })`
- B. `db.files.insertOne({ size: 1024, deleted: true })`
- C. `db.files.addOne({ size: 1024, deleted: true })`
- D. `db.files.insert({ size: 1024, deleted: True })`
Answer: B

### Q2.2
`replaceOne` behavior is:
- A. Only adds missing fields
- B. Replaces full document except `_id`
- C. Same as `$set`
- D. Always inserts new `_id`
Answer: B

### Q2.3
`$set` update does what?
- A. Replaces whole doc
- B. Updates specific fields only
- C. Drops collection
- D. Sorts documents
Answer: B

### Q2.4
Correct upsert option syntax:
- A. `{ $upsert: true }`
- B. `{ upsert: true }`
- C. `{ upserted: true }`
- D. `{ setUpsert: true }`
Answer: B

### Q2.5
To update all matching docs:
- A. `updateOne`
- B. `replaceOne`
- C. `updateMany`
- D. `updateBulk`
Answer: C

### Q2.6
Delete one and return deleted document:
- A. `deleteOne`
- B. `deleteMany`
- C. `findOneAndDelete`
- D. `remove`
Answer: C

### Q2.7
Remove all documents with status C:
- A. `deleteOne({status:"C"})`
- B. `deleteMany({status:"C"})`
- C. `findOneAndDelete({status:"C"})`
- D. `drop({status:"C"})`
Answer: B

### Q2.8
Single equality lookup for one doc:
- A. `findOne({x:3})`
- B. `find({x:3}).toArray()`
- C. `lookupOne({x:3})`
- D. `searchOne({x:3})`
Answer: A

### Q2.9
`{ tags: "red" }` can match `{ tags: ["red", "blue"] }`?
- A. No
- B. Yes
- C. Only with text index
- D. Only with `$elemMatch`
Answer: B

### Q2.10
Which query matches `price <= 100`?
- A. `{ price: { $gte: 100 } }`
- B. `{ price: { $lte: 100 } }`
- C. `{ price: { $eq: [100] } }`
- D. `{ price: { $in: 100 } }`
Answer: B

### Q2.11
Membership operator is:
- A. `$nin`
- B. `$exists`
- C. `$in`
- D. `$allMatch`
Answer: C

### Q2.12
Operator for matching multiple conditions on one array element:
- A. `$elemMatch`
- B. `$in`
- C. `$all`
- D. `$slice`
Answer: A

### Q2.13
Logically correct `$and` expression:
- A. `{ $and: { a: 1, b: 2 } }`
- B. `{ $and: [{ a: 1 }, { b: 2 }] }`
- C. `{ and: [{ a: 1 }, { b: 2 }] }`
- D. `{ $and: "a=1,b=2" }`
Answer: B

### Q2.14
Top-rated document query pattern:
- A. `.sort({rating:1}).limit(1)`
- B. `.sort({rating:-1}).limit(1)`
- C. `.limit(1).sort({rating:-1})` always equivalent in exam options
- D. `.findOne().sort({rating:-1})`
Answer: B

### Q2.15
Invalid projection pattern:
- A. `{ name: 1, _id: 0 }`
- B. `{ name: 0, email: 0 }`
- C. `{ name: 1, email: 0 }`
- D. `{ city: 1 }`
Answer: C

### Q2.16
Get all cursor results:
- A. `findAll()`
- B. `find().toArray()`
- C. `cursor.readAll()`
- D. `findMany().all()`
Answer: B

### Q2.17
Count API for matched documents:
- A. `countDocuments(filter)`
- B. `countAll(filter)`
- C. `count(filter).total`
- D. `size(filter)`
Answer: A

### Q2.18
Autocomplete beginning-of-word tokenization:
- A. `matchNGram`
- B. `edgeGram`
- C. `regexCaptureGroup`
- D. `prefixOnly`
Answer: B

### Q2.19
Atlas Search text keys:
- A. `field`, `synonym`
- B. `path`, `query`
- C. `column`, `text`
- D. `where`, `value`
Answer: B

### Q2.20
After `$group`, `$match` filters:
- A. Source docs
- B. Grouped output docs
- C. Index definitions
- D. Collection metadata
Answer: B

### Q2.21
Join-like aggregation stage:
- A. `$join`
- B. `$lookup`
- C. `$relate`
- D. `$pair`
Answer: B

### Q2.22
Stage that writes aggregation output to collection:
- A. `$out`
- B. `$save`
- C. `$write`
- D. `$dump`
Answer: A

### Q2.23
Which statement is true about `replaceOne`?
- A. It merges fields like `$set`
- B. It replaces full doc body except immutable `_id`
- C. It updates all matched documents
- D. It can change `_id` freely
Answer: B

### Q2.24
Which API is best when requirement says "update if exists, insert if missing"?
- A. `insertOne` with options
- B. `updateOne` with `{ upsert: true }`
- C. `replaceOne` only
- D. `findOneAndDelete`
Answer: B

### Q2.25
For array-of-objects query requiring same element to satisfy two conditions, use:
- A. `$in`
- B. `$and`
- C. `$elemMatch`
- D. `$exists`
Answer: C

### Q2.26
Which is valid projection?
- A. `{ name: 1, email: 0 }`
- B. `{ name: 1, email: 1, _id: 0 }`
- C. `{ name: 0, _id: 1, city: 1 }`
- D. `{ name: true, email: false }` as mixed include/exclude projection semantics
Answer: B

### Q2.27
`find()` returns:
- A. Document
- B. Cursor
- C. Boolean
- D. Write result
Answer: B

### Q2.28
Which query shape is correct for top-rated one document?
- A. `find().limit(1).sort({rating:-1})` as exam-best canonical choice
- B. `find().sort({rating:-1}).limit(1)`
- C. `findOne().sort({rating:-1})`
- D. `aggregate([{ $sort: { rating: -1, limit: 1 } }])`
Answer: B

### Q2.29
Atlas Search text operator requires which keys?
- A. `field`, `synonym`
- B. `path`, `query`
- C. `column`, `term`
- D. `search`, `value`
Answer: B

### Q2.30
After pipeline `[{$group:...}, {$match:...}]`, the second stage evaluates:
- A. Original source documents
- B. Grouped documents
- C. Index metadata
- D. Collection options
Answer: B

[Previous: Section 1](../01-overview-document-model/README.md) | [Back to Notes Index](../README.md) | [Next: Section 3](../03-indexes/README.md)
