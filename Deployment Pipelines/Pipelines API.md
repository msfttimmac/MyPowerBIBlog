# OVERVIEW
Recently I engaged on some work around the Power BI Rest APIs concerning the Deployment Pipelines. During the process of working with the Deployment Pipeline APIs, I started noticing returned information from the **Admin - Pipelines GetPipelinesAsAdmin** that does not have any Power BI Workspace information associated to it. 

Continuing to research this information, I walked through the Deployment Pipeline Creation process and discovered that if you do not complete the creation process of the deployment pipeline you will receive results similar to the below **Response to the above powershell code** section.

If you click the **Skip Button** on the Assign your workspace to a stage screen it will close the *Assign your workspace to a stage* dialog; however, you will now see another page that allows you to enter the information for the pipeline. 

![image](https://github.com/msfttimmac/MyPowerBIBlog/assets/50430004/efddf3f3-60c0-4209-9cc0-ff7306bae6c5)

Clicking the **Skip Button** does make the dialog go away; however, it does lead you to the creation screen to walk through the Deployment Pipeline stages. Now if you leave this screen without filling in the information, the pipeline information is still returned by the API; however, this just now means that it has been orphaned.  

![image](https://github.com/msfttimmac/MyPowerBIBlog/assets/50430004/8c546aee-c985-46bc-a298-273a3ab7e96d)


# POWERSHELL CODE
  $myurl = "https://api.powerbigov.us/v1.0/myorg/admin/pipelines?%24expand=stages,users"
  
  (Invoke-PowerBIRestMethod -Url $myurl -Method get | convertfrom-json).value | Convertto-Json -Depth 100

# RESPONSE TO THE ABOVE POWERSHELL CODE 
(If you click the Skip button on the Assign your workspace to a stage screen.)

```
   {
        "id":  "53e584eb-4352-4fb0-924f-c66f529097c0",
        "displayName":  "timmacpipelinetest",
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
                          "identifier":  "Timothy.Macaulay@va.gov",
                          "principalType":  "User"
                      }
                  ]
    }
```

# RESOURCES / API DOCUMENTATION
1. Admin - Pipelines GetPipelinesAsAdmin - https://learn.microsoft.com/en-us/rest/api/power-bi/admin/pipelines-get-pipelines-as-admin

