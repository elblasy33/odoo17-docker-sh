# odoo-docker-sh
 # Quick install

Installing Odoo 17 with one command.

(Supports multiple Odoo instances on one server)

Install [docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/) yourself, then run:

``` bash
curl -s https://raw.githubusercontent.com/elblasy33/odoo17-docker-sh/main/run.sh | sudo bash -s odoo17-one 10017 20017
```

to set up first Odoo instance @ `localhost:10014` (default master password: `Elblasy2022@1234`)

and

``` bash
curl -s https://raw.githubusercontent.com/elblasy33/odoo17-docker-sh/main/run.sh | sudo bash -s odoo17-two 11027 21027
```

to set up another Odoo instance @ `localhost:11014` (default master password: `Elblasy2022@1234`)

Some arguments:
* First argument (**odoo17-one**): Odoo deploy folder
* Second argument (**100174**): Odoo port
* Third argument (**20017**): live chat port

If `curl` is not found, install it:

``` bash
$ sudo apt-get install curl
# or
$ sudo yum install curl
```

# Usage

Start the container:
``` sh
docker-compose up
```

* Then open `localhost:10014` to access Odoo 17.0. If you want to start the server with a different port, change **10017** to another value in **docker-compose.yml**:

```
ports:
 - "10014:8069"
```

Run Odoo container in detached mode (be able to close terminal without stopping Odoo):

```
docker-compose up -d
```

**If you get the permission issue**, change the folder permission to make sure that the container is able to access the directory:

``` sh
$ git clone https://github.com/elblasy33/odoo14-docker-sh.git
$ sudo chmod -R 777 addons
$ sudo chmod -R 777 etc
$ mkdir -p postgresql
$ sudo chmod -R 777 postgresql
```

Increase maximum number of files watching from 8192 (default) to **524288**. In order to avoid error when we run multiple Odoo instances. This is an *optional step*. These commands are for Ubuntu user:

```
$ if grep -qF "fs.inotify.max_user_watches" /etc/sysctl.conf; then echo $(grep -F "fs.inotify.max_user_watches" /etc/sysctl.conf); else echo "fs.inotify.max_user_watches = 524288" | sudo tee -a /etc/sysctl.conf; fi
$ sudo sysctl -p    # apply new config immediately
```

# Custom addons

The **addons/** folder contains custom addons. Just put your custom addons if you have any.

# Odoo configuration & log

* To change Odoo configuration, edit file: **etc/odoo.conf**.
* Log file: **etc/odoo-server.log**
* Default database password (**admin_passwd**) is `Elblasy2022@1234`, please change it @ [etc/odoo.conf#L60](/etc/odoo.conf#L60)

# Odoo container management

**Run Odoo**:

``` bash
docker-compose up -d
```

**Restart Odoo**:

``` bash
docker-compose restart
```

**Stop Odoo**:

``` bash
docker-compose down
```

# Live chat

In [docker-compose.yml#L21](docker-compose.yml#L21), we exposed port **20015** for live-chat on host.

Configuring **nginx** to activate live chat feature (in production):

``` conf
#...
server {
    #...
    location /longpolling/ {
        proxy_pass http://0.0.0.0:20015/longpolling/;
    }
    #...
}
#...
```

# docker-compose.yml

* odoo:17.0
* postgres:14

# Odoo 14 screenshots

<img src="screenshots/odoo-15-apps-screenshot.png" width="50%">

<img src="screenshots/odoo-15-product-form.png" width="100%">

<img src='screenshots/odoo-15-sales-screen.png' width="100%">

