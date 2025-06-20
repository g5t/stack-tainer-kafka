# stacktainer-kafka

# Build

Use the Confluent Kafka Debian packages to build an Apptainer image which will launch a standalone
server listening on `localhost:9092` with suitable defaults.

```cmd
apptainer build stacktainer-kafka.sif image.def
```

# Run
The Kafka server can be started through either `run`, in which case the server logging output is visible,
or in a detached thread via `instance run` in which case no output is visible and the service must
be managed through `apptainer instance`

```cmd
apptainer run --writable-tmpfs stacktainer-kafka.sif 
```

```cmd
apptainer instance run --writable-tmpfs stacktainer-kafka.sif 
```

Note that in either case a writable temporary filesystem overlay is needed to allow the server to write
to the filesystem. This means that all stream data is lost when the server is restarted, so this mode
of operation is only suitable for ephemeral (stream) data as when producing simulated results.

Also note that the Kafka KRaft server uses port `9093`, so either launch method is liable to fail with
a hard-to-spot error message if either port `9092` or `9093` is already in use.

To support running on user-specified ports, the image uses two environment variables which default to
`BROKER_PORT=9092` and `CONTROLLER_PORT=9093`, and that can be overridden in the launch, e.g.,

```cmd
apptainer run --env BROKER_PORT=19092 --env CONTROLLER_PORT=19093 --writable-tmpfs stacktainer-kafka.sif
```
