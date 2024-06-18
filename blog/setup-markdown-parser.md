# How to setup Markdown Parser in nginx.

 - A markdown parser parses .md files to .html
 - We will be using parsedown in linux to convert .md to .html using php
 - fastCgi will be used in nginx to render that html.

### Steps to install and setup parsedown.

1. Install php, php-cgi and nginx on the server using the following commands:
    `sudo apt update && sudo apt install php8.2 php8.2-cgi php8.2-fpm`

    ![installing dependencies](https://i.imgur.com/X9dkOzt.png)

    Here,
    - php8.2 is the version of php we are using.
    - php8.2-cgi communicates with our server (nginx), so nginx can pass data to php, and get the data from it.
    - php8.2-fpm spawns multiple workers to handle multiple requests simeltaneously.
1. Download Parsedown, which is used to parse markdown to html. Here is how it's set up:
    - clone parsedown using:
    
        `git clone https://github.com/erusev/parsedown`
    - cd into the newly cloned repository:

        `cd parsedown`

    - copy the Parsedown.php from here to your site's root. example:

        `cp Parsedown.php /var/www/html/parsedown`
    - create a parser.php which will return the parsed .md file to nginx.
    
    
    ```php
    <?php
	$uri = $_SERVER['REQUEST_URI']; /* used to get the request url. such as:
    - if full url is:
    https://example.com/something.md
    then REQUEST_URL will be /something.md
    */
    $docRoot = $_SERVER['DOCUMENT_ROOT']; /*
    Specifies document root. Such as if your website is set in /var/www/html, that will be the document root
    */
	
    $file = file_get_contents($docRoot.$uri); /*
    $docRoot.$uri concatenates /something.md and /var/www/html for example.
    Full file path will become (as an example): /var/www/html/something.md
    */
    if ($file === false) die ("File not found"); // returns false if file is not found
	
    require "Parsedown.php"; // just imports the Parsedown.php
	
    $Parsedown = new Parsedown(); // Initiates Parsedown
	
    echo $Parsedown->text($file); // Displays the file in HTML format.
    ?>
    ```

1. Configure Nginx.

    - Create a new file in /etc/nginx/sites-available/filename (put filename of your choice)

    - put this in the file.


    ```nginx

    server {
        server_name www.example.com;
        root /var/www/html/parsedown;
        index index.html;

        # This is used to process urls ending with .md
        location ~ \.md$ {
            include fastcgi.conf;
            # This passes any .md file to parse.php
            fastcgi_param SCRIPT_FILENAME $document_root/parse.php;
            fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        }

    # this is simply used to display php files
        location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        }


    # SSL configuration which is automatically configured using certbot. (sudo apt install certbot)
        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/www.example.com/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/www.example.com/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    }

    server {

        if ($host = www.example.com) {
            return 301 https://$host$request_uri;
        } # managed by Certbot
        listen 80;
        if ($host ~ ^\d+\.\d+\.\d+\.\d+$) {
            return 444;
        }
        server_name www.example.com;
        return 404; # managed by Certbot
    }
    server {
        if ($host = www.example.com) {
            return 301 https://$host$request_uri;
        } # managed by Certbot
        server_name www.example.com;
        listen 80;
        return 404; # managed by Certbot
    }
    server {
        if ($host = www.example.com) {
            return 301 https://$host$request_uri;
        } # managed by Certbot
        server_name www.example.com;
        listen 80;
        return 404; # managed by Certbot
    }
    ```



1. Enable the generated nginx configuration file.

- create a symbolic link in /etc/nginx/sites-enabled using: ln /etc/nginx/sites-available/filename /etc/nginx/sites-enabled/filename

- check if the configuration file is correct using: sudo nginx -t

- if the test is successful, reload nginx. (sudo systemctl restart nginx)