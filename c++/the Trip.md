## the trip
- 문제 해결기법 공부 중 여행자문제의 풀이 코드이다
- 
```c++
#include <fstream>
#include <iomanip> 
#include<iostream>
#include<cmath>
using namespace std;
double inputData[1001]; // 돈 

int main(void) {
	//파일 입출력
	ifstream fin;
	ofstream fout;
	fin.open("trip.inp");
	fout.open("trip.out");

	int num = 0;

	while (true) {
		fin >> num; //학생수
		if (num == 0) break; //0이면 종료
		double sum = 0;
		double small = 0;
		double big = 0;

		for (int i = 0; i < num; i++) { //학생이 사용한 돈과 합계
			fin >> inputData[i];
			sum += inputData[i];
		}
		double average = round(sum / num * 100) / 100.0; //평군 산출하고 소수점 3번쨰 자리에서 반올림

		for (int i = 0; i < num; i++) {  //돈을 많이쓴 사람기준
			if (inputData[i] > average) {
				big += double(inputData[i] - average);  
			}
		}
		for (int i = 0; i < num; i++) {  //돈을 적게쓴사람 기준
			if (inputData[i] < average) {
				small += double(average - inputData[i]);
			}
		}

		
		if (small < big) {		//더 작은거 출력
			fout << fixed << setprecision(2) << "$" << big << "\n";
		}
		else {
			fout << fixed << setprecision(2) << "$" << small << "\n";
			
		}
	}
	fout.close();
	fin.close();
	return 0;
}
```
