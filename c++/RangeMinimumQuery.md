## 구간내 최솟값의 인덱스 번호 찾기   

### 시행착오
- vecotr를 이용하여 minelement함수와 find함수를 이용하여 접근 했지만 시간초과
- 동적 배열을 이용한 접근도 시간초과

### 최종 결론
세그먼트 트리를 이용하여 모두 접근하는 것이 아닌 범위내의 상단 노드에만 접근하여 접근량을 줄이고 비교연산은 updated와 build에서 최대한 수행한다.

```c++
#include <algorithm>
#include<fstream>
#include <vector>

using namespace std;
const long long int INF = numeric_limits<long long int>::max();//무한대 정의


class SegmentTreeNode {
public:
    long long int minValue;
    long long int minIndex;
    void merge(const SegmentTreeNode& left, const SegmentTreeNode& right) {
        if (left.minValue < right.minValue || (left.minValue == right.minValue && left.minIndex < right.minIndex)) {
            minValue = left.minValue;
            minIndex = left.minIndex;
        }
        else {
            minValue = right.minValue;
            minIndex = right.minIndex;
        }
    }
};
class SegmentTree {
private:
    vector<SegmentTreeNode> tree;
    int n;

    void build(const vector<long long int>& arr, int node, int start, int end) { //tree 생성
        if (start == end) {
            tree[node].minValue = arr[start];
            tree[node].minIndex = start;
        }
        else {
            int mid = (start + end) / 2;
            build(arr, 2 * node, start, mid);
            build(arr, 2 * node + 1, mid + 1, end);
            tree[node].merge(tree[2 * node], tree[2 * node + 1]);
        }
    }

    void update(int node, int start, int end, int idx, long long int val) { //값 변경시 min값도 바꾸어야 함
        if (start == end) {
            tree[node].minValue = val;
            tree[node].minIndex = idx;
        }
        else {
            int mid = (start + end) / 2;
            if (idx <= mid) {
                update(2 * node, start, mid, idx, val);
            }
            else {
                update(2 * node + 1, mid + 1, end, idx, val);
            }
            tree[node].merge(tree[2 * node], tree[2 * node + 1]);
        }
    }

    SegmentTreeNode query(int node, int start, int end, int l, int r) { // 가장 작은 값을 묻는 결과
        if (r < start || end < l) {
            return { INF, -1 }; // 범위 밖
        }
        if (l <= start && end <= r) {
            return tree[node];
        }
        int mid = (start + end) / 2;
        SegmentTreeNode q1 = query(2 * node, start, mid, l, r);
        SegmentTreeNode q2 = query(2 * node + 1, mid + 1, end, l, r);
        SegmentTreeNode result;
        result.merge(q1, q2);
        return result;
    }

public:
    SegmentTree(const vector<long long int>& arr) {
        n = arr.size();
        tree.resize(4 * n);
        build(arr, 1, 0, n - 1);
    }

    void update(int idx, long long int val) {
        update(1, 0, n - 1, idx, val);
    }

    long long int query(int l, int r) {
        return query(1, 0, n - 1, l, r).minIndex;
    }
};
int main(void) {
	ifstream fin("rmq.inp");
	ofstream fout("rmq.out");
	long long int N;
	fin >> N;
	
	vector<long long int> rmq(N);
	for (long long int i = 0; i < N; ++i) {
		fin >> rmq[i];
	}

	SegmentTree st(rmq);
	long long int first, second;
	char trigger;
	long long int sum = 0;
	while (true){
		fin >> trigger >> first >> second;
        if (trigger == 's') break;
        else if (trigger == 'c') st.update(first, second);
		else sum += st.query(first, second);
	}
	fout << (sum)%100000;
	fin.close();
	fout.close();
	return 0;
}
```
