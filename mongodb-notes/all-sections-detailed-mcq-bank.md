# All-Sections Detailed MCQ Bank (MongoDB Associate Node.js)

[Home](../README.md) | [Notes Index](README.md)

This bank is a high-coverage exam-style set across all 6 official sections.

How to use:
1. Attempt each question without looking at answer.
2. Check answer and read the reasoning.
3. Re-do wrong questions after 24 hours.

---

## Section 1: MongoDB Overview and Document Model

### S1-Q1
Which numeric type is an explicitly valid MongoDB BSON type label?
- A. Number
- B. BIGINT
- C. 32-bit integer
- D. Numeric
Answer: C
Why: MongoDB BSON names include Int32 and Int64; generic labels like Number/BIGINT are not official BSON type names in this context.

### S1-Q2
Given existing documents with `_id` values 1 and 2, which insert can fail only because of `_id` uniqueness?
- A. `{ _id: 3, x: 1 }`
- B. `{ _id: 1, x: 99 }`
- C. `{ _id: 4, y: "z" }`
- D. `{ _id: 5, arr: [1,2] }`
Answer: B
Why: Duplicate `_id` in same collection violates uniqueness.

### S1-Q3
What happens when `_id` is omitted in `insertOne`?
- A. Insert always fails
- B. `_id` becomes 0
- C. MongoDB auto-generates `_id`
- D. `_id` added only during update
Answer: C
Why: MongoDB generates an ObjectId `_id` automatically if not provided.

### S1-Q4
Which statement is true about document shapes in one collection?
- A. All documents must have same fields and same types
- B. Different shapes can co-exist
- C. Arrays are not allowed
- D. Embedded documents are not allowed
Answer: B
Why: MongoDB collections are schema-flexible by default.

### S1-Q5
Which boolean literal is valid in mongosh inserts?
- A. TRUE
- B. True
- C. true
- D. T
Answer: C
Why: JavaScript-style lowercase boolean literal is required.

### S1-Q6
Which `_id` value form is typically invalid in exam options?
- A. `_id: 3`
- B. `_id: ObjectId("...")`
- C. `_id: [4]`
- D. `_id: "abc"`
Answer: C
Why: `_id` can be many types, but array `_id` is not allowed.

### S1-Q7
Which document contains an embedded document?
- A. `{ tags: ["a","b"] }`
- B. `{ profile: { age: 22, city: "Lahore" } }`
- C. `{ age: 22 }`
- D. `{ active: true }`
Answer: B
Why: Nested object value is an embedded document.

### S1-Q8
Which statement about BSON vs JSON is true?
- A. BSON has fewer types than JSON
- B. BSON has only string and number
- C. BSON extends JSON with additional types
- D. BSON cannot store dates
Answer: C
Why: BSON adds Date, ObjectId, Decimal128, etc.

### S1-Q9
Best BSON type for exact decimal monetary values:
- A. Double
- B. Decimal128
- C. Int32
- D. String
Answer: B
Why: Decimal128 avoids binary floating-point precision issues.

### S1-Q10
Which syntax is valid for adding a date field?
- A. `{ createdAt: Date.now }`
- B. `{ createdAt: "ISODate" }`
- C. `{ createdAt: new Date() }`
- D. `{ createdAt: date() }`
Answer: C
Why: `new Date()` creates JS Date object compatible with BSON Date.

### S1-Q11
Which is a valid mixed-shape pair in one collection?
- A. `{_id:1, a:1}` and `{_id:1, b:2}`
- B. `{_id:1, a:1}` and `{_id:2, b:2}`
- C. `{_id:[1], a:1}` and `{_id:2, b:2}`
- D. `{_id:1, a:1}` and `{_id:1, a:2}`
Answer: B
Why: Different shapes are fine, duplicate `_id` is not.

### S1-Q12
What is true about `null` in MongoDB documents?
- A. `null` is invalid
- B. `null` is valid BSON Null type
- C. `null` converts to empty string automatically
- D. `null` only allowed in arrays
Answer: B
Why: Null is a valid BSON type.

---

## Section 2: CRUD

### S2-Q1
Which insert command is valid?
- A. `db.files.insertOne({ size: 1KB, deleted: true })`
- B. `db.files.insertOne({ size: 1024, deleted: True })`
- C. `db.files.insertOne({ size: 1024, deleted: true })`
- D. `db.files.addOne({ size: 1024, deleted: true })`
Answer: C
Why: Correct numeric literal, boolean casing, and method name.

### S2-Q2
`replaceOne({ _id: 1 }, { a: "x" })` on `{ _id:1, a:"old", b:2 }` results in:
- A. `{ _id:1, a:"x", b:2 }`
- B. `{ _id:1, a:"x" }`
- C. `{ a:"x" }`
- D. `{ _id:1, a:"old", b:2 }`
Answer: B
Why: replaceOne replaces full document except immutable `_id`.

### S2-Q3
`updateOne({ _id: 1 }, { $set: { b: 9 } })` changes:
- A. Whole document replaced
- B. Only field `b` (or adds it)
- C. `_id`
- D. Collection schema
Answer: B
Why: `$set` is field-level update.

### S2-Q4
Correct upsert syntax:
- A. `{ $upsert: true }`
- B. `{ upsert: true }`
- C. `{ upserted: true }`
- D. `{ setUpsert: true }`
Answer: B
Why: `upsert` is a normal options key.

### S2-Q5
Update all docs with `year < 2000`:
- A. `updateOne(..., { multi: true })`
- B. `updateMany({ year: { $lt: 2000 } }, { $set: { classic: true } })`
- C. `updateMulti(...)`
- D. `updateBulk(...)`
Answer: B
Why: `updateMany` is the modern, correct method.

### S2-Q6
Delete one doc and return deleted document:
- A. `deleteOne`
- B. `deleteMany`
- C. `findOneAndDelete`
- D. `remove`
Answer: C
Why: findOneAndDelete atomically deletes and returns one document.

### S2-Q7
Remove all `status: "C"` docs:
- A. `deleteOne({status:"C"})`
- B. `deleteMany({status:"C"})`
- C. `findOneAndDelete({status:"C"})`
- D. `drop({status:"C"})`
Answer: B
Why: Requirement says all matching documents.

### S2-Q8
Get one document where `hotel` is `CCC`:
- A. `find({hotel:"CCC"})`
- B. `findOne({hotel:"CCC"})`
- C. `find_many({hotel:"CCC"})`
- D. `returnOne({hotel:"CCC"})`
Answer: B
Why: `findOne` returns a single matched document.

### S2-Q9
For `{ color: ["white", "purple"] }`, query `{ color: "purple" }`:
- A. Does not match
- B. Matches
- C. Matches only with `$elemMatch`
- D. Matches only with text index
Answer: B
Why: Equality on array field tests membership.

### S2-Q10
Query for `price <= 800`:
- A. `{ price: { $gte: 800 } }`
- B. `{ price: { $lt: 800 } }`
- C. `{ price: { $lte: 800 } }`
- D. `{ price: 800 }`
Answer: C
Why: `$lte` means less-than-or-equal.

### S2-Q11
Correct usage of `$in`:
- A. `{ status: { $in: "A" } }`
- B. `{ status: { $in: ["A", "B"] } }`
- C. `{ $in: { status: ["A","B"] } }`
- D. `{ status: ["A","B"] }`
Answer: B
Why: `$in` expects an array of possible values.

### S2-Q12
When multiple conditions must match the same array element, use:
- A. `$in`
- B. `$all`
- C. `$elemMatch`
- D. `$exists`
Answer: C
Why: `$elemMatch` binds conditions to one element.

### S2-Q13
Logical query combining conditions correctly:
- A. `{ $and: { a:1, b:2 } }`
- B. `{ $and: [{a:1},{b:2}] }`
- C. `{ and: [{a:1},{b:2}] }`
- D. `{ $and: "a=1,b=2" }`
Answer: B
Why: `$and` expects an array of filter objects.

### S2-Q14
Top-rated single restaurant query:
- A. `find({}).limit(1).sort({rating:-1})`
- B. `find({}).sort({rating:-1}).limit(1)`
- C. `findOne({}).sort({rating:-1})`
- D. `find({}).sort({rating:1}).limit(1)`
Answer: B
Why: Sort descending then limit 1.

### S2-Q15
Invalid projection pattern:
- A. `{ name: 1, _id: 0 }`
- B. `{ name: 0, email: 0 }`
- C. `{ name: 1, email: 0 }`
- D. `{ city: 1 }`
Answer: C
Why: Mixing include and exclude (except `_id`) is invalid.

### S2-Q16
`find()` returns:
- A. Document
- B. Cursor
- C. Boolean
- D. WriteResult
Answer: B
Why: `find()` returns a cursor, not materialized docs.

### S2-Q17
Get all docs from cursor in mongosh:
- A. `findAll()`
- B. `find().toArray()`
- C. `readCursor()`
- D. `findMany().all()`
Answer: B
Why: `toArray()` materializes the cursor results.

### S2-Q18
Count matched documents by filter:
- A. `count()`
- B. `countAll()`
- C. `countDocuments(filter)`
- D. `size(filter)`
Answer: C
Why: `countDocuments` is the canonical filtered count API.

### S2-Q19
In Atlas Search autocomplete definition for beginning-of-word matching, tokenization should be:
- A. `regexCaptureGroup`
- B. `edgeGram`
- C. `matchNGram`
- D. `exactGram`
Answer: B
Why: edgeGram is the expected tokenizer for prefix-like autocomplete.

### S2-Q20
In `$search.text`, correct keys are:
- A. `field` + `synonym`
- B. `path` + `query`
- C. `column` + `text`
- D. `searchField` + `term`
Answer: B
Why: Official syntax uses `path` and `query`.

### S2-Q21
Pipeline:
`[{ $group: { _id:"$player", score:{ $avg:"$score" } } }, { $match: { score: { $gt: 70 } } }]`
Second stage filters:
- A. Original source documents
- B. Grouped output documents
- C. Index metadata
- D. BSON schema
Answer: B
Why: `$match` runs after `$group`, so it sees grouped docs.

### S2-Q22
`$lookup` output field `as: "customer"` has shape:
- A. Single object only
- B. Array of matched documents
- C. Number
- D. String
Answer: B
Why: `$lookup` always outputs an array field.

### S2-Q23
`$out` stage behavior:
- A. Appends aggregation results to cursor only
- B. Writes pipeline output to target collection
- C. Drops source collection
- D. Returns one boolean
Answer: B
Why: `$out` writes final results into collection.

### S2-Q24
Which statement is true about `replaceOne({}, {a:"ten", b:"five"})`?
- A. All docs are replaced
- B. One matched doc is replaced
- C. No doc changes unless upsert true
- D. `_id` is removed
Answer: B
Why: replaceOne affects a single matched document.

### S2-Q25
Which method name is fake in mongosh CRUD exam options?
- A. `findOne`
- B. `updateMany`
- C. `findMany`
- D. `deleteMany`
Answer: C
Why: `findMany` is not a valid mongosh collection method.

### S2-Q26
Find one cookie recipe without chocolate in Atlas Aggregation view:
- A. Put `$limit` inside `$match`
- B. Use `$match` then `$limit: 1`
- C. Use `findOne` stage
- D. Use `$group` only
Answer: B
Why: In aggregation, `$limit` is a separate stage after matching.

---

## Section 3: Indexes

### S3-Q1
For `find({ employer:"ABC" }).sort({ last_name:1, job:1 })`, best index:
- A. `{ last_name:1, job:1, employer:1 }`
- B. `{ employer:1, last_name:1, job:1 }`
- C. `{ employer:1 }`
- D. `{ job:1, last_name:1 }`
Answer: B
Why: Equality key first, then sort keys in order.

### S3-Q2
For `find({}).sort({ product:1, price:1 })`, which two indexes can satisfy sort?
- A. `{ product:1, price:1 }`
- B. `{ product:1, price:-1 }`
- C. `{ product:-1, price:1 }`
- D. `{ product:-1, price:-1 }`
Answer: A and D
Why: Same key order required; complete reverse direction can be traversed.

### S3-Q3
Best index for query `{ "objs.a": 1 }`:
- A. `{ objs:1 }`
- B. `{ a:1 }`
- C. `{ "objs.a":1 }`
- D. `{ objs:1, a:1 }`
Answer: C
Why: Path-specific index matches query path.

### S3-Q4
Command to list indexes:
- A. `db.inventory.showIndexes()`
- B. `db.inventory.getIndexes()`
- C. `db.inventory.displayIndexes()`
- D. `db.inventory.indexes()`
Answer: B
Why: `getIndexes()` returns index definition array.

### S3-Q5
Explain showing `COLLSCAN` suggests:
- A. Ideal index usage
- B. Full collection scan
- C. Unique index violation
- D. Aggregation-only query
Answer: B
Why: COLLSCAN means scanning full collection.

### S3-Q6
If `totalDocsExamined` is very high and `nReturned` is low:
- A. Highly efficient query
- B. Potentially poor index/selectivity fit
- C. Guaranteed syntax error
- D. Cluster offline
Answer: B
Why: Many scanned docs for few results indicates inefficiency.

### S3-Q7
Index type for matching values in array fields:
- A. hashed
- B. text
- C. multikey
- D. wildcard
Answer: C
Why: Array indexing creates multikey behavior.

### S3-Q8
Correct text index definition:
- A. `createIndex({ title: 1, body: 1 }, { text: true })`
- B. `createIndex({ title: "text", body: "text" })`
- C. `createTextIndex({ title: 1 })`
- D. `createIndex({ $text: "title" })`
Answer: B
Why: Text index keys are declared with value `"text"`.

### S3-Q9
TTL index purpose:
- A. Full-text search
- B. Auto-delete expired documents
- C. Enforce uniqueness
- D. Geospatial radius lookup
Answer: B
Why: TTL removes documents after expiration window.

### S3-Q10
Correct unique index syntax:
- A. `createIndex({ username:1, unique:true })`
- B. `createIndex({ username:1 }, { unique:true })`
- C. `createUnique({ username:1 })`
- D. `addIndex({ username:1 }, { unique:true })`
Answer: B
Why: Unique is passed as index option document.

### S3-Q11
Wildcard index key pattern is:
- A. `{ "*": 1 }`
- B. `{ "$all": 1 }`
- C. `{ "$**": 1 }`
- D. `{ "**": 1 }`
Answer: C
Why: MongoDB wildcard key is `$**`.

### S3-Q12
Partial index is best when:
- A. You want every document indexed
- B. You need only subset indexed by predicate
- C. You need geo query acceleration
- D. You need text tokenization
Answer: B
Why: partialFilterExpression limits which docs are indexed.

### S3-Q13
Hashed index is mainly used for:
- A. Prefix text search
- B. Sharding distribution/hash lookups
- C. Sorting by date
- D. Array element matching
Answer: B
Why: Hashed indexes are useful for hashed shard keys and hash-based lookups.

### S3-Q14
Which is true about Atlas Search indexes?
- A. Created by normal `createIndex` only
- B. Same as B-tree indexes
- C. Separate search index definitions in Atlas
- D. Require `$group`
Answer: C
Why: Atlas Search indexes are a different subsystem.

---

## Section 4: Data Modeling

### S4-Q1
Embedding is usually best when:
- A. Child set grows unbounded forever
- B. Child data is bounded and co-read with parent
- C. Child has independent heavy query workload
- D. Parent and child are unrelated
Answer: B
Why: Embedding favors bounded, co-accessed data.

### S4-Q2
Referencing is usually best when:
- A. Child is tiny and always read with parent
- B. Child has independent lifecycle and can be large
- C. You need single-document atomicity only
- D. You want to avoid any joins forever
Answer: B
Why: References suit independently queried/unbounded child data.

### S4-Q3
Which is an anti-pattern in high-write workload?
- A. Small embedded object for profile
- B. Unbounded activity array inside hot user doc
- C. Reference to customer from order
- D. Index on common lookup field
Answer: B
Why: Continuously growing array in hot doc harms write performance.

### S4-Q4
Primary decision factor for embed vs reference:
- A. Alphabetical field names
- B. Access and update patterns
- C. Driver version
- D. Atlas cluster tier
Answer: B
Why: Data model should match query/write workload.

### S4-Q5
If app always fetches order with small shippingAddress object:
- A. Put address in separate collection always
- B. Embed shippingAddress in order
- C. Use text index on address only
- D. Use wildcard index only
Answer: B
Why: Co-read bounded subdocument is good embed candidate.

### S4-Q6
If comments per post can be millions and queried independently:
- A. Embed all comments inside post
- B. Keep comments in separate collection with postId reference
- C. Use only arrays without indexes
- D. Store comments in `_id`
Answer: B
Why: Unbounded one-to-many should be referenced.

### S4-Q7
Which model can reduce repeated expensive lookups for common page load?
- A. Reference every tiny child always
- B. Embed frequently displayed small child fields
- C. Split every field into separate collection
- D. Remove `_id`
Answer: B
Why: Strategic embedding can optimize frequent read path.

### S4-Q8
A model forcing many lookups for one common read is usually:
- A. Preferred by default
- B. Potential anti-pattern
- C. Required by BSON
- D. Needed for `findOne`
Answer: B
Why: Excessive joins for common reads can be avoidable overhead.

---

## Section 5: Tools and Tooling

### S5-Q1
Correct first step order for Atlas sample-data workflow:
- A. Open Data Explorer, then load sample dataset
- B. Load sample dataset, then open Data Explorer
- C. Build pipeline first, then create cluster
- D. Create wildcard index first
Answer: B
Why: Dataset must exist before querying it in Data Explorer.

### S5-Q2
For direct filter query in Atlas UI, preferred view:
- A. Aggregation
- B. Find
- C. Performance Advisor
- D. Charts
Answer: B
Why: Find view is intended for direct filter documents.

### S5-Q3
Correct filter for cookie recipes excluding chocolate ingredient:
- A. `{ ingredients: { $all: ["chocolate"] } }`
- B. `{ ingredients: { $nin: ["chocolate"] } }`
- C. `{ ingredients: { $in: ["chocolate"] } }`
- D. `{ ingredients: { $eq: "chocolate" } }`
Answer: B
Why: `$nin` excludes docs where array contains that value.

### S5-Q4
In Aggregation view, limiting to first document uses:
- A. `limit` key inside `$match`
- B. `$limit` stage
- C. `findOne` stage
- D. `$project: { limit: 1 }`
Answer: B
Why: `$limit` is its own pipeline stage.

### S5-Q5
Which option is a common exam distractor?
- A. Valid JSON-like filter syntax
- B. `$limit` inside filter object
- C. `$match` then `$limit`
- D. Find view for simple query
Answer: B
Why: `$limit` belongs to aggregation stage list, not filter object.

### S5-Q6
Prompt asks specifically for aggregation steps. Best response style:
- A. Use Find with projection
- B. Use Aggregation pipeline with `$match` and `$limit`
- C. Use only Compass Schema tab
- D. Use text index command only
Answer: B
Why: Requirement explicitly asks for aggregation-stage solution.

---

## Section 6: Drivers (Node.js)

### S6-Q1
Official package name for Node.js MongoDB driver:
- A. `mongo-node`
- B. `mongodb`
- C. `mongojs-driver`
- D. `mongoclient`
Answer: B
Why: Official npm package is `mongodb`.

### S6-Q2
Which are valid MongoClient methods? (Choose 2)
- A. `db()`
- B. `open()`
- C. `close()`
- D. `destroy()`
Answer: A and C
Why: Modern driver uses `db()` and `close()`, not `open()`/`destroy()`.

### S6-Q3
Valid SRV URI prefix:
- A. `mongo+srv://`
- B. `mongodb+srv://`
- C. `db+srv://`
- D. `atlas+srv://`
Answer: B
Why: Official SRV URI scheme is `mongodb+srv://`.

### S6-Q4
Main benefit of connection pooling:
- A. Removes need for credentials
- B. Reuses connections to reduce latency
- C. Disables TLS
- D. Eliminates indexes
Answer: B
Why: Pooling avoids opening/closing network connections per request.

### S6-Q5
Preferred app pattern for MongoClient lifecycle:
- A. New client per request
- B. Shared client and pool across requests
- C. Close after every query in loop
- D. Disable pooling
Answer: B
Why: Reusing one client with pool is the recommended scalable pattern.

### S6-Q6
Correct insertMany usage:
- A. `insertMany({a:1},{a:2})`
- B. `insertMany([{a:1},{a:2}])`
- C. `insert([{a:1},{a:2}])`
- D. `addMany([{a:1},{a:2}])`
Answer: B
Why: insertMany expects one array argument of documents.

### S6-Q7
Correct updateMany usage:
- A. `updateMany({x:1},{set:{y:2}})`
- B. `updateMany({x:1},{$set:{y:2}})`
- C. `update({x:1},{$set:{y:2}},true)`
- D. `modifyMany({x:1},{$set:{y:2}})`
Answer: B
Why: `$set` operator must include `$` and method is `updateMany`.

### S6-Q8
Correct delete API for many docs:
- A. `deleteAll`
- B. `removeMany`
- C. `deleteMany`
- D. `findAndDeleteMany`
Answer: C
Why: `deleteMany` is valid official method.

### S6-Q9
Read one document API:
- A. `find().one()`
- B. `findOne`
- C. `getOne`
- D. `find_first`
Answer: B
Why: `findOne` is the correct method.

### S6-Q10
Read multiple docs to JS array:
- A. `findMany().all()`
- B. `find(...).toArray()`
- C. `getAll()`
- D. `findOne()`
Answer: B
Why: `find` returns cursor; `toArray` materializes it.

### S6-Q11
Correct aggregation entry method:
- A. `pipeline()`
- B. `aggregate()`
- C. `runAggregation()`
- D. `groupBy()`
Answer: B
Why: Collection `aggregate()` starts an aggregation cursor.

### S6-Q12
MQL find syntax vs aggregation syntax:
- A. Both always use same shape
- B. Find uses one filter doc; aggregation uses stage array
- C. Aggregation cannot filter
- D. Find cannot use operators
Answer: B
Why: This is the key syntax distinction.

### S6-Q13
Which result field indicates successful insertOne acknowledgment?
- A. `matchedCount`
- B. `insertedCount`
- C. `acknowledged`
- D. `deletedCount`
Answer: C
Why: insertOne result includes `acknowledged` and `insertedId`.

### S6-Q14
Which statement about update result object is true?
- A. `modifiedCount` exists for update operations
- B. `insertedId` exists for every update
- C. `deletedCount` exists for updateOne
- D. Updates return only array of docs
Answer: A
Why: update results include matchedCount and modifiedCount.

---

## Revision schedule suggestion
1. Do Section 2 and Section 3 first.
2. Do Section 6 next.
3. Then Section 1, 4, and 5.
4. Re-attempt only incorrect questions after each pass.

## Related resources
- [Question Pattern Analysis](question-pattern-analysis.md)
- [Section 1 Notes](01-overview-document-model/README.md)
- [Section 2 Notes](02-crud/README.md)
- [Section 3 Notes](03-indexes/README.md)
- [Section 4 Notes](04-data-modeling/README.md)
- [Section 5 Notes](05-tools-and-tooling/README.md)
- [Section 6 Notes](06-drivers-nodejs/README.md)
