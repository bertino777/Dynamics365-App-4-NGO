# Fundraising and Engagement App for NGO
This is the Repository where all documentation for project implementation of Dynamics 365 for an NGO based in Paris (France) is stored.
To keep confidentiality closes, the NGO will be called Alpha.

## Context
- Alpha is a Non Profit Organizations with main activities such as :
- Network: Community Platform for Stakeholder acting in Sustainable Energy
- Events – Online and Offline (Debates, RENdez-vous online debates, and Conferences)
- Marketing Campaign 
- Yearly reports through research and surveys on the status of Renewables’ development
- Fundraising

Alpha uses a basic version of Insightly which was deployed in 2013 with a single user account. Insightly basic version is lacking in its functionality in Contact Management, Activity and Event management, Marketing and Campaign Management, Metric and Reporting causing a desinterest from Alpha to use Insightly. Insightly is used as an address book where team members are doing data entry, data tagging, export the reports requested by management. The lack of understanding what a CRM is for and thorough training on how to use the platform properly is causing poor data entry quality. As result, very few staff members spend time now in Insightly, some never use or open it.

## Description of the target mission

- [x] Target the entities to configure (Lead, Contact, Account, Opportunity, Gift, Task, Campaign, Event, Email, PhoneCall) and the settings to achieve (volume & complexity)
- [x] Identify the business processes to be configured (Constituent management, Fundraising & Engagement, Marketing and Events Management)
- [x] Identify the steps of each process, including the prerequisites to be validated to move to the next step in the CRM.
- [x] Identify related cross-functional needs if applicable: mail synchronization, MailChimp, Zoom synchronization, alert settings.
- [x] Identify the KPIs and dashboards required to the operational management of activities on the one hand and reporting (for decision-makers).

## Methodology & Organization

![](images/methodology.png) 

*The solution will be delivered in Agile Mode*

1. Analysts (Functional and Technical) will create the product backlog items based on the pre-study conclusions
2. Delivery teams will take in charge the items across sprints (from READY to DONE)
3. Items will be grouped by topics. When a topic is full done, we would not come back on it (except via a new official demand)
Note: Items will be managed using Azure DevOps (CRM Assets Tool)

## Agile methodology and sprint role

![](images/agile.png) 

*Sprint Cycle Manager (“Scrum Master”) – CRM Assets Resource*
- Coordinates resources during the sprint
- Ensures the methodology respect
- Lead the meetings

*Business Analyst – CRM Assets Resource*
- Animates analysis workshops and build backlog according to the product owner requests
- Is key reference for the project team

*Product Owner – RENT 21 Resource*
- Makes the link between the project team and the business line
- Provide all required elements that must be collected near the business line
- Validates the deliveries (testing sessions results review & approval)

*Project Team Members – CRM Assets Resources*
- Delivers the backlog items
- Package the solution and deploy it in the environments

## Defintion of ready and definition of done

A task is considered as READY state when the following conditions are met: 
- Detailed analysis is done and documented
- The analysis has been reviewed and approved by the Product Owner
- All required information, document or support from the business have been received and reviewed by the Business Analyst
- When a task is READY, it can go into a Sprint backlog

A task is considered as DONE state when the following conditions are met: 
- Item is delivered/developed 
- Item is deployed in Test environment and ready to be deployed
- When a task is DONE, it can go out of the sprint.

Note: Acceptance testing as well as deployment tasks will be considered as Sprint tasks as well

## Team organization and meetings

1. A SteerCo will be scheduled 
- At the beginning of the project 
- A backlog refinement at the beginning of a sprint cycle
- A sprint review at the end of a sprint cycle 

2. 2-weekly meetings will receive the participation of all involved actors of the running Sprint (+/- 15mn )

3. Analysis meeting, for application or technical analysis

## Project task and project review


## Deliverables

Dynamics 365 Sales Application on 3 environments (DEV, UAT and PROD)

- Sales App, Marketing App, Fundraising & Engagement App, Custom Entities & Business Processes
- Entity Diagram updated 
- BPMN updated 
- Functional / Technical descriptions of the developments 
- Functional / Technical descriptions of the integrations 
- Training Manual: Basic & Advanced
- Import templates for data Migration

## Timeframe

