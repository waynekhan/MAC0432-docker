# README

## SonarQube via the Docker image

```text
# Get a shell to the VM
rdctl shell

# Temporarily update (VM) kernel params
sudo sysctl -w vm.max_map_count=524288
sudo sysctl -w fs.file-max=131072
```

Therefter, navigating into `postgres` and `sonarqube-99-lts-de` and running `docker-compose up` should work.

## SonarScanner

### Scanning via the zip file

```text
cd $HOME/src/github.com/waynekhan/wordpress-export-emitter && \
  ~/Downloads/sonar-scanner-5.0.1.3006-macosx/bin/sonar-scanner \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.projectKey=wordpress-exporter-emitter \
  -Dsonar.login=REDACTED \
  -X
```

### Scanning via the Docker image

```text
docker run \
  --rm \
  -e SONAR_HOST_URL="http://192.168.5.15:9000" \
  -e SONAR_SCANNER_OPTS="-Dsonar.projectKey=wordpress-exporter-emitter" \
  -e SONAR_TOKEN="REDACTED" \
  -v "$HOME/src/github.com/waynekhan/wordpress-export-emitter:/usr/src" \
  sonarsource/sonar-scanner-cli \
  -X
```

Check [Advanced Docker configuration](https://docs.sonarsource.com/sonarqube/9.9/analyzing-source-code/scanners/sonarscanner/#advanced-docker-configuration) if you run into problems communicating with your SonarQube instance; e.g., `cacerts`-related problems.
