```c++
#include <iostream>
#include<fstream>
using namespace std;
int grid[1001][1001] = { 0 };

int main(void) {
	ifstream fin("grid.inp");
	ofstream fout("grid.out");
	int T;
	int x, y, a, b, k;
	fin >> T;
	while (T--) {
		fin >> x >> y >> a >> b >> k;
		int count[1001][1001][11] = { 0 };
		for (int i = 0; i < a; i++) {
			int tmpx, tmpy;
			fin >> tmpx >> tmpy;
			grid[tmpx][tmpy] = 1;
		}
		for (int i = 0; i < b; i++) {
			int tmpx, tmpy;
			fin >> tmpx >> tmpy;
			grid[tmpx][tmpy] = -1;
		}
		for (int i = 1; i <= y; i++) {
			for (int j = 1; j <= x; j++) {
				if (grid[j][i] == 0) {
					for (int t = 0; t < k; t++) {
						if (t == 0) count[j][i][t] = count[j - 1][i][t] + count[j][i - 1][t] + 1;
						else count[j][i][t] = count[j - 1][i][t] + count[j][i - 1][t];
					}
				}
				else if (grid[j][i] == 1) {
					for (int t = 0; t < k; t++) {
						if (t == 0) count[j][i][t] = 0;
						else count[j][i][t+1] = count[j - 1][i][t] + count[j][i - 1][t] + 1;
					}
				} 
			}
		}
		for (int t = 0; t < k; t++) {
			cout << count[x][y][t]%1000000007 << endl;
		}
	}

	return 0;
}
```
