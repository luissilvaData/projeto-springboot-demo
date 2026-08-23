# ==========================================
# Estágio 1: Build
# ==========================================
FROM maven:3.9.6-eclipse-temurin-21 AS builder

WORKDIR /app

# Copia o pom.xml e baixa as dependências (melhora o cache do Docker)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copia o código-fonte da aplicação
COPY src ./src

# Executa o build do projeto ignorando os testes para ser mais rápido
RUN mvn clean package -DskipTests

# ==========================================
# Estágio 2: Runtime (Imagem Final)
# ==========================================
FROM eclipse-temurin:21-jre-jammy

WORKDIR /app

# Copia apenas o arquivo .jar gerado no Estágio 1
COPY --from=builder /app/target/*.jar app.jar

# Expõe a porta padrão do Spring Boot
EXPOSE 8080

# Comando para iniciar a aplicação
ENTRYPOINT ["java", "-jar", "app.jar"]