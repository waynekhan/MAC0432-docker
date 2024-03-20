# MAC0432-docker

> Because SonarQube uses an embedded Elasticsearch, make sure that your Docker host configuration complies with the Elasticsearch production mode requirements and File Descriptors configuration.

Ref: https://hub.docker.com/_/sonarqube

On Rancher Desktop, set the recommended values for the current session by running the following commands:

```text
# Run an interactive shell into the VM
rdctl shell

# Temporarily update kernel params
sudo sysctl -w vm.max_map_count=524288 && \
  sudo sysctl -w fs.file-max=131072

# As the ulimit command is not found, these aren't needed, and that's OK
sudo ulimit -n 131072 && \
  sudo ulimit -u 8192
```

Therefter, navigate into the directory of interest and run `docker-compose up`.
