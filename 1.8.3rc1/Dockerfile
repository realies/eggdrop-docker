FROM alpine:3.7
MAINTAINER Geo Van O <geo@eggheads.org>

RUN adduser -S eggdrop

# grab su-exec for easy step-down from root
RUN apk add --no-cache 'su-exec>=0.2'

RUN apk add --no-cache tcl bash openssl
RUN apk add --no-cache --virtual egg-deps tcl-dev wget ca-certificates make tar gpgme build-base openssl-dev \
  && wget ftp://ftp.eggheads.org/pub/eggdrop/source/1.8/eggdrop-1.8.3rc1.tar.gz \
  && wget ftp://ftp.eggheads.org/pub/eggdrop/source/1.8/eggdrop-1.8.3rc1.tar.gz.asc \
  && gpg --keyserver pgp.mit.edu --recv-key E01C240484DE7DBE190FE141E7667DE1D1A39AFF \
  && gpg --batch --verify eggdrop-1.8.3rc1.tar.gz.asc eggdrop-1.8.3rc1.tar.gz \
  && rm eggdrop-1.8.3rc1.tar.gz.asc \
  && tar -zxvf eggdrop-1.8.3rc1.tar.gz \
  && rm eggdrop-1.8.3rc1.tar.gz \
  && ( cd eggdrop-release-1.8.3 \
    && ./configure \
    && make config \
    && make \
    && make install DEST=/home/eggdrop/eggdrop ) \
  && rm -rf eggdrop-release-1.8.3 \
  && mkdir /home/eggdrop/eggdrop/data \
  && chown -R eggdrop /home/eggdrop/eggdrop \
  && apk del egg-deps

ENV NICK=""
ENV SERVER=""
ENV LISTEN="3333"
ENV OWNER=""
ENV USERFILE="eggdrop.user"
ENV CHANFILE="eggdrop.chan"

WORKDIR /home/eggdrop/eggdrop
EXPOSE 3333
COPY entrypoint.sh /home/eggdrop/eggdrop
COPY docker.tcl /home/eggdrop/eggdrop/scripts/

ENTRYPOINT ["/home/eggdrop/eggdrop/entrypoint.sh"]
CMD ["eggdrop.conf"]
