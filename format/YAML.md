# KA:OA1:YAML1 - YAML Format Specification

## Overview

KA:OA1:YAML1 defines the YAML format standard for Kalo OpenAPI specifications. This format serves as the base format for OpenAPI documents generated from Morphe schemas.

## Version

**Specification**: KA:OA1:YAML1  
**OpenAPI Version**: 3.1.0  
**YAML Version**: 1.2  
**Status**: 🚧 In Progress

## Format Features

- **Human-readable** - Easy to read and edit
- **Standard YAML 1.2** - Compatible with all YAML parsers
- **Deterministic ordering** - Sorted keys for version control
- **Indentation**: 4 spaces (not tabs)
- **Line endings**: LF (Unix-style)
- **Encoding**: UTF-8

## Document Structure

### Complete Document Example

```yaml
openapi: 3.1.0
info:
    title: Generated API
    description: API generated from Morphe schema
    version: 1.0.0
servers:
    - url: http://localhost:8080
      description: Development server
paths:
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
                nationality:
                    $ref: '#/components/schemas/Nationality'
            required:
                - firstName
                - lastName
                - nationality
            description: Person model
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
        Nationality:
            type: string
            enum:
                - German
                - French
                - American
            description: Nationality enumeration
    responses:
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
tags:
    - name: Person
      description: Operations on Person
```

## Top-Level Structure

### Required Fields

```yaml
openapi: 3.1.0         # OpenAPI version (always 3.1.0)
info:                  # API metadata
paths:                 # API endpoints
components:            # Reusable components
```

### Optional Fields

```yaml
servers:               # API server configurations
tags:                  # Tag definitions for operations
```

## Info Object

```yaml
info:
    title: Generated API                      # API title
    description: API generated from Morphe schema  # API description
    version: 1.0.0                            # API version (semver)
```

**Field Rules**:
- `title`: Required, string
- `description`: Optional, string
- `version`: Required, semver format (X.Y.Z)

## Servers

```yaml
servers:
    - url: http://localhost:8080
      description: Development server
    - url: https://api.example.com
      description: Production server
```

**Field Rules**:
- Array of server objects
- Each server has `url` (required) and `description` (optional)
- URLs must be valid HTTP(S) URLs

## Paths Object

### Path Item Structure

Each path follows this structure:

```yaml
paths:
    /{path}:               # Path template
        {method}:          # HTTP method (get, post, patch, delete)
            tags:          # Operation tags
            summary:       # Brief summary
            description:   # Detailed description
            operationId:   # Unique operation identifier
            parameters:    # Path/query parameters
            requestBody:   # Request body (POST, PATCH)
            responses:     # Response definitions
```

### Collection Path Pattern

```yaml
/api/{resources}:
    get:
        tags:
            - {ResourceName}
        summary: List {resources}
        description: Retrieve a paginated list of {resources}
        operationId: list_{ResourceName}
        parameters:
            - $ref: '#/components/parameters/page'
            - $ref: '#/components/parameters/pageSize'
        responses:
            "200":
                description: List of {resources}
                content:
                    application/json:
                        schema:
                            $ref: '#/components/schemas/{ResourceName}List'
            "400":
                $ref: '#/components/responses/Error'
            "500":
                $ref: '#/components/responses/Error'
    post:
        tags:
            - {ResourceName}
        summary: Create {resource}
        description: Create a new {resource}
        operationId: create_{ResourceName}
        requestBody:
            description: {ResourceName} to create
            content:
                application/json:
                    schema:
                        $ref: '#/components/schemas/{ResourceName}Create'
            required: true
        responses:
            "201":
                description: Created {resource}
                content:
                    application/json:
                        schema:
                            $ref: '#/components/schemas/{ResourceName}'
            "400":
                $ref: '#/components/responses/Error'
            "500":
                $ref: '#/components/responses/Error'
```

### Item Path Pattern

```yaml
/api/{resources}/{id}:
    get:
        tags:
            - {ResourceName}
        summary: Get {resource}
        description: Retrieve a single {resource} by ID
        operationId: get_{ResourceName}
        parameters:
            - name: id
              in: path
              description: {ResourceName} ID
              required: true
              schema:
                type: {integer|string}      # integer for AutoIncrement, string for UUID
                format: {int64|uuid}        # format hint
        responses:
            "200":
                description: {ResourceName} details
                content:
                    application/json:
                        schema:
                            $ref: '#/components/schemas/{ResourceName}'
            "404":
                $ref: '#/components/responses/Error'
            "500":
                $ref: '#/components/responses/Error'
    patch:
        tags:
            - {ResourceName}
        summary: Update {resource}
        description: Update an existing {resource}
        operationId: update_{ResourceName}
        parameters:
            - name: id
              in: path
              description: {ResourceName} ID
              required: true
              schema:
                type: {integer|string}
                format: {int64|uuid}
        requestBody:
            description: {ResourceName} updates
            content:
                application/json:
                    schema:
                        $ref: '#/components/schemas/{ResourceName}Update'
            required: true
        responses:
            "200":
                description: Updated {resource}
                content:
                    application/json:
                        schema:
                            $ref: '#/components/schemas/{ResourceName}'
            "400":
                $ref: '#/components/responses/Error'
            "404":
                $ref: '#/components/responses/Error'
            "500":
                $ref: '#/components/responses/Error'
    delete:
        tags:
            - {ResourceName}
        summary: Delete {resource}
        description: Delete a {resource}
        operationId: delete_{ResourceName}
        parameters:
            - name: id
              in: path
              description: {ResourceName} ID
              required: true
              schema:
                type: {integer|string}
                format: {int64|uuid}
        responses:
            "204":
                description: Successfully deleted
            "404":
                $ref: '#/components/responses/Error'
            "500":
                $ref: '#/components/responses/Error'
```

## Components Object

### Schemas

#### Resource Schema Pattern

```yaml
{ResourceName}:
    type: object
    properties:
        id:                             # Primary ID field
            type: {integer|string}
            {format: {int64|uuid}}      # Optional format
            readOnly: true              # Always read-only
        {fieldName}:                    # Regular field
            type: {type}
            {description: {text}}       # Optional description
            {nullable: true}            # For optional fields
        {relationName}ID:               # Foreign key field
            type: string
            description: Foreign key to {RelatedResource}
            nullable: true
        {enumField}:                    # Enum reference
            $ref: '#/components/schemas/{EnumName}'
    required:
        - {requiredField1}
        - {requiredField2}
    description: {ResourceName} model
```

**Field Ordering**:
1. ID field first
2. Regular fields (alphabetical)
3. Foreign key fields (alphabetical)
4. Enum reference fields (alphabetical)

**Type Mappings**:

| Morphe Type | JSON Schema Type | Format | Additional |
|-------------|------------------|--------|------------|
| AutoIncrement | integer | int64 | readOnly: true |
| UUID | string | uuid | readOnly: true |
| String | string | - | - |
| Integer | integer | - | - |
| Float | number | double | - |
| Boolean | boolean | - | - |
| Date | string | date | - |
| Time | string | date-time | - |
| Protected | string | - | writeOnly: true |
| Sealed | string | - | writeOnly: true |
| {Enum} | $ref | - | Reference to enum schema |

#### Create DTO Schema Pattern

```yaml
{ResourceName}Create:
    type: object
    properties:
        {fieldName}:                    # No ID field
            type: {type}
            {description: {text}}
        {relationName}ID:
            type: string
            description: Foreign key to {RelatedResource}
            nullable: true
        {enumField}:
            $ref: '#/components/schemas/{EnumName}'
    required:
        - {requiredField1}
        - {requiredField2}
    description: Create {ResourceName} request
```

**Rules**:
- Excludes auto-generated ID fields (AutoIncrement, UUID)
- Includes all other fields
- Same required constraints as base schema

#### Update DTO Schema Pattern

```yaml
{ResourceName}Update:
    type: object
    properties:
        {fieldName}:                    # All fields nullable
            type: {type}
            nullable: true
            {description: {text}}
        {relationName}ID:
            type: string
            description: Foreign key to {RelatedResource}
            nullable: true
        {enumField}:                    # Enums referenced, not nullable inline
            $ref: '#/components/schemas/{EnumName}'
    # No required array - all fields optional
    description: Update {ResourceName} request
```

**Rules**:
- All fields optional (no `required` array)
- Scalar fields marked `nullable: true`
- Enum references not marked nullable (handled by reference)
- Supports partial updates (PATCH semantics)

#### List Response Schema Pattern

```yaml
{ResourceName}List:
    type: object
    properties:
        data:
            type: array
            items:
                $ref: '#/components/schemas/{ResourceName}'
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

**Rules**:
- `data` contains array of resource objects
- `meta` contains pagination metadata
- Both `data` and `meta` required

#### Enum Schema Pattern

```yaml
{EnumName}:
    type: {string|integer|number}     # Based on Morphe enum type
    enum:
        - {value1}
        - {value2}
        - {value3}
    description: {EnumName} enumeration
```

**Type Mappings**:

| Morphe Enum Type | JSON Schema Type |
|------------------|------------------|
| String | string |
| Integer | integer |
| Float | number |

**Example - String Enum**:
```yaml
Nationality:
    type: string
    enum:
        - German
        - French
        - American
    description: Nationality enumeration
```

**Example - Integer Enum**:
```yaml
StatusCode:
    type: integer
    enum:
        - 100
        - 200
        - 404
        - 500
    description: StatusCode enumeration
```

### Responses

#### Error Response Pattern

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

**Usage**:
```yaml
responses:
    "400":
        $ref: '#/components/responses/Error'
    "404":
        $ref: '#/components/responses/Error'
    "500":
        $ref: '#/components/responses/Error'
```

### Parameters

#### Pagination Parameters

```yaml
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

**Usage**:
```yaml
parameters:
    - $ref: '#/components/parameters/page'
    - $ref: '#/components/parameters/pageSize'
```

#### Path ID Parameter Pattern

```yaml
parameters:
    - name: id
      in: path
      description: {ResourceName} ID
      required: true
      schema:
        type: {integer|string}
        {format: {int64|uuid}}
```

## Tags

```yaml
tags:
    - name: {ResourceName}
      description: Operations on {ResourceName}
```

**Rules**:
- One tag per resource
- Tag name matches resource name (PascalCase)
- Description follows pattern: "Operations on {ResourceName}"

## Segmented Output Format

### Fragment Structure

When `segmentedOutput: true`, files are organized as:

```
{outputPath}/
  generated/
    entities/
      {Resource}.entity.yaml
    dtos/
      {Resource}.create.yaml
      {Resource}.update.yaml
      {Resource}.list.yaml
    enums/
      {Enum}.enum.yaml
    paths/
      {resources}.paths.yaml
    parameters/
      pagination.parameters.yaml
    responses/
      error.response.yaml
  composed/
    root.yaml
  dist/
    openapi.yaml
```

### Entity Fragment Format

```yaml
kalo-morphe-id-strategy: {autoincrement|uuid}
kalo-morphe-name: {ResourceName}
kalo-morphe-origin: {model|entity}
kalo-morphe-resource-type: {model|entity}
schema:
    type: object
    properties:
        # ... properties
    required:
        # ... required fields
    description: {description}
```

### DTO Fragment Format

```yaml
kalo-morphe-name: {ResourceName}{Create|Update|List}
kalo-morphe-origin: dto
schema:
    type: object
    properties:
        # ... properties
    {required:}
        # ... required fields (create/list only)
    description: {description}
```

### Enum Fragment Format

```yaml
kalo-morphe-name: {EnumName}
kalo-morphe-origin: enum
schema:
    type: {string|integer|number}
    enum:
        # ... values
    description: {description}
```

### Path Fragment Format

```yaml
kalo-morphe-operation-type: crud
kalo-morphe-resource: {resources}
kalo-morphe-resource-type: {model|entity}
paths:
    /api/{resources}:
        get:
            # ... list operation
        post:
            # ... create operation
    /api/{resources}/{id}:
        get:
            # ... get operation
        patch:
            # ... update operation
        delete:
            # ... delete operation
```

### Parameters Fragment Format

```yaml
kalo-morphe-origin: parameter
kalo-morphe-shared: true
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

### Responses Fragment Format

```yaml
kalo-morphe-origin: response
kalo-morphe-shared: true
responses:
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

### Composed Root Format

```yaml
openapi: 3.1.0
info:
    title: Generated API
    description: API generated from Morphe schema
    version: 1.0.0
servers:
    - url: http://localhost:8080
      description: Development server
paths:
    /api/{resources}:
        $ref: ./generated/paths/{resources}.paths.yaml
components:
    schemas:
        {ResourceName}:
            $ref: ./generated/entities/{Resource}.entity.yaml#/schema
        {ResourceName}Create:
            $ref: ./generated/dtos/{Resource}.create.yaml#/schema
        {ResourceName}Update:
            $ref: ./generated/dtos/{Resource}.update.yaml#/schema
        {ResourceName}List:
            $ref: ./generated/dtos/{Resource}.list.yaml#/schema
        {EnumName}:
            $ref: ./generated/enums/{Enum}.enum.yaml#/schema
    responses:
        Error:
            $ref: ./generated/responses/error.response.yaml#/responses/Error
    parameters:
        page:
            $ref: ./generated/parameters/pagination.parameters.yaml#/parameters/page
        pageSize:
            $ref: ./generated/parameters/pagination.parameters.yaml#/parameters/pageSize
tags:
    - name: {ResourceName}
      description: Operations on {ResourceName}
kalo-morphe-composed: true
kalo-morphe-version: 1.0.0
```

## Reference Patterns

### Internal Schema References

```yaml
$ref: '#/components/schemas/{SchemaName}'
```

### Internal Response References

```yaml
$ref: '#/components/responses/{ResponseName}'
```

### Internal Parameter References

```yaml
$ref: '#/components/parameters/{ParameterName}'
```

### External Fragment References (Composed Mode)

**Schema Reference**:
```yaml
$ref: ./generated/entities/{Resource}.entity.yaml#/schema
$ref: ./generated/dtos/{Resource}.create.yaml#/schema
$ref: ./generated/enums/{Enum}.enum.yaml#/schema
```

**Path Reference**:
```yaml
$ref: ./generated/paths/{resources}.paths.yaml
```

**Response Reference**:
```yaml
$ref: ./generated/responses/error.response.yaml#/responses/Error
```

**Parameter Reference**:
```yaml
$ref: ./generated/parameters/pagination.parameters.yaml#/parameters/page
```

## Formatting Rules

### Indentation

- Use **4 spaces** for each indentation level
- Never use tabs
- Consistent indentation throughout document

### Line Endings

- Use **LF** (Line Feed, Unix-style) line endings
- Not CRLF (Windows) or CR (old Mac)

### Encoding

- Always use **UTF-8** encoding
- No BOM (Byte Order Mark)

### Key Ordering

Keys are ordered for deterministic output:

**Schemas**:
1. `type`
2. `properties`
3. `required`
4. `description`
5. Other fields (alphabetical)

**Properties**:
1. ID field
2. Regular fields (alphabetical)
3. Foreign key fields (alphabetical)

**Operations**:
1. `tags`
2. `summary`
3. `description`
4. `operationId`
5. `parameters`
6. `requestBody`
7. `responses`

### Quoting

- **Response codes**: Always quoted (e.g., `"200"`, `"404"`)
- **URLs**: Quoted if contains special characters
- **Descriptions**: Quoted if multi-line or contains special characters
- **Simple values**: Unquoted when possible

### Arrays

**Single-line arrays** (short, few items):
```yaml
enum: [value1, value2, value3]
```

**Multi-line arrays** (longer, many items):
```yaml
enum:
    - value1
    - value2
    - value3
```

**Convention**: Use multi-line for 3+ items or items > 20 chars

## Complete Multi-Resource Example

```yaml
openapi: 3.1.0
info:
    title: Generated API
    description: API generated from Morphe schema
    version: 1.0.0
servers:
    - url: http://localhost:8080
      description: Development server
paths:
    /api/companies:
        get:
            tags:
                - Company
            summary: List companies
            description: Retrieve a paginated list of companies
            operationId: list_Company
            parameters:
                - $ref: '#/components/parameters/page'
                - $ref: '#/components/parameters/pageSize'
            responses:
                "200":
                    description: List of companies
                    content:
                        application/json:
                            schema:
                                $ref: '#/components/schemas/CompanyList'
                "400":
                    $ref: '#/components/responses/Error'
                "500":
                    $ref: '#/components/responses/Error'
        post:
            tags:
                - Company
            summary: Create company
            description: Create a new company
            operationId: create_Company
            requestBody:
                description: Company to create
                content:
                    application/json:
                        schema:
                            $ref: '#/components/schemas/CompanyCreate'
                required: true
            responses:
                "201":
                    description: Created company
                    content:
                        application/json:
                            schema:
                                $ref: '#/components/schemas/Company'
                "400":
                    $ref: '#/components/responses/Error'
                "500":
                    $ref: '#/components/responses/Error'
    /api/companies/{id}:
        get:
            tags:
                - Company
            summary: Get company
            description: Retrieve a single company by ID
            operationId: get_Company
            parameters:
                - name: id
                  in: path
                  description: Company ID
                  required: true
                  schema:
                    type: integer
                    format: int64
            responses:
                "200":
                    description: Company details
                    content:
                        application/json:
                            schema:
                                $ref: '#/components/schemas/Company'
                "404":
                    $ref: '#/components/responses/Error'
                "500":
                    $ref: '#/components/responses/Error'
        patch:
            tags:
                - Company
            summary: Update company
            description: Update an existing company
            operationId: update_Company
            parameters:
                - name: id
                  in: path
                  description: Company ID
                  required: true
                  schema:
                    type: integer
                    format: int64
            requestBody:
                description: Company updates
                content:
                    application/json:
                        schema:
                            $ref: '#/components/schemas/CompanyUpdate'
                required: true
            responses:
                "200":
                    description: Updated company
                    content:
                        application/json:
                            schema:
                                $ref: '#/components/schemas/Company'
                "400":
                    $ref: '#/components/responses/Error'
                "404":
                    $ref: '#/components/responses/Error'
                "500":
                    $ref: '#/components/responses/Error'
        delete:
            tags:
                - Company
            summary: Delete company
            description: Delete a company
            operationId: delete_Company
            parameters:
                - name: id
                  in: path
                  description: Company ID
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
components:
    schemas:
        Company:
            type: object
            properties:
                id:
                    type: integer
                    readOnly: true
                name:
                    type: string
                taxID:
                    type: string
            required:
                - name
                - taxID
            description: Company model
        CompanyCreate:
            type: object
            properties:
                name:
                    type: string
                taxID:
                    type: string
            required:
                - name
                - taxID
            description: Create Company request
        CompanyUpdate:
            type: object
            properties:
                name:
                    type: string
                    nullable: true
                taxID:
                    type: string
                    nullable: true
            description: Update Company request
        CompanyList:
            type: object
            properties:
                data:
                    type: array
                    items:
                        $ref: '#/components/schemas/Company'
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
        Person:
            type: object
            properties:
                companyID:
                    type: string
                    description: Foreign key to Company
                    nullable: true
                firstName:
                    type: string
                id:
                    type: integer
                    readOnly: true
                lastName:
                    type: string
                nationality:
                    $ref: '#/components/schemas/Nationality'
            required:
                - firstName
                - lastName
                - nationality
            description: Person model
        PersonCreate:
            type: object
            properties:
                companyID:
                    type: string
                    description: Foreign key to Company
                    nullable: true
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
        PersonUpdate:
            type: object
            properties:
                companyID:
                    type: string
                    description: Foreign key to Company
                    nullable: true
                firstName:
                    type: string
                    nullable: true
                lastName:
                    type: string
                    nullable: true
                nationality:
                    $ref: '#/components/schemas/Nationality'
            description: Update Person request
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
        Nationality:
            type: string
            enum:
                - German
                - French
                - American
            description: Nationality enumeration
    responses:
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
tags:
    - name: Company
      description: Operations on Company
    - name: Person
      description: Operations on Person
```

## Validation

Valid KA:OA1:YAML1 documents must:

1. Be valid YAML 1.2
2. Be valid OpenAPI 3.1.0
3. Follow Kalo naming conventions
4. Include all required Kalo patterns (CRUD operations, schemas, responses)
5. Use consistent indentation (4 spaces)
6. Use LF line endings
7. Be UTF-8 encoded

## Tooling

Recommended tools for working with KA:OA1:YAML1:

- **Validation**: OpenAPI CLI, Spectral
- **Editing**: VS Code with OpenAPI extension
- **Documentation**: Swagger UI, Redoc
- **Testing**: Postman, Insomnia
- **Bundling**: openapi-generator-cli

## License

This specification is licensed under the MIT License.

