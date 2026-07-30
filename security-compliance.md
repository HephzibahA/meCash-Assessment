SECURITY & COMPLIANCE TESTING STRATEGY

**Overview:** This strategy outlines the QA approach and methodology for validating the security posture and regulatory compliance of the core money transfer API POST ( /api/v1/transfers/)

- **SECURITY TESTING METHODOLOGY**

1. **Authentication**

 Objective:
- Ensure only verified, active users with valid login credentials can access the transfer API and make transfers.
Testing Approach:
- Missing Tokens: Send POST /api/v1/transfers/ request without an Authentication header and verify 401 unauthorized error.
- Invalid or Expired Tokens: Send request with an expired or invalid or fake token and verify 401 unauthorized error.
- Inactive/Unverified/Suspended Account: Send request using a valid token from an account that is (i) unverified (ii) inactive (iii) suspended and verify the API blocks the requests and sends 403 forbidden errors.

2. **Authorization**

Objective:
- Ensure logged-in users can only send money from their account and cannot access someone else's wallet.
Testing Approach:
- Cross-user Wallet Access(BOLA/IDOR): Log in as User A but put User B's walletId in the senderWalletId field. Verify the API blocks the request and returns a 403 forbidden error.
- Sending Money to Restricted Account: Attempt sending money from a standard account to a restricted account. Verify the request is rejected and a 403 error is returned
- Unauthorized Privilege Transfer: Attempt to make a transfer that bypass business rule (>£5000, > £10,000) by adding admin headers. Verify the API ignores the header and enforces business rules and sends appropriate responses (202 accepted with status "PENDING APPROVAL" , 202 accepted status code for amount >£5000) and ( 400 bad request with error "EXCEED MAXIMUM LIMIT") for amount > than £10,000).

3. **Sensitive Data Exposure**

Objective:

Ensure private user information is protected and not exposed in web traffic, API responses or system errors.

Testing Approach:
- Unencrypted Connections: Send a request using an unencrypted connection (http:// in place of https://). Verify the system rejects the connection.
- Exposing Private Data in Responses: Check successful and failed transfer responses. Verify sensitive fields are not exposed in the response body and are appropriately masked.

4. **Replay Attacks**

Objective:

Ensure that a valid, stopped transfer request cannot be re-sent by an attacker or re-submitted accidentally by a user to process duplicate payments.

Testing Approach:
- Duplicate Request Submission (Replay Check): Stop a valid transfer request and send the exact same request again immediately. Verify recognises this as a duplicate request and does not process.
- Idempotency Key Validation: Send two requests using the exact same unique _Idempotency-Key header_. Verify that the second request returns the existing transaction result and does not create a new payment.

5. **Rate Limiting**

Objective:
- Ensure the API limits the number of requests a user can send within a specific time period to protect against spam and server overload.
Testing Approach:
- Exceeding Request Limit: Send various requests from the same user within a short period. Verify the API blocks subsequent requests and sends an error.
- Cool Down Period: Wait for the duration specified to cool down and retry. Verify the limit has been reset and the request is successfully processed.

6. **Input Validating**

Objective:
- Ensure all fields in the request body are properly validated and cleaned so bad data or malicious code cannot break the system or tamper with the database.
Testing Approach:
- Code Injection Checks: Try injecting SQL or script code into text inputs. Verify the API rejects and returns a 400 Bad Request error.
- Data Type Checks: Attempt submitting with negative values, excess decimals or mismatched data types. Verify the API rejects the request with a 400 Bad request error.
- Missing Fields Check: Send request body with missing required field (currency, walledId, amount) and verify the API returns 400 bad requests, Missing fields required.

7. **OWASP Top 10 Considerations**

Objectives:
Ensure the transfer API is protected against the most critical industry-standard API security vulnerabilities as defined by the global OWASP API Security framework.

Testing Approach
- Resource Overuse & DoS: Send real large batch arrays and JSON payload in a single request. Verify the API enforces payload limits to prevent memory.
- Add administrative field to valid transfer payload. Verify the API ignores and sends bad request errors.
- Attempt to call administrative endpoint. Verify the API blocks access.
  
- **COMPLIANCE VALIDATION**
- PCI DSS: Ensured TLS 1.3 encryption for data in transit and masked sensitive payment payload data (no plain-text cardholder/PAN data stored or logged).
- GDPR: Validated PII data minimization in request payloads and verified users can exercise right-to-erasure/data access.
- Audit Logging: Verified all critical actions (auth, transfers, updates) emit structured logs capturing timestamp, user_id, action, and IP.
- Transaction Traceability: Enforced unique correlation_id / transaction_id tracking across all API headers for end-to-end request tracing.
- Data Retention & Privacy: Validated data retention policies, ensuring sensitive logs auto-expire and payload storage aligns with privacy requirements.
