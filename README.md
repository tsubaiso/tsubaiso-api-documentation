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

You can create an access token from the page below: (Tsubaiso account required)

[Get an API access token](https://tsubaiso.net/api_keys)

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

Description: This endpoint returns a single accounts receivable transaction.

Method: GET

URL Structure:
``` sh
https://tsubaiso.net/ar/show/:id
```

Sample JSON response:
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

Description: Creates a new accounts receivable transaction. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/ar/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`price` | *required* | Integer | Amount of the transaction.
`realization_timestamp` | *required* | String | Actual date of the transaction. Format must be "YYYY-MM-DD"
`customer_master_code` | *required* | String | Code of the transaction party.
`reason_master_code` | *required* | String | Reason of the transaction. This is used to create the journal entry.
`dc` | *required* | String | 'd' if the transaction was a debit to AR, 'c' if it was a credit.
`memo` | *required* | String | Memo for the transaction. Can be blank but must be provided.
`year` | *optional* | Integer | Year of the transaction. If provided, month must be provided as well. Will use current year if not provided.
`month` | *optional* | Integer | Month of the transaction. If provided, year must be provided as well. Will use current month if not provided.
`tax_code` | *required* | Integer | Tax code for the transaction.
`dept_code` | *optional* | String | Code of the internal department involved.
`sales_tax` | *optional* | Integer | Sales tax on the transaction. Is automatically calculated if not provided.
`scheduled_receipt_timestamp` | *optional* | String | Date of receipt. Format must be “YYYY-MM-DD”.
`scheduled_memo` | *optional* | String | Optional memo regarding receipt of funds.

Sample Request:
```sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"year": 2015, "month": 10, "price": 5000, "realization_timestamp": "2015-10-31", "customer_master_code": "101", "dept_code": "DEPT A", "reason_master_code": "SALES", "dc": "d", "memo": "500 widgets", "tax_code": 0}' https://tsubaiso.net/ar/create
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

Description: Creates a new accounts payable transaction. The created transaction will be sent back as JSON if successful.

Method: POST

URL Structure:
```sh
https://tsubaiso.net/ap_payments/create
```

Parameters:

Parameter | Necessity | Type | Description
--- | --- | --- | ---
`price` | *required* | Integer | Amount of the transaction.
`accrual_timestamp` | *required* | String | Actual date of the transaction. Format must be "YYYY-MM-DD"
`customer_master_code` | *required* | String | Code of the transaction party.
`reason_master_code` | *required* | String | Reason of the transaction. This is used to create the journal entry.
`dc` | *required* | String | 'd' if the transaction was a debit to AP, 'c' if it was a credit.
`memo` | *required* | String | Memo for the transaction. Can be blank but must be provided.
`tax_code` | *required* | Integer | Tax code for the transaction.
`port_type` | *required* | Integer | 1 for domestic transaction. 2 for foreign transaction.
`year` | *optional* | Integer | Year of the transaction. If provided, month must be provided as well. Will use current year if not provided.
`month` | *optional* | Integer | Month of the transaction. If provided, year must be provided as well. Will use current month if not provided.
`dept_code` | *optional* | String | Code of the internal department involved.
`buying_tax` | *optional* | Integer | Sales tax on the transaction. Is automatically calculated if not provided.
`scheduled_pay_timestamp` | *optional* | String | Date of payment. Format must be "YYYY-MM-DD".
`scheduled_memo` | *optional* | String | Optional memo regarding payment of funds.
`need_tax_deduction` | *optional* | Integer | 1 if tax needs to be withheld. 0 if not necessary.
`preset_withholding_tax_amount` | *optional* | Integer | Withholding tax amount
`withholding_tax_base` | *optional* | Integer | 1 if withholding tax includes sales tax, 2 if it does not.
`withholding_tax_segment` | *optional* | String | National Tax Agency tax code (ex: "nta2795" references https://www.nta.go.jp/taxanswer/gensen/2795.htm)

Sample Request:
``` sh
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X POST -d '{"year": 2015, "month": 10, "price": 5000, "accrual_timestamp": "2015-10-31", "customer_master_code": "8201", "dept_code": "DEPT C", "reason_master_code": "BUYING_IN", "dc": "c", "memo": "Office Supplies for Frank", "tax_code": 0, "port_type": 1 }' https://tsubaiso.net/ap_payments/create
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
`receive_closing_schedule` | *optional* | String | Invoice cut-off day for AR. "": No setting, "0": Upon Receipt, "1": Beginning of month, "5": 5th of each month, "10": 10th of each month, "15": 15th of each month, "20": 20th of each month, "25": 25th of each month, "-1": End of month.
`receive_sight` | *optional* | String | Payment deadline for AR. "": No setting, "0": Upon receipt, "1m20": 20th of cut-off month, "1m27": 27th of cut-off month, "1m-1": End of cut-off month, "2m5": 5th of following month, "2m10": 10th of month, "2m15": 15th of following month, "2m20": 20th of following month, "2m25": 25th of following month, "2m27": 27th of following month, "2m-1": End of following month, "3m5": 5th day of two months after cut-off month, "3m10": 10th day of two months after cut-off month, "3m15": 15th day of two months after cut-off month, "3m20": 20th day of two months after cut-off month, "3m25": 25th day of two months after cut-off month, "3m-1": End of two months after cut-off month, "4m5": 5th of three months after cut-off month, "4m10": 10th of three months after cut-off month, "4m15": 15th of three months after cut-off month, "-1m20": 20th of preceding month, "-1m-1": End of preceding month.
`bill_detail_round_rule` | *optional* | Integer | Rounding setting for quotes and invoices. 1: Round-down, 2: Round-up, 3: Round.
`used_in_ap` | *required* | Integer | Payables classification. 0: No AP, 1: Non-trade Payables, 2: Accounts Payable.
`pay_closing_schedule` | *optional* | String | Invoice cut-off day for AP. "": No setting, "0": Upon Receipt, "1": Beginning of month, "5": 5th of each month, "10": 10th of each month, "15": 15th of each month, "20": 20th of each month, "25": 25th of each month, "-1": End of month.
`pay_sight` | *optional* | String | Payment deadline for AP. "": No setting, "0": Upon receipt, "1m20": 20th of cut-off month, "1m27": 27th of cut-off month, "1m-1": End of cut-off month, "2m5": 5th of following month, "2m10": 10th of month, "2m15": 15th of following month, "2m20": 20th of following month, "2m25": 25th of following month, "2m27": 27th of following month, "2m-1": End of following month, "3m5": 5th day of two months after cut-off month, "3m10": 10th day of two months after cut-off month, "3m15": 15th day of two months after cut-off month, "3m20": 20th day of two months after cut-off month, "3m25": 25th day of two months after cut-off month, "3m-1": End of two months after cut-off month, "4m5": 5th of three months after cut-off month, "4m10": 10th of three months after cut-off month, "4m15": 15th of three months after cut-off month, "-1m20": 20th of preceding month, "-1m-1": End of preceding month.
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

**/customer_masters/update**

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
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"staff_id": 10000, "code": "BIRTH_YMD"}'  https://tsubaiso.net/staff_data/show
curl -i -H "Content-Type: application/json" -H "Accept: application/json" -H "Access-Token: XXXXXXXXXXXXXX" -X GET -d '{"staff_id": 10000, "code": "BIRTH_YMD", "time": "2001-01-01"}'  https://tsubaiso.net/staff_data/show
```

Sample JSON Response:
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

