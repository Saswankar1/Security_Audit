# Smart Contract Audit Process

## 1. The Audit Process
   a. **Scoping:** Determine the duration and pricing for the audit.
   b. **Reconnaissance:** Conduct an initial code walkthrough and use relevant tools.
   c. **Vulnerability Identification:** Identify and document vulnerabilities in the code.
   d. **Reporting:** Create a comprehensive report outlining ways to enhance protocol security.

## 2. Protocol Fixes
   a. **Fix Issues:** Implement solutions to address identified vulnerabilities.
   b. **Retest and Add Tests:** Ensure that fixes are effective and comprehensive by retesting and adding new tests.

## 3. Mitigation Review
   - Recheck the code to identify and address any new potential security risks.

## Two Tests Before Starting a Smart Contract Audit

### 1. The Rekt Test Questions
- Ensure thorough documentation of actors, roles, and privileges.
- Document all external services, contracts, and oracles relied upon.
- Establish and test an incident response plan.
- Document potential attack vectors on the system.
- Perform identity verification and background checks on all employees.
- Assign a team member with a dedicated security role.
- Enforce the use of hardware security keys for production systems.
- Implement a key management system with multiple human and physical steps.
- Define key invariants for the system and test them with every commit.
- Utilize the best automated tools to identify security issues in the code.
- Undergo external audits and maintain a vulnerability disclosure or bug bounty program.
- Consider and mitigate potential avenues for abusing users of the system.

### 2. Simple Security Toolkit
[Simple Security Toolkit](https://github.com/nascentxyz/simple-security-toolkit)

