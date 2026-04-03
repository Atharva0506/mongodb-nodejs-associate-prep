# Section 5: Tools and Tooling (2%)

Official references:
- Atlas Data Explorer docs in MongoDB Atlas documentation

## Objective 5.1: Atlas Sample Dataset + Data Explorer workflow

You should know this practical sequence:
1. Open Atlas project and cluster.
2. Load Sample Dataset.
3. Open Data Explorer.
4. Select database and collection.
5. Use Find view for direct filters.
6. Use Aggregation view for pipeline scenarios.
7. Apply limit correctly:
- Find view: use query limit control
- Aggregation view: use `$limit` stage

Example Find filter:
```javascript
{ dessert_type: "Cookie", ingredients: { $nin: ["chocolate"] } }
```

Example Aggregation pipeline:
```javascript
[
	{ $match: { dessert_type: "Cookie", ingredients: { $nin: ["chocolate"] } } },
	{ $limit: 1 }
]
```

## Common exam traps
- Putting `$limit` inside a filter document.
- Using wrong view when the question explicitly asks for aggregation steps.
- Incorrect JSON-like filter syntax.

## MCQ Practice (Section 5)

### Q5.1
To find one matching document in Aggregation view, limit should be applied using:
- A. `limit` key inside `$match`
- B. `$limit` stage
- C. `findOne` stage
- D. `sort.limit` field
Answer: B

### Q5.2
For simple equality/exclusion filtering in Atlas Data Explorer, preferred starting view is usually:
- A. Schema tab
- B. Performance Advisor
- C. Find view
- D. Charts view
Answer: C

### Q5.3
Which filter excludes documents containing `"chocolate"` in an ingredients array?
- A. `{ ingredients: { $all: ["chocolate"] } }`
- B. `{ ingredients: { $nin: ["chocolate"] } }`
- C. `{ ingredients: { $in: ["chocolate"] } }`
- D. `{ ingredients: { $not: "chocolate" } }`
Answer: B

### Q5.4
Correct workflow order starts with:
- A. Open Data Explorer, then load sample dataset
- B. Load sample dataset, then open Data Explorer
- C. Build aggregation, then create collection
- D. Create shard key, then open Find view
Answer: B

### Q5.5
Which is a common syntax mistake in exam options?
- A. Using `$limit` as aggregation stage
- B. Using `$nin` with array value
- C. Putting `$limit` inside filter object
- D. Using Find view for simple query
Answer: C
