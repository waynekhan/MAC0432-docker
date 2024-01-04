# SonarQube via the Docker image

In case adding `local.conf` to the VM's `/etc/sysctl.d/` doesn't work for you:

```text
# Run an interactive shell in the Rancher Desktop-managed VM
rdctl shell

# Temporarily update VM kernel params
sudo sysctl -w vm.max_map_count=524288
sudo sysctl -w fs.file-max=131072
```

Therefter, navigating into the folders and running `docker-compose up` should work.
