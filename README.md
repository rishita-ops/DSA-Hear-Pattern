# DSA — Heart Pattern ❤️

A focused Python implementation that **prints a heart made of asterisks** on a 6 × 7 grid using **two nested loops and four coordinate rules**. Clean, minimal, and correct — this program is the entry point to understanding **pattern printing**, the row/column (`i`, `j`) view of a picture, and the difference between **drawing an outline with equalities** and **filling a shape with inequalities**. The same idea — _for every cell `(i, j)`, decide whether to print a star or a space_ — sits behind every star, pyramid, and diamond pattern.

---

## Problem Statement

Print a **heart** made of `*` characters, **6 rows tall and 7 columns wide**. The program takes **no input**.

**Example Input:**

```
(none)
```

**Example Output:**

```
 ** **
*  *  *
*     *
 *   *
  * *
   *
```

---

## The Code

```python
for i in range(6):
    for j in range(7):
        if (i == 0 and j % 3 != 0) or (i == 1 and j % 3 == 0) or (i - j == 2) or (i + j == 8):
            print("*", end="")
        else:
            print(" ", end="")
    print()
```

---

## The Critical Idea — Every Cell Asks One Question

This is the most important thing to understand about this program.

**A pattern is just a set of cells.** The program visits every cell `(i, j)` of the grid — `i` is the **row** (0 to 5, top to bottom) and `j` is the **column** (0 to 6, left to right) — and asks one question: _does this cell belong to the heart?_ If yes, it prints `*`. If no, it prints a space so the next star stays in the right column.

A heart needs **four rules**, joined with `or`:

| Rule  | Condition               | Cells it selects          | What it draws                                     |
| ----- | ----------------------- | ------------------------- | ------------------------------------------------- |
| **A** | `i == 0 and j % 3 != 0` | `(0,1) (0,2) (0,4) (0,5)` | The **tops** of the two lobes                     |
| **B** | `i == 1 and j % 3 == 0` | `(1,0) (1,3) (1,6)`       | The **outer sides** and the **dip** in the middle |
| **C** | `i - j == 2`            | `(2,0) (3,1) (4,2) (5,3)` | The **left slope**, going down and to the right   |
| **D** | `i + j == 8`            | `(2,6) (3,5) (4,4) (5,3)` | The **right slope**, going down and to the left   |

Here is the grid with each star labeled by the rule that draws it (`.` is a space):

```
         j = 0 1 2 3 4 5 6
        ┌──────────────────
i = 0   │    . A A . A A .
i = 1   │    B . . B . . B
i = 2   │    C . . . . . D
i = 3   │    . C . . . D .
i = 4   │    . . C . D . .
i = 5   │    . . . X . . .
```

`X` marks cell `(5, 3)`, the bottom tip, where **both** C and D are true. Because the rules are joined with `or`, it still prints just one star.

**Why diagonals are easy to write.** Along a line going down-right, the row and column grow together, so their **difference** stays the same. Along a line going down-left, one grows while the other shrinks, so their **sum** stays the same.

| Shape                 | Equation     | Direction                               |
| --------------------- | ------------ | --------------------------------------- |
| Main diagonal         | `i == j`     | Down and right                          |
| Any parallel diagonal | `i - j == c` | Down and right (this code uses `c = 2`) |
| Any anti-diagonal     | `i + j == c` | Down and left (this code uses `c = 8`)  |

**Why `j % 3` draws the lobes.** The multiples of 3 among the columns `0 … 6` are `0, 3, 6`. In row 0 the rule `j % 3 != 0` **skips** those columns, leaving the two lobe tops with a gap in the middle. In row 1 the opposite rule `j % 3 == 0` **keeps** exactly those columns, giving the two sides and the center dip.

---

## How the Loops Work

```python
for i in range(6):
    for j in range(7):
        if (...four rules...):
            print("*", end="")
        else:
            print(" ", end="")
    print()
```

- **Outer loop:** `i` runs `0 … 5` — one pass per **row**
- **Inner loop:** `j` runs `0 … 6` — one pass per **column** in that row
- **The test:** the four rules joined with `or`; any one of them being true selects the cell
- **Star:** `print("*", end="")` prints without moving to a new line
- **Space:** `print(" ", end="")` also stays on the line, and holds the column position
- **New line:** `print()` after the inner loop ends the row

> **Off-by-one note:** `range(6)` gives rows `0 … 5` and `range(7)` gives columns `0 … 6`. The right slope reaches column `6` (at row 2, since `2 + 6 == 8`), so shrinking the inner loop to `range(6)` would cut off the heart's right edge. Likewise, `range(5)` for the rows would cut off the bottom tip.

> **Why spaces must be printed:** an empty cell has to print a **space**, not nothing. If empty cells printed nothing, every star would slide to the left and the heart would collapse.

---

## Algorithm (Pseudocode)

```
for each row i from 0 to 5:
    for each column j from 0 to 6:
        if   (row 0 and j is not a multiple of 3)       -> star
          or (row 1 and j is a multiple of 3)           -> star
          or (i - j == 2)                               -> star
          or (i + j == 8):                              -> star
            print "*"
        else:
            print " "
    go to the next line
```

---

## Dry Run

**Row by row** (`·` stands for a space):

| `i` | Rules that fire         | Star columns `j` | Printed row |
| --- | ----------------------- | ---------------- | ----------- |
| 0   | A                       | 1, 2, 4, 5       | `·**·**·`   |
| 1   | B                       | 0, 3, 6          | `*··*··*`   |
| 2   | C, D                    | 0, 6             | `*·····*`   |
| 3   | C, D                    | 1, 5             | `·*···*·`   |
| 4   | C, D                    | 2, 4             | `··*·*··`   |
| 5   | C and D (the same cell) | 3                | `···*···`   |

**Cell by cell for row `i = 1`:**

| `j` | `j % 3 == 0`? | Rule B true? | Other rules (`i - j == 2`, `i + j == 8`) | Printed |
| --- | ------------- | ------------ | ---------------------------------------- | ------- |
| 0   | Yes           | Yes          | No (`1 - 0 = 1`, `1 + 0 = 1`)            | `*`     |
| 1   | No            | No           | No                                       | space   |
| 2   | No            | No           | No                                       | space   |
| 3   | Yes           | Yes          | No                                       | `*`     |
| 4   | No            | No           | No                                       | space   |
| 5   | No            | No           | No                                       | space   |
| 6   | Yes           | Yes          | No (`1 - 6 = -5`, `1 + 6 = 7`)           | `*`     |

**Row 1 printed:** `*  *  *` ✅

**Whole program output:** the heart shown in the Problem Statement ✅

---

## Watch the Heart Appear

The heart is drawn **one row at a time**. Here is the picture after each pass of the outer loop:

**After row 0** — `i = 0`

```
 ** **
```

**After row 1** — `i = 1`

```
 ** **
*  *  *
```

**After row 2** — `i = 2`

```
 ** **
*  *  *
*     *
```

**After row 3** — `i = 3`

```
 ** **
*  *  *
*     *
 *   *
```

**After row 4** — `i = 4`

```
 ** **
*  *  *
*     *
 *   *
  * *
```

**After row 5** — `i = 5`, the heart is complete ❤️

```
 ** **
*  *  *
*     *
 *   *
  * *
   *
```

---

## Complexity Analysis

| Metric | Complexity                                                                                                      |
| ------ | --------------------------------------------------------------------------------------------------------------- |
| Time   | **O(R × C)** — every one of the `R` rows and `C` columns is checked once; here that is `6 × 7 = 42` cell checks |
| Space  | **O(1)** — nothing is stored; every character is printed as soon as it is decided                               |

For this fixed heart the work is constant. If the same idea were scaled to a heart of size `n`, the work would grow as **O(n²)**, because the grid has `n²` cells.

---

## Edge Cases

| Scenario                                | Behavior                                                                                                 |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Cell `(5, 3)` matches two rules at once | `or` prints a **single** star ✅                                                                         |
| Row 0 columns `0`, `3`, `6`             | `j % 3 != 0` is false, so they print spaces — this forms the gap between the lobes ✅                    |
| No input needed                         | The program runs the same way every time ✅                                                              |
| Rows end with spaces                    | Empty cells are printed, so every row is exactly 7 characters wide ✅                                    |
| `end=""` removed from the `print` calls | Every character would land on its own line ⚠️                                                            |
| `range(7)` changed to `range(6)`        | The right edge of the heart is cut off ⚠️                                                                |
| `range(6)` changed to `range(5)`        | The bottom tip is missing ⚠️                                                                             |
| Non-monospace font                      | The columns no longer line up, and the heart looks distorted ⚠️                                          |
| Changing the grid size                  | The numbers `2`, `3`, `6`, `7`, and `8` are tied to this exact grid, so they must all be recalculated ⚠️ |

---

## Code-Quality Notes

**Magic numbers.** The values `6`, `7`, `3`, `2`, and `8` are tied to this one heart. Giving them names (such as `ROWS`, `COLS`) or computing them from a size makes the code easier to resize and explain.

**One `print` call per character.** The code makes 42 small `print` calls. Building each row as a string and printing it once is cleaner and avoids the `end=""` bookkeeping:

```python
for i in range(6):
    print("".join(
        "*" if (i == 0 and j % 3 != 0) or (i == 1 and j % 3 == 0) or (i - j == 2) or (i + j == 8) else " "
        for j in range(7)
    ))
```

**The long condition.** Putting the four rules into a small function, for example `is_heart_cell(i, j)`, lets each rule be named, tested, and reused.

**Trailing spaces.** Each row ends with spaces, which are invisible but part of the output. That is harmless on screen, but matters if the output is compared against a file.

---

## The Natural Extension — A Filled Heart

The outline uses **equalities** (`i - j == 2`, `i + j == 8`) to pick the cells **on** the two slopes. A filled heart uses **inequalities** to pick every cell **between** them:

```python
for i in range(6):
    for j in range(7):
        if (i == 0 and j % 3 != 0) or (i > 0 and i - 2 <= j <= 8 - i):
            print("*", end="")
        else:
            print(" ", end="")
    print()
```

For every row below the top, the left slope is the column `j = i - 2` and the right slope is `j = 8 - i`. The heart's inside is everything between them:

| `i` | Fill range `i - 2 <= j <= 8 - i`          | Stars drawn |
| --- | ----------------------------------------- | ----------- |
| 0   | (special rule: `j % 3 != 0`)              | `·**·**·`   |
| 1   | `-1 <= j <= 7`, which covers every column | `*******`   |
| 2   | `0 <= j <= 6`                             | `*******`   |
| 3   | `1 <= j <= 5`                             | `·*****·`   |
| 4   | `2 <= j <= 4`                             | `··***··`   |
| 5   | `3 <= j <= 3`                             | `···*···`   |

**Output:**

```
 ** **
*******
*******
 *****
  ***
   *
```

| Property    | Outline heart (this code)              | Filled heart                                  |
| ----------- | -------------------------------------- | --------------------------------------------- |
| Slope cells | **Equalities** pick cells on the lines | **Inequalities** pick cells between the lines |
| Rules       | Four rules (A, B, C, D)                | Two rules (the top row, and the fill range)   |
| Look        | Hollow                                 | Solid                                         |
| Time        | O(R × C)                               | O(R × C)                                      |
| Space       | O(1)                                   | O(1)                                          |

---

## Repository Structure

```
DSA-Heart-Pattern/
│
├── heart_pattern.py      # Main Python implementation
└── README.md             # Project documentation
```

---

## How to Compile and Run

**Prerequisites:** Python 3.8+

```bash
# Clone the repository
git clone https://github.com/rishita-ops/DSA-Heart-Pattern.git
cd DSA-Heart-Pattern

# Run
python3 heart_pattern.py
```

**On Windows:**

```bash
python heart_pattern.py
```

**Expected output:**

```
 ** **
*  *  *
*     *
 *   *
  * *
   *
```

---

## Key Concepts Covered

- **Pattern printing** — turning a picture into a rule for each cell
- **Nested loops** — the outer loop picks the row, the inner loop picks the column
- **Row/column coordinates** — every star is a pair `(i, j)`
- **Diagonals with `i - j` and `i + j`** — a constant difference or sum describes a diagonal line
- **Modulo (`%`)** — `j % 3` picks out repeating columns
- **Combining conditions with `or`** — a cell is drawn if **any** rule selects it
- **`print(..., end="")`** — staying on the same line, with `print()` ending the row
- **Printing spaces** — empty cells must hold their column position
- **Equalities vs inequalities** — drawing an outline versus filling a shape

---

## Why This Problem Matters in DSA

The row/column coordinate view appears across a broad family of problems:

| Problem / Concept                     | Connection                                                 |
| ------------------------------------- | ---------------------------------------------------------- |
| **Zigzag Conversion** (LeetCode #6)   | Placing characters on a grid by row, then reading them out |
| **Spiral Matrix** (LeetCode #54)      | Walking a grid with row and column bounds                  |
| **Pascal's Triangle** (LeetCode #118) | Building a triangular pattern row by row                   |
| **Diagonal Traverse** (LeetCode #498) | Moving along diagonals where `i + j` is constant           |
| **Rotate Image** (LeetCode #48)       | Mapping each cell `(i, j)` to a new position               |
| **Set Matrix Zeroes** (LeetCode #73)  | Using row and column indices to mark whole lines           |

Every problem that works on a grid, a matrix, or a printed pattern traces back to the `(i, j)` thinking introduced here.

---

## Contributing

Contributions are welcome. Consider adding:

- A version where the **heart size is a parameter** instead of hard-coded numbers
- A version that builds each row as a string and prints it **once**
- A heart printed with the **`♥`** character or in **color** using ANSI codes
- A function `is_heart_cell(i, j)` with a small test for every cell
- The **filled heart** as a separate file
- Implementations in C++, Java, or JavaScript

```bash
git checkout -b feature/your-feature
git commit -m "Add: your feature description"
git push origin feature/your-feature
# Then open a Pull Request
```

---

## License

This project is open-source and available under the [MIT License](LICENSE).

---

_Part of a structured DSA practice series — fundamentals, done right._
