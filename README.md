## Get started in 10 minutes with PostgreSQL on Ubuntu


`sudo apt-get -y install postgresql postgresql-contrib`

And optional autocomplete if the postgreSQL CLI isnt enough (requires pip):

`sudo pip install pgcli`


You now have a user postgres, but `sudo -u` each time is annoying, so:

`alias psql='sudo -u postgres psql'`. You may as well edit ~/.bashrc , eg with `kate ~/.bashrc` to persist the alias.


`sudo -u postgres createuser DBUSER --superuser`, or if you prefer within `psql`,
```
CREATE USER dbuser;

ALTER USER dbuser WITH SUPERUSER;
```

Problems creating user?
Could not change directory to "/home/project/directory": Permission denied ?
Yup, user postgres wants 775 access to *each* directory, including `~`, down to where you are now.
```
chmod 775 .
chmod 775 ..
chmod 775 ../..
```
and so on.


Made the user OK? Let's see where we are at...
```
psql
\du
\l
\conninfo
```
Think of a good name for your database
```
CREATE DATABASE a_good_name;
GRANT ALL PRIVILEGES ON DATABASE a_good_name TO dbuser;
ALTER USER dbuser PASSWORD 'password';
```
Yes, you need to set a password if you want to access via port.
Remember that SQL requires singles quotes.

Now you could set:
`env DATABASE_URL=postgresql://username:password@hostname/dbname`
but, why not place it in .env ?
In .env, remember to `export` and don't add spaces around the `=`.

(Port 5432 is the default, so no need to specify it)

Make sure .env is in your .gitignore - there's no such thing as 'removing' your secrets from github!

Now we'll need `npm i` as we're using env2.

Bang ! - `npm run dbbuild` and you're away...

