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

03:06 - Installed Node Version Manager to manage different versions of nodejs. Source: [Here](https://github.com/nvm-sh/nvm?tab=readme-ov-file)

03:34 - Installed MariaDB version 15.1

03:48 - install python 3.11

03:59 - started installing redis from source since version 6 was not available in the package manager.

04:10 - installed jemalloc - a dependency of redis and hiredis

04:23 - installed redis 7

04:31 - installed frappe


# Manual Installation of frappe framework on debian 12.5

9:30 - installed ddocker and installed docker image of frappe

11:30 - started to re install frappe manually

12:00 - Frappe requires different dependencies, which were conflicting with the ones in OS. Therefore reinstalled the OS.

2:00 - installed debian 12 fresh

03:06 - Installed Node Version Manager to manage different versions of nodejs. Source: (https://github.com/nvm-sh/nvm?tab=readme-ov-file#git-install)[Here]

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
