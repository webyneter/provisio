# proviso

Remote host provisioning


## Prerequisites

1. You have `root` credentials to access the remote host.
1. `PROVISIONING_HOST` and `PROVISIONED_HOST_USER` envs are set.


## Steps

### 0. Clone this project locally

### 1. Initialize the remote host

1.1. From your local shell,
```bash
export PROVISIONING_HOST=
export PROVISIONING_ADDRESS="root@${PROVISIONING_HOST}"
# New username, like `staging`, all lowercase
# (excluding look-alike symbols is a good practice):
export PROVISIONING_NON_ROOT_USER=

# Enter the remote host:
ssh "${PROVISIONING_ADDRESS}"
``` 

1.2. From the remote shell,

```bash
# Change root password:
passwd root

# Same as PROVISIONING_NON_ROOT_USER
export NEW_USER=

# Creating a user with restricted rights, and intervening in the system with root rights:
adduser "${NEW_USER}"

# Add the new user to sudoers:
usermod -aG sudo "${NEW_USER}"

# Exit the remote host:
exit
```

1.3. And locally again:
```bash
# Copy your public keys to the host to further enable Ansible to connect:
ssh-copy-id "root@${PROVISIONING_HOST}"
ssh-copy-id "${PROVISIONING_NON_ROOT_USER}@${PROVISIONING_HOST}"

# Authorize CI runner for access by public key
export CI_RUNNER_PRIVATE_KEY_PATH=
ssh-copy-id -i "${CI_RUNNER_PRIVATE_KEY_PATH}" "root@${PROVISIONING_HOST}"
ssh-copy-id -i "${CI_RUNNER_PRIVATE_KEY_PATH}" "${PROVISIONING_NON_ROOT_USER}@${PROVISIONING_HOST}" 
```
