# Atlas Search

## 1. Overview
- Atlas Search provides full-text search features via the `$search` aggregation stage.
- The question set targets two high-value areas: text search query shape and autocomplete index tokenization.
- Atlas Search indexes are configured separately from classic database indexes.
- Reference docs: https://www.mongodb.com/docs/atlas/atlas-search/ and https://www.mongodb.com/docs/atlas/atlas-search/autocomplete/

## 2. Core Concepts
- `$search` stage: Runs search operators inside aggregation.
- `text` operator: Matches terms in indexed string fields.
- `path`: Field name to search.
- `query`: Search term or terms.
- Autocomplete index `tokenization` options:
- `edgeGram` supports matches from the beginning of terms.

## 3. Commands / Syntax
- Text search by name:

```javascript
db.restaurants.aggregate([
  {
    $search: {
      text: {
        path: "name",
        query: "cuban"
      }
    }
  }
]);
```

- Example autocomplete index mapping (conceptual):

```json
{
  "mappings": {
    "dynamic": false,
    "fields": {
      "name": [
        {
          "type": "autocomplete",
          "tokenization": "edgeGram"
        }
      ]
    }
  }
}
```

- Node.js driver execution:

```javascript
const results = await db.collection("restaurants").aggregate([
  {
    $search: {
      text: { path: "name", query: "cuban" }
    }
  }
]).toArray();
```

## 4. Key Rules / Limits
- `$search` is an aggregation stage, not a `find()` operator.
- `path` is the field key used by Atlas Search operators.
- `query` supplies search text; `synonym` is not a drop-in replacement for `query`.
- Autocomplete behavior depends on index mapping configuration, including tokenization.

## 5. Exam Patterns
- Select the valid `$search` syntax with correct keys (`path`, `query`).
- Select the correct autocomplete tokenization for prefix matching.
- Common traps:
- Using `field` instead of `path`.
- Replacing `query` with `synonym` in a standard text query.
- Treating Atlas Search config like regular `createIndex` definitions.

## 6. Practice Questions
- MCQ: Which `$search` stage correctly finds restaurant names matching "cuban"?
- MCQ: Which autocomplete tokenization supports beginning-of-word matching?
- Scenario-based: A mobile app needs prefix-based restaurant name suggestions. How should the index be configured?
- Scenario-based: Given an existing Atlas Search index, write the aggregation query for name matching.
- Output-based: Predict whether a malformed `$search` object runs or fails validation.

## 7. Best Practices
- Keep search index mappings explicit for exam-relevant fields.
- Use `path` and `query` consistently in search pipelines.
- Validate Atlas Search pipelines in Atlas UI or mongosh before app integration.
- Separate search concerns from transactional query logic in application design.
