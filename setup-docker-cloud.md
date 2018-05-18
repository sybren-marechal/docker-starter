# Docker-compose

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration. To learn more about all the features of Compose, see [the list of features](https://docs.docker.com/compose/overview/#features).

## Docker-compose file

For this project we will use an example of VIVES Projectwerk.

```text
git clone https://github.com/Projectwerk2-2018/smart-campus-docker-compose.git
```

clone the project end go to the `docker-compose.yml` file.

```text
version: "3"services:version: "3"
services:
  frontend:
    image: projectwerk2/frontend:develop
    ports:
      - 80:5000
    restart: always
    environment:
     - REACT_APP_API_URL=http://192.168.99.100:8181
  backend:
    image: projectwerk2/backend
    ports:
      - 8181:8181
    environment:
      - APP_ENV=production
      - APP_KEY=${APP_KEY}
      - DB_HOST=database
      - DB_DATABASE=smartcampus
      - DB_USERNAME=smartcampus
      - DB_PASSWORD=smartcampus
    depends_on: 
      - database
    restart: always
  listener:
    image: projectwerk2/listener
    environment:
     - TTN_APPID=${TTN_APPID}
     - TTN_ACCESSKEY=${TTN_ACCESSKEY}
     - HTTP_HOST=backend
     - HTTP_PORT=8181
     - HTTP_PATH=/api/listener/
     - HTTP_HTTP=http
    restart: always
  database:
    image: mariadb
    environment:
     - MYSQL_ROOT_PASSWORD=smartcampus
     - MYSQL_DATABASE=smartcampus
     - MYSQL_USER=smartcampus
     - MYSQL_PASSWORD=smartcampus
    volumes:
     - database-data:/var/lib/mysql
    restart: always
volumes:
  database-data:
```

Currently we can divide the file into 4 pieces `frontend, backend, listener` and `database.`   
these project have all different variables.

 clone the repository from docker

```text
ports:
```

Here, you’ve mapped port 80 on the host operating system, to port 3000 from the container. That way, when you’ve moved this container to a production host, users of the application can go to the host machine’s port 80 and have those requests answered from the container on port 3000.





