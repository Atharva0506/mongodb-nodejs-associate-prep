# Node.js Driver Fundamentals

## 1. Overview
- The practice set includes direct Node.js driver API recognition and connection pooling behavior.
- Exam questions test method names, proper update syntax, and why pooling improves runtime behavior.
- Reference docs: https://www.mongodb.com/docs/drivers/node/current/ and https://www.mongodb.com/docs/drivers/node/current/connect/connection-options/connection-pools/

## 2. Core Concepts
- `MongoClient`: Primary entry point for connections and database access.
- `db()`: Returns a `Db` instance from a client.
- `close()`: Closes client resources and pooled connections.
- Connection pool: Reusable connections maintained per server endpoint.
- Benefit focus:
- Reduces latency by reusing established connections.
- Limits connection explosion via pool size controls.

## 3. Commands / Syntax
- Client lifecycle and CRUD in Node.js:

```javascript
import { MongoClient } from "mongodb";

const client = new MongoClient(process.env.MONGODB_URI, {
  maxPoolSize: 20,
  minPoolSize: 5
});

await client.connect();
const db = client.db("app");
const movies = db.collection("movies");

await movies.updateMany(
  { genre: "sci-fi" },
  { $set: { genre: "science fiction" } }
);

await client.close();
```

- Aggregation in Node.js:

```javascript
const top = await db.collection("restaurants")
  .aggregate([{ $sort: { rating: -1 } }, { $limit: 1 }])
  .toArray();
```

## 4. Key Rules / Limits
- Use valid method names from the driver API (`db`, `close`, `updateMany`, `aggregate`).
- Reuse one `MongoClient` per process/service context instead of creating a client per request.
- Pooling does not remove authentication requirements.
- Pooling does not eliminate the need for proper startup/shutdown lifecycle handling.

## 5. Exam Patterns
- Method-name validation (real API vs fake methods).
- Proper object-literal syntax in JavaScript update documents.
- Connection pooling conceptual benefits.
- Common traps:
- Using assignment syntax (`=`) inside object literals.
- Calling deprecated or nonexistent methods.
- Misstating pooling as a replacement for authentication.

## 6. Practice Questions
- MCQ: Which two are valid `MongoClient` methods?
- MCQ: Which statement correctly describes connection pooling benefits?
- Scenario-based: A service spikes in traffic and creates many request-time clients. What should change?
- Scenario-based: Write a valid `updateMany` call to rename a genre.
- Output-based: Given four code snippets, identify which one executes without syntax/API errors.

## 7. Best Practices
- Create one shared `MongoClient` and reuse it.
- Tune `maxPoolSize` and `minPoolSize` using workload characteristics.
- Use async/await with explicit error handling around DB calls.
- Close the client during graceful shutdown to release resources cleanly.
