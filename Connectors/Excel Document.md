**PRODUCTS INVOLVED/ POWER BI FEATURES INVOLVED**
  - Microsoft Power BI Desktop
  - Microsoft Power BI Service (Scheduled Refresh) 
  - SharePoint Online and/or SharePoint On-Premise
  - Microsoft Excel (XLSX)

**PROBLEM SCENARIO DESCRIPTION**
Are you building a Microsoft Power BI solution that is pulling data from a Microsoft Excel (XLSX) file that is residing on a SharePoint Site?    Are you experiencing issues trying to get that connection to work?  Maybe you have tried the Web Connector or the Excel Connector with still no luck in getting it to work.   Well, my goal for this article is to assist in understanding how to get this to work in your Power BI Solution.
 
**ERROR MESSAGE**
You might encounter a message like this This message is displayed when attempting to Schedule a Refresh in the Power BI Service.  We then go to enter the credentials.   
		
![image](https://github.com/user-attachments/assets/c861d350-40d7-46f4-925a-d313175a7b37)


**EXCEL FILE PATH EXAMPLES THAT DO NOT WORK**

<ins>Copy link Menu Item: </ins>

The Excel Path will look like this: https://exceltestinglinkscenario.sharepoint.com/:x:/t/some-tboguseam-test/ETCnElhrOvBJtrAYPeMBJyYB_Xw8FyS4PeigPv82EAJ4jA?e=60ElaD

![image](https://github.com/user-attachments/assets/e16d0997-83cd-4e5e-aed6-3b5d3d5255ab)


Result
* Does not work for Web Connector
* Does not work for Excel Connector
* Power BI does not work well with this URL pathing to the file.  The reason, is that Power BI does not understand
  * the :x/t  in the path
  * The end of the path with the long alphanumeric string.



<ins>URL from Excel Online</ins>

The Excel Path will look like this: https://exceltestinglinkscenario.sharepoint.com/:x:/r/teams/some-bogusteam-test/_layouts/15/Doc.aspx?sourcedoc=%7B5812A730-3A6B-49F0-B6B0-183DE3012726%7D&file=Book_Book.xlsx&action=default&mobileredirect=true

Result
* Does not work
* Power BI does not work well with this URL pathing to the Excel file.

<ins>Copy path to clipboard</ins>
The Excel Path will look like this: https://exceltestinglinkscenario.sharepoint.com/teams/some-bogusteam-test/Shared%20Documents/General/BookBlah.xlsx?web=1

![image](https://github.com/user-attachments/assets/ab57870b-2f10-45ab-9b12-9050f242d29f)

Result
* Does not work
* Power BI does not like the ?web=1 at the end of this URL

  ![image](https://github.com/user-attachments/assets/26375ae2-1e76-45f9-81ee-ac2b74f95dbf)

**TROUBLESHOOTING ITEMS**
  1. We attempted to follow the steps to obtain the file path from the Excel Application, but we were not able to navigate to the Excel document to even be able to right click and select "Copy File Path" menu option.  I do believe that this was related to the Operating System, Security Policy (Local and/or Group) and the version of Microsoft Excel that the customer had loaded.
  2. I did the steps in my environment and obtained the correct path in my lab solution.  Once I had this, I then obtained the customer's URL and then made the format of the customer's URL to match mine.  I then provided this back to the customer and we were able to get the Excel Connector to work for us.

**CAUSE**
The reason that we could not see the OAUTH2 in the drop down list, is because the URL that is being utilized to point to the Microsoft Excel (XLSX) file is not one that Power BI understands.

**RESOLUTION**
  * To resolve this issue, the bad URL path to the Excel Document stored on SharePoint Online, will need to be converted to a URL that is fully understandable to Power BI.

<ins>Excel Document URL Format</ins>
  * **HTTPS**: If this is SharePoint Online, HTTPS will always need to be utilized.  If it is not, authentication errors and other errors will be displayed
Sharepoint Path	If this is SharePoint Online, the format should be something like this: companyname.sharepoint.com

	_EXAMPLE: Microsoft.sharepoint.com_
	
  * **Sites Path**: If the document exists on a Site within SharePoint, that will need be included without extra characters.  This format should be something like this:sharepoint-online-path/sitename

	_EXAMPLE:Microsoft.sharepoint.com/teams_
	
  * **File Name**: This will be the file name of the Excel Document.

	_EXAMPLE: Microsoft.sharepoint.com/teams/myexceldoc.xlsx_

* Resolving this issue, we had to modify the bad URL to be a URL that Power BI understands.

  - Bad Hyperlink: https://microsoft.sharepoint.com/:x:/t/timmac-team-test/ETCnElhrOvBJtrAYPeMBJyYB_Xw8FyS4PeigPv82EAJ4jA?e=60ElaD
  - Power BI understanding URL	https://microsoft.sharepoint.com/timmac-team-test/books1.xlsx
    

**ADDITIONAL INFORMATION**
  * Get data from files for Power BI: https://docs.microsoft.com/en-us/power-bi/service-get-data-from-files
  * Get data from Excel Workbook Files: https://docs.microsoft.com/en-us/power-bi/service-get-data-from-files
  * Connect to Excel in Power BI Desktop: https://docs.microsoft.com/en-us/power-bi/desktop-connect-excel
  * Connect to CSV file in Power BI Desktop: https://docs.microsoft.com/en-us/power-bi/desktop-connect-csv


#msfttimmacexcel #pbitimmacexcel
