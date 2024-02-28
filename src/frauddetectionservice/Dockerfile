# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0


FROM gradle:8-jdk17 AS builder

WORKDIR /usr/src/app/

COPY ./src/frauddetectionservice/ ./
COPY ./pb/ ./src/main/proto/
RUN gradle shadowJar

# -----------------------------------------------------------------------------

FROM gcr.io/distroless/java17-debian11

ARG version=2.0.0
WORKDIR /usr/src/app/

COPY --from=builder /usr/src/app/build/libs/frauddetectionservice-1.0-all.jar ./
ADD --chmod=644 https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v$version/opentelemetry-javaagent.jar /app/opentelemetry-javaagent.jar
ADD --chmod=644 https://cgx.jfrog.io/artifactory/maven/com/coralogix/opentelemetry-java-extensions/1.0/opentelemetry-java-extensions-1.0-jar-with-dependencies.jar /app/opentelemetry-cx-extension.jar
ENV JAVA_TOOL_OPTIONS="-javaagent:/app/opentelemetry-javaagent.jar -Dotel.javaagent.extensions=/app/opentelemetry-cx-extension.jar"

ENTRYPOINT [ "java", "-jar", "frauddetectionservice-1.0-all.jar" ]
