---
layout: post
title: "Huawei Internship Interview Questions Summary"
tagline: "interview"
description: "The experience of Huawei Internship"
category: experience
tags: [interview,c]
excerpt: "I went to the internship interview of Huawei and this is my summary. I think I almost answer the questions but still have a lot of knowledge to learn anyway"
---

Well, I think I am very lucky to have some simple technology questions this time.

## The first round interview

The interviewer is a kind man, and I think he is a c++ engineer.

**1. Tell the difference of the flowing C statement.**
(1) char * const p
(2) char const * p
(3) const char * p

Answer:
(1) const pointer p indicate the pointer can't point to other char after definition.
(2) and (3) indicates the string is a const string which means that the content is read only.

Summary: Observe that which is nearer to p, * or const. If * is nearer, than this is the latter. Otherwise, the former.

**2. Tell the what's wrong with the following c program:**

    int countAs(char s[]) {
       int length = sizeof(s) / sizeof(char);
       int i, result = 0;
       for(i = 0; i < length; i++) {
         if(s[i] == 'A' || s[i] == 'a') {
           result++;
         }
       }
      return result;
    }

Answer: sizeof(s) does not work, because you lost the length of the array in C when you passing it as an parameter. It becomes a pointer. sizeof(s) is a unchanged value.

Question: What is the value?

Answer: In 32 bits operation system, sizeof(s) is 4 while in 64 bits operation system, 8.

Question: How can you fix it?

Answer: pass one more argument indicates the length of the array.

Question: And other methods?

Answer: Using strlen() function to calculate the length of the string.

Question: What should you pay attention to when you use strlen()?

Answer: Well, you should check NULL pointers when the parameters is passed in and also there should be a '\0' char at the end of the string.

Question: And other methods?

Answer: It's unnecessary to calculate the length of the string. You can just using the '\0' char to know the end of the string when you write for loop.

Question: What other problem about the function?

Answer: The argument does not have side effect in this case, it should be made const. Also the return type int may be overflow in the case the string is too long. And the name of the function is not good in some way, make it "count_letter_a".

Interviewer: Your level of knowledge is OK. Talk about one of your project experience......

And then I talk about my experience about the online judge system Matrix I am working for now.

## The second round interview

The interviewer is a project manager.

Question: Do you know what is Cyclomatic Complexity in code?

Answer: Meng Biing......

Well I do not know what is that, but I get back and learn someting more.

> Cyclomatic complexity is a software metric (measurement), used to indicate the complexity of a program. It is a quantitative measure of the number of linearly independent paths through a program's source code. It was developed by Thomas J. McCabe, Sr. in 1976.

> Cyclomatic complexity is computed using the control flow graph of the program: the nodes of the graph correspond to indivisible groups of commands of a program, and a directed edge connects two nodes if the second command might be executed immediately after the first command. Cyclomatic complexity may also be applied to individual functions, modules, methods or classes within a program.

> One testing strategy, called basis path testing by McCabe who first proposed it, is to test each linearly independent path through the program; in this case, the number of test cases will equal the cyclomatic complexity of the program.[1]

*From Wikipedia*

I know it now.

The definition is:

$$
M = E − N + 2P
$$

Where:

E = the number of edges of the graph.

N = the number of nodes of the graph.

P = the number of connected components.


Question:

What qualities should a programmer have?

Answer:

The most important is that a programmer should be familiar with a programming language, c/c++ is better. Secondly, basic knowledge of CS including operation system, database system, computer architecture, data structure and algorithm, computer network and compile theory should be acquired by him. A programmer should always be in touched withe his teammates which means communication and idea is also very important.

Ok, the interview result will sent to you in five days.  
