FROM alpine:edge

LABEL org.opencontainers.image.authors="anton.feldmann@gmail.com"
LABEL version="1.0"
LABEL description="an docker distcc worker or master"

#install distcc with ccache
ARG REQUIRE="sudo build-base clang distcc-pump distcc ccache cloc"
RUN apk update && apk upgrade \
      && apk add --no-cache ${REQUIRE}

#### ADD DEFAULT USER ####
ARG USER=distcc
ENV USER ${USER}
RUN echo "${USER}   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# build ccache (see: https://wilsonhongblog.wordpress.com/2016/05/24/using-ccache-on-distcc-server/)
RUN mkdir -p /cache
RUN chown distcc: /cache
RUN find /usr/lib/ccache/ -type l > /etc/distcc/DISTCC_CMDLIST

ENV DISTCC_CMDLIST /etc/distcc/DISTCC_CMDLIST
ENV CCACHE_DIR /cache
ENV PATH /usr/lib/ccache:$PATH

# This is the operations port
EXPOSE 3632
# This is the statistics port
EXPOSE 3633

USER distccd
