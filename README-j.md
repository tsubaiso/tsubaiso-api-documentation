# Tsubaiso API (beta)

このドキュメントでは Tsubaiso API のベータ版の説明をします。
Tsubaiso API ベータ版では売上明細、仕入・経費明細、取引先、社員のデータをやりとりできます。
将来のバージョンでは、ツバイソシステムの他のモジュールにアクセスするための新しいエンドポイントが加わる予定です。

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

アクセストークンは以下のページから取得できます。（ツバイソアカウント必要）

[Get an API access token](https://tsubaiso.net/api_keys)

## レスポンスコードとエラー処理

Code | Description
--- | ---
`200 OK` | リクエスト成功 |
`204 No Content` | リクエストに成功したが返されるコンテンツはありません。
`401 Not Authorized` | アクセストークンが送られていないか正しくありません。
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
        "ar_reason_master_id": 0,
        "ar_receipt_attachments_count": null,
        "code": null,
        "created_at": "2015/10/05 15:26:43 +0900",
        "customer_master_id": 101,
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
        "sales_price": 5000,
        "sales_tax": 400,
        "sales_tax_type": 18
    }, {
        "ar_reason_master_id": 121278,
        "ar_receipt_attachments_count": null,
        "code": null,
        "created_at": "2015/10/05 17:39:34 +0900",
        "customer_master_id": 895820,
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
        "sales_price": 10000,
        "sales_tax": 800,
        "sales_tax_type": 0
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
    "ar_reason_master_id": 0,
    "ar_receipt_attachments_count": null,
    "code": null,
    "created_at": "2015/10/05 15:26:43 +0900",
    "customer_master_id": 101,
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
    "sales_price": 5000,
    "sales_tax": 400,
    "sales_tax_type": 18
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
`price` | *required* | Integer | 売上高(税込)
`realization_timestamp` | *required* | String | 明細の実現日。 "YYYY-MM-DD" 形式
`customer_master_code` | *required* | String | 取引先コード
`reason_master_code` | *required* | String | 明細の原因コード。仕訳を作成するために使われます。
`dc` | *required* | String | 原因区分。 'd' は debit の意で「増加」に、'c' は credit の意で「減少」になります。
`memo` | *required* | String | メモ。値は空文字でも構いませんが必須項目です。
`tax_code` | *required* | Integer | 税区分コード
`year` | *optional* | Integer | 明細の年。 年が指定された場合は月も必須項目になります。年が指定されない場合は現在の年が使われます。
`month` | *optional* | Integer | 明細の月。月が指定された場合は年も必須項目になります。月が指定されない場合は現在の月が使われます。
`dept_code` | *optional* | String | 部門コード
`sales_tax` | *optional* | Integer | 消費税額。指定されなかった場合、自動で計算されます。
`scheduled_receipt_timestamp` | *optional* | String | 入金予定日。 “YYYY-MM-DD”形式
`scheduled_memo` | *optional* | String | 入金予定に関するメモ

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"year": 2015, "month": 10, "price": 5000, "realization_timestamp": "2015-10-31", "customer_master_code": "101", "dept_code": "DEPT A", "reason_master_code": "SALES", "dc": "d", "memo": "500 widgets", "tax_code": 0}' https://tsubaiso.net/ar/create
```

**/ar/destroy/:id**

説明: 指定された id の売上明細を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ar/destroy/:id
```

#### 仕入・経費明細

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
        "ap_reason_master_id": 1,
        "buying_journal_dc_id": 9835,
        "code": null,
        "created_at": "2015/10/06 15:45:21 +0900",
        "customer_master_id": 8201,
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
        "buying_price": 5000,
        "buying_tax": 400,
        "buying_tax_type": 0
    }, {
        "accrual_timestamp": "2015/10/31 00:00:00 +0900",
        "ap_payment_attachments_count": null,
        "ap_reason_master_id": 1,
        "buying_journal_dc_id": 9836,
        "code": null,
        "created_at": "2015/10/06 15:48:42 +0900",
        "customer_master_id": 101,
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
        "buying_price": 10000,
        "buying_tax": 800,
        "buying_tax_type": 0
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
    "ap_reason_master_id": 1,
    "buying_journal_dc_id": 9835,
    "code": null,
    "created_at": "2015/10/06 15:45:21 +0900",
    "customer_master_id": 8201,
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
    "buying_price": 5000,
    "buying_tax": 400,
    "buying_tax_type": 0
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
`price` | *required* | Integer | 発生額(税込)
`accrual_timestamp` | *required* | String | 発生日。  "YYYY-MM-DD" 形式
`customer_master_code` | *required* | String | 取引先コード
`reason_master_code` | *required* | String | 明細の原因コード。仕訳を作成するために使われます。
`dc` | *required* | String | 原因区分。 'd' は「減少」に、'c' は「増加」になります。
`memo` | *required* | String | メモ。値は空文字でも構いませんが必須項目です。
`tax_code` | *required* | Integer | 税区分コード
`port_type` | *required* | Integer | エリア区分。 1 は「国内」、 2 は「国外」
`year` | *optional* | Integer | 明細の年。 年が指定された場合は月も必須項目になります。年が指定されない場合は現在の年が使われます。
`month` | *optional* | Integer | 明細の月。月が指定された場合は年も必須項目になります。月が指定されない場合は現在の月が使われます。
`dept_code` | *optional* | String | 部門コード
`buying_tax` | *optional* | Integer | 消費税額。省略された場合は自動で計算されます。
`scheduled_pay_timestamp` | *optional* | String | 支払予定日。  "YYYY-MM-DD" 形式
`scheduled_memo` | *optional* | String | 支払いに関する追加のメモ
`need_tax_deduction` | *optional* | Integer | 源泉徴収の対象とするか否か。 0:しない, 1:する
`preset_withholding_tax_amount` | *optional* | Integer | 事前設定源泉徴収額
`withholding_tax_base` | *optional* | Integer | 源泉徴収基準額。 1:消費税込額, 2:消費税抜額
`withholding_tax_segment` | *optional* | String | 源泉徴収区分コード (例: "nta2795"。 次のページを参照してください https://www.nta.go.jp/taxanswer/gensen/2795.htm)

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"year": 2015, "month", 10, "price": 5000, "accrual_timestamp": "2015-10-31", "customer_master_code": "8201", "dept_code": "DEPT C", "reason_master_code": "BUYING_IN", "dc": "c", "memo": "Office Supplies for Frank", "tax_code": 0, "port_type": 1 }' https://tsubaiso.net/ap_payments/create
```

**/ap/destroy/:id**

説明: 指定された id の仕入・経費明細を削除します。成功した場合 204 No Content が返ります。

HTTP メソッド: POST

URL 構成例:
```sh
https://tsubaiso.net/ap/destroy/:id
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
    update_user_code: "user"
    updated_at: "2015/11/13 11:14:00 +0900"
    used_in_ap: 1
    used_in_ar: 1
    withholding_tax_base: 1
    withholding_tax_segment: "nta2795"
    zip: "1510072"
}
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
`tax_type_for_remittance_charge` | *required* | Integer | 支払手数料の課税区分。3は「共通売上分」、0は「対象外又は非課税仕入」
`sender_name` | *optional* | String | 自動消込キーワード(40文字以内)
`locale` | *optional* | String | 請求書の言語 "ja-JP": 日本語、"en": 英語
`foreign_currency` | *optional* | Integer | 外貨取引の有無。0:無（外貨での請求書なし）、1: 「有（外貨での請求書あり）」
`used_in_ar` | *required* | Integer | 販売管理の債権区分。0: 使用しない、 1: 主に売掛金（売上代金の入金先）として使用する、2: 主に未収入金（売上代金以外の入金先）として使用する
`receive_closing_schedule` | *optional* | String | 請求締日。 "":未設定、"0": 即日、 1: 月初、5: 5日、10: 10日、15: 15日、20: 20日、25: 25日、-1: 月末
`receive_sight` | *optional* | String | 入金サイト。 "": 未設定、"0": 即日払、"1m20": 締月20日払、"1m27": 締月27日払、"1m-1": 締月末払、"2m5": 締月翌月5日払、"2m10": 締月翌月10日払、"2m15": 締月翌月15日払、"2m20": 締月翌月20日払、"2m25": 締月翌月25日払、"2m27": 締月翌月27日払、"2m-1": 締月翌月末払、"3m5": 締月翌々月5日払、"3m10": 締月翌々月10日払、"3m15": 締月翌々月15日払、"3m20": 締月翌々月20日払、"3m25": 締月翌々月25日払、"3m-1": 締月翌々月末払、"4m5": 締月翌々々月5日払、"4m10": 締月翌々々月10日払、"4m15": 締月翌々々月15日払、"-1m20": 締月前月20日払、"-1m-1": 締月前月末払
`bill_detail_round_rule` | *optional* | Integer | 見積書・請求書の明細の端数処理設定。 1: 切り捨て、2: 切り上げ、3: 四捨五入
`used_in_ap` | *required* | Integer | 購買管理の債務区分。0: 使用しない、1: 主に未払金（仕入代金以外の支払い先）として使用する、2: 主に買掛金（仕入代金の支払い先）として使用する
`pay_closing_schedule` | *optional* | String | 支払締日。 "": 未設定、"0": 即日、 "1": 月初、"5": 5日、"10": 10日、"15": 15日、"20": 20日、"25": 25日、"-1": 月末
`pay_sight` | *optional* | String | 支払サイト。"": 未設定、"0": 即日払、"1m20": 締月20日払、"1m27": 締月27日払、"1m-1": 締月末払、"2m5": 締月翌月5日払、"2m10": 締月翌月10日払、"2m15": 締月翌月15日払、"2m20": 締月翌月20日払、"2m25": 締月翌月25日払、"2m27": 締月翌月27日払、"2m-1": 締月翌月末払、"3m5": 締月翌々月5日払、"3m10": 締月翌々月10日払、"3m15": 締月翌々月15日払、"3m20": 締月翌々月20日払、"3m25": 締月翌々月25日払、"3m-1": 締月翌々月末払、"4m5": 締月翌々々月5日払、"4m10": 締月翌々々月10日払、"4m15": 締月翌々々月15日払、"-1m20": 締月前月20日払、"-1m-1": 締月前月末払
`need_tax_deductions` | *optional* | String | 源泉徴収を行うか否か。 "0": 行わない、"1": 行う
`withholding_tax_segment` | *optional* | String | 源泉徴収区分。  例："nta2795"  値については、[国税庁の源泉徴収分類コード](https://www.nta.go.jp/taxanswer/gensen/gensen35.htm)を参照ください。
`withholding_tax_base` | *optional* | Integer | 源泉徴収基準額。1: 消費税込額、2: 消費税抜額
`bank_code` | *optional* | String | 取引先の銀行コード。(4桁)
`bank_branch_code` | *optional* | String | 取引先の支店コード(3桁)
`bank_course` | *optional* | Integer | 取引先の口座種別。 1: 普通、2: 当座、3: 定期預金(流動)、4: 定期預金(固定)、5: 定期積金（流動）、6: 定期積金（固定）
`bank_nominee` | *optional* | String | 取引先の口座名義（半角カナ30文字以内）
`bank_account_number` | *optional* | String | 取引先の口座番号
`is_valid` | *required* | Integer | 取引先ステータス。 1: 使用中、0: 使用停止

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{name: "テスト株式会社", name_kana: "テストカブシキガイシャ", code: "9000", tax_type_for_remittance_charge: "3", used_in_ar: 1, used_in_ap: 1, is_valid: 1 }' https://tsubaiso.net/customer_masters/create
```

**/customer_masters/update**

説明: 取引先を更新します。 更新に成功した場合、更新された明細が JSON として返されます。

Method: POST

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

#### Staff

**/staffs/list/**

説明: 社員の一覧を返します。

Method: GET

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
        "login": "XXXXX"
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
        "login": "YYYYY"
    }
]
```

**/staffs/show/**

説明: 単一の社員を返します。

Method: GET

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
    "login": "XXXXX"
}
```

#### Staff Data

**/staff_data/list/**

説明: このエンドポイントは社員情報の一覧を返します。

Method: GET

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

Method: GET

URL 構成例:
``` sh
https://tsubaiso.net/staff_data/show/:id
```

リクエストの例:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staff_data/show/1234
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"staff_id": 10000, "code": "BIRTH_YMD"}'  https://tsubaiso.net/staff_data/show
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"staff_id": 10000, "code": "BIRTH_YMD", "time": "2001-01-01"}'  https://tsubaiso.net/staff_data/show
```

JSON レスポンスの例:
```
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
`finish_timestamp` | *required or optional* | String | 終了年月日。もし在籍区分が”0: 非在籍”の場合、必須項目とないます。
`no_finish_timestamp` | *required or optional* | String | 在籍区分。　0: 非在籍 1: 在籍

リクエストの例:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -X POST -d '{"staff_id": 1000, "code": "NAME_MEI", "value": "Taro", "start_timestamp": "2015-12-15", "no_finish_timestamp": "1"}'  https://tsubaiso.net/staff_data/create
```

**/staff_data/update**

説明: 社員情報を更新します。更新に成功した場合、更新された明細が JSON として返されます。

Method: POST

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
