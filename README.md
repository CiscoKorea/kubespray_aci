![Figure](docs/figures/k8s.png?raw=true)

## Deploy a kubernetes cluster with Cisco ACI CNI Plugin 

This is my personal work and not officially supported by Cisco Systems. 
If you have questions, issues or feedback please drop me a [mail](kubesprayaci@gmail.com)

Supported Linux distributions
===============

* **Ubuntu** 16.04

Versions of supported components
--------------------------------


[kubernetes](https://github.com/kubernetes/kubernetes/releases) v1.7.11 <br>
[etcd](https://github.com/coreos/etcd/releases) v3.2.4 <br>
[docker](https://www.docker.com/) v1.13 (see note)<br>
[contiv-aci](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/kb/b_Kubernetes_Integration_with_ACI.html)<br>


Requirements
--------------

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
