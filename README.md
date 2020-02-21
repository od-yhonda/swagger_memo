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

# SwaggerViewer
VScodeでの編集も可能。（以下のプラグインを入れるとSwagger Editorと同じように確認できる。）
https://marketplace.visualstudio.com/items?itemName=Arjun.swagger-viewer
- 表示コマンド
  - $ Preview Swagger
  - Shift + Alt + P
  - File select to Preview Swagger

# ＜基本的は記述方法＞

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
title|string|●|API名
description|string||簡単な説明
termsOfService|string||利用規約
contact|Contact Object||コンタクト情報
license|License Object||ライセンス情報
version|string|●|APIドキュメントのバージョン

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
url|string|●|APIサーバーのURL
description|string||説明

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
schemas|Map[string, Schema Object/Reference Object]||モデル
responses|Map[string, Response Object/Reference Object]||レスポンスの内容
parameters|Map[string, Parameter Object/Reference Object]||パラメーター
examples|Map[string, Example Object/Reference Object]||サンプル

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
/{path}|Path Item Object||APIのパス

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
summary|string||エンドポイントのサマリー
description|string||説明
get|Operation Object||HTTPメソッド
put|Operation Object||HTTPメソッド
post|Operation Object||HTTPメソッド
delete|Operation Object||HTTPメソッド
parameters|[Parameter Object/Reference Object]||パラメータ

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
tags|[string]||APIのグルーピング用のタグ
summary|string||タグのサマリー
description|string||説明
externalDocs|External Documentation Object||追加のドキュメント

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
description|string||説明
url|string|REQUIRED. |ドキュメントリンク

### Sample
```yaml
description: Find more info here
url: https://example.com
```