# Security Role
Security is a fundamental pillar of any IT infrastructure, especially for a customer relationship management (CRM) system such as Dynamics. By ensuring data security, we not only protect our customers' sensitive information, but also guarantee that every user has the necessary access to perform their tasks without compromising the confidentiality or integrity of the system.



## Details

| Security role | Description |  Criteria |
| ------ | ------ | ------ |
|  Basic | As a Basic user, I can see everything with few exceptions. |As a basic user, I have read access to all data except contracts, salaries, hiring data and fundraising tracking data (on donors/funders). |
| Alpha Knowledge and data | - See everything with a few exceptions | - Read access to all data except salaries and hiring data. |
| | - manage events, campaigns, campaigns activities, marketing Lists and contacts including VIP contacts and including fundraising tracking data (on contributors, communicator, speaker, members), BUT excluding fundraising tracking data (on donors/funders) of leads/contacts. |- Create, read and write access to events, campaigns, campaigns activity, marketing Lists and contacts including VIP contacts including VIP contacts and including fundraising tracking data (on contributors, communicator, speaker, members), BUT excluding fundraising tracking data (on donors/funders) of leads/contacts. |
| Alpha Communications | - See everything with a few exceptions. |- Read access to all the data except salaries and hiring data. |
|  |- Manage events, campaigns, campaigns activities, marketing Lists and contacts including VIP contacts and including fundraising tracking data (on contributors, communicator, speaker, members), BUT excluding fundraising tracking data (on donors/funders) of leads/contacts ts.  |- Create, read and write access to events, campaigns, campaigns activities, marketing Lists and contacts including contact VIP type and including fundraising tracking data (on contributors, communicator, speaker, members), BUT excluding fundraising tracking data (on donors/funders) of leads/contacts. |
| Alpha Community and partnership |- See everything with a few exceptions. |- Read access to all data except for contracts, salaries and hiring data.  |
|  |Manage events, campaigns, campaigns activities, marketing List sand contacts including VIP contacts and including fundraising tracking data (on contributors, communicator, speaker, members), BUT excluding fundraising tracking data (on donors/funders) of leads/contacts.  |- Create, read and write access to events, campaigns, campaigns activities, marketing Lists and contacts including VIP contacts and including fundraising tracking data (on contributors, communicator, speaker, members), BUT excluding fundraising tracking data (on donors/funders) of leads/contacts. |
|  |  |Write access to the VIP contacts/organizations flag (i.e. give or remove VIP status). |
| Alpha Admin manager | - See everything with a few exceptions. |- Read access to all data except salaries and hiring data. |
|  |Manage events, campaigns, campaigns activities, marketing Lists and contacts including VIP contacts and including fundraising tracking data (on contributors, communicator, speaker, members), BUT excluding fundraising tracking data (on donors/funders) of leads/contacts.  |- Create, read and write access to events, campaigns, campaigns activities, marketing lists and contacts including VIP contacts and including fundraising tracking data (on contributors, communicator, speaker, members), BUT excluding fundraising tracking data (on donors/funders) of leads/contacts. |
| Alpha Direction | See everything. |- Read access to all data. |
|  |Manage events, campaigns, campaigns activities, marketing Lists, salaries and hiring information.  |- Create, read and write access to events, campaigns, campaigns activity, marketing List, salaries and hiring information. |
|  |  |Write access to the VIP contacts/organizations flag (i.e. give or remove VIP status). |
| Sharepoint access to contributors | As a contributor user, I can only comment documents from the SharePoint. |- All comments done be the contributors should be visible. |
|  |  |His comments must carry his name (or another identifier). |


## Implementation

- Rely on the Basic User role for creation
- Customize each role on the various tables according to specific requirements
- Use the system administrator role for additional admins
- Assign all roles to be used to the application to be developed
- Assign roles to users 


