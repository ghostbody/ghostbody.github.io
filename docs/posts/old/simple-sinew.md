

## Overview

We are going to do a project base on a dissertation named "Sinew" for a deeper understanding of mordern database system.

link of the dissertation: [sinew](http://dl.acm.org/citation.cfm?id=2612183&CFID=668626395&CFTOKEN=42884176)

<br>

## Team Members

Lingli Wu

Hang Xu

Li Xu

Jaiqi Ye

Tingting Zhang

<br>

## Description

### Destination

Simplely complete the two function modules Catalog and Serializer.

### Requirement

**Programming language**: C/C++ only and when using c++, STL is not allowed.

**System**: Only can use Linux with gcc/g++/gdb and use makefile for batch compilation.

### Problem

1. complete command "insert file" and Json date format is used for the original data file.
2. complete command "checkout catalog" which will print all the information in the catalog, here we simplify.
3. complete command "find A = B" which will print the found result in the format of json. If there is no result, print None.

### Experimental Data

We use nobench to generate the test data we need.

### Implementation requirements

1. for each json data record, translate it into the format describing in the dissertation "sinew" as the following:

![image](https://cloud.githubusercontent.com/assets/8371330/13001555/001462d8-d1a2-11e5-8124-56a2622447c3.png)

notice that you should store all of the data in binary format.

2. you should use "fwrite/fread" in C to read or write data. Every time you are going to have a file operation, you need to check the buffer space and you can only have the operation when it reach its full size, 8KB. No padding is expected in the data file.

3. all int type data is required to be 32 bit.

### Optimization

build a file index to reduce the time cost for querying.you can also use binary chop algorithm to speed up

<br>

## Solution

### Tools
- Programming language: C (pure C)
- RAM: 4GB
- OS: Ubuntu Kylin 14.10 amd64
- Compile Tool: gcc (Ubuntu 4.9.2-10ubuntu13) 4.9.2 & Makefile
- Main Test Script：Shell
- Test data：Nobench data generator
- Project Address：[git](https://www.github.com/ghostbody/sinew)
- Project Train of Thoughttrain of Thought

The whole experiment is divided into four parts, in the main function of the use of three main functions:

![image](https://cloud.githubusercontent.com/assets/8371330/13001596/5acfdd1a-d1a2-11e5-80b8-49b708718185.png)

### Catalog

According to the paper's tips to achieve the requirements of the Catalog. Corresponding to achieve Catalog update, find, output


### Insert

To achieve the serialization of data stored in the file, and call the CATALOG function update. Analysis (no use of third party tools, all of their own) to implement Json data, and the serialization of the data, and in a binary manner.

### Find

To achieve the search of data, the establishment of file index, the use of two points to find (aid in sequential storage), as well as the anti serialization (the data will be re - re Json data, and then output)

### Design Process

#### 1. Catalog
The data structure of the Catalog unit is as follow:

![image](https://cloud.githubusercontent.com/assets/8371330/13001618/7c73d296-d1a2-11e5-950c-479e9c374e65.png)

We use a method “list and array” to store it. The list is extendable and we can insert unit easily. Once we finish inserting, we will build an array to store all addresses of the array to form a fast visit index for the list.

![image](https://cloud.githubusercontent.com/assets/8371330/13001599/61ac5e74-d1a2-11e5-9967-6cea24b724dc.png)

After this data structure is established, we can use the catalog index to search the catalog data, we provide two functions:

FindByKeyName: the corresponding data element of catalog is searched by the key name and the type of key.

FindById: Use ids to find the corresponding data record in the catalog index.

![image](https://cloud.githubusercontent.com/assets/8371330/13001634/99c7b3c6-d1a2-11e5-84c8-26d85d94ed28.png)


In file (disk), catalog is stored in a format text:

(attrid, keyname, count, keytype)

![image](https://cloud.githubusercontent.com/assets/8371330/13001635/9d7ac3e6-d1a2-11e5-8678-0e24eebd6224.png)


#### 2.Json Analysis
Json stores data format as a string array. We define the structure, including the start pointer, end pointer, JSON data type (used to determine the read of the string as the key or value) and data type (used to judge the value of the type, including: int, bool, string, array, nested and other types). Using pointers to read JSON key or value, start pointer and the end pointer representing its starting position in the string array and end position, first read for the key and the second read the value, and so on (as shown in the figure). Judgment value of the first character in a text string, if the character is digital, the value of data type is int; if the character "t" or "F", the string bool type; if the quotation marks "," ", the string is the string; if the symbol" [", the string is an array type; if the symbol" {", the string as the nested; otherwise for unknown type.

![image](https://cloud.githubusercontent.com/assets/8371330/13001637/a4b431a6-d1a2-11e5-98fc-5379d78cf15d.png)

![image](https://cloud.githubusercontent.com/assets/8371330/13001639/ac3051ee-d1a2-11e5-9517-8bc4179d3368.png)


#### 3.Serialize

We define the following functions:

![image](https://cloud.githubusercontent.com/assets/8371330/13001642/b13e7f76-d1a2-11e5-9ce7-103f80d27baa.png)

To JSON data serialization, we must first implement the main function in the test_JsonSerializer function. The format of a tuple is as follows:

![image](https://cloud.githubusercontent.com/assets/8371330/13001647/b796426e-d1a2-11e5-921b-e0b84fe0171b.png)

When we have a look at the JSON data, we find that the data are int32, bool, array, and nested objects. Take the following JSON data as an example to illustrate the idea of serialization:

{"dyn2": 50378, “dyn2”:"ABC", "sparse_787": "GBRDCMBQGE======"}

At first, each tuple table is used to record the position of the whole table in the entire table, as shown in the above table.

And on behalf of the table in the #attributes_num is attribute in each tuple is the number and implement to example is each JSON data in key value pairs of a number; aid0~aid of (n-1) corresponding to the is each a key; and offset that is, from the tuple start to read the key value pairs according to the offset. The JSON data as an example, serialization, the tuple first position stored tuple ID, the second position stored key on the number. The key on the number 3, aid0~aid2 accounted for to the three to five positions; and because each area corresponding to an offset read the corresponding data, so offs0~offs2 also accounted for three position; and the ninth position is used to store all the data for the total length of len; then began to read data, so offs0 equal to 9. In this example, the first value is int, which takes 4 bytes, so offs1=9+4=13; similarly, the bool type data is treated as a int type; while the second value is a string, each character is recorded in the corresponding ASCII code, corresponding to a byte; so offs2=13+3=16.

And so on, and when we encounter an array or object type, compared to simply put them as a string, we have a different approach. When we encounter these two kinds of data types, we will build a new record, and then use the recursive method to resolve it. For nested object type, the tuple_id data from the JSON data in the whole table is returned as the data stored in the main table of the table, and we will put the number of attributes to -1, to distinguish the other value types, to facilitate the identification of the reverse sequence.

We have the following data structure:

![image](https://cloud.githubusercontent.com/assets/8371330/13001649/c92616c6-d1a2-11e5-9835-0b486d165dc8.png)

#### 4. Find

First, we establish the index of the entire file, the structure of the file index is shown in the following diagram:

![image](https://cloud.githubusercontent.com/assets/8371330/13001652/d2ccbc48-d1a2-11e5-8ad8-8580941f5203.png)

For each query, we first match the attribute, now with the string form of attribute in the catolog to find the corresponding attrid, with the attr_id traversal file records in a tuple. In traversing each of the time, use two points to find, find the file index in the array of attr_ids, find the record to the result, can not find it.
Finally get a result list, which records the results of the search Attrid.

Then match the value and match the specific values in the corresponding file record. Before the comparison, the record is first recorded, and then the two string matching operation. If you do not match then the tag match is false, and the match is found.

Then, for each record in the match, if result is true, we will be materialized, and output.

The process of physical and chemical process is to find the corresponding data from the file index, and then use Key_type to determine the data type:

For the int and bool types, read directly from the file record in the int

For string, offset, a data record is read and then, two offset subtraction, to the length of the string. Read these characters.

For nested array type, read ID array, and then find the record in the entire file record array, call the array function of the, to return a result string.

For nested JSON types, read the ID JSON, and then recursively call this JSON's physical function, to return to the JSON string of the sub object

Finally get the original JSON record of the string, the output of.

![image](https://cloud.githubusercontent.com/assets/8371330/13001656/dc6516c4-d1a2-11e5-97f5-c303f9213ad0.png)


#### 5.Buffer
![image](https://cloud.githubusercontent.com/assets/8371330/13001659/e0cad604-d1a2-11e5-839e-9850e1fa67a0.png)


### Test

#### Test Plan

Test is divided into two parts, the main project of the test and the function of each module function of small unit logic test.

The main project tests include:

1. the main project import test import (insert)
2. CHECK CATALOG test (catalog checkout) of the main project
3. the main item search and find time test (find)

![image](https://cloud.githubusercontent.com/assets/8371330/13001667/f31073be-d1a2-11e5-9ecc-ffb76f685732.png)


#### Project Test

The Import of Main Project using 100K data

![image](https://cloud.githubusercontent.com/assets/8371330/13001669/f7b23236-d1a2-11e5-9763-1d71453d1ca8.png)

![image](https://cloud.githubusercontent.com/assets/8371330/13001672/faa62cb8-d1a2-11e5-98e1-bf7cfd02d659.png)

Catalog Checkout

![image](https://cloud.githubusercontent.com/assets/8371330/13001678/01bf9f52-d1a3-11e5-9528-bb2a44e3d64c.png)

![image](https://cloud.githubusercontent.com/assets/8371330/13001679/03afdf70-d1a3-11e5-97cf-469322fa06e1.png)


Catalog size : 1013

Find Test

Script:

![image](https://cloud.githubusercontent.com/assets/8371330/13001683/0a46004e-d1a3-11e5-9771-a675c41b9fdc.png)

Result:

![image](https://cloud.githubusercontent.com/assets/8371330/13001686/114afe4e-d1a3-11e5-84b4-413b84ba0b0c.png)


## Conclusion

We have learn a lot form this project, especially at C programming, organizing such a project. Tanks for the hardwork of our team. And you can go to github to download the source code for detail. If you, reader have any question, you can email me or just leave your message at message wall in this website.
