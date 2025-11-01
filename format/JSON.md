# KA:OA1:JSON1 - JSON Format Specification

## Overview

KA:OA1:JSON1 defines the JSON format standard for Kalo OpenAPI specifications. This format provides an alternative representation to YAML for tools and systems that prefer JSON.

## Version

**Specification**: KA:OA1:JSON1  
**OpenAPI Version**: 3.1.0  
**JSON Version**: RFC 8259  
**Status**: 🚧 In Progress

## Format Features

- **Machine-readable** - Optimal for programmatic processing
- **Standard JSON** - RFC 8259 compliant
- **Deterministic ordering** - Sorted keys for version control
- **Indentation**: 2 spaces for readability
- **Line endings**: LF (Unix-style)
- **Encoding**: UTF-8

## Relationship to YAML Format

KA:OA1:JSON1 is **semantically equivalent** to KA:OA1:YAML1:
- Same structure and conventions
- Same field requirements
- Same validation rules
- Direct 1:1 transformation possible

The only differences are syntactic (JSON vs YAML).

## Document Structure

### Complete Document Example

```json
{
  "openapi": "3.1.0",
  "info": {
    "title": "Generated API",
    "description": "API generated from Morphe schema",
    "version": "1.0.0"
  },
  "servers": [
    {
      "url": "http://localhost:8080",
      "description": "Development server"
    }
  ],
  "paths": {
    "/api/people": {
      "get": {
        "tags": ["Person"],
        "summary": "List people",
        "description": "Retrieve a paginated list of people",
        "operationId": "list_Person",
        "parameters": [
          {
            "$ref": "#/components/parameters/page"
          },
          {
            "$ref": "#/components/parameters/pageSize"
          }
        ],
        "responses": {
          "200": {
            "description": "List of people",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PersonList"
                }
              }
            }
          },
          "400": {
            "$ref": "#/components/responses/Error"
          },
          "500": {
            "$ref": "#/components/responses/Error"
          }
        }
      },
      "post": {
        "tags": ["Person"],
        "summary": "Create person",
        "description": "Create a new person",
        "operationId": "create_Person",
        "requestBody": {
          "description": "Person to create",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/PersonCreate"
              }
            }
          },
          "required": true
        },
        "responses": {
          "201": {
            "description": "Created person",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Person"
                }
              }
            }
          },
          "400": {
            "$ref": "#/components/responses/Error"
          },
          "500": {
            "$ref": "#/components/responses/Error"
          }
        }
      }
    },
    "/api/people/{id}": {
      "get": {
        "tags": ["Person"],
        "summary": "Get person",
        "description": "Retrieve a single person by ID",
        "operationId": "get_Person",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "description": "Person ID",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Person details",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Person"
                }
              }
            }
          },
          "404": {
            "$ref": "#/components/responses/Error"
          },
          "500": {
            "$ref": "#/components/responses/Error"
          }
        }
      },
      "patch": {
        "tags": ["Person"],
        "summary": "Update person",
        "description": "Update an existing person",
        "operationId": "update_Person",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "description": "Person ID",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          }
        ],
        "requestBody": {
          "description": "Person updates",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/PersonUpdate"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Updated person",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Person"
                }
              }
            }
          },
          "400": {
            "$ref": "#/components/responses/Error"
          },
          "404": {
            "$ref": "#/components/responses/Error"
          },
          "500": {
            "$ref": "#/components/responses/Error"
          }
        }
      },
      "delete": {
        "tags": ["Person"],
        "summary": "Delete person",
        "description": "Delete a person",
        "operationId": "delete_Person",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "description": "Person ID",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          }
        ],
        "responses": {
          "204": {
            "description": "Successfully deleted"
          },
          "404": {
            "$ref": "#/components/responses/Error"
          },
          "500": {
            "$ref": "#/components/responses/Error"
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "Person": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "readOnly": true
          },
          "firstName": {
            "type": "string"
          },
          "lastName": {
            "type": "string"
          },
          "nationality": {
            "$ref": "#/components/schemas/Nationality"
          }
        },
        "required": ["firstName", "lastName", "nationality"],
        "description": "Person model"
      },
      "PersonCreate": {
        "type": "object",
        "properties": {
          "firstName": {
            "type": "string"
          },
          "lastName": {
            "type": "string"
          },
          "nationality": {
            "$ref": "#/components/schemas/Nationality"
          }
        },
        "required": ["firstName", "lastName", "nationality"],
        "description": "Create Person request"
      },
      "PersonUpdate": {
        "type": "object",
        "properties": {
          "firstName": {
            "type": "string",
            "nullable": true
          },
          "lastName": {
            "type": "string",
            "nullable": true
          },
          "nationality": {
            "$ref": "#/components/schemas/Nationality"
          }
        },
        "description": "Update Person request"
      },
      "PersonList": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/Person"
            }
          },
          "meta": {
            "type": "object",
            "properties": {
              "page": {
                "type": "integer",
                "description": "Current page number"
              },
              "pageSize": {
                "type": "integer",
                "description": "Items per page"
              },
              "total": {
                "type": "integer",
                "description": "Total number of items"
              },
              "totalPages": {
                "type": "integer",
                "description": "Total number of pages"
              }
            },
            "required": ["page", "pageSize", "total", "totalPages"]
          }
        },
        "required": ["data", "meta"]
      },
      "Nationality": {
        "type": "string",
        "enum": ["German", "French", "American"],
        "description": "Nationality enumeration"
      }
    },
    "responses": {
      "Error": {
        "description": "Error response",
        "content": {
          "application/json": {
            "schema": {
              "type": "object",
              "properties": {
                "error": {
                  "type": "object",
                  "properties": {
                    "code": {
                      "type": "string",
                      "description": "Error code"
                    },
                    "message": {
                      "type": "string",
                      "description": "Error message"
                    }
                  },
                  "required": ["code", "message"]
                }
              },
              "required": ["error"]
            }
          }
        }
      }
    },
    "parameters": {
      "page": {
        "name": "page",
        "in": "query",
        "description": "Page number",
        "schema": {
          "type": "integer",
          "minimum": 1
        }
      },
      "pageSize": {
        "name": "pageSize",
        "in": "query",
        "description": "Number of items per page",
        "schema": {
          "type": "integer",
          "minimum": 1,
          "maximum": 100
        }
      }
    }
  },
  "tags": [
    {
      "name": "Person",
      "description": "Operations on Person"
    }
  ]
}
```

## Formatting Rules

### Indentation

- Use **2 spaces** for each indentation level (JSON convention)
- Never use tabs
- Consistent indentation throughout document

### Line Endings

- Use **LF** (Line Feed, Unix-style) line endings
- Not CRLF (Windows) or CR (old Mac)

### Encoding

- Always use **UTF-8** encoding
- No BOM (Byte Order Mark)

### Key Ordering

Keys are ordered alphabetically within each object for deterministic output:

**Top-level**:
1. `components`
2. `info`
3. `openapi`
4. `paths`
5. `servers`
6. `tags`

**Within schemas**:
1. `description`
2. `properties`
3. `required`
4. `type`
5. Other fields (alphabetical)

**Within properties**:
- Alphabetical by property name
- ID field may appear first for readability

**Within operations**:
1. `description`
2. `operationId`
3. `parameters`
4. `requestBody`
5. `responses`
6. `summary`
7. `tags`

### String Escaping

- Double quotes for all strings
- Escape special characters: `\"`, `\\`, `\n`, `\t`
- Unicode escapes: `\uXXXX`

### Arrays

All arrays use multi-line format for readability:

```json
{
  "tags": [
    "Person"
  ],
  "enum": [
    "German",
    "French",
    "American"
  ]
}
```

### Numbers

- No quotes around numeric values
- Use standard JSON number format
- No leading zeros (except `0.x`)

```json
{
  "minimum": 1,
  "maximum": 100,
  "format": "int64"
}
```

### Booleans

- Use `true` or `false` (lowercase, unquoted)

```json
{
  "required": true,
  "readOnly": true,
  "nullable": false
}
```

### Null Values

- Use `null` keyword (lowercase, unquoted)
- Not common in OpenAPI specs (use `nullable: true` instead)

## Schema Patterns

All schema patterns follow the same structure as YAML format but with JSON syntax.

### Resource Schema Pattern

```json
{
  "ResourceName": {
    "type": "object",
    "properties": {
      "id": {
        "type": "integer",
        "readOnly": true
      },
      "fieldName": {
        "type": "string"
      },
      "relationID": {
        "type": "string",
        "description": "Foreign key to RelatedResource",
        "nullable": true
      },
      "enumField": {
        "$ref": "#/components/schemas/EnumName"
      }
    },
    "required": ["fieldName"],
    "description": "ResourceName model"
  }
}
```

### Create DTO Schema Pattern

```json
{
  "ResourceNameCreate": {
    "type": "object",
    "properties": {
      "fieldName": {
        "type": "string"
      },
      "relationID": {
        "type": "string",
        "description": "Foreign key to RelatedResource",
        "nullable": true
      }
    },
    "required": ["fieldName"],
    "description": "Create ResourceName request"
  }
}
```

### Update DTO Schema Pattern

```json
{
  "ResourceNameUpdate": {
    "type": "object",
    "properties": {
      "fieldName": {
        "type": "string",
        "nullable": true
      },
      "relationID": {
        "type": "string",
        "description": "Foreign key to RelatedResource",
        "nullable": true
      }
    },
    "description": "Update ResourceName request"
  }
}
```

### List Response Schema Pattern

```json
{
  "ResourceNameList": {
    "type": "object",
    "properties": {
      "data": {
        "type": "array",
        "items": {
          "$ref": "#/components/schemas/ResourceName"
        }
      },
      "meta": {
        "type": "object",
        "properties": {
          "page": {
            "type": "integer",
            "description": "Current page number"
          },
          "pageSize": {
            "type": "integer",
            "description": "Items per page"
          },
          "total": {
            "type": "integer",
            "description": "Total number of items"
          },
          "totalPages": {
            "type": "integer",
            "description": "Total number of pages"
          }
        },
        "required": ["page", "pageSize", "total", "totalPages"]
      }
    },
    "required": ["data", "meta"]
  }
}
```

### Enum Schema Pattern

```json
{
  "EnumName": {
    "type": "string",
    "enum": ["value1", "value2", "value3"],
    "description": "EnumName enumeration"
  }
}
```

## Path Patterns

### Collection Path Pattern

```json
{
  "/api/resources": {
    "get": {
      "tags": ["ResourceName"],
      "summary": "List resources",
      "description": "Retrieve a paginated list of resources",
      "operationId": "list_ResourceName",
      "parameters": [
        {"$ref": "#/components/parameters/page"},
        {"$ref": "#/components/parameters/pageSize"}
      ],
      "responses": {
        "200": {
          "description": "List of resources",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ResourceNameList"
              }
            }
          }
        },
        "400": {"$ref": "#/components/responses/Error"},
        "500": {"$ref": "#/components/responses/Error"}
      }
    },
    "post": {
      "tags": ["ResourceName"],
      "summary": "Create resource",
      "description": "Create a new resource",
      "operationId": "create_ResourceName",
      "requestBody": {
        "description": "ResourceName to create",
        "content": {
          "application/json": {
            "schema": {
              "$ref": "#/components/schemas/ResourceNameCreate"
            }
          }
        },
        "required": true
      },
      "responses": {
        "201": {
          "description": "Created resource",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ResourceName"
              }
            }
          }
        },
        "400": {"$ref": "#/components/responses/Error"},
        "500": {"$ref": "#/components/responses/Error"}
      }
    }
  }
}
```

### Item Path Pattern

```json
{
  "/api/resources/{id}": {
    "get": {
      "tags": ["ResourceName"],
      "summary": "Get resource",
      "description": "Retrieve a single resource by ID",
      "operationId": "get_ResourceName",
      "parameters": [
        {
          "name": "id",
          "in": "path",
          "description": "ResourceName ID",
          "required": true,
          "schema": {
            "type": "integer",
            "format": "int64"
          }
        }
      ],
      "responses": {
        "200": {
          "description": "ResourceName details",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ResourceName"
              }
            }
          }
        },
        "404": {"$ref": "#/components/responses/Error"},
        "500": {"$ref": "#/components/responses/Error"}
      }
    },
    "patch": {
      "tags": ["ResourceName"],
      "summary": "Update resource",
      "description": "Update an existing resource",
      "operationId": "update_ResourceName",
      "parameters": [
        {
          "name": "id",
          "in": "path",
          "description": "ResourceName ID",
          "required": true,
          "schema": {
            "type": "integer",
            "format": "int64"
          }
        }
      ],
      "requestBody": {
        "description": "ResourceName updates",
        "content": {
          "application/json": {
            "schema": {
              "$ref": "#/components/schemas/ResourceNameUpdate"
            }
          }
        },
        "required": true
      },
      "responses": {
        "200": {
          "description": "Updated resource",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ResourceName"
              }
            }
          }
        },
        "400": {"$ref": "#/components/responses/Error"},
        "404": {"$ref": "#/components/responses/Error"},
        "500": {"$ref": "#/components/responses/Error"}
      }
    },
    "delete": {
      "tags": ["ResourceName"],
      "summary": "Delete resource",
      "description": "Delete a resource",
      "operationId": "delete_ResourceName",
      "parameters": [
        {
          "name": "id",
          "in": "path",
          "description": "ResourceName ID",
          "required": true,
          "schema": {
            "type": "integer",
            "format": "int64"
          }
        }
      ],
      "responses": {
        "204": {
          "description": "Successfully deleted"
        },
        "404": {"$ref": "#/components/responses/Error"},
        "500": {"$ref": "#/components/responses/Error"}
      }
    }
  }
}
```

## Segmented Output Format

When `segmentedOutput: true` and `outputFormat: "json"`, the same directory structure is used with `.json` extensions:

```
{outputPath}/
  generated/
    entities/
      {Resource}.entity.json
    dtos/
      {Resource}.create.json
      {Resource}.update.json
      {Resource}.list.json
    enums/
      {Enum}.enum.json
    paths/
      {resources}.paths.json
    parameters/
      pagination.parameters.json
    responses/
      error.response.json
  composed/
    root.json
  dist/
    openapi.json
```

### Entity Fragment Format

```json
{
  "kalo-morphe-id-strategy": "autoincrement",
  "kalo-morphe-name": "ResourceName",
  "kalo-morphe-origin": "model",
  "kalo-morphe-resource-type": "model",
  "schema": {
    "type": "object",
    "properties": {
      "id": {
        "type": "integer",
        "readOnly": true
      }
    },
    "required": ["id"],
    "description": "ResourceName model"
  }
}
```

### DTO Fragment Format

```json
{
  "kalo-morphe-name": "ResourceNameCreate",
  "kalo-morphe-origin": "dto",
  "schema": {
    "type": "object",
    "properties": {},
    "required": [],
    "description": "Create ResourceName request"
  }
}
```

### Enum Fragment Format

```json
{
  "kalo-morphe-name": "EnumName",
  "kalo-morphe-origin": "enum",
  "schema": {
    "type": "string",
    "enum": ["value1", "value2"],
    "description": "EnumName enumeration"
  }
}
```

### Path Fragment Format

```json
{
  "kalo-morphe-operation-type": "crud",
  "kalo-morphe-resource": "resources",
  "kalo-morphe-resource-type": "model",
  "paths": {
    "/api/resources": {
      "get": {},
      "post": {}
    },
    "/api/resources/{id}": {
      "get": {},
      "patch": {},
      "delete": {}
    }
  }
}
```

### Parameters Fragment Format

```json
{
  "kalo-morphe-origin": "parameter",
  "kalo-morphe-shared": true,
  "parameters": {
    "page": {
      "name": "page",
      "in": "query",
      "description": "Page number",
      "schema": {
        "type": "integer",
        "minimum": 1
      }
    },
    "pageSize": {
      "name": "pageSize",
      "in": "query",
      "description": "Number of items per page",
      "schema": {
        "type": "integer",
        "minimum": 1,
        "maximum": 100
      }
    }
  }
}
```

### Responses Fragment Format

```json
{
  "kalo-morphe-origin": "response",
  "kalo-morphe-shared": true,
  "responses": {
    "Error": {
      "description": "Error response",
      "content": {
        "application/json": {
          "schema": {
            "type": "object",
            "properties": {
              "error": {
                "type": "object",
                "properties": {
                  "code": {
                    "type": "string",
                    "description": "Error code"
                  },
                  "message": {
                    "type": "string",
                    "description": "Error message"
                  }
                },
                "required": ["code", "message"]
              }
            },
            "required": ["error"]
          }
        }
      }
    }
  }
}
```

## Reference Patterns

References use the same syntax as YAML:

### Internal References

```json
{
  "$ref": "#/components/schemas/Person"
}
```

### External Fragment References

```json
{
  "$ref": "./generated/entities/Person.entity.json#/schema"
}
```

## Type Mappings

Same as YAML format:

| Morphe Type | JSON Schema Type | Format | Additional |
|-------------|------------------|--------|------------|
| AutoIncrement | `"integer"` | `"int64"` | `"readOnly": true` |
| UUID | `"string"` | `"uuid"` | `"readOnly": true` |
| String | `"string"` | - | - |
| Integer | `"integer"` | - | - |
| Float | `"number"` | `"double"` | - |
| Boolean | `"boolean"` | - | - |
| Date | `"string"` | `"date"` | - |
| Time | `"string"` | `"date-time"` | - |
| Protected | `"string"` | - | `"writeOnly": true` |
| Sealed | `"string"` | - | `"writeOnly": true` |

## Validation

Valid KA:OA1:JSON1 documents must:

1. Be valid JSON (RFC 8259)
2. Be valid OpenAPI 3.1.0
3. Follow Kalo naming conventions
4. Include all required Kalo patterns
5. Use consistent indentation (2 spaces)
6. Use LF line endings
7. Be UTF-8 encoded

## Conversion from YAML

To convert KA:OA1:YAML1 to KA:OA1:JSON1:

1. Parse YAML document
2. Convert to JSON preserving structure
3. Apply JSON formatting rules (2-space indent)
4. Sort keys alphabetically
5. Validate result

**Tools**:
- `yq` - YAML/JSON processor
- `jq` - JSON processor
- OpenAPI generators with format conversion

## Tooling

Recommended tools for working with KA:OA1:JSON1:

- **Validation**: OpenAPI CLI, Spectral
- **Editing**: VS Code with OpenAPI extension
- **Documentation**: Swagger UI, Redoc
- **Testing**: Postman, Insomnia
- **Processing**: `jq`, `openapi-generator-cli`

## When to Use JSON vs YAML

**Use JSON when**:
- Integrating with JSON-based tooling
- Programmatic generation/processing
- Strict parsing requirements
- Language doesn't have YAML support

**Use YAML when**:
- Human editing and review
- Version control friendly format
- Comments needed (YAML supports, JSON doesn't)
- Following OpenAPI ecosystem conventions

## License

This specification is licensed under the MIT License.

