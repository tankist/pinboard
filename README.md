Intaro Pinboard
=============================

[Intaro Pinboard][1] (Pinba Board) is a simple web monitoring system which aggregates and displays [Pinba][2] data. 

Developed on [Silex][3] framework and works with PHP 5.3.3 or later.

## Installation

1. Download application:

        $ git clone git://github.com/intaro/pinboard.git
        $ cd ./pinboard

2. Download [composer](http://getcomposer.org):

        $ curl -sS https://getcomposer.org/installer | php

3. Install dependency libraries throw composer:

        $ php composer.phar install

4. Create config file and enter parameters of connection to Pinba database:

        $ cp config/parameters.yml.dist config/parameters.yml
        $ nano config/parameters.yml

5. Initialize app (command will create additional tables and define crontab task):

        $ ./console init

6. Point the document root of your webserver or virtual host to the web/ directory. Read more in [Silex documentation][4]. Example for nginx + php-fpm:

        server {        
            listen 80;
            server_name pinboard.site.ru;
            root /var/www/pinboard/web;
    
            #site root is redirected to the app boot script
            location = / {
                try_files @site @site;
            }
    
            #all other locations try other files first and go to our front controller if none of them exists
            location / {
                try_files $uri $uri/ @site;
            }
    
            #return 404 for all php files as we do have a front controller
            location ~ \.php$ {
               return 404;
            }
    
            location @site {
                fastcgi_pass unix:/tmp/php-fpm.sock;
                include fastcgi_params;
                fastcgi_param  SCRIPT_FILENAME $document_root/index.php;
                fastcgi_param HTTPS $https if_not_empty;
            }
    
            location ~ /\.(ht|svn|git) {
                deny  all;
            }
        }

## More Information

Documentation in [Wiki][5].

## License

Intaro Pinboard is licensed under the MIT license.

[1]: http://intaro.github.io/pinboard/
[2]: http://pinba.org
[3]: http://silex.sensiolabs.org
[4]: http://silex.sensiolabs.org/doc/web_servers.html
[5]: https://github.com/intaro/pinboard/wiki
