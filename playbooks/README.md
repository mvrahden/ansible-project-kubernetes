[debian-base]: https://github.com/mvrahden/ansible-project-debian-base-setup

# Some instructions

These playbooks assume, that you played the [debian-base-setup][debian-base]-playbook to homogenize your devices before running this playbook.

## About this playbook bundle

This playbook bundle is a setup in 3 consecutive Steps:

- **0a-bootstrap** - sets static ips
- **0b-bootstrap** - hostnames and necessary packages
- **1-cluster-setup** - actual kubernetes install

## 0a-bootstrap.yml

When your initial IP addresses do **not** equal your static target addresses, then use this playbook with a dedicated *temporary inventory* (./inventory/hosts.tmp.bootstrap.ini).
As an example:

- your master has in the current default state a dynamic ip address `192.168.1.56`
- but during the play it will eventually receive a static ip address `192.168.1.200` if defined so.
- this requires you to either
  - to have a dedicated inventory e.g. `hosts.tmp.bootstrap.ini`, which only has a temporary usage and the exact **same layout** as the *default* inventory file. You can then run the following command, from the top-level of this project (but check the following advices first please): `ansible-playbook playbooks/0a-bootstrap.yml -i inventory/hosts.tmp.bootstrap.ini -k`
- **What needs to be done from your side upfront?**
  - we need to define for ourselves a list of **target ips** for the devices of our cluster. Something like the following:   
  ```[bash]
  for host `kube-r64-000` the target ip is `192.168.1.200`
  for host `kube-r64-001` the target ip is `192.168.1.201`
  for host `kube-r64-002` the target ip is `192.168.1.202`
  ...
  ```
  - Make sure these ips are vacant in your network, as they will be statically defined.
  - These **target ips** (subnet and ip endings/last digit) need to be defined in the variable file under `inventory/group_vars/all/all.yml`. This is guarantees, that the static ips will be set the right way and that the following plays can rely on these values.
  - The variable files under `inventory/group_vars/kube_master` and `inventory/group_vars/kube_slaves` need to get some basic information in order to set the networking interface profiles right. Things like DNS servers and network gateways need to be set. If you don't want to specify these things, just comment out the lines of those entries and default networking logic will take over.
  - the file `inventory/group_vars/all/k8s.yml` you have to define the **kube-master**. (Currently this provisioning project only supports a single-master setup. It is intended to develop this provisioning furhter to support multi-master setups as well.)
  - the file `inventory/group_vars/all/all.yml` contains the **primary domain** of the cluster. This value can also be configured.

## 0b-bootstrap.yml

This playbook installs further networking conditions and packages on the hosts, as well as OS-dependent stuff.

It **must** be executed with the **default inventory** like so:

`ansible-playbook playbooks/0b-bootstrap.yml -k`

**What needs to be done upfront?**
- You should be mostly setup. Please re-check the variables under [inventory/group_vars/all/all.yml](../inventory/group_vars/all/all.yml).

## 1-cluster-setup.yml

Checks availability of Docker-CE and fully removes any Kubernetes artifacts and reinstalls Kubernetes from the ground.

It **must** be executed with the **default inventory** like so:

`ansible-playbook playbooks/1-cluster-setup.yml -k`
