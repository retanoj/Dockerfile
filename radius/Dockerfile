FROM alpine:3.18
LABEL maintainer=retanoj

ARG branch=release_3_0_26
ARG WKDIR=/freeradius

ENV TZ=Etc/UTC
ENV PATH=${WKDIR}/bin:${WKDIR}/sbin:${PATH}


RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
	&&  echo $TZ > /etc/timezone \
	&&  apk add --no-cache \
			talloc \
			ca-certificates \
			bash \
			libwbclient \
			gdbm \
	&&  apk add --no-cache --virtual .build-deps \
			talloc-dev \
			alpine-sdk \
			git \
			linux-headers \
			openssl-dev \
			openldap-dev \
			ruby \
			gdbm-dev \
	&&  apk add --no-cache \
			tar \
			freeradius-mysql \
	&&  git clone -b ${branch} --depth=1 https://github.com/FreeRADIUS/freeradius-server.git \
	&&  cd freeradius-server \
	&&  ./configure --sysconfdir ${WKDIR}/etc --bindir ${WKDIR}/bin --sbindir ${WKDIR}/sbin \
	&&  make install \
	&&  cd .. \
	&&  rm -Rf freeradius-server \
	&&  apk del .build-deps

WORKDIR ${WKDIR}

EXPOSE 1812/tcp
EXPOSE 1812/udp

ENTRYPOINT ["radiusd", "-f"]
