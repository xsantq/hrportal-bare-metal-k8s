# hrportal-bare-metal-k8s
This project is Spring Boot web application that can be deployed on bare-metal kubernetes clusters. 
Normally in bare-metal kubernetes clusters there is no solution for loaadbalancing out of the box.
Loadbalancing and Ingress work in cloud environments(AWS,GCP,Azure) by default. As solution MetalLB
is used for to make the cluster accessible from outside world.

Steps: 
- Enable strict ARP mode of kube-proy
-     kubectl edit configmap -n kube-system kube-proxy
- Create a configmap for METAL LB
- 
