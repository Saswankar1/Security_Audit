Fuzz testing: Supplying random data to your system in an attempt to break it

Invariant: property of a code that is always same or doesnt change. Ex: alwaysZero

Examples:
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

    ```solidity
    // test function
        function testIsAlwaysZerofuzz(uint data) {
            assert(exampleContract.doStuff() == 0);
        }

    ```solidity
    // run test: forge test -m testIsAlwaysZerofuzz
    // it will gave error at data = 2

We can change the number of run a fuzz function do by adding the below code in foundry.toml
    // [fuzz]
       runs = 1000

Above test in stateless fuzz run i.e. where the state of the previous test is discarded for every run


Stateful Fuzzing: Fuzzing where the final state of our previous run is the final state of your previous run is the starting state of your next run

 Ex:
    import {Stdinvariant} from "forge-std/Stdinvariant.sol"
    contract test is Stdinvariant, Test{
        exampleContract = new MyContract();
        targetContract(address(exampleContract));

        function invariant_tesAlwaysReturnsZero() public {
            assert(exampleContract.doStuff() == 0);
        }
    }

In foundry:
    fuzz test: random data to one function(stateless)
    invariant test = random data and random function calls to   
                    many functions(stateful)

Real Examples: 
    - new token minted < inflation rate
    - only possible to have 1 winner
    - only withdraw what they deposit
