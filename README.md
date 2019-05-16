# proviso

Ansible role provisioning a remote host by 

* settings sudoers via [ahuffman.sudoers](https://galaxy.ansible.com/guisea/ansible-sudoers);
* installing Docker & Docker Compose via [geerlingguy.docker](https://galaxy.ansible.com/geerlingguy/docker); 
* installing a few other handy utilities.

Check out [./tasks/main.yml](./tasks/main.yml) for more details. 

## Required role parameters

* `non_root_user`: name of the non-root user to be created on the remote host.
