FROM node:14 as builder

ENV SERVER_PATH /var/www
ENV ACTIVE_BRANCH "master"
ENV SCRIPTS_DIR /tmp/scripts
ENV NODE_ENV "production"
ENV PORT 3000
ENV MONGODB_URI "mongodb://mongodb/tc"


RUN apt-get update
RUN apt-get install -y \
        curl \
        git \
        sudo \
        python \
        python-dev \
        build-essential \
        jq \
        unzip

# install node dependencies
RUN mkdir -p /tmp/scripts
COPY . /tmp/scripts/

RUN chmod +x /tmp/scripts/build.sh
RUN /tmp/scripts/build.sh

RUN chmod +x /tmp/scripts/entry.sh

FROM node:14
COPY --from=builder /var/www /var/www
COPY --from=builder /tmp/scripts/entry.sh /tmp/scripts/entry.sh
RUN  cd /var/www/jic-homepage && npm ci
ENTRYPOINT ["/tmp/scripts/entry.sh"]
