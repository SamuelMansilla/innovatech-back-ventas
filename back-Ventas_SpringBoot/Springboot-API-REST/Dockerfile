# ==========================================
# ETAPA 1: Compilación y Construcción (Maven)
# ==========================================
FROM maven:3.9-eclipse-temurin-17-alpine AS builder
WORKDIR /app

# Copiamos el archivo pom.xml para descargar las dependencias en caché
COPY pom.xml .
RUN mvn dependency:go-offline

# Copiamos el código fuente y compilamos el archivo .jar (omitiendo tests)
COPY src ./src
RUN mvn clean package -DskipTests

# ==========================================
# ETAPA 2: Entorno de Ejecución Ligero y Seguro
# ==========================================
FROM eclipse-temurin:17-jre-alpine

# Creamos un usuario de sistema sin privilegios para Spring
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring
WORKDIR /app

# Copiamos el JAR compilado desde la etapa anterior
COPY --from=builder /app/target/*.jar app.jar

# Exponemos el puerto interno de la aplicación
EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]