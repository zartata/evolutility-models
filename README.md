
# Evolutility-Models

Models (with metadata for both UI and back-end) for [Evolutility-UI-React](https://github.com/evoluteur/evolutility-ui-react) and [Evolutility-Server-Node](https://github.com/evoluteur/evolutility-server-node). 


## Sample models

Personal Information Manager (PIM)

- [To-do list](https://github.com/evoluteur/evolutility-models/blob/master/models/pim/todo.js)
- [Address book](https://github.com/evoluteur/evolutility-models/blob/master/models/pim/contact.js)
- [Restaurants list](https://github.com/evoluteur/evolutility-models/blob/master/models/pim/restaurant.js)
- [Graphic novels](https://github.com/evoluteur/evolutility-models/blob/master/models/pim/comics.js)
- [Wine cellar](https://github.com/evoluteur/evolutility-models/blob/master/models/pim/winecellar.js)

Music

- [Artists](https://github.com/evoluteur/evolutility-models/blob/master/models/music/artist.js)
- [Albums](https://github.com/evoluteur/evolutility-models/blob/master/models/music/album.js)
- [Tracks](https://github.com/evoluteur/evolutility-models/blob/master/models/music/track.js)

...and a [Test model](https://github.com/evoluteur/evolutility-models/blob/master/models/tests/test.js) with fields of all possible types.


## Models of models

If we store the models like the data (instead of JSON files), we can use models to create and modify other models.

- [Objects](https://github.com/evoluteur/evolutility-models/blob/master/models/designer/object.js)
- [Fields](https://github.com/evoluteur/evolutility-models/blob/master/models/designer/field.js)
- [Worlds](https://github.com/evoluteur/evolutility-models/blob/master/models/designer/world.js)

TODO: field groups and collections.

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

Generated models are saved in the directories "models-ui" and "models-db". The list of "full" models to generate from is specified in "/models/all_models.js".

Note: The full models can be used as they are by both UI and back-end (which ignore what they do not need in the models).
 
## Metamodel

The metamodel is the structure of the model (the model of models). 
I think about it as the vocabulary for making models.

Models describe objects with fields, groups of fields, and collections (nested lists). 
For any object, all views use the same model (single source of truth for UI metadata).

### Object

| Property     | Meaning                                 | UI | DB |
|--------------|-----------------------------------------|----|----|
| id           | Unique key to identify the entity (used as API parameter). |X|X|
| icon         | Icon file name for the entity (example: "cube.gif"). |X||
| name         | Object name (singular) (example: "contact").   |X| |
| namePlural   | Object name (plural) (example: "contacts").     |X| |
| title        | Application name (example: "Addressbook").         |X| |
| fields       | Array of fields.           |X|X|
| groups       | Array of groups. If not provided a single group will be used.   |X| |
| collections  | Array of collections (displayed as Lists).      |X|X|
| titleField   | Id of the field which value is used as record title. titleField can also be a function. |X|X| 
| table        | Driving database table name (there are secondary tables for fields of type "lov").     | |X| 
| pkey         | Name of the Primary key column (single column of type serial). Default to "id". In the data the key is always called "id". | |X|
| searchFields | Array of field ids for fields used to perform searches (default to fields of text value which are included in the List view.  | |X|
| defaultViewMany| Default view for Many records (possible values: list, cards, charts).  | |X|
| defaultViewOne| Default view for One record (possible values browse, edit).    | |X| 

X: Indicate if the property is used in UI/DB models.
 

### Field

For the backend, fields are columns in a database table. 
For the frontend, fields are textboxes, checkboxes, datepickers... in Edit view, and they are columns in List view.

| Property     | Meaning                               | UI | DB |
|--------------|---------------------------------------|----|----|
| id           | Unique key for the field (can be the same as column but doesn't have to be). |X|X|
| type         | Field type to show in the UI. Possible field types: <ul><li>boolean (yes/no)</li><li>date</li><li>datetime</li><li>decimal</li><li>document</li><li>email</li><li>image</li><li>integer</li><li>lov (list of values)</li><li>money</li><li>text</li><li>textmultiline</li><li>time</li><li>url</li></ul> |X|X|
| label        | Field description (displayed with an asterisk for required fields).      |X||
| labelShort   | Optional shorter version of the labels (used in List and Cards views). |X||
| required     | Determines if the field is required for saving.      |X|X|
| readonly     | Field value cannot be changed.   |X|X|
| defaultValue | Default field value for new records.                 |X|X|
| max, min     | Maximum/Minimum value allowed (only applies to numeric fields).      |X|X|
| maxLength, minLength | Maximum/Minimum length allowed (only applies to text fields).      |X|X|
| inMany       | Determines if the field is present (by default) in lists of records. |X|X|
| height       | For fields of type "textmultiline", number of lines used in the field (in Browse and Edit views). |X||
| width        | Field width in Browse and Edit views (in percent of parent width). Default: 100%  |X||
| help         | Optional help on the field. |X||
| chartType    | Default charts type used for the field ("Bars", "Pie", or "Table"). The default value is "Bars".  |X||
| noCharts     | Prevent the field to have a charts (only necessary for fields of type integer, decimal, money, boolean, list of values which are "chartable"). |X|X|
| column       | Database column name for the field    ||X|
| lovTable     | Table to join to for field value (only for fields of "lov" type). ||X|
| lovColumn    | Column name (in the lovTable) for field value (only for fields of "lov" type). ||X|
| lovIcon      | LOV items have icons (only for fields of "lov" type). |X|X|
| deleteTrigger | Deleting records in the lovTable will trigger a cascade delete (this property is only used for creating the database). | |X|
| object       | Model id for the object to link to (only for fields of "lov" type).       |X|X|

### Group

Groups are used to visually group fields on the page for browsing or editing.

Groups are only used in UI models and are optional. By default a single group holds all fields.

| Property     | Meaning                               | UI | DB |
|--------------|---------------------------------------|----|----|
| id           | Unique key for the group. It is optional.            |X||
| type         | Type of group. Only "panel" is currently implemented ("tab" and "accordeon" will be added later). |X||
| label        | Group title as displayed to the user.      |X||
| fields       | Array of field ids.                        |X||
| width        | Width (in % of the container total width). |X||
| header       | Text to be displayed at the top of the group (just below the group title).|X||
| footer       | Text to be displayed at the bottom of the group.    |X||


### Collection

Multiple Master-Details can be specified with collections. 

| Property     | Meaning                               | UI | DB |
|--------------|---------------------------------------|----|----|
| id           | Unique key for the collection.        |X|X|
| title        | Collection title.                     |X||
| table        | Table to query for the details list.  ||X|
| column       | Column in the detail table to match against id of object. ||X|
| object       | Model id for the object to link to.   |X|X|
| order        | "asc/desc" for sorting by the first field in fields.     ||X|
| fields       | Array of fields. Fields in collections do not need all properties of Fields in objects.   |X|X|


 
## License

Copyright (c) 2019 [Olivier Giulieri](https://evoluteur.github.io/).

Evolutility-Models is released under the [MIT license](http://github.com/evoluteur/evolutility-models/blob/master/LICENSE.md).
