🚀 meCash API Automated Test Suite

Automated QA test suite and CI pipeline for the meCash Money Transfer API (POST /api/v1/transfers/).

1. Technology stack chosen
   
   - Postman: Test collection creation & mock server setup.
     
   - Newman: CLI runner to execute Postman tests locally and in CI.
     
   - htmlextra: Generates visual HTML test reports.
     
   - Node.js & npm: Dependency management and test script execution.
     
   - GitHub Actions: CI pipeline that runs tests automatically on every push.
     
3. Framework structure
   
 meCash-Assessment/
 
├── .github/.                        # GitHub Actions CI workflow
     └── workflows/
     └── main.yml 

├── automation/                      # Postman collection & environment

├── reports/                         # Destination folder for HTML test reports

├── test-data/                        # Sample payloads and environment data

├── README.md                         # Main repository documentation & setup guide

├── package-lock.json                 # Locked dependency version tree

├── package.json                      # Dependencies and test runner script

├── security-compliance.md            # Security & OWASP testing specs

├── test-design.md                    # Detailed test cases & BVA scenarios

└── test-strategy.md                  # Scope, test approach, and risk assessment

4.  Setup instructions
git clone https://github.com/HephzibahA/meCash-Assessment.git

cd meCash-Assessment

npm install

6. Execution instructionsLocal Execution

   To run tests locally and generate an HTML report: npm test
   
CI Execution (GitHub Actions):

- Every push to main automatically runs the test suite.
  
- View results and download the HTML report under GitHub Actions- Workflow Run - Artifacts.
  
6. Assumptions made

- Mock Server: Postman mock server handles test requests.
  
- Currencies: Limited strictly to GBP, EUR, and USD.
  
- Idempotency: Unique X-Idempotency-Key header prevents duplicate charges.
  
7. Design decisions
   
- Markdown Docs: Standardized all test cases and strategies as .md files for clean reading directly on GitHub.
  
- Clean Repo: HTML test reports are saved as downloadable CI artifacts instead of clogging up git history.
  
- Focused Boundary Testing: Prioritized tests around the £5,000 approval trigger and £10,000 maximum transfer cap.
