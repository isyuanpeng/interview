# 一天一道算法题

## Day1. 快排

```
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100002;
int n;
int q[N];

void quic_sort(int q[], int l, int r) {
    if (l >= r) {
        return;
    }

    int rnd_idx = rand() % (r - l + 1) + l;
    
}


```