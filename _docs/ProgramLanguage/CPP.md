---
title: C++
permalink: /docs/PL1/
---

##### Table of Contents  



```c++

#include <iostream>

using namespace std;

void doSomething(int x)
{
	//x = 123;
	cout << x <<"\t  "<< &x<< endl;
}

int main()
{
	int x = 10;

	cout << x << "\t  " << &x << endl;
	doSomething(x);
	cout << x << "\t  " << &x << endl;

	return 0;
```
