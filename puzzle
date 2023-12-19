#include <iostream>
#include <string>
#include <algorithm>
#include <Windows.h>
#include <stdlib.h>
#include <time.h>
#include <conio.h>
#include <vector>

#define r_key 114
#define R_KEY 82
#define g_key 103
#define G_KEY 71
#define s_key 115
#define S_KEY 83
#define p_key 112
#define P_KEY 80
#define exit_key 48
#define debug_key 49

using namespace std;

HANDLE hConsole = GetStdHandle(STD_OUTPUT_HANDLE);

struct piece {
    int up, right, down, left;
    bool pixel;
};

bool** image;
piece** image_sliced;
int MaxPossibleValue = 5;
int puzzleSize = 1;
bool IsGenerated = false;

void generate()
{
    system("CLS");
    IsGenerated = true;
    printf("Enter puzzle size.\n");
    cin >> puzzleSize;
    system("CLS");
    if (puzzleSize < 2)
    {
        cout << "Can't generate.\n";
        exit(0);
    }

    printf("Please enter a number to make a puzzle sort difficulty.\n\nHard        Easy\n1            INF\n<-------------->\n");
    cin >> MaxPossibleValue;

    if (cin.fail())
    {
        cout << "Bad Value\n";
        return;
    }

    image = new bool* [puzzleSize];
    image_sliced = new piece * [puzzleSize];

    for (int i = 0; i < puzzleSize; i++)
    {
        image_sliced[i] = new piece[puzzleSize];
    }
    for (int i = 0; i < puzzleSize; i++)
    {
        image[i] = new bool[puzzleSize];

        for (int j = 0; j < puzzleSize; j++)
        {
            image[i][j] = rand() % 2;
            if (image_sliced[i][j].up >= 0 || image_sliced[i][j].up == -842150451) {
                image_sliced[i][j].up = (rand() % MaxPossibleValue) + 1;
            }

            image_sliced[i][j].right = (rand() % MaxPossibleValue) + 1;
            if (image_sliced[i][j].left >= 0 || image_sliced[i][j].left == -842150451)
            {
                image_sliced[i][j].left = (rand() % MaxPossibleValue) + 1;
            }
            image_sliced[i][j].down = (rand() % MaxPossibleValue) + 1;
            image_sliced[i][j].pixel = image[i][j];

            if (i < puzzleSize - 1) {
                image_sliced[i + 1][j].up = -image_sliced[i][j].down;
            }
            if (j < puzzleSize - 1)
            {
                image_sliced[i][j + 1].left = -image_sliced[i][j].right;
            }
        }
    }
    system("CLS");
}
void randomize()
{
    system("CLS");
    if (IsGenerated == false)
    {
        cout << "Image is not generated.\n\n";
        return;
    }
    time_t currentTime{};
    time(&currentTime);
    struct tm* myTime = localtime(&currentTime);

    piece buffer;
    for (int i = 0; i < puzzleSize; i++)
    {
        for (int j = 0; j < puzzleSize; j++)
        {
            int randomNumber = rand() % 100 + puzzleSize;
            int x = ((myTime->tm_sec) + randomNumber) % puzzleSize;
            int y = ((myTime->tm_sec) + randomNumber + 16) % puzzleSize;
            buffer = image_sliced[i][j];
            image_sliced[i][j] = image_sliced[x][y];
            image_sliced[x][y] = buffer;
        }
    }
    for (int i = 0; i < puzzleSize; i++)
    {
        for (int j = 0; j < puzzleSize; j++)
        {
            // cout << (image_sliced[i][j].pixel ? 1 : 0) << " ";
        }
        //cout << endl;
    }
    // cout << endl;
    system("CLS");
}
void solve()
{
    system("CLS");
    piece buffer;
    if (IsGenerated == false)
    {
        cout << "Image is not generated.\n\n";
        return;
    }
    for (int i = 0; i < puzzleSize; i++) // упростить работу, т.к. первый пазл всегда имеет числа >0
    {
        for (int j = 0; j < puzzleSize; j++)
        {
            if (image_sliced[i][j].up > 0 && image_sliced[i][j].down > 0 && image_sliced[i][j].left > 0 && image_sliced[i][j].right > 0)
            {
                buffer = image_sliced[0][0];
                image_sliced[0][0] = image_sliced[i][j];
                image_sliced[i][j] = buffer;
            }
        }
    }

    int repeater = 0;
    bool cantprint = true;
    bool cantquit = false;
    for (int j = 0; j < puzzleSize; j++)
    {
        for (int z = 0; z < puzzleSize; z++)
        {
            for (int a = 0; a < puzzleSize; a++)
            {
                if (image_sliced[z][a].left > 0 && image_sliced[z][a].up > 0)
                {
                    buffer = image_sliced[0][0];
                    image_sliced[0][0] = image_sliced[z][a];
                    image_sliced[z][a] = buffer;
                }

                if (j > 0 && image_sliced[z][a].up > 0 && image_sliced[0][j - 1].right == -(image_sliced[z][a].left))
                {
                    buffer = image_sliced[0][j];
                    image_sliced[0][j] = image_sliced[z][a];
                    image_sliced[z][a] = buffer;
                }
            }
        }
    }
    for (int i = 1; i < puzzleSize; i++)
    {
        for (int z = 1; z < puzzleSize; z++)
        {
            for (int a = 0; a < puzzleSize; a++)
            {
                if(image_sliced[z][a].left > 0 && image_sliced[i - 1][0].down == -(image_sliced[z][a].up))
                {
                    buffer = image_sliced[i][0];
                    image_sliced[i][0] = image_sliced[z][a];
                    image_sliced[z][a] = buffer;
                }
            }
        }
    }
   
    while (1)
    {
        for (int i = 1; i < puzzleSize; i++)
        {
            for (int j = 1; j < puzzleSize; j++)
            {
                if (i == 0)
                {
                    cout << (image_sliced[i][j].pixel ? SetConsoleTextAttribute(hConsole, 15), (char)254u : SetConsoleTextAttribute(hConsole, 10), (char)254u) << " ";
                    continue;
                }
                
                for (int z = 1; z < puzzleSize; z++)
                {
                    for (int a = 1; a < puzzleSize; a++)
                    {
                        if (a == j && z == i)
                        {
                            continue;
                        }
                       
                        if ((image_sliced[i][j].pixel != image[i][j] || image_sliced[i][j].up != -(image_sliced[i - 1][j].down) || image_sliced[i][j].left != -(image_sliced[i][j - 1].right)) &&
                            image_sliced[z][a].up == -(image_sliced[i - 1][j].down) && image_sliced[z][a].left == -(image_sliced[i][j - 1].right))
                        {
                            buffer = image_sliced[i][j];
                            image_sliced[i][j] = image_sliced[z][a];
                            image_sliced[z][a] = buffer;
                        }
                        
                    }
                }
            }
        }
        for (int i = 0; i < puzzleSize; i++)
        {
            for (int j = 0; j < puzzleSize; j++)
            {
                cout << (image_sliced[i][j].pixel ? SetConsoleTextAttribute(hConsole, 15), (char)254u : SetConsoleTextAttribute(hConsole, 10), (char)254u) << " ";
            }
            cout << endl;
        }
        cout << endl << endl << "SOLVED VER" << endl << endl;
        for (int i = 0; i < puzzleSize; i++)
        {
            for (int j = 0; j < puzzleSize; j++)
            {
                cout << (image[i][j] ? SetConsoleTextAttribute(hConsole, 15), (char)254u : SetConsoleTextAttribute(hConsole, 10), (char)254u) << " ";
                if (image_sliced[i][j].pixel != image[i][j])
                {
                    cantquit = true;
                }
            }
            cout << endl;
        }
        if (cantquit == false)
        {
            break;
        }
        cantquit = false;
        system("CLS");
    }

    system("CLS");
    for (int i = 0; i < puzzleSize; i++)
    {
        for (int j = 0; j < puzzleSize; j++)
        {
            cout << (image_sliced[i][j].pixel ? SetConsoleTextAttribute(hConsole, 15), (char)254u : SetConsoleTextAttribute(hConsole, 8), (char)254u) << " ";
        }
        cout << endl;
    }
    cout << "\n\nSolved. Press enter to go to menu.\n";
    cout << endl << endl << endl << endl;
    for (int i = 0; i < puzzleSize; i++)
    {
        for (int j = 0; j < puzzleSize; j++)
        {
            cout << (image[i][j] ? SetConsoleTextAttribute(hConsole, 15), (char)254u : SetConsoleTextAttribute(hConsole, 8), (char)254u) << " ";
        }
        cout << endl;
    }
    cin.get();
    system("CLS");
}

void printallpositives()
{
    int irr = 0;
    for (int i = 0; i < puzzleSize; i++)
    {
        for (int j = 0; j < puzzleSize; j++)
        {
            if (image_sliced[i][j].left > 0)
            {
                cout << endl << irr << "\nLEFT: " << image_sliced[i][j].left;
            }
            irr++;
        }
    }
}
void printpuzzle()
{
    system("CLS");
    if (IsGenerated == false)
    {
        cout << "Image is not generated.\n\n";
        return;
    }
    cout << "Normal Image\n";
    for (int i = 0; i < puzzleSize; i++)
    {
        for (int j = 0; j < puzzleSize; j++)
        {
            cout << (image[i][j] ? SetConsoleTextAttribute(hConsole, 15), (char)254u : SetConsoleTextAttribute(hConsole, 8), (char)254u) << " ";
        }
        cout << endl;
    }
    cout << "Puzzle\n";
    for (int i = 0; i < puzzleSize; i++)
    {
        for (int j = 0; j < puzzleSize; j++)
        {
            cout << (image_sliced[i][j].pixel ? SetConsoleTextAttribute(hConsole, 15), (char)254u : SetConsoleTextAttribute(hConsole, 8), (char)254u) << " ";
        }
        cout << endl;
    }
    SetConsoleTextAttribute(hConsole, 15);
    printf("Press enter to go back to menu.\n");
    cin.get();
    system("CLS");
}

int main()
{
    int c = 0;
    while (1)
    {

        SetConsoleTextAttribute(hConsole, 15);
        printf("Welcome to puzzle solver.\n\n");
        printf("Press 'g' to generate image.\nPress 'r' to transfer image into puzzle.\nPress 's' to solve.\nPress 'p' to print image.\nPress 0 to exit.\n\n");

        switch ((c = _getch()))
        {
        case exit_key:
            exit(0);
        case g_key:
        case G_KEY:
            generate();
            break;
        case r_key:
        case R_KEY:
            randomize();
            break;
        case s_key:
        case S_KEY:
            solve();
            break;
        case p_key:
        case P_KEY:
            printpuzzle();
            break;
        case debug_key:
            printallpositives();
            break;
        default:
            system("CLS");
            break;
        }
    }
}
