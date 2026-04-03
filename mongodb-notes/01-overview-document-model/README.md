# Section 1: MongoDB Overview and the Document Model (8%)

Official references:
- https://www.mongodb.com/docs/manual/core/document/
- https://www.mongodb.com/docs/manual/reference/bson-types/

## Objective 1.1: Identify BSON value types MongoDB supports

MongoDB stores data as BSON (Binary JSON), which extends JSON with extra types.

High-yield types for exam:
- String
- Boolean
- 32-bit integer (Int32)
- 64-bit integer (Int64)
- Double
- Decimal128
- Date
- ObjectId
- Array
- Embedded document (object)
- Null

Practical rules:
- Use valid JavaScript literals in mongosh.
- `true` and `false` are valid; `True` and `False` are invalid.
- `1KB` is invalid syntax; use numeric bytes like `1024`.

Example:
```javascript
db.files.insertOne({
	file: "a.log",
	owner: "applicationA",
	size: 1024,
	deleted: true,
	createdAt: new Date()
});
```

## Objective 1.2: Identify which document shapes can co-exist in one collection

MongoDB is schema-flexible by default:
- Different documents in the same collection can have different fields.
- Field types can differ across documents.

Always enforced:
- Every document has an `_id`.
- `_id` must be unique in the collection.

Examples:
```javascript
db.items.insertOne({ _id: 1, name: "X", tags: ["new"] });
db.items.insertOne({ _id: 2, sku: 9001, specs: { color: "black" } });
```

## Common exam traps
- Assuming all documents must share exactly one schema.
- Ignoring duplicate `_id` conflicts.
- Confusing BSON names with generic language type labels.

## MCQ Practice (Section 1)

### Q1
Which BSON numeric type is explicitly valid in MongoDB terminology?
- A. Number
- B. BIGINT
- C. 32-bit integer
- D. NumericType
Answer: C

### Q2
Which statement is true for one collection?
- A. All documents must have identical fields.
- B. Documents can differ in shape, but `_id` must remain unique.
- C. Arrays are not allowed.
- D. Embedded documents are not allowed.
Answer: B

### Q3
What happens if `_id` is omitted in `insertOne`?
- A. Insert fails always.
- B. MongoDB auto-generates `_id`.
- C. `_id` becomes 0.
- D. `_id` is added only after update.
Answer: B

### Q4
Which mongosh literal is valid for a boolean?
- A. TRUE
- B. True
- C. true
- D. t
Answer: C

### Q5
Which insert command is syntactically valid?
- A. `db.files.insertOne({ size: 1KB, deleted: True })`
- B. `db.files.insertOne({ size: 1024, deleted: true })`
- C. `db.files.addOne({ size: 1024, deleted: true })`
- D. `db.files.insert({ size: 1024, deleted: True })`
Answer: B

### Q6
Two documents have different fields but unique `_id`. Can both be inserted?
- A. No, because schemas differ.
- B. Yes, schema flexibility allows this.
- C. No, unless validation is disabled globally.
- D. Only in Atlas, not in self-managed MongoDB.
Answer: B
