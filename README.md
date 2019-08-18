# The JSON Data Definition Language Spec

This document contains the work-in-progress IETF Internet Draft for JSON Data
Definition Language ("JDDL"), as well as a test suite to aid in verifying
whether an implementation of JDDL is correct.

## Test Format

This repo contains tests to help implementors verify that they're implementing
JDDL correctly. These tests are located in the `tests` directory.

### Invalid Schema Tests

In `tests/invalid-schemas.json` are examples of data which are valid JSON, but
not valid schemas. The exact rules for what is and isn't a JDDL schema are a bit
complex. This file helps the implementor nail down all the corner cases.

This file is an array of objects containing:

* `name`, a human-readable description of why the "schema" is invalid, and
* `schema`, a JSON value that is not a correct schema

The contents of `tests/invalid-schema.json` can be described with this JDDL
schema:

```json
{
  "elements": {
    "properties": {
      "name": { "type": "string" },
      "schema": {}
    }
  }
}
```

Note well that `schema` is not always an object in these test cases.

### Validation Tests

In `tests/validation` are files containing examples of schemas, and instances to
validate against them. With each instance are the errors that a correct
validator should return when evaluating the instance against the schema.

Each of these files is an array of objects containing:

* `name`, a human-readable name describing the schema
* `schema`, a correct JDDL schema to validate against
* `strictInstance`, whether the validator should use strict instance semantics
* `instances`, an array of objects containing:
  * `instance`, a JSON value that should be validated against the schema
  * `errors`, the standard errors which should be returned from validating the
    instance against the schema

The spec goes into detail on the exact shape and meaning of `errors`. Note that
the spec explicitly notes that the order of the elements of `errors` is not
specified. Therefore it's fine if your implementation produces the same errors,
just in a different order.

The contents of each of the files in `tests/validation` can be described with
this JDDL schema:

```json
{
  "elements": {
    "properties": {
      "name": { "type": "string" },
      "schema": {},
      "strictInstance": { "type": "boolean" },
      "instances": {
        "elements": {
          "properties": {
            "instance": {},
            "errors": {
              "elements": {
                "properties": {
                  "instancePath": { "type": "string" },
                  "schemaPath": { "type": "string" }
                }
              }
            }
          }
        }
      }
    }
  }
}
```
