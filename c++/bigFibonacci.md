문제 : Big Fibonacci Numbers

문제 설명 :
피보나찌 수는 다음과 같이 정의된다.

아주 큰 에 대해 을 구하는 프로그램을 작성하고자 한다.

【입 력】
입력파일의 이름은 bigFibonacci.inp 이다. 첫째 줄에는 검사해야 할 테스트의 총 개수 가 있으며, 이어지는  줄 각각에 이 주어진다.

【출 력】
출력 파일의 이름은 bigFibonacci.out이다. 각 입력 에 대해, 과 을 한 줄에 출력하라.
```c++
#include <iostream>
#include <vector>
#include <fstream>

using namespace std;

const int MOD = 1000000007;

// 2x2 행렬을 나타내는 구조체
struct Matrix {
    long long mat[2][2];
};

// 두 2x2 행렬을 곱하는 함수
Matrix matrixMultiply(const Matrix& a, const Matrix& b, int mod) {
    Matrix result;
    for (int i = 0; i < 2; ++i) {
        for (int j = 0; j < 2; ++j) {
            result.mat[i][j] = 0;
            for (int k = 0; k < 2; ++k) {
                result.mat[i][j] = (result.mat[i][j] + a.mat[i][k] * b.mat[k][j]) % mod;
            }
        }
    }
    return result;
}

// 2x2 단위 행렬을 반환하는 함수
Matrix identityMatrix() {
    Matrix result = { {{1, 0}, {0, 1}} };
    return result;
}

// 행렬을 거듭제곱하는 함수
Matrix matrixPower(Matrix base, long long exp, int mod) {
    Matrix result = identityMatrix();
    while (exp > 0) {
        if (exp % 2 == 1) {
            result = matrixMultiply(result, base, mod);
        }
        base = matrixMultiply(base, base, mod);
        exp /= 2;
    }
    return result;
}

// 피보나치 수를 계산하는 함수
int fibonacciModulo(long long n, int mod) {
    if (n == 0) return 0;
    if (n == 1) return 1;

    Matrix base = { {{1, 1}, {1, 0}} };
    Matrix result = matrixPower(base, n - 1, mod);
    return result.mat[0][0];
}

int main() {
    ifstream fin("bigFibonacci.inp");
    ofstream fout("bigFibonacci.out");


    int T;
    fin >> T;
    vector<long long> numbers(T);

    for (int i = 0; i < T; i++) {
        fin >> numbers[i];
    }

    for (int i = 0; i < T; i++) {
        int result = fibonacciModulo(numbers[i], MOD);
        fout << numbers[i] << " " << result << "\n";
        
    }

    fin.close();
    fout.close();

    return 0;
}
```
