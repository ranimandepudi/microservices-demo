ARG TARGETPLATFORM

FROM --platform=$TARGETPLATFORM eclipse-temurin:21-jdk-jammy AS builder

WORKDIR /app

COPY build.gradle settings.gradle gradlew ./
COPY gradle gradle
RUN chmod +x gradlew

COPY . .
RUN chmod +x gradlew && ./gradlew installDist --no-daemon

FROM --platform=$TARGETPLATFORM eclipse-temurin:21-jre-jammy

WORKDIR /app
COPY --from=builder /app .

EXPOSE 9555
ENTRYPOINT ["/app/build/install/hipstershop/bin/AdService"]
