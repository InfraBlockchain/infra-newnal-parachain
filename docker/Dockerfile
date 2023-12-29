FROM docker.io/paritytech/ci-linux:production as builder

WORKDIR /infra-newnal

COPY . /infra-newnal

RUN cargo build --release --locked

FROM docker.io/library/ubuntu:20.04

COPY --from=builder /infra-newnal/target/release/infra-newnal-parachain /usr/local/bin

RUN useradd -m -u 1000 -U -s /bin/sh -d /infra-newnal infra-newnal-parachain && \
    mkdir -p /data /infra-newnal/.local/share && \
    chown -R infra-newnal-parachain:infra-newnal-parachain /data && \
    ln -s /data /infra-newnal/.local/share/infra-newnal-parachain && \
    /usr/local/bin/infra-newnal-parachain --version

USER infra-newnal-parachain

EXPOSE 30333 9933 9944 9615
VOLUME ["/data"]

ENTRYPOINT ["/usr/local/bin/infra-newnal-parachain"]