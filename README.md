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
git --version
```
### Step 2.1 : Prepare your repo
- Create a new GitHub repository or select an existing one.
- Initialize the repository with a README.md, and ensure the repository contains at least two branches:
develop for ongoing development.
master (or main) for stable releases
- add branch protection rules to main branch
  - open your repo
  - from setting open Branchs
  - press add rule button and put your branch main (production) branch in Branch name pattern
  - check "Require a pull request before merging"
  - then press create
## Step 3 : Install git flow
```bash
git clone -b feature/https-remote https://github.com/jgonggrijp/gitflow.git
curl -OL https://raw.github.com/jgonggrijp/gitflow/develop/contrib/git flow-installer.sh
chmod +x gitflow-installer.sh
sudo ./gitflow-installer.sh
```
### Step 3.1 : Make sure You installed git flow by Initializing Git Flow in your repository with default branch names.
```bash
git flow init
```
![git flow](https://github.com/Mohamed5ames/CI-CD/assets/50241889/805fcdea-1203-4fdd-a941-7acc7f44416d)

