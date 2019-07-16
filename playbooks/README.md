[debian-base]: https://github.com/mvrahden/ansible-project-debian-base-setup

# Some instructions

This playbook bundle assumes, that you have played the [debian-base-setup][debian-base]-playbook bundle to homogenize your devices prior to running this playbook.

## cluster-setup.yml

This playbook disables swap memory, checks the availability of Docker-CE and fully removes any Kubernetes artifacts and reinstalls Kubernetes from the ground.

It **must** be executed with the **default inventory** like so:

`ansible-playbook playbooks/1-cluster-setup.yml -k`

**What needs to be done upfront?**

- You should be mostly setup. Please re-check the variables under [inventory/group_vars/all/all.yml](../inventory/group_vars/all/all.yml).

**What can be done afterwards?**

Note that after a successful execution of the playbook, you can find files with the `kubeconfig` and an access token for the dashboard inside the `./dist/`-directory.
These files will be overwritten at the end of every execution of this playbook, to ensure that they are up-to-date.
You can copy the content of the generated `kubeconfig`-file into the kubernetes configuration of your **control machine** to make it possible to `kubectl proxy` into your cluster from that machine.
The local `kubeconfig` by default can be found in e.g. the `~/.kube`-directory on your **control machine** (e.g. localhost).
