# 3nplus1
- 유명한 3n+1 문제를 작성해 보았다.
```c++
#include <fstream>
#include <string>
using namespace std;

int main(void) {
	ifstream fin;
	ofstream fout;
	int start, end, n, max;
	string result;
	fin.open("3nplus1.inp");
	fout.open("3nplus1.out");
	while (fin >> start >> end) {
		max = 0;
		result = to_string(start) + " " + to_string(end) + " ";
		if (start > end) {
			int tmp = start;
			start = end;
			end = tmp;
		}
		n = start;
		while (n <= end) {
			int cnt = 0;
			while (true) {
				cnt++;
				if (n == 1) break;
				else if (n % 2 == 1) {
					n = 3 * n + 1;
				}
				else n = n / 2;
			}
			n = ++start;
			if (cnt > max) max = cnt;
		}
		result = result + to_string(max) +"\n";
		fout << result;
	}
	fin.close();
	fout.close();

}
```
