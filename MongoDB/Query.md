# Query documents

Some query operation are pretty easy and intuitive. In this document I will
display some cool ones used to find data better.

## Projection - Fetch only some fields

To use projections we need to give to the interface which fields we would like
to display.

```JavaScript
db.find({name: "Igor"}, {age: true, isSingle: true})
```

## Query for field combinations

This query will search for documents with name == Igor and age == 18

```JavaScript
db.find({$and: {name: "Igor", age: 18}})
```

We can apply the same idea for the or operator

```JavaScript
db.find({$or: {name: "Igor", age: 18}})
```

## Some operations

- `$gte` -> greater than
- `$match` -> off course match something
- `$sum` -> sum
- `$group` -> group operator
- `$sort` -> sorts based on a filed, asc or desc
