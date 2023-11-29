1. create new s3 bucket
> aws s3api create-bucket \
--bucket duongdx-kops-bucket \
--region ap-southeast-1 \
--create-bucket-configuration LocationConstraint=ap-southeast-1

2. <h3>Create EBS volume: persistent volume<h3/>
- 3 GB is fine
> aws ec2 create-volume \
--volume-type gp2 \
--size 3 \
--availability-zone ap-southeast-1a \
--tag-specifications 'ResourceType=volume,Tags=[{Key=KubernetesCluster,Value=duongdx.com}]'

3. install kubectl in local

4. install kops in local

5. Create cluster-configuration: 1.26.4 
> kops create cluster --name=duongdx.com --bastion=false \
--state=s3://duongdx-kops-bucket --zones=ap-southeast-1a,ap-southeast-1b \
--node-count=2 --node-size=t3.small --master-size=t3.medium --dns-zone=duongdx.com \
--node-volume-size=8 --master-volume-size=8

6. Create cluster-configuration: 1.26.4 
> kops create cluster --name=duongdx.com --bastion=false \
--state=s3://duongdx-kops-bucket --zones=ap-southeast-1a,ap-southeast-1b \
--node-count=2 --node-size=t3.small --control-plane-size=t3.medium --dns-zone=duongdx.com \
--node-volume-size=8 --control-plane-volume-size=8

7. Create cluster:
> kops update cluster --name=duongdx.com --state=s3://duongdx-kops-bucket --yes --admin

8. Validate cluster:
> kops validate cluster --name=duongdx.com --state=s3://duongdx-kops-bucket

9. Delete cluster:
> kops delete cluster --name=duongdx.com --state=s3://duongdx-kops-bucket --yes

10. 