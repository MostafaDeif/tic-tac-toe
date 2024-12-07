### Detailed Explanation of the Tic Tac Toe Code

The following explanation provides a detailed walkthrough of the Tic Tac Toe game code. The code is structured to let a player compete against an AI opponent powered by the Minimax algorithm. Each function is explained to ensure clarity and understanding of the program's operation. This explanation can serve as a comprehensive reference for academic or personal use.

---

#### 1. *Importing Libraries*
python
import numpy as np

The code imports the numpy library, which is used for creating and manipulating a 2D array representing the game board. This library simplifies operations such as checking rows, columns, and diagonals for a winner.

---

#### 2. **Function: initialize_board**
python
def initialize_board():
    return np.full((3, 3), ' ')

- *Purpose*: This function initializes the Tic Tac Toe board as a 3x3 grid filled with empty spaces (' ').
- *Explanation*: 
  - The np.full((3, 3), ' ') function creates a 3x3 NumPy array where every cell initially contains a blank space.
  - This blank board serves as the starting state of the game.

---

#### 3. **Function: display_board**
python
def display_board(board):
    for row in board:
        print('|'.join(row))
        print('-' * 5)

- *Purpose*: Displays the current state of the game board to the player.
- *Explanation*:
  - The for row in board loop iterates over each row of the board.
  - The '|'.join(row) function formats the row by separating each cell with a vertical bar (|).
  - After printing each row, a separator line ('-' * 5) is printed to visually divide the rows.
- *Output Example*:
  
  X|O| 
  -----
   |X|O
  -----
   | |X
  

---

#### 4. **Function: check_winner**
python
def check_winner(board):
    # Check rows and columns
    for i in range(3):
        if np.all(board[i, :] == board[i, 0]) and board[i, 0] != ' ':
            return board[i, 0]
        if np.all(board[:, i] == board[0, i]) and board[0, i] != ' ':
            return board[0, i]
    # Check diagonals
    if board[0, 0] == board[1, 1] == board[2, 2] and board[0, 0] != ' ':
        return board[0, 0]
    if board[0, 2] == board[1, 1] == board[2, 0] and board[0, 2] != ' ':
        return board[0, 2]
    return None

- *Purpose*: Checks if there is a winner by evaluating rows, columns, and diagonals.
- *Explanation*:
  1. *Rows*: For each row (i), np.all(board[i, :] == board[i, 0]) checks if all elements in that row are the same and not empty.
  2. *Columns*: For each column (i), np.all(board[:, i] == board[0, i]) checks if all elements in that column are the same and not empty.
  3. *Diagonals*:
      - The first diagonal (board[0, 0] == board[1, 1] == board[2, 2]) is checked.
      - The second diagonal (board[0, 2] == board[1, 1] == board[2, 0]) is also checked.
  4. If a match is found, the winning symbol ('X' or 'O') is returned. If no winner is detected, the function returns None.

---

#### 5. **Function: is_full**
python
def is_full(board):
    return not np.any(board == ' ')

- *Purpose*: Determines whether the board is full (no empty spaces remaining).
- *Explanation*:
  - np.any(board == ' ') checks if there are any empty spaces (' ') on the board.
  - not negates the result. If there are no empty spaces, the function returns True (board is full).

---

#### 6. **Function: minimax**
python
def minimax(board, is_maximizing, depth=0):
    winner = check_winner(board)
    if winner == 'X':  # X is maximizing player
        return 10 - depth
    elif winner == 'O':  # O is minimizing player
        return depth - 10
    elif is_full(board):
        return 0  # Draw

- *Purpose*: Implements the Minimax algorithm to evaluate the best possible move for the AI.
- *Explanation*:
  1. *Base Cases*:
      - If 'X' wins, return a high positive score (10 - depth).
      - If 'O' wins, return a high negative score (depth - 10).
      - If the board is full (a draw), return 0.
  2. *Recursive Evaluation*:
      - For the maximizing player (is_maximizing=True), the algorithm searches for the highest score.
      - For the minimizing player (is_maximizing=False), the algorithm searches for the lowest score.

---

#### 7. **Function: find_best_move**
python
def find_best_move(board, player):
    best_score = -np.inf if player == 'X' else np.inf
    best_move = None
    for i in range(3):
        for j in range(3):
            if board[i, j] == ' ':
                board[i, j] = player
                score = minimax(board, player == 'O', depth=0)
                board[i, j] = ' '
                if (player == 'X' and score > best_score) or (player == 'O' and score < best_score):
                    best_score = score
                    best_move = (i, j)
    return best_move

- *Purpose*: Finds the optimal move for the AI using the Minimax algorithm.
- *Explanation*:
  - The function iterates over all empty cells on the board.
  - For each empty cell, it simulates the move, evaluates it using minimax, and tracks the best move based on the score.

---

#### 8. **Function: player_move**
python
def player_move(board, player):
    while True:
        try:
            move = input("Enter your move (row and column separated by space, e.g., '1 2'): ")
            row, col = map(int, move.split())
            if board[row, col] == ' ':
                board[row, col] = player
                break
            else:
                print("Cell is already occupied. Try again.")
        except (ValueError, IndexError):
            print("Invalid input. Please enter row and column as numbers between 0 and 2.")

- *Purpose*: Handles input from the player and updates the board.
- *Explanation*:
  - The function validates the player's input to ensure it is within the board's bounds and the selected cell is not already occupied.

---

#### 9. **Function: play_tic_tac_toe**
- *Purpose*: The main function that drives the game logic, alternating turns between the player and the AI, and handles game states (win, loss, or draw).
- *Explanation*:
  - Initializes the board and sets up the player's symbol.
  - Alternates turns between the player and the AI until a winner is determined or the board is full.
  - After each game, prompts the user to decide whether to play again.

---

#### Conclusion
This code uses advanced algorithms and efficient logic to simulate a competitive game of Tic Tac Toe. It ensures fair play, intelligent decision-making from the AI, and a user-friendly experience for players.
