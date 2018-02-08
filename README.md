# Tsubaiso API (beta)

This is the documentation for the beta version of the Tsubaiso API. The beta version currently handles accounts receivables, accounts payable transactions, customer management and staff/staff data management. Future versions of this API will add new endpoints to access other modules of the Tsubaiso system.

## Table Of Contents

 - [Root Endpoint](#root-endpoint)
 - [Request Format](#request-format)
 - [Authentication](#authentication)
 - [Response Codes and Error Handling](#response-codes-and-error-handling)
 - [Resources](#resources)
   - [Accounts Receivables](#accounts-receivables)
   - [Accounts Payables](#accounts-payables)
   - [Customers](#customers)
   - [Staff](#staff)
   - [Staff Data](#staff-data)
   - [Staff Datum Master](#staff-datum-master)
   - [Journals](#journals)
   - [Manual Journals](#manual-journals)
   - [Reimbursements](#reimbursements)
   - [Reimbursement Transaction](#reimbursement-transaction)
   - [Departments](#departments)
   - [Tags](#tags)
   - [Reimbursement Reason Masters](#reimbursement-reason-masters)
   - [Ar Reason Masters](#ar-reason-masters)
   - [Ap Reason Masters](#ap-reason-masters)
   - [Bonuses](#bonuses)
   - [Payrolls](#payrolls)
   - [Journal Distributions](#journal-distributions)
   - [Monthly Balances](#monthly-balances)
   - [Bank Accounts](#bank-accounts)
   - [Bank Account Masters](#bank-account-masters)
   - [Bank Reason Masters](#bank-reason-masters)
- [Data Partners](#data-partners)

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
`tax_code` | *optional* | Integer | Tax code for the transaction. (If this parameter is omitted, it will use the tax_code associated with the reason_master_code.)
`dept_code` | *optional* | String | Code of the internal department involved.
`sales_tax` | *optional* | Integer | Sales tax on the transaction. Is automatically calculated if not provided.
`scheduled_receive_timestamp` | *optional* | String | Date of receipt. Format must be “YYYY-MM-DD”.
`scheduled_memo` | *optional* | String | Optional memo regarding receipt of funds.
`tag_list` | *optional* | String | Optional segment(formerly tag) code string.(Comma-separated)
`tag_name_list` | *optional* | String | Optional segment(formerly tag) name string.(Comma-separated) **Only if tag_list is not provided.**
`data_partner` | *optional* | Object | See [data partners](#data-partners) section for more details.

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

**/ar_receipts/balance/:year/:month**

Description: This endpoint returns list of customer's account receivable amount from given year and month parameters. If no year and month parameters are provided, it returns the list for the current month.

Method: GET

URL Structure:

``` sh
https://tsubaiso.net/ar_receipts/balance/:year/:month
```

Sample Request :
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ar_receipts/balance/2017/1
```

Sample JSON Response:
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
`port_type` | *required* | Integer | 1 for domestic transaction. 2 for foreign transaction.
`tax_code` | *optional* | Integer | Tax code for the transaction. (If this parameter is omitted, it will use the tax_code associated with the reason_master_code.)
`dept_code` | *optional* | String | Code of the internal department involved.
`buying_tax` | *optional* | Integer | Sales tax on the transaction. Is automatically calculated if not provided.
`scheduled_pay_timestamp` | *optional* | String | Date of payment. Format must be "YYYY-MM-DD".
`scheduled_memo` | *optional* | String | Optional memo regarding payment of funds.
`need_tax_deduction` | *optional* | Integer | 1 if tax needs to be withheld. 0 if not necessary.
`preset_withholding_tax_amount` | *optional* | Integer | Withholding tax amount
`withholding_tax_base` | *optional* | Integer | 1 if withholding tax includes sales tax, 2 if it does not.
`withholding_tax_segment` | *optional* | String | National Tax Agency tax code (ex: "nta2795" references https://www.nta.go.jp/taxanswer/gensen/2795.htm)
`tag_list` | *optional* | String | Optional segment(formerly tag) code string.(Comma-separated)
`tag_name_list` | *optional* | String | Optional segment(formerly tag) name string.(Comma-separated) **Only if tag_list is not provided.**
`data_partner` | *optional* | Object | See [data partners](#data-partners) section for more details.

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

**/ap_payments/balance/:year/:month**

Description: This endpoint returns list of customer's account payable amount from given year and month parameters. If no year and month parameters are provided, it returns the list for the current month.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/ap_payments/balance/:year/:month
```

Sample Request :
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ap_payments/balance/2017/1
```

Sample JSON Response :
``` sh
[
  {
    "created_at": null,
    "customer_master_id": 105,
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
    "login": "XXXXX",
    "seimei": "税務 太郎",
    "seimei_furigana": "ゼイム タロウ"
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
`tag_list` | Search for journals that have the specified segment(formerly tag).
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
`data_partner` | *optional* | Object | See [data partners](#data-partners) section for more details.
`journal_dcs` | *required* | Array of Object | Debit and Credit entries of the journal. Journal_dcs must be passed as an array (even if there is only one). One journal_dc can only contain one debit entry and/or one credit entry. A journal_dc does not have to be balanced but the total of all journal_dcs must be balanced.

*journal_dcs*

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`debit` | *optional* | Object | Debit information.
`credit` | *optional* | Object | Credit information.
`dept_code` | *optional* | String | Code of the internal department involved.
`memo` | *optional* | String | Memo for the manual journal.
`tag_list` | *optional* | String | Optional segment(formerly tag) code string.(Comma-separated)
`tag_name_list` | *optional* | String | Optional segment(formerly tag) name string.(Comma-separated) **Only if tag_list is not provided.**

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
`tag_list` | *optional* | String | Optional segment(formerly tag) code string.(Comma-separated)
`tag_name_list` | *optional* | String | Optional segment(formerly tag) name string.(Comma-separated) **Only if tag_list is not provided.**
`tax_type` | *optional* | String | Tax type of the transaction.
`data_partner` | *optional* | Object | See [data partners](#data-partners) section for more details.

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

Description: Create a new department. The created department will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/depts/create
```

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`code` | *required* | String | Department code. Up to 16 single-byte characters, hyphens, underscores or periods.
`sort_no` | *optional* | Integer | Sort Order
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

Description: This endpoint returns a list of segments(formerly tags).

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

Description: This endpoint returns information for a single segment(formerly tag).

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

Description: Creates a new segment(formerly tag). The created segment(formerly tag) will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/tags/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`code` | *required* | String | Identification code. Alphanumeric, underscore, hyphen, 50 chars below
`name` | *required* | String | Segment(formerly Tag) name. Actual Name of Segment(formerly Tag)
`sort_no` | *required* | Integer | Sort Order
`tag_group_code` | *required* | String | Segment(formerly Tag) Group Identification code
`start_ymd` | *required* | Datetime | Start date "YYYY/MM/DD" format
`finish_ymd` | *optional* | Datetime | End date "YYYY/MM/DD" format

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXX" -X POST -d '{"code":"APITEST_code","name":"APITEST_name","sort_no":"919","tag_group_code":"PROJECT","start_ymd":"2016/08/18","finish_ymd":"2016/08/23"}' https://tsubaiso.net/tags/create
```

**/tags/update/:id**

Description: Updates a segment(formerly tag). The updated segment(formerly tag) will be sent back as JSON if successful.

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

Description: Destroys the segment(formerly tag) by id. Returns a status of 204 No Content.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/tags/destroy/:id
```

#### Reimbursement Reason Masters

**/reimbursement_reason_masters/list/**

Description: Returns the entire list of reimbursement reason masters.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/reimbursement_reason_masters/list/
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/reimbursement_reason_masters/list
```

Sample JSON Response:
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

Description: Returns a single reimbursement reason masters member.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/reimbursement_reason_masters/show/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/reimbursement_reason_masters/show/1
```

Sample JSON Response:
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

#### Ar Reason Masters

**/ar_reason_masters/list/**

Description: Returns the entire list of ar reason masters

Method: GET

URL Structure:

```sh
https://tsubaiso.net/ar_reason_masters/list/
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/ar_reason_masters/list
```

Sample JSON Response:
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

Description: Returns a single ar reason masters

Method: GET

URL Structure:
```sh
https://tsubaiso.net/ar_reason_masters/show/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXX" http://tsubaiso.net/ar_reason_masters/show/1
```

Sample JSON Response:
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

#### Ap Reason Masters

**/ap_reason_masters/list**

Description: This endpoint returns a list of ap reason masters.

Method: GET

URL Structure:

```sh
https://tsubaiso.net/ap_reason_masters/list
```

Sample Request:

```sh
 +curl -H "Content-Type: application/json" -H "Accept: application/json" -H"Access-Token: XXXXXXXXXX" http://tsubaiso.net/ap_reason_masters/list
```

Sample JSON response:

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

Description: This endpoint returns information for a single ap reason masters.

Method: GET

URL Structure:

``` sh
https://tsubaiso.net/ap_reason_masters/show/:id
```

Sample Request:

```sh
 +curl -H "Content-Type: application/json" -H "Accept: application/json" -H"Access-Token: XXXXXXXXXX" http://tsubaiso.net/ap_reason_masters/show/1
```

Sample JSON response:

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
 +  "memo": "仕入高とは、商品や原材料の仕入、外注費等、会社の主たる営業活動にかかわる費用をいいます。売上高と直接対応する費用である点が特徴です。",
  "port_type": null,
  "reason_code": "BUYING_IN",
  "reason_name": "仕入高",
  "regist_user_code": null,
  "release_version": 0,
  "sales_tax_system": null,
  "sort_number": 10
}
```

#### Bonuses

**/bonuses/list/**

Description: This endpoint returns list of bonuses from given bonus_no(sequence number or bonus) and target_year(year of payment) parameters. If no bonus_no or target_year parameters provided, it will raise an error.

Method: GET

URL Structure:

```sh
https://tsubaiso.net/bonuses/list/
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"bonus_no": 1, "target_year": "2016" }' https://tsubaiso.net/bonuses/list/
```

Sample JSON Response:
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

Description: Returns a single bonuses member

Method: GET

URL Structure:
```sh
https://tsubaiso.net/bonuses/show/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bonuses/show/650504937
```

Sample JSON Response:
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

#### Payrolls

**/payrolls/list/**

Description: Returns the entire list of payrolls based year and month.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/payrolls/list/:year/:month
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/payrolls/list/2016/11
```

Sample JSON Response:
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

Description: Returns a single payrolls member.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/payrolls/show/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/payrolls/show/1071569834
```

Sample JSON Response:
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

#### Journal Distributions

**/journal_distributions/create**

Descripion: Create a new journal distribution entry based on journals that have been created previously. Journal distribution created by distributing price over selected departments or segments. The created entry will be sent back as JSON.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/journal_distributions/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`target_timestamp` | *required* | String | Target date of the journal distribution. Format must be "YYYY-MM-DD".
`criteria` | *required* | String | Allocation criteria for journal distribution that being distributed. Only available options are "dept" for departments ot "segment" for segments.
`distribution_conditions` | *required* | Object | Departments or segments to be allocated within journal distribution. ``` {dept_code1 : ratio, dept_code2 : ratio,...} or {segment_code1 : ratio, segment_code2 : ratio,...} ```
`memo` | *optional* | String | Memo for the entry.
`search_conditions` | *required* | Object | See below.

*search_conditions*

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`start_date` | *required* | String | Start date of journal timestamp to be searched. Format must be "YYYY-MM-DD".
`finish_date` | *required* | String | Finish date of journal timestamp to be searched. Format must be "YYYY-MM-DD".
`account_codes` | *required* | Array of String | Search for journals that are using the specified account code.
`dept_code` | *optional*\*| String | Code of the internal department involved.
`tag_list` | *optional*\*| String | Segment(formerly tag) code string.(Comma-separated).

\*Either one of dept_code or tag_list must be provided.

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -d '{ "search_conditions": { "start_date": "2016-09-01", "finish_date": "2016-09-30", "account_codes": ["135~999","604"], "dept_code": "SHOP" }, "memo": "", "criteria": "dept", "target_timestamp": "2017-02-01", "distribution_conditions": { "SETSURITSU": "1", "HEAD": "1" } }'  https://tsubaiso.net/journal_distributions/create
```

**/journal_distribution/destroy/:id**

Description: Destroy the journal distribution specified as the id. Returns a status of 204 No Content

Method: POST

URL Structure:
```sh
https://tsubaiso.net/journal_distributions/destroy/:id
```

#### Monthly Balances

**/balances/list**

Description: This endpoint shows the company wide or department filtered monthly balances for a specified timespan.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/balances/list
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`year` | *required* | String | Year (last).
`month` | *required* | String | Month (last).
`m_span` | *optional* | Object | Number of prior months balances to show preceding the specified `year` and `month`. Default: 12
`dept_code` | *optional* | String | Department code. Default: Company wide balances.


Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXXXXX" -X GET -d '{ "year" : 2017, "month" : 11, "m_span" : 12, "dept_code" : "SALES" }'  https://tsubaiso.net/balances/list
```

Sample JSON Response:
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

#### Bank Account Masters

**/bank_account_masters/list**

Description: Returns the entire list of bank account masters.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/bank_account_masters/list/
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_account_masters/list/
```

Sample JSON response:
```
[
  {
    "id": 0,
    "name": "xxxxx",
    "account_type": "1",
    "account_number": "xxxxx",
    "nominee": "xxxxx",
    "account_code": "111",
    "zengin_bank_code": "0000",
    "zengin_branch_code": "0000",
    "dept_code": "HEAD",
    "memo": "xxxxx",
    "regist_user_code": null,
    "update_user_code": null,
    "start_ymd": "2001/01/01",
    "finish_ymd": null,
    "zengin_client_code_sogo": null,
    "currency_code": null,
    "currency_rate_master_id": null,
    "created_at": "2017/12/11 17:21:03 +0900",
    "updated_at": "2017/12/11 17:21:03 +0900"
  },
  {
    "id": 1,
    "name": "xxxxx",
    "account_type": "1",
    "account_number": "xxxxx",
    "nominee": "xxxxx",
    "account_code": "111",
    "zengin_bank_code": "0000",
    "zengin_branch_code": "0000",
    "dept_code": "HEAD",
    "memo": "xxxxx",
    "regist_user_code": null,
    "update_user_code": null,
    "start_ymd": "2001/01/01",
    "finish_ymd": null,
    "zengin_client_code_sogo": null,
    "currency_code": null,
    "currency_rate_master_id": null,
    "created_at": "2017/12/11 17:21:03 +0900",
    "updated_at": "2017/12/11 17:21:03 +0900"
  }
]
```

**/bank_account_masters/show/:id**

Description: Returns a single bank account master.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/bank_account_masters/show/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_account_masters/show/0
```

Sample JSON Response:
```
{
  "id": 0,
  "name": "xxxxx",
  "account_type": "1",
  "account_number": "xxxxx",
  "nominee": "xxxxx",
  "account_code": "111",
  "zengin_bank_code": "0000",
  "zengin_branch_code": "0000",
  "dept_code": "HEAD",
  "memo": "xxxxx",
  "regist_user_code": null,
  "update_user_code": null,
  "start_ymd": "2001/01/01",
  "finish_ymd": null,
  "zengin_client_code_sogo": null,
  "currency_code": null,
  "currency_rate_master_id": null,
  "created_at": "2017/12/11 17:21:03 +0900",
  "updated_at": "2017/12/11 17:21:03 +0900"
}
```

#### Bank Accounts

**/bank_accounts/list**

Description: Returns the entire list of bank accounts.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/bank_accounts/list/
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_accounts/list/
```

Sample JSON response:
```
[
  {
    "id": 0,
    "bank_account_master_id": 0,
    "is_closed": 0,
    "is_ok": 0,
    "start_timestamp": "2008/07/01 00:00:00 +0900",
    "finish_timestamp": "2020/01/31 00:00:00 +0900",
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
    "bank_account_master_id": 0,
    "is_closed": 0,
    "is_ok": 0,
    "start_timestamp": "2008/07/01 00:00:00 +0900",
    "finish_timestamp": "2020/01/31 00:00:00 +0900",
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

Description: Returns a single bank account.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/bank_accounts/show/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_accounts/show/0
```

Sample JSON Response:
```
{
  "id": 0,
  "bank_account_master_id": 0,
  "is_closed": 0,
  "is_ok": 0,
  "start_timestamp": "2008/07/01 00:00:00 +0900",
  "finish_timestamp": "2020/01/31 00:00:00 +0900",
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
#### Bank Reason Masters

**/bank_reason_masters/list**

Description: Returns the entire list of bank reason masters.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/bank_reason_masters/list/
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_reason_masters/list/
```

Sample JSON response:
```
[
  {
    "id": 100,
    "ccode": 1,
    "allowed_domains": null,
    "sort_number": 0,
    "reason_code": "xxxx",
    "reason_name": "xxxx",
    "dc": "d",
    "account_code": "500",
    "is_valid": 1,
    "memo": "xxxx",
    "created_at": "2017/12/11 17:20:48 +0900",
    "regist_user_code": null,
    "updated_at": "2017/12/11 17:20:48 +0900",
    "update_user_code": null
  },
  {
    "id": "xxxx",
    "ccode": 1,
    "allowed_domains": null,
    "sort_number": 0,
    "reason_code": "xxxx",
    "reason_name": "xxxx",
    "dc": "d",
    "account_code": "500",
    "is_valid": 1,
    "memo": "xxxx",
    "created_at": "2017/12/11 17:20:48 +0900",
    "regist_user_code": null,
    "updated_at": "2017/12/11 17:20:48 +0900",
    "update_user_code": null
  }
]
```

**/bank_reason_masters/show/:id**

Description: Returns a single bank reason master.

Method: GET

URL Structure:
```sh
https://tsubaiso.net/bank_reason_masters/show/:id
```

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" https://tsubaiso.net/bank_reason_masters/show/0
```

Sample JSON Response:
```
{
    "id": 100,
    "ccode": 1,
    "allowed_domains": null,
    "sort_number": 0,
    "reason_code": "xxxx",
    "reason_name": "xxxx",
    "dc": "d",
    "account_code": "500",
    "is_valid": 1,
    "memo": "xxxx",
    "created_at": "2017/12/11 17:20:48 +0900",
    "regist_user_code": null,
    "updated_at": "2017/12/11 17:20:48 +0900",
    "update_user_code": null
}
```

#### Bank Account Transaction

**/bank_account_transactions/index/**

Description: This endpoint returns a list of bank account transactions.


Description: Returns a single bank account transaction.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/bank_account_transactions/show/:id
```

Sample JSON response:
```
{
    "id":288,
    "personal_id":3,
    "serial_no":1,
    "journal_dc_id":23072,
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

**/bank_account_transactions/create/:bank_account_id**

Description: Create a bank account transaction. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/bank_account_transactions/create/:bank_account_id
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`journal_timestamp` | *required* | String | Date of journal. Format must be “YYYY-MM-DD”.
`price_value` | *required* | Integer | Price value of a bank account transaction.
`reason_code` | *required* | String | Reason of the transaction.
`dc` | *optional* | String | 'd' if the transaction was a debit to AR, 'c' if it was a credit.
`brief` | *optional* | String| Summary of the transaction.
        `memo` | *optional* | String | Memo for the transaction.
`tag_list` | *optional* | String | Optional segment(formerly tag) code string.(Comma-separated)
        `dept_code` | *optional* | String | Code of the internal department involved.

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"journal_timestamp": "2018-02-04", "price_value": 10000, "reason_code": "xxxx_123", "dc": "d", "brief": "test brief", "memo": "test memo", "tag_list": "GROUP3_1, GROUP2_2", "dept_code": "NEVER_ENDING"}' https://tsubaiso.net/bank_account_transactions/create/1000000001
```

**/bank_account_transactions/update/:id**

Description: Updates a bank account transaction. The updated transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/bank_account_transactions/update/:id
```

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"memo": "updated memo", "price_value": 10800 }'  https://tsubaiso.net/reimbursement_transactions/update/8833
```

**/bank_account_transactions/destroy/:id**

Description: Destroys the bank account transaction specified as the id. Returns a status of 204 No Content.

Method: POST

### Data Partners

Description: For certain resources, additional metadata can be provided when creating a transaction regarding the data source that the created transaction is connected to.

Applicable resources: Accounts Receivables, Accounts Payables, Reimbursement Transactions, Manual Journals

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`partner_code` | *optional* | String | Reference code for the connected transaction.
`link_url` | *optional* | String | URL where the connected transaction can be accessed from.
`editable` | *optional* | Integer | 1 if you want the transaction to be editable by users who are below manager level. (Default is 0)
`deletable` | *optional* | Integer | 1 if you want the transaction to be deletable by users who are below manager level. (Default is 0)
`partner_editable` | *optional* | Integer | 1 if you want the transaction to be editable via API requests. (Default is 1)
`partner_deletable` | *optional* | Integer | 1 if you want the transaction to be deletable via API requests. (Default is 1)
