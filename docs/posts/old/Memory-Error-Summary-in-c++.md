---
layout: post
title: "Memory Error Summary in C++"
tagline: "interview"
description: "Memory Error Summary in C++"
category: experience
tags: [c++]
excerpt: "Many students from class 2015 c++ complain about the error of memory when they are doing homework. I summarize some kinds of errors in the system which appear very often"
---


# A small summary for 15 C++ learning

##  What's the problem with my program? Why eden fetch me a lot of error information?

![image](https://cloud.githubusercontent.com/assets/8371330/14198764/73180642-f810-11e5-9c0f-15ed8cebb909.png)

Answer: Eden, the online judge system uses a memory check tool named "valgrind" for grading assignment. There are several kinds memory errors you will open meet.

**1. Memory Leak:**
The picture below show this error. It's your mistake that a block of memory become unreachable when you take pointer operations. This will only take place with heap allocation when you are setting a pointer to point to another block of memory while ignoring the legacy memory.

For example, in a list:
```cpp
head = head->next;
```

The memory block which was pointed by pointer head is definitely lost after the assign operation above.


**2. Invalid read or write of memory:**
This kind of mistake is very common because c++ compiler will not check invalid memory access. Mostly, you are accessing an element of an array of which the index is out of range.

![image](https://cloud.githubusercontent.com/assets/8371330/14203946/86513b52-f833-11e5-9cde-6e3296791411.png)

For example:
```cpp
int * a = new int[5]();
a[5] = 0;
```

a[5] is an invalid element of the array. And valgrind will throw "Invalid write of size 4.... Address 0x5202054 is 0 bytes after a block of size 20 alloc'd". This means that the address a+5 is not allocated by statement `int * a = new int[5]()`.
Also you will get runtime error in this case.

**3. Condition jump or move depends on uninitialized variables.**
This means that some of your conditional statements, i.e. for loop, if statement or etc. use uninitialized variables.

For example:

    int * a = new int[5]();
    if(a[0] == '0') {
      cout << "Bad Ta!" << endl;
    }

First of all, if the programmer forget to call `delete a`, it will cause a memory leak error.
Well, the if statement above uses a[0], which has not been initialized. Then valgrind will detect and report the error for you.

**4. Mismatched new/malloc and delete/free.**
As we all know, when you are writing c++ code using dynamic memory allocation, you are supposed to manage the heap memory by your self. New and delete are "paired operators" which means that when you call 3 of new, you should call 3 of delete after that.

For a mismatch example:(Tecent recruitment examination)

    int * a = new int[10];
    free(a);

This will cause a mismatched error in valgrind.
Why?Think about what's difference between new and malloc as well as delete and free? (hint: constructor and destructor)

 ## How can I fix these issues?

Basically, you should know the concepts in c++ programming, or you will know nothing when you meet these issues.

After that:

**Establishing good coding habits. A good coding style does not indicate that you pass the google style check in the system. This is not good enough.You should always be careful when you meet a pointer which is an annoying but powerful tool and you should remember to delete the pointer when you are using heap allocation to get memory.**

Be careful to these parts of a class:
1. default constructor in which you are supposes to initialize the member variables. Note that the compiler will automatically add an default constructor if you do not defined one.
2. copy constructor in which you should think about show copy and deep copy and also, you should remember to initialize the member variables because this is also a constructor.
3. assign operator which is similar to the constructors while you need to think about to clear the legacy content of "this" object. Also remember the condition that the parameter of the assign operator is the object itself, i.e. determine whether `this == &another` or not.  
Later, if I have time, I will choose some codes from your exercises submissions to put here for you to have a discussion "What is good style of coding?"
4. Using memory check tool.
We can use memory check tools like valgrind. In fact, it's a powerful memory check tool.
I remember valgrind can be simply installed in unbuntu using apt-get. You can referenced to this website for more details. Don't be annoyed when you see a lot of English words, please be patient to read it through.

[valgrind](http://valgrind.org/)

It's quite easy to use this tool, just compile you code and then run the program with a system argument which should be your executable program name.

```sh
valgrind ./a.out
```

or you can have more options, like to check full memory leak.

```sh
valgrind --leak-check=full ./a.out
```

or

```sh
valgrind -v ./a.out
```

check `valgrind -h` for more details.

you can see a lot of information after running the program.

**Be patient when you meet errors. This is a test that must be experienced before you become a supper programmer in c++.**
