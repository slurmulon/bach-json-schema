# bach-json-schema
> :musical_score: :triangular_ruler: JSON Schema definition for the `bach.json` format
---

## Language

[`bach`](https://github.com/slurmulon/bach) is a notation for representing musical tracks with a focus on readability and productivity.

Compilation of `bach` tracks results in `bach.json` data, a JSON micro-format that makes writing `bach` interpreters/engines incredibly simple.

This repository contains the official [JSON Schema](http://json-schema.org/) definition of the `bach.json` micro-format.

## Media Type

`application/vnd.bach+json`

## Schema

```
{
  "$schema": "http://json-schema.org/draft-06/schema#",
  "definitions": {
    "headers": {
      "type": "object",
      "properties": {
        "meter": {
          "type": "array",
          "items": {
            "type": "number",
            "minimum": 1
          },
          "minItems": 2,
          "maxItems": 2
        },
        "tempo": {
          "type": "number",
          "minimum": 0
        },
        "beat-unit": {
          "type": "number",
          "minimum": 0.001953125
        },
        "pulse-beat": {
          "type": "number",
          "minimum": 0.001953125
        },
        "beat-units-per-measure": {
          "type": "number",
          "minimum": 0
        },
        "pulse-beats-per-measure": {
          "type": "number",
          "minimum": 0
        },
        "total-beats": {
          "type": "number",
          "minimum": 0
        },
        "total-beat-units": {
          "type": "number",
          "minimum": 0
        },
        "total-pulse-beats": {
          "type": "number",
          "minimum": 0
        },
        "ms-per-pulse-beat": {
          "type": "number",
          "minimum": 1
        },
        "ms-per-beat-unit": {
          "type": "number",
          "minimum": 1
        }
      },
      "required": ["meter", "tempo", "beat-unit", "pulse-beat", "beat-units-per-measure", "pulse-beats-per-measure", "total-beats", "total-beat-units", "total-pulse-beats", "ms-per-pulse-beat", "ms-per-beat-unit"],
      "additionalProperties": true
    },
    "element": {
      "type": "string",
      "pattern": "^(Note|Scale|Chord|Rest|Mode|Triad|#|~)$"
    },
    "arguments": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "item": {
      "type": "object",
      "properties": {
        "keyword": {
          "$ref": "#/definitions/element"
        },
        "arguments": {
          "$ref": "#/definitions/arguments"
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
            "items": {
              "type": "array",
              "items": {
                "$ref": "#/definitions/item"
              }
            }
          },
          "required": ["duration", "items"]
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
