```c++
#include <iostream>
#include<fstream>
using namespace std;

int main(void) {
	ifstream fin("rangeSum.inp");
	ofstream fout("rangeSum.out");
	long long int N;
	fin >> N;
	long long int* rangeSum = new long long int[N+1];
	for (long long int i = 1; i < N + 1; i++) 
		fin >> rangeSum[i];
	long long int first, second;
	char trigger;
	while (fin >> trigger >> first >> second) {
		if (trigger == 'q') break;
		else if (trigger == 'c') rangeSum[first] = second;
		else {
			long long int sum = 0;
			for (long long int i = first; i <= second; i++) {
				sum += rangeSum[i];
			}
			fout << sum << endl;
		}
	}
	
	fin.close();
	fout.close();
	return 0;
}
```
