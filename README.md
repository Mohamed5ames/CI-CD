# CI-CD PIPELINE
## Running Pipeline Automatically with Jenkins on EC2 or VM
Setting up Jenkins to run pipelines automatically on your server, whether it's an EC2 instance or a virtual machine, is straightforward. 
Follow these steps:
## Step 1: Install JavaJDK && Jenkins
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo dnf upgrade
# Add required dependencies for the jenkins package
sudo dnf install fontconfig java-17-openjdk
sudo dnf install jenkins
sudo systemctl daemon-reload
```
### Step 1.1 : Start Jenkins
```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
### Note: Port 8080 must be opened in your VM's firewall or Add a rule to allow incoming connections on port 8080 to EC2
## Step 2: Install GIT
```bash
yum install git
```
