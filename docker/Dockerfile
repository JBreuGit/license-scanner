FROM eclipse-temurin:21-jre-jammy AS jre

# Python 3.9 is the newest interpreter supported by the ScanCode Toolkit
# release pinned in requirements.txt; the Java 21 runtime required by the
# service jar is copied in from the Temurin image above.
FROM python:3.9-bookworm

ENV JAVA_HOME=/opt/java/openjdk
ENV PATH="${JAVA_HOME}/bin:${PATH}"
COPY --from=jre ${JAVA_HOME} ${JAVA_HOME}

RUN apt-get update \
    && apt-get install -y --no-install-recommends jq \
    && rm -rf /var/lib/apt/lists/*

RUN python -m pip install --upgrade pip

COPY docker/requirements.txt /app/requirements.txt
RUN pip install -r /app/requirements.txt scancode-toolkit[full]

COPY build/libs/license-scanner-service*.jar /app/service.jar
COPY docker/start.sh /app/start.sh

EXPOSE 8080

ADD REPO .
ADD TAGS .

RUN mkdir /root/.ssh

ENTRYPOINT ["/app/start.sh"]
