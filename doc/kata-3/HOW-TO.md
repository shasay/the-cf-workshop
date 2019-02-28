Story 3: Add an Auto Scaling Group and Elastic Load Balancer

Objectives:
- Focus on iterative development of the template (and not on mundane provisioning tasks); relying upon rapid feedback from the pipeline

Steps:
- Remember to commit changes to the template to version control on the `develop` branch, and push changes to the `remote` to provision the stack in the `dev` environment

```
$ git add templates/template.yaml
$ git commit -m "Story 3 (work-in-progress): my change"
$ git push foo develop

```

- As the pipeline obtains and deploys template changes to the stack, you can monitor the stack changes from the console 
- Perform each of the following changes atomically as commits and push to the `develop` branch
    - Add a [Lifecycle Configuration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-as-launchconfig.html) to the template
    - Add an [Auto Scaling Group](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-as-group.html) to the template 
    - Add an [Elastic Load Balancer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-elb.html) to the template
    - Change the template output to print the load balancer DNS address. Verify that the website is available at the published DNS address
    - Remove the EC2 instance for the webserver, as it is no longer needed 
- Commit the changes and move on to [kata-4](../kata-4/HOW-TO.md)
