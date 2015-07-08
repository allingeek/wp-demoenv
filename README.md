# wp-demoenv
This docker-compose based environment will create a WordPress demo environment. Each node of the wordpress service will use a unique database name on the shared db node. Other than that minor customization this environment uses a stock MariaDB and WordPress image from Docker Hub.

## Prerequisites

1. Install docker - https://docs.docker.com/installation/mac/
2. Install docker-machine - https://docs.docker.com/machine/install-machine/ 
3. Install docker-compose - https://docs.docker.com/compose/install/
4. Clone this repo - `git clone git@github.com:allingeek/wp-demoenv.git`

## Running Local
#### Start up the database and a single WP instance
1. From the cloned repo, run `docker-compose up -d`
2. There is no step 2.
3. Checkout your instance. Each running instance will be bound to an unused port. Discover the port by listing the running containers: `docker-compose ps`
````
        Name                       Command               State           Ports         
--------------------------------------------------------------------------------------
wpexample_db_1          /docker-entrypoint.sh mysqld     Up      3306/tcp              
wpexample_wordpress_1   /entrypoint.sh apache2-for ...   Up      0.0.0.0:32777->80/tcp 
````
If you are running on a mac, your docker daemon will be running inside a boot2docker virtual machine. To access your WP instance you need to discover the IP address of the virtual machine: `boot2docker ip`. Open a web browser and access that IP address and the port listed for the WP instance. In the above example, the port is 32777. The URL you would access in this case is http://&lt;ip address&gt;:32777/.

## Running in DigitalOcean (simple)

## Running in EC2 (slightly more complicated)
