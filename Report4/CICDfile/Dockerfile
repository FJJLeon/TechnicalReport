FROM tomcat:9-jre8
RUN apt-get update \
    && apt-get install -y --no-install-recommends cowsay \
    && rm -rf /var/lib/apt/lists/*
ENV PATH "$PATH:/usr/games"

ADD ./target/Mybk-iteration3-0.0.1-SNAPSHOT.war /usr/local/tomcat/webapps/

ENTRYPOINT ["cowsay"]
CMD ["Hello, World!"]