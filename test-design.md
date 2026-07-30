> **1.** **FUNCTIONAL** **SCENARIOS**

<img src="./dvjxdwh2.png"
style="width:10.29702in;height:1.47929in" />TEST CASE ID TEST CASE TITLE

TC_FUNC_001 Successful transer

TC_FUNC_002 Unsupported currency

TC_FUNC_003 Insufficient balance

TC_FUNC_004 Invalid receiver Wallet ID

TC_FUNC_005 Invalid sender Wallet ID

TC_FUNC_006 Transfer requred amount

TC_FUNC_007 Transfer above maximum limit

> **2.** **BOUNDARY** **VALUE** **ANALYSIS**

TEST CASE ID BOUNDARY VALUE

TC_BVA_001 £0.00

TC_BVA_002 £1.00

TC_BVA_003 £4,999.99

TC_BVA_004 £5,000.00

TC_BVA_005 £5,000.01

TC_BVA_006 £9,999.99

TC_BVA_007 £10,000.00

TC_BVA_008 £10,000.01

SCENARIO TYPE

Positive

Negative

Negative

Negative

Negative

Positive/Review

Negative

BOUNDARY VALUE CATEGORY

Below minimum limit

Minimum limit allowed

Just below approval limit

At limit

Just above approval limit

Below maximum limit

At maximum limit

Just above maximum limit

PRE-CONDITIONS TEST STEPS TEST DATA EXPECTED HTTP CODE

\- Sender has sufficient wallet balance (\>£500) - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 201 Created - Enter
valid walletId, amount and accepted "receiverWalletId": "WALLET002",

> currency (GBP, EUR, USD) "amount": 500.00, "currency": "GBP"}

System is active - Send POST /api/v1/transfers/ - 400 Bad Request -
Enter unaccepted currency {"senderWalletId": "WALLET001",

> "receiverWalletId": "WALLET002", "amount": 500.00,
>
> "currency": "JPY"}

\- Sender has an insufficient wallet balance (\< - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 400 Bad Request
£1) - Enter amount greater than wallet balance "receiverWalletId":
"WALLET002",

> "amount": 500.00, "currency": "GBP"}

System is active - Send POST /api/v1/transfers/ {"senderWalletId":
"WALLET001", - 404 Not Found - Enter non-existent or invalid
"receiverWalletId": "Invalid",

> receiverWalletId "amount": 500.00, - Other fields remain valid
> "currency": "GBP"}

System is active - Send POST /api/v1/transfers/ {"senderWalletId":
"Invalid", - 404 Not Found - Enter non-existent or invalid
"receiverWalletId": "WALLET002",

> receiverWalletId "amount": 500.00, - Other fields remain valid
> "currency": "GBP"}

\- Sender has balance greater than (£5,000) - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 202 Accepted - Set
amount \> 5000 "receiverWalletId": "WALLET002",

> "amount": 5500.00, "currency": "GBP"}

\- Sender has an active wallet - Send POST /api/v1/transfers/ - 400 Bad
Request - Set amount \> 10000 {"senderWalletId": "WALLET001",

> "receiverWalletId": "WALLET002", "amount": 15000.00,
>
> "currency": "GBP"}

PRE-CONDITIONS TEST STEPS TEST DATA EXPECTED HTTP CODE

\- Sender has an active account - Send POST /api/v1/transfers/
{"senderWalletId": "WALLET001", - 400 Bad Request - Set amount to £0.00
"receiverWalletId": "WALLET002",

> "amount": 0.00, "currency": "GBP"}

\- Sender has balance greater than or equal - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 201 Created (£1) -
Set amount to 1.00 "receiverWalletId": "WALLET002",

> "amount": 1.00, "currency": "GBP"}

\- Sender has balance greater than or equal - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 201 Created
(£5,000) - Set amount to 4999.99 "receiverWalletId": "WALLET002",

> "amount": 4999.99, "currency": "GBP"}

\- Sender has balance greater than or equal - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 201 Created
(£5,000) - Set amount to 5000.00 "receiverWalletId": "WALLET002",

> "amount": 5000.00, "currency": "GBP"}

\- Sender has balance greater than (£5,000) - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 202 Accepted - Set
amount to 5000.01 "receiverWalletId": "WALLET002",

> "amount": 5000.01, "currency": "GBP"}

\- Sender has balance greater than or equal - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 202 Accepted
(£10,000) - Set amount to 9999.99 "receiverWalletId": "WALLET002",

> "amount": 9999.99, "currency": "GBP"}

\- Sender has balance greater than or equal - Send POST
/api/v1/transfers/ {"senderWalletId": "WALLET001", - 202 Accepted
(£10,000) - Set amount to 10000.00 "receiverWalletId": "WALLET002",

> "amount": 10000.00, "currency": "GBP"}

\- Sender has balance greater than (£10,000) - Send POST
/api/v1/transfers/ - 400 Bad Request - Set amount to 10000.01
{"senderWalletId": "WALLET001",

> "receiverWalletId": "WALLET002", "amount": 10000.01,
>
> "currency": "GBP"}

EXPECTED RESPONSE BODY

\- TransactionId is present - Status is "SUCCESS"

\- Error code: "UNSUPPORTED_CURRENCY"

\- Error message: "currency must be one of GBP, EUR, USD"

\- Error field: "currency"

\- Error code: "INSUFFICIENT_BALANCE" - Error message: "Insufficient
balance"

\- Error field: "amount"

\- Error code: "WALLET_NOT_FOUND"

\- Error message: "Receiver wallet does not exist"

\- Error field: "receiverWalletId"

\- Error code: "WALLET_NOT_FOUND"

\- Error message: "Sender wallet does not exist"

\- Error field: "senderWalletId"

\- TransactionId is present

\- Status is "PENDING APPROVAL"

\- Error code: "AMOUNT EXCEEDS MAXIMUM"

\- Error message: "Enter an amount less than 10000"

\- Error field: "amount"

EXPECTED RESPONSE BODY

\- Error code: "INVALID AMOUNT"

\- Error message: "Amount must be greater than 0"

\- Error field: "amount"

\- TransactionId is present - Status is "SUCCESS"

\- TransactionId is present - Status is "SUCCESS"

\- Auto-approved

\- TransactionId is present - Status is "SUCCESS"

\- Auto-approved

\- TransactionId is present

\- Status is "PENDING APPROVAL"

\- TransactionId is present

\- Status is "PENDING APPROVAL"

\- TransactionId is present

\- Status is "PENDING APPROVAL"

\- Error code: "AMOUNT EXCEEDS MAXIMUM"

\- Error message: "Enter an amount less than 10000"

\- Error field: "amount"
