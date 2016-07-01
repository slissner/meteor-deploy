# Meteor deploy

## Purpose

Tired of Galaxy, Meteor Up, you name them?

Try to deploy youself with this little shell scripts! Browse to the `sh` folder to find the scripts.

## Known issues

### Fibers errors

See http://stackoverflow.com/a/13327111/3757139

### If npm packages are missing

    cd programs/server          # Go to app folder and do
    npm install

### Switch node versions

Use "n" node version manager, see [ask ubuntu](http://askubuntu.com/a/480642):

    sudo n 0.10.41 # uses "n" node version manager
    sudo ln -sf /usr/local/n/versions/node/0.10.41/bin/node /usr/bin/node # creat symlink

## Further set up

### Nginx

See this [tutorial](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-meteor-js-application-on-ubuntu-14-04-with-nginx)

#### Install

    sudo apt-get update
    sudo apt-get intsall nginx


#### Configuration

Create virtual host for your app:

    sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/my-app # on the server, create virtual host
    sudo vi my-app

Edit virtual host configurations:

    @ change "listen 80" to "listen 3000"  # also in the subsequent line
    @ change "server_name localhost" to "server_name app.myhost.com" # and/or: change subdomain or domain name
    @ :wq! # save changes

Create virtual link:

    sudo ln -s /etc/nginx/sites-available/billy-chat /etc/nginx/sites-enabled/billy-chat # enable site via link

Test if server is running:

    sudo nginx -t # test if nginx is running successfully

If conflicting server name error appears, remove default:

    sudo rm /etc/nginx/sites-enabled/default

#### Start

    sudo service nginx restart # restart ngninx

Test if nginx is running on your port 3000 (or your host name):

    http://myhostname:3000/

### MongoDb

#### Install

See official [docs](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/).

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

#### Start

Start mongo service

    sudo service mongod start
    vi /var/log/mongodb/mongod.log # just to see if runs correctly. Last line shall be: [initandlisten] waiting for connections on port 27017

#### Configure

Configure mongo, if you want:

    sudo vi /etc/mongod.conf

#### Files

Mongo files, to be found under:

    /var/lib/mongodb # data files
    /var/log/mongodb # logs
    /etc/mongod.conf # config
    /etc/init.d/mongod # The mongodb-org package includes various init scripts, including the init script /etc/init.d/mongod. You can use these scripts to stop, start, and restart daemon processes.

#### Create Database

To create database: (See [Mongo manual](http://www.tutorialspoint.com/mongodb/mongodb_create_database.htm))

    >use billy-chat
    >db # see curent database
    >show dbs # see list of databases, ours is missing
    >db.test.insert({"test":"data"}) # insert some data
    >show dbs # see list of databases, ours is shown
    >exit

#### Errors

If you run in the following error:

    Failed global initialization: BadValue Invalid or no user locale set. Please ensure LANG and/or LC_* environment variables are set correctly.

Do the following [steps](http://blog.knazsky.com/post/109391934452/mongo-and-ubuntu-problem-with-locale-settings).