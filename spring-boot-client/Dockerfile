FROM maven:3-eclipse-temurin-17-alpine
RUN apk add --no-cache docker-cli
COPY . .
CMD ["mvn", "compile" ,"jib:dockerBuild"]