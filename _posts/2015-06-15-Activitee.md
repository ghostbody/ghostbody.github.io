---
layout: post
title: "Activitee"
tagline: "A web information platform for Campus Activities"
description: "A web information platform for Campus Activities"
category: projects
tags: [nodejs,web,Mongodb]
excerpt: "Activitee is the Course Project of SE-386 Software Process Improvement. We have a long discuss and deep think about the need of this product. And it's actully a great one.Its main function is to provide a platform for organizers and paticipators to hold great activities by intergrating information and optimizing resourses."
---

## Overview

Activitee is the Course Project of SE-386 Software Process Improvement. We have a long discuss and deep think about the need of this product. And it's actully a great one.

Its main function is to provide a platform for organizers and paticipators to hold great activities by intergrating information and optimizing resourses.

<img class="img-responsive" src="https://cloud.githubusercontent.com/assets/8371330/13001965/b5071ea8-d1a5-11e5-996e-ce721de4dc13.png">


Team Members

- Zhuopeng Liu (Leader)
- Jiahao Kuang
- Xiao Tan(Designer)
- Yiwang Zheng
- Zaihua Lin
- Jiankun Liu
- Zeming Lin
- Jiaqi Ye
- Guowei Zhang

## Operational Concepts

**What is Activitee?**

This is a platform for information collection on campus activities. Users, can browse and publish information on campus activities through this platform. (the activities includes large, formal activities and some small activities that some students can get together for an interest or other things.

**Why are we going to design Activitee?**

There are two groups that are directly related to this Platform.

For participators, they have the following troubles:

#### Mess platforms
There are so many information platforms to get the activity information. However, a variety of platforms such as QQ group, microblog and wechat public platform make users overwhelmed

#### Mess information:
There are a lot of such information of all the activities. Participators usually search for the activity he want by having a look at the posters on the propaganda column.But the poster board experience is very poor, let people dazzling, users need to spend more time to get the information you want.

#### Tiring
Users want to get the latest event message, you need to take the initiative to get. However, due to the information is not the user browsing information, so the user will not be frequent browsing, which will lead to the user may miss their interest in activities.

#### Expensive questions
If users want to get more information, or have questions about the activities, usually to consult the organizers, and these are usually a common consultation, but i can not get my information on time even the question is answered for many times.

### For organizers, they are also very hard:

#### Need of popularity

Organizers need to publish information on a very popular platform with many followers to achieve the desired results. And most individuals or small groups do not have such resources (the number of fans of the public wechat platform or microblog)

#### Need of interest

It’s hard to find the people who is actually interesting in the activity. We want to, but we need a great work to let them informed.

#### Expensive questions

Similar to the participators, each user has the same problem will generate a lot of time to waste.

## Acitivitee is a good way

1. Platform aggregation: Activitee will be all of the information gathered to the same platform.
2. Fast for search activities, You can quickly find their own interest in activities by searching and filtering. Reduce the time cost of user access to information.
3. Comments: Users in the use of Activitee, can be consulted in the comments of the organizers, and these comments and the answer, are able to be other users browse, with the same questions users do not have to repeat the question, so as to reduce the cost of consulting and Q & A.
4. User subscription mechanism: when the user is registered, the subscription is interested in tag, the active initiator in the release of information, to the activity to add a specific tag, and then subscribe to the tag of the user push information (website message or e-mail).
　　For participators: without the need to get the user to obtain, reduce the time cost, but not easy to miss their own interest in activities. And when the user receives the push, will click the link to enter the Activitee, so as to increase the user's stickiness (refer to Netease cloud classroom, know almost the subscription push).
　　For organizers: Activitee achieved the division of the user's browser, the publisher can be aimed at the user group of interested users push to send the event message, the effect is better.
5. Operation strategy: Activitee is divided into two stages:
　　Early operation: the operation team to publish each community will be organized by the event information, the development of the number of users; (through human collection, or through the program to grab the application system data)
6. Post operation: after the increase in the amount of users, the use of traffic to attract organizations and other organizations to register, publish information.
Activitee for whom?

Mainly students and community, do not rule out other users.

<img class="img-responsive" src="https://cloud.githubusercontent.com/assets/8371330/13002038/436045f8-d1a6-11e5-923a-a281041f3e4a.png">

System Requirements:

1. Index page: users can see all the activities that are verified.
  - Sortable activities: activities can be sorted by the number of participators, time and etc.
  - Filters: users can user it to find the activities they interested.
  - Paging if there is a lot of activities.

2. Detail Page: users can see the detail information about a selected activity.
  - Can see the activity details, including the activities of the introduction, time, place, number, picture, etc.;
  - Users can comment on the activity, can also browse to other users of the comments, but only the user can review the login.
  - For users interested in the activity, the user can choose to focus on the activity, users have to login to use this feature.

3. Publish information page: the user is used to publish information on the page
  - A user to fill in the form of information activities, including activities, time, place, number, pictures, etc.;
  - After the event information is released, the administrator will be submitted to the audit, and all users will be able to browse through the home page to the home page, and then it is not visible;
  - Only logged in users can enter this page.

4. "My interesting activities" page: a user's attention of all activities:
  - All activities of the user's attention are presented in the form of a list;
  - Users can choose to cancel the attention at the right side of the list";
  - Only logged in users can enter this page.

5. "My posted activities" page: a user's release of all activities:
  - All the activities of the user are presented in the form of a list, with the status of the audit / review / revision;
  - Users can choose the right side of the list to modify the activity information, into the active information editing page, modify the end of the post by the administrator audit, audit information will be modified through the post, not by the same;
  - Only logged in users can enter this page.

6. Administrator page:
  - only the administrator of the user can enter the audit activity page, all audit activities in the form of a list, audit information will be released, and removed from the audit page.

7. other senior requirements: in the case of the completion of all the above requirements
  - Activity recommendation: according to the user's attention to the behavior of the activity, analysis of the user's preferences, so that the user may be interested in activities;
  - Reminds for activity information changes: when the activity of the publisher to modify the activity information, attention to the activity of all users immediately receive the activity information is modified to remind;
  - Users reputation mechanism: to each user to increase the credibility value and users to publish the event date, other users can to the scoring, thus changing the activities of the initiator of the reputation value. On the home page ranking can be sorted by "the credibility of the initiator".
  - User authentication mechanism: users can apply for identity authentication, such as a community in charge, the administrator of the user authentication after verification (similar to the micro blog V). In the home screen can choose to choose the activities of the user authentication".

## System Architecture
- Programming language: Jade、Less and Livescript.
- Front-Side Framework: Bootstrap
- Server-Side: NodeJS
- Database: Mongodb

<img class="img-responsive" src="https://cloud.githubusercontent.com/assets/8371330/13002136/1fbb3c7e-d1a7-11e5-90cd-3b64f2105fd2.png">


You are more than welcome to fork our project and add some functions. And you can use our open source project to run.
