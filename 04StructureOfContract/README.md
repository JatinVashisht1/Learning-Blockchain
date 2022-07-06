# 04 Structure of a Contract
<p>Exploring different components of a contract</p>

## Before we proceed
- This blog is inspired from official solidity docs.
- You can go-to the docs by clicking [here](https://docs.soliditylang.org/en/v0.8.15/structure-of-a-contract.html).
- This blog is also posted on my [github](https://github.com/JatinVashisht1/Learning-Blockchain/tree/master/04StructureOfContract)

## Overview
- Contracts in solidity are similar to the concept of classes in Object Oriented Languages, they can be inherited and can inherited other contracts as well.
- We can have declarations of state variables, functions, events, errors, etc.
- There are some special types of contracts also such as libraries and interfaces, which we will talk about later.
- Contracts on its own is a large topic and we will dive deep into it in upcoming parts.
- In this we are keeping it easy and are just exploring different components of a contract.

## State Variables
- State variables are variables whose value is permanently stored in the storage.
- Below is an example of a state variable
```
uint storedData;
```
- State variables can be set to `public`, `internal`, `private`.
- When a state variable is declared `public`, compiler will automatically generate a `getter` function for that variable.
- If we try to access a public variable from inside the same contract using `this` keyword then it will invoke the getter function. If we try to access the state variable without this keyword then we get value directly from the storage.
- Declaring a variable `internal` works similar to `protected` access modifier in c/cpp. External contracts won't be able to access this variable but the derived contracts can access them.
- If we mark a variable `private` then external contracts and derived contracts won't be able to access it.
#### Note*
<p>
Making something private or internal only prevents other contracts from reading or modifying the information, but it will still be visible to the whole world outside of the blockchain.
</p>

## Functions 
- They are resuabel and executable units of code that can be called again and again.
- Functions can be defined both inside and outside of a contract.
- Below is an example of function
```
function helper(uint x) pure returns (uint)
```
- Let us understand what above code snippet means
- word `function` is a keyword which is used to declare a function.
- `helper` is the name of the function, by which it will be invoked later at some point.
- inside brackets, `uint x` means that it takes a parameter of type `uint`.
- word `pure` is also a keyword, which means that this function is not modifying or read variable of any state variable.
- `returns (uint) means that this function is returning a value of type uint.
- Function calls can happen internally or externally depending upon the visibility of the function.

## Function Modifiers
- Modifiers can be used to change the behaviour of a function in a declarative way
- For example you can use a modifier to automatically check a condition prior to executing of a function.
- Modfiers are derivable properties of a contract only if they are marked `virtual`.
- Modifiers are overridable but NOT overloadable.
- Below is an example of a function modifier
```
contract Mutex {
    bool locked;
    modifier noReentrancy() {
        require(
            !locked,
            "Reentrant call."
        );
        locked = true;
        _;
        locked = false;
    }

    /// This function is protected by a mutex, which means that
    /// reentrant calls from within `msg.sender.call` cannot call `f` again.
    /// The `return 7` statement assigns 7 to the return value but still
    /// executes the statement `locked = false` in the modifier.
    function f() public noReentrancy returns (uint) {
        (bool success,) = msg.sender.call("");
        require(success);
        return 7;
    }
}
```
- Well well well... Let's understand what's going on here
- It is a regular contract named Mutex, which contains one boolean variable, one modifier and one function.
- Let us understand modifier `noReetrancy`.
- This modifier is taking care that no one re-enters and it is applied on function `f()`.
- The _ means that the function body will be executed after this and there can be multiple _ in a function, each of them will be replaced with the function body.
- After the function is returned the value will be returned but the controll flow will continue, i.e, locked will be marked false afterwards.
- Thus when the function will return 7 then the value locked will be re-initialized to false.
- Multiple modifiers can be applied to a function and they will be evaluated in the order presented.

## Events
- Events gives abstractions on the top of EVM's logging functionallity.
- Application (usually frontend apps) can subscribe to different events and can listen and act accordingly when an event is fired.
- Let's see an example of event
```
event HighestBidIncreased(address bidder, uint amount); // Event

    function bid() public payable {
        // ...
        emit HighestBidIncreased(msg.sender, msg.value); // Triggering event
    }
```
## Errors
- Error helps you to pre-define name of error and its data that can occur later in the code.
- They are used with revert statements.
- In comparison to `string descriptions` errors are much cheaper and allow us to encode additional data.
- Now it is time to see an example of error
```
error NotEnoughFunds(uint requested, uint available);

if (balance < amount)
            revert NotEnoughFunds(amount, balance);
```
- If we take a closer look at the example, we can see that we have defined an `error` which will be used when the requested amount is greater than the amount available.
- We can see that when we check the condition of unsuccifient balance we revert instantly with NotEnoughFunds error with current amount and balance as data of error.

## Struct Types
- If you are familiar with c/cpp you might be already familiar with the concept of structs.
- Struct are custom/user-defined data types, that can group different variables.
- Example of `struct` type is given belo
```
struct Voter { // Struct
        uint weight;
        bool voted;
        address delegate;
        uint vote;
    }
```

## Enum Types
- Enums are used to define custom data types but with finite set of possible values.
- Its representation is very similar to that in c programming language.
- Let us understand this with an example
```
enum State { Created, Locked, Inactive }
```
- Enums can be used for state management also.
- Like in the above example we can see that `State` enum can have only three values/states `Created`, `Locked` and `Inactive`.
- We can react accordingly depending upon the `State`.
- You may be already familiar with the approach of state management with enum classes if you are familiar with `Kotlin` programming language.
