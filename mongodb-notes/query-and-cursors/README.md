# Query Operators and Cursors

## 1. Overview
- Query questions test filter logic, array matching behavior, sorting, and cursor handling.
- MongoDB query documents use operators such as `$and`, `$or`, `$lte`, `$gt`, `$lt`, and `$nin`.
- `find()` returns a cursor; additional methods control materialization and iteration.
- Reference docs: https://www.mongodb.com/docs/manual/tutorial/query-documents/ and https://www.mongodb.com/docs/manual/tutorial/iterate-a-cursor/

## 2. Core Concepts
- Logical operators: `$and`, `$or`, `$nor`.
- Comparison operators: `$lt`, `$lte`, `$gt`, `$gte`.
- Array query behavior:
- Equality on an array field matches if the value exists in the array.
- `$nin` excludes documents whose field equals any disallowed values.
- Cursor: lazy result object returned from `find()`.
- Materialization: `toArray()` converts cursor results into an array.

## 3. Commands / Syntax
- mongosh filter logic and cursor usage:

```javascript
const cursor = db.products.find({
  $and: [
    { price: { $lte: 800 } },
    { $or: [{ color: "purple" }, { storage: 1024 }] }
  ]
});

const docs = cursor.toArray();
```

- Atlas-style filter example used in aggregation builder:

```javascript
db.desserts.aggregate([
  { $match: { dessert_type: "Cookie", ingredients: { $nin: ["chocolate"] } } },
  { $limit: 1 }
]);
```

- Node.js driver equivalent:

```javascript
const docs = await db.collection("products")
  .find({ price: { $lte: 800 }, $or: [{ color: "purple" }, { storage: 1024 }] })
  .toArray();
```

## 4. Key Rules / Limits
- `findOne()` returns a single document, not a cursor.
- `find().toArray()` reads all matched documents into memory.
- For large result sets, streaming/iteration is safer than immediate `toArray()`.
- Operator structure must be valid JSON-like syntax with correctly nested objects/arrays.

## 5. Exam Patterns
- Output-based questions: determine which document matches a composite filter.
- API-choice questions: choose `findOne` vs `find().toArray()`.
- Scenario-driven filtering with exclusion logic (`$nin`) and limit.
- Common traps:
- Using nonexistent methods like `findMany()`.
- Misplacing `$limit` inside filter documents instead of pipeline stages.
- Ignoring array-match semantics.

## 6. Practice Questions
- MCQ: Which command returns all matching documents as an array in mongosh?
- MCQ: Which filter excludes documents containing `"chocolate"` in an ingredients array?
- Scenario-based: A product can match if it has color `purple` OR storage `1024`, but only if price is <= 800. Write the filter.
- Scenario-based: Choose between `findOne()` and `find().toArray()` for a report that needs all rows.
- Output-based: Given five products, which one is returned by a composite `$and/$or` filter?

## 7. Best Practices
- Keep filters simple and explicit; use logical operators only when needed.
- Validate query shape in code reviews for nested operators.
- Use projection to reduce payload size when returning many documents.
- Choose cursor consumption strategy based on expected cardinality.
