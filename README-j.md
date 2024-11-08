# Tsubaiso API

このドキュメントでは Tsubaiso API の説明をします。
こちらの Tsubaiso REST API 1.0 のドキュメントも合わせてご参照ください。
https://tsubaiso.net/eap/apidoc

## 目次
 - [Root Endpoint](#root-endpoint)
 - [リクエストのフォーマット](#リクエストのフォーマット)
 - [認証](#認証)
 - [レスポンスコードとエラー処理](#レスポンスコードとエラー処理)
 - [リソース](#リソース)
   - [売上明細](#売上明細)
   - [入金・消込明細](#入金消込明細)
   - [支払・消込明細](#支払消込明細)
   - [仕入・経費明細](#仕入経費明細)
   - [取引先](#取引先)
   - [社員](#社員)
   - [社員基本情報](#社員基本情報)
   - [社員基本情報マスタ](#社員基本情報マスタ)
   - [仕訳帳](#仕訳帳)
   - [マニュアル仕訳](#マニュアル仕訳)
   - [旅費・経費精算](#旅費経費精算)
   - [旅費・経費精算明細](#旅費経費精算明細)
   - [部門管理](#部門管理)
   - [セグメント(旧タグ)マスタ](#セグメント旧タグマスタ)
   - [経費精算原因マスタ](#経費精算原因マスタ)
   - [販売原因マスタ](#販売原因マスタ)
   - [購買原因マスタ](#購買原因マスタ)
   - [銀行原因マスタ](#銀行原因マスタ)
   - [現金原因マスタ](#現金原因マスタ)
   - [賞与データ](#賞与データ)
   - [給与データ](#給与データ)
   - [仕訳配賦](#仕訳配賦)
   - [月次推移表](#月次推移表)
   - [銀行口座マスタ](#銀行口座マスタ)
   - [銀行口座](#銀行口座)
   - [銀行口座明細](#銀行口座明細)
   - [現金出納帳マスタ](#現金出納帳マスタ)
   - [現金出納帳](#現金出納帳)
   - [現金出納帳明細](#現金出納帳明細)
   - [税区分マスタ](#税区分マスタ)
   - [棚卸資産マスタ](#棚卸資産マスタ)
   - [資産種類マスタ](#資産種類マスタ)
   - [API履歴](#API履歴)
   - [外部連携機能](#外部連携機能)
   - [予定日](#予定日)
- [外部連携機能](#外部連携機能)

## Root Endpoint

```sh
https://tsubaiso.net
```

## リクエストのフォーマット

API へのすべてのリクエストは JSON を使って行います。

## 認証

Tsubaiso API にアクセスするためには、アクセストークンを取得する必要があります。
以下は、アクセストークンを使って売上明細一覧のデータを取得する例です。

```
$ curl -i -H "Access-Token: xxxxxxxxxxxxxxxxx" -H "Accept: application/json" -H "Content-Type: application/json" https://tsubaiso.net/ar/list
```

### クライアント認証トークン

アクセストークンには、任意のクライアント認証トークンを付加することができます。
クライアント認証トークンが設定されている場合は、クライアント認証トークンも HTTP リクエストヘッダに付加する必要があります。

```
$ curl -i -H "Access-Token: xxxxxxxxxxxxxxxxx" -H "Client-Auth-Token: yyyyyyyyyyyy" -H "Accept: application/json" -H "Content-Type: application/json" https://tsubaiso.net/ar/list
```

なお、クライアント認証トークンを**付加していない**アクセストークンを使用する場合、リクエストヘッダにクライアント認証トークンを付加してリクエストすると認証エラーになります。
この措置は、クライアント認証トークンを使用する運用を想定しているときに、誤ってクライアント認証トークンの設定をせずに運用を開始してしまうケースを防止するためです。

## レスポンスコードとエラー処理

Code | Description
--- | ---
`200 OK` | リクエスト成功 |
`204 No Content` | リクエストに成功したが返されるコンテンツはありません。
`401 Not Authorized` | アクセストークンが送られていないか正しくありません。もしくはクライアント認証トークンが正しくありません。
`403 Forbidden` | そのリクエストに必要な権限がありません。
`404 Not found` | 指定されたパスは正しくないか、リソースが見つかりません。
`422 Unprocessable Entity` | 1つ以上のパラメータが正しくないか不足しています。エラーメッセージで原因が判別できます。
`500 Internal Server Error` | サーバーで何らかのエラーが起こりました。
`503 Service Unavailable` | あなたの IP アドレスから非常に多くのリクエストがあった場合、このエラーが発生します。次のリクエストまで少し時間を開けてください。

## リソース

#### 売上明細

**/ar/list/:year/:month**

説明: このエンドポイントは特定の年月の売上明細の一覧を返します。年月パラメータが指定されなかった場合、現在の月の明細が返されます。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/ar/list/:year/:month
```

JSON レスポンスの例:
```
[
    {
        "ar_receipt_attachments_count": null,
        "code": null,
        "created_at": "2015/10/05 15:26:43 +0900",
        "dc": "d",
        "dept_code": "DEPT A",
        "id": 8833,
        "memo": "500 widgets",
        "realization_timestamp": "2015/10/31 00:00:00 +0900",
        "regist_user_code": "sample_user",
        "sales_journal_dc_id": 6832,
        "scheduled_memo": null,
        "scheduled_receive_timestamp": null,
        "update_user_code": null,
        "updated_at": "2015/10/05 15:26:43 +0900",
        "account_code": "501",
        "price_including_tax": 5400,
        "price_excluding_tax": 5000,
        "sales_tax": 400,
        "tax_code": 18,
        "customer_master_code": 101,
        "reason_master_code": "SALES"
    }, {
        "ar_receipt_attachments_count": null,
        "code": null,
        "created_at": "2015/10/05 17:39:34 +0900",
        "dc": "d",
        "dept_code": "DEPT B",
        "id": 8834,
        "memo": "100 clocks",
        "realization_timestamp": "2015/10/31 00:00:00 +0900",
        "regist_user_code": "sample_user",
        "sales_journal_dc_id": 8834,
        "scheduled_memo": null,
        "scheduled_receive_timestamp": null,
        "update_user_code": null,
        "updated_at": "2015/10/05 17:39:34 +0900",
        "account_code": "500",
        "price_including_tax": 10800,
        "price_excluding_tax": 10000,
        "sales_tax": 800,
        "tax_code": 0,
        "customer_master_code": 895820,
        "reason_master_code": "SALES"
    }
]
```

**/ar/show/:id**

説明: このエンドポイントは単一の売上明細を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/ar/show/:id
```

JSON レスポンスの例:
```
{
    "ar_receipt_attachments_count": null,
    "code": null,
    "created_at": "2015/10/05 15:26:43 +0900",
    "dc": "d",
    "dept_code": "DEPT A",
    "id": 8833,
    "memo": "500 widgets",
    "realization_timestamp": "2015/10/31 00:00:00 +0900",
    "regist_user_code": "sample_user",
    "sales_journal_dc_id": 6832,
    "scheduled_memo": null,
    "scheduled_receive_timestamp": null,
    "update_user_code": null,
    "updated_at": "2015/10/05 15:26:43 +0900",
    "account_code": "501",
    "price_including_tax": 5400,
    "price_excluding_tax": 5000,
    "sales_tax": 400,
    "tax_code": 18,
    "customer_master_code": 101,
    "reason_master_code": "SALES"
}
```

**/ar/create**

説明: 売上明細を新規作成します。作成に成功した場合、新規作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ar/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`price_including_tax` | *required* | Integer | 売上高(税込)
`realization_timestamp` | *required* | String | 明細の実現日。 "YYYY-MM-DD" 形式
`customer_master_code` | *required* | String | 取引先コード
`reason_master_code` | *required* | String | 明細の原因コード。仕訳を作成するために使われます。
`dc` | *required* | String | 原因区分。 'd' は debit の意で「増加」に、'c' は credit の意で「減少」になります。
`memo` | *required* | String | メモ。値は空文字でも構いませんが必須項目です。
`tax_code` | *optional* | Integer | 税区分コード (省略された場合、原因コードのデフォルト税区分が使用されます。)
`dept_code` | *optional* | String | 部門コード
`sales_tax` | *optional* | Integer | 消費税額。指定されなかった場合、自動で計算されます。
`scheduled_receive_timestamp` | *optional* | String | 入金予定日。 “YYYY-MM-DD”形式
`scheduled_memo` | *optional* | String | 入金予定に関するメモ
`tag_list` | *optional* | String | セグメント(旧タグ)識別コード文字列(カンマ区切り)。
`tag_name_list` | *optional* | String | セグメント(旧タグ)名称文字列(カンマ区切り)。**このオプションはtag_listが存在しない場合にのみ有効です**
`usage_no` | *optional* | Integer | 明細の種類. value: 0: 通常明細, 1: 決算整理明細. **システム機能設定の決算整理機能が有効の場合にのみ使用できます。決算整理機能が無効の場合に1を指定するとエラーが返却されます。**
`data_partner` | *optional* | Object | 詳細は[外部連携機能](#外部連携機能)を参照。

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"price_including_tax": 5400, "realization_timestamp": "2015-10-31", "customer_master_code": "101", "dept_code": "DEPT A", "reason_master_code": "SALES", "dc": "d", "memo": "500 widgets", "tax_code": 0}' https://tsubaiso.net/ar/create
```

**/ar_receipts/update/:id**

説明: 指定された id の売上明細を更新します。更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ar_receipts/update/:id
```

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "更新  メモ", "price_including_tax": 10800}'  https://tsubaiso.net/ar_receipts/update/8833
```

**/ar/destroy/:id**

説明: 指定された id の売上明細を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ar/destroy/:id
```

**/ar_receipts/balance/:year/:month**

説明: このエンドポイントは特定の年月の取引先の売上明細の残高一覧を返します。年月パラメータが指定されなかった場合、現在の月の明細が返されます。

HTTPメソッド: GET

URL 構成例:

``` sh
https://tsubaiso.net/ar_receipts/balance/:year/:month
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ar_receipts/balance/2017/1
```

JSON レスポンス例:
``` sh
[
  {
    "ym": "2017-01",
    "customer_master_code": "0",
    "ar_segment": 1,
    "start_balance": 262510800,
    "finish_balance": 262510800,
    "received_amount": 0,
    "sales": 0,
    "tax": 0,
    "remittance_charge": 0,
    "exchange_gain_and_loss": 0,
    "memo": null,
    "created_at": null
  },
  {
    "ym": "2017-01",
    "customer_master_code": "101",
    "ar_segment": 1,
    "start_balance": 10000,
    "finish_balance": 10000,
    "received_amount": 0,
    "sales": 0,
    "tax": 0,
    "remittance_charge": 0,
    "exchange_gain_and_loss": 0,
    "memo": null,
    "created_at": null
  }
]
```
#### 入金消込明細

**/ar_reconciliations/list**

説明: このエンドポイントは対象の年月の入金・消込明細の一覧を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/ar_reconciliations/list/:year/:month
```

JSON レスポンスの例:
```
[
  {
    "id":3098,
    "serial_no":265,
    "customer_master_id":123,
    "dept_code":"NEVER_ENDING",
    "memo":"",
    "dc":"d",
    "receipt_journal_dc_id":23778,
    "transfer_journal_id":23481,
    "regist_user_code":"yamakawa",
    "update_user_code":null,
    "created_at":"2018/11/13 14:18:28 +0900",
    "updated_at":"2018/11/13 14:18:28 +0900",
    "tag_list":[],
    "reconciliation":123,
    "exchange_gain_and_loss":null,
    "remittance_charge_inclusive":null,
    "data_partner":{}}]},
    {
     "journal_timestamp":"2018/11/30 00:00:00 +0900",
     "brief":"摘要",
     "memo":"メモ",
     "receipt_amount":123,
     "reconciliation_id":23790,
     "reconcile_transactions”:[]},
  {
    "id":3099,
    "serial_no":266,
    "customer_master_id":123,
    "dept_code":"NEVER_ENDING",
    "memo":"",
    "dc":"d",
    "receipt_journal_dc_id":23779,
    "transfer_journal_id":23482,
    "regist_user_code":"yamakawa",
    "update_user_code":null,
    "created_at":"2018/11/13 14:18:28 +0900",
    "updated_at":"2018/11/13 14:18:28 +0900",
    "tag_list":[],
    "reconciliation":123,
    "exchange_gain_and_loss":null,
    "remittance_charge_inclusive":null,
    "data_partner":{}}]},
    {
     "journal_timestamp":"2018/11/30 00:00:00 +0900",
     "brief":"摘要",
     "memo":"メモ",
     "receipt_amount":123,
     "reconciliation_id":23790,
     "reconcile_transactions”:[]}
]
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ar_reconciliations/list/2021/5
```

**/ar_reconciliations/show/**

説明: このエンドポイントは単一の入金・消込明細を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/ar_reconciliations/show?reconciliation_id=:id
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reconciliation_id` | *required* | Integer | 消込仕訳明細Id

JSON レスポンスの例:
```
{
    "id":3098,
    "serial_no":265,
    "customer_master_id":123,
    "dept_code":"NEVER_ENDING",
    "memo":"",
    "dc":"d",
    "receipt_journal_dc_id":23778,
    "transfer_journal_id":23481,
    "regist_user_code":"yamakawa",
    "update_user_code":null,
    "created_at":"2018/11/13 14:18:28 +0900",
    "updated_at":"2018/11/13 14:18:28 +0900",
    "tag_list":[],
    "reconciliation":123,
    "exchange_gain_and_loss":null,
    "remittance_charge_inclusive":null,
    "data_partner":{}}]},
    {
     "journal_timestamp":"2018/11/30 00:00:00 +0900",
     "brief":"摘要",
     "memo":"メモ",
     "receipt_amount":123,
     "reconciliation_id":23790,
     "reconcile_transactions”:[]
}
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ar_reconciliations/show?reconciliation_id=:id
```

**/ar_reconciliations/reconcile/**

説明: 入金・消込明細を消込します。消込に成功した場合、作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ar_reconciliations/reconcile
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reconciliation_id` | *required* | Integer | 消込仕訳明細Id
`reconcile_transactions` | *required* | Array of Object | reconcile_transactionsは配列で指定します。

reconcile_transactions:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reconciliation` | *required* | Integer | 入金額
`customer_master_code` | *required* | String | 取引先コード
`memo` | *optional* | String | メモ
`dept_code` | *optional* | String | 部門コード
`remittance_charge` | *optional* | Integer | 当方負担送金手数料
`exchange_gain_and_loss` | *optional* | Integer | 為替差損益
`tag_list` | *optional* | String | セグメント(旧タグ)識別コード文字列(カンマ区切り)。
`tag_name_list` | *optional* | String | セグメント(旧タグ)名称文字列(カンマ区切り)。**このオプションはtag_listが存在しない場合にのみ有効です**
`data_partner` | *optional* | Object | 詳細は[外部連携機能](#外部連携機能)を参照。

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXX" -X POST -d '{ "reconcile_transactions": [{"reconciliation": 100, "customer_master_code": "individual"}, {"reconciliation": 23,"remittance_charge": 300, "customer_master_code": "KAI", "memo": "This is a scheduled memo2"}] }' http://burikama.tech/tsubaiso.wen/eap/ar_reconciliations/reconcile?reconciliation_id=:id
```

**/ar_reconciliations/unreconcile/**

説明: 指定された id の入金・消込明細を未消込します。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ar_reconciliations/unreconcile
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reconciliation_id` | *required* | Integer | 消込Id

リクエストの例:
```sh
 curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXX" -X POST  http://burikama.tech/tsubaiso.wen/eap/ar_reconciliations/unreconcile?reconciliation_id=:id
```

#### 支払消込明細

**/ap_reconciliations/list**

説明: このエンドポイントは対象の年月の支払・消込明細の一覧を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/ap_reconciliations/list/:year/:month
```

JSON レスポンスの例:
```
[
  {
    "journal_timestamp": "2019/04/22 00:00:00 +0900",
    "brief": "テスト",
    "memo": "",
    "payment_amount": 100,
    "reconciliation_id": 23328,
    "reconcile_transactions": [
      {
        "id": 643,
        "serial_no": 15,
        "customer_master_id": 123,
        "dept_code": "NEVER_ENDING",
        "memo": "",
        "dc": "c",
        "payment_journal_dc_id": 23328,
        "transfer_journal_id": 22973,
        "regist_user_code": "yamakawa",
        "update_user_code": null,
        "created_at": "2019/04/22 16:48:10 +0900",
        "updated_at": "2019/04/22 16:48:11 +0900",
        "tag_list": [],
        "reconciliation": 100,
        "withholding_tax": null,
        "exchange_gain_and_loss": null,
        "remittance_charge_inclusive": null,
        "data_partner": {}
      }
    ]
  },
  {
    "journal_timestamp": "2019/04/22 00:00:00 +0900",
    "brief": "memo",
    "memo": "",
    "payment_amount": 9999,
    "reconciliation_id": 23332,
    "reconcile_transactions": []
  }
]
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ap_reconciliations/list/2021/5
```

**/ap_reconciliations/show/**

説明: このエンドポイントは単一の支払・消込明細を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/ap_reconciliations/show?reconciliation_id=:id
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reconciliation_id` | *required* | Integer | 消込仕訳明細Id

JSON レスポンスの例:
```
{
  "journal_timestamp": "2019/04/22 00:00:00 +0900",
  "brief": "memo",
  "memo": "",
  "payment_amount": 9999,
  "reconciliation_id": 23332,
  "reconcile_transactions": []
}
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ap_reconciliations/show?reconciliation_id=:id
```

**/ap_reconciliations/reconcile/**

説明: 支払・消込明細を消込します。消込に成功した場合、作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ap_reconciliations/reconcile/
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reconciliation_id` | *required* | Integer | 消込仕訳明細Id
`reconcile_transactions` | *required* | Array of Object | reconcile_transactionsは配列で指定します。

reconcile_transactions:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`customer_master_id` | *required* | Integer | 消込先(取引先Id)
`withholding_tax` | *optional* | Integer | 源泉徴収額
`withholding_tax` | *optional* | Integer | 為替差損益
`exchange_gain_and_loss` | *optional* | Integer | 為替差損益
`remittance_charge_inclusive` | *optional* | Integer | 支払手数料(支払先負担額)
`reconciliation` | *required* | Integer | 支払額
`dept_code` | *optional* | String | 部門
`tag_list` | *optional* | Integer | セグメント
`memo` | *optional* | String | メモ
`data_partner` | *optional* | Object | 詳細は[外部連携機能](#外部連携機能)を参照。


リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXX" -X POST -d '{ "reconcile_transactions": [{"reconciliation": 100, "customer_master_code": "individual"}]}' https://tsubaiso.net/eap/ap_reconciliations/reconcile?reconciliation_id=:id
```

**/ar_reconciliations/unreconcile/**

説明: 指定された id の支払・消込明細を未消込します。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ap_reconciliations/unreconcile/
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reconciliation_id` | *required* | Integer | 消込仕訳明細Id

リクエストの例:
```sh
 curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXX" -X POST  https://tsubaiso.net/ar_reconciliations/unreconcile?reconciliation_id=:id
```

#### 仕入経費明細

**/ap_payments/list/:year/:month**

説明: このエンドポイントは特定の年月の仕入・経費明細の一覧を返します。年月パラメータが指定されなかった場合、現在の月の明細が返されます。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/ap_payments/list/:year/:month
```

JSON レスポンスの例:
```
[
    {
        "accrual_timestamp": "2015/10/31 00:00:00 +0900",
        "ap_payment_attachments_count": null,
        "buying_journal_dc_id": 9835,
        "code": null,
        "created_at": "2015/10/06 15:45:21 +0900",
        "dc": "c",
        "dept_code": "DEPT C",
        "id": 6621,
        "memo": "Office Supplies for Frank",
        "need_tax_deduction": null,
        "port_type": 1,
        "preset_withholding_tax_amount": null,
        "regist_user_code": "sample_user",
        "scheduled_memo": null,
        "scheduled_pay_timestamp": null,
        "update_user_code": null,
        "updated_at": "2015/10/06 15:45:21 +0900",
        "withholding_tax_base": null,
        "withholding_tax_segment": null,
        "account_code": "604",
        "price_including_tax": 5400,
        "price_excluding_tax": 5000,
        "sales_tax": 400,
        "tax_code": 0,
        "customer_master_code": 8201,
        "reason_master_code": "BUYING_IN"
    }, {
        "accrual_timestamp": "2015/10/31 00:00:00 +0900",
        "ap_payment_attachments_count": null,
        "buying_journal_dc_id": 9836,
        "code": null,
        "created_at": "2015/10/06 15:48:42 +0900",
        "dc": "c",
        "dept_code": "SETSURITSU",
        "id": 622,
        "memo": "Television for Cafeteria",
        "need_tax_deduction": null,
        "port_type": 1,
        "preset_withholding_tax_amount": null,
        "regist_user_code": "client_user",
        "scheduled_memo": null,
        "scheduled_pay_timestamp": null,
        "update_user_code": null,
        "updated_at": "2015/10/06 15:48:42 +0900",
        "withholding_tax_base": null,
        "withholding_tax_segment": null,
        "account_code": "604",
        "price_including_tax": 10800,
        "price_excluding_tax": 10000,
        "sales_tax": 800,
        "tax_code": 0,
        "customer_master_code": 101,
        "reason_master_code": "BUYING_IN"
    }
]
```

**/ap_payments/show/:id**

説明: このエンドポイントは単一の仕入・経費明細を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/ap_payments/show/:id
```

JSON レスポンスの例:
```
{
    "accrual_timestamp": "2015/10/31 00:00:00 +0900",
    "ap_payment_attachments_count": null,
    "buying_journal_dc_id": 9835,
    "code": null,
    "created_at": "2015/10/06 15:45:21 +0900",
    "dc": "c",
    "dept_code": "DEPT C",
    "id": 6621,
    "memo": "Office Supplies for Frank",
    "need_tax_deduction": null,
    "port_type": 1,
    "preset_withholding_tax_amount": null,
    "regist_user_code": "sample_user",
    "scheduled_memo": null,
    "scheduled_pay_timestamp": null,
    "update_user_code": null,
    "updated_at": "2015/10/06 15:45:21 +0900",
    "withholding_tax_base": null,
    "withholding_tax_segment": null,
    "account_code": "604",
    "price_including_tax": 5400,
    "price_excluding_tax": 5000,
    "sales_tax": 400,
    "tax_code": 0,
    "customer_master_code": 8201,
    "reason_master_code": "BUYING_IN"
}
```

**/ap_payments/create**

説明: 仕入・経費明細を新規作成します。作成に成功した場合、新規作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ap_payments/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`price_including_tax` | *required* | Integer | 発生額(税込)
`accrual_timestamp` | *required* | String | 発生日。  "YYYY-MM-DD" 形式
`customer_master_code` | *required* | String | 取引先コード
`reason_master_code` | *required* | String | 明細の原因コード。仕訳を作成するために使われます。
`dc` | *required* | String | 原因区分。 'd' は「減少」に、'c' は「増加」になります。
`memo` | *required* | String | メモ。値は空文字でも構いませんが必須項目です。
`port_type` | *required* | Integer | エリア区分。 1 は「国内」、 2 は「国外」
`tax_code` | *optional* | Integer | 税区分コード (省略された場合、原因コードのデフォルト税区分が使用されます。)
`dept_code` | *optional* | String | 部門コード
`sales_tax` | *optional* | Integer | 消費税額。省略された場合は自動で計算されます。
`scheduled_pay_timestamp` | *optional* | String | 支払予定日。  "YYYY-MM-DD" 形式
`scheduled_memo` | *optional* | String | 支払いに関する追加のメモ
`need_tax_deduction` | *optional* | Integer | 源泉徴収の対象とするか否か。 0:しない, 1:する
`preset_withholding_tax_amount` | *optional* | Integer | 事前設定源泉徴収額
`withholding_tax_base` | *optional* | Integer | 源泉徴収基準額。 1:消費税込額, 2:消費税抜額
`withholding_tax_segment` | *optional* | String | 源泉徴収区分コード (例: "nta2795"。 次のページを参照してください https://www.nta.go.jp/taxanswer/gensen/2795.htm)
`tag_list` | *optional* | String | セグメント(旧タグ)識別コード文字列(カンマ区切り)。
`tag_name_list` | *optional* | String | セグメント(旧タグ)名称文字列(カンマ区切り)。**このオプションはtag_listが存在しない場合にのみ有効です**
`usage_no` | *optional* | Integer | 明細の種類. value: 0: 通常明細, 1: 決算整理明細. **システム機能設定の決算整理機能が有効の場合にのみ使用できます。決算整理機能が無効の場合に1を指定するとエラーが返却されます。**
`data_partner` | *optional* | Object | 詳細は[外部連携機能](#外部連携機能)を参照。

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"price_including_tax": 5400, "accrual_timestamp": "2015-10-31", "customer_master_code": "8201", "dept_code": "DEPT C", "reason_master_code": "BUYING_IN", "dc": "c", "memo": "Office Supplies for Frank", "tax_code": 0, "port_type": 1 }' https://tsubaiso.net/ap_payments/create
```

**/ap_payments/update/:id**

説明: 仕入・経費明細を更新します。更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ap_payments/update/:id
```

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "更新  メモ", "price_including_tax": 5400 }'  https://tsubaiso.net/ap_payments/update/6621
```

**/ap/destroy/:id**

説明: 指定された id の仕入・経費明細を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ap/destroy/:id
```

**/ap_payments/balance/:year/:month**

説明: このエンドポイントは特定の年月の取引先の仕入・経費明細の残高一覧を返します。年月パラメータが指定されなかった場合、現在の月の明細が返されます。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/ap_payments/balance/:year/:month
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ap_payments/balance/2017/1
```

JSON レスポンス:
``` sh
[
  {
    "created_at": null,
    "customer_master_code": "individual",
    "finish_balance": 197149998,
    "memo": null,
    "ym": "2017-01",
    "ap_segment": 2,
    "start_balance": 197149998,
    "paid_amount": 0,
    "buying_amount": 0,
    "tax_buying_amount": 0,
    "remittance_charge": 0,
    "exchange_gain_and_loss": 0
  }
]
```

#### 取引先

**/customer_masters/list/**

説明: このエンドポイントは取引先の一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/customer_masters/list/
```

JSON レスポンスの例:
```
[
    {
        accountant_email: "accountant@test.co.jp"
        address: "東京都渋谷区幡ヶ谷2-6-5 6F"
        administrator_name: null
        ap_account_code: "325~999"
        ap_reason_selections: ""
        ar_account_code: "135~999"
        ar_reason_selections: ""
        bank_account_number: ""
        bank_branch_code: ""
        bank_branch_name: null
        bank_code: ""
        bank_course: 2
        bank_name: null
        bank_nominee: ""
        bill_detail_round_rule: 1
        code: "100"
        created_at: "2015/11/13 11:10:39 +0900"
        dept_code: "COMMON"
        email: null
        fax: null
        finish_timestamp: null
        foreign_currency: 0
        id: 100
        is_valid: 1
        locale: "ja-JP"
        name: "テスト株式会社"
        name_kana: "テストカブシキガイシャ"
        need_tax_deductions: 1
        pay_closing_schedule: "-1"
        pay_interface_id: null
        pay_sight: "-1m-1"
        receive_closing_schedule: "-1"
        receive_interface_id: null
        receive_sight: "1m20"
        regist_user_code: "user"
        sender_name: "reconciliation_keyword"
        sort_no: null
        start_timestamp: null
        tax_type_for_remittance_charge: 3
        tel: "03-1234-5678"
	memo: "memo1"
        update_user_code: "user"
        updated_at: "2015/11/13 11:14:00 +0900"
        used_in_ap: 1
        used_in_ar: 1
        withholding_tax_base: 1
        withholding_tax_segment: "nta2795"
        zip: "1510072"
    }, {
        accountant_email: null
        address: "東京都中野区幡ヶ谷1-7-20-101"
        administrator_name: null
        ap_account_code: "325~999"
        ap_reason_selections: null
        ar_account_code: "135~999"
        ar_reason_selections: null
        bank_account_number: null
        bank_branch_code: null
        bank_branch_name: null
        bank_code: null
        bank_course: null
        bank_name: null
        bank_nominee: null
        bill_detail_round_rule: 1
        code: "102"
        created_at: "2015/11/10 18:55:54 +0900"
        dept_code: null
        email: null
        fax: null
        finish_timestamp: null
        foreign_currency: 0
        id: 10002
        is_valid: 1
        locale: null
        name: "テスト株式会社102"
        name_kana: "テストカブシキガイシャ 2"
        need_tax_deductions: null
        pay_closing_schedule: null
        pay_interface_id: null
        pay_sight: null
        receive_closing_schedule: null
        receive_interface_id: null
        receive_sight: null
        regist_user_code: null
        sender_name: "テストカブシキガイシャ 2"
        sort_no: null
        start_timestamp: null
        tax_type_for_remittance_charge: 1
        tel: null
	memo: "memo2"
        update_user_code: null
        updated_at: "2015/11/13 18:55:54 +0900"
        used_in_ap: 1
        used_in_ar: 1
        withholding_tax_base: null
        withholding_tax_segment: null
        zip: null
    }
]
```

**/customer_masters/show/:id**

説明: このエンドポイントは単一の取引先を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/customer_masters/show/:id
```

JSON レスポンスの例:
```
{
    accountant_email: "accountant@test.co.jp"
    address: "東京都渋谷区幡ヶ谷2-6-5 6F"
    administrator_name: null
    ap_account_code: "325~999"
    ap_reason_selections: ""
    ar_account_code: "135~999"
    ar_reason_selections: ""
    bank_account_number: ""
    bank_branch_code: ""
    bank_branch_name: null
    bank_code: ""
    bank_course: 2
    bank_name: null
    bank_nominee: ""
    bill_detail_round_rule: 1
    code: "100"
    created_at: "2015/11/13 11:10:39 +0900"
    dept_code: "COMMON"
    email: null
    fax: null
    finish_timestamp: null
    foreign_currency: 0
    id: 100
    is_valid: 1
    locale: "ja-JP"
    name: "テスト株式会社"
    name_kana: "テストカブシキガイシャ"
    need_tax_deductions: 1
    pay_closing_schedule: "-1"
    pay_interface_id: null
    pay_sight: "-1m-1"
    receive_closing_schedule: "-1"
    receive_interface_id: null
    receive_sight: "1m20"
    regist_user_code: "user"
    sender_name: "reconciliation_keyword"
    sort_no: null
    start_timestamp: null
    tax_type_for_remittance_charge: 3
    tel: "03-1234-5678"
    memo: "memo1"
    update_user_code: "user"
    updated_at: "2015/11/13 11:14:00 +0900"
    used_in_ap: 1
    used_in_ar: 1
    withholding_tax_base: 1
    withholding_tax_segment: "nta2795"
    zip: "1510072"
}
```

**/customer_masters/show**

説明: このエンドポイントは単一の取引先を返します。

HTTP メソッド: GET

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`id` or `code` | *required* | String | ID か識別コード。

URL 構成例:
``` sh
https://tsubaiso.net/customer_masters/show
```

JSON レスポンスの例: customer_masters/show/:id を参照

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"code": "100" }' https://tsubaiso.net/customer_masters/show
```

**/customer_masters/create**

説明: 取引先を新規作成します。作成に成功した場合、新規作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/customer_masters/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`name` | *required* | String | 取引先名称(40文字以内)
`name_kana` | *required* | String | 取引先名称フリガナ(全角カタカナ40文字以内)
`code` | *required* | String | 識別コード(半角英数字及びハイフン10桁以内)
`zip` | *optional* | String | 郵便番号
`address` | *optional* | String | 住所(80文字以内)
`tel` | *optional* | String | 電話番号(半角数字,ハイフン,アスタリスク,シャープ 40文字以内)
`accountant_email` | *optional* | String | 経理担当者メールアドレス。請求書をツバイソからメール送信する場合に使います。
`dept_code` | *optional* | String | 部門
`tax_type_for_remittance_charge` | *required* | Integer | 消込時の銀行手数料の消費税区分。3は「共通売上分一般仕入（10%）」、0は「対象外又は非課税仕入」
`sender_name` | *optional* | String | 自動消込キーワード(40文字以内)
`locale` | *optional* | String | 請求書の言語 "ja-JP": 日本語、"en": 英語
`foreign_currency` | *optional* | Integer | 外貨取引の有無。0:無（外貨での請求書なし）、1: 「有（外貨での請求書あり）」
`used_in_ar` | *required* | Integer | 販売管理の債権区分。0: 使用しない、 1: 主に売掛金（売上代金の入金先）として使用する、2: 主に未収入金（売上代金以外の入金先）として使用する
`receive_closing_schedule` | *optional* | String | 請求締日。 "":未設定、"0": 即日、 1: 月初、5: 5日、10: 10日、15: 15日、18: 18日、20: 20日、25: 25日、-1: 月末
`receive_sight` | *optional* | String | 入金サイト。 "": 未設定、"0": 即日払、"1m5": 締月5日払、"1m8": 締月8日払、"1m10": 締月10日払、"1m12": 締月12日払、"1m15": 締月15日払、"1m18": 締月18日払、"1m20": 締月20日払、"1m22": 締月22日払、"1m25": 締月25日払、"1m26": 締月26日払、"1m27": 締月27日払、"1m28": 締月28日払、"1m-1": 締月末払、"2m5": 締月翌月5日払、"2m8": 締月翌月8日払、"2m10": 締月翌月10日払、"2m12": 締月翌月12日払、"2m15": 締月翌月15日払、"2m18": 締月翌月18日払、"2m20": 締月翌月20日払、"2m22": 締月翌月22日払、"2m25": 締月翌月25日払、"2m26": 締月翌月26日払、"2m27": 締月翌月27日払、"2m28": 締月翌月28日払、"2m-1": 締月翌月末払、"3m5": 締月翌々月5日払、"3m8": 締月翌々月8日払、"3m10": 締月翌々月10日払、"3m12": 締月翌々月12日払、"3m15": 締月翌々月15日払、"3m18": 締月翌々月18日払、"3m20": 締月翌々月20日払、"3m22": 締月翌々月22日払、"3m25": 締月翌々月25日払、"3m26": 締月翌々月26日払、"3m27": 締月翌々月27日払、"3m28": 締月翌々月28日払、"3m-1": 締月翌々月末払、"4m5": 締月翌々々月5日払、"4m8": 締月翌々々月8日払、"4m10": 締月翌々々月10日払、"4m12": 締月翌々々月12日払、"4m15": 締月翌々々月15日払、"4m18": 締月翌々々月18日払、"4m20": 締月翌々々月20日払、"4m22": 締月翌々々月22日払、"4m25": 締月翌々々月25日払、"4m26": 締月翌々々月26日払、"4m27": 締月翌々々月27日払、"4m28": 締月翌々々月28日払、"4m-1": 締月翌々々月末払、"-1m5": 締月前月5日払、"-1m8": 締月前月8日払、"-1m10": 締月前月10日払、"-1m12": 締月前月12日払、"-1m15": 締月前月15日払、"-1m18": 締月前月18日払、"-1m20": 締月前月20日払、"-1m22": 締月前月22日払、"-1m25": 締月前月25日払、"-1m26": 締月前月26日払、"-1m27": 締月前月27日払、"-1m28": 締月前月28日払、"-1m-1": 締月前月末払
`bill_detail_round_rule` | *optional* | Integer | 見積書・請求書の明細の端数処理設定。 1: 切り捨て、2: 切り上げ、3: 四捨五入
`used_in_ap` | *required* | Integer | 購買管理の債務区分。0: 使用しない、1: 主に未払金（仕入代金以外の支払い先）として使用する、2: 主に買掛金（仕入代金の支払い先）として使用する
`pay_closing_schedule` | *optional* | String | 支払締日。 "": 未設定、"0": 即日、 "1": 月初、"5": 5日、"10": 10日、"15": 15日、"18": 18日、"20": 20日、"25": 25日、"-1": 月末
`pay_sight` | *optional* | String | 支払サイト。"": 未設定、"0": 即日払、"1m5": 締月5日払、"1m8": 締月8日払、"1m10": 締月10日払、"1m12": 締月12日払、"1m15": 締月15日払、"1m18": 締月18日払、"1m20": 締月20日払、"1m22": 締月22日払、"1m25": 締月25日払、"1m26": 締月26日払、"1m27": 締月27日払、"1m28": 締月28日払、"1m-1": 締月末払、"2m5": 締月翌月5日払、"2m8": 締月翌月8日払、"2m10": 締月翌月10日払、"2m12": 締月翌月12日払、"2m15": 締月翌月15日払、"2m18": 締月翌月18日払、"2m20": 締月翌月20日払、"2m22": 締月翌月22日払、"2m25": 締月翌月25日払、"2m26": 締月翌月26日払、"2m27": 締月翌月27日払、"2m28": 締月翌月28日払、"2m-1": 締月翌月末払、"3m5": 締月翌々月5日払、"3m8": 締月翌々月8日払、"3m10": 締月翌々月10日払、"3m12": 締月翌々月12日払、"3m15": 締月翌々月15日払、"3m18": 締月翌々月18日払、"3m20": 締月翌々月20日払、"3m22": 締月翌々月22日払、"3m25": 締月翌々月25日払、"3m26": 締月翌々月26日払、"3m27": 締月翌々月27日払、"3m28": 締月翌々月28日払、"3m-1": 締月翌々月末払、"4m5": 締月翌々々月5日払、"4m8": 締月翌々々月8日払、"4m10": 締月翌々々月10日払、"4m12": 締月翌々々月12日払、"4m15": 締月翌々々月15日払、"4m18": 締月翌々々月18日払、"4m20": 締月翌々々月20日払、"4m22": 締月翌々々月22日払、"4m25": 締月翌々々月25日払、"4m26": 締月翌々々月26日払、"4m27": 締月翌々々月27日払、"4m28": 締月翌々々月28日払、"4m-1": 締月翌々々月末払、"-1m5": 締月前月5日払、"-1m8": 締月前月8日払、"-1m10": 締月前月10日払、"-1m12": 締月前月12日払、"-1m15": 締月前月15日払、"-1m18": 締月前月18日払、"-1m20": 締月前月20日払、"-1m22": 締月前月22日払、"-1m25": 締月前月25日払、"-1m26": 締月前月26日払、"-1m27": 締月前月27日払、"-1m28": 締月前月28日払、"-1m-1": 締月前月末払
`need_tax_deductions` | *optional* | String | 源泉徴収を行うか否か。 "0": 行わない、"1": 行う
`withholding_tax_segment` | *optional* | String | 源泉徴収区分。  例："nta2795"  値については、[国税庁の源泉徴収分類コード](https://www.nta.go.jp/taxanswer/gensen/gensen35.htm)を参照ください。
`withholding_tax_base` | *optional* | Integer | 源泉徴収基準額。1: 消費税込額、2: 消費税抜額
`bank_code` | *optional* | String | 取引先の銀行コード。(4桁)
`bank_branch_code` | *optional* | String | 取引先の支店コード(3桁)
`bank_course` | *optional* | Integer | 取引先の口座種別。 1: 普通、2: 当座、3: 定期預金(流動)、4: 定期預金(固定)、5: 定期積金（流動）、6: 定期積金（固定）
`bank_nominee` | *optional* | String | 取引先の口座名義（半角カナ30文字以内）
`bank_account_number` | *optional* | String | 取引先の口座番号
`is_valid` | *required* | Integer | 取引先ステータス。 1: 使用中、0: 使用停止
`memo` | *optional* | String | 自由記述欄

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{name: "テスト株式会社", name_kana: "テストカブシキガイシャ", code: "9000", tax_type_for_remittance_charge: "3", used_in_ar: 1, used_in_ap: 1, is_valid: 1 }' https://tsubaiso.net/customer_masters/create
```

**/customer_masters/update/:id**

説明: 取引先を更新します。 更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/customer_masters/update/:id
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"name": "アップデート株式会社", "name_kana": "アップデートカブシキガイシャ"}' https://tsubaiso.net/customer_masters/update/1
```

**/customer_masters/destroy/:id**

説明: 指定された id の取引先を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/customer_masters/destroy/:id
```

#### 社員

**/staffs/list/**

説明: 社員の一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/staffs/list/
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staffs/list
```

JSON レスポンスの例:
```
[
    {
        "ccode": XX,
        "code": "XXXXX",
        "created_at": "2015/12/07 16:48:10 +0900",
        "id": XXX,
        "regist_user_code": null,
        "status": 0,
        "update_user_code": null,
        "updated_at": "2015/12/07 16:48:10 +0900",
        "visibility": 0,
        "login": "XXXXX",
        "seimei": "税務 太郎",
        "seimei_furigana": "ゼイム タロウ"
    },
    {
        "ccode": XX,
        "code": "YYYYY",
        "created_at": "2015/12/07 16:48:10 +0900",
        "id": YYY,
        "regist_user_code": null,
        "status": 0,
        "update_user_code": null,
        "updated_at": "2015/12/07 16:48:10 +0900",
        "visibility": 0,
        "login": "YYYYY",
        "seimei": "",
        "seimei_furigana": ""
    }
]
```

**/staffs/show/**

説明: 単一の社員を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/staffs/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staffs/show/1
```

JSON レスポンスの例:
```
{
    "ccode": XX,
    "code": "XXXXX",
    "created_at": "2015/12/07 16:48:10 +0900",
    "id": 1,
    "regist_user_code": null,
    "status": 0,
    "update_user_code": null,
    "updated_at": "2015/12/07 16:48:10 +0900",
    "visibility": 0,
    "login": "XXXXX",
    "seimei": "税務 太郎",
    "seimei_furigana": "ゼイム タロウ"
}
```

#### 社員基本情報

**/staff_data/list/**

説明: このエンドポイントは社員情報の一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/staff_data/list?staff_id=:staff_id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" http://tsubaiso.net/staff_data/list/10000
```

JSON レスポンスの例:
```
[
    {
        "ccode": XX,
        "code": "BIRTH_YMD",
        "created_at": "2009/04/28 05:37:41 +0900",
        "finish_timestamp": "2019/12/31 00:00:00 +0900",
        "id": XX,
        "is_closed": 0,
        "is_ok": 0,
        "lock_version": 0,
        "memo": "",
        "regist_user_code": null,
        "staff_id": 10000,
        "start_timestamp": "2001/01/01 00:00:00 +0900",
        "update_user_code": null,
        "updated_at": "2009/04/28 06:40:51 +0900",
        "value": "1950/01/01"
    },
    ...
]
```

**/staff_data/show/**

説明: このエンドポイントは特定の社員情報のデータを返します。IDをURLに入れるか、社員情報IDとコードをリクエストボディに入れる必要があります。さらに、年月日を指定した場合、指定した年月日時点のデータが返されます。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/staff_data/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staff_data/show/1234
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"staff_id": 10000, "code": "QUALIFICATION"}'  https://tsubaiso.net/staff_data/show
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"staff_id": 10000, "code": "QUALIFICATION", "time": "2001-01-01"}'  https://tsubaiso.net/staff_data/show
```

JSON レスポンスの例:
```
{
    "ccode": XX,
    "code": "QUALIFICATION",
    "created_at": "2009/04/28 05:37:41 +0900",
    "finish_timestamp": "2019/12/31 00:00:00 +0900",
    "id": XX,
    "is_closed": 0,
    "is_ok": 0,
    "lock_version": 0,
    "memo": "",
    "regist_user_code": null,
    "staff_id": 10000,
    "start_timestamp": "2001/01/01 00:00:00 +0900",
    "update_user_code": null,
    "updated_at": "2009/04/28 06:40:51 +0900",
    "value": "TOEIC"
}
```

**/staff_data/create**

説明: 社員を新規作成します。作成に成功した場合、新規作成された明細が JSON として返されます。

HTTP メッソド: POST

URL 構成例:
```sh
https://tsubaiso.net/staff_data/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`staff_id` | *required* | Integer | ID
`code` | *required* | String | スタッフ情報コード
`value` | *required* | String | 値
`memo` | *optional* | String | メモ
`start_timestamp` | *required* | String | 開始年月日
`finish_timestamp` | *required or optional* | String | 利用終了年月日。もし利用終了区分が”0: 設定する”の場合、必須項目となります。
`no_finish_timestamp` | *required or optional* | String | 利用終了区分。　0: 設定する 1: 設定しない　利用終了年月日を設定しない場合、"1: 設定しない"が必須となります。

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -X POST -d '{"staff_id": 1000, "code": "NAME_MEI", "value": "Taro", "start_timestamp": "2015-12-15", "no_finish_timestamp": "1"}'  https://tsubaiso.net/staff_data/create
```

**/staff_data/update**

説明: 社員情報を更新します。更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/staff_data/update/:id
```

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -X POST -d '{"value": "1960-01-01"}'  http://tsubaiso.net/staff_data/update/1
```

**/staff_data/destroy/:id**

説明: 指定したIDの社員情報を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/staff_data/destroy/:id
```

#### 社員基本情報マスタ

**/staff_datum_masters/list/**

説明: このエンドポイントは社員情報マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/staff_datum_masters/list
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" http://tsubaiso.net/staff_datum_masters/list
```

JSON レスポンスの例:
```
[
    {
      "code": "BIRTH_YMD"
      "created_at": "2016/01/22 13:46:05 +0900"
      "default": null
      "editable_domains": null
      "id": 201083078
      "input_type": "ymd"
      "interval": 0
      "lock_version": 0
      "memo": null
      "multiple": 0
      "name": "生年月日"
      "regexp": "^[0-9]{4}[\-/][0-9]{2}[\-/][0-9]{2}$"
      "regexp_description": "xxxx/xx/xx"
      "regist_user_code": null
      "segment": "general"
      "tab": null
      "update_user_code": null
      "updated_at": "2016/01/22 13:46:05 +0900"
      "viewable_domains": null
    },
    ...
]
```

**/staff_datum_masters/show/**

説明: このエンドポイントは特定の社員情報マスタのデータを返します。IDをURLに入れるか、コードをリクエストボディに入れる必要があります。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/staff_datum_masters/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staff_datum_masters/show/1234
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"code": "BIRTH_YMD"}'  https://tsubaiso.net/staff_datum_masters/show
```

JSON レスポンスの例:
```
{
  "code": "BIRTH_YMD"
  "created_at": "2016/01/22 13:46:05 +0900"
  "default": null
  "editable_domains": null
  "id": 201083078
  "input_type": "ymd"
  "interval": 0
  "lock_version": 0
  "memo": null
  "multiple": 0
  "name": "生年月日"
  "regexp": "^[0-9]{4}[\-/][0-9]{2}[\-/][0-9]{2}$"
  "regexp_description": "xxxx/xx/xx"
  "regist_user_code": null
  "segment": "general"
  "tab": null
  "update_user_code": null
  "updated_at": "2016/01/22 13:46:05 +0900"
  "viewable_domains": null
}
```

#### 仕訳帳

**/journals/list**

説明: このエンドポイントは仕訳帳の検索結果を返します。デフォルトで返却されるレコード数は20です。

HTTPメソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/journals/list
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`id` | *optional* | String | idで検索します。
`price_min` | *optional* | Integer | 金額で検索します。税抜金額、または税額が指定した金額以上の科目を含む仕訳を検索します。
`price_max` | *optional* | Integer | 金額で検索します。税抜金額、または税額が指定した金額以下の科目を含む仕訳を検索します。
`memo` | *optional* | String | メモに書かれてある内容で部分検索します。
`dept_code` | *optional* | String | 部門コードで検索します。
`tag_list` | *optional* | String | セグメント(旧タグ)で検索します。
`start_date` | *optional* | String | 期間の開始日で検索します。形式は"YYYY-MM-DD"です。
`finish_date` | *optional* | String | 期間の終了日で検索します。形式は"YYYY-MM-DD"です。
`start_created_at` | *optional* | String | 登録日の開始日で検索します。形式は"YYYY-MM-DD"です。
`finish_created_at` | *optional* | String | 登録日の終了日で検索します。形式は"YYYY-MM-DD"です。
`account_codes` | *optional* | Array | 勘定科目コードで検索します。
`timestamp_order` | *optional* | String | 返却されるレコードの、日付の並びを指定します。"desc"を指定すると降順になり、"asc"を指定すると昇順になります。デフォルトは降順です。
`per_page` | *optional* | String | 1回の検索で取得するレコード数を設定します。デフォルトでは20件で、最大200までの設定が可能です。
`page` | *optional* | String | どのページの検索結果を取得するか設定します。デフォルトでは1ページ目です。

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXX" -X GET -d '{"account_codes": "500", "start_date": "2014-01-01", "finish_date": "2014-12-31", "per_page": 2}' https://tsubaiso.net/journals/list
```

JSON レスポンスの例:
```
{
    "records":[
        {
            "id":2707,
            "journal_timestamp":"2014/06/01 00:00:00 +0900",
            "journal_dcs":[
                {
                    "debit":{
                        "account_code":"135~0",
                        "tax_type":0,
                        "price_excluding_tax":2160,
                        "price_including_tax":2160,
                        "sales_tax":0
                    },
                    "credit":{
                        "account_code":"500",
                        "tax_type":1007,
                        "price_excluding_tax":1000,
                        "price_including_tax":1080,
                        "sales_tax":80
                    },
                "dept_code":null,
                "memo":""
                },
                {
                    "debit":{
                        "account_code":"",
                        "tax_type":0,
                        "price_excluding_tax":0,
                        "price_including_tax":0,
                        "sales_tax":0
                    },
                    "credit":{
                        "account_code":"500",
                        "tax_type":1007,
                        "price_excluding_tax":1000,
                        "price_including_tax":1080,
                        "sales_tax":80
                    },
                    "dept_code":null,
                    "memo":""
                }
            ]
        },
        {
            "id":2689,
            "journal_timestamp":"2014/04/30 00:00:00 +0900",
            "journal_dcs":[
                {
                    "debit":{
                        "account_code":"135~999",
                        "tax_type":0,
                        "price_excluding_tax":263500000,
                        "price_including_tax":263500000,
                        "sales_tax":0
                    },
                    "credit":{
                        "account_code":"500",
                        "tax_type":1007,
                        "price_excluding_tax":243981482,
                        "price_including_tax":263500000,
                        "sales_tax":19518518
                    },
                    "dept_code":null,
                    "memo":"4-2014 huge sale"
                }
            ]
        }
    ],
    "metadata":{
        "page":1,
        "per_page":2,
        "total_pages":5,
        "total_entries":10
    }
}
```

**/journals/show/:id**

説明: 1レコードの仕訳を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/journals/show/:id
```

JSON レスポンスの例:
```
{
    "records":{
        "id":2707,
        "journal_timestamp":"2014/06/01 00:00:00 +0900",
        "journal_dcs":[
            {
                "debit":{
                    "account_code":"135~0",
                    "tax_type":0,
                    "price_excluding_tax":2160,
                    "price_including_tax":2160,
                    "sales_tax":0
                },
                "credit":{
                    "account_code":"500",
                    "tax_type":1007,
                    "price_excluding_tax":1000,
                    "price_including_tax":1080,
                    "sales_tax":80
                },
                "dept_code":null,
                "memo":""
            },
            {
                "debit":{
                    "account_code":"",
                    "tax_type":0,
                    "price_excluding_tax":0,
                    "price_including_tax":0,
                    "sales_tax":0
                },
                "credit":{
                    "account_code":"500",
                    "tax_type":1007,
                    "price_excluding_tax":1000,
                    "price_including_tax":1080,
                    "sales_tax":80
                },
                "dept_code":null,
                "memo":""
            }
        ]
    }
}
```

#### マニュアル仕訳

**/manual_journals/list/:year/:month**

説明: このエンドポイントは特定の年月のマニュアル仕訳の一覧を返します。年月パラメータが指定されなかった場合、現在の月の仕訳が返されます。

```sh
https://tsubaiso.net/manual_journals/list/:year/:month
```

JSON レスポンスの例:
```
[
    {
        "id":29068,
        "journal_timestamp":"2016/04/01 00:00:00 0900",
        "journal_dcs": [
            {
                "debit":{
                    "account_code":"100",
                    "tax_type":0,
                    "price_including_tax":10000,
                    "sales_tax":0
                },
                "credit":{
                    "account_code":"130",
                    "tax_type":0,
                    "price_including_tax":10000,
                    "sales_tax":0
                },
                "dept_code":"COMMON",
                "memo":""
            }
        ]
    },
    {
        "id":29069,
        "journal_timestamp":"2016/04/01 00:00:00 0900",
        "journal_dcs": [
            {
                "debit":{
                    "account_code":"110",
                    "tax_type":0,
                    "price_including_tax":500000,
                    "sales_tax":0
                },
                "credit":{
                    "account_code":"330",
                    "tax_type":0,
                    "price_including_tax":1000000,
                    "sales_tax":0
                },
                "dept_code":"COMMON",
                "memo":""
            },
            {
                "debit":{
                    "account_code":"100",
                    "tax_type":0,
                    "price_including_tax":500000,
                    "sales_tax":0
                },
                "credit":{
                    "account_code":null,
                    "tax_type":null,
                    "price_including_tax":null,
                    "sales_tax":null
                },
                "dept_code":"COMMON",
                "memo":""
            }
        ]
    }
]
```

**/manual_journals/show/:id**

説明: このエンドポイントは単一のマニュアル仕訳を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/manual_journals/show/:id
```

JSON レスポンスの例:

```
{
    "id":29068,
    "journal_timestamp":"2016/04/01 00:00:00 0900",
    "journal_dcs": [
        {
            "debit":{
                "account_code":"100",
                "tax_type":0,
                "price_including_tax":10000,
                "sales_tax":0
            },
            "credit":{
                "account_code":"130",
                "tax_type":0,
                "price_including_tax":10000,
                "sales_tax":0
            },
            "dept_code":"COMMON",
            "memo":""
        }
    ]
}
```

**/manual_journals/create**

説明: マニュアル仕訳を新規作成します。作成に成功した場合、新規作成された仕訳が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/manual_journals/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`journal_timestamp` | *required* | String | 仕訳日。"YYYY-MM-DD" 形式
`data_partner` | *optional* | Object | 詳細は[外部連携機能](#外部連携機能)を参照。
`journal_dcs` | *required* | Array of Object | 仕訳の借方(debit)、貸方(credit)。jouranal_dcsは配列で指定します。(2つの場合でも要素は省略できません) 1つのjournal_dcにつき、"debit", "credit"を最大一つずつ指定できます。 *journal_dc 1つで借方、貸方の金額を合わせる必要はありません。*
`usage_no` | *optional* | Integer | 明細の種類. value: 0: 通常明細, 1: 決算整理明細. **システム機能設定の決算整理機能が有効の場合にのみ使用できます。決算整理機能が無効の場合に1を指定するとエラーが返却されます。**

*journal_dcs*

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`debit` | *optional* | Object | 借方情報。
`credit` | *optional* | Object | 貸方情報。
`dept_code` | *optional* | String | 部門コード。
`memo` | *optional* | String | メモ。
`tag_list` | *optional* | String | セグメント(旧タグ)識別コード文字列(カンマ区切り)。
`tag_name_list` | *optional* | String | セグメント(旧タグ)名称文字列(カンマ区切り)。**このオプションはtag_listが存在しない場合にのみ有効です**

*debit and credit*

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`account_code` | *required* | String | 勘定科目コード。
`tax_type` | *required* | Integer | 税区分。
`price_including_tax` | *required* | Integer | 税込金額。
`sales_tax` | *optional* | Integer | 税額。(入力が省略された場合は、税区分と税込金額から自動で計算します。)

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"journal_timestamp": "2016-04-01", "journal_dcs" : [{"debit" : {"account_code" : "110", "price_including_tax" : 100000, "tax_type" : 0}, "credit" : {"account_code" : "100", "price_including_tax" : 100000, "tax_type" : 0}}]}' https://tsubaiso.net/manual_journals/create
```

**/manual_journals/update/:id**

説明: 指定された id のマニュアル仕訳を更新します。更新に成功した場合、更新された仕訳が JSON として返されます。       　
  **journal_dcsを指定した場合はその内容で更新され、更新前のjournal_dcsは全て削除されます。**

HTTP メソッド: POST

URL 構成例:

```sh
https://tsubaiso.net/manual_journals/update/:id
```

リクエスト例:

```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"journal_timestamp": "2016-05-01"}' https://tsubaiso.net/manual_journals/update/29072
```

**/manual_journals/destroy/:id**

説明: 指定された id のマニュアル仕訳を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/manual_journals/destroy/:id
```

#### 旅費・経費精算

**/reimbursements/list/:year/:month**

説明: 旅費・経費精算 申請書一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/reimbursements/list/:year/:month
```

JSON レスポンスの例:
```
[
    {
        id: 212
        applicant: "ヤマカワ"
        applicant_staff_code: "EP0001"
        application_term: "2007-07-01 00:00:00"
        pay_date: "2020/10/01"
        owner_user_code: "clientuser"
        reimbursement_transactions_count: 0
        dept_code: "COMMON"
        memo: "Everythings is ok"
        journal: 0
        start_timestamp: "2007-07-01 00:00:00"
        finish_timestamp: "2007-07-31 00:00:00"
    }, {
        id: 213
        applicant: "タカシ"
        applicant_staff_code: "EP0002"
        application_term: "2008-02-01 00:00:00"
        pay_date: "2020/10/01"
        owner_user_code: "yamakawa"
        reimbursement_transactions_count: 1
        dept_code: "SETSURITSU"
        memo: ""
        journal: 0
        start_timestamp: "2008-01-01 00:00:00"
        finish_timestamp: "2008-01-31 00:00:00"
    }
]
```

**/reimbursements/show/:id**

説明: 単一の旅費・経費精算 申請書のデータを返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/reimbursements/show/:id
```

JSON レスポンスの例:
```
{
        id: 213
        applicant: "タカシ"
        applicant_staff_code: "EP0002"
        application_term: "2008-02-01 00:00:00"
        pay_date: "2020/10/01"
        owner_user_code: "yamakawa"
        reimbursement_transactions_count: 1
        dept_code: "SETSURITSU"
        memo: ""
        journal: 0
        start_timestamp: "2008-01-01 00:00:00"
        finish_timestamp: "2008-01-31 00:00:00"
}
```

**/reimbursements/create**

説明: 旅費・経費精算 申請書を新規作成します。作成に成功した場合は、新規作成された明細が JSON として返されます。

HTTP メッソド: POST

URL 構成例:
```sh
https://tsubaiso.net/reimbursements/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`application_term` | *required* | String | 申請書作成月。 "YYYY-MM-DD"形式
`applicant` | *required-optional* | String | 申請者名 (20字まで)。applicant_staff_codeが存在しない場合、申請者名は必須です。
`pay_date` | *optional* | String | 支払日。"YYYY-MM-DD"形式 OR "YYYY/MM/DD"形式
`applicant_staff_code` | *optional* | String | スタッフコード
`dept_code` | *optional* | String | 部門コード。デフォルトは"COMMON"。
`memo` | *optional* | String | メモ (30字まで)。
`transactions` | *optional* | Array[Object] | 旅費経費精算明細の配列。パラメータは reimbursement_transactions/create parameters を参照。 (reimbursement_id は不要です) (最大20)

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXX" -X POST -d '{"application_term": "2015-09-01", "applicant":"ナカムラ", "memo":"Everythings is okey", "applicant_staff_code":"EP2000", "pay_date": "2020-10-01" }' https://tsubaiso.net/reimbursements/create
```

**/reimbursements/update/:id**

説明: 旅費・経費精算 申請書を更新します。更新に成功した場合、更新された明細がJSONとして返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/reimbursements/update/:id
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"applicant": "アップデート株式会社", "term_application": "2016-10-01", "pay_date": "2020-10-01" }' https://tsubaiso.net/reimbursements/update/1
```

**/reimbursements/destroy/:id**

説明: 指定されたidの旅費・経費精算 申請書を削除します。 成功した場合、 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/reimbursements/destroy/:id
```

#### 旅費・経費精算明細

**/reimbursement_transactions/list/:reimbursement_id**

説明: このエンドポイントは特定の旅費・経費精算明細の一覧を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/reimbursement_transactions/list/:reimbursement_id
```

JSON レスポンスの例:
```
[
    {
        "id" : 12370123,
        "reimbursement_id": 123,
        "journal_dc_id": 321,
        "slip_no": "S230_0291",
        "reason_code": "SUPPLIES",
        "tag_list": "Education,Japan",
        "brief": "everything ok",
        "price_value": 400000,
        "tax_type": 0,
        "transaction_timestamp": "2016-01-01",
        "memo": "good",
        "port_type": 1,
        "dc": "c"
    }, {
        "id" : 123456789,
        "reimbursement_id": 333,
        "journal_dc_id": 444,
        "slip_no": "S230_1234",
        "reason_code": "SUPPLIES",
        "tag_list": "Education,Japan",
        "brief": "everything ok",
        "price_value": 10000,
        "tax_type": 10,
        "transaction_timestamp": "2016-02-01",
        "memo": "good",
        "port_type": 1,
        "dc": "d"
    },
    ...
]
```

**/reimbursement_transactions/show/:id**

説明: このエンドポイントは単一の旅費・経費精算明細を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/reimbursement_transactions/show/:id
```

JSON レスポンスの例:
```
{
    "id" : 123456789,
    "reimbursement_id": 123,
    "journal_dc_id": 321,
    "slip_no": "S230_0291",
    "reason_code": "SUPPLIES",
    "tag_list": "Education,Japan",
    "brief": "everything ok",
    "price_value": 400000,
    "tax_type": 0,
    "transaction_timestamp": "2016-01-01",
    "memo": "good",
    "port_type": 1,
    "dc": "c"
}
```

**/reimbursement_transactions/create**

説明: 旅費・経費精算明細を新規作成します。作成に成功した場合、新規作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/reimbursement_transactions/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reimbursement_id` | *required* | Integer | 対象となる旅費・経費精算のID
`transaction_timestamp` | *required* | String | 支払日。"YYYY-MM-DD"形式
`price_value` | *required* | Integer | 金額
`reason_code` | *required* | String | 原因
`port_type` | *optional* | Integer | エリア区分。1: 国内 2: 国外
`dc` | *optional* | String | 入出金区分。d: 入金 c: 出金。省略すると'c'となります。
`brief` | *optional* | String| 摘要
`memo` | *optional* | String | メモ
`tag_list` | *optional* | String | セグメント(旧タグ)識別コード文字列(カンマ区切り)
`tag_name_list` | *optional* | String | セグメント(旧タグ)名称文字列(カンマ区切り)。**このオプションはtag_listが存在しない場合にのみ有効です**
`tax_type` | *optional* | String | 課税区分コード
`data_partner` | *optional* | Object | 詳細は[外部連携機能](#外部連携機能)を参照。

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"reimbursement_id": 106, "transaction_timestamp": "2016-05-01", "price_value": 10000, "reason_code": "SUPPLIES", "port_type": 1, "dc": "c", "brief": "test brief", "memo": "test memo", "tag_list": "Education,Japan", "tax_type": "1003"}' https://tsubaiso.net/reimbursement_transactions/create
```

**/reimbursement_transactions/update/:id**

説明: 指定された id の旅費・経費精算明細を更新します。更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/reimbursement_transactions/update/:id
```

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "updated memo", "price_value": 10800 }'  https://tsubaiso.net/reimbursement_transactions/update/8833
```

**/reimbursement_transactions/destroy/:id**

説明: 指定された id の旅費・経費精算明細を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/reimbursement_transactions/destroy/:id
```

#### 部門管理

**/depts/list/**

説明: このエンドポイントは部門の一覧を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/depts/list
```

JSON レスポンスの例:
```
[
  {
    "ccode"      : 3 ,
    "code"       : "SETSURITSU" ,
    "sort_no"    : null,
    "color"      : "#f00" ,
    "finish_date": "2017/02/17" ,
    "memo"       : "" ,
    "name"       : "会社設立事業部" ,
    "name_abbr"  : "設立" ,
    "start_date" : "2016/02/17",
    "created_at" : "2016/02/17",
    "updated_at" : "2016/02/17",
    "regist_user_code" : "hiro",
    "update_user_code" : "fuji"
 } ,
  ...
]
```

**/depts/show/:id**

説明: このエンドポイントは特定の部門のデータを返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/depts/show/:id
```

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" http://tsubaiso.net/depts/show/1
```

JSON レスポンスの例:
```
{
 "ccode"      : 3 ,
 "code"       : "SETSURITSU" ,
 "sort_no"    : 1 ,
 "color"      : "#f00" ,
 "finish_date": "2017/02/17" ,
 "memo"       : "" ,
 "name"       : "会社設立事業部" ,
 "name_abbr"  : "設立" ,
 "start_date" : "2016/02/17",
 "created_at" : "2016/02/17",
 "updated_at" : "2016/02/17",
 "regist_user_code" : "hiro",
 "update_user_code" : "fuji"
}
```

**/depts/create**

説明: 部門を新規作成します。作成に成功した場合、新規作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/depts/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`code` | *required* | String | 部門コード *半角英数字及びハイフン、アンダーバー、ピリオド16文字以内
`sort_no` | *optional* | Integer | 並び順
`name` | *required* | String | 部門名 *32文字以内
`name_abbr` | *optional* | String | 部門名(略称) *16文字以内
`color` | *optional* | String | 識別色 (HTMLカラーコード形式)
`memo`| *optional* | String | メモ *40文字以内
`start_date` | *required* | String | 開始日 "YYYY/MM/DD"形式
`finish_date` | *optional* | String | 終了日 "YYYY/MM/DD"形式

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"code":"API_TEST", "name":"api_test","name_abbr": "AT", "start_date":"2016/04/01","color": "#ffffff"}' http://tsubaiso.net/depts/create
```

JSON レスポンスの例:
```
{
 "ccode"      : 3 ,
 "code"       : "API_TEST" ,
 "sort_no"    : 1 ,
 "color"      : "#ffffff" ,
 "finish_date": "" ,
 "memo"       : "" ,
 "name"       : "api_test" ,
 "name_abbr"  : "AT" ,
 "start_date" : "2016/04/01",
 "created_at" : "2016/02/17",
 "updated_at" : "2016/02/17",
 "regist_user_code" : "hiro",
 "update_user_code" : "fuji"
}
```

**/depts/update/:id**

説明: 部門を更新します。更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/depts/update/:id

```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"name": "アップデート"}' https://tsubaiso.net/depts/update/1
```

**/depts/destroy/:id**

説明: 指定された id の部門を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/customer_masters/depts/destroy/:id
```

#### セグメント(旧タグ)マスタ

**/tags/list**

説明: このエンドポイントはセグメント(旧タグ)の一覧を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/tags/list
```

JSON レスポンスの例:
```
{
 "DEFAULT": [
  {
   "id": 2,
   "ccode": 3,
   "code": "BANANA",
   "name": "Banana",
   "sort_no": 1,
   "tag_group_id": 1,
   "start_ymd": "2000-01-01T00:00:00.000+09:00",
   "finish_ymd": "2017-02-17T00:00:00.000+09:00",
   "lock_version": 0,
   "regist_user_code": null,
   "update_user_code": null,
   "created_at": "2016-09-29T17:58:48.000+09:00",
   "updated_at": "2016-09-29T17:58:48.000+09:00"
  },
  {
   "id": 3,
   "ccode": 3,
   "code": "CANADA",
   "name": "Canada",
   "sort_no": 2,
   "tag_group_id": 1,
   "start_ymd": "2000-01-01T00:00:00.000+09:00",
   "finish_ymd": "2017-02-17T00:00:00.000+09:00",
   "lock_version": 0,
   "regist_user_code": null,
   "update_user_code": null,
   "created_at": "2016-09-29T17:58:48.000+09:00",
   "updated_at": "2016-09-29T17:58:48.000+09:00"
  }
 ]
}
```

**/tags/show/:id**

説明: このエンドポイントはidで指定したセグメント(旧タグ)を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/tags/show/:id
```

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" http://tsubaiso.net/tags/show/1
```

JSON レスポンスの例:
```
{
 "id": 2,
 "ccode": 3,
 "code": "BANANA",
 "name": "Banana",
 "sort_no": 1,
 "tag_group_id": 1,
 "start_ymd": "2000-01-01T00:00:00.000+09:00",
 "finish_ymd": "2017-02-17T00:00:00.000+09:00",
 "lock_version": 0,
 "regist_user_code": null,
 "update_user_code": null,
 "created_at": "2016-08-04T15:08:45.000+09:00",
 "updated_at": "2016-08-04T15:08:45.000+09:00"
}
```

**/tags/create**

説明: セグメント(旧タグ)を作成します。作成に成功した場合、作成されたセグメント(旧タグ)がJSONとして返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/tags/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`code` | *required* | String | セグメント(旧タグ)識別コード。半角英数字及びハイフン、アンダーバー50文字以内
`name` | *required* | String | セグメント(旧タグ)名称。32文字以内
`sort_no` | *required* | Integer | 並び順
`tag_group_code` | *required* | String | セグメント(旧タグ)グループの識別コード
`start_ymd` | *required* | Datetime | 開始日。 "YYYY/MM/DD"形式
`finish_ymd` | *optional* | Datetime | 終了日。 "YYYY/MM/DD"形式

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXX" -X POST -d '{"code":"APITEST_code","name":"APITEST_name","sort_no":"919","tag_group_code":"PROJECT","start_ymd":"2016/08/18","finish_ymd":"2016/08/23"}' https://tsubaiso.net/tags/create
```

**/tags/update/:id**

説明: idで指定したセグメント(旧タグ)を更新します。更新に成功した場合は更新されたセグメント(旧タグ)がJSONとして返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/tags/update/:id
```

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXX" -X POST -d '{"code":"APITEST_code","name":"APITEST_update","sort_no":"919","tag_group_code":"PROJECT","start_ymd":"2016/08/18","finish_ymd":"2016/08/23"}' https://tsubaiso.net/tags/create
```

**/tags/destroy/:id**

説明: 設定されたidのセグメント(旧タグ)を削除します。 成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/tags/destroy/:id
```

#### 経費精算原因マスタ

**/reimbursement_reason_masters/list/**

説明: このエンドポイントは経費精算原因マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/reimbursement_reason_masters/list/
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/reimbursement_reason_masters/list
```

JSON レスポンスの例:
```
[
    {
        "ccode": XX,
        "id": "XXXXX",
        "sort_number": XX,
        "reason_code": XXX,
        "reason_name": "XXXX",
        "dc": 'c',
        "account_code": "XXX~XX",
        "port_type": X,
        "is_valid": X,
        "memo": "XXXXX",
	    "reimbursement_reason_taxes": [
	      {
	        "is_default":1,
            "port_type":1,
            "sales_tax_system":4,
            "sort_no":1,
            "tax_master_id":3
          }
      },
      {
        "ccode": XX,
        "id": "XXXXX",
        "sort_number": XX,
        "reason_code": XXX,
        "reason_name": "XXXX",
        "dc": 'c',
        "account_code": "XXX~XX",
        "port_type": X,
        "is_valid": X,
        "memo": "XXXXX",
	    "reimbursement_reason_taxes": [
	      {
	        "is_default":1,
            "port_type":1,
            "sales_tax_system":4,
            "sort_no":1,
            "tax_master_id":3
          }
      },
      .....
]
```

**/reimbursement_reason_masters/show/**

説明: このエンドポイントはidで指定した経費精算原因マスタを返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/reimbursement_reason_masters/show/:id
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/reimbursement_reason_masters/show/1
```

JSON レスポンスの例:
```
{
  "ccode": XX,
  "id": "XXXXX",
  "sort_number": XX,
  "reason_code": XXX,
  "reason_name": "XXXX",
  "dc": 'c',
  "account_code": "XXX~XX",
  "port_type": X,
  "is_valid": X,
  "memo": "XXXXX",
  "reimbursement_reason_taxes": [
    {
	  "is_default":1,
      "port_type":1,
      "sales_tax_system":4,
      "sort_no":1,
      "tax_master_id":3
    }
}
```

#### 販売原因マスタ

**/ar_reason_masters/list/**

説明: このエンドポイントは販売算原因マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:

```sh
https://tsubaiso.net/ar_reason_masters/list/
```

リクエスト例:

```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ar_reason_masters/list
```

JSON レスポンスの例:

```
[
  {
    "account_code": "500",
    "ccode": 3,
    "dc": "d",
    "id": 606428701,
    "is_valid": 1,
    "memo": "幡ヶ谷建設用",
    "reason_code": "SUPER_URIAGEDAKA",
    "reason_name": "(幡ヶ谷建設)売上高",
    "sort_number": 25,
    "ar_reason_taxes": [

    ]
  },
  {
    "account_code": "500",
    "ccode": 3,
    "dc": "d",
    "id": 339216794,
    "is_valid": 1,
    "memo": "MEMO",
    "reason_code": "SALES",
    "reason_name": "課税売上高",
    "sort_number": 30,
    "ar_reason_taxes": [
      {
        "is_default": 1,
        "sales_tax_system": 7,
        "sort_no": 10,
        "tax_master_id": 7
      },
      {
        "is_default": 1,
        "sales_tax_system": 7,
        "sort_no": 0,
        "tax_master_id": 1007
      }
    ]
  }
]
```

**/ar_reason_masters/show/**

説明: このエンドポイントはidで指定した販売原因マスタを返します。

HTTP メソッド: GET

URL 構成例:

```sh
https://tsubaiso.net/ar_reason_masters/show/:id
```

リクエスト例:

```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXX" http://tsubaiso.net/ar_reason_masters/show/1
```

JSON レスポンスの例:
```
{
   "account_code": "500",
   "ccode": null,
   "dc": "d",
   "id": 1,
   "is_valid": 1,
   "memo": "主たる営業活動である商品の販売やサービスの提供などにより獲得した収益に使用します。※税率5％の課税売上げ取引に使用してください。",
   "reason_code": "SALES",
   "reason_name": "課税売上高",
   "sort_number": 30,
   "ar_reason_taxes": [
      {
         "is_default": 1,
         "sales_tax_system": 7,
         "sort_no": 10,
         "tax_master_id": 7
      },
      {
         "is_default": 1,
         "sales_tax_system": 4,
         "sort_no": 0,
         "tax_master_id": 7
      },
      {
         "is_default": 1,
         "sales_tax_system": 7,
         "sort_no": 110,
         "tax_master_id": 1007
      },
      {
         "is_default": 1,
         "sales_tax_system": 4,
         "sort_no": 100,
         "tax_master_id": 1007
      }
   ]
}
```

#### 購買原因マスタ

**/ap_reason_masters/list**

説明: このエンドポイントはidで指定したの購買原因マスタ返します。

HTTP メソッド: GET

URL 構成例:

```sh
https://tsubaiso.net/ap_reason_masters/list
```

リクエストの例:

```sh
 curl -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXX" http:/tsubaiso.net/ap_reason_masters/list
```

JSON レスポンスの例:

```
[
 {
   "ap_reason_taxes": [],
   "updated_at": "2016/12/19 23:13:11 +0900",
   "update_user_code": "yamakawe",
   "is_valid": 1,
   "id": 1044389493,
   "expense_mode": 1,
   "dc": "c",
   "created_at": "2016/12/07 17:24:53 +0900",
   "ccode": 3,
   "allowed_domains": null,
   "account_code": "604",
   "memo": "",
   "port_type": 1,
   "reason_code": "BUYING_IN2",
   "reason_name": "仕入高",
   "regist_user_code": null,
   "release_version": 0,
   "sales_tax_system": 7,
   "sort_number": 10
 },
 {
   "ap_reason_taxes": [
     {
       "updated_at": "2016/12/07 17:24:39 +0900",
       "tax_master_id": 0,
       "sort_no": 0,
       "ap_reason_master_id": 10002,
       "created_at": "2016/12/07 17:24:39 +0900",
       "description": "test1",
       "id": 41,
       "is_default": 0,
       "personal_id": null,
       "port_type": null,
       "sales_tax_system": 0
     }
   ],
   "updated_at": "2016/12/07 17:24:53 +0900",
   "update_user_code": null,
   "is_valid": 1,
   "id": 10002,
   "expense_mode": 0,
   "dc": "c",
   "created_at": "2016/12/07 17:24:53 +0900",
   "ccode": 3,
   "allowed_domains": [
     "CL_ADMIN",
     "CL_BANK_MGR",
     "CL_REIM_MGR",
     "CL_CASH_MGR",
     "CL_SALES_MGR",
     "CL_BUYING_MGR",
     "CL_INVENTORY_MGR",
     "CL_MOVEMENT_MGR",
     "CL_POTPOURRI_MGR",
     "CL_JOURNAL_MGR",
     "CL_DIRECT_DEBIT_MASTER_MGR",
     "CL_DIRECT_DEBIT_MGR",
     "CL_HR_MGR",
     "CL_PAYROLL_MGR",
     "BW_ADMIN",
     "BW_STAFF"
   ],
   "account_code": "604",
   "memo": "幡ヶ谷建設・マネージャのみ\n",
   "port_type": null,
   "reason_code": "SUPER_BUYING_IN",
   "reason_name": "(幡ヶ谷建設・マネージャのみ)仕入高",
   "regist_user_code": null,
   "release_version": 0,
   "sales_tax_system": null,
   "sort_number": 15
 }
]
```

**/ap_reason_masters/show/:id**

説明: このエンドポイントはidで指定したの購買原因マスタを返します。

HTTP メソッド: GET

URL 構成例:

``` sh
https://tsubaiso.net/ap_reason_masters/show/:id
```

リクエストの例:

```sh
 curl -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXX" http:/tsubaiso.net/ap_reason_masters/show/1
```

JSON レスポンスの例:

```
{
 "ap_reason_taxes": [
   {
     "updated_at": "2016/12/07 17:24:39 +0900",
     "tax_master_id": 2,
     "sort_no": 20,
     "ap_reason_master_id": 1,
     "created_at": "2016/12/07 17:24:39 +0900",
     "description": null,
     "id": 1,
     "is_default": 0,
     "personal_id": null,
     "port_type": 1,
     "sales_tax_system": 7
   }
 ],
 "updated_at": "2016/12/07 17:24:53 +0900",
 "update_user_code": null,
 "is_valid": 1,
 "id": 1,
 "expense_mode": 0,
 "dc": "c",
 "created_at": "2016/12/07 17:24:53 +0900",
 "ccode": null,
 "allowed_domains": null,
 "account_code": "604",
  "memo":"仕入高とは、商品や原材料の仕入、外注費等、会社の主たる営業活動にかかわる費用をいいます。売上高と直接対応する費用である点が特徴です。",
 "port_type": null,
 "reason_code": "BUYING_IN",
 "reason_name": "仕入高",
 "regist_user_code": null,
 "release_version": 0,
 "sales_tax_system": null,
 "sort_number": 10
}
```

#### 銀行原因マスタ

**/bank_reason_masters/list**

説明: 銀行原因マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/bank_reason_masters/list/
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_reason_masters/list/
```

JSON レスポンスの例:
```
[
  {
    "id": 728025315,
    "ccode": 3,
    "sort_number": 0,
    "reason_code": "keywords",
    "reason_name": "keywords",
    "dc": "d",
    "account_code": "1",
    "is_valid": 1,
    "memo": "",
    "created_at": "2019/06/20 18:07:39 +0900",
    "regist_user_code": "yamakawa",
    "updated_at": "2019/06/20 18:07:39 +0900",
    "update_user_code": null,
    "keywords": [
      {
        "text": "world"
      },
      {
        "text": "hello"
      }
    ],
    "bank_reason_taxes": [
      {
        "id": 1059893882,
        "personal_id": null,
        "tax_master_id": 17,
        "description": null,
        "is_default": 0,
        "sort_no": 2,
        "created_at": "2019/08/05 16:32:57 +0900",
        "updated_at": "2019/08/05 16:32:57 +0900",
        "is_default_view": "デフォルトでない",
        "tax_master": {
          "id": 17,
          "code": 17,
          "name": "有価証券の譲渡5%分",
          "abbr_name": "有",
          "description": "5%分の有価証券の譲渡に伴う減少有価証券及び売却損益に使用します。",
          "tax_ratio_division": 0,
          "taxable_division": 0,
          "dc": "c",
          "controll_business_segment": 0,
          "color": "7fff00",
          "status": 100,
          "start_timestamp": "1997/04/01 00:00:00 +0900",
          "finish_timestamp": "2014/03/31 23:59:59 +0900",
          "created_at": "2008/09/29 17:22:11 +0900",
          "updated_at": "2008/09/29 17:22:11 +0900"
        }
      },
      {
        "id": 1059893883,
        "personal_id": null,
        "tax_master_id": 1007,
        "description": null,
        "is_default": 0,
        "sort_no": 2,
        "created_at": "2019/08/05 16:33:04 +0900",
        "updated_at": "2019/08/05 16:33:04 +0900",
        "is_default_view": "デフォルトでない",
        "tax_master": {
          "id": 1007,
          "code": 1007,
          "name": "一般売上(8%)",
          "abbr_name": "売8",
          "description": "税率8％の課税売上げ取引に使用します。",
          "tax_ratio_division": 8,
          "taxable_division": 2,
          "dc": "c",
          "controll_business_segment": 1,
          "color": "ff7f00",
          "status": 100,
          "start_timestamp": "2014/04/01 00:00:00 +0900",
          "finish_timestamp": "2019/09/30 23:59:59 +0900",
          "created_at": "2014/02/21 00:00:00 +0900",
          "updated_at": "2014/02/21 00:00:00 +0900"
        }
      }
    ]
  }
]
```

**/bank_reason_masters/show/:id**

説明: 1レコードの銀行原因マスタを返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/bank_reason_masters/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_reason_masters/show/:id
```

JSON レスポンスの例:
```
{
  "id": 100,
  "ccode": 3,
  "sort_number": 0,
  "reason_code": "xxxx_123",
  "reason_name": "テスト用",
  "dc": "d",
  "account_code": "999~999",
  "is_valid": 1,
  "memo": "テストメモ",
  "created_at": "2019/02/25 11:47:36 +0900",
  "regist_user_code": null,
  "updated_at": "2019/02/25 11:47:36 +0900",
  "update_user_code": null,
  "keywords": [
    {
      "text": "JR"
    },
    {
      "text": "keyword"
    }
  ],
  "bank_reason_taxes": [
    {
      "id": 1059893877,
      "personal_id": null,
      "tax_master_id": 6,
      "description": null,
      "is_default": 0,
      "sort_no": 1,
      "created_at": "2019/06/20 17:39:06 +0900",
      "updated_at": "2019/06/20 17:39:06 +0900",
      "is_default_view": "デフォルトでない",
      "tax_master": {
        "id": 6,
        "code": 6,
        "name": "共通売上分新車購入",
        "abbr_name": "共",
        "description": "税率4.5％の課税仕入れ（新車購入）のうち、課税売上げ・非課税売上げに共通する取引に使用します。",
        "tax_ratio_division": 2,
        "taxable_division": 1,
        "dc": "d",
        "controll_business_segment": 0,
        "color": "7fffff",
        "status": 200,
        "start_timestamp": null,
        "finish_timestamp": null,
        "created_at": "2008/09/29 17:22:11 +0900",
        "updated_at": "2008/09/29 17:22:11 +0900"
      }
    },
    {
      "id": 1059893878,
      "personal_id": null,
      "tax_master_id": 6,
      "description": null,
      "is_default": 1,
      "sort_no": 1,
      "created_at": "2019/06/20 17:59:08 +0900",
      "updated_at": "2019/06/20 17:59:08 +0900",
      "is_default_view": "デフォルト",
      "tax_master": {
        "id": 6,
        "code": 6,
        "name": "共通売上分新車購入",
        "abbr_name": "共",
        "description": "税率4.5％の課税仕入れ（新車購入）のうち、課税売上げ・非課税売上げに共通する取引に使用します。",
        "tax_ratio_division": 2,
        "taxable_division": 1,
        "dc": "d",
        "controll_business_segment": 0,
        "color": "7fffff",
        "status": 200,
        "start_timestamp": null,
        "finish_timestamp": null,
        "created_at": "2008/09/29 17:22:11 +0900",
        "updated_at": "2008/09/29 17:22:11 +0900"
      }
    }
  ]
}
```

**/bank_reason_masters/create**

説明: 銀行原因マスタを新規作成します。作成に成功した場合、新規作成された銀行原因マスタが JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/bank_reason_masters/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`sort_number` | *optional* | String | 並び順
`reason_code` | *required* | String | 原因コード。半角英数字及びアンダーバー60文字以内。
`reason_name` | *required* | String | 原因名
`dc` | *required* | String | 入出金区分　'd' は debit の意で「増加」に、'c' は credit の意で「減少」になります。
`is_valid` | *required* | Integer | 表示区分 2:マネージャのみ　1: 表示、0: 非表示
`memo` | *optional* | String | メモ
`account_code` | *required* | String | 勘定科目コード

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token:XXXXXXXXXXXXXXXXXX" -d '{ "sort_number" : "0" , "reason_code" : "Test_Reason_code",  "reason_name" : "Tsubaiso_reason" , "dc":"c" , "is_valid": 1, "account_code": "1"}' https://tsubaiso.net/bank_reason_masters/create
```

**/bank_reason_masters/update/:id**

説明: 1レコードの銀行原因マスタを更新します。更新に成功した場合、更新された銀行原因マスタがJSONとして返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/bank_reason_masters/update/:id
```

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token:XXXXXXXXXXXXXXXXXX" -d '{ "reason_name" : "Tsubaiso Bank Reason Update" ,  "memo" : "Updated_reason_memo"}' https://tsubaiso.net/bank_account_masters/update/:id
```

**/bank_account_masters/destroy/:id**

説明：指定されたidの銀行原因マスタを削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
``` sh
https://tsubaiso.net/bank_reason_masters/destroy/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST https://tsubaiso.net/bank_account_masters/destroy/:id
```

#### 現金原因マスタ

**/petty_cash_reason_masters/list**

説明: 現金原因マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/petty_cash_reason_masters/list/
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET https://tsubaiso.net/petty_cash_reason_masters/list/
```

JSON レスポンスの例:
```
[
  {
    "id": 19992,
    "ccode": 3,
    "sort_number": 20,
    "reason_code": "SHOUMOUHINHI",
    "reason_name": "消耗品費",
    "dc": "c",
    "account_code": "710",
    "is_valid": 1,
    "memo": "消耗品費とは、事務用消耗品や消耗工具器具備品などの購入費用をいいます。事務用消耗品はボールペン、ノートなど事務作業で使用するもので、１回で使い切ってしまうものや長期間繰り返し使用できないものなどです。消耗工具器具備品は事務用机やイス、本棚などで耐用年数が１年未満のものや取得価額が１０万円未満の少額のものなどです。 ",
    "regist_user_code": null,
    "update_user_code": null,
    "created_at": "2019/02/25 11:47:31 +0900",
    "updated_at": "2019/02/25 11:47:31 +0900",
    "port_type": null
  },
  {
    "id": 1000300270,
    "ccode": 3,
    "sort_number": 270,
    "reason_code": "BANK2CASH_",
    "reason_name": "銀行から小口現金への入金",
    "dc": "d",
    "account_code": "190~30",
    "is_valid": 1,
    "memo": "銀行口座から引き出して小口現金に入金した時に選択してください。",
    "regist_user_code": null,
    "update_user_code": null,
    "created_at": "2019/01/10 17:32:06 +0900",
    "updated_at": "2019/01/10 17:32:06 +0900",
    "port_type": 1,
    "petty_cash_reason_taxes": [
      {
        "tax_master_id": 1003,
        "sales_tax_system": 4,
        "port_type": 1,
        "is_default": 1,
        "sort_no": 101
      },
      {
        "tax_master_id": 1007,
        "sales_tax_system": 7,
        "port_type": 2,
        "is_default": 0,
        "sort_no": 105
      }
    ]
  }
]
```

**/petty_cash_reason_masters/show/:id**

説明: 個別の現金原因マスタを返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/petty_cash_reason_masters/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET https://tsubaiso.net/petty_cash_reason_masters/show/:id
```

JSON レスポンスの例:
```
{
  "id": 19992,
  "ccode": 3,
  "sort_number": 20,
  "reason_code": "SHOUMOUHINHI",
  "reason_name": "消耗品費",
  "dc": "c",
  "account_code": "710",
  "is_valid": 1,
  "memo": "消耗品費とは、事務用消耗品や消耗工具器具備品などの購入費用をいいます。事務用消耗品はボールペン、ノートなど事務作業で使用するもので、１回で使い切ってしまうものや長期間繰り返し使用できないものなどです。消耗工具器具備品は事務用机やイス、本棚などで耐用年数が１年未満のものや取得価額が１０万円未満の少額のものなどです。 ",
  "regist_user_code": null,
  "update_user_code": null,
  "created_at": "2019/02/25 11:47:31 +0900",
  "updated_at": "2019/02/25 11:47:31 +0900",
  "port_type": null
}
```
**/petty_cash_reason_masters/create**

説明: 現金原因マスタを作成します。作成に成功した場合、作成された現金原因マスタがJSONとして返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/petty_cash_reason_masters/create/
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`sort_no` | *optional* | Integer | 並び順
`reason_code` | *required* | String | 原因コード。半角英数字及びアンダーバー60文字以内。
`reason_name` | *required* | String | 原因名
`dc` | *required* | Text | 入出金区分　d: 入金 c: 出金
`account_code` | *required* | text | 勘定科目コード
`port_type` | *required* | Integer | エリア区分。 1 は「国内」、 2 は「国外」、３は「国内・国外」
`is_valid` | *required* | Integer | 表示区分 1: 表示、0: 非表示
`memo` | *optional* | Strings | 説明

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXX" -X POST -d '{"petty_cash_reason_master" : { "reason_code" : "tsubaiso test" , "reason_name" : "reason name" , "dc":"d", "account_code":"100", "is_valid":"1" , "memo":"This is Test from API.", "port_type" : "0"}}' https://tsubaiso.net/petty_cash_reason_masters/create/
```

**/petty_cash_reason_masters/update**

説明: 現金原因マスタを更新します。更新に成功した場合、更新された現金原因マスタがJSONとして返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/petty_cash_reason_masters/update/:id
```

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token:  XXXXXXXXXXXXXXX" -X POST -d '{"memo":"updating memo", "reason_code":"updating_code"}' https://tsubaiso.net/petty_cash_reason_masters/update/:id
```

**/petty_cash_reason_masters/destroy**

説明: 現金原因マスタを削除します。削除に成功した場合、204 No Content が返ります。

Method: POST

URL 構成例:
```sh
https://tsubaiso.net/petty_cash_reason_masters/destroy/:id
```

#### 賞与データ

**/bonuses/list/**

説明: このエンドポイントは bonus_no (賞与の回数番号)と target_year (対象年)で指定された賞与データの一覧を返します。もしこれらのパラメータが与えられなかったら、エラーが発生します。

HTTP メソッド: GET

URL 構成例:

```sh
https://tsubaiso.net/bonuses/list/
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`bonus_no` | *required* | String | 賞与の回数番号
`target_year` | *required* | String | 対象年

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"bonus_no": 1, "target_year": "2016" }' https://tsubaiso.net/bonuses/list/
```

JSON レスポンス例:
```
[
  {
    "bal_1": 740000,
    "bal_10": nil,
    "bal_11": nil,
    "bal_12": nil,
    "bal_13": nil,
    "bal_14": nil,
    "bal_15": nil,
    "bal_16": nil,
    "bal_17": 0,
    "bal_18": nil,
    "bal_19": nil,
    "bal_2": nil,
    "bal_20": nil,
    "bal_21": 740000,
    "bal_22": 740000,
    "bal_23": 740000,
    "bal_24": nil,
    "bal_25": nil,
    "bal_26": nil,
    "bal_27": nil,
    "bal_28": nil,
    "bal_29": nil,
    "bal_3": nil,
    "bal_30": 740000,
    "bal_4": nil,
    "bal_5": nil,
    "bal_6": nil,
    "bal_7": nil,
    "bal_8": nil,
    "bal_9": nil,
    "balance_amount": nil,
    "bde_1": 30340,
    "bde_10": nil,
    "bde_11": nil,
    "bde_12": nil,
    "bde_13": nil,
    "bde_14": nil,
    "bde_15": nil,
    "bde_16": nil,
    "bde_17": nil,
    "bde_18": 0,
    "bde_19": 0,
    "bde_2": 0,
    "bde_20": 0,
    "bde_21": 0,
    "bde_22": nil,
    "bde_23": nil,
    "bde_24": 88097,
    "bde_25": 6660,
    "bde_26": nil,
    "bde_27": nil,
    "bde_28": 648425,
    "bde_29": nil,
    "bde_3": 56795,
    "bde_30": 149910,
    "bde_4": 15473,
    "bde_5": 4440,
    "bde_6": 51874,
    "bde_7": nil,
    "bde_8": -1238,
    "bde_9": nil,
    "ccode": 3,
    "created_at": "2016-12-01 11:40:12 +0900",
    "group_code": nil,
    "id": 650504937,
    "is_closed": 0,
    "is_ok": 0,
    "memo": nil,
    "no": 1,
    "regist_user_code": nil,
    "staff_id": 2001,
    "target_year": 2016,
    "update_user_code": nil,
    "updated_at": "2016-12-01 11:40:12 +0900"
  },
  {
    "bal_1": 740000,
    "bal_10": nil,
    "bal_11": nil,
    "bal_12": nil,
    "bal_13": nil,
    "bal_14": nil,
    "bal_15": nil,
    "bal_16": nil,
    "bal_17": 0,
    "bal_18": nil,
    "bal_19": nil,
    "bal_2": nil,
    "bal_20": nil,
    "bal_21": 740000,
    "bal_22": 740000,
    "bal_23": 740000,
    "bal_24": nil,
    "bal_25": nil,
    "bal_26": nil,
    "bal_27": nil,
    "bal_28": nil,
    "bal_29": nil,
    "bal_3": nil,
    "bal_30": 740000,
    "bal_4": nil,
    "bal_5": nil,
    "bal_6": nil,
    "bal_7": nil,
    "bal_8": nil,
    "bal_9": nil,
    "balance_amount": nil,
    "bde_1": 30340,
    "bde_10": nil,
    "bde_11": nil,
    "bde_12": nil,
    "bde_13": nil,
    "bde_14": nil,
    "bde_15": nil,
    "bde_16": nil,
    "bde_17": nil,
    "bde_18": 0,
    "bde_19": 0,
    "bde_2": 0,
    "bde_20": 0,
    "bde_21": 0,
    "bde_22": nil,
    "bde_23": nil,
    "bde_24": 88097,
    "bde_25": 6660,
    "bde_26": nil,
    "bde_27": nil,
    "bde_28": 648425,
    "bde_29": nil,
    "bde_3": 56795,
    "bde_30": 149910,
    "bde_4": 15473,
    "bde_5": 4440,
    "bde_6": 51874,
    "bde_7": nil,
    "bde_8": -1238,
    "bde_9": nil,
    "ccode": 3,
    "created_at": "2016-12-01 11:40:12 +0900",
    "group_code": nil,
    "id": 927467588,
    "is_closed": 0,
    "is_ok": 0,
    "memo": nil,
    "no": 1,
    "regist_user_code": nil,
    "staff_id": 2000,
    "target_year": 2016,
    "update_user_code": nil,
    "updated_at": "2016-12-01 11:40:12 +0900"
  }
]

```

**/bonuses/show/**

説明: このエンドポイントはidで指定した賞与データを返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/bonuses/show/:id
```

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bonuses/show/650504937
```

JSON レスポンスの例:
```
{
  "bal_1": 740000,
  "bal_10": nil,
  "bal_11": nil,
  "bal_12": nil,
  "bal_13": nil,
  "bal_14": nil,
  "bal_15": nil,
  "bal_16": nil,
  "bal_17": 0,
  "bal_18": nil,
  "bal_19": nil,
  "bal_2": nil,
  "bal_20": nil,
  "bal_21": 740000,
  "bal_22": 740000,
  "bal_23": 740000,
  "bal_24": nil,
  "bal_25": nil,
  "bal_26": nil,
  "bal_27": nil,
  "bal_28": nil,
  "bal_29": nil,
  "bal_3": nil,
  "bal_30": 740000,
  "bal_4": nil,
  "bal_5": nil,
  "bal_6": nil,
  "bal_7": nil,
  "bal_8": nil,
  "bal_9": nil,
  "balance_amount": nil,
  "bde_1": 30340,
  "bde_10": nil,
  "bde_11": nil,
  "bde_12": nil,
  "bde_13": nil,
  "bde_14": nil,
  "bde_15": nil,
  "bde_16": nil,
  "bde_17": nil,
  "bde_18": 0,
  "bde_19": 0,
  "bde_2": 0,
  "bde_20": 0,
  "bde_21": 0,
  "bde_22": nil,
  "bde_23": nil,
  "bde_24": 88097,
  "bde_25": 6660,
  "bde_26": nil,
  "bde_27": nil,
  "bde_28": 648425,
  "bde_29": nil,
  "bde_3": 56795,
  "bde_30": 149910,
  "bde_4": 15473,
  "bde_5": 4440,
  "bde_6": 51874,
  "bde_7": nil,
  "bde_8": -1238,
  "bde_9": nil,
  "ccode": 3,
  "created_at": "2016-12-01 11:40:12 +0900",
  "group_code": nil,
  "id": 650504937,
  "is_closed": 0,
  "is_ok": 0,
  "memo": nil,
  "no": 1,
  "regist_user_code": nil,
  "staff_id": 2001,
  "target_year": 2016,
  "update_user_code": nil,
  "updated_at": "2016-12-01 11:40:12 +0900"
}
```

#### 給与データ

**/payrolls/list/**

説明: このエンドポイントは給与データの一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/payrolls/list/:year/:month
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/payrolls/list/2016/11
```

JSON レスポンス例:
```
[
    {
        "id": 1071569834,
        "ccode": 3,
        "staff_id": 2001,
	    "group_code: nil,
	    "target_ym": "2016-11-01 00:00:00",
	    "al_1": 440000,
	    "al_2": 0,
	    "al_3": 0,
	    "al_4": nil,
	    "al_5": nil,
	    "al_6": nil,
	    "al_7": nil,
	    "al_8": nil,
	    "al_9": nil,
	    "al_10": nil,
	    "al_11": nil,
	    "al_12": nil,
	    "al_13": nil,
	    "al_14": nil,
	    "al_15": nil,
	    "al_16": nil,
	    "al_17": nil,
	    "al_18": nil,
	    "al_19": 0,
	    "al_20": 0,
	    "al_21": 440000,
	    "al_22": 440000,
	    "al_23": 440000,
	    "al_24": nil,
	    "al_25": nil,
	    "al_26": nil,
	    "al_27": nil,
	    "al_28": nil,
	    "al_29": nil,
	    "al_30": 440000,
	    "de_1": 18040,
	    "de_2": 0,
	    "de_3": 33770,
	    "de_4": nil,
	    "de_5": 2640,
	    "de_6": 6800,
	    "de_7": 5200,
	    "de_8": nil,
	    "de_9": nil,
	    "de_10": nil,
	    "de_11": nil,
	    "de_12": nil,
	    "de_13": nil,
	    "de_14": nil,
	    "de_15": 0,
	    "de_16": nil,
	    "de_17": nil,
	    "de_18": 0,
	    "de_19": 0,
	    "de_20": 0,
	    "de_21": 0,
	    "de_22": nil,
	    "de_23": nil,
	    "de_24": nil,
	    "de_25": 3960,
	    "de_26": 0,
	    "de_27": nil,
	    "de_28": 385550,
	    "de_29": nil,
	    "de_30": 66450,
	    "balance_amount": nil,
	    "log": nil,
	    "is_ok": 0,
	    "is_closed": 0,
	    "regist_user_code": nil,
	    "update_user_code": nil,
	    "calculated_at": nil,
	    "calc_user_code": nil,
	    "created_at": "2016-12-19 14:16:28",
	    "updated_at": "2016-12-19 14:16:28"
      },
      .....
]
```

**/payrolls/show/**

説明: このエンドポイントはidで指定した給与データを返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/payrolls/show/:id
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/payrolls/show/1071569834
```

JSON レスポンスの例:
```
     {
        "id": 1071569834,
        "ccode": 3,
        "staff_id": 2001,
	    "group_code: nil,
	    "target_ym": "2016-11-01 00:00:00",
	    "al_1": 440000,
	    "al_2": 0,
	    "al_3": 0,
	    "al_4": nil,
	    "al_5": nil,
	    "al_6": nil,
	    "al_7": nil,
	    "al_8": nil,
	    "al_9": nil,
	    "al_10": nil,
	    "al_11": nil,
	    "al_12": nil,
	    "al_13": nil,
	    "al_14": nil,
	    "al_15": nil,
	    "al_16": nil,
	    "al_17": nil,
	    "al_18": nil,
	    "al_19": 0,
	    "al_20": 0,
	    "al_21": 440000,
	    "al_22": 440000,
	    "al_23": 440000,
	    "al_24": nil,
	    "al_25": nil,
	    "al_26": nil,
	    "al_27": nil,
	    "al_28": nil,
	    "al_29": nil,
	    "al_30": 440000,
	    "de_1": 18040,
	    "de_2": 0,
	    "de_3": 33770,
	    "de_4": nil,
	    "de_5": 2640,
	    "de_6": 6800,
	    "de_7": 5200,
	    "de_8": nil,
	    "de_9": nil,
	    "de_10": nil,
	    "de_11": nil,
	    "de_12": nil,
	    "de_13": nil,
	    "de_14": nil,
	    "de_15": 0,
	    "de_16": nil,
	    "de_17": nil,
	    "de_18": 0,
	    "de_19": 0,
	    "de_20": 0,
	    "de_21": 0,
	    "de_22": nil,
	    "de_23": nil,
	    "de_24": nil,
	    "de_25": 3960,
	    "de_26": 0,
	    "de_27": nil,
	    "de_28": 385550,
	    "de_29": nil,
	    "de_30": 66450,
	    "balance_amount": nil,
	    "log": nil,
	    "is_ok": 0,
	    "is_closed": 0,
	    "regist_user_code": nil,
	    "update_user_code": nil,
	    "calculated_at": nil,
	    "calc_user_code": nil,
	    "created_at": "2016-12-19 14:16:28",
	    "updated_at": "2016-12-19 14:16:28"
    }
```

#### 仕訳配賦

**/journal_distributions/create**

説明: このエンドポイントは、指定された配賦対象条件に基づいて按分し、仕訳を作成します。配賦基準は部門かセグメントです。新規作成された配賦が JSON 形式で返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/journal_distributions/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`target_timestamp` | *required* | String | 配賦対象日。形式は"YYYY-MM-DD"です。
`title` | *required* | String | 表題。
`criteria` | *required* | String | 配賦基準。"dept"(部門)か"segment"(セグメント)のいずれかを文字列で指定します。
`distribution_conditions` | *required* | Object | 配賦比率。criteria で指定した基準で、それぞれへの配賦比率を指定します。 ``` { 部門コード1 : 配賦率, 部門コード2 : 配賦率,...} or { セグメントコード1 : 配賦率, セグメントコード2 : 配賦率,...} ```
`memo` | *optional* | String | メモに書かれてある内容で部分検索します。
`search_conditions` | *required* | Object | 配賦対象条件。

*search_conditions*

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`start_date` | *required* | String | 配賦検索の開始日。形式は"YYYY-MM-DD"です。
`finish_date` | *required* | String | 配賦検索の終了日。形式は"YYYY-MM-DD"です。
`account_codes` | *required* | Array of String | 勘定科目コード。(複数指定可)
`dept_code` | *optional*\*| String | 部門コード。tag_listと同時に指定できません。
`tag_list` | *optional*\*| String | セグメント(旧タグ)識別コード文字列(カンマ区切り)。dept_codeと同時に指定できません。

\*dept_code か tag_list のいずれかの指定が必要です。

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -d '{ "search_conditions": { "start_date": "2016-09-01", "finish_date": "2016-09-30", "accou\
nt_codes": ["135~999","604"], "dept_code": "SHOP" }, "memo": "", "title": "タイトル", "criteria": "dept", "target_timestamp": "2017-02-01", "distribution_conditions": { "SETSURITSU": "1", "HEAD": "1" } }'  https://tsubaiso.n\
et/journal_distributions/create
```

**/journal_distribution/destroy/:id**

説明:指定された id の配賦を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST


URL 構成例:
```sh
https://tsubaiso.net/journal_distributions/destroy/:id
```


#### 月次推移表

**/balances/list**

説明: このエンドポイントは、指定された年月を最終月として設定し、そこから過去へ遡る形で指定月数分の月次推移表を返却します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/balances/list
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`year` | *required* | String | 年(最終)。
`month` | *required* | String | 月(最終)。
`m_span` | *optional* | Object | 取得月数。`year`, `month` で指定された年月から過去に遡ります。指定されなければ 12ヶ月になります。
`dept_code` | *optional* | String | 対象の部門を指定します。指定されなければ全部門になります。


リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -X GET -d '{ "year" : 2017, "month" : 11, "m_span" : 12, "dept_code" : "SALES" }'  https://tsubaiso.net/balances/list
```

JSON レスポンスの例:
```
{
  "bs" : {
           [
             { "type" : "account",
               "acount_master" : { "id" => 12345,
                                   "personal_id" => 3,
                                   "account_code" => "1",
                                   "account_name" => "テスト勘定科目",
                                   "account_short_name" => 'TEST_ACCOUNT',
                                   "account_kana" => "テストカンジョウカモク",
                                   "dc" => "d",
                                   "bspl"=>"bs",
                                   "tax_type"=>nil,
                                   "vat_io"=>nil,
                                   "brief"=>"-",
                                   "regist_user_code"=>nil,
                                   "update_user_code"=>nil,
                                   "summary_account_id"=>1,
                                   "use_in_balance"=>1,
                                   "use_in_statement"=>1,
                                   "status"=>100,
                                   "minus"=>0,
                                   "inputtable"=>1,
                                   "created_at"=>"2017/11/08 15:09:14 +0900",
                                   "updated_at"=>"2017/11/08 15:09:14 +0900" },
                 "sub_account_masters"=>[],
                 "amounts"=> [177776, 177776, 177776, 177776, 177776, 177776, 177776, 0, 177776, 1034320, 2513078, 2868630],
                 "total"=>2868630
             },
             { "type"=>"summary",
               "summary_account"=> { "id"=>134409808,
                                     "ccode"=>3,
                                     "sum_no"=>1,
                                     "sort_no"=>1,
                                     "name"=>"現金計",
                                     "dc"=>"d",
                                     "bspl"=>"bs",
                                     "selectable"=>1,
                                     "name_as_statement"=>nil,
                                     "invert"=>nil,
                                     "add_summary_account"=>nil,
                                     "subtract_summary_account"=>nil,
                                     "use_in_statement"=>0,
                                     "bracket_in_statement"=>nil,
                                     "brief"=>nil,
                                     "created_at"=>"2017/11/08 15:09:14 +0900",
                                     "updated_at"=>"2017/11/08 15:09:14 +0900",
                                     "use_in_balance"=>1 },
               "amounts"=> [8464, 8464, 8464, 8464, 8464, 8464, 8464, 0, 0, 678768, 1316642, 1316642],
               "total"=>1316642},
             },
           ],
         },
  "pl" : {
           ...
         },
  "pc" : {
           ...
         },
  "dept_code" : "SALES"
}
```

#### 銀行口座マスタ

**/bank_account_masters/list**

説明: 銀行口座マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/bank_account_masters/list/
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_account_masters/list/
```

JSON レスポンスの例:
```
[
    {
    "id": 0,
    "name": "テスト銀行",
    "account_type": "1",
    "account_number": "12345678",
    "nominee": "テスト（カ",
    "account_code": "111",
    "zengin_bank_code": "0001",
    "zengin_branch_code": "001",
    "dept_code": "HEAD",
    "memo": "テストメモ",
    "regist_user_code": null,
    "update_user_code": "yamakawa",
    "start_ymd": "2001/01/01",
    "finish_ymd": null,
    "zengin_client_code_sogo": "",
    "currency_code": "EUR",
    "currency_rate_master_id": null,
    "created_at": "2019/02/25 11:47:43 +0900",
    "updated_at": "2019/08/21 16:59:26 +0900",
    "currency_rate_master_code": null
  },
  {
    "id": 1,
    "name": "三菱UFJ銀行青葉台支店",
    "account_type": "1",
    "account_number": "12345678",
    "nominee": "ブルドッグウォータ（カ",
    "account_code": "111",
    "zengin_bank_code": "0005",
    "zengin_branch_code": "003",
    "dept_code": "HEAD",
    "memo": "入金口座",
    "regist_user_code": null,
    "update_user_code": null,
    "start_ymd": "2001/01/01",
    "finish_ymd": null,
    "zengin_client_code_sogo": "1234567890",
    "currency_code": null,
    "currency_rate_master_id": null,
    "created_at": "2019/02/25 11:47:43 +0900",
    "updated_at": "2019/02/25 11:47:43 +0900",
    "currency_rate_master_code": null
  }
]
```

**/bank_account_masters/show/:id**

説明: 1レコードの銀行口座マスタを返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/bank_account_masters/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_account_masters/show/0
```

JSON レスポンスの例:
```
{
    "id": 1,
    "name": "三菱UFJ銀行青葉台支店",
    "account_type": "1",
    "account_number": "12345678",
    "nominee": "ブルドッグウォータ（カ",
    "account_code": "111",
    "zengin_bank_code": "0005",
    "zengin_branch_code": "003",
    "dept_code": "HEAD",
    "memo": "入金口座",
    "regist_user_code": null,
    "update_user_code": null,
    "start_ymd": "2001/01/01",
    "finish_ymd": null,
    "zengin_client_code_sogo": "1234567890",
    "currency_code": null,
    "currency_rate_master_id": null,
    "created_at": "2019/02/25 11:47:43 +0900",
    "updated_at": "2019/02/25 11:47:43 +0900",
    "currency_rate_master_code": null
}
```

**/bank_account_masters/create**

説明: 銀行口座マスタを新規作成します。作成に成功した場合、新規作成された銀行口座が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/bank_account_masters/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`name` | *required* | String | 名称(銀行名及び支店名) *20文字以内
`account_type` | *optional* | Integer | 口座種別``` 1: 普通 2: 当座 3: 定期預金（固定） 4: 定期預金（流動） 5: 定期積金(固定)  6: 定期積金(流動) 入力なき場合デフォルトで普通口座が選択されます。 ```
`account_number` | *required* | Integer | 銀行口座番号　*半角数字8桁
`nominee` | *required* | String | 銀行口座名義 *半角(英数字、カタカナ、記号)30文字以内
`start_ymd` | *required* | String | 開設日 "YYYY-MM-DD"形式
`finish_ymd` | *optional* | String | 閉鎖日 "YYYY-MM-DD"形式 ```  入力なき場合閉鎖日が設定されません。```
`memo` | *optional* | String | メモ *60文字以内
`zengin_bank_code` | *required* | String | 銀行コード *半角数字4桁
`zengin_branch_code` | *required* | String | 銀行支店コード　*半角数字3桁
`zengin_client_code_sogo` | *required* | String | 依頼人コード(総合振込) *半角数字10桁
`currency_code` | *optional* | String | 通貨コード *日本円以外の時に設定 ``` 例）EUR CNY HKD USD  入力なき場合デフォルトで円通貨が設定されます。```
`currency_rate_master_code` | *optional* | Integer | 為替マスタの識別コード *日本円以外の時に使用``` 為替マスタの登録時に識別コードをします。為替マスタはAPIによる登録ができません。 ```

```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token:XXXXXXXXXXXXXX" -d '{ "name" : "Bank of Hatagaya" , "account_number" : 12345678,  "nominee" : "Tsubaiso Taro" , "start_ymd":"2019-03-03" , "zengin_bank_code": "0001" , "zengin_branch_code": "001", "zengin_client_code_sogo": 1234567891,  "account_type": "1","currency_code": "EUR", "currency_rate_master_code": "euro_currency_exchange" }' https://tsubaiso.net/bank_account_masters/create
```

**/bank_account_masters/update/:id**

説明: 指定された id の銀行口座マスタを更新します。更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/bank_account_masters/update/:id
```

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token:XXXXXXXXXXXXXX" -d '{ "account_type" : "2", "start_ymd":"2019-01-03"}' https://tsubaiso.net/bank_account_masters/update/247
```

JSON レスポンスの例:
```
{
  "id": 247,
  "name": "Bank of Hatagaya",
  "account_type": "2",
  "account_number": "12345678",
  "nominee": "Tsubaiso Taro",
  "account_code": "110",
  "zengin_bank_code": "0001",
  "zengin_branch_code": "001",
  "dept_code": "COMMON",
  "memo": null,
  "regist_user_code": "yamakawa",
  "update_user_code": "yamakawa",
  "start_ymd": "2019/01/03",
  "finish_ymd": null,
  "zengin_client_code_sogo": "1234567891",
  "currency_code": "EUR",
  "currency_rate_master_id": 6,
  "created_at": "2019/08/27 10:58:47 +0900",
  "updated_at": "2019/08/27 11:11:58 +0900",
  "currency_rate_master_code": "euro_currency_exchange"
}

```

**/bank_account_masters/destroy/:id**

説明：指定されたidの銀行口座マスタを削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
``` sh
https://tsubaiso.net/bank_account_masters/destroy/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST https://tsubaiso.net/bank_account_masters/destroy/139
```

#### 銀行口座

**/bank_accounts/list**

説明: 銀行口座の一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/bank_accounts/list/:year/:month
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_accounts/list/2008/7
```

JSON レスポンスの例:
```
[
  {
    "id": 0,
    "bank_account_master_id": 0,
    "is_closed": 0,
    "is_ok": 0,
    "start_timestamp": "2008/07/01 00:00:00 +0900",
    "finish_timestamp": "2008/07/31 00:00:00 +0900",
    "start_balance_fixed": null,
    "start_balance": null,
    "finish_balance": xxxxx,
    "start_balance_cache": xxxxx,
    "finish_balance_cache": xxxxx,
    "is_balanced": true,
    "bank_account_transactions_count": 0,
    "regist_user_code": null,
    "update_user_code": null,
    "start_balance_cache_fc": null,
    "finish_balance_cache_fc": null,
    "start_balance_fixed_fc": null,
    "finish_balance_fc": null,
    "exchange_gl_journal_id": null,
    "created_at": "2017/12/11 17:20:38 +0900",
    "updated_at": "2017/12/11 17:20:38 +0900"
  },
  {
    "id": 1,
    "bank_account_master_id": 1,
    "is_closed": 0,
    "is_ok": 0,
    "start_timestamp": "2008/07/01 00:00:00 +0900",
    "finish_timestamp": "2008/07/31 00:00:00 +0900",
    "start_balance_fixed": null,
    "start_balance": null,
    "finish_balance": xxxxx,
    "start_balance_cache": xxxxx,
    "finish_balance_cache": xxxxx,
    "is_balanced": true,
    "bank_account_transactions_count": 0,
    "regist_user_code": null,
    "update_user_code": null,
    "start_balance_cache_fc": null,
    "finish_balance_cache_fc": null,
    "start_balance_fixed_fc": null,
    "finish_balance_fc": null,
    "exchange_gl_journal_id": null,
    "created_at": "2017/12/11 17:20:38 +0900",
    "updated_at": "2017/12/11 17:20:38 +0900"
  },
]
```

**/bank_accounts/show/:id**

説明: 1レコードの銀行口座を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/bank_accounts/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_accounts/show/0
```

JSON レスポンスの例:
```
{
  "id": 0,
  "bank_account_master_id": 0,
  "is_closed": 0,
  "is_ok": 0,
  "start_timestamp": "2008/07/01 00:00:00 +0900",
  "finish_timestamp": "2008/07/31 00:00:00 +0900",
  "start_balance_fixed": null,
  "start_balance": null,
  "finish_balance": xxxxx,
  "start_balance_cache": xxxxx,
  "finish_balance_cache": xxxxx,
  "is_balanced": true,
  "bank_account_transactions_count": 0,
  "regist_user_code": null,
  "update_user_code": null,
  "start_balance_cache_fc": null,
  "finish_balance_cache_fc": null,
  "start_balance_fixed_fc": null,
  "finish_balance_fc": null,
  "exchange_gl_journal_id": null,
  "created_at": "2017/12/11 17:20:38 +0900",
  "updated_at": "2017/12/11 17:20:38 +0900"
}
```

**/bank_accounts/create**

説明: 銀行口座を新規作成します。作成に成功した場合、新規作成された銀行口座が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/bank_accounts/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`bank_account_master_id` | *required* | Integer | 銀行口座マスタID。
`start_timestamp` | *required* | String | 開始日 “YYYY-MM-DD”形式です。
`finish_timestamp` | *required* | String | 終了日 “YYYY-MM-DD”形式です。

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token:XXXXXXXX" -d '{"bank_account_master_id": "129" , "start_timestamp" : "2019-07-31" ,  "finish_timestamp" : "2019-08-30"}' https://tsubaiso.net/bank_accounts/create
```

#### 銀行口座明細

**/bank_account_transactions/list?bank_account_id=:bank_account_id**

説明: このエンドポイントは特定の銀行口座明細の一覧を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/bank_account_transactions/list?bank_account_id=1
```

JSON レスポンスの例:
```
[
    {
        "id":288,
        "personal_id":3,
        "serial_no":1,
        "reconciliation_id":23072,
        "bank_account_id":1064278031,
        "bank_reason_master_id":30001,
        "dc":"d",
        "brief":"aaa",
        "memo":"memo.",
        "regist_user_code":"yamakawa",
        "update_user_code":null,
        "created_at":"2018/02/01 19:16:33 +0900",
        "updated_at":"2018/02/01 19:16:33 +0900",
        "reason_code":"AR_RECEIPT",
        "journal_timestamp":"2018/02/01 00:00:00 +0900",
        "tag_list":["GROUP3_1"],
        "dept_code":"NEVER_ENDING",
        "price_value":112,
        "price_value_fc":null,
        "exchange_rate":null
    }, {
        "id":289,
        "personal_id":3,
        "serial_no":2,
        "reconciliation_id":23073,
        "bank_account_id":1064278031,
        "bank_reason_master_id":104,
        "dc":"d",
        "brief":"aaa",
        "memo":"",
        "regist_user_code":"yamakawa",
        "update_user_code":null,
        "created_at":"2018/02/01 19:16:45 +0900",
        "updated_at":"2018/02/01 19:16:45 +0900",
        "reason_code":"INTEREST",
        "journal_timestamp":"2018/02/01 00:00:00 +0900",
        "tag_list":["GROUP3_1",
        "GROUP2_2"],
        "dept_code":"NEVER_ENDING",
        "price_value":112998,
        "price_value_fc":null,
        "exchange_rate":null
    },
    ...
]
```

**/bank_account_transactions/show/:id**

説明: このエンドポイントは単一の銀行口座明細を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/bank_account_transactions/show/288
```

JSON レスポンスの例:
```
{
    "id":288,
    "personal_id":3,
    "serial_no":1,
    "reconciliation_id":23072,
    "bank_account_id":1064278031,
    "bank_reason_master_id":30001,
    "dc":"d",
    "brief":"brief text.",
    "memo":"memo text.",
    "regist_user_code":"yamakawa",
    "update_user_code":null,
    "created_at":"2018/02/01 19:16:33 +0900",
    "updated_at":"2018/02/01 19:16:33 +0900",
    "reason_code":"AR_RECEIPT",
    "journal_timestamp":"2018/02/01 00:00:00 +0900",
    "tag_list":["GROUP3_1"],
    "dept_code":"NEVER_ENDING",
    "price_value":112,
    "price_value_fc":null,
    "exchange_rate":null
}
```

**/bank_account_transactions/create**

説明: 銀行口座明細を新規作成します。作成に成功した場合、新規作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/bank_account_transactions/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`bank_account_id` | *required* | Integer | 銀行口座ID。
`journal_timestamp` | *required* | String | 入金・出金日。"YYYY-MM-DD"形式
`price_value` | *required* | Integer | 金額
`price_value_fc` | *optional* | Integer | 金額(外貨)
`exchange_rate` | *optional* | Integer | 為替レート
`reason_code` | *required* | String | 原因
`dc` | *required* | String | 入出金区分。d: 入金 c: 出金。
`brief` | *required* | String| 摘要
`memo` | *optional* | String | メモ
`tag_list` | *optional* | String | セグメント(旧タグ)識別コード文字列(カンマ区切り)
`dept_code` | *optional* | String | 部門コード

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"bank_account_id" : 1, "journal_timestamp": "2018-02-04", "price_value": 10000, "reason_code": "xxxx_123", "dc": "d", "brief": "test brief", "memo": "test memo", "tag_list": "GROUP3_1, GROUP2_2", "dept_code": "NEVER_ENDING"}' https://tsubaiso.net/bank_account_transactions/create/1000000001
```

**/bank_account_transactions/update/:id**

説明: 指定された id の銀行口座明細を更新します。更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/bank_account_transactions/update/:id
```

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "updated memo", "price_value": 10800 }'  https://tsubaiso.net/reimbursement_transactions/update/8833
```

**/bank_account_transactions/destroy/:id**

説明: 指定された id の銀行口座明細を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

#### 現金出納帳マスタ

**/petty_cash_masters/list**

説明: 現金出納帳マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/petty_cash_masters/list/
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/petty_cash_masters/list/
```

JSON レスポンスの例:
```
T.B.D.
```

**/petty_cash_masters/show/:id**

説明: 1レコードの現金出納帳マスタを返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/petty_cash_masters/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/petty_cash_masters/show/0
```

JSON レスポンスの例:
```
T.B.D.
```

#### 現金出納帳

**/petty_cashes/list**

説明: 現金出納帳の一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/petty_cashes/list/:year/:month
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/petty_cashes/list/2018/2
```

JSON レスポンスの例:
```
T.B.D.
```

**/petty_cashes/show/:id**

説明: 1レコードの現金出納帳を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/petty_cashes/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/petty_cashes/show/0
```

JSON レスポンスの例:
```
T.B.D.
```

**/petty_cashes/create**

説明: 現金出納帳の期間を追加します。作成に成功した場合、新規作成された現金出納帳の期間が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/petty_cashes/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`petty_cash_master_id` | *required* | Integer | 現金出納帳マスタID。
`start_ymd` | *required* | Date | 開始日。 "YYYY-MM-DD"形式
`finish_ymd` | *optional* | Date | 終了日。 "YYYY-MM-DD"形式
`start_balance_fixed` | *optional* | Integer | 開始日残高。*通常は自動計算されますので空欄にしてください。一番最初の期間のみ設定可能です。*

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"petty_cash_master_id" : 1, "start_timestamp" : "2018-02-01", "finish_timestamp" : "2018-02-28"}' https://tsubaiso.net/petty_cashes/create
```

#### 現金出納帳明細

**/petty_cash_transactions/list?petty_cash_id=:petty_cash_id**

説明: このエンドポイントは特定の現金出納帳明細の一覧を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/petty_cash_transactions/list?petty_cash_id=1
```

JSON レスポンスの例:
```
T.B.D.
```

**/petty_cash_transactions/show/:id**

説明: このエンドポイントは単一の現金出納帳明細を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/petty_cash_transactions/show/1
```

JSON レスポンスの例:
```
T.B.D.
```

**/petty_cash_transactions/create**

説明: 現金出納帳明細を新規作成します。作成に成功した場合、新規作成された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/petty_cash_transactions/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`petty_cash_id` | *required* | Integer | 現金出納帳ID。
`journal_timestamp` | *required* | Date | 入金・出金日。"YYYY-MM-DD"形式
`price_value` | *required* | Integer | 金額
`reason_code` | *required* | String | 原因コード
`dc` | *required* | String | 入出金区分。d: 入金 c: 出金。
`tax_type` | *optional* | Integer | 税区分コード
`brief` | *optional* | String| 摘要
`memo` | *optional* | String | メモ
`tag_list` | *optional* | String | セグメント(旧タグ)識別コード文字列(カンマ区切り)
`dept_code` | *optional* | String | 部門コード

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"petty_cash_id" : 1, "journal_timestamp": "2018-02-04", "price_value": 10000, "reason_code": "xxxx_123", "dc": "d", "brief": "test brief", "memo": "test memo", "tag_list": "GROUP3_1, GROUP2_2", "dept_code": "NEVER_ENDING"}' https://tsubaiso.net/petty_cash_transactions/create
```

**/petty_cash_transactions/update/:id**

説明: 指定された id の現金出納帳明細を更新します。更新に成功した場合、更新された明細が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/petty_cash_transactions/update/:id
```

リクエスト例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "updated memo", "price_value": 10800 }'  https://tsubaiso.net/petty_cash_transactions/update/8833
```

**/petty_cash_transactions/destroy/:id**

説明: 指定された id の現金出納帳明細を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

#### 税区分マスタ

**/tax_masters/list**

説明: 税区分マスタをJSON形式で取得します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/tax_masters/list
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET https://tsubaiso.net/tax_masters/list
```

JSON レスポンスの例:
```
[
  {
    "id": 0,
    "code": 0,
    "name": "対象外又は非課税仕入",
    "abbr_name": "　",
    "description": "消費税の課税対象外の取引や、不課税取引及び非課税仕入れとなる取引に使用します。",
    "dc": "d",
    "controll_business_segment": 0,
    "color": "aaaaaa",
    "start_timestamp": "1989/04/01 00:00:00 +0900",
    "finish_timestamp": null,
    "created_at": "2008/09/29 17:22:11 +0900",
    "updated_at": "2008/09/29 17:22:11 +0900",
    "tax_ratio_division_view": "0%",
    "dc_view": "借方(D)",
    "taxable_division_view": "その他"
  },
  {
    "id": 1,
    "code": 1,
    "name": "課税売上分一般仕入(5%)",
    "abbr_name": "仕",
    "description": "税率5％の課税仕入れのうち、課税売上げにのみ対応する取引に使用します。",
    "dc": "d",
    "controll_business_segment": 0,
    "color": "7fff00",
    "start_timestamp": "1997/04/01 00:00:00 +0900",
    "finish_timestamp": "2014/03/31 23:59:59 +0900",
    "created_at": "2008/09/29 17:22:11 +0900",
    "updated_at": "2008/09/29 17:22:11 +0900",
    "tax_ratio_division_view": "5%",
    "dc_view": "借方(D)",
    "taxable_division_view": "課税仕入"
  }
]
```

**/tax_masters/show/:id**

説明: 単一の税区分マスタをJSON形式で取得します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/tax_masters/show/:id
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET https://tsubaiso.net/tax_masters/show/0
```

JSON レスポンスの例:
```
{
  "id": 0,
  "code": 0,
  "name": "対象外又は非課税仕入",
  "abbr_name": "　",
  "description": "消費税の課税対象外の取引や、不課税取引及び非課税仕入れとなる取引に使用します。",
  "dc": "d",
  "controll_business_segment": 0,
  "color": "aaaaaa",
  "start_timestamp": "1989/04/01 00:00:00 +0900",
  "finish_timestamp": null,
  "created_at": "2008/09/29 17:22:11 +0900",
  "updated_at": "2008/09/29 17:22:11 +0900",
  "tax_ratio_division_view": "0%",
  "dc_view": "借方(D)",
  "taxable_division_view": "その他"
}
```

#### 棚卸資産マスタ

**/physical_inventory_masters/list**

説明: 棚卸資産マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/physical_inventory_masters/list
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET http://tsubaiso.net/physical_inventory_masters/list
```

JSONレスポンスの例:
``` sh
[
  {
    "id": 212,
    "name": "Hello",
    "memo": "",
    "start_ymd": "2019/06/01",
    "finish_ymd": null,
    "dept_code": "NEVER_ENDING",
    "tag_list": [
      "GROUP2_2"
    ]
  },
  {
    "id": 252,
    "name": "hirasaki",
    "memo": "",
    "start_ymd": "2019/06/01",
    "finish_ymd": null,
    "dept_code": "NEVER_ENDING",
    "tag_list": [
      "GROUP2_1",
      "GROUP3_1"
    ]
  }
]
```

**/physical_inventory_masters/show**

説明: idで指定した棚卸資産マスタを返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/physical_inventory_masters/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET https://tsubaiso.net/physical_inventory_masters/show/1
```

JSONレスポンスの例:
``` sh
{
  "id": 1,
  "name": "赤",
  "memo": "This is test for",
  "start_ymd": "2010/01/01",
  "finish_ymd": "2019/11/01",
  "dept_code": "ETERNAL"
}
```

**/physical_inventory_masters/create**

説明: 保管場所を新規作成します。成功した場合、新規作成された保管場所が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
``` sh
https://tsubaiso.net/physical_inventory_masters/create
```

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`name` | *required* | String | 保管場所の名称。最低一文字
`memo` | *optional* | String | メモ
`start_ymd` | *required* | Datetime | 開始日。 "YYYY/MM/DD"形式
`finish_ymd` | *optional* | Datetime | 終了日。 "YYYY/MM/DD"形式
`dept_code` | *optional* | String| 部門コード
`tag_list` | *optional* | String | セグメント(旧タグ)識別コード文字列(カンマ区切り)。



リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXX" -X POST -d '{"name": "New Tsubaiso inventory", "memo": "from_API", "start_ymd": "2019-03-03", "finish_ymd": "2019-03-10", "dept_code": "NEVER_ENDING", "tag_list": "GROUP2_1,GROUP3_2"}' https://tsubaiso.net/physical_inventory_masters/create
```

**/physical_inventory_masters/update/:id**

説明: 指定されたidの保管場所を更新します。更新に成功した場合、更新された保管場所が JSON として返されます。

HTTP メソッド: POST

URL 構成例:
``` sh
https://tsubaiso.net/physical_inventory_masters/update/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXX" -X POST -d '{"name": "New Inventory Name", "memo": "Edited from_API", "start_ymd": "2019-03-03", "finish_ymd": "2019-03-04"}' https://tsubaiso.net/physical_inventory_masters/update/:id
```

**/physical_inventory_masters/destroy/:id**

説明：指定されたidの保管場所を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
``` sh
https://tsubaiso.net/physical_inventory_masters/destroy/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST https://tsubaiso.net/physical_inventory_masters/destroy/:id
```

#### 資産種類マスタ
**/personalized_asset_type_masters/list**

説明: このエンドポイントは資産種類マスタの一覧を返します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/personalized_asset_type_masters/list
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET http://tsubaiso.net/personalized_asset_type_masters/list
```

JSON レスポンスの例:
```
[
  {
    "id":42,
    "code":"221",
    "name":"建設仮勘定",
    "summary_account_no":8,
    "local_tax_segment_code":null,
    "corporate_tax_segment_name":null,
    "asset_account_code":"221",
    "contra_account_code":"730",
    "contra_mc_account_code":"656",
    "impairment_account_code":"730",
    "impairment_mc_account_code":"656",
    "sort_no":null,
    "regist_user_code":null,
    "update_user_code":null,
    "created_at":"2020/11/12 15:45:12 +0900",
    "updated_at":"2020/11/12 15:45:12 +0900",
    "accumulated_depreciation_account_code":"230"
  },
  {
    "id":41,
    "code":"247",
    "name":"リース資産(ソフトウェア)",
    "summary_account_no":9,
    "local_tax_segment_code":null,
    "corporate_tax_segment_name":"リース資産(ソフトウェア)",
    "asset_account_code":"247",
    "contra_account_code":"730",
    "contra_mc_account_code":"656",
    "impairment_account_code":"730",
    "impairment_mc_account_code":"656",
    "sort_no":null,
    "regist_user_code":null,
    "update_user_code":null,
    "created_at":"2020/11/12 15:45:12 +0900",
    "updated_at":"2020/11/12 15:45:12 +0900",
    "accumulated_depreciation_account_code":"230"
  }
]
```

**/personalized_asset_type_masters/show/:id**

説明: このエンドポイントは単一の資産種類マスタを返します。

HTTP メソッド: GET

URL 構成例:
``` sh
https://tsubaiso.net/personalized_asset_type_masters/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET https://tsubaiso.net/personalized_asset_type_masters/show/1
```

JSON レスポンスの例:
```
{
  "id":42,
  "code":"221",
  "name":"建設仮勘定",
  "summary_account_no":8,
  "local_tax_segment_code":null,
  "corporate_tax_segment_name":null,
  "asset_account_code":"221",
  "contra_account_code":"730",
  "contra_mc_account_code":"656",
  "impairment_account_code":"730",
  "impairment_mc_account_code":"656",
  "sort_no":null,
  "regist_user_code":null,
  "update_user_code":null,
  "created_at":"2020/11/12 15:45:12 +0900",
  "updated_at":"2020/11/12 15:45:12 +0900",
  "accumulated_depreciation_account_code":"230"
}
```

**/personalized_asset_type_masters/create**

説明: 資産種類マスタを新規作成します。作成に成功した場合、新規作成されたマスタが JSON として返されます。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/personalized_asset_type_masters/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`code` | *required* | String | 資産種類コード
`name` | *required* | String | 名称
`sort_no` | Optional | Integer | 並び順
`local_tax_segment_code` | Optional | Integer | 1: 構築物<br>2: 機械及び装置<br>3: 船舶<br>4: 航空機<br>5: 車両及び運搬具<br>6: 工具、器具及び備品
`corporate_tax_segment_name` | Optional | String | 法人税名
`asset_account_code` | *required* | String | 資産勘定科目 (勘定科目コード)
`contra_account_code` | *required* | String | 償却勘定科目(勘定科目コード)
`contra_mc_account_code` | *required* | String | 償却勘定科目(製造原価)(勘定科目コード)
`impairment_account_code` | *required* | String | 減損勘定科目のデフォルト(勘定科目コード)
`impairment_mc_account_code` | *required* | String | 減損勘定科目(製造原価)のデフォルト(勘定科目コード)
`accumulated_depreciation_account_code` | *required* | String | 減価償却累計額勘定科目(勘定科目コード)


リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"code": "BUILDING", "name": "建設", "asset_account_code": "221", "contra_account_code": "730", "contra_mc_account_code": "656", "impairment_account_code": "730", "impairment_mc_account_code": "656", "accumulated_depreciation_account_code": "230"}' https://tsubaiso.net/personalized_asset_type_masters/create
```

**/personalized_asset_type_masters/destroy/:id**

説明: 指定された id の資産種類マスタを削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/personalized_asset_type_masters/destroy/:id
```

#### API履歴

**/api_histories/index**

説明: 月ごとのAPI呼び出し回数をJSON形式で取得します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/api_histories/index
```
リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token:XXXXXXXXXXXX" -X GET  http://tsubaiso.net/api_histories/index
```

JSON レスポンスの例:
```
[
  {
    "ym": "201905",
    "cnt": 411
  },
  {
    "ym": "201904",
    "cnt": 805
  }
]
```

**/api_histories/list**

説明: API履歴をJSON形式で取得します。

HTTP メソッド: GET

URL 構成例:
```sh
https://tsubaiso.net/api_histories/list/:year/:month
```

リクエスト例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET https://tsubaiso.net/api_histories/list/api_histories/list/2019/5
```

JSON レスポンスの例:
```
[
  {
    "access_timestamp": "2016/01/23 18:53:21 +0900",
    "url": "https://tsubaiso.net/api_histories/list/api_histories/list",
    "access_token": "XXXXXX*****",
    "controller": "staffs",
    "method": "list"
  },
  {
    "access_timestamp": "2016/01/23 18:53:21 +0900",
    "url": "https://tsubaiso.net/staffs/create",
    "access_token": "XXXXXX*****",
    "controller": "staffs",
    "method": "create"
  }
]
```

### 外部連携機能

説明: リソースが外部サービスと連携していることを示すための、追加のメタデータを付与するオプションです。

対象リソース名: 売上明細, 仕入・経費明細, 旅費・経費精算明細, マニュアル仕訳, 取引先

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`partner_code` | *optional* | String | パートナーコード。
`link_url` | *optional* | String | データ連携している外部サービスのURL。
`editable` | *optional* | Integer | ツバイソでの編集可否。(1: 編集可, 0: マネージャ相当ユーザのみ編集可(Default))
`deletable` | *optional* | Integer | ツバイソでの削除可否。(1: 削除可, 0: マネージャ相当ユーザのみ削除可(Default))
`partner_editable` | *optional* | Integer | 外部サービスからの編集可否。(1: 編集可(Default), 0: 編集不可)
`partner_deletable` | *optional* | Integer | 外部サービスからの削除可否。(1: 削除可(Default), 0: 削除不可)

### 予定日

説明: 入金/支払サイトと締日によって、対象の入金/支払予定日を確定して返します。

対象リソース名: -

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`target_date` | *required* | String(日付 ex. ("%Y-%m-%d")) | 対象日(計上日)
`sight` | *required* | String (取引先#pay_sight, or receive_sight) | 入金/支払サイト
`closing_day` | *required* | String (取引先#pay_closing_schedule, or receive_closing_schedule) | 締日
`shift` | *optional* | String | 予定日が休日と重なった場合に前営業日にするか翌営業日にするかのオプション。('before': 前営業日(default),  'after': 翌営業日)
