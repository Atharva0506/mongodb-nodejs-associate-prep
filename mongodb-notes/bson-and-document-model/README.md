# BSON and Document Model

## 1. Overview
- MongoDB stores data as BSON documents.
- BSON supports richer types than plain JSON, including ObjectId, Date, Decimal128, Int32, Int64, and Boolean.
- Collections are schema-flexible, so documents in the same collection can have different fields and types.
- Every document must have an `_id` field, and MongoDB enforces uniqueness for `_id` within a collection.
- Reference docs: https://www.mongodb.com/docs/manual/reference/bson-types/ and https://www.mongodb.com/docs/manual/core/document/

## 2. Core Concepts
- BSON: Binary representation of JSON-like documents.
- `_id`: Required primary key field in every document.
- Schema flexibility: Different documents can have different shapes in one collection.
- Type correctness: Literals in commands must be valid JavaScript/mongosh values.
- Scalar vs array fields: Queries can match array members directly when equality is used.

## 3. Commands / Syntax
- mongosh insert with valid numeric and boolean values:

```javascript
db.files.insertOne({
  file: "a.log",
  owner: "applicationA",
  size: 1024,
  deleted: true
});
```

- Node.js driver insert with explicit types:

```javascript
import { MongoClient, Int32 } from "mongodb";

const client = new MongoClient(process.env.MONGODB_URI);
await client.connect();

const coll = client.db("app").collection("files");
await coll.insertOne({
  file: "a.log",
  owner: "applicationA",
  size: new Int32(1024),
  deleted: true
});

await client.close();
```

## 4. Key Rules / Limits
- `_id` must be unique in a collection.
- A document maximum size is 16 MiB.
- Field names cannot contain the null character.
- BSON has specific numeric types; generic labels like "Number" are not BSON type names.
- JavaScript boolean literals are lowercase: `true` and `false`.

## 5. Exam Patterns
- Valid vs invalid literal syntax in mongosh commands.
- Which documents can be inserted together (focus on `_id` collisions).
- Type-choice questions that test exact BSON terminology.
- Common traps:
- Confusing `True` with `true`.
- Using invalid numeric literals like `1KB` instead of `1024`.
- Assuming all fields in a collection must share one strict schema.

## 6. Practice Questions
- MCQ: Which is a valid BSON numeric type name?
- MCQ: Which insert command is valid in mongosh for a boolean field?
- Scenario-based: A collection already has `_id: 2`. Which of four candidate documents can be inserted without error?
- Scenario-based: Two new documents have different field sets and types. Can both be inserted into the same collection?
- Output-based: Predict whether insertMany succeeds or fails when one document duplicates `_id`.

## 7. Best Practices
- Keep `_id` stable and unique; let MongoDB generate ObjectId if there is no business key.
- Use consistent field naming and data types even in schema-flexible collections.
- Validate input values in application code before inserting.
- Use BSON-native types intentionally when precision and range matter.
