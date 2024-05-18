```c++
#include <iostream>
#include<fstream>
#include<vector>

using namespace std;
struct Point {
	int x, y;
	Point() : Point(0, 0) {}
	Point(int x1, int y1) :x(x1), y(y1) {}

};

struct Line {
	Point p1, p2;
	Line() : Line(0, 0, 0, 0) {}
	Line(int x1, int y1, int x2, int y2) {
		p1 = Point(x1, y1);
		p2 = Point(x2, y2);
	}
	Line(Point p1, Point p2) : p1(p1), p2(p2) {}
};


struct Edge {
	int capacity;
	int flow;
	bool residual;
};

Point corners[1000];
Point hides[50];
Point mouses[250];
int Max_V = 250 + 50 + 2;

vector <int>graph[250];
int capacity[250][50];
int flow[250][50];

int ccw(Point r, Point p, Point q) {
	int result =(p.x - r.x) * (q.y - r.y) - (p.y - r.y) * (q.x - r.x);
	if (result < 0)  return -1;
	else if (result > 0) return 1;
	return 0;
}

bool isIntersection(Line l1, Line l2) { //벽에 존재하지 않기에 겹치기만 해도 false
	Point p1 = l1.p1;
	Point p2 = l1.p2;
	Point p3 = l2.p1;
	Point p4 = l2.p2;

	int p1p2 = ccw(p1, p2, p3) * ccw(p1, p2, p4); // l1 기준
	int p3p4 = ccw(p3, p4, p1) * ccw(p3, p4, p2); // l2 기준
	if (p1p2 == 0 && p3p4 == 0) return false;

	return p1p2 <= 0 && p3p4 <= 0;

}


int mian(void) {
	ifstream fin("mice.inp");
	ofstream fout("mice.inp");
	int T;
	fin >> T;


	while (T--) {
		int n, k, h, m;
		Point temp1;
		fin >> n >> k >> h >> m;

		for (int i = 0; i < n; i++) {
			fin >> temp1.x >> temp1.y;
			corners[i] = temp1;
		}
		for (int i = 0; i < h; i++) {
			fin >> temp1.x >> temp1.y;
			hides[i] = temp1;
		}
		for (int i = 0; i < m; i++) {
			fin >> temp1.x >> temp1.y;
			mouses[i] = temp1;
		}

		for (int i = 0; i < m; i++) {
			for (int j = 0; j < h; j++) {
				Line route = {mouses[i],hides[j]};
				bool check = true;
				for (int k = 0; k < n; k++) {
					Line boundary = { corners[k], corners[(k + 1) % k] };
					check = isIntersection(route, boundary);
					if (!check)break;
				}
				if (check) {
					graph[i].push_back(j);
				}

			}
		}
		


	}
		
	


	return 0;
}

```
