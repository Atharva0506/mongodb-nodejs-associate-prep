# Official Question Pattern Analysis (Based on offical-question.md)

[Home](../README.md) | [Notes Index](README.md)

Source analyzed:
- [Official Practice Questions](../offical-question.md)

## Why this matters

MongoDB Associate questions are usually not trivia-only. They are pattern-recognition questions where distractors look almost correct.

## Pattern 1: Near-valid syntax distractors

Common wrong options intentionally include one small syntax issue:
- Invalid literal casing: `True` instead of `true`
- Invalid numeric form: `1KB` instead of `1024`
- Fake method names: `findMany`, `updateMulti`, `runAggregation`
- Wrong option key: `$upsert` instead of `upsert`

How to solve fast:
1. Validate method name first.
2. Validate operator key names second.
3. Validate value literals and JSON-like shape third.

## Pattern 2: One-vs-many operation traps

Frequent confusion pairs:
- `updateOne` vs `updateMany`
- `deleteOne` vs `deleteMany`
- `findOne` vs `find(...).toArray()`

How to solve fast:
- Look for words like "all", "every", "multiple", "one" in the prompt.

## Pattern 3: Replace vs update semantics

Questions compare:
- `replaceOne` (replaces full doc except `_id`)
- `updateOne(..., { $set: ... })` (field-level change)

How to solve fast:
- If output removes unrelated fields, it was replace-style.
- If unrelated fields remain, it was $set-style.

## Pattern 4: Array matching behavior

Frequent tested differences:
- Equality on array field means "array contains value"
- `$elemMatch` binds multiple conditions to same element
- `$in` checks value membership set

How to solve fast:
- If prompt says "same array element", choose `$elemMatch`.

## Pattern 5: Aggregation stage-order reasoning

Very common:
- `$group` then `$match` means match is applied to grouped output.
- `$match` then `$group` means only matching raw docs are grouped.

How to solve fast:
- Evaluate pipeline left to right.

## Pattern 6: Index key order and direction

Common traps:
- Right fields, wrong order.
- Sort direction confusion in compound indexes.

How to solve fast:
- Match field order to query/filter/sort pattern.
- Remember full reverse direction can often support sort.

## Pattern 7: Atlas Search vs normal indexes

Confusion areas:
- Atlas Search index definition is not `createIndex`.
- In `$search.text`, key is `path`, not `field`.
- Query term key is `query`.

How to solve fast:
- Separate Atlas Search syntax from CRUD/index syntax mentally.

## Pattern 8: Node.js driver API realism

Wrong options often use fake Node driver APIs:
- `open()` / `destroy()` on MongoClient
- `update()` legacy style in places where `updateMany()` expected

How to solve fast:
- Prefer modern documented driver methods: `connect`, `db`, `collection`, `findOne`, `find(...).toArray()`, `aggregate`.

## Option-quality heuristic used in official set

Most wrong options are wrong by exactly one of these:
- Wrong method name
- Wrong operator key
- Wrong argument shape
- Wrong stage placement
- Wrong output reasoning

Exam strategy:
1. Eliminate syntactically impossible options immediately.
2. Eliminate semantically impossible options next.
3. Compare remaining options by operation cardinality (one vs many).
4. Validate output state transition mentally.

## High-yield checklist before exam

- Can you spot fake method names in under 2 seconds?
- Can you distinguish replaceOne from $set output instantly?
- Can you reason array matching with `$elemMatch` vs equality?
- Can you pick index field order for filter + sort?
- Can you evaluate `$group` and `$match` stage order effects?
- Can you identify Atlas Search `path` + `query` syntax?

## Where to practice next

- [All-Sections Detailed MCQ Bank](all-sections-detailed-mcq-bank.md)
- [Section 2: CRUD](02-crud/README.md)
- [Section 3: Indexes](03-indexes/README.md)
- [Section 6: Drivers - Node.js](06-drivers-nodejs/README.md)
