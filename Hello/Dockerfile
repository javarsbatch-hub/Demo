
#FROM maven:3.9.9-eclipse-temurin-17 AS build
#WORKDIR /app
#COPY pom.xml .
#COPY src ./src
#RUN mvn -DskipTests package

#FROM eclipse-temurin:17-jre
#WORKDIR /app
#COPY --from=build /app/target/*.jar app.jar
#ENTRYPOINT [ "java","-jar","app.jar" ]

# ---------- Build Stage ----------
FROM maven:3.9.9-eclipse-temurin-17 AS build
WORKDIR /app

# Cache dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source and build
COPY src ./src
RUN mvn clean package -DskipTests

# ---------- Runtime Stage ----------
FROM eclipse-temurin:17-jre
WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

# Railway provides PORT automatically
ENV PORT=8080

EXPOSE 8080

ENTRYPOINT ["sh", "-c", "java -jar app.jar --server.port=8080"]