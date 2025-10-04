-----------------------------------------------------
Jenkins Setup on AWS  (Updated)
-----------------------------------------------------
STEP-1: Launch EC2 Instance

Launch an EC2 instance:

Instance type: t2.micro

Storage: 20 GB SSD

OS: Amazon Linux 2 or Ubuntu 20.04/22.04 (recommended)


STEP-2: Install jenkins on aws and add the components in jenkins 

go to this link -https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/

STEP-3:  create a pipeline and run the Jenkinsfile

------------------------------------------------------
Kubernetes Cluster Setup on AWS using kOps (Updated)
-----------------------------------------------------
STEP-4: Launch EC2 Instance

Launch an EC2 instance:

Instance type: t2.micro

Storage: 20 GB SSD

OS: Amazon Linux 2 or Ubuntu 20.04/22.04 (recommended)

STEP-2: Install AWS CLI (v2 latest)
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install


✅ Check version:

/usr/local/bin/aws --version


✅ Add AWS CLI to PATH (if needed):

vim ~/.bashrc


Add this line:

export PATH=$PATH:/usr/local/bin/


Apply changes:

source ~/.bashrc
aws --version

STEP-5: Install kubectl & kOps (Latest Versions)

📌 Latest Stable kubectl (v1.31.0 – Sept 2025)

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"


📌 Latest Stable kOps (v1.29.2 – Sept 2025)

wget https://github.com/kubernetes/kops/releases/download/v1.29.2/kops-linux-amd64


📌 Give permissions & move to bin

chmod +x kubectl kops-linux-amd64
sudo mv kubectl /usr/local/bin/kubectl
sudo mv kops-linux-amd64 /usr/local/bin/kops


✅ Check versions:

kubectl version --client
kops version

STEP-6: IAM Role with Admin Access

Go to AWS Console → IAM

Create a role with AdministratorAccess policy

Attach this IAM role to your EC2 instance:

EC2 → Instance → Actions → Security → Modify IAM Role

STEP-7: Infrastructure Setup

📌 Create S3 Bucket for kOps State Store

aws s3api create-bucket --bucket nikhil.k8s.cluster --region ap-south-1

(here any error came then)
aws s3api create-bucket \
  --bucket nikhil.k8s.cluster \
  --region ap-south-1 \
  --create-bucket-configuration LocationConstraint=ap-south-1



📌 Enable Versioning

aws s3api put-bucket-versioning --bucket nikhil.k8s.cluster --region ap-south-1 --versioning-configuration Status=Enabled
(here any error came then)
aws s3api put-bucket-versioning \
  --bucket nikhil.k8s.cluster \
  --region ap-south-1 \
  --versioning-configuration Status=Enabled




📌 Export KOPS_STATE_STORE

export KOPS_STATE_STORE=s3://nikhil.k8s.cluster


📌 (Optional) Generate SSH Key

ssh-keygen

STEP-8: Create Kubernetes Cluster
kops create cluster \
  --name nikhil.k8s.local \
  --zones ap-south-1a \
  --control-plane-size t2.medium \
  --node-size t2.micro \
  --control-plane-count 1 \
  --node-count 2


📌 View cluster:

kops get cluster

STEP-9: To Run the Both dp.yml and se.yml in kops cluster.
