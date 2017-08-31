# Creating a CI/CD pipeline for a NodeJS bot

### Prerequisites

- Create a [Visual Studio Team Services](https://www.visualstudio.com/team-services/) account
- An active Azure subscription
- Fork this repository
 
### Creating a build & deployment definition

1. After creating a new team project, navigate to the **Build** tab and add a new build definition

![new build definition](http://content.screencast.com/users/louisleong/folders/nodejs-bot-devops/media/e657ac08-b449-43e1-aaa5-e77443253abc/1.PNG)

2. Scroll all the way down and select **Empty** to start with an empty template. 
![empty template](http://content.screencast.com/users/louisleong/folders/nodejs-bot-devops/media/418963ea-4019-4c04-9be3-81540c966fd0/2%20empty%20template.PNG)

3. Under **Processes**, select **Hosted** as your agent queue

4. In ** Get Sources**, select **GitHub** and authorise VSTS to access it using OAuth

![authorize GitHub](http://content.screencast.com/users/louisleong/folders/nodejs-bot-devops/media/d9a9b5e9-513c-4085-836a-09e68a37ec45/3%20authorize%20github.PNG) 

5. Ensure that you're pointing to the correct repository

![Repository config](http://content.screencast.com/users/louisleong/folders/nodejs-bot-devops/media/b12666d6-1c02-4788-a2ff-961f2d041c9a/4%20repo%20settings.PNG)

6. Add the following tasks

![tasks](http://content.screencast.com/users/louisleong/folders/nodejs-bot-devops/media/b93af072-d3aa-4470-a19a-0e33d64aa418/6.%20task%20list.png)

**npm install**

Property | Value
--- | --- 
Command | install
Working folder with package.json | src 


**Archive files**

Property | Value
--- | --- 
Root folder (or file) to archive | src 
Archive type | .zip 
Archive file to create | $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip 
Replace existing archive | true 

**Publish Build Artifacts**

Property | Value
--- | --- 
Root folder (or file) to archive | src 
Path to Publish | src
Artifact Name | drop 
Artifact Type | Server 

**Azure App Service Deploy**

Property | Value
--- | --- 
Root folder (or file) to archive | src 
Azure subscription | *select your subscription*
App Service name | *select the web app to deploy to*
Package or folder | $(Build.ArtifactStagingDirectory)/**/*.zip 
Generate Web.config | true 
Web.config parameters | -Handler iisnode -NodeStartFile server.js -appType node 
 
7. Save and queue the build definition

8. You should see your build starting, and your application should be deployed once the process is completed