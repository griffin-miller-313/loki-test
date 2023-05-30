FROM golang:1.17.2 as build
# TOUCH_PROTOS signifies if we should touch the compiled proto files and thus not regenerate them.
# This is helpful when file system timestamps can't be trusted with make
ARG TOUCH_PROTOS
COPY . /src/loki
WORKDIR /src/loki
RUN make clean && (if [ "${TOUCH_PROTOS}" ]; then make touch-protos; fi) && make BUILD_IN_CONTAINER=false loki-canary

FROM alpine:3.13
RUN apk add --update --no-cache ca-certificates
COPY --from=build /src/loki/cmd/loki-canary/loki-canary /usr/bin/loki-canary
ENTRYPOINT [ "/usr/bin/loki-canary" ]
