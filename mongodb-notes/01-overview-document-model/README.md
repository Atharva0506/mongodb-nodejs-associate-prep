# Section 1: MongoDB Overview and the Document Model (8%)

[Home](../../README.md) | [Notes Index](../README.md) | [Next: Section 2](../02-crud/README.md)

Official references:
- https://www.mongodb.com/docs/manual/core/document/
- https://www.mongodb.com/docs/manual/reference/bson-types/

This section checks whether you understand MongoDB document structure, BSON types, and schema flexibility.

## Objective 1.1: Identify BSON value types MongoDB supports

MongoDB stores documents as BSON (Binary JSON). BSON extends JSON with MongoDB-specific types like ObjectId, Date, Decimal128, and different integer sizes.

Common BSON types tested in the exam:
- String
- Boolean
- Int32
- Int64
- Double
- Decimal128
- Date
- ObjectId
- Array
- Embedded document
- Null

### Syntax example

```javascript
db.files.insertOne({
  file: "a.log",
  owner: "applicationA",
  sizeBytes: Int32(1024),
  checksum: NumberLong("922337203685477580"),
  deleted: true,
  tags: ["audit", "ops"],
  metadata: { region: "us-east-1" },
  price: NumberDecimal("19.99"),
  createdAt: new Date(),
  optionalNote: null
});
```

### Expected output (mongosh)

```javascript
{ acknowledged: true, insertedId: ObjectId("...") }
```

Notes:
- Use lowercase boolean literals: true and false.
- 1KB is not valid syntax. Use 1024.
- If you omit _id, MongoDB auto-generates an ObjectId.

## Objective 1.2: Identify which document shapes can co-exist in one collection

MongoDB collections are schema-flexible by default. Documents in the same collection can have different fields and field types.

Hard rule that is always enforced:
- Every document has _id.
- _id must be unique inside that collection.

### Syntax example

```javascript
db.items.insertMany([
  { _id: 1, name: "XPhone", price: 799, specs: { color: "black" } },
  { _id: 2, sku: "A-100", inStock: true, tags: ["sale"] },
  { _id: 3, name: "Bundle", products: [{ sku: "A-100", qty: 2 }] }
]);
```

### Expected output (mongosh)

```javascript
{
  acknowledged: true,
  insertedIds: {
    '0': 1,
    '1': 2,
    '2': 3
  }
}
```

### Query to verify mixed document shapes

```javascript
db.items.find({}, { _id: 1, name: 1, sku: 1, specs: 1 }).sort({ _id: 1 });
```

### Expected output (sample)

```javascript
{ _id: 1, name: "XPhone", specs: { color: "black" } }
{ _id: 2, sku: "A-100" }
{ _id: 3, name: "Bundle" }
```

## Common exam traps
- Choosing invalid BSON names from fake options (for example BIGINT as written text).
- Assuming one collection must have one rigid schema.
- Forgetting that duplicate _id causes write failure even when other fields are valid.

[Back to Notes Index](../README.md) | [Next: Section 2](../02-crud/README.md)
