CONTENT OF THIS FILE
—————————————————

* Introduction
* Requirements
* Recommended modules
* Installation
* Configuration
* Troubleshooting
* FAQ
* Maintainers


INTRODUCTION
————————

Welcome and thank you for reading the README file. 

First of all, this script is part of the assignment of subject Unix Systems Administration and Programming (Linux). 
Just for you know this is a simple program tend to control the LED of the Raspberry Pi 3. 
The script will search the default directory of the operating systems and list out the option available for the user to test out.

REQUIREMENTS
—————————
First, there must be a valid LED in the system, such as Caps Lock.
Second, the administrator should grant execute permission for the script before running the script. 
Third, scripts must place together in order for it to work properly. 

RECOMMENDED MODULES
——————————————
LED that build on top of Raspberry Pi 3 is workable as well.

INSTALLATION
————————
No specific installation is required. Place both files in the same directory.

CONFIGURATION
—————————
Run the command "chmod +x" on the files provided.

TROUBLESHOOTING
———————————
If the script is not executable by "./" please check the configuration section to ensure it was setup properly.

FAQ
————
Q: File is not executable, what should I do?
A: Please read the configuration and installation section

Q: LED blink like usual after associated to process, is that normal?
A: Yes, the calculation to delay the LED blink is by percentage(%) of total program running either CPU or MEMORY and times the 1 second.

Q: Can multiple LED associate to multiple process?
A: For now it only work for 1 single process. If another associate is requested, it will terminate the first before run the second.

Q: Can change the LED colour using the script?
A: The script does not have such feature yet.

Q: Why I can’t terminate the script with CTRL+C?
A: It already been disable within the script. 

MAINTAINERS
——————————
Current maintainers:
    * Weng Lim, Low (William) - s3697652@student.rmit.edu.au