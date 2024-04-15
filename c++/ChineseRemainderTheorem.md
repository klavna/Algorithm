
```c++
#include <iostream>
#include <fstream>
#include <vector>
#include <algorithm>
using namespace std;
/*
3s + 5t = 1
gcd(9,12) = 3
3s + 5t = 3

9 합동 4(mod 12)
3 합동 0(mod 4)
*/
long long int gcd(long long int a, long long int b) {
    while (b != 0) {
        long long int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
long long int extended_gcd(long long int a, long long int b) {
    long long int r1 = a;
    long long int r2 = b;
    long long int t1 = 0;
    long long int s1 = 1;
    long long int t2 = 1;
    long long int s2 = 0;
    long long int r;
    long long int s;
    long long int t;
    while (r2 > 0) {
        long long int q = r1 / r2;
        r = r1 - q * r2;
        r1 = r2;
        r2 = r;
        s = s1 - q*s2;
        s1 = s2;
        s2 = s;
        t = t1 - q + t2;
        t1 = t2;
        t2 = t1;
    }
    return s1;
}



int main() {
    ifstream fin("crt.inp");
    ofstream fout("crt.out");

    long long int T;
    fin >> T;

    while (T--) {
        int k;
        fin >> k;

        vector<long long> r(k);
        vector<long long> m(k);

        for (long long int i = 0; i < k; ++i) {
            fin >> r[i] >> m[i];
        }
        long long int tmpR;
        long long int g;
        long long int s;
        long long int tmpM;
        bool a = true;
        for (int i = 0; i < k-1; i++) {
            tmpR = (r[i + 1] - r[i])%m[i+1];
            if (tmpR < 0) {
                tmpR += m[i + 1];
            }
            g = gcd(m[i], m[i + 1]);
            tmpM = m[i];

            m[i] = m[i] / g;
            m[i+1] = m[i+1] / g;

            if (tmpR % g != 0) {    //해가 없음
                a = false;
                break;
            }
            tmpR = tmpR / g;

            s = extended_gcd(m[i], m[i + 1]);
            tmpR = (tmpR * s) % m[i + 1];
            if (tmpR < 0) tmpR += m[i + 1];
            m[i + 1] = m[i + 1] * tmpM;
            r[i+1] = tmpR * tmpM +r[i];
            
            
        }
        if (a) {
            fout << r[k - 1] << endl;
        }
        else {
            fout << -1 << endl;
        }
        
    }

    fin.close();
    fout.close();

    return 0;
}



```
