# Backtracking
- choices
    - decision space
- constraints
    - decision space is restricted somehow
- goal

### ex. sudoku board
- we need to fill cells by making choices
- key choice is on the cell
- a cell
    - sits on a row and a col

```java
solve(row,col ...){
    //goal is to reach base case
    if (col == board[row].length){
        jlfasdlk
    }

    for(int value = 1; value <= 9; value++){ // our for loop is for exploration
        board[row][col] = value;
        if(validPlacement(row,col...)){
            if(solve(row,col+1,board)){
                return true
            }
        }
    }
    board[row][col] = EMPTY_ENTRY //undo deciison after exploring
}

```
- think of subproblems
- think of only one row
- our choice at every single cell is to place a number
    - 1,2,3,4,5,6,7,8,9
    - this is our decision space ^^

- we need to validate the row, col, and subgrid that the numnber sits in
- does the cell break the row, col, and subgrid

