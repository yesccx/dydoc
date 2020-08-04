FROM ubuntu:18.04

MAINTAINER hightman, hightman@twomice.net

# Install required packages
RUN apt-get update -qq
RUN apt-get install -qy --no-install-recommends \
	wget make gcc g++ bzip2 zlib1g-dev

# Download & Install xunsearch-dev
RUN cd /root && wget -qO - http://www.xunsearch.com/download/xunsearch-full-dev.tar.bz2 | tar xj
RUN cd /root/xunsearch-full-* && sh setup.sh --prefix=/usr/local/xunsearch

RUN echo '' >> /usr/local/xunsearch/bin/xs-ctl.sh \
    && echo 'tail -f /dev/null' >> /usr/local/xunsearch/bin/xs-ctl.sh

# Configure it
#VOLUME ["/usr/local/xunsearch/data"]

WORKDIR /usr/local/xunsearch
RUN echo "#!/bin/sh" > bin/xs-docker.sh \
    && echo "rm -f tmp/pid.*" >> bin/xs-docker.sh \
    && echo "echo -n > tmp/docker.log" >> bin/xs-docker.sh \
    && echo "bin/xs-indexd -l tmp/docker.log -k start" >> bin/xs-docker.sh \
    && echo "sleep 1" >> bin/xs-docker.sh \
    && echo "bin/xs-searchd -l tmp/docker.log -k start" >> bin/xs-docker.sh \
    && echo "sleep 1" >> bin/xs-docker.sh \
    && echo "tail -f tmp/docker.log" >> bin/xs-docker.sh

ENTRYPOINT ["sh"]
CMD ["bin/xs-docker.sh"]