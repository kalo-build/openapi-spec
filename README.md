# Kalo OpenAPI Specification (KA:OA1) - Morphe-Generated REST API Standard

## Table of Contents

- [Overview](#overview)
- [Specification Versions](#specification-versions)
- [Specification Hierarchy](#specification-hierarchy)
- [Key Features](#key-features)
- [Core Concepts](#core-concepts)
  - [Resource-Oriented Design](#resource-oriented-design)
  - [CRUD Operations](#crud-operations)
  - [Schema Patterns](#schema-patterns)
  - [Kalo-Morphe Annotations](#kalo-morphe-annotations)
- [Document Structure](#document-structure)
  - [Metadata](#metadata)
  - [Paths](#paths)
  - [Components](#components)
  - [Tags](#tags)
- [Schema Conventions](#schema-conventions)
  - [Resource Schemas](#resource-schemas)
  - [DTO Schemas](#dto-schemas)
  - [Enum Schemas](#enum-schemas)
  - [List Response Schemas](#list-response-schemas)
- [Path Conventions](#path-conventions)
  - [Collection Endpoints](#collection-endpoints)
  - [Item Endpoints](#item-endpoints)
  - [Naming Conventions](#naming-conventions)
- [Response Patterns](#response-patterns)
  - [Success Responses](#success-responses)
  - [Error Responses](#error-responses)
  - [Pagination Metadata](#pagination-metadata)
- [Output Modes](#output-modes)
  - [Bundled Mode](#bundled-mode)
  - [Segmented Mode](#segmented-mode)
  - [Composed Mode](#composed-mode)
- [Configuration Options](#configuration-options)
- [Kalo-Morphe Extensions](#kalo-morphe-extensions)
- [Best Practices](#best-practices)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)

## Overview

KA:OA1 (Kalo OpenAPI 1) is a specialized OpenAPI 3.1 specification standard for REST APIs generated from Morphe schemas. It defines conventions, patterns, and extensions that enable seamless integration between Morphe's declarative data modeling and OpenAPI's REST API specification.

This specification builds on OpenAPI 3.1.0 while establishing Kalo-specific conventions for:
- **Resource naming** and URL structure
- **CRUD operation** patterns
- **Schema generation** from Morphe models and entities
- **Metadata annotations** for traceability
- **Modular composition** for team collaboration

## Specification Versions

Version | Status | Description | Docs
--------|---------|------------|------
KA:OA1 | 🚧 In Progress | Core OpenAPI standard for Morphe-generated APIs | This document
KA:OA1:YAML1 | 🚧 In Progress | YAML format standard for KA:OA1 | [format/YAML.md](format/YAML.md)
KA:OA1:JSON1 | 🚧 In Progress | JSON format standard for KA:OA1 | [format/JSON.md](format/JSON.md)

## Specification Hierarchy

The Kalo OpenAPI specification system follows the Kalo specification hierarchy:

### Base Specification (`KA:OA1`)
- `KA:` - Kalo organization prefix
- `OA1` - OpenAPI specification version 1
- *Defines:* Core conventions, patterns, and extensions for Morphe-generated OpenAPI documents

### Format Specifications
- `KA:OA1:YAML1` - YAML format specification (base format)
- `KA:OA1:JSON1` - JSON format specification

### Base Format
**YAML** (`KA:OA1:YAML1`) serves as the base format for the Kalo OpenAPI specification, providing:
- **Human Readability**: Easy to read and edit API specifications
- **OpenAPI Ecosystem**: Standard format for OpenAPI tooling
- **Morphe Alignment**: Consistency with Morphe's YAML base format
- **Version Control**: Diff-friendly format for collaboration

## Key Features

1. 📋 **Convention-Based CRUD**
   * Automatic REST endpoint generation
   * Consistent operation patterns (list, create, get, update, delete)
   * Standard HTTP method mappings
   * Predictable URL structures

2. 🔄 **Morphe Integration**
   * Direct mapping from Morphe models and entities
   * Type-safe schema generation
   * Enum and structure support
   * Relationship handling

3. 🏷️ **Traceability Annotations**
   * `kalo-morphe-*` metadata fields
   * Source tracking (model, entity, DTO)
   * ID strategy identification
   * Resource type classification

4. 📦 **Modular Architecture**
   * Segmented output for team collaboration
   * Reference-based composition
   * Clean bundled distribution
   * Selective schema inclusion

5. 🎯 **Production Ready**
   * OpenAPI 3.1.0 compliant
   * JSON Schema validation
   * Standard error responses
   * Pagination support

## Core Concepts

### Resource-Oriented Design

The Kalo OpenAPI specification follows REST principles with resources as first-class concepts:

**Resource**: A Morphe model or entity exposed via REST endpoints
- **Collection**: Multiple resource instances (e.g., `/api/people`)
- **Item**: Single resource instance (e.g., `/api/people/{id}`)

**Resource Sources**:
- `entities` - Generate endpoints from Morphe entities (domain-oriented)
- `models` - Generate endpoints from Morphe models (data-oriented)
- `both` - Generate from both entities and models

### CRUD Operations

Each resource supports five standard operations:

| Operation | HTTP Method | Path | Description |
|-----------|-------------|------|-------------|
| **List** | `GET` | `/api/{resources}` | Retrieve paginated collection |
| **Create** | `POST` | `/api/{resources}` | Create new resource |
| **Get** | `GET` | `/api/{resources}/{id}` | Retrieve single resource |
| **Update** | `PATCH` | `/api/{resources}/{id}` | Partially update resource |
| **Delete** | `DELETE` | `/api/{resources}/{id}` | Delete resource |

### Schema Patterns

Each resource generates four schema types:

1. **{Resource}** - Complete resource representation (read operations)
2. **{Resource}Create** - Create request payload (POST)
3. **{Resource}Update** - Update request payload (PATCH)
4. **{Resource}List** - Paginated list response (GET collection)

### Kalo-Morphe Annotations

Metadata annotations provide traceability from OpenAPI back to Morphe sources:

```yaml
kalo-morphe-name: Person
kalo-morphe-origin: model
kalo-morphe-resource-type: model
kalo-morphe-id-strategy: autoincrement
```

See [Kalo-Morphe Extensions](#kalo-morphe-extensions) for complete annotation reference.

## Document Structure

### Metadata

Every Kalo OpenAPI document includes:

```yaml
openapi: 3.1.0
info:
  title: Generated API
  description: API generated from Morphe schema
  version: 1.0.0
servers:
  - url: http://localhost:8080
    description: Development server
```

### Paths

Paths follow resource-oriented conventions:

```yaml
paths:
  /api/{resources}:
    get:      # List operation
    post:     # Create operation
  /api/{resources}/{id}:
    get:      # Get operation
    patch:    # Update operation
    delete:   # Delete operation
```

### Components

Components organize reusable API elements:

```yaml
components:
  schemas:       # Resource, DTO, and enum schemas
  responses:     # Shared response definitions (Error)
  parameters:    # Shared parameters (pagination)
```

### Tags

Tags group operations by resource:

```yaml
tags:
  - name: Person
    description: Operations on Person
  - name: Company
    description: Operations on Company
```

## Schema Conventions

### Resource Schemas

Resource schemas represent the complete data model:

```yaml
Person:
  type: object
  properties:
    id:
      type: integer
      readOnly: true
    firstName:
      type: string
    lastName:
      type: string
    nationality:
      $ref: '#/components/schemas/Nationality'
  required:
    - firstName
    - lastName
    - nationality
  description: Person model
```

**Conventions**:
- ID fields marked `readOnly: true`
- Auto-generated fields excluded from create DTOs
- Foreign keys include descriptive comments
- Required fields explicitly listed

### DTO Schemas

DTOs (Data Transfer Objects) define request payloads:

**Create DTO** - Excludes auto-generated fields:
```yaml
PersonCreate:
  type: object
  properties:
    firstName:
      type: string
    lastName:
      type: string
    nationality:
      $ref: '#/components/schemas/Nationality'
  required:
    - firstName
    - lastName
    - nationality
  description: Create Person request
```

**Update DTO** - All fields optional, supports partial updates:
```yaml
PersonUpdate:
  type: object
  properties:
    firstName:
      type: string
      nullable: true
    lastName:
      type: string
      nullable: true
    nationality:
      $ref: '#/components/schemas/Nationality'
  description: Update Person request
```

### Enum Schemas

Enums use JSON Schema enum validation:

```yaml
Nationality:
  type: string
  enum:
    - German
    - French
    - American
  description: Nationality enumeration
```

### List Response Schemas

List responses include data array and pagination metadata:

```yaml
PersonList:
  type: object
  properties:
    data:
      type: array
      items:
        $ref: '#/components/schemas/Person'
    meta:
      type: object
      properties:
        page:
          type: integer
          description: Current page number
        pageSize:
          type: integer
          description: Items per page
        total:
          type: integer
          description: Total number of items
        totalPages:
          type: integer
          description: Total number of pages
      required:
        - page
        - pageSize
        - total
        - totalPages
  required:
    - data
    - meta
```

## Path Conventions

### Collection Endpoints

**List Operation** - `GET /api/{resources}`:
```yaml
/api/people:
  get:
    tags:
      - Person
    summary: List people
    description: Retrieve a paginated list of people
    operationId: list_Person
    parameters:
      - $ref: '#/components/parameters/page'
      - $ref: '#/components/parameters/pageSize'
    responses:
      "200":
        description: List of people
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/PersonList'
      "400":
        $ref: '#/components/responses/Error'
      "500":
        $ref: '#/components/responses/Error'
```

**Create Operation** - `POST /api/{resources}`:
```yaml
post:
  tags:
    - Person
  summary: Create person
  description: Create a new person
  operationId: create_Person
  requestBody:
    description: Person to create
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/PersonCreate'
    required: true
  responses:
    "201":
      description: Created person
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Person'
    "400":
      $ref: '#/components/responses/Error'
    "500":
      $ref: '#/components/responses/Error'
```

### Item Endpoints

**Get Operation** - `GET /api/{resources}/{id}`:
```yaml
/api/people/{id}:
  get:
    tags:
      - Person
    summary: Get person
    description: Retrieve a single person by ID
    operationId: get_Person
    parameters:
      - name: id
        in: path
        description: Person ID
        required: true
        schema:
          type: integer
          format: int64
    responses:
      "200":
        description: Person details
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Person'
      "404":
        $ref: '#/components/responses/Error'
      "500":
        $ref: '#/components/responses/Error'
```

**Update Operation** - `PATCH /api/{resources}/{id}`:
```yaml
patch:
  tags:
    - Person
  summary: Update person
  description: Update an existing person
  operationId: update_Person
  parameters:
    - name: id
      in: path
      description: Person ID
      required: true
      schema:
        type: integer
        format: int64
  requestBody:
    description: Person updates
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/PersonUpdate'
    required: true
  responses:
    "200":
      description: Updated person
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Person'
    "400":
      $ref: '#/components/responses/Error'
    "404":
      $ref: '#/components/responses/Error'
    "500":
      $ref: '#/components/responses/Error'
```

**Delete Operation** - `DELETE /api/{resources}/{id}`:
```yaml
delete:
  tags:
    - Person
  summary: Delete person
  description: Delete a person
  operationId: delete_Person
  parameters:
    - name: id
      in: path
      description: Person ID
      required: true
      schema:
        type: integer
        format: int64
  responses:
    "204":
      description: Successfully deleted
    "404":
      $ref: '#/components/responses/Error'
    "500":
      $ref: '#/components/responses/Error'
```

### Naming Conventions

Resources follow configurable naming conventions:

| Convention | Example | Collection | Item |
|------------|---------|-----------|------|
| **kebab-case** (default) | ContactInfo | `/api/contact-infos` | `/api/contact-infos/{id}` |
| **camelCase** | ContactInfo | `/api/contactInfos` | `/api/contactInfos/{id}` |
| **snake_case** | ContactInfo | `/api/contact_infos` | `/api/contact_infos/{id}` |

**Pluralization**:
- Enabled by default (`pluralize: true`)
- Collections use plural form (person → people)
- Irregular plurals handled (company → companies)

**Operation IDs**:
- Format: `{operation}_{ResourceName}`
- Examples: `list_Person`, `create_Company`, `update_ContactInfo`

## Response Patterns

### Success Responses

| Operation | Status Code | Response Body |
|-----------|-------------|---------------|
| List | `200 OK` | `{Resource}List` with data and meta |
| Create | `201 Created` | Created `{Resource}` object |
| Get | `200 OK` | `{Resource}` object |
| Update | `200 OK` | Updated `{Resource}` object |
| Delete | `204 No Content` | Empty body |

### Error Responses

Standard error response schema:

```yaml
Error:
  description: Error response
  content:
    application/json:
      schema:
        type: object
        properties:
          error:
            type: object
            properties:
              code:
                type: string
                description: Error code
              message:
                type: string
                description: Error message
            required:
              - code
              - message
        required:
          - error
```

**Error Status Codes**:
- `400 Bad Request` - Invalid request payload or parameters
- `404 Not Found` - Resource not found
- `500 Internal Server Error` - Server error

### Pagination Metadata

Shared pagination parameters:

```yaml
parameters:
  page:
    name: page
    in: query
    description: Page number
    schema:
      type: integer
      minimum: 1
  pageSize:
    name: pageSize
    in: query
    description: Number of items per page
    schema:
      type: integer
      minimum: 1
      maximum: 100
```

## Output Modes

### Bundled Mode

**Default mode** - Single OpenAPI file:

```
openapi.yaml (660 lines)
```

**Characteristics**:
- ✅ Single file for easy distribution
- ✅ Standard OpenAPI tooling compatible
- ✅ No annotations (clean spec)
- ❌ Hard to collaborate on
- ❌ Large diffs on changes

**Use Cases**:
- API documentation sites
- Client SDK generation
- Third-party integration
- Simple projects

### Segmented Mode

**Team-friendly mode** - Modular fragments:

```
openapi/
  generated/
    entities/
      Person.entity.yaml
      Company.entity.yaml
    dtos/
      Person.create.yaml
      Person.update.yaml
      Person.list.yaml
      Company.create.yaml
      ...
    enums/
      Nationality.enum.yaml
    paths/
      people.paths.yaml
      companies.paths.yaml
    parameters/
      pagination.parameters.yaml
    responses/
      error.response.yaml
  composed/
    root.yaml
  dist/
    openapi.yaml
```

**Characteristics**:
- ✅ Small, focused files
- ✅ Easy to review changes
- ✅ Team collaboration friendly
- ✅ Includes kalo-morphe annotations
- ✅ Clean bundled output in dist/
- ❌ More files to manage

**Use Cases**:
- Large teams
- Iterative API design
- Morphe schema evolution
- Custom fragment editing

### Composed Mode

**Reference-based composition** - Uses `$ref` for modularity:

```yaml
# composed/root.yaml
components:
  schemas:
    Person:
      $ref: ./generated/entities/Person.entity.yaml#/schema
    PersonCreate:
      $ref: ./generated/dtos/Person.create.yaml#/schema
    PersonList:
      $ref: ./generated/dtos/Person.list.yaml#/schema
paths:
  /api/people:
    $ref: ./generated/paths/people.paths.yaml
```

**Characteristics**:
- ✅ DRY (Don't Repeat Yourself)
- ✅ Selective imports
- ✅ Custom composition
- ❌ Requires bundling for standard tools

## Configuration Options

The Kalo OpenAPI plugin supports extensive configuration:

```yaml
# Resource Configuration
resourceSource: "entities"      # entities | models | both
modelsPathsMode: "none"         # none | namespaced | replace_entities
modelsPathsNamespace: "/_models"

# Naming
naming: "kebab"                 # kebab | camel | snake
collections:
  pluralize: true

# Paths
basePath: "/api"
idParam: "id"
responseEnvelope: false         # Wrap responses in {data, meta}

# Schemas
includeAllSchemas: false        # Include unreferenced enums/structures

# Output
outputFormat: "yaml"            # yaml | json
segmentedOutput: false          # Enable modular output
emitAnnotations: true           # Include kalo-morphe-* metadata

# Pagination
pagination:
  type: "page"                  # page | cursor
  maxPageSize: 100
  defaultPageSize: 20

# Relations
relations:
  expand: false                 # Expand relations in responses

# Authentication
auth:
  scheme: "none"                # none | bearer | oauth2

# Servers
servers:
  - url: "http://localhost:8080"
    description: "Development server"
```

See [Configuration Reference](#configuration-reference) for detailed option descriptions.

## Kalo-Morphe Extensions

Kalo OpenAPI documents include custom annotations for traceability:

### Document-Level Annotations

```yaml
kalo-morphe-composed: true
kalo-morphe-version: 1.0.0
```

### Schema Annotations

**Resource Schemas**:
```yaml
kalo-morphe-name: Person
kalo-morphe-origin: model
kalo-morphe-resource-type: model
kalo-morphe-id-strategy: autoincrement
```

**DTO Schemas**:
```yaml
kalo-morphe-name: PersonCreate
kalo-morphe-origin: dto
```

**Enum Schemas**:
```yaml
kalo-morphe-name: Nationality
kalo-morphe-origin: enum
```

### Path Annotations

```yaml
kalo-morphe-operation-type: crud
kalo-morphe-resource: people
kalo-morphe-resource-type: model
```

### Parameter Annotations

```yaml
kalo-morphe-origin: parameter
kalo-morphe-shared: true
```

### Response Annotations

```yaml
kalo-morphe-origin: response
kalo-morphe-shared: true
```

### Annotation Reference

| Annotation | Scope | Values | Description |
|------------|-------|--------|-------------|
| `kalo-morphe-composed` | Document | `true`, `false` | Indicates $ref-based composition |
| `kalo-morphe-version` | Document | Version string | Plugin version |
| `kalo-morphe-name` | Schema | Resource name | Original Morphe model/entity name |
| `kalo-morphe-origin` | Schema, Parameter, Response | `model`, `entity`, `dto`, `enum`, `parameter`, `response` | Source type |
| `kalo-morphe-resource-type` | Schema, Path | `model`, `entity` | Resource classification |
| `kalo-morphe-id-strategy` | Schema | `autoincrement`, `uuid` | ID field type |
| `kalo-morphe-operation-type` | Path | `crud` | Operation category |
| `kalo-morphe-resource` | Path | Resource name (plural) | Target resource |
| `kalo-morphe-shared` | Parameter, Response | `true` | Indicates shared/reusable component |

## Best Practices

### 1. Use Entities for Public APIs

```yaml
resourceSource: "entities"
```

**Rationale**: Entities represent business domain, hiding implementation details.

### 2. Enable Schema Filtering

```yaml
includeAllSchemas: false
```

**Rationale**: Only include referenced schemas for cleaner specifications.

### 3. Choose Output Mode by Team Size

**Small Teams/Solo**: Bundled mode
```yaml
segmentedOutput: false
```

**Large Teams**: Segmented mode
```yaml
segmentedOutput: true
```

### 4. Use Kebab-Case for URLs

```yaml
naming: "kebab"
```

**Rationale**: Standard REST convention, URL-safe without encoding.

### 5. Leverage Composed Root for Custom APIs

Edit `composed/root.yaml` to:
- Add custom paths
- Include selective schemas
- Override generated operations

Then regenerate `dist/openapi.yaml` without losing customizations.

### 6. Version Your API

```yaml
info:
  version: 1.0.0
```

Update version on breaking changes.

### 7. Document with Tags

Use descriptive tag descriptions:
```yaml
tags:
  - name: Person
    description: Manage people and their contact information
```

## Examples

### Minimal Example

**Input** - Morphe model:
```yaml
# person.mod
name: Person
fields:
  ID:
    type: AutoIncrement
  FirstName:
    type: String
  LastName:
    type: String
identifiers:
  primary: ID
```

**Output** - OpenAPI (bundled):
```yaml
openapi: 3.1.0
info:
  title: Generated API
  version: 1.0.0
paths:
  /api/people:
    get:
      operationId: list_Person
      # ... list operation
    post:
      operationId: create_Person
      # ... create operation
  /api/people/{id}:
    get:
      operationId: get_Person
      # ... get operation
    patch:
      operationId: update_Person
      # ... update operation
    delete:
      operationId: delete_Person
      # ... delete operation
components:
  schemas:
    Person:
      type: object
      properties:
        id:
          type: integer
          readOnly: true
        firstName:
          type: string
        lastName:
          type: string
      required:
        - firstName
        - lastName
```

### Complete Example

See [format/YAML.md](format/YAML.md) for comprehensive examples including:
- Enums
- Relationships
- Multiple resources
- Pagination
- Error handling

## Configuration Reference

### resourceSource

**Type**: `string`  
**Default**: `"entities"`  
**Options**: `"entities"`, `"models"`, `"both"`

Controls which Morphe definitions generate CRUD endpoints.

### modelsPathsMode

**Type**: `string`  
**Default**: `"none"`  
**Options**: `"none"`, `"namespaced"`, `"replace_entities"`

Controls how model paths are exposed when `resourceSource` is `"both"`.

### modelsPathsNamespace

**Type**: `string`  
**Default**: `"/_models"`

Namespace prefix for model paths when `modelsPathsMode` is `"namespaced"`.

### naming

**Type**: `string`  
**Default**: `"kebab"`  
**Options**: `"kebab"`, `"camel"`, `"snake"`

URL path naming convention.

### basePath

**Type**: `string`  
**Default**: `"/api"`

Base path prefix for all endpoints.

### collections.pluralize

**Type**: `boolean`  
**Default**: `true`

Pluralize collection resource names.

### idParam

**Type**: `string`  
**Default**: `"id"`

Parameter name for ID fields in path parameters.

### responseEnvelope

**Type**: `boolean`  
**Default**: `false`

Wrap responses in `{data, meta}` structure.

### pagination.type

**Type**: `string`  
**Default**: `"page"`  
**Options**: `"page"`, `"cursor"`

Pagination strategy.

### pagination.maxPageSize

**Type**: `integer`  
**Default**: `100`

Maximum items per page.

### pagination.defaultPageSize

**Type**: `integer`  
**Default**: `20`

Default items per page.

### relations.expand

**Type**: `boolean`  
**Default**: `false`

Whether to expand relations in responses.

### auth.scheme

**Type**: `string`  
**Default**: `"none"`  
**Options**: `"none"`, `"bearer"`, `"oauth2"`

Authentication scheme.

### servers

**Type**: `array`  
**Default**: `[{url: "http://localhost:8080", description: "Development server"}]`

List of server configurations with URL and optional description.

### outputFormat

**Type**: `string`  
**Default**: `"yaml"`  
**Options**: `"yaml"`, `"json"`

Output file format.

### segmentedOutput

**Type**: `boolean`  
**Default**: `false`

Enable modular fragment output.

### includeAllSchemas

**Type**: `boolean`  
**Default**: `false`

Include all enums and structures, even if unreferenced.

### emitAnnotations

**Type**: `boolean`  
**Default**: `true`

Include `kalo-morphe-*` metadata annotations.

## Contributing

We welcome contributions to the Kalo OpenAPI specification! Here's how you can help:

### Types of Contributions

1. **Specification Improvements**
   * Clarifying conventions
   * Adding examples
   * Documenting edge cases
   * Improving best practices

2. **Implementation Feedback**
   * Testing with OpenAPI tools
   * Identifying compatibility issues
   * Suggesting optimizations
   * Reporting bugs

### How to Contribute

1. **Issues First**
   * Open an issue to discuss proposed changes
   * Reference specification version (KA:OA1)
   * Provide examples and use cases

2. **Pull Requests**
   * Fork the repository
   * Create descriptive branch names
   * Update relevant documentation
   * Include examples for new features

### Style Guidelines

1. **Specification Changes**
   * Use clear, concise language
   * Follow existing formatting patterns
   * Provide YAML examples
   * Maintain consistency with Morphe spec

2. **Examples**
   * Include minimal reproducible examples
   * Show both input (Morphe) and output (OpenAPI)
   * Demonstrate best practices
   * Cover common use cases

## License

This project is licensed under the MIT License.

