
# Before we start
- this blog is inspired from the `introduction to smart contracts` page of official documentation.
- Click [here](https://docs.soliditylang.org/en/v0.8.15/introduction-to-smart-contracts.html) goto that page.
- I am assuming that `solidity` is **NOT** your first programming language as basic concepts would not be explained in this post.

# Smart Contracts
- According to definition on official documentation of [solidity](https://docs.soliditylang.org/en/v0.8.15/introduction-to-smart-contracts.html), a contract is defined as, "A contract in the sense of Solidity is a collection of code (its functions) and data (its state) that resides at a specific address on the Ethereum blockchain."
- In simple words smart contract is a piece of code, it can be a function a state or both that can be identified by an address on Ethereum blockchain.
- In solidity, any data is refered to as `state`, whether it is a primitive variable or an array.
## Simple example (Storage)
```
// SPDX-License-Identifier: GPL-3.0 // this gives detail about license
pragma solidity >=0.4.16 <0.9.0; 
// pragma just like c/cpp is used as preprocessor, 0.4.16 tells us the solidity version it is written for and 
// <0.9.0 tells us that this this code is valid for solidity versions below 0.9.0

// contract keyword used to define a contract
contract SimpleStorage {
    uint storedData;

    // public keyword to set function public
    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

### Understanding the code example
- In the above example we we have defiined made a `contract` named `SimpleStorage` refering to the objective for which it is made.
- Next line `uint storedData` state variable is declared with data-type `uint`.
    - uint is a data-type refering to `unsigned integer` of 256 bits.
- After that we have defined two functions `set` and `get`, as their name suggests, they will act as `setter` and `getter` for the storedData.
- This contract currently don't do anything apart from setting a single number and allowing anyone to do it.
- Although anyone can onverwrite the value of state storedData but the previous values would be stored in the history of blockchain.

## Subcurrency (Example)
```
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

contract Coin {
    // The keyword "public" makes variables
    // accessible from other contracts
    address public minter;
    mapping (address => uint) public balances;

    // Events allow clients to react to specific
    // contract changes you declare
    event Sent(address from, address to, uint amount);

    // Constructor code is only run when the contract
    // is created
    constructor() {
        minter = msg.sender;
    }

    // Sends an amount of newly created coins to an address
    // Can only be called by the contract creator
    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);

    // Sends an amount of existing coins
    // from any caller to an address
    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```
### Understanding the above Subcurrency Example
- Well well well... don't get overwhelmed with the length of the code above, we will understand the code part by part.
- `address public minter` declares a public variable of data type address. Address is a data type of 160 bits.
    - the keyword public automatically generates a getter for the state variable.
- `mapping (address=>uint) public balances` is a map of address data type to uint data type.
    - One thing to remember about `mapping` is that it is not possible to extract all keys or all values, so you should store keys and values if you want to access them later.
- `event Sent(address from, address to, uint amount)` this syntax declares an event named Sent which can be used later.
    - client side applications can listen to the event emitted and can act accordingly

- In the `constructor` of this contract you will find an expression `minter = msg.sender`.
    - here we are initializing minter variable with the address of sender.
    - because this is constructor it will always permanently store the address of person creating the contract.
    - `msg` is specific global variable and it is alwas associated with the contract, `msg.sender` will give us the address from where the current (external) function call came from.

- Next we have created a function with signature `function mint(address receiver, uint amount) public{};`
    - In this function you can notice a `require` statement.
    - This statement checks for a given condition and if the condition is not met this will revert the whole process and will undo any state changes done in the time frame.
    - In the context of this function it will only allow to mint coin to the creator of the contract.

- We can also define our own `error` and can throw at appropiate situation.
    - For example in our example above we have defined an Error named InsufficientBalance with syntax, `error InsufficientBalance(uint, requested, uint available);`

- Lastly we have defined a public function `send` which takes two parameters, `receiver` and `amount`. Receiver is the address where coins will be sent and amount is the number of coins we want to send.
    - Here we have firstly checked condition if amount we want to send is not greater than the balance the sender have, if it is the case then we can use `revert` statement and can throw our the error we declared above named `InsufficientBalance`.
    - Revert statement unlike require allows us to send additional information about the error or the problem before reverting changes.
    - After we are done with the arithmetic of sending coins we are sending the event we declared above to the client side application.

---
### Note* 
- If you use this contract to send coins to an address, you will not see anything when you look at that address on a blockchain explorer, because the record that you sent coins and the changed balances are only stored in the data storage of this particular coin contract. By using events, you can create a “blockchain explorer” that tracks transactions and balances of your new coin, but you have to inspect the coin contract address and not the addresses of the coin owners.
--- 
