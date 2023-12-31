# Smart Contract Security Tools

## 1. Static Analysis

Static analysis is a process of analyzing or scanning smart contract code to detect security vulnerabilities and ensure correctness without actually running the code.

### a. **Slither**

Slither is a static analysis tool for smart contracts written in Python. It is known for its ability to find a wide range of vulnerabilities.

### b. **Consensys MythX**

Consensys MythX is a cloud-based static analysis service for smart contracts. It can automatically scan for security vulnerabilities in Ethereum and other EVM-based blockchain smart contracts.

### c. **Rattle**

Rattle is a static binary EVM analysis framework that produces a control flow graph for better contract understanding.

### d. **Mythril**

Mythril is both a static and dynamic analysis tool for Solidity smart contracts. It is designed to detect potential vulnerabilities and security weaknesses in Solidity code.

## 2. Mutation Testing

Mutation testing evaluates and improves test suites by introducing faults (mutants) into the program being tested. It helps in understanding how well the test suite detects and eliminates these mutants.

### a. **Vertigo-rs**

Vertigo-rs is a mutation testing tool actively maintained, with added support for Foundry.

### b. **Certora Gambit**

Certora Gambit is a sophisticated Solidity mutation system. It utilizes mutation operators to modify the source code of a Solidity program.

### c. **SuMo**

SuMo is a mutation testing tool for Solidity Smart Contracts, designed to run mostly in the NodeJS environment.

## 3. Fuzzing and Invariant Testing

Fuzzing and invariant testing are dynamic analysis techniques used for finding vulnerabilities in smart contracts.

### a. **Consensys Diligence Fuzzing**

Consensys Diligence Fuzzing is a tool for identifying flaws in smart contracts. It provides both CLI and cloud-based dashboard support.

### b. **Foundry Invariant Testing**

Foundry is a fast smart contract development tool built with Rust, supporting invariant testing.

### c. **Echidna**

Echidna is a fuzzing and property-based security testing tool for Ethereum smart contracts written in Haskell.

## 4. Formal Verification and Symbolic Execution

Formal verification is the process of proving and disproving the correctness of a program (smart contracts) using mathematical proofs.

### a. **Certora Prover**

Certora Prover analyzes smart contracts against specifications written in Certora Verification Language (CVL).

### b. **Solidity SMTChecker**

Solidity SMTChecker is a built-in formal verification module in the Solidity compiler. It uses SMT (Satisfiability Modulo Theories) and Horn solving.

### c. **Halmos**

Halmos is an open-source symbolic execution tool for Ethereum smart contracts developed by the a16z team.

### d. **HEVM**

HEVM is an implementation of the Ethereum Virtual Machine in Haskell, used for finding bugs and verifying correctness.

### e. **Manticore (Symbolic Execution)**

Manticore is a symbolic execution tool for analyzing smart contracts and binaries, supporting Ethereum smart contracts, Linux ELF binaries, and WASM modules.

## 4. Visualization Tools

Visualization tools help auditors and developers understand the structure and control flow of smart contracts.

### a. **Surya by ConsenSys**

Surya is a tool by the ConSenSys team for visualizing the structure of smart contracts. It provides features like flattening contracts, viewing inheritance trees, and generating graphs.

### b. **Solidity Visual Auditor**

Solidity Visual Auditor is a VSCode extension by tintinweb that supports syntax highlighting for Solidity code and detects security issues.
