# gitproject-
import numpy as np
import time

# The set of all possible tetrominoes.
TETROMINOES = np.array([
    [0, 0, 1, 0],
    [0, 0, 1, 1],
    [0, 0, 1, 0],
    [0, 0, 1, 0],

    [0, 0, 0, 0],
    [1, 1, 1, 1],
    [0, 0, 0, 0],
    [0, 0, 0, 0],

    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [1, 1, 1, 1],
    [0, 0, 0, 0],

    [0, 1, 0, 0],
    [0, 1, 0, 0],
    [1, 1, 0, 0],
    [0, 0, 0, 0],

    [0, 0, 0, 0],
    [1, 1, 1, 1],
    [0, 0, 0, 0],
    [0, 0, 0, 0],

    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [1, 1, 1, 1],
    [0, 0, 0, 0],

    [0, 1, 1, 0],
    [0, 1, 1, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],

    [0, 0, 0, 0],
    [1, 1, 1, 1],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
])

class Tetris:
    def __init__(self, num_rows=20, num_cols=10):
        self.num_rows = num_rows
        self.num_cols = num_cols
        self.reset()

    def reset(self):
        self.board = np.zeros((self.num_rows, self.num_cols))
        self.tetromino = TETROMINOES[np.random.randint(len(TETROMINOES))]
        self.x = self.num_cols // 2 - len(self.tetromino) // 2
        self.y = self.num_rows - len(self.tetromino)
        self.game_over = False

    def check_collision(self, x, y, tetromino):
        for i in range(len(tetromino)):
            for j in range(len(tetromino[0])):
                if tetromino[i][j]:
                    if (j + x >= self.num_cols or j + x < 0 or i + y >= self.num_rows or i + y < 0 or self.board[i + y][j + x]):
                        return True
        return False

    def check_rows(self):
        for i in range(self.num_rows):
            if not np.any(self.board[i] == 0):
                self.board = np.concatenate((self.board[:i], self.board[i+1:]), axis=0)
                self.board = np.concatenate((self.board, np.zeros((1, self.num_cols))), axis=0)
        return

    def update_board(self, x, y, tetromino):
        self.board[y:y+len(tetromino), x:x+len(tetromino[0])] += tetromino

    def get_board(self):
        return self.board

    def play(self):
        while not self.game_over:
            print(self.get_board())
            print("\n".join([""] * 5))
            self.check_rows()
            self.tetromino = TETROMINOES[np.random.randint(len(TETROMINOES))]
            self.x = self.num_cols // 2 - len(self.tetromino) // 2
            self.y = self.num_rows - len(self.tetromino)
            print("Tetromino:")
            print(self.tetromino)
            if self.check_collision(self.x, self.y, self.tetromino):
                self.game_over = True
            self.update_board(self.x, self.y, self.tetromino)
            input("Press enter to move down: ")
            self.y += 1
            if self.check_collision(self.x, self.y, self.tetromino):
                self.y -= 1

tetris = Tetris()
tetris.play()
