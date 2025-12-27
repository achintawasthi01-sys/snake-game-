# snake-game-
this is my first project in c
#include <stdio.h>
#include <conio.h>
#include <windows.h>
#include <stdlib.h>

/* Game field size */
#define WIDTH 40
#define HEIGHT 20

/* Direction enum */
enum Direction { STOP = 0, LEFT, RIGHT, UP, DOWN };
enum Direction dir;

/* Global variables */
int gameOver;
int x, y;          // Snake head position
int foodX, foodY;  // Food position
int score;

/* Snake tail */
int tailX[100], tailY[100];
int nTail;

/* Function declarations */
void Setup();
void Draw();
void Input();
void Logic();

/* Game setup */
void Setup() {
    gameOver = 0;
    dir = STOP;
    x = WIDTH / 2;
    y = HEIGHT / 2;
    foodX = rand() % (WIDTH - 2) + 1;
    foodY = rand() % (HEIGHT - 2) + 1;
    score = 0;
}

/* Draw the game */
void Draw() {
    system("cls");

  // Top wall
    for (int i = 0; i < WIDTH + 2; i++)
        printf("#");
    printf("\n");
   // Side walls & content
    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
          if (j == 0)
                printf("#");
      if (i == y && j == x)
                printf("O");          // Snake head
            else if (i == foodY && j == foodX)
                printf("F");          // Food
            else {
                int printTail = 0;
                for (int k = 0; k < nTail; k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        printf("o");  // Snake tail
                        printTail = 1;
                        break;
                    }
            if (!printTail)
                    printf(" ");     }     if (j == WIDTH - 1)
          printf("#");
      }
  printf("\n");
    }

   // Bottom wall
    for (int i = 0; i < WIDTH + 2; i++)
        printf("#");
    printf("\nScore : %d\n", score);
    printf("Controls: W A S D | X to exit\n");
}

/* Keyboard input */
void Input() {
    if (_kbhit()) {
        switch (_getch()) {
            case 'a': dir = LEFT;  break;
            case 'd': dir = RIGHT; break;
            case 'w': dir = UP;    break;
            case 's': dir = DOWN;  break;
            case 'x': gameOver = 1; break;
            default: break;
        }
    }
}

/* Game logic */
void Logic() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;

   for (int i = 1; i < nTail; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }
   switch (dir) {
        case LEFT:  x--; break;
        case RIGHT: x++; break;
        case UP:    y--; break;
        case DOWN:  y++; break;
        default: break;
    }

 Wall collision
    if (x < 0 || x >= WIDTH || y < 0 || y >= HEIGHT)
        gameOver = 1;

// Self collision
    for (int i = 0; i < nTail; i++) {
        if (tailX[i] == x && tailY[i] == y)
            gameOver = 1;
    }

 // Food collision
    if (x == foodX && y == foodY) {
        score += 10;
        foodX = rand() % (WIDTH - 2) + 1;
        foodY = rand() % (HEIGHT - 2) + 1;
        nTail++;
    }
}

/* Main function */
int main() {
    Setup();
    while (!gameOver) {
        Draw();
        Input();
        Logic();
        Sleep(100);
    }

 system("cls");
    printf("GAME OVER!\n");
    printf("Final Score: %d\n", score);
    return 0;
}

