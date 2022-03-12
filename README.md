# Cracker-Login-Web
Today we will see how to create a cracker for login pages that use the POST method to transfer information. In this tutorial, we will use the requests library in Python to send POST requests to the server. The requests library has a function called post, which we use to do our work.

Before we get started, let me first explain about HTTP requests. When a user wants to log in to a website through the login page, he must first go to the login page address through his browser and enter his username and password and, for example, press the login button. This causes an HTTP request to be sent via the POST method to the server of that site, which contains the username and password entered by the user. After the server receives this request, it will see if the username and password entered by the user are valid or not. If it is valid, it is allowed to enter and if it is not valid, it is not allowed to enter.

Our cracker's job is to test our username and password on the login page to see which one is correct. Well then we have to write a script that sends the same HTTP request that the user sends but with the difference that it repeats this many times and we use a different username and password. Each of the requests received the correct answer from the server, ie the username and password were the same.

But if we want to examine HTTP request more precisely, we need to know what HTTP protocol is. HTTP stands for Hyper Text Transfer Protocol. The function of this protocol is to transfer information between the server and the client. The way it works is that each client sends an HTTP request to a web address and that server sends an HTTP response to the client. The following image :

![HTTP](https://user-images.githubusercontent.com/96992358/158018970-c36c82ba-0cb2-4dc8-82b4-eecf18b2fa36.png)

<h1>Each HTTP request consists of three main parts !!!!</h1>

1- Request Line section: This section has three sections. The first part specifies the method or method of communication, which can be one of the HTTP methods such as GET or post. The second part of this section specifies the destination url on the target server, for example:

/apps/login.php

The third part specifies the version of the HTTP protocol. For example, the text could be similar to the following:

HTTP 1/1

Most login pages of websites that are designed in principle use the POST method for transfer, so we must use the same method to test our username and password.

2- Headers section: Headers are actually a set of information about the client through which the server can communicate with the client. For example, it specifies information such as the language or type of browser that the client uses.

 

3- Information body part or Message Body: All the previous parts are actually for the purpose of being able to transfer the third part, ie information to the body. The information or message body is the information that we want to transfer through this request, which is actually our username and password for our project.

 

Well, these are some basic things we need to know about this project.

Preparation: Before we move on to the next step, I suggest that you read about the above discussion in Google.

Let's go to scripting. Let me say before that I use the vulnerable dvwa application to test the script. I also suggest you write your own login page to test the script or use vulnerable applications like dvwa.

 

My vulnerable application login page at http://192.168.1.105/dvwa/login.php in the following format.

