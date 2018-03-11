# {{ cookiecutter.project_name }}

**This is an example of how to create a minimal pipeline for SAM based Serverless Apps**

## Requirements

* AWS CLI already configured with at least PowerUser permission

## Pipeline creation

Run the following AWS CLI command to create your first pipeline for your SAM based Serverless App:

```bash
aws cloudformation create-stack \
    --stack-name {{ cookiecutter.project_slug }}-pipeline \
    --template-body file://pipeline.yaml \
    --capabilities CAPABILITY_NAMED_IAM
```

This may take a couple of minutes to complete so give it some time and then run the following command to retrieve Git repository and other important resources to be aware of:

```bash
aws cloudformation describe-stacks \
    --stack-name {{ cookiecutter.project_slug }}-pipeline \
    --query 'Stacks[].Outputs'
```

Next, you'll need to Copy `buildspec.yaml` (CodeBuild) and `template.yaml` (SAM) to the root of your Git repository recently cloned. From here, you can go ahead and customize the build process depending on your code structure -- Comments have been added to both files for your convenience.

## Information about services used


### CodeCommit

AWS CodeCommit can be used as the source code repository for your existing project.

> **Note**: [Github can also be used with AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/GitHub-authentication.html)

### CodeBuild

AWS CodeBuild can be used to install Python dependencies, run Python tests, security dependency checks and anything else useful you can think of that can be run within a Docker container.

The important part here is the SAM Packaging that will be made available as an Artifact for Cloudformation to deploy at the deployment stage within the pipeline.

**Important:** Copy ``buildspec.yaml`` to the root of your source code repository.

> **Note**: You can also integrate [Jenkins with CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/jenkins-plugin.html) should you already have a Jenkins pipeline in place over CodePipeline

### CodePipeline

AWS CodePipeline can be used to orchestrate the pipeline to enable CI/CD for your SAM based Serverless App. 

As other AWS Services can be used within the pipeline it is important to create [IAM Service Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) and define the appropriate permissions to ensure these AWS services will be able to create, manage and update resources accordingly.

For this example, we create Cloudformation, Codepipeline and S3 Bucket (code artifacts) with appropriate permissions to execute within this pipeline sample - Do however ensure you adjust the permissions to fit within your needs.

# TODO

* [ ] Copy CodeBuild ``buildspec.yaml`` to the root of your git repository
* [ ] Copy SAM ``template.yaml`` to the root of your git repository
* [ ] Update ``buildspec.yaml`` to reflect your code structure to ensure dependencies and custom activities are executed correctly
