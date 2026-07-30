Here is the formatted version converted into structured list sections (instead of Markdown tables) so it displays cleanly in GitHub without relying on table rendering!

📋 TEST DESIGN SPECIFICATION
⚙️ 1. FUNCTIONAL SCENARIOS
TC_FUNC_001: Successful transfer
Scenario Type: Positive

Pre-conditions: Sender has sufficient wallet balance (>£500)

Test Steps:

- Send POST /api/v1/transfers/

- Enter valid walletId, amount, and accepted currency (GBP, EUR, USD)

- Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 500.00,
  "currency": "GBP"
}
Expected HTTP Code: 201 Created

Expected Response Body:

transactionId is present

Status is "SUCCESS"

TC_FUNC_002: Unsupported currency
Scenario Type: Negative

Pre-conditions: System is active

Test Steps:

Send POST /api/v1/transfers/

Enter unaccepted currency

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 500.00,
  "currency": "JPY"
}
Expected HTTP Code: 400 Bad Request

Expected Response Body:

Error code: "UNSUPPORTED_CURRENCY"

Error message: "currency must be one of GBP, EUR, USD"

Error field: "currency"

TC_FUNC_003: Insufficient balance
Scenario Type: Negative

Pre-conditions: Sender has an insufficient wallet balance (<£1)

Test Steps:

Send POST /api/v1/transfers/

Enter amount greater than wallet balance

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 500.00,
  "currency": "GBP"
}
Expected HTTP Code: 400 Bad Request

Expected Response Body:

Error code: "INSUFFICIENT_BALANCE"

Error message: "Insufficient balance"

Error field: "amount"

TC_FUNC_004: Invalid receiver Wallet ID
Scenario Type: Negative

Pre-conditions: System is active

Test Steps:

Send POST /api/v1/transfers/

Enter non-existent or invalid receiverWalletId

Other fields remain valid

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "Invalid",
  "amount": 500.00,
  "currency": "GBP"
}
Expected HTTP Code: 404 Not Found

Expected Response Body:

Error code: "WALLET_NOT_FOUND"

Error message: "Receiver wallet does not exist"

Error field: "receiverWalletId"

TC_FUNC_005: Invalid sender Wallet ID
Scenario Type: Negative

Pre-conditions: System is active

Test Steps:

Send POST /api/v1/transfers/

Enter non-existent or invalid senderWalletId

Other fields remain valid

Test Data:

JSON
{
  "senderWalletId": "Invalid",
  "receiverWalletId": "WALLET002",
  "amount": 500.00,
  "currency": "GBP"
}
Expected HTTP Code: 404 Not Found

Expected Response Body:

Error code: "WALLET_NOT_FOUND"

Error message: "Sender wallet does not exist"

Error field: "senderWalletId"

TC_FUNC_006: Transfer required amount
Scenario Type: Positive / Review

Pre-conditions: Sender has balance greater than (£5,000)

Test Steps:

Send POST /api/v1/transfers/

Set amount > 5000

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 5500.00,
  "currency": "GBP"
}
Expected HTTP Code: 202 Accepted

Expected Response Body:

transactionId is present

Status is "PENDING APPROVAL"

TC_FUNC_007: Transfer above maximum limit
Scenario Type: Negative

Pre-conditions: Sender has an active wallet

Test Steps:

Send POST /api/v1/transfers/

Set amount > 10000

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 15000.00,
  "currency": "GBP"
}
Expected HTTP Code: 400 Bad Request

Expected Response Body:

Error code: "AMOUNT EXCEEDS MAXIMUM"

Error message: "Enter an amount less than 10000"

Error field: "amount"

📏 2. BOUNDARY VALUE ANALYSIS (BVA)
TC_BVA_001: £0.00
Boundary Value Category: Below minimum limit

Pre-conditions: Sender has an active account

Test Steps:

Send POST /api/v1/transfers/

Set amount to £0.00

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 0.00,
  "currency": "GBP"
}
Expected HTTP Code: 400 Bad Request

Expected Response Body:

Error code: "INVALID AMOUNT"

Error message: "Amount must be greater than 0"

Error field: "amount"

TC_BVA_002: £1.00
Boundary Value Category: Minimum limit allowed

Pre-conditions: Sender has balance greater than or equal (£1)

Test Steps:

Send POST /api/v1/transfers/

Set amount to 1.00

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 1.00,
  "currency": "GBP"
}
Expected HTTP Code: 201 Created

Expected Response Body:

transactionId is present

Status is "SUCCESS"

TC_BVA_003: £4,999.99
Boundary Value Category: Just below approval limit

Pre-conditions: Sender has balance greater than or equal (£5,000)

Test Steps:

Send POST /api/v1/transfers/

Set amount to 4999.99

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 4999.99,
  "currency": "GBP"
}
Expected HTTP Code: 201 Created

Expected Response Body:

transactionId is present

Status is "SUCCESS"

Auto-approved

TC_BVA_004: £5,000.00
Boundary Value Category: At limit

Pre-conditions: Sender has balance greater than or equal (£5,000)

Test Steps:

Send POST /api/v1/transfers/

Set amount to 5000.00

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 5000.00,
  "currency": "GBP"
}
Expected HTTP Code: 201 Created

Expected Response Body:

transactionId is present

Status is "SUCCESS"

Auto-approved

TC_BVA_005: £5,000.01
Boundary Value Category: Just above approval limit

Pre-conditions: Sender has balance greater than (£5,000)

Test Steps:

Send POST /api/v1/transfers/

Set amount to 5000.01

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 5000.01,
  "currency": "GBP"
}
Expected HTTP Code: 202 Accepted

Expected Response Body:

transactionId is present

Status is "PENDING APPROVAL"

TC_BVA_006: £9,999.99
Boundary Value Category: Below maximum limit

Pre-conditions: Sender has balance greater than or equal (£10,000)

Test Steps:

Send POST /api/v1/transfers/

Set amount to 9999.99

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 9999.99,
  "currency": "GBP"
}
Expected HTTP Code: 202 Accepted

Expected Response Body:

transactionId is present

Status is "PENDING APPROVAL"

TC_BVA_007: £10,000.00
Boundary Value Category: At maximum limit

Pre-conditions: Sender has balance greater than or equal (£10,000)

Test Steps:

Send POST /api/v1/transfers/

Set amount to 10000.00

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 10000.00,
  "currency": "GBP"
}
Expected HTTP Code: 202 Accepted

Expected Response Body:

transactionId is present

Status is "PENDING APPROVAL"

TC_BVA_008: £10,000.01
Boundary Value Category: Just above maximum limit

Pre-conditions: Sender has balance greater than (£10,000)

Test Steps:

Send POST /api/v1/transfers/

Set amount to 10000.01

Test Data:

JSON
{
  "senderWalletId": "WALLET001",
  "receiverWalletId": "WALLET002",
  "amount": 10000.01,
  "currency": "GBP"
}
Expected HTTP Code: 400 Bad Request

Expected Response Body:

Error code: "AMOUNT EXCEEDS MAXIMUM"

Error message: "Enter an amount less than 10000"

Error field: "amount"
