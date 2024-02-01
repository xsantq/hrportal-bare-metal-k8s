# hrportal-bare-metal-k8s
This project is Spring Boot web application that can be deployed on bare-metal kubernetes clusters. 
Normally in bare-metal kubernetes clusters there is no solution for loaadbalancing out of the box.
Loadbalancing and Ingress work in cloud environments(AWS,GCP,Azure) by default. As solution MetalLB
is used for to make the cluster accessible from outside world.

Steps: 
- Enable strict ARP mode of kube-proxy
-     kubectl edit configmap -n kube-system kube-proxy
- Create a namespace for MetalLB
-     kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.12.1/manifests/namespace.yaml
- Install MetalLB system resources
-     kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.12.1/manifests/metallb.yaml
- Create a configmap for MetalLB
-       apiVersion: v1
        kind: ConfigMap
        metadata:
        namespace: metallb-system
        name: config
        data:
          config: |
            address-pools:
            - name: default
            protocol: layer2
            addresses:
            -  192.168.254.140-192.168.254.150

