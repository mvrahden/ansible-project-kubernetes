# Deploy a Kubernetes Cluster

![social-banner](https://github.com/mvrahden/openclusters/raw/22133c21f7b10a46d4555dd21455f9932b34d00e/images/social-preview-color.png)

## Motivation

This Ansible project was created to ensure an automated and reproducible process of setting up a Kubernetes Cluster environment on a defined list of host machines.

## Prerequisites

- install required roles: `ansible-galaxy install -r requirements.yml`
- install fresh versions of a Debian-based on your devices by flashing their persistent storage devices   
  For your **Intel NUC** this could be [this Ubuntu Server image](http://cdimage.ubuntu.com/ubuntu/releases/19.04/release/ubuntu-19.04-server-amd64.iso) or [check other releases](https://ubuntu.com/download/server).   
  For your **Rock64** this could be [this minimal debian image](https://github.com/ayufan-rock64/linux-build/releases/download/0.8.3/stretch-minimal-rock64-0.8.3-1141-arm64.img.xz) or [check other releases](https://github.com/ayufan-rock64/linux-build/releases/).   
  For your **RaspberryPi** this could be [Raspbian Lite](https://downloads.raspberrypi.org/raspbian_lite_latest).

## What it does

This Ansible project sets up a working kubernetes environment on your cluster of host machines.
It can be configured with an overlay network, such as weave-net or coreos/flannel (defaults to: flannel).


## How it works

The Ansible project contains multiple playbooks, each with its specific purpose.
The bootstrap process (which tries to setup the networking and other needed packages) is split into two playbooks.
The first bootstrap-playbook guarantees **static IPs** for the host machines, as after a clean install host machines will fallback to a dynamically allocated IP via DHCP.
The second bootstrap-playbook then goes on with **guaranteed IPs** and **sets hostnames** and **installs further packages**.
To know more on how to use them, please read [this README-file](./playbooks/README.md)

## License

MIT

## Author Information

- Menno van Rahden
- Larry Smith Jr.
