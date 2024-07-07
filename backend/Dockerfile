# Etapa 1 - Crear un contenedor para el build, y empaquetar el backend
FROM maven:3.6.3-jdk-11-slim as builder
# Ejecucion
RUN groupadd --system --gid 1000 legolas
RUN useradd --system --gid legolas --uid 1000 --shell /bin/bash --create-home legolas
USER legolas
WORKDIR /home/legolas
RUN mkdir /home/legolas/app
COPY src /home/legolas/app/src
COPY pom.xml /home/legolas/app
RUN mvn -f /home/legolas/app clean package -DskipTests

# Etapa 2 - Crear contenedor con Tomcat para producción
FROM openjdk:11.0.4-jre-slim as prod
# Variables
ARG PORT=${PORT}
ARG DB_URL=${DB_URL}
ARG DB_USERNAME=${DB_USERNAME}
ARG DB_PASSWORD=${DB_PASSWORD}
ARG DB_METHOD=${DB_METHOD}
ARG JWT_NAME=${JWT_NAME}
ARG JWT_KEY=${JWT_KEY}
ARG JWT_EXPIRES=${JWT_EXPIRES}
ARG JWT_REFRESH_EXPIRES=${JWT_REFRESH_EXPIRES}
ARG MAIL_HOST=${MAIL_HOST}
ARG MAIL_PORT=${MAIL_PORT}
ARG MAIL_USERNAME=${MAIL_USERNAME}
ARG MAIL_PASSWORD=${MAIL_PASSWORD}
ARG FRONT_BASE_URL=${FRONT_BASE_URL}
ARG AWS_ACCESS_KEY=${AWS_ACCESS_KEY}
ARG AWS_SECRET_KEY=${AWS_SECRET_KEY}
ARG PAYPAL_CLIENT_ID=${PAYPAL_CLIENT_ID}
ARG PAYPAL_CLIENT_SECRET=${PAYPAL_CLIENT_SECRET}
ARG SUPER_ADMIN_USERNAME=${SUPER_ADMIN_USERNAME}
ARG SUPER_ADMIN_PASSWORD=${SUPER_ADMIN_PASSWORD}
ARG GOOGLE_DM_API_KEY=${GOOGLE_DM_API_KEY}
ARG DISTANCIA_RESTAURANTE_MAX=${DISTANCIA_RESTAURANTE_MAX}

# Ejecucion
RUN groupadd --system --gid 1000 gandalf
RUN useradd --system --gid gandalf --uid 1000 --shell /bin/bash --create-home gandalf
USER gandalf
WORKDIR /home/gandalf
COPY --from=builder /home/legolas/app/target/backend-*.jar /home/gandalf/app/backend.jar
#CMD ["sh", "-c", "java -D  -Dserver.port=${PORT} -Ddatabase.url=${DB_URL} -Ddatabase.username=${DB_USERNAME} -Ddatabase.pass=${DB_PASSWORD} -Ddatabase.method=${DB_METHOD} -Djwt.name=${JWT_NAME} -Djwt.key=${JWT_KEY} -Djwt.expires=${JWT_EXPIRES} -Djwt.refresh.expires=${JWT_REFRESH_EXPIRES} -Dmail.host=${MAIL_HOST} -Dmail.port=${MAIL_PORT} -Dmail.username=${MAIL_USERNAME} -Dmail.pass=${MAIL_PASSWORD} -Dfronturl.base=${FRONT_BASE_URL} -Daws.accessKey=${AWS_ACCESS_KEY} -Daws.secretKey=${AWS_SECRET_KEY} -jar /home/gandalf/app/backend.jar"]
ENTRYPOINT [ "java", "-D spring.datasource.url=${DB_URL}", "-D spring.datasource.username=${DB_USERNAME}", "-D spring.datasource.password=${DB_PASSWORD}", "-D spring.jpa.hibernate.ddl-auto=${DB_METHOD}", "-D jwt.auth.app=${JWT_NAME}", "-D jwt.auth.secret_key=${JWT_KEY}", "-D jwt.auth.expires_in=${JWT_EXPIRES}", "-D jwt.auth.refresh.expires_in=${JWT_REFRESH_EXPIRES}", "-D spring.mail.host=${MAIL_HOST}", "-D spring.mail.port=${MAIL_PORT}", "-D spring.mail.username=${MAIL_USERNAME}", "-D spring.mail.password=${MAIL_PASSWORD}", "-D front.base.url=${FRONT_BASE_URL}", "-D server.port=${PORT}", "-D aws.accessKey=${AWS_ACCESS_KEY}", "-D aws.secretKey=${AWS_SECRET_KEY}", "-D paypal.client.id=${PAYPAL_CLIENT_ID}", "-D paypal.client.secret=${PAYPAL_CLIENT_SECRET}", "-D super.admin.username=${SUPER_ADMIN_USERNAME}", "-D super.admin.password=${SUPER_ADMIN_PASSWORD}", "-D google.dm.api.key=${GOOGLE_DM_API_KEY}", "-D distancia.maxima=${DISTANCIA_RESTAURANTE_MAX}", "-jar", "/home/gandalf/app/backend.jar" ]