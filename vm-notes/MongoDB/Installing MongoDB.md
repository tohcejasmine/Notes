# Installing MongoDB

## Using Docker

1. Ensure Docker is installed

2. Create folder structure like:

   ```bash
   | mongodb
   ---| db
   ---| config
   ---| log
   ```

3. Run Docker container with MongoDB container image

   * Enable authentication

   ```shell
   docker run -d -p 27017:27017 \
   --name mongodb \
   -v /path/to/mongodb/db:/data/db \
   -v /path/to/mongodb/config:/data/configdb \
   -v /path/to/mongodb/log:/data/log \
   mongo --auth
   ```

4. Check if `mongodb` container is running correctly

   ```shell
   docker ps -a | grep mongo
   ```

5. Enter container and login to the default 'admin' database

   * Execute command `mongo admin`

   ```shell
   docker exec -it mongodb mongo admin
   ```

6. Create a user with **administrator** role

   ```shell
   > db.createUser({
   	user: 'admin',
   	pwd: 'mongoisfun123',
   	roles: [{
   		role: 'userAdminAnyDatabase',
   		db: 'admin'
   	}]
   })
   > exit
   ```

7. Login as administrator

   * Returns 1

   ```shell
   docker exec -it mongodb mongo admin
   > db.auth('admin', 'mongoisfun123')
   ```

8. Create other users by specifying their information and roles

   ```shell
   > db.createUser({
   	user: 'someuser',
   	pwd: 'somepw456',
   	roles: [{
   		role: 'dbOwner',
   		db: '<db_name_here>'
   	}]
   })
   ```

9. To forcefully stop and remove container

   ```shell
   docker rm -f mongodb
   ```

10. Consider creating a `.sh` launch file for starting the docker container

    * `--restart=always`: Restart when Docker daemon restarts
    
    ```bash
    # Stop existing containers
    docker rm -f mongodb
    # Run container
    docker run -d --restart=always \
    	-p 27017:27017 \
    	--name mongodb \
    	-v /path/to/mongodb/db:/data/db \
    	-v /path/to/mongodb/config:/data/configdb \
    	-v /path/to/mongodb/log:/data/log \
    	mongo
    ```

## Using Docker-Compose

1. Ensure Docker, Docker Compose is installed

2. Create folder structure like:

   ```shell
   mkdir -pv mongodb/database
   | mongodb
   ---| database # maps container's database location
   ```

3. Create `docker-compose.yml` file

   * `version`: dictates options available in compose file, minimum supported Docker engine version
   * `mongodb`: name of service and container
   * `environment`: environment variables to define "user" and "group" of container
   * `volumes`: **bind-mount volumes** - mapping local database folder to container database folder
   * `ports`: map local port to internal port
   * `restart`: restart unless set by user

   ```yaml
   version: "3.8"
   services:
     mongodb:
       image : mongo
       container_name: mongodb
       environment:
        - PUID=1000
        - PGID=1000
       volumes:
        - /home/barry/mongodb/database:/data/db
       ports:
        - 27017:27017
       restart: unless-stopped
   ```

4. Create MongoDB container

   * `up`: pull image and create container using given parameters in compose file
   * `-d`: run container in detached mode

   ```shell
   sudo docker-compose up -d
   ```

5. Check if containers are running

   ```shell
   sudo docker ps -a
   ```

6. Access terminal of MongoDB container

   ```shell
   sudo docker exec -it mongodb bash
   ```

## Using `apt` Package Manger

1. Import public key used by package management system

   * Returns OK

   ```shell
   wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
   ```

2. Create list file for MongoDB

   * File is `/etc/apt/sources.list.d/mongodb-org-4.4.list`
   * For Ubuntu 20.04 (Focal)

   ```shell
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
   ```

3. Reload local package database

   ```shell
   sudo apt-get update
   ```

4. Install MongoDB packages

   * Directories created: `/var/lib/mongodb`, `/var/log/mongodb`
   * Configuration file: `/etc/mongod.conf`

   ```shell
   sudo apt-get install -y mongodb-org
   ```

5. Start MongoDB

   ```shell
   sudo systemctl start mongod
   
   # Run if error 'Failed to start mongod.service: Unit mongod.service not found.', then run prev command again
   sudo systemctl daemon-reload
   ```

6. Verify that MongoDB started successfully

   ```shell
   sudo systemctl status mongod
   
   # To enable MongoDB will start following a system reboot
   sudo systemctl enable mongodb
   ```

7. Begin using MongoDB

   * Starts a shell on the same host machine as `mongod` (localhost, default port 27017)

   ```shell
   mongo
   ```

8. To stop MongoDB

   ```shell
   sudo systemctl stop mongod
   ```

9. To restart MongoDB

   ```shell
   sudo systemctl restart mongod
   ```

## Default MongoDB Ports

| Default Port | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| 27017        | The default port for `mongod` and `mongos` instances. You can change this port with `port` or `--port`. |
| 27018        | The default port for `mongod` when running with `--shardsvr` command-line option or the `shardsvr` value for the `clusterRole` setting in a configuration file. |
| 27019        | The default port for `mongod` when running with `--configsvr` command-line option or the `configsvr` value for the `clusterRole` setting in a configuration file. |

## References

1. https://dev.to/jemaloqiu/install-mongodb-with-docker-in-ubuntu-18-04-4pbi
2. https://www.bmc.com/blogs/mongodb-docker-container/
3. https://docs.mongodb.com/manual/reference/default-mongodb-port/
4. https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/