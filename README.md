# SharePoint Delegation Solution for Power Apps
What is included:
- ✅ Delegation support for full-text search
- ✅ Dynamic multi-select filtering (no more manually listing items in a multi-select combo box)
- ✅ Multi-column full-text search - search on multiple columns with one search box
- ✅ Delegable sort for choice columns
- ✅ Better pagination support - Yes, the default pagination can be improved upon.
  - ✔️ Choose your page size - You are no longer stuck with the 100-item page size
  - ✔️ Give your user the choice to choose how many items to show in the gallery at a time.
 
## Dataset
Create a list named TicketingSystem_Teams with the following schema
- Title: Single line text
- Email: Single line text
- Lead: Person or Group (don't allow multiple selections)

Add the following data to the list

| Title | Email |
| ------------- | ------------- |
| Domain Service Team	| ds_team@contoso.com |
| Network Infrastructure Team | ni_team@contoso.com |
| Operations Support Team |	ops_support@constoso.com |
| End User Support Team |	eus@contoso.com |

For the Lead column, manually add users in your environment.

Create another list named TicketingSystem_ServiceTickets with the following schema
- Title: Single line text
- ServiceType: Choice
  - Incident
  - Problem
  - Change
- ServiceCategory: Choice
  - Work Devices
  - Networking
  - Software
  - Equipment
  - Domain and Servers
- TeamToImplement: Lookup to TicketingSystem_Teams and select Title as the column to look up
- AssignedTo: Person or Group (don't allow multiple selections)
- TicketID: Single line of text
- ServiceDescription: Multiple lines of text

You can download the large dataset [here](Dataset/TicketingSystem.json)

If you want to generate a different set of data with the same schema, use the link below to download the large dataset from [mockaroo](https://mockaroo.com/)

You can generate data using cURL with the following command:
```
curl "https://api.mockaroo.com/api/fc9fbc90?count=1000&key=e2064450" > "TicketingSystem.json"
```
Or just visit the public URL:
https://www.mockaroo.com/fc9fbc90

If you don't have a paid mockaroo account, you are limited to 1000 items per download.
You can download as much as you need and combine them into one file.

### Assigning users
To populate the assignedTo column for the TicketingSystem_ServiceTickets list, first download this [flow template](https://github.com/adedaporh/powerplatform/blob/main/Power%20Automate/GetSiteUsers_20230930110536.zip).
This flow will retrieve the IDs of all the users with access to your site. To use the IDs, you have 2 options depending on whether you are using the provided dataset or you prefer to generate another one from mockaroo.

**From dataset**

Edit the downloaded dataset and find *"AssignedToId":*. This will highlight all the places this column value is set in the json file. Replace all the values that are not in your set of IDs. For example, you can replace all instance of *"AssignedToId":1,* with *"AssignedToId":13,* etc. Note the inclusion of the comma.

**From mockaroo**

Simply add all the IDs in a comma seperated list in the *AssignedToId* field. Then generate your data.

### Import data

To import the data into the list, download this [flow](https://github.com/adedaporh/powerplatform/blob/main/Power%20Automate/LoadDataPlus_20230921094630.zip) and pass the downloaded dataset json file as well as the SP site address and SP list name as inputs to the flow.
