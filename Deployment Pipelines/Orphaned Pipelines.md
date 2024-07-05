# OVERVIEW
Recently, I was asked about why the **[Pipelines - Get Pipeline Stages](https://learn.microsoft.com/en-us/rest/api/power-bi/pipelines/get-pipeline-stages)** and the **[Admin - Pipelines GetPipelinesAsAdmin](https://learn.microsoft.com/en-us/rest/api/power-bi/admin/pipelines-get-pipelines-as-admin)** was not returning Power BI Workspace Information.  Puzzling, I reviewed the output of the API and noticed the issue.  But then the question is how did we get into this state?  I walked through the creation process of a Deployment Pipepline and discovered that if the process is not completed, you can end up in this state, which is essentially an orphaned deployment pipeline.  Review the **[Response to the above powershell code](https://github.com/msfttimmac/MyPowerBIBlog/blob/master/Deployment%20Pipelines/Orphaned%20Pipelines.md#response-to-the-above-powershell-code)** section for what the output looks like for an orphaned pipeline based on no workspace information.  The reason that it is orphaned, is that from the GUI perspective, one would not see it and thus would end up creating a new deployment pipeline for that Power BI Workspace. 

# DIALOGS TO PAY ATTENTION TOO
If you click the **Skip Button** on the Assign your workspace to a stage screen it will close the *Assign your workspace to a stage* dialog; however, you will now see another page that allows you to enter the information for the pipeline. 

![image](https://github.com/msfttimmac/MyPowerBIBlog/assets/50430004/efddf3f3-60c0-4209-9cc0-ff7306bae6c5)

Clicking the **Skip Button** does make the dialog go away; however, it does lead you to the creation screen to walk through the Deployment Pipeline stages. Now if you leave this screen without filling in the information, the pipeline information is still returned by the API; however, this just now means that it has been orphaned.  

![image](https://github.com/msfttimmac/MyPowerBIBlog/assets/50430004/8c546aee-c985-46bc-a298-273a3ab7e96d)


# POWERSHELL CODE
  $myurl = "https://api.powerbi.com/v1.0/myorg/admin/pipelines?%24expand=stages,users"
  
  (Invoke-PowerBIRestMethod -Url $myurl -Method get | convertfrom-json).value | Convertto-Json -Depth 100

# RESPONSE TO THE ABOVE POWERSHELL CODE 
(If you click the Skip button on the Assign your workspace to a stage screen.)

```
   {
        "id":  "<deployment pipeline id guid>",
        "displayName":  "mypipelinetest",
        "stages":  [
                       {
                           "order":  0
                       },
                       {
                           "order":  1
                       },
                       {
                           "order":  2
                       }
                   ],
        "users":  [
                      {
                          "accessRight":  "Admin",
                          "identifier":  "theemail@thedomain.com",
                          "principalType":  "User"
                      }
                  ]
    }
```

# QUESTIONS
### Can a workspace be assigned to an orphaned deployment pipeline?
Yes.  There are limitations around the ability to do this; however, as long as the Power BI Workspace does not have a Deployment Pipeline already and a Power BI Workspace Admin is doing this, one should be able to do with the [Pipelines - Assign Workspace Rest API](https://learn.microsoft.com/en-us/rest/api/power-bi/pipelines/assign-workspace)

### How was this discovered?
The results were discovered during a Power BI Service inventory.  Becoming very granular with the inventory and understanding each piece, this issue was discovered.  This is an orphaned pipeline.  

### How to find my orphaned and existing pipelines?
1. In the Power BI Service, click on Workspaces on the left hand side
2. At the bottom, directly above **Create a Workspace** is **Deployment Pipelines**
3. Click on Deployment Pipelines
   ![image](https://github.com/msfttimmac/MyPowerBIBlog/assets/50430004/18239675-33a4-48cc-9c6c-c2dc108ec553)
4. This will show you all of your Deployment Pipelines.

# RESOURCES / API DOCUMENTATION
1. [Admin - Pipelines GetPipelinesAsAdmin](https://learn.microsoft.com/en-us/rest/api/power-bi/admin/pipelines-get-pipelines-as-admin)
2. [Pipelines - Get Pipeline Stages](https://learn.microsoft.com/en-us/rest/api/power-bi/pipelines/get-pipeline-stages)
3. [Pipelines - Assign Workspace Rest API](https://learn.microsoft.com/en-us/rest/api/power-bi/pipelines/assign-workspace)

