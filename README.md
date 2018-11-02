# unixAssignment2

COSC1131/1133 UNIX Systems Administration and Programming
Assignment 2 – Shell Scripting

Due date: Sunday, 14th October at 9 pm

Task 1: Compilation of the Kernel (10 marks)
By the end of week 10 you will need to get checked off in the lab that you have completed the lab 7 exercise in compilation of the kernel and installation on your raspberry pi. 

Scenario for the rest of the assigment: 

Consider the leds (the little lights) on your raspberry pi. These lights can be changed by sending messages to the device about how to behave. 

The first script you are to write (which you must decompose into smaller scripts/functions) is to allow a user of your script  to manipulate the leds on your pi. 

When the script starts up, it will scan the contents of /sys/class/leds to get a list of the leds on the system (each link in this directory points to a directory for the settings of that led). 

If you change into one of those directories you will see a set of files that looks like this: 

brightness
device
max_brightness
power
subsystem
trigger
uevent

The two files we are interested in are “brightness” and “trigger”. 

The brightness file provides a way for us to turn on the led, such as: 

echo 1>brightness

turns on the led. And: 

echo 0>brightness

turns off the led. 

Please review the redirection operators from week 2 if you are fuzzy on these as they will be fairly heavily relied upon in this assignment. If you have trouble with this, try using tee. You will find you cannot write to these devices unless you are root. 

The trigger file on the other hand provides us with a way to associate an led trigger with an event in the system. For example on my pi I have the following for the led0 led (you can “cat trigger” to see these for any led on your pi) (see the next page): 

none rc-feedback kbd-scrolllock kbd-numlock kbd-capslock kbd-kanalock kbd-shiftlock kbd-altgrlock kbd-ctrllock kbd-altlock kbd-shiftllock kbd-shiftrlock kbd-ctrlllock kbd-ctrlrlock timer oneshot heartbeat backlight gpio cpu cpu0 cpu1 cpu2 cpu3 default-on input panic mmc1 [mmc0] rfkill-any rfkill0 rfkill1 

As you can see, these are related to system events – you may not know what all these do. If you are unsure on some of these, do a web search to find out. You should know what things do before you use them. The event that currently has square brackets around it is the current event and should indicated with a star next to it on all appropriate menus. There are also some interesting and fun ones like “timer” which blinks on and off at a regular interval and “heartbeat” which blinks on and off at the rhythm like a heartbeat. 

You can change the event that is currently being handled by the LED by echoing that event name into the trigger file, eg: 

echo heartbeat > /sys/class/leds/led1/trigger

The scripting tasks: 
Each of these scripting tasks must be in a separate function. It should have a sensible name to streamline maintainability (task1, task2, etc will be marked down) and should follow standard conventions such as paths to executables specified in variables at the top of the script, functions should only reference local variables UNLESS they represent shared state in the program (hardly any of these should so you need to make a case as to why you have done this in your README if you have). 

You are required to check for errors after each command you enter and capture any output text into a variable. You should then output error messages in a reasonable format such as “This operation has failed:” followed by the error message that was output by the command. You are required to disable the control-c interupt of the script and the script should handle any requests to quit gracefully. You should opt in all cases for the calling of programs to do work for you rather than performing operations in your script. For the purpose of these tasks you must restrict yourself to standard UNIX utilities. This means the following (next page) are ok and mostly essentially to completing the assignment: 


sed
awk
grep
cut
ps
echo
printf
test
bash
cat
select

for loops
while loops
variables
expr
bc
let expressions
sleep
more (the program)
head
tail

Note that all submitted scripts must use bash and must specify the interpreter at the top of the script.
For all other utilities you wish to use, please ask on the discussion board. Please do not just assume as that often results in conflict situations later on and we would like to avoid that.

Task 2: Script launch (8 marks) 
On start of your script it should read the contents of the /sys/class/leds directory and present a menu for your program. An example run of your program should look like: 

Welcome to Led_Konfigurator!
============================
Please select an led to configure: 
1. input0::capslock
2. input0::numlock
3. input0::scrolllock
4. input4::capslock
5. input4::numlock
6. input4::scrolllock
7. led0
8. led1
9. Quit
Please enter a number (1-9) for the led to configure or quit:

Please note that hardcoding this menu will get you 0 marks. It must be generated from the contents of the /sys/class/leds directory.

Task 3: LED Manipulation Menu (10 Marks)
Create a function that will display a menu of options for any led selected which shall look like the following: 

[LED NAME] <-- replace this name with the actual name of the led. 
==========
What would you like to do with this led? 
1) turn on
2) turn off
3) associate with a system event
4) associate with the performance of a process
5) stop association with a process’ performance
6) quit to main menu
Please enter a number (1-6) for your choice:

Task 4: Turn on and off the led (4 Marks)
The “turn on” command above echoes 1 into the brightness file for the respective led and the “turn off” command echoes 0 into the brightness file. 

Task 5: Associate LED with a system event (10 Marks)
The associate with a system event menu will list the events from the trigger file with a * next to the currently selected event. So the menu for that option would look like (next page): 

Associate Led with a system Event
=================================
Available events are: 
---------------------
1) none
2) rc-feedback
3) kbd-scrolllock
4) kbd-numlock
5) kbd-capslock
6) kbd-kanalock
7) kbd-shiftlock
8) kbd-altgrlock
9) kbd-ctrllock
10) kbd-altlock
11) kbd-shiftllock
12) kbd-shiftrlock
13) kbd-ctrlllock
14) kbd-ctrlrlock
15) timer
16) oneshot
17) heartbeat
18) backlight
19) gpio
20) cpu
21) cpu0
22) cpu1
23) cpu2
24) cpu3
25) default-on
26) input
28) panic
29) mmc1
30) mmc0*
31) rfkill-any
32) rfkill0
33) rfkill1
34) Quit to previous menu
Please select an option (1-34):
Please note that if the options on the menu scroll off the screen, you are required to page the menu with more.  Please note that hard coding this menu will get you 0 marks. It must be dynamically generated from the contents of the trigger file. 

Please note that tasks 6 and 7 are expected to be harder tasks. Not everyone will be able to complete them. In fact only HD students can expect to complete all of question 6 and not everyone can get a HD. 

Task 6: Associate LED with the performance of a process. (20 Marks)
When the user selects this item from the menu, a new prompt should be displayed that looks like this: 

Associate LED with the performance of a process
------------------------------------------------
Please enter the name of the program to monitor(partial names are ok): chromium
(Continued next page) 
Do you wish to 1) monitor memory or 2) monitor cpu? [enter memory or cpu]: memory
starting to monitor chromium-browser

After the user has entered the name of the program, you should invoke ps in the background to search for a program that matches the name specified. If none is found, display an error that this has failed. Please note: you will be marked on the quality of your responses to user input!! If there are multiple matches, display a prompt for which program to monitor, eg: 

Name Conflict
-------------
I have detected a name conflict. Do you want to monitor: 
1) chromium-browser
2) chromium-foobar
3) Cancel Request
Please select an option (1-3):

At this point the user can select any of the programs listed to be monitored or they can select to cancel this request and go back to the main menu.

After this the script will redisplay the menu for this LED. It will also spawn in the background a script that will call “ps” repeatedly and search for all instances of the selected program. You will use  the brightness file to turn on and off the led at a rate that reflects the resource consumption of this process. If a process never uses any resources (or close to 0), it will never turn on however the closer the process gets to using 100% of resources (be that memory or cpu) the more the time the led should be on. For the time between when the LED is turned on and off or vice versa, you may use sleep to do the delays. Do not use cpu intensive forms of delay or delays that run at different speeds on different systems as that will attract a mark down. Note: your script should only ever be running for one process using one led so if there is a second request to monitor a process you should kill the first instance of your script. Finally the background script you implement for this requirement must use getopts as that is a much more reliable approach than using 


Task 7: Unassociate an LED with performance monitoring (4 Marks)
This is an implementation of menu item 5 from the LED menu. You will search for the background process in the ps list and “kill” it with the kill command and appropriate arguments. 

Demonstration Requirement (4 Marks)
You are required to show reasonable progress on your assignment prior to submission. This will be required from you at some point prior to the end of week 11. To do this you will need to show a mostly complete implementation of requirements 2-4. These requirements do not have to be bug free but they should mostly work correctly. This is also a great opportunity for you to get feedback on your scripts prior to final submission. Please note that this is a reward for getting started early and so late demos will not be considered in the absence of extenuating circumstances.

Submit Git Log (10 Marks)
You shall create a git repository either on one of the rmit servers or using a commercial account (in any case it must not be world readable – there must be no opportunity for others to copy your work). You must set up your git repository so you are clearly identified by name (the same name as used for enrolment) and your student email account. Each commit must have a clear description and you must commit on a regular basis to get full marks. 

Program Quality (10 Marks)
These marks are for appropriate coding standards across your scripts. All scripts must specify the command interpreter on the first line of the script. All executable paths must be assigned to variables at the top of the script, all variables in functions must be local variables; variables and functions must be appropriately named. Your scripts must also be appropriately named. We will also check your code with shellcheck.  

Program Documentation (10 Marks)
All functions must be appropriately comments with a header comment for each function describing parameters and the expected outcome of the function. There must be a reasonable number of comments in your program so that it is clear what you intention is and that the maintainer of your scripts will be able to refer to such comments to help them. Also, you must provide a readme file that explains how we can use your program. A reasonable template for a readme file can be found here: https://www.drupal.org/docs/develop/documenting-your-project/readme-template 

You don't need to include all sections (no need to install) but it’s a good starting point for what is required in maintainer and user documentation to ensure your work will be usable well beyond the timeline of the current project. Your readme should also have a references section outlining where you got any ideas from for the completion of this project. 

Plagiarism
All work submitted at RMIT University must be your own work with the exception that any work you based your work on must be referenced. When you submit your work, you are required to agree to the RMIT assessment declaration that can be found here: https://www.rmit.edu.au/students/student-essentials/assessment-and-exams/assessment/assessment-declaration 

We will check your work for originality and if there are problems we may need to refer your case on. 

What to submit: 
a zip file containing: 
The main shell script you develop
The background shell script you develop for task 6
A readme file explaining how to use your program and what it does.
a git log as created from your repo with git log --pretty

Where to get help
As usual, your teachers in class are a great resource. You are encouraged to talk to each other about how you are solving the problems but sharing of shellscript assignment code is expressly forbidden.  I have created a wiki on canvas where you can add details on sites that you have found useful. I’ve already added some and you can find it here: https://rmit.instructure.com/courses/17200/pages/assignment-2-resources-wiki 

Late Penalties
For each day late that you submit this assignment, 10% of the marks available for the late component will be deducted. If you submit this assignment more than 5 days late no marks will be available. You must apply for extensions before the due date and the reason for the extension and the evidence required is stipulated in the “Adjustments to assessments” document available here: https://www.rmit.edu.au/students/student-essentials/assessment-and-exams/assessment/adjustments-to-assessment 

If your circumstances are particularly extenuating, you can apply for special consideration here: https://www.rmit.edu.au/students/student-essentials/assessment-and-exams/assessment/special-consideration 


