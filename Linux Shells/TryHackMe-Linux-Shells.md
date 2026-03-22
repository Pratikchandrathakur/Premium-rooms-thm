---
created: 2026-03-22T14:36:32 (UTC +05:45)
tags: []
source: https://tryhackme.com/room/linuxshells
author: 
---

# TryHackMe | Linux Shells

> ## Excerpt
> Learn about scripting and the different types of Linux shells.

---
A shell script is nothing but a set of commands. Suppose a repetitive task requires you to enter multiple commands using a shell. Instead of entering them one after one on every repetition of that task, which may take more of your time, you can combine them into a script. To execute all those commands, you will only execute the script, and all the commands will be executed. All the shells mentioned in the previous tasks have scripting capabilities. Scripting helps us to automate tasks. Before learning how to write a script, we need to know that even though shells have scripting capabilities, this does not mean that you can only make a script using a shell. Scripting can be done in various programming languages as well. However, the scope of this room is to cover scripting using a shell.

The first step is to open the terminal and select a shell. Let’s go with the bash shell, the default, and widely used shell in most distributions.

Unlike the other commands we type in the shell, we first need to create a file using any text editor for the script. The file must be named with an extension `.sh`, the default extension for bash scripts. The following terminal shows the script file creation:

Create Script File

```shell
           user@tryhackme:~$ nano first_script.sh

        
```

Every script should start from shebang. Shebang is a combination of some characters that are added at the beginning of a script, starting with `#!` followed by the name of the interpreter to use while executing the script. As we are writing our script in bash, let’s define it as the interpreter in the shebang.

first\_script.sh

```shell
           #!/bin/bash

        
```

We are all set to write our first script now. There are some fundamental building blocks of a script that together make an efficient script. Let’s learn and utilize these script constructs to write one script ourselves.

## Variables

A variable stores a value inside it. Suppose you need to use some complex values, like a URL, a file path, etc., several times in your script. Instead of memorizing and writing them repeatedly, you can store them in a variable and use the variable name wherever you need it.

The script below displays a string on the screen: "Hey, what’s your name?” This is done by `echo` command. The second line of the script contains the code `read name`. `read` is used to take input from the user, and `name` is the variable in which the input would be stored. The last line uses `echo` to display the welcome line for the user, along with its name stored in the variable.

```shell
# Defining the Interpreter 
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"
```

Now, save the script by pressing `CTRL+X`. Confirm by pressing `Y` and then `ENTER`.

To execute the script, we first need to make sure that the script has execution permissions. To give these permissions to the script, we can type the following command in our terminal:

Execution Permission to Script

```shell
           user@tryhackme:~$ chmod +x first_script.sh

        
```

Now that the script has execution permissions use `./` before the script name to execute it. We use `./` before the script to run rather than typing the script name directly because `./` tells the shell to execute the file that is present in the current directory. If you don't define `./` before the script name, the shell will search the script in the PATH environment variable (that contains all the directories except the current one), and it will not find the defined script in any of those directories and generate an error. The below terminal shows the script in which we utilized the variables:

Script Execution

```shell
           user@ubuntu:~$ ./first_script.sh
Hey, What's your name?
John
Welcome, John

        
```

## Loops

Loop, as the name suggests, is something that is repeating. For example, you have a list of many friends, and you want to send them the same message. Instead of sending them individually, you can make a loop in your script, give your friend list to the loop and the message, and it will send that message to all your friends.

For a general explanation of loops, let’s write a loop that will display all numbers starting from 1 to 10 on the screen. First, create a new file named `loop_script.sh`, then enter the code below. Save your file by pressing `CRTL+X`, then confirm with `y` and then `ENTER`.

```shell
# Defining the Interpreter 
#!/bin/bash
for i in {1..10};
do
echo $i
done
```

The first line has the variable `i` that will iterate from 1 to 10 and execute the below code every time. `do` indicates the start of the loop code, and `done` indicates the end. In between them, the code we want to execute in the loop is to be written. The for loop will take each number in the brackets and assign it to the variable `i` in each iteration. The `echo $i` will display this variable’s value every iteration.

Now, let’s execute the script after giving it the execution permission.

Script Execution

```shell
           user@tryhackme:~$ ./loop_script.sh
1
2
3

        
```

The output of the above terminal is cut to `3` numbers only for demonstration. However, when executed according to the script's logic, it would display the numbers from `1 to 10`.

### Conditional Statements

Conditional statements are an essential part of scripting. They help you execute a specific code only when a condition is satisfied; otherwise, you can execute another code. Suppose you want to make a script that shows the user a secret. However, you want it to be shown to only some users, only to the high-authority user. You will create a conditional statement that will first ask the user their name, and if that name matches the high authority user’s name, it will display the secret. 

First, create a new file named `conditional_script.sh`, then enter the code below. Save your file by pressing `CRTL+X`, then confirm with `y` and then `ENTER`.

```shell
# Defining the Interpreter 
#!/bin/bash
echo "Please enter your name first:"
read name
if [ "$name" = "Stewart" ]; then
        echo "Welcome Stewart! Here is the secret: THM_Script"
else
        echo "Sorry! You are not authorized to access the secret."
fi
```

The above script takes the user’s name as input and stores it into a variable (studied in the Variables section). The conditional statement starts with if and compares the value of that variable with the string Stewart; if it’s a match, it will display the secret to the user, or else it will not. The fi is used to end the condition.

Following is the terminal showing the script execution when the user name matches the authorized one defined in the script:

conditional\_script.sh

```shell
           user@tryhackme:~$ ./conditional_script.sh
Please enter your name first:
Stewart
Welcome, Stewart! Here is the secret: THM_Script

        
```

However, the following terminal shows the script execution when the user name does not match the authorized one defined in the script:

conditional\_script.sh

```shell
           user@tryhackme:~$ ./conditional_script.sh
Please enter your name first:
Alex
Sorry! You are not authorized to access the secret.

        
```

## Comments

Sometimes, the code can be very lengthy. In this scenario, the code might confuse you when you look at it after some time or share it with somebody. An easy way to resolve this problem is to use comments in different parts of the code. A comment is a sentence that we write in our code just for the sake of our understanding. It is written with a # sign followed by a space and the sentence you need to write. For example, let’s rewrite the script we discussed in the conditional statements section and add comments to it. Open the `conditional_script.sh` with `nano`, then add the comments starting with a # sign. Save your file by pressing `CRTL+X`, then confirm with `y` and then `ENTER`.

```shell
# Defining the Interpreter
#!/bin/bash

# Asking the user to enter a value.
echo "Please enter your name first:"

# Storing the user input value in a variable.
read name

# Checking if the name the user entered is equal to our required name.
if [ "$name" = "Stewart" ]; then

# If it equals the required name, the following line will be displayed.
echo "Welcome Stewart! Here is the secret: THM_Script"

# Defining the sentence to be displayed if the condition fails.
else
        echo "Sorry! You are not authorized to access the secret."
fi
```

See how easy a script looks with comments. Comments don’t affect the working of any script. A good script always has some comments. The example shown above contains a comment for each line. This is just a better explanation of its concept. However, the best way to include comments is to define them in the major and complex areas of the script.

**Note:** Other types of variables, loops, and conditional statements can also be used to achieve different tasks. Moreover, multiple lines of comments can also be added within a single comment. However, it is not the scope of this room.
