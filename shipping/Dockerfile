#
# Build
#
FROM openjdk:17-slim as build

RUN apt-get update && \
    apt-get install -y maven && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /opt/shipping

COPY pom.xml /opt/shipping/
COPY src /opt/shipping/src/

RUN mvn clean package -DskipTests

#
# Run
#
FROM openjdk:17-slim

EXPOSE 8080

WORKDIR /opt/shipping

ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]
