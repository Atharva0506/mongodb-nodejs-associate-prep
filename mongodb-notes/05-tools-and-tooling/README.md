# Section 5: Tools and Tooling (2%)

[Home](../../README.md) | [Notes Index](../README.md) | [Previous: Section 4](../04-data-modeling/README.md) | [Next: Section 6](../06-drivers-nodejs/README.md)

Official references:
- https://www.mongodb.com/docs/atlas/atlas-ui/
- https://www.mongodb.com/docs/atlas/atlas-ui/collections/
- https://www.mongodb.com/docs/atlas/atlas-ui/agg-pipeline-builder/

This section is about using Atlas Data Explorer correctly for filters and aggregations.

## Objective 5.1: Atlas Sample Dataset + Data Explorer workflow

Recommended flow:
1. Open Atlas project and cluster.
2. Click Load Sample Dataset.
3. Open Data Explorer.
4. Select sample database and collection.
5. Use Find for direct filters.
6. Use Aggregation for pipeline questions.

### Find view syntax example

Filter:

```javascript
{ dessert_type: "Cookie", ingredients: { $nin: ["chocolate"] } }
```

Expected output (sample):

```javascript
{ _id: ObjectId("..."), name: "Oatmeal Raisin", dessert_type: "Cookie", ingredients: ["oats", "raisins"] }
```

### Aggregation view syntax example

Pipeline:

```javascript
[
  { $match: { dessert_type: "Cookie", ingredients: { $nin: ["chocolate"] } } },
  { $limit: 1 }
]
```

Expected behavior:
- $match filters first.
- $limit trims result set to one document.

Expected output (sample):

```javascript
{ _id: ObjectId("..."), name: "Oatmeal Raisin", dessert_type: "Cookie", ingredients: ["oats", "raisins"] }
```

## Common exam traps
- Putting $limit inside a Find filter object.
- Using Find UI when prompt explicitly asks for aggregation pipeline stages.
- Using invalid JSON-like syntax in Atlas filter bar.

Detailed MCQ practice for this section: [All-Sections Detailed MCQ Bank](../all-sections-detailed-mcq-bank.md#section-5-tools-and-tooling)
Pattern analysis of official options: [Question Pattern Analysis](../question-pattern-analysis.md)

[Previous: Section 4](../04-data-modeling/README.md) | [Back to Notes Index](../README.md) | [Next: Section 6](../06-drivers-nodejs/README.md)
