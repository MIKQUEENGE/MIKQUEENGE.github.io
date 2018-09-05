---
title: PAT刷题记录
toc: true
date: 2018-07-19 22:54:17
categories: OJ
tags:
- PAT
- 刷题
---



OJ：[牛客网](https://www.nowcoder.com/pat)

---



## 1001 A+B Format (20)

**题目描述**

> Calculate a + b and output the sum in standard format -- that is, the digits must be separated into groups of three by commas (unless there are less than four digits).

**输入描述**

> Each input file contains one test case.  Each case contains a pair of integers a and b where -1000000 <= a, b <= 1000000.  The numbers are separated by a space.

**输出描述**

> For each test case, you should output the sum of a and b in one line.  The sum must be written in the standard format.

**输入例子**

> -1000000 9

**输出例子**

> -999,991

**实现代码**

```c++
#include <iostream>
using namespace std;

int main() {
    int digit[10];
    for (int i = 0; i < 10; i++) digit[i] = -1;
    int a,b,re;
    cin >> a >> b;
    re = a + b;
    if (re < 0) {
        cout<<'-';
        re = -re;
    }
    int i = 0;
    while(re) {
        digit[i] = (re%10);
        re/=10;
        i++;
    }
    for (int k = i-1; k >=0; k--) {
        cout<<digit[k];
        if (k%3 == 0 && k>0) cout<<',';
    }
    return 0;
}
```



## 1002 A+B for Polynomials (25)

**题目描述**

> This time, you are supposed to find A+B where A and B are two polynomials.

**输入描述**

> Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial:
> K N1 aN1 N2 aN2 ... NK aNK, where K is the number of nonzero terms in the polynomial, Ni and aNi (i=1, 2, ..., K) are the exponents and coefficients, respectively.  It is given that 1 <= K <= 10，0 <= NK < ... < N2 < N1 <=1000.

**输出描述**

> For each test case you should output the sum of A and B in one line, with the same format as the input.  Notice that there must be NO extra space at the end of each line.  **Please be accurate to 1 decimal place.**

**输入例子**

> 2 1 2.4 0 3.2
> 2 2 1.5 1 0.5

**输出例子**

> 3 2 1.5 1 2.9 0 3.2

**实现代码**

注意保留小数。

```c++
#include <iostream>
#include <stdlib.h>
#include <vector>
using namespace std;

struct item
{
	int n;
	float value;
	item(int a, float b) {n=a,value=b;}
};

int main() {
	vector<item> polynomials;
	int k;

	cin>>k;
	int tmp_n;
	float tmp_value;
	while (k--) {
		cin>>tmp_n>>tmp_value;
		polynomials.push_back(item(tmp_n,tmp_value));
	}

	cin>>k;
	while (k--) {
		cin>>tmp_n>>tmp_value;
		bool hasItem = false;
		for (int i = 0; i < polynomials.size(); i++) {
			if (polynomials[i].n == tmp_n) {
				hasItem = true;
				polynomials[i].value+=tmp_value;
				if (polynomials[i].value == 0)
					polynomials.erase(polynomials.begin()+i);
				break;
			}
		}		
		if (!hasItem) polynomials.push_back(item(tmp_n,tmp_value));	
	}

	for (int k = 0; k < polynomials.size(); k++) {
		for (int i = 0; i < polynomials.size()-1; i++) {
			if (polynomials[i].n < polynomials[i+1].n) {
				item tmp_item = polynomials[i];
				polynomials[i] = polynomials[i+1];
				polynomials[i+1] = tmp_item;
			}
		}
	}

	cout<<polynomials.size();
	for (int i = 0; i < polynomials.size(); i++)
		printf(" %d %.1f", polynomials[i].n, polynomials[i].value);

	return 0;
}
```



## 1003 Emergency (25)

**题目描述**

> As an emergency rescue team leader of a city, you are given a special map of your country.  The map shows several scattered cities connected by some roads.  Amount of rescue teams in each city and the length of each road between any pair of cities are marked on the map.  When there is an emergency call to you from some other city, your job is to lead your men to the place as quickly as possible, and at the mean time, call up as many hands on the way as possible.

**输入描述**

> Each input file contains one test case. For each test case, the first line contains 4 positive integers: N (<= 500) - the number of cities (and the cities are numbered from 0 to N-1), M - the number of roads, C1 and C2 - the cities that you are currently in and that you must save, respectively.  The next line contains N integers, where the i-th integer is the number of rescue teams in the i-th city.  Then M lines follow, each describes a road with three integers c1, c2 and L, which are the pair of cities connected by a road and the length of that road, respectively.  
> It is guaranteed that there exists at least one path from C1 to C2.

**输出描述**

> For each test case, print in one line two numbers: the number of different shortest paths between C1 and C2, and the maximum amount of rescue teams you can possibly gather.
>
> All the numbers in a line must be separated by exactly one space, and there is no extra space allowed at the end of a line.

**输入例子**

> 5 6 0 2
> 1 2 1 5 3
> 0 1 1
> 0 2 2
> 0 3 1
> 1 2 1
> 2 4 1
> 3 4 1

**输出例子**

> 2 4

**实现代码**

dijkstra最短路

```c++
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

struct road
{
	int endCity;
	int length;
	road(int e, int l) {endCity = e, length = l;}
};


int main() {
	int n,m,c1,c2;
	cin>>n>>m>>c1>>c2;
	vector<int> teamsCount;

	int tmp1,tmp2,tmp3;
	for (int i = 0; i < n; i++) {
		cin>>tmp1;
		teamsCount.push_back(tmp1);
	}

	vector<vector<road> > roads(n);
	for (int i = 0; i < m; i++) {
		cin>>tmp1>>tmp2>>tmp3;
		roads[tmp1].push_back(road(tmp2,tmp3));
		roads[tmp2].push_back(road(tmp1,tmp3));
	}

	vector<int> maxTeams(n);
	for (int i = 0; i < n; i++) maxTeams[i] = 0;
	maxTeams[c1] = teamsCount[c1];

	vector<int> minLen(n);
	for (int i = 0; i < n; i++) minLen[i] = INT_MAX;
	minLen[c1] = 0;

	int currentCity = c1;
	vector<int> minLenRoadCount(n);
	for (int i = 0; i < n; i++) minLenRoadCount[i] = 1;

	vector<bool> visited(n);
	for (int i = 0; i < n; i++) visited[i] = false;
	visited[c1] = true;
	while (currentCity != c2) {
		int len = roads[currentCity].size();
		for (int i = 0; i < len; i++) {
			if (visited[roads[currentCity][i].endCity]) continue;
			int endCity = roads[currentCity][i].endCity;
			int length = roads[currentCity][i].length;
			if (minLen[endCity] > minLen[currentCity] + length) {
				minLen[endCity] = minLen[currentCity] + length;
				minLenRoadCount[endCity] = 1;
				maxTeams[endCity] = maxTeams[currentCity] + teamsCount[endCity];
			} else if (minLen[endCity] == minLen[currentCity] + length) {
				minLenRoadCount[endCity]++;
				if (maxTeams[endCity] < maxTeams[currentCity] + teamsCount[endCity])
					maxTeams[endCity] = maxTeams[currentCity] + teamsCount[endCity];
			}
		}

		int min = INT_MAX;
		for (int i = 0; i < n; i++) {
			if (visited[i]) continue;
			if (minLen[i] < min) {
				min = minLen[i];
				currentCity = i;
			}
		}
		visited[currentCity] = true;
	}

	cout<<minLenRoadCount[c2]<<' '<<maxTeams[c2];

	return 0;
}
```



## 1004 Counting Leaves (30)

**题目描述**

> A family hierarchy is usually presented by a pedigree tree.  Your job is to count those family members who have no child.

**输入描述**

> Each input file contains one test case. Each case starts with a line containing 0 < N < 100, the number of nodes in a tree, and M (< N), the number of non-leaf nodes.  Then M lines follow, each in the format:
>
> ID K ID[1] ID[2] ... ID[K]
>
> where ID is a two-digit number representing a given non-leaf node, K is the number of its children, followed by a sequence of two-digit ID's of its children.  For the sake of simplicity, let us fix the root ID to be 01.

**输出描述**

> For each test case, you are supposed to count those family members who have no child **for every seniority level** starting from the root.  The numbers must be printed in a line, separated by a space, and there must be no extra space at the end of each line.
> The sample case represents a tree with only 2 nodes, where 01 is the root and 02 is its only child.  Hence on the root 01 level, there is 0 leaf node; and on the next level, there is 1 leaf node.  Then we should output "0 1" in a line.

**输入例子**

> 2 1
> 01 1 02

**输出例子**

> 0 1

**实现代码**

```c++
#include <iostream>
#include <stdlib.h>
#include <vector>
using namespace std;

#define MAX_NODES 100

struct node
{
	int id = -1;
	int childs_count = 0;
	vector<int> childs;
};

int main() {
	int n,m;
	cin>>n>>m;
	node nodes[MAX_NODES];

	int id,k,tmp;
	while (m--) {
		cin>>id>>k;
		nodes[id].id = id;
		nodes[id].childs_count = k;
		while (k--) {
			cin>>tmp;
			(nodes[id].childs).push_back(tmp);
		}
	}

	vector<int> this_level, next_level;
	this_level.push_back(1);
	while (1) {
		int count_output = 0;
		for (int i = 0; i < this_level.size(); i++) {
			if (nodes[this_level[i]].childs_count == 0) count_output++;
			else next_level.insert(next_level.begin(),nodes[this_level[i]].childs.begin(),nodes[this_level[i]].childs.end());
		}
		cout<<count_output;
		if (next_level.empty()) break;
		cout<<' ';
		this_level = next_level;
		next_level.clear();
	}

	return 0;
}
```



## 1005 Spell It Right (20)

**题目描述**

> Given a non-negative integer N, your task is to compute the sum of all the digits of N, and output every digit of the sum in English.

**输入描述**

> Each input file contains one test case. Each case occupies one line which contains an N (<= 10^100).

**输出描述**

> For each test case, output in one line the digits of the sum in English words. There must be one space between two consecutive words, but no extra space at the end of a line.

**输入例子**

> 12345

**输出例子**

> one five

**实现代码**

```c++
#include <iostream>
#include <string>
using namespace std;

int main() {
    string n;
    cin>>n;
    int len = n.size();
    int re = 0;
    for (int i = 0; i < len; i++) re+=(n[i]-'0');
    string english[10] = {"zero","one","two","three","four","five","six","seven","eight","nine"};
    int digit[100], re_len = 0;
    while (re) {
        digit[re_len] = (re%10);
        re/=10;
        re_len++;
    }
    for (int i = re_len-1; i >= 0; i--) {
        cout<<english[digit[i]];
        if (i != 0) cout<<' ';
    }
    return 0;
}
```



## 1006 Sign In and Sign Out (25)

**题目描述**

> At the beginning of every day, the first person who signs in the computer room will unlock the door, and the last one who signs out will lock the door.  Given the records of signing in's and out's, you are supposed to find the ones who have unlocked and locked the door on that day.

**输入描述**

> Each input file contains one test case. Each case contains the records for one day.  The case starts with a positive integer M, which is the total number of records, followed by M lines, each in the format:
>
> ID_number Sign_in_time Sign_out_time
>
> where times are given in the format HH:MM:SS, and ID number is a string with no more than 15 characters.

**输出描述**

> For each test case, output in one line the ID numbers of the persons who have unlocked and locked the door on that day.  The two ID numbers must be separated by one space.
> Note:  It is guaranteed that the records are consistent.  That is, the sign in time must be earlier than the sign out time for each person, and there are no two persons sign in or out at the same moment.

**输入例子**

> 3
> CS301111 15:30:28 17:00:10
> SC3021234 08:00:00 11:25:25
> CS301133 21:45:00 21:58:40

**输出例子**

> SC3021234 CS301133

**实现代码**

```c++
#include <iostream>
#include <string>
using namespace std;

bool a_larger_than_b(string a, string b) {
    for (int i = 0; i < a.size(); i++) {
        if (a[i] < b[i]) return false;
        if (a[i] > b[i]) return true;
    }
    return true;
}

int main() {
    int n;
    cin>>n;
    string id,in,out;
    string result_in_id, result_in_time, result_out_id, result_out_time;
    cin>>id>>in>>out;
    result_in_id = result_out_id = id;
    result_in_time = in;
    result_out_time = out;
    n--;
    while (n--) {
        cin>>id>>in>>out;
        if (a_larger_than_b(result_in_time,in)) {
            result_in_id = id;
            result_in_time = in;
        }
        if (a_larger_than_b(out,result_out_time)) {
            result_out_id = id;
            result_out_time = out;
        }
    }
    cout<<result_in_id<<' '<<result_out_id;
    return 0;
}
```



## 1007 Maximum Subsequence Sum (25)

**题目描述**

> Given a sequence of K integers { N1
> , N2
> , ..., NK
>  }.  A continuous subsequence is defined to be { Ni
> , Ni+1
> , ..., Nj
>  } where 1 <= i <= j <= K.  The *Maximum Subsequence*
>  is the continuous subsequence which has the largest sum of its
> elements.  For example, given sequence { -2, 11, -4, 13, -5, -2 }, its
> maximum subsequence is { 11, -4, 13 } with the largest sum being 20.
>
> Now you are supposed to find the largest sum, together with the first
> and the last numbers of the maximum subsequence.

**输入描述**

> Each input file contains one test case. Each case occupies two lines.  The first line contains a positive integer K (<= 10000).  The second line contains K numbers, separated by a space.

**输出描述**

> For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line.  In case that the maximum subsequence is not unique, output the one with the smallest indices i and j (as shown by the sample case).  If all the K numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

**输入例子**

> 10
> -10 1 2 3 4 -5 -23 3 7 -21

**输出例子**

> 10 1 4

**实现代码**

<u>和为负的子序列一定不会是最大和子序列的开头。</u>

当当前和小于0时，使用临时"first number"记录下一个数，

更新最大和时，当前数为“last number”， 临时“first number”中存储的值为“first number”的值。

```c++
#include <iostream>
#include <stdlib.h>
#include <queue>
using namespace std;


int main() {
	int n;
	cin>>n;
	int* nums = new int[n];
	bool allNegative = true;

	for (int i = 0; i < n; i++) {
		cin>>nums[i];
		if (nums[i] >= 0) allNegative = false;
	}

	if (allNegative) {
		cout<<0<<' '<<nums[0]<<' '<<nums[n-1];
	} else {
		int maxSum = 0, currentSum = 0;
		int firstNum = nums[0], lastNum = nums[0];
		int firstNumCurrent = firstNum;
		for (int i = 0; i < n; i++) {
			currentSum+=nums[i];
			if (currentSum < 0) {
				currentSum = 0;
				firstNumCurrent = nums[i+1];
			} else if (currentSum > maxSum) {
				maxSum = currentSum;
				firstNum = firstNumCurrent;
				lastNum = nums[i];
			}
		}
		cout<<maxSum<<' '<<firstNum<<' '<<lastNum;
	}

	delete nums;

	return 0;
}
```



## 1008 Elevator (20)

**题目描述**

> The highest building in our city has only one elevator. A request list is made up with N positive numbers. The numbers denote at which floors the elevator will stop, in specified order. It costs 6 seconds to move the elevator up one floor, and 4 seconds to move down one floor. The elevator will stay for 5 seconds at each stop.
> For a given request list, you are to compute the total time spent to fulfill the requests on the list. The elevator is on the 0th floor at the beginning and does not have to return to the ground floor when the requests are fulfilled.

**输入描述**

> Each input file contains one test case. Each case contains a positive integer N, followed by N positive numbers. All the numbers in the input are less than 100.

**输出描述**

> For each test case, print the total time on a single line.

**输入例子**

> 3 2 3 1

**输出例子**

> 41

**实现代码**

```c++
#include <iostream>
using namespace std;

int main() {
    int a = 0, b, n, re = 0;
    cin >> n;
    while (n--) {
        cin >> b;
        if (b > a) {
            re+=((b-a)*6);
        } else {
            re+=((a-b)*4);
        }
        re+=5;
        a = b;
    }
    cout<<re;
    return 0;
}
```



## 1009 Product of Polynomials (25)

**题目描述**

> This time, you are supposed to find A*B where A and B are two polynomials.

**输入描述**

> Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial:
> K N1 aN1 N2 aN2 ... NK aNK, where K is the number of nonzero terms in the polynomial, Ni and aNi (i=1, 2, ..., K) are the exponents and coefficients, respectively.  It is given that 1 <= K <= 10, 0 <= NK < ... < N2 < N1 <=1000.

**输出描述**

> For each test case you should output the product of A and B in one line, with the same format as the input.  Notice that there must be NO extra space at the end of each line.  Please be accurate up to 1 decimal place.

**输入例子**

> 2 1 2.4 0 3.2
> 2 2 1.5 1 0.5

**输出例子**

> 3 3 3.6 2 6.0 1 1.6

**实现代码**

把[1002 A+B for Polynomials (25)](#1002-A-B-for-Polynomials-25)稍微改一下就好。

使用`float`虽然算出来明明是对的但是过不了牛客的测试，全部换成`double`就好了，不知道是什么问题。

```c++
#include <iostream>
#include <stdlib.h>
#include <vector>
using namespace std;

struct item
{
	int n;
	double value;
	item(int a, double b) {n=a,value=b;}
};

int main() {
	vector<item> polynomials, result;
	int k;

	cin>>k;
	int tmp_n;
	double tmp_value;
	while (k--) {
		cin>>tmp_n>>tmp_value;
		polynomials.push_back(item(tmp_n,tmp_value));
	}

	cin>>k;
	while (k--) {
		cin>>tmp_n>>tmp_value;
		for (int k = 0; k < polynomials.size(); k++) {
			bool hasItem = false;
			for (int i = 0; i < result.size(); i++) {
				if (result[i].n == tmp_n+polynomials[k].n) {
					hasItem = true;
					result[i].value+=(tmp_value*polynomials[k].value);
					if (result[i].value == 0)
						result.erase(result.begin()+i);
					break;
				}
			}		
			if (!hasItem) result.push_back(item(tmp_n+polynomials[k].n,tmp_value*polynomials[k].value));
		}
	}

	for (int k = 0; k < result.size(); k++) {
		for (int i = 0; i < result.size()-1; i++) {
			if (result[i].n < result[i+1].n) {
				item tmp_item = result[i];
				result[i] = result[i+1];
				result[i+1] = tmp_item;
			}
		}
	}

	cout<<result.size();
	for (int i = 0; i < result.size(); i++)
		printf(" %d %.1f", result[i].n, result[i].value);

	return 0;
}
```



## 1015 Reversible Primes (20)

**题目描述**

> A *reversible prime*
>  in any number system is a prime whose "reverse" in that
> number system is also a prime. For example in the decimal system 73 is a
> reversible prime because its reverse 37 is also a prime.
>
> 
>
> Now given any two positive integers N (< 105
> ) and D (1 < D <= 10), you are supposed to tell if N is a
> reversible prime with radix D.

**输入描述**

> The input file consists of several test cases.  Each case occupies a line which contains two integers N and D.  The input is finished by a negative N.

**输出描述**

> For each test case, print in one line "Yes" if N is a reversible prime with radix D, or "No" if not.

**输入例子**

> 73 10
> 23 2
> 23 10
> -2

**输出例子**

> Yes
> Yes
> No

**实现代码**

求N以及N在D进制下反转后是否均为质数。

```c++
#include <iostream>
#include <stdlib.h>
#include <math.h>
#include <vector>
using namespace std;

int reverseWithRadix(int n, int d) {
	vector<int> remainers;
	while (n > 0) {
		remainers.push_back(n%d);
		n/=d;
	}
	int re = 0;
	int bit_count = remainers.size();
	for (int i = 0; i < bit_count; i++)
		re+=(pow(d,bit_count-i-1)*remainers[i]);
	return re;
}

bool isPrime(int n) {
	if (n == 1) return false;
	if (n < 4) return true;
	if (n % 2 == 0) return false;
	int sqrt_n = sqrt(n) + 1;
	for (int i = 3; i <= sqrt_n; i+=2) {
		if (n % i == 0) return false;
	}
	return true;
}

int main() {
	int n,d;
	while (1) {
		cin>>n;
		if (n < 0) break;
		cin>>d;
		if (isPrime(n) && isPrime(reverseWithRadix(n,d))) cout<<"Yes"<<endl;
		else cout<<"No"<<endl;
	}
	return 0;
}

```



## 1020 Tree Traversals (25)

**题目描述**

> Suppose that all the keys in a binary tree are distinct positive integers.  Given the postorder and inorder traversal sequences, you are supposed to output the level order traversal sequence of the corresponding binary tree.

**输入描述**

> Each input file contains one test case.  For each case, the first line gives a positive integer N (<=30), the total number of nodes in the binary tree.  The second line gives the **postorder** sequence and the third line gives the **inorder** sequence.  All the numbers in a line are separated by a space.

**输出描述**

> For each test case, print in one line the level order traversal sequence of the corresponding binary tree.  All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

**输入例子**

> 7
> 2 3 1 5 7 6 4
> 1 2 3 4 5 6 7

**输出例子**

> 4 1 6 3 5 7 2

postorder: 后序遍历

inorder: 中序遍历

level order: 层序遍历（从根开始,依次向下,对于每一层从左向右遍历）

**实现代码**

```c++
#include <iostream>
#include <stdlib.h>
#include <queue>
using namespace std;


struct node
{
	node* left;
	node* right;
	int value;
};


node* binaryTreeRoot(int* postorder, int* inorder, int len) {
	if (len <= 0) return NULL;

	node* root = new node;
	root->value = *(postorder+len-1);

	int pos = 0;
	for (; pos < len; pos++)
		if (*(inorder+pos) == root->value) break;

	root->left = binaryTreeRoot(postorder, inorder, pos);
	int rightLen = len - pos - 1;
	root->right = binaryTreeRoot(postorder+pos, inorder+pos+1, rightLen);

	return root;
}

void deleteNodes(node* root) {
	if (root == NULL) return;
	deleteNodes(root->left);
	deleteNodes(root->right);
	delete root;
}

int main() {
	int n;
	cin>>n;
	int* postorderNodes = new int[n];
	int* inorderNodes = new int[n];
	for (int i = 0; i < n; i++) cin>>postorderNodes[i];
	for (int i = 0; i < n; i++) cin>>inorderNodes[i];

	node* root = binaryTreeRoot(postorderNodes, inorderNodes, n);
	if (!root) return 0;

	queue<node*> levelNodes;
	levelNodes.push(root);
	while (1) {
		node* tmp = levelNodes.front();
		if (tmp->left) levelNodes.push(tmp->left);
		if (tmp->right) levelNodes.push(tmp->right);
		cout<<tmp->value;
		levelNodes.pop();
		if (!levelNodes.empty()) cout<<' ';
		else break;
	}

	deleteNodes(root);
	delete postorderNodes;
	delete inorderNodes;

	return 0;
}
```



## 1023 Have Fun with Numbers (20)

**题目描述**

> Notice that the number 123456789 is a 9-digit number consisting exactly the numbers from 1 to 9, with no duplication.  Double it we will obtain 246913578, which happens to be another 9-digit number consisting exactly the numbers from 1 to 9, only in a different permutation.  Check to see the result if we double it again!
>
> Now you are suppose to check if there are more numbers with this property.  That is, double a given number with k digits, you are to tell if the resulting number consists of only a permutation of the digits in the original number.

**输入描述**

> Each input file contains one test case.  Each case contains one positive integer with no more than 20 digits.

**输出描述**

> For each test case, first print in a line "Yes" if doubling the input number gives a number that consists of only a permutation of the digits in the original number, or "No" if not.  Then in the next line, print the doubled number.

**输入例子**

> 1234567899

**输出例子**

> Yes
> 2469135798

**实现代码**

若双倍后多一位则为No;

使用`digit_count[i]`保存数字`i`（0-9）的个数，

再减去双倍后各个数字的个数，若每一个`digit_count[i]`均为0则为Yes,否则为No。

```c++
#include <iostream>
#include <stdlib.h>
#include <vector>
#include <stack>
#include <climits>
using namespace std;



int main() {
	int digit_count[10];
	for (int i = 0; i < 10; i++) digit_count[i] = 0;
	string s;
	cin>>s;
	int k = s.size();
	vector<int> digits;
	for (int i = 0; i < k; i++) {
		int digit = s[i]-'0';
		digits.push_back(digit);
		digit_count[digit]++;
	}
	int carry = 0;
	for (int i = k-1; i >= 0; i--) {
		digits[i] = 2*digits[i] + carry;
		carry = digits[i] / 10;
		digits[i] %= 10;
		digit_count[digits[i]]--;
	}
	if (carry) {
		cout<<"No\n"<<carry;
		for (int i = 0; i < k; i++) cout<<digits[i];
		return 0;
	}
	bool result = true;
	for (int i = 0; i < 10; i++)
		if (digit_count[i] != 0) {
			result = false;
			break;
		}
	if (result) cout<<"Yes\n";
	else cout<<"No\n";
	for (int i = 0; i < k; i++) cout<<digits[i];

	return 0;
}

```





## 1027 Colors in Mars (20)

**题目描述**

> People in Mars represent the colors in their computers in a similar way as the Earth people.  That is, a color is represented by a 6-digit number, where the first 2 digits are for Red, the middle 2 digits for Green, and the last 2 digits  for Blue.  The only difference is that they use radix 13 (0-9 and A-C) instead of 16.  Now given a color in three decimal numbers (each between 0 and 168), you are supposed to output their Mars RGB values.

**输入描述**

> Each input file contains one test case which occupies a line containing the three decimal color values.

**输出描述**

> For each test case you should output the Mars RGB value in the following format: first output "#", then followed by a 6-digit number where all the English characters must be upper-cased.  If a single color is only 1-digit long, you must print a "0" to the left.

**输入例子**

> 15 43 71

**输出例子**

> \#123456

**实现代码**

```c++
#include <iostream>
using namespace std;

int main() {
    cout<<'#';
    int n;
    for (int i = 0; i < 3; i++) {
        cin>>n;
        int tmp = n/13;
        if (tmp > 9) cout<<char((tmp-10)+'A');
        else cout<<tmp;
        tmp = n%13;
        if (tmp > 9) cout<<char((tmp-10)+'A');
        else cout<<tmp;
    }
    return 0;
}
```



## 1028 List Sorting (25)

**题目描述**

> Excel can sort records according to any column.  Now you are supposed to imitate this function.

**输入描述**

> Each input file contains one test case.  For each case, the first line contains two integers N (<=100000) and C, where N is the number of records and C is the column that you are supposed to sort the records with.  Then N lines follow, each contains a record of a student.  A student's record consists of his or her distinct ID (a 6-digit number), name (a string with no more than 8 characters without space), and grade (an integer between 0 and 100, inclusive).

**输出描述**

> For each test case, output the sorting result in N lines.  That is, if C = 1 then the records must be sorted in increasing order according to ID's; if C = 2 then the records must be sorted in non-decreasing order according to names; and if C = 3 then the records must be sorted in non-decreasing order according to grades.  **If there are several students who have the same name or grade, they must be sorted according to their ID's in increasing order.**

**输入例子**

> 3 1
> 000007 James 85
> 000010 Amy 90
> 000001 Zoe 60

**输出例子**

> 000001 Zoe 60
> 000007 James 85
> 000010 Amy 90

**实现代码**

利用结构体，使用sort函数排序。

```c++
#include <iostream>
#include <vector>
#include <string>
#include <climits>
#include <algorithm>
using namespace std;

struct student
{
	string id;
	string name;
	int grade;
	student(string i, string n, int g) {id = i, name = n, grade = g;}
};

bool cmp1(student a, student b) {
	return a.id < b.id;
}

bool cmp2(student a, student b) {
	return (a.name == b.name)?(a.id < b.id):(a.name < b.name);
}

bool cmp3(student a, student b) {
	return (a.grade == b.grade)?(a.id < b.id):(a.grade < b.grade);
}

int main() {
	int n,c;
	cin>>n>>c;
	vector<student> students;

	string id,name;
	int grade;
	for (int i = 0; i < n; i++) {
		cin>>id>>name>>grade;
		students.push_back(student(id,name,grade));
	}

	if (c == 1) sort(students.begin(),students.end(),cmp1);
	else if (c == 2) sort(students.begin(),students.end(),cmp2);
	else sort(students.begin(),students.end(),cmp3);

	if (n > 0) cout<<students[0].id<<' '<<students[0].name<<' '<<students[0].grade;
	for (int i = 1; i < n; i++) {
		cout<<"\n"<<students[i].id<<' '<<students[i].name<<' '<<students[i].grade;
	}

	return 0;
}
```



## 1029 Median (25)

**题目描述**

> Given an increasing sequence S of N integers, the *median* is the number at the middle position.  For example, the median of S1={11, 12, 13, 14} is 12, and the median of S2={9, 10, 15, 16, 17} is 15.  The median of two sequences is defined to be the median of the nondecreasing sequence which contains all the elements of both sequences.  For example, the median of S1 and S2 is 13.
> Given two increasing sequences of integers, you are asked to find their median.

**输入描述**

> Each input file contains one test case.  Each case occupies 2 lines, each gives the information of a sequence.  For each sequence, the first positive integer N (<=1000000) is the size of that sequence.  Then N integers follow, separated by a space.  It is guaranteed that all the integers are in the range of **long int**.

**输出描述**

> For each test case you should output the median of the two given sequences in a line.

**输入例子**

> 4 11 12 13 14
> 5 9 10 15 16 17

**输出例子**

> 13

**实现代码**

用两个数组存储两组数据，每组数据一个pos，比较两个pos出的数值大小，较小值的pos向后移，直到找到中位数。

```c++
#include <iostream>
#include <stdlib.h>
#include <queue>
using namespace std;


int main() {
	int m,n;
	cin>>m;
	int* firstSequence = new int[m];
	for (int i = 0; i < m; i++) cin>>firstSequence[i];
	cin>>n;
	int* secondSequence = new int[n];
	for (int i = 0; i < n; i++) cin>>secondSequence[i];

	int firstPos = 0, secondPos = 0;
	for (int i = int((m+n+1)/2); i > 0; i--) {
		if (i == 1) {
			if (firstPos == m) cout<<secondSequence[secondPos];
			else if (secondPos == n) cout<<firstSequence[firstPos];
			else if (firstSequence[firstPos] < secondSequence[secondPos]) cout<<firstSequence[firstPos];
			else cout<<secondSequence[secondPos];
		} else {
			if (firstPos == m) secondPos++;
			else if (secondPos == n) firstPos++;
			else if (firstSequence[firstPos] < secondSequence[secondPos]) firstPos++;
			else secondPos++;
		}
	}

	delete firstSequence;
	delete secondSequence;

	return 0;
}
```



## 1030 Travel Plan (30)

**题目描述**

> A traveler's map gives the distances between cities along the highways, together with the cost of each highway. 
>  Now you are supposed to write a program to help a traveler to decide the shortest path between his/her starting city and the destination.
>  If such a shortest path is not unique, you are supposed to output the one with the minimum cost, which is guaranteed to be unique.
>
>  DECLARE: The test data in PAT is wrong,we strengthened the test data.If the same code got passed in pat,it may not be able to get passed in NOWCODER,please check your code.(This means that our test data is no problem,I guarantee.)

**输入描述**

> Each input file contains one test case. Each case starts with a line containing 4 positive integers N, M, S, and D, where N (<=500) is the number of cities (and hence the cities are numbered from 0 to N-1); M is the number of highways; S and D are the starting and the destination cities, respectively. Then M lines follow, each provides the information of a highway, in the format:
>  City1 City2 Distance Cost
>
>  where the numbers are all integers no more than 500, and are separated by a space.

**输出描述**

> For each test case, print in one line the cities along the shortest path from the starting point to the destination, followed by the total distance and the total cost of the path. The numbers must be separated by a space and there must be no extra space at the end of output.

**输入例子**

> 4 5 0 3
>
> 0 1 1 20
>
> 1 3 2 30
>
> 0 3 4 10
>
> 0 2 2 20
>
> 2 3 1 20

**输出例子**

> 0 2 3 3 40

**实现代码**

最短路问题，用Dijkstra（迪杰斯特拉）算法解即可。

```c++
#include <iostream>
#include <stdlib.h>
#include <vector>
#include <stack>
#include <climits>
using namespace std;

struct highway
{
	int endCity;
	int distance;
	int cost;
	highway(int e, int d, int c) {endCity=e,distance=d,cost=c;}
};


int main() {
	// N为城市数，M为公路数，S为开始城市，D为结束城市
	int N,M,S,D;
	cin>>N>>M>>S>>D;

	// highways(i) 为与城市i相连的所有公路
	vector<vector<highway> > highways(N);
	bool* visited = new bool[N];
	int* distance = new int[N];
	int* cost = new int[N];
	int* lastCity = new int[N];
	for (int i = 0; i < N; i++) 
		visited[i] = false, distance[i] = cost[i] = INT_MAX;

	// 读入数据，更新highways
	int tmp_city_1, tmp_city_2, tmp_distance, tmp_cost;
	while (M--) {
		cin>>tmp_city_1>>tmp_city_2>>tmp_distance>>tmp_cost;
		highways[tmp_city_1].push_back(highway(tmp_city_2,tmp_distance,tmp_cost));
		highways[tmp_city_2].push_back(highway(tmp_city_1,tmp_distance,tmp_cost));
	}

	// dijkstra
	int currentCity = S;
	visited[S] = true, distance[S] = cost[S] = 0, lastCity[S] = S;
	while (currentCity != D) {
		for (int i = 0; i < highways[currentCity].size(); i++) {
			int tmp_endCity = highways[currentCity][i].endCity, 
				tmp_distance = highways[currentCity][i].distance,
				tmp_cost = highways[currentCity][i].cost;				
			if (visited[tmp_endCity]) continue;
			if (distance[currentCity] + tmp_distance < distance[tmp_endCity] ||
				(distance[currentCity] + tmp_distance == distance[tmp_endCity] && 
				 cost[currentCity] + tmp_cost < cost[tmp_endCity])) {
				distance[tmp_endCity] = distance[currentCity] + tmp_distance;
				cost[tmp_endCity] = cost[currentCity] + tmp_cost;
				lastCity[tmp_endCity] = currentCity;
			}
		}
		int minDistance = INT_MAX;
		for (int i = 0; i < N; i++) {
			if (visited[i]) continue;
			if (distance[i] < minDistance) minDistance=distance[i],currentCity = i;
		}
		// 添加离起始点最短的点到已访问集
		visited[currentCity] = true;
	}

	// 逆序列逆向输出即为最短路径
	stack<int> pathStack;
	pathStack.push(D);
	currentCity = D;
	while (currentCity != S) {
		currentCity = lastCity[currentCity];
		pathStack.push(currentCity);
	}

	while (!pathStack.empty()) {
		cout<<pathStack.top()<<' ';
		pathStack.pop();
	}
	cout<<distance[D]<<' '<<cost[D];

	delete visited,distance,cost,lastCity;
	return 0;
}

```



## 1035 Password (20)

**题目描述**

> To prepare for PAT, the judge sometimes has to generate random passwords for the users.  The problem is that there are always some confusing passwords since it is hard to distinguish 1 (one) from l (L in lowercase), or 0 (zero) from O (o in uppercase).  One solution is to replace 1 (one) by @, 0 (zero) by %, l by L, and O by o.  Now it is your job to write a program to check the accounts generated by the judge, and to help the juge modify the confusing passwords.

**输入描述**

> Each input file contains one test case.  Each case contains a positive integer N (<= 1000), followed by N lines of accounts.  Each account consists of a user name and a password, both are strings of no more than 10 characters with no space.

**输出描述**

> For each test case, first print the number M of accounts that have been modified, then print in the following M lines the modified accounts info, that is, the user names and the corresponding modified passwords.  The accounts must be printed in the same order as they are read in.  If no account is modified, print in one line "There are N accounts and no account is modified" where N is the total number of accounts.  However, if N is one, you must print "There is 1 account and no account is modified" instead.

**输入例子**

> 3
> Team000002 Rlsp0dfa
> Team000003 perfectpwd
> Team000001 R1spOdfa

**输出例子**

> 2
> Team000002 RLsp%dfa
> Team000001 R@spodfa

**实现代码**

```c++
#include <iostream>
#include <vector>
#include <string>
#include <climits>
#include <algorithm>
using namespace std;

struct account
{
	string id;
	string password;
	account(string i, string p) {id = i, password = p;}
};

int main() {
	int n;
	cin>>n;
	vector<account> result;

	string id,password;
	for (int i = 0; i < n; i++) {
		cin>>id>>password;
		if (password.find_first_of('0') == string::npos &&
			password.find_first_of('O') == string::npos &&
			password.find_first_of('1') == string::npos &&
			password.find_first_of('l') == string::npos)
			continue;
		for (int s = 0; s < password.size(); s++) {
			if (password[s] == '0') password[s] = '%';
			if (password[s] == 'O') password[s] = 'o';
			if (password[s] == '1') password[s] = '@';
			if (password[s] == 'l') password[s] = 'L';
		}
		result.push_back(account(id,password));
	}

	int result_len = result.size();
	if (n == 1 && result_len == 0) {
		cout<<"There is 1 account and no account is modified";
	} else if (result_len == 0) {
		cout<<"There are "<<n<<" accounts and no account is modified";
	} else {
		cout<<result_len;
		for (int i = 0; i < result_len; i++) {
			cout<<"\n"<<result[i].id<<' '<<result[i].password;
		}
	}

	return 0;
}
```





## 1036 Boys vs Girls (25)

**题目描述**

> This time you are asked to tell the difference between the lowest grade of all the male students and the highest grade of all the female students.

**输入描述**

> Each input file contains one test case.  Each case contains a positive integer N, followed by N lines of student information.  Each line contains a student's name, gender, ID and grade, separated by a space, where name and ID are strings of no more than 10 characters with no space, gender is either F (female) or M (male), and grade is an integer between 0 and 100.  It is guaranteed that all the grades are distinct.

**输出描述**

> For each test case, output in 3 lines.  The first line gives the name and ID of the female student with the highest grade, and the second line gives that of the male student with the lowest grade.  The third line gives the difference gradeF-gradeM.  If one such kind of student is missing, output "Absent" in the corresponding line, and output "NA" in the third line instead.

**输入例子**

> 3
> Joe M Math990112 89
> Mike M CS991301 100
> Mary F EE990830 95

**输出例子**

> Mary EE990830
> Joe Math990112
> 6

**实现代码**

```c++
#include <iostream>
#include <stdlib.h>
using namespace std;

int main() {
	int n;
	cin>>n;
	string re_male_name, re_male_id;
	string re_female_name, re_female_id;
	int re_male_grade = 101, re_female_grade = -1;

	string name, gender, id;
	int grade;
	while(n--) {
		cin>>name>>gender>>id>>grade;
		if (gender=="M") {
			if (grade < re_male_grade) {
				re_male_name = name;
				re_male_id = id;
				re_male_grade = grade;
			}
		} else {
			if (grade > re_female_grade) {
				re_female_name = name;
				re_female_id = id;
				re_female_grade = grade;
			}
		}
	}

	if (re_female_name=="") cout<<"Absent"<<endl;
	else cout<<re_female_name<<' '<<re_female_id<<endl;
	if (re_male_name=="") cout<<"Absent"<<endl;
	else cout<<re_male_name<<' '<<re_male_id<<endl;
	if (re_female_name=="" || re_male_name=="") cout<<"NA";
	else cout<<re_female_grade - re_male_grade;
	
	return 0;

}
```



## 1037 Magic Coupon (25)

**题目描述**

> The magic shop in Mars is offering some magic coupons.  Each coupon has an integer N printed on it, meaning that when you use this coupon with a product, you may get N times the value of that product back!  What is more, the shop also offers some bonus product for free.  However, if you apply a coupon with a positive N to this bonus product, you will have to pay the shop N times the value of the bonus product... but hey, magically, they have some coupons with negative N's! 
> For example, given a set of coupons {1 2 4 -1}, and a set of product values {7 6 -2 -3} (in Mars dollars M\$) where a negative value corresponds to a bonus product.  You can apply coupon 3 (with N being 4) to product 1 (with value M$7) to get M$28 back; coupon 2 to product 2 to get M$12 back; and coupon 4 to product 4 to get M$3 back.  On the other hand, if you apply coupon 3 to product 4, you will have to pay M\$12 to the shop.
> Each coupon and each product may be selected at most once.  Your task is to get as much money back as possible.

**输入描述**

> Each input file contains one test case.  For each case, the first line contains the number of coupons NC, followed by a line with NC coupon integers.  Then the next line contains the number of products NP, followed by a line with NP product values.  Here 1<= NC, NP <= 105, and it is guaranteed that all the numbers will not exceed 230.

**输出描述**

> For each test case, simply print in a line the maximum amount of money you can get back.

**输入例子**

> 4
> 1 2 4 -1
> 4
> 7 6 -2 -3

**输出例子**

> 43

**实现代码**





## 1054 The Dominant Color (20)

**题目描述**

> Behind the scenes in the computer's memory, color is always talked about as a series of 24 bits of information for each pixel.  In an image, the color with the largest proportional area is called the dominant color.  **A *strictly* dominant color takes more than half of the total area.**  Now given an image of resolution M by N (for example, 800x600), you are supposed to point out the strictly dominant color.

**输入描述**

> Each input file contains one test case.  For each case, the first line contains 2 positive numbers: M (<=800) and N (<=600) which are the resolutions of the image.  Then N lines follow, each contains M digital colors in the range [0, 224).  It is guaranteed that the strictly dominant color exists for each input image.  All the numbers in a line are separated by a space.

**输出描述**

> For each test case, simply print the dominant color in a line.

**输入例子**

> 5 3
> 0 0 255 16777215 24
> 24 24 0 0 24
> 24 0 24 24 24

**输出例子**

>  24

首先要注意到dominant color是超过半数的，刚开始只想到了排序后最中间的数一定为结果，但是要存储的数据太多，后来看了[参考链接](https://blog.csdn.net/zhu_liangwei/article/details/9734671)，学会了下边这个方法。

**实现代码**

```c++
#include <iostream>
using namespace std;

int main() {
    int m, n, all_count, tmp, re = -1, count = 0;
    cin>>m>>n;
    all_count = m*n;
    while (all_count--) {
        cin>>tmp;
        if (count == 0) re = tmp;
        if (re == tmp) count++;
        else count--;
    }
    cout<<re;
    return 0;
}
```

## 1081 Rational Sum (20)

**题目描述**

> Given N rational numbers in the form "numerator/denominator", you are supposed to calculate their sum.

**输入描述**

> Each input file contains one test case. Each case starts with a positive integer N (<=100), followed in the next line N rational numbers "a1/b1 a2/b2 ..." where all the numerators and denominators are in the range of "long int".  If there is a negative number, then the sign must appear in front of the numerator.

**输出描述**

> For each test case, output the sum in the simplest form "integer numerator/denominator" where "integer" is the integer part of the sum, "numerator" < "denominator", and the numerator and the denominator have no common factor.  You must output only the fractional part if the integer part is 0.

**输入例子**

> 5
> 2/5 4/15 1/30 -2/60 8/3

**输出例子**

> 3 1/3

**实现代码**

```c++
#include <iostream>
#include <stdlib.h>
using namespace std;

long long gcd(long long a, long long b) {
    return ((b==0)?abs(a):gcd(b,a%b));
}

int main() {
    long long n, tmp_gcd;
    cin>>n;
    char c;
    long long re_integer = 0, re_numerator, re_denominator;
    cin>>re_numerator>>c>>re_denominator;
    tmp_gcd = gcd(re_numerator,re_denominator);
    re_numerator/=tmp_gcd,re_denominator/=tmp_gcd;
    re_integer += int(re_numerator/re_denominator);
    re_numerator%=re_denominator;
    while(--n) {
        long long tmp_nu, tmp_de, tmp_nu_re, tmp_de_re;
        cin>>tmp_nu>>c>>tmp_de;
        tmp_de_re = tmp_de * re_denominator;
        tmp_nu_re = tmp_nu * re_denominator + tmp_de * re_numerator;
        tmp_gcd = gcd(tmp_de_re,tmp_nu_re);
        tmp_de_re/=tmp_gcd,tmp_nu_re/=tmp_gcd;
        re_integer += int(tmp_nu_re/tmp_de_re);
        re_numerator = tmp_nu_re % tmp_de_re;
        re_denominator = tmp_de_re;
    }
    if (re_integer == 0 && re_numerator == 0) cout<<0;
    else if (re_integer == 0) {
    	cout<<re_numerator<<'/'<<re_denominator;
	} else if (re_numerator == 0) {
		cout<<re_integer;
	} else {
		cout<<re_integer<<' '<<re_numerator<<'/'<<re_denominator;
	}
    return 0;
}
```



## 1082 Read Number in Chinese (25)

**题目描述**

> Given an integer with no more than 9 digits, you are supposed to read it in the traditional Chinese way.  Output "Fu" first if it is negative.  For example, -123456789 is read as "Fu yi Yi er Qian san Bai si Shi wu Wan liu Qian qi Bai ba Shi jiu".  Note: zero ("ling") must be handled correctly according to the Chinese tradition.  For example, 100800 is "yi Shi Wan ling ba Bai".

**输入描述**

> Each input file contains one test case, which gives an integer with no more than 9 digits.

**输出描述**

> For each test case, print in a line the Chinese way of reading the number.  The characters are separated by a space and there must be no extra space at the end of the line.

**输入例子**

> -123456789

**输出例子**

> Fu yi Yi er Qian san Bai si Shi wu Wan liu Qian qi Bai ba Shi jiu

**实现代码**

首先按照数字单位的顺序添加到结果容器中，然后遍历结果容器，如果有连续重复的"ling"只留一个，如果"Wan"前有“ling”去掉0，如果"Wan"直接跟在“Yi”后边将“Wan”换成“ling”。

```c++
#include <iostream>
#include <stdlib.h>
#include <math.h>
#include <vector>
#include <string>
using namespace std;

int main() {
	string units[] = {"","Shi","Bai","Qian","Wan","Shi","Bai","Qian","Yi"};
	string digits[] = {"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};
	vector<string> re;
	int n;
	cin>>n;
	if (n < 0) {
		re.push_back("Fu");
		n = -n;
	}
	if (n == 0) {
		cout<<"ling";
		return 0;
	}
	vector<int> n_digits;
	while (n > 0) {
		n_digits.push_back(n%10);
		n/=10;
	}
	int len = n_digits.size();
	for (int i = len-1; i >= 0; i--) {
		re.push_back(digits[n_digits[i]]);
		if (i==4 || n_digits[i] != 0 && i>0) re.push_back(units[i]);
	}
	vector<string>::iterator iter = re.begin()+1;
	while (iter < re.end()) {
		if (*(iter)=="ling" && *(iter-1)=="ling")
			re.erase(iter);
		else if (*(iter)=="Wan" && *(iter-1)=="ling")
			re.erase((iter--)-1);
		else if (*(iter)=="Wan" && *(iter-1)=="Yi")
			*(iter++) = "ling";
		else
			iter++;
	}
	if (re.size()>1 && re[re.size()-1]=="ling") re.erase(re.end()-1);
	cout<<re[0];
	for (int i = 1; i < re.size(); i++) cout<<' '<<re[i];
		
	return 0;
}

```



## 1083 List Grades (25)

**题目描述**

> Given a list of N student records with name, ID and grade.  You are supposed to sort the records with respect to the grade in non-increasing order, and output those student records of which the grades are in a given interval.

**输入描述**

> Each input file contains one test case.  Each case is given in the following format:
> N
> name[1] ID[1] grade[1]
> name[2] ID[2] grade[2]
> ... ...
> name[N] ID[N] grade[N]
> grade1 grade2
>
> where name[i] and ID[i] are strings of no more than 10 characters with no space, grade[i] is an integer in [0, 100], grade1 and grade2 are the boundaries of the grade's interval.  It is guaranteed that all the grades are *distinct*.

**输出描述**

> For each test case you should output the student records of which the grades are in the given interval [grade1, grade2] and are in non-increasing order.  Each student record occupies a line with the student's name and ID, separated by one space.  If there is no student's grade in that interval, output "NONE" instead.

**输入例子**

> 4
> Tom CS000001 59
> Joe Math990112 89
> Mike CS991301 100
> Mary EE990830 95
> 60 100

**输出例子**

> Mike CS991301
> Mary EE990830
> Joe Math990112

**实现代码**

```c++
#include <iostream>
#include <stdlib.h>
#include <vector>
using namespace std;


struct student
{
	string name;
	string id;
	int grade;
	student(string n,string i, int g) {
		name = n;
		id = i;
		grade = g;
	}
};

int main() {
	int n;
	cin>>n;
	vector<student> re;

	string name,id;
	int grade;
	while(n--) {
		cin>>name>>id>>grade;
		student tmp(name,id,grade);
		re.push_back(tmp);
	}
	int min_grade,max_grade;
	cin>>min_grade>>max_grade;

	vector<student>::iterator iter = re.begin();
	while (iter != re.end()) {
		if ((*iter).grade < min_grade || (*iter).grade > max_grade)
			re.erase(iter);
		else
			iter++;
	}

	for (int k = 0 ; k < re.size(); k++) {
		for (int i = 0 ; i < re.size()-1; i++) {
			if (re[i].grade < re[i+1].grade) {
				student tmp(re[i].name,re[i].id,re[i].grade);
				re[i] = re[i+1];
				re[i+1] = tmp;
			}
		}
	}

	if (re.empty()) cout<<"NONE";
	else {
		cout<<re[0].name<<' '<<re[0].id;
		for (int i = 1; i < re.size(); i++)
			cout<<"\n"<<re[i].name<<' '<<re[i].id;
	}

	return 0;
}
```



## 1086 Tree Traversals Again (25)

**题目描述**

> An inorder binary tree traversal can be implemented in a non-recursive way with a stack.  For example, suppose that when a 6-node binary tree (with the keys numbered from 1 to 6) is traversed, the stack operations are: push(1); push(2); push(3); pop(); pop(); push(4); pop(); pop(); push(5); push(6); pop(); pop().  Then a unique binary tree (shown in Figure 1) can be generated from this sequence of operations.  Your task is to give the postorder traversal sequence of this tree.
>
> ![](/images/pat_1086.jpg)

**输入描述**

> Each input file contains one test case.  For each case, the first line contains a positive integer N (<=30) which is the total number of nodes in a tree (and hence the nodes are numbered from 1 to N).  Then 2N lines follow, each describes a stack operation in the format: "Push X" where X is the index of the node being pushed onto the stack; or "Pop" meaning to pop one node from the stack.

**输出描述**

> For each test case, print the postorder traversal sequence of the corresponding tree in one line.  A solution is guaranteed to exist.  All the numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

**输入例子**

> 6
> Push 1
> Push 2
> Push 3
> Pop
> Pop
> Push 4
> Pop
> Pop
> Push 5
> Push 6
> Pop
> Pop

**输出例子**

> 3 4 2 6 5 1

**实现代码**

以上述例子为例，按顺序排下来123456为前序，使用栈的pop顺序为中序，利用前序和中序可以得到后序。

注意1-N为标号，每次push的为值，值有可能重复，但是push顺序为标号1-N。

可以用下边这个测试用例测试：

> //输入
>
> 19
> Push 4
> Push 11
> Push 7
> Push 12
> Pop
> Pop
> Pop
> Push 14
> Push 17
> Pop
> Pop
> Push 6
> Push 18
> Pop
> Push 8
> Pop
> Pop
> Push 4
> Pop
> Pop
> Push 11
> Push 16
> Push 11
> Push 12
> Pop
> Push 2
> Pop
> Pop
> Pop
> Push 7
> Push 4
> Pop
> Pop
> Push 12
> Pop
> Pop
> Push 11
> Pop
>
> // 输出
>
> 12 7 17 8 18 4 6 14 11 2 12 11 4 12 7 16 11 11 4 

我的代码如下：

```c++
#include <iostream>
#include <stdlib.h>
#include <math.h>
#include <vector>
#include <string>
#include <stack>
using namespace std;

vector<int> post;
void generatePostOrder(vector<int> pre, vector<int> in, int len) {
	if (len <= 0) return;
	if (len == 1) {
		post.push_back(pre[0]);
		return;
	}
	int root_num = pre[0], root_pos_of_in = 0;
	for (; root_pos_of_in < len; root_pos_of_in++) {
		if (in[root_pos_of_in] == root_num) break;
	}
	int left_len = root_pos_of_in;
	int right_len = len - left_len - 1;
	vector<int> pre_left,pre_right,in_left,in_right;
	pre_left.assign(pre.begin()+1, pre.begin()+1+left_len);
	pre_right.assign(pre.begin()+1+left_len, pre.end());
	in_left.assign(in.begin(),in.begin()+left_len);
	in_right.assign(in.begin()+left_len+1, in.end());
	generatePostOrder(pre_left,in_left,left_len);
	generatePostOrder(pre_right,in_right,right_len);
	post.push_back(root_num);
}

int main() {
	int n, tmp;
	cin>>n;
	string op;
	vector<int> pre,in,value;
	stack<int> tmp_stack;
	int index = 0;
	for (int i = 0; i < 2*n; i++) {
		cin>>op;
		if (op == "Push") {
			cin>>tmp;
			value.push_back(tmp);
			tmp_stack.push(index);
			pre.push_back(index++);
		} else {
			in.push_back(tmp_stack.top());
			tmp_stack.pop();
		}
	}
	generatePostOrder(pre,in,n);
	if (n > 0) cout<<value[post[0]];
	for (int i = 1; i < n; i++) {
		cout<<" "<<value[post[i]];
	}
	return 0;
}

```







## 10xx

**题目描述**

> 

**输入描述**

> 

**输出描述**

> 

**输入例子**

> 

**输出例子**

> 

**实现代码**

