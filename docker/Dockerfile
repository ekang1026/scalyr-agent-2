# base image that creates all necessary dependencies for
# the scalyr-agent
# NOTE: multi-stage builds require Docker 17.05 or greater
FROM python:2.7-slim as scalyr-dependencies
MAINTAINER Scalyr Inc <support@scalyr.com>

RUN apt-get update && apt-get install -y build-essential
RUN pip --no-cache-dir install --root /tmp/dependencies ujson yappi docker-py==1.10.3

# main image - copies dependencies from scalyr-dependencies and extracts
# the tar-zipped file containing the scalyr-agent code
FROM python:2.7-slim as scalyr
MAINTAINER Scalyr Inc <support@scalyr.com>

COPY --from=scalyr-dependencies  /tmp/dependencies/ /
ADD scalyr-docker-agent.tar.gz /

CMD ["/usr/sbin/scalyr-agent-2", "--no-fork", "--no-change-user", "start"]
