# Testing dingo-postgresql docker containers

We are using [delmo](https://github.com/bodymindarts/delmo) to test the automatic failover and backup/recovery features of the docker containers used in dingo-postgresql.

To run the tests you need the following docker-tools pre-installed
- docker-compose
- docker-machine

Download the latest [delmo-release](https://github.com/bodymindarts/delmo/releases)

Since the tests integrate with aws to assert backup and recovery you will need to export the following variables to run the tests successfully:

```bash
cat .envrc # direnv
export AWS_ACCESS_KEY_ID=<aws_access_key_id>
export AWS_SECRET_ACCESS_KEY=<aws_secret_access_key>
export WAL_S3_BUCKET=test-backups-bucket
export AWS_REGION=eu-central-1
```

To execute the tests on a docker daemon running locally
```bash
delmo -f images/delmo.yml --only-build-task --localhost <your-ip>
```

Or to execute the tests on a remote docker-machine
```bash
delmo -f images/delmo.yml --only-build-task -m <remote-machine-name>
```

Delmo will bring up a preconfigured cluster of containers as defined by [docker-compose.yml](./docker-compose.yml) and run tests against it as defined in [delmo.yml](./delmo.yml).
The scripts used during the tests can be found under `images/tests/scripts/*`.
They get built into a [container](./tests/Dockerfile) image by delmo before the tests get run.
