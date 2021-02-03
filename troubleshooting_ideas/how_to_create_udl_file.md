## How to create a UDL file to test the connection to SQL Server ##

1. Create a new text file on the Desktop and call it TestSQL.Txt 
2. Change the file extension to UDL (TestSQL.UDL) 
3. Open a Command-Prompt where you right click while holding the Shift button down to select Run As. 
		1. You want to run it as the FIM Service Management Agent Account 
		2. Or you could log into the machine as the FIM Service Management Agent Account 
4. Double click on the TestSQL.UDL 
5. On the Provider Tab ensure that Microsoft OLE DB Provider for SQL Server is selected 
6. Click Next 
7. On the Connection Tab, 
		1. Select or enter a server name: Enter the name of the SQL Server that the Synchronization Service is pointing which is housing the FIMSynchronizationService database. 
		2. Use Windows NT Integrated Security 
		3. Click Test Connection 
If this works, then you have a successful connection to SQL Server 
