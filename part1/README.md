# Create a Kubernetes Cluster on AWS with EKS tools

## STEP 1 - Create VPC

aws cloudformation deploy --template-file eks-ipv4-vpc-pub-and-priv-subnets.yaml \
--stack-name wishbliss-v2

- view progress here...
`https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filteringStatus=active&filteringText=&viewNested=true&hideStacks=false`

- took about 2-3 minutes to complete

- `https://eksctl.io/usage/minimum-iam-policies/`
- `https://www.arin.net/`
- `https://www.ietf.org/`

## Issues

Ran into max EIP limit of 5 in region (created for NAT Gateway)

## STEP 2 - Create Control Plane

