# Solidity Reference Guide


# Table of contents
- [Solidity Reference Guide](#solidity-reference-guide)
- [Table of contents](#table-of-contents)
    - [*Versions*](#versions)
    - [*Functions*](#functions)
    - [*Import*](#import)
    - [*Storage vs Memory*](#storage-vs-memory)
    - [*Function Visibility Specifiers*](#function-visibility-specifiers)
    - [*Function modifiers*](#function-modifiers)
    - [*Solidity Variables Types*](#solidity-variables-types)
    - [*If statements*](#if-statements)
    - [*SafeMath underflow/overflow*](#safemath-underflowoverflow)
    - [*Fixed-size Arrays*](#fixed-size-arrays)
    - [*Dynamically-sized Arrays*](#dynamically-sized-arrays)
    - [*BytesAndStrings*](#bytesandstrings)
    - [*Require*](#require)
    - [*Inheritance*](#inheritance)
    - [*Struct*](#struct)
    - [*Events*](#events)
    - [*Typecasting*](#typecasting)
    - [*Enums*](#enums)
    - [*Mappings*](#mappings)
    - [*Interfaces*](#interfaces)
    - [*Global Variables*](#global-variables)
    - [*Contract Address*](#contract-address)
    - [*Fallback Functions*](#fallback-functions)
    - [*Access Contract Balance*](#access-contract-balance)
- [Solidity Concepts](#solidity-concepts)
    - [*Immutability of Contracts*](#immutability-of-contracts)


### *Versions*

Add to top of file to clarify the version you want to use.

`pragma solidity ^0.8.0;`

nothing lower than 0.8.0 will be used in this example

Make sure the version is the same as the  `hardhat.config.js`

### *Functions*


- it is convention to start private function names with an underscore (_).
- In Solidity, the function declaration contains the type of the return value

```
function foo() public returns (string memory) {
  return foo;
}
```
### *Import*



### *Storage vs Memory*

variables can be stored in two places. storage and memory 

Storage refers to variables stored permanently on the blockchain
Memory variables are temporary, and are erased between external function calls to your contract.

The Gas consumption of Memory is not very significant as compared to the gas consumption of Storage

1.State variables and Local Variables of structs, array are always stored in storage by default.
2.Function arguments are in memory.
3.Whenever a new instance of an array is created using the keyword ‘memory’, a new copy of that variable is created. Changing the array value of the new instance does not affect the original array.
`
### *Function Visibility Specifiers*

*** Functions by default are set to public this can make you vulnerable to attacks  ***
*** only make public the functions you want to expose to the world. ***
- public: visible externally and internally (creates a getter function for storage/state variables)

- private: only visible in the current contract

- external: only visible externally (only for functions) - i.e. can only be message-called (via this.func) they can't be called by other functions inside that contract.

- internal: only visible internally

### *Function modifiers*

- **_ pure _** for functions: Disallows modification or access of state.
- **_ view _** for functions: Disallows modification of state.
- **_ payable_** for functions: Allows them to receive Ether together with a call.
- **_ constant_** for state variables: Disallows assignment (except initialisation), does not occupy storage slot.
- **_ immutable_** for state variables: Allows exactly one assignment at construction time and is constant afterwards. Is stored in code.
- **_ anonymous_** for events: Does not store event signature as topic.
- **_ indexed_** for event parameters: Stores the parameter as topic.
- **_ virtual_** for functions and modifiers: Allows the function’s or modifier’s behaviour to be changed in derived contracts.
- **_ override:_** States that this function, modifier or public state variable changes the behaviour of a function or modifier in a base contract.

### *Solidity Variables Types*

**Simple Types**

- Boolean, By default initialized value is false

- Signed and unsigned integers of various sizes

- | Unsigned Int | uint8 to uint 256 In steps on 8, default 0 | `uint8 public x = 255` |
- | Signed Int | int8 to int 256 In steps on 8, default 0 | `int8 public x = -10` |
- | Boolean | True,False | `bool public name` |

### *If statements*

In Solidity if statements look similar to javascript syntax,

```
if (expression 1) {
   Statement(s) to be executed if expression 1 is true
} else if (expression 2) {
   Statement(s) to be executed if expression 2 is true
} else if (expression 3) {
   Statement(s) to be executed if expression 3 is true
} else {
   Statement(s) to be executed if no expression is true
}

```



### *SafeMath underflow/overflow*

batch overflow exploit

```uint 8 public x = 255


function f1() public {
    x += 1;
}
```

255 + 1 = 0

This function causes a int overflow.

To avoid overflow in solidity.

An old fix for this issue was to use the library safemath

starting from solidity 0.8 arithmetic revert on underflow and overflow.
If a overflow or underflow happens after this version the contract will revert to the initial state.

https://hackernoon.com/hack-solidity-integer-overflow-and-underflow
https://peckshield.medium.com/alert-new-batchoverflow-bug-in-multiple-erc20-smart-contracts-cve-2018-10299-511067db6536

### *Fixed-size Arrays*

- Has a compile-time fixed size
- bytes1, bytes 2, ... bytes 32
- can hold any type
- length

```
// example


contract FixedSizeArrays{
    uint[3] public numbers = [2,3,4],
}

```

Outer Bounds Error

If you try to assess an array greater than or less than the array

**_Change An Array Value_**

```
// example: Outer Bounds Error


contract FixedSizeArrays{
    uint[3] public numbers = [2,3,4],



// change value
function setElement(uint index, uint value) public{
    numbers[index] = value
}


// get an array length.
function getLength() public view return(uint)_{
    return numbers.length
}
}

```

```
// example:


contract FixedSizeArrays{
bytes1 public b1;   //Default value  0x00
bytes2 public b2;   //Default value  0x0000
bytes3 public b3;   //Default value  0x0000000


function setBytesArray() public {

    b1 =  'a'
    b2 = 'ab'
    b3 = 'abc'
}

// after this function is called
bytes1 public b1;   //value  0x61  (ASCII code of `a` in hex)
bytes2 public b2;  // value  0x6162
bytes3 public b3;  // value  0x616263


// b3[0] = 'a'; // ERROR => can not change individual bytes

}

```

https://www.asciitable.com/

if an array is initialized with not a full value padding will be added.

### *Dynamically-sized Arrays*

- bytes
- string (UTF-8 encoded) is a dynamic array similar to bytes
- can hold any type
- members: length and push, pop

- length(length of array)
- push(push item to end of an array)
- pop(removes element from the end of an array)

```
//SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.5.0 <0.9.0;

contract DynamicArrays{
// dynamic array of type uint
uint[] public numbers;

// returning length
function getLength() public view returns(uint){
return numbers.length;
}

// appending a new element
function addElement(uint item) public{
numbers.push(item);
}

// returning an element at an index
function getElement(uint i) public view returns(uint){
if(i < numbers.length){
return numbers[i];
}
return 0;
}


// removing the last element of the array
function popElement() public{
numbers.pop();
}

function f() public{
// declaring a memory dynamic array
// it's not possible to resize memory arrays (push() and pop() are not available).
uint[] memory y = new uint[](3);
y[0] = 10;
y[1] = 20;
y[2] = 30;
numbers = y;
}

}

```

### *BytesAndStrings*

One difference between bytes and string is that we cannot add elements to a string variable as we can with type bytes

```
contract BytesAndString {

    bytes public b1 = 'abc'  // Value: 0x616263
    string public s1 = 'sbc  // Value: abc

}
```
### *Require*

require makes it so that the function will throw an error and stop executing if some condition is not true

require takes two parameters, first is the condition that you want to check and the second is an optional error message you want to show to the user when and if the value returned is false.

### *Inheritance*



### *Struct*

A Struct types are used to represent a record.

To define a Struct, you must use the **_ struct_** keyword

```
// Example

struct Car{
    string brand;
    uint year;
    uint price;
}


```


### *Events*
Event is an inheritable member of a contract.
An event can be declared using event keyword.

```
//Declare an Event
event Deposit(address indexed _from, bytes32 indexed _id, uint _value);
```


```emit Deposit(msg.sender, _id, msg.value)```


how front end will listen u

```YourContract.IntegersAdded(function(error, result) {
  // do something with result
})
```
### *Typecasting*

Typecasting converts data types 


### *Enums*

Enums restrict a variable to have one of only a few predefined values. The values in this enumerated list are called enums.

```

    // declaring a new enum type
    enum State {Open, Closed, Unknown}

 // declaring and initializing a new state variable of type State
    State public academyState = State.Open;


  if (academyState == State.Open){
        *** do something ***
            }

    later in the code you can use for if statements ect
```

### *Mappings*

Mapping is a reference type as arrays and structs. Following is the syntax to declare a mapping type.

`mapping(_KeyType => _ValueType)`

- All keys must have the same type and all values must have the same type
- The keys can not be of type mapping, dynamic array, enum or struct. the values can be any type.
- Mapping is always saved to storage. its a state variable.
- You can not loop through a mapping
- Keys are not saved into the mappings
- Mapping can be marked public. Solidity automatically create getter for it.

- \_KeyType − can be any built-in types plus bytes and string. No reference type or complex objects are allowed.

- \_ValueType − can be any type.

```
//SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.5.0 <0.9.0;
contract Auction{

    // declaring a variable of type mapping
    // keys are of type address and values of type uint
    mapping(address => uint) public bids;

    // initializing the mapping variable
    // the key is the address of the account that calls the function
    // and the value the value of wei sent when calling the function
    function bid() payable public{
        bids[msg.sender] = msg.value;
    }
}

```

### *Interfaces*
Solidity allows you to interact with other contracts without having their code by using their interface.

- The Solidity interface can inherit from other interfaces
- Contracts can inherit interfaces as they would inherit other contracts
- You can override an interface function
- Data types defined inside interfaces can be accessed from other       contracts

```
interface Divide {
   function getOutput() external view returns(uint);
}
```


### *Global Variables*



***commonly used***

```
//SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.5.0 <0.9.0;

contract GlobalVars{
// the current time as a timestamp (seconds from 01 Jan 1970)
uint public this_moment = block.timestamp; // `now` is deprecated and is an alias to block.timestamp)

// the current block number
uint public block_number = block.number;

// the block difficulty
uint public difficulty = block.difficulty;

// the block gas limit
uint public gaslimit = block.gaslimit;


address public owner;
uint public sentValue;

constructor(){
// msg.sender is the address that interacts with the contract (deploys it in this case)
owner = msg.sender;
}


function changeOwner() public{
// msg.sender is the address that interacts with the contract (calls the function in this case)
owner = msg.sender;
}

function sendEther() public payable{ // must be payable to receive ETH with the transaction
// msg.value is the value of wei sent in this transaction (when calling the function)
sentValue = msg.value;
}


// returning the balance of the contract
function getBalance() public view returns(uint){
return address(this).balance;
}
}


```

- `abi.decode(bytes memory encodedData, (...)) returns (...)`: :ref:`ABI <ABI>`-decodes
  the provided data. The types are given in parentheses as second argument.
  Example: `(uint a, uint[2] memory b, bytes memory c) = abi.decode(data, (uint, uint[2], bytes))`
- `abi.encode(...) returns (bytes memory)`: :ref:`ABI <ABI>`-encodes the given arguments
- `abi.encodePacked(...) returns (bytes memory)`: Performs :ref:`packed encoding <abi_packed_mode>` of
  the given arguments. Note that this encoding can be ambiguous!
- `abi.encodeWithSelector(bytes4 selector, ...) returns (bytes memory)`: :ref:`ABI <ABI>`-encodes
  the given arguments starting from the second and prepends the given four-byte selector
- `abi.encodeCall(function functionPointer, (...)) returns (bytes memory)`: ABI-encodes a call to `functionPointer` with the arguments found in the
  tuple. Performs a full type-check, ensuring the types match the function signature. Result equals `abi.encodeWithSelector(functionPointer.selector, (...))`
- `abi.encodeWithSignature(string memory signature, ...) returns (bytes memory)`: Equivalent
  to `abi.encodeWithSelector(bytes4(keccak256(bytes(signature)), ...)`
- `bytes.concat(...) returns (bytes memory)`: :ref:`Concatenates variable number of arguments to one byte array<bytes-concat>`
- `string.concat(...) returns (string memory)`: :ref:`Concatenates variable number of arguments to one string array<string-concat>`
- `block.basefee` (`uint`): current block's base fee (`EIP-3198 <https://eips.ethereum.org/EIPS/eip-3198>`_ and `EIP-1559 <https://eips.ethereum.org/EIPS/eip-1559>`_)
- `block.chainid` (`uint`): current chain id
- `block.coinbase` (`address payable`): current block miner's address
- `block.difficulty` (`uint`): current block difficulty
- `block.gaslimit` (`uint`): current block gaslimit
- `block.number` (`uint`): current block number
- `block.timestamp` (`uint`): current block timestamp in seconds since Unix epoch
- `gasleft() returns (uint256)`: remaining gas
- `msg.data` (`bytes`): complete calldata
- `msg.sender` (`address`): sender of the message (current call)
- `msg.sig` (`bytes4`): first four bytes of the calldata (i.e. function identifier)
- `msg.value` (`uint`): number of wei sent with the message
- `tx.gasprice` (`uint`): gas price of the transaction
- `tx.origin` (`address`): sender of the transaction (full call chain)
- `assert(bool condition)`: abort execution and revert state changes if condition is `false` (use for internal error)
- `require(bool condition)`: abort execution and revert state changes if condition is `false` (use
  for malformed input or error in external component)
- `require(bool condition, string memory message)`: abort execution and revert state changes if
  condition is `false` (use for malformed input or error in external component). Also provide error message.
- `revert()`: abort execution and revert state changes
- `revert(string memory message)`: abort execution and revert state changes providing an explanatory string
- `blockhash(uint blockNumber) returns (bytes32)`: hash of the given block - only works for 256 most recent blocks
- `keccak256(bytes memory) returns (bytes32)`: compute the Keccak-256 hash of the input
- `sha256(bytes memory) returns (bytes32)`: compute the SHA-256 hash of the input
- `ripemd160(bytes memory) returns (bytes20)`: compute the RIPEMD-160 hash of the input
- `ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address)`: recover address associated with
  the public key from elliptic curve signature, return zero on error
- `addmod(uint x, uint y, uint k) returns (uint)`: compute `(x + y) % k` where the addition is performed with
  arbitrary precision and does not wrap around at `2**256`. Assert that `k != 0` starting from version 0.5.0.
- `mulmod(uint x, uint y, uint k) returns (uint)`: compute `(x * y) % k` where the multiplication is performed
  with arbitrary precision and does not wrap around at `2**256`. Assert that `k != 0` starting from version 0.5.0.
- `this` (current contract's type): the current contract, explicitly convertible to `address` or `address payable`
- `super`: the contract one level higher in the inheritance hierarchy
- `selfdestruct(address payable recipient)`: destroy the current contract, sending its funds to the given address
- `<address>.balance` (`uint256`): balance of the :ref:`address` in Wei
- `<address>.code` (`bytes memory`): code at the :ref:`address` (can be empty)
- `<address>.codehash` (`bytes32`): the codehash of the :ref:`address`
- `<address payable>.send(uint256 amount) returns (bool)`: send given amount of Wei to :ref:`address`,
  returns `false` on failure
- `<address payable>.transfer(uint256 amount)`: send given amount of Wei to :ref:`address`, throws on failure
- `type(C).name` (`string`): the name of the contract
- `type(C).creationCode` (`bytes memory`): creation bytecode of the given contract, see :ref:`Type Information<meta-type>`.
- `type(C).runtimeCode` (`bytes memory`): runtime bytecode of the given contract, see :ref:`Type Information<meta-type>`.
- `type(I).interfaceId` (`bytes4`): value containing the EIP-165 interface identifier of the given interface, see :ref:`Type Information<meta-type>`.
- `type(T).min` (`T`): the minimum value representable by the integer type `T`, see :ref:`Type Information<meta-type>`.
- `type(T).max` (`T`): the maximum value representable by the integer type `T`, see :ref:`Type Information<meta-type>`.

### *Contract Address*

- Any contract has its own unique address which is generated at deployment
- The contract address is generated based on the address of the creator contract and the no. transactions of that account (nonce) it cant be calculated in advance
- there are 2 types of contracts plain and payable
  - Plain can not have ether sent to the contract
  - A smart contract can only have ETH sent to it if there is a defined **_ payable_** function
- Address has these members
  - balance
  - transfer()
  - send()
  - call() callcode() delegatecall()

### *Fallback Functions*


fallback function is an external function with neither a name, parameters, or return values. It is executed in one of the following cases:

- If a function identifier doesn’t match any of the available functions in a smart contract.
- If there was no data supplied along with the function call.
- Only one such unnamed function can be assigned to a contract.

### *Access Contract Balance*


//SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.5.0 <0.9.0;

- getBalance()
- require(owner == msg.sender, "Transfer failed, you are not the owner!!"); will prevent anyone stealing balance

```
 contract Deposit{
     address public owner;

     constructor(){
        owner = msg.sender;
     }

     receive() external payable{
     }


     function getBalance() public view returns(uint){
         return address(this).balance;
     }


    // transfering ether from the contract to another address (recipient)
     function transferEther(address payable recipient, uint amount) public returns(bool){
         // checking that only contract owner can send ether from the contract to another address
         require(owner == msg.sender, "Transfer failed, you are not the owner!!");

         if (amount <= getBalance()){
             // transfering the amount of wei from the contract to the recipient address
             // anyone who can call this function have access to the contract's funds
             recipient.transfer(amount);
             return true;
         }else{
             return false;
         }
     }
 }

```

# Solidity Concepts 


### *Immutability of Contracts*

Smart contracts are immutable so the contract can not be modified or updated again

"This is one reason security is such a huge concern in Solidity. If there's a flaw in your contract code, there's no way for you to patch it late"


BUT - developers can create inbuilt upgrade and delete functions to there contracts. 


