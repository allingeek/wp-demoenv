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

#### Prerequisites
1. You have a http://www.digitalocean.com account. Get one, it is awesome.
2. You've generated an API token.

#### Creating, Running, Scaling, and Destroying the Environment
````
# Create a box on DO
docker-machine create \
  --driver digitalocean \
  --digitalocean-region "sfo1" \
  --digitalocean-size "4gb" \
  --digitalocean-access-token <INSERT ACCESS TOKEN> \
  wp-demobox

# setup your environment variables
eval $(docker-machine env wp-demobox)

# Start the services
docker-compose up -d

# Set the number of WP instances you need
docker-compose scale wordpress=3 # or 50 depending on your class size

# Discover the IP of your DO droplet
docker-machine ip wp-demobox

# Shows you all the containers running, and the port they are bound to
docker-compose ps 

# When you're done, kill the droplet
docker-machine rm wp-demobox

# Unset the env vars for that machine you just destroyed
eval "$(docker-machine env -u)"
````

After the `docker-compose ps` command, you will have a list of all the ports that the WordPress instances are bound on. These will likely be sequential starting from some high port number. Each of these ports will be accessible on the host's IP address.
