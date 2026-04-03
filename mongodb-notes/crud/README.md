# CRUD Operations

## 1. Overview
- CRUD in MongoDB covers Create, Read, Update, and Delete operations on documents.
- The official APIs differ by operation granularity: one document vs many documents.
- Questions frequently test exact method names and option objects.
- Reference docs: https://www.mongodb.com/docs/manual/crud/ and https://www.mongodb.com/docs/drivers/node/current/crud/

## 2. Core Concepts
- Create: `insertOne`, `insertMany`.
- Read: `find`, `findOne`.
- Update: `updateOne`, `updateMany`, `replaceOne`.
- Delete: `deleteOne`, `deleteMany`, `findOneAndDelete`.
- Upsert: Insert if no matching document exists (`upsert: true`).
- Replacement vs modifier update:
- `replaceOne` replaces the full document except immutable `_id`.
- `$set` updates only targeted fields.

## 3. Commands / Syntax
- mongosh examples:

```javascript
// Create
db.cakeFlavors.insertOne({ flavor: "chocolate", number: 15 });

// Read
db.ratings.findOne({ hotel: "CCC" });

// Update many
db.movie.updateMany({ year: { $lt: 2000 } }, { $set: { classic: true } });

// Upsert
db.cakeFlavors.updateOne(
  { flavor: "strawberry" },
  { $set: { number: 100 } },
  { upsert: true }
);

// Delete many
db.inventory.deleteMany({ status: "C" });

// Delete and return deleted document
db.loans.findOneAndDelete({ book: "EFF", name: "T.B." });
```

- Node.js driver examples:

```javascript
const result = await movies.updateMany(
  { genre: "sci-fi" },
  { $set: { genre: "science fiction" } }
);

const deleted = await loans.findOneAndDelete({ book: "EFF", name: "T.B." });
```

## 4. Key Rules / Limits
- `updateMany` applies to all matching documents.
- `replaceOne` overwrites matched document content except `_id`.
- `findOneAndDelete` returns the deleted document; `deleteOne` does not.
- `upsert` is a top-level option in the options object: `{ upsert: true }`.
- Deprecated/invalid method names are common traps (for example `updateMulti`, `updateBulk`, `find_one`).

## 5. Exam Patterns
- Pick the syntactically valid API call.
- Predict resulting documents after `replaceOne` vs `$set` updates.
- Choose one-document vs many-document delete/update method.
- Common traps:
- Putting options in wrong shape (for example `$upsert` instead of `upsert`).
- Using nonexistent methods.
- Assuming `replaceOne` only changes supplied fields.

## 6. Practice Questions
- MCQ: Which method updates all documents with `year < 2000`?
- MCQ: Which method both deletes one document and returns it?
- Scenario-based: You need to set `b:2` for all documents, including those missing `b`. Which update statement is correct and what is the final output?
- Scenario-based: You need to update by filter and insert if absent. Which options object is valid?
- Output-based: Given three docs, what changes after `replaceOne({}, {a:"ten", b:"five"})`?

## 7. Best Practices
- Prefer specific methods (`updateOne`, `updateMany`) over generic legacy patterns.
- Use precise filters; avoid empty filters in production unless intentionally broad.
- Use `findOneAndDelete` when you need the deleted payload for audit or workflow continuation.
- Log `matchedCount`, `modifiedCount`, and `upsertedId` from write results in application code.
