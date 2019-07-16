[debian-base]: https://github.com/mvrahden/ansible-project-debian-base-setup

# Deploy a Kubernetes Cluster <!-- omit in toc -->

![social-banner](https://github.com/mvrahden/openclusters/raw/22133c21f7b10a46d4555dd21455f9932b34d00e/images/social-preview-color.png)

## Motivation <!-- omit in toc -->

This Ansible project was created to ensure an automated and reproducible process of setting up a Kubernetes Cluster environment on a defined list of host machines.

## Table of Contents <!-- omit in toc -->

- [Prerequisites](#Prerequisites)
- [What it does](#What-it-does)
- [How it works](#How-it-works)
  - [Playbook: `playbooks/cluster-setup.yml`](#Playbook-playbookscluster-setupyml)
- [License](#License)
- [Author Information](#Author-Information)

## Prerequisites

- install required roles: `ansible-galaxy install -r requirements.yml`
- install fresh versions of a Debian-based on your devices by flashing their persistent storage devices
  - For your **Intel NUC** this could be [this Ubuntu Server image](http://cdimage.ubuntu.com/ubuntu/releases/19.04/release/ubuntu-19.04-server-amd64.iso) or [check other releases](https://ubuntu.com/download/server).
  - For your **Rock64** this could be [this minimal debian image](https://github.com/ayufan-rock64/linux-build/releases/download/0.8.3/stretch-minimal-rock64-0.8.3-1141-arm64.img.xz) or [check other releases](https://github.com/ayufan-rock64/linux-build/releases/).
  - For your **Raspberry Pi** this could be [Raspbian Lite](https://downloads.raspberrypi.org/raspbian_lite_latest).
- homogenize your devices so you can expect a consistent behavior. To achieve this, use e.g. this playbook bundle [debian-base-setup][debian-base].

## What it does

This Ansible project sets up a working **Kubernetes** environment on your cluster of host machines.
It can be configured with an overlay network, such as **weave-net** or **coreos/flannel** (defaults to: flannel).

## How it works

This playbook bundle assumes, that you have played the [debian-base-setup][debian-base]-playbook bundle to homogenize your devices prior to running this playbook.

### Playbook: `playbooks/cluster-setup.yml`

This playbook disables swap memory, checks the availability of Docker-CE and fully removes any Kubernetes artifacts and reinstalls Kubernetes from the ground.

It **must** be executed with the **default inventory** like so:

`ansible-playbook playbooks/cluster-setup.yml -k`

**What needs to be done upfront?**

- You should be mostly setup. Please re-check the variables under [inventory/group_vars/all/all.yml](../inventory/group_vars/all/all.yml).

**What can be done afterwards?**

Note that after a successful execution of the playbook, you can find two files inside the `dist/`-directory.
These files will be overwritten at the end of every execution of this playbook, to ensure that they are up-to-date.

- `dist/admin.conf` - gives you the content of the cluster admin configuration (also known as **kubeconfig**).
  Copy the content of this file into the file `~/.kube/config` of your local control machine and then execute `kubectl proxy` to initiate a successful binding of your control machine to the cluster.
- `dist/token` - gives your the token of the **kubernetes-admin** service account, that has been allocated to the dashboard.
  Use this token to log into the dashboard.
  Log into the Dashboard under:
  http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login

## License

MIT

## Author Information

- Menno van Rahden
- Larry Smith Jr.
