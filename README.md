# warble-json-schema
> :musical_score: :triangular_ruler: JSON Schema definition for the `warble.json` format
---

## Language

[`warble`](https://github.com/slurmulon/warble) is a notation for representing musical tracks with a focus on readability and productivity.

Compilation of `warble` tracks results in `warble.json` data, a JSON micro-format that makes writing `warble` interpreters/engines incredibly simple.

This repository contains the official [JSON Schema](http://json-schema.org/) definition of the `warble.json` micro-format.

## Media Type

`application/vnd.warble+json`

## Schema

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {
    "headers": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "time": {
          "type": "array",
          "items": {
            "type": "number"
          },
          "minItems": 2,
          "maxItems": 2
        },
        "tempo": {
          "type": "number",
          "minimum": 0
        },
        "total-beats": {
          "type": "number",
          "minimum": 0
        },
        "lowest-beat": {
          "type": "number",
          "minimum": 0.001953125
        },
        "ms-per-beat": {
          "type": "number",
          "minimum": 1
        }
      },
      "required": ["title", "time", "tempo", "total-beats", "lowest-beat", "ms-per-beat"],
      "additionalProperties": true
    },
    "element": {
      "type": "string",
      "pattern": "^(Note|Scale|Chord|Rest|#|~)$"
    },
    "arguments": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "note": {
      "type": "object",
      "properties": {
        "atom": {
          "type": "object",
          "properties": {
            "init": {
              "type": "object",
              "properties": {
                "arguments": {
                  "$ref": "#/definitions/arguments"
                }
              },
              "keyword": {
                "$ref": "#/definitions/element"
              }
            }
          }
        }
      }
    },
    "beat": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "duration": {
              "type": "number"
            },
            "notes": {
              "oneOf": [
                {
                  "type": "array",
                  "items": {
                    "$ref": "#/definitions/note"
                  }
                },
                {
                  "$ref": "#/definitions/note"
                }
              ]
            }
          },
          "required": ["duration", "notes"]
        },
        {
          "type": "null"
        }
      ]
    }
  },

  "type": "object",
  "properties": {
    "headers": {
      "type": "object"
    },
    "data": {
      "type": "array",
      "items": {
        "type": "array",
        "items": {
          "$ref": "#/definitions/beat"
        }
      }
    }
  },
  "required": ["headers", "data"]
}

```

## License

MIT
