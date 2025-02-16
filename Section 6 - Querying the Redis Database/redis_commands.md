## Redis Commands Cheat Sheet

### RediSearch Commands

#### FT.CREATE: Creating an Index
```sh
FT.CREATE users_idx SCHEMA name TEXT age NUMERIC location TEXT
```
- `users_idx`: The name of the index.
- `SCHEMA`: Specifies that we’re defining the schema of the index.
- `name TEXT`: Creates a text index for the name field (useful for full-text search).
- `age NUMERIC`: Creates a numeric index for the age field (for range queries).
- `location TEXT`: Creates a text index for the location field.

#### FT.ADD: Adding Documents to the Index
```sh
HSET user:1 name "John Doe" age 29 location "New York"
FT.ADD users_idx user:1 1.0
```
- `user:1`: The Redis key that stores the user data.
- `FT.ADD users_idx user:1 1.0`: Adds the `user:1` document to the `users_idx` index with a score of 1.0.

#### FT.SEARCH: Basic Search Query
```sh
FT.SEARCH users_idx "John" LIMIT 0 10
```
- Searches the `users_idx` index for the term "John" and returns the first 10 results.

#### FT.SEARCH: Searching with Filters
```sh
FT.SEARCH users_idx "@age:[25 inf]" LIMIT 0 10
```
- `@age:[25 inf]`: Filters results to users whose age is greater than or equal to 25.
- `LIMIT 0 10`: Limits the result to the first 10 matching documents.

#### FT.SEARCH: Sorting Results
```sh
FT.SEARCH users_idx "John" SORTBY age ASC LIMIT 0 10
```
- `SORTBY age ASC`: Sorts results by the `age` field in ascending order.

#### FT.SEARCH: Combining Filters, Sorting, and Querying
```sh
FT.SEARCH users_idx "@name:John @age:[25 inf]" SORTBY age ASC LIMIT 0 10
```
- Searches for users named "John" who are over 25 years old and sorts them by age in ascending order.

#### FT.DUMP: Exporting the Index Data
```sh
FT.DUMP users_idx
```
- Exports the index data in a format that can be stored or further processed.
