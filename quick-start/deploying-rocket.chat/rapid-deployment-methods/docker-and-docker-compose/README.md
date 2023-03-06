# Docker & Docker Compose

Deploying Rocket.Chat with Docker and Docker Compose is as easy as it can get.

### Installing Docker and Docker Compose

* If you don't have already, make sure you have [Docker](https://docs.docker.com/install) and [Docker-compose](https://docs.docker.com/compose/install/) (v2 is required) installed and operational. To quickly do that you can use Docker's official helper script

```bash
curl -L https://get.docker.com | sh
```

* Navigate to any directory of your choice create a `docker-compose.yml` based on [our example](https://github.com/RocketChat/Docker.Official.Image/blob/master/compose.yml). OR you can download directly by executing the following command on your terminal.

```bash
curl -L https://raw.githubusercontent.com/RocketChat/Docker.Official.Image/master/compose.yml -O
```

#### **Editing Variables:**

If you are using MongoDB Atlas as the database provider, edit the value of the `MONGO_URL` variable in your compose file to be your connection string in this format

`MONGO_URL=mongodb://<user>:<pass>@host1:27017,host2:27017,host3:27017/<databaseName>?replicaSet=<replicaSet>&ssl=true&authSource=admin`

Environment variables are set using a `.env` file. See the example in [here](https://github.com/RocketChat/Docker.Official.Image/blob/master/env.example). If you cloned the repo initially, you can just rename the example file `cp env.example .env`.

* Set `RELEASE` to your desired Rocket.Chat version, defaults to `latest`. Keeping it `latest` is **not recommended** (see section [Docker Images Available](docker-containers/available-images.md) )
* Edit `ROOT_URL` from the default `http://localhost:3000` to match your domain name or IP address as you wish
*   If you have a registration token to automatically register the workspace you can provide with:

    ```
    REG_TOKEN={your token here} docker-compose up -d
    ```
* Next, start up the container by executing:

```
docker compose up -d
```

This is going to:

* Start a MongoDB service named `mongodb`.
* Start a service `rocketchat`, that will also wait for `mongodb` to be ready.

Mongo supports 24 x 7 operations and live backup. You should not need to restart it too frequently. See [MongoDB documentation](https://www.mongodb.com/docs/manual/) for proper operation and management of a Mongo server.

Optionally, if you want to manage your messages and configuration information, edit the file again to uncomment the volume mounts. Make sure you have a `data` subdirectory to mount and store the data.

### Updating Rocket.Chat Docker Image

To update the `rocketchat` docker image to the latest version, update the `RELEASE` value in your `.env` file, then simply run `docker compose up -d`. Your data should not be affected by this, since it's located in the `mongo` image.
