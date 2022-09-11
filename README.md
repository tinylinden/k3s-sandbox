# k3s-sandbox

[![k3s-sandbox.jpg](docs/k3s-sandbox.jpg)](https://unsplash.com/photos/2FaCKyEEtis)

[![stability: experimental](https://masterminds.github.io/stability/experimental.svg)](https://masterminds.github.io/stability/experimental.html)
![platform: linux-64](https://img.shields.io/badge/platform-linux--64-lightgrey)

> \- You can't solve a problem just by saying techy things.  
> \- Kubernetes.

## Motivation

I just needed a local Kubernetes cluster to play with. I know that one can be
quickly set up with [minikube][minikube], but where is the fun with simple
`minikube start`?

It is going to be used for experiments, so there must be a way to
set it up and tear it down with a single command. I decided to experiment
with [Vagrant][vagrant] and [Ansible][ansible] to create my very own, naive
[K3s][k3s] three node cluster running inside [VirtualBox][virtualbox].

## Usage 

By default three virtual machines (nodes) will be created - master and two
workers. That count can be changed by setting `WORKERS_COUNT` in `Vagrantfile`,
mind that total nodes count is equal to workers count + 1 (as there is always
`k3s-node-0` acting as master).

All nodes are named according to the scheme - `k3s-node-N`, where `N` is the
node number. And IP address assigned to `eth1` network interface attached to
VirtualBox private network is set to `192.168.56.(100 + N)`.

> **NOTE:** If there is already host-only interface assigned to 192.168.56.0/24
> address space, there can be some connectivity problems between virtual
> machines created by Vagrant ([#1][issue-1]).

![k3s-nodes](docs/light/k3s-nodes.png#gh-light-mode-only)
![k3s-nodes](docs/dark/k3s-nodes.png#gh-dark-mode-only)

### Prerequisites

All was developed and tested on Debian GNU/Linux 11 (bullseye) x86_64, but
I see no reason why it shouldn't work on any other distro. 

Download and install:

- [VirtualBox][virtualbox]
- [Vagrant][vagrant]
- [Ansible][ansible]

### From nothing to running cluster

It's as simple as calling:

```
vagrant up
```

and waiting few minutes. If the job is finished without errors, virtual machines
should be created and provisioned. And cluster state can be checked with:

```
vagrant ssh k3s-node-0 -- 'kubectl get nodes'
```

which should return something like:

```
NAME         STATUS   ROLES                  AGE   VERSION
k3s-node-0   Ready    control-plane,master   54s   v1.24.4+k3s1
k3s-node-2   Ready    <none>                 32s   v1.24.4+k3s1
k3s-node-1   Ready    <none>                 32s   v1.24.4+k3s1
```

### When the mess creeps in

When something really bad happens to the cluster or you want to start fresh -
everything can be destroyed with a single command:

```
vagrant destroy -f
```

But destroying and creating virtual machines can take few minutes. If you do
not want to wait that long, and there is no problem with the virtual machine
or guest operating system but the cluster itself - it can be quickly purged 
and installed again with Ansible:

```
ansible-playbook reset.yml site.yml
```

### Show me the dashboard

[Portainer][portainer] can be installed with Ansible:

```
ansible-playbook portainer.yml
```

Installation is done when:

```
vagrant ssh k3s-node-0 -- 'kubectl get pods -n portainer'
```

shows that pod is running:

```
NAME                        READY   STATUS    RESTARTS   AGE
portainer-5d4958459-s66fl   1/1     Running   0          34s
```

Portainer UI can be accessed by navigating web browser to:

- http://192.168.56.100:9000
- http://k3s-node-0.local:9000 if [Avahi][avahi] is running on host system

[virtualbox]: https://www.virtualbox.org/
[vagrant]: https://www.vagrantup.com/
[ansible]: https://www.ansible.com/
[minikube]: https://minikube.sigs.k8s.io/docs/
[k3s]: https://k3s.io/
[portainer]: https://www.portainer.io/
[avahi]: https://www.avahi.org/

[issue-1]: https://github.com/tinylinden/k3s-sandbox/issues/1