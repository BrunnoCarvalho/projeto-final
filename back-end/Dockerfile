# --- FASE 1: Build (Construção) ---
# Usamos uma imagem com o JDK completo para compilar nosso código
FROM eclipse-temurin:17-jdk-alpine as builder

# Define o diretório de trabalho dentro do contêiner
WORKDIR /app

# Copia os arquivos de configuração do Maven e baixa as dependências
COPY .mvn/ .mvn
COPY mvnw pom.xml ./
RUN ./mvnw dependency:go-offline

# Copia o código-fonte da nossa aplicação
COPY src ./src

# Compila o projeto e gera o arquivo .jar, pulando os testes
RUN ./mvnw package -DskipTests


# --- FASE 2: Run (Execução) ---
# Usamos uma imagem apenas com o JRE, que é mais leve, para rodar a aplicação
FROM eclipse-temurin:17-jre-alpine

# Define o diretório de trabalho
WORKDIR /app

# Copia apenas o arquivo .jar compilado da fase de build
COPY --from=builder /app/target/*.jar app.jar

# Expõe a porta 8080, que é a padrão do Spring Boot
EXPOSE 8080

# O comando final que será executado quando o contêiner iniciar
ENTRYPOINT ["java", "-jar", "app.jar"]