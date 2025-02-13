# KU Eater's Custom Postgres Image

**Supported Architecture**: `amd64`

**Supported PostgreSQL version**: `17` (working on versions that is supported at least in 2027)

We used `postgres:MAJOR_VERSION` as our base image then, add few extensions to the engine along with enabling them by default:
* [`tensorchord/pgvecto.rs`](https://github.com/tensorchord/pgvecto.rs)
    * Not supporting nightly releases
* [`fboulnois/pg_uuidv7`](https://github.com/fboulnois/pg_uuidv7)
    * This might not be required in PostgreSQL v18+

## After Using the Image
Enable the two extensions by using the following SQL script:
```sql
DROP EXTENSION IF EXISTS vectors;
CREATE EXTENSION vectors;

DROP EXTENSION IF EXISTS pg_uuidv7;
CREATE EXTENSION pg_uuidv7;
```

# Building an Image
Use Docker to build like so,
`docker build --build-arg POSTGRES_MAJOR=17 ... -f Dockerfile .`

## Build Arguments
If you are building the Docker image by yourself, here are the build arguments required:
* `POSTGRES_MAJOR`: the number of major version of PostgreSQL (e.g. `16`, `17`)
* `VECTORS_RELEASE`: the version of `pgvecto.rs` extension, needs to be prefixed with `v` (e.g. `v0.4.0`)
* `UUIDV7_RELEASE`: the version of `pg_uuidv7` extension, needs to be prefixed with `v` (e.g. `v1.6.0`)