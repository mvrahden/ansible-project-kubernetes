# Some instructions

These playbooks assume, that you played the `mvrahden/ansible-init-debian`-playbook to homogenize your devices before running this playbook.

It is a setup in 3 consecutive Steps:

- 0a-bootstrap - static ips
- 0b-bootstrap - hostnames and necessary packages
- 1-cluster-setup - actual kubernetes install

## 0a-bootstrap.yml

When your initial IP addresses do **not** equal your static target addresses, then use this playbook with a dedicated *temporary inventory* (./inventory/hosts.tmp.bootstrap.ini).
As an example:

- your master has in the current default state a dynamic ip address `192.168.1.56`
- but during the play it will eventually receive a static ip address `192.168.1.200`
- this requires you to either
  - have a dedicated inventory e.g. `hosts.tmp.bootstrap.ini`, which only has a temporary usage and the exact **same layout** as the *default* inventory file. You can then run the following command, from the top-level of this project: `ansible-playbook playbooks/0a-bootstrap.yml -i inventory/hosts.tmp.bootstrap.ini -k`
  - or you type the current ips into the `hosts.ini` and change them to their specific static target ips, after you have played this playbook once.

## 0b-bootstrap.yml

This playbook installs further networking conditions and packages on the hosts, as well as OS-dependent stuff.

It **must** be executed with the **default inventory** like so:

`ansible-playbook playbooks/0b-bootstrap.yml -k`

## 1-cluster-setup.yml

Checks availability of Docker-CE and fully removes any Kubernetes artifacts and reinstalls Kubernetes from the ground.

It **must** be executed with the **default inventory** like so:

`ansible-playbook playbooks/1-cluster-setup.yml -k`
