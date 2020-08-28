

# **PRODUCTS - FEATURES INVOLVED**
- Microsoft Power BI Service (Sharing of Power BI Reports w/ External Users)
- Azure B2B Scenario (Inviting External Users)

# **PROBLEM SCENARIO DESCRIPTION**
This scenario is where you invite an External User from another Azure Tenant into your Azure Tenant and then share a Power BI Report.
You have provided them with *Viewer Role* access to the Power BI Workspace.  The External User then attempts to navigate to the Power BI Report and receives a message indicating access is denied.

# **CAUSE**
This is a very common issue.  The reason this occurs is that the External User has not signed in to your Power BI Service on your Azure Tenant.  Most of the time, this occurs on the initial sharing of the Power BI Report.
The reason, is that when the External User navigates to the shared Power BI Report, it takes them to the Power BI Service on their Azure Tenant.   Their Power BI Service has no knowledge of your Power BI Report and/or the Power BI Workspace that the Power BI Report resides.

# **RESOLUTION**
The key to resolve the issue is that we have to tell the Power BI Service which Tenant to navigate to.  We can do this with the follwing at the end of the URL.

           *?ctid=<your azure tenant id>*

| NOTE | Your Azure Tenant ID can be found in the Azure Active Directory with in the Azure Portal (portal.azure.com) |
| --- | --- |

Here are some easy steps to help resolve quickly.

1.	On your Azure Tenant, navigate to the Power BI Service.
2.	In the upper right, select the “?” and then About Power BI
3.	Copy the Tenant URL
4.	Send that Tenant URL (format should look like: app.powerbi.com/home?ctid=<tenant id>) to your external end-user and have them sign in to the Power BI Service for your Tenant.
5.	Once they are signed in, have them navigate to the Power BI Workspace in question and view the Power BI Report.


