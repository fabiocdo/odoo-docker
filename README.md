# odoo-docker
#### Repositório adaptado de [Trustcode Sistemas Empresariais](https://github.com/Trust-Code)

How do use this docker image ?
---------------------

Minimal command to run this image

```bash
▶ docker run --name odoo --net host -d -e PG_USER=odoo -e PG_PASSWORD=odoo fabiocdo/odoo-docker:12.0
```

Other parameters:

* PG_HOST=localhost
* PG_PORT=5432
* PG_USER=odoo
* PG_PASSWORD=odoo
* PORT=8069
* LONGPOLLING_PORT=8072
* WORKERS=3
* ODOO_PASSWORD=senha_admin
* DISABLE_LOGFILE=0
* ODOO_ENTERPRISE=0
* ODOO_VERSION=12.0

Example: Switching the port on which Odoo will listen to:

```bash
▶ docker run --name odoo --net host -d -e PG_USER=odoo -e PG_PASSWORD=odoo -e PORT=8050 fabiocdo/docker_odoo:12.0
```

Preferred way:
---------------------

Install [docker-compose](https://docs.docker.com/compose/install/) to manage docker containers.

Create a docker-compose file following this example:
```yaml
version: '3'
services:
  odoo-update:
    image: fabiocdo/docker-odoo:12.0
    network_mode: host
    command: autoupdate
    volumes:
      - ~/.ssh:/home/temp/.ssh
      - ~/custom_addons:/opt/dados/addons/12.0
    environment:
      ODOO_VERSION: 12.0
      ODOO_ENTERPRISE: 0
      PORT: 8090
      DATABASE: odoo
      DISABLE_LOGFILE: 1
      TIME_CPU: 600
      TIME_REAL: 720
```

Parameters:

- ODOO_ENTERPRISE - download the enterprise version (it needs a valid ssh key to be mounted under /home/temp/.ssh)
- DATABASE - optional database name (required if you use autoupdate command when run the image)
- DISABLE_LOGFILE - disable odoo logs to a file, instead output to standard (useful with autoupdate)
- TIME_CPU - cpu limit before timeout
- TIME_REAL - real limit before timeout

Change the parameters as you want and run:
```bash
▶ docker-compose up
```

Updating the Odoo instance
----------------------------------

Download the latest version of this docker image and follow below. We run daily builds of this image, it's safer to run this process in your Odoo instance at same periodicity.

If you want to update your Odoo instance just add to your docker-compose file the following command:
```yaml
    image: fabiocdo/docker-odoo:12.0
    command: autoupdate
    network_mode: host
```
But before run this you should install the module "module_auto_update", without the module installed in the database the above command will not update Odoo. More info on the [module](https://github.com/OCA/server-tools/tree/12.0/module_auto_update).


Using for development and testing:
-----------------------------------

Download this repository, change the environment variables in docker-compose.yml and run:
```bash
▶ docker-compose build && docker-compose up
```
