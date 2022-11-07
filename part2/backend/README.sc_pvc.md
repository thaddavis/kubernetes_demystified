# Setting up for Persistent Storage

Reference: `https://github.com/kubernetes-sigs/aws-ebs-csi-driver/tree/master/examples/kubernetes`
Reference: `https://github.com/kubernetes-sigs/aws-ebs-csi-driver/tree/master/examples/kubernetes/dynamic-provisioning`

kubectl get sc -A

## STEP 1

- kubectl apply -f storageclass.yaml

## STEP 2

https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes

- kubectl create namespace pg-db
- kubectl apply -f pvc.yaml

## STEP 3

Refernece: `https://phoenixnap.com/kb/postgresql-kubernetes`
- kubectl apply -f pg-deployment.yaml
- kubectl get all -n pg-db

- kubectl get pod -n pg-db -o wide

Ran into issue with accessMode for `pvc.yaml` - only ReadWriteOnce is supported

- kubectl scale deployment pg-db --replicas=2 -n pg-db

## STEP 4

kubectl exec -it -n ruby-be ruby-be-7f74c59c4c-42fzw -- sh

TROUBLESHOOT EKS NETWORK WITH THIS -> kubectl run netshoot --image=nicolaka/netshoot --command sleep infinity

kubectl get endpoints -A

kubectl exec -it netshoot -- sh

nslookup pg-db-service.pg-db

Reference: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-to-a-previous-revision

rake db:create
rake db:setup