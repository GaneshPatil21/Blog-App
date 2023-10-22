# Blog App - Spring Boot Project

Table of Contents

- About
- Prerequisites
- Services
- Sample Requests
- How to Run
- Troubleshooting Docker
- Additional Notes

## About

Blog App is a sample Java/Maven/Spring Boot application for a blog platform. It provides a set of RESTful web services for managing blog entries. This application is packaged as a JAR file that can be easily run within a Docker container. The embedded Tomcat server eliminates the need for separate configuration.

## Prerequisites

Before running the Blog App, ensure you have the following prerequisites installed:

- Java 1.8 or higher
- Maven
- Docker

## Services

This API provides the following services:

- Create a new blog entry.
- Update a blog entry.
- Delete a blog entry.
- View a particular blog entry.
- View all blog entries in the database.

## Sample Requests

- Creating a New Entry

`POST http://localhost:8080/blog/create`

```
{
  "createBy": "string",
  "data": "string",
  "lastUpdateBy": "string"
}
```

- Updating an Entry

`PUT http://localhost:8080/blog/update`

```
{
  "id": "2",
  "data": "string",
  "lastUpdateBy": "string"
}
```

- Deleting an Entry

`DELETE http://localhost:8080/blog/delete/{id}`

- To view a particular blog entry:

`GET http://localhost:8080/blog/view/{id}`

- To view all blog entries:

`GET http://localhost:8080/blog/viewall`

## How to Run

Follow these steps to run the "Blog App" project:

Unzip the source code to a suitable location.

Open your command line interface with administrative privileges (Cmd on Windows, Terminal on macOS).

Navigate to the project directory.

Build the project and run tests by executing the following command:

mvn clean package

This will create a JAR file named blog-app-1.0.jar inside the "target" folder.

Create a Docker container and deploy the application within it:

Open your command line interface with administrative privileges.

Navigate to the project directory.

Run the following commands to set up and start a MySQL database in a Docker container. This will download the MySQL 5.6 image from Docker Hub and run it for you:

- docker pull mysql:5.6
- docker run --name=mysql-db --env="MYSQL_ROOT_PASSWORD=root" --env="MYSQL_PASSWORD=root" --env="MYSQL_DATABASE=blog" mysql:5.6

Once MySQL is up and running, it's time to start our REST application. Run the following commands to create a Docker image of our application named blog-app-1.0 and link it with the MySQL container. It will run our application on port 8080:

- docker build -f Dockerfile -t blog-app-1.0 .
- docker run -t --name blog-app-1.0 --link mysql-db:mysql -p 8080:8080 blog-app-1.0

After running the above command, you should see the following output in the console:

```
[main] o.s.b.w.embedded.tomcat.TomcatWebServer : Tomcat started on port(s): 8080 (http) with the context path ''
[main] com.blog.app.LaunchApplication : Started LaunchApplication in 8.054 seconds (JVM running for 9.083)
```

You can access the application at http://localhost:8080/swagger-ui.html#/.

## Troubleshooting Docker

If you encounter an error like the one below while starting the Docker container:

Error response from daemon: Conflict. The container name "/blog-app-1.0" is already in use

You can forcefully kill the container by following these commands:

Restart the Docker engine.

Stop and remove the MySQL container:

- docker stop mysql-db
- docker rm -f mysql-db

Stop and remove the application container:

- docker stop blog-app-1.0
- docker rm -f blog-app-1.0

Restart the containers:

- docker pull mysql:5.6
- docker run --name=mysql-db --env="MYSQL_ROOT_PASSWORD=root" --env="MYSQL_PASSWORD=root" --env="MYSQL_DATABASE=blog" mysql:5.6
- docker build -f Dockerfile -t blog-app-1.0 .
- docker run -t --name blog-app-1.0 --link mysql-db:mysql -p 8080:8080 blog-app-1.0

## Additional Notes

If you decide to change the JAR name of the application, make sure to update all the Docker commands accordingly:

- docker build -f <docker-file-name> -t <project-jar-name> .
- docker run -p <physical machine port>:<docker port> <image-name>

If you change the name of the MySQL image, ensure you update all the MySQL-related Docker commands and the database properties in the application.properties file accordingly:

- docker run --name <some-app> --link <mysql-db-name>:mysql -d <application-using-mysql>
- docker run -it --link <mysql-db-name>:mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -proot'
