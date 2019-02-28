Story 2: Create a pipeline for Continuous Deployment

Objectives:
- Put in place a rapid feedback mechanism for development of CloudFormation templates, using a pipeline

Pre-requisites:
- Complete [kata-1](../kata-1/HOW-TO.md) and commit the newly-created basic template to `templates/template.yaml` on the `develop` branch locally  

Steps:
- (if not already done) Create a repository on your account on github.com, and 
    - Add it as a "remote" to your local repository
    - Push your local changes on the `develop` branch on the remote
    - Obtain a [personal access token](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line)

```
# Add a remote pointing to the upstream repository on github.com, push to the upstream remote
$ git remote add foo <YOUR_REPOSITORY_CLONE_URL_HERE>
$ git push -u foo develop

```
    
- Create a pipeline to deploy the stack off the `develop` branch
    - Copy [pipeline.yaml](pipeline.yaml) to templates/pipeline.yaml
    - Copy [pipeline-parameters.example.json](pipeline-parameters.example.json) to templates/pipeline-parameters.json
    - _IMPORTANT_: Edit `templates/pipeline-parameters.json`. 
        - Provide the stack name, repository name, the repository owner, and personal access token

```
$ export DEV_PIPELINE_STACK_NAME=pipeline-webapp-dev-foo

# Create a stack to provision the pipeline in CodePipeline
$ aws cloudformation create-stack \
    --template-body file://templates/pipeline.yaml \
    --parameters file://templates/pipeline-parameters.json \
    --capabilities CAPABILITY_IAM \
    --stack-name ${DEV_PIPELINE_STACK_NAME}

$ aws cloudformation wait \
    stack-create-complete \
    --stack-name ${DEV_PIPELINE_STACK_NAME}
    
```

- Ensure that the pipeline runs successfully and deploys the CloudFormation stack in the `DEV` environment. You will now have two stacks:
    - one that provisions the pipeline in [AWS CodePipeline](https://aws.amazon.com/codepipeline/), and 
    - another that is provisioned _BY_ the pipeline
- All subsequent template changes will be deployed to the stack by the pipeline, on changes being pushed to the `develop` branch. Delete any stack created earlier by hand
- onwards to [kata-3](../kata-3/HOW-TO.md) 