# maze-solver
As the name suggests its a game to solve the given maze
To use this program:

Run the program.
Enter the maze dimensions (rows and columns).
Input the maze, using '#' for walls and ' ' (space) for paths.
Enter the start position (row and column).
Enter the end position (row and column).
Watch as the program solves the maze!


#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

#define MAX_SIZE 20

char maze[MAX_SIZE][MAX_SIZE];
int rows, cols;
int startX, startY, endX, endY;

void clearScreen() {
    #ifdef _WIN32
        system("cls");
    #else
        system("clear");
    #endif
}

void printMaze() {
    clearScreen();
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            printf("%c ", maze[i][j]);
        }
        printf("\n");
    }
    usleep(100000); // Delay for visualization
}

int isValid(int x, int y) {
    return (x >= 0 && x < rows && y >= 0 && y < cols && maze[x][y] == ' ');
}

int solveMaze(int x, int y) {
    if (x == endX && y == endY) {
        maze[x][y] = 'E';
        printMaze();
        return 1;
    }

    if (isValid(x, y)) {
        maze[x][y] = '*';
        printMaze();

        int dx[] = {1, 0, -1, 0};
        int dy[] = {0, 1, 0, -1};

        for (int i = 0; i < 4; i++) {
            if (solveMaze(x + dx[i], y + dy[i])) {
                return 1;
            }
        }

        maze[x][y] = ' ';
        printMaze();
    }

    return 0;
}

int main() {
    printf("Enter maze dimensions (rows cols, max %d): ", MAX_SIZE);
    scanf("%d %d", &rows, &cols);

    if (rows > MAX_SIZE || cols > MAX_SIZE || rows < 1 || cols < 1) {
        printf("Invalid maze size.\n");
        return 1;
    }

    printf("Enter the maze (%dx%d):\n", rows, cols);
    printf("Use '#' for walls and ' ' for paths\n");
    getchar(); // Consume newline
    for (int i = 0; i < rows; i++) {
        fgets(maze[i], MAX_SIZE, stdin);
        for (int j = 0; j < cols; j++) {
            if (maze[i][j] == '\n') maze[i][j] = ' ';
        }
    }

    printf("Enter start position (row col): ");
    scanf("%d %d", &startX, &startY);
    printf("Enter end position (row col): ");
    scanf("%d %d", &endX, &endY);

    if (startX < 0 || startX >= rows || startY < 0 || startY >= cols ||
        endX < 0 || endX >= rows || endY < 0 || endY >= cols ||
        maze[startX][startY] == '#' || maze[endX][endY] == '#') {
        printf("Invalid start or end position.\n");
        return 1;
    }

    maze[startX][startY] = 'S';
    maze[endX][endY] = 'E';

    printf("Solving maze...\n");
    if (solveMaze(startX, startY)) {
        printf("\nMaze solved!\n");
    } else {
        printf("\nNo solution exists.\n");
    }

    return 0;
}

example input
Enter maze dimensions (rows cols, max 20): 5 5
Enter the maze (5x5):
Use '#' for walls and ' ' for paths
Enter start position (row col): 1 1
Enter end position (row col): 3 3
