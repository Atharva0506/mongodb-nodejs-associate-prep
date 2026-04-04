# Section 2: CRUD (51%)

[Home](../../README.md) | [Notes Index](../README.md) | [Previous: Section 1](../01-overview-document-model/README.md) | [Next: Section 3](../03-indexes/README.md)

Official references:
- https://www.mongodb.com/docs/manual/crud/
- https://www.mongodb.com/docs/manual/reference/operator/query/
- https://www.mongodb.com/docs/manual/aggregation/
- https://www.mongodb.com/docs/atlas/atlas-search/

This is the highest-weight section. Most questions combine syntax correctness and output reasoning.

## Optional setup data used by examples

```javascript
db.products.deleteMany({});
db.products.insertMany([
  { _id: 1, name: "XPhone", price: 799, color: ["white", "black"], storage: [64, 128, 256], status: "A" },
  { _id: 2, name: "XPad", price: 899, color: ["white", "black", "purple"], storage: [128, 256, 512], status: "B" },
  { _id: 3, name: "GPad", price: 699, color: ["white", "orange", "gold", "gray"], storage: [128, 256, 1024], status: "A" }
]);
```

## Objective-by-objective guide

### 2.1 Insert command correctness

Syntax:

```javascript
db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: true });
```

Expected output:

```javascript
{ acknowledged: true, insertedId: ObjectId("...") }
```

### 2.2 Replace-style update output

replaceOne replaces the entire matched document except immutable _id.

Syntax:

```javascript
db.coll.replaceOne({ _id: 1 }, { a: "ten", b: "five" });
```

Expected output:

```javascript
{ acknowledged: true, matchedCount: 1, modifiedCount: 1, upsertedId: null }
```

If original document is:

```javascript
{ _id: 1, a: "one", b: "four", c: "three" }
```

After replaceOne:

```javascript
{ _id: 1, a: "ten", b: "five" }
```

### 2.3 $set update output

Syntax:

```javascript
db.coll.updateOne({ _id: 1 }, { $set: { b: 2 } });
```

Expected output:

```javascript
{ acknowledged: true, matchedCount: 1, modifiedCount: 1, upsertedId: null }
```

Only field b changes. Other fields remain.

### 2.4 Upsert command selection

Correct key is upsert in options object (not $upsert).

Syntax:

```javascript
db.cakeFlavors.updateOne(
  { flavor: "strawberry" },
  { $set: { number: 100 } },
  { upsert: true }
);
```

Expected output when no document matches:

```javascript
{
  acknowledged: true,
  matchedCount: 0,
  modifiedCount: 0,
  upsertedCount: 1,
  upsertedId: ObjectId("...")
}
```

### 2.5 Multi-document update expression

Syntax:

```javascript
db.movies.updateMany(
  { year: { $lt: 2000 } },
  { $set: { classic: true } }
);
```

Expected output:

```javascript
{ acknowledged: true, matchedCount: 12, modifiedCount: 12, upsertedId: null }
```

### 2.6 findAndModify concurrent scenario

Use single-document atomic methods when you must update/delete and return one affected document.

Syntax:

```javascript
db.loans.findOneAndDelete({ book: "EFF", name: "T.B." });
```

Expected output (deleted document):

```javascript
{ _id: ObjectId("..."), book: "EFF", name: "T.B.", dueDate: ISODate("...") }
```

### 2.7 Delete expression selection

Syntax:

```javascript
db.inventory.deleteOne({ status: "C" });
db.inventory.deleteMany({ status: "C" });
```

Expected output:

```javascript
{ acknowledged: true, deletedCount: 1 }
{ acknowledged: true, deletedCount: 7 }
```

### 2.8 Single-document equality lookup

Syntax:

```javascript
db.ratings.findOne({ hotel: "CCC" });
```

Expected output:

```javascript
{ _id: ObjectId("..."), hotel: "CCC", stars: 4 }
```

### 2.9 Array-field equality matching

Equality on array field matches membership.

Syntax:

```javascript
db.products.find({ color: "purple" });
```

Expected output (sample):

```javascript
{ _id: 2, name: "XPad", color: ["white", "black", "purple"], ... }
```

### 2.10 Relational operators

Syntax:

```javascript
db.products.find({ price: { $lte: 800 } });
```

Expected output (sample):

```javascript
{ _id: 1, name: "XPhone", price: 799, ... }
{ _id: 3, name: "GPad", price: 699, ... }
```

### 2.11 $in matching

Syntax:

```javascript
db.products.find({ status: { $in: ["A", "B"] } });
```

Expected output:

```javascript
{ _id: 1, status: "A", ... }
{ _id: 2, status: "B", ... }
{ _id: 3, status: "A", ... }
```

### 2.12 $elemMatch

Use $elemMatch when multiple conditions must be true on the same array element.

Syntax:

```javascript
db.orders.find({
  items: { $elemMatch: { sku: "ABC", qty: { $gte: 2 } } }
});
```

Expected output:

```javascript
{ _id: ObjectId("..."), items: [{ sku: "ABC", qty: 2 }, { sku: "XYZ", qty: 1 }] }
```

### 2.13 Logical operators

Syntax:

```javascript
db.products.find({
  $and: [
    { price: { $lte: 800 } },
    { $or: [{ color: "purple" }, { storage: 1024 }] }
  ]
});
```

Expected output for sample data:

```javascript
{ _id: 3, name: "GPad", price: 699, storage: [128, 256, 1024], ... }
```

### 2.14 Sort + limit output

Syntax:

```javascript
db.restaurants.find({}).sort({ rating: -1 }).limit(1);
```

Expected behavior:
- First sort all matches by rating descending.
- Then return only the first document.

### 2.15 Incorrect projection detection

Rule:
- Include (1) or exclude (0), not both.
- _id can be excluded while including other fields.

Syntax:

```javascript
db.users.find({}, { name: 1, email: 1, _id: 0 });
```

Expected output:

```javascript
{ name: "Ava", email: "ava@example.com" }
{ name: "Noah", email: "noah@example.com" }
```

### 2.16 Cursor materialization

find returns a cursor. toArray materializes all matching documents.

Syntax:

```javascript
db.inventory.find({}).toArray();
```

Expected output:

```javascript
[
  { _id: 1, item: "pen" },
  { _id: 2, item: "notebook" }
]
```

### 2.17 Count matched documents

Syntax:

```javascript
db.orders.countDocuments({ status: "OPEN" });
```

Expected output:

```javascript
42
```

### 2.18 Search index definition (Atlas Search)

Atlas Search indexes are configured in Atlas, not with db.collection.createIndex.

Definition example:

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

Expected result:
- Atlas creates the Search index and eventually shows Ready status.

### 2.19 Search query selection

Use path and query under $search.text.

Syntax:

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

Expected output (sample):

```javascript
{ _id: 4, name: "Cubanos Inc.", active: false }
```

### 2.20 Aggregation output with $match and $group

Stage order matters. If $match is after $group, it filters grouped results.

Syntax:

```javascript
db.scores.aggregate([
  { $group: { _id: "$player", score: { $avg: "$score" } } },
  { $match: { score: { $gt: 70 } } }
]);
```

Expected output (sample):

```javascript
{ _id: "p1", score: 89 }
{ _id: "p2", score: 75 }
{ _id: "p6", score: 100 }
```

### 2.21 Aggregation output with $lookup

Syntax:

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

Expected output shape:

```javascript
{
  _id: ObjectId("..."),
  customerId: ObjectId("..."),
  customer: [
    { _id: ObjectId("..."), name: "Alex" }
  ]
}
```

### 2.22 Aggregation output with $out

Syntax:

```javascript
db.sales.aggregate([
  { $match: { status: "PAID" } },
  { $out: "sales_paid" }
]);
```

Expected behavior:
- Pipeline writes final documents into collection sales_paid.
- The command itself does not return the written documents.

Verify output collection:

```javascript
db.sales_paid.find();
```

## High-yield exam checklist
- Method choice: one vs many
- replaceOne vs updateOne with $set
- Correct options key: upsert
- Array queries: equality, $in, $elemMatch
- Projection include/exclude rule
- Cursor APIs vs document APIs
- Aggregation stage order reasoning

## Common exam traps
- Fake methods like findMany, updateMulti, updateBulk
- Invalid option key $upsert
- Using field instead of path in Atlas Search text query
- Putting limit inside a filter document

Detailed MCQ practice for this section: [All-Sections Detailed MCQ Bank](../all-sections-detailed-mcq-bank.md#section-2-crud)
Pattern analysis of official options: [Question Pattern Analysis](../question-pattern-analysis.md)

[Previous: Section 1](../01-overview-document-model/README.md) | [Back to Notes Index](../README.md) | [Next: Section 3](../03-indexes/README.md)
