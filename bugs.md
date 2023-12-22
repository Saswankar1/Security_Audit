### Denial of Service: 
  It check for duplicates value but as the main array is long it will cost more gas.
  So assume in a lottery, the entrant fee depends on the number of people in the lottery.
  Now a hacker can call the function a million time using different accounts so no other player can join the lottery. 
  
```solidity
      // SPDX-License-Identifier: MIT
      pragma solidity 0.8.20;
      
      contract DoS {
          address[] entrants;
      
          function enter() public {
              // Check for duplicate entrants
              for (uint256 i; i < entrants.length; i++) {
                  if (entrants[i] == msg.sender) {
                      revert("You've already entered!");
                  }
              }
              entrants.push(msg.sender);
          }
      }
```
