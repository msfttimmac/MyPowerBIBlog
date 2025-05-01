Today, I was challenged with the question, what can a user with a Power BI Pro License do inside a Premium Per User Power BI Workspace.  I took some time to investigate and this is what I found. 

The following documentation (https://learn.microsoft.com/en-us/power-bi/enterprise/service-premium-per-user-faq#end-user-experience) indicates for us, that in order to interact with items in a Premium Per user Power BI Workspace, one has to have a Premium Per User license.  

![image](https://github.com/user-attachments/assets/16f421b0-4ac6-4ded-907f-62ed043ff1d7)


I then added a user with a Power BI Pro License to a Premium Per User Power BI Workspace with the Admin Role (Power BI Workspace Roles: https://learn.microsoft.com/en-us/power-bi/collaborate-share/service-roles-new-workspaces).  

Initially entering the Premium Per User Power BI Workspace nothing happens; however, when the Power BI Pro Licensed user attempts to view a Power BI Report the user is then prompted with the "Upgrade your Power BI License" dialog, telling the user that they would need to uupgrade their Power BI License to the Premium Per User License.  (Image below)

![image](https://github.com/user-attachments/assets/d875d57c-1cad-41e9-bd7e-12338e0d5572)

The Power BI Pro User then reviewed the License Info in the Power BI Workspace Settings. The Power BI Pro user with the Admin Role has the ability to change the Power BI Workspace Licensing to Pro; however, they cannot upgrade it to a Premium Per User License.  Now this is important to note, because  if the Power BI Premium Per User has shared out reports from this Power BI Workspace, then reports could be broken if the licensing is downgraded to a Power BI Pro License.

![image](https://github.com/user-attachments/assets/13ca7ad8-3471-4787-9f2a-ed675a8aa574)

If the Poower BI Pro License User navigates to the Settings of the Semantic Model, they will be presented with the Settings page with many disabled items as below documented.

![image](https://github.com/user-attachments/assets/438949aa-5747-4144-9f03-68ba8fd74cea)
