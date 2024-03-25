#Red-Black Tree

## 문제 조건
트리에 insert에 해당하는 key는 트리에 존재하지 않고  delete와 find에 해당하는 키는 트리에 항상 존재한다.
insert는 i delete는d chooice는 c와 함께 정수 입력이 들어온다
출력은 선택한 key해당하는 노드의 색상을 출력한다.

```c++
#include <iostream>
#include <fstream>

using namespace std;

class Node {
public:
    Node* parent;
    Node* left;
    Node* right;
    int key;
    bool color; // true : 레두, false ; 블랙
    Node(int k) : key(k), parent(nullptr), left(nullptr), right(nullptr), color(true) {}
};

class rbTree {
public:
    Node* root;
    rbTree() : root(nullptr) {}

    void leftRotate(Node* x) {
        Node* y = x->right;
        x->right = y->left;
        if (y->left != nullptr)
            y->left->parent = x;
        y->parent = x->parent;
        if (x->parent == nullptr)
            this->root = y;
        else if (x == x->parent->left)
            x->parent->left = y;
        else
            x->parent->right = y;
        y->left = x;
        x->parent = y;
    }


    void rightRotate(Node* x) {
        Node* y = x->left;
        x->left = y->right;
        if (y->right != nullptr) {
            y->right->parent = x;
        }
        y->parent = x->parent;
        if (x->parent == nullptr) {
            this->root = y;
        }
        else if (x == x->parent->right) {
            x->parent->right = y;
        }
        else {
            x->parent->left = y;
        }
        y->right = x;
        x->parent = y;
    }


    void treeInsert(Node* newNode) {
        Node* y = nullptr;
        Node* x = this->root;

        while (x != nullptr) {
            y = x;
            if (newNode->key < y->key)
                x = x->left;
            else
                x = x->right;
        }
        newNode->parent = y;

        if (y == nullptr)
            this->root = newNode;
        else if (newNode->key < y->key)
            y->left = newNode;
        else
            y->right = newNode;
    }

    void rbTreeInsert(int key) {
        Node* x = new Node(key);
        treeInsert(x);
        x->color = true; // 빨간색 지정

        while (x != root && x->parent->color == true) {//부모의 색이 빨간색인 경우
            if (x->parent == x->parent->parent->left) { //부모가 왼쪽 자식인 경우
                Node* y = x->parent->parent->right; //삼촌 지정
                if (y == nullptr || y->color == false) {//case 1
                    if (x == x->parent->right) { //case 2
                        x = x->parent;
                        leftRotate(x);
                    }
                    // Case 3
                    x->parent->color = false;
                    x->parent->parent->color = true;
                    rightRotate(x->parent->parent);
                }
                else {
                    x->parent->color = false;
                    y->color = false;
                    x->parent->parent->color = true;
                    x = x->parent->parent;
                }
            }
            else {
                Node* y = x->parent->parent->left; //삼촌 지정
                if (y == nullptr || y->color == false) { //case 1
                    if (x == x->parent->left) {//case 2
                        x = x->parent;
                        rightRotate(x);
                    }
                    x->parent->color = false; // case3
                    x->parent->parent->color = true;
                    leftRotate(x->parent->parent);
                }
                else {
                    x->parent->color = false;
                    y->color = false;
                    x->parent->parent->color = true;
                    x = x->parent->parent;
                }
            }
        }
        root->color = false;
    }

    Node* TreeMinimum(Node* x) {
        while (x->left != NULL)
            x = x->left;
        return x;
    }

    Node* successor(Node* x) {
        if (x->right != NULL)
            return TreeMinimum(x->right);
        Node* y = x->parent;
        while (y != NULL and x == y->right) {
            x = y;
            y = y->parent;
        }
        return y;
    }

    Node* search(int key) {
        Node* x = root;
        while (x != nullptr and key != x->key) {
            if (key < x->key)
                x = x->left;
            else
                x = x->right;
        }
        return x;
    }

    Node* rbDelete(Node* z) {
        Node* y = nullptr;
        Node* x = nullptr;
        bool check = true;
        if (z->left == NULL or z->right == NULL) y = z; // leaf노드인 경우
        else y = successor(z);
        if (y->left != NULL)         //우리가 삭제하고 싶은 노드
            x = y->left;
        else
            x = y->right;

        if (x != NULL)               //노드 참조 변경
            x->parent = y->parent;

        if (y->parent == NULL)
            root = x;
        else if (y == y->parent->left) {
            y->parent->left = x;
            check = true;
        }
        else {
            y->parent->right = x;
            check = false;
        }

        if (y != z)
            z->key = y->key;
        if (y->color == false) {
            if (x == nullptr) {
                if (check) {
                    x = y;
                    x->parent = y->parent;
                    x->parent->left = x;
                    rbDeleteFixup(x);
                    x->parent->left = nullptr;
                }
                else {
                    x = y;
                    x->parent = y->parent;
                    x->parent->right = x;
                    rbDeleteFixup(x);
                    x->parent->right = nullptr;
                }
            }
            else
                rbDeleteFixup(x);
        }
        return y;
    }
    void rbDeleteFixup(Node* x) {
        while (x != root && (x == nullptr || x->color == false)) {
            if (x != nullptr && x == x->parent->left) {
                Node* w = x->parent->right;
                if (w != nullptr && w->color == true) {
                    w->color = false;
                    x->parent->color = true;
                    leftRotate(x->parent);
                    w = x->parent->right;
                }
                if ((w->left == nullptr || w->left->color == false) && (w->right == nullptr || w->right->color == false)) {
                    w->color = true;
                    x = x->parent;
                }
                else {
                    if (w->right == nullptr || w->right->color == false) {
                        if (w->left != nullptr) w->left->color = false;
                        w->color = true;
                        rightRotate(w);
                        w = x->parent->right;
                    }
                    w->color = x->parent->color;
                    x->parent->color = false;
                    if (w->right != nullptr) w->right->color = false;
                    leftRotate(x->parent);
                    x = root;
                }
            }
            else if (x != nullptr) {
                Node* w = x->parent->left;
                if (w != nullptr && w->color == true) {
                    w->color = false;
                    x->parent->color = true;
                    rightRotate(x->parent);
                    w = x->parent->left;
                }
                if ((w->right == nullptr || w->right->color == false) && (w->left == nullptr || w->left->color == false)) {
                    w->color = true;
                    x = x->parent;
                }
                else {
                    if (w->left == nullptr || w->left->color == false) {
                        if (w->right != nullptr) w->right->color = false;
                        w->color = true;
                        leftRotate(w);
                        w = x->parent->left;
                    }
                    w->color = x->parent->color;
                    x->parent->color = false;
                    if (w->left != nullptr) w->left->color = false;
                    rightRotate(x->parent);
                    x = root;
                }
            }
        }
        if (x != nullptr) x->color = false;
    }

};


int main() {
    ifstream fin;
    ofstream fout;
    Node* sResult=nullptr;
    Node* dResult = nullptr;
    string color;
    char triger;
    int key;
    fin.open("rbt.inp");
    fout.open("rbt.out");
    rbTree tree;
    int count = 0;
    while (true) {
        fin >> triger >> key;
        if (triger == 'i' && key == -1) {
            break;
        }
        else if (triger == 'i') {
            tree.rbTreeInsert(key);
        }
        else if (triger == 'c') {
            sResult=tree.search(key);
            if (sResult->color == true) color = "RED";
            else color = "BLACK";
            fout << "color(" << key << "): " << color << endl;
        }
        else {
            dResult = tree.search(key);
            tree.rbDelete(dResult);
        }
    }
    return 0;
}

```
