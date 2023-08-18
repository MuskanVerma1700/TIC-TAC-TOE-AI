# TIC-TAC-TOE-AI
Implement an AI agent that plays the classic game of Tic-Tac-Toe against a human player. You can use algorithms like Minimax with or without Alpha-Beta Pruning to make the AI player unbeatable. This project will help you understand game theory and basic search  algorithms.

#code
import random

# The Tic-Tac-Toe board is represented as a list of lists (3x3 grid)
# 'X' represents the AI, 'O' represents the human player, and None represents an empty space
board = [
    [None, None, None],
    [None, None, None],
    [None, None, None]
]

# Function to check if a player has won
def check_winner(player):
    for i in range(3):
        if all([board[i][j] == player for j in range(3)]):
            return True
        if all([board[j][i] == player for j in range(3)]):
            return True
    if all([board[i][i] == player for i in range(3)]) or all([board[i][2 - i] == player for i in range(3)]):
        return True
    return False

# Function to check if the board is full
def is_board_full():
    return all(all(cell is not None for cell in row) for row in board)

# Minimax algorithm with Alpha-Beta Pruning
def minimax(board, depth, is_maximizing, alpha, beta):
    if check_winner('X'):
        return 1
    if check_winner('O'):
        return -1
    if is_board_full():
        return 0
    
    if is_maximizing:
        max_eval = -float('inf')
        for i in range(3):
            for j in range(3):
                if board[i][j] is None:
                    board[i][j] = 'X'
                    eval = minimax(board, depth + 1, False, alpha, beta)
                    board[i][j] = None
                    max_eval = max(max_eval, eval)
                    alpha = max(alpha, eval)
                    if beta <= alpha:
                        break
        return max_eval
    else:
        min_eval = float('inf')
        for i in range(3):
            for j in range(3):
                if board[i][j] is None:
                    board[i][j] = 'O'
                    eval = minimax(board, depth + 1, True, alpha, beta)
                    board[i][j] = None
                    min_eval = min(min_eval, eval)
                    beta = min(beta, eval)
                    if beta <= alpha:
                        break
        return min_eval

# Function for the AI's move
def ai_move():
    best_move = None
    best_eval = -float('inf')
    for i in range(3):
        for j in range(3):
            if board[i][j] is None:
                board[i][j] = 'X'
                eval = minimax(board, 0, False, -float('inf'), float('inf'))
                board[i][j] = None
                if eval > best_eval:
                    best_eval = eval
                    best_move = (i, j)
    return best_move

# Main game loop
while not (check_winner('X') or check_winner('O') or is_board_full()):
    print("Current board:")
    for row in board:
        print(row)
    
    human_row = int(input("Enter row (0, 1, or 2) for your move: "))
    human_col = int(input("Enter column (0, 1, or 2) for your move: "))
    
    if board[human_row][human_col] is None:
        board[human_row][human_col] = 'O'
    else:
        print("Cell is already occupied. Try again.")
        continue
    
    if check_winner('O'):
        print("You win!")
        break
    
    if is_board_full():
        print("It's a draw!")
        break
    
    ai_row, ai_col = ai_move()
    board[ai_row][ai_col] = 'X'
    
    if check_winner('X'):
        print("AI wins!")
        break

print("Final board:")
for row in board:
    print(row)
