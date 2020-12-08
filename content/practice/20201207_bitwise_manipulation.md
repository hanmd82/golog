+++
title = "Bit Manipulation"

date = 2020-12-07T15:00:00+08:00
author = "Mingding Han"
+++

## Bitwise Operators:
- NOT ( `~` )
- AND ( `&` )
- OR ( `|` )
- XOR ( `^` )
- Left Shift ( `<<` ) => multiply by 2. If left-shifting `k` bits, multiply by `math.Pow(2, k)` or `2**k`
- Right Shift ( `>>` ) => divide by 2. If right-shifting `k` bits, divide by `math.Pow(2, k)` or `2**k`

---

# Exercises
### Ex1: IsPowerOfTwo

Check if a given number is a power of 2

```
package main

import "fmt"

func main() {
    fmt.Printf("0: \t want 'false', got '%v'\n", IsPowerOfTwo(0))
    fmt.Printf("1: \t want 'false', got '%v'\n", IsPowerOfTwo(1))
    fmt.Printf("2: \t want 'true', got '%v'\n", IsPowerOfTwo(2))
    fmt.Printf("4: \t want 'true', got '%v'\n", IsPowerOfTwo(4))
    fmt.Printf("5: \t want 'false', got '%v'\n", IsPowerOfTwo(5))
    fmt.Printf("6: \t want 'false', got '%v'\n", IsPowerOfTwo(6))
    fmt.Printf("1024: \t want 'true', got '%v'\n", IsPowerOfTwo(1024))
    fmt.Printf("1240: \t want 'false', got '%v'\n", IsPowerOfTwo(1240))
}

// IsPowerOfTwo checks if a given number is a power of 2
func IsPowerOfTwo(x int) bool {
    return IsPowerOfTwoBitwise(x)
}

func IsPowerOfTwoBasic(x int) bool {
    if x == 0 {
        return false
    }
    for (x%2 == 0) {
        x /= 2
    }
    return x == 1
}

func IsPowerOfTwoBitwise(x int) bool {
    return (x != 0) && (x & (x-1) == 0)
}
```

Notes:
- Numbers which are powers of 2 have one and only one bit set in their binary representation
- `x & (x-1)` has all the bits equal to `x` except for the rightmost `1` bit in `x`, e.g.:
  - `x`: 150 => `b(10010110)`
  - `x-1`: 149 => `b(10010101)`
  - `x & (x-1)` => `b(10010100)`
- For a non-zero number `x`:
    - if `x` is not a power of 2, it will have `1` bit in more than one place
    - if `x` is a power of 2, then `x & (x-1)` will flip that `1` bit, resulting in 0 value
- Time complexity of `IsPowerOfTwoBasic` is `O(logN)`
- Time complexity of `IsPowerOfTwoBitwise` is `O(1)`: just one bitwise `AND` operation

---

### Ex2: NumBinaryOnes

Count the number of ones in the binary representation of the given number

```
package main

import "fmt"

func main() {
    fmt.Printf("0: \t want '0', got '%v'\n", NumBinaryOnes(0))   //     '0'
    fmt.Printf("1: \t want '1', got '%v'\n", NumBinaryOnes(1))   //     '1'
    fmt.Printf("2: \t want '1', got '%v'\n", NumBinaryOnes(2))   //    '10'
    fmt.Printf("8: \t want '1', got '%v'\n", NumBinaryOnes(8))   //  '1000'
    fmt.Printf("10: \t want '2', got '%v'\n", NumBinaryOnes(10)) //  '1010'
    fmt.Printf("11: \t want '3', got '%v'\n", NumBinaryOnes(11)) //  '1011'
    fmt.Printf("15: \t want '4', got '%v'\n", NumBinaryOnes(15)) //  '1111'
    fmt.Printf("23: \t want '4', got '%v'\n", NumBinaryOnes(23)) // '10111'
}

// NumBinaryOnes counts the number of ones in the binary representation of the given number
func NumBinaryOnes(x int) int {
    return NumBinaryOnesBitwise(x)
}

func NumBinaryOnesBasic(x int) int {
    var count int
    for (x != 0) {
        if (x % 2 == 1) {
            count++
        }
        x /= 2
    }
    return count
}

func NumBinaryOnesBitwise(x int) int {
    var count int
    for (x != 0) {
        x = x & (x-1) // flip the rightmost set bit
        count++
    }
    return count
}
```

Notes:
- Since `x & (x-1)` has all the bits equal to `x` except for the rightmost `1` bit in `x` ('flipped'),
  - increment a counter and assign `x = x & (x-1)`, until `x == 0` (i.e. all 1's have been flipped)
  - the value of the counter will be the number of 1's in the original `x`
- Time complexity of `NumBinaryOnesBasic` is `O(logN)`, every time
- Time complexity of `NumBinaryOnesBitwise` is `O(k)`, given are `k` 1's in `x`. Upper bound: `O(logN)`

---

### Ex3: CheckBitSet

Check whether the i-th bit of given number is set

```
package main

import "fmt"

func main() {
    fmt.Printf("(0, 0): \t want 'false', got '%v'\n", CheckBitSet(0, 0))   //     '0'
    fmt.Printf("(0, 1): \t want 'true', got '%v'\n", CheckBitSet(0, 1))    //     '1'
    fmt.Printf("(0, 2): \t want 'false', got '%v'\n", CheckBitSet(0, 2))   //    '10'
    fmt.Printf("(1, 2): \t want 'true', got '%v'\n", CheckBitSet(1, 2))    //    '10'
    fmt.Printf("(0, 6): \t want 'false', got '%v'\n", CheckBitSet(0, 6))   //   '110'
    fmt.Printf("(1, 6): \t want 'true', got '%v'\n", CheckBitSet(1, 6))    //   '110'
    fmt.Printf("(2, 6): \t want 'true', got '%v'\n", CheckBitSet(2, 6))    //   '110'
    fmt.Printf("(2, 20): \t want 'true', got '%v'\n", CheckBitSet(2, 20))  // '10100'
    fmt.Printf("(3, 20): \t want 'false', got '%v'\n", CheckBitSet(3, 20)) // '10100'
    fmt.Printf("(4, 20): \t want 'true', got '%v'\n", CheckBitSet(4, 20))  // '10100'
}

// CheckBitSet checks whether the i-th bit of number x is set
func CheckBitSet(i, x int) bool {
    return (x & (1<<i)) != 0
}
```

---

### Ex4: LargestPowerOfTwo

Find the largest power of 2, which is less than or equal to the given number

```
package main

import "fmt"

func main() {
    fmt.Printf("2: \t want '2', got '%v'\n", LargestPowerOfTwo(2))
    fmt.Printf("3: \t want '2', got '%v'\n", LargestPowerOfTwo(3))
    fmt.Printf("5: \t want '4', got '%v'\n", LargestPowerOfTwo(5))
    fmt.Printf("10: \t want '8', got '%v'\n", LargestPowerOfTwo(10))
    fmt.Printf("15: \t want '8', got '%v'\n", LargestPowerOfTwo(15))
    fmt.Printf("21: \t want '16', got '%v'\n", LargestPowerOfTwo(21))
}

// LargestPowerOfTwo return the largest power of 2, which is less than or equal to the given number n
func LargestPowerOfTwo(n int) int {
    // change all the bits in n to '1'
    n |= (n >> 1)
    n |= (n >> 2)
    n |= (n >> 4)
    n |= (n >> 8)
    n |= (n >> 16)

    return (n+1) >> 1
}
```

---
# References
- https://www.hackerearth.com/practice/basic-programming/bit-manipulation/basics-of-bit-manipulation/tutorial/
