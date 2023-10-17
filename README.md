### A Simple Ansible Playbook for Installation RKE2 Cluster (Debian Bullseye)

&nbsp;

#### 1. Inventory

&nbsp;

```yaml
all:
  hosts:
    rke2-master-1:
      ansible_host: 10.0.1.11
    rke2-master-2:
      ansible_host: 10.0.1.12
    rke2-master-3:
      ansible_host: 10.0.1.13

    rke2-worker-1:
      ansible_host: 10.0.1.21
    rke2-worker-2:
      ansible_host: 10.0.1.22
    rke2-worker-3:
      ansible_host: 10.0.1.23

master:
  hosts:
    rke2-master-1:
    rke2-master-2:
    rke2-master-3:

worker:
  hosts:
    rke2-worker-1:
    rke2-worker-2:
    rke2-worker-3:

rke2_cluster:
  children:
    master:
    worker:
```

&nbsp;

&nbsp;

#### 2. Variables

&nbsp;

```yaml
## RKE2 Server Variables
# inventory/group_vars/rke2_cluster/main.yml

change_hostnames: true

rke2_version: v1.23.5+rke2r1 #latest

subject_alt_name: "10.0.1.10, rke2.basitbir.site"

logrotate:
  rotate: '3'
  maxsize: '10M'

#private_registries:
#  nexus.local:
#    mirrors:
#      endpoint:
#        - https://nexus.local
#    configs:
#      auth:
#        username: user
#        password: pass
```

&nbsp;

&nbsp;

#### 3. Install RKE2 Cluster

&nbsp;

```bash
# Install PIP
apt update && \
apt install -y python3-pip sshpass

pip install -U pip
pip install ansible ansible-lint

# Install Ansible requirements
ansible-galaxy install -r collections/requirements.yml

# Run playbook
ansible-playbook playbooks/setup-rke2.yml
```

&nbsp;

&nbsp;

#### 4. Purge RKE2 Cluster

&nbsp;

```bash
ansible-playbook playbooks/purge-cluster.yml
```

&nbsp;

&nbsp;
