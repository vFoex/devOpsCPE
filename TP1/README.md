# TP1

## 1-1 Database

```
Dockerfile :
    - FROM {{image}}
    - ENV {{NOM_VARIABLE}}={{value}}
    - ADD {{script.sql}} /docker-entrypoint-initdb.d  //script executer pendant le run de l'image

Commandes :
    - docker network create {{network-name}} // Create Network
    - docker network ls // Liste networks

    - docker volume create {{volume-name}} // Create Volume
    - docker volume ls // Liste volumes

    - docker build -t vfoex/database . //Build the image from the Dockerfile

    - docker run --network="app-network" --name database -e POSTGRES_PASSWORD=123 -p 5432:5432 -d vfoex/database //Run the image
        --network to define the network
        --name to name the contener
        -e {{VARIABLE}}={{value}} to create/override an environment variable
        -p to expose the port
        -d to not being attached to the console
```

## 1-2 Why do we need a multistage build?

On a besoin du multistage pour pouvoir premièrement, installer les dependencies et compiler le programme dans le premier stage, puis par la suite le deuxieme stage sert a run ce qui est compilé dans le premier. 

```Dockerfile
# Build
FROM maven:3.8.6-amazoncorretto-17 AS myapp-build #récupération de l''image java pour compile le projet avec maven
ENV MYAPP_HOME /opt/myapp #set la variable d''envirenement MYAPP_HOME 
WORKDIR $MYAPP_HOME #set l''environement de travail
COPY pom.xml .
COPY src ./src
RUN mvn package -DskipTests

# Run
FROM amazoncorretto:17 #image qui va servir à lancer le point jar
ENV MYAPP_HOME /opt/myapp #set la variable d''envirenement MYAPP_HOME 
WORKDIR $MYAPP_HOME #set l''environement de travail
COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar
#récupération du fichier .jar compilé
ENTRYPOINT java -jar myapp.jar #execute le fichier jar compilé
```