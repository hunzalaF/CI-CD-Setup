###  Pipelines with Azure Devops 
Here how current Azure pipelines are configured right now. 
### Build pipelines 

- Import JSON File into Azure pipelines.
- Select the agent pool in pipeline configuration.
- Select Get Sources.
- Connect Bitbucket repository if required.
- Select branch working on.
- remember to change arguments /p: environment=Dev in pipeline according to your requirement in - task: Publish.
- Add (AES Encryption in Azure Artifacts ) connect restore with this feed and also check Nuget option as well.
- `dotnet nuget push --source "AESEncryption" --api-key az <package-path>`

### Release Pipeline 

- Connect Artifacts from your build pipelines as per requirements.
- It contains 2 stages  IIS and SQL Deployement
- Add DB_USER and DB_PASSWORD in variables in release pipelines 
- Assign DB user permissions for DB ownership(User Mapping), create DB (server role), Manage, Also edit securables for this user Alter any DB , Connect any DB, Connect SQL, Create any DB. 
- Also allow it to sign in SQL Server Authentication.


### Azure Pipeline [Agents](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=yaml%2Cbrowser) 

Currenty we are using two agents:
- Microsoft  Hosted Agents (MHSA)
- Self Hosted Agents (SHA)


![Untitled](https://github.com/HUNZALAMUSHTAQ/devops-readme/assets/75185145/89be01f4-df08-41ad-9582-90ec489169ce)



## Here how to add SHA. 

There are 3 Pools in Agents you can create as many pool you like according to your needs 
- Azure Pipeline (**MHSA**)
- Self Hosted Agents (**HSA**)


![image](https://github.com/HUNZALAMUSHTAQ/devops-readme/assets/75185145/ba55a177-dcee-4a5f-a59b-67404deed78c)

You add agents in your pool by clicking New agents according to your system specifications. 

![image](https://github.com/HUNZALAMUSHTAQ/devops-readme/assets/75185145/8d309d96-7aee-48fe-b2c4-34f042e76aae)

Configure your account by following the steps outlined [here](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/windows-agent?view=azure-devops#permissions).

![image](https://github.com/HUNZALAMUSHTAQ/devops-readme/assets/75185145/dc470d18-6675-4487-b745-e836c15abf18)



**Why Azure artifacts  ?**

We have our own AES Encryption Package used in Health Quest other than Nuget Packages we restore packages from azure artifacts and Nuget itself. 

Manage all package types (NuGet, npm, Maven, etc.) across various programming languages from a single platform, simplifying package management.

***Publish Package to Azure Artifacts *
**

`dotnet nuget push --source "AESEncryption" --api-key az <package-path>`

***How to create, host, and share packages with Azure Artifacts* **[click here](https://www.youtube.com/watch?v=xHNXwqxV7Uc)

### Deployment Process
###### 1. Code Retrieval
*Source*: Latest code from Bitbucket repository

###### 2. Build Orchestration
![image](https://github.com/HUNZALAMUSHTAQ/devops-readme/assets/75185145/fc4e2e07-371f-4a29-af8b-678cee5cf94c)



Build pipeline on Azure Pipelines build process is done on **SHA** or **MHSA**. Select Agent according to your requirements.

###### 3. Web Application Deployment and Database Deployment

###### Release Pipelines 

***Target***:  On-premises **Folio3** machine and SQL Server database on Folio3 server

***Mechanism***: IIS (Internet Information Services) web server and On-premises SQL scripts executed directly generated from build pipelines.



![image](https://github.com/HUNZALAMUSHTAQ/devops-readme/assets/75185145/9de39615-8290-4c39-aa25-8c1633b8e2b3)

Here is how your Stages Look like: 

![image](https://github.com/HUNZALAMUSHTAQ/devops-readme/assets/75185145/1b7b65a0-f0e9-461a-8a15-a92be46ca94a)

#### [Deployment Group ](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/deployment-groups/?view=azure-devops)
> A deployment group is a logical set of deployment target machines that have agents installed on each one. Deployment groups represent the physical environments; for example, "Dev", "Test", "UAT", and "Production". In effect, a deployment group is just another grouping of agents, much like an agent pool.

![image](https://github.com/HUNZALAMUSHTAQ/devops-readme/assets/75185145/57fdb3ec-ee63-42c4-a6c3-56bb48fc5aa3)

Deployment groups define sets of target machines for deployments (Dev, Test, Production), while agent pools manage the deployment agents that execute tasks on those machines.

In our case we are deploying to the folio3 machine **(on-premise)**.

### FAQ
**What is a parallel job?**

Each running job consumes a *parallel job* that runs on an agent.
When not enough job? The jobs are queued up and run one after the other.

**Are there any limits on the number of builds and release pipelines that I can create?**

No. You can create hundreds or even thousands of pipelines for no charge. You can register any number of self-hosted agents for no charge.
