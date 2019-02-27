
Story 1: Deploy web application to an EC2 instance

Objectives:
- Understand the life-cycle of stacks in CloudFormation, common operations, and the permissions model
- Interpret the documentation to evolve stacks iteratively

Steps:
- Fork this repository into your account on github.com, then clone the same. 
    - (Alternatively, clone this repository and push to a new repository on your own github.com account)
- Refer to the [CloudFormation AWS Resource Types Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)
- Create a first template (using either YAML or JSON formats). YAML is both more expressive, and less verbose; when compared to a similar template in JSON.
- The `/templates` folder is not present on the `master` branch. This is where you will progress on the workshop when working on your own repository.

```
$ mkdir templates
$ touch templates/template.yaml
# Edit the template using your favourite text editor to implement the first story 

```

- (OPTIONAL) Create a service role for CloudFormation. See [here](../../service-role/HOW-TO.md) for instructions.

- Create the stack using the template. Repeat the actions below as needed to create the stack successfully.

```
# Choose a stack name. 
# Choose a suffix, (say, `-foo`) to the stack-name, if multiple people are attempting to create stacks on the same account in the region

$ export DEV_STACK_NAME=webapp-dev-foo 

# Create the stack (without a service role assigned)
$ aws cloudformation create-stack \
    --template-body file://templates/template.yaml \ 
    --stack-name ${DEV_STACK_NAME}   
    
# (optional) You may choose to assign a service role
# Create the stack (with a service role assigned)
$ aws cloudformation create-stack \
    --template-body file://templates/template.yaml \
    --role-arn arn:aws:iam::<AWS-ACCOUNT-ID>:role/AWS-CloudFormation-ServiceRole \
    --stack-name ${DEV_STACK_NAME}   

# Wait for stack creation to complete
$ aws cloudformation wait stack-create-complete \
    --stack-name ${DEV_STACK_NAME}   

# Describe stack events    
$ aws cloudformation describe-stack-events \
    --stack-name ${DEV_STACK_NAME}   

# Describe the stack
$ aws cloudformation describe-stacks \
    --stack-name ${DEV_STACK_NAME}   

```

- Enhance the template incrementally to add resources and resource configuration, or to fix issues
- Update the existing stack, or delete and re-create the stack   
    
```    
# Update the stack
$ aws cloudformation update-stack \
    --template-body file://templates/template.yaml \
    --stack-name ${DEV_STACK_NAME}   
    
# Wait for stack update to complete
$ aws cloudformation wait stack-update-complete \
    --stack-name ${DEV_STACK_NAME}   

# Delete stack (if needed)
$ aws cloudformation delete-stack \
    --stack-name ${DEV_STACK_NAME}   

```  

- Verify that the web server is running at the published IP address (available as a stack output)

```    
# Print the only output value from the stack
$ aws cloudformation describe-stacks \
    --query "Stacks[0].Outputs[0].OutputValue" \
    --output text \
    --stack-name ${DEV_STACK_NAME}   

```

- Commit the template to version control.

```
$ git checkout -b develop
$ git add templates/template.yaml
$ git commit -m "story-1 (work-in-progress)"
$ git push foo develop # If you have configured a remote (say, `foo`) that points to your own repository

```

- onwards to [kata-2](../kata-2/HOW-TO.md)