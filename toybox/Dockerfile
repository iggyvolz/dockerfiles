# ideally, we'd use official static binary releases from http://landley.net/toybox/downloads/binaries/ but they don't include the (pending) "sh" implementation so they're not quite enough for a full system yet
# https://github.com/landley/toybox/commits/master/toys/pending/sh.c

FROM alpine:3.15 AS build

RUN set -eux; \
	apk add --no-cache \
		bash \
		gcc \
		linux-headers \
		make \
		musl-dev \
	;

WORKDIR /toybox

# https://landley.net/toybox/downloads/?C=M;O=D
# https://github.com/landley/toybox/releases
ENV TOYBOX_VERSION 0.8.5

RUN set -eux; \
	wget -O toybox.tgz "https://landley.net/toybox/downloads/toybox-$TOYBOX_VERSION.tar.gz"; \
	tar -xf toybox.tgz --strip-components=1; \
	rm toybox.tgz

RUN make root BUILTIN=1
# (we set "BUILTIN=1" to avoid cpio / initramfs creation because we aren't building / don't need a kernel)

FROM scratch
COPY --from=build /toybox/root/host/fs/ /
CMD ["sh"]
