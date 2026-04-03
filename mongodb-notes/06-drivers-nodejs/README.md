# Section 6: Drivers - Node.js (18%)

[Home](../../README.md) | [Notes Index](../README.md) | [Previous: Section 5](../05-tools-and-tooling/README.md)

Official references:
- https://www.mongodb.com/docs/drivers/node/current/
- https://www.mongodb.com/docs/drivers/node/current/connect/

## 6.1 What Node.js driver is

The official MongoDB Node.js driver is the package that lets Node applications connect to MongoDB and execute CRUD, queries, and aggregation pipelines.

## 6.2 How an application connects and uses the driver

Typical lifecycle:
1. Instantiate `MongoClient`
2. Connect once at app startup
3. Get `Db` and `Collection` handles
4. Execute operations
5. Close during graceful shutdown

## 6.3 URI components used by MongoClient

Example:
```text
mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/dbName?retryWrites=true&w=majority
```

Parts:
- Protocol: `mongodb://` or `mongodb+srv://`
- Credentials: `username:password`
- Host/cluster
- Optional default database
- Options query string

## 6.4 Connection pooling and advantages

Pooling means reusing established connections instead of creating one per request.

Benefits:
- Lower latency
- Better throughput
- Controlled connection count

## 6.5 Insert one and many syntax

```javascript
await coll.insertOne({ title: "A" });
await coll.insertMany([{ title: "B" }, { title: "C" }]);
```

## 6.6 Update one and many syntax

```javascript
await coll.updateOne({ title: "A" }, { $set: { title: "A1" } });
await coll.updateMany({ genre: "sci-fi" }, { $set: { genre: "science fiction" } });
```

## 6.7 Delete one and many syntax

```javascript
await coll.deleteOne({ title: "B" });
await coll.deleteMany({ archived: true });
```

## 6.8 Find one and many syntax

```javascript
const one = await coll.findOne({ title: "A1" });
const many = await coll.find({ genre: "science fiction" }).toArray();
```

## 6.9 Aggregation pipeline syntax

```javascript
const agg = await coll.aggregate([
  { $match: { genre: "science fiction" } },
  { $group: { _id: "$genre", count: { $sum: 1 } } }
]).toArray();
```

## 6.10 MQL syntax vs Aggregation syntax

MQL filter:
```javascript
{ genre: "science fiction", year: { $gte: 2000 } }
```

Aggregation pipeline:
```javascript
[
  { $match: { genre: "science fiction", year: { $gte: 2000 } } },
  { $project: { title: 1, year: 1 } }
]
```

Difference:
- MQL find uses one filter document.
- Aggregation uses ordered stage documents in an array.

## End-to-end reference snippet

```javascript
import { MongoClient } from "mongodb";

const client = new MongoClient(process.env.MONGODB_URI, {
  maxPoolSize: 20
});

await client.connect();
const db = client.db("app");
const coll = db.collection("movies");

await coll.insertOne({ title: "A" });
const docs = await coll.find({}).toArray();

await client.close();
```

## MCQ Practice (Section 6)

### Q6.1
Valid `MongoClient` method:
- A. `open()`
- B. `db()`
- C. `destroy()`
- D. `attach()`
Answer: B

### Q6.2
Another valid `MongoClient` method:
- A. `close()`
- B. `halt()`
- C. `terminate()`
- D. `flush()`
Answer: A

### Q6.3
Valid SRV URI prefix:
- A. `mongo+srv://`
- B. `mongodb+srv://`
- C. `db+srv://`
- D. `atlas://`
Answer: B

### Q6.4
Connection pooling primarily helps by:
- A. Removing authentication
- B. Reusing connections and reducing latency
- C. Disabling retries
- D. Replacing TLS
Answer: B

### Q6.5
Correct insertMany syntax:
- A. `coll.insertMany({a:1},{a:2})`
- B. `coll.insertMany([{a:1},{a:2}])`
- C. `coll.insert([{a:1},{a:2}])`
- D. `coll.addMany([{a:1},{a:2}])`
Answer: B

### Q6.6
Correct updateMany syntax:
- A. `coll.updateMany({x:1},{set:{y:2}})`
- B. `coll.updateMany({x:1},{$set:{y:2}})`
- C. `coll.update({x:1},{$set:{y:2}},true)`
- D. `coll.modifyMany({x:1},{$set:{y:2}})`
Answer: B

### Q6.7
Correct delete API for many docs:
- A. `deleteAll`
- B. `removeMany`
- C. `deleteMany`
- D. `findAndDeleteMany`
Answer: C

### Q6.8
Correct find one API:
- A. `findOne`
- B. `find().one()`
- C. `getOne`
- D. `find_first`
Answer: A

### Q6.9
Correct aggregation entry method:
- A. `pipeline()`
- B. `aggregate()`
- C. `group()`
- D. `runAggregation()`
Answer: B

### Q6.10
MQL and aggregation differ because:
- A. Both are always same shape
- B. MQL uses filter doc, aggregation uses stage array
- C. MQL cannot filter ranges
- D. Aggregation cannot use `$match`
Answer: B

### Q6.11
Preferred service pattern for client lifecycle:
- A. New client per request
- B. Shared client with pooling
- C. Close after each query in loop
- D. Disable pooling entirely
Answer: B

### Q6.12
To read many query results in Node.js, commonly use:
- A. `findOne` only
- B. `find(...).toArray()`
- C. `findMany().all()`
- D. `getAll()`
Answer: B

[Previous: Section 5](../05-tools-and-tooling/README.md) | [Back to Notes Index](../README.md)
