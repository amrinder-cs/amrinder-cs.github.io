# Diary for TR-103

# 12th June 2023
# Setting up Parsedown to display .md files using nginx

## Introduction
- Parsedown is a php file which lets us render .md files into .html files.
- fastCgi is used to host a php server via nginx
- Nginx is used to serve our PHP applications via the internet.


## Installation and setup of Parsedown using nginx


1. Install php, php-cgi and nginx on the server using:
`sudo apt update && sudo apt upgrade && sudo apt install php8.2 php8.2-cgi php8.2-fpm`

1. Download Parsedown which will help us parse .md to HTML using php. source: [here](https://github.com/erusev/parsedown)

1. This is how we setup the nginx configuration file to do the above: [markdown parser config](/Parser.conf)


# 13th June 2024
# Manual Installation of frappe framework on debian 12.5

9:30 - installed ddocker and installed docker image of frappe

11:30 - started to re install frappe manually

12:00 - Frappe requires different dependencies, which were conflicting with the ones in OS. Therefore reinstalled the OS.

2:00 - installed debian 12 fresh

03:06 - Installed Node Version Manager to manage different versions of nodejs. Source: [Here](https://github.com/nvm-sh/nvm?tab=readme-ov-file#git-install)

03:34 - Installed MariaDB version 15.1

03:48 - install python 3.11

03:59 - started installing redis from source since version 6 was not available in the package manager.

04:10 - installed jemalloc - a dependency of redis and hiredis

04:23 - installed redis 7

04:31 - installed frappe bench

# 14 June 2024

09:00 - attempted to install redis 6 from source again without success.

11:00 - copied the sources.list from debian:bullseye and pasted it into debian 12, then ran sudo apt update && sudo apt install redis

12:00 - installed redis 6 and started installation of frappe-bench again, but caught an error from yarn.

12:30 - realzied frappe uses version 1.12 of yarn package manager, which is deprecated 6 years ago. Had to install the older version again using a hacky method. (uninstalled yarn using apt and then re installed it using `npm install yarn@1.12.0`

01:00 - While restarting frappe bench, it threw an error which said supervisorctl is not found, so installed supervisorctl using python's pip3. However that did not change anything. Therefore installed it via OS package manager, which seemed to work normally but not through bench.

01:30 - Learnt the basics of supervisorctl.

02:00 - Went to forums and found out another person having same issue. Turns out when we install frappe-bench, it does not install the latest version by default. We have to manually upgrade it to a newer version, therefore upgrading it right now.
source: [Here](https://discuss.frappe.io/t/bench-restart-error-sub-process/103874/9)
```
bench update
bench migrate
bench build
sudo chmod o+x $HOME
```

2:48 - Realized frappe requires you to run bench as superuser. Which could be:
 - Insecure since in case frappe is taken over, the attacker will have instant superuser privilages
 - Not recommended by python3 anymore because it is considered bad practice. source:


```
user@physics:~$ pip3 install supervisor
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
    python3-xyz, where xyz is the package you are trying to
    install.
    
    If you wish to install a non-Debian-packaged Python package,
    create a virtual environment using python3 -m venv path/to/venv.
    Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
    sure you have python3-full installed.
    
    If you wish to install a non-Debian packaged Python application,
    it may be easiest to use pipx install xyz, which will manage a
    virtual environment for you. Make sure you have pipx installed.
    
    See /usr/share/doc/python3.11/README.venv for more information.
```


 - Not available in package manager any way:


```
user@physics:~$ sudo apt install python3-supervisor
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package python3-supervisor
```


# 15 June 2024

## Followed the Developement version of frappe installation documentation instead of production one

09:00 - Fresh installed Docker on Debian 12.5

09:30 - Cloned the frappe_docker repository of frappe. [Here](https://github.com/frappe/frappe_docker/)

 - Figured out in latest docker version, `docker compose` no longer works. Requires `docker-compose`

 - The syntax does not match while installation, it gives an error about invalid command `docker compose ...`

 - Using docker-compose, ran the command: `docker-compose -f pwd.yml -d`

 - It still gave the error regarding syntax of command which are abstracted with docker 



# 16, 17, 18 June 2024

Started creating the own docker compose file for frappe.
[Spent weekend learning docker compose](https://docs.docker.com/compose/gettingstarted/)


# 19 June 2024

Studied about how Drillbit Plagiarism checker works. It works by first selecting the type of document to be uploaded, then it asks for the name of submitter, and the name of the mentor plus their email. The results are then emailed to both the parties, or optionally just the mentor. The plagiarism checker has the option to check the grammer, it also has an experimental feature to detect AI submittions. The report is generated in 5 to 10 minutes based on the submittion length. The report tells us the sources of where the content might be plagiarised from. It also detects how much percentage was generated by AI.

The openapi of drillbit is also provided by the service. [drillbit openapi](https://drillbitplagiarism.com/openAPI)

The API has following methods:

- Create/Delete/Rename a folder
- Upload document for plagiarism check in the folder
- Check for plagiarism on the internet or from institutional repository (already submitted documents are also checked against)
- Get results of the submitted document 

# 20 June 2024

Initialized the app for drillbit. This is how you initialize the application in drillbit:


```bash
$ bench init frappe-bench # creates a new website enviornment
$ cd frappe-bench
$ bench new-site sitename.com
$ bench new-app drillbit

```

the app would be installable by: 

```bash
bench get-app https://github.com/amrinder-cs/drillbit
bench --site sitename.com install-app drillbit
```

# 21 June 2024

Created a new doctype named Assignment, the assignment has the options such as:

- mentor name
- mentor email
- student name
- student email
- assignment
- type of submittion
- title

these are the basic options, there are more flags which tell us the state of the document.

The document has these stages: 
	- Pending
	- Submitted
 	- Accepted
	- Rejected

These stages tell us where the assignment is right now at the moment. The plagiarism check is done at the submitted stage by the teacher/mentor or head of department.


#  24 June 2024

Added  the ability to add API credentials from the frontend for drillbit, so that we don't have to do it in the code. The credentials are configurable by the administartor. The python code fetches the credentials and uses it to authenticate with drillbit.


# 25 June 2024

created scaffolding with python code, made all the individual units of the full program and tested them for proper functioning. These are the units which were tested:
- Creating a document and reading from python code
- Changing states and triggering python code upon stage change
- Changing views based on the user, so only authorized users can see the plagiarism check part
- Uploading the document via frontend, and reading it by drillbit.


# 26 June 2024
Today, I worked on a feature in Frappe to fetch the current logged-in user and the selected user using Python. I utilized Frappe's built-in methods to retrieve session data and implemented logic to identify the selected user. After debugging and testing for different user roles and scenarios, the functionality worked as expected. It was a productive task, enhancing my understanding of Frappe's user management.


# 27 June 2024

Today, I debugged a looping issue in my code caused by a recursive function call. After identifying the problem, I implemented a flag to prevent the recursion from repeating unnecessarily. This resolved the issue effectively and ensured the function behaved as expected.

# 28 June 2024

Today, I exported permissions and a Doctype into my Frappe application. This involved ensuring the configurations and structure were correctly mapped to the app for seamless integration. The process went smoothly, and the exported components are now ready for deployment.

# 29 June 2024

Today, I implemented a feature that allows folders to be created directly from the frontend. This involved updating the UI and backend logic to handle folder creation seamlessly. The new functionality is now working as intended, improving user accessibility and workflow.

