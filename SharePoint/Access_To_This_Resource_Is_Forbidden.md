# **PRODUCTS - FEATURES INVOLVED**
- Microsoft Power BI Desktop (SharePoint Online List Connector)
- Microsoft SharePoint Online (SharePoint Online List)

# **PROBLEM SCENARIO DESCRIPTION**
Attempting to connect to a SharePoint Online List, and receive the "Access is Forbidden" message.

## **STEPS TO REPRODUCE BEHAVIOR**
- Power BI Desktop
- Get Data
- SharePoint Online List
- Enter the URL to the SharePoint Team Site
- Click Okay to Connect
- Error Message is displayed



# **TROUBLESHOOTING TOOLS AND RESOURCES**
| **Troubleshooting Tool** | **Information** |
| -------------------- | ----------- |
| Power BI Desktop Traces | This will enable us to be able to collect information about what is happening when we reproduce the issue.  Here, the "ResourceAccessForbiddenException" is the exception being thrown.  From the Mashup Logs, you will see a (403) Forbidden error. |
| Telerik Fiddler Trace | Since we are accessing a datasource that is online, Telerik Fiddler is a great tool to help us understand what is happening when we go out to connect to the Online datasource.<br><br>-- If you do not capture anything in a fiddler trace, this is an indication that the Credntials Dialog is not popping up and that the issue could be related to the Datasource Settings. |

<table><tr style="background-color:navy"><th style="color: white; font-weight: bold">EXCEPTION IN POWER BI DESKTOP TRACE LOG</th></tr><tr><td>

| NOTE	| A couple keywords that may assist in identifying / troubleshooting the issue. <br>- ResourceAccessForbiddenException <br>- "identity":null |
| --- | --- |

<br>
"Action":"RemoteDocumentEvaluator/RemoteEvaluation/TranslateCancelExceptions","HostProcessId":"16896","identity":null,"evaluationID":"1","cancelled":"False","Exception":"Exception:\r\nExceptionType: Microsoft.Mashup.Engine.Interface.ResourceAccessForbiddenException, Microsoft.MashupEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=\r\nMessage: Exception of type 'Microsoft.Mashup.Engine.Interface.ResourceAccessForbiddenException' was thrown<br>
</td></tr><table>

<table><tr style="background-color:navy"><th style="color: white; font-weight: bold">MASHUP CONTAIN LOG</th></tr><tr><td>
DataMashup.Trace Error: 24579 : "Action":"Engine/IO/OData/GetResponse","ResourceKind":"SharePoint","ResourcePath":"https://some.sharepointonlinesite.example/sites/MyExampleSite","HostProcessId":"16896","PartitionKey":"Section1/Query1/Source","RequestUri":"https://some.sharepointonlinesite.example/sites/MyExampleSite/_api/web/lists?$select=Id,Title,Items","RequestMethod":"GET","Exception":"Exception:\r\nExceptionType: System.Net.WebException, System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=\r\nMessage: The remote server returned an error: (403) Forbidden.\r\nStackTrace:\n   at Microsoft.Mashup.Engine1.Library.Common.WrappingHttpWebRequest.WrapResponse(Func`1 getResponse)\r\n   at Microsoft.Mashup.Engine1.Library.OData.ODataRequest.GetResponse(WebRequest webRequest)\r\n\r\n\r\n","ProductVersion":"2.82.5858.1161 (20.06)","ActivityId":"<some guid entry>","Process":"Microsoft.Mashup.Container.NetFX45","Pid":10516,"Tid":1,"Duration":"00:00:00.0999579"}
</td></tr><table>

# **CAUSE**
The credentials in the datasource settings were invalid and needed to be refreshed / updated.

# **RESULT OF CAUSE**
Because the datasource settings were not correct, the Credential Dialog was not received, thus the identity of the user is null.


# **RESOLUTION**
You can resolve this in a couple different ways:
1. In the datasource setting dialog, clear the permissions for the SharePoint Online Team Site and then recreate the connection to the SharePoint Team Site.  You should get prompted for credentials now.
2. In the datasource setting dialog, Edit the permissions for the SharePoint Online Team Site and re-authenticate to the SharePoint Online Team Site.

# **ADDITIONAL INFORMATION AND RESOURCES**
- [Create a report on a SharePoint Online List](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-sharepoint-online-list#:~:text=%20Part%201%3A%20Connect%20to%20your%20SharePoint%20List,your%20list.%20From%20a%20page%20in...%20More%20)


### *Tags*
pbitimmac pbiSharePoint pbiForbidden
