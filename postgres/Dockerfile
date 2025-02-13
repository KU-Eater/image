# Source the `deb` file for vecto.rs from remote image
ARG POSTGRES_MAJOR
ARG VECTORS_RELEASE
FROM tensorchord/pgvecto-rs-binary:pg${POSTGRES_MAJOR}-${VECTORS_RELEASE}-amd64 as vectors

# Download the UUIDv7 extension from GitHub
FROM alpine:3.20 AS uuid
ARG POSTGRES_MAJOR
ARG UUIDV7_RELEASE
RUN wget -q https://github.com/fboulnois/pg_uuidv7/releases/download/${UUIDV7_RELEASE}/pg_uuidv7.tar.gz -O /tmp/pg_uuidv7.tar.gz \
    && mkdir /tmp/pg_uuidv7 \
    && tar -xzf /tmp/pg_uuidv7.tar.gz -C /tmp/pg_uuidv7

ARG POSTGRES_MAJOR
FROM postgres:${POSTGRES_MAJOR}

# Install the vecto.rs extension
COPY --from=vectors /pgvecto-rs-binary-release.deb /tmp/vectors.deb
RUN apt-get install -y /tmp/vectors.deb && rm -f /tmp/vectors.deb

# Copy to install UUID extension
ARG POSTGRES_MAJOR
COPY --from=uuid /tmp/pg_uuidv7/${POSTGRES_MAJOR}/pg_uuidv7.so /usr/lib/postgresql/${POSTGRES_MAJOR}/lib
COPY --from=uuid /tmp/pg_uuidv7/pg_uuidv7* /usr/share/postgresql/${POSTGRES_MAJOR}/extension

# Blatantly copied from https://github.com/tensorchord/pgvecto.rs/blob/main/docker/pgvecto-rs.Dockerfile
CMD ["postgres", "-c", "shared_preload_libraries=vectors.so", "-c", "search_path=\"$user\", public, vectors", "-c", "logging_collector=on"]