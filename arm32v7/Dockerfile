#! Arm32v7 version

#* Build
FROM arm32v7/openjdk AS BUILD

ARG PAPERMC_CI_URL=https://papermc.io/api/v1/paper/1.15.2/latest/download

WORKDIR /opt/build

# Download paperclip
ADD ${PAPERMC_CI_URL} paperclip.jar

# Run paperclip and obtain patched jar
RUN java -Xmx500M -jar /opt/build/paperclip.jar; exit 0

# Copy built jar
RUN mv /opt/build/cache/patched*.jar papermc.jar

#* Runtime
FROM arm32v7/openjdk AS runtime

# Obtain runable jar from build stage
COPY --from=BUILD /opt/build/papermc.jar /opt/build/papermc.jar

# Volume for external data
VOLUME "/data"

# Expose mc port
EXPOSE 25565/tcp

# Memory that jvm max can use
ARG JVM_MEMORY=3G
ENV JVM_MEMORY=$JVM_MEMORY

# Custom JVM arguments
ARG CUSTOM_JVM_ARGS=""
ENV CUSTOM_JVM_ARGS=$CUSTOM_JVM_ARGS

WORKDIR /data

# Run papermc.jar
CMD java -Dcom.mojang.eula.agree=true -Xmx$JVM_MEMORY ${CUSTOM_JVM_ARGS} -XX:+UseG1GC -jar /opt/build/papermc.jar nogui
