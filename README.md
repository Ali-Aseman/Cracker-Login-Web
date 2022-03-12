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

![1](https://user-images.githubusercontent.com/96992358/158019243-fa46b2f7-c8cc-47de-b433-b5797f612cee.png)

Now we need some information. 1. Need a key text to identify whether the login was successful or not. For example, in this website that I test, if I enter the password correctly, I will enter a page that says Welcome first. So later in the script I say if the word Welcome was in the return text, it means that the password and username are correct.

The next thing we need is the third part of the HTTP request, MessageBody. We need to know the variables in this section. To do this, I first log in to the login page with my browser (Firefox)

Then right click and select Inspect Element.


![2](https://user-images.githubusercontent.com/96992358/158019290-741667d9-5779-4067-9cf5-2da76e93cb96.png)

Then I go to the Network tab

![3](https://user-images.githubusercontent.com/96992358/158019313-26493a35-40ea-4a32-b547-8500e0874ea4.png)

Now I enter a test password incorrectly and log in.

As you can see, after entering the wrong password, the network tab changes :


![4](https://user-images.githubusercontent.com/96992358/158019354-79903391-0b2b-40bc-a0b4-13e1f720fe74.png)

Now we can select the main request to enter the website with the post method :

![5](https://user-images.githubusercontent.com/96992358/158019418-35826350-569d-4ae9-8892-5161c4955317.png)

And finally in the request tab I can see the variables. As you can see, I have three variables here. password, username and Login.

![6](https://user-images.githubusercontent.com/96992358/158019431-27a0ad36-f46f-4d33-a4fd-81c8f4d3c19c.png)

We will set these values ​​later in the source code. It is enough to put the desired username instead of the username variable and the desired password instead of the password variable, and the login is always fixed.

We need two libraries to write the script. One is the requests that are installed by default and the colorama library for writing color texts that we can use the pip tool to install.

<h1>Install the library on Linux :</h1>

    GHOST@GHOSTEPROG:~$ sudo pip3 install colorama

<h1>Install the library on Windows :</h1>

    C:\GHOSTEPROG> pip install colorama
    
<h1>Source script :</h1>

    from requests import post
    import colorama

    colorama.init() ## initialize the colorama

    url = "http://192.168.1.105/dvwa/login.php"

    usernamefile = input("Username List : ")
    passwordfile = input("Password List : ")

    find = False
    for username in open(usernamefile):
        username = username.strip("\n")
        for password in open(passwordfile):
            password = password.strip('\n')
            print (colorama.Fore.RED + "{}:{}".format(username,password))
            res = post(url,data = {"username" : username , "password" : password , "Login" : "Login"})
            content = res.content
            content = content.decode()
            if "Welcome" in content:
                print("-"*50)
                print(colorama.Fore.GREEN + "Correct USERNAME / PASSWORD --> {}:{}".format(username,password))
                find = True
                break
        if find :
            break

<h1>Description of source code :</h1>

First we imported the required libraries. Then, using colorama.init, we configured the colorama module to generate color text for us (colorama preparation). After that, we got two entries from the user, one usernamelist and the other passwordlist, which are actually the addresses of username files and password lists. Then we defined a variable called find that is equal to True only if we find the username and password. Then we wrote a for loop for the usernamelist file so that we can access each of the usernames in the file. In the first line of the code of this loop, we deleted the phrase n \ at the end of each username.

Then we created another for loop for the password file and in the first code of this loop, like the previous loop, we deleted the end of each password. After that, we came to print what password and username we have to test. Then we used the post method to send an http request to that address with the required parameters that we mentioned above, one was a username and the other was a password. We also had a login that was always fixed.

Then we put the answer to our request coming from the server into the res variable. Now using res.content we came to see the contents of that back page. If you remember, when I logged in correctly, one of the words that showed us on the page was the word Welcome. So here I said if the word Welcome was in the return text, it means that the login was successful and show the user the correct username and password, and finally set the find variable to True and exit the circles and that's it.

            
 <h2>After executing the script and giving the username and password files to the program, we will be updated with the following result :</h2>
 
 ![fff](https://user-images.githubusercontent.com/96992358/158019600-99b87942-c76a-4c9e-b82d-3894b9d21342.png)

 
