# Tsubaiso API (beta)

This is the documentation for the beta version of the Tsubaiso API. The beta version currently handles accounts receivables, accounts payable transactions, customer management and staff/staff data management. Future versions of this API will add new endpoints to access other modules of the Tsubaiso system.

## Root Endpoint

```sh
https://tsubaiso.net
```


## Request Format

We ask that all requests to the API be made using JSON.

## Authentication

The user must provide their access token in order to access the Tsubaiso API.

```
$ curl -i -H "Access-Token: xxxxxxxxxxxxxxxxx" -H "Accept: application/json" -H "Content-Type: application/json" https://tsubaiso.net/ar/list
```

## Response Codes and Error Handling

Code | Description
--- | ---
`200 OK` | A successful request was made. |
`204 No Content` | A successful request was made but no content is passed back.
`401 Not Authorized` | The access token was not provided or was incorrect.
`403 Forbidden` | You do not have the correct privileges to make this request.
`404 Not found` | Path is incorrect or resource was not found at the specified path.
`422 Unprocessable Entity` | One or more parameters were incorrect or insufficient. The error message will tell you the reason.
`500 Internal Server Error` | There was an error on the server.
`503 Service Unavailable` | This error occurs when there are too many requests coming from your IP address. Wait a little bit to make your next request.

## Resources

#### Accounts Receivables

**/ar/list/:year/:month**

Description: This endpoint returns a list of accounts receivables transactions for a particular month. If no year and month parameters are provided. It returns the transactions for the current month.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/ar/list/:year/:month
```

Sample JSON response:
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
        "tag_list": ["Payment", "Foreign"],
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
        "tag_list": ["Payment", "Foreign"],
        "sales_tax": 800,
        "tax_code": 0,
        "customer_master_code": 895820,
        "reason_master_code": "SALES"
    }
]
```

**/ar/show/:id**

Description: This endpoint returns a single accounts receivable transaction.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/ar/show/:id
```

Sample JSON response:
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
    "tag_list": ["Payment", "Foreign"],
    "sales_tax": 400,
    "tax_code": 18,
    "customer_master_code": 101,
    "reason_master_code": "SALES"
}
```

**/ar/create**

Description: Creates a new accounts receivable transaction. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/ar/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`price_including_tax` | *required* | Integer | Amount of the transaction including tax.
`realization_timestamp` | *required* | String | Actual date of the transaction. Format must be "YYYY-MM-DD"
`customer_master_code` | *required* | String | Code of the transaction party.
`reason_master_code` | *required* | String | Reason of the transaction. This is used to create the journal entry.
`dc` | *required* | String | 'd' if the transaction was a debit to AR, 'c' if it was a credit.
`memo` | *required* | String | Memo for the transaction. Can be blank but must be provided.
`tax_code` | *required* | Integer | Tax code for the transaction.
`dept_code` | *optional* | String | Code of the internal department involved.
`sales_tax` | *optional* | Integer | Sales tax on the transaction. Is automatically calculated if not provided.
`scheduled_receive_timestamp` | *optional* | String | Date of receipt. Format must be “YYYY-MM-DD”.
`scheduled_memo` | *optional* | String | Optional memo regarding receipt of funds.
`tag_list` | *optional* | String | Optional tag code string.(Comma-separated)
`tag_name_list` | *optional* | String | Optional tag name string.(Comma-separated) **Only if tag_list is not provided.**

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"price_including_tax": 5400, "realization_timestamp": "2015-10-31", "customer_master_code": "101", "dept_code": "DEPT A", "reason_master_code": "SALES", "dc": "d", "memo": "500 widgets", "tag_list": "Payment,Foreign", "tax_code": 0}' https://tsubaiso.net/ar/create
```

**/ar_receipts/update/:id**

Description: Updates an accounts receivables transaction. The updated transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/ar_receipts/update/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "updated memo", "price_including_tax": 10800 }'  https://tsubaiso.net/ar_receipts/update/8833
```

**/ar/destroy/:id**

Description: Destroys the accounts receivable transaction specified as the id. Returns a status of 204 No Content.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/ar/destroy/:id
```

#### Accounts Payables

**/ap_payments/list/:year/:month**

Description: Returns a list of accounts payables transactions for a particular month. If no year and month parameters are provided. It returns the transactions for the current month.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/ap_payments/list/:year/:month
```

Sample JSON response:
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
        "tag_list": ["Payment", "Foreign"],
        "buying_tax": 400,
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
        "price_including_tax": 10800
        "price_excluding_tax": 10000,
        "tag_list": ["Payment", "Foreign"],
        "buying_tax": 800,
        "tax_code": 0,
        "customer_master_code": 101,
        "reason_master_code": "BUYING_IN"
    }
]
```

**/ap_payments/show/:id**

Description: This endpoint returns a single accounts payable transaction.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/ap_payments/show/:id
```

Sample JSON response:
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
    "price_including_tax": 5400
    "price_excluding_tax": 5000,
    "tag_list": ["Payment", "Foreign"],
    "buying_tax": 400,
    "tax_code": 0,
    "customer_master_code": 8201,
    "reason_master_code": "BUYING_IN"
}
```

**/ap_payments/create**

Description: Creates a new accounts payable transaction. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/ap_payments/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`price_including_tax` | *required* | Integer | Amount of the transaction including tax.
`accrual_timestamp` | *required* | String | Actual date of the transaction. Format must be "YYYY-MM-DD"
`customer_master_code` | *required* | String | Code of the transaction party.
`reason_master_code` | *required* | String | Reason of the transaction. This is used to create the journal entry.
`dc` | *required* | String | 'd' if the transaction was a debit to AP, 'c' if it was a credit.
`memo` | *required* | String | Memo for the transaction. Can be blank but must be provided.
`tax_code` | *required* | Integer | Tax code for the transaction.
`port_type` | *required* | Integer | 1 for domestic transaction. 2 for foreign transaction.
`dept_code` | *optional* | String | Code of the internal department involved.
`buying_tax` | *optional* | Integer | Sales tax on the transaction. Is automatically calculated if not provided.
`scheduled_pay_timestamp` | *optional* | String | Date of payment. Format must be "YYYY-MM-DD".
`scheduled_memo` | *optional* | String | Optional memo regarding payment of funds.
`need_tax_deduction` | *optional* | Integer | 1 if tax needs to be withheld. 0 if not necessary.
`preset_withholding_tax_amount` | *optional* | Integer | Withholding tax amount
`withholding_tax_base` | *optional* | Integer | 1 if withholding tax includes sales tax, 2 if it does not.
`withholding_tax_segment` | *optional* | String | National Tax Agency tax code (ex: "nta2795" references https://www.nta.go.jp/taxanswer/gensen/2795.htm)
`tag_list` | *optional* | String | Optional tag code string.(Comma-separated)
`tag_name_list` | *optional* | String | Optional tag name string.(Comma-separated) **Only if tag_list is not provided.**

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"price_including_tax": 5400, "accrual_timestamp": "2015-10-31", "customer_master_code": "8201", "dept_code": "DEPT C", "reason_master_code": "BUYING_IN", "dc": "c", "memo": "Office Supplies for Frank", "tax_code": 0, "port_type": 1, "tag_list": "Payment,Foreign" }' https://tsubaiso.net/ap_payments/create
```

**/ap_payments/update/:id**

Description: Updates an accounts payable transaction. The updated transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/ap_payments/update/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "updated memo", "price_including_tax": 10800 }'  https://tsubaiso.net/ap_payments/update/6621
```

**/ap/destroy/:id**

Description: Destroys the accounts payable transaction specified as the id. Returns a status of 204 No Content.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/ap/destroy/:id
```

#### Customers

**/customer_masters/list/**

Description: Returns the entire list of customers.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/customer_masters/list/
```

Sample JSON Response:
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

Description: Returns a single customer.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/customer_masters/show/:id
```

Sample JSON response:
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
    id: 1000
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

Description: Create a new customer. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/customer_masters/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`name` | *required* | String | Name of customer (limit: 40 letters).
`name_kana` | *required* | String | Furigana for name (limit: 40 full-width katakana characters).
`code` | *required* | String | Code for the customer (limit: 10 half-width English characters, numbers, or hyphens).
`zip` | *optional* | String | Zip code.
`address` | *optional* | String | Address (limit: 80 characters).
`tel` | *optional* | String | Phone Number (limit: 40 numbers, hyphens, asterisks, or pounds).
`accountant_email` | *optional* | String | Accountant's email address. Invoices will be emailed to this address.
`dept_code` | *optional* | String | Code of department.
`tax_type_for_remittance_charge` | *required* | Integer | Tax on transaction fees. 3: Taxed, 0: Non-taxed.
`sender_name` | *optional* | String | Keyword to use when applying receipts(limit: 40 characters)
`locale` | *optional* | String | Language for invoice. "ja-JP": Japanese, "en": English.
`foreign_currency` | *optional* | Integer | Foreign currency transactions. 0: No, 1: Yes.
`used_in_ar` | *required* | Integer | Receivables classification. 0: No AR, 1: Accounts Receivable, 2: Non-trade Receivables.
`receive_closing_schedule` | *optional* | String | Invoice cut-off day for AR. "": No setting, "0": Upon Receipt, "1": Beginning of month, "5": 5th of each month, "10": 10th of each month, "15": 15th of each month, "18": 18th of each month,"20": 20th of each month, "25": 25th of each month, "-1": End of month.
`receive_sight` | *optional* | String | Payment deadline for AR. "": No setting, "0": Upon receipt, "1m5": 5th of cut-off month, "1m8": 8th of cut-off month, "1m10": 10th of cut-off month, "1m12": 12th of cut-off month, "1m15": 15th of cut-off month, "1m18": 18th of cut-off month, "1m20": 20th of cut-off month, "1m22": 22th of cut-off month, "1m25": 25th of cut-off month, "1m26": 26th of cut-off month, "1m27": 27th of cut-off month, "1m28": 28th of cut-off month, "1m-1": End of cut-off month, "2m5": 5th of following month, "2m8": 8th of following month, "2m10": 10th of month, "2m12": 12th of month, "2m15": 15th of following month, "2m18": 18th of following month, "2m20": 20th of following month, "2m22": 22th of following month, "2m25": 25th of following month, "2m26": 26th of following month, "2m27": 27th of following month, "2m28": 28th of following month, "2m-1": End of following month, "3m5": 5th day of two months after cut-off month, "3m8": 8th day of two months after cut-off month, "3m10": 10th day of two months after cut-off month, "3m12": 12th day of two months after cut-off month, "3m15": 15th day of two months after cut-off month, "3m18": 18th day of two months after cut-off month, "3m20": 20th day of two months after cut-off month, "3m22": 22th day of two months after cut-off month, "3m25": 25th day of two months after cut-off month, "3m26": 26th day of two months after cut-off month, "3m27": 27th day of two months after cut-off month, "3m28": 28th day of two months after cut-off month, "3m-1": End of two months after cut-off month, "4m5": 5th of three months after cut-off month, "4m8": 8th of three months after cut-off month, "4m10": 10th of three months after cut-off month, "4m12": 12th of three months after cut-off month, "4m15": 15th of three months after cut-off month, "4m18": 18th of three months after cut-off month, "4m20": 20th of three months after cut-off month, "4m22": 22th of three months after cut-off month, "4m25": 25th of three months after cut-off month, "4m26": 26th of three months after cut-off month, "4m27": 27th of three months after cut-off month, "4m28": 28th of three months after cut-off month, "4m-1": End of three months after cut-off month, "-1m5": 5th of preceding month, "-1m8": 8th of preceding month, "-1m10": 10th of preceding month, "-1m12": 12th of preceding month, "-1m15": 15th of preceding month, "-1m18": 18th of preceding month, "-1m20": 20th of preceding month, "-1m22": 22th of preceding month, "-1m25": 25th of preceding month, "-1m26": 26th of preceding month, "-1m27": 27th of preceding month, "-1m28": 28th of preceding month, "-1m-1": End of preceding month.
`bill_detail_round_rule` | *optional* | Integer | Rounding setting for quotes and invoices. 1: Round-down, 2: Round-up, 3: Round.
`used_in_ap` | *required* | Integer | Payables classification. 0: No AP, 1: Non-trade Payables, 2: Accounts Payable.
`pay_closing_schedule` | *optional* | String | Invoice cut-off day for AP. "": No setting, "0": Upon Receipt, "1": Beginning of month, "5": 5th of each month, "10": 10th of each month, "15": 15th of each month, "18": 18th of each month, "20": 20th of each month, "25": 25th of each month, "-1": End of month.
`pay_sight` | *optional* | String | Payment deadline for AP. "": No setting, "0": Upon receipt, "1m5": 5th of cut-off month, "1m8": 8th of cut-off month, "1m10": 10th of cut-off month, "1m12": 12th of cut-off month, "1m15": 15th of cut-off month, "1m18": 18th of cut-off month, "1m20": 20th of cut-off month, "1m22": 22th of cut-off month, "1m25": 25th of cut-off month, "1m26": 26th of cut-off month, "1m27": 27th of cut-off month, "1m28": 28th of cut-off month, "1m-1": End of cut-off month, "2m5": 5th of following month, "2m8": 8th of following month, "2m10": 10th of month, "2m12": 12th of month, "2m15": 15th of following month, "2m18": 18th of following month, "2m20": 20th of following month, "2m22": 22th of following month, "2m25": 25th of following month, "2m26": 26th of following month, "2m27": 27th of following month, "2m28": 28th of following month, "2m-1": End of following month, "3m5": 5th day of two months after cut-off month, "3m8": 8th day of two months after cut-off month, "3m10": 10th day of two months after cut-off month, "3m12": 12th day of two months after cut-off month, "3m15": 15th day of two months after cut-off month, "3m18": 18th day of two months after cut-off month, "3m20": 20th day of two months after cut-off month, "3m22": 22th day of two months after cut-off month, "3m25": 25th day of two months after cut-off month, "3m26": 26th day of two months after cut-off month, "3m27": 27th day of two months after cut-off month, "3m28": 28th day of two months after cut-off month, "3m-1": End of two months after cut-off month, "4m5": 5th of three months after cut-off month, "4m8": 8th of three months after cut-off month, "4m10": 10th of three months after cut-off month, "4m12": 12th of three months after cut-off month, "4m15": 15th of three months after cut-off month, "4m18": 18th of three months after cut-off month, "4m20": 20th of three months after cut-off month, "4m22": 22th of three months after cut-off month, "4m25": 25th of three months after cut-off month, "4m26": 26th of three months after cut-off month, "4m27": 27th of three months after cut-off month, "4m28": 28th of three months after cut-off month, "4m-1": End of three months after cut-off month, "-1m5": 5th of preceding month, "-1m8": 8th of preceding month, "-1m10": 10th of preceding month, "-1m12": 12th of preceding month, "-1m15": 15th of preceding month, "-1m18": 18th of preceding month, "-1m20": 20th of preceding month, "-1m22": 22th of preceding month, "-1m25": 25th of preceding month, "-1m26": 26th of preceding month, "-1m27": 27th of preceding month, "-1m28": 28th of preceding month, "-1m-1": End of preceding month.
`need_tax_deductions` | *optional* | String | Perform tax withholding or not. "0": No withholding, "1": Withholding.
`withholding_tax_segment` | *optional* | String | National Tax Agency tax code (ex: "nta2795" references https://www.nta.go.jp/taxanswer/gensen/2795.htm).
`withholding_tax_base` | *optional* | Integer | 1 if withholding tax includes sales tax, 2 if it does not.
`bank_code` | *optional* | String | Customer bank code (4 digits).
`bank_branch_code` | *optional* | String | Customer bank branch code (3 digits).
`bank_course` | *optional* | Integer | Type of bank account. 1: Savings, 2: Checking, 3: Time Deposit(Current), 4: Time Deposit(Fixed), 5; Installment Savings(Current), 6: Installment Savings(Fixed).
`bank_nominee` | *optional* | String | Name on bank account (limit: 30 characters).
`bank_account_number` | *optional* | String | Bank account number.
`is_valid` | *required* | Integer | Customer use status. 1: In use, 0: Not in use.

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"name": "テスト株式会社", "name_kana": "テストカブシキガイシャ", "code": "9000", "tax_type_for_remittance_charge": "3", "used_in_ar": 1, "used_in_ap": 1, "is_valid": 1 }' https://tsubaiso.net/customer_masters/create
```

**/customer_masters/update/:id**

Description: Updates a customer. The updated transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/customer_masters/update/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"name": "アップデート株式会社", "name_kana": "アップデートカブシキガイシャ"}' https://tsubaiso.net/customer_masters/update/1
```

**/customer_masters/destroy/:id**

Description: Deletes the customer with the specified id. Will return 204 No Content if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/customer_masters/destroy/:id
```

#### Staff

**/staffs/list/**

Description: Returns the entire list of staff.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/staffs/list/
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staffs/list
```

Sample JSON Response:
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

Description: Returns a single staff member.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/staffs/show/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staffs/show/1
```

Sample JSON Response:
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

Description: Returns all personal data entries for a staff member.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/staff_data/list?staff_id=:staff_id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" http://tsubaiso.net/staff_data/list/10000
```

Sample JSON Response:
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

Description: Returns a particular data entry for a staff member. Either an ID must be sent in the URL or both a staff ID and code must be sent in the body of the request. In addition, a time parameter can be sent if you would like to return the value for a specific time when there is more than one entry for a code.

Method: GET

URL Structure when sending ID:
``` sh
https://tsubaiso.net/staff_data/show/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staff_data/show/1234
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"staff_id": 10000, "code": "QUALIFICATION"}'  https://tsubaiso.net/staff_data/show
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"staff_id": 10000, "code": "QUALIFICATION", "time": "2001-01-01"}'  https://tsubaiso.net/staff_data/show
```

Sample JSON Response:
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

Description: Create a new data entry for a staff member. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/staff_data/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`staff_id` | *required* | Integer | ID of staff member.
`code` | *required* | String | Code for the data subject to be created.
`value` | *required* | String | Value of the data.
`memo` | *optional* | String | Memo for the data.
`start_timestamp` | *required* | String | The date from which this data applies.
`finish_timestamp` | *required or optional* | String | The date from which this data no longer applies. If no_finish_timestamp is nil or "0", this parameter must be provided.
`no_finish_timestamp` | *required or optional* | String | "1" if there should be no end date for the application of this data. If finish_timestamp is not provided, this parameter must be provided.

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -X POST -d '{"staff_id": 1000, "code": "NAME_MEI", "value": "Taro", "start_timestamp": "2015-12-15", "no_finish_timestamp": "1"}'  https://tsubaiso.net/staff_data/create
```

**/staff_data/update**

Description: Update a data entry for a staff member. The updated transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/staff_data/update/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -X POST -d '{"value": "1960-01-01"}'  http://tsubaiso.net/staff_data/update/1
```

**/staff_data/destroy/:id**

Description: Destroys the staff data specified as the id. Returns a status of 204 No Content.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/staff_data/destroy/:id
```

#### Staff Datum Master

**/staff_datum_masters/list/**

Description: Returns the entire list of staff datum masters.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/staff_datum_masters/list
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" http://tsubaiso.net/staff_datum_masters/list
```

Sample JSON Response:
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

Description: Returns a particular data entry for a staff datum master. Either an ID must be sent in the URL or code must be sent in the body of the request.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/staff_datum_masters/show/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/staff_datum_masters/show/1234
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"code": "BIRTH_YMD"}'  https://tsubaiso.net/staff_datum_masters/show
```

Sample JSON Response:
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

#### Journals

**/journals/list**

Description: This endpoint returns searches journal entries based on search criteria in the request body. The default amount of records is 20.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/journals/list
```

Parameters:

Parameter | Description
--- | ---
`id` | Search using the journal id directly.
`price` | Search for journals with the specified amount. Matching journals will be returned if either its price excluding tax or sales tax price match the specified price.
`memo` | Search journals that contain the specified keywords in their memo.
`dept_code` | Search journals that belong to the specified department.
`tag_list` | Search for journals that have the specified tag.
`start_date` | Search for journals that have a journal date after the specified date. Format needs to be "YYYY-MM-DD".
`finish_date` | Search for journals that have a journal date before the specified date. Format needs to be "YYYY-MM-DD".
`start_created_at` | Search for journals that have a create date after the specified date. Format needs to be "YYYY-MM-DD".
`finish_created_at` | Search for journals that have a create date before the specified date. Format needs to be "YYYY-MM-DD".
`account_code` | Search for journals that are using the specified account code.
`timestamp_order` | Specify the order in which the journals are displayed. "desc" for descending journal date order or "asc" for ascending. Default is descending.
`per_page` | Specify the amount of records to be returned. Default amount is 20 if there are more than 20 records. Maximum amount the can be returned per request is 200.
`page` | Specify which page of records to return. Default page is 1.

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXX" -X GET -d '{"account_code": "500", "start_date": "2014-01-01", "finish_date": "2014-12-31", "per_page": 2}' https://tsubaiso.net/journals/list
```

Sample JSON Response:
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

Description: This endpoint returns a single journal.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/journals/show/:id
```

Sample JSON response:
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

#### Manual Journals

**/manual_journals/list/:year/:month**

Description: This endpoint returns a list of manual journal entries for a particular month. If no year and month parameters are provided, it returns the entries for the current month.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/manual_journals/list/:year/:month
```

Sample JSON response:
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
                    "price_excluding_tax":10000,
                    "price_including_tax":10000,
                    "sales_tax":0
                },
                "credit":{
                    "account_code":"130",
                    "tax_type":0,
                    "price_excluding_tax":10000,
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
                    "price_excluding_tax":500000,
                    "price_including_tax":500000,
                    "sales_tax":0
                },
                "credit":{
                    "account_code":"330",
                    "tax_type":0,
                    "price_excluding_tax":1000000,
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
                    "price_excluding_tax":500000,
                    "price_including_tax":500000,
                    "sales_tax":0
                },
                "credit":{
                    "account_code":null,
                    "tax_type":null,
                    "price_excluding_tax":null,
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

Description: This endpoint returns a single manual journal entry.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/manual_journals/show/:id
```

Sample JSON response:
```
{
    "id":29068,
    "journal_timestamp":"2016/04/01 00:00:00 0900",
    "journal_dcs": [
        {
            "debit":{
                "account_code":"100",
                "tax_type":0,
                "price_excluding_tax":10000,
                "price_including_tax":10000,
                "sales_tax":0
            },
            "credit":{
                "account_code":"130",
                "tax_type":0,
                "price_excluding_tax":10000,
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

Description: Creates a new manual journal entry. The created entry will be sent back as JSON.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/manual_journals/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`journal_timestamp` | *required* | String | Journal entry date. Format must be "YYYY-MM-DD"
`journal_dcs` | *required* | Array of Object | Debit and Credit entries of the journal. Journal_dcs must be passed as an array (even if there is only one). One journal_dc can only contain one debit entry and/or one credit entry. A journal_dc does not have to be balanced but the total of all journal_dcs must be balanced.

*journal_dcs*

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`debit` | *optional* | Object | Debit information.
`credit` | *optional* | Object | Credit information.
`dept_code` | *optional* | String | Code of the internal department involved.
`memo` | *optional* | String | Memo for the manual journal.
`tag_list` | *optional* | String | Optional tag code string.(Comma-separated)
`tag_name_list` | *optional* | String | Optional tag name string.(Comma-separated) **Only if tag_list is not provided.**

*debit and credit*

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`account_code` | *required* | String | Account code for the journal entry.
`tax_type` | *required* | Integer | Tax type of the transaction.
`price_including_tax` | *required* | String | Amount of debit or credit including tax.
`sales_tax` | *optional* | Integer | Sales tax on the transaction. Is automatically calculated if not provided.

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"journal_timestamp": "2016-04-01", "journal_dcs" : [{"debit" : {"account_code" : "110", "price_including_tax" : 100000, "tax_type" : 0}, "credit" : {"account_code" : "100", "price_including_tax" : 100000, "tax_type" : 0}}]}' https://tsubaiso.net/manual_journals/create
```

**/manual_journals/update/:id**

Description: Updates a manual journal entry. The updated entry will be sent back as JSON if successful. **If journal_dcs are specified, the old journal_dcs will be deleted and replaced with the new journal_dcs**

Method: POST

URL Structure:
```sh
https://tsubaiso.net/manual_journals/update/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"journal_timestamp": "2016-05-01"}' https://tsubaiso.net/manual_journals/update/29072
```

**/manual_journals/destroy/:id**

Description: Deletes the manual journal entry specified as the id. Returns a status of 204 No Content.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/manual_journals/destroy/:id
```

#### Reimbursements

**/reimbursements/list/:year/:month**

Description: Returns the entire list of reimbursements by specific year and month.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/reimbursements/list/:year/:month
```

Sample JSON Response:
```
[
    {
        id: 212
        applicant: "ヤマカワ"
        application_term: "2007-07-01 00:00:00"
        applicant_staff_code: "EP0001"
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
        application_term: "2008-02-01 00:00:00"
        applicant_staff_code: "EP0002"
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

Description: Returns a single reimbursement.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/reimbursements/show/:id
```

Sample JSON response:
```
{
    id: 213
        applicant: "タカシ"
        application_term: "2008-02-01 00:00:00"
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

Description: Create a new reimbursement. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/reimbursements/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`application_term` | *required* | String | Date of issuing. Format must be "YYYY-MM-DD"
`applicant` | *required-optional* | String | Applicant name (limit: 20 characters). Applicant would be Required if applicant_staff_code doesn't exist.
`applicant_staff_code` | *optional* | String | Code of Staff.
`dept_code` | *optional* | String | Dept code which the default is "COMMON".
`memo` | *optional* | String | Memo for a reimbursement.

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXX" -X POST -d '{"application_term": "2015-09-01", "applicant":"ナカムラ", "memo":"Everything is okay", "applicant_staff_code":"EP2000"}' https://tsubaiso.net/reimbursements/create
```

**/reimbursements/update/:id**

Description: Updates a reimbursement. The updated transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/reimbursements/update/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"applicant": "アップデート株式会社", "term_application": "2016-10-01"}' https://tsubaiso.net/reimbursements/update/1
```

**/reimbursements/destroy/:id**

Description: Deletes the reimbursement with the specified id. Will return 204 No Content if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/reimbursements/destroy/:id
```

#### Reimbursement Transaction

**/reimbursement_transactions/list/:reimbursement_id**

Description: This endpoint returns a list of reimbursement transactions from a specific reimbursement.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/reimbursement_transactions/list/:reimbursement_id
```

Sample JSON response:
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

Description: This endpoint returns a single reimbursement transaction.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/reimbursement_transactions/show/:id
```

Sample JSON response:
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

Description: Creates a new reimbursement transaction. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/reimbursement_transactions/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`reimbursement_id` | *required* | Integer | Targeted reimbursement which is represented by reimbursement id.
`transaction_timestamp` | *required* | String | Actual date of the transaction. Format must be "YYYY-MM-DD"
`price_value` | *required* | Integer | Price value of a reimbursement transaction.
`reason_code` | *required* | String | Reason of the transaction.
`port_type` | *optional* | Integer | 1 for domestic transaction. 2 for foreign transaction.
`dc` | *optional* | String | Debit or credit. Default value is 'c' which represents credit.
`brief` | *optional* | String| Summary of the transaction.
`memo` | *optional* | String | Memo for the transaction.
`tag_list` | *optional* | String | Optional tag code string.(Comma-separated)
`tag_name_list` | *optional* | String | Optional tag name string.(Comma-separated) **Only if tag_list is not provided.**
`tax_type` | *optional* | String | Tax type of the transaction.

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"reimbursement_id": 106, "transaction_timestamp": "2016-05-01", "price_value": 10000, "reason_code": "SUPPLIES", "port_type": 1, "dc": "c", "brief": "test brief", "memo": "test memo", "tag_list": "Education,Japan", "tax_type": "1003"}' https://tsubaiso.net/reimbursement_transactions/create
```

**/reimbursement_transactions/update/:id**

Description: Updates a reimbursement transaction. The updated transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/reimbursement_transactions/update/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "updated memo", "price_value": 10800 }'  https://tsubaiso.net/reimbursement_transactions/update/8833
```

**/reimbursement_transactions/destroy/:id**

Description: Destroys the reimbursement transaction specified as the id. Returns a status of 204 No Content.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/reimbursement_transactions/destroy/:id
```

#### Departments

**/depts/list/**

Description: This endpoint returns a list of departments.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/depts/list
```

Sample JSON response:
```
[
  {
    "ccode"      : 3 ,
    "code"       : "SETSURITSU" ,
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

Description: This endpoint returns a single department.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/depts/show/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" http://tsubaiso.net/depts/show/1
```

Sample JSON response:
```
{
 "ccode"      : 3 ,
 "code"       : "SETSURITSU" ,
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

Description: Create a new department. The created department will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/depts/create
```

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`code` | *required* | String | Department code. Up to 16 single-byte characters, hyphens, underscores or periods.
`name` | *required* | String | Department name. Up to 32 characters.
`name_abbr` | *optional* | String | Abbreviation. Up to 16 characters.
`color` | *optional* | String | Color. (HTML Color Codes)
`memo`| *optional* | String | Memo. Up to 40 characters.
`start_date` | *required* | String | Start date. Format must be "YYYY/MM/DD".
`finish_date` | *optional* | String | Finish date. Format must be ""YYYY/MM/DD".

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"code":"API_TEST", "name":"api_test","name_abbr": "AT", "start_date":"2016/04/01","color": "#ffffff"}' http://tsubaiso.net/depts/create
```

Sample JSON response:
```
{
 "ccode"      : 3 ,
 "code"       : "API_TEST" ,
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

Description: Update a department. The updated department will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/depts/update/:id

```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"name": "アップデート"}' https://tsubaiso.net/depts/update/1
```

**/depts/destroy/:id**

Description: Deletes the department with the specified id. Will return 204 No Content if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/depts/destroy/:id
```

#### Tags

**/tags/list**

Description: This endpoint returns a list of tags.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/tags/list
```

Sample JSON response:
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

Description: This endpoint returns information for a single tag.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/tags/show/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" http://tsubaiso.net/tags/show/1
```

Sample JSON response:
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

Description: Creates a new tag. The created tag will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/tags/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`code` | *required* | String | Identification code. Alphanumeric, underscore, hyphen, 50 chars below
`name` | *required* | String | Tag name. Actual Name of Tag
`sort_no` | *required* | Integer | Sort Order
`tag_group_code` | *required* | String | Tag Group Identification code
`start_ymd` | *required* | Datetime | Start date "YYYY/MM/DD" format
`finish_ymd` | *optional* | Datetime | End date "YYYY/MM/DD" format

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXX" -X POST -d '{"code":"APITEST_code","name":"APITEST_name","sort_no":"919","tag_group_code":"PROJECT","start_ymd":"2016/08/18","finish_ymd":"2016/08/23"}' https://tsubaiso.net/tags/create
```

**/tags/update/:id**

Description: Updates a tag. The updated tag will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/tags/update/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXX" -X POST -d '{"code":"APITEST_code","name":"APITEST_update_name","sort_no":"919","tag_group_code":"PROJECT","start_ymd":"2016/08/18","finish_ymd":"2016/08/23"}' https://tsubaiso.net/tags/update/4789
```

**/tags/destroy/:id**

Description: Destroys the tag by id. Returns a status of 204 No Content.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/tags/destroy/:id
```
