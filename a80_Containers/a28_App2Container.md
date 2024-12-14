
# App2Container (A2C)

1. **101**
   1. Is a CLI tool to migrate and modernize Java & .NET web apps into Docker Containers
   2. Lifts & Ships in any of the below environments into AWS
      1. On-prem running bare metal app
      2. Virtual machines
      3. Other cloud providers
2. Advantage
   1. No code change to migrate legacy apps
   2. Supports pre-built CICD pipelines
3. How achieved?
   1. Behind the scene, CloudFormation templated is used
   2. Registers generated docker containers into ECR
   3. Deploys to ECS, EKS or App Runner
4. Steps to deploy
   1. Discover & Analyze
      1. Create app inventor & analyze runtime deps
   2. Extract & Containerize
      1. Extract app with deps and create docker image
   3. Create deployment artifacts
      1. Generate ECS tasks
      2. EKS Pod definitions
      3. Create CICD pipelines
      4. ...
      5. (Done ^ using CloudFormation template)
   4. Deploy to AWS
      1. Image stored in ECR
      2. Deployment done in ECS, EKS or App Runner

