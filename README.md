# azureDockerPush.ado
This template utilises pushing the docker images from development to staging environment in azure. It follows the two tasks: Get and Set the latest image from the ACR, Import image from the registry


This repository provides a common Azure DevOps pipeline templates that simplifies the following step: 
1) Pushing the Docker Image from Dev to Staging
   

## Pipeline Requirements

The pipeline requires the following parameters to be defined:
Paramaters:


| Name  | type | Default | Values | Opional/Required | Comments |
| ------------- | :-------------: | :------------- | ------------- | ------------- | ------------- |
| serviceConnectionType | string |  | | Required | This enables to define the type of service connection for Azure |
| serviceConnection | string |  | | Required | This enables to define the name of Service connection for Azure |
| imageACR | string |  | | Required | This enables to define the Azure Container Registry (ACR) name |
| imageRepository | string |  | | Required | This enables to define the Docker image repository name |
| azureSubscriptionNameStaging | string |  | | Required | This enables to define the Azure subscription name for staging |
| sourceContainerRegistry | string | | | Required | This enables to define the name of source container registry |
| targetContainerRegistry | string |  | | Required | This enables to define the name of target container registry |
| sourceRepositary | string |  | | Required | This enables to define the name of target container repository |
| targetRepositary | string |  | | Required | This enables to define the name of target container repository |
| enableSourceImageTag | string | "No tag Present!" | | Required | This parameter provides the use cases for source image tag inputs | 
| promoteImage | boolean | true | true/false | Required | This parameter provides the use cases whether to use promote image step or not |
| azureSubscriptionDev | string | | | Required | This parameter provides the name of Azure Subscription for Dev environment |


  These parameters provide multiple use case options for the nightWatch pipeline.



### Variables:

In order to use these templates in pipeline, following pipeline variables must be set:

| Name  | Opional/Required | Value | Comments |
| :------------- | :-------------: | :-------------: | :------------- |
| currentDate  | Required | $(Get-Date -Format yyyyMMddhhmmss) | This variable sets the current date and time in a specific format |
| sourceImageTag | Required | "No tag Present!" | "sourceImageTag" variable specifies the default source image tag |



### Pushing the Docker Image from Dev to Staging

The following example showcases how to use this template in a systematic approach. For example: 

```yaml

# azure-pipeline.yaml
resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/azureDevopsPush.ado
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

stages:
  - stage: Test
    displayName: Run Tests    
    jobs:
    - deployment: ImportImage
      displayName: Import Image
      environment: Test
      strategy:
        runOnce:
          deploy:
            steps:
            - checkout: Template
            - template: DockerPushFromDevTOStage.yml@Template
              parameters:
                ${{ insert }}: ${{ parameters }}
   

```

The parameters are provided to configure the template into the pipeline according to the desired build configuration and stages.

Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

 



