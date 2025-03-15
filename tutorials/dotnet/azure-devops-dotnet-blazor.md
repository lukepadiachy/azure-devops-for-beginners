# Learn how to create a Classic pipeline in Azure DevOps for a .NET Blazor project.

## Prerequisites
- Basic understanding of Azure DevOps concepts
- An Azure DevOps account and organization (see [Getting Started](../../docs/actually-getting-started.md) for setup instructions)
- A Microsoft account (for accessing Azure DevOps)
- Git installed on your local machine
- .NET SDK installed (latest or LTS version)
- Visual Studio (IDE)
- A simple Blazor project to work with (or you can create a new one during the tutorial)
- Familiarity with .NET development
- Access to Azure Portal

# Azure Classic Pipelines vs. YAML Pipelines

Azure DevOps provides two methods for setting up CI/CD pipelines: **Classic Pipelines** and **YAML Pipelines**. Understanding their differences can help you choose the best approach for your projects.

## Classic Pipelines

- **Graphical User Interface (GUI):** Basically no need to write YAML, allows you to define build and release processes using interactive forms and dialogs.

- **Storage:** Configurations are stored within the Azure DevOps project and are not versioned alongside the application code.

- **Use Case:** Basically for users who prefer a visual setup.

## YAML Pipelines

- **Code-Based Configuration:** Using YAML syntax and stored as code within the repository. This approach treats pipeline configurations as code, enabling version control and collaboration.

- **Version Control Integration:** Since the pipeline configuration resides in the repository, it follows the same branching and versioning as the application code, promoting consistency across different branches.

- **Features:** Supports templates and runtime parameters, enhancing reusability and flexibility.

- **Use Case:** Ideal for teams embracing "infrastructure as code" practices and seeking better control over their CI/CD processes.

## Disabling Classic Editor Pipeline by Default

To encourage the usage of YAML pipelines and align with modern DevOps practices, Azure DevOps introduced a feature allowing organizations to disable the creation of classic pipelines. This setting can be enabled at both the organization and project levels. When activated, it prevents the creation of new classic build pipelines, release pipelines, task groups and deployment groups. However, existing classic pipelines remain functional and users can still edit and run them.

It's important to note that classic pipelines are not deprecated and continue to be supported. The decision to disable the creation of new classic pipelines by default in new organizations aims to encourage the use of YAML pipelines, aligning with modern DevOps practices that emphasize configuration as code.

For a visual comparison and deeper insights into the differences between Classic and YAML pipelines, you might find the following video helpful:

[Azure Pipelines: Classic vs YAML - Build and Release Comparison](https://www.youtube.com/watch?v=3cGtA__dKUc)

## 🔍 The Breakdown

- [Setup project in Azure Devops](#setup-project-in-azure-devops)
- [Create a Blazor project for Azure Devops](#create-a-blazor-project-for-azure-devops)
- [Create CICD Pipeline](#create-cicd-pipeline)
- [Create A Azure Web App](#create-a-azure-web-app)
- [Deploy To Azure Web App](#deploy-to-azure-web-app)

## Setup project in Azure Devops
1. **Login to Azure DevOps**  
   Go to [Azure DevOps](https://dev.azure.com) and log in with your Microsoft account.

2. **Create a New Project**  
   - Click on **New Project**.
   - Fill in the project name (e.g., `BlazorDemo`).
   - Set **Version Control** to `Git`.
   - Set **Work Item Process** to `Scrum`.

![image](https://github.com/user-attachments/assets/6199aa3e-5c8b-494d-b670-89380690f873)

![image](https://github.com/user-attachments/assets/c7e5203a-1911-42d0-bd2f-c690fa16863f)

[![Scrum Process](https://img.shields.io/badge/Scrum_Process-Guide-blue?style=flat-square)](https://learn.microsoft.com/en-us/devops/plan/scrum) [![Git Version Control](https://img.shields.io/badge/Git_Version_Control-Tutorial-green?style=flat-square)](https://learn.microsoft.com/en-us/azure/devops/repos/git/gitworkflow?view=azure-devops)

 ### Key Terms:
   - **Version Control (Git):** A system to track changes in your code. Git is the most popular version control system.
   - **Work Item Process (Scrum):** A framework for managing work in Agile projects. Scrum helps teams organize tasks into sprints. 

**Enable Classic Pipelines**  
   By default, Azure DevOps may disable Classic Pipelines. To enable them:
   - Go to **Project Settings** → **Pipelines** → **Settings**.
   - Disable the following options:
     - `Disable creation of classic build pipelines`
     - `Disable creation of classic release pipelines`

![image](https://github.com/user-attachments/assets/8683c078-b025-4acb-b424-ca701cc42de9)

![image](https://github.com/user-attachments/assets/b10862e2-51ac-44bd-a869-42b836c58d7a)

### Key Terms:
   - **Classic Pipelines:** A visual, GUI-based way to create CI/CD pipelines in Azure DevOps. It’s easier for beginners compared to YAML pipelines.

Boom , we pretty much ready to go :).

## Create a Blazor project for Azure Devops
In here we will assume you are familiar with .NET Development , if not you can look at this [Create a Blazor WebApp with ASP.NET Core](https://dotnet.microsoft.com/en-us/learn/aspnet/blazor-tutorial/intro). Im pretty sure there's a couple ways to do the sending of the project to Azure DevOps. We just taking a simpler route.

**Set Up Your Blazor Project in Visual Studio**  
   - Create a new Blazor Web App project in Visual Studio.
   - Ensure the project builds and runs locally.

![image](https://github.com/user-attachments/assets/ceda324f-29a6-4a9f-93a2-3eb359f1d33b)

**Push to Azure DevOps**  
   - In Visual Studio, go to **Git Changes** → **Push to Azure DevOps**.
   - Select your organization and repository.
   - Create and push

![image](https://github.com/user-attachments/assets/baa0bbba-a0c7-4dd5-9efb-0544c7052d85)

![image](https://github.com/user-attachments/assets/73833267-1eec-46ef-876a-5d8a4cea81cb)

once thats completed lets head over to DevOps

**Branch Management**  
   - In Azure DevOps, create a `main` branch for future work.
   - Set `main` as the default branch.
   - Create a `develop branch` too

   ### Key Terms:
   - **Default Branch:** The primary branch used for deployments. Changes merged into this branch trigger the pipeline.

![image](https://github.com/user-attachments/assets/477400e7-91b1-49cf-a090-1bb9ddf77c8a)

There we go heres our project

![image](https://github.com/user-attachments/assets/bc726b55-933c-405f-bbe3-a93579dde89f)

Im going to do some small bonus runs 

- Click on branches
- Select new branch

![image](https://github.com/user-attachments/assets/3b61241a-babc-434a-ad8a-bcbd1f81919b)

Basically we going to end the master, whoever he/she is.

Let's call it main 

- Select Options (when you hover over main you will see three dots)
- Set it as the default branch 

![image](https://github.com/user-attachments/assets/772cbc85-52d0-41e2-b2b4-728ead98a1fd)

Then I'll create another branch and call it develop .

whoop we pretty much sorted.

![image](https://github.com/user-attachments/assets/4c0056d9-40bf-4573-96af-2a344f1d31bb)


## Create A Azure Web App
I thought well since we going to use a WebApp hosted on azure let's cook it up right-away, since the resources could take some time to kick-in when you need it. [Login](https://portal.azure.com/#home). If you are not familiar with this process, by all means, there's docs for it too [Docs](https://learn.microsoft.com/en-us/azure/azure-portal/)

- Click on **App Services**

![image](https://github.com/user-attachments/assets/f56eeaae-7ecf-49a9-b8d5-61dec589b5f9)

- Create, then within the drop-down, select Web App

![image](https://github.com/user-attachments/assets/317ec3d4-5f6f-4f46-b270-29b5d7431276)

- Once you have filled in all the deets

![image](https://github.com/user-attachments/assets/7cd2b3ac-0d17-427b-8e08-eb8b5a63d14b)

- Click Review and Create 
- Click Create after that again

Then its abit of a waiting game here 

![image](https://github.com/user-attachments/assets/7f194a7a-bf60-4210-ad13-8cfd973fc864)

- Once completed you can click on your resource group 
- Then Click on your App Service 

![image](https://github.com/user-attachments/assets/c5d55e99-8b40-4455-b9df-faa81e8e2b05)

- Scroll abit down to domains 

![image](https://github.com/user-attachments/assets/f31c7d32-175d-42ac-8f07-36011f6cea9f)

And Whoop we are half way there 

![image](https://github.com/user-attachments/assets/fa332e87-7ab1-4af0-87fc-365a19e85a16)


## Create CICD Pipeline
At this point we basically going to create a Classic pipeline in Azure DevOps for our Blazor project.

- Click on Pipelines
- Click on Create Pipeline

![image](https://github.com/user-attachments/assets/d2b72cbe-e8a5-4d39-87ea-f9af7d108810)

First problem if you havent turned the classic pipelines off you will have a view like this, also you'll be thinking I played you ! haha 

![image](https://github.com/user-attachments/assets/cfc4984d-b58c-4f84-bc73-7c72d0d47e9f)

So when its enabled , you should see 

![image](https://github.com/user-attachments/assets/b653432e-357d-4dd4-b3d8-7f880094e34a)

- Select **Use the classic editor**
- Make sure all the deets are correct
- Be sure the branch is on main as this is the one we'll be using

![image](https://github.com/user-attachments/assets/c4355439-cf46-4f20-ad76-c7053ed32242)

- Search ASP.NET Core
- Click on the template

![image](https://github.com/user-attachments/assets/bcf1c05b-b29a-46c8-8a4c-1155dea5e4ed)

- Let's do some small set ups
- Click on Pipeline 
- Select Azure Pipelines as the Agent Pool
- Select windows-latest from the drop-down

![image](https://github.com/user-attachments/assets/203bc5b3-5d6b-4d30-acc2-98915bf24ca3)

- Click on Agent Job 1 and do the same thing under **Agent Selection**

![image](https://github.com/user-attachments/assets/efcc26ad-ccc3-4a3e-8a73-bcf40ce4d13d)

- Let's disable the tests since we have no tests

![image](https://github.com/user-attachments/assets/a39a11d4-61e9-47fd-940e-16a34bd1ac4b)

Then we can save and queue

![image](https://github.com/user-attachments/assets/53597fcb-7df0-449b-8a80-306fd7c64dfd)

Basically if you ran your project on Visual Studio before we loaded it onto Azure. Probably refresh your screen if it loads abit long 

Then you should see the pipeline running :

![image](https://github.com/user-attachments/assets/bb4b7a53-13e7-444d-83dd-da46924cf790)

![image](https://github.com/user-attachments/assets/fccd1d72-b703-4a6d-add7-8b13639f8063)

## Deploy To Azure Web App
Let's get this onto our Web App hosted in Azure !

- Click on Releases

![image](https://github.com/user-attachments/assets/7c3cff52-2255-4ce5-b95b-2f45b08c09e6)

- We can close this for now 

![image](https://github.com/user-attachments/assets/b41255c3-273b-4397-9176-347081dc77fa)

- Add an artifact
- Select your project that was built from the first pipeline we created within the Source (Build Pipeline)
- Click on Add

![image](https://github.com/user-attachments/assets/0fa3800f-2b2f-460f-9a07-2fc004f2bb40)

- Click on the lightning icon for Continous deployment
- Enable it on top and close
  
![image](https://github.com/user-attachments/assets/37b42902-2769-4e5f-9c7b-4eed9b7fd550)

- Add a stage
- Select **Azure App Service Deployment**
- Click Apply 

![image](https://github.com/user-attachments/assets/bbd7c0d3-60b6-4314-9b3a-62650dd079f0)

Once added it will shine red as we need to add some extra information

- Click on it 
- Click Stage 1
- Select your subscription
- Authorize it 
- Select your App Service name

![image](https://github.com/user-attachments/assets/2a321e72-eaf7-4395-8746-67ed9d055c88)

- Click on Run on Agent
- Under Agent Selection choose Azure Pipelines as Agent Pool then windows-latest as Agent Specification

![image](https://github.com/user-attachments/assets/3969bde7-c3e4-4317-9e9b-8fdd6f6f7f90)

- Click on Deploy to Azure App Service
- Change the Package or Folder Directory 

![image](https://github.com/user-attachments/assets/d9a7aef1-d198-4182-b23f-94906c99c5c2)

- Select the zip file

![image](https://github.com/user-attachments/assets/b899f2b8-e6d4-4e03-829a-7236f72b921a)
 
- Click save
- Create the release

![image](https://github.com/user-attachments/assets/f9230eee-7b0f-4fd5-a40c-14f0740e1c1b)

Once it has succeeded

- Head back to your site
- Click Refresh

And woohoo you have deployed your Blazor App 

![image](https://github.com/user-attachments/assets/40eb4c12-3371-4755-9bd6-2223e687a6b1)

Just for fun and testing purposes , were going to make some changes in code and push it up 

- Head back to Visual Studio 
- Make sure to pull changes

![image](https://github.com/user-attachments/assets/bc00d24c-1714-413e-8e65-64d5392ce40e)

Reason for this locally we still only have the master which we ended a year ago.

- Clicked pull
- Click on master
- Click on Remote
- Select develop

We actually going to create a PR for this wooohoo and see how which branch gets affected.

- Made some changes , pushed it up to develop

Something to note, your branch will not run , as we have set the trigger to only hit off at main branch 

So only once we create a PR to main will the pipeline activate our entire flow 

![image](https://github.com/user-attachments/assets/cfb2fd9e-a306-4e3a-8967-40d78a974b2a)

- Lets create the PR

![image](https://github.com/user-attachments/assets/4e6881fa-a1f8-4225-90d3-995a50828f54)

![image](https://github.com/user-attachments/assets/e8241dad-ad66-4bdc-8dc4-c2b0d5147583)


Oops forgot to set our trigger for the pipeline 

![image](https://github.com/user-attachments/assets/1d0e59a6-4777-4256-bf26-089c30f1dfa0)

- Enable Continuous integration
- Save and queue

After the pipeline is built 

boom , new release

![image](https://github.com/user-attachments/assets/7bb0190a-52e4-4181-aae2-fba5765f6ff6)

Pretty cool stuff right ??

Once the release build completes you can refresh the website and whoop, changes have been added  !

![image](https://github.com/user-attachments/assets/74d071aa-aa38-47e1-a549-23eaf7df4fb6)

## 🎉 Success!

You’ve successfully set up a Classic Pipeline in Azure DevOps for your .NET Blazor project. This pipeline will automatically build and deploy your app to Azure whenever changes are pushed to the `main` branch.

## Resources

**Azure Pipelines**

- **Overview**: Understand the fundamentals of Azure Pipelines, including YAML and Classic pipelines. [Learn more](https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines)

**Blazor and .NET**

- **Introduction to Blazor**: Learn about building interactive web UIs using Blazor with .NET 9. [Learn more](https://learn.microsoft.com/en-us/aspnet/core/blazor/?view=aspnetcore-9.0)

**Azure App Service**

- **Overview**: Get a comprehensive understanding of Azure App Service, its features, and use cases. [Learn more](https://learn.microsoft.com/en-us/azure/app-service/overview)

**Learning Paths**

- **Azure DevOps**: Follow a structured path to master Azure DevOps. [Learn more](https://learn.microsoft.com/en-us/training/paths/azure-devops-developer)

- **ASP.NET Core and Blazor**: Learn to build and deploy web apps with ASP.NET Core and Blazor. [Learn more](https://learn.microsoft.com/en-us/training/paths/build-web-apps-with-blazor)



