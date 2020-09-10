SortOrder: 0

# Contact Extended Fields Service

## Introduction

Fields are the way to store data about your contacts. A contact is consists of system fields (such as Name, Email and Phone) and custom fields that extend the built-in system fields.
This service provides full CRUD functionality for custom fields. Custom fields have a type (text/number/URL/date) and a name and are limited to 500 per site.

### The Extended Field Entity

An extended field can be used to add extended information on a contact. 
There is a set of built-in (system/predefined) fields, e.g. `TODO example1` or `TODO example2`, and there an option to create a custom (user-defined) field that will apply to each contact.

A custom field can be created by the owner in the contact dashboard (or via this API)

The extended field has the following properties:
- `key` - a read-only property that serves as a unique identifier of each extended field.
  - The key is a friendly id, auto-generated based on the first displayName of the field
  - User defined fields will be prefixed with "custom."
- `displayName` - field name - as user sees it. E.g. `First Name`, `E-mail`, etc. Read-only for SYSTEM fields.
- `dataType` - type of data that the fill can contain. Read-only for system fields. Can be one of the following:
    - `TEXT` - stands for textual data
    - `NUMBER` - stands for numeric data
    - `DATE` - stands to TimeStamp data - date only, does not support time resolution
    - `URL` - represents a Web Address
- `fieldType` - a read-only property that defines fields type, can be one of the following values:
  - `SYSTEM` - System predefined
  - `USER_DEFINED` - Defined by the user, i.e. custom fields.
- `namespace` - Defines the namespace for the fields's `key`. (Always `custom` for USER_DEFINED fields).
- `createdDate` - When the field was created.
- `updatedDate` - When the field was last updated.    
    
   

### Example

```json
   {
     "key": "custom.favorite-color",
     "displayName": "Favorite Color",
     "namespace" : "custom",
     "dataType": "TEXT",
     "fieldType" : "USER_DEFINED",
     "createdDate": "2019-10-10T12:30:56Z",
     "updatedDate": "2020-05-10T07:14:59Z"
   }
```

### Sample Usage
In a *Contact* object:

```json
{
  "id": "8046df3c-7575-4098-a5ab-c91ad8f33c47",
  ...
  "info": {
    ...
    "extendedFields": {
      "custom.favorite-color": "Blue"
    }
  }
}
```