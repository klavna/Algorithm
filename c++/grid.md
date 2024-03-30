```c++
#include <iostream>
#include<fstream>

using namespace std;
long long int m = 1000000007;

int main(void) {
	ifstream fin("grid.inp");
	ofstream fout("grid.out");
	int T;
	int x, y, a, b, k;
	fin >> T;
	while (T--) {
		fin >> x >> y >> a >> b >> k;
		long long int*** count = new long long int** [x + 1];
		int** grid = new int* [x + 1];
		for (int i = 0; i <= x; ++i) {
			count[i] = new long long int* [y + 1];
			grid[i] = new int[y + 1];

			for (int j = 0; j <= y; ++j) {
				count[i][j] = new long long int[k+1];
				
				for (int z = 0; z <= k; ++z) {
					count[i][j][z] = 0;
				}
				grid[i][j] = 0;
			}
		}
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

		for (int i = 0; i <= y; i++) {
			for (int j = 0; j <= x; j++) {
				if (k == 0) {
					if (grid[j][i] != -1) {
						if (i == 0 && j == 0) {
							count[0][0][0] = 1;
							continue;
						}
						
						if (i == 0)  count[j][i][0] = count[j - 1][i][0];
						else if (j == 0) count[j][i][0] = count[j][i - 1][0];
						else count[j][i][0] = (count[j - 1][i][0] + count[j][i - 1][0]) % m;

					}
				}
				else {
					if (grid[j][i] == 0) {
						if (i == 0 && j == 0) {
							count[0][0][0] = 1;
							continue;
						}
						for (int t = 0; t <= k; t++) {
							if (i == 0)  count[j][i][t] = count[j - 1][i][t];
							else if (j == 0) count[j][i][t] = count[j][i - 1][t];
							else count[j][i][t] = (count[j - 1][i][t] + count[j][i - 1][t]) % m;
						}
					}
					else if (grid[j][i] == 1) {
						for (int t = 0; t <= k; t++) {
							if (i == 0) {
								if (t == 0) count[j][i][t] = 0;
								else if (t == k) count[j][i][t] = (count[j - 1][i][t - 1] + count[j - 1][i][t]) % m;
								else count[j][i][t] = count[j - 1][i][t - 1];
							}
							else if (j == 0) {
								if (t == 0) count[j][i][t] = 0;
								else if (t == k) count[j][i][t] = (count[j][i - 1][t - 1] + count[j][i - 1][t]) % m;
								else count[j][i][t] = count[j][i - 1][t - 1];
							}
							else {
								if (t == 0) count[j][i][t] = 0;
								else if (t == k) count[j][i][t] = ((count[j - 1][i][t - 1] + count[j][i - 1][t - 1]) % m + (count[j - 1][i][t] + count[j][i - 1][t]) % m) % m;
								else count[j][i][t] = (count[j - 1][i][t - 1] + count[j][i - 1][t - 1]) % m;
							}
						}
					}
				}
			}
		}
		
		fout << count[x][y][k] << endl;
		
		for (int i = 0; i <= x; ++i) {
			for (int j = 0; j <= y; ++j) {
				delete[] count[i][j];
			}
			delete[] count[i];
		}
		delete[] count;
	}

	return 0;
}
```
