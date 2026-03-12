 

Hey Everybody, 

I hope everyone is doing well!  I wanted to share knowledge gained by a recent Power BI - Row Level Security (RLS) project that I engaged.  This was a bit challenging a long the way.  In the end, I was able obtain success through the use of Dynamic Row Level Security (RLS).  

Let's chat about it!

## <u> PROJECT GOAL</u>: 
The project goal is to build out a Row Level Security (RLS) solution based on the user that opens a specific Power BI Report.   The below table outlines the different View Levels a user can have that we need to be able to implement. 

<img width="686" height="146" alt="image" src="https://github.com/user-attachments/assets/63c75384-fe57-4136-9a0f-1f5d70d21fc0" />













Below is the diagram of the Row Level Security (RLS) solution that I put together to help with the overall project.

<img width="674" height="325" alt="image" src="https://github.com/user-attachments/assets/08f22727-5448-4df7-bddc-21c6956fe971" />














## <u>PROJECT CHALLENGES / QUESTIONS</u>
Based on the data modeling, I had some challenges.  Some questions I had 

How do I filter for the currently logged in user?
How do I control the View Level of the user and deeper into the RLS Rules.


# <u>MEASURES CREATED</u>
I created a series of DAX Measures to help isolate the value for the items that I needed to filter.  

<img width="685" height="201" alt="image" src="https://github.com/user-attachments/assets/f2242d80-c376-4788-9b64-00ce1089a70f" />








# <u>ROW LEVEL SECURITY (RLS) RULES</u>
## User Filter
The first rule that I implemented was to filter my data to the specific user that is logged in to the Power BI Service.  To do this, I utilized a very simple filter [UserEmail] = USERPRINCIPALNAME().  

UserPrincipalName is a DAX Function that pulls the currently logged in user.  

- DAX Documentation: https://docs.microsoft.com/en-us/dax/userprincipalname-function-dax
- DAX Guide Documentation: https://dax.guide/userprincipalname/
- Power BI Docs: https://powerbidocs.com/2021/01/18/dax-userprincipalname-use-in-rls/#:~:text=DAX%20DAX%20USERPRINCIPALNAME%20function%20returns%20the%20user%20name,returns%20the%20login%20email%20of%20currently%20logged%20user.

<img width="616" height="128" alt="image" src="https://github.com/user-attachments/assets/b3e0191e-1929-405d-b224-9601031fe0ab" />











## View Level Filter
I had some challenges getting the View Level to work using the below Dynamic RLS DAX.  To assist with that, I created a small table that contained just the View Level column and made it distinct. 


<img width="632" height="29" alt="image" src="https://github.com/user-attachments/assets/43685939-6f30-4ce6-9910-921a441e6798" />

<img width="632" height="29" alt="image" src="https://github.com/user-attachments/assets/34dec8d2-af41-47b3-b472-f4644fa2d3d1" />

















## Dynamic RLS DAX

To fully achieve success, I utilized the SWITCH, VALUES and FILTER Functions.  I built the SWITCH Function based on the View Level of the currently logged in user.  

- CALCULATETABLE: https://docs.microsoft.com/en-us/dax/calculatetable-function-dax
- SWITCH: https://docs.microsoft.com/en-us/dax/switch-function-dax
- VALUES: https://docs.microsoft.com/en-us/dax/values-function-dax
- FILTER: https://docs.microsoft.com/en-us/dax/filter-function-dax


<img width="632" height="29" alt="image" src="https://github.com/user-attachments/assets/538091e0-feef-420f-aa5d-dc0624b7b72d" />



## Dynamic RLS Results
Here are the results of the implementation of this Dynamic RLS Solution.



### View Level = Store

<img width="632" height="29" alt="image" src="https://github.com/user-attachments/assets/910e4576-3ab3-4318-9310-c6e019877f66" />





### View Level = Zone

<img width="632" height="29" alt="image" src="https://github.com/user-attachments/assets/94a78d02-94fe-41ef-a1ce-cb0c57caac65" />






### View Level = National

<img width="676" height="352" alt="image" src="https://github.com/user-attachments/assets/8823678d-588d-44e6-bebc-e39673471758" />






# <u>POWER BI ROW LEVEL SECURITY (RLS) RESOURCE LINKS</u>
In my work on this project / scenario I came across some really great resources that can help to line up a good Dynamic Row Level Security (RLS) solution.  Check these out.

- Dynamic Row Level Security (RLS) made simple: https://radacad.com/dynamic-row-level-security-with-power-bi-made-simple
- Dynamic Row Level Security (RLS) with Exclude and Include Rules: https://radacad.com/dynamic-row-level-security-in-power-bi-with-exclude-and-include-rules
- Row Level Security (RLS): https://docs.microsoft.com/en-us/power-bi/admin/service-admin-rls
