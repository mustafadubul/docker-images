ARG GO_VERSION

FROM golang:${GO_VERSION}

# Install protoc-gen-go.
ARG PROTOC_VERSION
RUN set -ex ;\
    go get -d -u github.com/golang/protobuf/protoc-gen-go ;\
    git -C ${GOPATH}/src/github.com/golang/protobuf checkout $PROTOC_VERSION ;\
    go install -v github.com/golang/protobuf/protoc-gen-go ;\
    go clean -n -cache

WORKDIR /

# Install protocol buffer compiler.
ARG PROTOBUF_VERSION
ADD https://github.com/google/protobuf/releases/download/v${PROTOBUF_VERSION}/protobuf-cpp-${PROTOBUF_VERSION}.tar.gz /tmp/protobuf-cpp-${PROTOBUF_VERSION}.tar.gz
RUN set -ex ;\
    apt-get update ;\
    apt-get install -y --no-install-recommends \
        autoconf \
        automake \
        curl \
        g++ \
        libtool \
        make \
    ; \
    rm -rf /var/lib/apt/lists/*
RUN set -ex ;\
    tar -xzf /tmp/protobuf-cpp-${PROTOBUF_VERSION}.tar.gz -C /tmp/ ;\
    cd /tmp/protobuf-${PROTOBUF_VERSION}/ ;\
    ./configure ;\
    make ;\
    make check ;\
    make install ;\
    ldconfig ;\
    cd / ;\
    rm -r /tmp/protobuf-*

COPY docker-entrypoint /usr/local/bin/docker-entrypoint
RUN chmod +x /usr/local/bin/docker-entrypoint

VOLUME [ "/protos", "/out" ]

ENTRYPOINT ["docker-entrypoint"]
CMD ["--proto_path=/protos", "--go_out=plugins=grpc:/out", "/protos/*.proto"]