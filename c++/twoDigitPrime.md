## 문제 : 두 자리 소수

### 문제 설명 
특정 숫자에서 2개의 숫자를 뽑은 다음 만든 숫자가 소수일 경우가 하나라도 존재할 경우 이를 2자리 소수라고 부른다. 예를 들어, 153의 경우 1과 3을 뽑아 13을 만들면 13이 소수가 되기 때문에 2자리 소수이다. 자연수의 특정 구간을 나타내는 자연수 와 가 입력되었을 경우, 부터  사이에 있는 수(,  포함) 중 2자리 소수의 개수를 찾는 프로그램을 작성하라.
### 【입 력】
입력파일의 이름은 twoDigitPrime.inp이다. 첫째 줄에는 검사하고자 하는 총 경우의 수 가 주어진다. 이어지는  줄 각각엔 두 정수 , 가 하나의 공백으로 구분되어 주어진다. 

### 【출 력】
출력 파일의 이름은 twoDigitPrime.out이다. 검사하는 각 경우에 대해 부터  사이에 있는 수(,  포함) 중 2자리 소수의 개수를 출력하라.


제한조건: 프로그램은 twoDigitPrime.{c,cpp,java}로 한다.

```c++
#include <iostream>
#include <fstream>
#include <vector>
#include <algorithm>
#include<string>
using namespace std;

bool isPrime(int num) {
    if (num % 2 == 0) return num == 2;
    for (int div = 3; div * div <= num; div += 2) {
        if (num % div == 0) return false;
    }
    return true;

}

vector<int> primes;



// 주어진 숫자에서 2자리 소수 개수를 찾는 함수
int countTwoDigitPrimes(int num, const vector<int>& primes) {
    string s = to_string(num);
    vector<int> formedNumbers;

    for (int i = 0; i < s.length(); i++) {
        for (int j = 0; j < s.length(); j++) {
            if (i != j) {
                formedNumbers.push_back((s[i] - '0') * 10 + (s[j] - '0'));
            }
        }
    }

    // 중복 제거
    sort(formedNumbers.begin(), formedNumbers.end());
    formedNumbers.erase(unique(formedNumbers.begin(), formedNumbers.end()), formedNumbers.end());

    // 소수 카운트
    int count = 0;
    for (int num : formedNumbers) {
        if (find(primes.begin(), primes.end(), num) != primes.end()) {
            count++;
        }
    }

    return count;
}

int main() {
    ifstream fin("twoDigitPrime.inp");
    ofstream fout("twoDigitPrime.out");

    int T, A, B;
    fin >> T;

    for(int i = 10; i < 100; i++) {
        if (isPrime(i)) {
            primes.push_back(i);
        }
    }

    while (T--) {
        fin >> A >> B;
        int count = 0;
        for (int i = A; i <= B; i++) {
            if (countTwoDigitPrimes(i, primes) > 0) {
                count++;
            }
        }
        fout << count << endl;
    }

    fin.close();
    fout.close();
    return 0;
}

```
