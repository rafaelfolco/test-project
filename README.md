# Test Project
This project demonstrates a typical CI/CD design using AWS services. It includes a very basic Python application to be verified by CI/CD staging steps. These stages are described in more detail below. 

## CI/CD Design
Typical CI/CD Pipeline
![Typical CI Pipeline](/img/cicd.svg)

1. SOURCE
1. BUILD
1. TEST
1. DEPLOY

If any of these stages fail, the code can't be merged.  

### Source
Pull test-project from GitHub.

### Build
Compile the source code.
On AWS CodeBuild, build the source using steps defined in buildspec.yml.

### Test
Run unit/functional/integration tests. Automated checks can accelerate code development process.
Reviews can be done when these syntax/linters/basic-code-checks pass so we don't waste developers' time. 
This sample project includes a single unit test that can be run in the *TEST* stage.
```
$ python src/HelloWorld_tst.py 
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
```

### Deploy
Deploy the code in the cloud. Deployment stage is critical as it ensures the stack is elastic, that is, it can expand and shrink its instances. In a very dynamic environment, deploy stage can verify that the stack scales up/down due to transaction load peaks. For database instances running on RDS, for instance, one can verify how database can scale vertically and horizontally in the cloud:
* Vertically: Adding more resources to the RDS instance
* Horizontally: Adding more replicas w/ Load Balancer that syncs up with the master instance.
On AWS CodeDeploy, deploy the code using steps defined in appspec.yml.

## Pipelines
A typical CI/CD design has the following pipelines: 
- [x] Check: Before code gets merged (e.g. pull requests)
- [x] Gate: After code is merged

### Check
Ensure the code builds and tests run against submitted/proposed changes before actually merging the code.

### Gate
Ensure the merged change doesn't break anything. It's not so unusual to developers having to revert merged changes even after running all CI tests. Integration tests help to mitigate this issue. 

## Implementation Proof-Of-Concept (PoC)
To demonstrate a full CI/CD pipeline implementation, an AWS CodePipeline has been created using the console's wizard. The creation steps are not described in this document and can be easily found at the AWS documentation online. Refer to the AWS portal for more details. 
The CI/CD PoC was implemented using the following components:
- [x] AWS Codepipeline
- [x] AWS CodeBuild
- [x] AWS CodeDeploy
- [x] AWS EC2 Instance (t2.micro)

EC2 Ansible module used to deploy the EC2 instance: https://github.com/rafaelfolco/ansible-aws-deploy
```
ansible-playbook -i inventory/hosts playbooks/aws-deploy.yml 
```

### Gate Pipeline
For the sake of simplicity, only the "gate" pipeline has been implemented in this demo.
The pipeline stages are triggered for every change pushed to dev/ branch.

#### AWS CodePipeline (Gate)
![AWS CodePipeline](/img/codepipeline.png)
