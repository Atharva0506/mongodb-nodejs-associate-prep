# Section 6: Drivers - Node.js (18%)

[Home](../../README.md) | [Notes Index](../README.md) | [Previous: Section 5](../05-tools-and-tooling/README.md)

Official references:
- https://www.mongodb.com/docs/drivers/node/current/
- https://www.mongodb.com/docs/drivers/node/current/connect/
- https://www.mongodb.com/docs/drivers/node/current/fundamentals/connection/

This section tests practical Node.js driver usage: connection lifecycle, CRUD APIs, and aggregation syntax.

## 6.1 What the Node.js driver is

The official mongodb package lets Node.js applications connect to MongoDB and run CRUD, query, and aggregation operations.

Install syntax:

```bash
npm install mongodb
```

## 6.2 How an application connects and uses the driver

Typical lifecycle from official docs:
1. Create a MongoClient.
2. Connect once at application startup.
3. Reuse db and collection handles.
4. Close gracefully on shutdown.

Syntax:

```javascript
import { MongoClient } from "mongodb";

const client = new MongoClient(process.env.MONGODB_URI);
await client.connect();

const db = client.db("app");
const movies = db.collection("movies");

await client.close();
```

## 6.3 URI components used by MongoClient

URI example:

```text
mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/app?retryWrites=true&w=majority
```

Important parts:
- Scheme: mongodb:// or mongodb+srv://
- Credentials: username:password
- Host or SRV seed
- Optional default database
- Query options

## 6.4 Connection pooling and advantages

Driver uses connection pooling. Reusing pooled connections improves throughput and latency compared to opening a new connection per request.

Syntax:

```javascript
const client = new MongoClient(process.env.MONGODB_URI, {
  maxPoolSize: 30,
  minPoolSize: 5
});
```

## 6.5 Insert one and many syntax

Syntax:

```javascript
const oneResult = await coll.insertOne({ title: "A" });
const manyResult = await coll.insertMany([{ title: "B" }, { title: "C" }]);
```

Expected output objects:

```javascript
oneResult.acknowledged === true
oneResult.insertedId // ObjectId(...)

manyResult.acknowledged === true
manyResult.insertedCount === 2
manyResult.insertedIds // { '0': ObjectId(...), '1': ObjectId(...) }
```

## 6.6 Update one and many syntax

Syntax:

```javascript
const one = await coll.updateOne({ title: "A" }, { $set: { title: "A1" } });
const many = await coll.updateMany({ genre: "sci-fi" }, { $set: { genre: "science fiction" } });
```

Expected output fields:

```javascript
one.matchedCount
one.modifiedCount

many.matchedCount
many.modifiedCount
```

## 6.7 Delete one and many syntax

Syntax:

```javascript
const one = await coll.deleteOne({ title: "B" });
const many = await coll.deleteMany({ archived: true });
```

Expected output fields:

```javascript
one.deletedCount // 0 or 1
many.deletedCount // number of removed docs
```

## 6.8 Find one and many syntax

Syntax:

```javascript
const oneDoc = await coll.findOne({ title: "A1" });
const manyDocs = await coll.find({ genre: "science fiction" }).toArray();
```

Expected output shape:

```javascript
oneDoc // document object or null
manyDocs // array of document objects
```

## 6.9 Aggregation pipeline syntax

Syntax:

```javascript
const agg = await coll
  .aggregate([
    { $match: { genre: "science fiction" } },
    { $group: { _id: "$genre", count: { $sum: 1 } } }
  ])
  .toArray();
```

Expected output example:

```javascript
[{ _id: "science fiction", count: 12 }]
```

## 6.10 MQL syntax vs Aggregation syntax

MQL filter (used by find):

```javascript
{ genre: "science fiction", year: { $gte: 2000 } }
```

Aggregation pipeline:

```javascript
[
  { $match: { genre: "science fiction", year: { $gte: 2000 } } },
  { $project: { _id: 0, title: 1, year: 1 } }
]
```

Difference:
- MQL find uses one filter document.
- Aggregation uses an ordered array of stage documents.

## End-to-end snippet (exam-safe pattern)

```javascript
import { MongoClient } from "mongodb";

const client = new MongoClient(process.env.MONGODB_URI, { maxPoolSize: 20 });

try {
  await client.connect();

  const db = client.db("app");
  const coll = db.collection("movies");

  await coll.insertOne({ title: "Primer", year: 2004, genre: "science fiction" });

  const docs = await coll
    .aggregate([
      { $match: { genre: "science fiction" } },
      { $project: { _id: 0, title: 1, year: 1 } }
    ])
    .toArray();

  console.log(docs);
} finally {
  await client.close();
}
```

Detailed MCQ practice for this section: [All-Sections Detailed MCQ Bank](../all-sections-detailed-mcq-bank.md#section-6-drivers-nodejs)
Pattern analysis of official options: [Question Pattern Analysis](../question-pattern-analysis.md)

[Previous: Section 5](../05-tools-and-tooling/README.md) | [Back to Notes Index](../README.md)
