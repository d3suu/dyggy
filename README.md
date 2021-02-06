# dyggy
Weekend project: Python3+MariaDB+fcgiwrap social network

![Main](/diggymain.png)

Idea was to create single social network in one weekend, to learn or remind SQL, MariaDB, nginx, CGI and Python3. Code is far from being perfect, almost no comments, I'm pretty much sure it's unsafe in some places, absolutely no dynamic parsers for input, but it works, and was able to handle group of friends without any problems!

## Self-hosting
Check "build log.md" for my steps of server setup. Everything in "dyggy" directory should be in `/var/www/dyggy`. Change domain name in nginx config. Make sure everything has executable flag (except templates).

## Modification
For frontend, dyggy uses simple html templates located in `/var/www/dyggy/templates` which are later rendered in `index`, `register.html`, `main` and `post` using `.format()` in Python.

## Security
First of all, no HTTPS included. If You want to use Cloudflare, You'll have to change Cookie parsing method since Cloudflare adds some data to requests. Second, we tested some simple XSS/SQL Injection, and it's pretty safe, but XSS safety straight up removes `<` and `>` characters from any input, and SQL removes `'` character (returns `200 OK - No 's no mkay.`).

## Possible redirects map
![Map](/redirects.png)

## Some post-project thoughts
Character encoding is hell, OOTB dyggy should use UTF-8, but if You're willing to fork this project, make sure to encode data directly to database, and then decode at template rendering stage.
