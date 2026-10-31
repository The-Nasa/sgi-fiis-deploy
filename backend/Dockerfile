# Etapa 1: Compilación (Build con Maven y OpenJDK 17)
FROM maven:3.9-eclipse-temurin-17-alpine AS build
WORKDIR /app
COPY fiis/pom.xml .
COPY fiis/src ./src
RUN mvn clean package -DskipTests

# Etapa 2: Imagen final de ejecución
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
