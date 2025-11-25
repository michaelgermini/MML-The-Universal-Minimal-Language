# Appendix D — MML DOM JSON Schema

## Formal specification of the MML Document Object Model

The MML DOM defines the tree structure of a parsed MML document. This appendix presents the official JSON Schema for validating MML DOM structures.

### Main MML DOM schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://mml-lang.org/schema/dom/v1.0",
  "title": "MML Document Object Model",
  "description": "JSON schema for validating the DOM structure of an MML document",
  "type": "object",
  "properties": {
    "type": {
      "const": "document",
      "description": "Root node type"
    },
    "title": {
      "type": "string",
      "description": "Main document title",
      "minLength": 1
    },
    "metadata": {
      "type": "object",
      "description": "Global document metadata",
      "patternProperties": {
        "^.*$": {
          "type": "string",
          "description": "Metadata value"
        }
      },
      "additionalProperties": false
    },
    "sections": {
      "type": "array",
      "description": "Document sections",
      "items": {
        "$ref": "#/$defs/section"
      }
    },
    "links": {
      "type": "array",
      "description": "Global document links",
      "items": {
        "$ref": "#/$defs/link"
      }
    }
  },
  "required": ["type", "metadata", "sections"],
  "additionalProperties": false,

  "$defs": {
    "section": {
      "type": "object",
      "properties": {
        "type": {
          "const": "section",
          "description": "Section node type"
        },
        "title": {
          "type": "string",
          "description": "Section title",
          "minLength": 1
        },
        "metadata": {
          "type": "object",
          "description": "Section-specific metadata",
          "patternProperties": {
            "^.*$": {
              "type": "string"
            }
          },
          "additionalProperties": false
        },
        "children": {
          "type": "array",
          "description": "Section child elements",
          "items": {
            "oneOf": [
              {"$ref": "#/$defs/paragraph"},
              {"$ref": "#/$defs/link"},
              {"$ref": "#/$defs/image"},
              {"$ref": "#/$defs/code"},
              {"$ref": "#/$defs/quote"}
            ]
          }
        }
      },
      "required": ["type", "title", "metadata", "children"],
      "additionalProperties": false
    },

    "paragraph": {
      "type": "object",
      "properties": {
        "type": {
          "const": "paragraph"
        },
        "content": {
          "type": "string",
          "description": "Paragraph textual content"
        }
      },
      "required": ["type", "content"],
      "additionalProperties": false
    },

    "link": {
      "type": "object",
      "properties": {
        "type": {
          "const": "link"
        },
        "text": {
          "type": "string",
          "description": "Link display text",
          "minLength": 1
        },
        "url": {
          "type": "string",
          "description": "Target URL or reference",
          "minLength": 1
        }
      },
      "required": ["type", "text", "url"],
      "additionalProperties": false
    },

    "image": {
      "type": "object",
      "properties": {
        "type": {
          "const": "image"
        },
        "description": {
          "type": "string",
          "description": "Textual image description",
          "minLength": 1
        },
        "url": {
          "type": "string",
          "description": "Image path or URL",
          "minLength": 1
        }
      },
      "required": ["type", "description", "url"],
      "additionalProperties": false
    },

    "code": {
      "type": "object",
      "properties": {
        "type": {
          "const": "code"
        },
        "content": {
          "type": "string",
          "description": "Code block content"
        },
        "language": {
          "type": "string",
          "description": "Programming language (optional)",
          "enum": ["javascript", "python", "java", "cpp", "c", "bash", "sql", "json", "xml", "html"]
        }
      },
      "required": ["type", "content"],
      "additionalProperties": false
    },

    "quote": {
      "type": "object",
      "properties": {
        "type": {
          "const": "quote"
        },
        "content": {
          "type": "string",
          "description": "Quote content"
        }
      },
      "required": ["type", "content"],
      "additionalProperties": false
    }
  }
}
```

## Schema extensions

### Configuration node

```json
{
  "$defs": {
    "configuration": {
      "type": "object",
      "properties": {
        "type": {
          "const": "configuration"
        },
        "section": {
          "type": "string",
          "description": "Configuration section name"
        },
        "parameters": {
          "type": "object",
          "description": "Configuration parameters",
          "patternProperties": {
            "^.*$": {
              "type": "string"
            }
          },
          "additionalProperties": false
        }
      },
      "required": ["type", "section", "parameters"],
      "additionalProperties": false
    }
  }
}
```

### DNF packet node

```json
{
  "$defs": {
    "packet": {
      "type": "object",
      "properties": {
        "type": {
          "const": "packet"
        },
        "id": {
          "type": "string",
          "description": "Unique packet identifier",
          "pattern": "^[A-Z0-9_-]+$"
        },
        "children": {
          "type": "array",
          "description": "Packet content",
          "items": {
            "oneOf": [
              {"$ref": "#/$defs/section"},
              {"$ref": "#/$defs/paragraph"},
              {"$ref": "#/$defs/link"},
              {"$ref": "#/$defs/image"},
              {"$ref": "#/$defs/code"},
              {"$ref": "#/$defs/quote"}
            ]
          }
        }
      },
      "required": ["type", "id", "children"],
      "additionalProperties": false
    }
  }
}
```

## Validation and examples

### Valid document example

```json
{
  "type": "document",
  "title": "Emergency report",
  "metadata": {
    "author": "Dr. Smith",
    "date": "2025-11-15"
  },
  "sections": [
    {
      "type": "section",
      "title": "Situation",
      "metadata": {},
      "children": [
        {
          "type": "paragraph",
          "content": "Fire in progress"
        },
        {
          "type": "link",
          "text": "Sector map",
          "url": "fire-map.png"
        }
      ]
    }
  ]
}
```

### Schema usage

#### JavaScript validation
```javascript
const Ajv = require('ajv');
const ajv = new Ajv();

const validate = ajv.compile(mmlDomSchema);
const valid = validate(documentObject);

if (!valid) {
  console.log('Validation errors:', validate.errors);
}
```

#### Python validation
```python
import jsonschema

try:
    jsonschema.validate(document_object, mml_dom_schema)
    print("Valid document")
except jsonschema.ValidationError as e:
    print(f"Validation error: {e.message}")
```

### Compliance tests

#### Minimal valid document
```json
{
  "type": "document",
  "title": "Test",
  "metadata": {},
  "sections": []
}
```

#### Invalid document (missing title)
```json
{
  "type": "document",
  "metadata": {},
  "sections": []
}
```
**Error**: Required property "title" missing

#### Invalid document (incorrect type)
```json
{
  "type": "document",
  "title": "Test",
  "metadata": {},
  "sections": [
    {
      "type": "invalid_type",
      "title": "Section",
      "metadata": {},
      "children": []
    }
  ]
}
```
**Error**: Value "invalid_type" does not match constant "section"

## Schema evolution

### Versioning
- **v1.0**: Initial schema with basic types
- **v1.1**: Addition of configuration and packet nodes
- **v2.0**: Support for multilingual extensions (planned)

### Compatibility
The schema is designed to be:
- **Backward compatible**: Old structures remain valid
- **Extensible**: New types can be added
- **Modular**: Reusable definitions

### Validation tools
- **Online validator**: Web interface to test documents
- **CLI validator**: Command-line tool
- **Library validators**: Integration into parsers

---

**This schema formally defines the expected structure of the MML DOM and enables automatic validation of all parsed documents.**

