# @transportial/gsjson

A fast, concise JSON Getter and Setter language for JavaScript. 

This is the JavaScript port of the GSJson Kotlin library, allowing you to use a powerful path syntax to read, write, filter, and transform JSON data with ease.

## Installation

```bash
npm install @transportial/gsjson
```

## Quick Start

```javascript
import GSJson from '@transportial/gsjson';

const data = {
  store: {
    book: [
      { category: "reference", author: "Nigel Rees", title: "Sayings of the Century", price: 8.95 },
      { category: "fiction", author: "J. R. R. Tolkien", title: "The Lord of the Rings", price: 22.99 }
    ]
  }
};

// Simple dot notation
const firstAuthor = GSJson.get(data, 'store.book.[0].author'); 
// "Nigel Rees"

// Wildcards and Modifiers
const prices = GSJson.get(data, 'store.book.[*].price'); 
// [8.95, 22.99]

// Filtering
const fiction = GSJson.get(data, 'store.book.[category == "fiction"]'); 
// [{ category: "fiction", ... }]
```

## Features and Syntax

GSJson supports an extensive and highly flexible path language:

### Basic Selectors
* **Dot notation:** `name.first`
* **Escaped dots:** `fav\.movie`
* **Array index:** `children.[0]`
* **Array length:** `children.[#]`
* **All child values:** `friends.[#.firstName]`

### Advanced Querying
* **Wildcards:** `*.name` or `name.fir?t`
* **Filtering:** 
  * `friends.[age > "40"].first`
  * `friends.[last == "Murphy"].first`
  * `friends.[first % "D*"].last` *(like/glob)*
  * `friends.[first !% "D*"].[0].first` *(not-like)*
* **Back-references:** `friends.[age == <<.age].[0].first` *(cross-references a parent node)*
* **Nested multi-query:** `friends.[nets.[# == "fb"]].[#.first]`

### Modifiers (Pipes)
Transform data right in your query using the `|` operator:
* **Formatting:** `|@pretty`, `|@ugly`, `|@tostr`, `|@fromstr`
* **Arrays/Objects:** `|@reverse`, `|@sort`, `|@sortBy:age:desc`, `|@flatten`, `|@keys`, `|@values`
* **Aggregators:** `|@count`, `|@sum`, `|@avg`, `|@min`, `|@max`, `|@join:", "`
* **Math:** `|@multiply:2`, `|@add:5`, `|@round:2`, `|@abs`
* **Advanced Array Ops:** `|@flatMap:path`, `|@filter:field == "value"`, `|@groupBy:field`, `|@unique`

### JSON Lines
GSJson natively supports processing JSON Lines (newline-delimited JSON) by starting your path with `..`:
* **Length:** `..#`
* **Indexed access:** `..[1].name`
* **Field extraction:** `..#.name`

## API Reference

### `GSJson.get(data, path, [defaultValue])`
Retrieves the value at the given path. Returns `defaultValue` (or `undefined`) if not found.

### `GSJson.set(data, path, value)`
Sets a value at the given path. If intermediate objects/arrays don't exist, they are created. Returns a new JSON string representing the mutated object.

### `GSJson.exists(data, path)`
Returns a boolean indicating if the path resolves to a non-null/undefined value.

### `GSJson.getResult(data, path)`
Returns a rich `GSJsonResult` object containing helper methods for type conversion (e.g. `.string()`, `.int()`, `.array()`).

### `GSJson.forEachLine(jsonLines, action)`
Iterates over a string of JSON Lines, executing the `action` callback for each parsed line. Return `false` from the callback to stop iteration.

---
**License**: Apache-2.0  
**Author**: Transportial BV
