# RDS is more practical than EBS

## 

https://us-east-1.console.aws.amazon.com/rds/home?region=us-east-1#launch-dbinstance:gdb=false;s3-import=false

##

launched a rds instance PostgreSQL 14.5 into the eks VPC public subnets with public access

##

the security groups need inbound whitelisted for my personal computer and the eks VPC subnets

## kubectl exec'd into the ruby-be pod and performed standard rails db initialization
