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
- kubectl create -f metallb.yaml
  In this config MetalLB run in layer2 mode. BGP modes requires loadbalancing hardware. In this project nginx server is used as LB
  
- Create hrportal deployment
-         kubectl apply -f hrportal-deployment.yaml
- Expose deployment with service definition
-         kubectl expose deploy hrportal --port 8080 --type LoadBalancer

- After creating the service with type laddbalancer MetalLB exposes it with the defined external IP

-                 NAME         TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)          AGE
                  hrportal     LoadBalancer   10.100.133.63   192.168.254.140   8080:30771/TCP   19h

  
   
