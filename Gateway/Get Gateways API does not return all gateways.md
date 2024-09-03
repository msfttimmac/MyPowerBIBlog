API DOCUMENTATION: https://learn.microsoft.com/en-us/rest/api/power-bi/gateways/get-gateways

PROBLEM SCENARIO
Recently engaged on an issue where not all Gateways were being returned by the Get Gateways Rest API call.  

CAUSE
Reviewed each gateway cluster.  Noticed the account that is being used in the script pulling the information is not listed in each of the Gateways.  According to the documentation the user that is executing the Rest API call must be listed as a Gateway Administrator.

RESOLUTION
Add the user executing the script to the Gateway Administrators. 

#msfttimmac #powerbi #gateways #restapi  #missinggateways
