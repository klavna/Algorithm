```c++
#include <iostream>
#include<fstream>
#include <vector>
#include <cmath>
#include <set>
using namespace std;

int main() {
    ifstream fin("jolly.inp");
    ofstream fout("jolly.out");
    std::vector<int> jolly;
    int n;
    while (fin>>n) {
        std::vector<int> jolly(n);
        for (int i = 0; i < n; ++i) {
            fin >> jolly[i]; 
        }
        set<int> differences;
        for (int i = 1; i < n; ++i) {
            int diff = abs(jolly[i] - jolly[i - 1]);
            differences.insert(diff);
        }
        bool check = true;
        for (int i = 1; i < n; i++) {
            if (differences.find(i) == differences.end()) {
                check = false;
                break;
            }
        }
        if (check) fout << "Jolly" << endl;
        else  fout << "Not Jolly" << endl;
    }

  

    return 0;
}
```
