#include <iostream>
#include <cmath>
#include <locale>
#include <string>
#include <iomanip>

#define N 4

using namespace std;

void plenum(int array[N][N]) {
    int num = 1;
    for (int pi = 0;pi < N;pi++) {
        if (pi % 2 == 0) {
            for (int li = 0; li < N; li++)
                array[li][pi] = num++;
        }
        else {
            for (int li = N - 1; li >= 0; li--) 
                array[li][pi] = num++;
        }
    }
}

void print_array(int array[N][N]) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            if (array[i][j] >= 10)
                cout << array[i][j] << " ";
            else
                cout << array[i][j] << "  ";
        }
        cout << endl;
    }

}

int determinant(int matrix[N][N], int n) {
    int det = 0;

    if (n == 1) {
        return matrix[0][0];
    }

    if (n == 2) {
        return matrix[0][0] * matrix[1][1] - matrix[0][1] * matrix[1][0];
    }

    for (int x = 0; x < n; x++) {
        int minor[N][N];
        int m = 0, nIndex = 0;
        // Заполнение минорной матрицы
        for (int i = 1; i < n; i++) { // Начинаем с 1, чтобы пропустить первую строку
            for (int j = 0; j < n; j++) {
                if (j != x) { // Пропускаем столбец x
                    minor[m][nIndex++] = matrix[i][j];
                    if (nIndex == n - 1) {
                        nIndex = 0;
                        m++;
                    }
                }
            }
        }
        // Рекурсивный вызов для вычисления определителя
        int sign = (x % 2 == 0) ? 1 : -1; // Знак (-1)^x
        det += sign * matrix[0][x] * determinant(minor, n - 1);
    }
    return det;
}


int main() {
    setlocale(LC_CTYPE, "RUS");

    int array[N][N];

    plenum(array);

    print_array(array);

    int matrix[N][N] = {
        {1, 0, 2, -1},
        {3, 0, 0, 5},
        {2, 1, 4, -3},
        {1, 0, 5, 0}
    };

    int det = determinant(matrix, 4);
    cout << "Определитель матрицы: " << det << endl;


    return 0;
}
