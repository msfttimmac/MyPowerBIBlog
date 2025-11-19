Here’s a **dynamic rewrite** of your original content with improved formatting, clarity, and a new section for **Sources and Documentation** based on Microsoft Learn references:

***

# **API DOCUMENTATION**

[Power BI REST API – Get Gateways](https://learn.microsoft.com/en-us/rest/api/power-bi/gateways/get-gateways)

***

## **🚨 Problem Scenario**

While working on an automation script using the **Get Gateways** REST API, we noticed that **not all gateways were being returned** in the response. This caused incomplete visibility into the gateway inventory.

***

## **🔍 Root Cause**

After reviewing each gateway cluster, we discovered that the **account executing the REST API call was not listed as a Gateway Administrator**.

According to Microsoft documentation, the **Get Gateways API only returns gateways where the calling user has admin permissions**. If the user is not a Gateway Admin, those gateways will not appear in the API response.

***

## **✅ Resolution**

Add the user (or service principal) executing the script to the **Gateway Administrators** list for each gateway cluster.

Once the account has the required permissions, re-run the API call and verify that all gateways are returned.

***

### **Key Takeaways**

*   The **Get Gateways** API enforces strict permission checks.
*   Gateway visibility is **role-based**; only admins see the full list.
*   For automation scenarios, consider using a **Service Principal** with delegated permissions for better governance.

***

## **📚 Sources and Documentation**

*   [Gateways – Get Gateways (Power BI REST API)](https://learn.microsoft.com/en-us/rest/api/power-bi/gateways/get-gateways)
*   [Gateways – REST API Overview](https://learn.microsoft.com/en-us/rest/api/power-bi/gateways)
*   [Using the Power BI REST APIs](https://learn.microsoft.com/en-us/rest/api/power-bi/)
*   [Power BI Implementation Planning: Data Gateways](https://learn.microsoft.com/en-us/power-bi/guidance/powerbi-implementation-planning-data-gateways)

***

**Tags:** #msfttimmac #powerbi #gateways #restapi #missinggateways
