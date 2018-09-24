---
title: 求包含每个有序数组(共k个)至少一个元素的最小区间
toc: false
date: 2018-09-22 21:03:22
categories:
- OJ
tags:
- 归并
---

> 给定k个**有序**数组, 每个数组有个N个元素，找出一个最小的闭区间，使其包含每个数组中的至少一个元素。 
>
> 关于最小区间——
>
> 给定两个区间[a,b], [c,d]： 
>
> 如果 b-a < d-c，则认为[a, b]是更小的区间；
>
> 如果 b-a == d-c，且a < c，则认为[a, b]是更小的区间。

不妨设k为3，分别为a,b,c数组且a1<=b1<=c1，当前最小区间长度为INT_MAX

假设三个数组所有元素排序后为：

a1,...,b1,...,c1,.....

我们考虑最小的a1，若最小区间包含的a数组的元素为a1，则若[a1,c1]的长度小于当前最小区间长度，更新最小区间长度为c1-a1，

然后我们丢弃a1，因为若最小区间包含的a数组元素为a1，则最小区间一定为[a1,c1]，而我们已经更新了这个长度；若不是a1,则丢弃a1也无关紧要，不影响最后结果。

那么现在我们一直的最小区间长度为c1-a1，三个数组剩下所有元素排序后可能为

a2,..,b1,..,c1,...或

b1,..,a2,..c1,...或

b1,..,c1,..a2...

第一种情况就类似于上边讨论的情况，更新最小区间长度为c1-a2，然后丢弃a2。

对于后两种情况，我们考虑最小的b1：

若最小区间包含的b数组的元素为b1，则最小区间包含的一定为b1,c1,a2，我们更新最小区间长度，然后丢弃b1，

若不是b1，则丢弃b1;

这样不断循环地考虑最小值，知道丢弃完了某一个数组的所有值，这样我们的当前最小区间长度就是所求结果。

下边是代码：

```c++
#include <iostream>
#include <limits.h>
#include <vector>
using namespace std;

int main() {
    int k, n;
    cin>>k>>n;
    if (k <= 0 || n <= 0) return 0;
    vector<vector<int> > nums;
    vector<int> pos;
    for (int i = 0; i < k; i++) {
        vector<int> arrayI;
        int tmp;
        for (int j = 0; j < n; j++) {
            cin>>tmp;
            arrayI.push_back(tmp);
        }
        nums.push_back(arrayI);
        pos.push_back(0);
    }
    int minLength = INT_MAX;
    int realMin, realMax;
    while (true) {
        int arrayOfLeastNum = -1;
        int tmpMin = INT_MAX;
        int tmpMax = INT_MIN;
        for (int i = 0; i < k; i++) {
            if (nums[i][pos[i]] < tmpMin) {
                tmpMin = nums[i][pos[i]];
                arrayOfLeastNum = i;
            }
            if (nums[i][pos[i]] > tmpMax) {
                tmpMax = nums[i][pos[i]];
            }
        }
        if (tmpMax - tmpMin < minLength) {
            minLength = tmpMax - tmpMin;
            realMin = tmpMin, realMax = tmpMax;
        }
        if (++pos[arrayOfLeastNum] >= n) break;
    }
    cout<<realMin<<' '<<realMax;
    return 0;
}
```

但是，这个只能过90%:

> 运行超时:您的程序未能在规定时间内运行结束，请检查是否循环有错或算法复杂度过大。
> case通过率为90.00%

看了一下题下的讨论，可以用一个优先队列（priority_queue，一个STL）或自己实现一个最小堆来维护“当前最小区间队列”，这样查找当前的最大值和最小值就是O(logn)而不是O(n)

下边帖一下别人的代码：

```c++
链接：https://www.nowcoder.com/questionTerminal/0399d363fb594970bae7ad8a3978f86a
来源：牛客网

//思路2，用优先队列去实现多路归并
 
#include<bits/stdc++.h>
using namespace std;
struct pt{
    int x;
    int pos;
    pt(int a,int b):x(a),pos(b){}
};
struct cmp{        //重写比较函数
    bool operator()(const pt a,const pt b){
        return a.x>b.x;
    }
};
int main(){
    int k,n;
    while(cin>>k>>n){
        vector<vector<pt>>all;
        vector<int> vec;
        for(int i = 0;i<k;i++){
            vector<pt>temp;
            for(int j = 0;j<n;j++){
                int x;
                cin>>x;
                pt p(x,i);
                temp.push_back(p);
            }
            all.push_back(temp);
            vec.push_back(0);
        }
        vector<pt> sort;
        priority_queue<pt,vector<pt>,cmp> qu; //优先队列
        for(int i = 0;i<k;i++)
            qu.push(all[i][0]);
        while(!qu.empty()){
            pt temp = qu.top();
            qu.pop();
            sort.push_back(temp);
            vec[temp.pos]++;
            if(vec[temp.pos]<=n-1){
                qu.push(all[temp.pos][vec[temp.pos]]);
            }
        }
        for(int i = 0;i<vec.size();i++)
            vec[i] = 0;
        int begin = 0;
        int end = 0;
        int start = 0;
        int final = 0;
        int length = INT_MAX;
        bool flg = false;
        vec[sort[0].pos]++;
        while(begin<sort.size()-1||end<sort.size()-1){
            bool flgg = true;
            for(int i = 0;i<k;i++)
                flgg = flgg&&(vec[i]>=1);
            if(!flgg&&end<sort.size()){
                if(end<sort.size()-1)
                    end++;
                vec[sort[end].pos]++;
            }
            if(!flgg&&end==sort.size()-1)
                break;
            if(flgg&&begin<sort.size()){
                int x = sort[end].x-sort[begin].x;
                if(x<length){
                    start = sort[begin].x;
                    final = sort[end].x;
                    length = x;
                }
                vec[sort[begin].pos]--;
                if(begin<sort.size()-1)
                    begin++;
            }
            int a = 1;
        }
        cout<<start<<" "<<final<<endl;
    }
    system("pause");
    return 0;
}
```