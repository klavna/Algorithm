문제 : TV Show
	
문제 설명 :
TV show 프로그램 진행자로 유명한 디오프카스 씨는 가끔씩 청중들을 위해 흥미있는 게임을 제시하고 이를 통해 상품을 지급한다. 금주에는 다음과 같은 게임을 제안하였다.

무대에 설치된 개의 전구는 현재 모두 꺼진 상태다. 편의상 전구는 1부터 사이의 번호로 구분한다. 진행자의 주문에 따라 불이 켜질 때 각 전구는 Red 또는 Blue 색깔 중 한 색을 띄게 된다. 게임에 참가한 명의 사람은 전구에 불이 아직 들어오지 않은 상태에서, 각자 3개의 전구를 임의로 선택하고, 선택된 전구의 색깔을 예측한 기록지를 진행자에게 제출한다. 전구에 불이 들어왔을 때, 각 참가자는 자기가 예측한 전구의 색깔이 실제 색깔과 일치되는지를 검사하고, 2개 이상 일치된 사람은 준비된 상품을 받게 된다.

진행자 디오프카스 씨는 오늘 특별한 상품을 준비하였다. 즉, 전구의 색을 예측한 기록지를 모두 검토한 후, 가능하면 모든 참가자가 상품을 받을 수 있도록 전구의 색깔을 조정하려고 한다. 여러분은 디오프카스 씨의 관대함이 실현될 수 있도록, 즉 모든 참가자가 상품을 받을 수 있도록 개의 전구 색깔을 결정할 수 있는지 여부를 밝히는 프로그램을 작성하라. 

【입 력】
입력파일의 이름은 ‘show.inp’이다. 첫째 줄에는 총 테스트케이스의 수를 나타내는 정수 가 주어진다. 각 테스트케이스의 첫째 줄에는 전구의 수를 나타내는 정수 와 참가자 수를 나타내는 정수 이 주어진다. 이어지는 줄 각각에는 참가가 가 선택한 3개의 전구 번호와 그 전구의 색깔을 나타내는 문자가 차례로 주어진다. 전구번호는 1과 사이의 값으로 나타낸다.

【출 력】
출력파일의 이름은 ‘show.out’이다. 각 테스트케이스에 대해, 만약 모든 참가자가 상품을 받을 수 있도록 전구의 색깔을 정할 수 있으면 1, 불가능하면 –1을 출력하라.

```c++
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <unordered_map>

using namespace std;

struct Prediction {
    int bulb1, bulb2, bulb3;
    char color1, color2, color3;
};

bool solve(int n, int m, vector<Prediction>& predictions) {
    int total_combinations = 1 << n;

    for (int mask = 0; mask < total_combinations; ++mask) {
        bool valid = true;

        for (auto& pred : predictions) {
            int count = 0;
            if (((mask >> (pred.bulb1 - 1)) & 1) == (pred.color1 == 'R' ? 1 : 0)) ++count;
            if (((mask >> (pred.bulb2 - 1)) & 1) == (pred.color2 == 'R' ? 1 : 0)) ++count;
            if (((mask >> (pred.bulb3 - 1)) & 1) == (pred.color3 == 'R' ? 1 : 0)) ++count;

            if (count < 2) {
                valid = false;
                break;
            }
        }

        if (valid) {
            return true;
        }
    }

    return false;
}

int main() {
    ifstream fin("show.inp");
    ofstream fout("show.out");

    int t;
    fin >> t;

    while (t--) {
        int n, m;
        fin >> n >> m;

        vector<Prediction> predictions(m);
        for (int i = 0; i < m; ++i) {
            fin >> predictions[i].bulb1 >> predictions[i].color1
                >> predictions[i].bulb2 >> predictions[i].color2
                >> predictions[i].bulb3 >> predictions[i].color3;
        }

        if (solve(n, m, predictions)) {
            fout << "1" << endl;
        }
        else {
            fout << "-1" << endl;
        }
    }

    fin.close();
    fout.close();

    return 0;
}

```
