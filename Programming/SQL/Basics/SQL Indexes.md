An index is an in-memory structure that ensures that queries we run on a database are _performant_, that is to say, they run _quickly_. If you can remember back to the data structures course, most database indexes are just [binary trees](https://en.wikipedia.org/wiki/Binary_tree) or [B-trees](https://en.wikipedia.org/wiki/B-tree)! The binary tree can be stored in [ram](https://en.wikipedia.org/wiki/Random-access_memory) as well as on [disk](https://en.wikipedia.org/wiki/Computer_data_storage), and it makes it easy to lookup the location of an entire row.

`PRIMARY KEY` columns are indexed by default, ensuring you can look up a row by its `id` very quickly. However, if you have other columns that you want to be able to do quick lookups on, you'll need to _index_ them.

CREATE INDEX index_name ON table_name (column_name);

### Multi-Column Indexes
Multi-column indexes are useful for the exact reason you might think - they speed up lookups that depend on _multiple_ columns.

CREATE INDEX first_name_last_name_age_idx
ON users (first_name, last_name, age);

##### Rule of Thumb
Unless you have specific reasons to do something special, only add multi-column indexes if you're doing frequent lookups on a specific combination of columns.

#### Denormalizing for Speed
Storing duplicate information can drastically speed up an application that needs to look it up in _different ways_. For example, if you store a user's country information right on their user record, no expensive join is required to load their profile page!