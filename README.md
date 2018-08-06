# proviso

Remote host provisioning


## Prerequisites

1. You have `root` credentials to access the remote host.


## Steps

### 0. Clone this project locally

### 1. Initialize the remote host

```bash
# In your local shell,
export REMOTE_HOST_HOSTNAME=
export REMOTE_HOST_ADDRESS="root@${REMOTE_HOST_HOSTNAME}"
# New username, like `staging`, all lowercase
# (excluding look-alike symbols is a good practice):
export REMOTE_HOST_NEW_USER=

# Enter the remote host:
ssh "${REMOTE_HOST_ADDRESS}"

# Same as REMOTE_HOST_NEW_USER
export NEW_USER=

# Creating a user with restricted rights, and intervening in the system with root rights:
adduser "${NEW_USER}"

# Add the new user to sudoers:
usermod -aG sudo "${NEW_USER}"

# Exit the remote host:
exit

# Copy your public keys to the host to further enable Ansible to connect:
ssh-copy-id "root@${REMOTE_HOST_HOSTNAME}"
ssh-copy-id "${REMOTE_HOST_NEW_USER}@${REMOTE_HOST_HOSTNAME}"
```

### 2. Let Ansible do the rest

```bash
# Configure the remote host:
export PROVISIO_DOCKER_USER="${REMOTE_HOST_NEW_USER}"
ansible-playbook --inventory ./hosts.yml play.yml --extra-vars "host=${REMOTE_HOST_HOSTNAME} docker_user=${PROVISIO_DOCKER_USER}"
```
