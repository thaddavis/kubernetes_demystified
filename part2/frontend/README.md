# Walking through the FRONTEND Kubernetes components

## STEP 1 - warm up

- kubectl get all -A

- `https://hub.docker.com/`
- `https://kubernetes.io/docs/tasks/administer-cluster/namespaces/#creating-a-new-namespace`

- kubectl get namespace
- kubectl create namespace react-fe

## STEP 2 - deployment

- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- kubectl apply -f part2/frontend/deployment.yaml
- kubectl get all -n react-fe

## STEP 3 - service

- kubectl apply -f part2/frontend/service.yaml

## STEP 4 - add on the AWS Load Balancer Controller

`https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html`
`https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html` ***
`https://docs.aws.amazon.com/eks/latest/userguide/configure-sts-endpoint.html`
`https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.4/`

- $ curl -o iam_policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.4.4/docs/install/iam_policy.json
- aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://load_balancer_controller_iam_policy.json
- aws eks describe-cluster --name wishbliss-v2 --query "cluster.identity.oidc.issuer" --output text
- aws iam create-role \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --assume-role-policy-document file://"load-balancer-role-trust-policy.json"
- aws iam attach-role-policy \
  --policy-arn arn:aws:iam::333427308013:policy/AWSLoadBalancerControllerIAMPolicy \
  --role-name AmazonEKSLoadBalancerControllerRole
- kubectl apply -f aws-load-balancer-controller-service-account.yaml
- kubectl apply \
    --validate=false \
    -f https://github.com/jetstack/cert-manager/releases/download/v1.5.4/cert-manager.yaml
- curl -Lo v2_4_4_full.yaml https://github.com/kubernetes-sigs/aws-load-balancer-controller/releases/download/v2.4.4/v2_4_4_full.yaml
- sed -i.bak -e '480,488d' ./v2_4_4_full.yaml
- sed -i.bak -e 's|your-cluster-name|wishbliss-v2|' ./v2_4_4_full.yaml
- kubectl apply -f v2_4_4_full.yaml
- curl -Lo v2_4_4_ingclass.yaml https://github.com/kubernetes-sigs/aws-load-balancer-controller/releases/download/v2.4.4/v2_4_4_ingclass.yaml
- kubectl apply -f v2_4_4_ingclass.yaml
- kubectl get deployment -n kube-system aws-load-balancer-controller
- kubectl describe deployments aws-load-balancer-controller -n kube-system

- https://us-east-1.console.aws.amazon.com/iamv2/home#/identity_providers

NOTES:

- kubectl logs aws-load-balancer-controller-859ccc88bb-wp4kx -n kube-system
- kubectl describe ingress ssl-entrance-ingress -n react-fe
- kubectl get deployments -n kube-system
- Needed to create and setup the aws-load-balancer-controller so that cluster can support ingress objects

REFERENCE: `https://www.eksworkshop.com/`

## STEP 5 - ingress

- purchase a domain : ) -> wishbliss.link
- get SSL cert -> ACM -> `https://us-east-1.console.aws.amazon.com/acm/home?region=us-east-1#/welcome`
- `https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.4/guide/ingress/annotations/`
- kubectl apply -f part2/frontend/ingress.yaml




## Adding an IAM User in auth-config map

- kubectl edit -n kube-system configmap/aws-auth

https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions

https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html