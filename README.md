# SonarQube via the Docker image

```text
# Get a shell to the VM
rdctl shell

# Temporarily update (VM) kernel params, as preserving these changes across reboots don't work (ref: https://docs.rancherdesktop.io/getting-started/installation/#traefik-port-binding-access)
sudo sysctl -w vm.max_map_count=524288
sudo sysctl -w fs.file-max=131072
```

Therefter, navigating into the folders and running `docker-compose up` should work.
