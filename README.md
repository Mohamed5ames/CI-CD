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
- Initialize the repository with a `README.md`, and ensure the repository contains at least two branches:
`develop` for ongoing development.
`master` (or `main`) for stable releases
- add branch protection rules to `main` branch
  - open your repo
  - from setting open Branchs
  - press `add rule` button and put your branch main (production) branch in Branch name pattern
  - check `Require a pull request before merging`
  - then press `create`
## Step 3 : Add Jenkins File
Add a Jenkinsfile to your repository in the root directory. Start with a
simple pipeline
```bash
pipeline {
 agent any
   stages {
      stage('build') {
        steps {
           echo "Hello World!"
              }
                    }
          }
         }
```
## Step 4 : Install git flow
```bash
git clone -b feature/https-remote https://github.com/jgonggrijp/gitflow.git
curl -OL https://raw.github.com/jgonggrijp/gitflow/develop/contrib/git flow-installer.sh
chmod +x gitflow-installer.sh
sudo ./gitflow-installer.sh
```
### Step 4.1 : Make sure You installed git flow by Initializing Git Flow in your repository with default branch names.
```bash
git flow init
```
![git flow](https://github.com/Mohamed5ames/CI-CD/assets/50241889/805fcdea-1203-4fdd-a941-7acc7f44416d)

## Step 5 : Generate SSH Key
```bash
ssh-keygen -t rsa -b 4096
```
####  add public key to your GitHub repo 
- copy your public key
- open your repo then go to settings and open deploy keys in the left panel and add it
- check allows write access to gain permissions to push develop branch
  
## Step 6 : Configure Jenkins 
- Browse to http://localhost:8080 to open Jenkins
- Get Jenkins Password and Sign in
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
- Install necessary plugins in Jenkins
- Link Jenkins to GitHub
- Go to "Manage Jenkins" > "Manage Plugins" > "Available" and install "GitHub Integration Plugin".
### Step 6.1 : Configure Credentials
- Go to "Manage Jenkins" > "Credentials" > "System" > "Global credentials (unrestricted)"
- press on `Add Credentials` button 
- choose `SSH Username and Private key` option from `kind` list
- make `scope` is `Global`
- fill `id` with name you want 
- put your github username in `username` textbox
- check `Enter directly`
- insert your public ssh key after press `add` button
- press `Create` button
### Step 6.2 : Create a new Pipeline

1. Select "New Item" in Jenkins.
2. Name your pipeline (e.g., "GitHub Pipeline") and choose "Pipeline" as the type.
3. In the pipeline configuration, select "Pipeline script from SCM" and choose "Git" as the SCM.
4. Enter the repository URL and credentials.
5. Specify the branch to build (e.g., */develop).
![Screenshot 2024-05-01 205246](https://github.com/Mohamed5ames/CI-CD/assets/50241889/498a5882-d7b0-4f00-8261-149b4cff799f)
![Screenshot 2024-05-01 205234](https://github.com/Mohamed5ames/CI-CD/assets/50241889/17ae7f8b-59d1-4044-9040-a50347c1147c)

### Step 6.3 (`For EC2`) : Implement Webhooks for Continuous Integration 

1. In GitHub, go to your repository settings and select Webhooks.
2. Add a new webhook:
   - Payload URL: `http://<your-jenkins-url>/github-webhook/`
   - Content type: `application/json`
   - Select "Just the push event".
   - Ensure the webhook is active.
3. With the webhook, Jenkins will trigger a new build every time changes are pushed to the connected branch.
### Step 6.3 (`VM without accessable public ip`) : Implement scheduled trigger (Configure Jenkins to poll SCM for any changes)

1. Scroll down to the `Build Triggers` section in your pipeline job configuration.
2. Check the box next to `Poll SCM`.
3. In the "Schedule" field, enter the polling schedule using cron syntax. 
   For example, to poll every 5 minutes, you can use `H/5 * * * *`.
![Screenshot 2024-05-01 205224](https://github.com/Mohamed5ames/CI-CD/assets/50241889/f059fd94-1720-44b7-8e6e-e7d074719bd9)
### Step 7 :Testing and Validation

1. Push a change to the develop branch in your GitHub repository.
2. Verify that Jenkins triggers a build.
3. Check the Jenkins dashboard for build status and output.
