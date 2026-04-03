# MongoDB Associate Developer Node.js Practice Questions

Source:
https://learn.mongodb.com/learn/course/associate-developer-node-practice-questions/prep-questions/practice-questions?client=customer

---

## Question 1
### Prompt
Which numeric type is a valid MongoDB BSON type?

### Choose
(Choose 1)

### Options
- [ ] a. Float
- [ ] b. Number
- [ ] c. BIGINT
- [ ] d. 32-bit integer (Correct Answer)

---

## Question 2
### Prompt
Given the following documents in a collection:

```json
{ _id: 1, n: [1,2,5], p: 0.75, c: 'Green' },
{ _id: 2, n: 'Orange', p: 'Blue', c: 42, q: 14 },
{ _id: 3, n: [1,3,7], p: 0.85, c: 'Orange' }
```

Which two documents can successfully be added in the same collection?

### Choose
(Choose 2)

### Options
- [ ] a. { _id: 1, n: [1,2,5], p: 0.75, c: 'Green' }
- [ ] b. { _id: 5, n: [1,2,5], p: 0.75, c: 'Green' } (Correct Answer)
- [ ] c. { _id: 2, n: [1,2,5], p: 0.75, c: 'Green' }
- [ ] d. { _id: 6, n: [1,3,7], p: 0.85, c: 'Orange } (Correct Answer)

---

## Question 3
### Prompt
Given the following documents in a collection:

```json
{_id: 1, txt: "just some text"},
{_id: 2, txt: "just some text"}
```

Which two documents can successfully be added in the same collection?

### Choose
(Choose 2)

### Options
- [ ] a. {_id: 0, txt: "just some text"} (Correct Answer)
- [ ] b. {_id: 1, txt: "just some text"}
- [ ] c. {_id: [4], txt: "just some text"}
- [ ] d. {_id: 3, txt: "just some text"} (Correct Answer)

---

## Question 4
### Prompt
Given the following document:

The name is a.log, the owner of the file is applicationA, the size of the file is 1KB, and the file was deleted.

What command will properly add this document to the files collection using mongosh?

### Choose
(Choose 1)

### Options
- [ ] a. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1KB, deleted: true })
- [ ] b. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1KB, deleted: True })
- [ ] c. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: true }) (Correct Answer)
- [ ] d. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: True })

---

## Question 5
### Prompt
Given the following sample documents in products collection:

```json
{ "name" : "XPhone", "price" : 799, "color" : [ "white", "black" ], "storage" : [ 64, 128, 256 ] },
{ "name" : "XPad", "price" : 899, "color" : [ "white", "black", "purple" ], "storage" : [ 128, 256, 512 ] },
{ "name" : "GTablet", "price" : 899, "color" : [ "blue" ], "storage" : [ 16, 64, 128 ] },
{ "name" : "GPad", "price" : 699, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 1024 ] },
{ "name" : "GPhone", "price" : 599, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 512 ] }
```

Given the following query:

```js
db.products.find({$and : [{"price" : {$lte : 800}},
                                       {$or : [{"color" : "purple"}, {"storage" : 1024}]}]})
```

What is the correct output of the query?

### Choose
(Choose 1)

### Options
- [ ] a. { "name" : "XPhone", "price" : 799, "color" : [ "white", "black" ], "storage" : [ 64, 128, 256 ] }
- [ ] b. { "name" : "XPad", "price" : 899, "color" : [ "white", "black", "purple" ], "storage" : [ 128, 256, 512 ] }
- [ ] c. { "name" : "GPhone", "price" : 599, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 512 ] }
- [ ] d. { "name" : "GPad", "price" : 699, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 1024 ] } (Correct Answer)

---

## Question 6
### Prompt
An `inventory` collection consists of 200 documents.

What method should be used to get all documents from a cursor using mongosh?

### Choose
(Choose 1)

### Options
- [ ] a. db.inventory.findOne()
- [ ] b. db.inventory.find().toArray(); Correct Answer
- [ ] c. db.inventory.find();
- [ ] d. db.inventory.findMany().toArray()

---

## Question 7
### Prompt
A collection has documents like the following:

```js
{ _id: 1, name: 'Oatmeal Fruit Cake with Gummy Bears ', price: 11)},
{ _id: 2, name: 'Cheesecake Trifle with Chocolate Sprinkles ', price: 14)},
{ _id: 3, name: 'Pistachio Brownie with Walnuts ', price: 5},
{ _id: 4, name: 'Strawberry Ice Cream Cake with Butterscotch Syrup ', price: 3)}
```

How should the 'autocomplete' index be defined to look for matches at the beginning of a word on the name field?

### Choose
(Choose 1)

### Options
- [ ] a. {  "mappings": {    "dynamic": false,    "fields": {         "name": [   {  "type": "autocomplete",                              "tokenization": "regexCaptureGroup"} ]     } }}
- [ ] b. {  "mappings": {    "dynamic": false,    "fields": {         "name": [   {  "type": "autocomplete",                              "tokenization": "edgeGram"} ]    } }} Correct Answer
- [ ] c. {  "mappings": {    "dynamic": false,    "fields": {         "name": [   {  "type": "autocomplete",                              "tokenization": "nGram"} ]    } }}
- [ ] d. {  "mappings": {    "dynamic": false,    "fields": {         "name": [   {  "type": "autocomplete",                              "tokenization": "matchNGram"}} ]     } }}

---

## Question 8
### Prompt
Given the following sample documents:

```js
{_id:1, name: "Quesedillas Inc.", active: true },
{_id:2, name: "Pasta Inc.", active: true },
{_id:3, name: "Tacos Inc.", active: false },
{_id:4, name: "Cubanos Inc.", active: false },
{_id:5, name: "Chicken Parm Inc.", active: false }
```

A company wants to create a mobile app for users to find restaurants by name. The developer wants to show the user restaurants that match their search. An Atlas Search index has already been created to support this query.

What query satisfies these requirements?

### Choose
(Choose 1)

### Options
- [ ] a. db.restaurants.aggregate([{    "$search": {      "text": { "path": "name", "synonym": "cuban"}    } }])
- [ ] b. db.restaurants.aggregate([{    "$search": {      "text": { "path": "name", "query": "cuban"}    } }]) Correct Answer
- [ ] c. db.restaurants.aggregate([{    "$search": {      "text": { "field": "name", "query": "cuban"}    } }])
- [ ] d. db.restaurants.aggregate([{    "$search": {      "text": { "field": "name", "synonym": "cuban"}    } }])

---

## Question 9
### Prompt
Given the data set and query:

```js
{ "_id" : ObjectId("512bc95fe835e68f199c8686"), "player" : "p1", "score" : 89 }
{ "_id" : ObjectId("512bc962e835e68f199c8687"), "player" : "p2", "score" : 85 }
{ "_id" : ObjectId("55f5a192d4bede9ac365b257"), "player" : "p2", "score" : 65 }
{ "_id" : ObjectId("55f5a192d4bede9ac365b258"), "player" : "p3", "score" : 65 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b259"), "player" : "p3", "score" : 75 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b25a"), "player" : "p5", "score" : 70 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b25b"), "player" : "p6", "score" : 100 }


db.scores.aggregate(
[{ $group: {
      _id: '$player',
      score: {
           $avg: '$score'
      }
 },
 { $match: {
     score: {
            $gt: 70
      }
 }
])
```

What is the output?

### Choose
(Choose 1)

### Options
- [ ] a. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 85 } { "player" : "p3", "score" : 75 } { "player" : "p6", "score" : 100 }
- [ ] b. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p6", "score" : 100 } Correct Answer
- [ ] c. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p3", "score" : 70 } { "player" : "p5", "score" : 70 } { "player" : "p6", "score" : 100 }
- [ ] d. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p3", "score" : 70 } { "player" : "p6", "score" : 100 }

---

## Question 10
### Prompt
A collection coll in database mdb has the following documents:

```js
{_id: 1, type: "A", value: 60}
{_id: 2, type: "B", value: 80}
{_id: 3, type: "C", value: 10}
```

After executing the following aggregation pipeline:

```js
db.getSiblingDB("mdb").coll.aggregate([
    { $out: {db:'test', collection:'results'}} ])
```

What are two expected results?

### Choose
(Choose 2)

### Options
- [ ] a. Collection `results` is created in database `test`.
- [ ] b. There is a syntax error command. Collection `results` is not created Correct Answer
- [ ] c. No documents in collection `coll` are written to collection `results`.Correct Answer
- [ ] d. All documents in collection `coll` are written to collection `results`.

---

## Question 11
### Prompt
Given the following documents:

```js
{_id:1, a: "one", b: "four"}
{_id:2, a: "two", b: "four"}
{_id:3, a: "three", b: "four", c: "three"}
```

If the following command is executed:

```js
db.coll.replaceOne({}, {a: "ten", b: "five"})
```

What is the result?

### Choose
(Choose 1)

### Options
- [ ] a. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five"}
- [ ] b. {_id:1, a: "ten", b: "five"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"} Correct Answer
- [ ] c. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five", c: "three"}
- [ ] d. {_id:1, a: "one", b: "four"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}

---

## Question 12
### Prompt
Given the collection called coll, with only the following documents,

```js
{
 _id:1,
 a:1,
 b:1
},
{
 _id:2,
 a:2
}
```

The update operation db.coll.updateMany({},{$set:{b:2}}) successfully completes.

What is the output of db.coll.find()?

### Choose
(Choose 1)

### Options
- [ ] a. [{_id:1,  b:2}, {_id:2,  b:2}]
- [ ] b. [{_id:1,  a:1,  b:2}, {_id:2,  a:2}]
- [ ] c. [{_id:1,  a:1,  b:1}, {_id:2,  a:2,  b:2}]
- [ ] d. [{_id:1,  a:1,  b:2}, {_id:2,  a:2,  b:2}] Correct Answer

---

## Question 13
### Prompt
Given the following documents:

```js
{_id:1, a: "one", b: "four"}
{_id:2, a: "two", b: "four"}
{_id:3, a: "three", b: "four", c: "three"}
```

If the following command is executed:

```js
db.coll.replaceOne({}, {a: "ten", b: "five"})
```

What is the result?

### Choose
(Choose 1)

### Options
- [ ] a. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five"}
- [ ] b. {_id:1, a: "ten", b: "five"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"} Correct Answer
- [ ] c. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five", c: "three"}
- [ ] d. {_id:1, a: "one", b: "four"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}

---

## Question 14
### Prompt
Given the collection called coll, with only the following documents,

```js
{
 _id:1,
 a:1,
 b:1
},
{
 _id:2,
 a:2
}
```

The update operation db.coll.updateMany({},{$set:{b:2}}) successfully completes.

What is the output of db.coll.find()?

### Choose
(Choose 1)

### Options
- [ ] a. [{_id:1,  b:2}, {_id:2,  b:2}]
- [ ] b. [{_id:1,  a:1,  b:2}, {_id:2,  a:2}]
- [ ] c. [{_id:1,  a:1,  b:1}, {_id:2,  a:2,  b:2}]
- [ ] d. [{_id:1,  a:1,  b:2}, {_id:2,  a:2,  b:2}] Correct Answer

---

## Question 15
### Prompt
Given the following document from the cakeFlavors collection. All documents in this collection have the same schema.

```js
{
"_id" : 1,
"flavor" : "chocolate",
"number" : 15
}
```

What operation on the cakeFlavors collection will update the value of the number field to 100 for a document with a "strawberry" flavor value and insert a new document if it does not exist?

### Choose
(Choose 1)

### Options
- [ ] a. db.cakeFlavors.updateOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { $upsert: true })
- [ ] b. db.cakeFlavors.insertOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { $upsert: true })
- [ ] c. db.cakeFlavors.insertOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { upsert: true })
- [ ] d. db.cakeFlavors.updateOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { upsert: true }) Correct Answer

---

## Question 16
### Prompt
Given the following example document from the movie collection:

```js
{
   _id: ObjectId("62872ccd590c3c06d78af00d"),
   genres: [ "Drama", "Romance", "War" ],
   title: "A.B.",
   year: 1921,
   tomatoes: { rating: 3.9, votes: 507, id: "76" },
   countries: [ "USA" ],
   classic : false
}
```

All documents in this collection have the same schema.

What command updates the value of the classic field to true for all documents with a year value less than 2000?

### Choose
(Choose 1)

### Options
- [ ] a. db.movie.updateOne({ year: { $lt: 2000 } }, { $set: { classic: true } }, { multi: true })
- [ ] b. db.movie.updateMany({ year: { $lt: 2000 } }, { $set: { classic: true } }) Correct Answer
- [ ] c. db.movie.updateMulti({ year: { $lt: 2000 } }, { $set: { classic: true } })
- [ ] d. db.movie.updateBulk({ year: { $lt: 2000 } }, { $set: { classic: true } })

---

## Question 17
### Prompt
Given the following sample documents in the loans collection:

```js
{ _id: 122, book: "ABC", name: "L.A.", date: ISODate("2022-05-20") },
{ _id: 343, book: "EFF", name: "T.B.", date: ISODate("2022-05-22") },
{ _id: 454, book: "CFH", name: "M.C.", date: ISODate("2022-05-12") }
```

What command deletes the document where the value of book is "EFF" and user is "T.B.", and returns the deleted document?

### Choose
(Choose 1)

### Options
- [ ] a. db.loans.deleteOne( { book: "EFF", name: "T.B." } )
- [ ] b. db.loans.findOneAndDelete( { book: "EFF", name: "T.B." } ) Correct Answer
- [ ] c. db.loans.remove( { book: "EFF", name: "T.B." } , {returnDocument: true} )
- [ ] d. db.loans.deleteOne( { book: "EFF", name: "T.B." }, {returnDocument: true} )

---

## Question 18
### Prompt
Given the following sample documents in the inventory collection:

```js
{ _id: 6305, name : "A. S.", "assignment" : 5, "status" : "A" },
{ _id: 6308, name : "B. M.", "assignment" : 3, "status" : "B" },
{ _id: 6312, name : "E. M.", "assignment" : 5, "status" : "C" },
{ _id: 6319, name : "R. S.", "assignment" : 2, "status" : "D" },
{ _id: 6322, name : "A. S.", "assignment" : 2, "status" : "A" },
{ _id: 6234, name : "R. S.", "assignment" : 1, "status" : "B" },
{ _id: 6235, name : "A. S.", "assignment" : 1, "status" : "C" },
{ _id: 6315, name : "E. M.", "assignment" : 3, "status" : "A" }
```

What expression will remove all documents with "status" : "C" in the inventory collection?

### Choose
(Choose 1)

### Options
- [ ] a. db.inventory.delete ({"status" : "C"})
- [ ] b. db.inventory.deleteOne ({"status" : "C"})
- [ ] c. db.inventory.deleteMany ({"status" : "C"}) Correct Answer
- [ ] d. db.inventory.findOneAndDelete ({"status" : "C"})

---

## Question 19
### Prompt
Given the following documents in the ratings collection:

```js
{ _id: 0, hotel: "AAA", rating: 4.5 },
{ _id: 1, hotel: "BBB", rating: 3.0 },
{ _id: 2, hotel: "CCC", rating: 4.2 }
```

What mongosh command will return a document for hotel "CCC"?

### Choose
(Choose 1)

### Options
- [ ] a. db.ratings.return_one( {hotel: "CCC"} )
- [ ] b. db.ratings.find_one( {hotel: "CCC"} )
- [ ] c. db.ratings.returnOne( {hotel: "CCC"} )
- [ ] d. db.ratings.findOne( {hotel: "CCC"} ) Correct Answer

---

## Question 20
### Prompt
The following query generates a collection scan:

```js
db.people.find({employer : "ABC" }).sort ({last_name:1 , job:1})
```

Which two indexes will most improve the performance of the query?

### Choose
(Choose 2)

### Options
- [ ] a. db.people.createIndex({employer:1, last_name : 1  , job : 1 } ) Correct Answer
- [ ] b. db.people.createIndex({employer:1, last_name : -1  , job : -1 } ) Correct Answer
- [ ] c. db.people.createIndex({employer:1,last_name : -1  , job : 1 } )
- [ ] d. db.people.createIndex({employer:1,last_name : 1  , job : -1 } )

---

## Question 21
### Prompt
Given a collection called collection, in which all documents have the following shape:

```js
{
 _id:1,
 objs:[
   {a:1,b:2},{a:2,b:1}
 ]
}
```

And the query on this collection:

```js
db.collection.find({"objs.a":1})
```

What index will support this query?

### Choose
(Choose 1)

### Options
- [ ] a. {"objs.a":1} Correct Answer
- [ ] b. {objs:1}
- [ ] c. {objs:1,"objs.a"1}
- [ ] d. {objs:1,"a"1}

---

## Question 22
### Prompt
Given the following query:

```js
db.coll.find({}).sort({"product": 1, "price": 1})
```

Which two indexes will improve the performance of this query?

### Choose
(Choose 2)

### Options
- [ ] a. {"product": 1, "price": 1}Correct Answer
- [ ] b. {"product": 1, "price": -1}
- [ ] c. {"product": -1, "price": 1}
- [ ] d. {"product": -1, "price": -1} Correct Answer

---

## Question 23
### Prompt
Given the following query:

```js
db.coll.find({}).sort({"product": 1, "price": 1})
```

Which two indexes improve the performance of this query the most?

### Choose
(Choose 2)

### Options
- [ ] a. { v: 2, key: { price: 1, product: 1 }, name: 'price_1_product_1' }
- [ ] b. { v: 2, key: { product: 1, price: 1 }, name: 'product_1_price_1' } Correct Answer
- [ ] c. { v: 2, key: { price: -1, product: -1 }, name: 'price_-1_product_-1' }
- [ ] d. { v: 2, key: { product: -1, price: -1 }, name: 'product_-1_price_-1' } Correct Answer

---

## Question 24
### Prompt
What mongosh command shows how many indexes are associated with an inventory collection?

### Choose
(Choose 1)

### Options
- [ ] a. db.inventory.getIndexes() Correct Answer
- [ ] b. db.inventory.showIndexes()
- [ ] c. db.inventory.displayIndexes()
- [ ] d. db.inventory.indexes()

---

## Question 25
### Prompt
A typical products collection is in an e-commerce database.

What schema is the most effective?

### Choose
(Choose 1)

### Options
- [ ] a. Orders for products should be embedded as an array in each product document.
- [ ] b. Reviews for products should be embedded as an array in each product document.
- [ ] c. Current and historical prices for product should be embedded in each product document.
- [ ] d. Current inventory/availability for product should be embedded in each product document. Correct Answer

---

## Question 26
### Prompt
A Cooking dataset is in Atlas. There is a Recipes database with a Desserts collection.

How can one document be found that provides a recipe for cookies without chocolate using Atlas Data Explorer?

### Choose
(Choose 1)

### Options
- [ ] a. 1. Select the collection on the left-hand side. 2. Select the "Aggregation" view. 3. Specify the first stage as `$match` query: {dessert_type: "Cookie"} ``` 4. Specify the second stage as `$match` query: ``` {ingredients: {$all: ["chocolate"]}} 5. Set `$limit` to 1.
- [ ] b. 1. Select the collection on the left-hand side. 2. Select the "Aggregation" view. 3. Specify the first stage as `$match` query: ``` {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"]}} ``` 4. Set `$limit` to 1. Correct Answer
- [ ] c. 1. Select the collection on the left-hand side. 2. Specify the "Find" view. 3. Specify the filter query: ``` {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"]} ``` 4. Specify the project query: ``` {dessert_type: 1}
- [ ] d. 1. Select the collection on the left-hand side. 2. Specify the "Find" view. 3. Specify the filter query with limit: ``` {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"], $limit: 1}

---

## Question 27
### Prompt
Given the following documents:

```js
{_id:1, restaurant: "Quesadillas Inc.", rating: 4.5 },
{_id:2, restaurant: "Pasta Inc.", rating: 3.9},
{_id:3, restaurant: "Tacos Inc.", rating: 2.5}
```

A developer wants to find the highest rated restaurant in a list. An index has been created on the appropriate field.

What query satisfies the requirements?

### Choose
(Choose 1)

### Options
- [ ] a. const pipeline = [        { $sort: { rating : -1, limit: 1 } } ]; const aggCursor = coll.runAggregation(pipeline);
- [ ] b. const pipeline = [        { $sort: { rating : -1 } },    { $limit: 1 } ]; const aggCursor = coll.runAggregation(pipeline);
- [ ] c. const pipeline = [    { $sort: { rating : -1 , limit: 1} } ]; const aggCursor = coll.aggregate(pipeline);
- [ ] d. const pipeline = [    { $sort: { rating : -1 } },    { $limit: 1 } ]; const aggCursor = coll.aggregate(pipeline); Correct Answer

---

## Question 28
### Prompt
What are two valid method names for MongoClient class?

### Choose
(Choose 2)

### Options
- [ ] a. db() Correct Answer
- [ ] b. open()
- [ ] c. close() Correct Answer
- [ ] d. destroy()

---

## Question 29
### Prompt
What are two advantages of using Connection Pooling within the Javascript (NodeJS) Driver?

### Choose
(Choose 2)

### Options
- [ ] a. Reduce application latency Correct Answer
- [ ] b. Limit the number of connections to the server Correct Answer
- [ ] c. Remove the need for the application to authenticate
- [ ] d. Remove the need to open and close connections in the application

---

## Question 30
### Prompt
Given the following collection named movies, containing the following six documents:

```js
{ _id: 1, "title": ''A.", "genre": "sci-fi'', "year": "1983" }
{ _id: 2, "title": ''B.", "genre": "comedy'', "year": "1990" }
{ _id: 3, "title": ''C.", "genre": "documentary', "year": "1995" }
{ _id: 4, "title": ''D.", "genre": "sci-fi'', "year": "1985" }
{ _id: 5, "title": ''E.", "genre": "drama'', "year": "2000" }
{ _id: 6, "title": ''F.", "genre": "comedy'', "year": "1977" }
```

What syntax will update multiple documents in the collection?

### Choose
(Choose 1)

### Options
- [ ] a. const filter = { genre: "sci-fi" }; const updateDoc = {    $set: {        genre: "science fiction"        } }; const result = await movies.updateMany(filter, updateDoc); Correct Answer
- [ ] b. const filter = { genre = "sci-fi" }; const updateDoc = {        $set = {        genre = "science fiction",        review = 'While very fictional, I found the science lacking'    } }; const result = await movies.updateMany(filter, updateDoc);
- [ ] c. const filter = { genre = "sci-fi" }; const updateDoc = {    $set: {                genre = "science fiction",        review = 'I found it neither scientific nor fictional'    } }; const result = await movies.update(filter, updateDoc);
- [ ] d. const filter = { genre: "sci-fi" }; const updateDoc = {    $set: {        genre: "science fiction",        review: 'I found it neither scientific nor fictional'    } }; const result = await movies.update(filter, updateDoc);
