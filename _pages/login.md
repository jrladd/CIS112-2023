---
title: Login to Postgres
permalink: /login/
---


1. Make sure you are on the WashJeff campus Wifi network. **Postgres will only work when connected to this network!**
2. Check your email for your username and password.
3. Open the Command Prompt (either through Notepad++ or the Start Menu).
4. Enter the following command: `psql.exe -h wjcissvr.washjeff.edu -U yourusername -d yourdatabase`
5. When prompted, enter your full password (case sensitive) and press Enter. *(It won't look like you're typing anything, but this is normal!)*
6. Look for the prompt `username=>`, and you're ready to go!

Postgres is installed in our classroom and all of the lab computers. If you want to use it on your own machine, you'll need to install Postgres there yourself.

## Common `psql` Commands

- `\c` Connect to a database
- `\i` Execute a script
- `\d` List all tables and views of the available schema
- `help` List all `psql` help commands
- `\h` List all SQL commands
- `\?` List all `psql` commands
- `\q` Close the database connection and exit the program
- `\dn` List all viewable schemas
- `\dt` List all of the tables in the active schema
- `\l` List all viewable databases

## If this isn't working...

*Before you login, make sure the PATH is set correctly in your Environmental Variables. If you're having trouble getting psql to work, this is a likely cause. You'll need to add the following to your PATH:  
`C:\Program Files\pgAdmin 4\v7\runtime\`*
