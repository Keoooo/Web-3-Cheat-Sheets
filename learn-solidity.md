# Solidity notes/ cheatsheet

### Versions

Add to top of file to clarify the version you want to use.

`pragma solidity ^0.8.0;`

nothing lower than 0.8.0 will be used in this example

Make sure the version is the same as the  `hardhat.config.js`

### Solidity Variables Types

**Simple Types**

- Boolean, By default initialized value is false

- Signed and unsigned integers of various sizes

- | Unsigned Int | uint8 to uint 256 In steps on 8, default 0 | `uint8 public x = 255` |
- | Signed Int | int8 to int 256 In steps on 8, default 0 | `int8 public x = -10` |
- | Boolean | True,False | `bool public name` |

### SafeMath underflow/overflow

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

### Fixed-size Arrays

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

```
// example: Outer Bounds Error


contract FixedSizeArrays{
    uint[3] public numbers = [2,3,4,5],
}

```

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

### Dynamically-sized Arrays

- bytes
- string (UTF-8 encoded) is a dynamic array similar to bytes
- can hold any type
- members: length and push

```


```
