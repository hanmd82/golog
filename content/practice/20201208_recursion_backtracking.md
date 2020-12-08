+++
title = "Recursion & Backtracking"

date = 2020-12-08T00:00:00+08:00
author = "Mingding Han"
+++

## Concepts

To solve a problem using recursion, make sure that:

- The problem can broken down into smaller problems of same type
- Problem has some base case(s)
- Base case is reached before the stack size limit exceeds

In backtracking, if an attempt at solving a subproblem doesn't reach the desired solution, then undo/rollback the steps in solving that subproblem, and try solving another subproblem.

---

# Exercises
### Ex1: NQueens

1st pass

```
package main

import "fmt"

func main() {
    const N = 4

    board := make([][]int, N)
    for i := range(board) {
        board[i] = make([]int, N)
        for j := range(board[i]) {
            board[i][j] = 0
        }
    }

    NQueens(board, N)

    for i := range(board) {
        for j := range(board[i]) {
            fmt.Printf("%d ", board[i][j])
        }
        fmt.Printf("\n")
    }
}

func IsAttacked(x int, y int, board [][]int) bool {
    // check for attack at row
    for j:= 0; j < len(board); j++ {
        if (board[x][j] == 1) {
            return true
        }
    }

    // check for attack at column
    for i := 0; i < len(board); i++ {
        if (board[i][y] == 1) {
            return true
        }
    }

    // check for attack at diagonals
    sides := make([]int, len(board))
    for p := range(sides) {
        for q := range(sides) {
            if (p + q == x + y) && (board[p][q] == 1) {
                return true
            }

            if (p - q == x - y) && (board[p][q] == 1) {
                return true
            }
        }
    }
    return false
}

func NQueens(board [][]int, n int) bool {
    fmt.Printf("%d Queens left\n===========\n", n)
    if n == 0 {
        return true
    }

    sides := make([]int, n)
    for i := range(sides) {
        for j := range(sides) {
            if (IsAttacked(i, j, board)) {
                continue
            }
            board[i][j] = 1
            fmt.Printf("Added Queen to (%d,%d)\n", i, j)

            if NQueens(board, n-1) {
                return true
            }
            board[i][j] = 0
            fmt.Printf("Removed Queen from (%d,%d)\n", i, j)
        }
    }

    fmt.Printf("(0,0) is attacked: '%v'\n", IsAttacked(0,0,board))
    board[0][0] = 1
    fmt.Printf("Added Queen to (0,0)\n")

    fmt.Printf("(0,1) is attacked: '%v'\n", IsAttacked(0,1,board))
    fmt.Printf("(0,2) is attacked: '%v'\n", IsAttacked(0,2,board))
    fmt.Printf("(0,3) is attacked: '%v'\n", IsAttacked(0,3,board))
    fmt.Println("")

    fmt.Printf("(1,0) is attacked: '%v'\n", IsAttacked(1,0,board))
    fmt.Printf("(1,1) is attacked: '%v'\n", IsAttacked(1,1,board))
    fmt.Printf("(1,2) is attacked: '%v'\n", IsAttacked(1,2,board))
    board[1][2] = 1
    fmt.Printf("Added Queen to (1,2)\n")
    fmt.Printf("(1,3) is attacked: '%v'\n", IsAttacked(1,3,board))
    fmt.Println("")

    fmt.Printf("(2,0) is attacked: '%v'\n", IsAttacked(2,0,board))
    fmt.Printf("(2,1) is attacked: '%v'\n", IsAttacked(2,1,board))
    fmt.Printf("(2,2) is attacked: '%v'\n", IsAttacked(2,2,board))
    fmt.Printf("(2,3) is attacked: '%v'\n", IsAttacked(2,3,board))
    fmt.Println("")

    fmt.Printf("(3,0) is attacked: '%v'\n", IsAttacked(3,0,board))
    fmt.Printf("(3,1) is attacked: '%v'\n", IsAttacked(3,1,board))
    board[3][1] = 1
    fmt.Printf("Added Queen to (3,1)\n")

    fmt.Printf("(3,2) is attacked: '%v'\n", IsAttacked(3,2,board))
    fmt.Printf("(3,3) is attacked: '%v'\n", IsAttacked(3,3,board))
    fmt.Println("")

    fmt.Println("")
    return false
}
```

Output
```
1 0 0 0
0 0 1 0
0 0 0 0
0 1 0 0
```

---

2nd pass
```
package main

import "fmt"

func main() {
    const N = 4

    board := make([][]int, N)
    for i := range(board) {
        board[i] = make([]int, N)
        for j := range(board[i]) {
            board[i][j] = 0
        }
    }

    NQueens(board, N)

    fmt.Println("")
    for i := range(board) {
        for j := range(board[i]) {
            fmt.Printf("%d ", board[i][j])
        }
        fmt.Printf("\n")
    }
}

func IsAttacked(x int, y int, board [][]int) bool {
    // check for attack at row
    for j:= 0; j < len(board); j++ {
        if (board[x][j] == 1) {
            return true
        }
    }

    // check for attack at column
    for i := 0; i < len(board); i++ {
        if (board[i][y] == 1) {
            return true
        }
    }

    // check for attack at diagonals
    sides := make([]int, len(board))
    for p := range(sides) {
        for q := range(sides) {
            if (p + q == x + y) && (board[p][q] == 1) {
                return true
            }

            if (p - q == x - y) && (board[p][q] == 1) {
                return true
            }
        }
    }
    return false
}

func NQueens(board [][]int, n int) bool {
    fmt.Printf("%d Queens left\n", n)
    if n == 0 {
        return true
    }

    sides := make([]int, len(board))
    for i := range(sides) {
        for j := range(sides) {
            if (IsAttacked(i, j, board)) {
                continue
            }
            board[i][j] = 1
            fmt.Printf("  Added Queen to (%d,%d)\n", i, j)

            if NQueens(board, n-1) {
                return true
            }
            board[i][j] = 0
            fmt.Printf("  Removed Queen from (%d,%d) - %d queens left\n-----\n", i, j, n)
        }
    }
    return false
}
```

Output

```
0 1 0 0
0 0 0 1
1 0 0 0
0 0 1 0
```

---
# References
- https://www.hackerearth.com/practice/basic-programming/recursion/recursion-and-backtracking/tutorial/
