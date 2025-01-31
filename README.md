# Task2

**Implement an AI agent that plays the classic game of Tic-Tac-Toe
against a human player. You can use algorithms like Minimax with
or without Alpha-Beta Pruning to make the AI player unbeatable.**


import java.util.Scanner;
public class TicTacToe {
    //board setup for game representation
    static final char HUMAN = 'X';
    static final char AI = 'O';
    static final char EMPTY = ' ';
    static char[][] board = {
        {EMPTY, EMPTY, EMPTY},
        {EMPTY, EMPTY, EMPTY},
        {EMPTY, EMPTY, EMPTY}
    };

    //main game loop
    public static void main(String[] args) {
        System.out.println("Tic-Tac-Toe: You are 'X', AI is 'O'.");
        printBoard();
        while (true) {
            humanMove();
            printBoard();
            if (checkWinner(board) != EMPTY || isFull(board)) {
                break;
            }

            aiMove();
            printBoard();
            if (checkWinner(board) != EMPTY || isFull(board)) {
                break;
            }
        }
        char winner = checkWinner(board);
        if (winner == HUMAN) {
            System.out.println("Congratulations! You win!");
        } else if (winner == AI) {
            System.out.println("AI wins! Better luck next time.");
        } else {
            System.out.println("It's a draw!");
        }
    }

    // Prints the Tic-Tac-Toe Board
    public static void printBoard() {
        for (int i = 0; i < 3; i++) {
            System.out.println(" " + board[i][0] + " | " + board[i][1] + " | " + board[i][2]);
            if (i < 2) {
                System.out.println("---|---|---");
            }
        }
        System.out.println();
    }

    // Checks if the board is full (Draw Condition)
    public static boolean isFull(char[][] board) {
        for (char[] row : board) {
            for (char cell : row) {
                if (cell == EMPTY) {
                    return false;
                }
            }
        }
        return true;
    }

    // Checks if there is a winner
    public static char checkWinner(char[][] board) {
        for (int i = 0; i < 3; i++) {
            if (board[i][0] == board[i][1] && board[i][1] == board[i][2] && board[i][0] != EMPTY) {
                return board[i][0]; // Row win
            }
            if (board[0][i] == board[1][i] && board[1][i] == board[2][i] && board[0][i] != EMPTY) {
                return board[0][i]; // Column win
            }
        }
        if (board[0][0] == board[1][1] && board[1][1] == board[2][2] && board[0][0] != EMPTY) {
            return board[0][0]; // Diagonal win
        }
        if (board[0][2] == board[1][1] && board[1][1] == board[2][0] && board[0][2] != EMPTY) {
            return board[0][2]; // Other diagonal win
        }
        return EMPTY; // No winner yet
    }

    // Allows the human player to make a move
    public static void humanMove() {
        Scanner scanner = new Scanner(System.in);
        int row, col;
        while (true) {
            System.out.print("Enter your move (row [0-2] and column [0-2]): ");
            row = scanner.nextInt();
            col = scanner.nextInt();

            if (row >= 0 && row < 3 && col >= 0 && col < 3 && board[row][col] == EMPTY) {
                board[row][col] = HUMAN;
                break;
            } else {
                System.out.println("Invalid move! Try again.");
            }
        }
    }

    // AI Makes the Best Move using Minimax
    public static void aiMove() {
        int bestScore = Integer.MIN_VALUE;
        int bestRow = -1, bestCol = -1;

        for (int row = 0; row < 3; row++) {
            for (int col = 0; col < 3; col++) {
                if (board[row][col] == EMPTY) {
                    board[row][col] = AI;
                    int score = minimax(board, 0, false);
                    board[row][col] = EMPTY;

                    if (score > bestScore) {
                        bestScore = score;
                        bestRow = row;
                        bestCol = col;
                    }
                }
            }
        }
        board[bestRow][bestCol] = AI;
        System.out.println("AI has made its move!");
    }

    // Minimax Algorithm (without Alpha-Beta Pruning)
    public static int minimax(char[][] board, int depth, boolean isMaximizing) {
        char winner = checkWinner(board);
        if (winner == AI) return 10 - depth;
        if (winner == HUMAN) return depth - 10;
        if (isFull(board)) return 0;

        if (isMaximizing) {
            int bestScore = Integer.MIN_VALUE;
            for (int row = 0; row < 3; row++) {
                for (int col = 0; col < 3; col++) {
                    if (board[row][col] == EMPTY) {
                        board[row][col] = AI;
                        int score = minimax(board, depth + 1, false);
                        board[row][col] = EMPTY;
                        bestScore = Math.max(bestScore, score);
                    }
                }
            }
            return bestScore;
        } else {
            int bestScore = Integer.MAX_VALUE;
            for (int row = 0; row < 3; row++) {
                for (int col = 0; col < 3; col++) {
                    if (board[row][col] == EMPTY) {
                        board[row][col] = HUMAN;
                        int score = minimax(board, depth + 1, true);
                        board[row][col] = EMPTY;
                        bestScore = Math.min(bestScore, score);
                    }
                }
            }
            return bestScore;
        }
    }
}
//Everytime AI is unbeatable because Minimax finds the best move every time.
