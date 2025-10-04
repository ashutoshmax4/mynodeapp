Node.js App — CI/CD with Jenkins

Project: CI/CD Pipeline for a Node.js application using Jenkins, Mocha tests, and PM2 for deployment.

Summary

A simple Node.js application with an automated CI/CD pipeline using Jenkins. The pipeline clones the repo, installs dependencies, runs unit tests with Mocha, and deploys the application to an Ubuntu server using SSH and PM2 for process management.

Technical Stack
Runtime: Node.js, npm
CI/CD: Jenkins (Declarative Pipeline)
Testing: Mocha
Process manager: PM2
Version control: Git, GitHub
Deployment OS: Ubuntu Server
Jenkins plugins: NodeJS Plugin, SSH Agent Plugin, Git Plugin
Quick Start (local)
Clone the repo:
git clone git@github.com:<YOUR_USERNAME>/node-ci-cd-jenkins.git
cd node-ci-cd-jenkins

Install dependencies:
npm install

Run the app:
npm start

Run tests:
npm test

Project Structure
/
├─ index.js
├─ package.json
├─ Jenkinsfile
├─ test/
│  └─ test.js
├─ .gitignore

CI/CD (Jenkins) — How it works

The Jenkinsfile (root) defines the pipeline stages:

Git Checkout — clone main branch.
Build — npm install.
Test — npm test (Mocha).
Deploy — SSH into the server, git pull, npm install, and pm2 restart or pm2 start.

Example deploy commands (run on server):

cd /home/ubuntu/nodeapp
git pull origin main
npm install
sudo npm install -g pm2
pm2 restart index.js || pm2 start index.js

Jenkins Setup (brief)
Install Jenkins and plugins: NodeJS, SSH Agent, Git.
Manage Jenkins → Global Tool Configuration → add NodeJS (name it: mynode).
Add credentials:
SSH private key in Jenkins (ID: ssh-cred) that has access to your server and/or repo.
Create a Pipeline job:
Definition: Pipeline script from SCM → Git → git@github.com:<YOUR_USERNAME>/node-ci-cd-jenkins.git → Credentials: SSH key → Branch: */main → Script Path: Jenkinsfile
Add webhook in GitHub: http://<JENKINS_HOST>/github-webhook/ to trigger on push.
How to Deploy Manually (server)
SSH to server:
ssh ubuntu@<SERVER_IP>

Go to app directory:
cd /home/ubuntu/nodeapp

Pull latest and restart:
git pull origin main
npm install
sudo npm i -g pm2
pm2 restart index.js || pm2 start index.js

Troubleshooting
SSH errors: check ~/.ssh/authorized_keys on server; ensure Jenkins key is added and permissions are chmod 600.
Tests failing: run npm test locally to inspect errors.
Jenkins can't clone: verify repository URL and Jenkins credentials (SSH vs HTTPS).
Contribution
Fork the repo
Create a branch: git checkout -b feature/xyz
Commit changes: git commit -m "feat: add xyz"
Push: git push origin feature/xyz
Open a Pull Request
