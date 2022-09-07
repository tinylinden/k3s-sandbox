# k3s-sandbox

[![k3s-sandbox.jpg](docs/k3s-sandbox.jpg)](https://unsplash.com/photos/2FaCKyEEtis)

[![Stability: Experimental](https://masterminds.github.io/stability/experimental.svg)](https://masterminds.github.io/stability/experimental.html)

> \- You can't solve a problem just by saying techy things.  
> \- Kubernetes.

### Motivation

I just needed a local Kubernetes cluster to play with. I know that one can be
quickly set up with [minikube][minikube], but where is the fun with simple
`minikube start --driver=virtualbox`?

The cluster is going to be used for experiments, so there must be a way to
set it up and tear it down with a single command. So I decided to experiment
with [Vagrant][vagrant] and [Ansible][ansible] to create my very own, naive
[K3s][k3s] three node cluster running inside [VirtualBox][virtualbox].

### Usage 

By default three virtual machines (nodes) will be created - master and two
workers. That count can be changed by setting `WORKERS_COUNT` in `Vagrantfile`, mind that total nodes count is equal to workers count + 1 (as there is always
`k3s-node-0` acting as master).

All nodes are named according to the scheme - `k3s-node-N`, where `N` is the
node number. And IP address assigned to `eth1` network interface attached to
to VirtualBox private network is set to `192.168.56.(100 + N)` (mind that this
addressing scheme may be affected by VirtualBox networking settings).

![k3s-nodes](docs/k3s-nodes.png)

#### Prerequisites

All was developed and tested on Debian GNU/Linux 11 (bullseye) x86_64, but
I see no reason why it shouldn't work on any other Linux distro. 

Download and install:

- [VirtualBox][virtualbox]
- [Vagrant][vagrant]
- [Ansible][ansible]

[virtualbox]: https://www.virtualbox.org/
[vagrant]: https://www.vagrantup.com/
[ansible]: https://www.ansible.com/
[minikube]: https://minikube.sigs.k8s.io/docs/
[k3s]: https://k3s.io/