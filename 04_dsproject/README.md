### MLFLOW On AWS

## MLflow on AWS Setup:

1. Login to AWS console.
2. Create IAM user with AdministratorAccess
3. Export the credentials in your AWS CLI by running "aws configure"
4. Create a s3 bucket
5. Create EC2 machine (Ubuntu) & add Security groups 5000 port

Run the following command on EC2 machine
```bash
sudo apt update

sudo apt install python3-pip

sudo apt install pipenv

sudo apt install virtualenv

mkdir mlflow

cd mlflow

pipenv install mlflow

pipenv install awscli

pipenv install boto3

pipenv shell


## Then set aws credentials
aws configure

# Create 2GB swapfile
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make permanent across reboots
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Verify it's active
free -h

#Finally, start MLflow with only 1 worker to minimize memory usage:
mlflow server --host 0.0.0.0 --port 5000 --default-artifact-root s3://mlflow-tracking-1 --allowed-hosts "*" --uvicorn-opts "--workers 1"

#open Public IPv4 DNS to the port 5000
Remember put http not https like the following example: http://ec2-32-197-237-235.compute-1.amazonaws.com:5000/

#set uri in your local terminal and in your code 
export MLFLOW_TRACKING_URI=http://ec2-32-197-237-235.compute-1.amazonaws.com:5000/