# Installation, Part 4: MySQL
MySQL is required, but I believe that Ghost does support other database engines. I just prefer sticking with MySQL because after 25+ years I'm comfortable with it.


## Configuring MySQL
This part is super easy because all the config is self-contained. There are only a few variables to define in the environment file:
```ini
# Part 4: MySQL
DB_HOST=host.docker.internal
DB_NAME=ghost
DB_USER=ghostusr
## Passwords
## Beware of special characters in passwords that can be interpreted by shell.
## I generally just generate GUIDS and remove the hyphens
DB_PASS=(generate)
DB_ROOT_PASSWORD=(generate)
```
You can leave the first three alone. Or change them. Whatever.

* `DB_HOST` is the host name. This is used internally to call the server, and Docker will automatically resolve this to whatever IP address the container is assigned.
* `DB_NAME` is the name of the database inside MySQL. This is only important if you are trying to run multiple copies.
* `DB_USER` is the username that Ghost will use to contact the SQL server.
* `DB_PASS` is the password for that user
* `DB_ROOT_PASSWORD` is the password for the "root" user who has total permission over the database. You really only need this if something goes very, very wrong.

As with step 3, you can type anything for these passwords, but it will be more secure if you generate a GUID.


## Removing This Integration
Ghost is the only container in this stack that uses the database, so plugging in a different engine should be easy. I don't know offhand what Ghost supports, but all the variables are defined in the compose file.
