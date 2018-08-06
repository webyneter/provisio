# proviso

Remote host provisioning


## Prerequisites

1. You have `root` credentials to access the remote host.


## Steps

### 0. Clone this project locally

### 1. Initialize the remote host

```bash
# In your local shell,
export REMOTE_HOST_ADDRESS=root@<host>

# Enter the remote host:
ssh "${REMOTE_HOST_ADDRESS}"

# New username, like `v63q48wt5qgbpfee`,
# or `<project slug>`, all lowercase
# (excluding look-alike symbols is a good practice):
export NEW_USER=

# Creating a user with restricted rights, and intervening in the system with root rights:
adduser "${NEW_USER}"

# Add the new user to sudoers:
usermod -aG sudo "${NEW_USER}"

# Exit the remote host:
exit

# Copy your public keys to the host to further enable Ansible to connect:
export REMOTE_HOST_ADDRESS=<new user>@<host>
ssh-copy-id "${REMOTE_HOST_ADDRESS}"
```

### 2. Let Ansible do the rest

```bash
# Install Ansible Galaxy requirements:
ansible-galaxy install --role-file=./requirements.yml --roles-path=./roles/

# Configure the remote host:
export PROVISIO_DOCKER_USER=<the ${NEW_USER} you created earlier> 
ansible-playbook --inventory ./hosts.yml play.yml --extra-vars "docker_user=${PROVISIO_DOCKER_USER}"
```
