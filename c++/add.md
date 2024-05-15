```c++
#include <iostream>
#include <fstream>
#include <queue>
#include <vector>

using namespace std;

int main() {
    ifstream fin("add.inp");
    ofstream fout("add.out");

    while (true) {
        int n;
        fin >> n;
        if (n == 0) break;
        priority_queue<int, vector<int>, greater<int>> min_heap;
        int num;
        for (int i = 0; i < n; ++i) {
            fin >> num;
            min_heap.push(num);
        }

        long long int total_cost = 0;
        while (min_heap.size() > 1) {
            int first = min_heap.top(); min_heap.pop();
            int second = min_heap.top(); min_heap.pop();
            long long int  cost = first + second;
            total_cost += cost;
            min_heap.push(cost);
        }
        fout << total_cost << endl;
    }

    fin.close();
    fout.close();

    return 0;
}

```
