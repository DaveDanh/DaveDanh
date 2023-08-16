#include<stdlib.h>
#include<windows.h>
#include<iostream>
#include <conio.h>
#include <cstdlib>

using namespace std;

void printCol(int col){
    for (int i = 0; i<2;i++){
        for (int a = -1; a<col;a++){
            cout << "|     ";
        }
        cout << endl;
    }
}

void printRow(int col){
    for (int i = 0;i<col;i++){
        cout << "|_____";
    }
    cout << "|";
    cout << endl;
}

void printBoard(int col){
    for (int i = 0;i<col;i++){
        cout << " _____";
    }
    cout << endl;
    for (int k = 0;k<col;k++){
        printCol(col);
        printRow(col);

    }
}

void gotoxy(int x, int y){
    static HANDLE h = NULL;  
    if(!h)
        h = GetStdHandle(STD_OUTPUT_HANDLE);
    COORD c = { x, y };  
    SetConsoleCursorPosition(h,c);
}

bool CheckWin(char board[][12], int col, int k, char player){
    //Row
    int count = 0;
    for (int j = 0;j<col;j++){
        for (int i = 0;i<col;i++){
            if (board[i][j] == player){
                count ++;
                if (count == k){
                    return true;
                }
            }
            else{
                count = 0;
            }
        }
        count = 0;
    }

    //Col
    count = 0;
    for (int i = 0;i<col;i++){
        for (int j = 0;j<col;j++){
            if (board[i][j] == player){
                count ++;
                if (count == k){
                    return true;
                }
            }
            else{
                count = 0;
            }
        }
        count = 0;
    }

    //Dia
    for (int i = 0; i < col - (k - 1); i++) {
        for (int j = 0; j < col - (k - 1); j++) {
            int count = 0;
            for (int a = 0; a < k; a++) {
                if (board[i + a][j + a] == player) {
                    count++;
                    if (count == k) {
                        return true;
                    }
                } else {
                    count = 0;
                    break;
                }
            }
        }
    }

    //ReverseDia
    for (int i = 0; i < col - (k - 1); i++) {
        for (int j = col-1; j > col-k-1; j--) {
            int count = 0;
            for (int a = 0; a < k; a++) {
                if (board[i + a][j - a] == player) {
                    count++;
                    if (count == k) {
                        return true;
                    }
                } else {
                    count = 0;
                    break;
                }
            }
        }
    }
    return false;
}


void drawLogo(){
    system("cls");
    gotoxy(0,0);
    system("color 0D");
    cout << "        _________________                 _______                 _________________       _________________" << endl;
    cout << "       |$$$$$$$$$$$$$$$$$|               /%%/\\%%\\\\               |$$$$$$$$$$$$$$$$$|     |$$$$$$$$$$$$$$$$$|" << endl;
    cout << "       |$$ ___________ $$|              /%%/  \\%%\\\\              |$$ @@@@@@@@@@@|$$|     |$$ ___________ $$|"  << endl;
    cout << "       |$$|           |$$|             /%%/    \\%%\\\\             |$$|           |$$|     |$$|           |$$|" << endl;
    cout << "       |$$|                           /%%/      \\%%\\\\            |$$|           |$$|     |$$|           |$$|" << endl;
    cout << "       |$$|                          /%%/________\\%%\\\\           |$$|@@@@@@@@@@@|$$|     |$$|           |$$|" << endl;
    cout << "       |$$|                         /%%/##########\\%%\\\\          |$$|      %%%%          |$$|           |$$|" << endl;
    cout << "       |$$|                        /%%/------------\\%%\\\\         |$$|       %%%%         |$$|           |$$|" << endl;
    cout << "       |$$|            __         /%%/              \\%%\\\\        |$$|        %%%%        |$$|           |$$|" << endl;
    cout << "       |$$|___________|$$|       /%%/                \\%%\\\\       |$$|         %%%%       |$$|___________|$$|" << endl;
    cout << "       |$$$$$$$$$$$$$$$$$|      /%%/                  \\%%\\\\      |$$|          %%%%      |$$$$$$$$$$$$$$$$$|" << endl;
    cout << "       '-----------------'     /%%/                    \\%%\\\\     |$$|           %%%%     '-----------------'" << endl;
    cout << "" << endl;
}


int main(){
    drawLogo();
    int space = 0;
    bool turn = true;
    int col;
    int con;
    int mode;
    cout << "Enter the size of the board: ";
    cin >> col;
    cout << "Condition to win: ";
    cin >> con;
    cout << "Play with who?" << endl;
    cout << "1. Comp" << endl;
    cout << "2. Friend" << endl;
    cin >> mode;
    system("cls");
    system("color 0F");
    printBoard(col);
    int curX = 3;
    int curY = 2;
    int maxY = 3*col-2;
    int maxX = 6*col-3;
    char board[12][12];
    for (int i = 0; i < col; i++) {
        for (int j = 0; j < col; j++) {
            board[i][j] = ' ';
        }
    }
    while (space < col*col){
        gotoxy(0,3*col+2);
        if (turn == true){
            cout << "X turn";
        }
        else{
            cout << "O turn";
        }
        gotoxy(curX,curY);
        char ch = _getch();
        if (ch == 's' && curY<maxY){
            curY = curY + 3;
        }
        else if (ch == 'w' && curY>2){
            curY = curY - 3;
        }
        else if (ch == 'd' && curX<maxX){
            curX = curX + 6;
        }
        else if (ch == 'a' && curX>3){
            curX = curX - 6;
        }
        else if (ch == '\r'){
            int boardX = (curX-3)/6;
            int boardY = (curY-2)/3;
            if (board[boardX][boardY] == ' '){  
                if (turn == true){    
                    cout << 'X';
                    turn = false;
                    board[boardX][boardY] = 'X';
                    if (CheckWin(board, col,con,'X')){
                        gotoxy(0,3*col+2);
                        cout << "X IS THE WINNER";
                        exit(EXIT_SUCCESS);
                    }
                }
                else{
                    if (mode ==  2){    
                        cout << 'O';
                        turn = true;
                        board[boardX][boardY] = 'O';
                        if (CheckWin(board, col,con,'O')){
                            gotoxy(0,3*col+2);
                            cout << "O IS THE WINNER";
                            exit(EXIT_SUCCESS);
                        }
                    }
                    else {
                        while (true){    
                            int CompX = rand() % (col + 1);
                            int CompY = rand() % (col + 1);
                            if (board[CompX][CompY] == ' '){
                                board[CompX][CompY] = 'O';
                                gotoxy(CompX*6+3,CompY*3+2);
                                cout << 'O';
                                turn = true;
                                if (CheckWin(board, col,con,'O')){
                                    gotoxy(0,3*col+2);
                                    cout << "O IS THE WINNER";
                                    exit(EXIT_SUCCESS);
                                }
                            break;
                            }
                        }
                    }
                }
                space++;
            }
        }
    }
    
    gotoxy(0,3*col+2);
    cout << "A DRAW GAME";
}
