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


# info
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
