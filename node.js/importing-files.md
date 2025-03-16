# Imports

When making an import in node it will do the following:

1. Run all sync code in the files imported by the class
2. Set a reference in the memory for each file functions, variables and classes

Because of this import hoisting this behavior can lead to some easy to fix errors

## Example

Imagine you're importing a function that uses process.env for a enviroment
variable but it will only be configured after the import. This will cause to the
variable to return `undefined`.

```JavaScript
import dotenv from "dotenv";
import DB from "./database/config.js"
dotenv.config()
```

We must change the order since the sync code will be run in the import process.

```JavaScript
import dotenv from "dotenv";
dotenv.config()
import DB from "./database/config.js"
```

### Inside DB

```JavaScript
const url = process.env.URL // would return undefined
```
