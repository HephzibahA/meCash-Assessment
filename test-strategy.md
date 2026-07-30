🎯 TEST STRATEGY
🔍 1. SCOPE
✅ In Scope (What Will Be Tested)
API Endpoints:

Verification of the transfer processing via /api/v1/transfers

Request Validation:
Verifying request body input fields (senderWalletId, receiverWalletId, amount, currency).

Response Validation:
Verifying response returns required parameters (transactionId, status, amount, currency).

Header Validation:
Checking for valid JSON Content-Type headers.

Business Rules:
Maximum Transfer Limit: Verifying transfer up to maximum limit of £10,000.
High-Value Approval: Verifying high value transfer above £5,000 triggers additional approval.
Supported Currencies: Validating supported currencies (GBP, EUR, USD) and rejection of unsupported ones.
Audit Logging: Verifying successful transfers generate an audit record.
Idempotency: Validating prevention of duplicate transactions.

Boundary Condition:
Testing transfer amount within certain threshold:
At approved or below threshold: 4999.99, 5000.00
Above approved threshold: 5000.01, 10000.00, 10000.01
Zero and negative: 0, -1
Negative Scenarios:
Testing missing or null required fields (e.g., senderWalletId, currency).
Testing unsupported currencies.

❌ Out of Scope (What Will Not Be Tested)
Frontend UI Performance: Testing of web or mobile user interface.
Live Payment Integration: Postman mock server is used in place of real money movements.
Load Testing: Measuring server through high traffic loads.

🛠️ 2. TEST APPROACH
⚙️ Functional Testing: Validating core payment transfer aligns with business rules, such as verifying transfers above £5000 generate additional approval, maximum transfer do not exceed £10,000.

🚀 API Testing: Using Postman to execute request against the endpoint (/api/v1/transfers), verifying JSON response, mandatory fields and content-type headers.

🧪 Negative Testing: Intentionally submitting invalid requests; missing required fields, unsupported currencies, negative amounts and mismatched data types to verify the API returns error responses.

🔒 Security Testing: Checking the API to ensure a valid token or key is passed in the Authorization Header before allowing transfers and making sure checks are in place to prevent bad data from being processed.

🔄 Regression Testing: Running automated Postman test suites across the endpoint to be sure code changes do not break existing functionality.

⚠️ 3. RISK ASSESSMENT
🚨 High-Risk Areas
Enforcing Threshold: Transfers exceeding the £10,000 limit or transfers above £5000 skipping the required approval checks.
Invalid Data Types and Unsupported Currencies: The API accepting mismatched data types, bad data types and unsupported currencies.

💥 Potential Production Failures
Server Crashes: Broken format or unexpected input causing the server to crash instead of clear appropriate error messages.
No Authorization: API allowing transfer requests go through without valid authorization headers.

🕵️‍♂️ Fraud Related Risks
Bypassing Limit: Allowing large transfers go through without triggering approval checks.
Unauthorized Request: User making transfers with an account they do not own.
Failed Verification: Allowing user complete transfer with invalid account verification details (invalid Id).

⚖️ Data Integrity Concerns
Lack of Idempotency: Double charging a wallet if a network delay or user sends a duplicate request.
Missing Audit Logs: A successful transaction failing to generate an audit log.
