Contiv-ACI
======

## How to Configure Contiv-ACI:


* Enable Contiv-ACI by editing inventory/group_vars/k8s-cluster.yml and set the network_plugin to be conti-aci. You will also then to configure the service and pod subnet address ranges. kube_pods_subnet needs to match the pod_subnet configuration of the ACI CNI plugin. 


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

* Edit roles/network_plugin/contiv-aci/defaults/main.yml This file will contain the ACI CNI plugin configuration. This file is documented in the [Contiv-ACI documentation](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/kb/b_Kubernetes_Integration_with_ACI.html)

### Requiremetns:
* The kubernetes master nodes needs to be able to communicate with the APIC Management address to make API calls. 
* For node requirements refer to the [Contiv-ACI documentation|Node Preparation](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/kb/b_Kubernetes_Integration_with_ACI.html#task_vwm_rq1_41b)
