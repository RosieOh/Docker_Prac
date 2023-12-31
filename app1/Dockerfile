FROM openjdk:17-alpine

WORKDIR /usr/src/app

ARG WAR_PATH=./build/libs

COPY ./build/libs/app1-0.0.1-SNAPSHOT.war ./build/libs/app1-0.0.1-SNAPSHOT.war

CMD ["java","-jar","./build/libs/app1-0.0.1-SNAPSHOT.war"]