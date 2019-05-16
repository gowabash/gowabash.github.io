## Problem - I have assets that I want to model in PostgresSql.  
---
## Details 
* There are multiple kinds of assets each of which is a little different
  * Some assets have 1 important field headline, body, ... just have a text field
  * Some assets have multiple fields url has url, display_url, ...
* There couple be asset types I don't know about or that will show up later

## Possible solutions
* **Sparse Table**
  * Single table where all of the possible fields are enumerate in a single table
  * Fields - asset_id, asset_type, body, headline, call_to_action, url, displaty_url, ...
  * Pros
    * You know exactly what any data element is by the column header
  * Cons
    * Lots of nulls
    * You have to know what fields belong to what asset types
    * Any new asset type requires ddl change
* **Multiple Tables**
  * multiple tables where each asset type gets it's own table
  * Fields - each table has the asset_id and the fields appropriate to the type
  * Pros
    * You know exactly what data goes with any asset type
  * Cons
    * Lots of tables
    * Every new asset type requires new table / ddl change
    * Requires either lots of models or some fancy meta-programming
* **Maleable Table**
  * Single table with generic headings
  * Fields - asset_id, asset_type, data1, data2, data3, data4, data5
  * Pros
    * Super flexible
    * Most of the time what you care about is in data1
  * Cons
    * Not desriptive at all
    * You have to know what fields belong to what asset types
    * Requires a well mapped object in ruby
    * Feels icky
    * If something requires a 6th data element, ddl change required
* **jsonb field Table**
  * Single table where the asset level data is stored in a jsonb field
  * Fields - asset_id, asset_type, data
  * Pros
    * Super flexible
    * You can see all the data
    * Will never need a ddl change
  * Cons
    * Not relational
    * Specific data requires extra knowledge (see all the asset with data, but headline requires you to know `data->>'text' to get the headline text
    * You have to know what fields belong to what asset types
    * Requires a well mapped object in ruby

