# Setting Up AWS EBS CSI

Reference: `https://docs.aws.amazon.com/eks/latest/userguide/csi-iam-role.html`

## TIPS

- The ebs-csi add-on will run in the kube-system namespace
ie: `kubectl get pod -n kube-system`
ie: `kubectl get ds -n kube-system`
ie: `kubectl get deployment -n kube-system`
ie: `kubectl get nodes`

## STEP 1

- $ aws eks describe-cluster \
  --name wishbliss-v2 \
  --query "cluster.identity.oidc.issuer" \
  --output text

  OUTPUT: `https://oidc.eks.us-east-1.amazonaws.com/id/165D9CC9DD8AAE89D68B131865A8ABC0`

## STEP 2

- create `aws-ebs-csi-driver-trust-policy.json` with contents from referenced link

- $ aws iam create-role \
--role-name AmazonEKS_EBS_CSI_DriverRole \
--assume-role-policy-document file://aws-ebs-csi-driver-trust-policy.json

## STEP 3

- $ aws iam attach-role-policy \
  --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --role-name AmazonEKS_EBS_CSI_DriverRole

## STEP 4

- create `kms-key-for-encryption-on-ebs.json` with contents from the referenced link 
- needed to do so through console

## STEP 5

- $ aws iam create-policy \
  --policy-name KMS_Key_For_Encryption_On_EBS_Policy \
  --policy-document file://kms-key-for-encryption-on-ebs.json

## STEP 6

- $ aws iam attach-role-policy \
  --policy-arn arn:aws:iam::333427308013:policy/KMS_Key_For_Encryption_On_EBS_Policy \
  --role-name AmazonEKS_EBS_CSI_DriverRole

### TANGENT to deploy the add on

Reference: `https://docs.aws.amazon.com/eks/latest/userguide/managing-ebs-csi.html`

aws eks describe-addon-versions --addon-name aws-ebs-csi-driver

aws eks create-addon --cluster-name wishbliss-v2 --addon-name aws-ebs-csi-driver \
  --service-account-role-arn arn:aws:iam::333427308013:role/AmazonEKS_EBS_CSI_DriverRole

Needed to increase worker groups compute to 2 t2.small instances

## STEP 7 - after deploying the EBS CSI add-on

- $ kubectl annotate serviceaccount ebs-csi-controller-sa \
    -n kube-system \
    eks.amazonaws.com/role-arn=arn:aws:iam::333427308013:role/AmazonEKS_EBS_CSI_DriverRole