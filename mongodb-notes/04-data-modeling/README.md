# Section 4: Data Modeling (4%)

[Home](../../README.md) | [Notes Index](../README.md) | [Previous: Section 3](../03-indexes/README.md) | [Next: Section 5](../05-tools-and-tooling/README.md)

Official references:
- https://www.mongodb.com/docs/manual/data-modeling/
- https://www.mongodb.com/docs/manual/core/data-model-design/

This section is about choosing a model that matches read/write patterns, not about memorizing one universal rule.

## Objective 4.1: Parent/child embed vs link decisions

Use embedding when:
- Child data is bounded.
- Parent and child are read together most of the time.
- You want single-document atomic updates.

Use references (linking) when:
- Child data can grow unbounded.
- Child has independent lifecycle and query paths.
- Duplicating child data would be expensive.

### Embedded model example

```javascript
db.products.insertOne({
  _id: 1,
  name: "XPhone",
  inventory: { available: 25, warehouse: "W1" }
});
```

Read syntax:

```javascript
db.products.findOne({ _id: 1 });
```

Expected output:

```javascript
{ _id: 1, name: "XPhone", inventory: { available: 25, warehouse: "W1" } }
```

### Referenced model example

```javascript
db.orders.insertOne({
  _id: 9001,
  orderNo: "O-1001",
  customerId: ObjectId("64f0c8e0d5d1cf9f0c000001")
});
```

Join-style read syntax (aggregation lookup):

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
  _id: 9001,
  orderNo: "O-1001",
  customerId: ObjectId("64f0c8e0d5d1cf9f0c000001"),
  customer: [{ _id: ObjectId("64f0c8e0d5d1cf9f0c000001"), name: "Ava" }]
}
```

## Objective 4.2: Identify anti-pattern data models

Common anti-patterns:
- Unbounded arrays in frequently updated documents.
- Excessive references for a very common simple read.
- Large document growth that repeatedly rewrites documents.

### Anti-pattern example (unbounded array)

```javascript
{
  _id: 1,
  user: "u1",
  activityLog: [ /* grows forever */ ]
}
```

Why this is risky:
- Frequent updates to a very large parent document.
- Worse write performance as document grows.

Better pattern:

```javascript
db.activity.insertOne({ userId: 1, action: "login", at: new Date() });
db.activity.createIndex({ userId: 1, at: -1 });
```

Expected output from insertOne:

```javascript
{ acknowledged: true, insertedId: ObjectId("...") }
```

## Exam decision checklist
1. Find the most common access pattern in the prompt.
2. Check if child cardinality is bounded or unbounded.
3. Prefer the model that reduces frequent high-cost reads/writes.
4. Use embedding for co-read bounded data; use references for independent/unbounded data.

[Previous: Section 3](../03-indexes/README.md) | [Back to Notes Index](../README.md) | [Next: Section 5](../05-tools-and-tooling/README.md)
