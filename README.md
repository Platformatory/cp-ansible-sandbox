# cp-ansible-sandbox

podman-compose environment to run cp-ansible in a podman environment with systemd and ssh for the inventory nodes

## Getting Started

```sh
# This is run only once after cloning the repo
git submodule update --init --recursive

podman-compose up -d
```

### Verify if the containers are up

```
podman-compose ps
```
### Setup SSH keys on all containers

```sh
./setup-ssh-keys.sh
```

### Access the ansible control node

```sh
podman exec -it ansible-control bash

# Inside the ansible control node, verify ssh connectivity with other nodes
ssh root@zookeeper1
```

### Install the cp-ansible playbook

```sh
# Inside the ansible control node
ansible-playbook confluent.platform.all
# This assumes the inventory is in /ansible/inventories/ansible-inventory.yml since ANSIBLE_INVENTORY is pointing to that
```

### Change the inventory

The inventory files in `inventories` is mounted to `/ansible/inventories/` on the ansible control node container. You can create new files or update `ansible-inventory.yml` locally to let it reflect on the control node.

#### Using a custom inventory file

```sh
ansible-playbook -i /ansible/inventories/custom-inventory.yml confluent.platform.all
```

### Additional files

Add additional files to the ansible control node by placing them in the `share` directory locally. These will be available in `/usr/share/cp-ansible-sandbox` in the ansible control node

### Accessing the control center

Port 9021 is exposed locally as 9021 for the `control-center` container. If control-center is installed in the `control-center` container, it can be accessed locally in localhost:9021

### Verifying health of the cp-ansible cluster

```sh
podman exec -it kafka1 bash

# Once inside the container, you can run the Kafka CLIs to interact with the cluster
```

### Updating the cp-ansible playbook

```sh
git submodule update --recursive
```

### Destroy the environment

```sh
podman-compose down -v
```
