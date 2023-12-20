
## Points to keep in mind:
   - Convince the protocol this is an issue
   - how bad the issue is
   - how to fix the issue
     
## Layout for the report:
    [S-#] TITLE (Root Cause + Impact)
    Description:
    
    Impact:
    
    Proof of Concept:
    
    Recommended Mitigation:

## writing the Title: 
   - the title should include of the root cause and impact it will have in the code
   - Example:
      1) TITLE1: [S-#] Root(Varaibles stored in storage on-chain are visible to anyone, no matter the solidity visibility keyword) impact(meaning the password is not actually a private password)
      2) TITLE2: Storing the password on-chain makes it visible to anyone, and no longer private.

## writing the Description:
  - here we need to teach the protocol why this is an issue
  - for variables write them with their contract name.
  - Example:
      " All data stored on-chain is visible to anyone, and can be read directly from the blockchain. The `PasswordStore::s_password` variable is intended to be a private variable and only accessible through the `PasswordStore:get_password` function, which is intended to be only called by the owner of the contract.
We show such method of reading any data off-chain below. "

## writing the Impact
  - Just include the impact
  - Example:
      " Anyone can read the private password, severely breaking the functionality of the protocol. "

## writing the Proof of Concept(Proof of Code):
  - It is very important to make the protocol understand or give the proof of the issue that it is a very severe issue
  -  



