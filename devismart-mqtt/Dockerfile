ARG BUILD_FROM
FROM $BUILD_FROM AS build
ARG JAVA_VERSION=21

LABEL org.opencontainers.image.description="$(cat README.md)"

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
		git \
		xmlstarlet \
		maven \
        openjdk-${JAVA_VERSION}-jre-headless \
        zip && \
    c_rehash && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory inside the container
WORKDIR /app

# force to redownload
ADD "https://api.github.com/repos/bob-tm/ha-devi-mqtt/commits?per_page=1" latest_commit
RUN git clone https://github.com/bob-tm/ha-devi-mqtt

RUN mv ./ha-devi-mqtt/src ./src
RUN mv ./ha-devi-mqtt/pom.xml ./pom.xml
RUN mv ./ha-devi-mqtt/auto-discovery-templates  ./auto-discovery-templates

# change Java version to 17 (21 in latest versions)
RUN xmlstarlet ed --inplace -N pom=http://maven.apache.org/POM/4.0.0 -u "/pom:project/pom:properties/pom:maven.compiler.source" --value "21" ./pom.xml
RUN xmlstarlet ed --inplace -N pom=http://maven.apache.org/POM/4.0.0 -u "/pom:project/pom:properties/pom:maven.compiler.target" --value "21" ./pom.xml

# download dependencies
RUN mvn dependency:go-offline

# Build the project
RUN mvn clean package -DskipTests

# Stage 2: Create the final image to run the application
FROM $BUILD_FROM
ARG JAVA_VERSION=21

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
        locales \
        locales-all \
        openjdk-${JAVA_VERSION}-jre-headless \
        zip && \
    c_rehash && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
	
# ssh server 
RUN apt-get update && apt-get install -y python3


# Set the working directory inside the container
WORKDIR /app

# Create a directory for configuration files
RUN mkdir /app/auto-discovery-templates

# Copy the JAR file from the build stage
COPY --from=build /app/target/ha-devi-mqtt.jar ./ha-devi-mqtt.jar
COPY --from=build /app/auto-discovery-templates /app/auto-discovery-templates

# Expose volume with configuration and userdata dir
VOLUME /config

#config script
COPY config.sh /config.sh
RUN chmod +x /config.sh

# Set working directory and entrypoint
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
