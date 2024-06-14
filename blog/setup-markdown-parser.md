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
    https://example.com/something
    then REQUEST_URL will be /something
    */
    $docRoot = $_SERVER['DOCUMENT_ROOT'];
	
    $file = file_get_contents($docRoot.$uri);
	
    if ($file === false) die ("File not found");
	
    require "Parsedown.php";
	
    $Parsedown = new Parsedown();
	
    echo $Parsedown->text($file);
    ?>
    ```
    