![Figure](docs/figures/k8s.png?raw=true)

## Deploy a kubernetes cluster with Cisco ACI CNI Plugin 

This is my personal work and not officially supported by Cisco Systems. 
If you have questions, issues or feedback please drop me a [mail](mailto:kubesprayaci@gmail.com)

Supported Linux distributions
===============

* **Ubuntu** 16.04

Deploy a Production Ready Kubernetes Cluster
============================================

If you have questions, join us on the [kubernetes slack](https://kubernetes.slack.com), channel **\#kubespray**.

-   Can be deployed on **AWS, GCE, Azure, OpenStack or Baremetal**
-   **High available** cluster
-   **Composable** (Choice of the network plugin for instance)
-   Support most popular **Linux distributions**
-   **Continuous integration tests**

Quick Start
-----------

Versions of supported components
--------------------------------


[kubernetes](https://github.com/kubernetes/kubernetes/releases) v1.7  1.8 and 1.9 <br>
[etcd](https://github.com/coreos/etcd/releases) v3.2.4 <br>
[docker](https://www.docker.com/) v1.13 (see note)<br>
[contiv-aci](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/kb/b_Kubernetes_Integration_with_ACI.html)<br>
=======
-   [kubernetes](https://github.com/kubernetes/kubernetes/releases) v1.9.5
-   [etcd](https://github.com/coreos/etcd/releases) v3.2.4
-   [flanneld](https://github.com/coreos/flannel/releases) v0.10.0
-   [calico](https://docs.projectcalico.org/v2.5/releases/) v2.6.2
-   [canal](https://github.com/projectcalico/canal) (given calico/flannel versions)
-   [cilium](https://github.com/cilium/cilium) v1.0.0-rc8
-   [contiv](https://github.com/contiv/install/releases) v1.1.7
-   [weave](http://weave.works/) v2.2.1
-   [docker](https://www.docker.com/) v17.03 (see note)
-   [rkt](https://coreos.com/rkt/docs/latest/) v1.21.0 (see Note 2)

Note: kubernetes doesn't support newer docker versions. Among other things kubelet currently breaks on docker's non-standard version numbering (it no longer uses semantic versioning). To ensure auto-updates don't break your cluster look into e.g. yum versionlock plugin or apt pin).


Requirements
------------

* **Ansible v2.4 (or newer) and python-netaddr is installed on the machine
  that will run Ansible commands**
* **Jinja 2.9 (or newer) is required to run the Ansible Playbooks**
* The target servers must have **access to the Internet** in order to pull docker images.
* The target servers are configured to allow **IPv4 forwarding**.
* The **firewalls are not managed**, you'll need to implement your own rules the way you used to.
in order to avoid any issue during deployment you should disable your firewall.

For a complete gudie please refer to:
-------------------------------------
[Lab Deployment Guide](docs/lab_setup.md)
-   **Ansible v2.4 (or newer) and python-netaddr is installed on the machine
    that will run Ansible commands**
-   **Jinja 2.9 (or newer) is required to run the Ansible Playbooks**
-   The target servers must have **access to the Internet** in order to pull docker images.
-   The target servers are configured to allow **IPv4 forwarding**.
-   **Your ssh key must be copied** to all the servers part of your inventory.
-   The **firewalls are not managed**, you'll need to implement your own rules the way you used to in order to avoid any issue during deployment you should disable your firewall.
