## 주어진 4점의 정보를 가지고 평행사변형인지 판단하기

## 접근방법
평행사변형의 특징을 이용하여 접근하였다. 마주보는 두변의 길이가 같다라는 특징을 활용하였다
- 각 점사이의 거리를 구한다
- 중복된 거리를 제거한다
- 배열의 길이가 4이하이면 평행사변형이다
```c++
#include<iostream>
#include<fstream>
#include<vector>
#include<cmath>
#include<algorithm>
using namespace std;

struct Point {
    int x, y;
};

double calculateDistance(const Point& p1, const Point& p2) {
    return sqrt(pow(p2.x - p1.x, 2) + pow(p2.y - p1.y, 2));
}

int main(void) {
    ifstream fin("parallelogram.inp");
    ofstream fout("parallelogram.out");

    Point points[4];

    while (true) {
        vector<double> distance;
        int sum = 0;
        for (int i = 0; i < 4; i++) {
            fin >> points[i].x >> points[i].y;
            if (points[i].x == 0 && points[i].y == 0) sum += 1;
        }
        if (sum == 4) break;

        for (int i = 0; i < 4; i++) {
            for (int j = i + 1; j < 4; j++) {
                distance.push_back(calculateDistance(points[i], points[j]));
            }
        }
        sort(distance.begin(), distance.end());
        auto it = unique(distance.begin(), distance.end());

        distance.resize(std::distance(distance.begin(), it));
    
        int result = distance.size();

        if (result>=2&&result<5) fout << 1 << endl; 
        else fout << 0 << endl;
    }

    fin.close();
    fout.close();
    return 0;
}
```
