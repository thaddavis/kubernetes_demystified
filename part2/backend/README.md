# Walking through the FRONTEND Kubernetes components

## STEP 1 - warm up

- kubectl get all -A
- kubectl get namespace
- kubectl create namespace ruby-be
- kubectl apply -f deployment.yaml
- kubectl apply -f service.yaml

## STEP 2 - Ingress

kubectl apply -f ingress.yaml

kubectl scale deploy coredns -n kube-system --replicas=1

https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.0.0/docs/examples/external-dns.yaml

https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/08-ELB-Application-LoadBalancers/08-06-ALB-Ingress-ExternalDNS

kubectl exec -n ruby-be -it ruby-be-67479696c4-p4grn -- bash