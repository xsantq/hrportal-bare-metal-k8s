# hrportal-bare-metal-k8s
This project is Spring Boot web application that can be deployed on bare-metal kubernetes clusters. 
Normally in bare-metal kubernetes clusters there is no solution for loaadbalancing out of the box.
Loadbalancing and Ingress work in cloud environments(AWS,GCP,Azure) by default. As solution METAL LB
is used for maing the cluster accessible for outside world.
