# Architecture
![graphql-spring-mysql](media/graphql-javaspring-mysql-architecture.png)
The architecture that this development environment implements is a GraphQL API that talks to our SpringBoot Application that access data from MySQL database. To do this we will spin up 2 containers:
- Java Container: that will host GraphQL API and SpringBoot Application
- MySQL Container: what will host MySQL database


# Mount repository path to Container work directory
Clone the [GraphQL-MusicStore-Java](https://github.com/YilengYao/GraphQL-MusicStore-Java)https://github.com/YilengYao/GraphQL-MusicStore-Java

Add the path of [GraphQL-MusicStore-Maven folder](https://github.com/YilengYao/GraphQL-MusicStore-Java/tree/main/GraphQL-MusicStore-Maven) to .env folder as shown below
```
MUSIC_STORE_APPLICATION_PATH=.../GraphQL-MusicStore-Java/GraphQL-MusicStore-Maven
```
# Requirements
1. Install Docker
2. Install Docker Compose

# Build and Start Docker
Open up a terminal, cd into current directory, 
```
cd Local-MusicStore-Environment
```

Initiate docker swarm 

```
docker swarm init
```

Build and start the docker container
```
docker-compose build && docker-compose up
```

# Developing on Music Store Application
ssh into the music_store_application
```
docker exec -it music-store-application sh
```

Build your application one time
```
mvn clean spring-boot:run
```

# Calling GraphQL
Now go to http:localhost:8084/graphiql

and run the following query

```
query tracks {
  allTracks {
    track_id
		movie_title
    track_title
    tempo_name
    tempo_bpm
    composer {
      composer_id
      composer_name
    }
    instruments {
      instrument_id
      instrument_group
      instrument_name
    }
    in_library
    created
  }
}
```

# Debugging Tricks
If you are experience checksum errors because you modified the flyway migration scripts.

Since we mysql database is mounted in memory and not on hard disk. You can run the following commands.
```
docker-compose down
docker volume prune
docker-compose build && docker-compose up 
```
