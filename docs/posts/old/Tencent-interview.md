I am very luck to have two chance for a front-end interview and a back-end interview.
## Front-end interview

Question: Give you an "a" element, and the link is broken, how can you disable the link?
Answer: 
1. Write a js ajax script to test whether the link is good or not, then with a callback, you can disable it.
2. Deal with the request in back-end, redirect the page to other place.
3. Make another element over the a element or set its css to "display:none"
4. Using event listening, disable the basic page response message.

Question: How can you get elements from a document?
Answer:
1. Using getElementById().
2. Using JQuery, query string.
......

Question: Other methods?
Answer: Get the whole document, and then traversal it......

Question:........

Question: How to traversal it?
Answer: Using a stack, put the root node into it, and then loop until the stack is empty. For each iteration, get the top of the stack, and then push the child nodes of the node into the stack again.

Question: What kind of this traversal?
Answer: Deep first traversal.

Question: How can you implement breadth first traversal?
Answer: Using a queue rather than a stack.

Question: What's the 5 level of TCP/IP protocol?
Answer: Application, Transmission, Networking, data link layer and physical layer.

Question: What protocol does transmission layer have?
Answer: TCP and UDP

Question: Tell their difference?
Answer: TCP is an RDT (reliable data transmission) protocol and it provides traffic control and congestion avoidance. It's a linked based protocol, peers should linked to each other before data transport. HTTP, FTP base on TCP.
UDP is a connectionless protocol and it does not provide reliable data transmission or traffic control or  congestion avoidance. But it can get a big througoutput in practice. You should implement reliable data transmission if you need it in application layer if you use this protocol, for example QQ protocol. RDP, DHCP base on UDP.
## Back-end interview

Question: What language you are most familiar with?
Answer: C++

Question: What's the efficiency of the method push_back() in vector and list? Compare it.
Answer: In the condition that the storage of vector is not full, vector is faster than list. But in the condition that the storage is full, list is faster because vector need to allocate new storage and it's very slow.

Question: What's the value of "sizeof(object)" where object is an class instance?
Answer: If the class is empty or only with member functions, the value is 1. Otherwise the size should be the sum of the size of it's members and also memory alignment will be applied when you are calculating the size of a class.

Question: Does the order of the member variables matter the value of the sizeof(object)?
Answer: Nope.

Question: Give you a c string, say an char array 'char s1[100]' and a char point char \* s2 = "bacse", what's the value of sizeof() for them respectively?
Answer: sizeof(s1) is 100. And in 32 bits operating system s2 is 4, while s2 is 8 in 64 bits operation system.

Question: We have a condition that there is a class with so many member variables, how can you solve if I want the user of this class to create instance only in the heap rather than in the stack?
(我现在有一个类，里面有非常多的成员变量，我现在希望用户在实例化的时候不占用栈的空间，迫使他使用堆的内存，我应该怎么做？)
Answer: You can let the constructor of the class private as well as the assign operator like you are using a singleton and then provide a public function "static object \* getInstance()" for the user to get instance from the heap.
This answer is my answer, I think it for a long time when I am on my way back. It's a trick!!!!!
In fact, some of thr the objects are stored in the heap, they only store pointers in the stack!!!

Question: What is memory leak? How to solve it?
Answer: When you allocated memory from the heap using malloc or new, you forget to release it using delete or free, or you make a mistake when you have pointer operations, and a block of memory will be lost. This is memory leak.

Question: How to solve this problem? Or how to avoid this problem?
Answer: For single thread program, you need to pay attention when you using new or malloc. You should have paired operation , new with delete, malloc with free. Also you can use some software like valgrind to check you memory. Also you can overload new and delete operator to manage the memory. In multi-threading program, you should have some contract with the functions, specified the usage of each role. Never return a block of memory in a function. 

Question: What's the difference of fgets() in C standard library and read() system call in linux?
Answer: ........
I get it on my way back, fgets() is thread safe and read() is not (reader-writer problem in process). sad....

Question: What's inter process communication methods?
Answer: Message queue, shared Memory and ......socket.

Question: What's the disadvantage of using socket as communication methods?
Answer: It will pass lo(本地回环) and it will pass the TCP/IP stack. It will cause protocol overhead. And also it need to occupy some ports.

Question: How can you see all the occupying ports? (He means linux)
Answer: Using command "sudo nestat"

Question: Dose kernel thread allocate memory for malloc() when it uses system call?
Answer: ....... (MengBing)

Nope, you just need to switch to kernel mode and then allocate memory and then switch back to user 

Ok, your knowledge is just OK. C++ is good but operating system is poor.
