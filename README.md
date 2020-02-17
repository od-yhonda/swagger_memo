# Swaggerとは？
SwaggerはOAI(Open API Initiative)が採用している、
REST APIを構築するためのオープンソースのフレームワークです。

# 何ができるの？
- API仕様書（HTMLベース）を自動生成できる
- クライアント側(iOSなど)で実装するAPI通信コードを自動生成できる
- サーバー側のAPIインターフェース部分を自動生成できる
- 仮データを返すようなモックサーバーを自動生成できる

上記のようなことを、API定義ファイルを1つ作ることで可能にするAPI仕様書規格。
以前はSwaggerという名前で運営されていたが、技術仕様3.0からOpenAPIという名称に変更された。

# Tool
|ツール名|説明|
|:--|:--|
|Swagger Core|API実装コードからSwagger Specで記載された設計を自動生成|
|Swagger Editor|Swagger Specの設計書を記載するためのエディタ|
|Swagger UI|Swagger Specで記載された設計からドキュメントを自動生成|
|Swagger Codegen|Swagger Specで記載された設計からAPIのスタブを自動生成|

# 実際のサンプル

# SwaggerViewer
https://marketplace.visualstudio.com/items?itemName=Arjun.swagger-viewer


- $ Preview Swagger
- Shift + Alt + P
- File select to Preview Swagger


トップダウン
仕様を定義（swagger editor）で定義ファイルを生成し、公開（swagger ui）
定義ファイルからソースコードを生成(swagger codegen)
ボトムアップ
既に存在するREST APIのソースコードから定義を作成する（swagger core）
コード内にはSwagger用のAnnotationが書いてあること前提（JavaDocのようなものか）
生成された定義を公開（swagger ui）


# Info Object
Field Name|Type|REQUIRED|Description
:--|:--|:--:|:--|

### Sample
```yaml

```



# OpenAPI Object
|フィールド名|必須|概要|
|:--|:--|:--|
|openapi|YES|セマンティックなバージョニングを記述する。今回は3.0.0を用いる。詳しくはドキュメントを参照。|
|info|YES|APIのメタデータを記述する。|
|servers||APIを提供するサーバーを記述する。配列で複数記述可能(STG, PROD等)。|
|paths|YES|APIで利用可能なエンドポイントやメソッドを記述する。|
|components|YES|APIで使用するオブジェクトスキーマを記述する。|
|security||API全体を通して使用可能なセキュリティ仕様を記述する。(OAuth等)|
|tags||APIで使用されるタグのリスト。各種ツールによってパースされる際は、記述された順序で出力される。タグ名はユニークで無ければならない。|
|externalDocs||外部ドキュメントを記述する(API仕様書等)。|


# Info Object
Field Name|Type|REQUIRED|Description
:--|:--|:--:|:--|
title|string|●|The title of the application.
description|string||A short description of the application. CommonMark syntax MAY be used for rich text representation.
termsOfService|string||A URL to the Terms of Service for the API. MUST be in the format of a URL.
contact|Contact Object||The contact information for the exposed API.
license|License Object||The license information for the exposed API.
version|string|●|The version of the OpenAPI document (which is distinct from the OpenAPI Specification version or the API implementation version).

### Sample
```yaml
title: Sample Pet Store App
description: This is a sample server for a pet store.
termsOfService: http://example.com/terms/
contact:
  name: API Support
  url: http://www.example.com/support
  email: support@example.com
license:
  name: Apache 2.0
  url: http://www.apache.org/licenses/LICENSE-2.0.html
version: 1.0.1
```

# Server Object
Field Name|Type|REQUIRED|Description
:--|:--|:--:|:--|
url|string|●|A URL to the target host. This URL supports Server Variables and MAY be relative, to indicate that the host location is relative to the location where the OpenAPI document is being served. Variable substitutions will be made when a variable is named in {brackets}.
description|string||An optional string describing the host designated by the URL. CommonMark syntax MAY be used for rich text representation.
variables|Map[string, Server Variable Object]||A map between a variable name and its value. The value is used for substitution in the server's URL template.
### Sample
```yaml
servers:
- url: https://development.gigantic-server.com/v1
  description: Development server
- url: https://staging.gigantic-server.com/v1
  description: Staging server
- url: https://api.gigantic-server.com/v1
  description: Production server
```

# Components Object
Field Name|Type|REQUIRED|Description
:--|:--|:--:|:--|
schemas|Map[string, Schema Object/Reference Object]||An object to hold reusable Schema Objects.
responses|Map[string, Response Object/Reference Object]||An object to hold reusable Response Objects.
parameters|Map[string, Parameter Object/Reference Object]||An object to hold reusable Parameter Objects.
examples|Map[string, Example Object/Reference Object]||An object to hold reusable Example Objects.
requestBodies|Map[string, Request Body Object/Reference Object]||An object to hold reusable Request Body Objects.
headers|Map[string, Header Object/Reference Object]||An object to hold reusable Header Objects.
securitySchemes|Map[string, Security Scheme Object/Reference Object]||An object to hold reusable Security Scheme Objects.
links|Map[string, Link Object/Reference Object]||An object to hold reusable Link Objects.
callbacks|Map[string, Callback Object/Reference Object]||An object to hold reusable Callback Objects.

### sample
```yaml
components:
  schemas:
    Category:
      type: object
      properties:
        id:
          type: integer
          format: int64
        name:
          type: string
    Tag:
      type: object
      properties:
        id:
          type: integer
          format: int64
        name:
          type: string
  parameters:
    skipParam:
      name: skip
      in: query
      description: number of items to skip
      required: true
      schema:
        type: integer
        format: int32
    limitParam:
      name: limit
      in: query
      description: max records to return
      required: true
      schema:
        type: integer
        format: int32
  responses:
    NotFound:
      description: Entity not found.
    IllegalInput:
      description: Illegal input for operation.
    GeneralError:
      description: General Error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/GeneralError'
  securitySchemes:
    api_key:
      type: apiKey
      name: api_key
      in: header
    petstore_auth:
      type: oauth2
      flows: 
        implicit:
          authorizationUrl: http://example.org/api/oauth/dialog
          scopes:
            write:pets: modify pets in your account
            read:pets: read your pets
```

# Paths Object
Field Name|Type|REQUIRED|Description
:--|:--|:--:|:--|
/{path}|Path Item Object||A relative path to an individual endpoint. The field name MUST begin with a slash. The path is appended (no relative URL resolution) to the expanded URL from the Server Object's url field in order to construct the full URL. Path templating is allowed. When matching URLs, concrete (non-templated) paths would be matched before their templated counterparts. Templated paths with the same hierarchy but different templated names MUST NOT exist as they are identical. In case of ambiguous matching, it's up to the tooling to decide which one to use.

### Sample
```yaml
/pets:
  get:
    description: Returns all pets from the system that the user has access to
    responses:
      '200':
        description: A list of pets.
        content:
          application/json:
            schema:
              type: array
              items:
                $ref: '#/components/schemas/pet'
```


# Path Item Object
Field Name|Type|REQUIRED|Description
:--|:--|:--:|:--|
$ref|string||Allows for an external definition of this path item. The referenced structure MUST be in the format of a Path Item Object. If there are conflicts between the referenced definition and this Path Item's definition, the behavior is undefined.
summary|string||An optional, string summary, intended to apply to all operations in this path.
description|string||An optional, string description, intended to apply to all operations in this path. CommonMark syntax MAY be used for rich text representation.
get|Operation Object||A definition of a GET operation on this path.
put|Operation Object||A definition of a PUT operation on this path.
post|Operation Object||A definition of a POST operation on this path.
delete|Operation Object||A definition of a DELETE operation on this path.
options|Operation Object||A definition of a OPTIONS operation on this path.
head|Operation Object||A definition of a HEAD operation on this path.
patch|Operation Object||A definition of a PATCH operation on this path.
trace|Operation Object||A definition of a TRACE operation on this path.
servers|[Server Object]||An alternative server array to service all operations in this path.
parameters|[Parameter Object/Reference Object]||A list of parameters that are applicable for all the operations described under this path. These parameters can be overridden at the operation level, but cannot be removed there. The list MUST NOT include duplicated parameters. A unique parameter is defined by a combination of a name and location. The list can use the Reference Object to link to parameters that are defined at the OpenAPI Object's components/parameters.

### Sample
```yaml
get:
  description: Returns pets based on ID
  summary: Find pets by ID
  operationId: getPetsById
  responses:
    '200':
      description: pet response
      content:
        '*/*' :
          schema:
            type: array
            items:
              $ref: '#/components/schemas/Pet'
    default:
      description: error payload
      content:
        'text/html':
          schema:
            $ref: '#/components/schemas/ErrorModel'
parameters:
- name: id
  in: path
  description: ID of pet to use
  required: true
  schema:
    type: array
    style: simple
    items:
      type: string  
```


# Operation Object
Field Name|Type|REQUIRED|Description
:--|:--|:--:|:--|
tags|[string]||A list of tags for API documentation control. Tags can be used for logical grouping of operations by resources or any other qualifier.
summary|string||A short summary of what the operation does.
description|string||A verbose explanation of the operation behavior. CommonMark syntax MAY be used for rich text representation.
externalDocs|External Documentation Object||Additional external documentation for this operation.
operationId|string||Unique string used to identify the operation. The id MUST be unique among all operations described in the API. Tools and libraries MAY use the operationId to uniquely identify an operation, therefore, it is RECOMMENDED to follow common programming naming conventions.
parameters|[Parameter Object/Reference Object]||A list of parameters that are applicable for this operation. If a parameter is already defined at the Path Item, the new definition will override it but can never remove it. The list MUST NOT include duplicated parameters. A unique parameter is defined by a combination of a name and location. The list can use the Reference Object to link to parameters that are defined at the OpenAPI Object's components/parameters.
requestBody|Request Body Object/Reference Object||The request body applicable for this operation. The requestBody is only supported in HTTP methods where the HTTP 1.1 specification RFC7231 has explicitly defined semantics for request bodies. In other cases where the HTTP spec is vague, requestBody SHALL be ignored by consumers.
responses|Responses Object|REQUIRED.|The list of possible responses as they are returned from executing this operation.
callbacks|Map[string, Callback Object/Reference Object]||A map of possible out-of band callbacks related to the parent operation. The key is a unique identifier for the Callback Object. Each value in the map is a Callback Object that describes a request that may be initiated by the API provider and the expected responses. The key value used to identify the callback object is an expression, evaluated at runtime, that identifies a URL to use for the callback operation.
deprecated|boolean||Declares this operation to be deprecated. Consumers SHOULD refrain from usage of the declared operation. Default value is false.
security|[Security Requirement Object]||A declaration of which security mechanisms can be used for this operation. The list of values includes alternative security requirement objects that can be used. Only one of the security requirement objects need to be satisfied to authorize a request. This definition overrides any declared top-level security. To remove a top-level security declaration, an empty array can be used.
servers|[Server Object]||An alternative server array to service this operation. If an alternative server object is specified at the Path Item Object or Root level, it will be overridden by this value.

### Sample
```yaml
tags:
- pet
summary: Updates a pet in the store with form data
operationId: updatePetWithForm
parameters:
- name: petId
  in: path
  description: ID of pet that needs to be updated
  required: true
  schema:
    type: string
requestBody:
  content:
    'application/x-www-form-urlencoded':
      schema:
       properties:
          name: 
            description: Updated name of the pet
            type: string
          status:
            description: Updated status of the pet
            type: string
       required:
         - status
responses:
  '200':
    description: Pet updated.
    content: 
      'application/json': {}
      'application/xml': {}
  '405':
    description: Invalid input
    content: 
      'application/json': {}
      'application/xml': {}
security:
- petstore_auth:
  - write:pets
  - read:pets
```


# External Documentation Object
Field Name|Type|REQUIRED|Description
:--|:--|:--:|:--|
description|string||A short description of the target documentation. CommonMark syntax MAY be used for rich text representation.
url|string|REQUIRED. |The URL for the target documentation. Value MUST be in the format of a URL.

### Sample
```yaml
description: Find more info here
url: https://example.com
```