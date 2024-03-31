```c++
#include <iostream>
#include<fstream>
using namespace std;

int main(void) {
	ifstream fin("dish.inp");
	ofstream fout("dish.out");
	char dish,tmp;
	int M;
	int T;
	fin >> T;
	while (T--) {
		int result = 0;
		fin >> M;
		fin >> tmp;
		result = 10;
		while (M) {
			fin >> dish;
			if (tmp == dish) result += 5;
			else result += 10;
			tmp = dish;
		}
		fout << result << endl;
	}
	fin.close();
	fout.close();
	return 0;
}
```
