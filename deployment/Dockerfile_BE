FROM openjdk:latest as build

WORKDIR /build

COPY pom.xml mvnw ./

COPY .mvn /build/.mvn

RUN ./mvnw -B dependency:go-offline

COPY . .

ARG aws_access_key
ARG aws_secret_key
ENV aws_access_key=${aws_access_key}
ENV aws_secret_key=${aws_secret_key}

RUN ./mvnw clean package

FROM openjdk:latest as runtime

ENV SPRING_DATASOURCE_URL=jdbc:postgresql://192.168.1.57:5432/auctionapp?useSSL=false
ENV SPRING_DATASOURCE_USERNAME=postgres
ENV SPRING_DATASOURCE_PASSWORD=auction
ENV APP_JWTSECRET=d178899b7beeb99ec2ebdad4636c7e21a2364494414013f78dd5a00725b77f14
ENV STRIPE_SECRET_KEY=51N3gQwJfjmrWeWPQ$

WORKDIR /app

COPY --from=build /build/target/auctionapp-0.0.1-SNAPSHOT.jar /app

EXPOSE 8080

CMD ["java", "-jar", "auctionapp-0.0.1-SNAPSHOT.jar"]
