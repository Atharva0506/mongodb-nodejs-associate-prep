# Section 4: Data Modeling (4%)

Official references:
- https://www.mongodb.com/docs/manual/data-modeling/

## Objective 4.1: Parent/children embed vs link decisions

Choose data model based on access pattern and growth pattern.

Use embedding when:
- Child data is bounded
- Parent and child are read together frequently
- You want single-document reads/updates

Use linking (references) when:
- Child data can grow unbounded
- Child has independent lifecycle/query patterns
- Duplicating child data would be expensive

Embedded example:
```javascript
{
	_id: 1,
	product: "XPhone",
	inventory: { available: 25, warehouse: "W1" }
}
```

Referenced example:
```javascript
{
	_id: 9001,
	orderNo: "O-1001",
	customerId: ObjectId("64f...")
}
```

## Objective 4.2: Identify anti-pattern data models

Common anti-patterns:
- Unbounded arrays inside frequently updated documents
- Model requiring many lookups for a simple common read
- Document growth pattern that causes heavy rewrites

Exam reasoning method:
- Identify the most common read path in the prompt
- Check whether related data is bounded or unbounded
- Prefer model that minimizes frequent high-cost operations

## MCQ Practice (Section 4)

### Q4.1
Embedding is usually best when:
- A. Data is unbounded and independently queried
- B. Related data is bounded and read with parent
- C. Parent and child are unrelated
- D. Child docs are extremely large and rarely used
Answer: B

### Q4.2
Referencing is generally better when:
- A. Child data is small and fixed
- B. Child data is unbounded and independently accessed
- C. Single-document reads are required most of the time
- D. No relationship exists between entities
Answer: B

### Q4.3
Which is a common data model anti-pattern?
- A. Small embedded profile object
- B. Unbounded activity log array in a hot document
- C. Using ObjectId references
- D. Compound indexing frequent query fields
Answer: B

### Q4.4
Primary factor for embed vs link choice:
- A. Field name alphabetic order
- B. Access and update patterns
- C. Driver version only
- D. Number of databases in cluster
Answer: B

### Q4.5
If users always read parent + child together and child is bounded, prefer:
- A. Link only
- B. Embed
- C. Separate shard key first
- D. Text index only
Answer: B

### Q4.6
Which model often increases read complexity unnecessarily?
- A. Embedding tiny, frequently co-read data
- B. Referencing every small child even for simple common reads
- C. Keeping `_id` unique
- D. Using findOne for key lookups
Answer: B
