# How To Create YAML Pipelines In Azure DevOps For A .NET API Project.

## Prerequisites
- Basic understanding of Azure DevOps concepts
- An Azure DevOps account and organization (see [Getting Started](../../docs/actually-getting-started.md) for setup instructions)
- A Microsoft account (for accessing Azure DevOps)
- Git installed on your local machine
- .NET SDK installed (latest or LTS version)
- Visual Studio (IDE)
- A simple .NET API project to work with (or you can create a new one during the tutorial)
- Basic knowledge of YAML syntax
- Familiarity with .NET development
- Access to Azure Portal

## 🔍 The Breakdown

- [Setup project in Azure Devops](#setup-project-in-azure-devops)
- [Upload ASP DOTNET Core Web API project to Azure Devops](#upload-asp-dotnet-core-web-api-project-to-azure-devops)
- [Create CICD Pipeline](#create-cicd-pipeline)
- [Create A Azure Web App](#create-a-azure-web-app)


## Setup project in Azure Devops
- Login to your **Azure DevOps** account [link](https://dev.azure.com)
- Click on New Project 

![image](https://github.com/user-attachments/assets/cad193d7-5aa6-4b00-9c3a-243774a95196)

- Fill in a project name (Description optional)
- Version Control will be Git
- Work Item process will be scrum

![image](https://github.com/user-attachments/assets/3f0ad9ba-e861-4d3d-b4e5-bf9139497e36)

[![Scrum Process](https://img.shields.io/badge/Scrum_Process-Guide-blue?style=flat-square)](https://learn.microsoft.com/en-us/devops/plan/scrum) [![Git Version Control](https://img.shields.io/badge/Git_Version_Control-Tutorial-green?style=flat-square)](https://learn.microsoft.com/en-us/azure/devops/repos/git/gitworkflow?view=azure-devops)

That's pretty much it :)

![image](https://github.com/user-attachments/assets/954938c2-338d-4c0d-9e08-68774a2aa934)

You have sucesfully created you project on Azure DevOps. Interesting part. I was at work and my bro Sim was like how do we get our project (Source Code) into Azure DevOps. "I was like damn I genuinely don't know bro", pretty much how this repo was created haha.

## Upload ASP DOTNET Core Web API project to Azure Devops
In here we will assume you are familiar with .NET Development , if not you can look at this [Create web APIs with ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-9.0). Im pretty sure there's a couple ways to do this. We just taking a simpler route.

- We have our Web API project on Visual Studio

![image](https://github.com/user-attachments/assets/0ba98883-0618-4be7-8e57-998ca8b5f70b)

- Add Source Control
- Select Git
  
![image](https://github.com/user-attachments/assets/4edb2906-0ce2-4584-b98c-046a2ea1efef)

- Select Azure DevOps
- Make sure your microsoft account used for Azure DevOps is linked up (will take some time to reflect maybe)
- Organization might be preselected
- If you have more than one project on Azure DevOps select the one you created for this tutorial at the start
- Create and Push

![image](https://github.com/user-attachments/assets/b311ce25-3ab5-48d8-a022-d4f3b186ee4e)

- If you have the same browser open from Azure DevOps, Refresh
- You will probably think im playing around , because wheres the source code?
- Click on the branch on the highlighted section and select what you pushed up.

![image](https://github.com/user-attachments/assets/a7e6f649-c44a-4c2b-b9da-f201329e36c5)

Then you should see 

![image](https://github.com/user-attachments/assets/83847eef-d2bf-4c0d-8141-d67a31ebe8a6)

For this tutorial , lets change the branch name , master is bad.

- Click on branches
- Create a new branch
- Call it main
- Click on 3 dots, delete the master. (displays when you hover over the branch itself)
- Then click 3 dots for main and click on **set as default** and **set as compare**

![image](https://github.com/user-attachments/assets/225e9def-2e04-48ed-a835-1c67b1a6d6b1)

- Don't forget to pull these changes on your local :)
- On your Visual Studio project
- Click on the up and down arrow and select pull
- Click on the section next to it that says **master**
- Click on Remote, Select main branch and boom sorted
- LETS GO !!! 

![image](https://github.com/user-attachments/assets/207829ea-468d-4d29-a842-8790aaa9f5a2)

## Create CICD Pipeline
At this point we basically going to create a YAML pipeline in Azure DevOps for a .NET API project.

### Things to note:

- **YAML:** A straightforward, human-readable language used to write configuration files.
- **YAML Pipeline in Azure DevOps:** In Azure DevOps, a YAML pipeline is a script written in YAML that defines the automated process to build, test, and deploy your code.
- YAML is very indent-sensitive 

### Let's begin:

- Head over to the section called pipelines
- Click create pipeline

![image](https://github.com/user-attachments/assets/e33b4069-879f-4278-9590-09d22053d44b)

- Select where your code is
- Ours is inside Azure Git Repos which we set up 

![image](https://github.com/user-attachments/assets/9bec37e1-71b4-4914-9e32-04df911dbba2)

- Select the repository

![image](https://github.com/user-attachments/assets/43487f2e-65ce-443e-89fb-2c569e5d3777)

There are loads of templates to get you started right off the bat if you are uncertain

but we'll learn it from scracth :) 

- Click on Starter pipeline

![image](https://github.com/user-attachments/assets/d97180ba-a38d-411f-b1e2-83a223adc82b)

- Remove exisiting Code 

![image](https://github.com/user-attachments/assets/4438554b-8b22-4489-92dc-786a3c6db415)

### Time for YAML:

**In the code:** 

```yaml
trigger:
 branches:
   include:
     - main

stages:

- stage:  Build
  jobs:
    - job: Build
      displayName: 'Build Job'
      pool: 
        vmImage: 'windows-latest'
```

#### trigger
This lets us know when the pipeline should run and defines the condition to execute the pipeline

#### branches
Which lets us know the trigger is related to the branch within our repository, we will include that branch name here, you can have many, we using one.

#### stages
This is where we can build our project and publish our project , we can have many stages that defines it's purpose like build, test and deploy.

#### jobs
These stages will have jobs can be one or many jobs that relate to the stage. A job is basically a task that will be given to a specifc pool(VM) I choose.

#### displayName
Human-readble name that defines the job 

#### pool
Stages require a type of virtual machine in order to run. This is how we specify what image we would like to run. there can be many.

#### vmImage
Specifies what virtual machine we going to use e.g windows-latest or ubuntu-latest for linux users

**Side Note:** You will notice that it gives you some hints as you typing

![image](https://github.com/user-attachments/assets/6e04fc8f-3169-4a2c-a9f5-a8aeb5d0fb05)

**Next step: We need steps** 

```yaml
# Build Stage  
steps:
        - task: UseDotNet@2
          inputs:
            packageType: 'sdk'
            version: '9.x'
            installationPath: $(Aget.ToolsDirectory)/dotnet
```

#### steps
Basically jobs will contain steps that defines what has to happen.

#### task
`UseDotNet@2`, since we have a dotnet project, this will install and use the latest version of the dotnet sdk on our VM that we are using on azure.

#### inputs
Everything the dotnet sdk needs to run.

#### packageType
`sdk` Refers to the sdk which comes with compilers and tooling, things needed to run dotnet

#### version
`9.x` Since our project has been set to .NET 9 , we going to specify that here and x just means latest (need to dive in this abit more)

#### installationPath
This says where our sdk should be installed on the vm. This `$(Aget.ToolsDirectory)/dotnet` is a prebuilt directory in Azure DevOps which we can use.

**Run a Script:**

```yaml
        - script: |
           dotnet build --configuration Release
          displayName: 'Build Solution'

        - script: |
           dotnet publish -c Release -o $(Build.ArtifactStagingDirectory)
          displayName: 'Publish Artifacts' 
      
        - task: PublishBuildArtifacts@1
          inputs:
            PathtoPublish: $(Build.ArtifactsStagingDirectory)
            ArtifactName: drop
            publishLocation: Container
```

#### script
The `script` step is used to define the shell commands that should be executed. The `|` symbol tells YAML to allow multiple lines of shell script commands.

#### dotnet build --configuration Release
If you created and ran projects on .NET you would be familar with this part if you used the CLI, which basically when you change some code and hit the play button, praying that the build is succesfull. We basically want to build our project to see if everything is goodie through scripts with this command. The command itself will build it in release mode. Typically used for Production Builds.

#### dotnet publish -c Release -o $(Build.ArtifactStagingDirectory)
This command will publish the compiled output to the directory. `$(Build.ArtifactStagingDirectory)` is predefined variable that points to a location where build artifacts such as binaries are stored. It is also reusable.

#### PublishBuildArtifacts@1
There are built-in tasks in Azure DevOps which can be used. This one is used to publish the build artifacts and wraps all the packages, images compiled code to Azure DevOps. which can also be used in different step stages like deploy stage etc.

#### PathtoPublish
Tells the task where to find the artifacts to publish which would be `$(Build.ArtifactsStagingDirectory)`

#### ArtifactName
`drop` which specifies the name of the artifact , this is the common name for it.

#### publishLocation
`Container` where to store the artifact. Stores the artifact within Azure DevOps internal storage

**Next step: Another Stage** 
Usually this would have a test between the build and deploy but for this tutorial lets keep it simple and skip that. Going straight to deploy since we have already set up our entire build stage.

```yaml
- stage: Deploy
  jobs:
    - deployment: DeployWebApp
      displayName: "Deploy to Azure App Service"
      environment: 'production'
      strategy:
       runOnce:
          deploy:
             steps:
               - download: current
                 artifact: drop
                 
               - task: AzureWebApp@1
                 inputs:
                   azureSubscription: ''
                   appType: 'webApp'
                   appName: ''
                   package: $(Pipeline.Workspace)/drop
```

#### Deploy
Our second stage for deploy. Since our first stage above was for handling the building of an artifact and everything our second stage will be for deploys to Azure.

#### deployment
A special job in Azure DevOps that manages deployment to an environment. Allows us to define deployment strategies and actions

#### environment
Shows where we deploying to e.g QA, Development, Staging and Production.

#### strategy
How deployments are done ? `runOnce` basically the deployment strategy for the job. Ours is simple since its one project only 

#### deploy
Where the action for this process takes place. Requires steps too, in order to `download: current` the current artifact which is cooked up in our build stage. 

#### AzureWebApp@1
Another task , but instead of us installing the dotnet sdk , we are using this to deploy to an Azure Web Service which will be in our Azure Portal 

#### azureSubscription
A service connection.

#### appType
Type of Azure Resource to deploy to.

#### appName
From the Azure Portal, name of the web app itself

#### package
The path to the package that will be deployed. `$(Pipeline.Workspace)/drop` predefined variable that points to the workspace which has the downloaded artifacts which sits inside `/drop` that we specified above

So at this stage we actually require an app service to complete the pipeline, as it is now. it will fail since the inputs have not been provided.

## Create A Azure Web App
In order for us to fill in our inputs for the `AzureWebApp@1` task, we will need to create a web app on Azure Portal. [Login](https://portal.azure.com/#home)

![image](https://github.com/user-attachments/assets/e8a5d567-2b5d-4c91-a5fa-55c31c6d8de4)

- Click on App Services

![image](https://github.com/user-attachments/assets/6215face-0704-41f2-8ff2-3f78b623959b)

- Create new Web App

![image](https://github.com/user-attachments/assets/dae7118b-f369-49ec-a09b-858ee965689a)

This is pretty cool for good for websites and apis.

At the time of this I didnt really have a subscription, so i just used the default Subscription name :).

- Create a new resource group 

![image](https://github.com/user-attachments/assets/71efa4f3-0366-4605-9506-fb0227a24ffa)

**Instance Details**

- Name has to be unique
- Publish : Code
- Running Stack: the SDK version you're using (.NET 9 or 8 etc)
- Operating System: Windows (could differ if you're using something else)
- Then click Review and Create
- Region: South Africa North (just because)
- Windows Plan: Auto generated
- Pricing Plan: Standard S1 (Think i had some credits , this is a new azure account lol)
- Click Review + Create

![image](https://github.com/user-attachments/assets/c2c0f214-6197-4663-8a1a-aae981b66fcb)


- Then click create again

![image](https://github.com/user-attachments/assets/2299b3dd-482f-4a7a-9c15-c323cf174898)

- Once deployment is complete Click on the link next to `Resource Group`

![image](https://github.com/user-attachments/assets/ab69b179-cd83-436f-856a-74926abfd5ed)

- woohoo we got it
- Make sure to copy name higlighted and click on browse

![image](https://github.com/user-attachments/assets/1e9e1e3f-e2d4-4f3e-8b51-0dd627f732fc)

This is the current code thats deployed , note that it is just a normal template without changs so this is normal since we still have to deploy our code to it.

![image](https://github.com/user-attachments/assets/c5052317-b6ed-4f2d-9c36-cc8dc12af197)

So now with the name we copied head back to our pipeline
- Add the **appName**
```yaml
appName: 'lpazuredevopsdemo'
```

- Create azureSubscription
- Make sure to copy all code inside and paste it in your notepad or something icase you lose it :(
- azureSubscription is not the Subscription ID on Azure Portal of the web app created, its the service connection name
- Click on Project Settings at the bottom but Use keys `ctrl + Left Click to open a new tab`

![image](https://github.com/user-attachments/assets/3446bbb2-d555-49f9-aa95-e436e2703116)

- Click on Service connections under pipelines:

![image](https://github.com/user-attachments/assets/ee850c39-4efd-4538-8cbc-6f4364e27c08)

- Click on create service connection
- Select Azure Resource Manager
- Automatically selects your subscription
- If there's a delay in getting your resource group
- Likely a pop up to sign in thats causing the issue
- After signing in from the pop up , access to resource groups given :)
- Then when you give your Service connection Name
- Copy that name , it will be used in the pipeline 
- Grant access permissions to all pipelines

![image](https://github.com/user-attachments/assets/f19e1cb4-fa2d-4a4e-ad03-fdc4956c9b27)

**Success !**

![image](https://github.com/user-attachments/assets/4c87f177-ceeb-470a-9740-7640769fd7ae)


**Head back to pipeline** 
- Paste the Service connection name
- Should look something like this

```yaml
- task: AzureWebApp@1
                 inputs:
                   azureSubscription: 'azuredevopsdemo'
                   appType: 'webApp'
                   appName: 'lpazuredevopsdemo'
                   package: $(Pipeline.Workspace)/drop
```

**Everything should be good now**
- Hit Save and run
- If anything is borken you will notice it immediately
- Commit message 
- Lets gooo!

![image](https://github.com/user-attachments/assets/1c2713b6-3a8a-4bac-b556-28c29ce5973f)

Error given:

`##[error]No hosted parallelism has been purchased or granted. To request a free parallelism grant, please fill out the following form https://aka.ms/azpipelines-parallelism-request`

Reason: For Microsoft-hosted parallel jobs, you can get up to 10 free Microsoft-hosted parallel jobs that can run for up to 360 minutes (6 hours) each time for public projects. When you create a new Azure DevOps organization, you aren't given this free grant by default.

For private projects, you can get one free job that can run for up to 60 minutes each time. When you create a new Azure DevOps organization, you may not always be given this free grant by default.

To request the free grant for public or private projects, submit a [request](https://aka.ms/azpipelines-parallelism-request).

![image](https://github.com/user-attachments/assets/5068d5b6-ca19-4fb1-8d45-645aa45c2adb)



