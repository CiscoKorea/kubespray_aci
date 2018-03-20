End to End LAB Deployment with Contiv-ACI
======
## Requirements:
### Ansible Host:
* Install latest ansible version (I tested this with 2.4.2.0) 
  * http://docs.ansible.com/ansible/latest/intro_installation.html
  * Install all ansible requirements 
* Python 2.7.9 or higher (I tested with 2.7.12)
* Install PIP
  * curl https://bootstrap.pypa.io/get-pip.py | python
* Install pyvmom (I tested pyvmomi 6.5.0.2017.5)
  * pip install pyvmomi
* Install python-netaddr
  * pip install netaddr
* If using password to authenticate ssh session, you need to install sshpass [Installation guide](https://gist.github.com/arunoda/7790979)
* The kubernetes master nodes needs to be able to communicate with the APIC Management address to make API calls. 

### Supported OS:
* Ubuntu 16.04
* If you want you can grab an Ubuntu VM template at this link: [Ubuntu-16.04 Template](https://cisco.box.com/s/zboppg9jeutg1p9ttow2cr2zgr11ztsm)
  * This template is configured with:
    * 2 CPU, 16GB HD, 2GB RAM and 2 NIC
    * username/pass: cisco/123Cisco123
* You can create your own template as long as it is a Ubuntu 16.04. Edit **roles/vmware-vm/defaults/main.yml** and set the hd_size to be equal to your VM template disk size. You can also set the ram_size there. 
* When you create the Template configure the 2 NIC as CONNECTED or the cloned VM will boot up with disconencted Nic and the script will fail. 
### Supported K8S Versions:
* 1.7.11

# How To Use:

## Enable VM provisioning (Optional):
* Edit **inventory/inventory** and set which nodes are a VM, for most environment you can copy this example config:

```yaml
[vmware-vm:children]
kube-node
kube-master
```

### Configure Virtual Machine parameters (Optional):
* Edit **roles/vmware-vm/defaults/main.yml**, in this file we set the credentials and parameters to access the vCenter server as well as the basic VM parameters that are set with the host prep. This steps assume a Ubuntu 16.0.4 VM tempates exists. 

## Configure ansible parameters:
* Edit **inventory/group_vars/all.yml** and set the following parameters:

```yaml
## Internal loadbalancers for apiservers
loadbalancer_apiserver_localhost: true
## Your external DNS Servers
upstream_dns_servers:
 - IP1
 - IP2

ansible_ssh_pass: pass
ansible_user: user_name
ansible_sudo_pass: pass
aci_admin_pass: pass
```

## Configure Host parameters:
* Edit **roles/aci-host/defaults/main.yml**
In this file we set things like the Management interface name and static routes toward ACI. The playbook assumes we use the management interface as default GW. However is MANDATORY that all the traffic related to Kubernetes is routed trough the fabric. Set up routing accordingly!

## Download acc-provision:
From Cisco.com download dist-debs-3.1.1-kubernetes1.7-20171215.tar.gz, unzip it and place the .deb file under **roles/aci-host/acc-provision/files**

##  Configure Contiv-ACI

* Enable Contiv-ACI by editing **inventory/group_vars/k8s-cluster.yml** and set the network_plugin to be conti-aci. You will also then to configure the service and pod subnet address ranges. kube_pods_subnet needs to match the pod_subnet configuration of the ACI CNI plugin. 


```yaml
kube_network_plugin: contiv-aci
kube_service_addresses: 10.37.0.0/24
kube_pods_subnet: 10.33.0.0/16
```

Note: k8s-cluster.yml expects the subnets to end by 0, the ACI CNI plugins expects the subnet to end by .1 


* Edit the inventory file and configure, for each host, the 'ip' address to bind kubernetes services. This address MUST be configured on the interfaces toward the ACI fabric. The ansible_ssh_host variable is used for the Out of Band Management network. For example:

```yaml
k8s-01 ansible_ssh_host=192.168.66.42 ip=10.32.0.11 
k8s-02 ansible_ssh_host=192.168.66.43 ip=10.32.0.12 
```

* Edit **roles/network_plugin/contiv-aci/defaults/main.yml** This file will contain the ACI CNI plugin configuration. This file is documented in the [Contiv-ACI documentation](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/kb/b_Kubernetes_Integration_with_ACI.html) Note: an example file is present in the folder.

# Deploy with this command:
ansible-playbook -i inventory/inventory -b --become-user=root lab_setup.yml
