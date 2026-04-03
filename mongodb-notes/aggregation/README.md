# Aggregation Framework

## 1. Overview
- Aggregation pipelines transform and analyze documents through ordered stages.
- The exam set tests grouping, post-group filtering, sorting, limiting, and pipeline syntax correctness.
- Stage order matters because each stage receives output from the previous one.
- Reference docs: https://www.mongodb.com/docs/manual/aggregation/ and https://www.mongodb.com/docs/manual/core/aggregation-pipeline/

## 2. Core Concepts
- `$group`: Aggregates documents by key and computes accumulators (for example `$avg`).
- `$match`: Filters documents (can be before or after `$group` based on intent).
- `$sort`: Orders pipeline results.
- `$limit`: Restricts number of output documents.
- `$out`: Writes pipeline results to a collection.

## 3. Commands / Syntax
- Group and filter by computed average:

```javascript
db.scores.aggregate([
  {
    $group: {
      _id: "$player",
      score: { $avg: "$score" }
    }
  },
  {
    $match: {
      score: { $gt: 70 }
    }
  }
]);
```

- Highest-rated restaurant:

```javascript
const pipeline = [
  { $sort: { rating: -1 } },
  { $limit: 1 }
];
const aggCursor = coll.aggregate(pipeline);
```

- Node.js driver pipeline call:

```javascript
const docs = await db.collection("scores").aggregate([
  { $group: { _id: "$player", score: { $avg: "$score" } } },
  { $match: { score: { $gt: 70 } } }
]).toArray();
```

## 4. Key Rules / Limits
- Pipelines are arrays of stage documents.
- `$match` after `$group` filters aggregated output, not original documents.
- `$sort` and `$limit` are separate stages, not merged keys in one stage.
- `$out` writes final results to a target collection and can replace existing target content.
- Syntax errors in a pipeline prevent writes and results.

## 5. Exam Patterns
- Output-based aggregation questions (compute grouped averages then filter).
- Syntax validation for stage structure and method names.
- Scenario-based "top 1" retrieval using `$sort` + `$limit`.
- Common traps:
- Misplacing `limit` inside `$sort`.
- Using invalid driver helper names like `runAggregation`.
- Missing brackets/braces in pipeline stage arrays.

## 6. Practice Questions
- MCQ: Which pipeline returns players whose average score is greater than 70?
- MCQ: Which syntax correctly returns the single highest-rated restaurant?
- Scenario-based: You must group orders by customer and return only customers with average spend > 100.
- Scenario-based: You need to write aggregation output to another collection. Which stage is correct?
- Output-based: Given 6 documents and a `$group` + `$match` pipeline, what records remain?

## 7. Best Practices
- Place selective `$match` early when filtering source documents for performance.
- Keep stage responsibilities single-purpose and explicit.
- Use descriptive pipeline variable names in Node.js code for readability.
- Validate pipeline syntax in mongosh before embedding in application code.
