---
layout: post
title: "Tencent Technology Test Summary"
tagline: "Technology Test"
description: "Tencent Technology Test Summary"
category: experience
tags: [test,c++,js,operating system,database,computer network]
excerpt: "I participated in Tencent Technology Test, and the questions covers most of the knowledge of CS including programming language, operating system, database, computer network, data structure and algorithm and computer architecture"
---

I choose some of the questions here with respect to my memory.

## Question 1: Which of the operation of memory management is not correct?

A.
  int * p = new int[10];
  delete p;

B.
  int * p = new int(0);
  free(p);

C.
  int * p = static_cast<int * >(malloc(sizeof(int)));
  delete p;

Well, in my opinion, all of the three choices above is correct if you want to.
Someone will doubt about the first one, why not delete[] p for an array?
Because for c++ built-in types, delete and delete[] is the same.

For the second and third one, the same.
Operator new complete two thing, make a system call and the call the constructor of the object. Because c++ built-in types can accept the condition in which the constructor is not called. It's the same when you use delete operator.

## Question 2: The length of Longest Palindrome Substring

Well, this is my blog and also the answer:

![The length of Longest Palindrome Substring ](http://my.oschina.net/yejq08/blog/415675)

## Question 3:

Description:

print the squar with n = 4;

    1   2  3 4
    12 13 14 5
    11 16 15 6
    10  9  8 7

with n = 3：

    1 2 3
    8 9 4
    7 6 5

Solution:

    #include <stdio.h>

    int main() {
        int a[101][101], n, b, c, d, e;
        scanf("%d", &n);
        if (n % 2 == 0) {
            for (e = 1, d = 1; e <= n/2; e++) {
                for (b = e, c = e; c <= n-e+1; c++, d++)
                    a[b][c] = d;

                for (c = c-1, b = e+1; b <= n-e+1; b++, d++)
                    a[b][c] = d;

                for (b = b-1, c = c-1; c >= e; c--, d++)
                    a[b][c] = d;

                for (c = c+1, b = b-1; b >= e+1; b--, d++)
                    a[b][c] = d;
            }
        } else {
                for (e = 1, d = 1; e <= (n-1) /2; e++) {
                    for (b = e, c = e; c <= n-e+1; c++, d++)
                        a[b][c] = d;

                    for (c = c-1, b = e+1; b <= n-e+1; b++, d++)
                        a[b][c] = d;

                    for (b = b-1, c = c-1; c >= e; c--, d++)
                        a[b][c] = d;

                    for (c = c+1, b = b-1; b >= e+1; b--, d++)
                        a[b][c] = d;
                }
                a[(n+1) /2][(n+1) /2] = n*n;
            }

            for (b = 1; b <= n; b++) {
                for (c = 1; c <= n; c++) {
                    if (c == n)
                            printf("%d", a[b][c]);
                    else
                            printf("%d ", a[b][c]);
                }
            printf("\n");
            }
    }

# Qustion 4: Big integer multiply

input is two big positive integer

output the multiply answer
