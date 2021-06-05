# bach-json-schema
> :musical_score: :triangular_ruler: JSON Schema definition for the `bach.json` format
---

## Language

[`bach`](https://github.com/slurmulon/bach) is a semantic music notation that allows rhytmic timelines to be defined with text.

Compilation of `bach` tracks results in `bach.json` data, a JSON micro-format that makes writing performant `bach` engines incredibly simple (see [gig](https://github.com/slurmulon/gig)).

This repository contains the official [JSON Schema](http://json-schema.org/) definition of the `bach.json` micro-format.

## Media Type

`application/vnd.bach+json`

## Schema

```
{
  "$schema": "http://json-schema.org/draft-07/schema#",
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
        }
      },
      "required": ["meter", "tempo"],
      "additionalProperties": true
    },
    "unit": {
      "type": "object",
      "properties": {
        "step": {
          "type": "number",
          "minimum": 0
        },
        "pulse": {
          "type": "number",
          "minimum": 0
        }
      },
      "required": ["step", "pulse"]
    },
    "units": {
      "type": "object",
      "properties": {
        "beat": {
          "$ref": "#/definitions/unit"
        },
        "bar": {
          "$ref": "#/definitions/unit"
        },
        "time": {
          "allOf": [
            {
              "$ref": "#/definitions/unit"
            },
            {
              "type": "object",
              "properties": {
                "bar": {
                  "type": "number",
                  "minimum": 0
                }
              }
            }
          ]
        }
      },
      "required": ["beat", "bar", "time"],
      "additionalProperties": false
    },
    "metrics": {
      "type": "object",
      "properties": {
        "min": {
          "type": "number",
          "minimum": 0
        },
        "max": {
          "type": "number",
          "minimum": 0
        },
        "total": {
          "type": "number",
          "minimum": 0
        }
      },
      "required": ["min", "max", "total"],
      "additionalProperties": false
    },
    "elements": {
      "type": "object",
      "patternProperties": {
        ".*": {
          "type": "object",
          "patternProperties": {
            ".*": {
              "type": "object",
              "properties": {
                "value": {
                  "type": "string"
                },
                "props": {
                  "type": "array",
                  "default": []
                }
              },
              "required": ["value", "props"],
              "additionalProperties": false
            }
          }
        }
      }
    },
    "steps": {
      "type": "array",
      "items": {
        "type": "array",
        "items": [
          {
            "type": "array",
            "items": [
              { "type": "number" }
            ],
            "additionalItems": {
              "$ref": "#/definitions/id"
            },
            "minItems": 1
          },
          {
            "type": "array",
            "items": {
              "$ref": "#/definitions/id"
            }
          },
          {
            "type": "array",
            "items": {
              "$ref": "#/definitions/id"
            }
          }
        ],
        "minItems": 0,
        "additionalItems": false
      }
    },
    "id": {
      "type": "string",
      "pattern": "[a-zA-Z0-9_-]+\\.[a-zA-Z0-9]{6,}"
    },
    "uid": {
      "type": "string",
      "pattern": "[a-zA-Z0-9]{6,}"
    },
    "item": {
      "type": "object",
      "properties": {
        "duration": {
          "type": "number",
          "minimum": 0
        },
        "elements": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/id"
          }
        }
      },
      "required": ["duration", "elements"]
    },
    "beat": {
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
    }
  },

  "type": "object",
  "properties": {
    "headers": {
      "$ref": "#/definitions/headers"
    },
    "units": {
      "$ref": "#/definitions/units"
    },
    "metrics": {
      "$ref": "#/definitions/metrics"
    },
    "elements": {
      "$ref": "#/definitions/elements"
    },
    "steps": {
      "$ref": "#/definitions/steps"
    },
    "beats": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/beat"
      }
    }
  },
  "required": ["headers", "units", "metrics", "elements", "steps", "beats"]
}
```

## License

MIT
