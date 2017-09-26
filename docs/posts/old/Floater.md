
## Overview

Floater is a online chatting tool for strangers. The user can simply input a username and the server will find a fellow for the user to chat with in peer to peer mode. It can also transfer pictures and files.

<br>

## Team Members

Jiaqi Ye (Leader)

Jiawei Long

Qinglong Xu

Junjie Peng

<br>

## Protocol Design

## Abstract
Protocol Floater v0.5 use Json as the switching data type between peers.
There two types of protocols, client-server and client-client(know as peer to peer).

### 1. Server and Client
The server is only responsible for getting a fellow to chat with.
#### Packet switching type

<pre>
{"type":"Request"}
{"type":"Reply"}
</pre>

#### Packet actions
<pre>
{"action":"login"}
</pre>

When the client send a login action to the server, it will send its user name as well. Then the server will insert a record to the user list.

<pre>
{
  "uid" : 0,
  "username": "admin",
  "ip": "192.168.1.1",
}
</pre>

Notice that, the user in the user list means that it has not found a fellow to chat.

<pre>
{"action":"find"}
</pre>

### 2. Client and Client
When the address get a fellow from the server, it know the fellow's ip address. Then the client will send a message to the fellow's client.
The client can be treated a server.

This is an example:
<pre>
{
  "type":"Request",
  "username":"Bob",
  "action" : "hello",
  "ip":"192.168.1.1"
}
</pre>

<pre>
{
  "type":"Reply",
  "username":"Alice",
  "action":"hi",
  "ip":"192.168.1.2"
}
</pre>

Alice can also reject the connection.

<pre>
{
  "type":"Reply",
  "username":"Unknown",
  "action":"reject",
  "ip":"192.168.1.2"
}
</pre>

If Alice accept the connection:
Both Alice and Bob will start two threads, for sending message and receiving message.

<pre>
{
  "type":"Chat",
  "username":"Bob",
  "ip":"192.168.1.1",
  "message":"It's a nice day, isn't it?"
}
</pre>

Then Alice can reply
<pre>
{
  "type":"Chat",
  "username":"Bob",
  "ip":"192.168.1.1",
  "message":"Yes, and I feel so warm."
}
</pre>

At any time, the two people can leave and break up the conversation.
<pre>
{
  "type":"Bye",
  "username":"Bob",
  "ip":"192.168.1.1",
}
</pre>

<br>

### Conclusion

This phase is going to carry out the design of our protocol. We transmit the string in JSON format, and define the format of sending and receiving information, and define the semantic action of message response. (modeled on the HTTP protocol) to achieve their own application layer protocol.

Use this phase to the JSON format protocol is very popular in today's data transmission format (without the use of XML, the format too ugly). Our thinking is that such a transfer did not do good serialization, which leads to a large number of redundant symbols in the process of transmission, so we in image transmission, re design the special picture transfer protocol, using binary direct transmission information is true, do completely serialization to remove redundant role.

At the beginning of the design of the protocol is not standard, there are a lot of mistakes should not spend time, resulting in a lot of technical debt, but also to the team caused a lot of work. So the standard of the protocol should be insisted in the initial stage.


## Technology Discussion

### 1. Socket Programming

Using python socket programming is very simple.

Class `Socket`:
`socket(family,type[,protocal])`

**to create a TCP socket:**

      s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)

**to create a UDP socket**

      s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)

We need reliable data transfer. So we choose TCP as our transport layer protocol. UDP is also available (QQ uses UDP).

we can send and receive message using this way:

      sock.send('{"action":"login","uid":0,"username":"%s","ip":"%s"}' %(username, server))

      data = connection.recv(1024)

python socket can send byte Strings to remote which will help us to complete this project.

### 2. Client-Server and Peer to Peer Structure Implement

<img class="img-responsive"  src="https://github.com/ghostbody/computer-network/raw/master/doc/protocol.png?raw=true">

We are going to implement the two structure. C/S structure is the simple one and the peer to peer structure is the difficult one.

**C/S**

we run different protocol on client and server.

On the server side, we have a simple "database"(a array in the main memory in fact). Client will connect to the server forwardly. And server will accept client's request and then write the client's information into the database while the server will help the client to find fellow to chat with. If the fellow is found, a record which is information of the founded fellow will be sent to the requesting client as a string. With the information, the requesting client can send another request to the fellow to set up connection and then start to chat.

**Peer to Peer**

P2P, in fact, can be regarded as C/S for both side which means that peer 1 is another peer's (known as peer 2) server and peer 2 is peer 1's client while peer 2 roles also peer 1's server. We can see it clearly when we look at the picture above.

We run 2 protocols in our p2p structure. One is for chatting and another is for picture transferring. For chatting, we send message in a time. And for picture transferring, we send picture's format(.jpg, .png, .gif). And then we send the picture's byte stream to remote. So we are just send the file to remote.


### 3. OOP and Multi-Threading Programming

When we are going to run the protocols, we will have some "block code area" like:

      while ture:
          # wait to receive message

So we need to use Multi-Threading programming as a solution.

At the same time, we used OOP(object-oriented programming) to have a good development. We have classes post-office, post-man an so on.

We also use Queue( in python Multi-Threading Programming ) for sending and receiving.


### 4. Fast GUI Development

Today's era is the era of web, the use of WebKit rapid development of desktop applications are very common and technical development is difficult, the application of light. Many modern applications such as atom are developed using webkit.

This time we use the js+python cross language approach, the use of html+jquery+css development front, the use of Python language object in the JS, using JS to call the python program function. This way we achieve a very beautiful HTML5 interface and dynamic effects.


## View our project in github
[https://github.com/ghostbody/computer-network](https://github.com/ghostbody/computer-network)
