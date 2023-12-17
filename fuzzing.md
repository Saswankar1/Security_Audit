Certainly! Here's the content formatted for a README file:


# Fuzz Testing and Invariants

## Fuzz Testing
**Fuzz Testing:** Supplying random data to your system in an attempt to break it.

**Invariant:** A property of code that is always the same or doesn't change. Example: `alwaysZero`

## Examples
```solidity
contract MyContract {
    uint public alwaysZero = 0;
    uint private hiddenValue = 0;

    function doStuff() {
        if(data == 2){
            alwaysZero = 1;
        }
        if(hiddenValue == 7){
            alwaysZero = 1;
        }
        hiddenValue = data;
        return alwaysZero;
    }
}
```

## Test Function
```solidity
// Test function
function testIsAlwaysZeroFuzz(uint data) {
    assert(exampleContract.doStuff() == 0);
}
```

## Run Test
```bash
# Run test using Forge
# forge test -m testIsAlwaysZeroFuzz
# It will give an error at data = 2
```

## Configuring Fuzz Runs
We can change the number of runs a fuzz function does by adding the following code in `foundry.toml`:
```toml
[fuzz]
runs = 1000
```

Above test is a stateless fuzz run, i.e., where the state of the previous test is discarded for every run.

## Stateful Fuzzing
Fuzzing where the final state of your previous run is the starting state of your next run.

## Example Stateful Fuzzing Contract
```solidity
import { Stdinvariant } from "forge-std/Stdinvariant.sol";

contract Test is Stdinvariant {
    MyContract exampleContract = new MyContract();
    targetContract(address(exampleContract));

    function invariantTestAlwaysReturnsZero() public {
        assert(exampleContract.doStuff() == 0);
    }
}
```

## In Foundry
- Fuzz Test: Random data to one function (stateless).
- Invariant Test: Random data and random function calls to many functions (stateful).

## Real Examples
- New token minted should be less than the inflation rate.
- Only possible to have one winner.
- Only withdraw what they deposit.
