import streamlit as st
import numpy as np

def is_valid(board, num, pos):
    row, col = pos
    for i in range(9):
        if board[row][i] == num and col != i:
            return False
        if board[i][col] == num and row != i:
            return False
    start_row, start_col = 3 * (row//3), 3 * (col//3)
    for i in range(start_row, start_row + 3):
        for j in range(start_col, start_col + 3):
            if board[i][j] == num and (i, j) != pos:
                return False
    return True

def find_empty(board):
    for i in range(9):
        for j in range(9):
            if board[i][j] == 0:
                return i, j
    return None

def solve(board):
    empty = find_empty(board)
    if not empty:
        return True
    row, col = empty
    for num in range(1, 10):
        if is_valid(board, num, (row, col)):
            board[row][col] = num
            if solve(board):
                return True
            board[row][col] = 0
    return False

def get_new_puzzle():
    # Placeholder: replace with call to an open source generator
    return [
        [5,3,0,0,7,0,0,0,0],
        [6,0,0,1,9,5,0,0,0],
        [0,9,8,0,0,0,0,6,0],
        [8,0,0,0,6,0,0,0,3],
        [4,0,0,8,0,3,0,0,1],
        [7,0,0,0,2,0,0,0,6],
        [0,6,0,0,0,0,2,8,0],
        [0,0,0,4,1,9,0,0,5],
        [0,0,0,0,8,0,0,7,9],
    ]

st.title("Sudoku Solver Web App")

if "board" not in st.session_state:
    st.session_state.initial_board = get_new_puzzle()
    st.session_state.board = np.array(st.session_state.initial_board)

if st.button("New Game"):
    st.session_state.initial_board = get_new_puzzle()
    st.session_state.board = np.array(st.session_state.initial_board)

if st.button("Reset"):
    st.session_state.board = np.array(st.session_state.initial_board)

board = st.session_state.board
new_board = board.copy()

for i in range(9):
    cols = st.columns(9)
    for j in range(9):
        disabled = st.session_state.initial_board[i][j] != 0
        value = "" if board[i][j] == 0 else str(board[i][j])
        input_val = cols[j].text_input(
            label=f"Cell {i},{j}",
            value=value,
            key=f"cell_{i}_{j}",
            max_chars=1,
            disabled=disabled
        )
        if not disabled:
            try:
                iv = int(input_val)
                if 1 <= iv <= 9:
                    new_board[i][j] = iv
                elif input_val == "":
                    new_board[i][j] = 0
            except:
                new_board[i][j] = 0
        else:
            new_board[i][j] = st.session_state.initial_board[i][j]
st.session_state.board = new_board

if st.button("Solve"):
    board_to_solve = new_board.copy()
    if solve(board_to_solve):
        st.session_state.board = board_to_solve
        st.success("Solved!")
    else:
        st.warning("No solution exists or the puzzle is invalid.")
