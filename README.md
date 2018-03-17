# Test Project
This project demonstrates a typical CI/CD design using AWS services.

## CI/CD Design
Typical CI/CD Pipeline
![Typical CI Pipeline](/img/cicd.svg)

1. SOURCE
1. BUILD
1. TEST
1. DEPLOY

### Source
Pull test-project from GitHub.

### Build
Build using steps defined in buildspec.yml.

### Test
Run unit tests. Automated checks can accelerate code development process.
Reviews can be done when these syntax/linters/basic-code-checks pass so
we don't waste developers' time. 

### Deploy
Deploy the code in the cloud.

## Pipelines
- [x] Check: Before code gets merged (e.g. pull requests)
- [x] Gate: After code is merged

## Implementation Proof-Of-Concept (PoC)
The CI/CD PoC was implemented using the following components:
- [x] AWS Codepipeline
- [x] AWS CodeBuild
- [x] AWS CodeDeploy
- [x] AWS EC2 Instance (t2.micro)*
*EC2 Ansible module used to deploy the EC2 instance: https://github.com/rafaelfolco/ansible-aws-deploy

### Gate Pipeline
For the sake of simplicity, only the "gate" pipeline has been implemented.
The pipeline stages are triggered for every change pushed to dev/ branch.

### AWS CodePipeline
![AWS CodePipeline](/img/codepipeline.png)
