# evolutility-models

Models are brief but complete application descriptions (with metadata for both UI and back-end) to run with [Evolutility-UI-React](https://github.com/evoluteur/evolutility-ui-react) and [Evolutility-Server-Node](https://github.com/evoluteur/evolutility-server-node). 


## Sample models

- [To-do list](https://github.com/evoluteur/evolutility-models/blob/master/models/todo.js)
- [Address book](https://github.com/evoluteur/evolutility-models/blob/master/models/contact.js)
- [Restaurants list](https://github.com/evoluteur/evolutility-models/blob/master/models/restaurant.js)
- [Graphic novels](https://github.com/evoluteur/evolutility-models/blob/master/models/comics.js)
- [Wine cellar](https://github.com/evoluteur/evolutility-models/blob/master/models/winecellar.js)

...and a [Test model](https://github.com/evoluteur/evolutility-models/blob/master/models/tests/test.js) with fields of all possible types.



## Vocabulary for Models

Models describe objects with fields and collections. Fields can be grouped. 

### Object

| Property     | Meaning                                 | UI | DB |
|--------------|-----------------------------------------|----|----|
| id           | Unique key to identify the entity (used as API parameter). |X|X|
| icon         | Icon file name for the entity (example: "cube.gif". |X||
| name         | Object name (singular).    | X | |  |
| namePlural   | Object name (plural).      | X | |  |
| title        | Application name.          | X | |  |
| fields       | Array of fields.           | X | X |  |
| groups       | Array of groups. If not provided a single group will be used.   | X | |  |
| titleField   | Field id for the column value used as record title. titleField can also be a function. | X | X |  |
| table        | Database table name.       | | X |  |
| titleField    | Field id for the column value used as record title. | X | X |  |
| searchFields    | Array of field ids for fields used to perform searches.  | |X|  |  



### Field

| Property     | Meaning                               | UI | DB |
|--------------|---------------------------------------|----|----|
| id           | Unique key for the field (can be the same as column but doesn't have to be). |X|X|
| type         | Field type to show in the UI. Possible field types: <ul><li>boolean (yes/no)</li><li>date</li><li>datetime</li><li>decimal</li><li>document</li><li>email</li><li>image</li><li>integer</li><li>lov (list of values)</li><li>money</li><li>text</li><li>textmultiline</li><li>time</li><li>url</li></ul> |X|X|
| required     | Determines if the field is required for saving.      |X|X|
| readonly     | Field value cannot be changed.   |X|X|
| defaultValue | Default field value for new records.                 |X|X|
| max, min     | Maximum/Minimum value allowed (only applies to numeric fields).      |X|X|
| maxLength, minLength | Maximum/Minimum length allowed (only applies to text fields).      |X|X|              
| inMany       | Determines if the field is present (by default) in lists of records. |X|X|           
| height       | For fields of type "textmultiline", number of lines used in the field (in Browse and Edit views). |X||        
| width        | Percentage width in Browse and Edit views. |X||
| help         | Optional help on the field. |X||
| noCharts     | Prevent the field to have a charts (only necessary for fields of type integer, decimal, money, boolean, list of values which are "chartable"). |X|X|
| column       | Database column name for the field    ||X|
| lovtable     | Table to join to for field value (only for fields of "lov" type). ||X|  
| lovcolumn    | Column name (in the lovtable) for field value (only for fields of "lov" type). ||X|
| lovicon    | LOV items have icons. |X|X|
    

### Group

Groups are used to visually group fields on the page for browsing or editing.

Groups are only used in UI models.

| Property     | Meaning                               |
|--------------|---------------------------------------|
| id           | Unique key for the group. It is optional.            |
| type         | panel (tab, accordeon will be added later).
| label        | Group title as displayed to the user.      |
| fields       | Array of field ids.                        |


### Collection

Multiple Master-Details can be specified with collections. 

| Property     | Meaning                               | UI | DB |
|--------------|---------------------------------------|----|----|
| id           | Unique key for the collection.        |X|X|
| title        | Collection title.                     |X||
| table        | DB Table to query.                    ||X|
| column       | Column in the detail table to match against id of object. ||X|
| entity       | Object (model id) linked to.                        |X|X|
| order       | "asc/desc" for sorting by the first field in fields.     ||X|
| fields       | Array of fields.                        |X|X|


## Scripts

This project provides scripts to make UI-models or DB-models from models.

Make UI models (with only UI metadata) for [Evolutility-UI-React](https://github.com/evoluteur/evolutility-ui-react):

```bash
node js/models-ui.js

```


Make back-end models (with only back-end metadata) for [Evolutility-Server-Node](https://github.com/evoluteur/evolutility-server-node):

```bash
node js/models-db.js

``` 

Generated models are saved in the directories "gen-ui" and "gen-db". The list of "full" models to generate from is specified in "/models/all_models.js".
 
 