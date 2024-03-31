```c++
#include <iostream>
#include <vector>
#include <fstream>

using namespace std;

// 순열에서 순환의 개수를 찾는 함수
int countCycles(const vector<int>& permutation) {
    int n = permutation.size();
    vector<bool> visited(n + 1, false); // 방문 여부를 확인하는 벡터
    int count = 0;

    for (int i = 1; i <= n; ++i) {
        if (!visited[i]) {
            int current = i;
            while (!visited[current]) {
                visited[current] = true;
                current = permutation[current - 1];
            }
            ++count; // 순환을 하나 찾았으므로 카운트 증가
        }
    }

    return count;
}

int main() {
    ifstream fin("cycle.inp");
    ofstream fout("cycle.out");

    int T; // 테스트 케이스의 수
    fin >> T;
    while (T--) {
        int N;
        fin >> N; // 순열의 크기
        vector<int> permutation(N);
        for (int i = 0; i < N; ++i) {
            fin >> permutation[i]; // 순열 읽기
        }

        // 순환의 개수를 계산하고 결과 출력
        int count = countCycles(permutation);
        fout << count << endl;
    }

    fin.close();
    fout.close();

    return 0;
}

```
