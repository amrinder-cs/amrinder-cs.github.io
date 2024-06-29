# How to install Frappe on Debian 11

1. Install the dependencies using: `sudo apt install git python-dev python-pip redis-server software-properties-common python3-pip`

[!] Python version required here is 3.10.14

1. Install Node Version Manager. Follow the instructions from their github [here](https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating). Use version 14 for first time.

1. Install yarn using npm. `npm install -g yarn`

1. Install wkhtmltopdf

    - `wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.bullseye_amd64.deb`

    - `sudo dpkg -i wkhtmltox_0.12.6.1-2.bullseye_amd64.deb`

1. Install frappe bench using: `pip3 install frappe-bench`

1. Initialize frappe bench using: `bench init (name)` Example: `bench init frappe-bench`

1. Install supervisorctl using: `bench pip install supervisor` and then initialize it using: `echo_supervisord_conf > /home/user/.local/supervisord.conf`

1. Install MariaDB using: 
    - `apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8`
    - `add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://ftp.ubuntu-tw.org/mirror/mariadb/repo/10.3/ubuntu xenial main'`
    - `sudo apt update && sudo apt install mariadb-server`
    - mysql_secure_installation

1. Create a new website using: `bench new-site example.com`

1. we can get apps on frappe using: `bench get-app --resolve-deps app_name`

1. we can install those apps using: `bench --site example.com install-app app_name`