# Schema Design

## 1. Overview
- The questions include practical data modeling decisions focused on embedding related data in product documents.
- MongoDB schema design emphasizes workload-driven modeling instead of a single normalized shape.
- Exam scenarios test whether embedded arrays are appropriate for read patterns and update frequency.
- Reference docs: https://www.mongodb.com/docs/manual/data-modeling/ and https://www.mongodb.com/docs/manual/core/data-model-design/

## 2. Core Concepts
- Embedding: Store related data inside one document for locality and single-document reads.
- Referencing: Store relationships across collections when data grows unbounded or is shared.
- Bounded vs unbounded arrays:
- Bounded arrays are safer to embed.
- Unbounded arrays can cause document growth and performance issues.
- Access pattern first: Model around dominant queries and updates.

## 3. Commands / Syntax
- Embedded inventory in product document:

```javascript
db.products.insertOne({
  sku: "X-100",
  name: "XPhone",
  price: 799,
  inventory: {
    available: 42,
    warehouse: "W1",
    updatedAt: new Date()
  }
});
```

- Node.js driver example:

```javascript
await db.collection("products").updateOne(
  { sku: "X-100" },
  {
    $set: {
      "inventory.available": 40,
      "inventory.updatedAt": new Date()
    }
  }
);
```

## 4. Key Rules / Limits
- One MongoDB document has a 16 MiB size limit.
- Embedding works best when related data is frequently read together and remains bounded.
- Large, fast-growing, or independently queried entities are often better referenced.
- Keep field naming and types consistent across documents for operational simplicity.

## 5. Exam Patterns
- Architecture-choice MCQs: what should be embedded vs not embedded.
- Product-focused scenarios emphasizing current-state fields (for example availability).
- Common traps:
- Embedding unbounded historical/event arrays directly in hot documents.
- Choosing a model without considering read/write access pattern.

## 6. Practice Questions
- MCQ: For product pages, which data is most suitable to embed in each product document?
- MCQ: Which data is most risky to embed due to unbounded growth?
- Scenario-based: An app always needs product plus current inventory in one read. Suggest model direction.
- Scenario-based: Historical price snapshots grow continuously. Should this remain embedded or move to a separate collection?
- Output-based: Given two schema options, predict which yields fewer reads for an inventory lookup API.

## 7. Best Practices
- Start from query patterns, then design document boundaries.
- Keep frequently co-read data together when bounded.
- Avoid unbounded arrays in core documents.
- Revisit schema as access patterns evolve.
