# 1) Scoping

Before initiating a security review, it's essential to assess the readiness of the protocol and gather necessary information for scoping.

### a) Documentation
- Request comprehensive documentation containing all project information.
  - Business implementations may impact security, and documentation helps identify potential bugs.

### b) Find the Stats
- Evaluate project statistics:
  - Line of Code.(sudo npm install -g cloc / cloc ./src/)
  - Project complexity (complexity score).
  - Estimated time needed for the audit (important for private audits).

### c) How to Build the Project
- Understand the process of setting up the codebase and the test suite.
  - Example Commands:
    ```
    forge init
    forge install OpenZeppelin/openzeppelin-contracts --no-commit
    forge install vectorized/solady --no-commit
    forge build
    ```
- Learn how to run tests and check test coverage.
  - Example Commands:
    ```
    forge test
    ```

### d) Security Review Scope
- Define the scope of the security review.
  - Commit Hash: Unique identifier for a specific version of the code.
  - Repo URL: Repository URL where the code is hosted.
  - In Scope vs Out of Scope Contracts:
    - **In Scope:** Contracts or components actively considered for the security review.
    - **Out of Scope:** Contracts or components not included in the security review.

  #### Clarification: What Does "In Scope vs Out of Scope" Mean?
  - **In Scope:** Areas or components thoroughly examined for security vulnerabilities.
  - **Out of Scope:** Areas not the primary focus, either considered secure or covered in a different audit.

  #### Why Define Scope?
  - **Efficiency:** Allows auditors to focus on critical areas.
  - **Clarity:** Clearly communicates what is being assessed.
  - **Alignment:** Ensures alignment with the protocol's security goals.

  #### Example
  Consider a DeFi protocol:
  - **In Scope:** Smart contracts handling funds, user interactions, critical operations.
  - **Out of Scope:** Non-financial functionalities, admin contracts audited separately.

### e) Compatibilities
- Identify protocol compatibilities:
  - Solc Version.
  - Chains to deploy contracts.
  - Tokens (e.g., ERC20s, ERC721s).

### f) Roles
- Define different actors of the system, their powers, and responsibilities.
  - Example Actors:
    - Buyer: Project purchasing an audit.
    - Seller: Auditor willing to audit a project.
    - Arbiter: Impartial entity resolving disputes.

### g) Known Issues
- List any known issues that the protocol team is aware of and will not be acknowledging/fixing.

### h) Audit-Ready Protocol Example
- Refer to an example of an audit-ready protocol:
  [Example Protocol](https://github.com/Cyfrin/3-passwordstore-audit/tree/onboarded#test-coverage)

### i) Setting Up the Protocol in Local Dev Env
1. Set up the project using the quickstart section of the code readme.
   - Example:
     ```
     Quickstart
     git clone https://github.com/Cyfrin/3-passwordstore-audit
     cd 3-passwordstore-audit
     ```
2. Switch to the commit hash mentioned in the readme.
   - Commands:
     ```
     git checkout (commit hash)
     git switch -c (branch name)
     ```

This checklist ensures a comprehensive approach to preparing for a security audit, fostering a more secure protocol.
----------------------------------------------------------------------------------------------------------------------------------------
### Used by one of the leading dev just some basic tip:
  1) get all the code into the dev environtment
  2) use cloc to get all contract line then get it into spreadsheet start from the contract with shortest line of code
  3) USE notes to have notes or ideas
  4) while checking the code we can use comments to write that we visited the code
  5) We can also use soldity metrics extension(we can check the external and public funct as they are more likey used by hackers to attack)
