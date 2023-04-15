# Custom JSON Op Standard (CJOS)

This is a proposed standard for designing `custom_json` operations that dApps can use. It is designed to allow for easy protocol upgrades through an internal operation versioning system.

---

## IDs

Applications or protocols use their own IDs to broadcast their `custom_json` ops. For example:

- `community`: global community protocol used by Hivemind
- `polls`: global protocol for polls on Hive
- `splinterlands`: Splinterlands game operations

---

## Op JSON Data Structure

```
[
    [1, "app_name/version"],        array
    "op_internal_name",             string
    {} or [] or "" or               dictionary|array|string|integer
]
```

Operations are broadcast using an array as the main container in the `json` field.

### 1st element: Header

The first element contains the `header` which can contain application-specific metadata such as:

- `version` [int]: the version of the op, within the application/protocol
- `application version` [string]: the name of the app, version info, etc

Versioning is important because it allows for easy protocol upgrades. For example, if a new version of the protocol is released, the version number can be incremented and both the new and old versions can be handled by the application/protocol, through replays. With this, code can be written in a way that is backwards compatible with older versions, by handling the individual versions with different code paths.

### 2nd element: Internal Operation Name

The second element, `op_internal_name`, contains the internal name of the operation. This is used to differentiate between different operations within the same application/protocol.

Imbedding all the applications operation types in this element allows for easy filtering of operations by application/protocol, keeping the `id` field the same for all operations.

### 3rd element: Data

The third element contains the actual data/payload that the application/protocol uses, in one of these formats:
    - a JSON object (dictionary), or
    - an array/list
    - string
    - integer

---

## Example: Polls App

```
[
    [1, "polls/1"],
    "create_poll",
    {
        "title": "What is your favorite color?",
        "options": ["red", "green", "blue"]
    }
]
```