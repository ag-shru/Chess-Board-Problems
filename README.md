# Chess-Board-Problems
// Solving N- Queens and N-Rooks Problem
#include <iostream>
#include <vector>

using namespace std;

class NQueensNRooks {
public:
    // Constructor initializes the board and sets the number of solutions to zero
    NQueensNRooks(int n) : N(n), board(n, vector<char>(n, '.')), solutionsQueens(0), solutionsRooks(0) {}

    // Public method to start solving the N-Queens problem
    void solveQueens() {
        solveNQueens(0);  // Start solving from the first row for queens
        cout << "Number of solutions for " << N << " queens: " << solutionsQueens << endl;
    }

    // Public method to start solving the N-Rooks problem
    void solveRooks() {
        solveNRooks(0);  // Start solving from the first row for rooks
        cout << "Number of solutions for " << N << " rooks: " << solutionsRooks << endl;
    }

private:
    int N;  // Size of the board (N x N)
    int solutionsQueens;  // Counter for the number of solutions for queens
    int solutionsRooks;  // Counter for the number of solutions for rooks
    vector<vector<char>> board;  // 2D vector representing the chessboard

    // Method to check if it's safe to place a queen at board[row][col]
    bool isSafeQueen(int row, int col) {
        // Check the current column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q')
                return false;
        }

        // Check upper left diagonal
        for (int i = row, j = col; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q')
                return false;
        }

        // Check upper right diagonal
        for (int i = row, j = col; i >= 0 && j < N; i--, j++) {
            if (board[i][j] == 'Q')
                return false;
        }

        return true;  // It's safe to place the queen
    }

    // Method to check if it's safe to place a rook at board[row][col]
    bool isSafeRook(int row, int col) {
        // Check the current column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'R')
                return false;
        }

        return true;  // It's safe to place the rook
    }

    // Recursive method to solve the N-Queens problem
    void solveNQueens(int row) {
        if (row == N) {  // All queens are placed successfully
            solutionsQueens++;
            printSolution("Q");
            return;
        }

        // Try placing the queen in each column of the current row
        for (int col = 0; col < N; col++) {
            if (isSafeQueen(row, col)) {
                board[row][col] = 'Q';  // Place the queen
                solveNQueens(row + 1);  // Recur to place the rest of the queens
                board[row][col] = '.';  // Backtrack and remove the queen
            }
        }
    }

    // Recursive method to solve the N-Rooks problem
    void solveNRooks(int row) {
        if (row == N) {  // All rooks are placed successfully
            solutionsRooks++;
            printSolution("R");
            return;
        }

        // Try placing the rook in each column of the current row
        for (int col = 0; col < N; col++) {
            if (isSafeRook(row, col)) {
                board[row][col] = 'R';  // Place the rook
                solveNRooks(row + 1);  // Recur to place the rest of the rooks
                board[row][col] = '.';  // Backtrack and remove the rook
            }
        }
    }

    // Method to print the current solution in a bordered grid format
    void printSolution(const string& piece) {
        if (piece == "Q") {
            cout << "Solution for Queens #" << solutionsQueens << ":" << endl;
        } else {
            cout << "Solution for Rooks #" << solutionsRooks << ":" << endl;
        }
        // Print top border
        for (int i = 0; i < N; ++i) {
            cout << "+---";
        }
        cout << "+" << endl;

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                cout << "| " << board[i][j] << " ";
            }
            cout << "|" << endl;

            // Print row border
            for (int j = 0; j < N; ++j) {
                cout << "+---";
            }
            cout << "+" << endl;
        }
        cout << endl;
    }
};

int main() {
    int n;
    cout << "Enter the value of N: ";
    cin >> n;

    NQueensNRooks solver(n);  // Create an instance of NQueensNRooks with the given size
    cout << "Solving N-Queens problem..." << endl;
    solver.solveQueens();  // Solve the N-Queens problem

    cout << "Solving N-Rooks problem..." << endl;
    solver.solveRooks();  // Solve the N-Rooks problem

    return 0;
}
